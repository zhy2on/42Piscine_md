# Shell01 

## ex01 - print_groups (KO -> OK)

* 우선 FT_USER 환경변수를 설정해줘야 한다. 방법은 ```export 변수명=해당내용```
  
  이후 잘 설정됐는지 보려면  ```echo $변수명```으로 확인해주면 된다.
  - echo는 유닉스 계열 운영 체제에서 문자열을 컴퓨터 터미널에 출력하는 명령어이다. 개행문자를 인식하지 않고 그대로 출력하는 성격이 있다. 명령어를 echo로 감싸면 출력에서 개행문자를 따로 제거해 주지 않아도 된다. 
  
* groups, id -nG
  - <https://linuxize.com/post/how-to-list-groups-in-linux/>
    ```shell
    groups
    id -nG
    ```
    등으로 띄어쓰기를 포함한 그룹이름 출력 가능. 근데 man id 가 힌트였어서 그냥 id -nG로 했음.
  - 이 때, 공백 대신 쉼표를 출력하기 위해 sed명령어 사용.

* sed
  - <http://www.incodom.kr/Linux/%EA%B8%B0%EB%B3%B8%EB%AA%85%EB%A0%B9%EC%96%B4/sed>
    ```shell
    sed 's/찾을텍스트/바꿀텍스트/g' 파일명
    ```
  - 이 때 g를 입력하지 않으면 처음 찾은 문자에만 적용 되고 나머지 문자에는 적용 안 됨.  
  - 위 명령어를 적용하면  
    ```Shell
    sed 's/ /,/g' 파일명
    ```
    이 된다. 이 때 sed의 인풋 즉 파일명을 그룹이름 목록으로 넣어주기 위해 pipe( | )를 사용한다.
    
*  | (pipe)와 > (redirect)의 차이점.  
    - pipe는 결과를 다른 프로그램의 input으로 전달할 때 사용. 그니까 | 오른쪽에 명령어가 보통 옴.    
    - redirect는 결과를 파일에 남기는 것. > 오른쪽에는 파일이 옴. .txt 같은 거  
  ```shell
  id -nG $FT_USER | sed 's/ /,/g'
  ```
  
***
수정 (개행문자 제거 필요...)
 ```shell
  groups $FT_USER | sed 's/ /,/g' | tr -d '\n'
```
***
  
## ex02 - find_sh (OK)
  
* find
  - <https://recipes4dev.tistory.com/156>  
  - 해당 디렉토리 및 하위 디렉토리에서 검색 ``` find . -name ```  
  - <https://soooprmx.com/archives/6764>
  - -name pattern: 파일의 베이스 이름(디렉토리 이름 뗀)이 주어진 패턴과 매치하는지 본다.
  - 특정 이름 검색 ex) .sh로 끝나는 파일 ``` "*.sh" ``` 
  ```shell
  find . -name "*.sh"
  ```
 
* sed
  - . (dot) 같은 특수문자는 역슬래쉬를 붙여야 올바르게 인식된다.
  - 개행문자는 \n이 아니라 그냥 \ 으로 입력한다.

* basename
  - <https://steemit.com/bash/@pelican7/bash-basename>
  - basename은 기본적으로 디렉토리 빼고 확장자 포함 파일명을 보여줌. 여기서 확장자도 빼고 싶으면 ``` basename -s ".*" ``` 해주면 됨.

* exec / execdir
  - exec는 starting directory에서, execdir은 matching target이 있는 directory에서 command line실행
  - find와 같이 사용할 때 {}가 의미하는 바는, find에서 찾아진 파일명들이 쫙 들어가 있는 거.
  - \+ 또는 ; 로 종료돼야 하는데, basename에서 ;를 인식시켜주기 위해 \\;로 쓰는 것 같음. 그냥 + 써주기

```shell
find . -name "*.sh" -execdir basename -s ".sh" {} +
```

## ex03 - count_files (KO -> KO ->

* ls - Rl
  - 하위 디렉토리까지 포함해서 출력

* grep
  - <https://recipes4dev.tistory.com/157>
  - ```grep ^ ``` 문자열 라인의 처음 
  - ```grep "a\|b" ``` a 또는 b가 포함된 라인 검색. 파이프( | )를 이용하기 위해 앞에 백슬래쉬를 붙여준다.

* wc -l
  - wc == word count
  - 사용자가 지정한 파일의 행, 단어, 문자수를 세는 프로그램. 
  - -l 옵션은 행단위로 세는 것.
```shell
ls -Rl | grep "^d\|^-" | wc -l
```

***
수정
```shell
find . \( -type f -o -type d \) | wc -l | sed 's/       //'

또 수정

find . \( -type f -o -type d \) | wc -l | sed 's/^ *//'
```

- 현재 디렉토리 및 하위 디렉토리에서 파일 또는 디렉토리 타입을 찾고 행단위로 개수를 세서 출력을 하는데, 공백을 제거하고 출력한다.
***

## ex04 - MAC(OK)

* MAC Address
  - MAC 주소는 "Media Access Control"의 약자로 네트워크 카드 하드웨어에 부여되는 고유한 물리적 주소이다

* ifconfig
  - -a : 모든 네트워크 정보를 볼 수 있음 (근데 이거 안 해도 잘 나옴)
```shell
ifconfig | grep 'ether ' | sed 's/	ether //g' | sed 's/ //g'
```
## ex05 - Can you create it ? (KO ->

