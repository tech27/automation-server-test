# Automation-server-test

This code provide tests that validate and testing entire tools provided for Map Colonies services: raster only [for current version]:

### Deploy:
#### Docker:
- Run build.sh script to generate docker image of this test suite.from project directory: [CMD]\
`` ./build.sh``
- Add DUMP_IMAGE=1 param to export .tar file of created image
- Output folder will be created with "generated_dockers.txt" that save current docker image name:tag

- current version include direct running for raster tests -> will be changed on future

#### Wheels and packages:
- This project can be installed as python's package:
    - Install cloned ropo into environment (recommended with virtual env)\
    ``pip install .``\
    It will install to environment all dependencies [On network]
    - To dump package as wheel [from rep root dir - where the setup.py located:\
    ``python3 setup.py sdist bdist_wheel``
*** 
### Raster
 - Exporter tools - server side test cases:
   - Exporting data as geopackage with best layer
   - Export according restricted region size of geoPackage
   - Deletion of old packages after configurable time period -TBD [not implemented yet]
   - Package created on shared folder
   - Download locally package from shared storage
   
   ##### usage:
   1. To run functional testing on exporter tool:\
   ``pytest server_automation/tests/test_exporter_tools.py``
   2. Logging:\
        1.``Add  variable <DEBUG_LOGS=1> for debog logging level``\
        2.``Add variable <FILE_LOGS=1> to write logs into file beside consule``\
        3.``Add variable <LOGS_OUTPU=/tmp/directory/for/logs>``
   3. Choose storage method - S3 OR File System:\
   ``Add the variable <S3_EXPORT_STORAGE_MODE=True> for S3 mode or False for file system``
   4. if you choose S3 mode you should provide following variable on running:
        1. ``S3_ACCESS_KEY=<aws access key>``
        2. ``S3_SECRET_KEY=<aws secret key>`` 
        3. ``S3_END_POINT=<provided valid aws endpoint>``
        4. ``S3_BUCKET_NAME=<relevant bucket>``
        5. ``S3_DOWNLOAD_DIR=<temp directory> - as default ->/tmp/``
   5. In case running test with File system option\
   ``OUTPUT_EXPORT_PATH=<relevant directory mounted to worker's output dir - relative>``\
 
|  Variable   | Value       | Mandatory   |   Default   |
| :----------- | :-----------: | :-----------: | :-----------: |
| SERVICES_URL | QA \ DEV \ PROD | + | QA | 
| EXPORTER_PORT   | for dev mode [trigger api port]        | + | 8081 | 
| STORAGE_PORT   | for dev mode [export status api port]        | + | 8080 | 
| DOWNLOAD_PORT   | for dev mode [download service api port]        | in case of File system mode | 8082 | 
| MAX_EXPORT_RUNNING_TIME   | integer represent min for run timeout        | - | 5 | 
| OUTPUT_EXPORT_PATH   | geopackage output dir | in case of File system mode | /opt/output | 
| DEBUG_LOGS | 1 for debug logs | - | None | 
| FILE_LOGS | 1 for logs file output | - | None | 
| LOGS_OUTPUT | 1 for logs file output | in case of FILE_LOGS=1 | /tmp/mc_logs |
| BEST_LAYER_URL   | relevant layer url | + | provided on exported chart | 
| SOURCE_LAYER | source layer name | + | provided on exported chart |
| S3_EXPORT_STORAGE_MODE   | true for object storage mode | in case of prod environment(openshift) | False | 
| S3_DOWNLOAD_DIR | valid directory downloading data from S3 | in case of S3 mode | /tmp | 
| S3_BUCKET_NAME | export output bucket name | in case of S3 mode | provided from secret | 
| S3_ACCESS_KEY | AWS access key | in case of S3 mode | provided from secret |
| S3_SECRET_KEY | AWS secret key | in case of S3 mode | provided from secret | 
| S3_END_POINT | Storage procided endpoint | in case of S3 mode | provided from secret | 

