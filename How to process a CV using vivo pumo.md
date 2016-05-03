
## How to process a CV using the vivo pump?

This documentation explains how to create publications in VIVO from a CV. Note that this process is only effective for very large CV's and for whom no other format of data (such as bibtex) is not available. 

### Step 1 : Make a psv file from the resume

Create a file with a row header (first line) that looks something like below :

    doi|start_page|author|uri|type|journal|number|remove|pub_uri|volume|affiliation|title|pub_date|end_page

  The sample fields given above are explained in detail below :
  
  Field | Description | Example
  --- | --- | ---
  doi         |   Digital Object Identifier of the document. Unique. Mandatory. | 10.1177/0163278713501693
  start_page  |   Start page of the document. Optional.                         | 19
  end_page    |   End page of the document. Optional.                           | 32
  author      |   String value of the name that is displayed in the VIVO.       | Pearson, Thomas
  uri         |   The VIVO url of the author that is linked with publication.   | http://vivo.ufl.edu/individual/n6441370166
  type        |   Type of entry. #TODO Make an exhaustive list of types         | article
  journal     |   VIVO url of the journal linked to this entry. Manually create the journal if it is not already present on VIVO.       | http://vivo.ufl.edu/individual/n175049
  number      |   Issue number of the journal linked to this publication        | 1
  volume      |   Volume number of the journal linked to this publication       | 37
  remove      |   Leave Blank #TODO                                             | #TODO
  pub_uri     |   Randomly generate a value and check if that id is already present in the VIVO database. This randomly generated value is used to create a new VIVO Url and link that url to publication. | http://vivo.ufl.edu/individual/1461349101
  affiliation |   Leave Blank #TODO                                             | #TODO
  title       |   Title of the publication.                                     | Identifying emerging research collaborations and networks: method development
  
  
Populate the above file in the specified format. A single line corresponds to single publication. 
  
### Step 2 : Generate author_add.rdf and pub_add.rdf files.

1. Make a clone of vivo-pump forge repository.
2. Navigate to  <repo>/uf_examples/publications directory
3. Execution of the below command should generate three files named author_add.rdf, author_sub.rdf & author_sub.rdf.inverse:
    
        python ../../sv.py -c config/sv_author_pubs.cfg -d author_pub_def.json -s your_file_name.csv
4. Above command may take a while to execute. After the execution is completed, make sure the files created are of non empty size.
5. Now, to create the pub_add.rdf, pub_sub.rdf & pub_sub.rdf.inverse files, execute the below command:

        python ../../sv.py -c config/sv_pubs.cfg -d pub_def.json -s ~/git/tpearson.csv
6. And that's it. You have all the files required to ingest data in to VIVO.


### Step 3 : Ingest data in to VIVO

1. Open VIVO and log in. (Your account must have admin status to continue further with this process)
2. Go to Site Admin > Advanced Data Tools > Add/Remove RDF data
3. Choose the generated author_add.rdf file and select **add instance data (supports large data files)** option and select **N-Triples** option from the drop down. Hit Submit. If all goes well, RDF Upload Successful message must be displayed. Now, repeat the same for pub_add.rdf file.
4. Choose the generated pub_add.rdf file and select **add instance data (supports large data files)** option and select **N-Triples** option from the drop down. Hit Submit.
5. And that's it. All the publications must be reflected in the VIVO. It may take a while if the list of publications are large. 
