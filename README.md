# hw3

在mqtt的source和gesture_UI的source中可能有些名詞有重複定義到所以要再建立一個新的mbed-os-build2來compile這次的功課

跟著以下步驟執行

1. mkdir -p ~/ee2405new

2. cp -r ~/ee2405/mbed-os ~/ee2405new

3. cd ~/ee2405new

4. mbed compile --library --no-archive -t GCC_ARM -m B_L4S5I_IOT01A --build ~/ee2405new/mbed-os-build2

然後用 
sudo mbed compile --source . --source ~/ee2405new/mbed-os-build2/ -m B_L4S5I_IOT01A -t GCC_ARM --profile tflite.json -ff 
去compile hw3

接著就可以開始執行main.cpp了
如果有改變網路IP記得要重設

首先開啟兩個terminal一個執行sudo screen /dev/ttyACM0
另一個執行sudo python3 wifi_mqtt/mqtt_client.py

接著在screen上輸入UI_mode/run來執行手勢設定threshold angle的function
當前的threshold angle會顯示在螢幕上
當決定好了之後按下user_button則會將此角度送出然後由python接收
最後再確認到對方有收到之後就關閉UI_mode回到可以輸入指令的模式

再來要執行是測傾角的function
在screen輸入tile_mode/run後便會進入測傾角的模式
一開始先平放板子，讓他設定reference acceleration(此時會亮藍燈)
等到橘燈亮起了之後便可以開始測量傾角。

當角度超過threshold angle時便會將這個角度存在over_angle[]這個array裡面
直到超過的角度累積超過10個之後便會啟動mqtt將角度傳出
並在確認到PC/Python端有收到10筆角度後結束此function。
