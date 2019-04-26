---
title: Bir şirketin Internet etki alanını bir Azure Traffic Manager etki alanı adına yönlendirme
description: Bu makale, şirketinizin etki alanı adını Traffic Manager etki alanı adına yönlendirmenize yardımcı olacaktır.
services: traffic-manager
author: kumudd
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/11/2016
ms.author: kumud
ms.openlocfilehash: 77a5fbab6ecda910750ab2b8bae987e77607223a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60329709"
---
# <a name="point-a-company-internet-domain-to-an-azure-traffic-manager-domain"></a>Bir şirketin İnternet etki alanını Azure Traffic Manager etki alanına yönlendirme

Traffic Manager profili oluşturduğunuzda, Azure bu profil için otomatik olarak bir DNS adı atar. DNS bölgenizdeki bir adı kullanmak için Traffic Manager profilinizin etki alanı adıyla eşleşen bir CNAME DNS kaydı oluşturun. Traffic Manager etki alanı adını, Traffic Manager profilinin Yapılandırma sayfasındaki **Genel** bölümünde bulabilirsiniz.

Örneğin, `www.contoso.com` adıyla Traffic Manager `contoso.trafficmanager.net` DNS adına işaret etmek için, aşağıdaki DNS kaynak kaydını oluşturursunuz:

    www.contoso.com IN CNAME contoso.trafficmanager.net

Tüm trafik istekleri için *www\.contoso.com* yönlendirilmiş *contoso.trafficmanager.net*.

> [!IMPORTANT]
> *contoso.com* gibi ikinci düzey bir etki alanını Traffic Manager etki alanına yönlendiremezsiniz. DNS protokolü standartları, ikinci düzey etki alanı adları için CNAME kayıtlarına izin vermez.

## <a name="next-steps"></a>Sonraki adımlar

* [Traffic Manager yönlendirme yöntemleri](traffic-manager-routing-methods.md)
* [Traffic Manager - Bir profili devre dışı bırakma, etkinleştirme veya silme](disable-enable-or-delete-a-profile.md)
* [Traffic Manager - Bir uç noktayı devre dışı bırakma veya etkinleştirme](disable-or-enable-an-endpoint.md)
