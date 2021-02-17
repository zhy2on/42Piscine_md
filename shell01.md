# Shell 01 정리
## ex01

* 우선 FT_USER 환경변수를 설정해줘야 한다. 방법은 ```export 변수명=해당내용```
  
  이후 잘 설정됐는지 보려면  ```echo $변수명```으로 확인해주면 된다.
  
* groups, id -nG

  - <https://linuxize.com/post/how-to-list-groups-in-linux/>
  
  - 
    ```shell
    groups
    id -nG
    ```
    등으로 띄어쓰기를 포함한 그룹이름 출력 가능. 근데 man id 가 힌트였어서 그냥 id -nG로 했음.

  - 이 때, 공백 대신 쉼표를 출력하기 위해 sed명령어 사용.
* sed
  - <http://www.incodom.kr/Linux/%EA%B8%B0%EB%B3%B8%EB%AA%85%EB%A0%B9%EC%96%B4/sed>

  - 
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

## ex02
