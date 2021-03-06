# Open CV操作筆記
[原文網址](https://medium.com/@yanweiliu/python%E5%BD%B1%E5%83%8F%E8%BE%A8%E8%AD%98%E7%AD%86%E8%A8%98-%E4%B8%89-open-cv%E6%93%8D%E4%BD%9C%E7%AD%86%E8%A8%98-1eab0b95339c)
## 引入模組
```python=
import numpy as np
import cv2
```
## 讀取圖檔
```python=
img = cv2.imread('image.jpg')
#以 cv2.imread 讀進來的資料，會存成NumPy陣列
----------------------------------------------------------
# 以灰階的方式讀取圖檔
img_gray = cv2.imread('image.jpg', cv2.IMREAD_GRAYSCALE)
#cv2.IMREAD_COLOR     為預設值
#cv2.IMREAD_GRAYSCALE 以灰階的格式來讀取圖片。 
#cv2.IMREAD_UNCHANGED 讀取圖片中所有的 channels，包含透明度的 channel。
```
## 查看圖片屬性
```python=
#1920×1080的彩色圖片
img.shape
(1080, 1920, 3)      
#(圖片高度,圖片寬度,RGB維度)
RGB微度彩色為3，灰階為1
```
## 建立特徵分類器
```python=
faceCascade = cv2.CascadeClassifier(cascPath)
#Cascade為一個XML檔
```
## 偵測圖片
```python=
faces = faceCascade.detectMultiScale(
    gray,
    scaleFactor=1.1,
    minNeighbors=5,
    minSize=(30, 30),
    flags = cv2.cv.CV_HAAR_SCALE_IMAGE
)
#detectMultiScale為偵測特徵的功能
#gray是灰階圖片
#scaleFactor圖像縮放比例，類似相機X倍鏡頭
#minNeighbors針對特徵點附近進行檢測
#minSize特徵檢測點的最小尺寸
scaleFactor,minNeighbors,minSize數值大小將影響辨識結果，須依狀況進行實驗
```
## 繪製方框
```python=
for (x, y, w, h) in faces:
    cv2.rectangle(img, (x, y), (x + w, y + h), (0, 255, 0), 2)
#(0, 255, 0)欄位可以變更方框顏色(Blue,Green,Red)
#轉換成RGB格式
#rgb_image = cv2.cvtColor(image, cv2.COLOR_BGR2RGB)
```
## 在圖片內畫線
```python=
import cv2
output = image.copy()
cv2.line(output, (60, 20), (400, 200), (0, 0, 255), 5)
viewImage(output, "2 Doggos separated by a line")
#output為要畫線的圖片名稱
#(60, 20)=(x1, y1)
#(400, 200)=(x2, y2)
#(0, 0, 255)為線條顏色(GBR或RGB，取決於是否使用cv2.cvtColor(image, cv2.COLOR_BGR2RGB))
#5 為線條粗細度
--------------------------------------------------------------
cv2.line()畫直線
cv2.circle()畫圓
cv2.rectangle()畫矩形
cv2.ellipse()畫橢圓
cv2.polylines()畫多邊形
```
## 顯示圖片
```python=
cv2.imshow('My Image', img)
cv2.waitKey(0)                #持續等待至使用者按下按鍵為止（單位為毫秒）              
cv2.destroyAllWindows()       #關閉所有視窗
cv2.destroyWindow('My Image') #關閉My Image視窗
```
## 裁切圖片
```python=
import cv2

 讀取圖檔

img = cv2.imread("lena.jpg")

# 裁切區域的 x 與 y 座標（左上角）
x = 100
y = 100
# 裁切區域的長度與寬度
w = 250
h = 150
# 裁切圖片
crop_img = img[y:y+h, x:x+w]
# 顯示圖片
cv2.imshow("cropped", crop_img)
cv2.waitKey(0)
# 寫入圖檔
cv2.imwrite('crop.jpg', crop_img)
```
## 調整亮度
```python=
import cv2
import numpy as np
img = cv2.imread('lena.jpg')
res = np.uint8(np.clip((1.5 * img + 10), 0, 255))
tmp = np.hstack((img, res))  # 兩張圖片橫向合併（便於對比顯示）
cv2.imshow('image', tmp)
cv2.waitKey(0)
```
## 改變圖片比例
```python=
import cv2
scale_percent = 20 # percent of original size
width = int(img.shape[1] * scale_percent / 100)
height = int(img.shape[0] * scale_percent / 100)
dim = (width, height)
resized = cv2.resize(img, dim, interpolation = cv2.INTER_AREA)
viewImage(resized, "After resizing with 20%")
```
## 旋轉指定角度
```python=
import cv2
(h, w, d) = image.shape
center = (w // 2, h // 2)
M = cv2.getRotationMatrix2D(center, 180, 1.0)
rotated = cv2.warpAffine(image, M, (w, h))
viewImage(rotated, "旋轉180度後")
#image.shape顯示高,寬,channels. 
#M:從中心旋轉180度.
```
## 水平垂直翻轉
```python=
import cv2
image=cv2.flip(img, 1)
#參數2 = 0： 垂直翻轉(沿x軸)
#參數2 > 0: 水平翻轉(沿y軸)
#參數2 < 0: 水平垂直翻轉
```
## 灰階效果
```python=
import cv2
gray_image = cv2.cvtColor(image, cv2.COLOR_BGR2GRAY)
ret, threshold_image = cv2.threshold(im, 127, 255, 0)
viewImage(gray_image, "Gray-scale doggo")
viewImage(threshold_image, "Black & White doggo")
#ret, threshold = cv2.threshold(im, 150, 200, 10)
#調整參數會有不同效果
```
## 高斯模糊
```python=
import cv2
blurred = cv2.GaussianBlur(image, (51, 51), 0)
viewImage(blurred, "Blurred doggo")
#image是要模糊化的圖片名稱
#(51, 51)必須為正奇數，數字越高照片越模糊
#0: sigmaX and sigmaY，預設值即可
```
## 邊緣檢測
```python=
import cv2
import numpy as np
img = cv2.imread('handwriting.jpg', 0)
edges = cv2.Canny(img, 30, 70)  # canny邊緣檢測
cv2.imshow('canny', np.hstack((img, edges)))
cv2.waitKey(0)
#cv2.Canny()進行邊緣檢測，參數2、3表示最低、高閾值
```
## 匹配圖片
```python=
用模板圖片去尋找圖片中的物件
#讀入原圖和模板
img_rgb = cv2.imread('mario.jpg')
img_gray = cv2.cvtColor(img_rgb, cv2.COLOR_BGR2GRAY)
template = cv2.imread('mario_coin.jpg', 0)
h, w = template.shape[:2]
#標準相關模板匹配
res = cv2.matchTemplate(img_gray, template, cv2.TM_CCOEFF_NORMED)
threshold = 0.8
loc = np.where(res >= threshold)  # 匹配程度大於%80的坐標y,x
for pt in zip(*loc[::-1]):  # *號表示可選參數
right_bottom = (pt[0] + w, pt[1] + h)
cv2.rectangle(img_rgb, pt, right_bottom, (0, 0, 255), 2)
```
## 縮放視窗大小
```python=
# 讓視窗可以自由縮放大小
cv2.namedWindow('My Image', cv2.WINDOW_NORMAL)
cv2.imshow('My Image', img)
cv2.waitKey(0)
cv2.destroyAllWindows()
```
## 寫入圖片檔案
```python=
# 寫入圖檔
cv2.imwrite('result.jpg', img)
# 寫入不同圖檔格式
cv2.imwrite('result.png', img)
cv2.imwrite('result.tiff', img)
# 設定 JPEG 圖片品質為 90（可用值為 0 ~ 100）
cv2.imwrite('output.jpg', img, [cv2.IMWRITE_JPEG_QUALITY, 90])
# 設定 PNG 壓縮層級為 5（可用值為 0 ~ 9）
cv2.imwrite('output.png', img, [cv2.IMWRITE_PNG_COMPRESSION, 5])
```
## 使用 Matplotlib 顯示圖片
```python=
#彩色圖片
import numpy as np
import cv2
from matplotlib import pyplot as plt
# 使用 OpenCV 讀取圖檔
img_bgr = cv2.imread('image.jpg')
img_rgb = cv2.cvtColor(img_bgr, cv2.COLOR_BGR2RGB)
# 使用 Matplotlib 顯示圖片
plt.imshow(img_rgb)
plt.show()
#灰階圖片
# 使用 OpenCV 讀取灰階圖檔
img_gray = cv2.imread('image.jpg', cv2.IMREAD_GRAYSCALE)
# 使用 Matplotlib 顯示圖片
plt.imshow(img_gray, cmap = 'gray')
plt.show()
```
## 加入英文字
```python=
import numpy as np
import cv2

img = np.zeros((400, 400, 3), np.uint8)
img.fill(90)

# 文字
text = 'Hello, OpenCV!'

# 使用字體
# cv2.putText(影像, 要顯示的文字, 座標, 字型, 字體大小, 顏色, 線條寬度, 線條種類)
cv2.putText(img, text, (10, 40), cv2.FONT_HERSHEY_SIMPLEX,
  1, (0, 255, 255), 1, cv2.LINE_AA)

cv2.imshow('My Image', img)
cv2.waitKey(0)
cv2.destroyAllWindows()
```
## 加入中文字
```python=
import cv2
import numpy
from PIL import Image, ImageDraw, ImageFont

def cv2ImgAddText(img, text, left, top, textColor=(0, 255, 0), textSize=20):
    if (isinstance(img, numpy.ndarray)):  #判断是否OpenCV图片类型
        img = Image.fromarray(cv2.cvtColor(img, cv2.COLOR_BGR2RGB))
    draw = ImageDraw.Draw(img)
    fontText = ImageFont.truetype(
        "font/simsun.ttc", textSize, encoding="utf-8")
    draw.text((left, top), text, textColor, font=fontText)
    return cv2.cvtColor(numpy.asarray(img), cv2.COLOR_RGB2BGR)
--------------------------------------------------------
img = cv2ImgAddText(img, "大家好", 140, 60, (255, 255, 0), 20)
```
## 視訊鏡頭影像
```python=
import cv2
# 選擇第二隻攝影機（0 代表第一隻、1 代表第二隻）。
cap = cv2.VideoCapture(1)
while(True):
  # 從攝影機擷取一張影像
  ret, frame = cap.read()
  # 顯示圖片
  cv2.imshow('frame', frame)
  # 若按下 q 鍵則離開迴圈
  if cv2.waitKey(1) & 0xFF == ord('q'):
    break
# 釋放攝影機
cap.release()
# 關閉所有 OpenCV 視窗
cv2.destroyAllWindows()
#用cap.isOpened() 檢查攝影機是否有啟動
#用cap.open() 啟動它。
影片相關資訊
import cv2
cap = cv2.VideoCapture(1)
# 解析 Fourcc 格式資料的函數
def decode_fourcc(v):
  v = int(v)
  return "".join([chr((v >> 8 * i) & 0xFF) for i in range(4)])
# 取得影像的尺寸大小
width = cap.get(cv2.CAP_PROP_FRAME_WIDTH)
height = cap.get(cv2.CAP_PROP_FRAME_HEIGHT)
print("Image Size: %d x %d" % (width, height))
# 取得 Codec 名稱
fourcc = cap.get(cv2.CAP_PROP_FOURCC)
codec = decode_fourcc(fourcc)
print("Codec: " + codec)
cap.release()
```
## 變更影片解析度
``` python=
import cv2
cap = cv2.VideoCapture(1)
# 設定影像的尺寸大小
cap.set(cv2.CAP_PROP_FRAME_WIDTH, 1280)
cap.set(cv2.CAP_PROP_FRAME_HEIGHT, 960)
while(True):
  ret, frame = cap.read()
  cv2.imshow('frame', frame)
  if cv2.waitKey(1) & 0xFF == ord('q'):
    break
cap.release()
cv2.destroyAllWindows()
```
## 讀取影片檔案
```python=
import cv2
# 開啟影片檔案
cap = cv2.VideoCapture('my_video.avi')
# 以迴圈從影片檔案讀取影格，並顯示出來
while(cap.isOpened()):
  ret, frame = cap.read()
  cv2.imshow('frame',frame)
  if cv2.waitKey(1) & 0xFF == ord('q'):
    break
cap.release()
cv2.destroyAllWindows()
```
## 寫入影片檔案
```python=
import cv2
cap = cv2.VideoCapture(1)
# 設定擷取影像的尺寸大小
cap.set(cv2.CAP_PROP_FRAME_WIDTH, 640)
cap.set(cv2.CAP_PROP_FRAME_HEIGHT, 360)
# 使用 XVID 編碼
fourcc = cv2.VideoWriter_fourcc(*'XVID')
#影片常見編碼格式：DIVX、XVID、MJPG、X264、WMV1、WMV2
# 建立 VideoWriter 物件，輸出影片至 output.avi
# FPS 值為 20.0，解析度為 640x360
out = cv2.VideoWriter('output.avi', fourcc, 20.0, (640, 360))
while(cap.isOpened()):
  ret, frame = cap.read()
  if ret == True:
    # 寫入影格
    out.write(frame)
    cv2.imshow('frame',frame)
    if cv2.waitKey(1) & 0xFF == ord('q'):
      break
  else:
    break
# 釋放所有資源
cap.release()
out.release()
cv2.destroyAllWindows()
```
