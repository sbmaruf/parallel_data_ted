# Parallel data extractor from ted corpa


This is a script written in **python** to extract parallel sentences from ted corpa.

Here are the *parameters* of the *.py files. A default value is set to the script for easier use. 


```
usage: loader.py [-h] [--debug DEBUG] [--drop_dir DROP_DIR]
                 [--per_dir PER_DIR] [--war_dir WAR_DIR] [--lang1 LANG1]
                 [--lang2 LANG2] [--dir DIR] [--out_dir OUT_DIR]
                 [--out_file OUT_FILE]

Ted parallel data extraction.

optional arguments:
  -h, --help           show this help message and exit
  --debug DEBUG        Additional print operation for debugging.
  --drop_dir DROP_DIR  The dataset with ambiguous data will be transferred to
                       this directory.
  --per_dir PER_DIR    The dataset with perfect data will be transferred to
                       this directory.
  --war_dir WAR_DIR    The dataset with warning based data will be transferred
                       to this directory.
  --lang1 LANG1        Each of the input file's name starts with this Language
                       name.
  --lang2 LANG2        Each of the input file's name starts with this Language
                       name.
  --dir DIR            Directory of the dataset
  --out_dir OUT_DIR    NMT parallel sentences files
  --out_file OUT_FILE  Name of the output file
```


## I/O


The sentences are in multiple files. Each of the parallel files contains a name format like below,

```
Lang1.hash.ext
Lang2.hash.ext
```

An example of this file: `English.0cvHoFWiJxVO.srt`.
The hashes are unique for a pair of language. 

To extract data, at first provide the input files in a folder and send the address of the directory to the script (loader.py) by argument `--dir`

For a pair of language, `Lang1` and `Lang2` will be always same. The name of them will be given to the script by `--lang1` and `--lang2` argument. 

The file where all the output parallel sentences are written is `--out_dir`+`--out_file`.

the structure of the output file is
```
#-----<s>-----#
sentence 1
```
Before each of the sentence, there is a line containing `#-----<s>-----#`. This is added for further helping further processing. 

## Data file structure


The script is written assuming the format of the input file in the corpa are like below,
```
#line number
#Time stamp
#line
#line
#space
#space
#line number
#Time stamp
#line
#line
#space
```
* So there could be multiple new lines `\n` between sentences but each sentence will be starting with a line number. 
* Each sentence can also contain in multiple lines. 
* Each sentence will strictly start just after time stamp.

If there is any other change in the format of the input files then this parser may not work.

## Problem


* Sometimes the parallel data files (like `Lang1.hash.ext` and `Lang2.hash.ext`) does not contain the same number of lines. In this case, we match the timestamp to extract the data. The script will give you a warning for this kind of scenario. One copy of this type of files will be copied to `--war_dir`.
* Sometimes the parallel data files (like `Lang1.hash.ext` and `Lang2.hash.ext`) does not contain the same number of lines and one/both data file may contain all the time stamp same. These are called `critical` files and will be copied to `--drop_dir` folder.

***Till now we don't have any way to extract data from `critical` data files.***

If the number of lines for a parallel data file(like `Lang1.hash.ext` and `Lang2.hash.ext`) are same then one copy of this parallel data file will be copied to `--per_dir`.

## Related Repository

	[srt2txt]https://github.com/ahmed451/srt2txt
