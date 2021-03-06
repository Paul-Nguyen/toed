Toed
====

What is this
------------
**T**ool for **O**xygen **E**nterprise **D**iagnosis.

Oxygen Enterprise (oxygencloud.com) doesn't correct sync issue errors very well, and has not for a long time. This tool uses the Oxygen REST API to check your local dir against the remote, e.g. it will tell you if there are differences between what is synced on your hard drive with what is on the servers. If users already have an API acccount, then rolling this out as a support tool would be easy and worth while, but it's not, so it's more of a tool that I used to get my own Oxygen Cloud account synced properly before I decommissioned my laptop.

Ideally, this is not required, and Oxygen Cloud should add a function to traverse the local directory and compare to the server and fix itself.

Discrepancy types:
- Files exist remotely, but aren't here locally. These are just flagged and output as errors.
- Files exist locally, but aren't on server. Flag the parent directory as ready for a bump. This won't help if "~$blah.docx" and "*.tmp" files are the ones missing, since Oxygen ignores these. These are ignored.
- Files exist on both, but timestamps don't line up. Flag the parent directory as ready for a bump. 
- Files exist on both, but filesizes don't line up. Flag the parent directory as ready for a bump. 

We 'bump' the folder when "--fix/-f" is called by placing or deleting a file here and forcing Oxygen to reread the directory. This has solved directory sync issues where files have not been uploaded from a folder for months.

What Do I need
--------------
- An Oxygen API key. An Oxygen Admin can ask for one of these. Drop these in a .api file in the current directory, in a text file in form:

         KEY:somekeyhere019i234123341
         SECRET:somesecrethere98209820982098
- An Oxygen API username/password. You set this for your username via My Applications in the Oxygen webministration page. Drop it in a plain text file named '.user' in the form:

         id:youruserid@youraddress.example.com
         password:yourpasswordhere
- This is in Python 3. Use Python 3.
- Some python modules. Specifically hmac is the most obscure one. Check out the requirements.txt file.

This was also whipped up on a WINDOWS box. So beware windows-isms.

How Do I use it
---------------
Setup your .api and .user as above.
        
        ./toed.py
Running it will go through your spaces, directories, then files and error to screen if there are discrepancies.

Running it with -f or --fix will attempt to fix things when remote files are missing. Potentially a -f type fix might work with timestamp/filesize issues, but there is a danger here of overwriting good commits and I ran out of time to think about it.

Running it with --verbose will show you that it's actually doing something.

Running it with --debug tells all.

Deploy binary
-------------
There was intention to deploy this via windows with a dist/exe but that depends on rolling out of API accounts and has been messy. 

So in the meantime, there is enough there to start it but this has not been field tested. There is a setup.py file -- run as python setup.py build2exe if you want to generate a windows dist/ and executable.

Don't sue me because
--------------------
This is a quick hack. I don't even have test suites on it. I started turning it into a class but finished employment where I needed it and didn't get to revisit it.

If you run it with -f, it should only fix up directories and won't really overwrite anything (since on the server you already have a copy, and it'll cause things to push up and not down), but it will try upload a .paulnguyenfix file to trigger some changes if the remote is missing files.

Copyright
---------
2015 - Paul Nguyen
