---
title: Azure Traffic Manager'da yönetmek için PowerShell kullanma | Microsoft Docs
description: Trafik Yöneticisi ile Azure kaynak yöneticisi için PowerShell kullanma
services: traffic-manager
documentationcenter: na
author: kumudd
manager: timlt
ms.assetid: bc247448-1d2e-4104-ac03-42b59ebde065
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/16/2017
ms.author: kumud
ms.openlocfilehash: 951e845e23a1ed0cbdc83fc24a97a545f00c52ad
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2018
ms.locfileid: "31526650"
---
# <a name="using-powershell-to-manage-traffic-manager"></a>Trafik Yöneticisi'ni yönetmek için PowerShell kullanma

Azure Resource Manager Azure Hizmetleri için tercih edilen Yönetim arabirimidir. Azure Traffic Manager profillerini, Azure Resource Manager tabanlı API'ler ve araçlar kullanılarak yönetilebilir.

## <a name="resource-model"></a>Kaynak modeli

Azure Traffic Manager trafik Yöneticisi profili denir ayarlar koleksiyonu kullanılarak yapılandırılır. Bu profili, DNS ayarlarını, trafik yönlendirme ayarlarını, uç nokta izleme ayarlarını ve hizmet uç noktaları için trafiği yönlendirilir listesini içerir.

Her trafik Yöneticisi profili 'TrafficManagerProfiles' türünde bir kaynak temsil edilir. REST API düzeyinde her profil için URI aşağıdaki gibidir:

    https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Network/trafficManagerProfiles/{profile-name}?api-version={api-version}

## <a name="setting-up-azure-powershell"></a>Azure PowerShell ayarlama

Bu yönergeler Microsoft Azure PowerShell kullanın. Aşağıdaki makalede, Azure PowerShell'i yükleme ve yapılandırma açıklanmaktadır.

* [Azure PowerShell’i yükleme ve yapılandırma](/powershell/azure/overview)

Bu makaledeki örneklerde, varolan bir kaynak grubu olduğunu varsayalım. Aşağıdaki komutu kullanarak bir kaynak grubu oluşturabilirsiniz:

```powershell
New-AzureRmResourceGroup -Name MyRG -Location "West US"
```

> [!NOTE]
> Azure Resource Manager, tüm kaynak gruplarının bir konum olmasını gerektirir. Bu konum, bu kaynak grubunda oluşturduğunuz kaynaklar için varsayılan olarak kullanılır. Trafik Yöneticisi profili kaynaklar olduğundan genel, bölgesel değil, ancak kaynak grubu konumu seçiminin Azure Traffic Manager üzerinde etkisi yoktur.

## <a name="create-a-traffic-manager-profile"></a>Trafik Yöneticisi profili oluştur

Trafik Yöneticisi profili oluşturmak için kullanın `New-AzureRmTrafficManagerProfile` cmdlet:

```powershell
$profile = New-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG -TrafficRoutingMethod Performance -RelativeDnsName contoso -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
```

Aşağıdaki tabloda parametreler açıklanmaktadır:

| Parametre | Açıklama |
| --- | --- |
| Ad |Trafik Yöneticisi profili kaynak kaynak adı. Profilleri aynı kaynak grubunda benzersiz adlara sahip olmalıdır. Bu ad, DNS sorguları için kullanılan DNS adı ayrıdır. |
| ResourceGroupName |Profil kaynağını içeren kaynak grubunun adı. |
| TrafficRoutingMethod |Hangi uç noktaya bir DNS sorgusu yanıtta döndürülen belirlemek için kullanılan trafik yönlendirme yöntemini belirtir. Olası değerler 'Performans', 'Weighted' veya 'Priority' tır. |
| RelativeDnsName |Bu trafik Yöneticisi profili tarafından sağlanan DNS adının ana bilgisayar adı kısmını belirtir. Bu değer, profilin tam etki alanı adını (FQDN) oluşturmak için Azure Traffic Manager tarafından kullanılan DNS etki alanı adı ile birleştirilir. Örneğin, 'contoso' değerini ayarlama 'contoso.trafficmanager.net.' olur |
| TTL |DNS için-yaşam süresi (TTL), saniye cinsinden belirtir. Yerel DNS Çözümleyicileri ve DNS istemcilerini bu trafik Yöneticisi profili için önbellek DNS yanıtları ne kadar bu TTL bildirir. |
| MonitorProtocol |Uç nokta izlemesi için kullanılacak protokolü belirtir. Olası değerler şunlardır: 'HTTP' ve 'HTTPS'. |
| MonitorPort |Uç noktası durumunu izlemek için kullanılan TCP bağlantı noktasını belirtir. |
| MonitorPath |Uç noktası durumu için yoklama için kullanılan uç nokta etki alanı adı göreli yolunu belirtir. |

