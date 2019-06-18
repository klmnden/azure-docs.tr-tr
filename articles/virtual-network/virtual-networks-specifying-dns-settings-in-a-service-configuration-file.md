---
title: Bir hizmet yapılandırma dosyasında DNS ayarlarını belirtme | Microsoft Docs
description: sanal ağ için hizmet yapılandırma dosyasını kullanarak özel DNS ayarlarını belirtme
services: virtual-network
documentationcenter: na
author: genlin
manager: cshepard
editor: tysonn
ms.assetid: 467a4b99-8691-40b3-b640-e25e49675c71
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/24/2016
ms.author: genli
ms.openlocfilehash: 0ac488a67d8b9debf6539d199395997cf44cf1e4
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60232752"
---
# <a name="specifying-dns-settings-in-a-service-configuration-file"></a>Bir hizmet yapılandırma dosyasında DNS ayarlarını belirtme
## <a name="dns-elements"></a>DNS öğeleri
Hizmet yapılandırma dosyasını hizmetinin kullanacağı bir etki alanı adı sistemi (DNS) sunucuları için IPv4 adreslerinden oluşan bir liste DnsServers öğeyle içerebilir. Hizmet yapılandırma dosyasında ayarları ağ yapılandırma dosyasında ayarları önceliklidir. Daha fazla bilgi için [Azure hizmet yapılandırma şemasına (.cscfg dosyası)](https://msdn.microsoft.com/library/azure/ee758710.aspx).

**NetworkConfiguration öğesi**

      <DnsServers>
        <DnsServer name="ID1" IPAddress="IPAddress1" />
        <DnsServer name="ID2" IPAddress="IPAddress2" />
        <DnsServer name="ID3" IPAddress="IPAddress3" />
      </DnsServers>

> [!WARNING]
> **Adı** özniteliğini **DnsServer** öğesi yalnızca bir başvuru adı kullanılır. DNS sunucusu ana bilgisayar adını temsil etmiyor. Her **DnsServer** öznitelik değeri benzersiz olmalıdır tüm Microsoft Azure aboneliği arasında.
> 
> 

## <a name="see-also"></a>Ayrıca Bkz.
[Azure hizmet yapılandırma şeması (.cscfg)](https://msdn.microsoft.com/library/windowsazure/ee758710)

[Azure Virtual Network yapılandırma şeması](https://go.microsoft.com/fwlink/?LinkId=248093)

[Ağ yapılandırma dosyalarını kullanarak sanal ağ yapılandırma](https://go.microsoft.com/fwlink/?LinkId=248094)

[Yönetim Portalı'nda sanal ağ ayarları hakkında](https://go.microsoft.com/fwlink/?LinkId=248092)

