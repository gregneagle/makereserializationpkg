Introduction
============

**makereserializationpkg** is a tool Mac admins can use to create deployment packages that reserialize Adobe CS6 product installs.

**makereserializationpkg** is written in Python and is made available under the Apache 2.0 license.


Usage
=====

**makereserializationpkg** is a Python script. If you mark it as executable, you can invoke it directly; otherwise use the Python interpreter:

`python makereserializationpkg [options] package_pathname`

    makereserializationpkg --leid <PRODUCT_ID> \
                           --serial <SERIALNUMBER> \
                          [--identifier com.example.reserializer] \
                          [--version 1.0] \
                          package_pathname

**makereserializationpkg** creates a package that will reserialize the Adobe product specified by **&lt;PRODUCT\_ID>** with **&lt;SERIALNUMBER>**.
Created package is saved to package_pathname.

Product IDs can be found on Adobe's web site  [here](http://www.adobe.com/devnet/creativesuite/enterprisedeployment/licensing-identifiers.html).

Options
=======

    -h, --help            Show help message.

    --leid=LEID           Adobe product ID. 
                          See http://www.adobe.com/devnet/creativesuite/enterprisedeployment/licensing-identifiers.html
                          for a list of product IDs.

    --serial=SERIAL       Serial number to use.

    --identifier=IDENTIFIER, --id=IDENTIFIER
                          Optional. Package identifier for the package. Defaults
                          to "com.example.reserializer.pkg"

    --version=VERSION     Optional. Version number for the generated package.
                          Defaults to "1.0".
                          
Requirements
============

**makereserializationpkg** requires an Internet connection to download the Adobe APTEE tools.

It also uses `/usr/bin/pkgbuild` to build the final package. This available by default on Lion and Mountain Lion. On Snow Leopard you can obtain it by installing Xcode 3.2.6 or later.

What it does
============

**makereserializationpkg** downloads the Adobe Provisioning Toolkit Enterprise Edition (APTEE) tools from Adobe. See [here](http://www.adobe.com/devnet/creativesuite/enterprisedeployment.html) and [here](http://wwwimages.adobe.com/www.adobe.com/content/dam/Adobe/en/devnet/creativesuite/pdfs/Adobe_Provisioning_Toolkit_Enterprise_Edition_v5.pdf) for more info on these tools.

It uses these tools and the provided options to build a "payload-free" package; that is, a package that installs no files but exists only to run a script.

The script is this:

    #!/bin/sh
    
    LEID=<provided LEID>
    SERIAL=<provided serial number>
    
    SCRIPTDIR=`/usr/bin/dirname $0`
    
    "${SCRIPTDIR}/adobe_prtk" --tool=UnSerialize --leid="${LEID}"
    "${SCRIPTDIR}/adobe_prtk" --tool=Serialize --leid="${LEID}" --serial="${SERIAL}" --regsuppress=ss --eulasuppress
    
The created package is saved to the pathname given at the command line.

This package should be installed only on the current startup disk.




