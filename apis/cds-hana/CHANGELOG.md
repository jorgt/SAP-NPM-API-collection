# Changelog

All notable changes to this project will be documented in this file.

This project adheres to [Semantic Versioning](http://semver.org/).

The format is based on [Keep a Changelog](http://keepachangelog.com/).

## Version 0.8.0 - tbd

### Added

### Changed

### Fixed

### Removed

## Version 0.7.1 - 2018-09-05
   
### Changed

- Improved npm-shrinkwrap

## Version 0.7.0 - 2018-08-28

### Added

- Fallback in case certificate is used instead of ca at connect options

### Changed

- API documentation updated

## Version 0.6.1 - 2018-08-09

### Changed

- Require submodules on demand

## Version 0.6.0 - 2018-08-07

### Added

- Full SQL including eventual parameters to stack trace error message
- Support for abstract placeholders #now and #user
- Support for unary and binary expressions in contains

### Changed

- Increased default option of max. db connection clients to 100

### Fixed

- SQL error hides internal error messages and provides details in log

## Version 0.5.1 - 2018-07-02

### Fixed

 - Escaping of special characters in case of 'contains'

## Version 0.5.0 - 2018-06-25

### Added

 - Hana specific SQL generation for DROP statements
 - Hana specific SQL generation for SELECT statements in case of 'contains'
 - Added SQL Error to hide the internal information from other errors
 - support execution of blocks of statements
 - support plain mode of SQL name mapping

### Fixed

 - CDS injection

## Version 0.4.0 - 2018-05-02

### Changed

- connect options aligned to spec
- support for latest CQN spec changes

## Version 0.3.0 - 2018-04-16

### Added

- support CREATE statements

## Version 0.2.0 - 2018-03-16 
### Added

- usage of npm-shrinkwrap

### Changed

- improved performance for expand in case of one-to-many relations