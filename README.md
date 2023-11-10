# CSVCat

**csvcat** is a simple command-line utility for parsing, reformatting, and pretty-printing CSV files in a memory-efficient manner that makes it ideal for large files. I wrote this program because I was having some difficulties finding CLI tools that can print CSV files with equal column width. It supports both comma and tab delimited CSV files.

## Installation

```bash
git clone https://github.com/4g3nt47/csvcat.git
cd csvcat
chmod +x csvcat
./csvcat
```

## Usage

```bash
usage: csvcat [-h] [-b BORDER] [-m MATCH] [-i IGNORE] [-c] [-r] [-s SKIP]
              [-n MAX_ROWS]
              filename

A command-line CSV pretty-printer by 4g3nt47 (https://github.com/4g3nt47)

positional arguments:
  filename     CSV file to read

optional arguments:
  -h, --help   show this help message and exit
  -b BORDER    The columns border to use in output (default is whitespace)
  -m MATCH     Comma-separated column numbers to print (3,1,6-9) (prioritized
               over -i)
  -i IGNORE    Comma-separated column numbers to ignore (1,3,6-9)
  -c           Use comma as input delimiter
  -r           Output raw (no pretty-printing)
  -s SKIP      Number of rows to skip
  -n MAX_ROWS  Maximum number of rows to print
```

By default, `csvcat` assumes the input file is tab-delimited. When given a file with no other options, `csvcat` will parse and print all rows and columns with equal width (you may need to pipe the output to `less -S` if it's wider than your terminal screen);

```
(agent47@debian) ~> cat test.csv
name  age nationality
Umar  85  Nigeria
Fahad 33  Egypt
John  40  Unknown
Mike  some invalid val  Some unknown loc
(agent47@debian) ~> csvcat test.csv
name    age                nationality
Umar    85                 Nigeria
Fahad   33                 Egypt
John    40                 Unknown
Mike    some invalid val   Some unknown loc
```

Add custom column border with `-b`;
```
(agent47@debian) ~> csvcat test.csv -b '| '
name   | age               | nationality       |
Umar   | 85                | Nigeria           |
Fahad  | 33                | Egypt             |
John   | 40                | Unknown           |
Mike   | some invalid val  | Some unknown loc  |
```

Print only a certain columns with `-m`;
```
(agent47@debian) ~> csvcat test.csv -m 1,3
name    nationality
Umar    Nigeria
Fahad   Egypt
John    Unknown
Mike    Some unknown loc
```

Ignore some columns with `-i`;
```
(agent47@debian) ~> csvcat test.csv -i 1
age                nationality
85                 Nigeria
33                 Egypt
40                 Unknown
some invalid val   Some unknown loc
```

Disable pretty-printing with `-r`. Useful for re-organizing CSV files like in this example that selects 2 columns, re-arranged them, and changed the delimiter to a comma;
```
(agent47@debian) ~> csvcat test.csv -m 3,1 -r | sed 's/\t/,/g'
nationality,name
Nigeria,Umar
Egypt,Fahad
Unknown,John
Some unknown loc,Mike
```

Skip some rows using `-s`;
```
(agent47@debian) ~> csvcat test.csv -s 1
Umar    85                 Nigeria
Fahad   33                 Egypt
John    40                 Unknown
Mike    some invalid val   Some unknown loc
```

Define the maximum number of rows to print using `-n`;
```
(agent47@debian) ~> csvcat test.csv -n 2
name   age   nationality
Umar   85    Nigeria
```
