Seafile uses storage de-duplication technology to reduce storage usage.
Simply put, there would be two implications:

* Different versions of a file may share some data blocks;
* Different libraries may also share some data blocks.

The net result is that the underlying data blocks will not be removed
immediately after you delete a file library. As a result, the number of
unused data blocks will increase on Seafile server.

To release the storage space occupied by unused blocks, you have to run a
"garbage collection" program to clean up unused blocks on your server.

**Before running GC, you must shutdown the seafile program on your server.**
This is because new blocks written into Seafile while GC is running may be
mistakenly deleted by the GC program.

Supposed you've setup Seafile server as in [[Download and setup seafile server]],
and all your seafile related data is under "~/haiwen" directory, you should have
a directory layout similar to the following:

<pre>
   haiwen
     --seafile-server-{version} # untar from seafile package
     --seafile-data   # seafile configuration and data (if you choose the default)
     --seahub-data    # seahub data
     --ccnet          # ccnet configuration and data 
     --seahub.db      # sqlite3 database used by seahub
     --seahub_settings.py # optional config file for seahub
</pre>

To run GC program

    cd ~/haiwen/seafile-server-{version}
    ./bin/seafserv-gc -c ../ccnet -d ../seafile-data

If you [[built seafile server from source|Build and deploy seafile server from source]],
just run

    seafserv-gc -c /data/haiwen/ccnet -d /data/haiwen/seafile-data

After the GC program terminates, you may also check whether it mistakenly removed any
useful data blocks:

    seafserv-gc -c /data/haiwen/ccnet -d /data/haiwen/seafile-data --verify

It will print a warning if any useful blocks are missing. Normally it prints nothing ;)