# Section 09 DNS

- 호스트는 이륻 대신 IP 주소로 통신
  - `/etc/hosts` 파일에 IP 이름 매핑하면 사용 가능
  - 서버가 많아지고 IP가 자주 바뀌면 관리가 어려움.

### DNS 서버
- 중앙에서 IP-이름 매핑을 관리
- 모든 호스트의 `/etc/reolv.conf`에 DNS 서버 지정
- 모르는 호스트가 오면 DNS를 체크

### 네임 해석 순서
- `/etc/nsswith.conf`에 의해 결정
- 기본값: `files dns`
  - `/etc/hosts` 확인하고 없으면 DNS 서버 조회

### 도메인 이름
- `.`으로 그룹화 되어 있음.
- `mail.google.com`
  - `.` -> `com` -> `google` -> `mail`
- 사내 도메인도 자체 도메인을 가질 수 있다.
  - `/etc/resolv.conf`의 `search` 옵션 활용

### DNS 레코드 종류
- A 레코드: IPv4
- AAAA 레코드: IPv6
- CNAME 레코드: alias

### 명령어
- `ping`: 연결 확인 host + DNS
- `nslookup`: DNS 서버만 질의, host 무시
- `dig`: 더 상세한 DNS 쿼리 결과 표시