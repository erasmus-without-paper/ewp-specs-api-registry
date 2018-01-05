Release notes
=============

This document describes all the changes made to the *Registry API* document,
starting from its first released version.


1.4.0
-----

* Changes in `type` attribute of the `OtherHeiId` `complexType`:

  - Introduced the new `erasmus-charter` value to the enumeration.
  - The `euc` value is now deprecated (in favor of `erasmus-charter`).
  
  See the specs, and
  [this thread](https://github.com/erasmus-without-paper/ewp-specs-api-registry/issues/3)
  for more information.


1.3.2
-----

* Updated `catalogue-example.xml`.
* Stated explicitly, the the Registry will support both `4.x.x` and `5.x.x`
  versions of the Discovery Manifest API.


1.3.1
-----

* Updated `catalogue-example.xml`.


1.3.0
-----

* Previously, the catalogue contained only the digests of RSA public keys, but
  after recent changes to the specs of some new authentication methods, the
  actual bodies of these keys also became needed. You can read about it
  [here](https://github.com/erasmus-without-paper/ewp-specs-sec-cliauth-httpsig/issues/1).
  This resulted in a couple of changes:

  - New `binaries/rsa-public-key` catalogue section. It contains the contents
    (bodies) of all RSA public keys referenced in other parts of the catalogue.
    Both XSD and XML example were updated.

  - Updated XPath queries in the `README.md` file. These new versions fit
    better with the recent revisions of the `httpsig` authentication specs.

* Minor changes to internal schema inheritance structure. Extracted one data
  type (`ewp:Sha256Hex`) to an external (common) XML namespace.


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
