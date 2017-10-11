---
title: "-Microsoft tehdit modelleme aracı - Azure tehditler | Microsoft Docs"
description: "Tehdit kategori sayfası Microsoft tehdit modelleme aracı için kullanıma sunulan tüm kategorileri içeren tehditleri oluşturulur."
services: security
documentationcenter: na
author: RodSan
manager: RodSan
editor: RodSan
ms.assetid: na
ms.service: security
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: rodsan
ms.openlocfilehash: 704f9995828866d4d2e4969e3aa922ed1e23c4ea
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2017
---
# <a name="microsoft-threat-modeling-tool-threats"></a>Microsoft tehdit modelleme aracı tehditleri

Tehdit modelleme Aracı'nı bir çekirdek Microsoft Security Development Lifecycle (SDL) öğesidir. Yazılım mimarları tanımlamak ve görece kolay ve çözümlemek için uygun maliyetli olduklarında erken, olası güvenlik sorunlarını azaltmak sağlar. Sonuç olarak, geliştirme toplam maliyetini büyük ölçüde azaltır. Ayrıca, biz uzmanlarla tehdit modelleme tüm geliştiriciler için oluşturma ve tehdit modelleri analiz etme konusunda açık yönergeler sağlayarak kolaylaştırma güvenlikle ilgili olmayan unutmayın, aracı tasarlanmıştır.

> Ziyaret  **[tehdit modelleme aracı](./azure-security-threat-modeling-tool.md)**  bugün başlamak için!

Tehdit modelleme Aracı'nı olanlar gibi bazı soruları yanıtlamanıza yardımcı olur:

* Nasıl bir saldırgan, kimlik doğrulama verileri değiştirebilir miyim?
* Bir saldırgan kullanıcı profili verilerini okuyabiliyorsanız etkisi nedir?
* Kullanıcı profili veritabanına erişimi reddedilirse ne olur?

## <a name="stride-model"></a>STRIDE modeli

Bu tür bir işaret soru formüle daha iyi yardım için Microsoft tehditleri farklı türlerde kategorilere ayırır ve genel güvenlik görüşmeleri basitleştiren STRIDE modeli kullanır.

| Kategori | Açıklama |
| -------- | ----------- |
| **Kimlik sahtekarlığı** | Yasadışı erişme ve kullanıcı adı ve parola gibi başka bir kullanıcının kimlik bilgileri kullanılarak içerir |
| **Oynama** | Kötü amaçlı veri değiştirilmesini içerir. Gibi kalıcı verilerde yapılan yetkisiz değişiklikler Internet gibi açık bir ağ üzerinden iki bilgisayar arasında akıp bir veritabanı ve veri değişikliğinin tutulan örnekler |
| **Geri çevirme** | Aksi takdirde kanıtlamak için herhangi bir şekilde sahip diğer taraflar eylemi gerçekleştirme Reddet kullanıcılarıyla ilişkili — Örneğin, bir kullanıcının yasaklanmış işlemlerini izleme olanağı eksik bir sistemde geçersiz bir işlem gerçekleştirir. İnkar sayaç ret tehditleri sisteme yeteneğini gösterir. Örneğin, bir öğe satın alan bir kullanıcı öğeyi alındığında kaydolmak olabilir. Satıcı imzalı giriş kullanıcı paket aldınız kanıt olarak kullanabilirsiniz |
| **Bilgilerin açığa çıkmasına** | Erişiminiz olması değil kişiler bilgilerin olma içerir — örneğin, kullanıcıların bunlar erişimi verilmiş olmayan bir dosyayı okuma yeteneği veya yetkisiz bir kullanıcının iki bilgisayar arasında Aktarımdaki verileri okuma yeteneği |
| **Hizmet reddi** | Hizmet (DoS) saldırısı reddi Reddet hizmeti geçerli kullanıcılara — Örneğin, yaparak bir Web sunucusu geçici olarak kullanılamıyor veya kullanılamaz. Belirli türde bir yalnızca sistem kullanılabilirliği ve güvenilirliği iyileştirmek için DoS tehditlerine karşı korumanız gerekir |
| **Ayrıcalık yükseltme** | Ayrıcalıksız bir kullanıcı, ayrıcalıklı erişim kazanır ve böylece tehlikeye veya tüm sistemin yok etmek için yeterli erişimi vardır. Ayrıcalık tehditleri ayrıcalıkların güvenilir bir sistem kendisini gerçekten tehlikeli olabilecek bir durumda bir parçası haline gelir ve bu durumlarda, bir saldırganın etkin bir şekilde tüm sistem savunma penetrated içerir |

## <a name="next-steps"></a>Sonraki adımlar

İle devam  **[tehdit modelleme aracı Azaltıcı](./azure-security-threat-modeling-tool-mitigations.md)**  Azure ile bu tehditleri azaltmak farklı yollarını öğrenmek için.