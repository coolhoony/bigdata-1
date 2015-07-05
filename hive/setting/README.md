# hive setting

###### mysql 설치 / hive 유저생성 및 권한 설정 / hive가 사용할 metastore database생성
```
$mysql –u root –p
mysql> grant all privileges on *.* to hive@localhost identified by 'hive' with grant option;
mysql> flush privileges;
mysql> grant all privileges on *.* to hive@'%' identified by 'hive' with grant option;
mysql> flush privileges;`
```

###### 환경변수 등록 hadoop 에 tmp 디렉토리 생성
```
$vi ~/.bash_profile
export HIVE_HOME=/home/hadoop/hive
export PATH=$PATH:$HADOOP_HOME/bin:$HIVE_HOME/bin

$hadoop fs -mkdir /tmp
$hadoop fs -chmod g+w /tmp
```

###### $HIVE_HOME/conf/hive-site.xml
```
<configuration>
	<property>
		<name>javax.jdo.option.ConnectionURL</name>
		<value>jdbc:mysql://hadoop01/hive?createDatabaseIfNotExist=true</value>
	</property>
	<property>
		<name>javax.jdo.option.ConnectionDriverName</name>
		<value>com.mysql.jdbc.Driver</value>
	</property>
	<property>
		<name>javax.jdo.option.ConnectionUserName</name>
		<value>hive</value>
	</property>
	<property>
		<name>javax.jdo.option.ConnectionPassword</name>
		<value>hive</value>
	</property>
	<property>
		<name>hive.metastore.uris</name>
		<value>thrift://hadoop01:9083</value>
	</property>
</configuration>
```
###### metastore 와 hive 서버 실행
```
$ hive --service metastore
$ hive --service hiveserver2
```

###### hive 접속
```
$beeline
beeline>!connect jdbc:hive2://hadoop01:10000
```
