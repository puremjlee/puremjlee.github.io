**knowledge bundle** 에 포함되는것
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
분산검색 환경에서 k.b 복제상태는 검색헤드에서 확인, 검색헤드 클러스터링이라면 캡틴에서 확인가능<br>
**search head 에서 인덱서 추가**
1. web GUI<br>
2. CLI from search head
```
splunk add search-server https://192.168.1.1:8089 -auth admin:password -remoteUsername admin -remotePassword passremote
```
3.distsearch.conf from search head
```
[distributedSearch]
servers = https://192.168.1.1:8089,https://192.168.1.2:8089
```
search head 재시작<br>
1,2번은 Splunk가 자동으로 인증 해결하지만 3번은 manual하게 키파일을 복사해야 함<br>
search head $SPLUNK_HOME/etc/auth/distServerKeys/trusted.pem 를 인덱서 $SPLUNK_HOME/etc/auth/distServerKeys/<searchhead_name>/trusted.pem 로 복사, <searchhead_name> 은 search head server.conf 의 서버이름, 인덱서들 재시작

**search head 데이터를 indexer 로 포워딩**
search head 에 S.o.S 앱 있다면 인덱서에도 설치<br>
_audit, _internal 은 중복해서 포워딩할 필요 없음<br>
search head outputs.conf 의 샘플
```
# Turn off indexing on the search head
[indexAndForward]
index = false
 
[tcpout]
defaultGroup = my_search_peers 
forwardedindex.filter.disable = true  
indexAndForward = false 
 
[tcpout:my_search_peers]
server=10.10.10.1:9997,10.10.10.2:9997,10.10.10.3:9997
```
**knowledge bundle 설정**
search head 의 distsearch.conf 우선순위 적용은 다음처럼
```
[replicationWhitelist]
[replicationSettings:refineConf]
```
**parallel reduce search**
indexer는 map, search head는 reduce를 담당하는 분산 검색 구조에서 search head 의 reduce job 일부를 indexer 레이어에서 처리하도록 하는 방법으로 high-cardinality searches 에 유리<br>
indexer cluster / multi indexer 구조에 모두 적용 가능, 원리는 인덕서 일부를 intermediate reducers 로 지정해서 reduce job을 수행시키는 것<br>
indexer cluster 라면 random으로 선택되도록 하는것이 이상적, defulat 는 전체 indexer 개수의 50% 4개가 max (limits.conf에서 수정가능)<br>
multi indexer 에서는 dedicated indexer 를 명시<br>
redistribute 커맨드 사용시 num_of_reducers 키워드 사용하면 limits.conf 오버라이드<br>
intermediate reducer 에서 실행되는 동시 검색 수는 CPU 코어 갯수<br>


**동시검색수**
<br>
How many historycal(adhoc+scheduled) searches can be run concurrently?<br>
14 CPU cores<br>
20(max_hist_searches) = 1(max_search_per_cpu) x 14(number_of_cpus) + 6(base_max_searches)<br>

How many real-time searches can be run concurrently?<br>
20(max_rt_searches) = 1(max_rt_search_multiplier) x 20(max_hist_searches)<br>

How many scheduled(<->adhoc) searches can be run concurrently?<br>
max_searches_perc = 60<br>
12(max_hist_scheduled_searches) = 60%(max_searches_perc) x 20(max_hist_searches)<br>
12(max_rt_scheduled_searches) = 60%(max_searches_perc) x 20(max_rt_searches)<br>

How many auto summarization(Report Acceleration/DataModel Acceleration) searches can be run concurrently?
auto_summary_perc = 50<br>
6(max_auto_hist_summary_search) = 12(max_hist_scheduled_searches) x 50%(auto_summary_perc)<br>
6(max_auto_rt_summary_search) = 12(max_rt_scheduled_searches) x 50%(auto_summary_perc)<br>
```
| rest /services/server/status/limits/search-concurrency

```
