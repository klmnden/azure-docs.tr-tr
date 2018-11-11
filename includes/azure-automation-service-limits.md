---
title: include dosyası
description: include dosyası
services: automation
author: georgewallace
ms.service: automation
ms.topic: include
ms.date: 11/07/2018
ms.author: gwallace
ms.custom: include file
ms.openlocfilehash: 70cdd5a9d0482c24dfeb2037ae56b86cd9339fcf
ms.sourcegitcommit: ba4570d778187a975645a45920d1d631139ac36e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/08/2018
ms.locfileid: "51285743"
---
| Kaynak | Üst Sınır |Notlar|
| --- | --- |---|
| Otomasyon hesabı (olmayan zamanlanmış işler) her 30 saniyede gönderilen yeni işler en fazla sayısı |100 |Bu sınıra ulaştığınızda, bir iş oluşturmak için sonraki istekler başarısız. İstemci bir hata yanıtı alır.|
| En fazla eş zamanlı çalışan işleri, Otomasyon hesabı (olmayan zamanlanmış işler) her zaman aynı örneği sayısı |200 |Bu sınıra ulaştığınızda, bir iş oluşturmak için sonraki istekler başarısız. İstemci bir hata yanıtı alır.|
| İş meta verileri 30 günlük toplama süresi için en büyük depolama boyutu. | 10GB (yaklaşık 4 milyon işler)|Bu sınıra ulaştığınızda, bir iş oluşturmak için sonraki istekler başarısız. |
| En fazla 30 saniyede bir Otomasyon hesabı başına aktarılabilen bir modül sayısı |5 ||
| Bir modülün en büyük boyutu |100 MB ||
| Proje çalışma zamanı - ücretsiz katmanı |Takvim ayı başına abonelik başına 500 dakika ||
| Korumalı alan izin verilen disk alanı miktarını en fazla  **<sup>1</sup>** |1 GB |Yalnızca Azure sanal geçerlidir|
| En fazla bir korumalı alan için verilen bellek miktarını  **<sup>1</sup>** |400 MB |Yalnızca Azure sanal geçerlidir|
| Ağ yuvaları korumalı alan izin verilen en fazla sayısı  **<sup>1</sup>** |1000 |Yalnızca Azure sanal geçerlidir|
| Runbook izin verilen en uzun çalışma  **<sup>1</sup>** |3 saat |Yalnızca Azure sanal geçerlidir|
| Otomasyon hesapları bir abonelikte en fazla sayısı |Sınırsız ||
|En fazla sayıda eşzamanlı işler tek bir karma Runbook çalışanı üzerinde çalıştırma olması|50 ||
| En fazla Runbook işi parametreleri boyutu   | 512 kb||
| En fazla Runbook parametreleri   | 50|JSON veya XML dize için bir parametre geçirin ve 50 parametresi sınırına ulaşırsanız runbook ile ayrıştırılamıyor.|
| En fazla Web kancası yükü boyutu |  512 kb|
| İş verileri günde en fazla|30 gün|

**<sup>1</sup>**  bir korumalı alan birden fazla iş tarafından kullanılan paylaşılan bir ortamda, aynı sanal kullanarak işleri tarafından korumalı kaynak sınırlamaları bağlıdır.
