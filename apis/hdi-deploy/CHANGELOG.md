## 3.8.2

Features:
- introduced .hdbrevokes as a counterpart to .hdbgrants
- automatically find target service: if only one HDI service is bound, TARGET_CONTAINER does not have to be set 
- print timestamp of HDI messages
- print the status of the last build
- fallback to the .hdiconfig in src/ if it is missing in cfg/
- switch to @sap/hdi 2.1.2
- switch from hdb to @sap/hana-client
- check if the container supports locking and skip it if not

Fixes:
- support \n and \r\n as line endings in .hdiignore files
- correctly process grantor files in ZDM mode
- fix 'Maximum call stack size exceeded' in ZDM mode
- don't escape schema names in .hdbgrants

## 3.7.0

Features:
- warning messages will now be prepended with "WARNING:"
- error messages in case of a bad service binding now offer more detail on what caused the error
- now supports comments in JSON files like default services file
- allow file patterns in --deploy option
- added new options --exclude-filter and .hdiignore file that basically work like .gitignore
- improved error reporting when using the deployer as a library

Fixes:
- correctly handle component names of system privileges in .hdbgrants files
- handle empty privilege objects
- filter client files like .hdbgrants before deploying

## 3.6.0

Features:
- support ZDM deployment of HDI artifacts modeled in data/ and access/ folders inside db module
- support ZDM deployment of .hdbrole files in access schema
- target service is logged during deployment

## 3.5.1

Features:
- support ZDM deployment
- added warning message when using .hdbsysnonymtemplate or .hdbsynonymgrantor

## 3.4.1

Features:
- update dependencies

## 3.4.0

Features:
- provide `--treat-unmodified-as-modified` option to schedule also unmodified for deploy
- support procedure-type granting services
- improve Readme

Fixes:
- fix handling of grantor services which have a `db_hosts` value, but no `host` value
- fix occasional 'Callback was already called' error in case of deployment failures

## 3.3.0

Features:
- provide library.js to fork the deployer from a node.js app for a given content directory including support for stdio callbacks

Fixes:
- don't look at server folders outside cfg/, src/, and lib/

## 3.2.0

Features:
- support for @sap/hdi-dynamic-deploy

## 3.1.2

Fixes:
- use `db_hosts` from `VCAP_SERVICES` if it exists
- transform paths from `--include-filter`, `--deploy`, `--undeploy`, `--working-set` to the server format where reusable modules are located at `lib/` instead of `node_modules/`

## 3.1.1

Fixes:
- support @-scoped reusable modules

## 3.1.0

Features:
- support `--parameter` option to pass key-value parameters to the deployment

Fixes:
- use non-sync write to stdout (and stderr), and on exit, wait until stdout is drained to avoid loss of log output in case of crashes, etc.
- container locking is available since server version 2.0.1.0, not 2.0.0.0

## 3.0.0

Features:
- check `src/defaults/default_access_role.hdbrole` file against the working set
- check `.hdbgrants` files against the working set
- show the processing of individual `.hdbgrants` files
- on newer server versions, acquire the container lock while working with the container
- support `--lock-container-timeout <ms>` option for setting the number of milliseconds to wait for the container lock, defaults to 120.000 ms
- also support grantor services with only `"user"`, `"password"`, `"schema"`; and take `"host"`, `"port"`, `"certificate"` from the target container
- in `.hdbgrants` files, use the first non-undefined schema value (`"schema"`, `"reference"`, `"schema"` from grantor service); `"reference"` is only used for `"schema_privileges"`
- support schema roles in `.hdbgrants` files
- reject explicit delta detection of files outside the working set
- switch to Node.js 6.9.1 as minimal Node.js version
- provide `--working-set` option to restrict the set of files which can be modified in the container
- support global roles with admin option in `.hdbgrants` files
- support system privileges with admin option in `.hdbgrants` files
- support `--connection-timeout <ms>` option for setting the number of milliseconds to wait for the database connection(s)

Fixes:
- lock the container before showing the `synchronizing` message
- support `.hdbgrants` files without a name (`folder/.hdbgrants`)
- support the string-array style in schema_roles, too
- support typed global object privileges in `.hdbgrants` files
- correctly pass certificate to hdi client

## 2.3.0

Features:
- rename module to @sap/hdi-deploy

## 2.2.0

Fixes:
- fix schema name handling to allow schema names with special characters
- fix handling of more than 1 privilege in a single element in `.hdbgrants` files
- fix trace handling for VCAP_SERVICES and connection data

## 2.1.0

Features:
- detect server version; provide `--[no-]detect-server-version` option
- invert client feature version numbers if they are not available due to the detected server version
- support `server` in `--info`
- generalize handling of configuration file templating for `cfg/**/*config` files
- provide `--structured-log` option to send messages in a JSON-structured format into a log file
- provide `--[no-]verbose` option to suppress detailed messages at the console output
- allow consumption of reusable database modules via `package.json` and `npm install`
- provide `--simulate-make` option to enable server-side feature for simulating the make activities
- provide `--treat-warnings-as-errors` option to enable server-side feature for treating warnings as errors
- provide `--deploy` option to explicitly include a set of files in the deploy set
- provide `--undeploy` option to explicitly include a set of files in the undeploy set

Fixes:
- reject `--simulate-make` and `--treat-warnings-as-errors` if the server doesn't support these features
- consider the root path when looking for reuse modules
- throw an error if we find a nested node_modules folder inside a reuse module

## 2.0.1

Fixes:
- pass the certificate from the service binding down to the database (`certificate` field in the binding, `ca` field in the database connection)
- fix handling of file templating in `cfg/` folder and include-filter handling on Windows
- fix handling of multiple roles in `container_roles` elements in `.hdbgrants` files
- fix handling of `.hdbgrants` files with more than 1 grantors inside

## 2.0.0

Features:
- allow `.hdbgrants` as suffix for privilege grants / grantor mechanism files
- apply consistency check on file `src/defaults/default_access_role.hdbrole`
- show information about used service replacements
- reworked reporting of hdideploy.js errors and HDI errors/warnings
- refactored JSON parser handling
- automatic assignment of the deployed role `default_access_role` if the file `src/defaults/default_access_role.hdbrole` is in the processing file set
- provide `--info` option to show supported features
- provide timing information for certain actions, e.g. time spent for collecting files, time spent for synchronizing files with the server, etc.
- provide `--include-filter` option to define a filter for restricting the set of files which are processed by the deployer app for deploy/undeploy
- allow the indirection of service names via a `SERVICE_REPLACEMENTS` environment variables
- provide a generic templating mechanism for `cfg/**/*config` files, replacing the `hdbsynonymtemplate` files
- support `default-services.json` and `default-env.json` for local development
- show information about the effects of the `undeploy.json` file
- always show application name plus version number; for support cases
- always consider all deploy directories, even if they do not exist locally
- write the log message `Application can be stopped.` only once
- ignore errors when deleting non-existing directories
- show information about the steps that are performed
- provide `--[no-]strip-cr-from-csv` option to [not] strip carriage return characters from CSV files; not enabled by default anymore

Fixes:
- don't report a missing `undeploy.json` file as an error

## 1.1.0

Features:
- replace `--autoUndeploy` with `--auto-undeploy`; keep `--autoUndeploy` for backwards compatibility
- provide `--no-auto-undeploy` option to revert `--auto-undeploy`
- provide `--root` option
- provide `--exit` and `--no-exit` options
- allow to pass options via JSON structured `HDI_DEPLOY_OPTIONS` environment variable