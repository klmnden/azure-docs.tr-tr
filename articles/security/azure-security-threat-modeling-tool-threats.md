---
title: Tehditler - Microsoft tehdit modelleme aracı - Azure | Microsoft Docs
description: Tehdit kategorisi sayfa Microsoft tehdit modelleme aracı için kullanıma sunulan tüm kategorileri içeren tehditleri oluşturulur.
services: security
documentationcenter: na
author: jegeib
manager: jegeib
editor: jegeib
ms.assetid: na
ms.service: security
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: rodsan
ms.openlocfilehash: fd7c5fd929163dc7fcd22fbb045dee0fe3070394
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60611551"
---
# <a name="microsoft-threat-modeling-tool-threats"></a>Microsoft tehdit modelleme aracı tehditler

Tehdit modelleme aracı bir çekirdek Microsoft Security Development Lifecycle (SDL) öğesidir. Yazılım mimarları belirleyip görece kolay ve gidermek için uygun maliyetli olması durumunda olası güvenlik sorunlarını erkenden gidermek sağlar. Sonuç olarak, geliştirme toplam maliyeti önemli ölçüde azaltır. Ayrıca, araç unutmayın, güvenlikle ilgili olmayan uzmanlarla tehdit modelleme tüm geliştiriciler için oluşturma ve tehdit modelleri analiz etme hakkında açık yönergeler sağlayarak kolaylaştıracak tasarladığımız.

> Ziyaret **[tehdit modelleme aracı](./azure-security-threat-modeling-tool.md)** hemen kullanmaya başlamak için!

Tehdit modelleme aracı aşağıdaki olanlar gibi bazı soruları yanıtlamanıza yardımcı olur:

* Nasıl bir saldırgan, kimlik doğrulama verileri değiştirebilir miyim?
* Bir saldırgan, kullanıcı profili verileri okuyabilirse etkisi nedir?
* Kullanıcı profili veritabanına erişimi reddedilirse ne olur?

## <a name="stride-model"></a>STRIDE modeli

Daha iyi yardım için bu tür bir işaret soru formüle, Microsoft, tehditleri farklı kategorilere ayırır ve genel güvenlik görüşmeleri basitleştirir STRIDE modeli kullanır.

| Kategori | Açıklama |
| -------- | ----------- |
| **Kimlik sahtekarlığı** | Her tür erişim ve daha sonra başka bir kullanıcının kullanıcı adı ve parola gibi kimlik doğrulama bilgilerini kullanılmasını içerir |
| **Bozma** | Verilerin kötü amaçlı değiştirilmesini gerektirir. Kalıcı veri gibi yapılan yetkisiz değişiklikler Internet gibi açık bir ağ üzerinden iki bilgisayar arasında akan bir veritabanı ve veri değişikliğinin tutulan örnekler |
| **Red** | Aksi takdirde kanıtlamak için herhangi bir şekilde olan diğer taraflara bir eylem gerçekleştiren Reddet kullanıcılar ile ilişkili — Örneğin, bir kullanıcı yasaklanmış işlemleri izleme olanağı bulunmayan bir sistem üzerinde geçersiz bir işlem gerçekleştirir. İnkar sayacı Red tehditleri sisteme yeteneğini gösterir. Örneğin, bir kullanıcı bir öğeyi satın alındığında öğe için oturum gerekebilir. Satıcı imzalı giriş kullanıcı paket aldınız kanıt kullanabilirsiniz |
| **Bilgilerin açığa çıkması** | Kişiler erişimi olan görmemesi için bilgi açığa içerir — örneğin, onlara erişimi verilen değil bir dosyayı okumak için kullanıcıların açamamaları veya yetkisiz bir kullanıcının iki bilgisayar arasında aktarımda okuma yeteneği |
| **Hizmet reddi** | Geçerli kullanıcı için hizmet (DoS) saldırısı reddi Reddet hizmet — Örneğin, bir Web sunucusu geçici olarak kullanılamıyor veya kullanılamaz hale getirme tarafından. Belirli türlerdeki yalnızca sistem kullanılabilirliği ve güvenilirliği iyileştirmek için DoS tehditlere karşı korumanız gerekir |
| **Ayrıcalık yükseltme** | Ayrıcalıksız bir kullanıcı, ayrıcalıklı erişim kazanır ve böylece tehlikeye veya sistemin tamamını silmek için yeterli erişimi vardır. Tehditleri ayrıcalık yükselmesi güvenilen sisteme, tehlikeli durum gerçekten bir parçası haline gelir ve bu durum, bir saldırganın etkin bir şekilde tüm sistem savunmaları penetrated içerir |

## <a name="next-steps"></a>Sonraki adımlar

Devam **[tehdit modelleme aracı azaltmaları](./azure-security-threat-modeling-tool-mitigations.md)** Azure ile bu tehditleri azaltmak farklı yollarını öğrenin.