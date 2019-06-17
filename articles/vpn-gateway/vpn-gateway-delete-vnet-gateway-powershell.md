---
title: 'Bir sanal ağ geçidini silin: PowerShell: Azure Resource Manager | Microsoft Docs'
description: Resource Manager dağıtım modelinde PowerShell kullanarak bir sanal ağ geçidini silin.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.date: 02/07/2019
ms.author: cherylmc
ms.topic: conceptual
ms.openlocfilehash: 7b9503b2db14d4de6c4c8cf983c42bccd6f9f8fd
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66157441"
---
# <a name="delete-a-virtual-network-gateway-using-powershell"></a>PowerShell kullanarak bir sanal ağ geçidini silme
> [!div class="op_single_selector"]
> * [Azure portal](vpn-gateway-delete-vnet-gateway-portal.md)
> * [PowerShell](vpn-gateway-delete-vnet-gateway-powershell.md)
> * [PowerShell (klasik)](vpn-gateway-delete-vnet-gateway-classic-powershell.md)
>
>

Birkaç farklı yaklaşımların bir sanal ağ geçidi bir VPN ağ geçidi yapılandırması için silmek istediğiniz zaman izleyebileceğiniz vardır.

- Her şeyi silin ve baştan, bir test ortamı durumunda olduğu gibi istediğiniz kaynak grubunu silebilirsiniz. Bir kaynak grubu sildiğinizde grup içindeki tüm kaynakları siler. Bu yöntem, yalnızca tüm kaynakların kaynak grubunda korumak istemiyorsanız önerilir. Seçime bağlı olarak, bu yaklaşımı kullanarak yalnızca birkaç kaynak silinemiyor.

- Kaynaklardan bazıları, kaynak grubunuzda tutmak istiyorsanız, bir sanal ağ geçidi siliniyor biraz daha karmaşık olabilir. Sanal ağ geçidi silebilmeniz için önce ağ geçidinde bağımlı olan tüm kaynakları silmeniz gerekir. Oluşturduğunuz bağlantı türünü ve her bağlantı için bağımlı kaynakları kullandığınızda izleyeceğiniz adımlar bağlıdır.

## <a name="before-beginning"></a>Başlamadan önce

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

### <a name="1-download-the-latest-azure-resource-manager-powershell-cmdlets"></a>1. En son Azure Resource Manager PowerShell cmdlet'lerini indirin.

