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