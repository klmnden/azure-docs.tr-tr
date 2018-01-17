---
title: "Azure AD Connect eşitleme: teknik kavramlar | Microsoft Docs"
description: "Azure AD Connect eşitleme teknik kavramlarını açıklar."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: mtillman
editor: 
ms.assetid: 731cfeb3-beaf-4d02-aef4-b02a8f99fd11
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2018
ms.author: markvi;andkjell
ms.openlocfilehash: 5b757f6d75e5025049a381d3b64576f0b19a7ca8
ms.sourcegitcommit: 384d2ec82214e8af0fc4891f9f840fb7cf89ef59
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/16/2018
---
# <a name="azure-ad-connect-sync-technical-concepts"></a>Azure AD Connect Eşitleme: Teknik Kavramlar
Bu makalede konu özetidir [anlama mimarisi](active-directory-aadconnectsync-technical-concepts.md).

Azure AD Connect eşitleme bir düz meta dizin eşitleme platformu oluşturur.
Aşağıdaki bölümlerde meta dizin eşitleme kavramları tanıtır.
MIIS, ILM ve FIM temel oluşturma, Azure Active Directory Eşitleme Hizmetleri veri kaynaklarına bağlanma, veri kaynakları, aynı zamanda sağlama arasında veri eşitleme ve kimliklerini sağlamasını sonraki platformu sağlar.

![Teknik Kavramlar](./media/active-directory-aadconnectsync-technical-concepts/scenario.png)

Aşağıdaki bölümler, FIM eşitleme hizmeti şu yönlerini hakkında daha fazla ayrıntı sağlar:

* Bağlayıcı
* Öznitelik akışı
* Bağlayıcı alanı
* Meta veri deposu
* Sağlama

## <a name="connector"></a>Bağlayıcı
Bağlı bir dizin ile iletişim kurmak için kullanılan kod modülleri bağlayıcılar (önceki adıyla (MAs) yönetim aracıları olarak da bilinen) adı verilir.

Bu Azure AD Connect eşitleme çalıştıran bilgisayara yüklenir. Bağlayıcılar aracısız özelleştirilmiş aracıları dağıtımını güvenmek yerine uzak sistem protokolleri kullanılarak gruplarıdır olanağı sağlar. Bunun anlamı azalmasına risk ve dağıtım zamanlarını, özellikle Kritik uygulamalar ve sistemler ile ilgilenirken.

Yukarıdaki resimde bağlayıcı bağlayıcı alanıyla çalışır ancak dış sistemdeki tüm iletişim kapsar.

Bağlayıcı için tüm içe sorumludur ve işlevselliği sisteme dışarı aktarma ve bildirim temelli hazırlama veri dönüşümleri özelleştirmek için kullanırken her sistem için yerel olarak bağlanmak nasıl anlamak gerek gelen geliştiriciler boşaltır.

İçeri ve dışarı aktarmalar yalnızca zamanlanmış değişiklikleri otomatik olarak bağlı veri kaynağı değil yayıldığından sistem içinde gerçekleşen değişikliklere karşı daha fazla yalıtımı izin vermeyi oluşur. Ayrıca, geliştiricilerin neredeyse tüm veri kaynağına bağlanmak için kendi bağlayıcılarını oluşturabilir.

## <a name="attribute-flow"></a>Öznitelik akışı
Meta veri bağlayıcı alanları komşu gelen tüm birleştirilmiş kimlikleri birleştirilmiş görünümüdür. Yukarıdaki şekilde, hem gelen hem de giden akış için ok uçları satırıyla tarafından öznitelik akışı belirtilmiştir. Öznitelik akışı kopyalama veya bir sistemden veri dönüştürme işlemidir ve tüm akışlar (gelen veya giden) öznitelik.

Eşitleme (tam veya delta) işlemleri çalışmak üzere zamanlanmış öznitelik akışı meta veri ve bağlayıcı alanı arasında çift yönlü oluşur.

Öznitelik akışı yalnızca bu eşitlemeler çalıştırdığınızda oluşur. Öznitelik akışları eşitleme kurallarında tanımlanır. Bunlar, gelen (yukarıdaki resimde ISR) veya giden (yukarıdaki resimde OSR) olabilir.

