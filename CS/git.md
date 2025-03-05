 
# git Bash

git은 자체적으로 Linux계열 명령어를 사용. os 독립성 보유
매뉴얼 https://git-scm.com/book/en/v2
## basic

```bash

# 현재 위치
$pwd      


# 이동
$cd Documents    
$cd h:\ '내 드라이브' 

# 생성
$mkdir git_exam  # 폴더
$vim f1.txt      # txt파일 편집기


# 정보확인
$ls -al          # 파일목록 출력


# 조작
$ctrl+insert / shift+insert  # 복사/붙여넣기
$ clear          # 터미널창 지우기


# 삭제
$rm 파일이름
$rm f1.txt t2.txt f3.txt
$rm -r 폴더이름
$rm -r myfolder/ # 폴더 및 내부파일 삭제
$rm -rf 폴더이름 # 강제삭제
# -f : 강제삭제, -r : 폴더삭제


# 조건부 삭제
$find . -name "*.log" -delete  # 현재위치에서 .log 확장자 파일 모두 삭제
$find . -type f -mtime +30 -exec rm {} \; # 현재위치에서 30일 이상 지난 파일 삭제


```

## vim editor
txt 파일 만들기
- vim f1.txt : vim이라는 프로그램으로 f1이름의 txt파일 생성
- i : 편집모드
- esc : 리딩모드
- ":" : 명령어모드 터미널 진입
- :wq : 저장 종료
- cat f1.txt : gitbash에서 파일내용 출력
- cp f1.txt f2.txt : f1.txt를 카피한 f2.txt 생성



# version control

## 주요 상태
**Untracked - Unstaged - Staged - Committed**
-  Untracked : 미추적 상태, Git이 관리하지 않는 새 파일
	-  Unstaged : 수정했지만 git add 안 한 상태
-  Staged : stage(버전대기상태), 커밋할 준비가 된 상태, git add로 실행
-  Committed : repository(버전생성상태), 로컬 저장소에 커밋 완료된 상태, commit으로 실행

```bash
echo "Hello" > file.txt  # 새 파일 생성 
git status               # 'Untracked' 상태 확인 
git add file.txt         # Staged 상태로 변경 
git commit -m "Add file" # 커밋 완료
```



## initializing
==$git== : git 명령어 출력

==$ git 명령어 -옵션1옵션2옵션3== : 문법구조

==$git init==  : 버전관리 파일(.git) 생성

==$git config --global user.name imim555==
==$git config --global user.email imim555893@gmail.com==
	- 누가 버전을 만졌는지 사용자정보 생성

==$ git commit --help== 
- 메뉴얼 페이지 출력

==$ git commit -a== : 수정한 파일을 자동으로 add 시켜줌, 즉 add+commit 동시 진행
==$ git commit -am "version11"== : add+commit+message



## recording
==$ git status==
	- 파일 관리 상태 출력 
	- untracked files : 버전관리되고 있지 않음
==$ git add f1.txt==
	- tracking 되고 있는 상태로 stage area(커밋대기상태)에 넣은 것. 아직 버전생성X
	- 즉 add가 되어서 커밋대기상태에 들어가야 commit할 수가 있음
	- 이는 작업들을 선택적으로 commit하기 위한 사전절차인 셈 
	- 최초 추적이나 파일수정되어 재추적할 때 모두 사용 (버전생성 전)
	- 버전 : 의미있는 변화. 작업을 단위 단위로 구분할 수 있는 정도


f1.txt과 f2.txt를 모두 수정 후 f1.txt만 add 처리
```Bash 
$ git status
# ----------------------

On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        modified:   f1.txt

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   f2.txt

```


==$ git commit==
	- 버전 생성!!
	- stage area에 있는 파일을 repository로 생성
	- vim editor 모드로 넘어가서 추적상태가 주석으로 출력됨
	- 여기서 우리는 이 파일들이 왜 명명되었는지, 이 버전의 이름 등의 commit message 작성 필요 
- `git commit --amend` 커밋메시지 변경



## history
==$ git diff==
	- <mark style='background:var(--mk-color-yellow)'>add 이전</mark> 작업 변경 내용 확인, 즉 untracked 상태인 파일의 변경내용만 확인
	- 만약 모든 변경파일이 add해서 stage 상태로 넘어갔다면 아무것도 출력되지 않음



==$ git log==
	- <mark style='background:var(--mk-color-yellow)'>commit 이후</mark> 버전 변경 내용 확인
	- 버전관련 정보 출력 : commit ID, author, Date, message
	- 모든 버전들의 역사를 순차적으로 출력
	- 종료하려면 q
	- 로그 기록은 .git 에서 관리하는 영구적 정보


