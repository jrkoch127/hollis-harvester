# HOLLIS Harvester
Goal: Harvest books from HOLLIS (Harvard Online Library Catalog) for ingest to ADS. 
Includes documentation, Jupyter notebooks, and files for this project.

## Harvesting from HOLLIS API to Include as Content in ADS

The primary aim of this project is to increase ADS access to gray literature, i.e. books/monographic texts. As an institution affiliated with Harvard, we have endeavored to utlize our connection with the Harvard Library system, as their collection is ever-expanding. As a digital repository, ADS can utilize the HOLLIS (Harvard Online Library System) API to retrieve metadata and share as ADS records, which will in turn increase our coverage of gray literature.

Recently NASA has asked the Astrophysics Data System to develop a plan to expand its service from Astrophysics to cover all five scientific disciplines supported by NASA’s Science Mission Directorate (Heliophysics, Planetary Science, Astrophysics, Earth Science, Biophysics). The initial phase of this expansion, into Planetary Science and Heliophysics, has now been approved and funded ([Accomazzi 2021](https://ui.adsabs.harvard.edu/abs/2021AAS...23813203A/abstract)). Over the past year we developed a census to ensure research areas such as Space Science, Astrobiology, Aeronomy and Solar Physics are properly accounted for and represented in our database. The ultimate goal of this effort is to provide the same level of support for these disciplines as ADS currently provides for Astrophysics: current and accurate coverage of both refereed and gray literature, preprints, data and software. We expect that enhanced search capabilities will be developed in due time through collaborations with partners and stakeholders.

The project described in this document is aims to assess the bibliographic holidings of Harvard's Online Library Catalog (HOLLIS), retrieve the item metadata, transform the data, and curate them as individual bibliographic records in a format that can be ingested into ADS's collection. This is one example of ADS’s goal to increase access to scientific data and literature. The results of this project will be a step toward fulfilling ADS coverage of gray literature relevant in Planetary Sciences.

As a Librarian for Digital Technologies Development to the ADS Team supporting curation efforts and assisting in collection management, this appealed to my interests as I have been actively seeking new projects to hone my Python skills and learn new methods to curate content for the ADS. Jupyter Notebook is especially useful for beginner Python users because it helps break up scripts into more manageable blocks (cells) and notes, findings and documentation can be included along the way.
 
In this document I will outline the goals I established, the steps I took to accomplish them, and lessons learned. To accomplish this project, I used a combination of my own knowledge and expertise, read API documentation, searched the web for solutions as needed, and collaborated with ADS team members to debug and refine my code.

## Project Outline and Goals

The source data used in this project was retrieved from ([Harvard's LibraryCloud API](https://wiki.harvard.edu/confluence/display/LibraryStaffDoc/LibraryCloud)). Metadata retrieved includes: author name(s), publisher name and location, title, abstract, publication date (year), DOI if available, ISBN if available, OCLC number if available, and HOLLIS catalog identifier as assigned by Harvard).

The main overall goal of this HOLLIS Harvester was to write a process that could retrieve data for individual books, eliminate existing items in ADS from the set, and then curate records for new items to ingest into ADS. My second priority was to make this process repeatable for other subject areas in the future, as necessary for expanding coverage in the ADS. I split up my overal goal into three major tasks:

1. [Task 1: Retrieve Data from LibraryCloud API](#libcloud-api)
2. [Task 2: Match LibCloud Data to Existing ADS Records](#bibcode-match)
3. [Task 3: Curate Data into ADS Tagged Records](#ads-records)

<details>
 <summary>Task 1 Details</summary>
 
## <a name="libcloud-api">Task 1: Retrieve Data from LibraryCloud API</a>
  
my text here
</details>

<details>
 <summary>Task 2 Details</summary>
 
## <a name="bibcode-match">Task 2: Match LibCloud Data to Existing ADS Records</a>
  
my text here
</details>

<details>
 <summary>Task 3 Details</summary>
 
## <a name="ads-records">Task 3: Curate Data into ADS Tagged Records</a>
  
my text here
</details>
