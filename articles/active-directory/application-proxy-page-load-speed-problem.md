---
title: "Uygulama proxy'si uygulama yüklemek için çok uzun sürdüğü | Microsoft Docs"
description: "Azure AD uygulama proxy'si sayfa yükleme performans sorunlarını giderme"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: ce462c90746e6af0dc201686557121665b82b93d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="an-application-proxy-application-takes-too-long-to-load"></a>Uygulama proxy'si uygulama yüklemek için çok uzun sürüyor

Bu makalede neden bir Azure AD uygulama proxy'si uygulama yüklemek için bir uzun sürebilir ve bu sorunu çözmek için yapabileceklerinizi anlamanıza yardımcı olur.

## <a name="overview"></a>Genel Bakış
Uygulamalarınızı çalışıyoruz ancak uzun bir gecikme bkz, ağ topolojinizi hızını artırmak için göz önünde bulundurabilirsiniz bazı küçük tweaks olabilir. Farklı topolojilerinin değerlendirme için bkz: [ağ konuları belge](https://docs.microsoft.com/azure/active-directory/application-proxy-network-topology-considerations).

Bu konuları yardımcı olmuyorsa, biz ne yazık ki şu anda yoksa performans ayarlaması için diğer öneriler. Uygulama proxy'si hizmeti için daha yakından daha fazla veri merkezlerine genişletir gibi geliştirilmiş gecikme doğrudan görmek başlayabilir. Azure veri merkezlerinde tam listesini görmek için gördüğünüz [gecikme test sayfası](http://www.azurespeed.com/Azure/Latency). 

Uygulama proxy'si hizmeti ile veri merkezleri ile bulunabilir [Bağlayıcısı bağlantı noktaları Test aracı](https://aadap-portcheck.connectorporttest.msappproxy.net/). 

## <a name="feedback-on-application-proxy-data-center-locations"></a>Uygulama proxy'si veri merkezi konumlarını geri bildirimi 
Uygulama proxy'si olarak henüz içerme ancak harika gecikme geliştirme için sizin için sunulmasını Azure veri merkezlerinde olabilir. Veri merkezi konumda < aadapfeedback@microsoft.com > biz görüşlerinizi biz genişletin planla için kullanabilirsiniz.

Biz şu anda uzun gecikme bkz kiracılar için gecikme geliştirilmesine yardımcı olun bazı ek özellikler üzerinde çalıştığınız ve kullanılabilir bir kez belgeleri paylaşmak emin olun.

## <a name="next-steps"></a>Sonraki adımlar
[Mevcut şirket içi proxy sunucuları ile çalışma](application-proxy-working-with-proxy-servers.md)
