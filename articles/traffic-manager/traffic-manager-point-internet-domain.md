---
title: Bir şirketin Internet etki alanını bir Azure Traffic Manager etki alanı adına yönlendirme | Microsoft Docs
description: Bu makale, şirketinizin etki alanı adını Traffic Manager etki alanı adına yönlendirmenize yardımcı olacaktır.
services: traffic-manager
documentationcenter: ''
author: kumudd
ms.service: traffic-manager
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/11/2016
ms.author: kumud
ms.openlocfilehash: 2b9b718a61d13790de38ea82f9b479e372e9de63
ms.sourcegitcommit: d61faf71620a6a55dda014a665155f2a5dcd3fa2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/04/2019
ms.locfileid: "54052692"
---
# <a name="point-a-company-internet-domain-to-an-azure-traffic-manager-domain"></a>Bir şirketin İnternet etki alanını Azure Traffic Manager etki alanına yönlendirme

Traffic Manager profili oluşturduğunuzda, Azure bu profil için otomatik olarak bir DNS adı atar. DNS bölgenizdeki bir adı kullanmak için Traffic Manager profilinizin etki alanı adıyla eşleşen bir CNAME DNS kaydı oluşturun. Traffic Manager etki alanı adını, Traffic Manager profilinin Yapılandırma sayfasındaki **Genel** bölümünde bulabilirsiniz.

Örneğin, `www.contoso.com` adıyla Traffic Manager `contoso.trafficmanager.net` DNS adına işaret etmek için, aşağıdaki DNS kaynak kaydını oluşturursunuz:

    www.contoso.com IN CNAME contoso.trafficmanager.net

*www.contoso.com* adresi için yapılan tüm trafik istekleri *contoso.trafficmanager.net* adresine yönlendirilir.

> [!IMPORTANT]
> *contoso.com* gibi ikinci düzey bir etki alanını Traffic Manager etki alanına yönlendiremezsiniz. DNS protokolü standartları, ikinci düzey etki alanı adları için CNAME kayıtlarına izin vermez.

## <a name="next-steps"></a>Sonraki adımlar

* [Traffic Manager yönlendirme yöntemleri](traffic-manager-routing-methods.md)
* [Traffic Manager - Bir profili devre dışı bırakma, etkinleştirme veya silme](disable-enable-or-delete-a-profile.md)
* [Traffic Manager - Bir uç noktayı devre dışı bırakma veya etkinleştirme](disable-or-enable-an-endpoint.md)
