# pdfgen

![Build and publish](https://github.com/navikt/pdfgen/workflows/Build%20and%20publish/badge.svg)

Repository for `pdfgen`, an application written in Kotlin used to create PDFs

## Technologies & Tools

* Kotlin
* Gradle
* Ktor
* Spek
* Handlebars
* VeraPDF-validation

## Getting started

Most commonly, pdfgen is used as a base image alongside templates, fonts, additional resources, and potential test data to verify that valid PDFs get produced by the aforementioned templates.

In your own repository, create a Dockerfile with the following contents

```dockerfile
# Dockerfile
FROM ghcr.io/navikt/pdfgen:1.4.6

COPY templates /app/templates # handlebars templates
COPY fonts /app/fonts         # fonts to be embedded
COPY resources /app/resources # additional resources
```

Set up the basic folder structure
```bash
mkdir {templates,fonts,resources,data}
```

Create subfolders in `templates` and `data`
```bash
mkdir {templates,data}/your_teamname # your_teamname can be anything, but it'll be a necessary part of the API later
```

* `templates/your_teamname/` should then be populated with your .hbs-templates. the names of these templates will also decide parts of the API paths
* `data/your_teamname/` should be populated with json files with names corresponding to a target .hbs-template, this can be used to test your PDFs during development of templates.

[navikt/flex-sykepengesoknad-pdfgen](https://github.com/navikt/flex-sykepengesoknad-pdfgen) is a good example of how such a project can be set up.

## Developing pdfgen

### Build and run tests
`./gradlew shadowJar`

Running the application locally enables a GET endpoint at `/api/v1/genpdf/<application>/<template>`
which looks for test data at `data/<application>/<template>.json` and outputs a PDF to your browser.
Additionally, the template folder will be fetched on every request, and reflect any changes made since the last GET,
making this ideal for developing new templates for your application.

The template and data directory structure both follow the `<application>/<template>` structure.

## Contact us
### Code/project related questions can be sent to
* Kent Daleng, `kent.daleng@nav.no`
* Kevin Sillerud, `kevin.sillerud@nav.no`
  
### For NAV employees
We are also available on the slack channel #pdfgen for internal communication.
