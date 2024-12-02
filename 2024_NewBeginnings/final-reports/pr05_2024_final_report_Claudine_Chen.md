# Final Report for Simplifying the workflow for Processing libraries

| **Project** | [Simplifying the workflow for Processing libraries, tools, and modes](https://github.com/processing/pr05-grant/wiki/2024-Project-List-for-%60pr05%60-=-Processing-Foundation-Software-Development-Grant#%F0%9F%85%BF%EF%B8%8F-simplify-the-workflow-for-processing-libraries-tools-and-modes) | 
| :--- | :--- |
| **Grantee** | [Claudine Chen](https://github.com/mingness) |
| **Mentor** | [Stef Tervelde](https://github.com/Stefterv) |
| **Repos**| https://github.com/processing/processing-library-template <br> https://github.com/processing/processing-contributions <br>documentation repo: https://github.com/mingness/pr05-simplify-workflows|


## Technical Decisions

### Library template
1. The Gradle build file written in Kotlin, for the library template. Kotlin is now the default script language. This decision, however, forced us to put everything in one file, rather than having editable fields in one file, and release tasks in another. This is because Kotlin doesn't allow the build file to be split into multiple files.  
2. Gradle is not easy, which is probably why the template `processing-library-template-gradle` seemed to take a 
design approach of eliminating any interactions by the user with Gradle scripts, opting instead to have the user input all relevant build parameters into a config file. Propagating the information from this config file into Gradle tasks required 
additional code. So while this approach does work, it comes at a cost of readability of the Gradle scripts. We opted to not protect the user from Gradle, but let this be an opportunity to learn how to interact with Gradle. We tried to simplify the interaction as much as possible, and to instruct and explain as much as possible. In our template, the user will add their library name, domain, and dependencies into the Gradle file, guided by comments. 
3. We had a principle, to define values only once. Therefore, the version is defined in Gradle once, and propagated to the library properties file. Previously the version was also reported back by the library, but all the options available to accomplish this without setting the version twice were unappealing, and we chose to not implement this.
4. We chose to promote the use of MkDocs for creating documentation websites via Github project pages. This provides straightforward documentation websites based on markdown. Hosting documentation is a requirement for publishing a library in the Contribution Manager. The old documentation recommended people build and host their own documentation website which adds a lot of complexity. The main goal of this project was to simplify the process of creating and publishing a library, so it made sense to automate the process of creating/hosting this documentation as much as possible.
5. We chose to have a `release.properties` file, where the user will input values for the library properties text file. All fields are mapped directly, and the `version` in the 
Gradle build file is mapped to `prettyVersion` in the properties file.

### Add contributions workflow
1. We decided to make a new database file, called `contributions.yaml`. The format of the database file was selected to be a list of objects, in yaml format. Yaml format allows for ease of understanding any edits in the database. Each edit will appear per line with the field name, making each edit easy to see using Github's  interface, compared to tabular formats, like csv. For tabular formats, Github's interface, which shows if a row has an edit, would highlight more which contribution had and edit, but the edit itself would be harder to see, especially since the column label would be on a different line.
2. In the previous repository, some data from the properties files are overwritten by design, based on the categories the source url was listed under, or if it's a "broken" contribution. To make it intuitive to compare values in the database against the properties file, it was decided that the data in the database directly reflect the data in the properties files. To be able to overwrite this data in the output files, we've implemented an `override` field. The value of the `override` field is an object, where any component field values will replace the existing field values. For example, libraries in the `broken.conf` file are outdated, and we want to cap the `maxRevision` to `228`. This cap can be applied by setting `override` to {`maxRevision`: `228`}
3. We want the database file to contain all contributions ever. This means, we need to store contributions that are deprecated, and we need to store that state. This is set by a new `status` field. If we know a library has been deprecated, then we can manually set the `status` to `DEPRECATED` and updates for library will no longer be attempted. A value of `VALID` indicates the library is live. `BROKEN` indicates that the url did not serve the properties file as expected, but we will continue to check the source url for the properties. This `status` value is automatically set if the properties are not found at the url during an update. 
4. The previous process required the new contributor to email the Processing librarian.
We have replaced this initial contact with an issue form. The issue form makes it 
clear as to what is required to register the library, and can be used to trigger workflows. 
5. The creation of the issue form triggers a Github workflow, which automatically
retrieves the properties file, parses and validates it, and if valid, creates a 
pull request for the Processing librarian to review. The way the workflow was designed,
was to allow for a rerun of the workflow, in case of edits to the repository. In order to do this, the branch is deleted before being recreated again by the workflow.
6. To ensure readability and to use preexisting documentation, use of third-party 
actions was preferred.
7. The workflow that checks the urls daily needs to merge its changes automatically, without human intervention. For this reason, we make direct changes into the default branch `main`. This workflow will check the url of every contribution that doesn't have a `status` of `DEPRECATED`, and will update the contribution if the `version` changes. If there isn't a properties file at the url, the contribution will be marked with a status of `BROKEN` until it is live again.
8. In the new repository, the output files are no longer a part of the repository,
to make source of truth clear - the database file. These output files will instead
reside as workflow artifacts.
9. Two different levels of validation are used, where the validation for new contributions is more strict than for existing contributions. This is because some older properties files have different field names than currently expected. Currently we look for `authors` and `categories`, but in older files they might be `authorList` or `category`.  
10. We implemented validation using Pydantic's built in validation, simply because considering all the cases manually became too much. Pydantic wasn't used from the start, because it was intended to write the scripts in a language in the Java family, like Kotlin. However, they currently are in Python.


## Challenges

- Writing the Gradle build file was a challenge. Gradle is not well documented, and while I had used it before, coding in it is another matter. Also, we chose to write the build file in Kotlin, for which there is even less documentation.
- Gradle does not work well when run from command line. I had an apparent issue with caching when running Gradle from command line. It worked perfectly from the Gradle menus in Intellij and VS Code.
- There were some difficulties in implementing the contributions database file, simply because, data is messy, and is rarely as theoretically clean as expected. This work needed to convert logic enshrined in config files and a script into data. The process of doing this required iterations of trial and error.


## Tasks Completed

### Library template

- [x] A Gradle build file, that
  - uses jitpack to resolve Processing, instead of local jar files. Once Processing is offered on Maven, this will be changed to resolve via official sources.
  - has Gradle tasks for building the library, creating the properties file, creating all release artifacts, and installing the library in a local Processing instance.
- [x] The template contains an example library that uses an external dependency, as an a functioning example to work from. The template can be compiled as is. This template provides an example that can be easily accessed in Processing from the examples menu.
- [x] The template has a workflow that includes the necessary release artifacts in a Github release. The default release artifacts only include the source code.
- [x] It provides a new mkdocs powered documentation website example. This framework provides a simple format suitable for documentation websites, based on markdown files.
- [x] The template includes documentation, including detailed instructions on getting started, using the Gradle build file, and how to release and publish.
- [x] The library template is now integrated into the processing account. 
  - The previously named `processing/processing-library-template` is now named
`processing/processing-library-template-ant`. 
  - The previously named `mingness/processing-library-template` is now named
`processing/processing-library-template`. 

### Add contributions workflow
- [x] Create new database file, called `contributions.yaml`, as designed. The design is described in https://github.com/mingness/pr05-simplify-workflows/blob/main/Adding_contribution_workflow_notes.md#design-of-database-file
- [x] Write script for checking online property txt files for each contribution. 
- [x] Create scripts to create `pde/contribs.txt`, and `content/contributions/*.json` files. 
- [x] Create issue form for new library contributors to fill, which then automatically creates the pull request that changes the database file.
- [x] Test output files with website and contribution manager.
- [x] Create workflow such that, if there is an update, edit the database file, and recreate contribs.txt and json files. The output files are stored as workflow artifacts.
- [x] The processing contributions repository is now integrated into the processing account. 
  - The previously named and private repository `processing/processing-contributions` is now named `processing/processing-contributions-legacy`. 
  - There is now a repository named `processing/processing-contributions`, which contains
only the important files of `mingness/processing-contributions-new`.


## Limitations

- The library template at `processing/processing-library-template` has been tested only for a simple case. We will have to wait for a crowd of people to test the template, and hopefully let us know if something is not working well.
- The scripts in `processing/processing-contributions` do not have tests.
- The `contributions.yaml` database file in `processing/processing-contributions` has three fields which do not have comprehensive data. These fields are `dateAdded`, `dateUpdated`, and `previousVersions`. To fill in this data will require delving into the archives the Github commit history.
- Only a template for libraries was created. A template for tools and modes was not investigated.
- Originally there was an idea to run the examples from the library template using the IDE, but this was not implemented. However, it is possible to run the examples in Processing, so perhaps this is not a limitation.

## Work Remaining & Next Steps

- [ ] Write tutorial, and publish on Processing website
- [ ] Compare the output files from the daily update workflow of `processing/processing-contributions` with the output files from `processing/processing-contributions-legacy`
- [ ] Use the output files from the daily update workflow of `processing/processing-contributions` for Contribution Manager and Processing website
- [ ] Review feedback on the library template at `processing/processing-library-template`, and iterate on improvements
- [ ] Delve into archives of contributions, and add data for fields `dateAdded`, `dateUpdated`, and `previousVersions` in the `contributions.yaml` database file in `processing/processing-contributions`.
