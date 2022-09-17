## Git, Github의 기본 명령어 모음
<br/> 

#### Commit하기
$ cd [directory] : git에 내용을 저장할 폴더로 위치 설정<br/> 
$ git init : .git 폴더 생성<br/> 
$ git clone [URL] : github 등에 있는 다른 프로젝트를 복사해서 내 컴퓨터로 가져오기 (* URL : 가져오려는 프로젝트가 있는 github URL)<br/> 
$ git status : 현재 상태 (어떤 파일들이 수정되었고, 추가되었는지 등)을 알려줌<br/> 
$ git add [file name] : 해당 파일을 git에 추가하여 commit 가능한 상태로 만듦 (commit의 前 단계)<br/> 
$ git rm [file name] : 해당 파일을 git으로부터 삭제함 (삭제 후 commit하면 git에선 더 이상 해당 파일을 추적하지 않음)<br/> 
$ git mv [orignal file name] [revised file name] : 해당 파일의 이름을 수정함<br/> 
$ git commit -m "[memo]" : add되었던 파일들을 commit함. <memo>부분에는 이번 commit의 특이사항, 변동사항 등을 간단히 메모해서 나중에 알아보기 쉽게 함<br/> 
$ git commit -a -m "[memo]" : tracked 상태의 파일들을 add할 필요 없이 바로 commit함 (* untracked 상태의 파일들은 add부터 해야함)<br/> 
$ git commit --amend : commit할때 무언가 빠트린게 있다면, 수정한 부분을 다시 add한 후, 해당 명령어를 사용하면 새로 commit되지 않고 기존 commit을 덮어쓴다<br/> 
$ git log : 해당 프로젝트의 지금까지의 commit history를 보여줌<br/> 
$ git log --all --graph --oneline : 모든 commit history를 commit당 한줄씩 그림으로 보여줌<br/>
$ git checkout [commit hash value] : 현재 작업 중인 프로젝트를 해당 commit 상태로 되돌아감<br/> 
$ git reset --hard [commit hash value] : HEAD(現 commit)를 해당 commit으로 바꾸고, 그 앞에 있는 commit들은 취소<br/>

#### Stash (임시저장)
$ git stash : 현재 내용을 stack에 임시로 저장함 (현재 작업 중인 내용을 커밋하긴 힘든 상태에서 다른 작업으로 넘어갈 때 사용)<br/> 
$ git stash list : stack에 임시 저장된 목록을 불러오기<br/> 
$ git stash apply [stash name] : 해당 stash로 돌아감, stage 상태는 복원되지 않음<br/>
$ git stash apply --index : 해당 stash의 stage 상태까지 돌아감 <br/>
$ git stash drop [stash name] : 해당 stash를 삭제 <br/>
$ git stash pop [stash name] : 해당 stash으로 돌아가고, stash는 삭제 <br/>
  
#### Branch
$ git branch [branch name] : 새로운 branch를 생성<br/> 
$ git checkout [branch name] : 해당 branch로 이동 (현재 작업 상태에 있는 branch를 변경하는 명령어)<br/> 
$ git checkout -b [branch name] : 새로운 branch를 생성하면서 동시에 생성한 branch로 이동 <br/><br/>
$ git branch -d [branchname] : 해당 branch를 삭제 <br/>
  
