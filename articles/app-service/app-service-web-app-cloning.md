---
title: PowerShell - Azure App Service ile uygulama kopyalama
description: PowerShell kullanarak yeni bir uygulama için App Service uygulamanızı kopyalama hakkında bilgi edinin.
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
ms.openlocfilehash: 198fedbbd1e97dcda15c9124109e50664f58f8e7
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66139713"
---
# <a name="azure-app-service-app-cloning-using-powershell"></a>Azure App Service uygulaması PowerShell kullanarak kopyalama

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Microsoft Azure PowerShell sürüm 1.1.0 sürümünde yeni bir seçenek eklendi `New-AzWebApp` olanak tanıyan yeni oluşturulmuş uygulamayı farklı bir bölgede ya da aynı bölgede var olan bir App Service uygulamasına kopyalayın. Bu seçenek, müşterilerin uygulama sayısı farklı bölgeler arasında hızlı ve kolay bir şekilde dağıtmasına olanak sağlar.

Uygulama kopyalama şu anda yalnızca premium katmanı app service planları için desteklenir. Yeni bir özellik App Service yedekleme özelliğini onunla aynı sınırlamalara kullanır, bkz: [bir uygulamasını Azure App Service'te yedekleme](manage-backup.md).

## <a name="cloning-an-existing-app"></a>Mevcut bir uygulamayı kopyalama
Senaryo: Orta Güney ABD bölgesinde ve mevcut bir uygulamayı Kuzey Orta ABD bölgesinde yeni bir uygulama içeriği kopyalamak istersiniz. İle yeni bir uygulama oluşturmak için Azure Resource Manager sürümünü PowerShell cmdlet'ini kullanarak gerçekleştirilebilir `-SourceWebApp` seçeneği.

Kaynak uygulamayı içeren kaynak grubu adını bilmek, kaynak uygulamanın bilgi almak için aşağıdaki PowerShell komutunu kullanabilirsiniz (Bu durumda adlı `source-webapp`):

```powershell
$srcapp = Get-AzWebApp -ResourceGroupName SourceAzureResourceGroup -Name source-webapp
```

Yeni bir App Service planı oluşturmak için kullanabileceğiniz `New-AzAppServicePlan` aşağıdaki örnekteki gibi komutu

```powershell
New-AzAppServicePlan -Location "South Central US" -ResourceGroupName DestinationAzureResourceGroup -Name NewAppServicePlan -Tier Premium
```

Kullanarak `New-AzWebApp` komutu, Kuzey Orta ABD bölgesinde yeni uygulama oluşturun ve bir var olan premium katmanı için App Service planı bağlayın. Ayrıca, kaynak uygulaması olarak aynı kaynak grubunu kullanın veya yeni bir kaynak grubu, aşağıdaki komutta gösterildiği gibi tanımlayın:

```powershell
$destapp = New-AzWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp
```

Tüm ilişkili dağıtım yuvalarını dahil olmak üzere mevcut bir uygulamayı kopyalama için kullanmanız gerekir `IncludeSourceWebAppSlots` parametresi. Aşağıdaki PowerShell komutu ile bu parametre kullanımını gösteren `New-AzWebApp` komutu:

```powershell
$destapp = New-AzWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp -IncludeSourceWebAppSlots
```

Aynı bölge içinde mevcut bir uygulamayı kopyalama için yeni bir kaynak grubu oluşturmanız gerekir ve yeni bir app service planı aynı bölgede ve sonra uygulamayı kopyalamak için aşağıdaki PowerShell komutunu kullanın:

```powershell
$destapp = New-AzWebApp -ResourceGroupName NewAzureResourceGroup -Name dest-webapp -Location "South Central US" -AppServicePlan NewAppServicePlan -SourceWebApp $srcap
```

## <a name="cloning-an-existing-app-to-an-app-service-environment"></a>App Service ortamı için mevcut bir uygulamayı kopyalama
Senaryo: Orta Güney ABD bölgesinde ve mevcut bir uygulamayı bir mevcut App Service ortamı (ASE) için yeni bir uygulama içeriği kopyalamak istersiniz.

Kaynak uygulamayı içeren kaynak grubu adını bilmek, kaynak uygulamanın bilgi almak için aşağıdaki PowerShell komutunu kullanabilirsiniz (Bu durumda adlı `source-webapp`):

```powershell
$srcapp = Get-AzWebApp -ResourceGroupName SourceAzureResourceGroup -Name source-webapp
```

ASE'ın adı ve ASE ait olduğu kaynak grubu adı'nı bilerek, yeni uygulama mevcut ASE'de, aşağıdaki komutta gösterildiği gibi oluşturabilirsiniz:

```powershell
$destapp = New-AzWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -ASEName DestinationASE -ASEResourceGroupName DestinationASEResourceGroupName -SourceWebApp $srcapp
```

`Location` Eski nedeni parametre gerekiyor, ancak bir ASE'de uygulama oluşturduğunuzda göz ardı edilir. 

## <a name="cloning-an-existing-app-slot"></a>Mevcut bir uygulama yuva kopyalama
Senaryo: Bir uygulamanın ya da yeni bir uygulama için var olan bir dağıtım yuvası veya yeni bir yuva kopyalamak istersiniz. Yeni uygulama, özgün uygulaması yuvası ile aynı bölgedeki veya farklı bir bölgede olabilir.

Kaynak uygulamayı içeren kaynak grubu adı'nı bilerek kaynak uygulama yuvanın bilgi almak için aşağıdaki PowerShell komutunu kullanabilirsiniz (Bu durumda adlı `source-appslot`) bağlı `source-app`:

```powershell
$srcappslot = Get-AzWebAppSlot -ResourceGroupName SourceAzureResourceGroup -Name source-app -Slot source-appslot
```

Aşağıdaki komutu, yeni bir uygulama için kaynak uygulamasının bir kopyasını oluşturmayı gösterir:

```powershell
$destapp = New-AzWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-app -Location "North Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcappslot
```

## <a name="configuring-traffic-manager-while-cloning-an-app"></a>Bir uygulamayı kopyalama sırasında traffic Manager'ı yapılandırma
Çok bölgeli uygulama oluşturmaya ve bu uygulamalar için trafiği yönlendirmek için Azure Traffic Manager yapılandırma, müşterilerin uygulamaları yüksek oranda kullanılabilir olmasını sağlamak için önemli bir senaryo değildir. Mevcut bir uygulamayı kopyalama, her iki uygulama yeni bir traffic manager profili ya da mevcut bir bağlanmak için seçeneğiniz vardır. Yalnızca Azure Resource Manager sürümü Traffic Manager'ın desteklenmiyor.

### <a name="creating-a-new-traffic-manager-profile-while-cloning-an-app"></a>Bir uygulamayı kopyalama sırasında yeni bir Traffic Manager profili oluşturma
Senaryo: Her iki uygulama içeren bir Azure Resource Manager traffic manager profili yapılandırırken bir uygulamayı başka bir bölgeye kopyalamak istersiniz. Aşağıdaki komut, yeni bir Traffic Manager profili yapılandırırken kaynak uygulamanın yeni bir uygulama için bir kopya oluşturma gösterilmektedir:

```powershell
$destapp = New-AzWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "South Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp -TrafficManagerProfileName newTrafficManagerProfile
```

### <a name="adding-new-cloned-app-to-an-existing-traffic-manager-profile"></a>Uygulama için var olan bir Traffic Manager profilini kopyalanan yeni ekleme
Senaryo: Zaten bir Azure Resource Manager traffic manager profili varsa ve her iki uygulama uç noktalar olarak eklemek istediğiniz. Önce mevcut trafik derlemek gerekir, bunu yapmak için manager profili kimliği Abonelik kimliği, kaynak grubu adı ve mevcut traffic manager profil adı gerekir.

```powershell
$TMProfileID = "/subscriptions/<Your subscription ID goes here>/resourceGroups/<Your resource group name goes here>/providers/Microsoft.TrafficManagerProfiles/ExistingTrafficManagerProfileName"
```

Trafik Yöneticisi kimliği atandıktan sonra aşağıdaki komutu var olan bir Traffic Manager profiline eklenirken kaynak uygulamanın yeni bir uygulama için bir kopya oluşturma gösterilmektedir:

```powershell
$destapp = New-AzWebApp -ResourceGroupName <Resource group name> -Name dest-webapp -Location "South Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp -TrafficManagerProfileId $TMProfileID
```

## <a name="current-restrictions"></a>Geçerli sınırlamalar
Uygulama kopyalama bilinen kısıtlamalar şunlardır:

* Otomatik ölçek ayarları klonlanmaz
* Yedekleme zamanlaması ayarları klonlanmaz
* Sanal ağ ayarlarını klonlanmaz
* App Insights otomatik olarak hedef uygulamanın ayarlanmadınız
* Kolay kimlik doğrulama ayarları klonlanmaz
* Kudu uzantısı kopyalanmaz
* İpucu kuralları klonlanmaz
* Veritabanı içeriğini kopyalanan değil
* Giden IP adresleri için farklı bir ölçek birimi kopyalama, değiştirir.

### <a name="references"></a>Başvurular
* [App Service kopyalama](app-service-web-app-cloning.md)
* [Azure App Service'te bir uygulama yedekleme](manage-backup.md)
* [Azure Traffic Manager önizlemesi için Azure Resource Manager desteği](../traffic-manager/traffic-manager-powershell-arm.md)
* [App Service Ortamı’na giriş](environment/intro.md)
* [Azure PowerShell’i Azure Resource Manager ile kullanma](../azure-resource-manager/manage-resources-powershell.md)

