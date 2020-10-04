## slapd란 
Stand-Alone LDAP Daemon

<br/><br/><br/>

## 엔트리 종류 

주요 엔트리의 종류들을 정리 합니다. 
이 보다더 많은 엔트리들이 존재하며, 주로 사용되고 있는 엔트리들은 아래와 같습니다. 

- CN (Common Name) : KilDong Hong, SaRang Lee 와 같은 일반적인 이름들 
- SN (SirName) : 우리나라 성에 해당한다. Lee, Hong ..
- OU (Organiztion Unit) : 그룹에 해당  
- DC (Domain Component) : 도메인에 대한 요소 ex ) tech.example.com dc 는 example.com 혹은 tech.example.com 모두 해당 
- DN (Distinguished Name) : 위의 엔트리 및 기타 여러가지 엔트리들이 모여 특정한 한 사용자(혹은 물체)를 구분할 수 있는 고유의 이름.  


<br/><br/><br/>

## Objectclass 

모든 엔트리 들은 하나하나의 Objectclass 를 갖는다.  
Objectclass 란 동일한 설정을 갖고 있는 일종의 그룹이라고 이해를 하면 될 것 같습니다. 
새로운 엔트리가 생성될 때, 특정한 Objectclass에 속하는 엔트리로 생성을 하게 되면 해당 Objectclass의 속성을 그대로 상속받게 되는 원리임. 

<br/><br/><br/>

## ACL (Access Control List) 

ACL 이란 우리가 파일시스템에서 사용자 혹은 그룹에 파일에 대한 접근 권한을 주어서 보안을 유지하는 것 처럼 파일과 디렉토리에 대한 소유자와 권한을 설정해주는 것입니다. 
LDAP 내의 각각의 엔트리 및 속성에 접근 권한을 설정할 수 있으며, 이에 대한 기본은 다음과 같습니다. 

```
  access to <권한제한 설정>  
  by <누구> <엑세스 권한>  
```

Example >  
```
  access to attr=userpassword  
  by self write  
  by * none  
```

위의 예에서는 패스워드 변경 속성에 대해서 자기 자신만 write(변경) 할 수 있고 나머지 사용자들은 변경해서는 안된다는 속성에 대한 예를 보여줍니다. 
```
  access to *   
  by dn="cn=admin,dc=nahaeya,dc=com" write  
  by * read  
```
위의 예에서는 관리자만이 모든 속성에 대한 권한을 갖고 나머지 사용자들은 읽기 권한만 갖고 있는 것을 말한다. 

<br/><br/><br/>

##  ldap CLI 사용하기 

ldapscripts 패키지에는 아래와 같은 많은 CLI들을 제공 합니다. 
```
ldapadd                  ldapdelete               ldappasswd
ldapaddgroup             ldapdeletegroup          ldaprenameuser
ldapaddmachine           ldapdeleteuser           ldapsearch
ldapadduser              ldapdeleteuserfromgroup  ldapsetprimarygroup
ldapaddusertogroup       ldapmodify               ldapwhoami
ldapcompare              ldapmodrdn
```

<br/><br/><br/>

## LDIF

주로 사용하는 ldapadd 나 ldapmodify 명령어를 사용할 때에는 LDIF 파일 구조를 사용합니다. 
LDIF(LDAP Data Interchange Format)란 LDAP의 데이터를 텍스트 형식으로 표현하는 데이터 포맷입니다. 

LDIF 파일에 대한 example
```bash
cd /etc/ldapscripts/ 
ls 
```
```
ldapaddgroup.template.sample
ldapadduser.template.sample
ldapscripts.passwd
ldapaddmachine.template.sample
ldapscripts.conf
```
위와 같이 여러가지 sample 들이 있는 것을 알 수 있습니다. 


<br/><br/><br/>

## ldap client
1. http://directory.apache.org/studio/download/download-windows.html
2. http://www.ldapadmin.org/
3. 

<br/><br/><br/>

Ref
1. https://osankkk.tistory.com/entry/LDAP-%EC%A0%95%EB%A6%AC
2. https://www.golinuxcloud.com/ldap-tutorial-for-beginners-configure-linux/
3. https://dev.to/tomahawkpilot/setup-an-openldap-in-centos-5hge
4. https://linuxhostsupport.com/blog/how-to-install-ldap-on-centos-7/
5. https://jabcholove.tistory.com/89

