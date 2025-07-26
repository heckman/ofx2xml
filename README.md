# ofx2xml

This small utility will read an OFX file and print it as valid XML.

Beginning with OFX version 2.0, OFX files are valid XML files.
Many banks, however, use an older version of OFX (or QFX) based on SGML.

This utility remarshals the OFX data
regardless of the version of the provided OFX file,
so even when the provided OFX file is already valid XML,
the output produced will not necessarily be identical to the file.

The OFX data is remarshalled using the [OFXGo](https://github.com/aclindsa/ofxgo) library,
which can do so much more than this simple conversion.
Check it out if you need more OFX magic.

## QFX

QFX files are OFX files with the addition of some Intuit extensions.

Before passing the file contents to the [OFXGo](https://github.com/aclindsa/ofxgo) library for interpretation,
this utility will convert any _`<INTU.BID>`_ tag it finds into a _`<FI><FID>`_ tag pair.

The value of `<INTU.BID>` is an Intuit Bank ID; the values for North-American institutions can be found here: <https://ofx-prod-filist.intuit.com/qm2400/data/fidir.txt>). The `<FID>` tag is described on page 56 the [OFX v2.3 specification](https://www.financialdataexchange.org/common/Uploaded%20files/OFX%20files/OFX%20Banking%20Specification%20v2.3.pdf#page=56).

I'm only aware of one other QFX-specific tag: `<INTU.BROKERID>`;
it is not supported by this utility.
If you have suggestions about how to handle it,
or any other QFX-specific tag,
please let me know.

## Installation

Requires go \>= 1.17

```shell
go install github.com/heckman/ofx2xml@latest
```

This will install the utility to `$GOBIN/ofx2xml`,
or, if GOBIN is not set, to `$GOPATH/bin/ofx2xml`,
or, if GOPATH is not set, to `$HOME/go/bin/ofx2xml` on Macs and other Unix-like systems,
and to `%USERPROFILE%\go\bin\ofx2xml` on Windows.

## Usage

To print OFX file as valid XML:

```shell
ofx2xml <FILENAME>
```

## Intent

I wrote a script ([ofx2](https://github.com/heckman/ofx2)) to process the QFX files provided by my banks,
which are all stuck using the older SGML format.
After using this utility to convert them to valid XML,
my script can then parse the data using other command-line utilities.
