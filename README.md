# **Download raw sequence data from GEO/SRA**

##### The info below were taken from the following sources

[NCBI SRA toolkit Documentation](http://www.ncbi.nlm.nih.gov/Traces/sra/sra.cgi?view=toolkit_doc)

[NCBI SRA toolkit Configuration](https://github.com/ncbi/sra-tools/wiki/05.-Toolkit-Configuration)

[Robs manual for the computational genomics and bioinformatics class](https://linsalrob.github.io/ComputationalGenomicsManual/Databases/SRA.html)

The Sequence Read Archive SRA stores raw data in binary files, and provides a toolkit to extract that data into fastq files. Data in the SRA is organized into a series of metadata tables. For more information about the definition of each SRA accession prefix, see [here](https://www.ncbi.nlm.nih.gov/books/NBK56913/#search.what_do_the_different_sra_accessi). The runs (SRR) are where the actual DNA sequences reside and are what you want to download from NCBI, using the [SRA toolkit](http://www.ncbi.nlm.nih.gov/Traces/sra/sra.cgi?view=toolkit_doc).

### Where to find SRR accession codes
Go to GEO, and find the dataset of interest (GEO accession:GSE…). In “Relation”, you can find the SRA project number SRP. Click on it, and you have a list of the different samples. You can select the ones you are interest to downlowad the runs from and send them to run selector, from there you can retrieve their SRR code.


### Download the Toolkit from the SRA website

* If you are using a web browser, you can downlad the toolkit [here](https://www.ncbi.nlm.nih.gov/Traces/sra/?view=software)

*	If you are working from a command line interface: 
>ftp://ftp-trace.ncbi.nlm.nih.gov/sra/sdk/current


### Unpack the Toolkit
* For Linux, use tar:
>tar -xzf sratoolkit.current-centos_linux64.tar.gz

* For Mac OS X, double-click on the .tar.gz file and the Archive Utility will unpack it. Alternatively, command-line tar will also work (see Linux example above)

* For Windows, either use an archiving and compression utility (e.g., Winzip, 7-Zip, etc.), or simply double-click on the .zip file and drag the 'sratoolkit...' folder to the preferred install location.


### Configure and Test the Toolkit

The Toolkit comes with a default configuration that will dowload the repository into the default location which is:

* Linux: /home/[user_name]/ncbi/public
* Mac OS X: /Users/[user_name]/ncbi/public
* Windows: C:\Users\[user_name]\ncbi\public

You need to create such directory and it needs to be empty.
From the "bin' subdirectory of the sratoolkit folder, run the following command:

>./vdb-config -i

A videoscreen will open, where you can navigate by typing the letters in red. Go to Cache. Enable local file caching should be thick by default. The location of the user-repository is set to default (see above). You can change it by navigating to the target directory, or if you know the path, you can select Goto and type the path directly.
In the example below, I set up my user-repository to a "public" subdirectory of the "ncbi" directory in the sratoolkit folder: 

>/Users/...../sratoolkit.2.10.5-mac64/ncbi/public/


To test the toolikt, go to the "bin" subdirectory of the toolkit and dowload the SRR390728 (as example) run files the following command:

* Linux and OS X users:
>./fastq-dump -X 5 -Z SRR390728

* Windows users:
>fastq-dump.exe -X 5 -Z SRR390728


If it works, you can start using it. Once you found the SRR accession codes (see above), I would suggest to prefetch the sra files to you local repository, and to extract the fastq files to an external server, in order to avoid space problems.
From the "bin' subdirectory of the sratoolkit folder, run the following command:

>./prefetch SRR6294675

This will result in the SRR6294675.sra file to be downloaded in your local repository (the "public" subdirectory of the "ncbi" directory in the sratoolkit folder in the example above)

>./fastq-dump --split-files /Users/...../sratoolkit.2.10.5-mac64/ncbi/public/sra/SRR6294675.sra -I --gzip --outdir /Volumes/...../ncbi/public/fastq

This will result in 2 fastq files downloaded in your selected folder on the external server.


[Here](https://ncbi.github.io/sra-tools/fastq-dump.html) you can find the different options of the fastq-dump tool.

Sometimes, you need to retrieve may SRR runs from the same SRA file. In that case, it is worth to create a small program to do it automatically. In the example below, I am retrieving the sra files from SRR6267329 to SRR6267370 and downloading them in my default "public" folder:

>for (( i = 29; i <= 70; i++ ))
>
>do
>
>./prefetch SRR62673$i
>
>done

And with the following:

>for (( i = 29; i <= 70; i++ ))
>
>do
>
>./fastq-dump --split-files /Users/..../sratoolkit.2.10.5-mac64/ncbi/public/sra/SRR62673$i.sra -I --gzip --outdir /Volumes/...../ncbi/public/fastq/GSE106670
>
>done

I am retrieving the fastq files from SRR6267329 to SRR6267370 and downloading them on the selected folder on the external server.

