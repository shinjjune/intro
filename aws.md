# React를 aws S3, cloudfront 를 이용해서 배포하기

https://react-etc.vlpt.us/08.deploy-s3.html?q=

### 1. 빌드
```
npx create-react-app myapp
cd myapp

yarn build(혹은 npm run build)
```

### 2.IAM 에서 권한 설정 및 CLI 설정
```
1) 사용자추가
2) 엑세스 방식 : AmazonS3FullAccess 
3) 엑세스 키 저장 : .csv파일 다운로드
```
![image](https://user-images.githubusercontent.com/47058441/68078727-cb507600-fe1f-11e9-98b8-61b3a2c6c7cc.png)
![image](https://user-images.githubusercontent.com/47058441/68078735-ee7b2580-fe1f-11e9-9270-e17590e6eaac.png)
계속 다음다음다음...
![image](https://user-images.githubusercontent.com/47058441/68078756-35691b00-fe20-11e9-9499-12894e64f0b1.png)






### 3. AWS CLI 설치
```
1)설치를 완료하셨다면, CLI 를 통하여 유저를 추가하세요.
2) aws configure --profile myapp
AWS Access Key ID [None]: AKIAIGLC3EVKE3Z4JKWA
AWS Secret Access Key [None]: 비밀이에요비밀아까전에받은키여기에입력해요그리고이건다른사람한테절대로노출되면안돼요
Default region name [None]: ap-northeast-2
Default output format [None]: json
```
cmd
![image](https://user-images.githubusercontent.com/47058441/68078781-c8a25080-fe20-11e9-84c9-e306edd71572.png)

### 4. s3 설정
```
1)권한-버킷정책
{
  "Version":"2012-10-17",
  "Statement":[
    {
      "Sid":"AddPerm",
      "Effect":"Allow",
      "Principal": "*",
      "Action":["s3:GetObject"],
      "Resource":["arn:aws:s3:::myapp/*"]
    }
  ]
}
2) 정적웹사이트 호스팅
인덱스 문서 : index.html
3) 배포  package.json- scripts
 "scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test --env=jsdom",
    "eject": "react-scripts eject",
    "deploy": "aws s3 sync ./build s3://myapp --profile=myapp"
  }
4) $ yarn deploy
```

### 5. cloudfront 설정
```
1) create distribution -> web(Get Started) -> 
2) Origin Domain Name 에 커서를 위치시키면 만들어둔 S3 버킷이 보임 -> Viewer Protocol Policy (Redirect HTTP to HTTPS 설정) ->create distribution 
3) 해당 ID 클릭 후 -> EDIT -> Default Root Object :index.html설정( 왜???)
```

#### 코드 업데이트 시 
yarn build

-> yarn deploy

배포:
http://shinjh-react-news.s3-website.ap-northeast-2.amazonaws.com/
