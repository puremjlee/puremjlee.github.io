knowledge bundle 에 포함되는것
```
$SPLUNK_HOME/etc/system
$SPLUNK_HOME/etc/apps
$SPLUNK_HOME/etc/users.
```
이것들의 위치
```
search head :$SPLUNK_HOME/var/run
indexer :$SPLUNK_HOME/var/run/searchpeers
```
분산검색 환경에서 k.b 복제상태는 검색헤드에서 확인, 검색헤드 클러스터링이라면 캡틴에서 확인가능
