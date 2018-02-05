# git-coursework-for-team
https://github.com/newpouy/git-study-2

이 문서는 가상의 시나리오를 가정하고 작성한 실습용 문서입니다. Git이 무엇인지, 어떻게 사용될 수 있는지 개념적인 이해를 먼저 하시면 더 빠르게 실습하실 수 있습니다. 다음 자료를 먼저 보실것을 추천합니다.
http://rogerdudler.github.io/git-guide/index.ko.html  간단(함에도 자세)한 입문
http://git-scm.com/book/ko/v2 공식 레퍼런스 한국판
http://learnbranch.urigit.com/  브랜치 생성/관리 시각화
https://opentutorials.org/course/1492 생활코딩 짱짱맨

--------------------
git을 팀개발로 시뮬레이션하며 공부해보자. 모든 팀원에게 git과 git-bash(윈도우즈 터미널 작업용)는 설치되어 있어야한다.

일단 당신이 새로운 프로젝트를 시작하는 리더라고 가정하자.

먼저 당신은 팀원들과 공유할 원격저장소와 당신 자신의 로컬저장소를 만들어야 한다.
일단, 원격저장소로 깃허브를 쓰자. gitlab, bitbucket과 같은 다른 저장소를 사용할 수도 있지만 일단은.

github에 로그인해서 new repository를 만들자. 웹인터페이스가 친절하므로 쉽다. repository를 만들고 난 후 나오는 웹페이지에서 두번째 박스에 주목하자. 이 박스 안의 내용을 따라하면 방금 만든 원격저장소(remote repository)를 바라보는 로컬저장소를 만들 수 있다.

먼저 빈 폴더(디렉토리)를 만든다. 폴더이름은 방금 생성한 원격저장소의 이름과 같지 않아도 상관없지만, 일단 같게 하자. 깃을 사용하는 프로젝트는 git폴더 아래에서 하는게 좋다?는 이야기가 있다.

mkdir git-study-1 //폴더를 만들고

cd git-study-1  //이동한다.

echo “this is a project to study git” >> README.md // 이 프로젝트를 소개하는 README.md파일을 작성한다.

git init // 현재 폴더가 깃 로컬저장소로 초기화된다. ls -al로 .git폴더가 숨김폴더로 생성되어 있음을 확인하자. 폴더를 더 자세히 탐색해보면 온갖 폴더와 파일들을 볼 수 있다.  HEAD파일과 refs폴더는 나중에 자세히 살펴보자. 이 .git폴더를 삭제하면 git init을 취소한 것과 같다.

vi .gitignore // git의 tracking대상에서 제외할 파일들을 설정한다. 주로 db정보와 컴파일된 target, bin폴더 등을 작성해 놓는다. 만약 미리 작성하지 않으면 나중에 약간의 작업이 필요하다. git rm -r --cached . <-모든 인덱스를 캐시에서 지우기 위해 쩜찍기

git status // 현재 git상태를 알려주는 명령어이다.
 on branch master현재 git초기화의 기본 브랜치인 master브랜치에 위치해 있음을 알려준다.
 앞에서 작성한 README.md가 tracked되지 않았다고 나온다. 이는 git working tree(git init된 폴더 내의 모든 파일과 폴더)의 변화를 git이 모니터링하고 있음을 알려준다. 즉, 현재 로컬저장소에 반영될 수 있는 어떤 변화가 워킹트리 내에 있다는 것을 알려준다.

git add README.md // README.md를 commit하기 위해 staging한다(무대 위로 올린다?) git status로 다시 확인해보면 commit될 수 있다고 나온다.

git commit -m “first commit” // 이 커밋이 어떤 커밋인지 메시지를 작성해서 커밋한다. master브랜치로써 커밋했고 일곱자리의 알파벳과 숫자로 된 커밋고유번호가 부여됐음을 알려준다. 첫 커밋을 하고 나면 .git폴더의 하위폴더인 refs/heads폴더에 디폴트 브랜치인 master파일이 생성되고 이 파일에는 상세한 커밋고유번호(커밋버전) 기록된다. logs폴더에도 상세한 로그기록이 남는다.  

