# templates
This is a collection of oddgen generators and associated template files

## Files
This repository centers around a series of files.

| File | Description |
| --- | --- |
|README.md|this file.  You should also find a `README.md` file for each collection.
|ctan.xsd|defines both `ctan_index.xml` files and `MANIFEST.xsd` files
|ctan_index.xml|a collection of `MANIFEST.xml` found within this repository.  Updating of this file is not yet automated.
|MANIFEST.xml|Describes which files are needed for which version.  This is primarily for computer consumption.

## Collections
Each collection constists of a series of files.

| File | Description |
| --- | --- |
|README.md| A readme file describing the generator
|MANIFEST.xml| An XML describing which file(s) belong to which collection(s).  This is for automation tools like CTAN.
| *.pks/*/pkb | (optional) PL/SQL code for oddgen Database Generators
| *.java | (optional) for oddgen Client Generators
| *.xtend | (optional) files needed for eXtend engine
| *.texml | (optional) files needed for tePLSQL engine
| * | (optional) other files specific for a generator/engine

## How to help
Your assistance with expansion of this repository is greatly needed.

To assist, please:
- fork this repository
- add your generator to the mix
  - include a README.md
  - include a MANIFEST.xml based on ctan.xsd
- update ctan_index.xml based on ctan.xsd (to be automated)
- do a pull request

If you have a question, please ask as an Issue.

## directory structure
The directory structure should follow this design
`/{class}/{repository name}/`

Where `{class}` is one of the following:

| `{class}` | Description |
| --- | --- |
|DD|Data Dictionary API specific utilities
|TAPI|Various Table API generators belong her
|XAPI|Transactional API generators.  Thes span multiple tables and multiple DMLs. eg a Document Store where Version is a "gap-free sequential number"
|DBA|DBA utilities should exist here.
|developer|PL/SQL developer toolkits such as "Make AQ queue from table", or "create ODCIAggregat function for UDT"
|experimental|interesting code.
|examples|Example collections

Please post an `enhancement` request if you wish for a new `{class}`  before you do a Pull Request.

## MANIFEST.xml
The MANIFEXT.xml explains which files are needed for each specific version of your generator and must conform to the `ctan.xsd` that is found elsewhere in this repository.

You can use SQL*Developer to create/modify your `MANIFEST.xml` (and edit the `ctan_index.xml` file prior to auto-update).

The `<manifest>` describes all of the available versions of the generator that are available. It contains a `<description>` of the generator, a single list of `<versions>`, an optional list of `<tags>`, and a set of `<requirements>`.

The attributes for the `<manifest>` help descripe the collection itself.

| Attribute | Description |
| --- | --- |
|guid|this should be a globaly unique identifier.  eg `select SYS_GUID() from dual` should do well.
|name|This is a shortname used for display by applications
|path|This is a URI that describes the path to this collection relative to the repository's `/ctan_index.xml` file.  As such, it is only needed in the `ctan_index.xml` file.  (automation is expected to update this value)

The `<desription>` element should be a plain text description of the collection.  This is used as a "tooltip" from within CTAN-like applications.  Details of the collection should be found in the `README.md` file (which can be version specific.

Each `<version>` contains a list of `<file>`s that are specific to that version in addition to a list of `<requirements>`.  Each version should contain a single `<version>` entry.  If you have a multi-engine generator, than you will need to define a `<version>` for each engine.
eg, you will need to define one `<version>` for the `FTLDB` engine and another `<version>` for the `tePLSQL` engine.

Attributes for each `<version>` are:

| Attribute | Req? | Description |
| --- | --- | --- |
| engine | required |describes code generation engine this version uses (eg `FTLDB`, `tePLSQL`, `eXtend`, `plsql`, etc.).  This helps with searching. |
| major | required | the `2` in a version string such as `2.3.0 XP1` |
| minor | required | the `3` in a version string such as `2.3.0 XP1` |
| sub | required | the `0` in a version string such as `2.3.0 XP1` |
| patch | optional | the `XP1` a version string such as in `2.3.0 XP1`.  sort order is String based. |
| rdbms | optional | limits the version to a specific RDBMS. default is `oracle`.  This helps with searching |

Each `<file>` is a URI releative to the location of the `MANIFEST.xml` found within the collection.  For clarity, the relative path should start with "current directory" eg `./README.md`.

Each `<file>` can be reused in multiple `<version>`s.  This is especially usefule for the `README.md` file.

The attributes for `<file>` are

| Attribute | Description |
| --- | --- |
|type|This describes how search+retrieval applications should process the file |
|process_order|This describes in which order to process the file. |

The values for `type` can be.

| Type | Description |
| --- | --- |
|readme| the README file for this version |
|sql|run SQL |
|ignore|files that are ignored (eg `*.java` files are `ignore` while their `*.ant` file is used to compile it |
|(others)|any tag can be uses.  Various search+retrieval applications could define how these are treated. |

The `<requirements>` of a `<version>` is described by a single `<requirement>` entry.  This tag is not yet fully defined.

`<tags>` contains a list of `<tag>`s which are just simple strings that can be used for searching for that perfect collection.
eg `<tag>TAPI</tag>`

## ctan_index.xml
This file is a directory listing for this repository.
It should be an aggregation of all the `MANIFEST.xml` files that have their `/manifest/path` value update to be the location where the file was found (with respect to the `ctan_index.xml` file)

This is primarily for search+retrival applications.

