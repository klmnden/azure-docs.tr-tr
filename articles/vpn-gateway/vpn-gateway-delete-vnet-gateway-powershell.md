---
title: "Bir sanal ağ geçidini silmek: PowerShell: Azure Resource Manager | Microsoft Docs"
description: "Resource Manager dağıtım modelinde PowerShell kullanarak bir sanal ağ geçidini silin."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: 
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/20/2017
ms.author: cherylmc
ms.openlocfilehash: 4d0f085423d5bd60b24d88649ee1d77bcd1d009f
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2017
---
# <a name="delete-a-virtual-network-gateway-using-powershell"></a>PowerShell kullanarak bir sanal ağ geçidini Sil
> [!div class="op_single_selector"]
> * [Azure portal](vpn-gateway-delete-vnet-gateway-portal.md)
> * [PowerShell](vpn-gateway-delete-vnet-gateway-powershell.md)
> * [PowerShell (klasik)](vpn-gateway-delete-vnet-gateway-classic-powershell.md)
>
>

Birkaç VPN ağ geçidi yapılandırması için bir sanal ağ geçidi silmek istediğinizde uygulayabileceğiniz çeşitli yaklaşımlar vardır.

- Her şeyi silin ve üzerinde bir test ortamı durumda olduğu gibi başlatmak istiyorsanız, kaynak grubunu silebilirsiniz. Bir kaynak grubunu silmek gruptaki tüm kaynakları siler. Bu yöntem olduğundan tüm kaynakların kaynak grubunda saklamak istemiyorsanız yalnızca önerilir. Bu yaklaşımı kullanarak yalnızca birkaç kaynakları seçmeli olarak silemezsiniz.

- Bazı kaynakların kaynak grubunda tutmak istiyorsanız, bir sanal ağ geçidi silinmesi biraz daha karmaşık olabilir. Sanal ağ geçidi silmeden önce ağ geçidinde bağımlı herhangi bir kaynağa silmeniz gerekir. İzleyeceğiniz adımlar oluşturduğunuz bağlantı türünü ve her bağlantı için bağımlı kaynakları bağlıdır.

## <a name="before-beginning"></a>Başlamadan önce

### <a name="1-download-the-latest-azure-resource-manager-powershell-cmdlets"></a>1. En son Azure Resource Manager PowerShell cmdlet'lerini indirin.

