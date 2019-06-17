---
title: Azure uç noktası erişim denetim listelerini yönetebilir | PowerShell | Klasik | Microsoft Docs
description: PowerShell ile ACL yönetmeyi öğrenin
services: virtual-network
documentationcenter: na
author: genlin
ms.assetid: c84e40af-f351-4572-b3f0-d572d46bafe7
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2016
ms.author: genli
ms.openlocfilehash: c43dacaf7bb5ab17fe740dd429e4a40dbc11bb6e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64726919"
---
# <a name="manage-endpoint-access-control-lists-using-powershell-in-the-classic-deployment-model"></a>Uç nokta erişim denetim listeleri Klasik dağıtım modelinde PowerShell kullanarak yönetme
Oluşturun ve ağ erişim denetim listeleri (ACL'ler) uç noktalar için Yönetim Portalı'nda ya da Azure PowerShell kullanarak yönetin. Bu konu başlığında, PowerShell kullanarak tamamlayabilirsiniz ACL ortak görevler için yordamlar bulabilirsiniz. Azure PowerShell listesi için bkz: cmdlet'leri [Azure Management cmdlet'leri](https://go.microsoft.com/fwlink/?LinkId=317721). ACL'ler hakkında daha fazla bilgi için bkz. [bir ağ erişim denetimi listesi (ACL) nedir?](virtual-networks-acl.md). Yönetim portalını kullanarak, ACL'ler yönetmek istiyorsanız bkz [uç noktaları ayarlama bir sanal makineye nasıl](../virtual-machines/windows/classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

## <a name="manage-network-acls-by-using-azure-powershell"></a>Azure PowerShell kullanarak ağ ACL'lerini yönetme
Azure PowerShell cmdlet'leri oluşturma, kaldırmak ve (set) yapılandırmak için kullanabileceğiniz ağ erişim denetim listeleri (ACL'ler). PowerShell kullanarak bir ACL yapılandırabileceğiniz yollardan bazılarını birkaç örnek ekledik.

ACL PowerShell cmdlet'leri tam bir listesini almak için aşağıdakilerden birini kullanabilirsiniz:

    Get-Help *AzureACL*
    Get-Command -Noun AzureACLConfig

### <a name="create-a-network-acl-with-rules-that-permit-access-from-a-remote-subnet"></a>Uzak bir alt ağdan erişime kurallarla bir ağ ACL'si oluşturun
Aşağıdaki örnek, kuralları içeren yeni bir ACL oluşturmak için bir yol gösterir. Bu ACL sonra bir sanal makine uç noktası için uygulanır. ACL kuralları aşağıdaki örnekte, bir uzak alt ağından erişime izin verir. Yeni bir ağ ACL'si uzak bir alt ağ için izin verme kuralları oluşturmak için bir Azure PowerShell ISE'yi açın. Kopyalayın ve aşağıdaki komut dosyasını kendi değerlerinizle yapılandırma betiğini yapıştırın ve sonra komut dosyasını çalıştırın.

1. Yeni ağ ACL'si nesnesini oluşturun.
   
        $acl1 = New-AzureAclConfig
2. Uzak bir alt ağdan erişim veren bir kural kümesi. Aşağıdaki örnekte, kural kümesi *100* (sahip öncelik kuralına göre 200 ve üzeri) uzak alt izin vermek için *10.0.0.0/8* sanal makine uç noktasına erişim. Değerleri kendi yapılandırma gereksinimleri ile değiştirin. "SharePoint ACL yapılandırması" adı bu kural aramak istediğiniz kolay adı ile değiştirilmelidir.
   
        Set-AzureAclConfig –AddRule –ACL $acl1 –Order 100 `
            –Action permit –RemoteSubnet "10.0.0.0/8" `
            –Description "SharePoint ACL config"
