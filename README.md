# Image_Resize_Compare

**PIL thumbnail 함수는 resize된 이미지가 Rotate되는 issue가 있어서 detection 데이터셋엔 사용할 수 없다.** <br>
https://velog.io/@kpl5672/python-pillow-resize-thumbnail-%EC%82%AC%EC%9A%A9-%EC%8B%9C-%EC%9D%B4%EB%AF%B8%EC%A7%80-rotate%EB%90%98%EB%8A%94-%EB%AC%B8%EC%A0%9C-%ED%95%B4%EA%B2%B0

캡스톤에서 사용할 딥러닝 모델에 사용할 데이터셋을 구축하기위해 
aiHub(https://aihub.or.kr/aidata/30729) 에 있는 이미지를 다운로드하였다.

전체 이미지 152만장 총 2.5TB는 구글 드라이브에 업로드 할 수 없었기에,

이미지를 Resize(DownSampling)하여 저장하기로 생각하였다.

이미지의 일부를 살펴보니, 크기가 4000 x 6000 이미지 정도 되어 보여서(이미지 크기가 모두 달랐다)

단순하게 1/10로 Resize(DownSampling) 하였다.

Resize를 진행하던 중 2가지 문제를 발견했다.

**1. 너무 느리다.**

**2. 크기가 4000x6000인 이미지도 있는 반면, 크기가 100x120 처럼 매우 작은 이미지도 있어서 이미지 크기를 고려하여 Resize를 해주어야 했다.**


기존의 내가 쓰던 방법은 openCV의 Resize였고

1과 2의 문제점을 모두 해결한 방법은 PIL의 thumbnail이 었다.

사실 opencv가 느린 것은 Resize 함수 때문이 아니고, OpenCV의 I/O가 느린 것이었다.

<hr>

![image](https://user-images.githubusercontent.com/80030558/158776801-43bc794f-7868-40a1-be80-80656394b3be.png)

**<opencv와 PIL의 I/O 및 resize 속도 비교>**

<hr>

![image](https://user-images.githubusercontent.com/80030558/158776187-bba218e9-89a9-45a1-9438-f2f82dcba475.png)

**<12K 이미지에 대한 opencv와 PIL의 속도 비교>**

<hr>

thumbnail 함수는 aspect ratio를 유지하며 downsample 할 수 있는 함수이다.


<h1>시도해본 것</h1> 

**C++의 opencv** : python의 opencv가 더 빠르게 나왔다... <br>
**numba 라이브러리의 JIT** : numba는 numpy와만 호환되어 opencv를 사용한 코드에는 사용할 수 없었다.

<h1>참고하면 좋은 것들</h1>
https://python-pillow.org/pillow-perf/ 에서 각종 라이브러리들의 Resize외에도 rotate, blur 등의 함수의 benchmark를 보여주고있다.<br>
resize는 이미지 크기에 따라 함수 실행시간이 달라질 수 있다.

![image](https://user-images.githubusercontent.com/80030558/158916173-fcf8e675-e982-41eb-9d5f-0c5884be34d8.png)

🟪 ImageMagick
🟦 OpenCV
🟨 Pillow SIMD SSE
🟧 Pillow SIMD AVX2