```bash title:"최초 commit 했을 때 로그 기록"
$ git log
# -------------------------

commit 0d0bf423e7a436158d8efcbc1fa9687445112c8c (HEAD -> master)
Author: imim555 <imim555893@gmail.com>
Date:   Fri Oct 25 20:38:36 2024 +0900

    version_1
    : Please enter the commit message for your changes. Lines starting

```

```Bash title:"두번째 commit 했을 로그 기록" 
$ git log
# ------------------------

commit 74770f371ee9c2738a5688618d57e5aeaefb7a6b (HEAD -> master)
Author: imim555 <imim555893@gmail.com>
Date:   Fri Oct 25 20:53:03 2024 +0900

    version_2

commit 0d0bf423e7a436158d8efcbc1fa9687445112c8c
Author: imim555 <imim555893@gmail.com>
Date:   Fri Oct 25 20:38:36 2024 +0900

    version_1
    : Please enter the commit message for your changes. Lines starting

```



==$ git log -p==
	- 저장된 모든 version 간 차이점을 보기
	-  각 커밋마다 a(전) -> b(후) 순서로 변경된 작업내용을 보여줌
	- 즉, b 버전 기준으로 변경된내용(a)/갱신된내용(b) 표현 
	- 실제 작업과정
		- ver1: f1.txt 생성 (sorce: ll)
		- ver2: f1.txt 수정 (souce: 2)
		- ver3: f2.txt 생성 (souce: 2)
		- ver4: f1.txt 수정 (f1.txt : 2), 
		*- ver5: f1.txt 수정(f1.txt: 5), f2.txt 수정 (f2.txt : 2)* 
```Bash hl:4,9,11,12,20,print title:'$ git log -p'
$ git log -p


# 가장 최근 버전인 ver5 기준 변경된 정보 출력
commit 29b233b689fdf23f33c4bef9104ec73895ccc19b (HEAD -> master)
Author: imim555 <imim555893@gmail.com>
Date:   Sat Oct 26 03:30:15 2024 +0900

    version_5   # commit할때 내가 입력한 버전정보

# 대상 파일이 2개여서 별도로 정보 표시
diff --git a/f1.txt b/f1.txt   # f1.txt의 전후를 a(ver4) -> b(ver5) 
index 9462317..e3a30dc 100644
--- a/f1.txt     # (1) ver5에서 변경된 파일 : f1.txt
+++ b/f1.txt     # (2) ver5에서 변경된 파일 : f1.txt
@@ -1 +1 @@
-f1.txt : 2      # (1) 작업내용: "f1.txt : 2" 제거
+f1.txt : 5      # (2) 작업내용: "f1.txt : 5" 갱신

diff --git a/f2.txt b/f2.txt    # f2.txt의 전후를 a(ver4) -> b(ver5)
index 2456b16..ed48ea0 100644
--- a/f2.txt    # (1) ver5에서 변경된 파일 : f2.txt
+++ b/f2.txt    # (2) ver5에서 변경된 파일 : f2.txt
@@ -1 +1 @@
-source : 2     # (1) 작업내용: "source : 2" 제거
+f2.txt : 2     # (2) 작업내용: "f2.txt : 2" 갱신


# ver4 기준 변경된 정보 출력
commit f87d1c57ab95d03047eb4e58b75f4d5c9711304a (HEAD -> master)
Author: imim555 <imim555893@gmail.com>
Date:   Fri Oct 25 21:25:14 2024 +0900

    version_4    # commit할때 내가 입력한 버전정보

diff --git a/f1.txt b/f1.txt   # a(ver3)과 b(ver4)의 차이
index 2456b16..9462317 100644
--- a/f1.txt    # (1) ver4에서 변경된 파일 : f1.txt
+++ b/f1.txt    # (2) ver4에서 commit한 파일 : f1.txt >> 파일수정
@@ -1 +1 @@
-source : 2     # (1) 작업내용: "source : 2" 제거
+f1.txt : 2     # (2) 작업내용: "f1.txt : 2" 갱신


# ver3 기준 변경된 정보 출력
commit 5845bcca30b361c3a2eafe7c8648465ce4ef03dd
Author: imim555 <imim555893@gmail.com>
Date:   Fri Oct 25 21:11:14 2024 +0900

    version_3    # commit할때 내가 입력한 버전정보

diff --git a/f2.txt b/f2.txt
new file mode 100644
index 0000000..2456b16
--- /dev/null   #(1) ver3에서 변경된 파일 : 없음
+++ b/f2.txt    #(2) ver3에서 commit한 파일 : f2.txt >> 최초생성
@@ -0,0 +1 @@
+source : 2     #(2) 작업내용


# ver2 기준 변경된 정보 출력
commit 74770f371ee9c2738a5688618d57e5aeaefb7a6b
Author: imim555 <imim555893@gmail.com>
Date:   Fri Oct 25 20:53:03 2024 +0900

    version_2    # commit할때 내가 입력한 버전정보

diff --git a/f1.txt b/f1.txt
index a1c7d6a..2456b16 100644
--- a/f1.txt    #(1) ver2에서 변경된 파일 : f1.txt 
+++ b/f1.txt    #(2) ver2에서 commit한 파일 : f1.txt >> 파일수정
@@ -1 +1 @@
-source : ll    #(1) 작업내용: "soure : ll" 내용이 변경됨
+source : 2     #(2) 작업내용: "source : 2" 내용이 갱신됨


# ver1 기준 변경된 정보 출력
commit 0d0bf423e7a436158d8efcbc1fa9687445112c8c
Author: imim555 <imim555893@gmail.com>
Date:   Fri Oct 25 20:38:36 2024 +0900

    version_1    # commit할때 내가 입력한 버전정보
    : Please enter the commit message for your changes. Lines starting

diff --git a/f1.txt b/f1.txt
new file mode 100644
index 0000000..a1c7d6a
--- /dev/null   #(1) ver1에서 변경된 파일 : 없음
+++ b/f1.txt    #(2) ver1에서 commit한 파일 : f1.txt >> 최초생성
@@ -0,0 +1 @@
+source : ll    #(2) 작업내용: "source : 2" 내용이 생성


```



