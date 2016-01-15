Registry API
============

* [What is the status of this document?][statuses]
* [See the index of all other EWP Specifications][develhub]


Registry Service
----------------

### There shall be only one

This document is placed in the "APIs" section of the EWP documentation, but it
describes an API which you SHOULD NOT implement on your servers. You will be
only *using* it (as a client).

The Registry Service *implements* the Registry API described below. If you're
not sure what the Registry is, please read the [proper chapter][registry-intro]
in the EWP Architecture document first.


### Location

*WRTODO: Final Location (URL) of the Registry Service is not yet determined.*


### Maintainers

Contact us when you want to add your Manifest to the EWP Registry Service:

 * **University of Warsaw** - usos@usos.edu.pl


How to use the Registry
-----------------------

### Request and Response

The Registry Service takes no parameters. It simply returns the response at the
proper URL. The response format is described in the attached [catalogue.xsd]
(catalogue.xsd) file. You may also review the [catalogue-example.xml]
(catalogue-example.xml) file for an example of a valid registry response.


### Caching

 * If possible, clients SHOULD cache the registry responses in production
   environments.

 * The clients MAY use the HTTP headers returned in the Registry response to
   determine the amount of time the Registry response should be cached for (the
   response will contain proper `Cache-Control` and `Expires` headers).

 * Clients MAY also choose their own constant value for such expiry, but it
   SHOULD NOT be lower than 5 minutes, and MUST NOT be greater than 12 hours.
   A value of **30 minutes** seems a reasonable recommendation.


### Is caching safe?

Yes. Manifest authors are advised that it MAY take some time for all changes to
propagate to all clients.

Also, since all APIs described by the registry should be backward compatible
after they are deployed onto production systems, then caching their `version`
numbers is also safe.


### Fallback cache (backup)

Optionally, clients MAY keep the cached copy of the Registry as a backup. E.g.
if the Registry Server cannot be contacted for some reason, a stale cached copy
of the Registry's response MAY be used instead.


Manifest verification
---------------------

 * The Registry Server will verify the SSL server certificates when it fetches
   the Manifest files from EWP Hosts.

 * The Registry Server will validate the manifest files before importing them.

 * If either SSL verification or Schema validation fails, the server WILL NOT
   import any new changes. It will attempt to notify the administrator of the
   EWP Host OR a human maintainer of the Registry Server, so that the problem
   will be noticed.

 * The Registry will always accept the latest version of the Manifest file
   (immediately after it is released).


SCHAC identifiers
-----------------

One of the fundamental features required of the EWP Network is the ability to
uniquely identify individual HEIs. A couple of HEI identifier types are already
in use in Europe, but only one of them - SCHAC identifier - is universal
enough.

SCHAC identifiers are quite ingenious in their simplicity. They identify HEIs
by Internet Domain Names registered for them. E.g. `uw.edu.pl` is the SCHAC ID
for the University of Warsaw.

SCHAC identifiers are obviously easy to be acquired by *a human*. We do
acknowledge however, that its not so easy for machines. Some student
information systems will probably identify external HEIs by their PIC or
Erasmus codes, and such systems won't be capable to acquire SCHAC identifiers
"on the fly".

For this reason, the EWP Registry provides a **mapping**. You will be able
to use the Registry's response to help you map between PIC identifiers, Erasmus
codes, and SCHAC IDs. The Registry will hold a database of these identifiers.
Initially, this database will be based on Manifest files, but other sources MAY
be used later on.


[registry-intro]: https://github.com/erasmus-without-paper/ewp-specs-architecture/blob/master/README.md#registry
[discovery-api]: https://github.com/erasmus-without-paper/ewp-specs-api-discovery/blob/master/README.md
[develhub]: http://developers.erasmuswithoutpaper.eu/
[statuses]: https://github.com/erasmus-without-paper/ewp-specs-management/blob/master/README.md#statuses
