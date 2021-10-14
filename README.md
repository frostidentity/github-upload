# SECUAZ
[Spanish version of this README](./README-ES.md)

## Design objectives

- Automatic generation of user accounts defined in .csv files.
- Automatic generation of user accounts defined in a google drive spreadsheets.
- Automatic generation of google classroom courses and teacher accounts allocation.
- Automatic students enrolment in google classroom courses.
- Automatic generation of moodle users.
- Automatic generation of moodle courses and assignation 
- Automatic students enrolment in moodle courses and teacher accounts allocation.
- No human intervention in syncronization process.

### Future improvements wishlist

- Generation of output files directly in the cloud without using local files.
- Automatic detection of changes and syncronization between google directory, google classroom and moodle.
- Containerization.
- Friendly web user interface.

# Documentation

## How to enable google API's

We should check that every google API that we want to use in our project is efectively enabled.

I know that the headers of the screenshots are in Spanish, this is just a pet project so, ;) I'm Sorry! But I believe that you can understand it perfectly.

![alt Página del tutorial sobre las APIS de google](./doc/img/DirectoryAPI.png)

## Where to find code examples of the diferent API's used in this project

* Spreadsheet API: https://developers.google.com/sheets/api/quickstart/python
* Directory API Quickstart: https://developers.google.com/admin-sdk/directory/v1/quickstart/python?refresh=1
* Classroom API Quickstart: https://developers.google.com/classroom/quickstart/python
* Classroom Python client API reference: https://developers.google.com/resources/api-libraries/documentation/classroom/v1/python/latest/index.html
* Docs API Quickstart: https://developers.google.com/docs/api/quickstart/python
* Docs API Overview: https://developers.google.com/docs/api/how-tos/overview
* Docs API root page: https://developers.google.com/docs/api/
* How to generate Docs documents: https://developers.google.com/docs/api/how-tos/move-text
* How to merge Docs documents: https://developers.google.com/docs/api/how-tos/merge


The next step is to download a json file with your credentials, obviously you'll not find my credentials stored at the repo.
You should download yours and place them at a credentials directory in the root directory of the project.

Here you have a screenshot of how to download the credentials files.

![alt Diálogo para la descarga del archivo de credenciales](./doc/img/DownloadClientConfiguration.png)

## About TOKENS generation

You should take into account that in the API code examples, they generate a different TOKEN file for every service. This is really impractical.

As a first approach, I've added a main function at every module of the project that looks for an specific token file but I want to point out that the main script will have a single token file intended to be used with every API involved.

The key here is just to specify the correct SCOPE during authentication in the application.

## External config files

As I wrote before, this is just a pet project, but I would like it to be easily used at other schools so I've integrated [prettyconf](https://prettyconf.readthedocs.io/en/latest/introduction.html)

I've never used prettyconf before but it seems to be quite powerful.
I've choose to use .env file format because I think that is really human readable.

Your .env file must be stored at a directory named config that whould be created at the root of the project. The file should be named secuaz.env

I've added an sample config file called sample-config.env all the data stored in this file is fake-data because there can be pretty sensitive information.

## Syncronization process outline

If this is the first execution of the script it should compute the hash of the whole data and it will store that hash in the cache directory that will be allocated at the root of the project (this will change in the future)

In subsequent executions it will check if the computed hash value is diferent from the one that is stored in the hard disk. If not, it will ignore the rest of the syncronization process because the data hasn't changed.

On the other hand if the computed hash value differs from the one store at the machine hard disk then it will begin the syncronization proccess.

The machine stores an in-memory hash table indexed by hash and which every value will be a row defining all student's related information.

That hash table allows us to determine in a O(n) way if a record hash is seen for the first time or if it is unmodified.

There are defined functions that categorized those first time seen hashes between the ones that represent new values and the ones that corresponds with modified records.

## How to execute the tests

Place yourselves into the src directory and then execute:

python -m unittest discover -s tests

Some testing guidelines:
https://realpython.com/python-testing/#how-to-write-assertions

## Mocking google API services

As a lot of the code calls to google API services, while testing I've been forced to mock those calls. Google client library provides us with a couple of powerful utilities to help us in the
testing process.

Everything is great but I've found a little caveat and it's that you will need to use the Google API definition discovery files in JSON format.

As it's not easy to find, here you have a link where you will be able to download them.

[Google API Discovery service definition file download](https://developers.google.com/discovery/v1/reference/apis/getRest?apix_params=%7B%22api%22%3A%22drive%22%2C%22version%22%3A%22v3%22%7D)


## Interesting git log params to represent branches merge process

[alias]
lg1 = log --graph --abbrev-commit --decorate --format=format:'%C(bold blue)%h%C(reset) - %C(bold green)(%ar)%C(reset) %C(white)%s%C(reset) %C(dim white)- %an%C(reset)%C(bold yellow)%d%C(reset)' --all
lg2 = log --graph --abbrev-commit --decorate --format=format:'%C(bold blue)%h%C(reset) - %C(bold cyan)%aD%C(reset) %C(bold green)(%ar)%C(reset)%C(bold yellow)%d%C(reset)%n''          %C(white)%s%C(reset) %C(dim white)- %an%C(reset)' --all

lg = !"git lg1"