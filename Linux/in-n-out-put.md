# input output redirection

stdin stdout stderr

standard input
sandard output 
standard error


    ls -l /etc
    ls -l /etc | less
    cd /shop
    ls -al
    cat category.csv | sort | head -10

    ls -l /etc > etclist.txt
    cat ectlist.txt

    ls -l / > etclist.txt


    ls -l / 
    ls -l / >> list.txt
    cat list.txt
    ls -l /etc >> list.txt

file not deleted bcs it can create and append instead of replace


    less < category.csv
    ls -l 

    cat varlist 2>error.txt
    cat error.txt

    cat varlist 2 /dev/null
    ls /dev/null
    cat /dev/null

