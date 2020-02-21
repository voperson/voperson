# voPerson SAML v2.0.0

* [Introduction](#introduction)
* [voPerson SAML Mapping Profiles](#voperson-saml-mapping-profiles)
  * [The Simple Profile](#the-simple-profile)
  * [The Advanced Profile](#the-advanced-profile)
* [Sample SAML](#sample-saml)
* [References](#references)
* [Changelog](#changelog)
 
---

# Introduction

This document describes the recommended approach for using the voPerson schema
in a [SAML2](http://docs.oasis-open.org/security/saml/v2.0/saml-core-2.0-os.pdf)
environment. Because voPerson allows for complex representation of metadata on
attributes, there is not a direct mapping that covers all use cases. This
document therefore introduces two profiles: the *Simple Profile* and the
*Advanced Profile*.

# voPerson SAML Mapping Profiles

## The Simple Profile

The Simple Profile is suitable for use cases where a component (the "sender") is
capable of applying some set of rules to reduce the complexity of the underlying
data that is provided to another component (the "receiver", typically a SAML
Service Provider). For example, in a proxy-based infrastructure the proxy may
have access to a set of application UIDs, and it may know that for a specific
receiver only one of those application UIDs is appropriate. Therefore, the full
representation of the `voPersonApplicationUID` attribute, including attribute
options, is not required to be sent to the receiver. It is beyond the scope of
this document to determine how such decisions are made.

The basic representation of voPerson attributes in SAML is governed by the
[SAML V2.0 X.500/LDAP Attribute Profile](http://docs.oasis-open.org/security/saml/Post2.0/sstc-saml-attribute-x500-cs-01.html).
In particular:

1. The voPerson attribute OID should be used as the SAML attribute OID.
1. The voPerson attribute name should be used as the SAML friendly name.
1. The SAML Attribute Profile specifically prohibits the use of "tagging options"
   (LDAP Attribute Options), and so these values should not be used in the
   Simple Profile.

Additionally, the Simple Profile makes the following recommendations to the
sender:

1. Do not send attributes that would otherwise be flagged with `prior` values.
1. `internal` values should only be sent if the receiver can be expected to
   treat all possible values for the attribute as internal.
1. Only send attributes that would leverage `app-`, `role-`, `scope-`, or 
  `time-`, if the correct context can be determined by the receiver.
1. Do not send more than one `voPersonApplicationUID`, as it may confuse the
   receiver.
1. Do not send more than one `voPersonStatus`, as per the voPerson
   specification.

## The Advanced Profile

_Pending further discussion_

# Sample SAML

```
<saml:Attribute FriendlyName="voPersonApplicationUID" Name="urn:oid:1.3.6.1.4.1.34998.3.3.1.1"
    NameFormat="urn:oasis:names:tc:SAML:2.0:attrname-format:uri">
    <saml:AttributeValue xmlns:xs="http://www.w3.org/2001/XMLSchema"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:type="xs:string">juser1</saml:AttributeValue>
</saml:Attribute>
```

# References

1. [SAML2](http://docs.oasis-open.org/security/saml/v2.0/saml-core-2.0-os.pdf)
   Security Assertion Markup Language
1. [SAML2Attr](http://docs.oasis-open.org/security/saml/Post2.0/sstc-saml-attribute-x500-cs-01.html)
   SAML V2.0 X.500/LDAP Attribute Profile

# Changelog

voPerson SAML document version numbers correlate to the latest voPerson version
at the time of the most recent change. For example, if voPerson is at version
2.0.1 and voPerson SAML is at version 2.0.0, then there are no changes to this
document since voPerson 2.0.0.

## [v2.0.0](https://github.com/voperson/voperson/tree/2.0.0)

* Initial Release.