==$ git log ID1..ID2== : 
	- 두 소스코드 간의 차이가 a ->b 프레임으로 표현
	- a->b 정보는 실제 작업 순서대로 표시X, 명령어에 입력한 ID 순서대로 표시O
```Bash title:"ver2 -> ver3 의 차이"
$ git diff 74770f371ee9c2738a5688618d57e5aeaefb7a6b..5845bcca30b361c3a2eafe7c8648465ce4ef03dd  # ver2의 commitId .. ver3의 commitId
# -----------------------------------------------

diff --git a/f2.txt b/f2.txt   # a(ver2), b(ver3)  => f2.txt(source:2) 생성
new file mode 100644
index 0000000..2456b16
--- /dev/null
+++ b/f2.txt
@@ -0,0 +1 @@
+source : 2



```

```Bash title:"ver4 -> ver2 의 차이"
$ git diff f87d1c57ab95d03047eb4e58b75f4d5c9711304a..74770f371ee9c2738a5688618d57e5aeaefb7a6b  # ver4..ver2
# ---------------------------------------


# a..b 순으로 버전정보가 입력되며 시간상 b 기준으로 설명

diff --git a/f1.txt b/f1.txt  # f1.txt: a(ver4), b(ver2)
index 9462317..2456b16 100644
--- a/f1.txt  # b 기준으로 변경된 파일
+++ b/f1.txt  # b 기준으로 갱신된 파일
@@ -1 +1 @@
-f1.txt : 2  # b기준으로 변경된 정보(즉 a의 상태)
+source : 2  # b기준으로 갱신된 정보


diff --git a/f2.txt b/f2.txt  # f2.txt: a(ver4), b(ver2)
deleted file mode 100644  # b기준으로 파일은 삭제됨
index 2456b16..0000000
--- a/f2.txt   # b기준으로 변경된 파일
+++ /dev/null  # b기준으로 갱신된 파일 >> 없음
@@ -1 +0,0 @@
-source : 2   # b기준으로 갱신된 정보(f2.txt의 내용이 삭제)


```




## going back

### reset
==$ git reset ID --hard ==
	- reset 포인트 이후의 <mark style='background:var(--mk-color-yellow)'>커밋 취소</mark>
	- 실제로 정보는 .git에 여전히 남아있으므로 복구 가능함
	- 협업시 저장소의 버전을 인터넷에 공유할 때는 절대 reset하면 안됨
	- 즉 로컬 저장소의 개인 작업물에만 해야함
	- 옵션 : --hard, --mixed, --soft https://www.youtube.com/watch?v=4pw4RUaCo0Y&t=693s
```Bash title:"ver3으로 reset 이후 log 출력" hl:6

$ git reset 5845bcca30b361c3a2eafe7c8648465ce4ef03dd --hard # ver3_ID
$ git log
# ------------------------------------------

# ver3 이후의 버전들을 모두 삭제됨
commit 5845bcca30b361c3a2eafe7c8648465ce4ef03dd (HEAD -> master)
Author: imim555 <imim555893@gmail.com>
Date:   Fri Oct 25 21:11:14 2024 +0900

    version_3

commit 74770f371ee9c2738a5688618d57e5aeaefb7a6b
Author: imim555 <imim555893@gmail.com>
Date:   Fri Oct 25 20:53:03 2024 +0900

    version_2

commit 0d0bf423e7a436158d8efcbc1fa9687445112c8c
Author: imim555 <imim555893@gmail.com>
Date:   Fri Oct 25 20:38:36 2024 +0900

    version_1
    : Please enter the commit message for your changes. Lines starting


```



![[Pasted image 20250228012431.png]]


### revert

