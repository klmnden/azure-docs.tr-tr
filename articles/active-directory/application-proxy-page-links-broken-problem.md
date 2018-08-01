---
title: Sayfadaki bağlantıları için bir uygulama proxy'si uygulaması çalışmıyor | Microsoft Docs
description: Azure AD ile tümleştirilmiş uygulama Proxy uygulamaları bozuk bağlantıları ile ilgili sorunları giderme
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
ms.topic: conceptual
ms.date: 05/18/2018
ms.author: barbkess
ms.reviewer: asteen
ms.openlocfilehash: 09fa527f8a1e03d568dec67369651bbcee2a26f2
ms.sourcegitcommit: f86e5d5b6cb5157f7bde6f4308a332bfff73ca0f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2018
ms.locfileid: "39365830"
---
# <a name="links-on-the-page-dont-work-for-an-application-proxy-application"></a>Bir uygulama proxy'si uygulaması için çalışma sayfasındaki bağlantılar çalışmıyor

Bu makalede, Azure Active Directory Uygulama proxy'si uygulaması bağlantıları düzgün çalışmaz neden gidermenize yardımcı olur.

## <a name="overview"></a>Genel Bakış 
Uygulama proxy'si uygulaması yayımladıktan sonra çalışan uygulamada varsayılan olarak yalnızca hedeflere yayımlanan kök URL'si içinde yer alan bağlantıları bağlantılardır. Uygulamaların içindeki bağlantılar çalışmayan, uygulama için dahili URL'yi büyük olasılıkla uygulamadaki bağlantıların tüm hedefler dahil değildir.

**Bu neden gerçekleşir?** Bir uygulamada bir bağlantıya tıkladığınızda, uygulama ara sunucusu URL'si aynı uygulama içinde ya da bir iç URL veya harici olarak kullanılabilir bir URL olarak çözmeye çalışır. Bağlantı noktalarını aynı uygulama içinde bir iç URL için değil ya da bu kadar demeti ait ve bulunamadı bir hataya neden.

## <a name="ways-you-can-resolve-broken-links"></a>Bozuk bağlantılar, çözme yollarına

Bu sorunu çözmek için üç yolu vardır. Aşağıdaki seçenek artan karmaşıklık listelenir.

1.  Uygulama için tüm bağlantıları içeren bir kök İç URL olduğundan emin olun. Bu, aynı uygulama içinde yayımlanan içerik olarak çözülmesi tüm bağlantılara izin verir.

    İç URL değiştirme, ancak kullanıcıların giriş sayfası değiştirmek istemiyorsanız, giriş sayfası URL'si, önceden yayımlanmış dahili URL'yi değiştirin. Bu, "Azure Active Directory" - giderek yapılabilir&gt; uygulama kayıtları -&gt; - uygulamayı seçin&gt; özellikleri. Bu özellikler sekmesinde, "Giriş sayfasına istenen giriş sayfası olacak şekilde ayarlayabildiğiniz URL", alan görürsünüz.

2.  Uygulamalarınızı tam etki alanı adlarını (FQDN) kullanıyorsanız, [özel etki alanları](manage-apps/application-proxy-configure-custom-domain.md) uygulamalarınızı yayımlamak için. Bu özellik hem dahili olarak kullanılan ve dışarıdan aynı URL'yi sağlar.

    Bu seçenek, iç URL uygulamaya içindeki bağlantılar da harici olarak tanınır olduğundan uygulamanız bağlantıları uygulama proxy'si aracılığıyla harici olarak erişilebilen olmasını sağlar. Yayımlanan bir uygulamaya ait tüm bağlantıları yine de gerekir. Ancak, bu seçenekle bağlantıları aynı uygulamaya ait gerekmez ve birden çok uygulamalara ait olabilir.

3.  Bu seçeneklerin ikisi de uygulanabilir ise, URL çevirisi/yeniden yazma gerçekleştiren yeni bir özellik önizleyebilirsiniz. Bu özellik ile iç URL'ler veya mevcut uygulamalarınızı HTML gövdesinde bağlantıları çevrilmiş veya "Eşlenmiş, yayımlanan dış uygulama ara sunucusu URL'lerini". Bu çeviri yalnızca, CSS ve HTML bağlantılar üzerinde çalışır ve JS ile bağlantınız oluşturulursa yardımcı olmaz. 

Sonuç olarak, kullanarak öneririz [özel etki alanları](manage-apps/application-proxy-configure-custom-domain.md) mümkünse çözüm. Önizlemeye katılmak istemiyorsanız, e-posta <aadapfeedback@microsoft.com> applicationId(s) ile.

## <a name="next-steps"></a>Sonraki adımlar
[Mevcut şirket içi proxy sunucuları ile çalışma](manage-apps/application-proxy-configure-connectors-with-proxy-servers.md)

