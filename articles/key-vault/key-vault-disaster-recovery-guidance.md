---
title: Azure Key Vault - Azure Key Vault etkileyen kesinti olması durumunda bir Azure yapmanız gerekenler servis | Microsoft Docs
description: Azure Key Vault etkileyen bir Azure hizmet kesintisi olması durumunda yapmanız gerekenler öğrenin.
services: key-vault
author: barclayn
manager: barbkess
editor: ''
ms.service: key-vault
ms.topic: conceptual
ms.date: 05/24/2019
ms.author: barclayn
ms.openlocfilehash: dba1fe91a635f467f4a3aeeaa048897065822869
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66236650"
---
# <a name="azure-key-vault-availability-and-redundancy"></a>Azure Key Vault kullanılabilirlik ve yedeklilik

Azure Key Vault, birden çok hizmet bileşenlerini ayrı ayrı başarısız olsa bile anahtarlarınızı ve gizli bilgilerinizi, uygulamanız için kullanılabilir olarak kaldıklarından emin olmak için yedekleme katmanı sunar.

Anahtar kasanızın içeriklerini hemen ancak aynı coğrafyadaki bölge içinde ve en az 150 mil uzaklıktaki ikincil bir bölgeye çoğaltılır. Bu, yüksek bir dayanıklılık düzeyi anahtar ve gizli tutar. Bkz: [Azure eşleştirilmiş bölgeleri](https://docs.microsoft.com/azure/best-practices-availability-paired-regions) belge belirli bir bölge çiftleri hakkında ayrıntılı bilgi için.

Anahtar kasası hizmetindeki tek tek bileşenleri başarısız olursa, bölge içindeki diğer bileşenleri isteğiniz işlevlerin düşüşü olmadan olduğundan emin olmak için hizmet adımı. Bu tetiklemek için herhangi bir eylemde bulunmanız gerekmez. Otomatik olarak gerçekleştirilir ve sizin için saydam olacak.

Azure bölgesinin tamamını kullanılamıyor ender olayda, Azure Key Vault bu bölgede, yaptığınız istekleri otomatik olarak yönlendirilir (*yük devretti*) ikincil bir bölgeye. İstekleri birincil bölgeye yeniden kullanılabilir olduğunda geri yönlendirilir (*geri başarısız*) birincil bölgeye. Yine, çünkü bu otomatik olarak gerçekleşir, herhangi bir eylemde bulunmanız gerekmez.

Bu yüksek kullanılabilirlik tasarımı Azure anahtar kasası bakım etkinlikleri için kapalı kalma süresi gerektirir.

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
  * Şifre Çözme
  * Kaydırma
  * Sarmalamadan çıkarma
  * Doğrulama
  * oturum
  * Backup
* Bir yük devretme geri başarısız sonra tüm istek türleri (okuma da dahil olmak üzere *ve* yazma istekleri) kullanılabilir.