==$ git revert==
	- 커밋을 취소하면서 <mark style='background:var(--mk-color-yellow)'>새로운 버전</mark>을 생성




## restoring

### 손실되는 커밋 보관
reset은 현재 checkout 중인 브랜치의 최신 커밋을 바꾸는 작업
취소된 커밋 내용은 다음 2가지 디렉토리에 보관된다

`ORIG_HEAD` 위험한 작업을 할때(복구, 병합) 손실되는 커밋을 보관
`logs/refs/heads/master` 해당 브런치에서 발생하는 모든 사건을 기록


### 커밋 복구
`git reset --haed ORIG_HEAD` 해당 디렉토리에 보관된 커밋으로 되돌아감

`git reflog` 작업 기록과 그때로 이동할 수 있는 명령어 조회
![[Pasted image 20250227131852.png]]

`git checkout dfa5d04` HEAD가 브랜치에서 분리/독립 되어(detached HEAD state) 직접 커밋을 가리킴
![[Pasted image 20250227132805.png]]




## 주요 명령어

빈출 명령어
commit
push
pull
clone
checkout
add
branch
log
diff
fetch
merge
init
status
reset
tag
rebase
rm
show
bisect

명령어에 대한 도움말
$git commit --help
$git commit -am "ver3"




# git 내부원리

>[!info] gistory를 이용하여 git을 사용할 때 내부동작을 알아보자
> - 터미널로 진행
> - .git파일이 있는 위치에서 gstory 설치 ; pip install gistory
> - .git 위치에서 gstory 실행
> - 웹으로 접속 ; //localhost:8805
> - 다른 터미널로 git 명령어 실행하며 웹에서 변화 관찰

결과를 간단히 말하자면, git은 추적파일을 객체화하고 해쉬값으로 이를 참조케 한다. add를 하면 추적되는 각각의 파일내용은 객체를 구성하고 파일명은 해쉬알고리즘으로 해쉬값을 얻어 객체를 참조한다. commit을 하면 각각의 버전은 해당 상태에 해당하는 파일객체들을 그룹핑하여 다시 객체화한다.

그리고 객체의 주요 데이터타입은 다음과 같다.
[commit] : 버전에 대한 정보
[tree] : 파일명과 blob에 대한 정보
[parent] : 이전 버전에 대한 정보
[blob] : 내용에 대한 정보


## add

파일이름은 인덱스 디렉토리에, 파일내용은 객체 디렉토리에 생성되는데, 중요한 점은 파일이름이 다르더라도 내용이 같으면 동일한 object를 가리킨다.

>[!example]
> - 내용 'a'를 가진 f1.txt를 생성하여 add
> - 내용 'z'를 가진 f2.txt를 생성하여 add
> - f1.txt를 복사한 f3.txt를 생성하여 add
> - 결과) f1.txt와 f3.txt는 동일한 객체를 참조


짐작컨대, git add를 하면 그 내용을 압축 시킨다
그 결과를 해쉬알고리즘(sha1메커니즘)에 통과시켜 값(키)를 부여한다 
아래는 index 디렉토리에 f1.txt 파일이 해쉬값을 얻어 매칭된 모습
```\index\100644 6b673e8843861b8d8820fc3a3c4d32545723cdb0 0	f1.txt```

해쉬값의 이름으로 object 디렉토리에 파일 경로를 만들어 내용을 저장한다
아래는 object 디렉토리에 해쉬값으로 파일경로를 생성한 후 f1.txt의 내용을 저장한 모습
```.\objects\6b\673e8843861b8d8820fc3a3c4d32545723cdb0에 a저장```

인덱스 파일에 키와 사용자지정 파일이름을 매칭하여 저장한다 (딕셔너리 형태)


## commit

commit 결과인 버전은 object 디렉토리에 객체로서 저장된다
tree의 값은 그 버전에 해당하는 파일의 이름과 키값들을 함께 묶어서 참조한다
![[Pasted image 20250225222823.png]]


f1.txt와 f2.txt 내용을 수정하여 commit 해보자
새로운 버전이 객체로 생성되고 parent는 이전 버전의 트리를 참조한다
![[Pasted image 20250225223338.png]]


d1 폴더에 f1.txt를 복사한 f1.txt를 만들고 커밋해보자
$mkdir d1
$cp f1.txt d1/f1.txt
$git add d1/f1.txt
![[Pasted image 20250225224351.png]]

![[Pasted image 20250225224409.png]]

참조 주소 앞에 있는 단어들은 object type을 말한다
[commit] : 버전에 대한 정보
[tree] : 파일명과 blob에 대한 정보
[parent] : 이전 버전에 대한 정보
[blob] : 내용에 대한 정보


## status


- 파일의 상태는 3가지로 구분할 수 있다
	- working directory - index, staging area, cache - repository


