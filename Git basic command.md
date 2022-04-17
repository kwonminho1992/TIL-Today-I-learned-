## Git, Github의 기본 명령어 모음
<br/> 

#### Commit하기
$ cd [directory] : git에 내용을 저장할 폴더로 위치 설정<br/> 
$ git init : .git 폴더 생성<br/> 
$ git clone [URL] : github 등에 있는 다른 프로젝트를 복사해서 내 컴퓨터로 가져오기 (* URL : 가져오려는 프로젝트가 있는 github URL)<br/> 
$ git status : 현재 상태 (어떤 파일들이 수정되었고, 추가되었는지 등)을 알려줌<br/> 
$ git diff : 변경된 '내용'이 무엇인지 궁금할때 사용 (각각 unstaged, staged 상태일 때의 내용을 비교) <br/> 
$ git add [file name] : 해당 파일을 git에 추가하여 commit 가능한 상태로 만듦 (commit의 前 단계)<br/> 
$ git rm [file name] : 해당 파일을 git으로부터 삭제함 (삭제 후 commit하면 git에선 더 이상 해당 파일을 추적하지 않음)<br/> 
$ git mv [orignal file name] [revised file name] : 해당 파일의 이름을 수정함<br/> 
$ git commit -m "[memo]" : add되었던 파일들을 commit함. <memo>부분에는 이번 commit의 특이사항, 변동사항 등을 간단히 메모해서 나중에 알아보기 쉽게 함<br/> 
$ git commit -a -m "[memo]" : tracked 상태의 파일들을 add할 필요 없이 바로 commit함 (* untracked 상태의 파일들은 add부터 해야함)<br/> 
$ git commit --amend : commit할때 무언가 빠트린게 있다면, 수정한 부분을 다시 add한 후, 해당 명령어를 사용하면 새로 commit되지 않고 기존 commit을 덮어쓴다<br/> 
$ git log : 해당 프로젝트의 지금까지의 commit history를 보여줌<br/> 
$ git checkout [commit hash value] : 현재 작업 중인 프로젝트를 해당 commit 상태로 되돌아감
  

##### Branch
$ git branch [branch name] : 새로운 branch를 생성<br/> 
$ git checkout [branch name] : 해당 branch로 이동 (현재 작업 상태에 있는 branch를 변경하는 명령어)<br/> 
$ git checkout -b [branch name] : 새로운 branch를 생성하면서 동시에 생성한 branch로 이동 <br/> 
  
##### Remote 저장소
$ git remote add [name] [url] : remote 저장소(보통 git hub)의 url에게 이름(name)을 부여하고 추가<br/> 
$ git reomte show [name] : 이름(name)에 해당하는 remote 저장소의 구체적인 정보를 불러옴<br/> 
$ git push [name] [branch name] : 내용을 remote 저장소의 branch에 업로드함<br/> 
$ git remote rename [old name] [new name] : remote 저장소의 이름 바꾸기<br/> 
$ git remote remove [name] : remote 저장소 삭제<br/> 
<br/>  
  
##### Tag
$ git tag : 현재 旣 존재하는 tag를 조회 <br/> 
$ git tag -v [tag name] : 해당 tag를 만든 사람, tag에 달린 comment등의 정보를 알려줌<br/> 
$ git tag [tag name]  (git tag [branch name] or [commit code] [tag name]) : tag를 추가함 (commit code를 지정해주지 않으면 가장 최신의 commit에 tag가 추가됨) 주로 버전 이름을 지정할 때 사용 (Lightweight tag) (ex. 1.1.0)<br/> 
$ git tag -a [tag name] -m "[comment]" (+[branch name] or [commit code]) : tag를 추가함. tag와 함께 태그를 설명해주는 코멘트도 같이 달 수 있음 (Annotated tag) (ex. 1.1.0 "bug fixed version")<br/> 
$ git tag -d [tag name] : 해당 tag를 삭제<br/> 
$ git push [remote repository name] [tag name] : 해당 tag를 github로 push함 (commit들을 push할때 태그는 push되지 않으므로 따로 해줘야함) (모든 tag를 push 하고 싶으면 '--tags'를 입력)<br/> 
* commit code : $ git log 時, 첫줄에 나타나는 문자열을 말함 (commit "09f4acd3f8836b7f6fc44ad9e012f82faf861803"의 형태, ""안에 감싸진 부분)<br/> 
* 버전 관리 : ver a.b.c의 형태로 관리<br/>
  a (Major) - 이전 버전과 호환이 안되는 정도로 변경했을 때 바꿈<br/>
  b (Minor) - 기능의 추가, 변경 시 바꿈<br/>
  c (Patch) - 버그, 에러 등을 수정했을 때 바꿈<br/>
  
  
##### *용어<br/> 
- tracked : git에서 버전관리를 한 적이 있어서 git에서 변동사항을 추적할 수 있는 파일, git add를 하면 modified된다.<br/> 
- untracked : git에서 버전관리를 한 적이 없어서 git에서 변동사항을 추적할 수 없는 파일, git add를 하면 new file로 git에 추가된다<br/> 
- staged : add 되어서 commit 할 수 있는 상태의 파일 (add는 됬지만, 아직 commit은 안됨)<br/> 
- unstaged : add 되지 않았기 때문에 commit 할 수 없는 상태의 파일<br/> 

- snapshot : 특정 시점의 파일 상태 (특정 시점의 code)
- delta : 해당 파일의 이전 상태와 비교하여 변경된 사항
