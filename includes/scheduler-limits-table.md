---
title: include dosyası
description: include dosyası
services: scheduler
ms.service: scheduler
author: derek1ee
ms.topic: include
ms.date: 08/16/2016
ms.author: deli
ms.custom: include file
ms.openlocfilehash: be52bf289898dfcd3ee669f12329669f6fabd356
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "47006523"
---
Aşağıdaki tabloda her bir ana kotaları, sınırları, Varsayılanları ve Azure scheduler'da kısıtlamalar açıklanmaktadır.

| Kaynak | Sınırı açıklaması |
| -------- | ----------------- |
| **İş boyutu** | En fazla iş boyutu 16 K'dır. Bu sınırdan büyük bir işin boyutu PUT veya PATCH işlemi sonuçlanırsa, 400 Hatalı istek durum kodu döndürülür. | 
| **İş koleksiyonları** | En fazla Azure aboneliği başına iş koleksiyonları 200.000 sayısıdır. | 
| **Koleksiyon başına iş** | Varsayılan olarak, maksimum iş sayısını beş ücretsiz iş koleksiyonu işler ve 50 işlerinde standart iş koleksiyonu var. <p>İş koleksiyonu projelerde en fazla sayısını değiştirebilirsiniz. Bir iş koleksiyonundaki tüm işlerin, iş koleksiyonu üzerinde ayarlanan değer sınırlıdır. En büyük iş kotası değerinden daha fazla iş oluşturmaya çalışırsanız, istek 409 çakışma durum kodu ile başarısız olur. | 
| **Başlangıç saati süresi** | "Zaman başlangıç saati için" en fazla 18 aydır. |
| **Yinelenme aralığı** | En fazla yinelenme aralığı 18 aydır. | 
| **Sıklık** | Varsayılan olarak, en fazla sıklığını kota ücretsiz iş koleksiyonu bir saat ve bir dakika içinde bir standart iş koleksiyonu var. <p>En fazla sıklığını bir proje koleksiyonunda en küçük yapabilirsiniz. İş koleksiyonundaki tüm işlerin iş koleksiyonu üzerinde ayarlanan değer sınırlıdır. İş koleksiyonu üzerinde en fazla sıklığından daha yüksek sıklığa sahip bir iş oluşturma girişimi, istek 409 çakışma durum kodu ile başarısız olur. | 
| **Gövde boyutu** | Bir istek için en fazla gövdesi 8192 karakterden boyutudur. |
| **İstek URL'si boyutu** | İstek URL'si için boyut üst sınırı 2048 karakter var. |
| **Üst bilgi sayısı** | En fazla üst bilgi, 50 üstbilgileri sayısıdır. | 
| **Toplam üstbilgi boyutu** | Maksimum toplam üstbilgi boyutu 4096 karakter var. |
| **zaman aşımı** | İstek zaman aşımı statiktir (yapılandırılabilir) ve HTTP eylemleri için 60 saniyedir. Uzun çalışan işlemleri için zaman uyumsuz HTTP protokollerini izleyin. Örneğin, bir 202 hemen geri döner ancak arka planda çalışmaya devam. | 
| **İş geçmişi** | İş geçmişinde depolanan en yüksek yanıt gövdesi 2048 bayt olabilir. |
| **İş Geçmişi bekletme** | İş geçmişi, en fazla 2 ay için ya da son 1000 yürütmeleri kadar tutulur. | 
| **Tamamlanan ve hatalı iş bekletme** | Tamamlanan ve hatalı işler 60 gün boyunca tutulur. |
||| 