Azure Resource Manager PowerShell cmdlet'lerinin en son sürümünü yükleyip yeniden açın. Karşıdan yükleme ve PowerShell cmdlet'leri hakkında daha fazla bilgi için bkz: [Azure PowerShell'i yükleme ve yapılandırma nasıl](/powershell/azure/overview).

### <a name="2-connect-to-your-azure-account"></a>2. Azure hesabınıza bağlanın.

PowerShell konsolunuzu açın ve hesabınıza bağlanın. Bağlanmanıza yardımcı olması için aşağıdaki örneği kullanın:

```powershell
Login-AzureRmAccount
```

Hesapla ilişkili abonelikleri kontrol edin.

```powershell
Get-AzureRmSubscription
```

Birden fazla aboneliğiniz varsa, kullanmak istediğiniz aboneliği belirtin.

```powershell
Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"
```

## <a name="S2S"></a>Siteden siteye VPN ağ geçidini silin

Bir sanal ağ geçidi S2S yapılandırması için silmek için önce sanal ağ geçidi için ilgili her bir kaynağın silmeniz gerekir. Belirli bir sırada bağımlılıkları nedeniyle kaynakları silinmesi gerekir. Diğer değerleri bir çıktı sonuç durumdayken aşağıda bazı örnekler ile çalışırken değerler belirtilmelidir. Aşağıdaki değerleri örneklerde tanıtım amacıyla kullanırız:

Sanal ağ adı: VNet1<br>
Kaynak grubu adı: RG1<br>
Sanal ağ geçidi adı: GW1<br>

Aşağıdaki adımlar Resource Manager dağıtım modeli için geçerlidir.

### <a name="1-get-the-virtual-network-gateway-that-you-want-to-delete"></a>1. Silmek istediğiniz sanal ağ geçidi alın.

```powershell
$Gateway=get-azurermvirtualnetworkgateway -Name "GW1" -ResourceGroupName "RG1"
```

### <a name="2-check-to-see-if-the-virtual-network-gateway-has-any-connections"></a>2. Sanal ağ geçidi herhangi bir bağlantısı olup olmadığını denetleyin.

```powershell
get-azurermvirtualnetworkgatewayconnection -ResourceGroupName "RG1" | where-object {$_.VirtualNetworkGateway1.Id -eq $GW.Id}
$Conns=get-azurermvirtualnetworkgatewayconnection -ResourceGroupName "RG1" | where-object {$_.VirtualNetworkGateway1.Id -eq $GW.Id}
```

### <a name="3-delete-all-connections"></a>3. Tüm bağlantılar silin.

Her bağlantıların silme işlemini onaylamak için istenebilir.

```powershell
$Conns | ForEach-Object {Remove-AzureRmVirtualNetworkGatewayConnection -Name $_.name -ResourceGroupName $_.ResourceGroupName}
```

### <a name="4-delete-the-virtual-network-gateway"></a>4. Sanal ağ geçidini silin.

Ağ geçidini silme işlemini onaylamak için istenebilir. Bu sanal ağa P2S yapılandırma S2S yapılandırmanızı yanı sıra varsa, sanal ağ geçidi siliniyor uyarmadan tüm P2S istemcileri otomatik olarak bağlantısını keser.


```powershell
Remove-AzureRmVirtualNetworkGateway -Name "GW1" -ResourceGroupName "RG1"
```

Bu noktada, sanal ağ geçidinizi silindi. Sonraki adımlar, artık kullanılmayan kaynakları silmek için kullanabilirsiniz.

### <a name="5-delete-the-local-network-gateways"></a>5 yerel ağ geçitlerini silin.

Karşılık gelen yerel ağ geçitleri listesini alın.

```powershell
$LNG=Get-AzureRmLocalNetworkGateway -ResourceGroupName "RG1" | where-object {$_.Id -In $Conns.LocalNetworkGateway2.Id}
```

Yerel ağ geçitleri silin. Her bir yerel ağ geçidi silmeyi onaylamak için istenebilir.

```powershell
$LNG | ForEach-Object {Remove-AzureRmLocalNetworkGateway -Name $_.Name -ResourceGroupName $_.ResourceGroupName}
```

### <a name="6-delete-the-public-ip-address-resources"></a>6. Genel IP adresi kaynakları silin.

Sanal ağ geçidinin IP yapılandırmalarını alma.

```powershell
$GWIpConfigs = $Gateway.IpConfigurations
```

Bu sanal ağ geçidi için kullanılan genel IP adresi kaynakları listesini alın. Sanal ağ geçidi etkin-etkin iki genel IP adresleri görürsünüz.

```powershell
$PubIP=Get-AzureRmPublicIpAddress | where-object {$_.Id -In $GWIpConfigs.PublicIpAddress.Id}
```

Genel IP kaynakları silin.

```powershell
$PubIP | foreach-object {remove-azurermpublicIpAddress -Name $_.Name -ResourceGroupName "RG1"}
```

### <a name="7-delete-the-gateway-subnet-and-set-the-configuration"></a>7. Ağ geçidi alt ağını silmek ve yapılandırmasını ayarlayın.

```powershell
$GWSub = Get-AzureRmVirtualNetwork -ResourceGroupName "RG1" -Name "VNet1" | Remove-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet"
Set-AzureRmVirtualNetwork -VirtualNetwork $GWSub
```

## <a name="v2v"></a>VNet-VNet VPN ağ geçidini silin

V2V yapılandırması için bir sanal ağ geçidi silmek için önce sanal ağ geçidi için ilgili her bir kaynağın silmeniz gerekir. Belirli bir sırada bağımlılıkları nedeniyle kaynakları silinmesi gerekir. Diğer değerleri bir çıktı sonuç durumdayken aşağıda bazı örnekler ile çalışırken değerler belirtilmelidir. Aşağıdaki değerleri örneklerde tanıtım amacıyla kullanırız:

Sanal ağ adı: VNet1<br>
Kaynak grubu adı: RG1<br>
Sanal ağ geçidi adı: GW1<br>

Aşağıdaki adımlar Resource Manager dağıtım modeli için geçerlidir.

### <a name="1-get-the-virtual-network-gateway-that-you-want-to-delete"></a>1. Silmek istediğiniz sanal ağ geçidi alın.

```powershell
$Gateway=get-azurermvirtualnetworkgateway -Name "GW1" -ResourceGroupName "RG1"
```

### <a name="2-check-to-see-if-the-virtual-network-gateway-has-any-connections"></a>2. Sanal ağ geçidi herhangi bir bağlantısı olup olmadığını denetleyin.

```powershell
get-azurermvirtualnetworkgatewayconnection -ResourceGroupName "RG1" | where-object {$_.VirtualNetworkGateway1.Id -eq $GW.Id}
```
 
Farklı bir kaynak grubu parçası olan diğer sanal ağ geçidi bağlantıları olabilir. Her ek kaynak grubunda ek bağlantılar olup olmadığını denetleyin. Bu örnekte, biz RG2 gelen bağlantıları için denetleniyor. Bu, her kaynak grubu için hangi sanal ağ geçidi bağlantı olabilir sahip çalıştırın.

```powershell
get-azurermvirtualnetworkgatewayconnection -ResourceGroupName "RG2" | where-object {$_.VirtualNetworkGateway2.Id -eq $GW.Id}
```

### <a name="3-get-the-list-of-connections-in-both-directions"></a>3. Her iki yönde de bağlantıların listesini alır.

Bu VNet-VNet yapılandırması olduğundan, her iki yönde bağlantıların listesini gerekir.

```powershell
$ConnsL=get-azurermvirtualnetworkgatewayconnection -ResourceGroupName "RG1" | where-object {$_.VirtualNetworkGateway1.Id -eq $GW.Id}
```
 
Bu örnekte, biz RG2 gelen bağlantıları için denetleniyor. Bu, her kaynak grubu için hangi sanal ağ geçidi bağlantı olabilir sahip çalıştırın.

```powershell
 $ConnsR=get-azurermvirtualnetworkgatewayconnection -ResourceGroupName "<NameOfResourceGroup2>" | where-object {$_.VirtualNetworkGateway2.Id -eq $GW.Id}
 ```

### <a name="4-delete-all-connections"></a>4. Tüm bağlantılar silin.

Her bağlantıların silme işlemini onaylamak için istenebilir.

```powershell
$ConnsL | ForEach-Object {Remove-AzureRmVirtualNetworkGatewayConnection -Name $_.name -ResourceGroupName $_.ResourceGroupName}
$ConnsR | ForEach-Object {Remove-AzureRmVirtualNetworkGatewayConnection -Name $_.name -ResourceGroupName $_.ResourceGroupName}
```

### <a name="5-delete-the-virtual-network-gateway"></a>5. Sanal ağ geçidini silin.

Sanal ağ geçidini silme işlemini onaylamak için istenebilir. V2V yapılandırmanızı ek olarak, sanal ağlar P2S yapılandırmaları varsa, sanal ağ geçitlerini silme uyarmadan tüm P2S istemcileri otomatik olarak bağlantısını keser.

```powershell
Remove-AzureRmVirtualNetworkGateway -Name "GW1" -ResourceGroupName "RG1"
```

Bu noktada, sanal ağ geçidinizi silindi. Sonraki adımlar, artık kullanılmayan kaynakları silmek için kullanabilirsiniz.

### <a name="6-delete-the-public-ip-address-resources"></a>6. Genel IP adresi kaynakları silme

Sanal ağ geçidinin IP yapılandırmalarını alma.

```powershell
$GWIpConfigs = $Gateway.IpConfigurations
```

Bu sanal ağ geçidi için kullanılan genel IP adresi kaynakları listesini alın. Sanal ağ geçidi etkin-etkin iki genel IP adresleri görürsünüz.

```powershell
$PubIP=Get-AzureRmPublicIpAddress | where-object {$_.Id -In $GWIpConfigs.PublicIpAddress.Id}
```

Genel IP kaynakları silin. Genel IP silme işlemini onaylamak için istenebilir.

```powershell
$PubIP | foreach-object {remove-azurermpublicIpAddress -Name $_.Name -ResourceGroupName "<NameOfResourceGroup1>"}
```

### <a name="7-delete-the-gateway-subnet-and-set-the-configuration"></a>7. Ağ geçidi alt ağını silmek ve yapılandırmasını ayarlayın.

```powershell
$GWSub = Get-AzureRmVirtualNetwork -ResourceGroupName "RG1" -Name "VNet1" | Remove-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet"
Set-AzureRmVirtualNetwork -VirtualNetwork $GWSub
```

## <a name="deletep2s"></a>Bir noktadan siteye VPN ağ geçidini silin

P2S yapılandırması için bir sanal ağ geçidi silmek için önce sanal ağ geçidi için ilgili her bir kaynağın silmeniz gerekir. Belirli bir sırada bağımlılıkları nedeniyle kaynakları silinmesi gerekir. Diğer değerleri bir çıktı sonuç durumdayken aşağıda bazı örnekler ile çalışırken değerler belirtilmelidir. Aşağıdaki değerleri örneklerde tanıtım amacıyla kullanırız:

Sanal ağ adı: VNet1<br>
Kaynak grubu adı: RG1<br>
Sanal ağ geçidi adı: GW1<br>

Aşağıdaki adımlar Resource Manager dağıtım modeli için geçerlidir.


>[!NOTE]
> VPN ağ geçidini sildiğinizde, tüm bağlı istemcileri uyarmadan VNet kesilecektir.
>
>

### <a name="1-get-the-virtual-network-gateway-that-you-want-to-delete"></a>1. Silmek istediğiniz sanal ağ geçidi alın.

```powershell
$Gateway=get-azurermvirtualnetworkgateway -Name "GW1" -ResourceGroupName "RG1"
```

### <a name="2-delete-the-virtual-network-gateway"></a>2. Sanal ağ geçidini silin.

Sanal ağ geçidini silme işlemini onaylamak için istenebilir.

```powershell
Remove-AzureRmVirtualNetworkGateway -Name "GW1" -ResourceGroupName "RG1"
```

Bu noktada, sanal ağ geçidinizi silindi. Sonraki adımlar, artık kullanılmayan kaynakları silmek için kullanabilirsiniz.

### <a name="3-delete-the-public-ip-address-resources"></a>3. Genel IP adresi kaynakları silme

Sanal ağ geçidinin IP yapılandırmalarını alma.

```powershell
$GWIpConfigs = $Gateway.IpConfigurations
```

Bu sanal ağ geçidi için kullanılan genel IP adresleri listesi alınamadı. Sanal ağ geçidi etkin-etkin iki genel IP adresleri görürsünüz.

```powershell
$PubIP=Get-AzureRmPublicIpAddress | where-object {$_.Id -In $GWIpConfigs.PublicIpAddress.Id}
```

Genel IP'ler silin. Genel IP silme işlemini onaylamak için istenebilir.

```powershell
$PubIP | foreach-object {remove-azurermpublicIpAddress -Name $_.Name -ResourceGroupName "<NameOfResourceGroup1>"}
```

### <a name="4-delete-the-gateway-subnet-and-set-the-configuration"></a>4. Ağ geçidi alt ağını silmek ve yapılandırmasını ayarlayın.

```powershell
$GWSub = Get-AzureRmVirtualNetwork -ResourceGroupName "RG1" -Name "VNet1" | Remove-AzureRmVirtualNetworkSubnetConfig -Name "GatewaySubnet"
Set-AzureRmVirtualNetwork -VirtualNetwork $GWSub
```

## <a name="delete"></a>Kaynak grubunu silerek bir VPN ağ geçidini Sil

Kaynak grubunda kaynaklarınızın hiçbirini saklanması ilgili değildir ve baştan başlamak istiyorsanız, tüm kaynak grubunu silebilirsiniz. Bu, her şeyi kaldırmak için hızlı bir yoludur. Aşağıdaki adımlar yalnızca Resource Manager dağıtım modeli için geçerlidir.

### <a name="1-get-a-list-of-all-the-resource-groups-in-your-subscription"></a>1. Aboneliğinizdeki tüm kaynak gruplarının bir listesini alın.

```powershell
Get-AzureRmResourceGroup
```

### <a name="2-locate-the-resource-group-that-you-want-to-delete"></a>2. Silmek istediğiniz kaynak grubunu bulun.

Silin ve bu kaynak grubunda kaynaklar listesi görüntülemek istediğiniz kaynak grubunu bulun. Örnekte, RG1 kaynak grubunun adıdır. Tüm kaynakların bir listesini almak için örnek değiştirin.

```powershell
Find-AzureRmResource -ResourceGroupNameContains RG1
```

### <a name="3-verify-the-resources-in-the-list"></a>3. Listenin kaynaklarında doğrulayın.

Listenin döndürüldüğünde, kaynak grubunun yanı sıra, kaynak grubunun tüm kaynakları silmek istediğiniz doğrulamak için gözden geçirin. Bazı kaynakların kaynak grubunda tutmak istiyorsanız, adımlar bu makalenin önceki bölümlerinde, ağ geçidi silmek için kullanın.

### <a name="4-delete-the-resource-group-and-resources"></a>4. Kaynak grubu ve kaynakları silin.

Kaynak grubu ve kaynak grubunda bulunan tüm kaynak silmek için örnek değiştirin ve çalıştırın.

```powershell
Remove-AzureRmResourceGroup -Name RG1
```

### <a name="5-check-the-status"></a>5. Durumunu kontrol edin.

Tüm kaynakları silmek Azure için biraz zaman alabilir. Bu cmdlet'i kullanarak, kaynak grubunuzun durumunu kontrol edebilirsiniz.

```powershell
Get-AzureRmResourceGroup -ResourceGroupName RG1
```

Döndürülen sonucu 'Başarılı' gösterir.

```
ResourceGroupName : RG1
Location          : eastus
ProvisioningState : Succeeded
```