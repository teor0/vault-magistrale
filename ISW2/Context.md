With Java and the Jira API,  I extracted the release, along with their release dates, from the Apache Zookeeper project and saved them in a JSON file. Now, for each release, I have found the latest commit, defined as the last commit preceding the release date using the JGit library for Java. At this point, for each Java class, I want to write on CSV file the name of the class and different metrics. Some metrics that I want to compute are based on the classes and some on the commits. These are the metrics that need to be computed on each class:
-  Size (LOC): lines of code.
- LOC Touched: sum over revisions of LOC added and deleted.
- NR: number of revisions.
- Nauth: number of authors.
- LOC Added: sum over revisions of LOC added.
- Max LOC Added: maximum over revisions of LOC added.
- Average LOC Added: average LOC added per revision.
- Churn: sum over revisions of added and deleted LOC.
- Max Churn: maximum churn over revisions.
- Average Churn: average churn over revisions.
- Change Set Size: number of files committed together.
- Max Change Set: maximum change set size over revisions.
- Average Change Set: average change set size over revisions.
- Age: age of release.
- Weighted Age: age of release weighted by LOC touched.

And these are the metrics that need to be computed on each commits :
- Number of modified files (NF): changes touching many files are more likely to be defect prone.
- Lines of code added (LA): the more lines of code added the more likely a defect is introduced.
- Lines of code deleted (LD): the more lines of code deleted the higher the chance of a defect to occur.
- Lines of code in a file before the change (LT): the larger a file the more likely a change might introduce a defect.

Give me the implementation of the metrics calculator class and modify the classes I gave to you if needed.