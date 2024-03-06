


<a href="https://github.com/KIMGUUNI/A_EyeF/">
        <img src="https://github.com/Limmaji/hyeji/assets/118683437/dd5ab833-a1d1-4136-8821-d3df838b5cd2" width = "80%">
 </a>
 
 # 🖥 A-EYE 🖥

> #### 🏆 수상 : 우수상
> 
> ### Team name : A_eye
> 
> #### AI 기반 CCTV 분석을 통한 맞춤 광고 재공 서비스
> 
> #### 역할 : Modeling
>
> #### 기간 : 2024년 02월 01일 ~ 02월 27일 (27일 소요)
 <details>
 <summary><b>팀 프로젝트</b></summary>
 
</br>

## 1. 사용 기술
#### `Back-end`
  - Spring
  - JQuery
  - JavaScript
  - Oracle
  - AWS

#### `Front-end`
  - React
  - HTML
  - CSS
  - JavaScript
  - mui

</br>


## 2. ERD 설계
![image](https://github.com/KIMGUUNI/A_EyeF/assets/142488051/d002a731-40eb-4bec-9337-c2773b836a6a)

</br>


## 3. 담당 기능
 - 나이 예측 모델 구현
 - 성별 예측 모델 구현
 - 로그아웃 기능 구현
 - 모델 연결
 - Queue 전송

</br>
</br>


## 4. 데이터 수집 및 전처리

### 4.1. 데이터 수집

> #### [AI-hub 안면 인식 에이징 데이터](https://www.aihub.or.kr/aihubdata/data/view.do?currMenu=&topMenu=&aihubDataSe=data&dataSetSn=71415)
>
> 원천 데이터	
> - 참여자 1인당 50장의 유아~현재까지의 연령대별 사진
> - 초상권 사용 동의를 얻는 참여자 안면 제외하고 사진 블러링 처리 (PNG) 50,250장
>
>  라벨링 데이터
> - 참여자 출생년도, 현재 나이, 사진 상 나이, 원본 사진 촬영 기기 등의 수집 메타 정보
> - 얼굴 바운딩박스 및 5점 키포인트 가공 정보 (JSON) 50,250개

</br>

- #### 데이터 분포

<table>
  <tr>
    <td align="center">남자</td>
    <td align="center">여자</td>
  </tr>
  <tr>
    <td align="center"><strong>44.6%</strong></td>
    <td align="center"><strong>55.4%</strong></td>
  </tr>
  <tr>
    <td align="center"><b>22,400건</b></td>
    <td align="center"><b>27,850건</b></td>
  </tr>
</table>

 <table>
  <tr>
    <td align="center">20~35세</td>
    <td align="center">36~49세</td>
    <td align="center">50세 이상</td>
  </tr>
  <tr>
    <td align="center"><strong>41.6%</strong></td>
    <td align="center"><strong>35.7%</strong></td>
    <td align="center"><strong>22.7%</strong></td>
  </tr>
  <tr>
    <td align="center"><b>20,900건</b></td>
    <td align="center"><b>17,950건</b></td>
    <td align="center"><b>11,400건</b></td>
  </tr>
</table>

</br>



### 4.2. 데이터 전처리

![image](https://github.com/Limmaji/hyeji/assets/118683437/81acfc4f-affb-498e-8aa2-594da258fdf3)

- 압축 풀기
- 라벨링 데이터 추출
- 데이터 프레임으로 변환
- 얼굴 영역 추출
- 라벨 데이터 원핫인코딩
- 이미지 0~1 사이값으로 정규화

  </br>
  </br>

#### 4.2.1. 압축 풀기

<details>
<summary><b>코드 보기</b></summary>

~~~python

# 이미지 폴더
import zipfile
import os

# 압축 파일이 들어 있는 폴더 경로
zip_folder_path = './data/Training/image'

# 압축을 푼 폴더 경로
extract_folder = './data/Training/image_1'

# 압축 파일 폴더 내의 모든 파일에 대해 반복
for zip_file_name in os.listdir(zip_folder_path):
    # 각 압축 파일의 경로
    zip_file_path = os.path.join(zip_folder_path, zip_file_name)

    # 압축을 푸는 작업 (위의 코드를 활용)
    with zipfile.ZipFile(zip_file_path, 'r') as zip_ref:
        zip_ref.extractall(extract_folder)

    # 압축 해제된 파일 목록 출력
    extracted_files = os.listdir(extract_folder)
    print(f"Extracted Files from {zip_file_name}:", extracted_files)

~~~

~~~python

# 라벨링 폴더
import zipfile
import os
# 압축 파일이 들어 있는 폴더 경로
zip_folder_path = './data/Training/label'

# 압축을 푼 폴더 경로
extract_folder = './data/Training/label_1'

# 압축 파일 폴더 내의 모든 파일에 대해 반복
for zip_file_name in os.listdir(zip_folder_path):
    # 각 압축 파일의 경로
    zip_file_path = os.path.join(zip_folder_path, zip_file_name)

    # 압축을 푸는 작업 (위의 코드를 활용)
    with zipfile.ZipFile(zip_file_path, 'r') as zip_ref:
        zip_ref.extractall(extract_folder)

    # 압축 해제된 파일 목록 출력
    extracted_files = os.listdir(extract_folder)
    print(f"Extracted Files from {zip_file_name}:", extracted_files)

~~~

</details>

</br>


#### 4.2.2. 라벨링 데이터 특성 추출
  - age_past
  - imgsize
  - width
  - height
  - gender
  - annotation (box, x, y, w, h)

</br>

> - 15 ~ 59세의 나이 데이터만 추출
> (데이터 특성상 1 ~ 19세의 데이터가 많아, 소비력이 있다고 판단되는 15세부터 데이터를 추출하였다.)
>
> - 10대 ~ 50대로 범주화


<details>
<summary><b>코드 보기</b></summary>

~~~python

# 가져올 데이터의 폴더 경로
image_folder = './data/Training/image_1/'
label_folder = './data/Training/label_1/'

~~~

~~~python

def create_dataframe(image_folder, label_folder):

    # 이미지 폴더 내의 이미지 파일 목록 가져오기
    image_files = [f for f in os.listdir(image_folder) if f.lower().endswith(('.png', '.jpg', '.jpeg'))]

    # 데이터프레임을 저장할 리스트 생성
    data = []

    # 이미지 파일과 라벨 정보를 이용하여 데이터프레임에 추가
    for image_file in image_files:

        # 이미지 파일 경로
        image_path = os.path.join(image_folder, image_file)

        # 라벨 파일 경로
        label_file_path = os.path.join(label_folder, image_file.replace('.png', '.json'))  # 파일 확장자에 따라 수정

        # 라벨 파일 읽기
        if os.path.exists(label_file_path):
            labels = pd.read_json(label_file_path)

            # 이미지 파일에 대한 라벨 정보
            if not labels.empty:

                # 데이터프레임에 추가할 각 열의 값
                age_past = labels.iloc[0]['age_past']
                width = labels.iloc[0]['width']
                height = labels.iloc[0]['height']
                gender = labels.iloc[0]['gender']
                box_data = labels["annotation"][0]["box"]
                x, y, w, h = box_data["x"], box_data["y"], box_data["w"], box_data["h"]

                # 데이터프레임에 추가
                data.append([image_path, age_past, width, height, gender, x, y, w, h])

    # 데이터프레임 생성
    columns = ['image_path', 'age_past', 'width', 'height', 'gender', 'x', 'y', 'w', 'h']
    df = pd.DataFrame(data, columns=columns)

    return df

# 이미지 폴더 경로와 라벨 디렉토리 경로 지정
image_folder_path = './data/Training/image_1/'
label_folder_path = './data/Training/label_1/'

# 데이터프레임 생성
df_result = create_dataframe(image_folder_path, label_folder_path)

# 10살 이상만 추출하기
df_result=df_result[df_result['age_past']>=15]

# 60 미만만 추출하기
df_result=df_result[df_result['age_past']<60]

# 연령 구간별로 나누기
df_result['age_past'] = [x//10*10 for x in df_result['age_past']]

# 데이터프레임 저장
df_result.to_csv('output_dataframe.csv', index=False)


# 나이, 성별, 이미지 데이터 추출
ages = df['age_past'].values
genders = df['gender'].values
image_paths = df['image_path'].values


~~~

</details>

</br>

#### 4.2.3. 이미지에서 얼굴 영역만 추출

- 배열로 저장

<details>
<summary><b>코드 보기</b></summary>

~~~python

# 이미지 자르는 함수 만들어주기
def crop_image(image, x, y, w, h):
    return image[y:y+h, x:x+w]

# 이미지 데이터를 3D 배열로 변환
img_size = (224, 224)
images = []

# cropped_images 리스트 생성
cropped_images = []

~~~

~~~python

# 이미지 자르는 함수 사용하기
for index, row in df.iterrows():

    # x, y, w, h 값을 정수로 변환
    row['x'] = int(row['x'])
    row['y'] = int(row['y'])
    row['w'] = int(row['w'])
    row['h'] = int(row['h'])

    # 이미지 읽기
    img = cv2.imread(row['image_path'])

    # 이미지가 제대로 읽어졌는지 확인
    if img is not None:

        # 이미지 크기 확인
        if img.shape[0] > 0 and img.shape[1] > 0:

            # 이미지를 자르고 리스트에 추가
            if row['x'] >= 0 and row['y'] >= 0 and row['x'] + row['w'] <= img.shape[1] and row['y'] + row['h'] <= img.shape[0]:

              cropped_img_resized = cv2.resize(crop_image(img, row['x'], row['y'], row['w'], row['h']), img_size)

              # BGR에서 RGB로 변환
              img_1= cv2.cvtColor(cropped_img_resized, cv2.COLOR_BGR2RGB)

              images.append(img_1)

            else:
              print(f"Error: Invalid cropping box coordinates for image at index {index}, path: {row['image_path']}")
        else:
            print(f"Error: Invalid image size for {row['image_path']}")
    else:
        print(f"Error reading image: {row['image_path']}")

# 리스트를 numpy 배열로 변환
images = np.array(images)


~~~

</details>

</br>

#### 4.2.4. 나이 데이터 범주화

- 원 핫 인코딩 진행
- 0~1 값으로 정규화 진행 

<details>
<summary><b>코드 보기</b></summary>

~~~python

# 나이 인코딩
ages_dm = pd.get_dummies(df['age_past'])
ages_dm

# 성별 인코딩
genders_dm = pd.get_dummies(df['gender'])
genders_dm.head()


X= images/255.0    # ---> 정규화
y_a = ages_dm
y_g= genders_dm

~~~

</details>

</br>

### 4.3. 전처리 후 데이터 분포

- 50,250개 => 25,125개
  
![image](https://github.com/Limmaji/hyeji/assets/118683437/d5556432-6c40-41ad-a668-d777638e2476)

</br>

### 5. Modeling

- age_model
<details>
<summary><b>코드 보기</b></summary>

~~~python

# model 구축을 위한 라이브러리 import
from tensorflow.keras.models import Model, Sequential, load_model
from tensorflow.keras.layers import BatchNormalization, Conv2D, MaxPool2D, Activation, Dropout, Lambda, Dense, Flatten, Input
import tensorflow as tf
import tensorflow.keras as keras
from tensorflow.keras.optimizers import Adam
from tensorflow.keras.callbacks import ModelCheckpoint, EarlyStopping
from sklearn.model_selection import train_test_split
from tensorflow.keras import backend as k
from tensorflow.keras.layers import MaxPooling2D
import numpy as np
import pandas as pd

# train_test_split
X_train, X_test, y_train_a, y_test_a, y_train_g, y_test_g = train_test_split(
    X, y_a, y_g, test_size=0.2, random_state=42 )

~~~

~~~python

# age_model 학습 설정값
init_lr = 0.0001
epochs = 50
opt = Adam(learning_rate=init_lr) 

# 과적합 방지를 위한 callback 설정
callbacks = [EarlyStopping(monitor='val_accuracy',
                                           patience=5),
             ModelCheckpoint(filepath = 'a_model05_{epoch:02d}_{val_accuracy:.3f}.hdf5',
                                             monitor='val_accuracy',
                                             save_best_only=True)]

~~~

~~~python

# age_model 모델 만들기

cnn_model = Sequential()

# 특성 추출부(Convolutional)
cnn_model.add(Conv2D(filters=32, kernel_size=(3, 3), input_shape=X_train[0].shape, padding='same', activation='relu'))
cnn_model.add(MaxPooling2D(pool_size=(2, 2)))
cnn_model.add(Dropout(0.25))

cnn_model.add(Conv2D(filters=64, kernel_size=(3, 3), padding='same', activation='relu'))
cnn_model.add(MaxPooling2D(pool_size=(2, 2)))
cnn_model.add(Dropout(0.25))

cnn_model.add(Conv2D(filters=128, kernel_size=(3, 3), padding='same', activation='relu'))
cnn_model.add(MaxPooling2D(pool_size=(2, 2)))
cnn_model.add(Dropout(0.25))

# 분류부 (전결합층, mlp)
cnn_model.add(Flatten())
cnn_model.add(Dense(units=512, activation='relu'))
cnn_model.add(Dropout(0.5))

# 출력층 (다중 클래스 분류)
num_classes = 5  # 클래스 수에 맞게 설정
cnn_model.add(Dense(units=num_classes, activation='softmax'))

# 컴파일 설정
cnn_model.compile(loss='categorical_crossentropy', optimizer='adam', metrics=['accuracy'])

cnn_model.fit(X_train, y_train_a, validation_split=0.30, batch_size=32, epochs=1000, callbacks=callbacks)

~~~


</details>

</br>

- gender_model

<details>
<summary><b>코드 보기</b></summary>
	
~~~python

# gender_model 학습 설정값
init_lr = 0.0001
epochs = 50
opt = Adam(learning_rate=init_lr)  # 'lr'를 'learning_rate'로 변경

# 과적합 방지를 위한 callback 설정
callbacks = [EarlyStopping(monitor='val_accuracy',
                                           patience=5),
             ModelCheckpoint(filepath = 'g_model05_{epoch:02d}_{val_accuracy:.3f}.hdf5',
                                             monitor='val_accuracy',
                                             save_best_only=True)]

~~~

~~~python

# gender 모델
from keras.models import Sequential
from keras.layers import Conv2D, MaxPooling2D, Dropout, Flatten, Dense

# 모델 정의
cnn_model1 = Sequential()

# 특성 추출부(Convolutional)
cnn_model1.add(Conv2D(filters=32, kernel_size=(3, 3), input_shape=X[0].shape, padding='same', activation='relu'))
cnn_model1.add(MaxPooling2D(pool_size=(2, 2)))
cnn_model1.add(Dropout(0.25))

cnn_model1.add(Conv2D(filters=64, kernel_size=(3, 3), padding='same', activation='relu'))
cnn_model1.add(MaxPooling2D(pool_size=(2, 2)))
cnn_model1.add(Dropout(0.25))

cnn_model1.add(Conv2D(filters=128, kernel_size=(3, 3), padding='same', activation='relu'))
cnn_model1.add(MaxPooling2D(pool_size=(2, 2)))
cnn_model1.add(Dropout(0.25))

# 분류부 (전결합층, mlp)
cnn_model1.add(Flatten())
cnn_model1.add(Dense(units=512, activation='relu'))
cnn_model1.add(Dropout(0.5))

# 출력층
num_classes = 2
cnn_model1.add(Dense(units=num_classes, activation='sigmoid'))

# 컴파일 설정
cnn_model1.compile(loss='binary_crossentropy', optimizer='adam', metrics=['accuracy'])

cnn_model1.fit(X_train, y_train_g,validation_split=0.3, batch_size = 32, epochs=1000, callbacks = callbac

~~~

</details>

</br>




### 5.5. 모델 예측결과

![image](https://github.com/Limmaji/hyeji/assets/118683437/f6589a48-3790-4108-b18f-69fe09192595)

</br>


## 6. 모델 연결하기

- dlib 라이브러리를 활용하여 얼굴을 감지
- 얼굴 감지 시, 나이, 성별 예측 모델 실행

~~~python

import cv2
import dlib
import numpy as np
from keras.models import load_model
from keras.preprocessing.image import img_to_array
from keras.applications.mobilenet_v2 import preprocess_input
from google.colab.patches import cv2_imshow
# from ultralytics import YOLO

# dlib 초기화
detector = dlib.get_frontal_face_detector()

# 얼굴 검출 모델 로드
# face_model = YOLO("./model/yolov8m-face_0/yolov8m-face.pt")  # 얼굴 검출 모델 로드

# 나이 및 성별 예측 모델 로드
age_model = load_model('./model/model1048_05_0.481.hdf5')  # 나이 모델 로드
gender_model = load_model('./model/model2_02_0.854.hdf5')  # 성별 모델 로드

def preprocess_image(image):
    # 모델이 기대하는 입력 형상으로 이미지 전처리
    image = cv2.resize(image, (224, 224))
    image = img_to_array(image)
    image = preprocess_input(image)
    image = np.expand_dims(image, axis=0)
    return image

# 함수 정의
def predict_age(model, input_image):
    # 예측을 위해 이미지를 적절한 형식으로 전처리
    input_image = preprocess_image(input_image)

    # 모델에 이미지 전달하여 예측 수행
    age_prediction = model.predict(input_image)

    # 예측 결과 반환
    return age_prediction

def predict_gender(model, input_image):
    # 예측을 위해 이미지를 적절한 형식으로 전처리
    input_image = preprocess_image(input_image)

    # 모델에 이미지 전달하여 예측 수행
    gender_prediction = model.predict(input_image)

    # 예측 결과 반환
    return gender_prediction

# 카메라 열기 
cap = cv2.VideoCapture('./cctv2.mp4') 

while True:
    ret, frame = cap.read()

    # 얼굴 감지
    faces = detector(frame)

    for face in faces:
        x, y, w, h = (face.left(), face.top(), face.width(), face.height())

        # 추출된 얼굴 영역
        face_image = frame[y:y+h, x:x+w]

        # 나이 예측
        age_predictions = predict_age(age_model, face_image)
        predicted_age_index = np.argmax(age_predictions)
        predicted_age = predicted_age_index * 10 + 10  # 인덱스를 나이로 변환
        age_accuracy = np.max(age_predictions)  # 나이 예측 정확도

        # 성별 예측
        gender_predictions = predict_gender(gender_model, face_image)
        predicted_gender_index = np.argmax(gender_predictions)

        gender_accuracy = np.max(gender_predictions)  # 성별 예측 정확도

        # 얼굴에 박스 그리기
        cv2.rectangle(frame, (x, y), (x + w, y + h), (0, 255, 0), 2)

        # 나이 및 성별 출력
        predicted_gender = 'f' if predicted_gender_index == 0 else 'm'
        text = f"age: {predicted_age}, gender: {predicted_gender}, acc: {age_accuracy * 100:.2f}% / {gender_accuracy * 100:.2f}%"
        cv2.putText(frame, text, (x, y - 10), cv2.FONT_HERSHEY_SIMPLEX, 0.9, (0, 255, 0), 2)

    # 화면에 표시
    cv2_imshow(frame)

    # 종료 키: 'q'
    if cv2.waitKey(1) & 0xFF == ord('q'):
        break

# 작업 완료 후 해제
cap.release()
cv2.destroyAllWindows()

~~~

- 실행결과
   
![image](https://github.com/Limmaji/hyeji/assets/118683437/fe1e80e3-3cfc-4ea0-8442-3b63a3421a4a)

</br>

## 7. Queue 전송

- SQS Message Body에 예측 값을 담아 전송해주기

~~~python

import boto3
import json
import uuid
import cv2
import numpy as np
from keras.models import load_model
from keras.preprocessing.image import img_to_array
from keras.applications.mobilenet_v2 import preprocess_input
from IPython.display import display, Image
import mediapipe as mp
from ultralytics import YOLO
import time


# AWS 계정 및 리전 설정
aws_access_key_id = 'aws_access_key_id'
aws_secret_access_key = 'aws_secret_access_key'
aws_region = 'ap-northeast-2'

# SQS 대기열 URL
sqs_queue_url = 'https://sqs.ap-northeast-2.amazonaws.com/851725308174/v2q.fifo'
# Boto3 SQS 클라이언트 생성
sqs = boto3.client('sqs', region_name=aws_region, aws_access_key_id=aws_access_key_id, aws_secret_access_key=aws_secret_access_key)
unique_id = str(uuid.uuid4())

# 큐에 메시지 보내는 함수
def send_message_to_sqs(added_value):
    try:
        
        added_value['age'] = str(added_value['age'])
        response = sqs.send_message(
        QueueUrl=sqs_queue_url,
        MessageBody=json.dumps(added_value),
        MessageGroupId=unique_id,
        MessageDeduplicationId=unique_id
        )
        print(f"Message sent to SQS: {response['MessageId']}")
    except Exception as e:
        print(f"Error sending message to SQS: {e}")


~~~


~~~python



~~~

## 8. 회고 / 느낀점


</br>

>
> ss
> ss

</details>



</br>
</br>
</br>




<a href="https://github.com/2023-SMHRD-IS-CLOUD-1/StrongRepo">
        <img src="https://github.com/Limmaji/hyeji/assets/118683437/36549b89-cf1d-493c-95db-2c7824672f35" width = "80%">
        
        
 </a>
 
 # 💊 Drug is Death 💊

> ### Team name : 강력 1팀
>
> #### openCV기반 10대 청소년 대상 마약 예방 교육 자료 및 체험 서비스
>
> #### 역할 : 대시보드 구현, 로그인 및 회원가입 기능 구현
>
> #### 기간 : 2023년 11월 22일 ~ 12월 07일 (21일 소요)
 <details>
 <summary><b>팀 프로젝트</b></summary>
 
</br>

## 1. 사용 기술
#### `Back-end`
  - Java 8
  - JQuery
  - JavaScript
  - Oracle
  - Chart.js
  - BootStrap
  - JSP

#### `Front-end`
  - HTML
  - CSS
  - JavaScript

</br>


## 2. ERD 설계
![image](https://github.com/Limmaji/hyeji/assets/118683437/bafbbaf7-6c10-4ab6-b4f5-752477a2b423)

</br>


## 3. 담당 기능
### MVC 패턴을 이용한 대시보드 구현
   - 연령 / 지역 / 년도별 통계에 따른 대시보드 구현

![image](https://github.com/Limmaji/hyeji/assets/118683437/6057f0be-d2de-4f87-a764-897ec171a420)
 
</br>

### 3.1. 전체 흐름

>  #### 1. JQuery 사용, AJAX를 통해 요청 수행
>  #### 2. DB에서 데이터 받아오기
>  #### 3. 차트 생성
> 
> ####  - [main.jsp](Strong1team2/src/main/webapp/WEB-INF/main.jsp) </br>
> ####  - [FrontController.java](Strong1team2/src/main/java/com/smhrd/frontcontroller/FrontController.java) </br>
> ####  - [DashBoardService.java](Strong1team2/src/main/java/com/smhrd/controller/DashBoardService.java) </br>
> ####  - [DashBoardMemberVO.java](Strong1team2/src/main/java/com/smhrd/model/DashBoardMemberVO.java) </br>
> ####  - [DashBoardDAO.java](Strong1team2/src/main/java/com/smhrd/model/DashBoardDAO.java) </br>
> ####  - [DashMemberMapper.xml](Strong1team2/src/main/java/com/smhrd/database/DashMemberMapper.xml) </br>

</br>

### 3.2. Controller

<summary><b>Controller</b></summary>

<div markdown="1">

~~~java
@WebServlet("*.do")
public class FrontController extends HttpServlet {
	private static final long serialVersionUID = 1L;

	private HashMap<String, Command> map = null;

	@Override
	public void init(ServletConfig config) throws ServletException {
		map = new HashMap<String, Command>();
		
		map.put("DashBoard.do", new DashBoardService());
	}

	protected void service(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {

		request.setCharacterEncoding("UTF-8");

		String uri = request.getRequestURI();

		String cp = request.getContextPath();

		String finaluri = uri.substring(cp.length() + 1);

		String path = null;
		Command com = null;
		if (finaluri.contains("Go")) {
			path = finaluri.substring(2).replaceAll(".do", "");
		} else {
			com = map.get(finaluri);
			path = com.execute(request, response);
		}
		// 3. 페이지 이동
		if (path == null) {
		} else if (path.contains("redirect:/")) {
			response.sendRedirect(path.substring(10));
		} else {
			RequestDispatcher rd = request.getRequestDispatcher("WEB-INF/" + path + ".jsp");
			rd.forward(request, response);
		}

	}

}
~~~
</div>

</br>

### 3.3. Service

<summary><b>Service</b></summary>
<div markdown="1">

~~~java

@WebServlet("/DashBoard")
public class DashBoardService implements Command {

	public String execute(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {

		// 대시보드 보여지게 하는 코드
		DashBoardDAO dao = new DashBoardDAO();

		List<DashBoardMemberVO> result = dao.temp1();
		List<DashBoardMemberVO> result1 = dao.temp2();
		List<DashBoardMemberVO> result2 = dao.temp3();
		List<DashBoardMemberVO> result3 = dao.temp4();
		
		List<DashBoardMemberVO> combinedResult = new ArrayList<>();
		    combinedResult.addAll(result);
		    combinedResult.addAll(result1);
		    combinedResult.addAll(result2);
		    combinedResult.addAll(result3);
		    
		for (DashBoardMemberVO value : result) {
		}
		for (DashBoardMemberVO value : result1) {
		}
		for (DashBoardMemberVO value : result2) {
		}
		for (DashBoardMemberVO value : result3) {
		}
		// 조회된 결과를 JSON으로 변환하여 클라이언트에게 응답
		response.setContentType("application/json");
		response.setCharacterEncoding("UTF-8");

		PrintWriter out = response.getWriter();
	    Gson gson = new Gson();
	    String jsonResult = gson.toJson(combinedResult);
	    out.print(jsonResult);

		return null; // 뷰 이름을 반환하지 않음
	
	}

}
~~~
</div>

</br>

### 3.4. Chart

#### 1. Bar-Chart : 연령별 단속 현황

        
<b>dashboard1.min.js</b>
<div markdown="1">
       
~~~java

$.ajax({

		// 데이터 요청 주소
		url: "DashBoard.do",

		// 데이터 타입
		dataType: "json",

		// 성공
		success: function(result) {
			console.log("성공");
			console.log("데이터: ", result); // 객체 전체를 출력해 보기 위해 콤마(,)를 사용하여 분리
			console.log("DashBoard.jsp2");

			// 서버로부터 받은 JSON 객체의 c_year와 c_num 속성값 출력
			if (result) {
				new Chartist.Bar(".net-income",
					{
						labels: ["0-19세", "20대", "30대", "40대", "50대", "60세 이상"],
						series: [[result[48].a_num + result[49].a_num, result[47].a_num, result[46].a_num, result[45].a_num, result[44].a_num, result[43].a_num ]]
					},
					e, [["screen and (max-width: 640px)",
						{
							seriesBarDistance: 7,
							axisX: { labelInterpolationFnc: function(e) { return e[0] } }
						}]])

			} else {
				// 예외 처리: res가 비어있거나 유효한 데이터가 없는 경우
				console.log("데이터가 없거나 형식이 맞지 않습니다.");
			}
		},
		error: function(xhr, status, error) {
			console.log("Error: ", error); // 에러 로그 출력
			console.log("DashBoard.jsp3");
		}
	});
~~~









#### 2. Polar-Chart : 직업별 단속 현황
<details>
        
<summary><b>chartjs.init2.js</b></summary>
<div markdown="1">        

~~~java

$(function() {
	"use strict";

	$.ajax({

		// 데이터 요청 주소
		url: "DashBoard.do",

		// 데이터 타입
		dataType: "json",

		// 성공
		success: function(result) {
			console.log("성공");
			console.log("데이터: ", result); // 객체 전체를 출력해 보기 위해 콤마(,)를 사용하여 분리
			console.log("DashBoard.jsp2");

			// 서버로부터 받은 JSON 객체의 c_year와 c_num 속성값 출력
			if (result) {

				//Polar Chart
				new Chart(document.getElementById("polar-chart"), {
					type: 'polarArea',
					data: {
						labels: [result[26].j_job_group, result[27].j_job_group, result[29].j_job_group, result[28].j_job_group + result[39].j_job_group, result[30].j_job_group, result[33].j_job_group, result[32].j_job_group, result[31].j_job_group, result[34].j_job_group],
						datasets: [
							{

								label: "Population (millions)",
								backgroundColor: ["#aee5ed", "#74c8d5", "#45a9b9", "#56abb9", "#60969f", "#3c838f", "#106e7d","#026777", "#00525f"],
								data: [result[26].j_num, result[27].j_num, result[29].j_num, result[28].j_num + result[39].j_num, result[30].j_num, result[33].j_num, result[32].j_num, result[31].j_num, result[34].j_num]
							}





						]
					},
					options: {
						title: {
							display: true,
							text: '마약사범 직업'
						}
					}
				});
			} else {
				// 예외 처리: res가 비어있거나 유효한 데이터가 없는 경우
				console.log("데이터가 없거나 형식이 맞지 않습니다.");
			}
		},
		// 예외
		error: function(xhr, status, error) {
			console.log("Error: ", error); // 에러 로그 출력
			console.log("DashBoard.jsp3");
		}
	});


}); 
~~~
</details>


#### 3. Morris-Line-Chart : 단속 현황

<details>

<summary><b>morris-data2.js</b></summary>
<div markdown="1">  
        
~~~java

        $(function() {
	"use strict";

	$.ajax({

		// 데이터 요청 주소
		url: "DashBoard.do",

		// 데이터 타입
		dataType: "json",

		// 성공
		success: function(result) {
			console.log("성공");
			console.log("데이터: ", result); // 객체 전체를 출력해 보기 위해 콤마(,)를 사용하여 분리
			console.log("DashBoard.jsp2");

			// 서버로부터 받은 JSON 객체의 c_year와 c_num 속성값 출력
			if (result) {
				var line = new Morris.Line({
					element: 'morris-line-chart',
					resize: true,
					data: [
						{ y: result[0].c_year.toString() , item1: result[0].c_num },
						{ y: result[1].c_year.toString(), item1: result[1].c_num },
						{ y: result[2].c_year.toString(), item1: result[2].c_num },
						{ y: result[3].c_year.toString(), item1: result[3].c_num },
						{ y: result[4].c_year.toString(), item1: result[4].c_num },
						{ y: result[5].c_year.toString(), item1: result[5].c_num },
						{ y: result[6].c_year.toString(), item1: result[6].c_num },
						{ y: result[7].c_year.toString(), item1: result[7].c_num },
						{ y: result[8].c_year.toString(), item1: result[8].c_num },
						{ y: result[9].c_year.toString(), item1: result[9].c_num }

					],
					xkey: 'y',
					ykeys: ['item1'],
					labels: ['Item 1'],
					gridLineColor: '#eef0f2',
					lineColors: ['#e4281c'],
					lineWidth: 3,
					hideHover: 'auto'


				});

			} else {
				// 예외 처리: res가 비어있거나 유효한 데이터가 없는 경우
				console.log("데이터가 없거나 형식이 맞지 않습니다.");
			}
		},
		error: function(xhr, status, error) {
			console.log("Error: ", error); // 에러 로그 출력
			console.log("DashBoard.jsp3");
		}
	});
});
~~~


</details>
</br>

## 4. 회고 / 느낀점


</br>

> 웹 페이지를 개발하는 프로젝트를 처음 진행하면서 배운 점도 아쉬운 점도 많았습니다.</br>
> 수업 시간에 배운 코드를 직접 작성하고 기능들을 구현하는 과정에서 평소 어렵게 느꼈던 JQuery 문법에 대해 더욱 자세히 알게 되었습니다.</br>
> 또한 AJAX(비동기 통신)로 대시보드에 필요한 데이터를 직접 받아오는 과정에서 눈에 보이지 않는 데이터의 흐름에 대해 이해도가 높아졌습니다.</br>
> 개발은 약 2주간의 짧은 시간이었지만 저의 능력을 확인하고 더욱 열심히 하게 되는 계기가 되었습니다.</br>
> 프로젝트를 진행하며 아쉬웠던 점은 첫 번째로 적절한 팀 회의를 진행하지 못한 것입니다.</br>
> 저희 팀은 프로젝트 초반 적절한 회의가 이루어지지 않아 개발 단계에서 버려지는 하루가 생기기도 했습니다.</br>
> 여섯명의 다양한 아이디어를 합쳐 더 좋은 웹페이지를 개발할 수 있는 기회였지만, 회의를 통해 풀어나갈 수 없었던 점이 아쉬웠습니다.</br>
> 두 번째는 주제에 대한 아쉬움이었습니다.</br>
> 저희 팀은 사업성 아닌 공공성을 목표로 교육용 웹 페이지를 개발했습니다. 기존의 교육용 페이지와 차별성을 두기 위해 다양한 체험 기능을 추가하고자 했습니다.</br>
> 하지만 기획 단계에서 아이디어가 나오지 않았고, 웹 페이지의 볼륨이 작아졌습니다.</br>
> 차별성 있는 좋은 주제라도 기획 단계에서 구현할 수 있는 기능들이 얼마 되지 않을 때 주제를 바꾸는 것도 하나의 방법이라는 것을 배웠습니다.</br>

</details>



