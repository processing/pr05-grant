# Final Report for Simplifying the workflow for Processing libraries

| **Project** | [Simplifying the workflow for Processing libraries, tools, and modes](https://github.com/processing/pr05-grant/wiki/2024-Project-List-for-%60pr05%60-=-Processing-Foundation-Software-Development-Grant#%F0%9F%85%BF%EF%B8%8F-simplify-the-workflow-for-processing-libraries-tools-and-modes) | 
| :--- | :--- |
| **Grantee** | [Claudine Chen](https://github.com/mingness) |
| **Mentor** | [Stef Tervelde](https://github.com/Stefterv) |
| **Repos**| https://github.com/processing/processing-library-template <br> https://github.com/processing/processing-contributions <br>documentation repo: https://github.com/mingness/pr05-simplify-workflows|

## Introduction
This project's aim was to improve the developer experience for contributing
libraries to Processing.
The work from this project manifests as 2 separate projects, as well as improved 
documentation. These two projects are the processing library template, the new 
contributions workflow, and I will write about them in separate sections.

There were two main guiding principles in this work:

1. automate what can be automated
2. make the interactions as intuitive and self-explanatory as possible.


## Project Plan
Below is a broad overview of the process of contributing to libraries, tools, and modes, and the initial resources for this. The work accomplished or <span style="color: grey">planned</span> to be accomplished
is in the final column.

| Step | Previous workflow | Current workflow |
|------|------------------|-------------------|
| The idea | | <span style="color: grey">An invitation to contribute, via a tutorial on the website</span> |
|The information | Information on how to develop a library can be found in the Github wiki documents for processing 4, in three separate pages, and then there is a page for modes. <br><br> https://github.com/benfry/processing4/wiki/Library-Basics <br> https://github.com/benfry/processing4/wiki/Library-Guidelines  <br> https://github.com/benfry/processing4/wiki/Library-Overview  <br> https://github.com/benfry/processing4/wiki/Mode-Overview | <span style="color: grey">Update information in tutorial on website, and in repository for library template.</span> |
| The development | There are a series of templates that have been developed. The older ones are using Eclipse and Ant, newer ones with Intellij and Gradle. In addition to the executable, the developer is required to create a website that holds documentation for the library. <br><br> https://github.com/processing/processing-library-template <br> https://github.com/processing/processing-library-template-gradle <br> https://github.com/processing/processing-templates <br> https://github.com/processing/processing-tool-template <br> https://github.com/processing/processing-android-library-template | Repository templates for libraries using the new Gradle templates, keeping in mind the needs of the beginner. Consolidate existing functionality: resolution of Processing core using jitpack and unofficial Processing repo`; automation for building and exporting artifacts, including PDEX/PDEZ bundles. <br><br> CI/CD deployment of the reference to GitHub pages <br><br> Simple integration with local Processing |
| The integration - discoverability | The current process is to contact the Processing librarian with a url to your release. <br><br> https://forum.processing.org/one/topic/publishing-contributed-libraries-and-tools <br><br> The librarian then manually updates the private repository  https://github.com/processing/processing-contributions  | Submissions of new contributions via an issue template. <br><br> Develop automated pipeline that updates the repository from the properties file, with the objective to reduce the workload on Processing librarian. <br><br> This also includes a refactor on how contributions are stored, storing all information in a database file in the repository. |



## Processing Library Template

This project's target was to create an as-intuitive-as-possible, Gradle-based library 
template. The existing `processing-library-template` was based on Ant and Eclipse, 
and both technologies that have been superceded by Gradle and Intellij/VS Code, 
respectively. The existing `processing-library-template-gradle` seemed to take a 
design approach of eliminating any interactions by the user with Gradle scripts, 
opting instead to have the user input all relevant build parameters into a config 
file. Propagating the information from this config file into Gradle tasks required 
additional code. So while this approach does work, it comes at a cost of readability 
of the Gradle scripts.

We took a different approach, which is to not protect the user from Gradle, but 
let this be an opportunity to learn how to interact with Gradle. We tried to
simplify the interaction as much as possible, and to instruct and explain as much
as possible. In our template, the user will add their library name, domain, and
dependencies into the Gradle file, guided by comments. We also had a principle, to 
define values only once.

The library template is now integrated into the processing account. 
The previously named `processing/processing-library-template` is now named
`processing/processing-library-template-ant`. 
The previously named `mingness/processing-library-template` is now named
`processing/processing-library-template`. 


All features are:

- use of jitpack to resolve Processing, instead of local jar files. 
Once Processing is offered on Maven, this will be changed to resolve via 
official sources.
- A Gradle build file, that
  - asks user to edit build file directly,  to add their own 
dependencies, library name, domain name, and version.
  - provides gradle tasks for releasing the library and all required artifacts
  - installs the library in a local Processing instance.
- A `release.properties` file, where the user will input values for the library 
properties text file. All fields are mapped directly, and the `version` in the 
Gradle build file is mapped to `prettyVersion`.
- The template can be compiled as is. It includes a working example library, and
defaults that can be compiled. This provides a working example to work from. 
- This template provides fully documented example code that can be easily accessed
in Processing from the examples menu.
- It provides a new mkdocs powered documentation website example. This framework 
provides a simple format suitable for documentation websites, based on markdown files.
- The template has a workflow that includes the necessary release artifacts in a Github
release. The default release artifacts only include the source code.


## Adding a New Contribution

This project's objective was to refactor how Processing tracks contributions, 
considering the guiding principles of automation and intuitiveness. A previously 
manual process was automated, and the contributions data was consolidated into 
a database file.

The processing contributions repository is now integrated into the processing account. 
The previously named and private repository `processing/processing-contributions` is now named
`processing/processing-contributions-legacy`. 
There is now a repository named `processing/processing-contributions`, which contains
only the important files of `mingness/processing-contributions-new`.


### Logic of previous repository
The previous process required the Processing librarian to add entries to the 
`source.conf` file for the new contribution according to which categories might be 
associated with the contribution. The listing of the source in this file under the
categories served to override the categories listed in the properties file.
The new entry included the new id number, and the url of the properties file. 
A comment at the top tracked what the next id number should be, and was manually 
iterated. There were also two other configuration files, `skipped.conf` and
`broken.conf`, which contained lists of id numbers. 

The `scripts/build_contribs.py` script would parse these config files, read in all 
the properties files from the internet, and output files used by the website and 
Contribution Manager in the PDE. These files also were a part of the repository.
The `scripts/build_contribs.py` script performed some alterations to the data in
the properties files with the following rules.

- if the id was listed in `skipped.conf`, the contribution was not included
- if the id was listed in `broken.conf`, the contribution's maxRevision field was capped at 228,
which is a previous version of Processing.
- the categories the contribution was listed under would overwrite what was listed
in the properties file.

In short, the data about the contributions were distributed across multiple 
files, including within algorithms, and across the internet. This does not provide 
an intuitive interaction with the data. 


### Logic of this project
Considering the guideline, to make processes and the data and code as intuitive 
and self-explanatory as possible, the refactor was to create a single database 
file that contains all the data related to contributions that we might need. 
From this database file, we can then create the output files. Another way of thinking
about it, during the running of the build script of previous process, all this 
data is in memory; just the state is never recorded more permanently. Previous 
formatting logic that was stored as algorithms in the build script are now stored as data. 

The format of the database file was selected to be a list of objects, in yaml format.
Yaml format allows for ease of understanding any edits in the database. Each edit
will appear per line with the field name, making each edit easy to see using Github's 
interface, compared to tabular formats, like csv. For tabular formats, Github's interface, which shows 
if a row has an edit, would highlight more which contribution had and edit, but the 
edit itself would be harder to see, especially since the column label would be on a 
different line.

To make it intuitive to compare values in the database against the properties file, 
the data in the database directly reflect the data in the properties files. The visibility
of a contribution is set by a new `status` field. If anything needs to be overwritten, 
like the categories, or maxRevision,  this is defined in the field `override`. 

For the `status` field, a value of `VALID` indicates the library is live. There are two 
states if the url is not valid; both of these states result in the library not being
able to be installed. `BROKEN` indicates that the url did not serve the 
properties file as expected, but we will continue to check. This `status` value is 
automatically set if encountered during an update. 
If we know a library has been deprecated, then we can manually set the `status` to 
`DEPRECATED` and updates for library will no longer be attempted. 

The value of the `override` field is an object, where any component field values will 
replace the existing field values. For example, libraries in the `broken.conf` file are 
outdated, and we want to cap the `maxRevision` to `228`. This cap can be applied by setting 
`override` to {`maxRevision`: `228`}

In the new repository, the output files are no longer a part of the repository,
to make source of truth clear - the database file. These output files will instead
reside as workflow artifacts.

### Implementation
There were some difficulties in implementing this solution, simply because, data is messy, and
is rarely as theoretically clean as expected. This work needed to convert logic enshrined in
config files and a script into data. The process of doing this required iterations of trial
and error.

Two different levels of validation are used, where the validation for new contributions is more 
strict than for existing contributions. This is because some older properties files have 
different field names than currently expected. Currently we look for `authors` and `categories`, 
but in older files they might be `authorList` or `category`.  

We implemented validation using Pydantic's built in validation, simply because considering all
the cases manually became too much. Pydantic wasn't used from the start, because it was
intended to write the scripts in a language in the Java family, like Kotlin. However, they
currently are in Python.

### User Interaction

The previous process required the new contributor to email the Processing librarian.
We have replaced this initial contact with an issue form. The issue form makes it 
clear as to what is required to register the library, and can be used to trigger workflows. 

## Workflows
The creation of the issue form triggers a Github workflow, which automatically
retrieves the properties file, parses and validates it, and if valid, creates a 
pull request for the Processing librarian to review. The way the workflow was designed,
was to allow for a rerun of the workflow, in case of edits to the repository. In order to 
do this, the branch is deleted before being recreated again by the workflow.

Also, ensure readability and to use preexisting documentation, use of third-party 
actions was preferred.

The workflow that checks the urls daily needs to merge its changes automatically, without
humen intervention. For this reason, we make direct changes into the default branch `main`.
This workflow will check the url of every contribution that doesn't have a `status` of
`DEPRECATED`, and will update the contribution if the `version` changes. If there isn't
a properties file at the url, the contribution will be marked with a status of `BROKEN`
until it is live again.


# APPENDIX A: The original process for adding contributions
1. The library developer will email the Processing librarian, with the url of the release artifacts.
2. The librarian will feedback on any recommendations for changes to the library developer.
3. The librarian adds the library manually to `sources.conf`. This file lists the categories, under 
which, in each row, is an id number and the url to the properties file (*.txt) file for the applicable 
contribution. These contributions override the categories listed in the properties file.
4. The librarian then runs some scripts to add the library into necessary locations.
    1. https://github.com/processing/processing-contributions/blob/master/scripts/build_contribs.py 
    is run as github action, daily. This generates `pde/contribs.txt` file - used by contributions 
    manager. This script visits the url to the properties file (*.txt), and pulls the values from 
    this file, and updates them in the `pde/contribs.txt` file. If any of the ids are in the  
    `skipped.conf` file, the library is skipped. If any of the ids are in the `broken.conf` file, 
    the `maxRevision` is pinned to 228, which is an older version. These two *.conf files are also 
    manually updated.
    2. run `update-contributions.js` script in `processing-website` repo. this copies files from 
    the `sources` folder in the `processing-contributions` repo, into the `content/contributions` 
    folder in the `processing-website` repo, while removing the `id` key-value pair. The `sources` 
    folder contains json files, one for each contribution. 


# APPENDIX B: Design of contributions database file
* The file is named `contributions.yaml`
* All fields from the properties file will be included directly, except for the categories, which will
be parsed into a list. 
The fields from the `library.properties` file are: `name`, `version`, `prettyVersion`, 
`minRevision`, `maxRevision`, `authors`, `url`, `type`, `categories`, `sentence`, `paragraph`. 
* Other relevant fields that are represented in the output files are
   * `id` - a unique integer identifier. This is also used in Processing.
   * `download`- the url for the zip file, that contains the library
   * `source` - the url that links to the properties file, from the published library.
* Newly introduced fields are
   * `status` - Possible values are 
      * `DEPRECATED` - Libraries that seem to be permanently down, or have been deprecated. 
      These are libraries that are commented out of `source.conf`. This is manually set.
      * `BROKEN` - libraries who's properties file cannot be retrieved, but we will still check. 
      These are libraries listed in `skipped.conf`
      * `VALID` - libraries that are valid and available
   * `override` - This is an object, where any component field values will replace the existing field values. 
   For example, libraries in the `broken.conf` file are outdated, and we want to cap the
   `maxRevision` to `228`. This cap can be applied by setting `override` to {`maxRevision`: `228`}
   * `log` - Any notes of explanation, such as why a library was labeled `BROKEN`
* Other fields to be included are
   * `previousVersions` - a list of previous `prettyVersion` values. This is a future facing field.
   * `dateAdded` - Date library was added to contributions. This will be added whenever a new library is
   added. To have complete data for this field will require some detective work into the archives.
   * `lastUpdated` - Date library was last updated in the repo. This will be added whenever a library is
   updated. To have complete data for this field will require waiting for all libraries to be updated, or
   will require some detective work into the archives.

