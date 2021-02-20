# shell00

## ex00 - z (OK)

* cat
  - 파일 내용을 그대로 출력해주는 명령어

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

* 링크 파일
  - 윈도우에서 바로가기와 같은 역할.
  - 하드링크와 심볼릭 링크가 있음.
  
  ```shell
  ln -s 원본파일 링크파일 (심볼릭링크)
  ```
  ```shell
  ln 원본파일 링크파일 (하드링크)
  ```
  
* 심볼릭 링크와 하드링크의 차이
  - <https://koromoon.blogspot.com/2018/05/inode-symbolic-link-hard-link.html>
  - 전통적인 유닉스 계통 파일 시스템에서 inode라는 자료구조를 사용함. 
  - 파일 시스템 내에서 파일이나 디렉토리는 고유한 inode를 가지고 있으며, inode 번호를 통해 각 파일이 구분 될 수 있는 것임.
  - 심볼릭 링크는 원본 파일의 inode를 가리키는 정보가 있는 파일. 원본 파일 위치에 대한 포인터만 포함 되므로 새로운 inode를 가진 링크파일이 생성됨.
  - 하드 링크는 원본 파일의 inode에 대한 직접적인 포인터. 하드링크에는 새로운 inode 생성이 없음. 
    <img src = "https://t1.daumcdn.net/cfile/tistory/215B143E56B6C9F72A" width = "400">
  - 하드링크는 원본파일을 삭제해도 계속 사용가능 vs 심볼릭링크는 원본파일을 삭제하면 사용 할 수 없음.
  - 심볼릭 링크는 파일 유형에 l로 표시되지만, 하드링크는 표시되지 않음.

* touch
  - 파일의 날짜시간 정보를 변경하는 명령어
  - ```touch -t yyyymmddhhmm 파일명```으로 사용 가능.
 
## ex03 - Connect me! (OK)

* Kerberos
  - 클라이언트-서버 구조에서 서버 접근권한에 대한 관리를 위해 대칭키 방식을 이용하여 인증하는 네트워크 인증 암호화 프로토콜
  - <http://blog.naver.com/PostView.nhn?blogId=hancury&logNo=221775416124&categoryNo=0&parentCategoryNo=10&viewDate=&currentPage=1&postListTopCurrentPage=1&from=search>
  - ```klist``` 만들어진 티켓목록 확인
  - ```kinit``` 티켓 생성 ```kinit id```로 해당 아이디 티켓 생성 가능한듯.
  - ```kdestroy``` 티켓 삭제. 순차로 1개만 삭제

## ex04 - midLS (KO -> 

* ls -mptU
  - ```ls``` 디렉토리 내용 확인
  - ```-m``` 파일명을 쉼표로 구분해준다
  - ```-p``` 파일 형태를 붙여서 보여준다. 디렉토리는 슬래시( / )가 붙는다
  - ```-t``` 최종수정일이 가장 최근인 것대로 정렬
  - ```-U``` 디스크의 순서대로 파일을 나열한다  do not sort; list entries in directory order 이게 뭔말인지 정확히 모르겠어서 찾아봤는데, 운영체제에 따라 달라지는듯.. 일단 mac에선 이 옵션을 추가하는 거랑 추가 안 하는 거랑 차이가 없게 나옴. 근데 우분투에선 값이 달라짐
  - <https://unix.stackexchange.com/questions/13451/what-is-the-directory-order-of-files-in-a-directory-used-by-ls-u>

## ex05 - Git commit (KO ->

```shell
git log -5 --format="%H"
```
* git log
  - ```-5``` 최근 커밋부터 5개를 출력해라
  - ```--format="%H"``` 커밋 해시만 출력해라
 
* 실패했던 코드
  ```shell
  git log -5 --pretty=format:"%H"
  ```
  - 이유: bash 쉘에서 확인해보면 마지막 줄에 줄바꿈이 안 일어나고 공백이 있는 상태로 끝나버리는 걸 볼 수 있다. moulinette에선 bash쉘을 이용하기 때문에 틀렸던 거라고 한다ㅠㅠ
 
## ex06 - gitignore (KO ->

```shell
git ls-files --others --ignored --exclude-standard
```
* .gitignore
  - 프로젝트에 원하지 않는 파일들을 깃에서 제외시킬 수 있는 설정 파일.
  - .gitignore파일을 생성하고 해당 파일에 제외시킬 이름이나 규칙을 적으면 된다.
  - 이 때 .gitignore파일은 항상 최상위 폴더에 위치해야 한다.
  - ```ls-files``` 파일형식만 출력
  - ```--others``` 트래킹 되지 않은 것들
  - ```--ignored --exclude-standard``` 그 중에서도 무시된 것들을 출력

## ex07 - diff (KO ->
```shell
patch -b a sw.diff
mv a b
rm a.origin sw.dirr
```

* diff
  - <https://www.geeksforgeeks.org/diff-command-linux-examples/>
  - 파일 비교 명령어
  - ```diff file1 file2```하면 file1과 file2의 차이점을 볼 수 있음.
  - < 는 file1 내용, > 는 file2 내용.
  - (file1라인)Special symbols(file2라인) 이런식으로 해당 파일의 무슨무슨 라인이 어떤 차이점이 발견된건지 요약해주고 < , > 로 해당 부분 내용 그대로 출력 해줌.
  - Special symbols에는 a(add), c(change), d(delete) 등이 있음. 1,2c1,8 이런식으로 표현됨. Special symbols를 기준으로 파일을 나눠서 생각하면 됨. file1의 1~2 라인이 file2에서 1~8라인으로 변경됐다는 걸 뜻함.

* patch
  - <https://www.thegeekstuff.com/2014/12/patch-command-examples/>
  - ```patch [option] original file patch file```
  - 오리지널 파일을 패치파일로 패치시킴. 이 때 -b 옵션을 통해 원본을 백업해둘 수 있음. 원본.origin 백업파일이 생성됨.
