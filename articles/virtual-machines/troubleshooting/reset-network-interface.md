---
title: Azure Windows VM için ağ arabirimi sıfırlama | Microsoft Docs
description: Azure Windows VM için ağ arabirimi sıfırlama gösterir
services: virtual-machines-windows, azure-resource-manager
documentationcenter: ''
author: genlin
manager: willchen
editor: ''
tags: top-support-issue, azure-resource-manager
ms.service: virtual-machines-windows
ms.tgt_pltfrm: vm-windows
ms.topic: troubleshooting
ms.date: 11/16/2018
ms.author: genli
ms.openlocfilehash: 61001d4926dcce68872a368afb5b28f2d3a8e2c0
ms.sourcegitcommit: 8899e76afb51f0d507c4f786f28eb46ada060b8d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/16/2018
ms.locfileid: "51819009"
---
# <a name="how-to-reset-network-interface-for-azure-windows-vm"></a>Azure Windows VM için ağ arabirimi sıfırlama 

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

Varsayılan ağ arabirimi (NIC) devre dışı bırakmak veya bir statik IP NIC için el ile ayarlar sonra Microsoft Azure Windows sanal makinesi (VM) için bağlantı kurulamıyor Bu makalede, Azure Windows Uzak bağlantı sorunu çözer, VM için ağ arabirimini sıfırlayın gösterilmektedir.

[!INCLUDE [support-disclaimer](../../../includes/support-disclaimer.md)]
## <a name="reset-network-interface"></a>Ağ arabirimini sıfırlayın

### <a name="for-classic-vms"></a>Klasik VM'ler için

Ağ arabirimi sıfırlamak için şu adımları izleyin:

#### <a name="use-azure-portal"></a>Azure portalı kullanma

1.  [Azure Portal]( https://ms.portal.azure.com) gidin.
2.  Seçin **sanal makineler (Klasik)**.
3.  Etkilenen sanal makineyi seçin.
4.  Seçin **IP adresleri**.
5.  Varsa **özel IP ataması** değil **statik**, değiştirin **statik**.
6.  Değişiklik **IP adresi** başka bir IP adresine alt ağdaki kullanılabilir.
7.  Kaydet'i seçin.
8.  Sanal makine sistemde yeni NIC'yi başlatmak için yeniden başlatılır.
9.  Makinenize yönelik RDP deneyin. İsterseniz başarılı olursa, özgün durumuna geri özel IP adresi değişebilir. Aksi takdirde, tutabilirsiniz. 

#### <a name="use-azure-powershell"></a>Azure PowerShell kullanma

1. Sahip olduğunuzdan emin olun [en son Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview) yüklü.
2. Yükseltilmiş (yönetici olarak çalıştır) Azure PowerShell oturumu açın. Aşağıdaki komutları çalıştırın:

    ```powershell
    #Set the variables 
    $SubscriptionID = "<Suscription ID>"
    $VM = "<VM Name>"
    $CloudService = "<Cloud Service>"
    $VNET = "<Virtual Network>"
    $IP = "NEWIP"

    #Log in to the subscription 
    Add-AzureAccount
    Select-AzureSubscription -SubscriptionId $SubscriptionId 

    #Check whether the new IP address is available in the virtual network.
    Test-AzureStaticVNetIP –VNetName $VNET –IPAddress  $IP
    
    #Add/Change static IP. This process will not change MAC address
    Get-AzureVM -ServiceName $CloudService -Name $VM | Set-AzureStaticVNetIP -IPAddress $IP |Update-AzureVM
    ```
3. Makinenize yönelik RDP deneyin. İsterseniz başarılı olursa, özgün durumuna geri özel IP adresi değişebilir. Aksi takdirde, tutabilirsiniz. 

### <a name="for-vms-deployed-in-resource-group-model"></a>Kaynak grubu modelde dağıtılan VM'ler için

1.  [Azure Portal]( https://ms.portal.azure.com) gidin.
2.  Etkilenen sanal makineyi seçin.
3.  Seçin **ağ arabirimleri**.
4.  Makineyle ilişkili ağ arabirimi seçin
5.  Seçin **IP yapılandırmaları**.
6.  IP adresi seçin. 
7.  Varsa **özel IP ataması** değil **statik**, değiştirin **statik**.
8.  Değişiklik **IP adresi** başka bir IP adresine alt ağdaki kullanılabilir.
9. Sanal makine sistemde yeni NIC'yi başlatmak için yeniden başlatılır.
10. Makinenize yönelik RDP deneyin. İsterseniz başarılı olursa, özgün durumuna geri özel IP adresi değişebilir. Aksi takdirde, tutabilirsiniz. 

#### <a name="use-azure-powershell"></a>Azure PowerShell kullanma

1. Sahip olduğunuzdan emin olun [en son Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview) yüklü
2. Yükseltilmiş (yönetici olarak çalıştır) Azure PowerShell oturumu açın. Aşağıdaki komutları çalıştırın:

    ```powershell
    #Set the variables 
    $SubscriptionID = "<Suscription ID>"
    $VM = "<VM Name>"
    $ResourceGroup = "<Resource Group>"
    $VNET = "<Virtual Network>"
    $IP = "NEWIP"

    #Log in to the subscription 
    Add-AzureRMAccount
    Select-AzureRMSubscription -SubscriptionId $SubscriptionId 
    
    #Check whether the new IP address is available in the virtual network.
    Test-AzureStaticVNetIP –VNetName $VNET –IPAddress  $IP

    #Add/Change static IP. This process will not change MAC address
    Get-AzureRMVM -ServiceName $ResourceGroup -Name $VM | Set-AzureStaticVNetIP -IPAddress $IP | Update-AzureRMVM
    ```
3. Makinenize yönelik RDP deneyin.  İsterseniz başarılı olursa, özgün durumuna geri özel IP adresi değişebilir. Aksi takdirde, tutabilirsiniz. 

## <a name="delete-the-unavailable-nics"></a>Kullanılamayan NIC Sil
Makinede Uzak Masaüstü yapabilecekleriniz sonra olası bir sorundan kaçınmak için eski NIC'yi silmeniz gerekir:

1.  Cihaz Yöneticisi'ni açın.
2.  Seçin **görünümü** > **gizli aygıtları göster**.
3.  Seçin **ağ bağdaştırıcıları**. 
4.  "Microsoft Hyper-V ağ bağdaştırıcısı" olarak adlandırılmış bağdaştırıcıları denetleyin.
5.  Kullanılamaz durumdaki gri renkli bir bağdaştırıcı görebilirsiniz. Bağdaştırıcısına sağ tıklayın ve ardından Kaldır'ı seçin.

    ![NIC görüntüsü](media/reset-network-interface/nicpage.png)

    > [!NOTE]
    > Yalnızca "Microsoft Hyper-V ağ bağdaştırıcısı" adlı kullanılamayan bağdaştırıcılar kaldırın. Herhangi bir gizli bağdaştırıcılarının kaldırırsanız, ek sorunları neden olabilir.
    >
    >

6.  Artık tüm kullanılamayan bağdaştırıcılar sisteminizden temizlenmiş olmalıdır.
