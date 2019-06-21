---
title: Azure portalını güvenli liste URL'leri | Microsoft Docs
description: Azure portalı ve sunduğu hizmetler ile iletişim kurmak için Ara sunucu atlamanın bu URL'leri eklemek
services: azure-portal
keywords: ''
author: kfollis
ms.author: kfollis
ms.date: 06/13/2019
ms.topic: conceptual
ms.service: azure-portal
manager: mtillman
ms.openlocfilehash: 87ec600a2f6c4a560ec7cbb064b561fa76e2b615
ms.sourcegitcommit: 82efacfaffbb051ab6dc73d9fe78c74f96f549c2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67304960"
---
# <a name="safelist-the-azure-portal-urls-on-your-firewall-or-proxy-server"></a>Azure portalını güvenli liste güvenlik duvarı veya Ara sunucunuzun URL'leri

İyi performans ve yerel veya geniş alan ağınızı ve Azure bulut arasında bağlantı için Azure portalına yönelik güvenlik kısıtlamalarını atlamak için şirket içi güvenlik cihazları yapılandırmak URL'leri. Ağ yöneticileri, proxy sunucuları, güvenlik duvarları ve diğer cihazların güvenliğini sağlamaya yardımcı olmak ve kullanıcıların İnternet'e erişme denetim vermek için genellikle dağıtın. Ancak, kullanıcıların korumaya yönelik kurallar bazen engelleyebilir veya ile Azure arasındaki iletişimi de dahil olmak üzere yasal işle ilgili internet trafiğini yavaşlatmaz. Ağınız ve Azure portalı ve sunduğu hizmetler arasında bağlantı iyileştirmek için Azure portalında eklediğiniz öneririz, güvenli liste URL'si.

## <a name="azure-portal-urls-for-proxy-bypass"></a>Azure portalında URL'ler için proxy atlama

Proxy sunucusu veya güvenlik duvarı kısıtlamalarını atlamak için bu uç noktalarına ağ trafiğine izin vermek için aşağıda listelenmiş URL'lere ekleyin:

* *.aadcdn.microsoftonline-p.com
* *.aimon.applicationinsights.io
* *.azure.com
* *.azuredatalakestore.net
* *.azureedge.net
* *.exp.azure.com
* *.ext.azure.com
* *.gfx.ms
* *.account.microsoft.com
* *.hosting.portal.azure.net
* *.marketplaceapi.microsoft.com
* *.microsoftonline.com
* *.msauth.net
* *.msftauth.net
* *.portal.azure.com
* *.portalext.visualstudio.com
* *.sts.microsoft.com
* *.vortex.data.microsoft.com
* *.vscommerce.visualstudio.com
* *.vssps.visualstudio.com
* *.wpc.azureedge.net

> [!NOTE]
> Bu uç noktaları için trafiği, HTTP (80) ve HTTPS (443) için standart TCP bağlantı noktaları kullanır.
>
>
## <a name="next-steps"></a>Sonraki adımlar

* Güvenli liste IP adreslerine ihtiyacınız var? Listesini indirme [Microsoft Azure veri merkezi IP aralıkları](https://www.microsoft.com/download/details.aspx?id=41653).
* Ek URL'leri ve IP adresleri diğer Microsoft Hizmetleri kullanımı için bağlantı. Microsoft 365 Hizmetleri için ağ bağlantısını en iyi duruma getirmek için bkz: [ağınızı ayarlamak için Office 365](/office365/enterprise/set-up-network-for-office-365).