Cmdlet ile Azure trafik Yöneticisi profili oluşturur ve PowerShell karşılık gelen bir profili nesnesini döndürür. Bu noktada, profil uç nokta içermiyor. Bir trafik Yöneticisi Profil uç noktaları ekleme hakkında daha fazla bilgi için bkz: [trafik Yöneticisi uç noktaları ekleme](#adding-traffic-manager-endpoints).

## <a name="get-a-traffic-manager-profile"></a>Trafik Yöneticisi profilini Al

Varolan bir trafik Yöneticisi profili nesnesini almak kullanın `Get-AzureRmTrafficManagerProfle` cmdlet:

```powershell
$profile = Get-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG
```

Bu cmdlet bir trafik Yöneticisi Profil nesnesi döndürür.

## <a name="update-a-traffic-manager-profile"></a>Trafik Yöneticisi profilini güncelleştir

Traffic Manager profillerini değiştirme 3 adımlı işlem aşağıdaki gibidir:

1. Profili kullanarak almak `Get-AzureRmTrafficManagerProfile` veya tarafından döndürülen profilini kullanan `New-AzureRmTrafficManagerProfile`.
2. Profil değiştirin. Ekleme ve uç noktaları kaldırın veya uç nokta veya profil parametrelerini değiştirin. Bu değişiklikler çevrimdışı işlemleridir. Profil temsil eden bellekte yerel nesnesi yalnızca değiştiriyorsunuz.
3. Kullanarak, değişiklikleri `Set-AzureRmTrafficManagerProfile` cmdlet'i.

Profilin RelativeDnsName tüm profil özellikleri değiştirilebilir. RelativeDnsName değiştirmek için profili ve yeni bir adla yeni bir profil silmeniz gerekir.

Aşağıdaki örnek, profilin TTL değiştirmek gösterilmiştir:

```powershell
$profile = Get-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG
$profile.Ttl = 300
Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile
```

Trafik Yöneticisi uç noktaları üç tür vardır:

1. **Azure uç noktaları** Azure üzerinde barındırılan hizmeti
2. **Dış uç noktalar** Azure dışında barındırılan hizmeti
3. **İç içe uç noktaları** iç içe geçmiş hiyerarşileri trafik Yöneticisi profili oluşturmak için kullanılır. İç içe geçmiş uç noktaları karmaşık uygulamalar için Gelişmiş trafik yönlendirme yapılandırmaları etkinleştirin.

Her üç durumda, iki yolla uç noktalar eklenebilir:

1. Daha önce açıklanan 3 adımlı işlem kullanıyor. Bu yöntemin avantajı, birkaç uç noktası değişiklikleri tek bir güncelleştirme yapılabilir ' dir.
2. Yeni AzureRmTrafficManagerEndpoint cmdlet'ini kullanarak. Bu cmdlet var olan bir Traffic Manager profilini tek bir işlemde bir uç nokta ekler.

## <a name="adding-azure-endpoints"></a>Azure uç noktaları ekleme

Azure uç noktaları Azure içinde barındırılan hizmetler başvuru. Azure uç noktaları iki tür desteklenir:

1. Azure Web Apps
2. (Bir yük dengeleyici veya bir sanal makine NIC eklenebilecek) azure Publicıpaddress kaynakları. Publicıpaddress trafik Yöneticisi'nde kullanılmak üzere atanmış bir DNS adı olmalıdır.

Her durumda:

* Hizmet 'uç noktası Targetresourceıd' parametresini kullanarak belirtilen `Add-AzureRmTrafficManagerEndpointConfig` veya `New-AzureRmTrafficManagerEndpoint`.
* 'Target' ve 'EndpointLocation' uç noktası Targetresourceıd tarafından kapsanan.
* Belirten 'ağırlığı' isteğe bağlıdır. Profil 'Weighted' trafik yönlendirme yöntemini kullanmak üzere yapılandırılmışsa, ağırlıkları yalnızca kullanılır. Aksi takdirde, bunlar yoksayılır. Belirtilmişse, değerin 1 ve 1000 arasında bir sayı olması gerekir. Varsayılan değer, '1' dir.
* Belirten 'Priority' isteğe bağlıdır. Profil 'Priority' trafik yönlendirme yöntemini kullanmak üzere yapılandırılmışsa, öncelikleri yalnızca kullanılır. Aksi takdirde, bunlar yoksayılır. Geçerli değerler 1 ile 1000 daha yüksek öncelik belirten alt ile değerlerdir. Bir uç nokta için belirtilmişse, tüm uç noktaları için belirtilmelidir. Atlanırsa, varsayılan değerleri '1'den başlayarak uç noktaları listelenen sırayla uygulanır.

### <a name="example-1-adding-web-app-endpoints-using-add-azurermtrafficmanagerendpointconfig"></a>Örnek 1: kullanarak Web uygulama uç noktaları ekleme `Add-AzureRmTrafficManagerEndpointConfig`

Bu örnekte, size bir trafik Yöneticisi profili oluşturun ve kullanarak iki Web uygulaması uç nokta ekleyin `Add-AzureRmTrafficManagerEndpointConfig` cmdlet'i.

```powershell
$profile = New-AzureRmTrafficManagerProfile -Name myprofile -ResourceGroupName MyRG -TrafficRoutingMethod Performance -RelativeDnsName myapp -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
$webapp1 = Get-AzureRMWebApp -Name webapp1
Add-AzureRmTrafficManagerEndpointConfig -EndpointName webapp1ep -TrafficManagerProfile $profile -Type AzureEndpoints -TargetResourceId $webapp1.Id -EndpointStatus Enabled
$webapp2 = Get-AzureRMWebApp -Name webapp2
Add-AzureRmTrafficManagerEndpointConfig -EndpointName webapp2ep -TrafficManagerProfile $profile -Type AzureEndpoints -TargetResourceId $webapp2.Id -EndpointStatus Enabled
Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile
```
### <a name="example-2-adding-a-publicipaddress-endpoint-using-new-azurermtrafficmanagerendpoint"></a>Örnek 2: Publicıpaddress kullanarak uç nokta ekleme `New-AzureRmTrafficManagerEndpoint`

Bu örnekte, bir ortak IP adresi kaynağı trafik Yöneticisi profiline eklenir. Genel IP adresi yapılandırılmış bir DNS adına sahip olmalıdır ve bir VM NIC veya bir yük dengeleyiciye bağlı olabilir.

```powershell
$ip = Get-AzureRmPublicIpAddress -Name MyPublicIP -ResourceGroupName MyRG
New-AzureRmTrafficManagerEndpoint -Name MyIpEndpoint -ProfileName MyProfile -ResourceGroupName MyRG -Type AzureEndpoints -TargetResourceId $ip.Id -EndpointStatus Enabled
```

## <a name="adding-external-endpoints"></a>Dış uç noktalar ekleme

Trafik Yöneticisi dış uç noktalar Azure dışında barındırılan hizmetleri trafiği yönlendirmek için kullanır. Dış uç noktalar ya da Azure uç noktaları ile eklenen kullanarak `Add-AzureRmTrafficManagerEndpointConfig` arkasından `Set-AzureRmTrafficManagerProfile`, veya `New-AzureRMTrafficManagerEndpoint`.

Dış uç noktalar belirtirken:

* 'Target' parametresini kullanarak uç nokta etki alanı adı belirtilmelidir
* Trafik yönlendirme 'Performans' yöntemi kullanılırsa, 'EndpointLocation' gereklidir. Aksi takdirde isteğe bağlıdır. Değer olmalıdır bir [geçerli Azure bölgesi adı](https://azure.microsoft.com/regions/).
* 'Weight' ve 'Priority' isteğe bağlıdır.

### <a name="example-1-adding-external-endpoints-using-add-azurermtrafficmanagerendpointconfig-and-set-azurermtrafficmanagerprofile"></a>Örnek 1: kullanarak dış uç noktalar ekleme `Add-AzureRmTrafficManagerEndpointConfig` ve `Set-AzureRmTrafficManagerProfile`

Bu örnekte, size bir trafik Yöneticisi profili oluşturun, iki dış uç noktalar ekleyin ve değişiklikleri uygulayın.

```powershell
$profile = New-AzureRmTrafficManagerProfile -Name myprofile -ResourceGroupName MyRG -TrafficRoutingMethod Performance -RelativeDnsName myapp -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
Add-AzureRmTrafficManagerEndpointConfig -EndpointName eu-endpoint -TrafficManagerProfile $profile -Type ExternalEndpoints -Target app-eu.contoso.com -EndpointLocation "North Europe" -EndpointStatus Enabled
Add-AzureRmTrafficManagerEndpointConfig -EndpointName us-endpoint -TrafficManagerProfile $profile -Type ExternalEndpoints -Target app-us.contoso.com -EndpointLocation "Central US" -EndpointStatus Enabled
Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile
```

### <a name="example-2-adding-external-endpoints-using-new-azurermtrafficmanagerendpoint"></a>Örnek 2: kullanarak dış uç noktalar ekleme `New-AzureRmTrafficManagerEndpoint`

Bu örnekte, bir profil harici bir uç nokta ekleriz. Profil profili ve kaynak grubu adları kullanılarak belirtilir.

```powershell
New-AzureRmTrafficManagerEndpoint -Name eu-endpoint -ProfileName MyProfile -ResourceGroupName MyRG -Type ExternalEndpoints -Target app-eu.contoso.com -EndpointStatus Enabled
```

## <a name="adding-nested-endpoints"></a>'İç içe' uç noktaları ekleme

Her trafik Yöneticisi profili, tek bir trafik yönlendirme yöntemini belirtir. Ancak, tek bir trafik Yöneticisi profili tarafından sağlanan yönlendirme olandan daha karmaşık trafik yönlendirme gerektiren senaryolar vardır. Birden çok trafik yönlendirme yöntemini yararları birleştirmek için Traffic Manager profillerini yerleştirebilirsiniz. İç içe geçmiş profilleri destek daha büyük ve daha karmaşık uygulama dağıtımları için varsayılan trafik Yöneticisi davranışı geçersiz kılmanıza olanak sağlar. Daha ayrıntılı örnekler için bkz: [iç içe trafik Yöneticisi profillerine](traffic-manager-nested-profiles.md).

İç içe geçmiş uç noktaları üst profilde, belirli uç nokta türü, 'NestedEndpoints' kullanılarak yapılandırılır. İç içe geçmiş uç noktaları belirtirken:

* Uç nokta 'uç noktası Targetresourceıd' parametresi kullanılarak belirtilmelidir.
* Trafik yönlendirme 'Performans' yöntemi kullanılırsa, 'EndpointLocation' gereklidir. Aksi takdirde isteğe bağlıdır. Değer olmalıdır bir [geçerli Azure bölgesi adı](http://azure.microsoft.com/regions/).
* 'Weight' ve 'Priority' için olduğu gibi Azure uç noktaları isteğe bağlıdır.
* 'MinChildEndpoints' parametresi isteğe bağlıdır. Varsayılan değer, '1' dir. Kullanılabilir uç nokta sayısı bu eşiğin altına düşmesi durumunda üst profili alt profili 'düşürülmüş' ve üst profili diğer uç noktalardan trafiğini yönlendiren göz önünde bulundurur.

### <a name="example-1-adding-nested-endpoints-using-add-azurermtrafficmanagerendpointconfig-and-set-azurermtrafficmanagerprofile"></a>Örnek 1: kullanarak iç içe geçmiş uç noktaları ekleme `Add-AzureRmTrafficManagerEndpointConfig` ve `Set-AzureRmTrafficManagerProfile`

Bu örnekte, biz yeni trafik Yöneticisi alt ve üst profilleri oluşturma, alt üst iç içe geçmiş bir uç noktası olarak ekleyin ve değişiklikleri uygulayın.

```powershell
$child = New-AzureRmTrafficManagerProfile -Name child -ResourceGroupName MyRG -TrafficRoutingMethod Priority -RelativeDnsName child -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
$parent = New-AzureRmTrafficManagerProfile -Name parent -ResourceGroupName MyRG -TrafficRoutingMethod Performance -RelativeDnsName parent -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
Add-AzureRmTrafficManagerEndpointConfig -EndpointName child-endpoint -TrafficManagerProfile $parent -Type NestedEndpoints -TargetResourceId $child.Id -EndpointStatus Enabled -EndpointLocation "North Europe" -MinChildEndpoints 2
Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile
```

Bu örnekte okumanızdır biz diğer uç nokta alt veya üst profillere eklemediniz.

### <a name="example-2-adding-nested-endpoints-using-new-azurermtrafficmanagerendpoint"></a>Örnek 2: kullanarak iç içe geçmiş uç noktaları ekleme `New-AzureRmTrafficManagerEndpoint`

Bu örnekte, var olan bir alt profilini iç içe geçmiş bir uç noktası olarak var olan bir üst profilini ekleriz. Profil profili ve kaynak grubu adları kullanılarak belirtilir.

```powershell
$child = Get-AzureRmTrafficManagerEndpoint -Name child -ResourceGroupName MyRG
New-AzureRmTrafficManagerEndpoint -Name child-endpoint -ProfileName parent -ResourceGroupName MyRG -Type NestedEndpoints -TargetResourceId $child.Id -EndpointStatus Enabled -EndpointLocation "North Europe" -MinChildEndpoints 2
```

## <a name="adding-endpoints-from-another-subscription"></a>Başka bir abonelikten uç noktaları ekleme

Trafik Yöneticisi uç noktalarından farklı Aboneliklerde çalışabilirsiniz. Trafik Yöneticisi için gerekli giriş almak için eklemek istediğiniz uç noktası ile aboneliğine geçmeniz gerekir. Ardından trafik Yöneticisi profili aboneliklerle geçin ve encpoint kendisine eklemeniz gerekir. Aşağıdaki örnekte, bir ortak IP adresi ile bunun gösterilmektedir.

```powershell
Set-AzureRmContext -SubscriptionId $EndpointSubscription
$ip = Get-AzureRmPublicIpAddress -Name $IpAddresName -ResourceGroupName $EndpointRG

Set-AzureRmContext -SubscriptionId $trafficmanagerSubscription
New-AzureRmTrafficManagerEndpoint -Name $EndpointName -ProfileName $ProfileName -ResourceGroupName $TrafficManagerRG -Type AzureEndpoints -TargetResourceId $ip.Id -EndpointStatus Enabled
```

## <a name="update-a-traffic-manager-endpoint"></a>Trafik Yöneticisi uç noktasını güncelleyin

Mevcut bir trafik Yöneticisi uç noktası güncelleştirmek için iki yolu vardır:

1. Trafik Yöneticisi profili kullanarak Al `Get-AzureRmTrafficManagerProfile`profilindeki uç noktası özelliklerini güncelleştirmek ve kullanarak değişiklikleri `Set-AzureRmTrafficManagerProfile`. Bu yöntem tek bir işlemde birden fazla uç noktasını güncelleyin yapamamasına avantajına sahiptir.
2. Trafik Yöneticisi uç noktası kullanarak alma `Get-AzureRmTrafficManagerEndpoint`uç noktası özelliklerini güncelleştirmek ve kullanarak değişiklikleri `Set-AzureRmTrafficManagerEndpoint`. Profil uç noktaları dizisinde içine dizin gerektirmediğinden bu yöntem basittir.

### <a name="example-1-updating-endpoints-using-get-azurermtrafficmanagerprofile-and-set-azurermtrafficmanagerprofile"></a>Örnek 1: kullanarak uç noktaları güncelleştirme `Get-AzureRmTrafficManagerProfile` ve `Set-AzureRmTrafficManagerProfile`

Bu örnekte, iki uç nokta bir profil içinde öncelik değiştirin.

```powershell
$profile = Get-AzureRmTrafficManagerProfile -Name myprofile -ResourceGroupName MyRG
$profile.Endpoints[0].Priority = 2
$profile.Endpoints[1].Priority = 1
Set-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile
```

### <a name="example-2-updating-an-endpoint-using-get-azurermtrafficmanagerendpoint-and-set-azurermtrafficmanagerendpoint"></a>Örnek 2: bir uç nokta kullanarak güncelleştirme `Get-AzureRmTrafficManagerEndpoint` ve `Set-AzureRmTrafficManagerEndpoint`

Bu örnekte, var olan bir profili tek bir uç nokta ağırlığı değiştirin.

```powershell
$endpoint = Get-AzureRmTrafficManagerEndpoint -Name myendpoint -ProfileName myprofile -ResourceGroupName MyRG -Type ExternalEndpoints
$endpoint.Weight = 20
Set-AzureRmTrafficManagerEndpoint -TrafficManagerEndpoint $endpoint
```

## <a name="enabling-and-disabling-endpoints-and-profiles"></a>Etkinleştirme ve uç noktaları ve profilleri devre dışı bırakma

Trafik Yöneticisi etkin ve devre dışı, etkinleştirme ve tüm profillerini devre dışı bırakma izin vererek yanı sıra tek tek uç noktaları sağlar.
Bu değişiklikler uç noktası veya profili kaynakları alma/güncelleştirme/ayarı tarafından yapılabilir. Bu ortak işlemleri kolaylaştırmak için bunlar ayrılmış cmdlet'leri da desteklenir.

### <a name="example-1-enabling-and-disabling-a-traffic-manager-profile"></a>Örnek 1: Etkinleştirme ve trafik Yöneticisi profili devre dışı bırakma

Trafik Yöneticisi profili etkinleştirmek için `Enable-AzureRmTrafficManagerProfile`. Profil, bir profil nesnesi kullanılarak belirtilebilir. Profil nesnesi kullanarak veya ardışık düzeni aracılığıyla geçirilen '-TrafficManagerProfile' parametresi. Bu örnekte, biz profili ve kaynak grubu adıyla profilini belirtin.

```powershell
Enable-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyResourceGroup
```

Trafik Yöneticisi profili devre dışı bırakmak için:

```powershell
Disable-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyResourceGroup
```

Devre dışı bırakma AzureRmTrafficManagerProfile cmdlet onaylamanızı ister. Bu istemi kullanılarak gizlenebilir. '-Force' parametresi.

### <a name="example-2-enabling-and-disabling-a-traffic-manager-endpoint"></a>Örnek 2: Etkinleştirme ve trafik Yöneticisi uç noktası devre dışı bırakma

Trafik Yöneticisi uç noktasını etkinleştirmek için `Enable-AzureRmTrafficManagerEndpoint`. Uç nokta belirtmek için iki yolu vardır

1. Ardışık Düzen geçirilen TrafficManagerEndpoint nesnesi veya kullanarak '-TrafficManagerEndpoint' parametresi
2. Uç nokta adı, uç nokta türü, profil adı ve kaynak grubu adı kullanma:

```powershell
Enable-AzureRmTrafficManagerEndpoint -Name MyEndpoint -Type AzureEndpoints -ProfileName MyProfile -ResourceGroupName MyRG
```

Benzer şekilde, trafik Yöneticisi uç noktası devre dışı bırakmak için:

```powershell
Disable-AzureRmTrafficManagerEndpoint -Name MyEndpoint -Type AzureEndpoints -ProfileName MyProfile -ResourceGroupName MyRG -Force
```

İle `Disable-AzureRmTrafficManagerProfile`, `Disable-AzureRmTrafficManagerEndpoint` cmdlet için onay ister. Bu istemi kullanılarak gizlenebilir. '-Force' parametresi.

## <a name="delete-a-traffic-manager-endpoint"></a>Trafik Yöneticisi uç noktasını sil

Tekil uç noktalarını kaldırmak için kullanın `Remove-AzureRmTrafficManagerEndpoint` cmdlet:

```powershell
Remove-AzureRmTrafficManagerEndpoint -Name MyEndpoint -Type AzureEndpoints -ProfileName MyProfile -ResourceGroupName MyRG
```

Bu cmdlet onaylamanızı ister. Bu istemi kullanılarak gizlenebilir. '-Force' parametresi.

## <a name="delete-a-traffic-manager-profile"></a>Trafik Yöneticisi profilini Sil

Trafik Yöneticisi profilini silmek için kullanın `Remove-AzureRmTrafficManagerProfile` cmdlet, profil ve kaynak grubu adlarını belirtme:

```powershell
Remove-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG [-Force]
```

Bu cmdlet onaylamanızı ister. Bu istemi kullanılarak gizlenebilir. '-Force' parametresi.

Silinecek profili bir profil nesnesi kullanılarak da belirtilebilir:

```powershell
$profile = Get-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG
Remove-AzureRmTrafficManagerProfile -TrafficManagerProfile $profile [-Force]
```

Bu sıra ayrıca yöneltilen:

```powershell
Get-AzureRmTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG | Remove-AzureRmTrafficManagerProfile [-Force]
```

## <a name="next-steps"></a>Sonraki adımlar

[Trafik Yöneticisi izleme](traffic-manager-monitoring.md)

[Traffic Manager için performans konuları](traffic-manager-performance-considerations.md)
