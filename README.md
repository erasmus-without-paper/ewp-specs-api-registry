Registry API
============

* [What is the status of this document?][statuses]
* [See the index of all other EWP Specifications][develhub]


Summary
-------

This document describes the API implemented by the <b>Registry Service</b>. It
is placed in the "APIs" section of the documentation, but this does not imply
that we want you to implement it. You will be only <b>using</b> it (as a
client).


Registry Service
----------------

The Registry Service *implements* the Registry API described below. If you're
not sure what the Registry is, please read the [proper chapter][registry-intro]
in the EWP Architecture document first.

As opposed to all other services served inside the EWP network, the location of
the catalogue served by the Registry Service is fixed and should not change.
This means that you may *hardcode* this location into your client applications:

```
https://registry.erasmuswithoutpaper.eu/catalogue-v1.xml
```


How to use the Registry
-----------------------

### Request and Response

The Registry Service takes no parameters. It simply returns the response at the
proper URL. The response format is described in the attached
[catalogue.xsd](catalogue.xsd) file. You may also review the
[catalogue-example.xml](catalogue-example.xml) file for an example of a valid
registry response.


### Caching

 * If possible, clients SHOULD cache the registry responses in production
   environments. That is, clients should query the registry only once in a time
   and then reuse this response.

 * The clients MAY use the HTTP headers returned in the Registry response to
   determine the amount of time the Registry response should be cached for (the
   response will contain proper `Cache-Control` and `Expires` headers).

 * Clients MAY also choose their own constant value for such expiry, but it
   SHOULD NOT be lower than 1 minute, and MUST NOT be greater than 3 hours.
   A value of **15 minutes** seems a reasonable recommendation.

 * When querying the Registry, clients MAY also utilize web caching techniques,
   such as
   [If-Modified-Since](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.25) or
   [If-None-Match](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.26)
   HTTP headers. If these request headers are used properly, the Registry will
   respond with *HTTP 304 Not Modified* status (thus reducing the time you need
   for receiving and loading the catalogue).


### Is caching safe?

Yes. Manifest authors are advised that it MAY take some time for all changes to
propagate to all clients.

Also, since all APIs described by the registry should be backward compatible
after they are deployed onto production systems, then caching their `version`
numbers is also safe.


### Fallback (backup) cache

Clients MAY keep the stale copy of the Registry's catalogue as a backup. E.g.
if the Registry Server cannot be contacted for some reason, a stale cached copy
of the Registry's response MAY be used instead (for a limited time).


<a name='queries'></a>

### XPath queries

The purpose of the catalogue served by the Registry Service is to allow the
clients to answer the following questions:

* **Question 1:** At which URLs and in which versions API X is implemented for
  institution Y?

  Let's assume that you are searching for implementations of Echo API `1.x.x`,
  and Y's ID is `hei.edu`. The list of all matching API implementations can be
  found with an XPath expression similar to this one:

  `//r:hei-id[text()="hei.edu"]/../../r:apis-implemented/e1:echo`

  Then, you need look through the list of returned elements and:

  - Make sure that APIs are implemented in the versions you require. Usually
    you will want the entry with the highest `version` attribute (the entries
    in the Registry response are not ordered, you will need to order them
    yourself).

* **Question 2:** I have received a HTTPS request signed by a client
  certificate `cert`. Data of which HEIs is this client privileged to access?

  Determine the certificate's SHA-256 fingerprint first (e.g.
  `DigestUtils.sha256Hex(cert.getEncoded())` if you're using Java). Then, you
  can use an XPath expression similar to this one:

  `//r:certificate[@sha-256="<your-digest>"]/../../r:institutions-covered/r:hei-id`

  Note, that the IDs returned by this query are **not necessarily unique**.

Namespace context used in the XPath examples above:

 * `r` - Registry API response namespace,
 * `e1` - Echo API `stable-v1` manifest-entry namespace.

There are many other types of queries which can be run against the catalogue.
If you think we should include more examples here, please start a new issue for
that.


### Support libraries

There might be some client libraries implemented for accessing the Registry
Service response. If there is one for your language, then - perhaps - you won't
need to implement it yourself. Check out the [EWP Developers][develhub] page.


How is the registry updated?
----------------------------

Data served by the Registry Service can be acquired from various sources, but
the majority of it is being fetched from the
[manifest.xml files][discovery-api] served by all the EWP partners.


### How manifest files are fetched

 * The Registry Server will verify the SSL server certificates when it fetches
   the Manifest files from EWP Hosts.

 * The Registry Server will validate the manifest files before importing them.
   This includes the XML Schema validation, and meeting some other specific
   constraints (e.g. related to the security of certificates used). Note, that
   each EWP Host can be a subject to additional constraints specific to its own
   (e.g. EWP Hosts in Poland might be required to cover institutions with `.pl`
   SCHAC ID suffix only).

 * If validation fails, the registry MAY attempt to import some of the data (the
   parts that are valid), but is NOT REQUIRED to. The registry MUST attempt to
   notify the maintainers of the faulty manifest file, and/or the maintainers of the
   Registry Service itself, so that the problem will be noticed.

 * The Registry Service will understand Discovery Manifest files formatted
   according to Discovery API [specifications][discovery-api-releases] in
   versions `4.0.0` **and higher**.


### Adding a new manifest file

Contact Registry Service maintainers when you want to add your Manifest to the
EWP Registry Service. The email address of the current Registry Service
maintainer can be found on the
[Registry Service's welcome page](https://registry.erasmuswithoutpaper.eu/).


<a name="schac-ids"></a>

SCHAC identifiers
-----------------

One of the fundamental features required of the EWP Network is the ability to
uniquely identify individual HEIs. A couple of HEI identifier types were
already in use in Europe at the time the EWP network was designed, but only one
of them - SCHAC identifier - seemed to be universal enough.

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


[registry-intro]: https://github.com/erasmus-without-paper/ewp-specs-architecture/blob/stable-v1/README.md#registry
[discovery-api]: https://github.com/erasmus-without-paper/ewp-specs-api-discovery
[discovery-api-releases]: https://github.com/erasmus-without-paper/ewp-specs-api-discovery/releases
[develhub]: http://developers.erasmuswithoutpaper.eu/
[statuses]: https://github.com/erasmus-without-paper/ewp-specs-management/blob/stable-v1/README.md#statuses
