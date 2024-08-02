# July Monthly Report - for Simplifying the Workflow for Libraries, Tools, and Modes
## Claudine Chen

### Work Accomplished
The main goal this month was to make initial strides in developing a library template using Gradle. Initial work was to explore what work had already been done by the community, and to understand what each repo did, and how they did it. From this understanding, a template that uses the best-of ideas is being assembled at https://github.com/mingness/processing-library-template. 
This template has been the collaborative work of myself and Katsuya Endoh, with helpful guidance from Stef Tervelde and Raphaël de Courville.

Once work on the template is completed at this stage, we will have completed the following expected outcomes:
- Repository templates for libraries using the new Gradle templates.
- Automation for building and exporting artifacts, including PDEX/PDEZ bundles. 
- CI/CD deployment of the reference to GitHub pages including install links using the new pdez:// and pdex:// protocols.

The main challenge met in this work was working with Gradle. The documentation has not been clarifying with respect to what plugins specifically do, and I would have to do extra searches to find out how to implement something using Kotlin, the new default and our chosen language, instead of Groovy, the previous long-time standard. Also, I had unexpected difficulties building from the commandline - the commandline build would break for situations that would build successfully from the Gradle menu in Intellij.

Also, in an effort to create a process and resources that are intuitive and address the communities needs, a handful of people have been interviewed thus far about their own experiences developing libraries, which suggests some takeaways:
- Most people expected documentation for building libraries to be on the website. This is a good idea, because not everyone knows to explore Github for information.
- People eventually found resources using internet search. The information found and path followed varied by individual. This suggests existing information and resources are fractured and don't lead to predictable outcomes.
- Needing to go to Processing to test the library grew tiresome after a while. Some would like a way to test library installation within intellij
- The processing hosted gradle template is designed for the developer to interact with a build.properties file. While users found the template worked and was well documented, some didn't realize that dependencies were also to be listed in the build.properties file, and they interacted with the gradle file to add dependencies. This reminds us that users will not always interact with a design as expected.
- Some people struggled with javadocs, and instead opted for mkdocs, which creates pages using markdown.
- Intellij is a featureful IDE that is good for beginners.

### August Plans
The plans for next month are twofold: 
1. I plan to finish the initial draft of the library template, and then release it for testing. I would like to invite developers to test the new template at https://github.com/mingness/processing-library-template, but also the previous template at https://github.com/processing/processing-library-template-gradle. I will solicit feedback on their experiences.
2. Design and start the implementation of a more automated, simplified workflow for adding contributions to the processing website and contributions manager.


