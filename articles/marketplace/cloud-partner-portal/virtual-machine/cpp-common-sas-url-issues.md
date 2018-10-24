---
title: Ortak SAS URL'si sorunları ve çözümlerini, Azure Market'te | Microsoft Docs
description: Paylaşılan erişim imzası URI'ler ve olası çözümleri kullanarak geçici olarak sık karşılaşılan sorunları listeler.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: pbutlerm
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 09/27/2018
ms.author: pbutlerm
ms.openlocfilehash: b20b1506dfcd32ea7d5bfca0847393d1652afb78
ms.sourcegitcommit: 17633e545a3d03018d3a218ae6a3e4338a92450d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/22/2018
ms.locfileid: "49639994"
---
# <a name="common-sas-url-issues-and-fixes"></a>Ortak bir SAS URL'si sorunları ve çözümlerini

Aşağıdaki tabloda (Bu, tanımlamak ve çözümünüz için karşıya yüklenen VHD paylaşmak için kullanılan) paylaşılan erişim imzaları ile çalışırken karşılaşılan yaygın sorunlardan bazılarını listeler önerilen çözümleriyle birlikte.

| **Sorunu** | **Hata iletisi** | **Fix** | 
| --------- | ------------------- | ------- | 
| &emsp;  *Görüntüleri kopyalama hatası* |  |  |  |
| "?" SAS URL bulunamadı | `Failure: Copying Images. Not able to download blob using provided SAS Uri.` | Önerilen araçlar kullanarak SAS URL'sini güncelleştirme. |
| SAS URL'si değil, "s" ve "se" parametreleri | `Failure: Copying Images. Not able to download blob using provided SAS Uri.` | Uygun SAS URL'sini güncelleştirme **başlangıç tarihi** ve **bitiş tarihi** değerleri. | 
| "sp rl SAS URL'si değil =" | `Failure: Copying Images. Not able to download blob using provided SAS Uri` | Olarak ayarlanan izinler ile SAS URL'sini güncelleştirme `Read` ve `List`. | 
| SAS URL'si boşluk VHD adı vardır. | `Failure: Copying Images. Not able to download blob using provided SAS Uri.` | Beyaz boşlukları kaldırmak için SAS URL'sini güncelleştirin. |
| SAS URL yetkilendirme hatası | `Failure: Copying Images. Not able to download blob due to authorization error` | Gözden geçirin ve SAS URI biçimi düzeltin. Gerekirse, yeniden oluşturun. |
| SAS URL'si "st" ve "se" parametreler tam tarih-saat belirtimine sahip değil | `Failure: Copying Images. Not able to download blob due to incorrect SAS URL` | SAS URL'si **başlangıç tarihi** ve **bitiş tarihi** parametreleri (`st` ve` se` alt dizeler) tam tarih/saat biçimi gibi olması gerekli `11-02-2017T00:00:00Z`. Kısaltılmış sürümleri, geçerli değildir. (Bazı Azure CLI komutlarında kısaltılmış değerleri varsayılan olarak oluşturabilir.) | 
|  |  |  |

Daha fazla bilgi için [paylaşılan erişim imzaları (SAS) kullanma](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/).