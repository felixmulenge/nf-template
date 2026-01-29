## Before deployment

Before deployment itself, it is good to understand which are the directories and the files that represent the core of nf-core's pipeline organization, which is the one we will be using as this template was created with the "nf-core create" command.

Organization:

```bash
nf-template
├── assets                                      # Directory containing some examples for the pipeline and the files for rendering the execution mail
│   ├── adaptivecard.json
│   ├── email_template.html
│   ├── email_template.txt
│   ├── methods_description_template.yml
│   ├── multiqc_config.yml
│   ├── samplesheet.csv
│   ├── schema_input.json
│   ├── sendmail_template.txt                   # File containing custom message to write in the execution report mail
│   └── slackreport.json
├── CHANGELOG.md                                # File to describe changes between versions
├── CITATIONS.md                                # File to put in the citations of tools used in the pipeline
├── conf                                        # Place to store the '.config' files that will be managing everything in the pipeline!
│   ├── awsbatch.config                         # Institutions' AWS configurations
│   ├── base.config                             # Configuration of labels for managing the use of resources
│   ├── igenomes.config                         # Configuration for reference genome resources
│   ├── igenomes_ignored.config                 # Empty genomes dictionary to use when igenomes is ignored
│   ├── modules.config                          # Configuration of all the labels and directives of a module
│   ├── test.config                             # Setting up a min test profile
│   └── test_full.config                        # Setting up a comprehensive test profile
├── DEPLOYMENT_TODOS.md                         # This file describing the main steps for deployment
├── docs                                        # Place for storing pipeline's documentation
│   ├── images
│   │   └── ZS_Discovery_logo.png
│   ├── output.md
│   ├── README.md
│   └── usage.md
├── LICENSE                                     # A generic use license for the pipeline
├── main.nf                                     # The pipeline main script. Will call all the workflows and subworkflows.
├── modules                                     # Place to store the DSL2 modules
│   └── nf-core                                 # nf-core modules go here
│       ├── fastqc
│       │   ├── environment.yml
│       │   ├── main.nf
│       │   ├── meta.yml
│       │   └── tests
│       └── multiqc
│           ├── environment.yml
│           ├── main.nf
│           ├── meta.yml
│           └── tests
├── modules.json                                 # from nf-core -- don't need to worry about it
├── nextflow.config                              # main script config wich will handle and load all the parameters and configuration set in the files present on "conf" dir.
├── nextflow_schema.json                         # This is what nf-core framework uses as the core target for handling pipeline execution. You don't need to edit it by hand.
├── nf-test.config                               # Configurations for testing the pipelines
├── README.md                                    # File to describe the pipeline and what to expect from it
├── subworkflows                                 # Place to store our subworkflows
│   ├── local                                    # Our custom subworkflow go here
│   │   └── utils_nfcore_template_pipeline
│   │       └── main.nf
│   └── nf-core                                  # nf-core subworkflows go here
│       ├── utils_nextflow_pipeline
│       │   ├── main.nf
│       │   ├── meta.yml
│       │   └── tests
│       ├── utils_nfcore_pipeline
│       │   ├── main.nf
│       │   ├── meta.yml
│       │   └── tests
│       └── utils_nfschema_plugin
│           ├── main.nf
│           ├── meta.yml
│           └── tests
├── tests                                        # place to run testings
│   ├── default.nf.test
│   └── nextflow.config
└── workflows
    └── template.nf                             # place to store our workflows

```

## Steps to get pipeline deployed

> Always check if things defined in the template are actually applicable to your pipeline in development. This template should be taken as a guidance, something to start from and have a look at it, but they are not rules that should be followed blindly.

### metadata
- [ ] Copy entire template directory to another place for editing it
- [ ] Change the pipeline name across the available files. In the template it is called "ZS/template". It can be changed all at once in VS Code using "Cmd + Shift + H".
- [ ] Change pipeline manifest in nextflow.config

### workflows and modules

> Remember to make the most from subworkflows. This makes it easier to spot an error across the pipeline and find its location because, nextflow will always tell you in which workflow/subworkflows/module an error happenned. But how to know when to "split" sub-workflows? For that you have to think on "tasks" Your workflow just be as easier as possible to read, basically calling modules or workflows directly without much work/preparation on channels and inputs.
> When adding a (sub)workflow and module, remember to make sure its directives have been added to the "conf/modules.config" file and its parameters (with defaults when possible) added to the "nextflow.config" file

> Remember, (sub)workflows and modules names go uppercase, and channels, variables, etc go lowercase to help on quick differentiation.

- [ ] Add all the desired modules in the "modules/local" folder if they are your own and inside the "modules/nf-core/modules" if they are from nf-core.
- [ ] Using the "workflows/template.nf" as a guide, start building your own workflow. Remember to use sub-workflows to help on organization and modularization whenever applicable.
- [ ] Include the created workflow in "main.nf". To keep things standardized, no modifications are required on "main.nf". 
- [ ] Remember to edit the "nextflow.config" file and include all the parameters the pipeline will be using.
- [ ] After adding desired parameters in the configs, remember to run "nf-core schema build" in the root directory to use the nf-core tool to read everything and help on getting a valid JSON schema of the pipeline's configurations. This JSON is used for NF Tower and for parameters parsing during execution and logging.
- [ ] Add the scripts you will be using inside the "bin/" directory. If they are there nexflow will automatically make it accessible in $PATH for each module. Just remember to make them executable. 
- [ ] Add the line `Copyright (c) ZS` to scripts, modules and workflows. 

### Finalising

- [ ] Remember to go through all the "TODO" statements that are scattered across the template. With VS Code you can use the extension called "TODO Tree".
- [ ] Remember to edit the "assets/email_template.txt" file if you want users to have a nice and custom way of receiving a warning when a pipeline finishes.

## Notes

Most of the things from the template can be removed (e.g. ".nf", ".md"), others may raise an error. Be careful when removing them. As they are light files and will not pose any hindrance to the pipeline neither will raise conflicts, you may leave them there. So you can copy, create your own, and let the templates there to serve as your backup guide.
>**Caution**: We strongly recommend avoiding edits to the pipeline `utils`, as this may disrupt pipeline execution. 

## Helping on getting a better template

If you find something that can be changed in the template, to improve guidance, remove complexity that we will not need, add something usefull etc., please talk about it so we can always work towards having a better template.

## After all these steps

Finally, if you have checked all the boxes in the previous sections, you must be ready to run and test your pipeline.
If anything comes up, give me a call.

Cheers,
Felix und Felipe.
