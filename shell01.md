# Shell01 정리
## ex01

* 우선 FT_USER 환경변수를 설정해줘야 한다. 방법은 ```export 변수명=해당내용```
  
  이후 잘 설정됐는지 보려면  ```echo $변수명```으로 확인해주면 된다.
  
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
  
## ex02
  
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

## ex03

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
ls -Rl | grep "^d\|-" | wc -l
```

## ex04

* MAC Address
  - MAC 주소는 "Media Access Control"의 약자로 네트워크 카드 하드웨어에 부여되는 고유한 물리적 주소이다

* ifconfig
  - -a : 모든 네트워크 정보를 볼 수 있음 (근데 이거 안 해도 잘 나옴)
```shell
ifconfig | grep 'ether ' | sed 's/	ether //g' | sed 's/ //g'
```
## ex05

* file name이 될 수 없는 특수 문자
  - ```\ / : * ? " ' < > |```
  - 해당 이름으로 파일을 만들기 위해 백슬래쉬를 사용. -->파일 접근할 때도 백슬래쉬 써줘야 한다;; 개귀찮 따로 적어두기
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

## ex06

* awk
  - <https://unix.stackexchange.com/questions/26723/print-odd-numbered-lines-print-even-numbered-lines>
  - 특정 부분만 출력할 때 사용. 패턴 탐색과 처리를 위한 명령어.
  - <http://www.incodom.kr/Linux/%EA%B8%B0%EB%B3%B8%EB%AA%85%EB%A0%B9%EC%96%B4/awk>
  ```
  $ awk 'pattern' filename 조건
  ```
```shell
ls -l | awk 'NR % 2 == 0'
```

## ex07

* grep -v
  - 해당하는 부분 '빼고' 출력
```grep -v '^#'```

* sed -n "_start_, _end_p"
  - 특정 범위 출력
  - 이 때, 파일 끝을 표현하기 위해선 \$를 사용
