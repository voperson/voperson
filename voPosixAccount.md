# voPosixAccount v2.0.0

* [Introduction](#introduction)
* [voPosixAccount Attribute Options](#voposixaccount-attribute-options)
  * [`scope-*` Attribute Description Option](#scope--attribute-description-option)
* [voPosixAccount Object Class and Attributes](#voposixaccount-object-class-and-attributes)
  * [voPosixAccount Object Class Definition](#voposixaccount-object-class-definition)
  * [`voPosixAccountGecos` Attribute Definition](#voposixaccountgecos-attribute-definition)
  * [`voPosixAccountGidNumber` Attribute Definition](#voposixaccountgidnumber-attribute-definition)
  * [`voPosixAccountHomeDirectory` Attribute Definition](#voposixaccounthomedirectory-attribute-definition)
  * [`voPosixAccountLoginShell` Attribute Definition](#voposixaccountloginshell-attribute-definition)
  * [`voPosixAccountUidNumber` Attribute Definition](#voposixaccountuidnumber-attribute-definition)
* [voPosixGroup Object Class](#voposixgroup-object-class)
  * [voPosixGroup Object Class Definition](#voposixgroup-object-class-definition)
* [Sample LDIF](#sample-ldif)
* [References](#references)
* [Changelog](#changelog)

---
![Creative Commons License](https://i.creativecommons.org/l/by-sa/4.0/88x31.png) 

This work is licensed under the [Creative Commons Attribution-ShareAlike 4.0 International License](http://creativecommons.org/licenses/by-sa/4.0/).

Copyright © 2022 by the respective authors.

---
# Introduction

In many virtual organizations, it is desirable to reflect cluster information on
the person's LDAP record (as opposed to in a standalone LDAP entry), and it may
be necessary to reflect multiple accounts on different clusters. This is not
possible with the *[posixAccount](https://datatracker.ietf.org/doc/html/rfc2307)*
schema, which dates from 1998, as almost all attributes are defined as
`SINGLE-VALUE`.

*voPosixAccount* is basically an update to the *posixAccount*, with the core
attributes in support of this use case redefined without the `SINGLE-VALUE`
limitation. The `voPosixAccount` and `voPosixGroup` object classes replace the
legacy definitions with definitions permitting multiple values.

# voPosixAccount Attribute Options

In order to support multiple values in the same object, it must be possible to
identify which attributes are associated with which accounts. voPosixAccount
uses *attribute options*. For more information on attribute options, see the
[main voPerson document](voPerson.md#voperson-attribute-options).

## `scope-*` Attribute Description Option

All voPosixAccount attributes are expected to use a scoping label to denote
the cluster for which that value is valid. The value of the scoping label is
meaningful only in the local context, and should otherwise be treated as opaque
except for comparison purposes (caseExactMatch).

# voPosixAccount Object Class and Attributes

## voPosixAccount Object Class Definition

```
( 1.3.6.1.4.1.25178.4.2
  NAME 'voPosixAccount'
  AUXILIARY
  MUST ( cn $
         uid $
         voPosixAccountUidNumber $
         voPosixAccountGidNumber $
         voPosixAccountHomeDirectory )
  MAY ( voPosixAccountLoginShell $
        voPosixAccountGecos ) 
)
```

## `voPosixAccountGecos` Attribute Definition

<table>
 <tr>
  <th>OID</th>
  <td>1.3.6.1.4.1.25178.4.2.1</td>
 </tr>
 
 <tr>
  <th>RFC4512 Definition</th>
  <td>
<pre>( 1.3.6.1.4.1.25178.4.2.1
        NAME 'voPosixAccountGecos'
        DESC 'voPerson domain specific GECOS field'
        EQUALITY caseIgnoreMatch
        SYNTAX '1.3.6.1.4.1.1466.115.121.1.15' )</pre>
  </td>
 </tr>
 
 <tr>
  <th>Multiple Values?</th>
  <td>Yes</td>
 </tr>
 
 <tr>
  <th>Attribute Options</th>
  <td>
   <ul>
    <li><code>scope-<i>cluster</i></code>: Denotes cluster for which this
    value is valid</li>
   </ul>
  </td>
 </tr>
</table>

### Definition

The GECOS field for the cluster account.

### Example

```
voPosixAccountGecos;scope-hpc: Pat X Lee
voPosixAccountGecos;scope-lab: Pat Lee,,,
```

## `voPosixAccountGidNumber` Attribute Definition

<table>
 <tr>
  <th>OID</th>
  <td>1.3.6.1.4.1.25178.4.2.2</td>
 </tr>
 
 <tr>
  <th>RFC4512 Definition</th>
  <td>
<pre>( 1.3.6.1.4.1.25178.4.2.2
        NAME 'voPosixAccountGidNumber'
        DESC 'voPerson domain specific primary group identifier'
        EQUALITY integerMatch
        SYNTAX '1.3.6.1.4.1.1466.115.121.1.27' )</pre>
  </td>
 </tr>
 
 <tr>
  <th>Multiple Values?</th>
  <td>Yes</td>
 </tr>
 
 <tr>
  <th>Attribute Options</th>
  <td>
   <ul>
    <li><code>scope-<i>cluster</i></code>: Denotes cluster for which this
    value is valid</li>
   </ul>
  </td>
 </tr>
</table>

### Definition

The primary group identifier for the cluster account.

### Example

```
voPosixAccountGidNumber;scope-hpc: 1008
voPosixAccountGidNumber;scope-lab: 10008
```

## `voPosixAccountHomeDirectory` Attribute Definition

<table>
 <tr>
  <th>OID</th>
  <td>1.3.6.1.4.1.25178.4.2.3</td>
 </tr>
 
 <tr>
  <th>RFC4512 Definition</th>
  <td>
<pre>( 1.3.6.1.4.1.25178.4.2.3
        NAME 'voPosixAccountHomeDirectory'
        DESC 'voPerson domain specific absolute path to the home directory'
        EQUALITY caseExactMatch
        SYNTAX '1.3.6.1.4.1.1466.115.121.1.15' )</pre>
  </td>
 </tr>
 
 <tr>
  <th>Multiple Values?</th>
  <td>Yes</td>
 </tr>
 
 <tr>
  <th>Attribute Options</th>
  <td>
   <ul>
    <li><code>scope-<i>cluster</i></code>: Denotes cluster for which this
    value is valid</li>
   </ul>
  </td>
 </tr>
</table>

### Definition

The home directory for the cluster account.

### Example

```
voPosixAccountHomeDirectory;scope-hpc: /home/plee
voPosixAccountHomeDirectory;scope-lab: /users/p/plee
```

## `voPosixAccountLoginShell` Attribute Definition

<table>
 <tr>
  <th>OID</th>
  <td>1.3.6.1.4.1.25178.4.2.4</td>
 </tr>
 
 <tr>
  <th>RFC4512 Definition</th>
  <td>
<pre>( 1.3.6.1.4.1.25178.4.2.4
        NAME 'voPosixAccountLoginShell'
        DESC 'voPerson domain specific path to the login shell'
        EQUALITY caseExactMatch
        SYNTAX '1.3.6.1.4.1.1466.115.121.1.15' )</pre>
  </td>
 </tr>
 
 <tr>
  <th>Multiple Values?</th>
  <td>Yes</td>
 </tr>
 
 <tr>
  <th>Attribute Options</th>
  <td>
   <ul>
    <li><code>scope-<i>cluster</i></code>: Denotes cluster for which this
    value is valid</li>
   </ul>
  </td>
 </tr>
</table>

### Definition

The login shell for the cluster account.

### Example

```
voPosixAccountLoginShell;scope-hpc: /bin/bash
voPosixAccountLoginShell;scope-lab: /bin/tcsh
```

## `voPosixAccountUidNumber` Attribute Definition

<table>
 <tr>
  <th>OID</th>
  <td>1.3.6.1.4.1.25178.4.2.5</td>
 </tr>
 
 <tr>
  <th>RFC4512 Definition</th>
  <td>
<pre>( 1.3.6.1.4.1.25178.4.2.5
        NAME 'voPosixAccountUidNumber'
        DESC 'voPerson domain specific unique user identifier'
        EQUALITY integerMatch
        SYNTAX '1.3.6.1.4.1.1466.115.121.1.27' )</pre>
  </td>
 </tr>
 
 <tr>
  <th>Multiple Values?</th>
  <td>Yes</td>
 </tr>
 
 <tr>
  <th>Attribute Options</th>
  <td>
   <ul>
    <li><code>scope-<i>cluster</i></code>: Denotes cluster for which this
    value is valid</li>
   </ul>
  </td>
 </tr>
</table>

### Definition

The UID number for the cluster account.

### Example

```
voPosixAccountUidNumber;scope-hpc: 1008
voPosixAccountUidNumber;scope-lab: 10008
```

# voPosixGroup Object Class

## voPosixGroup Object Class Definition

```
( 1.3.6.1.4.1.25178.4.3
  NAME 'voPosixGroup'
  AUXILIARY
  MUST ( cn $ voPosixAccountGidNumber )
  MAY ( memberUid )
)
```

# Sample LDIF

```
dn: voPersonID=V097531, ou=People, dc=myvo, dc=org
objectClass: person
objectClass: organizationalPerson
objectClass: inetOrgPerson
objectClass: voPerson
objectClass: voPosixAccount
cn: Patricia Q Lee
displayName: Pat Lee
givenName: Patricia
sn: Lee
uid;scope-hpc: pxlee
uid;scope-lab: plee
voPosixAccountUidNumber;scope-hpc: 1008
voPosixAccountUidNumber;scope-lab: 10008
voPosixAccountGidNumber;scope-hpc: 1008
voPosixAccountGidNumber;scope-lab: 10008
voPosixAccountHomeDirectory;scope-hpc: /home/plee
voPosixAccountHomeDirectory;scope-lab: /users/p/plee
voPosixAccountLoginShell;scope-hpc: /bin/bash
voPosixAccountLoginShell;scope-lab: /bin/tcsh
voPosixAccountGecos;scope-hpc: Pat X Lee
voPosixAccountGecos;scope-lab: Pat Lee,,,
voPersonCertificateDN;scope-cert1: CN=Pat Lee A251,O=Example,C=US,DC=cilogon,DC=org
voPersonCertificateIssuerDN;scope-cert1: CN=CILogon Basic CA 1, O=CILogon, C=US, DC=cilogon, DC=org
voPersonID: V097531
voPersonStatus: active
```

# References

1. [RFC 2307](https://datatracker.ietf.org/doc/html/rfc2307) posixAccount Object
Class Specification

# Changelog

voPersonAccount version numbers correlate to the latest voPerson version
at the time of the most recent change. For example, if voPerson is at version
2.0.1 and voPosixAccount is at version 2.0.0, then there are no changes to this
document since voPerson 2.0.0.

## [v2.0.0](https://github.com/voperson/voperson/tree/2.0.0)

* voPosixAccount and voPosixGroup were introduced with voPerson 2.0.0.