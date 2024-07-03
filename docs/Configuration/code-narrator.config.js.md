```md
## code-narrator.config.js

The `code-narrator.config.js` file is a configuration file used to set up the `code-narrator` tool for your project. This file contains various settings and options that dictate how the tool will behave and what files it will interact with.

### Configuration Options

- **entry_file**: Specifies the entry point of your application.
- **cli_file**: Specifies the command-line interface file for your application.
- **project_name**: The name of your project.
- **config_files**: An array of configuration files used by the project.
- **readmeRoot**: A boolean indicating if the README should be at the root.
- **folderRootFileName**: The name of the root file in each folder.
- **repository_url**: The URL of the project's repository.
- **project_file**: The main project file, usually `package.json`.
- **source_path**: The path to the source code.
- **documentation_path**: The path where documentation will be generated.
- **test_path**: The path to the test files.
- **include**: An array of files and patterns to include.
- **generatorPlugin**: An array of generator plugins.
- **builderPlugins**: An array of builder plugins.
- **builders**: An array of builder configurations.

### Example Configuration

```javascript
const ConfigurationBuilder = require("./src/documentation/plugins/builders/Configuration/ConfigurationBuilder");
const FilesBuilder = require("./src/documentation/plugins/builders/Files/FilesBuilder");
const FoldersBuilder = require("./src/documentation/plugins/builders/Folders/FoldersBuilder");
const UserDefinedBuilder = require("./src/documentation/plugins/builders/UserDefined/UserDefinedBuilder");

/**
 * @type {ICodeNarratorConfig}
 */
const config = {
    entry_file: "./dist/src/App.js",
    cli_file: "./dist/src/cli.js",
    project_name: "code-narrator",
    config_files: [
        "code-narrator.config.js"
    ],
    readmeRoot: true,
    folderRootFileName: 'README',
    repository_url: "https://github.com/ingig/code-narrator",
    project_file: "package.json",
    source_path: "src",
    documentation_path: "docs",
    test_path: "__tests__",
    include: [
        "code-narrator.config.js",
        "src/**/*.ts"
    ],
    generatorPlugin : [],
    builderPlugins: [
        ConfigurationBuilder,
        FilesBuilder,
        FoldersBuilder,
        UserDefinedBuilder,
    ],
    builders: [
        {
            type: "README",
            sidebarPosition: 1,
            template: "README",
            name: "ReadMe",
            files: [
                {
                    path: "package.json",
                    JSONPath: [
                        "$.name",
                        "$.description",
                        "$.version",
                        "$.homepage",
                        "$.bugs",
                        "$.author",
                        "$.repository", "$.license"
                    ]
                }
            ]

        },
        {
            name: "FAQ",
            template : "faq",
            sidebarPosition: 2,
            type: "faq",
            args : {
                projectFile : 'content(package.json)'
            }
        },
        {
            name: "Prerequisites",
            template: `prerequisites`,
            sidebarPosition: 3,
            args: {
                entryFileContent: 'content(./dist/src/cli.js)',
                configFile: 'content(code-narrator.config.js)'
            },
            files: [
                {
                    path: "package.json",
                    jsonPaths: [
                        "$.dependencies",
                        "$.devDependencies",
                        "$.engine"
                    ]
                }
            ],
            type: 'Custom'
        },

        {
            //https://github.com/ingig/code-narrator/tree/master/docs/howto
            type: "README",
            template: "overview_readme",
            name: "README",
            path: "howto",
            files: [{
                path: "howto/*.md"
            }
            ],
            pages: [

                {
                    type: "howto",
                    template: "howto_create_howto",
                    name: "How to create HowTo",
                    files: [
                        {
                            path: "code-narrator.config.js",
                            extract: "builders"
                        }
                    ]
                },
                {
                    type: "howto",
                    template: "howto_content_to_long",
                    name: "Content to long",
                },
                {
                    type: "howto",
                    name:"HowTo run CLI",
                    template: "howto_run_cli",
                    args : {
                        docUrl : "https://github.com/ingig/code-narrator/blob/master/docs/Configuration/code-narrator.config.js.md"
                    },
                    files : [
                        {
                            path:"src/utils/CliHelper.ts",
                            extract: "what arguments are available"
                        }
                    ]
                },
                {
                    type: "howto",
                    template: "howto_create_custom_builder",
                    name: "Create your own custom builder",
                    files: [
                        {
                            path: 'code-narrator.config.js',
                            extract: 'builders' //You can use natural language to extract, e.g. "first 20 lines", "first paragraph"
                        },
                        {
                            path: 'src/documentation/builders/UserDefinedBuilderHelper.ts',
                            extract: 'how code parses content(...) and extract:'
                        }
                    ]
                },
            ]
        },
        {
            //https://github.com/ingig/code-narrator/tree/master/docs/howto
            type: "README",
            template: "overview_readme",
            name: "README",
            path: "tutorial",
            files: [{
                path: "tutorial/*.md"
            }
            ],
            pages: [
                {
                    type: "tutorial",
                    template: "tutorial_create_builder_plugin",
                    name: "Tutorial - Create builder plugin",
                    files: [
                        {
                            path: "src/documentation/plugins/builders/Files/FilesBuilder.ts"
                        },
                        {
                            path: "code-narrator.config.js",
                            extract: "builderPlugins"
                        }
