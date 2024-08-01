# July Monthly Report - for Simplifying the Workflow for Libraries, Tools, and Modes
## Claudine Chen

### Work Accomplished
The main goal this month was to make initial strides in developing a library template using Gradle. Initial work was to explore what work had already been done by the community, and to understand what each repo did, and how they did it. From this understanding, a template that uses the best-of ideas is being assembled at https://github.com/mingness/processing-library-template. Once work on the template is completed at this stage, we will have completed the following expected outcomes:
- Repository templates for libraries using the new Gradle templates.
- Automation for building and exporting artifacts, including PDEX/PDEZ bundles. 
- CI/CD deployment of the reference to GitHub pages including install links using the new pdez:// and pdex:// protocols.

The main challenge met in this work was working with Gradle. The documentation has not been clarifying with respect to what plugins specifically do, and I would have to search to find out how to implement something using Kotlin, the new default and our chosen language, instead of Groovy. Also, I had unexpected difficulties building from the commandline - changes to the gradle build file would sometimes break the build when run on commandline. Running from the Gradle menu produced much more reliable behavior.

Also, in an effort to create a process and resources that is intuitive and address the communities needs, four people have been interviewed thus far about their own experiences developing libraries, which suggests some takeaways:
- Some expected documentation to be on the website. It would be an improvement to put information about contributing to libraries on the website, because not everyone knows to explore Github for information.
- People eventually found resources using internet search. The information found and path followed varied by individual. This suggests fracturing of existing information and resources.
- Needing to go to Processing to test the library grated after a while. Some would like a way to test library installation within intellij
- The processing gradle template is designed for the developer to interact with a build.properties file. While users found the template worked, some didn't realize that dependencies were also to be listed in the build.properties file, and they interacted with the gradle file to add dependencies. This reminds us that users will not always interact with a design as expected.
- Some people struggled with javadocs, and instead opted for mkdocs, which creates pages using markdown.
- Intellij is a featureful IDE that is good for beginners.

### August Plans
The plans for next month are twofold: 
1. I plan to finish the initial draft of the library template, and then release it for testing. I would like to invite developers to test the new template at https://github.com/mingness/processing-library-template, and also at https://github.com/processing/processing-library-template-gradle. also to provide feedback on their experiences.
2. Design and start the implementation of a more automated, simplified workflow for adding contributions to the processing website and contributions manager.


