옵션| 설명
---- | ----
-p | 각 커밋에 적용된 패치를 보여준다.
--stat | 각 커밋에서 수정된 파일의 통계정보를 보여준다.
--shortstat | --stat 명령의 결과 중에서 수정한 파일, 추가된 라인, 삭제된 라인만 보여준다.
--name-only | 커밋 정보중에서 수정된 파일의 목록만 보여준다.
--name-status | 수정된 파일의 목록을 보여줄 뿐만 아니라 파일을 추가한 것인지, 수정한 것인지, 삭제한 것인지도 보여준다.
--abbrev-commit | 40자 짜리 SHA-1 체크섬을 전부 보여주는 것이 아니라 처음 몇 자만 보여준다.
--relative-date | 정확한 시간을 보여주는 것이 아니라 “2 weeks ago” 처럼 상대적인 형식으로 보여준다.
--graph | 브랜치와 머지 히스토리 정보까지 아스키 그래프로 보여준다.
--pretty | 지정한 형식으로 보여준다. 이 옵션에는 oneline, short, full, fuller, format이 있다. format은 원하는 형식으로 출력하고자 할 때 사용한다.
--oneline | --pretty=oneline --abbrev-commit 두 옵션을 함께 사용한 것과 같다.