# 設定正確的批次大小

為了Spark Streaming應用程式能夠在集群中穩定運行，系统應該能夠以足夠的速度處理接收的資料（即處理速度應該大於或等於接收資料的速度）。這可以藉由串流的網路UI觀察得到。批次處理時間應該小於批次間隔時間。

根據串流計算的性質，批次間隔時間可能顯著的影響資料處理速率，這個速率可以藉由應用程式維持。可以考慮`WordCountNetwork`這個例子，對於一個特定的資料處理速率，系统可能可以每2秒印出一次單字計數
（批次間隔時間為2秒），但無法每500毫秒印出一次單字計數。所以，為了在生產環境中維持期望的資料處理速率，就應該設定合適的批次間隔時間(即批次資料的容量)。

找出正確的批次大小的一個好的办法是用一個保守的批次間隔時間（5-10,秒）和低資料速率來測試你的應用程式。為了驗證你的系统是否能滿足資料處理速率，你可以藉由檢查點到點的延遲值來判斷（可以在
Spark驅動程式的log4j日誌中查看"Total delay"或者利用StreamingListener介面）。如果延遲維持穩定，那麼系统是穩定的。如果延遲持續增長，那麼系统無法跟上資料處理速率，是不穩定的。
你能夠嘗試着增加資料處理速率或者減少批次大小來作進一步的測試。注意，因為瞬間的資料處理速度增加導致延遲瞬間的增長可能是正常的，只要延遲能重新回到了低值（小於批次大小）。

