# Testing of the specification

The specification is currently tested at a minimal technical level.
The test includes:
* rendering the specifcation in a bundle script:
    * ` npx @redocly/cli bundle ./spec.yaml -d -o ../ooapiv5_MBO.yaml`
* buidling the specification in Maven using the supplied pom.xml 
    * Create new folder working folder to test spec
    * copy spec.yaml and other specification files to folder in directory 
`/src/main/resources/NED-OOAPI/specification/v5/spec.yaml `
    * copy `pom.xml` into working
    * run `mvn package`