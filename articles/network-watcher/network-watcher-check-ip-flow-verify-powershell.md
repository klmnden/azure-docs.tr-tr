---
title: "trafik doğrulayın Azure Ağ İzleyicisi IP akış doğrulama - PowerShell | Microsoft Docs"
description: "Bu makalede denetlemek için veya bir sanal makineden trafiğin izin verilen veya PowerShell kullanarak reddedildi varsa açıklar"
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: 
ms.assetid: e1dad757-8c5d-467f-812e-7cc751143207
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: jdial
ms.openlocfilehash: 5257a70aa2dbc25bfe4eca5e2e0db87ca5e6b6fe
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2017
---
# <a name="check-if-traffic-is-allowed-or-denied-to-or-from-a-vm-with-ip-flow-verify-a-component-of-azure-network-watcher"></a>Trafik izin verilen ya da IP akış ile bir VM'den/VM'ye reddedildi onay Azure Ağ İzleyicisi'nin bir bileşeni doğrulayın

> [!div class="op_single_selector"]
> - [Azure portal](network-watcher-check-ip-flow-verify-portal.md)
> - [PowerShell](network-watcher-check-ip-flow-verify-powershell.md)
> - [CLI 1.0](network-watcher-check-ip-flow-verify-cli-nodejs.md)
> - [CLI 2.0](network-watcher-check-ip-flow-verify-cli.md)
> - [Azure REST API'si](network-watcher-check-ip-flow-verify-rest.md)


IP akış doğrulayın özelliğidir Ağ İzleyicisi, trafik için veya bir sanal makineden izin verilip verilmediğini doğrulamanızı sağlar. Bu senaryo, bir sanal makine için bir dış kaynağa veya arka uç iletişim kurabilirsiniz geçerli durumunu almak yararlıdır. IP akış doğrulayın, ağ güvenlik grubu (NSG) kurallarınızı düzgün şekilde yapılandırıldığından ve NSG kuralları tarafından engellenen akışları sorun giderme doğrulamak için kullanılabilir. IP kullanan başka bir nedeni akış durumda engellenen istediğiniz trafiği engellenmiş düzgün tarafından NSG emin olmak için doğrulayın.

## <a name="before-you-begin"></a>Başlamadan önce

Bu senaryo zaten izlediğiniz adımlarda varsayar [bir Ağ İzleyicisi oluşturmak](network-watcher-create.md) Ağ İzleyicisi mevcut bir örneği olması veya bir Ağ İzleyicisi oluşturun. Senaryo da geçerli bir sanal makine ile bir kaynak grubu kullanılacak var olduğunu varsayar.

## <a name="scenario"></a>Senaryo

Bir sanal makine bilinen bir Bing IP adresine iletişim kurabilirsiniz doğrulamak için bu senaryo kullanan IP akışı doğrulayın. Trafiği reddedilirse, bu trafiği engelleyen güvenlik kuralı döndürür. Doğrulayın IP akışı hakkında daha fazla bilgi için [IP akış doğrulayın genel bakış](network-watcher-ip-flow-verify-overview.md)

## <a name="retrieve-network-watcher"></a>Ağ İzleyicisi alma

Ağ İzleyicisi örneği almak için ilk adımdır bakın. `$networkWatcher` Değişkeni geçirilir IP akışı cmdlet'i doğrulayın.

```powershell
$nw = Get-AzurermResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq "WestCentralUS" } 
$networkWatcher = Get-AzureRmNetworkWatcher -Name $nw.Name -ResourceGroupName $nw.ResourceGroupName 
```

## <a name="get-a-vm"></a>VM Al

IP akış testleri trafiği için veya bir IP adresi için bir sanal makinede veya uzaktan bir hedef doğrulayın. Bir sanal makine kimliği cmdlet'i için gereklidir. Kullanmak için sanal makine Kimliğini biliyorsanız, bu adımı atlayabilirsiniz.

```powershell
$VM = Get-AzurermVM -ResourceGroupName "testrg" -Name "testvm1"
```

## <a name="get-the-nics"></a>NIC alma

Bu örnekte, bir sanal makinede NIC alıyoruz sanal makinede bir NIC IP adresi gereklidir. Sanal makinede test etmek istediğiniz IP adresi zaten biliyorsanız, bu adımı atlayabilirsiniz.

```powershell
$Nics = Get-AzureRmNetworkInterface | Where {$_.Id -eq $vm.NetworkProfile.NetworkInterfaces.Id.ForEach({$_})}
```

## <a name="run-ip-flow-verify"></a>Çalışma IP akış doğrulayın

Biz cmdlet'i biz çalıştırmak çalıştırmak için gereken bilgileri sahip olduğunuza `Test-AzureRmNetworkWatcherIPFlow` trafiği test etmek için cmdlet. Bu örnekte, ilk IP adresi ilk NIC üzerinde kullanıyoruz

```powershell
Test-AzureRmNetworkWatcherIPFlow -NetworkWatcher $networkWatcher -TargetVirtualMachineId $VM.Id `
-Direction Outbound -Protocol TCP `
-LocalIPAddress $nics[0].IpConfigurations[0].PrivateIpAddress -LocalPort 6895 -RemoteIPAddress 204.79.197.200 -RemotePort 80
```

> [!NOTE]
> IP akış doğrulayın gerektirir VM kaynak çalıştırmak için ayrılır.

## <a name="review-results"></a>Sonuçları gözden geçirin

Çalıştırdıktan sonra `Test-AzureRmNetworkWatcherIPFlow` sonuçların döndürülmesini, aşağıdaki örnek önceki adımdan döndürülen sonuç.

```
Access RuleName                                  
------ --------                                  
Allow  defaultSecurityRules/AllowInternetOutBound
```

## <a name="next-steps"></a>Sonraki adımlar

Trafik engelleniyor ve olmamalıdır, bkz: [ağ güvenlik grupları yönet](../virtual-network/virtual-network-manage-nsg-arm-portal.md) tanımlanan ağ güvenlik grubu ve güvenlik kuralları izlemek için.

[1]: ./media/network-watcher-check-ip-flow-verify-portal/figure1.png
[2]: ./media/network-watcher-check-ip-flow-verify-portal/figure2.png













