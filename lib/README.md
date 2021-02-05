This file explains how to download and install required libraries for WW3_GRIB. 

WW3_GRIB requires the following libraries
GRIB2 encoder (gw2lib-3.2.0.tar)
GRIB1 encoder (w3lib-2.2.0.tar)
BACIO - Fortran Interface to C (bacio-1.3.tar)
JASPER - JPEG Image compression (jasper-1.900.1.zip)
PNG - PNG Image compression (libpng-1.2.34.tar)
ZLIB - compression routines for PNG (zlib-1.2.6.tar)

All libraries can be downlaoded from the following NCEP page: 
https://www.nco.ncep.noaa.gov/pmb/codes/GRIB2/

###### **INSTALL DIRECTIONS**
Download all library packages to /lib directory
Unzip and Install each package with make 

Install image compression libraries: (jasper,png,zlib)
./configure
make
sudo make install


**G2 Library:**

edit makefile
modify INCDIR to point to include files for PNG and JASPER libraries installed above
ex: INCDIR=-I/usr/local/include/jasper -I/usr/local/include/libpng12
modify: FC and CC 
ex: FC=ifort, CC=gcc

Then: 
make

A successful make will result in libg2.a file in working directory

**W3LIB Library:**
edit Makefile options for compiler and compiler flags
for me: CC = gcc

THen:
make

A successful make will result in libw3.a file in working directory

**

**BACIO LIBRARY**
edit makefile for compiler and compiler flags
for me: 

CC=gcc
FC=ifort
LIB=libbacio.a
INC=clib.h
FFLAGS= -O3 
\#AFLAGS= -m64
CFLAGS=-O3 

Then: 

A successful make will result in libbacio.a file in working directory
 

###### **COMPILE WW3_GRIB executable**
Modify switch file. Make sure NOGRB is replaced with NCEP2
Modify link file to link to the appropriate recently compiled libaries. 

Example: 
  if [ "$ncep_grib_compile" = 'yes' ]
  then
      G2_LIB4=/home/ec2-user/ww3/lib/g2lib-3.2.0/libg2.a
      W3NCO_LIB4=/home/ec2-user/ww3/lib/w3lib-2.2.0/libw3.a
      BACIO_LIB4=/home/ec2-user/ww3/lib/bacio-1.3/libbacio.a
      JASPER_LIB=/usr/local/lib/libjasper.a
      PNG_LIB=/usr/local/lib/libpng12.a
      Z_LIB=/usr/local/lib/libz.a
    opt="$opt -convert big_endian -assume byterecl -prec-div -prec-sqrt"
    libs="$libs ${G2_LIB4} ${W3NCO_LIB4} ${BACIO_LIB4} ${JASPER_LIB} ${PNG_LIB} ${Z_LIB}"
  fi

now compile ww3_grib in ~/ww3/model/bin directory: 
w3_make ww3_grib

Important to note that other ww3 executables can use the NOGRB switch, which may allow for more efficient executables. 
Hasn't been tested.  


