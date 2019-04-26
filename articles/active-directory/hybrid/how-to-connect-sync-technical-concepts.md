---
title: 'Azure AD Connect eşitleme: Teknik kavramlar | Microsoft Docs'
description: Azure AD Connect eşitleme teknik kavramlarını açıklar.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
editor: ''
ms.assetid: 731cfeb3-beaf-4d02-aef4-b02a8f99fd11
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
origin.date: 01/15/2018
ms.date: 11/12/2018
ms.component: hybrid
ms.author: v-junlch
ms.openlocfilehash: b8ec4a6100cfbb4419d7e30f4b97589113b88939
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60347587"
---
# <a name="azure-ad-connect-sync-technical-concepts"></a>Azure AD Connect eşitleme: Teknik Kavramlar
Bu makalede konunun bir özetidir [anlama mimarisi](how-to-connect-sync-technical-concepts.md).

Azure AD Connect eşitleme, düz bir senaryonun meta dizin eşitleme platformuna göre oluşturur.
Aşağıdaki bölümlerde meta dizin eşitleme için kavramları tanıtır.
MIIS, ILM ve FIM temel oluşturma, Azure Active Directory Sync Hizmetleri veri kaynaklarına bağlanmak, veri kaynakları, hem de sağlama arasında veri eşitleme ve kimliklerini sağlamasını ileri bir platform sağlar.

![Teknik Kavramlar](./media/how-to-connect-sync-technical-concepts/scenario.png)

Aşağıdaki bölümler, FIM eşitleme hizmeti şu yönlerini hakkında daha fazla ayrıntı sağlar:

- Bağlayıcı
- Öznitelik akışı
- Bağlayıcı alanı
- Meta veri deposu
- Sağlama

## <a name="connector"></a>Bağlayıcı
Bağlı bir dizin ile iletişim kurmak için kullanılan kod modülleri bağlayıcılar (eski adıyla yönetim aracıları (MAs) da bilinir) olarak adlandırılır.

Bu Azure AD Connect eşitleme çalıştıran bilgisayarda yüklü. Bağlayıcılar, aracısız özel aracıların dağıtımı güvenmek yerine uzak sistem protokolleri kullanarak yazma olanağı sağlar. Düşük risk ve dağıtım süreleri, yani Kritik uygulamalar ve sistemlerle özellikle ilgilenirken.

Yukarıdaki resimde, bağlayıcı bağlayıcı alanıyla eşanlamlıdır ancak dış sistemdeki tüm iletişimleri kapsar.

Bağlayıcı sisteme işlevleri dışarı aktarmak için tüm içe sorumludur ve bildirim temelli sağlama veri dönüşümleri özelleştirmek için kullanırken her sunucuya yerel olarak bağlanmak nasıl anlamanıza gerek kalmadan gelen geliştiriciler serbest bırakır.

İçeri ve dışarı aktarmalar yalnızca zamanlanmış, sistem içinde değişiklikler otomatik olarak bağlı veri kaynağına yayılmamasını beri gerçekleşen değişikliklere karşı daha fazla yalıtım için izin verme ortaya çıkar. Ayrıca, geliştiricilerin neredeyse tüm veri kaynağına bağlanmak için kendi bağlayıcılarını oluşturabilir.

## <a name="attribute-flow"></a>Öznitelik akışı
Meta veri bağlayıcı alanları komşu gelen tüm birleştirilmiş kimlikleri birleştirilmiş görünümüdür. Yukarıdaki şekilde öznitelik akışı gelen ve giden akış ok ucu ile çizgilerle gösterilir. Tüm akışlar (gelen veya giden) öznitelik ve öznitelik akışı kopyalama veya verileri bir sistemden diğerine dönüştürme işlemidir.

Eşitleme (tam veya delta) işlemleri çalışmak üzere zamanlanır öznitelik akışı bağlayıcı alanı ve meta veri deposu arasında çift meydana gelir.

Öznitelik akışı yalnızca bu eşitlemeler çalıştırıldığında gerçekleşir. Öznitelik akışları eşitleme kurallarını tanımlanır. Bunlar, gelen (yukarıdaki resimde ISR) veya giden (yukarıdaki resimde OSR) olabilir.

