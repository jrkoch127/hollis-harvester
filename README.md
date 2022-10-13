# HOLLIS Harvester

The primary aim of this project is to increase ADS access to gray literature, i.e. books/monographic texts. As an institution affiliated with Harvard, we have endeavored to utilize our connection with the Harvard Library system as their collection grows. As a digital repository, ADS can utilize the HOLLIS (Harvard Online Library System) API to retrieve book metadata and share as ADS records, which will in turn increase our coverage of gray literature. This process will contribute to the ADS Expansion effort, our recently accepted project to expand ADS services, covering all five scientific disciplines supported by NASA’s Science Mission Directorate (Heliophysics, Planetary Science, Astrophysics, Earth Science, Biophysics).
 
In this document I will outline the goals I established, the steps I took to accomplish them, and lessons learned. To accomplish this project, I used a combination of my own knowledge and expertise, read API documentation, searched the web for solutions as needed, and collaborated with ADS team members to debug and refine my code.

## Project Outline and Goals

The source data was retrieved from ([Harvard's LibraryCloud API](https://wiki.harvard.edu/confluence/display/LibraryStaffDoc/LibraryCloud)). Metadata retrieved includes: author name(s), publisher name and location, title, abstract, publication date (year), DOI if available, ISBN if available, OCLC number if available, and HOLLIS catalog identifier as assigned by Harvard.

The main overall goal of this HOLLIS Harvester was to write a process that could retrieve data for individual books, eliminate existing items in ADS from the set, and then curate records for new items to ingest into ADS. We start by retrieving books with an LC classification number QB - books related to astronomy and astrophysics. After the process is finalized and complete, we'll expand the harvest to include other subject areas (such as QC for Physics, QE for Earth Sciences). Thus, my second priority was to make this process repeatable for other subject areas in the future, as necessary for expanding coverage in the ADS. 

I split up my overal goal into three major tasks:

<details>
 <summary>Task 1: Harvest Data from HOLLIS </summary>

## Task 1: Harvest Data from HOLLIS
First we connect to the LibraryCloud API where HOLLIS allows for retrieving the book metadata. The python process for harvesting the data now can be run with the [HOLLIS1_Harvester](https://github.com/jrkoch127/hollis-harvester/blob/main/HOLLIS1_Harvester.ipynb) notebook. The only thing to change is the header/input data: change the date (YYMM), the LC classification (QB, QC, etc.), and make sure there is a directory ready on my local drive where I will store the results.

```
# Input data, then run the notebook
date = "2209"
classification = "QB"
filepath = "hollis_harvest/" + classification + "/"
```

By running the full HOLLIS1_Harvester notebook, I will first obtain a full file of results according to the classification input. We were able to come up with a process that sends the API request multiple times, appending any new items to the file, until no new results are found. Originally, I found that the API would send me different results each time, so this way, it'll gather as many new items as possible in the harvest.

Second, the script will look at my file of HOLLIS ids for books that we've already reviewed and vetted in the initial haul ('hollis_exclusions.xlsx'). The script removes those items reviewed from the data set. 

Next, the process extracts metadata that we want, using a series of regular expressions and zipping together data into records. Once the data is transformed into a workable state, we then create reference strings out of author, title, and year. Sending the reference strings to the ADS Reference Service API, we can identify additional books that already exist in ADS and remove those from the data set.

Finally, the end of the notebook will provide a "Results Summary": number of new records generated from HOLLIS, number of bibcode matches, and number of new items for ingest review. The output files include spreadsheets for each, so we can review any bibcode matches made, as well as the new ingests.

_Important note for later_: Copy all the HOLLIS ids from the 'hollis_results.xlsx' file, and append it to the 'hollis_exclusions.xlsx' file. Next time we run the harvest, these items will be excluded from the results as having been reviewed.
</details>

<details>
 <summary>Task 2: Data Review & Curation </summary>

## Task 2: Data Review & Curation
**Data Review:**
After we have successfully harvested data from HOLLIS, it's time to manually review items for metadata updates and curation of new items.

First we can look at the bibcode matching results ('ref_results.xlsx'), and make notes for metadata updates as needed. One thing to pay attention to is where the comment reads _"Exception: Hypotheses exhausted: X solutions with equal (good) score"_. This is where the ref service could not decide on a match because there are duplicate records. Make a tab in the excel file and record the duplicate bibcodes - the curation staff can later deduplicate the records.

Next we'll review the new items in the 'hollis_ingests.xlsx' file, where we make curation adjustments as needed (whether that is tweaking the title, making formatting changes to the publication field, cleaning up author names, etc.). For the first large harvest, if we're pulling a new LC classification, we should look at each item in the file, and manually check if it exists in ADS (missed by the ref service). From there we can decide if it needs a new record, or update an existing record with better metadata. We also need to check each item to make sure it's within the scope of our collections. 

Any new ingests should remain on the first tab of the curation spreadsheet, however any metadata updates should be moved to a second tab, with a column for "bibcode". Let's delete or move the 'out of scope' items to a separate third tab so they are not included for ingest. (I'm currently keeping the hollis ids in my hollis_exclusions.xlsx file, which I'll continue to do for now).

Notes on curation review:
- Out of scope items: juvenile literature, 'scientific fiction', books that are beyond the scope of the classification, issues with translation, bad links, etc.
- Keep books that are a newer version of a record that already exists in ADS. We'll ingest these as new items. 
--- However, if there is a ebook/reprint version of something that's much older (say 20+ years older than the current), we can check for a DOI and try to make a single unifying record of the item with its various copyright/reprint/ebook dates. Let's update the existing record in ADS, rather than ingest a new version. Examples: 1989saes.book.....E, 1989snsm.book.....G, 1986gala.book.....H, 1972gala.book.....S
- Keep book versions of PhD theses ADS already has, unless the book/ISBN is mentioned in the record (in which case we can merge the record with any new metadata HOLLIS provides).

**Curation:**
Finally, when the review is complete, we can run the [HOLLS2_Curation](https://github.com/jrkoch127/hollis-harvester/blob/main/HOLLIS2_Curation.ipynb). First, input the same initial date as the harvest (YYMM) and the classification. 

The first part of the notebook will transform the records that need metadata updates ('ingest_new.xlsx', sheet=1), and then the second part will transform the new records for ingest ('ingest_new.xlsx', sheet = 0), so make sure the excel sheets are in order. These will be output as json records.
</details>

<details>
 <summary>Task 3: Data Ingest & ADS Libraries </summary>

## Task 3: Data Ingest & ADS Libraries
Finally, when the review and curation is complete, we'll simply update the python scripts for the serializers ('serializer_updates.py' & 'serializer_ingests.py'). After running the serializers, we'll have ADS tagged format records to send to ADS.

Once the records are added to the system, I'll copy the bibcodes from the ref results, the metadata updates, and the new ingests, and put them into my bibocdes list to update the ADS libraries (hollis_library.xlsx). From that point, I can run the [HOLLIS3_Libraries](https://github.com/jrkoch127/hollis-harvester/blob/main/HOLLIS3_Libraries.ipynb) notebook and add the new bibcodes to my existing library. Alternatively, I have the option to make new libraries as I see fit (I may make new libraries for each classification).
</details>

## Lessons Learned

- Regular expressions can be fun but also my worst enemy! Lots of trial and error, but finally found patterns that worked for parsing what I needed out of the HOLLIS data. Two sites were useful for testing: [Pythex](https://pythex.org/) or [Regex101](https://regex101.com/).
- Learned new python methods to make lists of metadata and zip them together into records.
- This project gave me a great opportunity to practice making python 'for loops' which I was previously looking to gain experience with!
- I carried these new python skills (regular expressions, making 'for loops', and zipping data lists) into other projects and harvesters, so making processes repeatable is exponentially beneficial.
