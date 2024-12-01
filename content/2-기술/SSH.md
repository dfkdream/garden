# SSH Key 생성
```
ssh-keygen -t ed25519
```
Github 매뉴얼에서는 -c <이메일>을 옵션을 사용하는데, 일반 ssh 로그인에서는 필요 없는 것으로 보인다.
-> 생략하면 username@hostname으로 자동 입력됨.

# SSH Agent에 키 등록
```
ssh-add ~/.ssh/<...ed25519>
```
두 번째 인자는 private key 파일이어야 한다.

# Public Key 복사
```
ssh-copy-id -i <...pub> -p <port> <host>
```
-i 옵션 인자는 public key 파일이어야 한다.

# 인증 과정 확인 (디버그 출력)
```
ssh -v -p <port> <host>
```