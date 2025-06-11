# Automated Application Total Historical Records
Tired of pulling data around application history weekly? This will do it for you.

## Minimum Requirements
***
**Server Type:** Microsoft SQL

**Mercury Minimum Version:** 2021.4.8052 (This may work in 2021.3 but I have not tested it. It will not work prior to 2021.3 due to the need for custom dataset processes. To test this in 2021.3, RMS will need to confirm when the CE custom value pivot views were updated to exclude non-related entity instance record rows or whether this was done in 2021.4)

**Skill Level:** Advanced. This could involve some customization between your Mercury instance due to how your school classifies and processes bedspaces for the bedspace data configuration type listed below.

**Mandatory Warning:**  *If run incorrectly, Custom Dataset Processes can affect your Mercury Environment and have implications. If you are not familiar with Custom Dataset Processes and/or SQL coding, it is recommended that you seek assistance as damaging your database due to a Custom Dataset Process incident is not covered by RMS and would require a billable work order to assist. Additionally, Custom Dataset Processes should be thoroughly vetted and evaluated in your test environment after each update to ensure continued compatibility.*

## Process Concept
***
The concept behind this is that you need to report on application totals over time but don't have time to pull your statistics daily/weekly/monthly and would rather they be consistently pulled in the same way at the same time each day/week/month.

This process will automatically archive your totals into a custom entity for you to report on.

## Mercury Installation
***

1. Download this file: [MenuGroup-Application Totals.7z|attachment](assets/MenuGroup-Application%20Totals.7z) (14.1 KB)

1. Import the Menu Group into your Environment
   * Go to Setup -> Process -> Menu Management to begin. Follow the steps of the Import Wizard and be sure to **Save Changes** after completion. Note that you will be prompted to "*Create"* several objects.
   * The following items will be imported:
    * Custom Entities
      * Automatic Nominated Bed List Registration - XCEAutoNomBedList
        * Custom Fields:
          * XXCEAutoNomBedListCustFldAssociateID
          * XXCEAutoNomBedListCustFldNomSetID
          * XXCEAutoNomBedListCustFldRecordType
          * XXCEAutoNomBedListCustFldRangeStart
          * XXCEAutoNomBedListCustFldRangeEnd
        * CE - Easy Templates:
          * Automatic Nominated Bed List - Create
        * CE - Operations:
          * Update Automatic Nominated Bed Lists
          * Delete Automatic Nominated Bed Lists
    * Dynamic Lists
      * Automatic Nominated Bed List Registration
    * Operations - Instagrid
      * Nominated Set ID Lookup
      * User Positions ID Lookup
    * Operations - Custom Dataset Processes
      * CDP - Auto Nom Bed List Sync



## Mercury Setup
***
After installing, if you run the Sync Process, nothing will happen. The sync process will only sync nominated list sets that have a corresponding record in the Automatic Nominated Bed List Custom Entity. At Northeastern, we sync in two different ways:
* The first way we sync is based on user positions; this allows us to build nominated lists based off of the living area hierarchy and provision specific areas to specific sets which are accessible to staff depending on their position association.
* The second way we sync nominated bed spaces is based on bedspace data; currently, we code each bedspace with a specific population code in the Key_ID field; however, this function could be adjust to suit different needs like sorting on room type, eligibility criteria, etc.

### Syncing via User Positions
In order to sync via user positions, you will need to create or use an existing User Position to sync with your Bedspace Sets. To start, you will need to lookup the User Position ID that contains the appropriated Living Areas.
1. Select the `User Position ID Lookup` Instagrid.
1. Find the Position you want to sync and note the Associated Position ID.
1. Select the `Nominated Set ID Lookup` Instagrid.
1. Find the Nominated Bed Space List Set you want to sync with and note the Nominated Set ID. Please note, while you are syncing to Nominated Bed Lists, you are infact actually syncing to the sets within the list. These are the sets that display in different groups when you open a nominated bed list, you may or may not have person based filters on each set set.
1. Select the `Auto Nominated Bed List Config` Dynamic List.
1. Click `Show me List`.
1. Click `Add New` and `Add Item`.
1. Enter the Nominated Set ID from above into the `Nominated Set ID` field.
1. Enter the User Position ID from above into the `Auto Nominated Bed Space Associated ID` field.
1. Select 'Position' in the `Nom List Auto Record Type` field.
1. Enter your start and end dates of the bedspace config dates you want Mercury to add to your Nominated Bed Space list. This process will add any configs that overlap the date range.
1. Click `Finish`.

Now you should be able to sync your Nominated Sets and see a difference based on what living areas were nominated to your User Position Group.

### Syncing via Bedspace Data
Currently, at Northeastern we are syncing bedspaces by Matching Key_ID values to the appropriate Nominated Sets (For instance at Northeastern, Key_ID='132302' means that the space is a Boston Based, 3rd year, Honors Students Only, All Gender, 21+ Only Space.). By coding each space with these values, this allows us to build numerous nominated bed spaces sets with different person based filters for our housing selection process. This process could be tweaked to look at any bedspace data field(s) as necessary. As an example, information on how we are syncing Key_ID is below.
1. Select the `Nominated Set ID Lookup` Instagrid.
1. Find the Nominated Bed Space List Set you want to sync with and note the Nominated Set ID. Please note, while you are syncing to Nominated Bed Lists, you are in fact actually syncing to the sets within the list. These are the sets that display in different groups when you open a nominated bed list, you may or may not have person based filters on each set.
1. Select the `Auto Nominated Bed List Config` Dynamic List.
1. Click `Show me List`.
1. Click `Add New` and `Add Item`.
1. Enter the Nominated Set ID from above into the `Nominated Set ID` field.
1. Enter the corresponding Key_ID value you are looking for into the `Auto Nominated Bed Space Associated ID` field. (From our example above, this would be `132302`)
1. Select 'Selection Type' in the `Nom List Auto Record Type` field.
1. Enter your start and end dates of the bedspace config dates you want Mercury to add to your Nominated Bed Space list. This process will add any configs that overlap the date range.
1. Click `Finish`.

Now you should be able to sync your Nominated Sets and see a difference based on what Key_ID values were nominated.

### Syncing the Nominated Sets
To sync the nominated sets, click the Custom Dataset Process `Auto Sync Nom Lists`.