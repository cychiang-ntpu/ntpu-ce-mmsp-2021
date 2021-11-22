# 4. Fundamentals of DSP

###### tags: `Multimedia Signal Processing (U3329, Fall, 2020)`
###### 通訊系 江振宇 副教授


## 4.1 Simulating RC circuit with DSP

### 4.1.1 使用微分方程分析
![](https://i.imgur.com/cajskLT.jpg)

---
### 4.1.2 使用阻抗觀念分析
![](https://i.imgur.com/ZVNaTQn.jpg)


---
### 4.1.3 使用傅立葉轉換做分析工具
![](https://i.imgur.com/fLoiAOm.jpg)


---
### 4.1.4 探討不同頻率 f 的信號輸入至 RC circuit 的結果，注意 3dB 的定義！

當 RC low-pass filter 的 3dB 點在 400 Hz

![](https://i.imgur.com/GLe5iA0.jpg)

---
### 4.1.5 RC 電路的 transfer function
![](https://i.imgur.com/cSlu1mL.png)

---
### 4.1.6 From Continuous to Discrete

使用計算機模擬 RC low-pass filter

![image](https://user-images.githubusercontent.com/70786533/142796260-d9f34111-258e-439d-8314-29542473f2b9.png)

![image](https://user-images.githubusercontent.com/70786533/142796317-471c6115-8ae2-401d-909e-6d431bc1f4aa.png)

![image](https://user-images.githubusercontent.com/70786533/142796345-9cbdfeb2-ab75-4763-8ee0-57d249ce62d2.png)

### 4.1.7 使用 DSP 實現 RC low-Pass Filter 
參數：
1. 3dB 在 400Hz
2. Sampling rate = 16000Hz

![image](https://user-images.githubusercontent.com/70786533/142799846-e34bd738-8330-4fb8-b0be-7136ec779ce0.png)

---
使用傅立葉轉換分析

![image](https://user-images.githubusercontent.com/70786533/142800042-c4ec6be5-3f49-4acf-920a-6e1cd9b8db07.png)

---
使用 eigenfunction 分析

![image](https://user-images.githubusercontent.com/70786533/142800140-56bcaadc-074e-48fe-b39d-5e7158287a27.png)

---
使用 impulse response 分析

![image](https://user-images.githubusercontent.com/70786533/142800273-cc18d600-c950-4891-a6de-685332eb0fc0.png)

---
Magnitude and Phase

![image](https://user-images.githubusercontent.com/70786533/142800761-fdc1f791-d280-405d-ad8c-5ed9a3602eeb.png)

請點以下網頁看控制不同的 a 所造成的 Magnitude & Phase

https://www.geogebra.org/calculator/ng5hmw6v


<img src="https://latex.codecogs.com/svg.latex?\inline&space;\omega=2\pi\frac{400}{16000}" title="\omega=2\pi\frac{400}{16000}" />

<a href="https://www.codecogs.com/eqnedit.php?latex=|H(e^{j\omega})|\approx&space;0.68\neq&space;\frac{1}{\sqrt2}=0.70710678118" target="_blank"><img src="https://latex.codecogs.com/svg.latex?|H(e^{j\omega})|\approx&space;0.68\neq&space;\frac{1}{\sqrt2}=0.70710678118" title="|H(e^{j\omega})|\approx 0.68\neq \frac{1}{\sqrt2}=0.70710678118" /></a>


---

Linear Constant Coefficient Difference Equation (LCCDE)

![image](https://user-images.githubusercontent.com/70786533/142800343-436c9439-11eb-4e5c-be1f-722abb1ce2d5.png)

---
直接將 Analog System 離散化模擬，會與真正的 Analog System 有所落差！
![image](https://user-images.githubusercontent.com/70786533/142800993-f79bd8cc-3531-4889-97b3-971f0eee7b6b.png)

---
乾脆直接使用 DSP 的 LCCDE 之參數來設計 digital filter 算了！
![image](https://user-images.githubusercontent.com/70786533/142801331-ad2012c5-f348-454f-bd57-0b219f2f5011.png)



## 4.2 Discrete Signals and Systems
### 4.2.1 Eigensystem
![image](https://user-images.githubusercontent.com/70786533/142797383-21e482bc-e2d1-400b-a0e7-9f26825f8ac4.png)

![image](https://user-images.githubusercontent.com/70786533/142797410-372c4598-a44c-4fd7-a4f4-6d43b6d3a190.png)

![image](https://user-images.githubusercontent.com/70786533/142797438-cc3c9680-407b-4660-bf5e-873ad4b35cfb.png)

### 4.2.2 Steady-Sate and Transient State

![image](https://user-images.githubusercontent.com/70786533/142797489-4b708026-383a-46ed-abc7-e7cc8a487048.png)

![image](https://user-images.githubusercontent.com/70786533/142797524-ea725937-ce5f-4ee3-8367-e7e5fa0f23c3.png)

## 4.3 Disign of Digital Filter

### 4.3.1 Ideal Low-Pass Filter

![image](https://user-images.githubusercontent.com/70786533/142797540-ff51c4c0-7367-425a-8672-e2d2ae9acb49.png)

![image](https://user-images.githubusercontent.com/70786533/142797568-46d35d48-9a1b-4870-a06e-965d6b16c68e.png)













