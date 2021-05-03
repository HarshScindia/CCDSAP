# This is example of shell scripts or commands

```sh
ls -1 @(Archive|Backup)-[0-9][0-9][0-9][0-9]-[0-9][0-9]-@([0-9|[0-9][0-9])-@(.bak.bz2|bak.gz|bak.xz|.tar|.tar.bz2|.tar.gz|.tar.xz|.tgz)
# Get a list of all file extensions in a directory
for i in * ; do echo ${i#*.} ; done | sort -u

# Below is a more compact version of the ls command
ls -1 # Lists the contents of a directory in a single coloumn
# using shell globs we can match almost each and evey file in a dirctory
ls -1 @(Archive|Backup)-[0-9][0-9][0-9][0-9]-[0-9][0-9]-@([0-9]|[0-9][0-9])@(@(.bak|.tar)?(.bz2|.gz|.xz)|.tgz) | wc -l
``` 

# Using globs to edit file i.e rename copy or mv files
```sh
# Check the command if it is correct
for i in @(Archive|Backup)-[0-9][0-9][0-9][0-9]-[0-9][0-9]@(@(.bak|.tar)?(.bz2|.gz|.xz)|.tgz) ; do echo ${i%%.*}-1.${i#*.} ; done
# Using 'mv'  to rename the files
for i in @(Archive|Backup)-[0-9][0-9][0-9][0-9]-[0-9][0-9]@(@(.bak|.tar)?(.bz2|.gz|.xz)|.tgz) ; do mv -v $i ${i%%.*}-1.${i#*.}; done
```

# Using the above command in a multiline for loop format
```sh
for i in @(Archive|Backup)-[0-9][0-9][0-9][0-9]-[0-9][0-9]@(@(.bak|.tar)?(.bz2|.gz|.xz)|.tgz)
do
    echo ${i%%.*}-1.${i#*.}
done
# using mv to rename the files
for i in @(Archive|Backup)-[0-9][0-9][0-9][0-9]-@(@(.bak|.tar)?(.bz2|.gz|.xz)|.tgx)
do
    mv -v $i ${i%%.*}-1.${#*.}
done
```
# Files that start wih IMG or DSC but Do Not end in .CRW
```sh
# check syntax of case statement is there '))' in the end.
# we cannot use regular expressions in case statement.
# CASE statement with fall through functionality

for file in * ; do
    case $file in
        @(IMG|DSC)*@(@(.CR2|.NEF).xmp|.NEF|.CR2)) (( ALLRAW++ )) ;;&
        DSC*@(.NEF|.NEF.xmp))                     (( NIKRAW++ )) ;;&
        DSC*@(.NEF.xmp))                          (( NIKSUP++ )) ;;
        IMG*@(.CR2|.CR2.xmp))                     (( CANRAW++ )) ;;&
        *)                                        (( OTHER++  )) ;;
    esac
done
```

# Extended Globs in Pattern Substitution 
```sh
file=Archive-2016-07-01.tar.gz
echo ${file%%.*}
# Adding more power to pattern substition using extended globs
echo ${file%*}
echo ${file%%@(.tar|.bak)*}
```
# Code to output all file extensions in a directory with output
```sh
[devops@localhost testfiles]$ for i in * ; do echo ${i#*.} ; done | sort -u
bak.bz2
bak.gz
bak.xz
tar
tar.bz2
tar.gz
tar.xz
tgz
[devops@localhost testfiles]$
```

# Match all Archive|Backup files except for the files from 2015
```sh
ls @(Archive|Backup)-!(2015)-[0-9][0-9]-*@(.gz|.xz)
```

# Pattern Matching with extended globs
```sh
ls -1 @(Archive|Backup)-[0-9][0-9][0-9][0-9]-[0-9][0-9]-@([0-9]|[0-9][0-9]).@(@(bak|tar)?(.bz2|.gz|.xz)|tgz) | wc -l
```

# Using extended globs with commands
```sh
ls -1 !(*@(Archive|Backup)-[0-9][0-9][0-9][0-9]-[0-9][0-9]-@([0-9]|[0-9][0-9])@(@(.bak|.tar)?(.bz2|.gz|.xz)|.tgz)) | wc -l
ls -1 @(Archive|Backup)-[0-9][0-9][0-9][0-9]-[0-9][0-9]@(@(.bak|.tar)?(.bz2|.gz|.xz)|.tgz)
for i in @(Archive|Backup)-[0-9][0-9][0-9][0-9]-[0-9][0-9]@(@(.bak|.tar)?(.bz2|.gz|.xz)|.tgz) ;do echo $i ;done

for i in @(Archive|Backup)-[0-9][0-9][0-9][0-9]-[0-9][0-9]@(@(.bak|.tar)?(.bz2|.gz|.xz)|.tgz) ;do echo ${i%%.*} ;done
# output --> Archive-2017-05
# output --> Archive-2017-05
                                                                                                  # echo file name with changes
for i in @(Archive|Backup)-[0-9][0-9][0-9][0-9]-[0-9][0-9]@(@(.bak|.tar)?(.bz2|.gz|.xz)|.tgz) ;do echo ${i%%.*}-1.${i#*.} ;done
# output --> Archive-2017-05-1.bak.bz2
# output --> Archive-2017-05-1.tar.bz2
```
