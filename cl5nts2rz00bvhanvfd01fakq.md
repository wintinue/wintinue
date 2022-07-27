## [Windows][CMD] 특정 포트 종료하기

이미 특정 포트를 점유하고 있는 프로세스를 종료시켜 해당 포트를 다시 사용할 수 있게 해보자

## 포트 프로세스 id 알아내기
`netstat`(network statistics) 툴을 사용해 원하는 포트를 찾는다.<br>
`netstat`는 프로토콜 통계와 현재 TCP/IP 네트워크 연결을 표시하는 툴이다.
### 모든 포트 조회하기
```
netstat -ano
``` 
![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1658942851106/zHZXNEl6D.png align="center")
### 특정 포트 조회하기
모든 포트를 조회하면 꽤 많은 결과가 나와 원하는 걸 찾기 어려울 것이다.<br>
이럴 땐 특정 문자열을 검색하는 `findstr`(find string) 명령어를 활용한다.
```
netstat -ano | findstr {포트번호}
``` 
![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1658942515239/-iOA3nV-U.png align="center")

### netstat 옵션
`netstat --help`로 옵션 값을 확인할 수 있다.
![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1658941259639/fzEui7D1U.png align="center")
여러 옵션을 적용해서 조회하고 싶다면 `-a -n -o` 또는 `-ano`와 같이 입력하면 된다.
<br>
- -a : 살아있는 모든 포트 표시
- -b : 포트 관련 실행파일 표시
- -e : 이더넷 통계 표시![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1658941518700/4fkfFt1cZz.png align="left")
- -f : 외부 주소의 정규화 도메인 이름 표시
- -n : 주소와 포트 번호를 숫자 형식으로 표시
- -o : 프로세스 id 표시
- -p : proto로 지정한 프로토콜 연결 표시
- -q : 모든 포트 및 비수신 대기 TCP 포트 표시
- -r : 라우팅 테이블 표시
- -s : 프로토콜별 통계 표시
- -t : 현재 연결 오프로드 상태 표시
- -x : 연결, 수신기, 엔드포인트(공유 끝점) 표시
- -y : TCP 연결 템플릿 표시

## 프로세스 종료하기
`taskkill` 명령어로 PID(프로세스 id)를 사용해 원하는 프로세스를 종료한다.
```
taskkill /f /pid {프로세스 id}
``` 
![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1658943036944/ZwPajXJDO.png align="center")
### taskkill 옵션
`taskkill /?`로 옵션 값을 확인할 수 있다.
![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1658943198070/m18xpUuUD.png align="center")
- /s : 원격 시스템 지정
- /u : 명령 실행하는 사용자 컨텍스트 지정
- /p : 사용자 컨텍스트 암호 지정
- /fi : 작업 집합 필터
- /pid : 종료할 프로세스 id 지정
- /im : 프로세스 이미지 이름 지정
- /t : 프로세스의 자식 프로세스까지 종료
- /f : 강제 종료
