# Shell 01 정리
## ex01
* groups, id -nG

  <https://linuxize.com/post/how-to-list-groups-in-linux/>
  ```
  groups
  id -nG
  ```

  등으로 띄어쓰기를 포함한 그룹이름 출력 가능. 근데 man id 가 힌트였어서 그냥 id -nG로 했음.

  이 때, 공백 대신 쉼표를 출력하기 위해 sed명령어 사용.
* sed
  <http://www.incodom.kr/Linux/%EA%B8%B0%EB%B3%B8%EB%AA%85%EB%A0%B9%EC%96%B4/sed>

  ```
  sed 's/찾을텍스트/바꿀텍스트/' 파일명
  ```
  위 명령어를 적용하면
  ```
  sed 's/ /,/' 파일명
  ```
  이 된다. 이 때 sed의 인풋 즉 파일명을 그룹이름 목록으로 넣어주기 위해 pipe( | )를 사용한다.

*  | (pipe)와 > (redirect)의 차이점.
  pipe는 결과를 다른 프로그램의 input으로 전달할 때 사용. 그니까 | 오른쪽에 명령어가 보통 옴
  redirect는 결과를 파일에 남기는 것. > 오른쪽에는 파일이 옴. .txt 같은 거
 