$ git merge [branch name] : (2 way merge) 현재의 branch와 [branch name]을 병합 ([branch name]의 내용을 현재의 branch에 합침), 충돌되는 내용이 있으면 직접 수정해야함 <br/>
$ git cherry-pick [commit's hashcode] : 다른 브랜치의 특정 커밋만 가져와서 현재 브랜치에 붙임 
$ git diff : 변경된 '내용'이 무엇인지 궁금할때 사용 <br/>
$ git mergetool : (3 way merge) p4merge 같은 tool을 이용하여 충돌부분을 수정할 수 있음. (두 브랜치의 내용 중 하나를 취사선택 가능) <br/>
* p4merge 다운로드 주소 : https://www.perforce.com/downloads/visual-merge-tool </br>
* p4merge 사용법 (Mac) : https://gist.github.com/tony4d/3454372</br>
* p4merge 사용법 (Window) : https://teddylee777.github.io/git/study-git-2<br><br/> 

#### Git flow
![image](https://user-images.githubusercontent.com/69135840/185782138-83ee2810-9307-40aa-a974-1b9a3691ad4f.png)
개념, 사용법 : https://danielkummer.github.io/git-flow-cheatsheet/index.ko_KR.html

#### Remote 저장소
$ git remote add [name] [url] : remote 저장소(보통 git hub)의 url에게 이름(name)을 부여하고 추가<br/> 
$ git reomte show [name] : 이름(name)에 해당하는 remote 저장소의 구체적인 정보를 불러옴<br/> 
$ git push [name] [branch name] : 내용을 remote 저장소의 branch에 업로드함<br/> 
$ git remote rename [old name] [new name] : remote 저장소의 이름 바꾸기<br/> 
$ git remote remove [name] : remote 저장소 삭제<br/> 
  <br/> 
$ pull request (in github) : master(main)가 아닌 branch에 commit한 내용을 master에 반영해줄 것을 요청<br/> 
$ Merge pull request (in github) : 다른 branch에서 pull request한 내용을 master(main)에 합치도록 허락<br/> 
$ git pull [name] [branch name] : 다른 branch의 내용을 현재 branch에 가져오기 (동기화)<br/> 
<br/>  
  
#### Tag
$ git tag : 현재 旣 존재하는 tag를 조회 <br/> 
$ git tag -v [tag name] : 해당 tag를 만든 사람, tag에 달린 comment등의 정보를 알려줌<br/> 
$ git tag [tag name]  (git tag [branch name] or [commit code] [tag name]) : tag를 추가함 (commit code를 지정해주지 않으면 가장 최신의 commit에 tag가 추가됨) 주로 버전 이름을 지정할 때 사용 (Lightweight tag) (ex. 1.1.0)<br/> 
$ git tag -a [tag name] -m "[comment]" (+[branch name] or [commit code]) : tag를 추가함. tag와 함께 태그를 설명해주는 코멘트도 같이 달 수 있음 (Annotated tag) (ex. 1.1.0 "bug fixed version")<br/> 
$ git tag -d [tag name] : 해당 git에서 tag를 삭제<br/>   
$ git push [remote repository name] [tag name] : 해당 tag를 github로 push함 (commit들을 push할때 태그는 push되지 않으므로 따로 해줘야함) (모든 tag를 push 하고 싶으면 '--tags'를 입력)<br/> 
$ git push origin :[tag name] : github에 이미 업로드된 tag를 삭제<br/> 
* commit code : $ git log 時, 첫줄에 나타나는 문자열을 말함 (commit "09f4acd3f8836b7f6fc44ad9e012f82faf861803"의 형태, ""안에 감싸진 부분)<br/> 
* 버전 관리 : ver a.b.c의 형태로 관리<br/>
  a (Major) - 이전 버전과 호환이 안되는 정도로 변경했을 때 바꿈<br/>
  b (Minor) - 기능의 추가, 변경 시 바꿈<br/>
  c (Patch) - 버그, 에러 등을 수정했을 때 바꿈<br/>
  
  
#### *용어<br/> 
- tracked : git에서 버전관리를 한 적이 있어서 git에서 변동사항을 추적할 수 있는 파일, git add를 하면 modified된다.<br/> 
- untracked : git에서 버전관리를 한 적이 없어서 git에서 변동사항을 추적할 수 없는 파일, git add를 하면 new file로 git에 추가된다<br/> 
- staged : add 되어서 commit 할 수 있는 상태의 파일 (add는 됬지만, 아직 commit은 안됨)<br/> 
- unstaged : add 되지 않았기 때문에 commit 할 수 없는 상태의 파일<br/> <br/> 
- snapshot : 특정 시점의 파일 상태 (특정 시점의 code)<br/> 
- delta : 해당 파일의 이전 상태와 비교하여 변경된 사항<br/> <br/> 
- base : branch가 분기 되는 시점의 commit을 의미<br/> 
- cherry pick :<br/> 
base -- a1 -- a2 -- merged commit (a2 + b1, not a2 + b2)<br/> 
.... \- b1 -- b2 -/<br/> 
     