## <a name="connected-system"></a>Bağlı sistem
Bağlı sistem (bağlı dizin olarak da bilinir) Azure AD Connect eşitleme bağlandığı uzak sisteme başvuran ve okuma ve gelen ve giden kimlik verilerini yazma.

## <a name="connector-space"></a>Bağlayıcı alanı
Her bağlı veri kaynağı nesneleri ve öznitelikleri bağlayıcı alanında filtrelenmiş bir alt kümesi olarak temsil edilir.
Bu eşitleme hizmeti uzak sisteme nesneler eşitlenirken başvurun gerek kalmadan yerel olarak çalışmasına izin verir ve etkileşim içeri aktarmalar için sınırlar ve yalnızca dışa aktarır.

Ardından veri kaynağını ve bağlayıcı (delta içeri aktarma) değişikliklerin bir listesini, sağlama yeteneği varsa, alınıp son yoklama döngüsünden bu yana operasyonel verimlilik yalnızca değiştikçe ölçüde artar. Bağlayıcı alanı bağlı veri kaynağı otomatik olarak bağlayıcı zamanlama içeri ve dışarı aktarımların gerektirerek yayılan değişikliklere karşı korunmasını sağlar. Bu ek sigorta konusunda içiniz rahat olsun, test, Önizleme veya İleri güncelleştirmeyi onaylama sırasında verir.

## <a name="metaverse"></a>Meta veri deposu
Meta veri bağlayıcı alanları komşu gelen tüm birleştirilmiş kimlikleri birleştirilmiş görünümüdür.

Kimlikleri birbirine bağlı ve içeri aktarma akışı eşlemeleri aracılığıyla çeşitli öznitelikler için yetki verildiğini merkezi meta veri deposu nesnesi birden fazla sisteminden toplama bilgileri başlar. Bu nesne öznitelik akışı eşlemeleri giden sistemleri bilgileri getirir.

Yetkili bir sistem meta veri deposuna bunları projeleri nesneleri oluşturulur. Tüm bağlantılar kaldırılır hemen sonra meta veri deposu nesnesi silindi.

Meta veri nesneleri doğrudan düzenlenemez. Nesnesindeki tüm veriler, öznitelik akışı katkıda gerekir. Meta veri her bağlayıcı alanına kalıcı Bağlayıcılarla tutar. Bu bağlayıcılar, her bir eşitleme çalıştırmak için yeniden değerlendirme gerektirmez. Bu, Azure AD Connect eşitleme eşleşen uzak nesne her zaman bulunacak yok anlamına gelir. Bu, normalde nesneleri ilişkilendirmek için sorumlu olacak öznitelikler için değişiklikleri önlemek pahalı aracıları gereksinimini ortadan kaldırır.

Yönetilmesi gereken önceden var olan nesneleri olabilecek yeni veri kaynaklarını bulma, Azure AD Connect eşitleme olası adaylar olan bir bağlantı kurmak değerlendirilecek bir birleştirme kuralı olarak adlandırılan bir işlem kullanır.
Bağlantı kurulduktan sonra Bu değerlendirme yinelenmemesini ve normal öznitelik akışı bağlı uzak veri kaynağının meta veri deposu arasında ortaya çıkabilir.

## <a name="provisioning"></a>Sağlama
Bir yetkili kaynak projeleri, yeni bir nesneye yeni bir bağlayıcı alanı nesne meta veri deposu bağlı bir aşağı akış veri kaynağı temsil eden başka bir bağlayıcı oluşturulabilir.

Bu doğal olarak bir bağlantı kurar ve öznitelik akışı ıcmp'ye geçebilirsiniz.

Bir kural, yeni bir bağlayıcı alanı nesne oluşturulması gerektiğini belirler. her sağlama çağrılır. Bu işlem daha yalnızca bağlayıcı alanı içinde yer aldığından verme gerçekleştirilene kadar ancak bu bağlı veri kaynağına taşımaz.

## <a name="additional-resources"></a>Ek Kaynaklar
- [Azure AD Connect eşitleme: Eşitleme seçeneklerini özelleştirme](how-to-connect-sync-whatis.md)
- [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md)

<!--Image references-->
[1]: ./media/active-directory-aadsync-technical-concepts/ic750598.png

