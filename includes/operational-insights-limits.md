
>[!NOTE]
>Log Analytics daha önce Operasyonel Öngörüler olarak biliniyordu.
>
>

Abonelik başına Log Analytics kaynakları için aşağıdaki sınırlar geçerlidir:

| Kaynak | Varsayılan Sınır | Yorumlar
| --- | --- | --- |
| Abonelik başına ücretsiz çalışma alanı sayısı | 10 | Bu sınır yükseltilemez. |
| Abonelik başına ücretli çalışma alanı sayısı | Yok | Bir kaynak grubundaki kaynak sayısı ve abonelik başına kaynak grubu sayısı ile sınırlıdır | 


Aşağıdaki sınırlamalar Log Analytics çalışma alanlarının her biri için geçerlidir:

|  | Ücretsiz | Standart | Premium | Tek Başına | OMS |
| --- | --- | --- | --- | --- | --- |
| Gün başına toplanan veri birimi |500 MB<sup>1</sup> |None |None | None | None
| Veri saklama süresi |7 gün |1 ay |12 ay | 1 ay<sup>2</sup> | 1 ay <sup>2</sup>|

<sup>1</sup> Müşteriler 500 MB günlük veri aktarımı sınırına ulaştığında veri analizi durur ve sonraki günün başlangıcında devam eder. Bir gün için UTC temel alınır.

<sup>2</sup> Tek Başına ve OMS fiyatlandırma planları için veri saklama süresi 730 güne çıkarılabilir.

| Kategori | Sınırlar | Yorumlar
| --- | --- | --- |
| Veri Toplayıcı API’si | Tek bir gönderi için boyut üst sınırı 30 MB'tır<br>Alan değerleri için en büyük boyut 32 KB'dir | Büyük hacimleri birden fazla gönderiye bölme<br>32 KB'den daha uzun alanlar kesilir. |
| Arama API’si | Toplu olmayan veriler için 5000 kayıt döndürülür<br>Toplu veriler için 500000 kayıt döndürülür | Toplu veriler `measure` komutunu içeren bir aramadır
 
