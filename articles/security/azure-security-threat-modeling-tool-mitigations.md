---
title: Azaltıcı Etkenler - Microsoft tehdit modelleme aracı - Azure | Microsoft Docs
description: Microsoft tehdit modelleme en gösterilen olası çözümleri vurgulama aracı için Azaltıcı Etkenler sayfası tehditleri oluşturulur.
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
ms.openlocfilehash: 07ef1fd3d81d795c9164741d22b5a689f86bd720
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
ms.locfileid: "23867986"
---
# <a name="microsoft-threat-modeling-tool-mitigations"></a>Microsoft tehdit modelleme aracı Azaltıcı Etkenler

Tehdit modelleme Aracı'nı bir çekirdek Microsoft Security Development Lifecycle (SDL) öğesidir. Yazılım mimarları tanımlamak ve görece kolay ve çözümlemek için uygun maliyetli olduklarında erken, olası güvenlik sorunlarını azaltmak sağlar. Sonuç olarak, geliştirme toplam maliyetini büyük ölçüde azaltır. Ayrıca, biz uzmanlarla tehdit modelleme tüm geliştiriciler için oluşturma ve tehdit modelleri analiz etme konusunda açık yönergeler sağlayarak kolaylaştırma güvenlikle ilgili olmayan unutmayın, aracı tasarlanmıştır.

Ziyaret  **[tehdit modelleme aracı](./azure-security-threat-modeling-tool.md)**  bugün başlamak için!

## <a name="mitigation-categories"></a>Azaltma kategorileri

Tehdit modelleme aracı Azaltıcı Web uygulaması güvenlik aşağıdakilerden oluşur çerçevesi göre ayrılır:

| Kategori | Açıklama |
| -------- | ----------- |
| **[Denetim ve günlük](./azure-security-threat-modeling-tool-auditing-and-logging.md)** | Kimin ne zaman ve ne oldu? Denetim ve günlüğe kaydetme, uygulamanızın güvenlikle ilgili olayları kayıtları nasıl için başvurun |
| **[Kimlik doğrulaması](./azure-security-threat-modeling-tool-authentication.md)** | Kimsin? Kimlik doğrulaması, burada bir varlık genellikle bir kullanıcı adı ve parola gibi kimlik bilgilerini aracılığıyla başka bir varlık kimliğini kanıtlar işlemdir |
| **[Yetkilendirme](./azure-security-threat-modeling-tool-authorization.md)** | Ne yapabilirsiniz? Yetkilendirme, kaynakları ve işlemleri için uygulamanızı erişim denetimleri nasıl sağladığını gerçekleşir. |
| **[İletişim güvenliği](./azure-security-threat-modeling-tool-communication-security.md)** | Kim, konuşma? İletişim güvenliği yapılan tüm iletişimi olabildiğince güvenli sağlar |
| **[Yapılandırma Yönetimi](./azure-security-threat-modeling-tool-configuration-management.md)** | Kullanan uygulamanızı olarak çalışıyor mu? Hangi veritabanlarının bağlamak? Uygulamanızı nasıl yönetilir? Bu ayarları güvenliği nasıl? Uygulamanız bu işletimsel sorunlar nasıl işlediğini için yapılandırma yönetimi başvurur |
| **[Şifreleme](./azure-security-threat-modeling-tool-cryptography.md)** | Nasıl gizli (gizlilik) tutuyor musunuz? Yetkisiz değiştirmeye karşı sağlama şeklini veri veya kitaplıkları (bütünlüğü)? Nasıl oluştururken çekirdeği için şifreleme açısından güçlü rastgele değerler sağlama? Şifreleme nasıl uygulamanızı gizliliği ve bütünlük zorlayan için ifade eder. |
| **[Özel durum yönetimi](./azure-security-threat-modeling-tool-exception-management.md)** | Uygulamanızdaki bir yöntem çağrısı başarısız olduğunda, uygulamanızın ne yapar? Ne kadar ortaya? Son kullanıcılar için kolay hata bilgilerini döndürmek? Değerli özel durum bilgilerini geri çağırana geçirmesini? Uygulamanızın düzgün biçimde başarısız oluyor? |
| **[Giriş doğrulaması](./azure-security-threat-modeling-tool-input-validation.md)** | Nasıl uygulamanızı alır giriş geçerli ve güvenli olduğunu biliyor musunuz? Giriş doğrulaması nasıl uygulamanızı filtreleri, scrubs ya da ek işlemeden önce giriş reddeder anlamına gelir. Giriş noktaları üzerinden giriş sınırlama ve çıkış noktaları üzerinden çıktı kodlama göz önünde bulundurun. Veritabanları ve dosya paylaşımları gibi kaynaklardan veri güveniyor musunuz? |
| **[Hassas veriler](./azure-security-threat-modeling-tool-sensitive-data.md)** | Uygulamanızın hassas verileri nasıl işler? Uygulamanızın ağ üzerinden bellekte veya kalıcı depoları korunması gereken herhangi bir veri nasıl işlediğini için hassas verileri ifade eder |
| **[Oturum yönetimi](./azure-security-threat-modeling-tool-session-management.md)** | Nasıl yapar, uygulamanızın işlemek ve kullanıcı oturumlarını korumak? Bir kullanıcı, Web uygulamanızın arasındaki ilgili etkileşimleri bir dizi bir oturum başvurduğu |

Bu belirlemenize yardımcı olur:

* En yaygın hataları burada yapılır
* En tıklatılabilir geliştirmeleri nerede

Sonuç olarak, bu kategorilere odak kullanın ve giriş doğrulama, kimlik doğrulama ve yetkilendirme kategorilerde en yaygın güvenlik sorunları ortaya biliyorsanız, var. başlayabilmeniz için güvenlik işinizin önceliğini. Daha fazla bilgi için ziyaret  **[bu patent bağlantı](https://www.google.com/patents/US7818788)**

## <a name="next-steps"></a>Sonraki adımlar

Ziyaret  **[tehdit modelleme aracı tehditleri](./azure-security-threat-modeling-tool-threats.md)**  Aracı'nı kullanır olası tasarım tehditleri oluşturmak için tehdit kategorileri hakkında daha fazla bilgi için.