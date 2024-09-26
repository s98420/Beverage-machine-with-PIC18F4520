## 設備
- PIC18F4520 * 1
- 沉水抽水馬達 * 3
- 按鈕 * 4
- 飲料機機體
- 繼電器 * 3
## 功能
- 該飲料機具備三桶隔離的儲水槽，可以分別存放三種飲料
- 模式控制按鈕：按下後切換模式
    - 連續出水：該模式持續出水一段時間(或是再按一下以提早停止)，不需一直按著出水按鈕
    - 一般出水：按下按鈕即出水，放開後停止
- 出水按鈕：三桶儲水槽的各自的按鈕，按下後馬達隨即開始運轉

## 初步系統架構
- PIC18F4520：我們使用四個按鈕接收使用者輸入的訊號，一個timer計算時間和uart控制putty訊息
    - 按鈕1 ~ 3(RB3 ~ RB5)：使用busy waiting的方式偵測是否要輸出訊號控制馬達，如果是連續出水模式就將輸出切換為1直到再被按下為止，一般出水模式則是放開按鈕時就改回將輸出改回0。
    - 按鈕4(RB1)：使用interrupt來偵測輸入並更改出水模式。
    - timer2：用來計算持續出水的時間。
    - uart：配合putty在電腦螢幕上顯示目前輸出的飲料種類和其他提示訊息
## 系統流程圖
![image](https://hackmd.io/_uploads/rkERbN-tp.png)


## 系統開發工具、材料及技術
- 娃娃三隻：替代尿尿小童
- 繼電器：PIC18提供之電壓不足，無法驅動馬達，所以依賴繼電器當作開關控制外部電源
- 沉水馬達：將飲料從瓶中抽出
## 遇到的問題
- 原先想利用高低差出水，再使用伺服馬達控制出水孔開閉，但一塊pic18只能操作兩個伺服馬達，因故放棄
- 使用PIC18提供之電壓直接控制馬達，但因爲馬達吃掉太多電壓，晶片直接停擺，所以開始思考使用外部電壓
- 使用繼電器控制外部電源：便宜繼電器無法正常操作，可能是PIC18提供之電壓仍舊無法操控，討論是否添購增壓器
- 試圖使用伺服馬達製作簡易繼電器，但遭到擱置
- 轉而使用一顆158的繼電器，成功操作，但出現另一項問題
- 一但外部電源通路，PIC18有機率停止，MPLAB說target halt，請求電機系幫手過了4小時仍舊無法解決。
- 開始仰賴玄學，各種電路接法都嘗試過後，發現是外接電源電壓過大，不能使用電腦或行動電源提供，最後使用電池盒解決繼電器問題