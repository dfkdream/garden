# Upstream 머지하기
1. `git fetch upstream`
	1. `upstream/main` 브랜치가 생긴다.
2. main 브랜치에서 `git merge upstream/main`
3. 충돌 수정 후 커밋

# 로컬 시스템에 원격 저장소 생성
출처: https://byteclass.tistory.com/19
1. `git init --bare repository-name` 
	1. repository-name에 원격 저장소를 만든다.
2. `git remote add remote-name path-to-remote` 
	1. 원격 저장소 경로 추가