git remote // 현재 브랜치가 바라보고 있는 원격저장소를 알려준다. 현재는 아무 원격저장소도 등록되어 있지 않으므로 아무것도 출력되지 않는다.

git remote add organ https://github.com/newpouy/git-study-1.git // 원격저장소를 등록하는 부분이다. 원격저장소의 이름을 ‘organ’으로 해 보았다. 가이드에는 'origin’으로 나온다. git remote를 다시 실행해보면 organ이라는 이름의 원격저장소가 출력된다.

git remote show organ // organ이라는 이름의 원격저장소에 대한 자세한 정보가 출력된다.

git push -u organ master // organ이라는 이름의 원격저장소에 로컬저장소의 master브랜치가 바라보고 있는 커밋버전을 푸시한다. 이 명령을 실행하면 원격저장소인 github에 master브랜치가 생성되고 로컬 master브랜치의 소스가 푸시된다. -u는 --set-upstream옵션의 줄임이다.
이미 한번 push를 했다면 git push만 하여도 .git폴더의 config파일에 현재 로컬 브랜치에 어떤 원격브랜치가 물려 있는지 확인할 수 있다.

이제 기본적인 프로젝트 와꾸를  이클립스 등을 이용해 만들고 한 번 add부터 commit에 이어 push까지 해보자. (특정프로젝트->우클릭->팀->share project로 이클립스도 깃동기화한다) ….했다고 치자. 그럼 새로운 커밋 번호가 생기고 github에서 변경된 소스와 폴더들을 확인 할 수 있다.

2.  이클립스 윈도우  show view->git->git repositories 로 깃 저장소를 이클립스에 등록->우클릭 import project로 이클립스 프로젝트화


자, 이제부터 팀플레이를 준비하자. 지금 브랜치는 master 하나뿐이다. 이 브랜치는 최종 배포판이 될 것이라고 치고, 개발팀원들과 함께 사용할 개발브랜치를 ‘develop’이라는 이름으로 만들자.

git checkout -b develop  // -b옵션으로 develop브랜치를 만듦과 동시에 checkout까지 한다. checkout은 브랜치를 변경하는 명령어 이다. 이제 뭔가 소스를 변경하고 add, commit하면 develop브랜치에서 작업하는 게 된다. 이는 로컬 브랜치를 추가한 작업일 뿐 원격브랜치와는 아직 상관이 없다. // 만약 develop 뒤에 특정  브랜치명을 입력하면 해당 브랜치를 원형으로 develop브랜치를 만든다. 없으면 디폴트는 master브랜치이다.

git branch -a // master, develop 두개의 로컬 브랜치와 한개의 원격브랜치를 확인할 수 있다.

git push origin develop // 여기서 origan은 처음 만든 remote저장소 이름이다.(깃의 가이드에는 ‘origin’이다. )  만약 특정기능을 개발하는 feature원격브랜치라면 해당 로컬 feature브랜치를 만들어 작업하고 develop대신 feature원격브랜치로 push하면 자동으로 원격저장소에 featue브랜치가 만들어지게 된다.

git branch -r // remotes/origan/develop organ저장소의 원격develop브랜치가 만들어진다. 깃허브에 가서도 확인해보면 새로운 브랜치가 만들어져 있다.

git remote show organ // organ 원격 저장소의 상세정보를 볼 수 있다. push, pull할 때 어떤 브랜치들을 대상으로 하는지도 알 수 있다.

마지막으로 팀원들이 깃허브(github.com)원격 저장소에 push할 수 있도록 collaborator로 등록해주어야 한다. 웹인터페이스 상의 Settings메뉴에 Collaborators 메뉴가 있다.
팀원들이 깃허브에 가입하면 계정이 검색된다.

일단 여기까지다. 깃을 형상관리도구로 사용하는 프로젝트 리더로서 당신의 최초의 작업을 했다.   
이제  프로젝트 팀원의 작업을 해보자.

##########################################################################


