---
title: Risk azaltma işlemleri - Microsoft tehdit modelleme aracı - Azure | Microsoft Docs
description: Microsoft tehdit modelleme için sunulan en olası çözümleri vurgulama aracı için azaltma sayfası tehditleri oluşturulur.
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
ms.openlocfilehash: 24aa49fd4ccccda372d2632ef4aee22bd5cb2bf6
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60611310"
---
# <a name="microsoft-threat-modeling-tool-mitigations"></a>Microsoft tehdit modelleme aracı risk azaltma işlemleri

Tehdit modelleme aracı bir çekirdek Microsoft Security Development Lifecycle (SDL) öğesidir. Yazılım mimarları belirleyip görece kolay ve gidermek için uygun maliyetli olması durumunda olası güvenlik sorunlarını erkenden gidermek sağlar. Sonuç olarak, geliştirme toplam maliyeti önemli ölçüde azaltır. Ayrıca, araç unutmayın, güvenlikle ilgili olmayan uzmanlarla tehdit modelleme tüm geliştiriciler için oluşturma ve tehdit modelleri analiz etme hakkında açık yönergeler sağlayarak kolaylaştıracak tasarladığımız.

Ziyaret **[tehdit modelleme aracı](./azure-security-threat-modeling-tool.md)** hemen kullanmaya başlamak için!

## <a name="mitigation-categories"></a>Risk azaltma kategorileri

Tehdit modelleme aracı azaltmaları Web uygulaması güvenlik aşağıdakilerden oluşur çerçevesi göre kategorilere ayrılır:

| Kategori | Açıklama |
| -------- | ----------- |
| **[Denetim ve günlüğe kaydetme](./azure-security-threat-modeling-tool-auditing-and-logging.md)** | Kimin, ne ve ne zaman başladı? Uygulamanız ile ilgili olayları kümelerinin kayıtları için Denetim ve günlüğe kaydetme bakın |
| **[Kimlik doğrulaması](./azure-security-threat-modeling-tool-authentication.md)** | Kimsiniz? Kimlik doğrulaması bir varlık, genellikle bir kullanıcı adı ve parola gibi kimlik bilgileri aracılığıyla başka bir varlığın kimliğini nerede kanıtlar işlemdir |
| **[Yetkilendirme](./azure-security-threat-modeling-tool-authorization.md)** | Ne yapabilirsiniz? Yetkilendirme, kaynakları ve işlemleri için uygulamanızı erişim denetimleri nasıl sağladığını gerçekleşir. |
| **[İletişim güvenliği](./azure-security-threat-modeling-tool-communication-security.md)** | Kim, konuşma? İletişim güvenliği yapılan tüm iletişimin olabildiğince güvenli sağlar |
| **[Yapılandırma Yönetimi](./azure-security-threat-modeling-tool-configuration-management.md)** | Uygulamanızı kullanan olarak çalıştırıyor mu? Hangi veritabanlarının bağlama? Uygulamanızı nasıl yönetilir? Bu ayarları nasıl korumalıdır? Yapılandırma yönetimi, uygulamanız bu işletimsel sorunları nasıl işlediğini için ifade eder. |
| **[Şifreleme](./azure-security-threat-modeling-tool-cryptography.md)** | Nasıl gizli anahtarları (gizlilik) tutuyor musunuz? Yetkisiz sağlama şeklini veri veya kitaplıkları (bütünlüğü)? Nasıl çekirdekler için şifreleme açısından güçlü rastgele değerler sağlanmaktadır? Şifreleme nasıl uygulamanızı gizliliği ve bütünlüğü zorlar için ifade eder. |
| **[Özel durum yönetimi](./azure-security-threat-modeling-tool-exception-management.md)** | Uygulamanızdaki bir yöntem çağrısının başarısız olduğunda, uygulamanızın ne yapar? Ne kadar açığa? Son kullanıcılara kolay hata bilgilerini döndürmek? Değerli bir özel durum bilgilerini çağırana geri geçirdiğiniz? Uygulamanızın düzgün biçimde başarısız oluyor? |
| **[Giriş doğrulaması](./azure-security-threat-modeling-tool-input-validation.md)** | Nasıl giriş, uygulamanın aldığı geçerli ve güvenli olduğunu biliyor musunuz? Giriş doğrulaması nasıl uygulamanızı filtreler, scrubs veya ek işlemeden önce giriş reddeder ifade eder. Giriş noktaları girişini sınırlama ve çıkış noktaları çıkışını kodlama göz önünde bulundurun. Veritabanları ve dosya paylaşımları gibi kaynaklardaki verileri güveniyor musunuz? |
| **[Hassas verileri](./azure-security-threat-modeling-tool-sensitive-data.md)** | Uygulamanız, hassas verileri nasıl işliyor? Hassas verileri, uygulamanızın bellek, ağ üzerinden veya kalıcı depolarında korunması gereken tüm verileri nasıl işlediğini için ifade eder. |
| **[Oturum yönetimi](./azure-security-threat-modeling-tool-session-management.md)** | Nasıl uygulamanızı işlemek ve kullanıcı oturumlarını korumak? Bir kullanıcı ile Web uygulamanız arasındaki ilgili etkileşimlerin sırasını oturum başvurur |

Bu, belirlemenize yardımcı olur:

* En yaygın hatalar burada yapılır
* Eyleme dönüştürülebilir en iyileştirmeler nerede

Sonuç olarak, bu kategorilere odak kullanın ve giriş doğrulama, kimlik doğrulama ve yetkilendirme kategoride en yaygın güvenlik sorunlarının biliyorsanız, orada başlayabilmesi güvenlik işinizin önceliğini belirlemeye. Daha fazla bilgi için ziyaret  **[bu patent bağlantı](https://www.google.com/patents/US7818788)**

## <a name="next-steps"></a>Sonraki adımlar

Ziyaret **[tehdit modelleme aracı tehditleri](./azure-security-threat-modeling-tool-threats.md)** aracını kullanan olası tasarım tehditleri oluşturmak için tehdit kategorileri hakkında daha fazla bilgi için.