---
title: Bir uygulama proxy'si uygulama için gerekli güvenlik duvarı bağlantı noktalarının nasıl açılacağı | Microsoft Docs
description: Hangi Azure AD düzgün çalışması için uygulama proxy'si açmak için bağlantı noktalarını kullanıma Bul
services: active-directory
documentationcenter: ''
author: ajamess
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: 72acfbd21159e15fe237be6d509cb2c4a2b1bffd
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2018
---
# <a name="how-to-open-the-firewall-ports-required-for-an-application-proxy-application"></a>Bir uygulama proxy'si uygulama için gerekli güvenlik duvarı bağlantı noktalarının nasıl açılacağı

Gerekli bağlantı noktalarını ve her bağlantı noktası işlevi tam listesini görmek için Önkoşullar bölümüne bakın. [uygulama proxy'si belgelerine](manage-apps/application-proxy-enable.md). Uygulama proxy'si yalnızca giden bağlantı noktalarını kullandığını unutmayın.

Tüm gerekli bağlantı noktalarını açın açarak yüklü olup olmadığını da denetleyebilirsiniz [Bağlayıcısı bağlantı noktaları Test aracı](https://aadap-portcheck.connectorporttest.msappproxy.net/) şirket içi ağınızdan. Daha fazla yeşil onay işaretleri büyük esneklik anlamına gelir. 

## <a name="app-proxy-regions"></a>Uygulama proxy'si bölgeleri

Bu bölgeler hangisinin sizin için yeşil olması gereken size bildirmek için bir yol üzerinde çalışıyoruz. Şimdilik, tüm olduklarından emin olun. Orta ABD Ayrıca hangi bölgeyi bağımsız olarak gereklidir.

Aracının doğru sonuçlar verir emin olmak için emin olun:

-   Bir tarayıcı araç Bağlayıcısı'nı yüklediğiniz sunucudan açın.

-   Herhangi bir proxy veya güvenlik duvarları Bağlayıcınızı için uygulanabilir de bu sayfaya uygulandığından emin olun. Internet Explorer'da bu yapılabilir giderek **ayarları**  - &gt; **Internet Seçenekleri**  - &gt; **bağlantıları**  - &gt; **Lan ayarları**. Bu sayfada, alanın "Kullan bir Proxy sunucu için bilgisayarınızı LAN" konusuna bakın. Bu kutuyu seçin ve "Adres" alanına proxy adresi yerleştirin.

## <a name="next-steps"></a>Sonraki adımlar
[Azure AD uygulama proxy'si bağlayıcılar anlama](manage-apps/application-proxy-connectors.md)