* file name이 될 수 없는 특수 문자
  - ```\ / : * ? " ' < > |```
  - 해당 이름으로 파일을 만들기 위해 백슬래쉬를 사용. -->파일 접근할 때도 백슬래쉬 써줘야 한다
  ```shell
  \”\\\?$\*\’MaRViN\’\*$\?\\\”
  ```
* 개행문자 제거
  - <https://serverfault.com/questions/307370/in-linux-why-does-an-empty-file-have-a-size-of-0-but-a-text-file-with-any-cont>
  - ```$ echo -n 'a' > /tmp/test_text```

```shell
vi \”\\\?$\*\’MaRViN\’\*$\?\\\”
echo -n '42' > \”\\\?$\*\’MaRViN\’\*$\?\\\”
```

***
수정 큰따옴표 잘못 들어감
```shell
vi \"\\\?$\*\'MaRViN\'\*$\?\\\"
echo -n '42' > \"\\\?$\*\'MaRViN\'\*$\?\\\"
```
***

## ex06 - Skip (KO -> OK)

* awk
  - <https://unix.stackexchange.com/questions/26723/print-odd-numbered-lines-print-even-numbered-lines>
  - 특정 부분만 출력할 때 사용. 패턴 탐색과 처리를 위한 명령어
  - <http://www.incodom.kr/Linux/%EA%B8%B0%EB%B3%B8%EB%AA%85%EB%A0%B9%EC%96%B4/awk>
  - NR 변수 == new line 줄별로 조건을 주고 싶을 때 사용한다.
  ```
  $ awk 'pattern' filename 조건
  ```
```shell
ls -l | awk 'NR % 2 == 0'
```

***
수정 (total부터가 시작임. 홀수행은 나머지 1이 맞음)
```shell
ls -l | awk 'NR % 2 == 1'
```
***

## ex07 - r_dwssap (OK)

* etc/passwd
  - <https://webdir.tistory.com/129>
  - <https://harryp.tistory.com/878>
  - 리눅스에서는 파일로 사용자 계정을 관리 한다.
  - /etc/passwd 파일을 통해 계정을 관리하고, /etc/shadow 파일을 통해 패스워드를 관리하게 된다.

* grep -v
  - 해당하는 부분 '빼고' 출력
  ```grep -v '^#'```

* awk pattern filename
  ```awk 'NR % 2 == 0'```
  
* 로그인만 출력
  ```sed 's/:.*//'```
  
* rev
  - <https://webdir.tistory.com/143>
  - 한 행을 역순 출력
  ```rev```
  
* sort -r
  - 역순 정렬
  ```sort -r```
 
* sed -n "$환경변수, $환경변수p"
  - 위에서 설명한 것과 같음
  ```sed -n "${FT_LINE1},${FT_LINE2}p"```
* tr
  - sed와 비슷한데 문자열 지원 x
  - sed에서 개행문자 사용이 어려우니 tr로 '\n'먼저 바꾸고 진행
  ```tr '\n' ','```
  
* sed 's/,/, /g'
  - ', '으로 구분 되게 바꾸기

* sed 's/, $/./'
  - 마지막 ', '를 '.'으로 바꾸기

* tr -d '\n'
  - 개행문자 제거
```shell
cat /etc/passwd | grep -v '^#' | awk 'NR % 2 == 0' | sed 's/:.*//' | rev | sort -r | sed -n "${FT_LINE1},${FT_LINE2}p" | tr '\n' ',' | sed 's/,/, /g' | sed 's/, $/./' | tr -d '\n'
```

## ex08 - add_chelou (OK)

* 환경변수 세팅
  ```shell
  export FT_NBR1=\\\'\?\"\\\"\'\\
  export FT_NBR2=rcrdmddd
  --> Sault
  
  export FT_NBR1=\\\"\\\"\!\\\"\\\"\!\\\"\\\"\!\\\"\\\"\!\\\"\\\"\!\\\"\\\"
  export FT_NBR2=dcrcmcmooododmrrrmorcmcrmomo
  --> Segmantation fault
  ```
  
* tr
  - 문자 치환
  ``` tr "\'\\\\\"?\!" "01234"| tr "mrdoc" "01234" ```

* xargc
  - <http://www.dreamy.pe.kr/zbxe/CodeClip/164220>
  - 파이프 이전의 내용을 인자로 받아 명령어를 실행하는 구조

* ibase, obase
  - ibase는 인풋을 해당 진수로 변경
  - obase는 아웃풋을 해당 진수로 변경
  ```xargs echo "ibase=5; obase=23;"```
  - 주의! 
  - <https://stackoverflow.com/questions/9889839/bc-and-its-ibase-obase-options>
  - ibase를 하는 순간 ibase로 진수가 변환 됨. ibase를 먼저 하고 obase를 쓰려면 obase를 ibase 진수에 맞춰서 적어줘야 된다.
  - ex) ibase = 5; obase = 23; -->ibase를 5진수로 한 순간 obase도 5진수로 읽는다. 그러므로 여기서 obase = 23; 은 13진수를 의미한다.
  - 해결: obase를 먼저 쓰면 됨
  ``` xargs echo "obase=13; ibase=5;" ```

* bc
  - 계산

* tr 치환
  ``` tr "\'\\\\\"?\!" "01234"| tr "mrdoc" "01234" ```
```shell
echo $FT_NBR1 + $FT_NBR2 | tr "\'\\\\\"?\!" "01234"| tr "mrdoc" "01234" | xargs echo "obase=13; ibase=5;" | bc | tr "0123456789ABC" "gtaio luSnemf"
```