- status명령어는 index 디렉토리와 commit 디렉토리를 비교하여 파일상태를 판별한다
	- 파일 수정만 한 경우 => index에 반영X => 파일내용과 index 내용 불일치 => "changes not staged for commit"
	- add 한 경우 => index에 반영O => 파일내용과 index 내용 일치 => 현재 commit 내용과 index 내용 불일치 => "changes to be committed" 커밋대기상태
	- commit 한 경우 => 저장소와 index와 프로젝트폴더(working copy?) 가 모두 일치 => "nothing to commit, working directory clean"


![[Pasted image 20250226000127.jpg]]

![[Pasted image 20250226000109.jpg]]





## branch


<mark style='background:var(--mk-color-yellow)'>refs 디렉토리</mark>는 브런치 파일이 저장된다
`.git/refs/heads/master`  기본 브런치
`.git/refs/heads/exp`  exp 브런치 생성

refs 디렉토리의 head 파일은 각각의 브런치를 가리킨다. 즉 refs 디렉토리의 파일이 브런치이다.
이 파일들은 가장 최신 커밋 키값을 저장한 텍스트 파일이다.

`vim .git/refs/heads/exp` 에서 커밋내용을 저장하면 깃에서 exp 브런치가 생성된 것으로 인식
`rm .git/refs/heads/exp`  하면 깃에서 exp 브런치가 없는 것으로 인식

![[Pasted image 20250227113855.png]]


<mark style='background:var(--mk-color-yellow)'> HEAD 디렉토리</mark>는 현재 운용중인 브랜치를 가리킨다
`git checkout exp` HEAD 디렉토리에서 exp 파일을 가리킨다
![[Pasted image 20250227115258.png]]

`git checkout master` HEAD 디렉토리에서 master 파일을 가리킨다
![[Pasted image 20250227115348.png]]




# branch

여러가지 버전을 만들어서 협업할 때 버전이 분기되는 모습을 가지라고 표현
branch를 생성하면 생성 당시의 상태를 그대로 복제한다

## 만들기

$git branch : 현재 위치하고 있는 브런치 공간 확인, master는 기본 브런치
$git branch exp : exp이름의 새로운 브런치 생성
$git checkout exp : exp 브런치로 이동
$ git branch -d exp : exp이름의 브런치 삭제
$ git checkout -b exp : 브런치 생성+이동, -b는 branch
$ git switch -c exp  : 브런치 생성+이동 (최근 권장) , -c는 create


![[Pasted image 20250226001216.png]]

>[!note] 예제 
> - 커밋1 : f1.txt (a)
> - 커밋2 : f1.txt (b)
> - exp 브런치 생성
> - 커밋3 : f1.txt (c)  [exp]
> - 커밋4 : f2.txt (a)  [exp]
> - 커밋5 : f3.txt (a)  [master]

## 정보 확인

`$git log --branches --decorate --graph`

![[Pasted image 20250226004234.png]]

`$git log --branches --decorate --graph --oneline`
![[Pasted image 20250226004536.png]]

`$stree`  현재 저장소를 git의 gui 프로그램인 소스트리로 보여줌

`$git log master..exp`  브런치간 버전 차이점 확인. exp브런치에만 있는 버전을 보여줌
`$git log -p master..exp`  보다 자세히 제시
`$git diff master..exp`  브런치 간 차이점 서술

## 병합

### 병합방법
- fast forward병합
	병합하려는 각 브런치들이 서로의 부모가 되는 경우 <mark style='background:var(--mk-color-yellow)'>기존 커밋을 업데이트</mark> 한다
-  merge병합
	병합하려는 각 브런치들의 부모가 다를 때 <mark style='background:var(--mk-color-yellow)'>새로운 커밋을 생성</mark>하여 병합하고 이때 커밋메시지는 자동으로 입력된다 <"merge exp">
	동일파일에서 다르게 작업된 부분을 모두 합쳐서 병합한다
>[!example] master/common.txt 내용
> function b(){}
> function a(){}

>[!example] exp/common.txt 내용
> function a(){}
> function c(){}

![[Pasted image 20250227122958.png]]

### merge

==$git merge exp== 
- master로 checkout한 상태에서 exp 내용을 가져오기
- 새로운 커밋이 생성되고 커밋 메시지 자동 생성(Merge branch 'exp')
- 즉 master의 최종 버전에서 exp버전을 덮어쓰기 때문에 중복되는 파일은 exp버전으로 덮힌다
- 새로운 커밋의 부모는 2개이다(4번커밋, 5번커밋)

![[Pasted image 20250226011800.png]]

==$git merge master== 
- master로 chckeout한 상태에서 master 내용 가져오기
- 새로운 커밋이 생성되고 커밋 메시지 자동 생성(Merge branch 'exp')
- 새로운 커밋의 부모는 2개이다(4번커밋, 5번커밋)
- 현재 exp와 master가 동일한 상태
![[Pasted image 20250226012346.png]]

