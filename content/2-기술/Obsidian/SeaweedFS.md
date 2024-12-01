https://github.com/seaweedfs/seaweedfs/wiki/Amazon-S3-API
# 실행
```zsh
podman run --rm -p 8333:8333 -p 7333:7333 docker.io/chrislusf/seaweedfs server -webdav -s3
```
webdav와 s3 filler를 동시에 실행한다. webdav는 7333 포트, s3은 8333 포트를 사용한다.
# 버킷 생성
`dav://localhost:8333`에 접속해 `buckets/<버킷 이름>` 디렉토리를 생성한다. 버킷을 미리 생성해 두지 않으면 싱크 오류가 발생한다. 
# Remotely Save 설정
* Endpoint: `http://localhost:8333`
* Region: `us-east-1`
* Access Key ID: 공란
* Secret Access Key: 공란
* Bucket Name: `<버킷 이름>`
* S3 URL Style: Path-Style
# 남은 일
* 인증 설정
* WebDAV가 이상하게 동작하는 문제 해결
	* 파일 이름에 부모 디렉토리 경로가 같이 출력됨
		* ![[Pasted image 20240213155121.png | 300]]
	* MIME 오류로 파일이 열리지 않음