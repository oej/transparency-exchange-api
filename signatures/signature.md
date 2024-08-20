# Trusting digital signatures in TEA

Software transparency requires a trust platform so that users
can validate the information and artefacts published. Given
the situation today any information published is better than
none, so the framework for digital signatures will not
be mandatory for API compliance. Implementations may
require all published information to be signed and
validated.

Within the
API documents can be signed with an electronic
signature. CycloneDX boms supports signatures within
the JSON file, but other artefacts may need external
signature files, a detached signature.

- __Integrity__: Documents dowloaded needs to be the same
  as documents published
- __Identity__:
  - Customers need to be able to verify the
  publisher of the documents and verify that it is
  the expected publisher.
  - A TEA server may want to verify that published
  documents are signed by the expected publisher
  and that signatures are valid.

In order to sign an object, a pair of asymmetric keys will be
needed. The public key is used to create a certificate, signed
by a certificate authority (CA). The private key is used for
signing and needs to be protected.

A software publisher may buy CA services from a commercial vendor
or set up an internal solution. The issue with internal PKIs is that
external parties do not automatically trust that internal PKI.

This document outlines a proposal on how to build that trust and
make it possible for publishers to use an internal PKI. It is
of course important that this PKI is maintained according to
best current practise.

## API trust

The TEA API is built on the HTTP protocol with TLS encryption
and authentication, using the https:// URL scheme.

The TLS server certificate is normally issued by a public Certificate
Authority that is part of the Web PKI. The client needs to validate
the TLS server certificate to make sure that the certificate name
(CN or Subject Alt Name) matches the host part of the URI.

If the certificate validates properly, the API can be trusted.
Validation proves that the server is the right server for the
given host name in the URL. This trust can be used to
implement trust in a private PKI used to sign documents. In
addition, trust anchors can be published in DNS as an extra
level of validation.

## Getting trust anchors

Much like the EST protocol, the TEA protocol can be used
to download trust anchors for a private PKI. These are
PEM-encoded certificates in one single text file.

The TEA API has a `/trust-anchors/` API that will download
the current trust anchor APIs. This file is not signed,
that would cause a chicken-and-egg problem. The certificates
in the file are all signed.

An implementation should download these and apply them only
for this service, not in global scope. A PKI valid for example.com
is not valid for example.net.

## Validating the trust anchors using DNSsec (DANE)

## Digital signatures

### Digital signatures as specified for CycloneDX

> "Digital signatures may be applied to a BOM or to an assembly within a BOM.
> CycloneDX supports XML Signature, JSON Web Signature (JWS), and JSON Signature Format (JSF).
> Signed BOMs benefit by providing advanced integrity and non-repudiation capabilities."
<https://cyclonedx.org/use-cases/#authenticity>

### External (detached) digital signatures for other documents

- indication of hash algorithm
- indicator of cert used
- intermediate cert and sign cert

### Validating the digital signature

## Using Sigstore for signing

Sigstore is an excellent free service for both signing of GIT commits as well
as artefacts by using ephemeral certificates (very shortlived) and a
certificate transparency log for validation and verification.
Sigstore signatures contain timestamps from a timestamping service.

## Suggested PKI setup

### Root cert

#### Root cert validity and renewal

### Intermediate cert

#### Intermediate cert validity and renewal

### Signature

#### Time stamp services

### DNS entry

## References

- IETF RFC DANE
- IETF DANCE architecture (IETF draft)
- IETF Digital signature
- JSON web signatures (JWS) - <https://datatracker.ietf.org/doc/html/rfc7515>
- JSON signature format (JSF) - <https://cyberphone.github.io/doc/security/jsf.html>
- [IETF Enrollment over Secure Transport (EST) RFC 7030](https://www.rfc-editor.org/rfc/rfc7030)
