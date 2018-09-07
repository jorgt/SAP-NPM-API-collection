# Change Log
All notable changes to this project will be documented in this file.

This project adheres to [Semantic Versioning](http://semver.org/).

The format is based on [Keep a Changelog](http://keepachangelog.com/).

## 4.7.2 - 2018-04-03

### Fixed
- Do not call `setImmediate` when invoking stored procedures

## 4.7.1 - 2018-03-30

### Fixed
- Update dependencies
- Implicit commit when procedure with input table parameters is executed
- Cleanup of global temporary tables when a connection is returned to a pool
- Local temporary tables are now dropped without CASCADE
- Names of temporary tables are now properly escaped during cleanup of connections returned to a pool
- Prepared statement leak when calling a procedure without input table parameters and without parameters having a default value

## 4.7.0 - 2018-01-19

### Added
- npm-shrinkwrap.json

## 4.6.0 - 2018-01-12

### Added
- Support for `servername` option on connect

### Fixed
- Error when `authInfo` is missing `getGrantType` property
- Minimum idle connections is now 0

## 4.5.0 - 2017-11-23

### Added
- Stored procedures: support for default parameters

### Fixed
- Update dependencies

## 4.4.3 - 2017-10-12

### Added
- Support for Node.js 8

### Fixed
- Prevent using a client object that has been returned to the pool
- Update dependencies

## 4.4.2 - 2017-07-17

### Fixed
 - Client credentials token now doesn't throw error

## 4.4.1 - 2017-07-04

### Fixed
 - Allow pool release to be called only once

## 4.4.0 - 2017-06-30

### Added
 - Support for synonyms for procedures
 - Expose generic-pool object

### Fixed
 - Return only non-busy connections to pool
 - Additional options leaks in getPool
 - Fixes in passing input arguments as Array
 - Fixed passing `null` as single input argument

## 4.3.4 - 2017-05-02

### Fixed
 - Close connection if authentication fails
 - Handle `null` for procedures with input table parameters

## 4.3.3 - 2017-04-04

### Fixed
 - Support for INOUT parameters in stored procedures

## 4.3.2 - 2017-03-10

### Fixed
 - Report error if temp table delete fails
 - Updated hdb module to 0.12.1

## 4.3.1 - 2017-02-23

### Fixed
- The `locale` property in the object returned by `connOptions.getRequestOptions` now defaults to undefined instead of to an empty string when there is no language info in the provided request

## 4.3.0 - 2017-01-26

### Added
- Introduce pool.drain - a function to dispose of idle connections

### Fixed
- Log on level 'debug' in case of 'insufficient privilege' error during clean-up of temporary tables

## 4.2.3 - 2017-01-24

### Changed
- Rename package to use @sap scope

## 4.2.2 - 2017-01-24

### Fixed
- Clean-up temporary tables on connection release
- Fixes in procedures and inplace table parameters

## 4.2.1 - 2016-12-07

### Fixed
- `middleware` and `connOptions.getRequestOptions` now update SAP-Passports automatically with default component data

## 4.2.0 - 2016-11-16

### Added
- Make options optional in `pool.acquire`

### Fixed
- Quote name in set schema statement
- Rollback transaction before isolation level restore
- Support for multiple middlewares
- Allow calling a procedure with inplace table parameter
- Fix crash on connect

## 4.1.3 - 2016-10-14

### Fixed
- Fixes in database connectivity

## 4.1.2 - 2016-09-28

### Fixed
- Handle websocket connection end.
- Set DB connection locale from HTTP request in middleware.

## 4.1.1 - 2016-09-15

### Added
- Rollback of uncommitted changes when a connection is returned to a connection pool.

## 4.1.0 - 2016-09-14

### Added
- `autoCommit` connection option
- Set APPLICATION and APPLICATIONVERSION session variables in the middleware
- `connectionOptions.getGlobalOptions()` and `connectionOptions.getRequestOptions(req)` functions

## 4.0.0 - 2016-09-09

### Added
- `session` property in database connection options
- `certificate` property in database connection options

### Removed
- `sapPassport` property in database connection options, use `session.SAP_PASSPORT` instead.
- `userTokens` property in database connection options, use `session.XS_APPLICATIONUSER` instead.
Now a single token is expected.

## 3.0.0 - 2016-08-05

### Changed
- Removed additional functions attached to the returned HDB connection object (incompatible change).
  In previous versions the returned connection object was enriched with the following functions:
    - setSchema
    - setApplicationUser
    - unsetApplicationUser

  Those functions have been removed and we have provided a new function `updateConnectionOptions` instead,
  to be used as utility for setting the supported connection options.

- Read HANA service properties from environment as fallback if no HANA config provided has been removed.

- HANA config object no longer supports setting `userTokens` as string, it must be an object.

- Connection pooling API was changed incompatibly to fix issues with connection cleanup.
