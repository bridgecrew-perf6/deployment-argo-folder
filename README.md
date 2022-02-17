## BoNeS Template for Micronaut Service

This project contains the template for setting up a new Micronaut Service. The repository houses
a stable and an experimental version of the template. The template sources are under `templates`.

The templating engine is [cookiecutter](https://github.com/cookiecutter/cookiecutter).

The template is meant to be consumed by BoNeS Dev Portal (Backstage), although this is not strictly required.

## Updating the template

The `template-source` folder contains a working Java project from which the template is generated.
Think of it as a template for the template. The reason for including this is so that the template developer
can have a compilable version, without losing the ability to specify the templating needs.

You can build and run the test from `template-source/component-id` to check that everything works fine. To be able to run it using one of the data modules you should comment the not needed one from the build.gradle files.

The preprocessing tool for the template generation is [bones-genesis](https://github.com/nexmoinc/bones-genesis).
Refer to its documentation for its usage.

In order to update the `templates` folder run `make build`. This assumes that `bones-genesis` is in the PATH.

By default this command will take the contents in `template-source` and using `.bones-genesis.yaml` config file will perform the required transformations, that include
```yaml
transformers:
  - !<RenameFilePath>
    match: "component-id"
    replacement: "{{cookiecutter.component_id}}"
```

This will replace `component-id` in all folder names with `{{cookiecutter.component_id}}` and output the result to `templates/experimental`

If you want to run this rendering in a different folder you can run `bones-genesis -o <destination folder> <source folder>`. It is not needed that the destination folder exists before running the command.

## Promote experimental to stable

Run `make promote` to copy the contents of the experimental template to the stable one.

This will keep the catalog-info.yaml from `templates/stable`, so be careful making changes in the experimental catalog-info.yaml


## Adding steps to the catalog-info.yaml

This file is a catalog entity descriptor. For example in this project we have one entity of kind `Template`. There are some fileds common to all kinds. You can find them [here](https://backstage.io/docs/features/software-catalog/descriptor-format)

For the specific purpose of this project which is building modular projects from templates, we will focus on `templates/experimental/catalog-info.yaml`. The special detail about this file is that we are going to find the spec `parameters` and `steps`, with this we will build the project. 
- In the `parameters` block you can add the fileds that we will use to model the project, like `java_version`, if we are using `gateway_integration` or the `data_module` to use. We can play with these values to then perform actions in the `steps` block. You can find [here](https://backstage.io/docs/features/software-templates/writing-templates#specparameters---formstep--formstep) more information about `parameters`.
- In the `steps` block we will define some acctions to perform with the goal of building the final project. These are executed when scaffolding that component. You can find [here](https://backstage.io/docs/features/software-templates/writing-templates#specsteps---action) more about `steps`. Basically each step is an action to perform in a directory. You can find all the actions you can use running DebHub platform locally in http://localhost:3000/create/actions. 

http://localhost:3000/create/actions
## Render local changes

Backstage implements the underlying infrastructure for running the actions which render the template.
In order to render the template locally you must have a running Backstage application.

1. Clone the DevHub [repository](https://github.com/nexmoinc/bones-portal) and install all the needed packages.
2. To import the template from the local file path update the `app-config.yaml` in the `bones-portal` project and add:
```yaml
catalog:
  locations:
    # ... other locations
    - type: file
      target: "{{ Enter the file path here }}"
```
3. Generate a Github Access Token for nexmoinc. It requires the ['read:org', 'read:discussion’, user:email', 'read:user', 'repo'] scopes and to have the SSO enabled and authorised. Add the token to the required places in the config files.
4. Run DevHub with `make run-local`
5. Use the template: http://localhost:3000/create/templates/bones-template-micronaut-service-experimental
6. Fill the form and make sure that the owner is an existing user.

You can create a new project using the template, clone the repo that it is created and make it run to check it works fine.

