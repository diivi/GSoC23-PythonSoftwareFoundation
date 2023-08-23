# Google Summer of Code 2023 Final Report
## Python Software Foundation - Borg Collective
<br/>

![Header Image](./assets/cover-img.png)

**Contributor**: [Divyansh Singh (divi)](https://github.com/diivi)

**Proposal Link**: [Borg Collective: Divyansh Singh GSoC 2023 Proposal](https://blogs.python-gsoc.org/media/proposals/GSoC_Proposal_DIvi_2023.pdf)

**Organization**: [Python Software Foundation](https://python-gsoc.org/)

**Sub-Organization**: [Borg Collective](https://github.com/borgbackup/borg)

**Project**: [Borg Collective: Add beginner-friendly features and enhance the capabilities of Vorta & Borgmatic.](https://summerofcode.withgoogle.com/programs/2023/projects/0i8Q6ZrE)

**Mentors**: [Dan Helfman (witten)](https://github.com/witten), [Manuel Riel (m3nu)](https://github.com/m3nu), [Julian Hofer](https://github.com/Hofer-Julian), [real-yfprojects](https://github.com/real-yfprojects)

## Project Overview
Borg Collective is a well-known organization that offers a range of Python-based backup tools, including Borg, Borgmatic, and Vorta.

[Borg](https://github.com/borgbackup/borg) is a powerful file backup tool that performs tasks like compression, encryption, authentication, and data deduplication. It is a command-line tool, and is used by many users and organizations to back up their data.

[Vorta](https://github.com/borgbase/vorta) is a desktop GUI for Borg that makes it easier to interact with it without knowing any CLI commands or other intricacies. It is a cross-platform application that is built using Python and Qt, and is used by beginners and advanced users alike.

[borgmatic](https://github.com/borgmatic-collective/borgmatic) is a CLI wrapper around Borg that stores all Borg settings and preferences inside a configuration file and makes it easy to automate backups. It also extends Borg's capabilities by adding support for running pre and post-backup hooks, setting up monitoring, backing up databases, and more.

With this project, I improved Vorta by adding some essential features that users have frequently requested. I enhanced its user experience, making it easier for beginners to use, also designing and developing new interfaces along the way. 

As for Borgmatic, I expanded its already powerful capabilities, particularly in the areas of database backups and restores. I also helped decrease the number of unresolved issues in the Borgmatic repository and worked closely with project maintainers and users to incorporate more features into the project.

## Project Achievements

1. **A new and improved GUI dialog** in Vorta for managing exclusion rules before creating a backup.

   Before
   
   ![Exclude GUI Before](./assets/exclude-gui-before.png)

   After
   
   [New Exclude GUI](https://github.com/borgbase/vorta/assets/41837037/f7bf556a-77a2-4708-a5ba-a5f6bda1f641)

   The old GUI was very basic and **didn't allow users to do much**, it was a simple multi-line plaintext input box. It was also not very intuitive, and **users had a hard time figuring out how to use it**. There's a [GitHub issue](https://github.com/borgbase/vorta/issues/907) that has been open for a long time, requesting a better GUI for managing exclusion rules, where many Vorta users have shared their thoughts and ideas on how to improve it.
   
   The new GUI that I made is based on the famous KDE slogan, **"Simple by default, powerful when needed".** It allows users to **manage exclusions patterns** for their backups with ease and splits the exclusions into 3 tabs:
   
   1. **Custom** - This tab allows users to add custom exclusion rules, which are then passed to Borg as-is. This is useful for users who want to use **Borg's powerful exclusion syntax** to exclude files and directories using the various [patterns](https://borgbackup.readthedocs.io/en/stable/usage/help.html) that Borg supports. Adding and deleting patterns (multiple patterns too) is as simple as clicking a button. This tab also features a **context menu** that allows the users to do the same, and supports detecting the **Del key to delete patterns.**

   2. **Presets** - This tab allows users to select from a list of preset exclusion rules, which are stored as json files in the Vorta repository. These rules are **added by the community** and are **useful for beginners** who don't want to learn the Borg exclusion syntax and just want to exclude some common files and directories from their backups.

   3. **Raw** - Not only can users add singular patterns and presets, but they can also paste a **list of patterns** in this tab, and Vorta will **automatically split them into individual patterns** and add them to the list. This is useful for users who have a large number of patterns and don't want to add them one by one.
   This tab also **supports comments** in the patterns, which is useful for users who want to add some context to their patterns, or want to add a description of what the pattern does.

   Finally, all the exclusions that will be passed to Borg when the backup is created are **displayed in the Preview tab**, which allows users to see what will be excluded from their backup before they create it. The preview tab also features a **"Copy to Clipboard"** button, which allows users to copy the exclusions to their clipboard and paste them wherever they want, which makes **importing and exporting exclusions** very easy (you just need to copy the preview and paste it in the Raw tab of another Vorta instance).

2. **Enhanced the archive table by adding Quality of Life improvements** like **allowing users to rename an archive by double-clicking on it and pressing enter**, and adding a **new column to display whether the archive was created by the user or by the scheduler.**
   
   Earlier, users had to right-click on an archive, select the "Rename" option in a context menu, and enter the new name in a dialog box, which was not very intuitive, involved a lot of clicks, and was a **bad user experience.**

   Identifying whether an archive was created by the scheduler or by the user was also not possible before, and users had a hard time figuring this out.
   ![Rename and trigger](./assets/rename-and-trigger.png)

   Refreshing archives in Vorta recalculates the size of the archive, and also updates the archive table to reflect any other changes in the archive.
   I made it possible for users to **refresh multiple archives at once**, instead of one at a time, which was bad UX, especially for users with a large number of archives. Selecting an archive, clicking "Refresh", waiting for it to finish, and then repeating the process for every archive was a very tedious process.

   ![Refresh multiple archives](assets/refresh-multiple.gif)

   I added a **"Quick Mount" button** to the archive table, which allows users to mount an archive with a single click to a temporary location. This is useful for **quickly accessing files from a backup archive** without having to restore the entire archive, or creating a folder and then mounting the archive to it.

   ![Quick Mount](./assets/quick-mount.gif)
  
3. borgmatic - [Add functionality to support restoring a database dump to a different hostname/port/username than the ones used to create the dump.](https://github.com/borgmatic-collective/borgmatic/pull/73)
   
   Earlier, database dumps created by borgmatic for a particular database could only be restored to the same hostname, port, and username that was used to create the dump. This meant that if the user wanted to restore the dump to a different database, or change even a single parameter, they had to manually edit the dump file and then restore it, which is a long process and is not very user-friendly. And obviously, the user would have to revert the changes made to the configuration file after the restore was complete to avoid any issues with future backups, doubling the work.

   This was a pain point and a frequently requested feature and I worked on adding these parameters to the borgmatic **configuration file and CLI arguments**, so that users could easily change them without having to resort to any workarounds.
   
   I was also responsible for actually **checking and executing the restores** for all the databases that borgmatic supports, namely - **PostgreSQL, MySQL, MariaDB, MongoDB and SQLite**. I learned how to **write tests extensively** for this project, and also how to use **Docker** to test the database restores.

   ![GitHub OpenGraph image for the PR](https://opengraph.githubassets.com/1/borgmatic-collective/borgmatic/pull/73)

4. [Bootstrap a borgmatic restore from nothing](https://github.com/borgmatic-collective/borgmatic/pull/71) - This was another frequently requested feature, that would turn out to be a lifesaver for many users. It allows users to **restore their entire borgmatic-created backup, without having a borgmatic configuration file**. This is useful in cases where the user has lost their configuration file, or if they are trying to restore a backup created by someone else, or even when the user has **lost access to everything** except the backup archive.

   The solution was **designed and implemented completely by me from scratch**, thanks to the trust that Dan had in me. I learned a lot about the internals of borgmatic and Borg while working on this feature, and also how to **write tests** for it. The testing was one of the most challenging parts of this feature, as it required me to mock a lot of things, and I had never written tests so extensively before.

   The solution involved storing the borgmatic configuration file in the backup archive itself, and then using that to restore the backup. This was a very interesting solution, and I learned a lot about how to use Borg to add extra borgmatic metadata to the backup archive.

   I also added a flag to the borgmatic config file that allows users to **disable this feature** if they don't want it, as it can be a security risk in some cases.

   ![GitHub OpenGraph image for the PR](https://opengraph.githubassets.com/1/borgmatic-collective/borgmatic/pull/71)

## What's next?

I learned a lot this GSoC, and made some great connections along the way. I plan to **continue contributing to the Borg Collective**, assisting Dan in maintaining borgmatic, and also helping out with Vorta.

I also aim to **bring and assist new contributors** in getting started with the projects, and to help them with any issues they might face along the way. If the Borg Collective participates in GSoC again, I'd love to help out the new students by **reviewing their proposals and pull requests.**

As for borgmatic and Vorta, future work that I'd like to take up or assist with includes:
1. A button in the exclude GUI that opens up a window to show what the created backup will look like. From @real-yfprojects:
   
   > We could have a button Preview archive instead that opens a view similar to the extract view listing all files that would be archived. This list can be determined using borg create --dry-run.

2. Editing the added exclude rules in the exclude GUI inline, instead of first removing them and then adding them again. Check [this](https://github.com/borgbase/vorta/pull/1742#issuecomment-1680522052) comment for more details.

3. borgmatic supports including external configuration files in the main configuration file, but the bootstrap feature won't work if such a configuration file is used. A solution could be to include the external configuration files in the created archive, just like we do with the main one, so that they can be used to restore the backup.
For more details, check [this](https://projects.torsion.org/borgmatic-collective/borgmatic/issues/736) ticket.

## Challenges and Takeaways

True learning happens when you step out of your comfort zone, and this GSoC was a great example of that. I can confidently say that I am coming out of this GSoC as a much better developer and thinker than I was before, and I learned a lot of new things along the way. Some learnings that I'd like to share are:

* **Project Management & Planning** - I learned how to plan and manage a project, and how to break down a large project into smaller, more manageable tasks. I also learned how to prioritize tasks and how to manage my time to complete them on time. Another thing that I learned is that deciding what not to do is just as important as deciding what to do, and you can't make the perfect product in the first go, so it's better to focus on the most important features first and then add the rest later.
* **Design and accessibility** - I learned how to design and develop interfaces that are accessible to everyone, and how to make them look good and be functional at the same time. I also learned how to use the Qt Designer to design interfaces, and how to use the Qt Style Sheets to style them. During the contributor bonding period, I designed the interface I wanted to implement in Vorta with Figma, discussions with @real-yfprojects helped me improve it and learn more about design. I also implemented a lot of shortcuts and context menus in Vorta, which made it easier to use for users who prefer using the keyboard over the mouse.
* **Qt and GUIs** - Before GSoC, I had never worked with Qt, and I learned a lot about it during the summer. I learned how to use the Qt Designer to design interfaces, what slots and signals are, how to use them, and a lot more about Qt widgets and layouts. My mentors @m3nu, @real-yfprojects and @Hofer-Julian were very supportive, and did not give me the answers directly, but instead guided me in the right direction and helped me figure out the solutions myself. This has made me confident in my Qt skills, and I am sure I can use them to build more amazing things in the future.
* **CLIs and databases** - Adding subcommands and the "bootstrap" command to borgmatic taught me a lot about argparse and the complexities of CLIs. I learned how to use argparse to add subcommands and arguments to a CLI, and how to use the subcommands and arguments in the code. borgmatic also supports backing up and restoring databases, and I learned a lot about how databases work, and how to back them up and restore them using borgmatic. I also learned how to use Docker to test database backups and restores, and how to mock databases in tests. I worked with PostgreSQL, MySQL, MariaDB, MongoDB and SQLite, and was frequently surfing their documentation to learn more about them.
* **Writing tests** - I learned how to write tests for my code, and how to use Docker to test my code in different environments. I also learned how to mock fnctions and inbuilt methods in tests, how to write tests for GUIs, how to use the pytest framework, and also using the flexmock package to test my code. Some other takeaways of mine were using the pytest-qt plugin to write tests for Qt GUIs and using code coverage tools like pytest-cov to check the coverage of my tests. I had never written tests so extensively before, and I am sure this will help me in the future.

There's a lot more that I've learned during this GSoC, like using Docker and even 11ty to build static sites. Non-technical skills like communication and time management are also very important, and I learned a lot about them too. I am sure that I will be able to use all these skills in the future, and I am grateful to the Borg Collective for giving me this opportunity to improve not only my code quality but also my soft skills.
## Acknowledgements

I believe communication and collabration are the most important aspects of any project, and I was lucky to have a great mentor like Dan (@witten), who is great at both of these. He was always there to help me out whenever I got stuck, and was super active during GSoC to help me complete my projects on time. He did not miss a single meeting during the summer and even asked me if I wanted to have extra meetings to clear doubts, which shows how dedicated he was to the project too. I learned a lot from him, like writing tests, project management, and insights about careers in software development. He also helped me improve my communication skills, by introducing me to new words and phrases through our conversations. I'd like to thank him for being such a great mentor, in all aspects.

The Vorta maintainers have also been very helpful and supportive throughout the project, and I'd like to thank them for their help and guidance too. I had never worked with Qt before, but they were always there to help me out, and I learned a lot from them. Reviews from @m3nu, @real-yfprojects and @Hofer-Julian were very helpful and detailed. Their keen eye and attention to detail helped me improve my code quality and push my limits. I'd like to thank them for their dedication and support, and making me comfortable with Qt.

I am grateful to the Python Software Foundation and the Borg Collective for giving me this opportunity, and hope they participate in GSoC again to help more students like me. The Borg community, from IRC to GitHub, has been nothing but welcoming and supportive, and I'd like to thank them for their help and guidance too. Of course, I'd also like to thank Google for organizing GSoC and giving me this opportunity to work on such an amazing project, with such amazing people.

Lastly, I'd like to thank my parents for their support and encouragement, and my friends for always being there for me, from helping me with designs to reviewing my proposal, they were always there to help me out.

## Pull Requests
A list of all the pull requests I made before and during GSoC, in chronological order of their creation for each project.

### [borgmatic](https://github.com/borgmatic-collective/borgmatic)
![Borgmatic Contributor Stats](./assets/borgmatic-contributor-stats.png)
- [#50 feat: add dump-restore support for SQLite databases](https://github.com/borgmatic-collective/borgmatic/pull/50)
- [#52 fix: remove extra dark mode styles](https://github.com/borgmatic-collective/borgmatic/pull/52)
- [#53 feat: add optional check for the existence of source directories](https://github.com/borgmatic-collective/borgmatic/pull/53)
- [#54 feat: file:// URLs support](https://github.com/borgmatic-collective/borgmatic/pull/54)
- [#55 fix: rephrase error when running from config](https://github.com/borgmatic-collective/borgmatic/pull/55)
- [#56 fix: no error on database backups without source dirs](https://github.com/borgmatic-collective/borgmatic/pull/56)
- [#57 feat: tag repos](https://github.com/borgmatic-collective/borgmatic/pull/57)
- [#58 docs: copy to clipboard support](https://github.com/borgmatic-collective/borgmatic/pull/58)
- [#59 fix: remove extra links from docs css](https://github.com/borgmatic-collective/borgmatic/pull/59)
- [#60 feat: allow defining custom variables in config file](https://github.com/borgmatic-collective/borgmatic/pull/60)
- [#61 fix: docs cli reference create spelling](https://github.com/borgmatic-collective/borgmatic/pull/61)
- [#62 fix: replace primitive values in config without quotes](https://github.com/borgmatic-collective/borgmatic/pull/62)
- [#63 fix: make check repositories work with dict and str repositories](https://github.com/borgmatic-collective/borgmatic/pull/63)
- [#65 fix: run typos](https://github.com/borgmatic-collective/borgmatic/pull/65)
- [#66 chore: add favicon to documentation](https://github.com/borgmatic-collective/borgmatic/pull/66)
- [#67 feat: restore specific schemas](https://github.com/borgmatic-collective/borgmatic/pull/67)
- [#68 feat: add logfile name to hook context for interpolation](https://github.com/borgmatic-collective/borgmatic/pull/68)
- [#71 feat: store configs used to create an archive in the archive and add borgmatic bootstrap](https://github.com/borgmatic-collective/borgmatic/pull/71)
- [#73 feat: allow restoring to different port/host/username](https://github.com/borgmatic-collective/borgmatic/pull/73)
- [#74 docs: add docs for database restore params and config bootstrap](https://github.com/borgmatic-collective/borgmatic/pull/74)
- [#75 feat: optionally disable config bootstrap](https://github.com/borgmatic-collective/borgmatic/pull/75)

### [Vorta](https://github.com/borgbase/vorta)
![Vorta Contributor Stats](./assets/vorta-contributor-stats.png)
- [#1606 feat: remove paramiko](https://github.com/borgbase/vorta/pull/1606)
- [#1609 feat: add a link to the logs folder in borg warnings](https://github.com/borgbase/vorta/pull/1609)
- [#1612 chore: replace print with logging in application.py](https://github.com/borgbase/vorta/pull/1612)
- [#1613 feat: descriptive messages when no sources are selected to backup](https://github.com/borgbase/vorta/pull/1613)
- [#1621 feat: add settings for files list views](https://github.com/borgbase/vorta/pull/1621)
- [#1637 feat: add profile name to log messages](https://github.com/borgbase/vorta/pull/1637)
- [#1656 feat: block after vorta –create and log status](https://github.com/borgbase/vorta/pull/1656)
- [#1658 feat: settings to allow new networks and show notif when a network is disallowed](https://github.com/borgbase/vorta/pull/1658)
- [#1664 feat: quick mount](https://github.com/borgbase/vorta/pull/1664)
- [#1665 feat: assign names to repos](https://github.com/borgbase/vorta/pull/1665)
- [#1666 ci: add ruff for print checks](https://github.com/borgbase/vorta/pull/1666)
- [#1723 feat: refresh multiple archives](https://github.com/borgbase/vorta/pull/1723)
- [#1727 feat: remove compact button for borg versions < 1.2 ](https://github.com/borgbase/vorta/pull/1727)
- [#1732 feat: add a trigger column to the archive table](https://github.com/borgbase/vorta/pull/1732)
- [#1734 feat: inline edit for archive renaming](https://github.com/borgbase/vorta/pull/1734)
- [#1742 feat: exclude gui](https://github.com/borgbase/vorta/pull/1742)

### [Borg](https://github.com/borgbackup/borg)
- [#7388 feat: add cache dir to --debug](https://github.com/borgbackup/borg/pull/7388)


## Thanks to the Borg Collective for an amazing Summer of Code! 🎉