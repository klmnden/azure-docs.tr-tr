---
title: "Web uygulaması PowerShell kullanarak kopyalama"
description: "PowerShell kullanarak yeni Web uygulamaları için Web uygulamalarınızı kopyalama öğrenin."
services: app-service\web
documentationcenter: 
author: ahmedelnably
manager: stefsch
editor: 
ms.assetid: f9a5cfa1-fbb0-41e6-95d1-75d457347a35
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/13/2016
ms.author: aelnably
ms.openlocfilehash: dc252903571857b5fc89d1d9a2c63cd6b44e9021
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="azure-app-service-app-cloning-using-powershell"></a>Azure App Service uygulaması PowerShell kullanarak kopyalama
Microsoft Azure PowerShell sürüm 1.1.0 sürümü, kullanıcı yeni oluşturulan bir uygulamanın farklı bir bölgede ya da aynı bölgede var olan bir Web uygulamasının kopyalama olanağı verirsiniz New-AzureRMWebApp için yeni bir seçenek eklendi. Bu uygulama sayısı farklı bölgeler arasında hızlı ve kolay bir şekilde dağıtmasını sağlar.

Uygulama kopyalama şu anda yalnızca premium katmanı uygulama hizmeti planları için desteklenir. Yeni özellik aynı sınırlamalar Web Apps yedekleme özelliği kullanan, bkz: [Azure App Service'te bir web uygulaması yedekleme](web-sites-backup.md).

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="cloning-an-existing-app"></a>Mevcut bir uygulamayı kopyalama
Senaryo: Var olan bir web uygulamasının Orta Güney ABD bölgesindeki kullanıcı yeni bir web uygulaması Kuzey Orta ABD bölgesindeki içindekilere kopyalamak istiyor. Bu, - SourceWebApp seçeneği ile yeni bir web uygulaması oluşturmak için PowerShell cmdlet Azure Resource Manager sürümü kullanılarak gerçekleştirilebilir.

Kaynak web uygulamasını içeren kaynak grubu adı bilerek, biz (Bu durumda kaynak webapp adlı) kaynak web uygulamanızın bilgi almak için aşağıdaki PowerShell komutunu kullanabilirsiniz:

    $srcapp = Get-AzureRmWebApp -ResourceGroupName SourceAzureResourceGroup -Name source-webapp

Yeni bir uygulama hizmeti planı oluşturmak için size aşağıdaki örnekteki gibi yeni AzureRmAppServicePlan komutunu kullanabilirsiniz

    New-AzureRmAppServicePlan -Location "South Central US" -ResourceGroupName DestinationAzureResourceGroup -Name NewAppServicePlan -Tier Premium

Yeni AzureRmWebApp komutunu kullanarak, biz Kuzey Orta ABD bölgesinde yeni web uygulaması oluşturma ve bir varolan premium katmanına App Service planı tie, ayrıca biz kaynak web uygulaması aynı kaynak grubunu kullanın veya yeni bir kaynak grubu tanımlayın, şunlar gösterilmektedir:

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp

İlişkili tüm dağıtım dahil olmak üzere var olan bir web uygulamasının kopyalama yuvası, kullanıcının IncludeSourceWebAppSlots parametresini kullanmanız gerekir, bu parametrenin yeni AzureRmWebApp komutuyla kullanımını aşağıdaki PowerShell komutunu gösterir:

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp -IncludeSourceWebAppSlots

Aynı bölge içinde var olan bir web uygulaması kopyalamak için kullanıcının yeni bir kaynak grubu ve yeni bir uygulama hizmeti oluşturmanız gerekecektir planlama aynı bölgede ve web uygulaması kopyalamak için aşağıdaki PowerShell komutunu kullanarak

    $destapp = New-AzureRmWebApp -ResourceGroupName NewAzureResourceGroup -Name dest-webapp -Location "South Central US" -AppServicePlan NewAppServicePlan -SourceWebApp $srcap

## <a name="cloning-an-existing-app-to-an-app-service-environment"></a>Bir uygulama hizmeti ortamı var olan bir uygulamaya kopyalama
Senaryo: Var olan bir web uygulamasının Orta Güney ABD bölgesindeki kullanıcı yeni bir web uygulaması bir var olan uygulama hizmeti ortamı (ana) için içeriği kopyalamak istiyor.

Kaynak web uygulamasını içeren kaynak grubu adı bilerek, biz (Bu durumda kaynak webapp adlı) kaynak web uygulamanızın bilgi almak için aşağıdaki PowerShell komutunu kullanabilirsiniz:

    $srcapp = Get-AzureRmWebApp -ResourceGroupName SourceAzureResourceGroup -Name source-webapp

Ana'nın adını ve ana ait olduğu kaynak grubu adı bilerek kullanıcı içinde varolan ana yeni web uygulaması oluşturmak için New-AzureRmWebApp komutunu kullanabilirsiniz, şunlar gösterilmektedir:

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -ASEName DestinationASE -ASEResourceGroupName DestinationASEResourceGroupName -SourceWebApp $srcapp

Eski bir nedenden dolayı konum parametresi gereklidir, ancak ASE'de bir uygulama oluşturma söz konusu olduğunda, göz ardı edilir. 

## <a name="cloning-an-existing-app-slot"></a>Varolan bir uygulaması yuvası kopyalama
Senaryo: Kullanıcı ya da yeni bir Web uygulaması için var olan bir Web uygulaması yuvası veya yeni bir Web uygulaması yuvası kopyalama ister misiniz? Yeni Web uygulaması, özgün Web uygulaması yuvası ile aynı bölgede ya da farklı bir bölgede olabilir.

Kaynak web uygulamasını içeren kaynak grubu adı bilerek, biz Web uygulaması kaynak-webapp için bağlı (Bu durumda kaynak webappslot olarak adlandırılır) kaynak web uygulaması yuvanın bilgi almak için aşağıdaki PowerShell komutunu kullanabilirsiniz:

    $srcappslot = Get-AzureRmWebAppSlot -ResourceGroupName SourceAzureResourceGroup -Name source-webapp -Slot source-webappslot

Yeni bir web uygulaması için kaynak web uygulamasının bir kopyasını oluşturma gösterilmektedir:

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcappslot

## <a name="configuring-traffic-manager-while-cloning-a-app"></a>Bir uygulama kopyalanırken yapılandırma trafik Yöneticisi
Birden çok bölgede web uygulamaları oluşturma ve bu tüm web uygulamaları için trafiği yönlendirmek için Azure Traffic Manager yapılandırma, müşterilerin uygulamalar yüksek oranda kullanılabilir, var olan bir web uygulamasının kopyalarken yalnızca Azure Kaynak Yöneticisi'ni trafik Yöneticisi'nin sürümü desteklenen her iki web uygulamaları yeni bir trafik Yöneticisi profili ya da mevcut bir bağlanma - unutmayın seçeneğine sahip güvence altına almaya n önemli senaryodur.

### <a name="creating-a-new-traffic-manager-profile-while-cloning-a-app"></a>Bir uygulama kopyalama sırasında yeni bir Traffic Manager profili oluşturma
Senaryo: Kullanıcı her iki web uygulamaları içeren bir Azure Resource Manager trafik Yöneticisi profili yapılandırırken başka bir bölge, bir web uygulamasına kopyalamak istiyor. Yeni bir Traffic Manager profili yapılandırırken kaynak web uygulamasının yeni bir web uygulaması için bir kopyasını oluşturma gösterilmektedir:

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "South Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp -TrafficManagerProfileName newTrafficManagerProfile

### <a name="adding-new-cloned-web-app-to-an-existing-traffic-manager-profile"></a>Web uygulaması var olan bir Traffic Manager profilini klonlanmış yeni ekleme
Senaryo: Kullanıcı zaten kendisine her iki web uygulamaları uç noktalar olarak eklemek istediğiniz bir Azure Resource Manager trafik Yöneticisi profili sahip. Önce var olan trafik Yöneticisi Profil Kimliği derlemek ihtiyacımız Bunu yapmak için şu abonelik kimliği, kaynak grubu adı ve var olan trafik Yöneticisi profil adı gerekir.

    $TMProfileID = "/subscriptions/<Your subscription ID goes here>/resourceGroups/<Your resource group name goes here>/providers/Microsoft.TrafficManagerProfiles/ExistingTrafficManagerProfileName"

Trafik Yöneticisi kimliğine sahip sonra aşağıdaki kaynak web uygulamasının yeni bir web uygulaması için bir kopya var olan bir Traffic Manager profilini eklenirken oluşturmayı gösterir:

    $destapp = New-AzureRmWebApp -ResourceGroupName <Resource group name> -Name dest-webapp -Location "South Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp -TrafficManagerProfileId $TMProfileID

## <a name="current-restrictions"></a>Geçerli kısıtlamaları
Bu özellik şu anda önizlemede değil, zaman içinde yeni özellikler eklemek için çalışıyoruz, aşağıdaki listede uygulama kopyalama geçerli sürümü bilinen sınırlamalar:

* Otomatik ölçek ayarları klonlanmış değil
* Yedekleme zamanlaması ayarları klonlanmış değil
* VNET ayarlarını klonlanmış değil
* App Insights otomatik olarak hedef web uygulamasında belirlenmedi
* Kolay kimlik doğrulama ayarları klonlanmış değil
* Kudu uzantısı değil kopyalanamıyor
* İpucu kuralları klonlanmış değil
* Veritabanı içeriğini değil kopyalanamıyor

### <a name="references"></a>Başvurular
* [Web uygulaması kopyalama](app-service-web-app-cloning.md)
* [Azure App Service'in web uygulamasında yedekleyin](web-sites-backup.md)
* [Azure Traffic Manager Önizleme için Azure Resource Manager desteği](../traffic-manager/traffic-manager-powershell-arm.md)
* [App Service Ortamı’na giriş](environment/intro.md)
* [Azure PowerShell’i Azure Resource Manager ile kullanma](../azure-resource-manager/powershell-azure-resource-manager.md)

