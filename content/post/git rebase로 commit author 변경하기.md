---
title: "git rebase로 commit author 변경하기"
date: 2019-03-29T16:04:49+09:00
draft: true
---
# git rebase
git의 커밋 이력을 관리 할 수 있는 git rebase에 대해 정리해 보려고 합니다.
git rebase는 이미 커밋한 메세지를 수정한다거나, 사소한 커밋이 여러번 발생된 경우 하나의 커밋으로 merge시킨다거나 혹은 작성자를 잘못 커밋한 경우 수정변경하는데 사용할 수 있어요.

이중에서 오늘은 작성자를 잘못 커밋한 경우에 대해 수정하는 방법을 정리해 보려고 해요.
git에는 얼마나 커밋하며 활동했는지를 한눈에 볼 수 있는 초록색 도장이 찍혀있는 표가 있는데 작성자를 해당 프로젝트의 계정이 아닌 다른 계정으로 커밋할 경우 이 초록색 도장이 찍히지 않아요.
물론  이 초록색 도장이 그 사람의 실력을 말해주는 것은 아니지만 적어도 그 사람에게 있어 개발에 대한 성실도? 정도는 표현해 주는 표라 생각되므로 잘못 커밋한 내역의 초록색 도장을 살려내는 방법을 소개해 볼게요.

### rebase 실행
rebase대상 커밋의 바로 이전 커밋으로 rebase 명령어를 입력합니다.
```js
$ git rebase -i -p <commit hash>
```
![enter image description here](https://user-images.githubusercontent.com/43326846/53309514-992f6180-38eb-11e9-8cc5-56cc5d3ca5db.png)

> commit hash는 $git log를 통해서나 소스트리에 보면 커밋컬럼에서 알 수 있어요.

### edit모드로 저장
pick으로 되어 있는 리스트들이 rebase대상이에요.
![enter image description here](https://user-images.githubusercontent.com/43326846/53309525-a0566f80-38eb-11e9-8440-793026d57281.png)     

vim을 편집모드로 전환 후 pick -> edit로 변경후 명령모드에서 :wq 명령어로 저장합니다.
![enter image description here](https://user-images.githubusercontent.com/43326846/53310123-d0534200-38ee-11e9-86a1-d106d27cde15.png)

> vim화면에서 명령어모드에서는 아무리 글자를 써도 수정이 되지 않아요. 편집모드로 변경하는 단축키로 편집모드로 전환 후 수정해주세요.(windows 편집모드: insert키, mac 편집모드: i)
> 편집모드에서 명령어 모드로 전환은 esc키로 가능.

### 커밋로그의 작성자 변경
아래의 명령어로 커밋로그의 작성자를 변경해줍니다.
```js
$git commit --amend --author="작성자이름 <작성자 이메일>"
```
커밋로그에 순차적으로 하나씩 적용됨으로 작성자를 수정한 후에 아래 명령어를 실행하면 다음 커밋로그가 target이 되고 다시 작성자 변경하는 명령어를 실행합니다.
```js
$git rebase --continue
```
모든 rebase대상이 완료되면 아래와 같이 코멘트가 나옵니다.
![enter image description here](https://user-images.githubusercontent.com/43326846/53309596-2672b600-38ec-11e9-82cc-df9d8dfaf1d0.png)

### 원격저장소에 push
마지막으로 원격저장소에 강제 push를 하게 되면 수정된 내용이 반영됩니다.
```js
$git push origin +master
```