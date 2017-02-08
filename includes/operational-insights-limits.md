
Operasyonel Öngörüler / Log Analytics çalışma alanlarının her biri için aşağıdaki sınırlamalar geçerlidir.

|  | Ücretsiz | Standart | Premium | Tek Başına | OMS |
| --- | --- | --- | --- | --- | --- |
| Günlük veri aktarımı sınırı |500 MB<sup>1</sup> |None |None | None | None
| Veri saklama süresi |7 gün |1 ay |12 ay | 1 ay<sup>2</sup> | 1 ay <sup>2</sup>|
| Veri depolama sınırı |500 MB * 7 gün 3,5 = GB |sınırsız |sınırsız | sınırsız |sınırsız | 

<sup>1</sup> Müşteriler 500 MB günlük veri aktarımı sınırına ulaştığında veri analizi durur ve sonraki günün başlangıcında devam eder. Bir gün için UTC temel alınır.

<sup>2</sup> Tek Başına ve OMS fiyatlandırma planları için veri saklama süresi 730 güne çıkarılabilir.

| Kategori | Sınırlar | Yorumlar
| --- | --- | --- |
| Veri Toplayıcı API’si | Tek bir gönderi için boyut üst sınırı 30 MB'tır<br>Alan değerleri için en büyük boyut 32 KB'dir | Büyük hacimleri birden fazla gönderiye bölme<br>32 KB'den daha uzun alanlar kesilir. |
| Arama API’si | Toplu olmayan veriler için&5000; kayıt döndürülür<br>Toplu veriler için&50000;0 kayıt döndürülür | Toplu veriler `measure` komutunu içeren bir aramadır
 


<!--HONumber=Jan17_HO4-->


