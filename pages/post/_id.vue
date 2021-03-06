<template lang="md">
  # Git Cheat Sheet

  ## Config

  ##### 사용자 설정
  ```sh
  // get
  git config --global user.name
  git config --global user.email

  // set
  git config --global user.name djlee
  git config --global user.email djlee@test.com

  git commit --amend --reset-autor // 사용자 설정후 기존 commit 반영

  git config -e               // 로컬설정 vim으로 보기
  git config --global -e      // 글로벌설정 vim로 보기
  git config --list           // 설정 리스트 형식으로 보기
  git config --list|grep core // core 설정만 보게끔 필터링
  ```

  ##### 인코딩 설정
  ```sh
  gui.encoding				// utf-8
  i18n.commitEncoding			// utf-8/cp949
  core.quotepaht				// false
  i18n.logOutputEncoding 		// cp949
  ```

  ##### git diff 설정
  ```sh
  core.pager	                // cat   (default: '')
  ```
  > cat으로 설정시 diff 모든 내용 한번에 출력

  ## Log
  ```sh
  git log -p //변경사항까지 확인
  git log --branches  --decorate --graph --oneline // 모든 브랜치의 로그 합쳐보기
  git log [branch1]..[branch2]  // branch1에 없고 branch2에만 있는 로그 보기
  git reflog  // reference log 보기
  ```

  ## diff
  #####특정 commit간의 차이 확인
  ```sh
  git diff [commitID1]..[commitID2] +++ 기준이 commitID2로 비교
  git diff [branch1]..[branch2]   +++기준이 branch2로 비교
  ```

  ## Show
  ```sh
  git show --name-only [commitID] // 해당커밋에서 수정된 파일들 보기
  ```

  ## 리모트

  ##### 리모트 추가
  ```sh
  git remote add [remotename] [remoteurl]
  ```

  ##### push 리모트만 추가
  ```sh
  git remote set-url [remotename] --add [remoteurl]
  ```

  ##### push 리모트 Disabled 처리
  ```sh
  git remote set-url --push [remotename] [alias] ("DISABLE")  // URL 을 임의의 alias 로 바꿔주는 원리
  ```

  ## 브랜치

  ##### 로컬 브랜치명 수동설정
  ```sh
  git checkout -b [branch] [remotename]/[branch]
  ```

  ##### 로컬 브랜치명 자동설정

  ```sh
  git checkout --track [remotename]/[branch]
  ```

  #### 현재 브랜치를 리모트에 있는 해당 브랜치로 추적
  ```sh
  git branch -u [remotename]/[branch]
  ```

  ##### 모든 브랜치들 및 그 브랜치들의 업스트림과 마지막 커밋 나열하기
  ```
  git branch -vv
  ```

  ##### 특정 브랜치만 clone
  ```sh
  git clone [remoteurl] -b [branch] [dirname]
  ```

  ##### Branch 이름 변경
  ```sh
  git branch -m [change-new-branch]
  git branch -m [change-old-branch] [change-new-branch]
  ```

  ##### 리모트 브랜치 받기
  ```sh
  git fetch [remotename]
  git checkout -b [branchname]
  ```

  ## Reset
  ```sh
  git reset [commit id] --hard // 특정커밋으로 되돌리기 --hard 특정 커밋 이후 수정된파일 모두 사라짐
  git reset --hard ORIG_HEAD // reset 취소
  ```
  > reset은 말그대로 이전 커밋으로 돌아가는 것이기 때문에 공유되지않은 local환경에서만 활용해야한다
  이미 공유된 commit에서는 이전 커밋으로 돌아가도 그것을 이미 공유받은사람이 다시 push할 경우 되살아나기때문에
  이미 공유된 commit에서는 이전 커밋으로 돌아가는 reset이 아닌 이전 commit으로 하나의 commit을 새로 생성하는 revert를 활용해야 한다.

  ## Git Flow

  ##### feature추가시 base branch 지정
  ```sh
  git flow feature start [add-branch] [base-branch]
  ```

  ```sh
  git flow feature finish [add-branch]
  ```

  ##### release 따서 출시 branch 생성
  ```sh
  git flow release start [add-branch] [base-branch] // default base: develop
  git flow release finish [add-branch]
  git flow release publish [add-branch] 다른사람들에게 게시
  ```

  ##### hotfix 따서 branch 생성
  ```sh
  git flow hotfix start [hotfix-version] [base-branch] // default base: master
  git flow hotfix finish [hotfix-version]
  git flow hotfix publish [hotfix-version]
  ```

  ##### git reset
  ```sh
  git reset --hard
  // pull 이전으로 되돌리기
  ```

  ## Core
  ```
  // autocrlf 해제
  git config core.autocrlf false
  ```

  ## 태그
  ```
  // 로컬 태그 삭제
  git tag -d [태그]
  git tag -a [태그] -m "annotation"   // annotated tag 생성

  // 원격 태그 삭제
  git push [저장소명] :[태그]
  // 원격 태그 일괄 삭제
  git tag -l | grep <정규식>  | xargs git push --delete <remote name>
  예>>>>>   git tag -l | grep 20180[8-9].* | xargs git push --delete upstream
  // 원격 태그 가져와서 로컬 참조 만들기
  git fetch origin refs/tags/[:tag]]:refs/tags/[:tag]]
  // 원격 태그 모두 받기
  git fetch [:remote] --tags
  ```

  ## Stash
  ```sh
  git stash
  git stash save [name]

  git stash apply
  git stash pop
  ```
  > stash에 Untracked 파일은 stash에 포함되지 않는다.

  ## git flow 실험
  release start 시

  - base를 지정하지 않으면 현재 내가 위치한 branch와 상관없이 default로 develop를 기반으로 release 가 생성됨
  - base를 지정하면 설정한 branch를 기반으로 release가 생성되고 base branch로 relese finish 할때 머지됨

  release start 한 후 base가 되었던 branch를 지울운 후 release finish할 경우 에러발생

  `The base 'feature/test' doesn't exists locally or is not a branch. Can't finish the release branch 'release/test2'.`

  ## 충돌
  ```baash
  // 새로운 폴더로 기존 저장소 덮어쓰기
  git push // 혹은
  git pull // 시에 에러가 나게 된다
  fatal: refusing to merge unrelated histories  // 관련없는 히스토리의 병합이 거부되는 것인데

  git pull origin master --allow-unrelated-histories // 이 옵션을 주게 되면 충돌이 나면서 merge할수있도록 pull받아진다.
  // 충돌을 해결 한후 push 하면 되는 것이다.
  ```
</template>

<script>
import readme from '~/README.md'

export default {
  computed: {
    readme() {
      return readme
    }
  },
  asyncData() {
    return {
      title: 'asdf'
    }
  }
}
</script>
