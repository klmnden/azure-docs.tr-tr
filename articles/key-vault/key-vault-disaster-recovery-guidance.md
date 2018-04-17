---
title: Azure anahtar kasası etkiler kesintisi hizmet durumunda Azure yapmanız gerekenler | Microsoft Docs
description: Azure anahtar kasası etkileyen bir Azure hizmet kesintisi durumunda yapmanız gerekenler hakkında bilgi edinin.
services: key-vault
documentationcenter: ''
author: adamglick
manager: mbaldwin
editor: ''
ms.assetid: 19a9af63-3032-447b-9d1a-b0125f384edb
ms.service: key-vault
ms.workload: key-vault
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/07/2017
ms.author: aglick
ms.openlocfilehash: 11c2fe5d4b84f99c3b0e303d1abeea73442f57aa
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="azure-key-vault-availability-and-redundancy"></a>Azure anahtar kasası kullanılabilirlik ve artıklık
Azure anahtar kasası hizmetinin bileşenleri tek tek başarısız olsa bile anahtarları ve gizli anahtarları uygulamanıza kullanılabilir olarak kaldıklarından emin olmak için artıklık birden çok katman özellikleri.

Anahtar kasanızı içeriğini, bölge içinde ve ikincil bir bölgeye en az 150 mil hemen ancak aynı Coğrafya içinde çoğaltılır. Bu anahtarları ve gizli anahtarları, yüksek dayanıklılık tutar. Bkz: [Azure bölgeleri eşleştirilmiş](https://docs.microsoft.com/azure/best-practices-availability-paired-regions) belirli bir bölge çiftleri hakkındaki ayrıntılar için belge.

Anahtar kasası hizmetinde bileşenleri tek tek başarısız olursa, bölge içindeki diğer bileşenleri isteğiniz işlevlerin düşüşü olmadan olduğundan emin olmak için hizmet adımı. Bu tetiklemek için herhangi bir eylemde bulunmanız gerekmez. Otomatik olarak gerçekleşir ve size saydam olacaktır.

Ender olayda, tüm bir Azure bölgesi kullanılamıyor, bu bölgedeki Azure anahtar kasası, yaptığınız istekleri otomatik olarak yönlendirilir (*devredilir*) ikincil bir bölgeye. Birincil bölge yeniden kullanılabilir duruma geldiğinde, istekleri geri yönlendirilir (*geri başarısız*) birincil bölge için. Yeniden, bu otomatik olarak gerçekleşir çünkü herhangi bir eylemde bulunmanız gerekmez.

Dikkat edilmesi gereken birkaç uyarılar vardır:

* Bölge yük devretmesi durumunda, yük devretme hizmet birkaç dakika sürebilir. Yük devretme işlemi tamamlanana kadar bu süre boyunca olan istekler başarısız olabilir.
* Bir yük devretme işlemi tamamlandıktan sonra anahtar kasanızı salt okunur modda değil. Bu modda desteklenen istekleri şunlardır:
  * Anahtar kasalarının listesi
  * Anahtar kasalarını özelliklerini alır
  * Gizli anahtarları listeleme
  * Gizli kod dizeleri alma
  * Liste anahtarları
  * Get (özelliklerini) anahtarları
  * Şifreleme
  * Şifre Çöz
  * Kaydırma
  * Kaydırma
  * Doğrulama
  * İmzala
  * Backup
* Bir yük devretme geri başarısız olduktan sonra tüm istek türleri (okuma dahil olmak üzere *ve* yazma isteklerine) kullanılabilir.

