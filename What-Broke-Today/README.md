# What Broke Today
Need better oversight of your schedule tasks? This report will help significantly.

## Minimum Requirements
***
**Server Type:** Microsoft SQL

**Mercury Minimum Version:** 2021.4.8052 (This may work in 2021.3 but I have not tested it. It will not work prior to 2021.3 due to the need for custom dataset processes. To test this in 2021.3, RMS will need to confirm when the CE custom value pivot views were updated to exclude non-related entity instance record rows or whether this was done in 2021.4)

**Skill Level:** Beginner. This is Mercury Base Functionality and will assist you to troubleshoot Mercury Processes.

## Process Concept
***
This is a telerik that can be imported to your environment and used for Mercury Task Run Visualization.

Additionally included is a Mercury General Send Operation that can be used to send you a daily copy of the report for your viewing pleasure.


## Mercury Installation
***

### Report Installation
1. Download this file: [What Broke Today Report.xml](assets/What%20Broke%20Today%20Report.xml) (19 KB)

1. Import the Report into your Environment
   * Go to Setup -> Reports and Filters -> Filters to begin. Follow the steps of the Import Wizard and be sure to **Save Changes** after completion. Note that you will be prompted to "*Create"* several objects.

### Scheduled Email Setup
1. Start with Installing the Report above.

1. Download this file: [What Broke Today Email.xml](assets/What%20Broke%20Today%20Email.xml) (19 KB)

1. Import the Operation into your Environment
   * Go to Setup -> Process -> Operations to begin. Follow the steps of the Import Wizard and be sure to **Save Changes** after completion. Note that you will be prompted to "*Create"* several objects.

1. Edit the operation and setup your `To` addressees.

1. Schedule the Task in Setup -> Tasks -> Task Schedules.
