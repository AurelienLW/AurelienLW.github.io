# filter folders

    for file in $(ls); do echo $file; done

    ll
    ls -l
    ls -a
    ls | grep hist
    ls | sort
    ls | cut *.txt
    ls | cut -d'.' -f 1
    ls | cut -d'.' -f 2
    ls | cut -d'.' -f 2 | u
    ls | cut -d'.' -f 2 | unique
    ls | cut -d'.' -f 2 | uniq
    ls | uniq
    ls | cut -d'.' -f 2 | sort | uniq

# find files tools 

find -name "search.txt"
find /name "search.txt"



    find / -name "search.txt"

it gives us the directory

    find /etc -name "search.txt"

case sensitive desactivated with 

    sudo find /etc -iname "search.txt"

now result without case sensitive parameter

type option let us look for other types of things

    sudo find / -type f  -name "*.log"

c input devices
d directories
l links
l files

    find /etc -type f -user cloud_user
    ls -l /etc/search.txt

gives us the directory and the permission with owner of files

    man find 

man command gives helps about the find commands

    locate "search.txt"

doesnt work bcs locate needs to know where the file is

    sudo updatedb
    locate "search.txt"
    locate -i "search.txt" for case insensitive search


# which, where is and type


## which 

    which python

gives us where python binaries are located

    echo $PATH

    which nano
    /bin/nano

    whereis

gives a lot more, locations of binaries, man pages, sources files for python

    whereis python python | tr " " '\n' 

gives us a much better view of it

type python is sort of the same of which but also with command types

    type ls

ls is aliased to 'ls --color=auto'

which can help checking why a command isnt working