# Wordpress에 S3 연결하기

####  ec2 부스와 rds 부스는 모두 거쳐오셨나요? 저희 부스는 앞 2 부스의 이후 과정들을 다루고 있습니다. 특히 ec2 부스는 필수입니다!



#### 저희 부스에서는 Wordpress에 올린 미디어 파일들이 AWS S3 서비스에 자동으로 연결되어 저장되는 기능을 만든 후 과금이 일어나지 않도록 모든 서비스를 삭제시키는 과정을 담고 있습니다.



### 1. AWS S3 연결하기

---

##### 1. AWS S3 Bucket 생성하기

AWS 콘솔에 접속하여 순서대로 1️⃣번 서비스 버튼을 눌러 검색창에 2️⃣번과 같이 [S3] 라고 입력해주어 클라우드상의 확장 가능한 스토리지를 선택해주세요.

![](<https://raw.githubusercontent.com/AUSG/aws-univ-expo/master/s3/images/s3_1.png>)

S3 서비스 페이지에서 버킷 만들기를 선택해주세요.

![](<https://raw.githubusercontent.com/AUSG/aws-univ-expo/master/s3/images/s3_2.png>)

아래와 같은 창이 떴을 때 버킷이름을 wordpress-[영어 본인 이름] 을 입력해주신 뒤 리전이 [아시아 태평양(서울)] 인 것을 확인 후 다음 버튼을 눌러줍니다. 

![](<https://raw.githubusercontent.com/AUSG/aws-univ-expo/master/s3/images/s3_3.png>)

다음 버튼을 한 번더 누른 뒤 아래와 같은 화면이 떴을 때 모든 체크를 해제해줍니다.

![](<https://raw.githubusercontent.com/AUSG/aws-univ-expo/master/s3/images/s3_4.png>)

생성을 완료하면 대화상자가 사라집니다. wordpress-[영어 본인 이름] 글자를 클릭하여 해당 bucket 안으로 들어와 주세요.

![](<https://raw.githubusercontent.com/AUSG/aws-univ-expo/master/s3/images/s3_5.png>)

아래 사진과 같이 1️⃣ 권한 탭을 선택한 후 2️⃣ 액세스 제어 몱록을 클릭하여 3️⃣Everyone을 선택하여 뜨는 오른쪽 화면에서 4️⃣번과 같이 모든 설정을 체크한 후 저장버튼을 눌러주세요. S3 생성 및 설정이 완료되었습니다.

![](<https://raw.githubusercontent.com/AUSG/aws-univ-expo/master/s3/images/s3_6.png>)

##### 2. AWS IAM 사용하여 사용자 추가하기

AWS 콘솔에 접속하여 순서대로 1️⃣번 서비스 버튼을 눌러 검색창에 2️⃣번과 같이 [IAM] 라고 입력해주어 사용자 엑세스 및 암호화 키 관리를 선택해주세요. 해당 작업을 통하여 S3에 접근가능한 사용자를 생성하여 Wordpress에 연결해줄 수 있습니다.

![](<https://raw.githubusercontent.com/AUSG/aws-univ-expo/master/s3/images/iam_1.png>)

좌측의 1️⃣번 사용자 메뉴를 선택한 후 2️⃣번 사용자 추가를 선택해주세요.

![](<https://raw.githubusercontent.com/AUSG/aws-univ-expo/master/s3/images/iam_2.png>)

1️⃣번과 같이 사용자 이름을 [wordpress-user]로 입력한 뒤 2️⃣번과 같이 프로그래밍 방식 액세스를 선택하여 다음 버튼을 눌러주세요.

![](<https://raw.githubusercontent.com/AUSG/aws-univ-expo/master/s3/images/iam_3.png>)

1️⃣ 기존 정책 직접 연결을 선택 후 2️⃣번과 같이 s3를 검색하여 3️⃣ [AmazonS3FullAccess]를 선택 후 다음 버튼을 선택해주세요. 이 과정을 통해 wordpress-user에게 s3에 관련한 모든 권한을 주게 됩니다.

![](<https://raw.githubusercontent.com/AUSG/aws-univ-expo/master/s3/images/iam_4.png>)

계속 다음을 누르면 아래와 같이 계정이 생성됩니다. 화면에 나온 액세스 키 ID와 비밀 엑세스 키는 이 후 사용되므로 메모장에 복사해주세요. 비밀 엑세스 키는 표시 버튼을 눌러 원문을 복사하셔야 합니다.

![](<https://raw.githubusercontent.com/AUSG/aws-univ-expo/master/s3/images/iam_5.png>)

##### 3. Wordpress plugin[WP offload Media Lite] 설치하기

EC2 부스에서 사용하였던 Public 주소 뒤에 [/admin]을 추가하여 관리자 페이지로 이동합니다.

![](<https://raw.githubusercontent.com/AUSG/aws-univ-expo/master/s3/images/wp_1.png>)

좌측 메뉴에서 1️⃣[Plugins] 메뉴를 선택하고 상단의 2️⃣[Add New] 버튼을 클릭합니다.

![](https://raw.githubusercontent.com/AUSG/aws-univ-expo/master/s3/images/wp_2.png)

1️⃣번과 같이 [offload]를 검색하여 [WP Offload Media Lite for Amazon S3, DigitalOcean Spaces, and Google Cloud Storage] 플러그인의 우측 상단의 2️⃣[Install Now] 버튼을 클릭합니다. 버튼이 [Activate] 버튼으로 변경되면 [Activate] 버튼 역시 클릭합니다.

![](https://raw.githubusercontent.com/AUSG/aws-univ-expo/master/s3/images/wp_3.png)

다시 이동된 화면에서 1️⃣Active 버튼을 선택하여 그림과 같이 [WP Offload Media Lite]가 추가된 것을 확인하시고 2️⃣Settings를 선택해주세요.

![](https://raw.githubusercontent.com/AUSG/aws-univ-expo/master/s3/images/wp_4.png)

Amazon S3의 [I understand the risks but i`d like to...] 를 선택하여 메모장에 복사해 둔 Access Key ID와 Secret Access Key를 입력하여 하단의 Save 버튼을 눌러주세요.

![](https://raw.githubusercontent.com/AUSG/aws-univ-expo/master/s3/images/wp_5.png)

다음 화면에서 Bucket에 wordpress-[영어 본인 이름]을 입력해주세요.

![](https://raw.githubusercontent.com/AUSG/aws-univ-expo/master/s3/images/wp_6.png)

##### 4. 결과 확인하기

이제 잘 작동되는지 확인해 볼 차례입니다!  wordpress 좌측 메뉴에서 1️⃣Media를 선택하여 2️⃣Add New를 선택하고 3️⃣을 통해 다른 사람이 보아도 되는 사진을 하나 업로드 해주세요. 하단에 선택한 이미지가 표시되면 업로드가 완료된 것입니다.

![](https://raw.githubusercontent.com/AUSG/aws-univ-expo/master/s3/images/check_1.png)

AWS 홈페이지로 돌아가서 S3 서비스에 들어가 wordpress-[본인 영어 이름] Bucket을 확인해주세요. wp-content/uploads/2019/05/******/ 폴더 안에 업로드 한 사진의 여러 사이즈들이 자동으로 올라가 있습니다. 개 중에 하나를 선택하여 객체 Url을 복사해주세요.

![](https://raw.githubusercontent.com/AUSG/aws-univ-expo/master/s3/images/check_2.png)

![](https://raw.githubusercontent.com/AUSG/aws-univ-expo/master/s3/images/check_3.png)

다시 Wordpress로 돌아와 좌측 1️⃣Posts 메뉴를 클릭 후 2️⃣Add New 버튼을 클릭해주세요. 

![](https://raw.githubusercontent.com/AUSG/aws-univ-expo/master/s3/images/check_4.png)

제목을 입력하고 1️⃣ 번의 Add Image 버튼을 클릭해주세요. 그리고 2️⃣번과 같이 [Insert from URL] 버튼을 클릭 후 S3에서 복사해 둔 객체 url을 넣은 후 엔터를 입력해주세요. 우측 상단의 publish를 각각 2번 눌러주시면 업로드 된 사진을 Wordpress 퍼블릭 ip 주소에서 볼 수 있습니다!

![](https://raw.githubusercontent.com/AUSG/aws-univ-expo/master/s3/images/check_5.png)

![](https://raw.githubusercontent.com/AUSG/aws-univ-expo/master/s3/images/check_6.png)





### 2. 모든 서비스 삭제하기





