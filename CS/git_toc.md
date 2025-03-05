

- git Bash
	- basic : pwd, cd, mkdir, ls, rm, find
	- vim editor : vim, cat, cp
	- 명령어 -옵션1옵션2옵션3
- version control
	- 주요상태 : untracked-unstaged-staged-commited / status
	- initializing : init
	- recording : add, commit, -am, --amend
	- history : diff, log, -p, ID1..ID2
	- going back : reset, revert, --hard, --mixed, --soft
	- restoring
		- 손실되는 커밋 보관 : ORIG_HEAD, logs/refs
		- 커밋 복구 : reset --head ORIG_HEAD, checkout ID
	- 주요 명령어
- 내부 원리
	- add : index/key f1.txt -> object/key경로/'a'
	- commit : object/[commit]id ->[tree]id
	- status : working directory - index, staging area, cache - repository
	- branch 
		- refs/heads/txt파일
		- HEAD/현재branch
- branch
	- 만들기 : branch, -b, -d, checkout -b, switch -c
	- 정보 확인: log --branchse --decorate --graph --oneline
	- 병합
		- 방법 : fastforward 병합, merge 병합, 충돌
		- merge : 병렬적 처리
		- rebase : 직렬적 처리
		- 충돌 : 3way merge
- stash : stash [list/apply/drop/pop]
- 원격저장소
	- 생성 : init -bare <'name'>
	- 관리 : remote [add/-v/remove/set-url/show]
	- 동기화 : push, pull & fetch
	- 복제 : clone, https, ssh
- tag
- workflow : master-develop-feature-release-hotfixes

status
![[Pasted image 20250226000127.jpg]]

![[Pasted image 20250226000109.jpg]]



3way merge
![[Pasted image 20250228014744.png]]

workflow
![[Pasted image 20250228211109.png]]