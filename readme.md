xlripper-test
=============

## Purpose

This repository serves as a submodule to `xlripper` which is a minimalistic xlsx parsing library in Go. The `xlripper-test` repo holds test files that are too large to ship with the main repo. The main repo can optionally use git submodule functionality to pull `xlripper-test` and expand the test suite to include these larger files.

## Structure

Each xlsx test file should be represented by a cluster of files at the root of the repository, for example:

```
somefile.xlsx
somefile.meta.json
somefile.sheet0.csv
somefile.shhet1.csv
```

The test will automatically parse the xlsx file. It will use the information in `somefile.meta.json` to discover the sheet names. It will compare its extracted data and sheet names against the data provided in `somefile.sheet0.csv` (and additional sheets if they exist in the xlsx).

In the event that the test xlsx file is expected to fail parsing (for example, if the file is corrupt) then the JSON will indicate this and sheet.csv files are not necessary.

## JSON Meta Structure

Each test xlsx file should include a json file with metadata. If the xlsx file is named `myfile.xlsx` then the metadata file should be named `myfile.meta.json`. The json file should look like this:

```
{
  "name": "myfile.xlsx",
  "description": "file created with Libre Office",
  "is_failure_expected": false,
  "sheets": [
    "Sheet1"
  ]
}
```

* `name` is the xlsx filename, string
* `description` should tell us where the file came from or what is special about it, string.
* `is_failure_expected` should be true if the xlsx file is corrupted or unparseable, boolean.
* `sheets` A list of the sheets found in the xlsx file, array of strings in the correct order.

In the example above, we are declaring that the xlsx file has two sheets. Both of these sheets should be exported as separate csv documents using Microsoft Excel.

The test will then assert that `myfile.sheet0.csv` has the name `My Sheet 1` and that we correctly parsed the data from that sheet.

Similarly the test will assert that `myfile.sheet1.csv` has the name `Another Sheet Here` and that we correctly parsed the data from that sheet.

Notice that the sheet filenames and index arrays are zero based even though programs like Microsoft Excel typically give then default names with 1-based suffixes.

## Contributing

We need your help! We are looking for test failures of valid xlsx files from the wild. If you experience a failure with `xlripper` then this is the place to submit your failing file. Note that if you open your file with Microsoft Excel or another standard program in order to anonymize the data, then re-save it, you may strip the file of the uniqueness that caused it to fail.