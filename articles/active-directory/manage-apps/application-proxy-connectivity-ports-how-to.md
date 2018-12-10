---
title: Bir uygulama proxy'si uygulaması için gerekli güvenlik duvarı bağlantı noktalarının nasıl açılacağı | Microsoft Docs
description: Hangi Azure AD uygulama düzgün çalışması proxy'si için açmak için bağlantı noktalarını kullanıma Bul
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
ms.date: 05/21/2018
ms.author: barbkess
ms.reviewer: asteen
ms.openlocfilehash: bd3c0d2dffb2f3ed4569d115c21524377074bed3
ms.sourcegitcommit: 78ec955e8cdbfa01b0fa9bdd99659b3f64932bba
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/10/2018
ms.locfileid: "53136639"
---
# <a name="how-to-open-the-firewall-ports-required-for-an-application-proxy-application"></a>Bir uygulama proxy'si uygulaması için gereken güvenlik duvarı bağlantı noktaları açma

Gerekli bağlantı noktaları ve her bağlantı noktası işlevi tam listesini görmek için Önkoşullar bölümüne bakın. [uygulama proxy'si belgeleri](application-proxy-add-on-premises-application.md). Uygulama proxy'si yalnızca giden bağlantı noktaları kullandığına dikkat edin.

Tüm gerekli bağlantı noktalarını açmak açarak sahip olup olmadığını da denetleyebilirsiniz [Bağlayıcısı bağlantı noktaları Test aracı](https://aadap-portcheck.connectorporttest.msappproxy.net/) şirket içi ağınızdan. Daha fazla yeşil onay işaretleri koyacağız büyük dayanıklılık anlamına gelir. 

## <a name="app-proxy-regions"></a>Uygulama Ara sunucusu bölgeleri

Bu bölgeler hangisinin sizin için yeşil olması gereken bildirmek için bir yol üzerinde çalışıyoruz. Şimdilik, tüm olduklarından emin olun. Orta ABD bakılmaksızın hangi bölgeyi de gereklidir.

Aracı size doğru sonuçları emin olmak için emin olun:

-   Bir tarayıcıdan aracı Bağlayıcısı'nı yüklediğiniz sunucunun açın.

-   Herhangi bir proxy veya güvenlik duvarları, bağlayıcıya geçerli de bu sayfaya uygulandığından emin olun. Internet Explorer'da bu yapılabilir giderek **ayarları**  - &gt; **Internet Seçenekleri**  - &gt; **bağlantıları**  - &gt; **Lan ayarları**. Bu sayfada, alanın "Kullanımı bir Proxy sunucusu için bilgisayarınızı LAN" görürsünüz. Bu kutuyu seçin ve "Address" alanına proxy adresi yerleştirin.

## <a name="next-steps"></a>Sonraki adımlar
[Azure AD uygulama ara sunucusu bağlayıcıları anlama](application-proxy-connectors.md)
