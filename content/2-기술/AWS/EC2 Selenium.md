# 인스턴스 생성
`t2.micro` 사용 (`t2.nano`는 성능 문제 있음)

`ap-northeast-2`에서 ubuntu로만 설정한 후 생성

# 키 권한 수정
```zsh
chmod 0600 <key.pem>
```

# SSH 연결
```zsh
ssh -i <key.pem> ubuntu@<ip>
```

# 의존성 설치
## Chromium, Selenium
```zsh
sudo apt install -y python3-pip unzip
sudo snap install chromium
pip install selenium webdriver-manager
```

# 테스트 코드
```python
from selenium import webdriver
from selenium.webdriver.chrome.options import Options
from selenium.webdriver.chrome.service import Service as ChromiumService
from webdriver_manager.chrome import ChromeDriverManager
from webdriver_manager.core.os_manager import ChromeType

options = Options()
options.add_argument("--headless=new")
options.add_argument("--remote-debugging-port=9515")

driver = webdriver.Chrome(service=ChromiumService(ChromeDriverManager(chrome_type=ChromeType.CHROMIUM).install()), options=options)

driver.get("https://www.google.com")
print(driver.title)

driver.quit()
```

# 자동 실행
## Cron 등록
```zsh
crontab -e
```

### 매분 실행
```cron
* * * * * python3 <파일 경로 (상대경로도 가능)>
```

### 초단위 실행
```cron
* * * * * python3 <파일 경로>
* * * * * (sleep 30; python3 <파일 경로>)
```
실행할 횟수만큼 반복해 주기

## Screen 사용
```zsh
screen -S <세션 이름>
```
프로그램 실행한 다음 `Ctrl+A D` 입력해서 Detatched Mode로 진입

다시 접속하려면
```zsh
screen -r <세션 이름>
```