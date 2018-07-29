# ReadDecisionServer Zeuss

Align a short read against an indexed reference genome and process alignment positions against a mask

EXPERIMENTAL ALPHA VERSION

# Setup

This project depends on BWA (https://github.com/lh3/bwa, to generate the index files) and JBWA (https://github.com/lindenb/jbwa, to provide Java JNI mappings for BWA).
This repository comes with pre-compiled binaries for Linux in the /store dirctory.

The JBWA project provides a file named `libbwajni.so` which is required to run the server (it is in the /jni folder). This file must be in the in the `java.library.path`, or it must be specified via `-Djava.library.path={path}` upon starting the server (see example below).
I tend to copy that file into a new directory `/usr/lib/jni` and then the `-Djava.library.path` option is not necessary.

You can build it from source using `ant package-for-store` and this will overwrite the jar file in /store.

# Start

The compiled server can be started:
`java [-Djava.library.path={path to libbwajni.so}] -jar Zeuss.jar -l {path to index}`

This will start the server on port 9221 and create a mask file in the current directory.

There are 4 possible options:
'-p'  port (default 9221)
'-c'  cores (default 4*available system cores)
'-l'  path&prefix to genome index files
'-m'  path to mask file(s) (default .)

# Queries

The only URL currently availble is '/v1/proc' ("proceed - yes or no"). The sequence to me mapped is provided a query parameter 'seq'. For example:

`curl http://localhost:9221/v1/proc?seq=CTCTATTATTAATACTTCTTTTGAAGCTGCAGTTGTTGCTTCTACTTCAACATTAGAATTAATGTGTA`

This curretly returns a JSON response: (for example) `{"Pos":[120462618],"MaskVal":[0]}`
This lists the alignment position, and the mask value at that position.
