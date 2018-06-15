---
title: Bir sanal ağ yapılandırma dosyasında DNS ayarlarını belirtme | Microsoft Docs
description: Klasik dağıtım modelinde bir sanal ağ yapılandırma dosyası kullanarak bir sanal ağ içinde DNS sunucusu ayarlarını değiştirme
services: virtual-network
documentationcenter: na
author: genlin
manager: cshepard
editor: tysonn
tags: azure-service-management
ms.assetid: a8905927-92ac-42b5-8c33-8e42c000692c
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/23/2016
ms.author: genli
ms.openlocfilehash: ed7f02d3e389db3bc772c4fcb00a7b3877d60173
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2018
ms.locfileid: "31794533"
---
# <a name="specifying-dns-settings-in-a-virtual-network-configuration-file"></a>Bir sanal ağ yapılandırma dosyasında DNS ayarlarını belirtme
Bir ağ yapılandırma dosyası, etki alanı adı sistemi (DNS) ayarlarını belirlemek için kullanabileceğiniz iki öğe vardır: **DnsServers** ve **DnsServerRef**. IP adreslerini belirterek DNS sunucularının bir listesini ekleyin ve başvuru adlarına **DnsServers** öğesi. Daha sonra kullanabileceğiniz bir **DnsServerRef** öğesi hangi DNS sunucusu girdileri DnsServers öğeden, sanal ağ içindeki farklı ağ siteleri için kullanılan belirtin.

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

Bu makale, klasik dağıtım modelini kapsamaktadır.

Ağ yapılandırma dosyası, aşağıdaki öğeleri içerebilir. Her öğe başlığı öğesi değer ayarları hakkında ek bilgi sağlayan bir sayfasında bağlantılıdır.

> [!IMPORTANT]
> Ağ yapılandırma dosyası yapılandırma hakkında daha fazla bilgi için bkz: [bir ağ yapılandırma dosyası kullanarak bir sanal ağ yapılandırma](virtual-networks-using-network-configuration-file.md). Ağ yapılandırma dosyasında yer alan her öğe hakkında daha fazla bilgi için bkz: [Azure sanal ağ yapılandırma şeması](https://msdn.microsoft.com/library/azure/jj157100.aspx).
> 
> 

[DNS öğesi](http://go.microsoft.com/fwlink/?LinkId=248093)

    <Dns>
      <DnsServers>
        <DnsServer name="ID1" IPAddress="IPAddress1" />
        <DnsServer name="ID2" IPAddress="IPAddress2" />
        <DnsServer name="ID3" IPAddress="IPAddress3" />
      </DnsServers>
    </Dns>

> [!WARNING]
> **Adı** özniteliğini **DnsServer** öğe yalnızca başvuru olarak kullanılır **DnsServerRef** öğesi. DNS sunucusu için konak adı göstermiyor. Her **DnsServer** öznitelik değeri benzersiz olmalıdır tüm Microsoft Azure aboneliği
> 
> 

[Sanal ağ site öğesi](http://go.microsoft.com/fwlink/?LinkId=248093)

    <DnsServersRef>
      <DnsServerRef name="ID1" />
      <DnsServerRef name="ID2" />
      <DnsServerRef name="ID3" />
    </DnsServersRef>

> [!NOTE]
> Sanal ağ site öğesi için bu ayarı belirtmek için daha önce DNS öğesinde tanımlanması gerekir. DnsServerRef *adı* sanal ağ siteleri öğesi DnsServer için DNS öğesinde belirtilen bir ad değeri başvurmalıdır. *adı*.
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
* Anlamak [Azure Virtual Network yapılandırma şeması](http://go.microsoft.com/fwlink/?LinkId=248093).
* Anlamak [Azure hizmet yapılandırma şeması](https://msdn.microsoft.com/library/windowsazure/ee758710).
* [Ağ yapılandırma dosyalarını kullanarak bir sanal ağ yapılandırma](virtual-networks-using-network-configuration-file.md).

