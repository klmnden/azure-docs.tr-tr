Azure kendi sanal ortamını şu öncelikle kullanmak için Python sürümünü saptar:

1. kök klasöründe runtime.txt dosyasında belirtilen sürüm
2. web uygulaması yapılandırmasında Python tarafından belirtilen sürüm (Azure portalında web uygulamanız için **Ayarlar** > **Uygulama Ayarları** dikey penceresi)
3. yukarıdakilerden hiçbiri belirtilmemişse python 2.7 varsayılan sürümdür

Burada söz edilen 

    \runtime.txt

içeriği için geçerli değerler:

* python-2.7
* python-3.4

Mikro sürüm (üçüncü basamak) belirtilmişse göz ardı edilir.

<!--HONumber=Sep16_HO3-->


