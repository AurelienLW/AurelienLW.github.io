# linux cheat sheet to find stuff/filter file system, ti can all be applied to other commands i.e. 

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