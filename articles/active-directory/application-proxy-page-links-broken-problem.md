---
title: Sayfadaki bağlantıları için uygulama proxy'si uygulama çalışmıyor | Microsoft Docs
description: Azure AD ile tümleşik uygulama proxy'si uygulamaları kırık bağlantılı sorunlarını giderme
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2018
ms.author: barbkess
ms.reviewer: harshja
ms.openlocfilehash: 3e356beda3e95748cbee64a180612dd183a7ce0e
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34592305"
---
# <a name="links-on-the-page-dont-work-for-an-application-proxy-application"></a>Sayfadaki bağlantıları için uygulama proxy'si uygulama çalışmıyor

Bu makalede, Azure Active Directory Uygulama proxy'si uygulamanızı bağlantıları düzgün çalışmıyor neden sorunları gidermenize yardımcı olur.

## <a name="overview"></a>Genel Bakış 
Uygulama proxy'si uygulama yayımlandıktan sonra varsayılan olarak uygulama iş yalnızca bağlantılar hedeflere yayımlanan kök URL'si içinde yer alan bağlantıları vardır. Uygulamaların içindeki bağlantılar çalışmayan, uygulama için iç URL büyük olasılıkla uygulamadaki bağlantıların tüm hedefler dahil değildir.

**Bunun nedeni?** Bir uygulama bir bağlantıya tıklandığında, uygulama proxy'si aynı uygulama içinde ya da bir iç URL veya bir harici URL'si olarak URL çözümlemeye çalışır. Aynı uygulama içinde yer almayan bir iç URL için bağlantı noktaları, değil ya da bu demetlerin ait ve bulunamadı bir hata.

## <a name="ways-you-can-resolve-broken-links"></a>Bağlantılar bozuk çözebilmek için yollar

Bu sorunu çözmek için üç yolu vardır. Aşağıdaki seçenekler karmaşıklık artan sırada listelenir.

1.  Uygulama için tüm bağlantılar içeren bir kök İç URL olduğundan emin olun. Bu, aynı uygulama içinde yayımlanan içerik olarak çözümlenmesi tüm bağlantılar sağlar.

    İç URL'sini değiştirebilirsiniz, ancak kullanıcılar için giriş sayfası değiştirmek istemiyorsanız, giriş sayfası URL'si önceden yayımlanmış dahili URL'yi değiştirin. Bu, "Azure Active Directory'ye" - giderek yapılabilir&gt; uygulama kayıtlar -&gt; - uygulamayı seçin&gt; özellikleri. Bu özellikler sekmesinde "Giriş sayfası, istenen giriş sayfası olarak ayarla URL" alanına bakın.

2.  Uygulamalarınızı tam etki alanı adlarını (FQDN) kullanıyorsanız, [özel etki alanlarını](manage-apps/application-proxy-configure-custom-domain.md) uygulamalarınızı yayımlamak için. Bu özellik hem dahili olarak kullanılan ve dışarıdan aynı URL'ye sağlar.

    Bu seçenek iç URL'leri uygulamaya içindeki bağlantılar da dışarıdan tanınır beri uygulamanızı bağlantıları uygulama proxy'si aracılığıyla harici erişilebilir olmasını sağlar. Tüm bağlantılar hala yayımlanan uygulamaya ait olmaları gerekir. Ancak, bu seçenek ile bağlantıları aynı uygulamaya ait gerek yoktur ve birden çok uygulamalara ait olabilir.

3.  Bu seçeneklerin ikisi uygun varsa, URL çevirisi/yeniden yazma işlemi yapan yeni bir özellik önizleyebilirsiniz. Bu özellik ile iç URL'leri veya uygulamalarınızı HTML gövdesinde mevcut bağlantılar çevrilmiş, veya "Eşlenen, yayımlanan dış uygulama Proxy URL'lerini". Bu çeviri yalnızca HTML veya CSS, bağlantılar üzerinde çalışır ve bağlantı JS oluşturulursa yardımcı değil. 

Sonuç olarak, kullanarak tavsiye [özel etki alanlarını](manage-apps/application-proxy-configure-custom-domain.md) mümkünse çözümü. Önizleme katılmak istiyorsanız, e-posta <aadapfeedback@microsoft.com> applicationId(s) ile.

## <a name="next-steps"></a>Sonraki adımlar
[Mevcut şirket içi proxy sunucuları ile çalışma](manage-apps/application-proxy-configure-connectors-with-proxy-servers.md)

