---
title: Uygulama proxy'si uygulamanın yüklenmesi çok uzun sürer | Microsoft Docs
description: Azure AD uygulama ara sunucusu ile sayfa yükü performans sorunlarını giderme
services: active-directory
documentationcenter: ''
author: barbkess
manager: daveba
ms.assetid: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/11/2017
ms.author: barbkess
ms.reviewer: asteen
ms.openlocfilehash: 247b980a58b4a66a88afba91c5acb215e645eecc
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2019
ms.locfileid: "54470416"
---
# <a name="an-application-proxy-application-takes-too-long-to-load"></a>Uygulama proxy'si uygulaması yüklemek için çok uzun sürüyor

Bu makalede, neden Azure AD uygulama proxy'si uygulamasını yüklemek için bir uzun sürebilir anlamanıza yardımcı olur. Ayrıca, bu sorunu çözmek için yapabilecekleriniz açıklar.

## <a name="overview"></a>Genel Bakış
Uygulamalarınızı çalışıyor olsa da, bunlar uzun bir gecikme yaşayabilir. Hızı artırmak için yapabileceğiniz ağ topolojisi tweaks olabilir. Farklı topolojilerinin değerlendirme için bkz: [ağ konuları belge](application-proxy-network-topology.md).

Ağ topolojisi yanı sıra, şu anda daha fazla öneri için performans ayarlama vardır. Uygulama proxy'si hizmeti genişletir olarak, fiziksel olarak yakın bir veri merkezine gelebilir. Daha yakından yakınlık gecikme süresiyle yardımcı olabilir. Azure veri merkezlerinden oluşan bir liste için bkz [gecikme test sayfası](http://www.azurespeed.com/Azure/Latency). 

Uygulama proxy'si hizmeti ile veri merkezlerinden ile bulunabilir [Bağlayıcısı bağlantı noktaları Test aracı](https://aadap-portcheck.connectorporttest.msappproxy.net/). 

## <a name="feedback-on-application-proxy-data-center-locations"></a>Uygulama proxy'si veri merkezi konumlarını hakkında geri bildirim 
Yok ancak uygulama proxy'si içerir, ancak sizin için harika bir gecikme süresi geliştirmeyi sonuçlanabilecek Azure veri merkezleri olabilir. Veri Merkezi konumuna gönderme aadapfeedback@microsoft.com. Microsoft geri bildirim genişletme planları için kullanır.

Microsoft, gecikme süresini iyileştirmek için ek özellikler üzerinde çalışmaktadır. Bu geliştirmeler kullanılabilir hemen sonra belge güncelleştirilecektir.

## <a name="next-steps"></a>Sonraki adımlar
[Mevcut şirket içi proxy sunucuları ile çalışma](application-proxy-configure-connectors-with-proxy-servers.md)
