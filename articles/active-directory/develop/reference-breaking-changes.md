---
title: Azure Active Directory bozucu değişiklikler başvurusu | Microsoft Docs
description: Uygulamanızı etkileyebilecek Azure AD'ye protokolleri için yapılan değişiklikler hakkında bilgi edinin.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 68517c83-1279-4cc7-a7c1-c7ccc3dbe146
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/02/2018
ms.author: celested
ms.reviewer: hirsin
ms.custom: aaddev
ms.openlocfilehash: 266d9ff9ceb4aa71429a9afc3056d23d222a9c7b
ms.sourcegitcommit: 609c85e433150e7c27abd3b373d56ee9cf95179a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/03/2018
ms.locfileid: "48247380"
---
# <a name="whats-new-for-authentication"></a>Kimlik doğrulaması için yenilikler nelerdir? 

>Bu sayfaya güncelleştirmeler hakkında bildirim alın. Eklemeniz yeterlidir [bu URL'yi](https://docs.microsoft.com/api/search/rss?search=%22whats%20new%20for%20authentication%22&locale=en-us) Okuyucu için RSS akışı.

Kimlik doğrulama sistemi değiştirir ve güvenlik ve uyumluluk standartları geliştirmek için sürekli olarak özellikler ekler. İle en son gelişmeleri güncel kalmak için bu makalede, aşağıdakiler hakkında bilgi sağlar:

- En son özellikleri
- Bilinen sorunlar
- Protokol değişiklikleri
- Kullanım dışı işlev

> [!TIP] 
> Bu sayfayı düzenli olarak güncelleştirilen, sık sık ziyaret edin. Aksi belirtilmediği sürece, bu değişiklikler yalnızca yeni kayıtlı uygulamalar için yerinde yerleştirilir.  

## <a name="upcoming-changes"></a>Yaklaşan değişiklikleri

### <a name="authorization-codes-can-no-longer-be-reused"></a>Yetkilendirme kodları yeniden artık kullanılabilir

**Geçerlilik tarihi**: 10 Ekim 2018 **etkilenen uç noktaları**: hem v1.0 ve v2.0 **etkilenen Protokolü**: [kod akışı](v2-oauth2-auth-code-flow.md)

10 Ekim 2018 tarihinden itibaren Azure AD uygulamaları için daha önce kullanılan kimlik doğrulama kodları kabul durdurur. Bu güvenlik değişiklik v1 ve v2 Uç noktalara zorlanmasını sağlar ve Azure AD OAuth belirtimi ayarlarına uygun olarak çıkarmak yardımcı olur.

Uygulamanız için birden fazla kaynak belirteçlerini almak için yetkilendirme kodları yeniden kullanır, bir yenileme belirteci almak için kodu kullanın ve ardından diğer kaynaklar için ek belirteçlerini almak için yenileme belirtecini kullanmak öneririz. Yetkilendirme kodları yalnızca bir kez kullanılabilir, ancak yenileme belirteçleri birden fazla kaynak arasında birden çok kez kullanılabilir. Kimlik doğrulaması kodu OAuth kod akışı sırasında yeniden başlatmayı deneyen herhangi yeni bir uygulama bir invalid_grant hata alırsınız.

Yenileme belirteçleri hakkında daha fazla bilgi için bkz: [erişim belirteçlerini yenileme](v1-protocols-oauth-code.md#refreshing-the-access-tokens).

> [!NOTE]
> Bu özelliği olası, mevcut uygulamaları olarak az sayıda uygulamaları ayırmak için çaba, bu gereksinimin bir özel durum verildi.  10'dan fazla oturum açma bilgileri ile herhangi bir uygulama üzerinde yararlanmayı bu modeli temel bağlı olan bir gün olarak kabul edildi.  

## <a name="may-2018"></a>Mayıs 2018

### <a name="id-tokens-cannot-be-used-for-the-obo-flow"></a>OBO akışı kimlik belirteçlerini kullanılamaz

**Tarih**: 1 Mayıs 2018'den **etkilenen uç noktaları**: hem v1.0 ve v2.0 **etkilenen protokolleri**: örtük akış ve [OBO akış](v1-oauth2-on-behalf-of-flow.md)

1 Mayıs 2018'den sonra id_tokens bir OBO akışında onay olarak yeni uygulamalar için kullanılamaz. Erişim belirteçleri, bunun yerine, API'leri, hatta bir istemci ve orta katman aynı uygulamanın arasında güvenli hale getirmek için de kullanılmalıdır. 1 Mayıs 2018'den iş ve bir erişim belirteci için id_tokens alışverişi yapmak devam etmeden önce kayıtlı uygulamalar; Ancak, bu en iyi uygulama olarak kabul edilmez.

Bu değişikliği geçici olarak çözmek için aşağıdakileri yapabilirsiniz:

1. Web API'si orta katman uygulamanız için bir veya daha fazla kapsamı ile oluşturun. Bu, daha ayrıntılı denetim ve güvenlik olanak tanır.
1. Uygulama bildiriminde içinde [Azure portalında](https://portal.azure.com) veya [uygulama kayıt portalı](https://apps.dev.microsoft.com), örtük akış aracılığıyla erişim belirteçlerini vermek için uygulama izin verildiğinden emin olun. Bu aracılığıyla denetlenir `oauth2AllowImplicitFlow` anahtarı.
1. İstemci uygulamanız aracılığıyla bir id_token istediğinde `response_type=id_token`, ayrıca bir erişim belirteci isteği (`response_type=token`) Web API'si, yukarıda oluşturduğunuz için. Bu nedenle, v2.0 uç noktası kullanırken `scope` parametresi benzer şekilde görünmelidir `api://GUID/SCOPE`. V1.0 uç noktasında `resource` parametresi URI Web API'sini uygulama olmalıdır.
1. Orta katman id_token yerine bu erişim belirtecini geçirin.  
