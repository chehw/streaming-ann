Streamming ANN Framework - Demo Applications

=================================

### 簡介
[demo](https://github.com/tlzs-dev/streaming-ann/blob/master/demo/)目錄下提供了幾個示例程序，使用微服務（microservices）架構，預設了3個應用場景，通過實例來簡要介紹框架的使用方法。

   - Dependencies
    
        Library         |  Description
        ----------------|-----------------------
         libsoup-2.4    | HTTP server	(服務端)
         json-c         | json 			(服務端、客戶端)
         gtk+-3.0       | GUI			(客戶端)

#### 0. 服務端： 

[ai-server.c](https://github.com/tlzs-dev/streaming-ann/blob/master/demo/ai-server.c)

通過HTTP-POST方式，根據用戶請求返回相應的AI處理結果。（示例程序中加載並使用了兩個不同的模型）

- 接口仕樣：

	- Request URL:  http://server_ip:9090/ai?engine=1
		/* engine： =0時使用第一個模型； =1時使用第二個模型。*/
	- Method: POST
	- Content-type: image/jpeg
	- Example: 
	```
		$ curl -X POST -H "Content-Type: image/jpeg" \
		--data-binary "@image_file.jpg" \
		http://127.0.0.1:9090/ai?engine=0
	```
	- Response: application/json 
	
	/* 以下是示例中使用的yolov3時的返回仕樣 */
	```
		{
			"model": "darknet::YoloV3",
			"detections":                     // AI檢知到的所有結果（數組）。 
			[
				"class": "<object name>",     // 物體名稱
				"class_index": "<object id>", // ID
				"confidence": 1.0,            // 置信度，取值範圍0.x~1.0之間（AI-engine的內部處理中默認情況下會自動過濾掉可信度低於0.5的檢測結果）。
				"left": 0.0,      // 物體的x坐標，用相對坐標表示,取值範圍[0.0~1.0]
				"top": 0.0,       // 物體的y坐標
				"witdh": 0.0,     // 物體的寬度
				"height": 0.0,    // 物體的高度
			]
		}
	```

    備註：
     
    （1）如需使用https, 可通過soup_server_set_ssl_cert_file（）添加證書文件，並在listen時指定SOUP_SERVER_LISTEN_HTTPS方式。

    （2）如需支持跨站資源共用（CORS）,可參考框架提供的[webserver/upload.c](https://github.com/tlzs-dev/streaming-ann/blob/master/webserver/upload.c)中的實現。
        

#### 1. demo-01： (物體識別) 

[demo-01.c](https://github.com/tlzs-dev/streaming-ann/blob/master/demo/demo-01.c)
    
    渋谷スクランブル交差点的行人、車輛、信號燈等的識別。

#### 2. demo-02:  (根據物體id或名稱，在關聯的數據庫中檢索對應的資料)。

[demo-02.c](https://github.com/tlzs-dev/streaming-ann/blob/master/demo/demo-02.c)
    
    沖縄美ら海水族館的魚類識別，鼠標懸停時可顯示從數據庫中檢索到的詳細信息。

#### 3. demo-03： (根據檢測到物體坐標作相關處理)

[demo-03.c](https://github.com/tlzs-dev/streaming-ann/blob/master/demo/demo-03.c)
    
    竜王公園駐車場可用車位的實時檢測。



