0.2.5 / 2013-09-06
==================

  * Only trim strings when the value in the object is still a string (could be something else when convert function is given an the string was already converted)

0.2.4 / 2013-08-28
==================

  * Fix error when convert function is defined and format is an array

0.2.3 / 2013-08-15
==================

  * Fix error that conform method was not executed when value was an empty string and option strictRequired was true

0.2.2 / 2013-08-15
==================

  * Trim string values before checking if the property is required

0.2.1 / 2013-08-14
==================

  * Refer to incoming object in custom conform functions
  * Default message for `additionalProperties`
  * Add option 'trim' to trim string values
  * Add option 'strictRequired' which sets validity of empty strings to false

0.2.0 / 2013-08-08
==================

  * Fix error in color regex. ('and yellow' is now 'yellow')
  * Update vows to version 0.7.x
  * Fix spelling error in tests

0.1.9 / 2013-07-08
==================

  * Fix error in IE when using the keyword default (schema.default is now schema['default'])

0.1.8 / 2013-07-03
==================

  * Add support for validating multiple formats (array notation)

0.1.7 / 2013-05-22
==================

  * Fixed error that there was no check if the convert function exists in options

0.1.6 / 2013-05-17
==================

  * Fixed error when converting values in arrays

0.1.3 / 2012-10-17
==================

  * Fixed case problem with types

0.1.2 / 2012-06-27
==================

  * Added host-name String format
  * Added support for additionalProperties
  * Added few default validation messages for formats

0.1.1 / 2012-04-16
==================

  * Added default and custom error message support
  * Added suport for conform function
  * Updated date-time format

0.1.0 / 2011-11-09
=================

  * Initial release