자, 당신은 이제 프로젝트의 팀원이다.  git과 git-bash는 설치되어 있어야 하고, 우리의 프로젝트명은 git-study-1이다.
git clone https://github.com/newpouy/git-study-1.git git-study-1 // 이 작업은 당신의 현재 디렉토리 아래에 git-study-1이라는 이름의 디렉토리를 만들고 해당 디렉토리를 git으로 initialized하고 git-study-1원격저장소로부터 소스를 받아온다. 이때 받아지는 소스들은 master브랜치의 최신 소스들이다.  그리고 당신은 현재 master브랜치로 작업중이다.


git status // on master확인
git branch -a // 한 개의 master로컬브랜치 세 개의 원격브랜치가 있다.

당신은 개발팀이므로 develop브랜치로 작업을 해야 한다.

git checkout -b develop origin/develop // origan원격저장소의 develop브랜치를 원형으로 로컬 develop브랜치를 만들고 갈아탄다.

git branch -a // 로컬develop브랜치 포함 5개의 브랜치가 보인다.

git remote show organ // git push와 pull을 할 때 로컬브랜치가 어떤 원격브랜치와 작업하는지 확인한다.

git diff organ/develop // refs/organ/develop // 원격브랜치와 로컬브랜치의 차이가 있는지 확인. 이미 pull되어 있어서 git pull하면 Already up to date라고 나온다.

여기까지 팀원2도 똑같이 작업했다고 가정한다.

이제 팀원1이 워킹트리에서 기존파일에 수정하는 작업을 하고 저장한다.

git status // git이 지켜보고 있으므로 staging할 수정된 소스파일이 빨간색으로 표시될 것이다.

여기서 이 파일을 커밋하기 위해 staging하고 싶다면,
git add <파일>
여러 파일을 수정했는데 모두 staging하고 싶다면,
git add .
어떤 파일을 staging하고싶지 않다면
git checkout -- <파일>

git commit -m “message” // 스테이징 후 커밋한다.
git push // 원격저장소에 푸시한다. git remote show origin 에서 확인했듯 디폴트로 remote develop브랜치로 푸시된다.

이제 팀원2도 작업을 할 것이다. 여기서부터 다양한 갈래의 시나리오가 발생한다.
팀원1이 위에서 수행한 작업을 먼저 pull 받고 작업한다.
팀원2가 pull받지 않고 작업한 내용을 워킹트리에 저장했다.  
팀원2가 pull받지 않고 작업한 내용을 add 했다.
팀원2가 pull받지 않고 작업한 내용을 commit했다.
팀원2가 pull받지 않고 작업한 내용을 push했다.

1이 가장 깔끔하다. 즉 한 브랜치에서 공동작업할 때는 원격develop브랜치의 다른 팀원의 최신 작업결과를 항상 pull받은 후에 자신의 작업을 하는 게 좋다. 이는 모든 팀원의 개발 결과물이 항상 '반영할만'하다고 생각될 때 적용되는 시나리오다.

2~5는 좀 복잡해진다.  4까지는 아무 에러가 없다. 하지만 5는 쫑이난다. 충돌한다. 왜냐하면 팀원2의 작업결과가 이미 로컬브랜치에 커밋되었고 해당 커밋버전과 원격브랜치의 커밋버전이 다르기 때문이다. 이 두 개가  다른데 어떻게 할 꺼냐고 묻는다.  방법은 여러가지다. 팀원2의 최근 커밋을 취소(git reset HEAD^ --hard)한다. 그리고 깔끔하게 1로 pull받고 작업을 다시 한다. 작업내용을 아까워할 필요는 없다 로컬브랜치에 취소한 커밋버전도 남아 있으니까. 그래도 작업소스를 복붙해두는게 좋다.

권장되는 시나리오는 4번까지 너무 쉽게 가지 않는 것이다. 즉 commit을 하지 않는 것이다. 하지만 작업내용을 (복붙말고) 어떤 식으로든 남겨놓고 싶다면?

