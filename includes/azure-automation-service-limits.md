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
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38756285"
---
| Kaynak | Üst Sınır |Notlar|
| --- | --- |---|
| Otomasyon hesabı (olmayan zamanlanmış işler) her 30 saniyede gönderilen yeni işler en fazla sayısı |100 |Bu sınıra ulaştığınızda, bir iş oluşturmak için sonraki istekler başarısız. İstemci bir hata yanıtı alır.|
| En fazla eş zamanlı çalışan işleri, Otomasyon hesabı (olmayan zamanlanmış işler) her zaman aynı örneği sayısı |200 |Bu sınıra ulaştığınızda, bir iş oluşturmak için sonraki istekler başarısız. İstemci bir hata yanıtı alır.|
| En fazla 30 saniyede bir Otomasyon hesabı başına aktarılabilen bir modül sayısı |5 ||
| Bir modülün en büyük boyutu |100 MB ||
| Proje çalışma zamanı - ücretsiz katmanı |Takvim ayı başına abonelik başına 500 dakika ||
| Korumalı alan izin verilen disk alanı miktarını en fazla**<sup>1</sup>** |1 GB |Yalnızca Azure sanal geçerlidir|
| En fazla bir korumalı alan için verilen bellek miktarını**<sup>1</sup>** |400 MB |Yalnızca Azure sanal geçerlidir|
| Ağ yuvaları korumalı alan izin verilen en fazla sayısı**<sup>1</sup>** |1000 |Yalnızca Azure sanal geçerlidir|
| Otomasyon hesapları bir abonelikte en fazla sayısı |Sınırsız ||
|En fazla sayıda eşzamanlı işler tek bir karma Runbook çalışanı üzerinde çalıştırma olması|50 ||

**<sup>1</sup>**  bir korumalı alan birden fazla iş tarafından kullanılan paylaşılan bir ortamda, aynı sanal kullanarak işleri tarafından korumalı kaynak sınırlamaları bağlıdır.
