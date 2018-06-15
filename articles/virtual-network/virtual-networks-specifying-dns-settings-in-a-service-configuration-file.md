---
title: Bir hizmet yapılandırma dosyasında DNS ayarlarını belirtme | Microsoft Docs
description: sanal ağ için hizmet yapılandırma dosyası kullanarak özel DNS ayarlarını belirtme
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
ms.openlocfilehash: 009206f1e0ba848538ed2c666032a63051d062e4
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2018
ms.locfileid: "31790755"
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

