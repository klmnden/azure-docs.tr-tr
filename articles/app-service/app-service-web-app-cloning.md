---
title: Web uygulaması PowerShell kullanarak kopyalama
description: PowerShell kullanarak yeni Web uygulamaları için Web uygulamalarınızı kopyalama öğrenin.
services: app-service\web
documentationcenter: ''
author: ahmedelnably
manager: stefsch
editor: ''
ms.assetid: f9a5cfa1-fbb0-41e6-95d1-75d457347a35
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/14/2016
ms.author: aelnably
ms.openlocfilehash: 30817a1a6a8079e7a896305ab0b59e48fad4d644
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/18/2018
ms.locfileid: "27867488"
---
# <a name="azure-app-service-app-cloning-using-powershell"></a>Azure App Service uygulaması PowerShell kullanarak kopyalama
Microsoft Azure PowerShell sürüm 1.1.0 sürümle birlikte, yeni bir seçenek eklenmiştir `New-AzureRMWebApp` olanak tanıyan yeni oluşturulan bir uygulamanın farklı bir bölgede ya da aynı bölgede var olan bir Web uygulamasının klonlayın. Bu seçenek uygulamaları birkaç farklı bölgelerdeki hızlı ve kolay bir şekilde dağıtmasını sağlar.

Uygulama kopyalama şu anda yalnızca premium katmanı uygulama hizmeti planları için desteklenir. Yeni özellik aynı sınırlamalar Web Apps yedekleme özelliği kullanan, bkz: [Azure App Service'te bir web uygulaması yedekleme](web-sites-backup.md).

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="cloning-an-existing-app"></a>Mevcut bir uygulamayı kopyalama
Senaryo: Orta Güney ABD bölgesi ve var olan bir web uygulamasında istediğiniz yeni bir web uygulaması Kuzey Orta ABD bölgesindeki içindekilere kopyalanamıyor. İle yeni bir web uygulaması oluşturmak için PowerShell cmdlet Azure Resource Manager sürümü kullanılarak gerçekleştirilebilir `-SourceWebApp` seçeneği.

Kaynak web uygulamasını içeren kaynak grubu adı bilerek, kaynak web uygulamanızın bilgi almak için aşağıdaki PowerShell komutunu kullanabilirsiniz (Bu durumda adlı `source-webapp`):

```PowerShell
$srcapp = Get-AzureRmWebApp -ResourceGroupName SourceAzureResourceGroup -Name source-webapp
```

Yeni bir uygulama hizmeti planı oluşturmak için kullanabileceğiniz `New-AzureRmAppServicePlan` aşağıdaki örnekteki gibi komutu

```PowerShell
New-AzureRmAppServicePlan -Location "South Central US" -ResourceGroupName DestinationAzureResourceGroup -Name NewAppServicePlan -Tier Premium
```

Kullanarak `New-AzureRmWebApp` komutu, Kuzey Orta ABD bölgesinde yeni web uygulaması oluşturma ve bir varolan premium katmanına App Service planı bağlayın. Ayrıca, kaynak web uygulaması aynı kaynak grubunu kullanın veya yeni bir kaynak grubu aşağıdaki komutta gösterildiği gibi tanımlayın:

```PowerShell
$destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp
```

Tüm ilişkili dağıtım yuvası da dahil olmak üzere var olan bir web uygulamasının kopyalamak için kullanmanız gerekir `IncludeSourceWebAppSlots` parametresi. Aşağıdaki PowerShell komutunu bu parametreyle kullanımını gösteren `New-AzureRmWebApp` komutu:

```PowerShell
$destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp -IncludeSourceWebAppSlots
```

Aynı bölge içinde var olan bir web uygulaması kopyalamak için yeni bir kaynak grubu oluşturmanız gerekir ve yeni bir uygulama hizmeti aynı bölgede planlamak ve web uygulaması kopyalamak için aşağıdaki PowerShell komutunu kullanın

```PowerShell
$destapp = New-AzureRmWebApp -ResourceGroupName NewAzureResourceGroup -Name dest-webapp -Location "South Central US" -AppServicePlan NewAppServicePlan -SourceWebApp $srcap
```

## <a name="cloning-an-existing-app-to-an-app-service-environment"></a>Bir uygulama hizmeti ortamı var olan bir uygulamaya kopyalama
Senaryo: Orta Güney ABD bölgesi ve var olan bir web uygulamasında istediğiniz yeni bir web uygulaması bir var olan uygulama hizmeti ortamı (ana) için içeriği kopyalanamıyor.

Kaynak web uygulamasını içeren kaynak grubu adı bilerek, kaynak web uygulamanızın bilgi almak için aşağıdaki PowerShell komutunu kullanabilirsiniz (Bu durumda adlı `source-webapp`):

```PowerShell
$srcapp = Get-AzureRmWebApp -ResourceGroupName SourceAzureResourceGroup -Name source-webapp
```

Ana'nın adını ve ana ait olduğu kaynak grubu adı bilerek, yeni web uygulaması varolan ana içinde aşağıdaki komutta gösterildiği gibi oluşturabilirsiniz:

```PowerShell
$destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -ASEName DestinationASE -ASEResourceGroupName DestinationASEResourceGroupName -SourceWebApp $srcapp
```

`Location` Eski bir nedenden dolayı parametresi gereklidir, ancak uygulama ASE'de oluştururken dikkate alınmaz. 

## <a name="cloning-an-existing-app-slot"></a>Varolan bir uygulaması yuvası kopyalama
Senaryo: Varolan bir Web uygulaması yuvası ya da yeni bir Web uygulamasına veya yeni bir Web uygulaması yuvası kopyalama istiyor. Yeni Web uygulaması, özgün Web uygulaması yuvası ile aynı bölgede ya da farklı bir bölgede olabilir.

Kaynak web uygulamasını içeren kaynak grubu adı bilerek, kaynak web uygulaması yuvanın bilgi almak için aşağıdaki PowerShell komutunu kullanabilirsiniz (Bu durumda adlı `source-webappslot`) Web uygulamasına bağlı `source-webapp`:

```PowerShell
$srcappslot = Get-AzureRmWebAppSlot -ResourceGroupName SourceAzureResourceGroup -Name source-webapp -Slot source-webappslot
```

Aşağıdaki komut, yeni bir web uygulaması için kaynak web uygulamasının bir kopyasını oluşturma gösterilmektedir:

```PowerShell
$destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcappslot
```

## <a name="configuring-traffic-manager-while-cloning-an-app"></a>Bir uygulama kopyalanırken yapılandırma trafik Yöneticisi
Birden çok bölgede web uygulamaları oluşturma ve bu tüm web uygulamaları için trafiği yönlendirmek için Azure Traffic Manager yapılandırma, müşterilerin uygulamaları yüksek oranda kullanılabilir olduğundan emin olmak için önemli bir senaryodur. Var olan bir web uygulamasının kopyalarken, yeni bir trafik Yöneticisi profili ya da mevcut bir her iki web uygulamaları bağlamak için seçeneğiniz vardır. Trafik Yöneticisi'nin Azure Resource Manager sürümü yalnızca desteklenir.

### <a name="creating-a-new-traffic-manager-profile-while-cloning-an-app"></a>Bir uygulama kopyalama sırasında yeni bir Traffic Manager profili oluşturma
Senaryo: Her iki web uygulamaları içeren bir Azure Resource Manager trafik Yöneticisi profili yapılandırırken başka bir bölge için bir web uygulaması kopyalamak istiyorsunuz. Aşağıdaki komut, yeni bir Traffic Manager profili yapılandırırken kaynak web uygulamasının yeni bir web uygulaması için bir kopyasını oluşturma gösterilmektedir:

```PowerShell
$destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "South Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp -TrafficManagerProfileName newTrafficManagerProfile
```

### <a name="adding-new-cloned-web-app-to-an-existing-traffic-manager-profile"></a>Web uygulaması var olan bir Traffic Manager profilini klonlanmış yeni ekleme
Senaryo: Zaten bir Azure Resource Manager trafik Yöneticisi profili varsa ve her iki web uygulamaları uç noktalar olarak eklemek istediğiniz. Önce var olan trafiğini derlemek gereken Bunu yapmak için Yöneticisi profili kimliği. Abonelik kimliği, kaynak grubu adı ve var olan trafik Yöneticisi profil adı gerekir.

```PowerShell
$TMProfileID = "/subscriptions/<Your subscription ID goes here>/resourceGroups/<Your resource group name goes here>/providers/Microsoft.TrafficManagerProfiles/ExistingTrafficManagerProfileName"
```

Trafik Yöneticisi Kimliğine sahip sonra aşağıdaki komutu var olan bir Traffic Manager profilini eklenirken kaynak web uygulamasının yeni bir web uygulaması için bir kopyasını oluşturma gösterilmektedir:

```PowerShell
$destapp = New-AzureRmWebApp -ResourceGroupName <Resource group name> -Name dest-webapp -Location "South Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp -TrafficManagerProfileId $TMProfileID
```

## <a name="current-restrictions"></a>Geçerli kısıtlamaları
Bu özellik şu anda önizlemede değil ve yeni özellikleri zamanla eklenir. Uygulama kopyalama geçerli sürümü bilinen kısıtlamalar şunlardır:

* Otomatik ölçek ayarları klonlanmış değil
* Yedekleme zamanlaması ayarları klonlanmış değil
* VNET ayarlarını klonlanmış değil
* App Insights otomatik olarak hedef web uygulamasında belirlenmedi
* Kolay kimlik doğrulama ayarları klonlanmış değil
* Kudu uzantısı değil kopyalanamıyor
* İpucu kuralları klonlanmış değil
* Veritabanı içeriğini klonlanmış değil
* Farklı ölçek birimi için kopyalama, giden IP adreslerini değiştirir

### <a name="references"></a>Başvurular
* [Web uygulaması kopyalama](app-service-web-app-cloning.md)
* [Azure App Service'in web uygulamasında yedekleyin](web-sites-backup.md)
* [Azure Traffic Manager Önizleme için Azure Resource Manager desteği](../traffic-manager/traffic-manager-powershell-arm.md)
* [App Service Ortamı’na giriş](environment/intro.md)
* [Azure PowerShell’i Azure Resource Manager ile kullanma](../azure-resource-manager/powershell-azure-resource-manager.md)