==$git branch -d exp==  삭제
![[Pasted image 20250226012656.png]]


참고문헌 https://git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging





### rebase

merge와 rebase의 결과물은 같지만 처리방식이 다름
merge는 각 브랜치의 역사를<mark style='background:var(--mk-color-yellow)'> 병렬적으로 병합</mark>. 쉽고 안전.
rebase는 각 브랜치의 역사를 <mark style='background:var(--mk-color-yellow)'>직렬적으로 병합</mark>하기 때문에 히스토리 파악 용이. 어렵고 위험
![[Pasted image 20250228205458.png]]

`git checkout rb`
`git rebase master`
- rb의 부모(base)가 0bbb2a5 에서 master 브랜치의 기원으로 변경(rebase)
- rb 브랜치의 히스토리는 master 브랜치를 근거로 재구성됨
![[Pasted image 20250228202628.png]]
![[Pasted image 20250228203010.png]]


`git checkout master`
`git merge rb`
- 이 상태에서 rb 브랜치를 병합하면 fast forward 병합 진행
![[Pasted image 20250228203451.png]]

커밋 간 소스코드의 차이 정보를 fetch라고 하는데, M1과 R1의 fetch와 R1과 R2의 fetch가 임시보관소에 저장되고 rebase를 하면 R1의 fetch가 M1에 merge된다
병합시 줄을 근거로 충돌여부를 판단한다. 만약 동일한 줄에서 병합대상의 fetch정보가 다르다면 충돌이 발생한다. (2번째 라인에 master 브랜치는 m2, rb 브랜치는 r2 상태) 그러면 사용자가 직접 내용을 수정하고 rebase를 계속 진행한다(--continue)
`git rebase --continue`
![[Pasted image 20250228210956.png]]





### 충돌

- 3way merge와 2way merge
	- 넘버라인 기준으로 병합을 시도하는데, 동일한 넘버라인에서 브랜치 간 내용이 다르면 충돌이 발생한다
	- 충돌발생시 index디렉토리에는 3가지 상태가 나타남 (평소에는 0으로 표시)
		=> 공통부분(base) - 현재HEAD가 있는 브런치(ME) - 병합대상 브런치(Other)
	- 2way merge : base 부분을 제외하고 두 브런치의 내용만 비교
	- 3way merge : base 부분을 근거로 갱신된 브런치의 내용을 우선 반영하며 더 효과적

![[Pasted image 20250228014744.png]]


- 예제
>[!example] master/common.txt 내용
> function b(){}
> function a(master){}
> function c(){}

>[!example] exp/common.txt 내용
> function b(){}
> function a(exp){}
> function c(){}

![[Pasted image 20250227123523.png]]


병합 실패 후 문제 파일을 확인하면, 구분자 '=' 기준으로 각 브런치별 충돌내용이 표현됨
이는 충돌을 사용자에게 위임한 것으로 사용자가 병합할 내용을 수정하고 다시 커밋한다
![[Pasted image 20250227123904.png]]


- 충돌 관련 tool
`git config --global merge.tool kdiff3` 병합을 전문적으로 하는 툴 설치
`git mergetool` gui가 실행되며 충돌부분 쉽게 수정 가능





# stash

메뉴얼 file:///C:/Program%20Files/Git/mingw64/share/doc/git-doc/git-stash.html

어떤 브랜치에서 작업에 대해 add나 commit을 안하면 다른 브랜치에도 영향을 미치게 된다. 이때 작업을을 임시로 숨기고 다른 브랜치를 독립적으로 운용하는 기능. 즉 현재 작업중인 변경사항을 임시로 저장(stash)하고, 워킹 디렉토리를 깨끗하게 만드는 명령어

add를 하여 추적되고 있는 상태의 파일만 stash 가능


`$git reset --hard HEAD` 가장 최신 커밋으로 되돌리기, reset을 하더라고 stash 정보는 남아있기 때문에 복원가능

`$git stash` 숨기기
`$git stash apply` 꺼내기, 다만 여전히 stash 목록에 존재하여 따로 drop 필요
`$git stash drop` 삭제하기
`$git stash pop` apply+drop
`$git stash list` stash 처리한 작업 목록, 최신이 0번 지정

![[Pasted image 20250226171237.png]]




# 원격저장소

장소에 상관없이 프로젝트를 계속 진행하기 위해 원격저장소 개념 필요.
현재 내가 접속하고 있는 내부PC를 로컬저장소, 외부PC를 원격저장소
## 생성
init

- `git init <local_name>` 
	- local 이름의 저장소 생성. 
	- 내부에 .git 파일 존재
- `git init --bare <remote_name>` 
	- remote 이름의 원격저장소 생성. 
	- 작업기능(working directory)이 없고 저장기능(.git 내부)만 존재
![[Pasted image 20250228095236.png]]

## 관리 
remote

