Release notes
=============

This document describes all the changes made to the *Registry API* document,
starting from its first released version.


1.2.0
-----

* New element under `<client-credentials-in-use>`: `<rsa-public-key>`.
* New `<host>` section: `<server-credentials-in-use>`.
* Updated `manifest-example.xml`. Makes use of the newly introduced elements,
  and HTTP Signature authentication methods.
* Promote version 2 of the Echo API in the examples.


1.1.5
-----

* Updated `catalogue-example.xml` to be consistent with Discovery API's most
  recent `manifest-example.xml`.


1.1.4
-----

* More explicit security rules (add links to existing documents on security
  methods).


1.1.3
-----

* Updated example.


1.1.2
-----

* Just updated some links.


1.1.1
-----

* Updated example.


1.1.0
-----

* Extracted `<other-id>`'s type (`OtherHeiId`), so that it can now be reused in
  other APIs (in particular, the Institutions API).
* Added `euc` to the `<other-id>`'s `type` attribute's enumeration (Erasmus
  University Charter number).


1.0.2
-----

Minor changes only:

* Better explanations for XPath queries.
* Notices were added about the chance of having multiple `<other-id>` and
  `<name>` elements in HEI descriptions.
* The Registry Service is now allowed (but not required) to import malformed
  manifest files.
* Fixed the minimum version of importable manifest files.
* Added links to the Developers page in context of finding possible support
  libraries.
* Make catalogue example more consistent with actual registry response.
* Formatting fixes.


1.0.1
-----

* Added a paragraph about advantages of utilizing web caching when accessing
  Registry Service.

* Fixed an invalid `xsi:schemaLocation` reference in `catalogue-example.xml`.


1.0.0
-----

Initial release candidate.
