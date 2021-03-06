# Task: Deployment to ABAP

The deployment to ABAP task allows deploying Fiori applications to SAP systems using the [SAPUI5 Repository OData service](https://sapui5.hana.ondemand.com/#/topic/a883327a82ef4cc792f3c1e7b7a48de8.html).

**Pre-requisites:**
* SAP component SAP_UI 7.53 or higher is installed in your SAP system
* Service needs to be enabled and accessible from your development environment ([how to check this](https://help.sap.com/viewer/68bf513362174d54b58cddec28794093/7.52.5/en-US/bb2bfe50645c741ae10000000a423f68.html)) 
* For operations on a SAPUI5 ABAP repository, you need the S_DEVELOP authorization.

**Limitations:**
* the task does not create ABAP transports, therefore, it requires an existing transport if the target ABAP package requires a transport
* only Basic Authentication (user/password based authentication) as well as OAuth2 with the SAP Cloud Platform 

## Example Configuration
Executing `ui5 build --config ui5-deploy.yaml` in your project with the configuration below in a `ui5-deploy.yaml`, manually added to the project, would deploy all files of your `dist` folder except files ending with `.test.js` and the `internal.md` file. The target system is XYZ with client 200. Username and password for authentication will be read from the environment variables `XYZ_USER` and `XYZ_PASSWORD`.

Based on this example, the application will be created/updated as `/TEST/SAMPLE_APP` in package `/TEST/UPLOAD` and all changes will be recorded in transport request `XYZQ300582`.

Content of `ui5-deploy.yaml`

```
builder:
  customTasks:
  - name: deploy-to-abap
    afterTask: replaceVersion
    configuration:
      target:
        url: https://XYZ.sap-system.corp:44311
        client: 200
        auth: basic
      credentials:
        username: env:XYZ_USER
        password: env:XYZ_PASSWORD
      app:
        name: /TEST/SAMPLE_APP
        package: /TEST/UPLOAD
        transport: XYZQ300582
      exclude:
      - .*\.test.js
      - internal.md
```

### Command to create the ui5-deploy.yaml file
A newly created project does not contain a deployment configuration (`ui5-deploy.yaml`) but you can create it by executing `npx fiori create deploy-config`. You will be prompted for required information and then the file will be created based on your input and the content of the existing `ui5.yaml` file used for the preview. In addition to creating the configuration, the create deployment command will also update your `package.json` so that you can execute `npm run deploy` afterwards to deploy your application.


### Setting environment variables in a .env file
If you prefer to keep the environment variables in a file, an option can be to create ```.env``` file at the root of your project which contains the environment variables that can be referenced in the ui5.yaml file. 

It is recommended that you do not have your actual username and password in the ```ui5.yaml```.

```
XYZ_USER=[MY_USER_NAME]
XYZ_PASSWORD=[MY_PASSWORD]
```


## Command to deploy
After completing the changes in the configuration files, execute ```npm run deploy``` at command line

### Accessing the deployed app
Based on the sample configurations above, after deploying the app, you can access the app using URL: https://XYZ.sap-system.corp:44311/sap/bc/ui5-ui5/test/sample_app/index.html?sap-client=200#app-preview

## Documentation on the Configuration options
In addition to defining parameters in the main yaml file, every parameter can also be defined as environment variable that is referenced in yaml. Using the `dotenv` module, the task also supports project specific environment variables defined in a `.env` file in the root of your project. To reference an environment variable the pattern `env:VAR_NAME` must be used.

### target
The target object contains properties identifying your target SAP system.

#### url
- `<string> pattern <protocol>://<hostname>[:<port>]` (required)
- This parameter must contain a url pointing to your target SAP system


#### client
- `<number> range [0..999]` (optional)
- The client property is used to identify the SAP client that is to be used in the backend system. It translates to the url parameter `sap-client=<client>` If the client parameter is not provide, the default client will be used. 

#### scp
- `<boolean>` (default: `false`)
- By default the deployment task will use basic authentication when connecting to the backend. If the target system is ABAP Environment on SAP Cloud Platform, this parameter needs to be set to `true`.

#### service
- `<string>` (default: `/sap/opu/odata/UI5/ABAP_REPOSITORY_SRV`)
- Path pointing to the SAPUI5 ABAP repository OData service in your target system. This parameter only needs to be used if the service is exposed at a different path in your backend system e.g. via alias.

### credentials (optional)
The credentials object is mainly required for CI/CD based deployments and it needs to contain the required parameters to authenticate at your target system. We strongly encourage to not add the credentials directly but use references to environment variables e.g. `env:MY_VARIABLE` here.

For local usage, we recommand to not use the credentials object at all. As result, the deployment task will utilize the operating systems secure storage maintain credentials.

#### username
- `<string>` (required)
- SAP business user for the target system. The user requires authorizations to create/update the target ABAP development object.

#### password
- `<string>` (required)
- Password required to authenticate the previously configured user. IMPORTANT: while technically possible to add the password to your config, we strongly DISCOURAGE that but recommend instead the use of environment variables.

### app
The app object describes the backend object that is created/updated as result of the deployment.

#### name
- `<string>` (required)
- Unique name of the application. The name is used as part of the application url as well as the name of the ABAP development object used as container for the app.

#### package
- `<string>` (required for new apps)
- Name of an existing ABAP package that is used as parent of the deployed application. The parameter is required for the creation of the application in the backend. Any following deployment updating the application does not require the package parameter, i.e. it will be ignored.

#### transport 
- `<string>` (optional)
- The transport parameter refers to a transport request number that is to be used to record changes to the backend application object. The property is optional because it is only required if the package that is used for deployments requires transport requests.

#### description
- `<string>` (optional)
- Optional description added to the created application object in the backend.

### exclude
- `<string[] array of regex>` (optional)
- By default the deployment task will create an archive (zip file) of all build files and send it to the backend. By using exclude, you can define expressions to match files that shall not be included into the deployment. Note: `string.match()` is used to evaluate the expressions.

### index
- `true|false` (default: `false`)
- If set to `true`, then an additional index.html will be generated and deployed to run the application standalone.

### test
- `true|false` (default: `false`)
- If set to `true`, the task will run through all steps including sending the archive to the SAP backend. The backend will not deploy the app but run the pre-deployment checklist and return the result.

## FAQ

**My backend system contains the SAP_UI component version 7.53 or newer, but the SAPUI5 repository service cannot be reached.**

*A: Please check if the service has been activated. More information at https://help.sap.com/viewer/68bf513362174d54b58cddec28794093/7.52.5/en-US/bb2bfe50645c741ae10000000a423f68.html.*

**The SAPUI5 repository service is active and reachable but whenever I deploy an application I see the following error "Request failed with status code 400".**

This could have multiple reasons, please check the console for more information or open transaction `/IWFND/ERROR_LOG` and check the server logs. A common issue is that during the setup, configuring a virus scan profile has been forgotten. This can be corrected in transaction `/IWFND/VIRUS_SCAN`.


