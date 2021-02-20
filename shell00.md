# shell00

## ex00 - z (OK)
```shell
$ vi z
Z

:q
```

## ex01 - testShell00 (OK)

* chmod
  - <http://www.incodom.kr/Linux/%EA%B8%B0%EB%B3%B8%EB%AA%85%EB%A0%B9%EC%96%B4/chmod>
  - 리눅스 권한 관리 명령어
  - reference : user/group/others 순서. all은 a
  - operator : + 추가, - 제거, = 바로변경
  - modes : read/write/execute 순서. - 는 사용권한 없음. 4/2/1 로 각 자리 숫자 더해서 u/g/o 순서로 부여 가능.
 
* ls -l
  - 파일유형+각 유저의 권한 / 링크수 / 소유자 / 파일 크기(바이트) / 최종 수정 시간 / 파일이름
  - 순으로 출력

## ex02 - Oh yeah, mooore... (KO ->

* 심볼릭 링크
  - 윈도우에서 바로가기와 같은 역할.
  - 링크를 연결하여 원본 파일을 직접 사용하는 것과 같은 효과를 내는 링크
  
* 심볼릭 링크와 하드링크의 차이
  - <https://koromoon.blogspot.com/2018/05/inode-symbolic-link-hard-link.html>
  - 전통적인 유닉스 계통 파일 시스템에서 inode라는 자료구조를 사용함. 
  - 파일 시스템 내에서 파일이나 디렉토리는 고유한 inode를 가지고 있으며, inode 번호를 통해 각 파일이 구분 될 수 있는 것임.
  - 심볼릭 링크는 원본 파일의 inode를 가리키는 정보가 있는 파일. 원본 파일 위치에 대한 포인터만 포함 되므로 새로운 inode를 가진 링크파일이 생성됨.
  - 하드 링크는 원본 파일의 inode에 대한 직접적인 포인터. 하드링크에는 새로운 inode 생성이 없음. 
  - <img src = "https://t1.daumcdn.net/cfile/tistory/215B143E56B6C9F72A">