## <a name="connected-system"></a>Bağlı sistem
Bağlantılı sistem (diğer adıyla bağlı dizin) uzak sisteme Azure AD Connect eşitleme bağlandığı başvuran ve okuma ve gelen ve giden kimlik verilerini yazma.

## <a name="connector-space"></a>Bağlayıcı alanı
Her bağlı veri kaynağı nesneleri ve öznitelikleri için bağlayıcı alanı filtre uygulanmış bir alt olarak temsil edilir.
Bu eşitleme hizmeti uzak sistem nesneleri eşitlerken başvurun gerek olmadan yerel olarak çalışmasına izin verir ve etkileşim içeri aktarmalar için sınırlar ve yalnızca dışa aktarır.

Ardından veri kaynağı ve bağlayıcı değişikliklerin (delta içeri aktarma) listesini sağlama yeteneği varsa, son Yoklama döngüsü alınıp beri işletimsel verimliliğinin yalnızca değiştikçe önemli ölçüde artırır. Bağlayıcı alanı otomatik olarak bağlayıcı zamanlaması alır ve verir gerektirerek yayılıyor değişiklikleri bağlı veri kaynağından korunmasını sağlar. Bu eklenen sigorta içiniz rahat, test, Önizleme veya sonraki güncelleştirme onaylama sırasında verir.

## <a name="metaverse"></a>Meta veri deposu
Meta veri bağlayıcı alanları komşu gelen tüm birleştirilmiş kimlikleri birleştirilmiş görünümüdür.

Kimlikleri birbirine bağlı ve yetkilisi alma akışı eşlemeleri aracılığıyla çeşitli öznitelikler için atanmış merkezi meta veri deposu nesne birden çok sistemlerden toplama bilgileri başlar. Bu nesne öznitelik akışı eşlemeleri giden sistemlere bilgileri getirir.

Yetkili bir sistem bunları meta veri deposuna projeleri nesneleri oluşturulur. Tüm bağlantılar kaldırılır hemen meta veri deposu Nesne silindi.

Meta veri nesneleri doğrudan düzenlenemez. Tüm verileri nesnedeki öznitelik akışı katkıda bulunan gerekir. Meta veri her bağlayıcı alanı ile kalıcı bağlayıcılar tutar. Bu bağlayıcıların yeniden değerlendirme çalıştırmak için her eşitleme gerektirmez. Başka bir deyişle, Azure AD Connect eşitleme her zaman eşleşen uzak nesnesini bulmak yok. Bu normalde nesneleri ilişkilendirme için sorumlu olacaktır öznitelikleri için değişiklik önlemek maliyetli aracıları gereksinimini önler.

Azure AD Connect eşitleme bir birleşim kural adı verilen bir işlem yönetilmesi gereken önceden var olan nesneleri olabilir yeni veri kaynaklarını bulma, hangi bağlantı kurmak olası adaylar değerlendirmek için kullanır.
Bağlantı kurulduktan sonra bu değerlendirmeyi yeniden değil ve normal öznitelik akışı uzaktan bağlı veri kaynağı ve meta veri deposu gerçekleştirilebilir.

## <a name="provisioning"></a>Sağlama
Bir yetkili kaynağı projeleri, meta veri deposuna yeni bir bağlayıcı alanı nesne yeni bir nesne bir aşağı akış bağlı veri kaynağı temsil eden başka bir bağlayıcı oluşturulabilir.

Bu kendiliğinden bir bağlantı kurar ve öznitelik akışı çift yönlü devam edebilirsiniz.

Bir kural, yeni bir bağlayıcı alanı nesne oluşturulması gerektiğini belirler. her sağlama denir. Bu işlem daha yalnızca bağlayıcı alanı içinde yer aldığından verme gerçekleştirilene kadar ancak, bu bağlı veri kaynağına taşımaz.

## <a name="additional-resources"></a>Ek Kaynaklar
* [Azure AD Connect eşitleme: Eşitleme seçeneklerini özelleştirme](active-directory-aadconnectsync-whatis.md)
* [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](active-directory-aadconnect.md)

<!--Image references-->
[1]: ./media/active-directory-aadsync-technical-concepts/ic750598.png
