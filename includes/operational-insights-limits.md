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
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66238802"
---
Aşağıdaki sınırlar abonelik başına Azure Log Analytics kaynakları için geçerlidir.

| Resource | Varsayılan limit | Açıklamalar
| --- | --- | --- |
| Abonelik başına ücretsiz çalışma alanı sayısı | 10 | Bu sınır yükseltilemez. |
| Abonelik başına ücretli çalışma alanı sayısı | Yok | Bir kaynak grubundaki kaynak sayısı ve abonelik başına kaynak gruplarının sayısını sınırlama getirilir. | 

>[!NOTE]
>2 Nisan 2018'den itibaren otomatik olarak yeni çalışma alanlarında yeni bir abonelik kullanmak *GB başına* fiyatlandırma planı. 2 Nisan'dan önce oluşturulmuş mevcut abonelikleri veya mevcut bir Kurumsal Anlaşma kaydına bağlı bir abonelik için yeni çalışma alanları için üç fiyatlandırma katmanları arasından seçim devam edebilirsiniz. 
>

Her bir Log Analytics çalışma alanı için aşağıdaki sınırlar geçerlidir.

|  | Boş | Standart | Premium | Tek Başına | OMS | GB başına |
| --- | --- | --- | --- | --- | --- |--- |
| Gün başına toplanan veri birimi |500 MB<sup>1</sup> |None |Yok. | Yok. | Yok. | None
| Veri saklama süresi |7 gün |1 ay |12 ay | 1 ay<sup>2</sup> | 1 ay<sup>2</sup>| 1 ay<sup>2</sup>|

<sup>1</sup>zaman müşterilere kendi 500 MB günlük veri aktarım limiti, veri analizi durur ve sonraki günün başlangıcında devam eder. Bir gün için UTC temel alınır.

<sup>2</sup>tek başına, OMS ve fiyatlandırma planları GB başına veri saklama süresi 730 güne artırılabilir.

| Kategori | Limits | Açıklamalar
| --- | --- | --- |
| Veri Toplayıcı API’si | Tek bir gönderi için boyut üst sınırı 30 MB ' dir.<br>Alan değerleri için maksimum boyut 32 KB'dir. | Daha büyük birimleri birden fazla gönderiye bölme.<br>32 KB'den daha uzun alanlar kesilir. |
| Arama API’si | Toplu olmayan veriler için 5000 kayıt döndürdü.<br>500.000 kayıtları toplu veriler için. | Toplu veriler içeren bir arama olduğundan `summarize` komutu.
 
