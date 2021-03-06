# Upload ISIC data into FHIR Server

## Preparing data in suitable format
* input: ISIC patient metadata (Reads CSV files from the input directory)
* output: Transaction json(s) - Ready to upload into the FHIR server

For each patient, will generate two entries. The first one is related to patient details (id, birth date, and gender). The second entry is related to the corresponding image information. 
We are keeping the "anatom_site_general" field in the "bodySite" attribute and "lesion_id" filed in "reasonCode" attribute, that are defined in the FHIR structure. (https://www.hl7.org/fhir/media.html)

The "BATCH_SIZE" as an input parameter defines the number of entries in each transaction. So it will be more practical to upload the data into the FHIR server in several transactions.
There is also another parameter. It is the address of the MINIO server.

## Usage

```
npm install
node convert.js BATCH_SIZE MINIO_BASE_URL
```

```
node convert.js 500 "http://menzel.informatik.rwth-aachen.de:9000/isic/"
```

## FHIR Server
```
docker volume create blaze-data
docker run -d -p 9001:8080 -v blaze-data:/app/data -e BASE_URL=http://menzel.informatik.rwth-aachen.de:9001 --name blaze-latest samply/blaze:0.9.0-alpha.14
```
### Uploading data
```
blazectl upload --server http://menzel.informatik.rwth-aachen.de:9001/fhir output
```

## Useful Links
https://github.com/samply/blaze

https://github.com/samply/blazectl

https://hub.docker.com/r/samply/blaze

https://alexanderkiel.gitbook.io/blaze/deployment/environment-variables