- `git remote add <name:origin> <url:/h/내\ 드라이브/GIT/remote>
	- 원격저장소 추가
	- `origin`은 원격 저장소의 기본적인 별칭으로 사용
	- 이후 `git push origin 브랜치명` 같은 명령어에서 `origin`을 사용 가능
	- 공백 포함된 폴더는 따옴표 대신 `\`(역슬래시)로 이스케이프하는 게 더 안정적
	- 이 경로는 **로컬 폴더를 원격 저장소처럼 사용하려는 설정**. 만약 GitHub 같은 클라우드 저장소를 원격으로 추가하는 거라면 URL 형식(`git@github.com:사용자명/저장소명.git`)을 써야 해.

- `git remote -v` 
	- 원격저장소 목록 확인
	- `(fetch)` → 데이터를 가져올(fetch) 때 사용하는 URL
	- `(push)` → 데이터를 푸시(push)할 때 사용하는 URL
	- ![[Pasted image 20250228105945.png]]
- `git remote remove <name>` → 원격 저장소 제거
-  `git remote set-url <name> <새로운URL>` → 원격 저장소 주소 변경
- `git remote show <name>` → 원격 저장소의 상세 정보 확인

## 동기화

### 업로드 push

- `git push -set-upstream origin master` 
	- 축약형(-u). 
	- 로컬 브랜치(master)와 원격 브랜치(origin/master)를 추적(upstram)하는 브랜치 설정. 
	- 이후 `git push` `git pull`만 입력해도 자동으로 해당 브랜치로 연결됨

- `$ git branch -vv`
	- 추적중인 브랜치 목록 조회
	- 결과 : main 1731360 [origin/master] 1 ; main 브랜치가 [origin/master]를 추적중



- `git push <원격저장소> <브랜치>`
	- <원격저장소>: 원격 저장소 이름 (`origin`이 기본값)
	- <브랜치>: 원격 저장소에 반영할 로컬 브랜치 (`main`, `master`, `develop` 등)

- `git push origin main:main`
	- local main -> remote main 전송

- `git push --all <원격저장소>`
	- 로컬 저장소에 있는 모든 브랜치를 업로드
- `git push <원격저장소> --tags`
	- Git 태그를 원격저장소로 업로드
	- 보통 소프트웨어 릴리스 버전(tag) 공유시 사용
- `git push --force <원격저장소> <브랜치>`
	- 축약형 -f
	- 강제푸시는 로컬과 원격 저장소의 히스토리가 다를 때 강제로 덮어씀
- `git push <원격저장소> --delete <브랜치>`
	- 원격저장소에서 해당 브랜치 삭제
- `git config --global push.defalut simple` 
	- 설정변경 ; 전역적으로 push 형식을 simple방식으로 변경


### 가져오기 pull & fetch

- pull
	- 원격저장소 다운 + 로컬 브랜치에 병합
	- 원격 브랜치와 로컬 브랜치가 동일한 커밋 참조
	- 만약 원격 브랜치와 로컬 브랜치가 충돌하면 수동으로 해결해야
	`git pull origin main`

- fetch  
	- 원격 저장소 다운만 진행
	- 원격 브랜치는 최신 커밋을 참조하지만, 로컬 브랜치는 기존 작업 상태 보존하여 서로 다른 커밋 참조
	- 원격 저장소와 지역 저장소의 차이를 확인 가능
	- git diff HEAD origin/master
	- 병합과정(merge, rebase)을 거쳐야 원격저장소 브랜치의 최신 커밋이 반영됨
	- git merge origin/master  
```
# 원격 저장소 변경 사항 가져오기 
(fetch) git fetch origin 

# 원격 저장소의 변경 사항을 확인 
git log origin/main --oneline 

# 로컬 브랜치와 합치려면 merge나 rebase 실행 
git merge origin/main # 또는 git rebase origin/main
```

## 프로젝트 복제

- clone
	- **원격 저장소의 전체 복제**
	- 새로운 로컬 저장소를 만들고 원격 저장소의 모든 파일, 커밋 이력, 브랜치를 복사함.
	- 보통 github에서 처음 프로젝트를 가져올 때 사용.


-  https 이용하여 깃허브에서 프로젝트 가져오기
	- `git clone <repository_url> gitsrc` 
	- https 이용 방법
	- 깃허브에서 프로젝트 가져오기

- ssh 이용하여 깃허브에서 프로젝트 가져오기
	- `ssh-keygen` 
	- ssh 키생성
	- 제시된 경로에 private/public 두가지 키값이 생성된다.  private 키값은 로컬PC에 저장하고 public 키값은 특정서버에 저장함으로서 해당 private을 가진 PC가 그의 쌍인 public을 가진 다른 PC에 접속할 수 있게한다. 즉 두가지는 하나의 쌍으로 private는 비번, public은 ID 역할을 하며, 서버에 접속할 때마다 비번을 입력할 필요를 줄여준다. 
	- 이 절차에 따라 서버컴퓨터인 깃허브(ssh관련부분)에 로컬저장소 접속암호(public)를 저장한다
	- `git clone <ssh_주소>`

- 정보조회
	`git log --reverse`  커밋 기록 거꾸로 조회
	`git checkout <commitID>` 해당 커밋으로 체크아웃
	`git branch` `git log` `ls -al` 상태 확인

![[Pasted image 20250228132659.png]]

- 브랜치 추적
	`git branch -M main`
	- -M : 현재 브랜치 이름 강제 변경
	- 현재 GitHub은 main을 기본 브랜치로 사용하고 있어 충돌방지 위해 로컬 브랜치 이름을 동일하게 변경
	`$ git remote add origin https://github.com/imim555/gitex.git`
	- 원격저장소 목록 업로드




