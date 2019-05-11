**report란?** - saved search

**report의 목적**
1. 재사용
2. 스케줄링

**주요 topic**
리포트 생성
가속
스케줄링 설정


가속화
saved search(report)의 검색이 많아 시간이 오래걸리면 가속화 적용
가속화 적용하면 splunk가 백프로세스를 돌려서 data summary 를 만듦
나중에 다시 report로 검색하면 그 summary 를 대상으로 검색을 실행 (전체 인덱스가 아니라..)

가속화제한
피벗으로 생성한 report
role에 schedule_search / accelerate_search 기능없음
report에 write 권한 없을때
report 가속이 가능한 검색문이 아닐때
-transforming 명령어 혹은 streaming command(분산검색 가능한 명령어)로 시작
-스마트 혹은 고속모드로 실행
tags, event types, macros 사용한 검색은 invalid 결과를 반환
- verify summary 

**참고:세 가지 가속화 방식**
리포트 가속: 10만건 이상 혹은 hot bucket이 많으면 적용 추천
데이터 모델 가속
summary indexing
*리포트 가속 summary는 인덱스에, summary indexing summary는 SH에 저장
*리포트 가속은 통계를 미리 만들어 놓는 방식, 데이터 모델 가속은 데이터를 찾아가기 쉽도록 만드는 방식
*리포트 가속은 매 10분마다 실행

리포트 스케줄링
자원경합 시 우선순위: 실시간 모드(manually 스케줄링설정) > 연속모드(summary indexing) > 자동모드(report 가속)
연속모드는 Splunk 가 누락을 체크, 실시간 모드는 skip

**Dashboard**
http://dev.splunk.com/webframework