Azure Resource Manager PowerShell cmdlet'lerinin en son sürümünü indirip yeniden açın. Karşıdan yükleme ve PowerShell cmdlet'lerini yükleme hakkında daha fazla bilgi için bkz. [Azure PowerShell'i yükleme ve yapılandırma işlemini](/powershell/azure/overview).

### <a name="2-connect-to-your-azure-account"></a>2. Azure hesabınıza bağlanın.

PowerShell konsolunuzu açın ve hesabınıza bağlanın. Bağlanmanıza yardımcı olması için aşağıdaki örneği kullanın:

```powershell
Connect-AzAccount
```

Hesapla ilişkili abonelikleri kontrol edin.

```powershell
Get-AzSubscription
```

Birden fazla aboneliğiniz varsa, kullanmak istediğiniz aboneliği belirtin.

```powershell
Select-AzSubscription -SubscriptionName "Replace_with_your_subscription_name"
```

## <a name="S2S"></a>Siteden siteye VPN ağ geçidini silme

S2S yapılandırmasının için sanal ağ geçidini silmek için önce sanal ağ geçidi için ilgili her bir kaynak silmeniz gerekir. Belirli bir sırada bağımlılıklar nedeniyle kaynakları silinmesi gerekir. Çıkış sonucu diğer değerleri olmasına rağmen aşağıda bazı örnekler ile çalışırken değerler belirtilmelidir. Aşağıdaki değerleri örneklerde gösterim amaçları için kullanırız:

VNet adı: VNet1<br>
Kaynak grubu adı: RG1<br>
Sanal ağ geçidi adı: GW1<br>

Aşağıdaki adımlar Resource Manager dağıtım modeli için geçerlidir.

### <a name="1-get-the-virtual-network-gateway-that-you-want-to-delete"></a>1. Silmek istediğiniz sanal ağ geçidini alın.

```powershell
$GW=get-Azvirtualnetworkgateway -Name "GW1" -ResourceGroupName "RG1"
```

### <a name="2-check-to-see-if-the-virtual-network-gateway-has-any-connections"></a>2. Sanal ağ geçidi herhangi bir bağlantı olup olmadığını denetleyin.

```powershell
get-Azvirtualnetworkgatewayconnection -ResourceGroupName "RG1" | where-object {$_.VirtualNetworkGateway1.Id -eq $GW.Id}
$Conns=get-Azvirtualnetworkgatewayconnection -ResourceGroupName "RG1" | where-object {$_.VirtualNetworkGateway1.Id -eq $GW.Id}
```

### <a name="3-delete-all-connections"></a>3. Tüm bağlantıları silin.

Her bağlantı silme işlemini onaylamanız istenebilir.

```powershell
$Conns | ForEach-Object {Remove-AzVirtualNetworkGatewayConnection -Name $_.name -ResourceGroupName $_.ResourceGroupName}
```

### <a name="4-delete-the-virtual-network-gateway"></a>4. Sanal ağ geçidini silin.

Ağ geçidini silme işlemini onaylamanız istenebilir. Sanal ağ geçidi siliniyor, S2S yapılandırmasının yanı sıra bu sanal ağa bir P2S yapılandırması varsa, otomatik olarak tüm P2S istemcileri uyarmadan keser.


```powershell
Remove-AzVirtualNetworkGateway -Name "GW1" -ResourceGroupName "RG1"
```

Bu noktada, sanal ağ geçidinizin silindi. Sonraki adımlar, artık kullanılmayan tüm kaynakları silmek için kullanabilirsiniz.

### <a name="5-delete-the-local-network-gateways"></a>5, yerel ağ geçitlerini silin.

Karşılık gelen yerel ağ geçitleri listesini alın.

```powershell
$LNG=Get-AzLocalNetworkGateway -ResourceGroupName "RG1" | where-object {$_.Id -In $Conns.LocalNetworkGateway2.Id}
```

Yerel ağ geçitlerini silin. Her yerel ağ geçidini silme işlemini onaylamanız istenebilir.

```powershell
$LNG | ForEach-Object {Remove-AzLocalNetworkGateway -Name $_.Name -ResourceGroupName $_.ResourceGroupName}
```

### <a name="6-delete-the-public-ip-address-resources"></a>6. Genel IP adresi kaynakları silin.

Sanal ağ geçidinin IP yapılandırmaları alın.

```powershell
$GWIpConfigs = $Gateway.IpConfigurations
```

Bu sanal ağ geçidi için kullanılan genel IP adresi kaynakları listesini alın. Etkin-etkin sanal ağ geçidi, iki genel IP adresleri görürsünüz.

```powershell
$PubIP=Get-AzPublicIpAddress | where-object {$_.Id -In $GWIpConfigs.PublicIpAddress.Id}
```

Genel IP kaynakları silin.

```powershell
$PubIP | foreach-object {remove-AzpublicIpAddress -Name $_.Name -ResourceGroupName "RG1"}
```

### <a name="7-delete-the-gateway-subnet-and-set-the-configuration"></a>7. Ağ geçidi alt ağını silin ve yapılandırmayı ayarlayın.

```powershell
$GWSub = Get-AzVirtualNetwork -ResourceGroupName "RG1" -Name "VNet1" | Remove-AzVirtualNetworkSubnetConfig -Name "GatewaySubnet"
Set-AzVirtualNetwork -VirtualNetwork $GWSub
```

## <a name="v2v"></a>VNet-VNet VPN ağ geçidini silme

V2V yapılandırması için bir sanal ağ geçidini silmek için önce sanal ağ geçidi için ilgili her bir kaynak silmeniz gerekir. Belirli bir sırada bağımlılıklar nedeniyle kaynakları silinmesi gerekir. Çıkış sonucu diğer değerleri olmasına rağmen aşağıda bazı örnekler ile çalışırken değerler belirtilmelidir. Aşağıdaki değerleri örneklerde gösterim amaçları için kullanırız:

VNet adı: VNet1<br>
Kaynak grubu adı: RG1<br>
Sanal ağ geçidi adı: GW1<br>

Aşağıdaki adımlar Resource Manager dağıtım modeli için geçerlidir.

### <a name="1-get-the-virtual-network-gateway-that-you-want-to-delete"></a>1. Silmek istediğiniz sanal ağ geçidini alın.

```powershell
$GW=get-Azvirtualnetworkgateway -Name "GW1" -ResourceGroupName "RG1"
```

### <a name="2-check-to-see-if-the-virtual-network-gateway-has-any-connections"></a>2. Sanal ağ geçidi herhangi bir bağlantı olup olmadığını denetleyin.

```powershell
get-Azvirtualnetworkgatewayconnection -ResourceGroupName "RG1" | where-object {$_.VirtualNetworkGateway1.Id -eq $GW.Id}
```
 
Farklı bir kaynak grubu kapsamındaki diğer sanal ağ geçidi bağlantıları olabilir. Her ek kaynak grubunda başka bağlantılar olup olmadığını denetleyin. Bu örnekte biz RG2 gelen bağlantılar için iade ederler. Bu, her kaynak grubu için hangi sanal ağ geçidi bağlantınız sahip çalıştırın.

```powershell
get-Azvirtualnetworkgatewayconnection -ResourceGroupName "RG2" | where-object {$_.VirtualNetworkGateway2.Id -eq $GW.Id}
```

### <a name="3-get-the-list-of-connections-in-both-directions"></a>3. Her iki yönde bağlantılar listesini alın.

Bu, bir VNet-VNet yapılandırmasını olduğu için her iki yönde bağlantılar listesine gerekir.

```powershell
$ConnsL=get-Azvirtualnetworkgatewayconnection -ResourceGroupName "RG1" | where-object {$_.VirtualNetworkGateway1.Id -eq $GW.Id}
```
 
Bu örnekte biz RG2 gelen bağlantılar için iade ederler. Bu, her kaynak grubu için hangi sanal ağ geçidi bağlantınız sahip çalıştırın.

```powershell
 $ConnsR=get-Azvirtualnetworkgatewayconnection -ResourceGroupName "<NameOfResourceGroup2>" | where-object {$_.VirtualNetworkGateway2.Id -eq $GW.Id}
 ```

### <a name="4-delete-all-connections"></a>4. Tüm bağlantıları silin.

Her bağlantı silme işlemini onaylamanız istenebilir.

```powershell
$ConnsL | ForEach-Object {Remove-AzVirtualNetworkGatewayConnection -Name $_.name -ResourceGroupName $_.ResourceGroupName}
$ConnsR | ForEach-Object {Remove-AzVirtualNetworkGatewayConnection -Name $_.name -ResourceGroupName $_.ResourceGroupName}
```

### <a name="5-delete-the-virtual-network-gateway"></a>5. Sanal ağ geçidini silin.

Sanal ağ geçidini silme işlemini onaylamanız istenebilir. V2V yapılandırmanızı yanı sıra sanal ağlarınıza P2S yapılandırmaları varsa, sanal ağ geçitlerini silme uyarısı olmadan tüm P2S istemcileri otomatik olarak keser.

```powershell
Remove-AzVirtualNetworkGateway -Name "GW1" -ResourceGroupName "RG1"
```

Bu noktada, sanal ağ geçidinizin silindi. Sonraki adımlar, artık kullanılmayan tüm kaynakları silmek için kullanabilirsiniz.

### <a name="6-delete-the-public-ip-address-resources"></a>6. Genel IP adresi kaynakları silme

Sanal ağ geçidinin IP yapılandırmaları alın.

```powershell
$GWIpConfigs = $Gateway.IpConfigurations
```

Bu sanal ağ geçidi için kullanılan genel IP adresi kaynakları listesini alın. Etkin-etkin sanal ağ geçidi, iki genel IP adresleri görürsünüz.

```powershell
$PubIP=Get-AzPublicIpAddress | where-object {$_.Id -In $GWIpConfigs.PublicIpAddress.Id}
```

Genel IP kaynakları silin. Genel IP, silmeyi onaylamanız istenebilir.

```powershell
$PubIP | foreach-object {remove-AzpublicIpAddress -Name $_.Name -ResourceGroupName "<NameOfResourceGroup1>"}
```

### <a name="7-delete-the-gateway-subnet-and-set-the-configuration"></a>7. Ağ geçidi alt ağını silin ve yapılandırmayı ayarlayın.

```powershell
$GWSub = Get-AzVirtualNetwork -ResourceGroupName "RG1" -Name "VNet1" | Remove-AzVirtualNetworkSubnetConfig -Name "GatewaySubnet"
Set-AzVirtualNetwork -VirtualNetwork $GWSub
```

## <a name="deletep2s"></a>Noktadan siteye VPN ağ geçidini silme

P2S yapılandırması için bir sanal ağ geçidini silmek için önce sanal ağ geçidi için ilgili her bir kaynak silmeniz gerekir. Belirli bir sırada bağımlılıklar nedeniyle kaynakları silinmesi gerekir. Çıkış sonucu diğer değerleri olmasına rağmen aşağıda bazı örnekler ile çalışırken değerler belirtilmelidir. Aşağıdaki değerleri örneklerde gösterim amaçları için kullanırız:

VNet adı: VNet1<br>
Kaynak grubu adı: RG1<br>
Sanal ağ geçidi adı: GW1<br>

Aşağıdaki adımlar Resource Manager dağıtım modeli için geçerlidir.


>[!NOTE]
> VPN ağ geçidini sildiğinizde, uyarı olmadan sanal ağdan bağlanan tüm istemciler kesilir.
>
>

### <a name="1-get-the-virtual-network-gateway-that-you-want-to-delete"></a>1. Silmek istediğiniz sanal ağ geçidini alın.

```powershell
$GW=get-Azvirtualnetworkgateway -Name "GW1" -ResourceGroupName "RG1"
```

### <a name="2-delete-the-virtual-network-gateway"></a>2. Sanal ağ geçidini silin.

Sanal ağ geçidini silme işlemini onaylamanız istenebilir.

```powershell
Remove-AzVirtualNetworkGateway -Name "GW1" -ResourceGroupName "RG1"
```

Bu noktada, sanal ağ geçidinizin silindi. Sonraki adımlar, artık kullanılmayan tüm kaynakları silmek için kullanabilirsiniz.

### <a name="3-delete-the-public-ip-address-resources"></a>3. Genel IP adresi kaynakları silme

Sanal ağ geçidinin IP yapılandırmaları alın.

```powershell
$GWIpConfigs = $Gateway.IpConfigurations
```

Bu sanal ağ geçidi için kullanılan genel IP adresleri'nın listesini alın. Etkin-etkin sanal ağ geçidi, iki genel IP adresleri görürsünüz.

```powershell
$PubIP=Get-AzPublicIpAddress | where-object {$_.Id -In $GWIpConfigs.PublicIpAddress.Id}
```

Genel IP'ler silin. Genel IP, silmeyi onaylamanız istenebilir.

```powershell
$PubIP | foreach-object {remove-AzpublicIpAddress -Name $_.Name -ResourceGroupName "<NameOfResourceGroup1>"}
```

### <a name="4-delete-the-gateway-subnet-and-set-the-configuration"></a>4. Ağ geçidi alt ağını silin ve yapılandırmayı ayarlayın.

```powershell
$GWSub = Get-AzVirtualNetwork -ResourceGroupName "RG1" -Name "VNet1" | Remove-AzVirtualNetworkSubnetConfig -Name "GatewaySubnet"
Set-AzVirtualNetwork -VirtualNetwork $GWSub
```

## <a name="delete"></a>Kaynak grubunu silerek bir VPN ağ geçidini silme

Kaynak grubunda kaynaklarınızın herhangi etme konusunda endişe değildir ve baştan başlamak istiyorsanız, bir kaynak grubunun tamamını silebilirsiniz. Bu, her şeyi kaldırmak için hızlı bir yoludur. Aşağıdaki adımlar, yalnızca Resource Manager dağıtım modeli için geçerlidir.

### <a name="1-get-a-list-of-all-the-resource-groups-in-your-subscription"></a>1. Aboneliğinizdeki tüm kaynak gruplarının bir listesini alın.

```powershell
Get-AzResourceGroup
```

### <a name="2-locate-the-resource-group-that-you-want-to-delete"></a>2. Silmek istediğiniz kaynak grubunu bulun.

Silin ve bu kaynak grubunda kaynakların listesini görüntülemek istediğiniz kaynak grubunu bulun. Örnekte, RG1 kaynak grubunun adıdır. Tüm kaynakların bir listesini almak için örneği değiştirin.

```powershell
Find-AzResource -ResourceGroupNameContains RG1
```

### <a name="3-verify-the-resources-in-the-list"></a>3. Kaynak listesinde doğrulayın.

Listenin iade edildiğinde, kaynak grubunun yanı sıra kaynak grubunun tüm kaynaklarını silmek istediğinizi doğrulamak için gözden geçirin. Kaynak grubunda kaynaklardan bazıları tutmak istiyorsanız, ağ geçidi silmek için bu makalenin önceki bölümlerine adımları kullanın.

### <a name="4-delete-the-resource-group-and-resources"></a>4. Kaynak grubu ve kaynakları silin.

Kaynak grubu ve kaynak grubunda yer alan tüm kaynak silmek için örneği değiştirin ve çalıştırın.

```powershell
Remove-AzResourceGroup -Name RG1
```

### <a name="5-check-the-status"></a>5. Durumu denetleyin.

Azure tüm kaynakları silmek biraz zaman alabilir. Bu cmdlet kullanarak kaynak grubunuzun durumunu kontrol edebilirsiniz.

```powershell
Get-AzResourceGroup -ResourceGroupName RG1
```

Döndürülen sonuç 'Başarılı' gösterir.

```
ResourceGroupName : RG1
Location          : eastus
ProvisioningState : Succeeded
```
