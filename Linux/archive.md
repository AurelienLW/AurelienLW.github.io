# Archive    
    
    sudo tar cvf mybackup.tar /home/cloud_user/toarchive/ 

c for create v for verbose f for file

    sudo tar cvfz  mybackup.tar.gz /home/cloud_user/toarchive/

z for zip

    tar tf  mybackup.tar 

t for tar f for file

    tar tf  mybackup.tar.gz

    tar xvfz mybackup.tar.gz 

x for extract v verbose f file z zip
creates a cloud user in home/

in it theres the two files we extracted
 the command extracts in the directory we working in

    tar xcfz mybackup.tar gz --directory=toredstore

home -> toredstore -> its here