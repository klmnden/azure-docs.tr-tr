---
title: PowerShell - Azure App Service ile uygulama kopyalama
description: PowerShell kullanarak yeni Web Apps için Web Apps kopyalama hakkında bilgi edinin.
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
ms.custom: seodec18
ms.openlocfilehash: 87bae4db64c0a22790b7f52f919601f82aa548df
ms.sourcegitcommit: 1c1f258c6f32d6280677f899c4bb90b73eac3f2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2018
ms.locfileid: "53261876"
---
# <a name="azure-app-service-app-cloning-using-powershell"></a>Azure App Service uygulaması PowerShell kullanarak kopyalama
Microsoft Azure PowerShell sürüm 1.1.0 sürümünde yeni bir seçenek eklendi `New-AzureRMWebApp` olanak tanıyan yeni oluşturulan bir uygulamanın farklı bir bölgede ya da aynı bölgede bulunan mevcut bir Web uygulamasını kopyalayın. Bu seçenek, müşterilerin uygulama sayısı farklı bölgeler arasında hızlı ve kolay bir şekilde dağıtmasına olanak sağlar.

Uygulama kopyalama şu anda yalnızca premium katmanı app service planları için desteklenir. Yeni özellik, Web Apps yedeklemesini özelliği olarak onunla aynı sınırlamalara kullanır, bkz: [Azure App Service'te bir web uygulamasını yedekleme](web-sites-backup.md).

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="cloning-an-existing-app"></a>Mevcut bir uygulamayı kopyalama
Senaryo: Orta Güney ABD bölgesinde ve var olan bir web uygulamasında Kuzey Orta ABD bölgesinde yeni bir web uygulaması içeriği kopyalamak istersiniz. İle yeni bir web uygulaması oluşturmak için Azure Resource Manager sürümünü PowerShell cmdlet'ini kullanarak gerçekleştirilebilir `-SourceWebApp` seçeneği.

Kaynak web uygulamasını içeren kaynak grubu adını bilmek, kaynak web uygulamasının bilgi almak için aşağıdaki PowerShell komutunu kullanabilirsiniz (Bu durumda adlı `source-webapp`):

```PowerShell
$srcapp = Get-AzureRmWebApp -ResourceGroupName SourceAzureResourceGroup -Name source-webapp
```

Yeni bir App Service planı oluşturmak için kullanabileceğiniz `New-AzureRmAppServicePlan` aşağıdaki örnekteki gibi komutu

```PowerShell
New-AzureRmAppServicePlan -Location "South Central US" -ResourceGroupName DestinationAzureResourceGroup -Name NewAppServicePlan -Tier Premium
```

Kullanarak `New-AzureRmWebApp` komutu, Kuzey Orta ABD bölgesinde yeni web uygulaması oluşturun ve bir var olan premium katmanı için App Service planı bağlayın. Ayrıca, kaynak web uygulaması olarak aynı kaynak grubunu kullanın veya yeni bir kaynak grubu, aşağıdaki komutta gösterildiği gibi tanımlayın:

```PowerShell
$destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp
```

Tüm ilişkili dağıtım yuvalarını dahil olmak üzere mevcut bir web uygulaması kopyalamak için kullanmanız gerekir `IncludeSourceWebAppSlots` parametresi. Aşağıdaki PowerShell komutu ile bu parametre kullanımını gösteren `New-AzureRmWebApp` komutu:

```PowerShell
$destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp -IncludeSourceWebAppSlots
```

Kopyalamak aynı bölge içinde mevcut bir web uygulaması için yeni bir kaynak grubu oluşturmanız gerekir ve yeni bir app service planı aynı bölgede ve ardından web uygulamasını kopyalamak için aşağıdaki PowerShell komutunu kullanın

```PowerShell
$destapp = New-AzureRmWebApp -ResourceGroupName NewAzureResourceGroup -Name dest-webapp -Location "South Central US" -AppServicePlan NewAppServicePlan -SourceWebApp $srcap
```

## <a name="cloning-an-existing-app-to-an-app-service-environment"></a>App Service ortamı için mevcut bir uygulamayı kopyalama
Senaryo: Orta Güney ABD bölgesinde ve var olan bir web uygulamasında bir mevcut App Service ortamı (ASE) için yeni bir web uygulaması içeriği kopyalamak istersiniz.

Kaynak web uygulamasını içeren kaynak grubu adını bilmek, kaynak web uygulamasının bilgi almak için aşağıdaki PowerShell komutunu kullanabilirsiniz (Bu durumda adlı `source-webapp`):

```PowerShell
$srcapp = Get-AzureRmWebApp -ResourceGroupName SourceAzureResourceGroup -Name source-webapp
```

ASE'ın adı ve ASE ait olduğu kaynak grubu adı'nı bilerek, yeni web uygulaması mevcut ASE'de, aşağıdaki komutta gösterildiği gibi oluşturabilirsiniz:

```PowerShell
$destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -ASEName DestinationASE -ASEResourceGroupName DestinationASEResourceGroupName -SourceWebApp $srcapp
```

`Location` Eski nedeni parametre gerekiyor, ancak bir ASE'de uygulama oluşturduğunuzda göz ardı edilir. 

## <a name="cloning-an-existing-app-slot"></a>Mevcut bir uygulama yuva kopyalama
Senaryo: Mevcut bir Web uygulaması yuvası ya da yeni bir Web uygulamasına veya yeni bir Web uygulaması yuvası kopyalamak istersiniz. Yeni Web uygulaması, özgün Web uygulaması yuvası ile aynı bölgedeki veya farklı bir bölgede olabilir.

Kaynak web uygulamasını içeren kaynak grubu adı'nı bilerek kaynağı web uygulaması yuvanın bilgi almak için aşağıdaki PowerShell komutunu kullanabilirsiniz (Bu durumda adlı `source-webappslot`) Web uygulamasına bağlı `source-webapp`:

```PowerShell
$srcappslot = Get-AzureRmWebAppSlot -ResourceGroupName SourceAzureResourceGroup -Name source-webapp -Slot source-webappslot
```

Aşağıdaki komut, yeni bir web uygulaması için kaynak web uygulamasının bir kopyasını oluşturma gösterilmektedir:

```PowerShell
$destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcappslot
```

## <a name="configuring-traffic-manager-while-cloning-an-app"></a>Bir uygulamayı kopyalama sırasında traffic Manager'ı yapılandırma
Çok bölgeli web uygulamaları oluşturma ve bu web Apps'e tüm trafiği yönlendirmek için Azure Traffic Manager yapılandırma, müşterilerin uygulamaları yüksek oranda kullanılabilir olmasını sağlamak için önemli bir senaryodur. Mevcut bir web uygulaması kopyalama gerçekleştirilirken hem web uygulamaları yeni bir traffic manager profili ya da mevcut bir bağlanmak için seçeneğiniz vardır. Yalnızca Azure Resource Manager sürümü Traffic Manager'ın desteklenmiyor.

### <a name="creating-a-new-traffic-manager-profile-while-cloning-an-app"></a>Bir uygulamayı kopyalama sırasında yeni bir Traffic Manager profili oluşturma
Senaryo: Hem web uygulamaları içeren bir Azure Resource Manager traffic manager profili yapılandırırken bir web uygulamasını başka bir bölgeye kopyalamak istersiniz. Aşağıdaki komut, yeni bir Traffic Manager profili yapılandırırken yeni bir web uygulaması için kaynak web uygulamasının bir kopyasını oluşturmayı gösterir:

```PowerShell
$destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "South Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp -TrafficManagerProfileName newTrafficManagerProfile
```

### <a name="adding-new-cloned-web-app-to-an-existing-traffic-manager-profile"></a>Web uygulaması için var olan bir Traffic Manager profili kopyalanan yeni ekleme
Senaryo: Zaten bir Azure Resource Manager traffic manager profili varsa ve her iki web uygulama uç noktalar olarak eklemek istediğiniz. Önce mevcut trafik derlemek gerekir, bunu yapmak için manager profili kimliği Abonelik kimliği, kaynak grubu adı ve mevcut traffic manager profil adı gerekir.

```PowerShell
$TMProfileID = "/subscriptions/<Your subscription ID goes here>/resourceGroups/<Your resource group name goes here>/providers/Microsoft.TrafficManagerProfiles/ExistingTrafficManagerProfileName"
```

Trafik Yöneticisi kimliği atandıktan sonra aşağıdaki komutu var olan bir Traffic Manager profiline ekleme sırasında yeni bir web uygulaması için kaynak web uygulamasının bir kopyasını oluşturma gösterilmektedir:

```PowerShell
$destapp = New-AzureRmWebApp -ResourceGroupName <Resource group name> -Name dest-webapp -Location "South Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp -TrafficManagerProfileId $TMProfileID
```

## <a name="current-restrictions"></a>Geçerli sınırlamalar
Bu özellik şu anda önizlemededir ve yeni özellikler zamanla eklenir. Uygulama kopyalama geçerli sürümü bilinen sınırlamalar aşağıda verilmiştir:

* Otomatik ölçek ayarları klonlanmaz
* Yedekleme zamanlaması ayarları klonlanmaz
* Sanal ağ ayarlarını klonlanmaz
* App Insights otomatik olarak hedef web uygulamasında ayarlanmadınız
* Kolay kimlik doğrulama ayarları klonlanmaz
* Kudu uzantısı kopyalanmaz
* İpucu kuralları klonlanmaz
* Veritabanı içeriğini kopyalanan değil
* Giden IP adresleri için farklı bir ölçek birimi kopyalama, değiştirir.

### <a name="references"></a>Başvurular
* [Web uygulama kopyalama](app-service-web-app-cloning.md)
* [Azure App Service'te bir web uygulamasını yedekleme](web-sites-backup.md)
* [Azure Traffic Manager önizlemesi için Azure Resource Manager desteği](../traffic-manager/traffic-manager-powershell-arm.md)
* [App Service Ortamı’na giriş](environment/intro.md)
* [Azure PowerShell’i Azure Resource Manager ile kullanma](../azure-resource-manager/powershell-azure-resource-manager.md)

