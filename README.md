# Overview

If you are developing your own plugin for Fuel, you will also need to prepare the documentation set,
which includes Test Plan, Test Report and Plugin Guide.

## How to use

This repo is organized as the doc tree with 2 main folders:
- plugin guide
- testing documentation
  - Test Plan
  - Test Report

To use these doc templates, follow these steps:

1. Clone the repo:

   `git clone https://github.com/irinapovolotskaya/fuel-plugin-docs.git`
  
2. Populate the placeholders of the conf.py files (for Plugin Guide, Test Plan and Report) with plugin-specific information (e.g. document name, plugin release).

3. Populate the content of RST files which make up the document structure.

## How to build documentation

Once you're done with editing the conf.py and sample RST files, you should cd into the corresponding doc dir and
run `make latexpdf`.

For example:
```
cd plugin guide
make latexpdf
```

The PDF will be found in /build subdir.



