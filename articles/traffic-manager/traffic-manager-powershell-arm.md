---
title: Azure Traffic Manager'ı yönetmek için PowerShell kullanma
description: Traffic Manager ile Azure Resource Manager için PowerShell kullanma
services: traffic-manager
documentationcenter: na
author: kumudd
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/16/2017
ms.author: kumud
ms.openlocfilehash: 2ce2e2b35d731c3edfed931d158b420e66ed5620
ms.sourcegitcommit: 8313d5bf28fb32e8531cdd4a3054065fa7315bfd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2019
ms.locfileid: "59045756"
---
# <a name="using-powershell-to-manage-traffic-manager"></a>Traffic Manager'ı yönetmek için PowerShell kullanma

Azure Resource Manager, Azure Hizmetleri için tercih edilen Yönetim arabirimidir. Azure Traffic Manager profilleri, Azure Resource Manager tabanlı API'ler ve araçlar kullanılarak yönetilebilir.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="resource-model"></a>Kaynak modeli

Azure Traffic Manager kullanarak bir Traffic Manager profili adı verilen bir ayarlar koleksiyonu yapılandırılır. Bu profil, DNS ayarları, trafik yönlendirme ayarlarını, uç nokta izleme ayarları ve hizmet uç noktaları için trafik yönlendirilir listesini içerir.

Traffic Manager profillerine 'TrafficManagerProfiles' türünde bir kaynak tarafından temsil edilir. REST API düzeyinde her bir profil için URI aşağıdaki gibidir:

    https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Network/trafficManagerProfiles/{profile-name}?api-version={api-version}

## <a name="setting-up-azure-powershell"></a>Azure PowerShell ayarlama

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Bu yönergeler, Microsoft Azure PowerShell kullanırsınız. Aşağıdaki makalede, Azure PowerShell'i yükleme ve yapılandırma açıklanmaktadır.