>[!note] 통신방법
> 통신방법에는 일반적으로 HTTPS와 SSH(secure shell) 가 있다.
>- HTTPS 
>	- 가장 간단한 방식으로 URL을 사용해 저장소에 접근
>	- Git 명령어 실행 시 매번 사용자 인증(아이디/비밀번호 또는 개인 액세스 토큰)이 필요
>	- 방화벽이 있는 환경에서도 비교적 자유롭게 사용 가능하지만, 보안 면에서는 SSH보다 상대적으로 취약
>-  **SSH (Secure Shell)**
>	- 공개 키와 개인 키를 이용하여 안전하게 인증하는 방식
>	- 한 번 설정하면 추가적인 인증 없이 편리하게 사용
>	- 보안성이 높고, 자동화 작업(CI/CD)에도 적합
>	- 하지만 방화벽이 제한적인 네트워크에서는 제한적


![[Pasted image 20250228172108.png]]


-  my server에서 프로젝트 가져오기
	- 원격저장소 생성   https://www.youtube.com/watch?v=sAeXpcGCQfI&t=7s
	`git remote add origin ssh://imim555@13.124.42.13/home/git/git/remote/`
	- "ssh: //사용자이름@원격저장소PC의ip주소/원격저장소경로"
	- 자동로그인   https://www.youtube.com/watch?v=iJNQWP31gco
	`git remote add origin git@github.com:imim555/gitex.git`
	`git clone origin GIT_ssh`


- 원리
	- config 디렉토리에 origin, 브랜치 추적상태(upstream) 정보 포함
	- refs/remotes/origin/master 원격저장소의 현재 브랜치의 최신 push 내용 보관
	- refs/heads/master 지역저장소의 현재 브랜치의 최신 commit 내용 보관
![[Pasted image 20250228182635.png]]

![[Pasted image 20250228182551.png]]

# tag

브랜치는 참조하는 커밋이 계속 변동되지만
태그는 특정 커밋만을 계속 참조하기 때문에 이름표 같은 역할 수행

`git tag 1.0.0 (커밋ID) master` 태그 생성. light weighted tag

`git checkout (tag이름) ` 해당 커밋으로 복구

![[Pasted image 20250228185422.png]]

`git tag -a 1.1.0 -m "bug fix"` 
	- 태그+설명. annotated tag
	- refs/tags/tag_name  해당 경로는 tag_name의 텍스트 파일이 저장됨
	- tag 디렉토리에 키값이 생기고 사용자정보와 태그 정보가 저장됨. 이 인스턴스는 해당 커밋을 참조
`git tag` 태그 목록 확인
`git tag -v 1.1.0` 해당 태그에 대한 상세 정보 확인

![[Pasted image 20250228190005.png]]

`git push --tags` 원격저장소에 태그 전송. releases에서 확인 가능

`git tag -d 1.1.0` 태그 삭제




# workflow

|  branch  |         역할          | 특징                                                                                                                   |
| :------: | :-----------------: | :------------------------------------------------------------------------------------------------------------------- |
|  master  | 사용자에게 최종적으로 노출되는 버전 | 가장 엄격하게 통제됨                                                                                                          |
| develop  |      일상적 개발 관련      |                                                                                                                      |
| feature  |  새로운 업무(특정 기능 추가)   | - 기능 개발이 완료되면 develop으로 병합. <br>- 추후 제거할 수도 있기 때문에 해당 브랜치를 독립적으로 계속 운용. - 추후 병합시 충돌을 방지하기 위해 수시로 develop 상태를 동기화 시켜줌 |
| release  |    기능을 사용자에게 배포     | - 준비가 완료되면 master(tag작성) 및 develop 브랜치로 병합. <br>- 병합될때 master와 develop 브랜치는 동일한 소스코드 참조                              |
| hotfixes |        버그 수정        | 완료후 master 및 develop으로 병합                                                                                            |


https://nvie.com/posts/a-successful-git-branching-model/
![[Pasted image 20250228211109.png]]