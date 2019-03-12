---
title: include dosyası
description: include dosyası
services: log-analytics
author: MGoedtel
ms.service: log-analytics
ms.topic: include
ms.date: 05/16/2018
ms.author: magoedte
ms.custom: include file
ms.openlocfilehash: 34f2ab8f7ccafb8b30e298cd71e09171ad8c87cb
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/07/2019
ms.locfileid: "57554150"
---
Aşağıdaki sınırlar abonelik başına Azure Log Analytics kaynakları için geçerlidir.

| Kaynak | Varsayılan limit | Yorumlar
| --- | --- | --- |
| Abonelik başına ücretsiz çalışma alanı sayısı | 10 | Bu sınır yükseltilemez. |
| Abonelik başına ücretli çalışma alanı sayısı | Yok | Bir kaynak grubundaki kaynak sayısı ve abonelik başına kaynak gruplarının sayısını sınırlama getirilir. | 

>[!NOTE]
>2 Nisan 2018'den itibaren otomatik olarak yeni çalışma alanlarında yeni bir abonelik kullanmak *GB başına* fiyatlandırma planı. 2 Nisan'dan önce oluşturulmuş mevcut abonelikleri veya mevcut bir Kurumsal Anlaşma kaydına bağlı bir abonelik için yeni çalışma alanları için üç fiyatlandırma katmanları arasından seçim devam edebilirsiniz. 
>

Her bir Log Analytics çalışma alanı için aşağıdaki sınırlar geçerlidir.

|  | Ücretsiz | Standart | Premium | Tek Başına | OMS | GB başına |
| --- | --- | --- | --- | --- | --- |--- |
| Gün başına toplanan veri birimi |500 MB<sup>1</sup> |None |Yok. | Yok. | Yok. | None
| Veri saklama süresi |7 gün |1 ay |12 ay | 1 ay<sup>2</sup> | 1 ay<sup>2</sup>| 1 ay<sup>2</sup>|

<sup>1</sup>zaman müşterilere kendi 500 MB günlük veri aktarım limiti, veri analizi durur ve sonraki günün başlangıcında devam eder. Bir gün için UTC temel alınır.

<sup>2</sup>tek başına, OMS ve fiyatlandırma planları GB başına veri saklama süresi 730 güne artırılabilir.

| Kategori | Sınırlar | Yorumlar
| --- | --- | --- |
| Veri Toplayıcı API’si | Tek bir gönderi için boyut üst sınırı 30 MB ' dir.<br>Alan değerleri için maksimum boyut 32 KB'dir. | Daha büyük birimleri birden fazla gönderiye bölme.<br>32 KB'den daha uzun alanlar kesilir. |
| Arama API’si | Toplu olmayan veriler için 5000 kayıt döndürdü.<br>500.000 kayıtları toplu veriler için. | Toplu veriler içeren bir arama olduğundan `summarize` komutu.
 
