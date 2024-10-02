# August Monthly Report - for Simplifying the Workflow for Libraries, Tools, and Modes
## Claudine Chen

### Work Accomplished

Work continued to finalize the library template.
1. This included debugging Gradle tasks, and refining flows and comments in
the `build.gradle.kts` for usability and comprehension.
2. Also, the example library was extended to include an external dependency,
to ensure that the build task worked.
3. MkDocs support was added. There is a starter website, and a Github workflow 
that deploys the website as a Github Pages project website. This starter website
also documents the library template.

We also started planning and designing the adding contribution workflow. 
In a new repo, the existing contributions source files will be processed into 
a new database file, in yaml format. This data will then be processed into
the artifacts needed by the website and contributions manager.
New contributions will come in as an issue via form, which will configure and
create a new branch and pull request, with the contribution added to the
yaml database file.
This work will begin in September.


### Challenges Encountered
One goal that in the end was cut from the template, was returning the version of the library as a library method, 
and synchronizing that version with gradle. To do this, either the user must maintain the version manually, or it 
could be synchronized via code. This was synchronized previously by inputting this value into the `library.properties` file,
and propagating that value into the code with a search and replace script. Another solution could be to read in
a properties file and return the value; this would require the properties file to be part of the library. Other
ways were explored, to see if the Gradle file could be made aware of the Java method. In the end, it was decided
not to pursue this further, since the need for the version to be reported by the library would only be in cases of
debugging an error, so hopefully very rarely. We are also aware that any file or structure included in the template
might be regarded as required, so we only want to offer what is required in the template.


### September Plans

The plans in September are 
1. to release the library template, on Discourse, and to invite feedback.
2. continue work on the add contributions flow.
    1. create a new repo, and process the source data of processing-contributions into a new yaml database file.
    2. create scripts that export the database into artifacts for the website
    3. create scripts that export the database into artifacts for the contribution manager
    4. create the workflow, such as the issue form template, and the automatic creation of the pull request
