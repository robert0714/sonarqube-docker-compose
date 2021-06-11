# Executing th below command
```
 mkdir -p sonar/sonar/conf
 mkdir -p sonar/sonar/data
 mkdir -p sonar/sonar/logs
 mkdir -p sonar/sonar/extensions
 mkdir -p sonar/sonar/postgres
 chmod -R 777 sonar
```


# Setting  vm.max_map_count

由於SonarQube: 8.4.1 Community Edition 上有elasticsearch的關係，必須調高 vm.max_map_count的值。


觀察當下max_map_count的值
```
# more /proc/sys/vm/max_map_count
```
當下設定 max_map_count 的值 (重啟會失效)
```
# sysctl -w vm.max_map_count=262144
```
當下設定 max_map_count 的值 (永久生效)

```
1. # vi /etc/sysctl.conf
     vm.max_map_count=262144

2. # vi /etc/rc.local 
     #put parameter inside rc.local file
     echo 262144 > /proc/sys/vm/max_map_count
```
#### Vue Eslint Plugin for SonarQube
https://github.com/SonarSource/eslint-plugin-sonarjs

#### SonarLint - Eclipse plugins
[SonarLint](https://marketplace.eclipse.org/content/sonarlint#group-details)可以連線到SonarQube或是SonarCloud，取得掃描規則。直接對Eclipse中的projects進行掃描。

#### Docker command
(Option 1) Basic for Mac 
```
docker run --rm  -p 9000:9000 -v sonarqube_extensions:/opt/sonarqube/extensions   sonarqube:8.4.1-community
```
## Running SonarScanner from the Docker image
To scan using the SonarScanner Docker image, use the following command:
```bash
docker run \
    --rm \
    -e SONAR_HOST_URL="http://${SONARQUBE_URL}" \
    -e SONAR_LOGIN="myAuthenticationToken" \
    -v "${YOUR_REPO}:/usr/src" \
    sonarsource/sonar-scanner-cli
```

## Running SonarScanner from Maven
Edit the settings.xml file, located in $MAVEN_HOME/conf or ~/.m2, to set the plugin prefix and optionally the SonarQube server URL.

Example:
```xml
<settings>
    <pluginGroups>
        <pluginGroup>org.sonarsource.scanner.maven</pluginGroup>
    </pluginGroups>
    <profiles>
        <profile>
            <id>sonar</id>
            <activation>
                <activeByDefault>true</activeByDefault>
            </activation>
            <properties>
                <!-- Optional URL to server. Default value is http://localhost:9000 -->
                <sonar.host.url>
                  http://10.100.98.200:9000/sonar/
                </sonar.host.url>
            </properties>
        </profile>
     </profiles>
</settings>
```
#### Analyzing

Analyzing a Maven project consists of running a Maven goal: sonar:sonar from the directory that holds the main project pom.xml. You need to pass an authentication token using the sonar.login property in your command line.

```bash
mvn clean verify sonar:sonar -Dsonar.login=myAuthenticationToken
```

In some situations you may want to run the sonar:sonar goal as a dedicated step. Be sure to use install as first step for multi-module projects

```bash
mvn clean install
mvn sonar:sonar -Dsonar.login=myAuthenticationToken
```
To specify the version of sonar-maven-plugin instead of using the latest:

```bash
mvn org.sonarsource.scanner.maven:sonar-maven-plugin:3.7.0.1746:sonar
```

To get coverage information, you'll need to generate the coverage report before the analysis.


### Ldap Configuration
Add the parameters as the below.
```sonar.properties
ldap.authentication=simple
ldap.url=ldap://192.168.2.12:389
ldap.bindDn=
ldap.bindPassword=
ldap.user.baseDn=DC=iead,DC=local
ldap.user.request=(&(objectClass=person)(sAMAccountName={login}))
ldap.user.realNameAttribute=name
ldap.user.emailAttribute=mail
ldap.group.baseDn=DC=iead,DC=local
ldap.group.request=(&(objectClass=group)(member={dn}))
```