* [Azure PowerShell'i yükleme ve yapılandırma](/powershell/azure/overview)

Bu makaledeki örnekler, mevcut bir kaynak grubunu sahip olduğunuzu varsaymaktadır. Aşağıdaki komutu kullanarak bir kaynak grubu oluşturabilirsiniz:

```powershell
New-AzResourceGroup -Name MyRG -Location "West US"
```

> [!NOTE]
> Azure Resource Manager, tüm kaynak gruplarının bir konum olmasını gerektirir. Bu konum, varsayılan olarak, bu kaynak grubunda oluşturulan kaynakları için kullanılır. Ancak, traffic Manager profili kaynaklar olduğundan küresel ve bölgesel değil, kaynak grubu konumu seçiminin Azure Traffic Manager üzerindeki herhangi bir etkisi yoktur.

## <a name="create-a-traffic-manager-profile"></a>Traffic Manager profili oluşturma

Traffic Manager profili oluşturmak için kullanın `New-AzTrafficManagerProfile` cmdlet:

```powershell
$profile = New-AzTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG -TrafficRoutingMethod Performance -RelativeDnsName contoso -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
```

Aşağıdaki tabloda parametreler açıklanmaktadır:

| Parametre | Açıklama |
| --- | --- |
| Ad |Traffic Manager profili kaynak kaynak adı. Aynı kaynak grubundaki profiller benzersiz adlara sahip olmalıdır. Bu ad, DNS sorguları için kullanılan DNS ad ayrıdır. |
| ResourceGroupName |Profil kaynağını içeren kaynak grubunun adı. |
| TrafficRoutingMethod |Hangi uç noktaya bir DNS sorgusu yanıtta döndürülen belirlemek için kullanılan trafik yönlendirme yöntemini belirtir. Olası değerler şunlardır: 'Performans', 'Ağırlıklı' veya 'Öncelik'. |
| RelativeDnsName |Ana bilgisayar adı bölümü bu Traffic Manager profili tarafından sağlanan DNS adını belirtir. Bu değer, tam etki alanı adını (FQDN) profili oluşturmak için Azure Traffic Manager tarafından kullanılan DNS etki alanı adı ile birleştirilir. Örneğin, 'contoso' değeri olarak ayarlandığında 'contoso.trafficmanager.net.' olur |
| TTL |DNS-için-yaşam süresi (TTL), saniye cinsinden belirtir. Yerel DNS Çözümleyicileri ve DNS istemcilerini bu Traffic Manager profili için DNS yanıtları önbellek için ne kadar süreyle bu TTL bildirir. |
| MonitorProtocol |Uç nokta izlemesi için kullanılacak protokolü belirtir. Olası değerler şunlardır: 'HTTP' ve 'HTTPS'. |
| MonitorPort |Uç nokta durumunu izlemek için kullanılan TCP bağlantı noktasını belirtir. |
| MonitorPath |Uç nokta sistem durumu için yoklama için kullanılan uç nokta etki alanı adı göreli yolunu belirtir. |

Cmdlet, Azure Traffic Manager profili oluşturur ve PowerShell için karşılık gelen bir profili nesnesini döndürür. Bu noktada, profil, uç nokta içermiyor. Traffic Manager uç noktaları ekleme uç noktaları Traffic Manager profiline ekleme hakkında daha fazla bilgi için bkz.

## <a name="get-a-traffic-manager-profile"></a>Traffic Manager profili Al

Varolan bir Traffic Manager profili nesnesini almak için kullanın `Get-AzTrafficManagerProfle` cmdlet:

```powershell
$profile = Get-AzTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG
```

Bu cmdlet, bir Traffic Manager profili nesnesini döndürür.

## <a name="update-a-traffic-manager-profile"></a>Bir Traffic Manager profilini güncelleştir

Traffic Manager profillerini değiştirme 3 adım işlemi aşağıdaki gibidir:

1. Profili kullanılarak almak `Get-AzTrafficManagerProfile` veya profili tarafından döndürülen `New-AzTrafficManagerProfile`.
2. Profil değiştirin. Ekleyebilir ve uç noktaları kaldırın veya uç nokta veya profili parametrelerini değiştirin. Bu değişiklikler, çevrimdışı işlemlerdir. Yalnızca yerel nesne bellekte profili temsil eden değiştiriyorsunuz.
3. Kullanarak değişikliklerinizi işleyin `Set-AzTrafficManagerProfile` cmdlet'i.

Tüm profil özelliklerini, profilin RelativeDnsName dışında değiştirilebilir. RelativeDnsName değiştirmek için profili ve yeni bir adla yeni bir profil silmeniz gerekir.

Aşağıdaki örnek, profilin TTL değiştirmek gösterilmektedir:

```powershell
$profile = Get-AzTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG
$profile.Ttl = 300
Set-AzTrafficManagerProfile -TrafficManagerProfile $profile
```

Traffic Manager uç noktaları üç tür vardır:

1. **Azure uç noktalarını** Azure'da barındırılan hizmeti
2. **Dış uç noktalar** Azure dışında barındırılan hizmeti
3. **İç içe uç noktalar** iç içe Hiyerarşiler Traffic Manager profili oluşturmak için kullanılır. İç içe uç noktaları, gelişmiş yapılandırmalar karmaşık uygulamalar için trafik yönlendirme sağlar.

Her üç durumda, iki yolla uç noktaları eklenebilir:

1. Daha önce açıklanan 3 adımlık bir işlemdir kullanma. Bu yöntemin avantajı, tek bir güncelleştirmede birden fazla uç nokta değişiklik yapılabilir olmasıdır.
2. New-AzTrafficManagerEndpoint cmdlet'ini kullanarak. Bu cmdlet, tek bir işlemde var olan bir Traffic Manager profiline bir uç nokta ekler.

## <a name="adding-azure-endpoints"></a>Azure uç noktaları ekleme

Azure uç noktaları, Azure üzerinde barındırılan hizmetleri başvuru. Azure uç noktalarını iki türleri desteklenir:

1. Azure App Service
2. (Bir yük dengeleyici veya bir sanal makine NIC eklenebilecek) azure Publicıpaddress kaynakları. Publicıpaddress trafik Yöneticisi'nde kullanılmak üzere atanmış bir DNS adı olmalıdır.

Her durumda:

* Hizmeti 'Targetresourceıd' parametresini kullanarak belirtilen `Add-AzTrafficManagerEndpointConfig` veya `New-AzTrafficManagerEndpoint`.
* 'Target' ve 'EndpointLocation' Targetresourceıd tarafından kapsanan.
* Belirten 'ağırlık' isteğe bağlıdır. Profil 'Ağırlıklı' trafik yönlendirme yöntemini kullanmak için yapılandırılmışsa ağırlıkları yalnızca kullanılır. Aksi takdirde, bunlar yoksayılır. Bu seçenek belirtilmişse, değerin 1 ile 1000 arasında bir sayı olmalıdır. Varsayılan değer, '1' dir.
* Belirten 'öncelik' isteğe bağlıdır. Öncelikleri, yalnızca profili, 'Priority' trafik yönlendirme yöntemini kullanmak için yapılandırılmışsa, kullanılır. Aksi takdirde, bunlar yoksayılır. Geçerli değerler 1 ile 1000 ile düşük değerler daha yüksek bir önceliğe belirten:. Bir uç nokta için belirtilirse, tüm uç noktalar için belirtilmelidir. Atlanırsa, varsayılan değerleri '1'den başlayarak, uç noktaları listelenen sırayla uygulanır.

### <a name="example-1-adding-app-service-endpoints-using-add-aztrafficmanagerendpointconfig"></a>Örnek 1: Kullanarak App Service uç noktaları ekleme `Add-AzTrafficManagerEndpointConfig`

Bu örnekte, biz bir Traffic Manager profili oluşturma ve kullanarak, iki App Service uç noktası ekleme `Add-AzTrafficManagerEndpointConfig` cmdlet'i.

```powershell
$profile = New-AzTrafficManagerProfile -Name myprofile -ResourceGroupName MyRG -TrafficRoutingMethod Performance -RelativeDnsName myapp -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
$webapp1 = Get-AzWebApp -Name webapp1
Add-AzTrafficManagerEndpointConfig -EndpointName webapp1ep -TrafficManagerProfile $profile -Type AzureEndpoints -TargetResourceId $webapp1.Id -EndpointStatus Enabled
$webapp2 = Get-AzWebApp -Name webapp2
Add-AzTrafficManagerEndpointConfig -EndpointName webapp2ep -TrafficManagerProfile $profile -Type AzureEndpoints -TargetResourceId $webapp2.Id -EndpointStatus Enabled
Set-AzTrafficManagerProfile -TrafficManagerProfile $profile
```
### <a name="example-2-adding-a-publicipaddress-endpoint-using-new-aztrafficmanagerendpoint"></a>Örnek 2: Kullanarak bir Publicıpaddress uç noktası ekleme `New-AzTrafficManagerEndpoint`

Bu örnekte, bir genel IP adresi kaynağı Traffic Manager profiline eklenir. Genel IP adresini, yapılandırılmış bir DNS adı olmalıdır ve bir VM'nin NIC'ye ya da bir yük dengeleyiciye bağlı olabilir.

```powershell
$ip = Get-AzPublicIpAddress -Name MyPublicIP -ResourceGroupName MyRG
New-AzTrafficManagerEndpoint -Name MyIpEndpoint -ProfileName MyProfile -ResourceGroupName MyRG -Type AzureEndpoints -TargetResourceId $ip.Id -EndpointStatus Enabled
```

## <a name="adding-external-endpoints"></a>Dış uç noktaları ekleme

Traffic Manager dış uç noktalar Azure dışında barındırılan hizmetler trafiği yönlendirmek için kullanır. Dış uç noktaları ya da Azure uç noktaları ile eklenen kullanarak `Add-AzTrafficManagerEndpointConfig` ardından `Set-AzTrafficManagerProfile`, veya `New-AzTrafficManagerEndpoint`.

Dış uç noktalar belirtirken:

* Uç nokta etki alanı adı 'Target' parametresi kullanılarak belirtilmelidir.
* 'EndpointLocation', 'Performans' trafik yönlendirme yöntemi kullandıysanız gereklidir. Aksi halde isteğe bağlıdır. Değer olmalıdır bir [geçerli bir Azure bölgesi adı](https://azure.microsoft.com/regions/).
* 'Ağırlık' ve 'Öncelik' isteğe bağlıdır.

### <a name="example-1-adding-external-endpoints-using-add-aztrafficmanagerendpointconfig-and-set-aztrafficmanagerprofile"></a>Örnek 1: Dış uç noktaları kullanarak ekleyerek `Add-AzTrafficManagerEndpointConfig` ve `Set-AzTrafficManagerProfile`

Bu örnekte, biz Traffic Manager profili oluşturma, iki dış uç nokta ekleyin ve değişiklikleri uygulayın.

```powershell
$profile = New-AzTrafficManagerProfile -Name myprofile -ResourceGroupName MyRG -TrafficRoutingMethod Performance -RelativeDnsName myapp -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
Add-AzTrafficManagerEndpointConfig -EndpointName eu-endpoint -TrafficManagerProfile $profile -Type ExternalEndpoints -Target app-eu.contoso.com -EndpointLocation "North Europe" -EndpointStatus Enabled
Add-AzTrafficManagerEndpointConfig -EndpointName us-endpoint -TrafficManagerProfile $profile -Type ExternalEndpoints -Target app-us.contoso.com -EndpointLocation "Central US" -EndpointStatus Enabled
Set-AzTrafficManagerProfile -TrafficManagerProfile $profile
```

### <a name="example-2-adding-external-endpoints-using-new-aztrafficmanagerendpoint"></a>Örnek 2: Kullanarak dış uç noktaları ekleme `New-AzTrafficManagerEndpoint`

Bu örnekte, var olan bir profile dış uç noktası ekleriz. Profil, profili ve kaynak grubu adları kullanarak belirtilir.

```powershell
New-AzTrafficManagerEndpoint -Name eu-endpoint -ProfileName MyProfile -ResourceGroupName MyRG -Type ExternalEndpoints -Target app-eu.contoso.com -EndpointStatus Enabled
```

## <a name="adding-nested-endpoints"></a>'İç içe' uç noktaları ekleme

Traffic Manager profillerine tek bir trafik yönlendirme yöntemini belirtir. Ancak, tek bir Traffic Manager profili tarafından sağlanan yönlendirme çok daha karmaşık trafik yönlendirme gerektiren senaryolar da vardır. Traffic Manager profillerini birden fazla trafik yönlendirme yöntemini avantajlarını birleştirin iç içe yerleştirebilirsiniz. İç içe profiller, daha büyük destek ve daha karmaşık uygulama dağıtımları için varsayılan Traffic Manager davranışı geçersiz kılma olanak tanır. Daha ayrıntılı örnekler için bkz: [iç içe Traffic Manager profillerini](traffic-manager-nested-profiles.md).

İç içe uç noktaları, belirli bir uç nokta türü, 'NestedEndpoints' kullanan üst profilde yapılandırılır. İç içe uç noktalar belirtirken:

* Uç nokta 'Targetresourceıd' parametresi kullanılarak belirtilmelidir.
* 'EndpointLocation', 'Performans' trafik yönlendirme yöntemi kullandıysanız gereklidir. Aksi halde isteğe bağlıdır. Değer olmalıdır bir [geçerli bir Azure bölgesi adı](https://azure.microsoft.com/regions/).
* 'Ağırlık' ve 'Öncelik' Azure uç noktalarına hem isteğe bağlı.
* 'MinChildEndpoints' parametresi isteğe bağlıdır. Varsayılan değer, '1' dir. Kullanılabilir uç nokta sayısı bu eşiğin altına düşmesi durumunda, üst profili alt profil 'düşürülmüş' ve üst profili diğer uç noktalardan trafiği yönlendiren göz önünde bulundurur.

### <a name="example-1-adding-nested-endpoints-using-add-aztrafficmanagerendpointconfig-and-set-aztrafficmanagerprofile"></a>Örnek 1: Kullanarak iç içe uç noktaları ekleyerek `Add-AzTrafficManagerEndpointConfig` ve `Set-AzTrafficManagerProfile`

Bu örnekte, biz profilleri yeni bir Traffic Manager alt ve üst öğe oluşturma, alt, üst iç içe bir uç nokta ekleyin ve değişiklikleri uygulayın.

```powershell
$child = New-AzTrafficManagerProfile -Name child -ResourceGroupName MyRG -TrafficRoutingMethod Priority -RelativeDnsName child -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
$parent = New-AzTrafficManagerProfile -Name parent -ResourceGroupName MyRG -TrafficRoutingMethod Performance -RelativeDnsName parent -Ttl 30 -MonitorProtocol HTTP -MonitorPort 80 -MonitorPath "/"
Add-AzTrafficManagerEndpointConfig -EndpointName child-endpoint -TrafficManagerProfile $parent -Type NestedEndpoints -TargetResourceId $child.Id -EndpointStatus Enabled -EndpointLocation "North Europe" -MinChildEndpoints 2
Set-AzTrafficManagerProfile -TrafficManagerProfile $profile
```

Bu örnekte konuyu uzatmamak amacıyla, biz diğer uç noktalar alt veya üst profillere eklemediniz.

### <a name="example-2-adding-nested-endpoints-using-new-aztrafficmanagerendpoint"></a>Örnek 2: Kullanarak iç içe uç noktaları ekleme `New-AzTrafficManagerEndpoint`

Bu örnekte, var olan bir alt profilini iç içe uç noktası olarak var olan bir üst profile ekleriz. Profil, profili ve kaynak grubu adları kullanarak belirtilir.

```powershell
$child = Get-AzTrafficManagerEndpoint -Name child -ResourceGroupName MyRG
New-AzTrafficManagerEndpoint -Name child-endpoint -ProfileName parent -ResourceGroupName MyRG -Type NestedEndpoints -TargetResourceId $child.Id -EndpointStatus Enabled -EndpointLocation "North Europe" -MinChildEndpoints 2
```

## <a name="adding-endpoints-from-another-subscription"></a>Başka bir abonelikten uç noktaları ekleme

Traffic Manager uç noktaları farklı aboneliklere ait çalışabilirsiniz. Gerekli giriş için Traffic Manager'a almak için eklemek istediğiniz uç noktaya sahip bir abonelik için değiştirmeniz gerekir. Ardından Traffic Manager profili ile abonelik geçin ve uç noktası eklemeniz gerekir. Aşağıdaki örnekte genel bir IP adresi ile bunun nasıl yapılacağını gösterir.

```powershell
Set-AzContext -SubscriptionId $EndpointSubscription
$ip = Get-AzPublicIpAddress -Name $IpAddresName -ResourceGroupName $EndpointRG

Set-AzContext -SubscriptionId $trafficmanagerSubscription
New-AzTrafficManagerEndpoint -Name $EndpointName -ProfileName $ProfileName -ResourceGroupName $TrafficManagerRG -Type AzureEndpoints -TargetResourceId $ip.Id -EndpointStatus Enabled
```

## <a name="update-a-traffic-manager-endpoint"></a>Traffic Manager uç noktası güncelleştirme

Var olan bir Traffic Manager uç noktasını güncelleştirmek için iki yolu vardır:

1. Traffic Manager profili kullanarak `Get-AzTrafficManagerProfile`profilinde uç noktası özelliklerini güncelleştirmek ve kullanarak değişiklikleri `Set-AzTrafficManagerProfile`. Bu yöntem, tek bir işlemde birden fazla uç noktasını güncelleştirmek için avantajı vardır.
2. Traffic Manager uç nokta kullanarak `Get-AzTrafficManagerEndpoint`, uç nokta özelliklerini güncelleştirmek ve kullanarak değişiklikleri `Set-AzTrafficManagerEndpoint`. Profilde uç noktaları diziye dizin gerektirmediğinden bu yöntem basittir.

### <a name="example-1-updating-endpoints-using-get-aztrafficmanagerprofile-and-set-aztrafficmanagerprofile"></a>Örnek 1: Uç noktaları kullanarak güncelleştirme `Get-AzTrafficManagerProfile` ve `Set-AzTrafficManagerProfile`

Bu örnekte biz varolan bir profili içinde iki uç nokta önceliği değiştirin.

```powershell
$profile = Get-AzTrafficManagerProfile -Name myprofile -ResourceGroupName MyRG
$profile.Endpoints[0].Priority = 2
$profile.Endpoints[1].Priority = 1
Set-AzTrafficManagerProfile -TrafficManagerProfile $profile
```

### <a name="example-2-updating-an-endpoint-using-get-aztrafficmanagerendpoint-and-set-aztrafficmanagerendpoint"></a>Örnek 2: Bir uç nokta kullanarak güncelleştirme `Get-AzTrafficManagerEndpoint` ve `Set-AzTrafficManagerEndpoint`

Bu örnekte biz varolan profilini tek bir uç nokta ağırlığı değiştirin.

```powershell
$endpoint = Get-AzTrafficManagerEndpoint -Name myendpoint -ProfileName myprofile -ResourceGroupName MyRG -Type ExternalEndpoints
$endpoint.Weight = 20
Set-AzTrafficManagerEndpoint -TrafficManagerEndpoint $endpoint
```

## <a name="enabling-and-disabling-endpoints-and-profiles"></a>Uç noktaları ve profilleri devre dışı bırakma ve etkinleştirme

Traffic Manager, etkin ve devre dışı, tüm profilleri devre dışı bırakma ve etkinleştirme verme yanı sıra bireysel uç noktaları sağlar.
Bu değişiklikler, uç nokta veya profili kaynakları alma/güncelleştirme/ayarı tarafından yapılabilir. Ortak işlemlerini kolaylaştırmak için adanmış cmdlet'leri da desteklenir.

### <a name="example-1-enabling-and-disabling-a-traffic-manager-profile"></a>Örnek 1: Traffic Manager profili devre dışı bırakma ve etkinleştirme

Traffic Manager profili etkinleştirmek için `Enable-AzTrafficManagerProfile`. Profil, bir profil nesnesi kullanılarak belirtilebilir. Profili nesnesini kullanarak veya işlem hattı aracılığıyla geçirilebilir '-TrafficManagerProfile' parametresi. Bu örnekte, biz profili tarafından profil ve kaynak grubu adı belirtin.

```powershell
Enable-AzTrafficManagerProfile -Name MyProfile -ResourceGroupName MyResourceGroup
```

Traffic Manager profili devre dışı bırakmak için:

```powershell
Disable-AzTrafficManagerProfile -Name MyProfile -ResourceGroupName MyResourceGroup
```

Devre dışı bırakma AzTrafficManagerProfile cmdlet'i, onay ister. Bu istemi kullanılarak gizlenebilir. '-Force' parametresi.

### <a name="example-2-enabling-and-disabling-a-traffic-manager-endpoint"></a>Örnek 2: Traffic Manager uç noktası devre dışı bırakma ve etkinleştirme

Traffic Manager uç noktasını etkinleştirmek için `Enable-AzTrafficManagerEndpoint`. Uç nokta belirtmenin iki yolu vardır.

1. İşlem hattı geçirilen bir TrafficManagerEndpoint nesnesi veya kullanılarak '-TrafficManagerEndpoint' parametresi
2. Uç nokta adı, uç nokta türü, profil adı ve kaynak grubu adını kullanarak:

```powershell
Enable-AzTrafficManagerEndpoint -Name MyEndpoint -Type AzureEndpoints -ProfileName MyProfile -ResourceGroupName MyRG
```

Benzer şekilde, bir Traffic Manager uç noktası devre dışı bırakmak için:

```powershell
Disable-AzTrafficManagerEndpoint -Name MyEndpoint -Type AzureEndpoints -ProfileName MyProfile -ResourceGroupName MyRG -Force
```

Olduğu gibi `Disable-AzTrafficManagerProfile`, `Disable-AzTrafficManagerEndpoint` cmdlet'i, onay istemi. Bu istemi kullanılarak gizlenebilir. '-Force' parametresi.

## <a name="delete-a-traffic-manager-endpoint"></a>Traffic Manager uç noktasını sil

Tekil uç noktalarını kaldırmak için `Remove-AzTrafficManagerEndpoint` cmdlet:

```powershell
Remove-AzTrafficManagerEndpoint -Name MyEndpoint -Type AzureEndpoints -ProfileName MyProfile -ResourceGroupName MyRG
```

Bu cmdlet, onay ister. Bu istemi kullanılarak gizlenebilir. '-Force' parametresi.

## <a name="delete-a-traffic-manager-profile"></a>Bir Traffic Manager profilini Sil

Bir Traffic Manager profilini silmek için kullanın `Remove-AzTrafficManagerProfile` cmdlet'i, profil ve kaynak grubu adlarını belirten:

```powershell
Remove-AzTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG [-Force]
```

Bu cmdlet, onay ister. Bu istemi kullanılarak gizlenebilir. '-Force' parametresi.

Bir profili nesnesini kullanarak silinecek profili de belirtilebilir:

```powershell
$profile = Get-AzTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG
Remove-AzTrafficManagerProfile -TrafficManagerProfile $profile [-Force]
```

Bu sıra ayrıca yöneltilebilir:

```powershell
Get-AzTrafficManagerProfile -Name MyProfile -ResourceGroupName MyRG | Remove-AzTrafficManagerProfile [-Force]
```

## <a name="next-steps"></a>Sonraki adımlar

[Traffic Manager izleme](traffic-manager-monitoring.md)

[Traffic Manager için performans konuları](traffic-manager-performance-considerations.md)
