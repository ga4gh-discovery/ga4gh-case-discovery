# JSON Schemas

# What is JSON Schema?

[JSON Schema](http://json-schema.org/) is a vocabulary that allows you to annotate and validate JSON documents.

# What is here?

## schema_source

The `schema_source` folder contains JSON Schemas in YAML.

YAML is a format designed to be more human friendly for reading, while still being machine readable.

It can be translated into JSON to be used for validation using tools provided in this repo.

## Tooling

To convert the source YAML files to JSON using the script, you will need to have node.js and npm installed (npm comes with node.js).

You may wish to use [NVM](https://github.com/creationix/nvm) to isolate and contain your node.js installation.

To convert the YAML to JSON, run the following commands from the root of the repo.

`npm install`

`npm run yaml2json`

The JSON versions of the JSON Schemas will then be in the `schemas` folder.

# Using JSON Schema

See the [JSON Schema implementations](http://json-schema.org/implementations.html) page for JSON Schema implementations in you language of choice!

You will need to load in to the validator, all of the schemas within the `schemas` folder and child folders.

An implementation should be able to match up the use of `$id` with `$ref`. The ID of a schema does not need to be network accessible, and in this case they are not.

An implementation may require you to "cache" a schema based on it's URL, in which case you may need to extract the schemas ID.

[ajv](https://github.com/epoberezkin/ajv) is a node.js implementation of JSON Schema which works both on the server and in thr browser if needed.

To add the schemas to the validator instance in ajv, you need to use the [`addSchema` function](https://github.com/epoberezkin/ajv#addschemaarrayobjectobject-schema--string-key---ajv).

If you have any questions regarding JSON Schema, the easiest way to get answers is to join the JSON Schema slack server. An invite link is provided on the [official JSON Schema website](http://json-schema.org) by clicking on "discussion".