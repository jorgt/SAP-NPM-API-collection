@sap/hdi-dynamic-deploy
=======================

`@sap/hdi-dynamic-deploy` is a [Node.js](https://nodejs.org)-based http server for dynamic deployment to SAP HANA DI (HDI) containers, HDI Dynamic Deployer for short. The HDI Dynamic Deployer can be used in XS Advanced (XSA) and in SAP Cloud Platform (SAP CP)/Cloud Foundry (CF) to deploy database content to dynamically created containers (e.g. created via the Instance Manager).

The dynamic deployer is built upon `@sap/hdi-deploy` which should be used directly if a static deployment at deploytime is sufficient.

## README.md

**Installation**:
- [Integration into a Database Module](#integration-into-a-database-module)
- [Configuration of the dynamic deployer](#configuration-of-the-dynamic-deployer)

**Dynamic deployment**:
- [Triggering a dynamic deployment by a HTTP POST request](#triggering-a-dynamic-deployment-by-a-http-post-request)
- [How to use it in a multi-target application](how-to-use-it-in-a-multi-target-application)

## Integration into a Database Module

Usually, `@sap/hdi-dynamic-deploy` gets installed via a `package.json`-based dependency inside your application's `db` module:

`db/package.json`:

```
{
  "name": "deploy",
  "dependencies": {
    "@sap/hdi-dynamic-deploy": "1.2.2"
  },
  "scripts": {
    "start": "node node_modules/@sap/hdi-dynamic-deploy/"
  }
}
```


    
## Configuration of the dynamic deployer

The dynamic deployer needs to be configured via the following environment variables:

- `PORT`: port the HTTP server listens to
- `hdi_dynamic_deploy_user`: username for HTTP basic authentication
- `hdi_dynamic_deploy_password`: password for HTTP basic authentication

The `PORT` variable is automatically set by XSA. The username and password have to be given e.g. via the `mta.yaml` file:

```
modules:
  - name: db
    type: com.sap.xs.hdi-dynamic
    path: db
    properties:
      hdi_dynamic_deploy_user: ${generated-user}
      hdi_dynamic_deploy_password: ${generated-password}
    provides:
    - name: db_deployment
      properties:
         url: ${default-url}
         user: ${generated-user}
         password: ${generated-password}
```

## Triggering a dynamic deployment by a HTTP POST request

The dynamic deployer is a http server started for a specific db module. When the module is pushed to XSA or CF, the dynamic deployer starts listening for requests and eventually starts the (non-dynamic) deployer to deploy the content of the db module to a given container.

To trigger the deployment one has to send a HTTP POST request with basic authentication and content type `application/json` to the dynamic deployer. The api offers two urls for deployment, `http(s)://<hostname>:<port>/v1/deploy` and `http(s)://<hostname>:<port>/v1/deploy/to/instance`.

### Deployment via `http(s)://<hostname>:<port>/v1/deploy` (VCAP_SERVICES style)

The first way to trigger a deployment is to send a HTTP POST request to the url `http(s)://<hostname>:<port>/v1/deploy`. The body simply consists of a JSON object containing replacements for several of the HDI deployer's environment variables. Supported are replacements for:

- `HDI_DEPLOY_OPTIONS`
- `DEPLOY_ID`
- `TARGET_CONTAINER`
- `SERVICE_REPLACEMENTS`
- `VCAP_SERVICES`

In addition to providing `VCAP_SERVICES` for replacing the corresponding environment variable it is also possible to provide `ADDITIONAL_VCAP_SERVICES`. The deployer is then called with service bindings created from the `VCAP_SERVICES` of the dynamic deployer by adding the service definitions given by `ADDITIONAL_VCAP_SERVICES`.
The `ADDITIONAL_VCAP_SERVICES` object has the same structure as the original `VCAP_SERVICES`, i.e. it contains lists of service bindings.
If the request contains `ADDITIONAL_VCAP_SERVICES`, the server scans through all of its services and either adds the list of bindings to the `VCAP_SERVICES` environment variable or merges the two lists in case bindings for the given service already exist.
Existing bindings with the same name are replaced with the bindings from `ADDITIONAL_VCAP_SERVICES`.

Example:
```
{
    "TARGET_CONTAINER": "hdi_container_service_name",
    "ADDITIONAL_VCAP_SERVICES": {
        "hana" : [ {
            "name" : "hdi_container_service_name",
            "label" : "hana",
            "tags" : [ "hana", "database", "relational" ],
            "plan" : "hdi-shared",
            "credentials" : {
                "schema" : "DB_EXAMPLE",
                "hdi_password" : "hdi_password",
                "password" : "password",
                "driver" : "com.sap.db.jdbc.Driver",
                "port" : "30015",
                "host" : "srv1234567.host.name",
                "db_hosts" : [ {
                 "port" : 30015,
                 "host" : "srv7654321.host.name"
                } ],
                "hdi_user" : "hdi_user",
                "user" : "user",
                "url" : "jdbc:sap://srv1234567.host.name:30015/?currentschema=DB_EXAMPLE"
            }
        } ]
    }
}
```

### Deployment via `http(s)://<hostname>:<port>/v1/deploy/to/instance` (Instance Manager style)

Since version 1.2.0 of the dynamic deployer there is a second way to trigger a deployment by sending a HTTP POST request to the url `http(s)://<hostname>:<port>/v1/deploy/to/instance`. The request body is simply a managed service instance as retrieved from the Instance Manager with a HTTP GET.

Example:

```
{
  "tenant_id": "1",
  "id": "da7ff475-fd3f-4a86-a3e7-cd3e41e3653d",
  "binding_id": "3bb96cab-0bec-4088-9991-244b750e53b3",
  "instance_id": "d9cc0aef-16f7-40d2-8e10-1816b9214f2e",
  "managed_service_id": "79d9e11a-95c2-4771-ae2d-8ba703bd8fda",
  "managed_plan_id": "bebaad3e-352b-4928-bc6b-8783d754ac3b",
  "managed_instance_id": "0a4d365a-eec5-4083-85ae-677f77bb6f5d",
  "managed_binding_id": "a5cba4c8-95a3-42c8-982d-e075d0a6b941",
  "status": "CREATION_SUCCEEDED",
  "updated_on": 1494322225942,
  "credentials": {
    "host": "srv1234567.host.name",
    "port": "30015",
    "driver": "com.sap.db.jdbc.Driver",
    "url": "jdbc:sap://srv1234567.host.name:30015/?currentschema=55D392C7232649E8A2F08993645B28B5",
    "schema": "55D392C7232649E8A2F08993645B28B5",
    "hdi_user": "SBSS_78283957013891283645150604040575244555286482237534978212872169092",
    "hdi_password": "password",
    "user": "SBSS_92380540949443696814788249184554628165227387555319796659663474608",
    "password": "password"
  }
}
```

### The response from the dynamic deployer

If there was no problem with the basic authentication and the request reaches the dynamic deployer, it usually responds with status code 200 and a json body containing the result of the deployment. The response body has the following form:

```
{
  messages: [<list of result messages from the di server>],
  exitCode: <exit code of the call to the deployer app>
}
```

**IMPORTANT:** A status code of 200 does not mean that the deployment was successful. It just means that the dynamic deployer was able to call the (non-dynamic) deployer. If the deployer finished with no errors the `exitCode` attribute of the response is `0`, otherwise it is `1`. More detailed information about the deployment can be retrieved from the `messages` attribute of the response.


## How to use it in a multi-target application

A multi-target application (MTA) for multi-tenancy scenarios with the instance manager typically includes multiple db modules:
- N static db modules (`type: com.sap.xs.hdi`), e.g. for configuration or shared data. A static module depends on `@sap/hdi-deploy`; it does not depend on `@sap/hdi-dynamic-deploy`.
- M dynamic db modules (`type: com.sap.xs.hdi-dynamic`) where the business data for a certain type of tenant is contained. A dynamic module depends on `@sap/hdi-dynamic-deploy`, which internally depends on `@sap/hdi-deploy` for deployment to the correct tenant.
 
Example:
```
modules:
  - name: db-static-1
    type: com.sap.xs.hdi
    path: db-static-1
    
  - name: db-static-2
    type: com.sap.xs.hdi
    path: db-static-2
    
  - name: db-dynamic-1
    type: com.sap.xs.hdi-dynamic
    path: db-dynamic-1
    
  - name: db-dynamic-2
    type: com.sap.xs.hdi-dynamic
    path: db-dynamic-2
    
  - name: db-dynamic-3
    type: com.sap.xs.hdi-dynamic
    path: db-dynamic-3
```