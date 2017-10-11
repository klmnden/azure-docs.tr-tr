---
title: "Bir hizmet yapılandırma dosyasında DNS ayarlarını belirtme | Microsoft Docs"
description: "sanal ağ için hizmet yapılandırma dosyası kullanarak özel DNS ayarlarını belirtme"
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: tysonn
ms.assetid: 467a4b99-8691-40b3-b640-e25e49675c71
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/24/2016
ms.author: jdial
ms.openlocfilehash: 0fba2ea06827aff29a7a092933edb8120d668b29
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="specifying-dns-settings-in-a-service-configuration-file"></a>Bir hizmet yapılandırma dosyasında DNS ayarlarını belirtme
## <a name="dns-elements"></a>DNS öğeleri
Hizmet yapılandırma dosyası hizmetini kullanacak olan etki alanı adı sistemi (DNS) sunucuları için IPv4 adresleri listesi olan bir DnsServers öğesi içerebilir. Hizmet yapılandırma dosyası ayarlarında ayarları ağ yapılandırma dosyasında daha önceliklidir. Daha fazla bilgi için bkz: [Azure hizmet yapılandırma şeması (.cscfg dosyası)](https://msdn.microsoft.com/library/azure/ee758710.aspx).

**NetworkConfiguration öğesi**

      <DnsServers>
        <DnsServer name="ID1" IPAddress="IPAddress1" />
        <DnsServer name="ID2" IPAddress="IPAddress2" />
        <DnsServer name="ID3" IPAddress="IPAddress3" />
      </DnsServers>

> [!WARNING]
> **Adı** özniteliğini **DnsServer** öğe yalnızca bir başvuru adı kullanılır. DNS sunucusu için konak adı göstermiyor. Her **DnsServer** öznitelik değeri benzersiz olmalıdır tüm Microsoft Azure aboneliği.
> 
> 

## <a name="see-also"></a>Ayrıca Bkz.
[Azure hizmet yapılandırma şeması (.cscfg)](https://msdn.microsoft.com/library/windowsazure/ee758710)

[Azure Virtual Network yapılandırma şeması](http://go.microsoft.com/fwlink/?LinkId=248093)

[Ağ yapılandırma dosyalarını kullanarak bir sanal ağ yapılandırma](http://go.microsoft.com/fwlink/?LinkId=248094)

[Yönetim Portalı'nda sanal ağ ayarları hakkında](http://go.microsoft.com/fwlink/?LinkId=248092)

