# August Monthly Report - for Simplifying the Workflow for Libraries, Tools, and Modes
## Claudine Chen

### Work Accomplished
The Processing library template was completed to the point that we could invite feedback.
This was done by announcement in the newsletter.

Work was started on the adding contributions workflow. The existing workflow involves 
manual addition of new libraries to the data files of the repository
`processing-contributions`. The new work refactors this repository. There are two main 
aspects to this work - converting the data store from the current configuration files 
to a consolidated yaml file, and setting up an automated workflow that creates the pull
request with the new contribution added to the database file. 

The work accomplished this month on this workflow was 

1. the creation of an initial version of the database yaml file
2. scripts that convert the data in the yaml file into the file formats needed for the 
contribution manager, and the website
3. an issue form, where new contributors input the url to their released properties file
4. a workflow triggered by the issue form, that retrieves the properties, edits the 
database file, and create a pull request.

### Challenges Encountered

It was an interesting challenge, to submit a graphic to go along with the
newsletter announcement. My initial attempts at manual graphic design were rather 
terrible. I'd like to think that the graphic I eventually submitted, coded in Processing,
was a considerable improvement.

In hindsight, I'd say a previous challenge was balancing work with documentation.
This month, I allowed myself the space to write and sort out the brain, but I just
wanted to highlight that it was a challenge previously.


### October Plans

The plans in October are:

1. Finish work on the add contributions workflow.
2. Advertise the new library template more broadly, for instance in the discourse, 
discord for Processing, and writing an article.
3. Document the add contributions workflow
4. To refactor the Processing library wiki pages - these could be wiki pages for
the template, for the processing repo, or on the website