git stash save 이름 // ‘이름’으로 임시 작업내용 백업하기. 가장 최근 커밋버전으로 워킹트리가 복원됨. 작업내용이 불완전하거나 테스트용이라서 커밋하기 애매하지만 보관해두고 싶을 때 등 매우 유용함. unstash는 아래를 참고하자.
참고: http://wit.nts-corp.com/2014/03/25/1153

이런식으로 팀원 여러 명이 한 브랜치를 가지고 작업을 할 수 있다.
여기까지의 작업은 develop이라는 브랜치를 개발팀이 공동으로 사용하는 시나리오이다. 이후 부터는 기능별로 브랜치를 따서 작업하는 시나리오를 따라가 보자.

git checkout -b feature1 develop  // develop브랜치를 원형으로 feature1 브랜치를 만든다.  이 브랜치에서 당신이 만들 기능(featrue)을 위한 작업을 수행한다.

git status // 현재 어떤 브랜치에 있는지와 당신이 작업한 내용을 깃이 추적하는 내용을 확인한다. 최초 원형으로 삼은 develop브랜치와 달라진 작업내용이 있다면, 무엇을 하면 되는지 깃이 안내해준다.


git rebase 브랜치명
브랜치명의 변경사항을 현재 브랜치에 적용합니다.

git merge 브랜치명
브랜치명의 브랜치를 현재 브랜치로 합칩니다. --squash 옵션을 주면 브랜치명의 모든 커밋을 하나의 커밋으로 만듭니다.










--------
응용편

git stash save 이름  ‘이름’으로 임시 작업내용 백업하기. 가장 최근 커밋버전으로 워킹트리가 복원됨. 작업내용이 불완전하거나 테스트용이라서 커밋하기 애매하지만 보관해두고 싶을 때 등 매우 유용함.
참고: http://wit.nts-corp.com/2014/03/25/1153

git diff 특정브랜치명  현재브랜치와 ‘특정브랜치명'과의 차이 상세
git diff 특정브랜치명 --name-status 차이 있는 파일목록만


깃에서 HEAD란?
현재 사용 중인 브랜치의 선두 부분을 나타내는 이름입니다. 기본적으로는 'master'의 선두 부분을 나타냅니다. 'HEAD' 를 이동하면, 사용하는 브랜치가 변경됩니다.

git reset HEAD^ 최종 커밋을 취소. 워킹트리는 보존됨.
git reset HEAD^ --hard 워킹트리도 reset됨. 즉, stash해놓는 방법과, 아예 커밋하고 reset하는 방법이 용도가 유사한 듯하다.

git checkout 특정커밋번호의앞6자리. // 해당 특정 커밋으로 돌아가기. --hard로 워킹트리는??

*상황: 팀원이 작업하고 푸시한 원격저장소의 '특정 커밋'에서 '특정파일'만 받아오고 싶을 때:(같은 브랜치에서 작업중이라는 가정이 있다.)
git fetch // merge하지는 않기 때문에 워킹트리에는 영향이 없다.
git checkout 특정커밋 특정파일

git cherry-pick


.git폴더의 config파일을 보면 각 로컬브랜치들이
어떤 원격브랜치로부터 fetch나 pull을 받는지,(pull=fetch+merge)
또 어떤 원격브랜치로 push하는지
확인 할 수 있다.

.gitignore가 먹지 않을 때 다음을 수행한다.
git rm -r --cached .  // 현재 git이 tracking(staging)대상으로 삼는 모든 파일을 인덱스에서 제거한다.
vi로 .gitignore를 편집한다.  
git add . // .gitignore가 적용된 파일들을 제외하고 staging한다.
git commit -m “apply .gitignore”
git push

git commit --amend 이전 커밋메시지 수정




https://git-scm.com/book/ko/v1/Git-%EB%B8%8C%EB%9E%9C%EC%B9%98-%EB%A6%AC%EB%AA%A8%ED%8A%B8-%EB%B8%8C%EB%9E%9C%EC%B9%98
로컬브랜치(트래킹브랜치)-리모트브랜치. 로컬브랜치가 리모트브랜치와 연결되면 트래킹브랜치가 된다. 위 링크 최하단을 자세히 보자.


.git/폴더 안의 내용들을 파악해보자.
