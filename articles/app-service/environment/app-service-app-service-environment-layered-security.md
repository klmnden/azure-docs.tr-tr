---
title: Uygulama hizmeti ortamları ile katmanlı güvenlik mimarisi
description: Uygulama hizmeti ortamları katmanlı güvenlik mimarisiyle uygulama.
services: app-service
documentationcenter: ''
author: stefsch
manager: erikre
editor: ''
ms.assetid: 73ce0213-bd3e-4876-b1ed-5ecad4ad5601
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/30/2016
ms.author: stefsch
ms.openlocfilehash: 29c928c7d81eb3a2532f735be9132b49db5da373
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
---
# <a name="implementing-a-layered-security-architecture-with-app-service-environments"></a>Uygulama hizmeti ortamları katmanlı güvenlik mimarisiyle uygulama
## <a name="overview"></a>Genel Bakış
Uygulama hizmeti ortamları sanal ağınıza dağıtılmış bir yalıtılmış çalışma zamanı ortamı sağlamak için geliştiriciler için her fiziksel uygulama katmanı düzeyleri farklı ağ erişim sağlayan bir katmanlı güvenlik mimarisi oluşturabilir.

API arka uçları genel Internet erişimine karşı gizlemek ve yalnızca Yukarı Akış web uygulamaları tarafından çağrılmasına izin vermek için ortak bir desire var.  [Ağ güvenlik grupları (Nsg'ler)] [ NetworkSecurityGroups] App Service ortamları içeren alt ağlarda API uygulamaları için ortak erişimi kısıtlamak için kullanılabilir.

Aşağıdaki diyagramda, dağıtılan bir uygulama hizmeti ortamı tabanlı Webapı uygulamayla bir örnek mimari gösterilir.  Üç ayrı uygulama hizmeti ortamları üzerinde dağıtılan üç ayrı web app örnekleri aynı Webapı App arka uç çağrıları yapma.

![Kavramsal mimarisi][ConceptualArchitecture] 

Yeşil artı işaretini "apiase" içeren alt ağ üzerinde ağ güvenlik grubu kendisinden iyi çağrıları Yukarı Akış web uygulamalarından gelen çağrıları izin verdiğini gösterir.  Ancak aynı ağ güvenlik grubu, internetten genel gelen trafik için açıkça reddeder. 

Bu makalenin sonraki bölümlerinde "apiase." içeren alt ağ üzerinde ağ güvenlik grubunu yapılandırmak için gereken adımlar anlatılmaktadır

## <a name="determining-the-network-behavior"></a>Ağ davranışı belirleme
Hangi ağ güvenlik kuralları gerekli bilmek için hangi ağ istemcileri API uygulamasını içeren uygulama hizmeti ortamı erişmesine izin ve hangi istemcilerin engellenecek belirlemeniz gerekir.

Bu yana [güvenlik gruplarını (Nsg'ler) ağ] [ NetworkSecurityGroups] alt ağlara uygulanır ve App Service ortamları alt ağa dağıtılırsa, bir NSG kurallarını uygulamak **tüm** uygulamalar bir uygulama hizmeti ortamı üzerinde çalışıyor.  Bir ağ güvenlik grubu "apiase" içeren alt ağa uygulandıktan sonra bu makalede, örnek mimarisi kullanarak, "apiase üzerinde" uygulama hizmeti ortamı çalışan tüm uygulamaların aynı güvenlik kuralları kümesi tarafından korunur. 

* **Yukarı Akış arayanlar giden IP adresini belirlemek:** IP adresini veya adreslerini Yukarı Akış arayanlar nedir?  Bu adresler erişim NSG'de açıkça izin verilmesi gerekir.  Uygulama hizmeti ortamları arasında çağrıları "Internet" çağrıları kabul edilen olduğundan, giden bir IP adresi her üç Yukarı Akış App Service ortamları "apiase" alt ağı için NSG içinde erişimine izin gerekiyor atanmış.   Uygulama hizmeti ortamı'nda çalışan uygulamalar için giden IP adresi belirleme hakkında daha fazla bilgi için bkz: [ağ mimarisi] [ NetworkArchitecture] genel bakış makalesi.
* **Arka uç API uygulaması kendisini çağırması gerekir?**  Bazen gözden kaçan önemli ve zarif noktası burada kendisini çağırmak için arka uç uygulaması gereken bir senaryodur.  Bir uygulama hizmeti ortamı üzerinde bir arka uç API uygulamasının kendisini çağırmak gerekirse, ayrıca davranılır "Internet" çağrısı olarak.  Örnek mimarisinde, bu "apiase" uygulama hizmeti ortamı de giden IP adresinden, erişim gerektirir.

## <a name="setting-up-the-network-security-group"></a>Ağ güvenlik grubu ayarlama
Giden IP adresleri kümesini bilinir sonra bir ağ güvenlik grubu oluşturmak için sonraki adım olacaktır.  Sanal ağlar olarak Klasik sanal ağlar hem Resource Manager tabanlı için ağ güvenlik grupları oluşturulabilir.  Aşağıdaki örnekler, oluşturma ve Powershell kullanarak bir Klasik sanal ağda bir NSG yapılandırma gösterir.

Boş bir NSG bu bölgede oluşturulan şekilde örnek mimarisi, Orta Güney ABD içinde ortamlar bulunur:

    New-AzureNetworkSecurityGroup -Name "RestrictBackendApi" -Location "South Central US" -Label "Only allow web frontend and loopback traffic"

İlk açık bir izin ver kuralı üzerinde makalesinde belirtildiği gibi Azure Yönetim Altyapısı eklenir [gelen trafiği] [ InboundTraffic] App Service ortamları için.

    #Open ports for access by Azure management infrastructure
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW AzureMngmt" -Type Inbound -Priority 100 -Action Allow -SourceAddressPrefix 'INTERNET' -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '454-455' -Protocol TCP

Ardından, iki kural ilk Yukarı Akış uygulama hizmeti ortamı ("fe1ase") gelen HTTP ve HTTPS çağrılarına izin vermek için eklenir.

    #Grant access to requests from the first upstream web front-end
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP fe1ase" -Type Inbound -Priority 200 -Action Allow -SourceAddressPrefix '65.52.xx.xyz'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTPS fe1ase" -Type Inbound -Priority 300 -Action Allow -SourceAddressPrefix '65.52.xx.xyz'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP

Parlatıcı ve ikinci ve üçüncü Yukarı Akış App Service ortamları için ("fe2ase" ve "fe3ase") yineleyin.

    #Grant access to requests from the second upstream web front-end
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP fe2ase" -Type Inbound -Priority 400 -Action Allow -SourceAddressPrefix '191.238.xyz.abc'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTPS fe2ase" -Type Inbound -Priority 500 -Action Allow -SourceAddressPrefix '191.238.xyz.abc'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP

    #Grant access to requests from the third upstream web front-end
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP fe3ase" -Type Inbound -Priority 600 -Action Allow -SourceAddressPrefix '23.98.abc.xyz'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTPS fe3ase" -Type Inbound -Priority 700 -Action Allow -SourceAddressPrefix '23.98.abc.xyz'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP

Son olarak, böylece kendi içine geri çağırabilirsiniz arka uç API'nin uygulama hizmeti ortamı giden IP adresine erişimi verin.

    #Allow apps on the apiase environment to call back into itself
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP apiase" -Type Inbound -Priority 800 -Action Allow -SourceAddressPrefix '70.37.xyz.abc'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTPS apiase" -Type Inbound -Priority 900 -Action Allow -SourceAddressPrefix '70.37.xyz.abc'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP

Her NSG varsayılan olarak Internet'ten gelen erişimi engellemek varsayılan kuralları kümesi olduğundan başka bir ağ güvenlik kuralları gereklidir.

Ağ güvenlik grubu kuralları tam listesini aşağıda gösterilmektedir.  Vurgulanmış, son kural blokları açık olarak erişim izni arayanlar dışındaki tüm arayanlar erişimden nasıl gelen unutmayın.

![NSG yapılandırma][NSGConfiguration] 

Son adım, "apiase" uygulama hizmeti ortamı içeren alt ağı için NSG uygulamaktır.

     #Apply the NSG to the backend API subnet
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityGroupToSubnet -VirtualNetworkName 'yourvnetnamehere' -SubnetName 'API-ASE-Subnet'

Alt ağa uygulanan NSG ile yalnızca üç Yukarı Akış App Service ortamları ve API arka uç içeren uygulama hizmeti ortamı, "apiase" ortamına çağırmak için kullanılabilir.

## <a name="additional-links-and-information"></a>Ek bağlantıları ve bilgileri
Hakkında bilgi [ağ güvenlik grubu](../../virtual-network/security-overview.md).

Anlama [giden IP adreslerini] [ NetworkArchitecture] ve uygulama hizmeti ortamları.

[Ağ bağlantı noktalarını] [ InboundTraffic] App Service ortamları tarafından kullanılır.

[!INCLUDE [app-service-web-try-app-service](../../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[NetworkArchitecture]:  app-service-app-service-environment-network-architecture-overview.md
[InboundTraffic]:  app-service-app-service-environment-control-inbound-traffic.md

<!-- IMAGES -->
[ConceptualArchitecture]: ./media/app-service-app-service-environment-layered-security/ConceptualArchitecture-1.png
[NSGConfiguration]:  ./media/app-service-app-service-environment-layered-security/NSGConfiguration-1.png
