## 1. 리눅스 : **UBUNTU 18.04**
### 1. Python 2.x 설치 

  1. apt-get install python 
      - python --version ==> Python **2.7.17** 
  2. apt-get install python-pip
  3. python -m pip install numpy

- [Getting_Started.pdf](uploads/c319927f8c17d373f2631b4ec2a753e3/Getting_Started.pdf)
- [IoT_sdk_overview.pdf](uploads/aa9ea139690074195b6de17a7802f6a3/IoT_sdk_overview.pdf)
- [tls_certificate_loading.pdf](uploads/cdf7ed2862316075940ee273f997f6de/tls_certificate_loading.pdf)
### 2. 컴파일 
 - 전체 컴파일 : buildall.sh 
 - 개별 컴파일 : build.sh -p exs_tx 
### 3. 사이닝 키 ( Signing an Application & Verifying <= 리눅스에서 작업 )
 1. openssl ecparam -name secp384r1 -out secp384r1.param.pem
 2. openssl ecparam -in secp384r1.param.pem -genkey -noout -out app_rot.key
 3. openssl ec -in app_rot.key -out app_rot.pub -pubout
 4. openssl req -new -sha384 -x509 -out app_rot.der -outform DER -key app_rot.key -days 7300 -set_serial 1 <br>-subj /C=DE/ST=Berlin/L=Berlin/O="THALES DIS AIS Deutschland GmbH"/OU="R&D"/CN="Demo App Root of Trust"
 5. openssl x509 -in app_rot.der -inform DER -pubkey -noout > app_rot.pub
 6. **_python app.py sign --key app_rot.key examples/helloworld/build/helloworld.bin_**
 7. python app.py verify --pubkey app_rot.pub --keyform pem
..\examples\helloworld\build\signed\helloworld.bin<br>**Verification OK**

### 4. 앱 인증서 생성 ( PC 상에서 생성 ) 
  1. Java 1.8 32bit 설치 
  2. Load Cert 바이너리 생성 
      - java -jar .\cmd_IpCertMgr.jar -mode app_rot -cmd writecert -certfile app_rot.der<br> -certIndex 0 -sigType NONE -file .\LoadAppRotCert.bin
  3. Del Cert 바이너리 생성 
      - java -jar .\cmd_IpCertMgr.jar -mode app_rot -cmd delcert -certfile app_rot.der<br> -certindex 0 -sigType NONE -file .\DelAppRotCert.bin
  4. 결과 ( 2,3 동일 )
      - Java version: 32-bit<br>1.8.0_361<br>**signature skipped**

#### 4.1 앱 로드 상태 확인 
  - fs.py **ls** a:/
  - c:\Python27\python.exe  app.py **delete** helloworld

#### 4.2 앱 다운로드
  - c:\Python27\python.exe  app.py **download** .\comtcp.bin

#### 4.3 앱 실행
  - 


### 5. 컨넥션 
  - 리눅스 : 연결 오류나옴  
  - Windows 10 : 연결됨 [ connect.py 사용  -p COM? -s 115200 ]
     ( pip 빌트인 파이썬은 2.7.9 이상 설치요, 2.7.10 으로 OK ) 
