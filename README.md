# Openldap-CentOS-7
install openldap on centos7 and explain about openldap
ref : https://linuxhostsupport.com/blog/how-to-install-ldap-on-centos-7/

<br/><br/>

## Install OpenLDAP

```bash
sudo -s
yum -y update
yum -y install vim openldap compat-openldap openldap-clients openldap-servers openldap-servers-sql openldap-devel

systemctl start slapd
systemctl enable slapd

# admin password
slappasswd

```

<br/><br/>

## Configure OpenLDAP

```bash
# vim db.ldif

dn: olcDatabase={2}hdb,cn=config
changetype: modify
replace: olcSuffix
olcSuffix: dc=field,dc=linuxhostsupport,dc=com
 
dn: olcDatabase={2}hdb,cn=config
changetype: modify
replace: olcRootDN
olcRootDN: cn=ldapadm,dc=field,dc=linuxhostsupport,dc=com
 
dn: olcDatabase={2}hdb,cn=config
changetype: modify
replace: olcRootPW
olcRootPW: hashed_output_from_the_slappasswd_command
```

<br/>

```bash
ldapmodify -Y EXTERNAL -H ldapi:/// -f db.ldif

```

<br/>

```bash
# vim monitor.ldif

dn: olcDatabase={1}monitor,cn=config
changetype: modify
replace: olcAccess
olcAccess: {0}to * by dn.base="gidNumber=0+uidNumber=0,cn=peercred,cn=external, cn=auth" read by dn.base="cn=ldapadm,dc=field,dc=linuxhostsupport,dc=com" read by * none
```

<br/>

```bash
ldapmodify -Y EXTERNAL -H ldapi:/// -f monitor.ldif
```

<br/><br/>

### LDAPS protocol

```bash
openssl req -new -x509 -nodes \
-out /etc/openldap/certs/myldap.field.linuxhostsupport.com.cert \
-keyout /etc/openldap/certs/myldap.field.linuxhostsupport.com.key \
-days 365

chown -R ldap:ldap /etc/openldap/certs
```

<br/>

```bash
# vim certs.ldif

dn: cn=config
changetype: modify
replace: olcTLSCertificateKeyFile
olcTLSCertificateKeyFile: /etc/openldap/certs/myldap.field.linuxhostsupport.com.key

dn: cn=config
changetype: modify
replace: olcTLSCertificateFile
olcTLSCertificateFile: /etc/openldap/certs/myldap.field.linuxhostsupport.com.cert

```

<br/>


```bash
# deploy the configuration
ldapmodify -Y EXTERNAL -H ldapi:/// -f certs.ldif

# test the configuration
slaptest -u
```

<br/><br/>

## Setting up the OpenLDAP database

```bash
cp /usr/share/openldap-servers/DB_CONFIG.example /var/lib/ldap/DB_CONFIG
chown -R ldap:ldap /var/lib/ldap
```

<br/>

```bash

# Add the LDAP schemas

ldapadd -Y EXTERNAL -H ldapi:/// -f /etc/openldap/schema/cosine.ldif
ldapadd -Y EXTERNAL -H ldapi:/// -f /etc/openldap/schema/nis.ldif
ldapadd -Y EXTERNAL -H ldapi:/// -f /etc/openldap/schema/inetorgperson.ldif
```

<br/>

```bash
# vim base.ldif

dn: dc=field,dc=linuxhostsupport,dc=com
dc: field
objectClass: top
objectClass: domain

dn: cn=ldapadm,dc=field,dc=linuxhostsupport,dc=com
objectClass: organizationalRole
cn: ldapadm
description: LDAP Manager

dn: ou=People,dc=field,dc=linuxhostsupport,dc=com
objectClass: organizationalUnit
ou: People

dn: ou=Group,dc=field,dc=linuxhostsupport,dc=com
objectClass: organizationalUnit
ou: Group
```

<br/>

```bash
# deploy these configuration changes to the OpenLDAP server using the ldapadm user

ldapadd -x -W -D "cn=ldapadm,dc=field,dc=linuxhostsupport,dc=com" -f base.ldif
```




