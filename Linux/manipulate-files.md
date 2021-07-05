# Create and manipulate files

    ls -al

    cat [name of the file]

more category-files.txt
Good to look at content

    cat category.txt
also displays bunch of info

cat is more flexible


    less category.txt scroll up and down

manage file in real time

    sudo less +F /var/log/syslog

less continues to run bsc of the +F option but ??

    cat category.txt | sort

is gonn sort the file display without making any change of the file

    sort category.txt
    sort -r category.txt reversed sorted (from Z to A)

    sort category.txt > sorted.txt 

    sorted.txt file now has the sorted version of the file saved

touch is mainly used to create a file or to update the accesss of the file


    cat touch.txt
    doesnt exist
    touch touch.txt

    nano nano.txt  //Ctrl+X to close nano

ctrl+K delete line

# Compare files 
Diff


    diff file1.txt file2.txt

tells us the action to make the 2files be the same

    diff -c 

context option

    diff ../shopping/ ../data/shopping/

directory comparison is possible

Comm
only if files are sorted

    cmp files1 files2

first diff between the files

    cmp files1 files2

nothign returned if files are the same
