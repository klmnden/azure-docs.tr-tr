---
title: "Sayfadaki bağlantıları için uygulama proxy'si uygulama çalışmıyor | Microsoft Docs"
description: "Azure AD ile tümleşik uygulama proxy'si uygulamaları kırık bağlantılı sorunlarını giderme"
services: active-directory
documentationcenter: 
author: ajamess
manager: mtillman
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: 17f2afb0aaf3b899784a504b77f33a1284f0a232
ms.sourcegitcommit: a0be2dc237d30b7f79914e8adfb85299571374ec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/14/2018
---
# <a name="links-on-the-page-dont-work-for-an-application-proxy-application"></a>Sayfadaki bağlantıları için uygulama proxy'si uygulama çalışmıyor

Bu makalede, Azure Active Directory Uygulama proxy'si uygulamanızın bağlantıları düzgün çalışmıyor neden sorun giderme yardımcı olur.

## <a name="overview"></a>Genel Bakış 
Uygulama proxy'si uygulama yayımlandıktan sonra varsayılan olarak uygulama iş yalnızca bağlantılar hedeflere yayımlanan kök URL'si içinde yer alan bağlantıları vardır. Uygulamaların içindeki bağlantılar çalışmayan, uygulama için iç URL büyük olasılıkla uygulamadaki bağlantıların tüm hedefler dahil değildir.

**Bunun nedeni?** Bir uygulama bir bağlantıya tıklandığında, uygulama proxy'si aynı uygulama içinde ya da bir iç URL veya bir harici URL'si olarak URL çözümlemeye çalışır. Aynı uygulama içinde yer almayan bir iç URL için bağlantı noktaları, değil ya da bu demetlerin ait ve bulunamadı bir hata.

## <a name="ways-you-can-resolve-broken-links"></a>Bağlantılar bozuk çözebilmek için yollar

Bu sorunu çözmek için üç yolu vardır. Aşağıdaki seçenekler karmaşıklık artan sırada listelenir.

1.  Uygulama için tüm bağlantılar içeren bir kök İç URL olduğundan emin olun. Bu, aynı uygulama içinde yayımlanan içerik olarak çözümlenmesi tüm bağlantılar sağlar.

    İç URL'sini değiştirebilirsiniz, ancak kullanıcılar için giriş sayfası değiştirmek istemiyorsanız, giriş sayfası URL'si önceden yayımlanmış dahili URL'yi değiştirin. Bu, "Azure Active Directory'ye" - giderek yapılabilir&gt; uygulama kayıtlar -&gt; - uygulamayı seçin&gt; özellikleri. Bu özellikler sekmesinde, istenen giriş sayfası olarak Ayarla "giriş sayfası URL'si" alanına bakın.

2.  Uygulamalarınızı tam etki alanı adlarını (FQDN) kullanıyorsanız, [özel etki alanlarını](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-custom-domains) uygulamalarınızı yayımlamak için. Bu özellik hem dahili olarak kullanılan ve dışarıdan aynı URL'ye sağlar.

    Bu seçenek iç URL'leri uygulamaya içindeki bağlantılar da dışarıdan tanınır beri uygulamanızı bağlantıları uygulama proxy'si aracılığıyla harici erişilebilir olmasını sağlar. Tüm bağlantılar hala yayımlanan uygulamaya ait olması gerektiğini unutmayın. Ancak, bu seçenek ile bağlantıları aynı uygulamaya ait gerek yoktur ve birden çok uygulamalara ait olabilir.

3.  Bu seçeneklerin ikisi uygun varsa, URL çevirisi/yeniden yazma işlemi yapan yeni bir özellik için Önizleme katılın. Bu seçenek ile iç URL'leri uygulamalarınızı HTML gövdesinde mevcut bağlantıları çevrilmiş veya "Eşlenen, yayımlanan dış uygulama Proxy URL'lerini". Bu yalnızca HTML veya CSS bağlantıları için çalışır ve bu yardımcı bağlantı JS oluşturulursa. 

Sonuç olarak, kullanarak tavsiye [özel etki alanlarını](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-custom-domains) mümkünse çözümü. Önizleme katılmak istiyorsanız, e-posta <aadapfeedback@microsoft.com> applicationId(s) ile.

## <a name="next-steps"></a>Sonraki adımlar
[Mevcut şirket içi proxy sunucuları ile çalışma](application-proxy-working-with-proxy-servers.md)

