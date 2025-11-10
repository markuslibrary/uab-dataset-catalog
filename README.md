# UAB Dataset Catalog

This repository contains the code for the automated API harvesting of dataset records for the UAB Dataset Catalog. The catalog is located at https://digitalcommons.library.uab.edu/datasets/. 

Please note that this code is under development, and feel free to reach out to Claire at cwarner at rockefeller.edu with any questions, comments, or suggestions. 

## General Workflow

The goal for this project is to search for datasets authored by UAB-affiliated scholars in generalist repositories. Metadata for these datasets is then downloaded as  JSON file. The code then loads in the data as a Pandas dataframe and manipulates it to generate a new dataframe. This dataframe is compatible with the batch upload spreadsheet the corresponds to the collection in the UAB Digital Commons instance where the dataset catalog is hosted. Please see the section below for more information on the Digital Commons collection.

### De-duplication

To ensure that you are not adding entries that are already in the catalog, you can generate a batch revise spreadsheet showing the current status of the collection. Then, use the notebook titled "remove-existing-dois" to remove any DOIs that are already present in the catalog. 

### Curation and Upload to Digital Commons

After the de-duplicated spreadsheet is output as a .xlsx file you can then manually inspect and enhance the records. You will need to manually re-save it as a .xls file (Pandas does not write to this file type) before proceeding to the batch upload.

## Structure 

The repository is structured in a few sections corresponding to the repositories we access to populate the catalog. Below I have listed the current status of each "section".

### Zenodo

The Zenodo API section is the first one we have completed. It has two functions, allowing you to enter specific DOIs that you would like to process (download via API and reformat to Digital Commons batch upload format) or search for datasets published within a set timeframe. It will access Zenodo records, as well as some Dryad records which have been mirrored on Zenodo. We plan to to develop separate code for Dryad since this mirroring is no longer happening. 

### DataCite

Rather than using a repository-specific API, this code uses the DataCite API to search for datasets that have DOIs minted by DataCite. It accesses Dryad, Zenodo, Figshare, and some other smaller repositories. It then reduces the data (removing multiple DOIs that correspond to the same dataset) and outputs a spreadsheet in the Digital Commons batch upload format. 

## Digital Commons

This project hinges on using Digital Commons as the platform for the dataset catalog. In principle, another similar platform could do the same job, but the code and workflow may need to be altered. We use a standard collection in our Digital Commons instance, but add custom metadata fields to make it suitable for a dataset catalog. To replicate this collection for yourself, you may ask your Digital Commons representative to duplicate our collection. You can add to or change the metadata fields in your own collection, and alter the code correspondingly.   

## Future Plans

We have many ideas for the future of this code/project. Stay tuned for updates and please reach out with ideas or suggestions! Our current priorities are below:

 1. Improving the API search strategy and investigating the overlap (or lack thereof) between different searches. For example, how does the Zenodo API search compare to the DataCite API search, which should, in theory, cover all Zenodo datasets?
 2. Developing standalone code for the Dryad API. Many Dryad datasets can be accessed via the Zenodo API, but since this mirroring is no longer happening, we will need a new set of code to directly search Dryad records using their API. We will also look into whether this code would be made obsolete by the DataCite code.
 3. Developing standalone code for ICPSR. Currently, ICPSR metadata records must be downloaded individually in XML format from a manual search, but we can develop code to process these XML files in bulk and generate the Batch Upload spreadsheet.
 
