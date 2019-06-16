---
title: Bir sanal ağ yapılandırma dosyasında DNS ayarlarını belirtme | Microsoft Docs
description: Klasik dağıtım modelinde bir sanal ağ yapılandırma dosyası kullanarak bir sanal ağdaki DNS sunucusu ayarlarını değiştirme
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
ms.openlocfilehash: 36f7ed9b02b66718327c1a05a6cf29eedf39e7a5
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60232850"
---
# <a name="specifying-dns-settings-in-a-virtual-network-configuration-file"></a>Bir sanal ağ yapılandırma dosyasında DNS ayarlarını belirtme
Bir ağ yapılandırma dosyası, etki alanı adı sistemi (DNS) ayarlarını belirlemek için kullanabileceğiniz iki öğe vardır: **DnsServers** ve **DnsServerRef**. DNS sunucularının bir listesini, IP adreslerini belirterek ekleyin ve başvuru adlarına **DnsServers** öğesi. Daha sonra kullanabileceğiniz bir **DnsServerRef** hangi DNS sunucusu girdileri DnsServers öğeden sanal ağınız içinde farklı ağ siteleri için kullanılan belirtmek için öğesi.

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

Bu makale, klasik dağıtım modelini kapsamaktadır.

Ağ yapılandırma dosyasını aşağıdaki öğeleri içerebilir. Her öğenin title öğesi değer ayarları hakkında ek bilgi sağlayan bir sayfa bağlantılıdır.

> [!IMPORTANT]
> Ağ yapılandırma dosyasını yapılandırma hakkında daha fazla bilgi için bkz: [bir ağ yapılandırma dosyası kullanarak bir sanal ağ yapılandırma](virtual-networks-using-network-configuration-file.md). Ağ yapılandırma dosyasında yer alan her öğeyle ilgili daha fazla bilgi için bkz. [Azure sanal ağ yapılandırma şeması](https://msdn.microsoft.com/library/azure/jj157100.aspx).
> 
> 

[DNS öğesi](https://go.microsoft.com/fwlink/?LinkId=248093)

    <Dns>
      <DnsServers>
        <DnsServer name="ID1" IPAddress="IPAddress1" />
        <DnsServer name="ID2" IPAddress="IPAddress2" />
        <DnsServer name="ID3" IPAddress="IPAddress3" />
      </DnsServers>
    </Dns>

> [!WARNING]
> **Adı** özniteliğini **DnsServer** öğesi, yalnızca başvuru olarak kullanılır **DnsServerRef** öğesi. DNS sunucusu ana bilgisayar adını temsil etmiyor. Her **DnsServer** öznitelik değeri benzersiz olmalıdır tüm Microsoft Azure aboneliği
> 
> 

[Sanal ağ siteleri öğesi](https://go.microsoft.com/fwlink/?LinkId=248093)

    <DnsServersRef>
      <DnsServerRef name="ID1" />
      <DnsServerRef name="ID2" />
      <DnsServerRef name="ID3" />
    </DnsServersRef>

> [!NOTE]
> Sanal ağ siteleri öğesi için bu ayarı belirtmek için daha önce DNS öğesinde tanımlanmış olması gerekir. DnsServerRef *adı* sanal ağ sitelerinde öğesi DnsServer için DNS öğesinde belirtilen bir adı değerine başvurmalıdır *adı*.
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
* Anlamak [Azure Virtual Network yapılandırma şeması](https://go.microsoft.com/fwlink/?LinkId=248093).
* Anlamak [Azure hizmet yapılandırma şeması](https://msdn.microsoft.com/library/windowsazure/ee758710).
* [Ağ yapılandırma dosyalarını kullanarak sanal ağ yapılandırma](virtual-networks-using-network-configuration-file.md).

