---
title: Katmanlı güvenlik mimarisi App Service ortamları - Azure ile
description: App Service ortamları ile katmanlı güvenlik mimarisi uygulama.
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
ms.custom: seodec18
ms.openlocfilehash: 5e25de1ad2042ac978c3698165b9d9baba20e816
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62130696"
---
# <a name="implementing-a-layered-security-architecture-with-app-service-environments"></a>App Service ortamları ile yüksek katmanlı güvenlik mimarisi uygulama
## <a name="overview"></a>Genel Bakış
App Service ortamları dağıtılan bir sanal ağa bir yalıtılmış bir çalışma zamanı ortamı sağlayan olduğundan, geliştiricilere her fiziksel uygulama katmanı için ağ erişim düzeyleri farklı sağlama katmanlı güvenlik mimarisi oluşturabilirsiniz.

Ortak gizliliğinizi korumayı taahhüt eder API arka ucu genel Internet erişiminden gizleme ve yalnızca API'leri Yukarı Akış web uygulamalar tarafından çağrılmasına izin vermektir.  [Ağ güvenlik grupları (Nsg'ler)] [ NetworkSecurityGroups] App Service ortamları içeren alt ağlar, API uygulamaları için ortak erişimi kısıtlamak için kullanılabilir.

Aşağıdaki diyagramda, bir App Service ortamında dağıtılan WebAPI tabanlı uygulama ile bir örnek mimari gösterilmektedir.  Üç ayrı App Service ortamlarında, dağıtılan üç ayrı bir web uygulaması örneği aynı uygulamayı Webapı arka uç çağrı yapmak.

![Kavramsal mimari][ConceptualArchitecture] 

Yeşil artı işaretini "apiase" içeren alt ağ üzerinde ağ güvenlik grubu kendisinden iyi çağrılar gibi Yukarı Akış web uygulamalarından gelen çağrıları izin verdiğini belirtin.  Ancak bir ağ güvenlik grubunu, açıkça genel gelen trafiği Internet'ten erişimi engeller. 

Bu makalenin geri kalanında "apiase." içeren alt ağda ağ güvenlik grubunu yapılandırmak için gereken adımları size gösteriyor

## <a name="determining-the-network-behavior"></a>Ağ davranışı belirleme
Hangi ağ güvenliği kuralları gerekli öğrenmek için hangi ağ istemcileri API uygulamasını içeren App Service ortamı erişmesine izin verilecek ve hangi istemcilerinin engelleneceğini belirlemek gerekir.

