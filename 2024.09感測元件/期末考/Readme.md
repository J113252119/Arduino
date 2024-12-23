<h1>碩專一甲  J113252119 感測元件 期末作業</h1>

## 溫溼度警報器應用當溫度超過40度時，它會顯示當前溫度並啟動蜂鳴器 

## 電路準備材料 : 
>1. Arduino Nano板(CH340驅動程式.USB:MiniUSB) 
>2. MiniUSB 連接線 X 1 
>3. DHT11感測器
>4. TM1637 四位顯示器
>5. 蜂鳴器
>6. 麵包板 X 1 
===

## 電路圖
>![](https://github.com/J113252119/Arduino/blob/main/2024.09%E6%84%9F%E6%B8%AC%E5%85%83%E4%BB%B6/%E6%9C%9F%E6%9C%AB%E8%80%83/%E6%9C%9F%E6%9C%AB%E4%BD%9C%E6%A5%AD%E9%9B%BB%E8%B7%AF%E5%9C%96.JPG?raw=true)

## Arduinot程式碼函式庫下載
>![](https://github.com/J113252119/Arduino/blob/main/2024.09感測元件/期中考/Arduinot%20TM1637程式碼函式庫.JPG?raw=true)

>![](https://github.com/J113252119/Arduino/blob/main/2024.09感測元件/期中考/Arduinot%20DHT11程式碼函式庫.JPG?raw=true)

## 程式說明

[以下程式來源 DHT11.ino ]:[https://github.com/derricktsai0904/Arduino/blob/master/04%20NodeMCU/LEDControl/LED_Control.ino](https://github.com/derricktsai0904/Course/blob/main/2024.09%E6%84%9F%E6%B8%AC%E5%85%83%E4%BB%B6/Arduino%20LED%E9%9C%B9%E9%9D%82%E7%87%88/LED_Control.ino) "LED_Control.ino"
[以下程式來源 DHT11.ino ]
``` arduino
#include "DHT.h"
#include "TM1637.h"        //主程式需要程式庫 “TM1637.h”

#define DHTPIN 8   
#define DHTTYPE DHT11
#define CLK 3              //模組的CLK 接 Nano pin 3
#define DIO 2              //模組的DIO 接 Nano pin 2

TM1637 tm1637(CLK,DIO);    //宣告顯示晶片涵式庫
DHT dht(DHTPIN, DHTTYPE);

int dig1 = 0;
int dig2 = 0;
int dig3 = 0;
int dig4 = 0;

void setup() {
  Serial.begin(9600);
  dht.begin();

  tm1637.init();
  tm1637.set(BRIGHT_TYPICAL);
}

void loop() {
  float h = dht.readHumidity();
  float t = dht.readTemperature();
  if (isnan(t) || isnan(h)) {
    Serial.println("Failed to read from DHT");
    }
  else {
    //int dig1 = 0, dig2 = 0, dig3 = 0, dig4 = 0;
    dig4 = (t / 10);
    dig3 = t - ( dig4 * 10);

    dig2 = (h / 10);
    dig1 = h - ( dig2 * 10 );
    
    tm1637.display(0,dig4);       //千位數  千位 百位 = 溫度
    tm1637.display(1,dig3);       //百位數

    tm1637.display(2,dig2);       //十位數 十位 個位 = 濕度
