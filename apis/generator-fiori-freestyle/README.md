# @sap/generator-fiori-freestyle

## Features

The **SAP Fiori freestyle SAPUI5 application** provides a Yeoman template to generate an SAP Fiori freestyle SAPUI5 generator application based on a number of different templates.  The user can choose the type of template required, along with the relevant oData service endpoint, and an application will be generated into the specified folder. 

The generated application conforms to the [Fiori Design guidelines](https://experience.sap.com/fiori-design-web/floorplans/floorplan-overview/) and SAP best practices.

## Installation

1. Get [Node.js](https://nodejs.org/en/download/)
2. Install the generator
    ```sh
    npm install -g yo @sap/generator-fiori-freestyle
    ```
3. Verify your installation to see if Yeoman has been installed correctly
    ```sh
    yo
    ```
  Make sure you see the `@sap/generator-fiori-freestyle` generator listed.

## Usage

### Using Yeoman

- `yo`

OR

### Using Yeoman UI Wizard

- Download the latest release of the Yeoman UI Wizard from https://github.com/SAP/yeoman-ui/releases
- Install the associated VSIX file in VSCode
- Invoke the Yeoman UI Wizard in VSCode by calling `CMD + Shift + P -> Yeoman UI Generators`

### Generator Wizard Steps

#### 1. Template selection

Select the required template type to use when generating your application. The generator currently supports the following templates:

**SAP Fiori freestyle**
- SAP Fiori Worklist Application 
- SAP Fiori Master-Detail Application 
- SAP Fiori Worklist Application OData V4

#### 2. Select Data Source

Currently the generator supports the following methods to provide the Data Source:

- **Connect to an SAP System**

You can connect to an SAP System in VSCode by selecting one of the following methods:

You can choose to connect to an existing ABAP on premise system by providing the URL and optional SAP Client identifier. If the URL requires authentication, you will need to provide those details during generation.
You can connect to an ABAP environment on the SAP Cloud Platform. In this case, you must provide a local file that defines the service connection details for the desired ABAP Enviroment. Once you provide these details, a browser tab will launch for you to provide authentication details.
In both cases, if you choose to save the SAP system for future reference, the system details will be stored in the secure storage location of your operating system.

- **Connect to an OData service**

Enter the OData endpoint URL you wish to use in your generated application. Currently the generator supports an OData endpoint that is either unauthenticated or authenticated with Basic authentication. For an authenticated OData endpoint, you will be asked to provide a username and password.

- **Upload a data service metadata file**

Upload a service metadata file that represents the back end service from the file system. This allows the user to generate the application without relying on a back end service being available.

Note: Uploading a data service metadata file will restrict the generated application to only be available using mock data.


#### 3. Select relevant entities

Once the data source has been supplied, the **SAP Fiori freestyle SAPUI5 application generator** will present a list of entities from the OData service to choose.


#### 4. Add project descriptor data

In the final step, provide the following information:

- **Module name** Required.  Must be alpha-numberic and cannot contain spaces.  The generated NodeJS application will use the module name as its package name.  Will also be used as the folder name of the generated application.
- **App Title** Required.  This will be the title in the header of the generated application
- **Namespace** Required.  The UI5 project namespace to be used.  Must start with a letter and contain letters, digits and periods only.
- **Description** Required. The text description of the application.
- **Parent Folder** Required.  The parent folder into which the new application will be generated.  The new application will be generated in a new folder with the `Module Name` as detailed above.  If there already exists a folder with the same name, the user must choose a new Module name.

- **Advanced Configuration** Optional.  The user can choose to customise the generated application with the following options:

  - **UI5 CSS Theme**
  - **UI5 Javascript Library Version**

### Running the generated app

After the application has been successfully generated, open a terminal and browse to the root folder of the generated application. For a V2 template application, there are two methods available to run the application:

- **Running the application with connection to the live OData endpoint**

  Start the application using `npm start`.  This will make the application available on `localhost:8080`, and will connect to the live OData endpoint.  If the OData endpoint requires authentication, the user will be asked to authenticate in the browser.

- **Running the application using mock data (V2 only)**

  Start the application using `npm run start-mock`.  This will make the application available on `localhost:8080`, but will use a mock server to reflect the OData endpoint.  This ensures that developers can use the application without having to connect to a live OData service.

- **Running the application using context menu**

  Under the `src` folder, find the `app.json` file.  Right click and select **Open Preview in Browser** (launches a browser with your application).

## Prerequisites

The generated application requires the following software to be installed:

- [NodeJS](https://nodejs.org/en/download/) Node version must be >10.15.3 - 10.x or 12.13 LTS.
- Windows OS requires [windows-build-tools](https://www.npmjs.com/package/windows-build-tools) NPM module installed globally.

## Known Issues

- SAP Fiori freestyle SAPUI5 application generator does not support SSO authentication for the associated OData endpoint URL.