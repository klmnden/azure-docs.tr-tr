---
title: include dosyası
description: include dosyası
services: log-analytics
author: MGoedtel
ms.service: log-analytics
ms.topic: include
ms.date: 03/29/2018
ms.author: magoedte
ms.custom: include file
ms.openlocfilehash: 29256b3bcfedb7fe5045ff4c6c3842eb25e00a28
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2018
---
Abonelik başına Log Analytics kaynakları için aşağıdaki sınırlar geçerlidir:

| Kaynak | Varsayılan Sınır | Yorumlar
| --- | --- | --- |
| Abonelik başına ücretsiz çalışma alanı sayısı | 10 | Bu sınır yükseltilemez. |
| Abonelik başına ücretli çalışma alanı sayısı | Yok | Bir kaynak grubundaki kaynak sayısı ve abonelik başına kaynak grubu sayısı ile sınırlıdır | 

>[!NOTE]
>2 Nisan 2018 itibariyle, yeni bir abonelik yeni çalışma alanları otomatik olarak kullanacak *GB başına* planı fiyatlandırma.  2 Nisan önce oluşturulmuş mevcut abonelikleri veya varolan bir EA kayıt bağlı bir abonelik için yeni çalışma alanları için üç fiyatlandırma katmanları arasında seçim devam edebilirsiniz. 
>

Aşağıdaki sınırlamalar Log Analytics çalışma alanlarının her biri için geçerlidir:

|  | Ücretsiz | Standart | Premium | Tek Başına | OMS | GB başına |
| --- | --- | --- | --- | --- | --- |--- |
| Gün başına toplanan veri birimi |500 MB<sup>1</sup> |None |None | None | None | None
| Veri saklama süresi |7 gün |1 ay |12 ay | 1 ay<sup>2</sup> | 1 ay <sup>2</sup>| 1 ay <sup>2</sup>|

<sup>1</sup> Müşteriler 500 MB günlük veri aktarımı sınırına ulaştığında veri analizi durur ve sonraki günün başlangıcında devam eder. Bir gün için UTC temel alınır.

<sup>2</sup> tek başına, OMS ve fiyatlandırma planı GB başına veri saklama süresi 730 gün sayısı artırılabilir.

| Kategori | Sınırlar | Yorumlar
| --- | --- | --- |
| Veri Toplayıcı API’si | Tek bir gönderi için boyut üst sınırı 30 MB'tır<br>Alan değerleri için en büyük boyut 32 KB'dir | Büyük hacimleri birden fazla gönderiye bölme<br>32 KB'den daha uzun alanlar kesilir. |
| Arama API’si | Toplu olmayan veriler için 5000 kayıt döndürülür<br>Toplu veriler için 500000 kayıt döndürülür | Toplu veriler `measure` komutunu içeren bir aramadır
 
