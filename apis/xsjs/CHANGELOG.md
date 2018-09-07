# Change Log
All notable changes to this project will be documented in this file.

This project adheres to [Semantic Versioning](http://semver.org/).

The format is based on [Keep a Changelog](http://keepachangelog.com/).

## 3.4.0 - 2018-05-28

### Added
- Enable setting maxBodySize

### Fixed
- Use @sap/xsodata feature for lazy db connection creation
- Updated dependencies

## 3.3.7 - 2018-04-23

### Fixed
- Updated @sap/xsodata to 3.6.0

## 3.3.6 - 2018-04-20

### Fixed
- Update @sap/xssec to 2.1.10

## 3.3.5 - 2018-04-19

### Fixed
- Update @sap/fibers to 2.0.2-0
- Update @sap/fibrous to 0.5.0-3

## 3.3.4 - 2018-04-06

### Added
- A note in the documentation regarding the `redirectUrl` option.

### Fixed
- Update dependencies

## 3.3.3 - 2018-03-12

### Fixed
- Improved performance of $.hdb.

## 3.3.2 - 2018-03-02

### Fixed
- Fix callable statement output params.

## 3.3.1 - 2018-02-26

### Fixed
- Incorrect milliseconds in TIMESTAMPS.

## 3.3.0 - 2018-02-21

### Changed
- `@sap/xsodata` version.

### Added
- DSR metrics.

### Fixed
- Trailing zeros of decimals will not be removed.
- Performance issue for string to date conversions in $.hdb.

## 3.2.1 - 2018-02-13

### Changed
- `@sap/xsodata` version.

### Fixed
- `sqlcc` user overrides default one.
- Returned error code in batch affected rows.

## 3.2.0 - 2018-01-23

### Added
- npm-shrinkwrap.json

## 3.1.0 - 2018-01-16

### Added
- Result set indices in $.hdb after output type conversions.
- Support for gzipped and deflated requests.
- Expose security context to XSJS code.
- Documentation for `redirectUrl` and `libraryCache`.

### Fixed
- Missing $.request.body in some requests.
- Always return array result for batch update.

## 3.0.0 - 2017-12-06

### Changed
- An array of affected rows is returned for failed batch inserts via $.hdb instead of throwing an error.

### Added
- Node 8 support.
- Support for tilde (`~`) headers (except for `~server` headers).

### Fixed
- Update dependencies.

## 2.1.2 - 2017-11-27

### Fixed
- Updated dependencies.
- Apply compatible formatting for decimal strings.
- Warning messages for files that will be ignored.

## 2.1.1 - 2017-10-17

### Changed
- Dependencies' versions.

## 2.1.0 - 2017-10-13

### Added
- The authorization header is now exposed to application code.
- Improvements to documentation.

### Fixed
- Locale handling in OData.

### Changed
- Dependencies' versions.

## 2.0.0 - 2017-08-28

### Changed
- Missing UAA configuration when anonymous access is not enabled results in an error during application startup.
- Not valid files (e.g. .xsjs files with syntax errors) cause an error during application startup.
- $.hdb.ResultSetMetadata has been incompatibly changed (compared to @sap/xsjs v1) in order to become more compatible with XS Classic.

### Removed
- The deprecated xsjs.extend function.
- The support for the 'jwt' option (use the 'uaa' option instead).
- The support for Node.js v0.12.x.

### Fixed
- `$.session.language` now fallbacks to the value of `$.request.language` if `xsSessionLanguage` cookie is not present.

## 1.16.4 - 2017-07-28

### Added
- Provide a request-specific logger to @sap/xsodata.

### Fixed
- Upload of binary files.

## 1.16.3 - 2017-07-17

### Fixed
- $.session missing in OData exits.
- Errors from `.xsjs` files not handled properly.

## 1.16.2 - 2017-07-04

### Fixed
- `.xsaccess` rewrite rules scope.
- Default conversions on input parameters.
- Retrieval of job logs
- `$.hdb.rollback` is executed synchronously.

## 1.16.1 - 2017-06-02

### Added
- Support for HTTP method PATCH in $.request.method.

### Fixed
- Fix database connection pooling.
- Trace uncaught exceptions stack trace.
- Log package version and Node version.
- Improvements in README.md.

## 1.16.0 - 2017-05-09

### Added
- Support for Date objects as input parameters in $.hdb.
- Support for the treatDateAsUTC flag for $.hdb connections.
- Performance improvements via adopting @sap/logging version 3.

### Fixed
- Typo in the name of the category used for logging.
- Getting null values from $.db.ResultSet.
- Automatic closing of database connections.

### Changed
- The entry point of the application being accessed is used as a location of $.trace API entries.

## 1.15.1 - 2017-04-13

### Fixed
 - Allow query parameters for OData $batch requests

## 1.15.0 - 2017-04-06

### Added
 - API for clearing OData model cache.

## 1.14.2 - 2017-04-04

### Fixed
 - http compression was not enabled when configured.

### Added
 - README.md table of content.

## 1.14.1 - 2017-03-20

### Fixed
 -	ResultSet::getTimestamp regression when getting `null` values.
 -	`xsjob` can refer `.xsjs` files placed in the application root directory.

## 1.14.0 - 2017-03-13

### Added
- Multitenancy support via integration with Instance Manager

## 1.13.1 - 2017-02-21

### Fixed
- Fix jobs execution with authentication.
- Improve ResultSet getters parameter validation and functionallity.
- Fix $.session.hasAppPrivilege when using anonymous access.

## 1.13.0 - 2017-01-30

### Added
- Adding, altering and deleting entries from Zip objects.
- Log error for jobs without HANA config.

### Changed
- Rename package to use @sap scope

### Fixed
- npm restriction.

## 1.12.0 - 2017-01-06

### Added
- Column indexing functionality for $.hdb.ResultSet
- SAP passport support when connecting to db

### Fixed
- Jobs callback url
- Direct execution of queries in $.hdb

## 1.11.4 - 2016-11-25

### Fixed
- Fix in xsodata: use same quoting semantic for input parameters of calcviews as in XS Classic

## 1.11.3 - 2016-11-16

### Fixed
- Adapt Zip objects in xsjs APIs
- Align TupelList behavior
- Fix ReDoS issue in negotiator
- Use getter/setter for library execution result property assignment
- Do not trace an error stack for 4xx status codes
- Document decimal column incompatibility
- Use default previous component name in SAP passport

## 1.11.2 - 2016-10-14

### Fixed
 - Fixes database connectivity

## 1.11.1 - 2016-10-13

### Fixed
 - Fixes in xsodata

## 1.11.0 - 2016-10-11

### Fixed
 - Minor fixes and improvements

## 1.10.1 - 2016-09-28

### Fixed
 - Fixes in xsodata

## 1.10.0 - 2016-09-28

### Added
 - $.util.Zip
 - $.util.SAXParser

### Fixed
 - Align content-type header values with XS Classic
 - Minor bug fixes

## 1.9.0 - 2016-08-29

### Added
 - HANA connection pooling
 - Support for Node.js v6

## 1.8.0 - 2016-08-05

### Added
 - 'context' property in xsjs bootstrap options which can be used if you want to extend the xsjs scripts with additional global variables

### Fixed
 - Fixes in database connectivity

## 1.7.0 - 2016-07-13

### Added
 - $.util.compression
 - $.text.mining support
 - Support for compression