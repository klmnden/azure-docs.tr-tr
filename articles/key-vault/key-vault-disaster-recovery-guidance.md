---
title: Azure Key Vault etkileyen kesinti olması durumunda bir Azure yapmanız gerekenler servis | Microsoft Docs
description: Azure Key Vault etkileyen bir Azure hizmet kesintisi olması durumunda yapmanız gerekenler öğrenin.
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
ms.topic: conceptual
ms.date: 01/07/2017
ms.author: aglick
ms.openlocfilehash: 1e3da7bee0211380b31e1c8ae2f1de45ade8a5f6
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2018
ms.locfileid: "44296848"
---
# <a name="azure-key-vault-availability-and-redundancy"></a>Azure Key Vault kullanılabilirlik ve yedeklilik
Azure Key Vault, birden çok hizmet bileşenlerini ayrı ayrı başarısız olsa bile anahtarlarınızı ve gizli bilgilerinizi, uygulamanız için kullanılabilir olarak kaldıklarından emin olmak için yedekleme katmanı sunar.

Anahtar kasanızın içeriklerini hemen ancak aynı coğrafyadaki bölge içinde ve en az 150 mil uzaklıktaki ikincil bir bölgeye çoğaltılır. Bu, yüksek bir dayanıklılık düzeyi anahtar ve gizli tutar. Bkz: [Azure eşleştirilmiş bölgeleri](https://docs.microsoft.com/azure/best-practices-availability-paired-regions) belge belirli bir bölge çiftleri hakkında ayrıntılı bilgi için.

Anahtar kasası hizmetindeki tek tek bileşenleri başarısız olursa, bölge içindeki diğer bileşenleri isteğiniz işlevlerin düşüşü olmadan olduğundan emin olmak için hizmet adımı. Bu tetiklemek için herhangi bir eylemde bulunmanız gerekmez. Otomatik olarak gerçekleştirilir ve sizin için saydam olacak.

Azure bölgesinin tamamını kullanılamıyor ender olayda, Azure Key Vault bu bölgede, yaptığınız istekleri otomatik olarak yönlendirilir (*yük devretti*) ikincil bir bölgeye. İstekleri birincil bölgeye yeniden kullanılabilir olduğunda geri yönlendirilir (*geri başarısız*) birincil bölgeye. Yine, çünkü bu otomatik olarak gerçekleşir, herhangi bir eylemde bulunmanız gerekmez.

Dikkat edilmesi gereken bazı uyarılar şunlardır:

* Bir bölgeye yük devretmesi durumunda, bu hizmeti yük devretme birkaç dakika sürebilir. Yük devretme işlemi tamamlanana kadar bu süre boyunca gönderilen istekleri başarısız olabilir.
* Bir yük devretme işlemi tamamlandıktan sonra anahtar kasanıza salt okunur modda değil. Bu modda desteklenir istekleri şunlardır:
  * Anahtar kasalarının listesi
  * Anahtar kasalarının özelliklerini alma
  * Gizli anahtarları listeleme
  * Gizli anahtarları alma
  * Anahtarları Listele
  * (Özellikleri) anahtarları alma
  * Şifreleme
  * Şifre Çöz
  * Kaydırma
  * Sarmalamadan çıkarma
  * Doğrulama
  * İmzala
  * Backup
* Bir yük devretme geri başarısız sonra tüm istek türleri (okuma da dahil olmak üzere *ve* yazma istekleri) kullanılabilir.

