---
layout: post
title: "[ Java ] 셀레니움 IE 기본설정(selenium IE)"
categories: back
comments: true
---

셀레니움은 IE, 크롬, FF, 엣지를 지원을 하는데 공공기관, 금융기관에 접속하려면 IE가 필요하니 IE 드라이버를 설치한다.

(그나저나 IE는 32비트 64비트로 나뉜다... 64비트 IE에서는 엑티브엑스고 뭐고 설치도 안됨. 역시 지랄임 )

IE는 32,64비트 관계없이 32비트를 권장한다.

[셀레니움 IE 드라이버 다운로드](https://selenium-release.storage.googleapis.com/index.html) 에서 다운받는다. 난 새로운거 다운받아서 집컴에서 돌리니 너무 최신이라 버전이 안맞는다며 낮은버전으로 해야해서 회사에서 쓰는 드라이버로 다시 가져가 쓰니 됨.(참 버전문제랑 비트문제는 여러모로 속썩임)

### 보호모드 사용 끄기

인터넷옵션 - 보안 - 인터넷, 로컬인트라넷, 신뢰할수있는 사이트, 제한한 사이트 모두 '보호모드사용(IE를 다시 시작해야함)' 을 체크해제해준다!!!!

인터넷옵션 - 고급 - 보안에서 '향상된 보호모드 사용\*' 을 해지한다.

### ZOOM LEVEL

브라우저에서 about:blank 들어간뒤에 [보기]-[확대/축소]에 들어가서 100%인지 확인

디스플레이설정에 가서 배율 및 레이아웃이 100%인지도 확인(윈도우10에서는 확인란이 있음)

### 레지스트리 설정

window+R 누르고 regedit 검색. 64비트라면 두번 작업해주어야함.

- 32비트 윈도우

HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Internet Explorer\Main\FeatureControl\FEATURE_BFCACHE

- 64비트 윈도우

HKEY_LOCAL_MACHINE\SOFTWARE\Wow6432Node\Microsoft\Internet Explorer\Main\FeatureControl\FEATURE_BFCACHE

Feature Control에 들어가면 FEATURE_BFCACHE가 없을텐데 새 키 를 늘러서 새로 만들어준다. FEATURE_BFCACHE를 만들면 디폴트로 기본값이 뜰텐데 오른쪽에 새로운 값을 추가한다.

타입읍 DWORD로, 이름은 iexplore.exe로 , 값은 0으로 (아마 디폴트가 0.00000 일거임)
