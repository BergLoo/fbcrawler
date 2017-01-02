# fbcrawler
讀取IdList.txt中Facebook ID，使用restfb API查詢每個Facebook ID的“談論這個的用戶”和“粉絲專頁按讚數”，並將結果存回VoltDB
#安裝voltdb
完整教學：https://github.com/VoltDB/voltdb/wiki/Building-VoltDB

1. 事先安裝
  1. Java 1.8
  2. Apache Ant 1.7 or newer
  3. A compiler with C++11 support: GNU C++ 4.4 or newer or on OS X, XCode 5.0 (with Clang 3.3) or newer
  4. Python 2.6 or newer (probably works on 2.4 for now)
  5. cmake 2.8 or newer
2. 得到voltdb程式碼  
  `git clone https://github.com/VoltDB/voltdb.git`
3. build  
  `cd voltdb`  
  `ant`
4. 開啟voltdb  
  `cd voltdb`  
  `bin/voltdb create`
5. 網頁版介面

  連線到 http://127.0.0.1:8080/
  
#restfb API
1. 下載: http://restfb.com/
2. 到 https://developers.facebook.com/ 申請為developer
3. 到工具及支援處->Graph API Explorer 得到存取權杖
4. 延長權杖到期時間，參考 https://www.youtube.com/watch?v=qFZazZ1JXsM&t=0s  

#程式實作方式  

將檔案(IdList.txt)讀入，使用restfb API查詢每行facebook ID的談論數和按讚數，其中也過濾掉不屬於粉絲團的facebook ID，最後將結果寫入voltdb中，voltdb我用localhost的方式連線。  
  
API查詢的部分，我使用執行緒(thread)，開了十條執行緒，使執行時間相較於單條執行緒一筆一筆查詢降低十倍，另外，時間降低有極限，我曾經設定過100條執行緒，不僅facebook API權杖被ban掉，連宿舍網路都被切斷。也就是說執行速度可以再上去(開大於10條執行緒)，但不知道facebook API能接受到什麼程度。權杖也有request次數限制，所以如果發現存入db的筆數異常，則需再等一段時間才可再執行。
