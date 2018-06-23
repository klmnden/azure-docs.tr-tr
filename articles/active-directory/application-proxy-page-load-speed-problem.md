---
title: Uygulama proxy'si uygulama yüklemek için çok uzun sürdüğü | Microsoft Docs
description: Azure AD uygulama proxy'si sayfa yükleme performans sorunlarını giderme
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
ms.date: 07/11/2017
ms.author: barbkess
ms.reviewer: asteen
ms.openlocfilehash: 26acc620184b51719a2ee55b75bd01966d225b8e
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "36330936"
---
# <a name="an-application-proxy-application-takes-too-long-to-load"></a>Uygulama proxy'si uygulama yüklemek için çok uzun sürüyor

Bu makalede, neden bir Azure AD uygulama proxy'si uygulama yüklemek için uzun bir süre devam edebilir anlamanıza yardımcı olur. Ayrıca, bu sorunu çözmek için yapabileceklerinizi açıklar.

## <a name="overview"></a>Genel Bakış
Uygulamalarınızı çalıştığınız olsa da, bunlar uzun bir gecikme yaşayabilirsiniz. Hızını artırmak için yapabileceğiniz ağ topolojisi tweaks olabilir. Farklı topolojilerinin değerlendirme için bkz: [ağ konuları belge](manage-apps/application-proxy-network-topology.md).

Ağ topolojisi yanı sıra, şu anda performans ayarlaması için daha fazla öneri yok vardır. Uygulama hizmeti genişletir Proxy olarak fiziksel olarak daha yakın olan bir veri merkezi gelebilir. Daha yakın bir yerde konumlandırıldığında gecikmeyle yardımcı olabilir. Azure veri merkezlerinde listesi için bkz: [gecikme test sayfası](http://www.azurespeed.com/Azure/Latency). 

Uygulama proxy'si hizmeti ile veri merkezleri ile bulunabilir [Bağlayıcısı bağlantı noktaları Test aracı](https://aadap-portcheck.connectorporttest.msappproxy.net/). 

## <a name="feedback-on-application-proxy-data-center-locations"></a>Uygulama proxy'si veri merkezi konumlarını geri bildirimi 
Verme henüz uygulama proxy'si içerir, ancak bir harika gecikme geliştirme sizin için sunulmasını Azure veri merkezlerinde olabilir. Veri merkezi bir konuma gönderme aadapfeedback@microsoft.com. Microsoft geri bildirim genişletme planları için kullanır.

Microsoft, gecikme süresini artırmak için ek özellikler üzerinde çalışmaktadır. Bu geliştirmeler kullanılabilir duruma geldiğinde belgelere güncelleştirilir.

## <a name="next-steps"></a>Sonraki adımlar
[Mevcut şirket içi proxy sunucuları ile çalışma](manage-apps/application-proxy-configure-connectors-with-proxy-servers.md)