3. Ek kuralları için değerleri kendi yapılandırma gereksinimleri ile değiştirerek cmdlet'ini tekrarlayın. Kural değiştirdiğinizden emin olun uygulanacak kuralları istediğiniz sıralamasını yansıtacak şekilde sipariş numarası. Alt kural sayısı en yüksek sayıyı daha önceliklidir.
   
        Set-AzureAclConfig –AddRule –ACL $acl1 –Order 200 `
            –Action permit –RemoteSubnet "157.0.0.0/8" `
            –Description "web frontend ACL config"
4. Ardından, yeni bir uç noktası (Ekle) oluşturun veya mevcut bir uç noktası (Set) ACL'si ayarlayın. Bu örnekte, biz "web" adlı yeni bir sanal makine uç noktası ekleyin ve sanal makine uç noktası ACL ayarları ile güncelleştirin.
   
        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        | Add-AzureEndpoint –Name "web" –Protocol tcp –Localport 80 - PublicPort 80 –ACL $acl1 `
        | Update-AzureVM
5. Ardından, cmdlet öğelerini birleştirmek ve betiği çalıştırın. Bu örnekte, birleşik cmdlet'leri şöyle görünebilir:
   
        $acl1 = New-AzureAclConfig
        Set-AzureAclConfig –AddRule –ACL $acl1 –Order 100 `
            –Action permit –RemoteSubnet "10.0.0.0/8" `
            –Description "SharePoint ACL config"
        Set-AzureAclConfig –AddRule –ACL $acl1 –Order 200 `
            –Action permit –RemoteSubnet "157.0.0.0/8" `
            –Description "web frontend ACL config"
        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        |Add-AzureEndpoint –Name "web" –Protocol tcp –Localport 80 - PublicPort 80 –ACL $acl1 `
        |Update-AzureVM

### <a name="remove-a-network-acl-rule-that-permits-access-from-a-remote-subnet"></a>Uzak bir alt ağdan erişim veren bir ağ ACL'si kuralını kaldırın
Aşağıdaki örnek, bir ağ ACL kuralı kaldırmak için bir yol gösterir.  Bir ağ ACL'si kuralı uzak bir alt ağ için izin verme kuralları ile kaldırmak için bir Azure PowerShell ISE'yi açın. Kopyalayın ve aşağıdaki komut dosyasını kendi değerlerinizle yapılandırma betiğini yapıştırın ve sonra komut dosyasını çalıştırın.

1. İlk adım, sanal makine uç noktası için ağ ACL'si nesne almaktır. Ardından, ACL kuralı kaldırırsınız. Bu durumda, bu kural kimliğine göre kaldırıyoruz Bu gibi durumlarda bu kural kimliği 0 yalnızca ACL'SİNDEN kaldırılır. Sanal makine uç noktasından ACL nesne kaldırmaz.
   
        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        | Get-AzureAclConfig –EndpointName "web" `
        | Set-AzureAclConfig –RemoveRule –ID 0 –ACL $acl1
2. Ardından, sanal makine uç noktası için ağ ACL'si nesnesi geçerli ve sanal makine güncelleştirmeniz gerekir.
   
        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        | Set-AzureEndpoint –ACL $acl1 –Name "web" `
        | Update-AzureVM

### <a name="remove-a-network-acl-from-a-virtual-machine-endpoint"></a>Bir sanal makine uç noktasından bir ağ ACL'si Kaldır
Bazı senaryolarda, bir sanal makine uç noktasından bir ağ ACL'si nesnesi kaldırmak isteyebilirsiniz. Bunu yapmak için bir Azure PowerShell ISE'yi açın. Kopyalayın ve aşağıdaki komut dosyasını kendi değerlerinizle yapılandırma betiğini yapıştırın ve sonra komut dosyasını çalıştırın.

        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        | Remove-AzureAclConfig –EndpointName "web" `
        | Update-AzureVM

## <a name="next-steps"></a>Sonraki adımlar
[Bir ağ erişim denetimi listesi (ACL) nedir?](virtual-networks-acl.md)

