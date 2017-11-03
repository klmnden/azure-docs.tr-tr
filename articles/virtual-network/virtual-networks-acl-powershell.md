---
title: "Azure uç noktası erişim denetim listeleri yönetme | PowerShell | Klasik | Microsoft Docs"
description: "PowerShell ile ACL yönetmeyi öğrenin"
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: tysonn
ms.assetid: c84e40af-f351-4572-b3f0-d572d46bafe7
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2016
ms.author: jdial
ms.openlocfilehash: c3476908447380ccd7e8b9c0f1c2a55ae763cc1e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="manage-endpoint-access-control-lists-using-powershell-in-the-classic-deployment-model"></a>Uç noktası erişim denetim listeleri Klasik dağıtım modelinde PowerShell kullanarak yönetme
Oluşturun ve ağ erişim denetimi listeleri (ACL'ler) uç noktaları için yönetim portalında veya Azure PowerShell kullanarak yönetin. Bu konuda, PowerShell kullanarak tamamlayabilirsiniz ACL ortak görevler için yordamlar bulabilirsiniz. Cmdlet'leri Azure PowerShell listesi için bkz [Azure Yönetimi cmdlet'leri](http://go.microsoft.com/fwlink/?LinkId=317721). ACL'ler hakkında daha fazla bilgi için bkz: [bir ağ erişim denetimi listesi (ACL) nedir?](virtual-networks-acl.md). Yönetim Portalı'nı kullanarak, ACL'ler yönetmek isterseniz bkz [ayarlamak yukarı uç noktaları için bir sanal makine nasıl](../virtual-machines/windows/classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

## <a name="manage-network-acls-by-using-azure-powershell"></a>Ağ ACL'leri Azure PowerShell kullanarak yönetme
Azure PowerShell cmdlet'lerini oluşturmak, kaldırmak ve (küme) yapılandırmak için kullanabileceğiniz ağ erişim denetimi listeleri (ACL'ler). Biz, PowerShell kullanarak bir ACL yapılandırmak yollardan bazılarını birkaç örnekleri dahil ettiğiniz.

ACL PowerShell cmdlet'lerinin listesi almak için aşağıdakilerden birini kullanabilirsiniz:

    Get-Help *AzureACL*
    Get-Command -Noun AzureACLConfig

### <a name="create-a-network-acl-with-rules-that-permit-access-from-a-remote-subnet"></a>Uzak bir alt ağ üzerinden erişime kurallarla ağ ACL oluşturma
Aşağıdaki örnek kuralları içeren yeni bir ACL oluşturmak için bir yol gösterir. Bu ACL sonra sanal makine uç noktası için uygulanır. Aşağıdaki örnekte ACL kuralları, uzak bir alt ağdan erişimi izin verir. Uzak bir alt ağ için izin verme kuralları ile yeni bir ağ ACL oluşturmak için bir Azure PowerShell ISE açın. Kopyalama altında komut dosyasını kendi değerlerinizle yapılandırma betiğini yapıştırın ve sonra komut dosyasını çalıştırın.

1. Yeni ağ ACL nesnesi oluşturun.
   
        $acl1 = New-AzureAclConfig
2. Uzak bir alt ağdan erişimi veren bir kural kümesi. Aşağıdaki örnekte, kural kümesi *100* (olan öncelik kuralına göre 200 ve üzeri) uzak alt izin vermek için *10.0.0.0/8* sanal makine uç noktası erişimi. Değerleri kendi yapılandırma gereksinimlerine göre değiştirin. "SharePoint ACL yapılandırma" adı bu kural aramak istediğiniz kolay adı ile değiştirilmelidir.
   
        Set-AzureAclConfig –AddRule –ACL $acl1 –Order 100 `
            –Action permit –RemoteSubnet "10.0.0.0/8" `
            –Description "SharePoint ACL config"
3. Ek kurallar için kendi yapılandırma gereksinimlerine göre değerleri değiştirerek cmdlet'ini tekrarlayın. Kural değiştirdiğinizden emin olun uygulanacak kuralları istediğiniz sırayı yansıtacak şekilde sipariş numarası. Alt kural sayısı en yüksek sayıyı önceliklidir.
   
        Set-AzureAclConfig –AddRule –ACL $acl1 –Order 200 `
            –Action permit –RemoteSubnet "157.0.0.0/8" `
            –Description "web frontend ACL config"
4. Ardından, yeni bir uç noktası (Ekle) oluşturmak veya mevcut bir uç noktası (küme) ACL'si ayarlayın. Bu örnekte, biz "web" adlı yeni bir sanal makine uç noktası ekleyin ve sanal makine uç noktası ile ACL ayarlarını güncelleştirin.
   
        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        | Add-AzureEndpoint –Name "web" –Protocol tcp –Localport 80 - PublicPort 80 –ACL $acl1 `
        | Update-AzureVM
5. Ardından, cmdlet öğelerini birleştirmek ve betiği çalıştırın. Bu örnekte, birleştirilmiş cmdlet'leri şöyle olabilir:
   
        $acl1 = New-AzureAclConfig
        Set-AzureAclConfig –AddRule –ACL $acl1 –Order 100 `
            –Action permit –RemoteSubnet "10.0.0.0/8" `
            –Description "Sharepoint ACL config"
        Set-AzureAclConfig –AddRule –ACL $acl1 –Order 200 `
            –Action permit –RemoteSubnet "157.0.0.0/8" `
            –Description "web frontend ACL config"
        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        |Add-AzureEndpoint –Name "web" –Protocol tcp –Localport 80 - PublicPort 80 –ACL $acl1 `
        |Update-AzureVM

### <a name="remove-a-network-acl-rule-that-permits-access-from-a-remote-subnet"></a>Uzak bir alt ağdan erişimi veren bir ağ ACL kuralını Kaldır
Aşağıdaki örnek, bir ağ ACL kuralı kaldırmak için yol gösterir.  Uzak bir alt ağ için izin verme kuralları ile ağ ACL kuralı kaldırmak için bir Azure PowerShell ISE açın. Kopyalama altında komut dosyasını kendi değerlerinizle yapılandırma betiğini yapıştırın ve sonra komut dosyasını çalıştırın.

1. Sanal makine uç noktası için ağ ACL nesne get ilk adımdır. Ardından, ACL kuralı kaldırırsınız. Bu durumda, biz bu kural kimliğine göre kaldırıyorsunuz Bu gibi durumlarda bu kuralı kimliği 0 yalnızca ACL'den kaldırır. Sanal makine uç noktasından ACL nesne kaldırmaz.
   
        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        | Get-AzureAclConfig –EndpointName "web" `
        | Set-AzureAclConfig –RemoveRule –ID 0 –ACL $acl1
2. Ardından, sanal makine uç noktası için ağ ACL nesnesi geçerli ve sanal makineyi güncelleştirin.
   
        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        | Set-AzureEndpoint –ACL $acl1 –Name "web" `
        | Update-AzureVM

### <a name="remove-a-network-acl-from-a-virtual-machine-endpoint"></a>Bir sanal makine uç noktasından bir ağ ACL Kaldır
Bazı senaryolarda, bir sanal makine uç noktasından bir ağ ACL nesnesi kaldırmak isteyebilirsiniz. Bunu yapmak için bir Azure PowerShell ISE açın. Kopyalama altında komut dosyasını kendi değerlerinizle yapılandırma betiğini yapıştırın ve sonra komut dosyasını çalıştırın.

        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        | Remove-AzureAclConfig –EndpointName "web" `
        | Update-AzureVM

## <a name="next-steps"></a>Sonraki adımlar
[Bir ağ erişim denetimi listesi (ACL) nedir?](virtual-networks-acl.md)

