---
title: include dosyası
description: include dosyası
services: automation
author: georgewallace
ms.service: automation
ms.topic: include
ms.date: 12/13/2018
ms.author: gwallace
ms.custom: include file
ms.openlocfilehash: 61f82771e53ac9bb594484b29bb109a03cee674b
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/07/2019
ms.locfileid: "57554199"
---
#### <a name="process-automation"></a>Süreç otomasyonu

| Kaynak | Üst sınır |Notlar|
| --- | --- |---|
| Azure Otomasyonu hesabını (nonscheduled işler) her 30 saniyede gönderilen yeni iş sayısı üst sınırını |100 |Bu sınıra ulaşıldığında, bir iş oluşturmak için sonraki istekler başarısız. İstemci bir hata yanıtı alır.|
| En fazla eş zamanlı çalışan işleri, Otomasyon hesabı (nonscheduled işler) her zaman aynı örneği sayısı |200 |Bu sınıra ulaşıldığında, bir iş oluşturmak için sonraki istekler başarısız. İstemci bir hata yanıtı alır.|
| En fazla depolama boyutunu çalışırken 30 günlük dönem için iş meta verileri | 10 GB (yaklaşık 4 milyon işler)|Bu sınıra ulaşıldığında, bir iş oluşturmak için sonraki istekler başarısız. |
| Otomasyon hesabı başına her 30 saniyede aktarılan modülleri sayısı |5 ||
| Bir modülün en büyük boyutu |100 MB ||
| İş çalıştırma zamanı, ücretsiz katmanı |Takvim ayı başına abonelik başına 500 dakika ||
| Disk alanı korumalı alan izin verilen maksimum miktarı<sup>1</sup> |1 GB |Yalnızca Azure sanal için geçerlidir.|
| Bir korumalı alan için verilen bellek miktarını<sup>1</sup> |400 MB |Yalnızca Azure sanal için geçerlidir.|
| Korumalı alan izin verilen ağ yuva sayısı<sup>1</sup> |1000 |Yalnızca Azure sanal için geçerlidir.|
| Runbook izin verilen en uzun çalışma<sup>1</sup> |3 saat |Yalnızca Azure sanal için geçerlidir.|
| Otomasyon hesapları bir abonelikte en fazla sayısı |Sınırsız ||
|Tek bir karma Runbook çalışanı üzerinde çalıştırılabilir eşzamanlı iş sayısı|50 ||
| En fazla runbook işi parametre boyutu   | 512 kilobitlik||
| En fazla runbook parametreleri   | 50|50 parametresi sınıra ulaştıysanız, JSON veya XML dize için bir parametre geçirin ve runbook ayrıştırılamadı.|
| En fazla Web kancası yükü boyutu |  512 kilobitlik|
| İş verileri en fazla gün sayısı|30 gün|
| PowerShell iş akışı durumu üst sınırı |5 MB| PowerShell iş akışı runbook'ları için geçerli olduğunda denetim noktası oluşturma iş akışı.|

<sup>1</sup>bir korumalı alan birden fazla iş tarafından kullanılan paylaşılan bir ortamdır. Aynı sanal kullanan işleri tarafından korumalı kaynak sınırlamaları bağlıdır.

#### <a name="change-tracking-and-inventory"></a>Değişiklik izleme ve stok

Aşağıdaki tabloda değişiklik izleme için makine başına izlenen öğe sınırları gösterilmektedir.

| **Kaynak** | **Sınırı**| **Notlar** |
|---|---|---|
|Dosya|500||
|Kayıt Defteri|250||
|Windows yazılım|250|Yazılım güncelleştirmeleri dahil değildir.|
|Linux paketleri|1,250||
|Hizmetler|250||
|Program|250||