Bu yana [ağ güvenlik grupları (Nsg'ler)] [ NetworkSecurityGroups] alt ağlara uygulanır ve App Service ortamları, alt ağına dağıtılır, içerilen bir NSG'de kurallar uygulamak **tüm** uygulamaları bir App Service ortamında çalışır.  "Apiase" içeren alt ağa bir ağ güvenlik grubu uygulandıktan sonra bu makalede örnek mimarisi kullanarak, "apiase üzerinde" App Service ortamı çalıştırılan tüm uygulamalar aynı güvenlik kuralları kümesi tarafından korunur. 

* **Yukarı Akış çağıranlar giden IP adresini belirler:**  IP adresini veya adreslerini Yukarı Akış çağıranlar nedir?  Bu adresler açıkça bir NSG'de erişimine izin gerekir.  App Service ortamları arasında çağrıları, "Internet" aramaları değerlendirilir olduğundan, giden IP adresi her üç Yukarı Akış App Service ortamları için "apiase" alt ağı için NSG içinde erişimine izin gerekiyor atanmış.   Giden IP adresi için bir App Service ortamında çalışan uygulamaları belirleme hakkında daha fazla bilgi için bkz. [ağ mimarisi] [ NetworkArchitecture] genel bakış makalesi.
* **Arka uç API uygulaması kendisini çağırması gerekir?**  Bazen kaçan ve ince noktası arka uç uygulama kendisini çağırmak için gereken yere senaryodur.  Bir App Service ortamında bir arka uç API uygulaması kendisini çağrısı yapması gerekiyorsa, ayrıca değerlendirilir "Internet" çağrısı olarak.  Örnek mimaride, bu "apiase" App Service ortamı de giden IP adresinden erişime izin vermeyi gerektirir.

## <a name="setting-up-the-network-security-group"></a>Ağ güvenlik grubu ayarlama
Giden IP adresleri kümesini bilinir sonra ağ güvenlik grubu oluşturmak için sonraki adım gelir.  Klasik sanal ağlarda yanı sıra, sanal ağlar hem de Resource Manager tabanlı için ağ güvenlik gruplarında oluşturulabilir.  Aşağıdaki örnekler, oluşturma ve Powershell kullanarak bir Klasik sanal ağ üzerinde bir NSG yapılandırma gösterir.

Bu bölgede oluşturulan boş bir NSG için örnek mimarisi, Güney Orta ABD, ortamları bulunur:

    New-AzureNetworkSecurityGroup -Name "RestrictBackendApi" -Location "South Central US" -Label "Only allow web frontend and loopback traffic"

İlk açık bir izin ver kuralı üzerinde makalesinde belirtildiği gibi Azure Yönetim Altyapısı için eklenen [gelen trafiği] [ InboundTraffic] App Service ortamları için.

    #Open ports for access by Azure management infrastructure
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW AzureMngmt" -Type Inbound -Priority 100 -Action Allow -SourceAddressPrefix 'INTERNET' -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '454-455' -Protocol TCP

Ardından, HTTP ve HTTPS çağrılarından ilk Yukarı Akış App Service ortamı ("fe1ase") izin vermek için iki kural eklenir.

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

Son olarak, böylece kendi içine geri çağırabilirsiniz arka uç API'SİNİN App Service ortamı giden IP adresine erişimi verin.

    #Allow apps on the apiase environment to call back into itself
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP apiase" -Type Inbound -Priority 800 -Action Allow -SourceAddressPrefix '70.37.xyz.abc'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTPS apiase" -Type Inbound -Priority 900 -Action Allow -SourceAddressPrefix '70.37.xyz.abc'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP

Her NSG, varsayılan olarak Internet'ten gelen erişimi engellemek varsayılan kuralları kümesi olduğundan başka bir ağ güvenlik kuralları gereklidir.

Ağ güvenlik grubu kurallarında tam listesini aşağıda gösterilmektedir.  Vurgulanır, son kural bloklarını açıkça erişim verilmiş çağıranlar dışındaki tüm çağıranların erişimden nasıl gelen unutmayın.

![NSG yapılandırma][NSGConfiguration] 

Son adım, "apiase" App Service ortamını içeren alt ağa bir NSG uygulamaktır.

     #Apply the NSG to the backend API subnet
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityGroupToSubnet -VirtualNetworkName 'yourvnetnamehere' -SubnetName 'API-ASE-Subnet'

NSG alt ağa uygulanır, "apiase" ortamına çağırmak için yalnızca üç Yukarı Akış App Service ortamları ve API arka uç içeren App Service ortamı izin verilir.

## <a name="additional-links-and-information"></a>Ek bağlantıları ve bilgileri
Hakkında bilgi [ağ güvenlik grupları](../../virtual-network/security-overview.md).

Anlama [giden IP adresleri] [ NetworkArchitecture] ve App Service ortamları.

[Ağ bağlantı noktaları] [ InboundTraffic] App Service ortamları tarafından kullanılır.

[!INCLUDE [app-service-web-try-app-service](../../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[NetworkArchitecture]:  app-service-app-service-environment-network-architecture-overview.md
[InboundTraffic]:  app-service-app-service-environment-control-inbound-traffic.md

<!-- IMAGES -->
[ConceptualArchitecture]: ./media/app-service-app-service-environment-layered-security/ConceptualArchitecture-1.png
[NSGConfiguration]:  ./media/app-service-app-service-environment-layered-security/NSGConfiguration-1.png
