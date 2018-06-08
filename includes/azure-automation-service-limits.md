---
title: include dosyası
description: include dosyası
services: automation
author: georgewallace
ms.service: automation
ms.topic: include
ms.date: 04/05/2018
ms.author: gwallace
ms.custom: include file
ms.openlocfilehash: 6b6e4afa7c8b18c8ce9af8c6abd371b4321e3343
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34852050"
---
| Kaynak | Üst Sınır |Notlar|
| --- | --- |---|
| Otomasyon hesabı (harici zamanlanmış iş) her 30 saniyede gönderilen yeni iş maksimum sayısı |100 |Bu sınır, isabet, bir iş oluşturmak için sonraki istekleri başarısız olur. İstemci bir hata yanıtı alır.|
| En fazla eşzamanlı çalışan işleri Otomasyon hesabı (harici zamanlanmış iş) her zaman aynı örneği sayısı |200 |Bu sınır, isabet, bir iş oluşturmak için sonraki istekleri başarısız olur. İstemci bir hata yanıtı alır.|
| Otomasyon hesabı başına her 30 saniyede aktarılan modülleri sayısı üst sınırı |5 ||
| Bir modül en büyük boyutu |100 MB ||
| İş çalışma zamanı - ücretsiz katmanı |Aylık takvim abonelik başına 500 dakika ||
| Disk alanı korumalı alan izin verilen en fazla miktarını**<sup>1</sup>** |1 GB |Yalnızca Azure sanal uygular|
| En fazla bir korumalı alan verilen bellek miktarını**<sup>1</sup>** |400 MB |Yalnızca Azure sanal uygular|
| Ağ yuva korumalı alan izin verilen en fazla sayısını**<sup>1</sup>** |1000 |Yalnızca Azure sanal uygular|
| Automation hesapları bir abonelikte en fazla sayısı |Sınırsız ||
|En fazla sayıda eşzamanlı işleri tek bir karma Runbook çalışanı üzerinde çalışacak olabilir|50 ||

**<sup>1</sup>**  bir korumalı alan birden çok işlemler tarafından kullanılan paylaşılan bir ortamı, aynı sanal kullanarak işleri korumalı kaynak sınırlamaları tarafından bağlıdır.
