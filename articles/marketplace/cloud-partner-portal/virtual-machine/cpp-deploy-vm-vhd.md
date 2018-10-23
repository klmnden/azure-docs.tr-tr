---
title: Azure Market vhd'lerinizden VM dağıtma | Microsoft Docs
description: Azure ile dağıtılan bir VHD'den VM kaydedileceği açıklanmaktadır.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: pbutlerm
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 10/19/2018
ms.author: pbutlerm
ms.openlocfilehash: 2771549af29b3e717d117afb42de6db03fbee226
ms.sourcegitcommit: 17633e545a3d03018d3a218ae6a3e4338a92450d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/22/2018
ms.locfileid: "49639945"
---
# <a name="deploy-a-vm-from-your-vhds"></a>Vhd'lerinizden VM dağıtma

Bu makalede bir Azure ile dağıtılan sanal sabit diskten (VHD) nasıl bir sanal makine (VM) kaydedileceği açıklanmaktadır.  Gereken araçları ve bunları bir kullanıcı VM görüntüsü oluşturun, ardından kullanarak Azure'a dağıtmak için nasıl kullanılacağını listeler [Microsoft Azure Portal'da](https://ms.portal.azure.com/) veya PowerShell betikleri. 

Sanal sabit disklerinizin (VHD) yükledikten sonra — genelleştirilmiş işletim sistemi VHD'si ve sıfır veya daha fazla veri diski VHD'leri — Azure depolama hesabınızın, bunları bir kullanıcı VM görüntüsü olarak kaydedebilirsiniz. Ardından bu görüntüyü test edebilirsiniz. İşletim sistemi VHD'si genelleştirilmiş olduğundan VHD URL'sini sağlayarak VM'yi doğrudan dağıtamazsınız.

VM görüntüleri hakkında daha fazla bilgi edinmek için şu blog gönderilerine bakın:

- [VM görüntüsü](https://azure.microsoft.com/blog/vm-image-blog-post/)
- [VM görüntüsü PowerShell 'Nasıl yapılır'](https://azure.microsoft.com/blog/vm-image-powershell-how-to-blog-post/)


## <a name="set-up-the-necessary-tools"></a>Gerekli araçları ayarlayın

Zaten yapmadıysanız, Azure PowerShell ve Azure CLI aşağıdaki yönergeleri kullanarak yükleyin:

<!-- TD: Change the following URLs (in this entire topic) to relative paths.-->

- [PowerShellGet ile Windows üzerindeki Azure PowerShell'i yükleme](https://docs.microsoft.com/powershell/azure/install-azurerm-ps)
- [Azure CLI 2.0’ı yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli)


## <a name="create-a-user-vm-image"></a>Bir kullanıcı VM görüntüsü oluşturma

Ardından, genelleştirilmiş bir VHD'den yönetilmeyen görüntü oluşturur.

#### <a name="capture-the-vm-image"></a>VM görüntüsü yakalama

Erişim yaklaşımınızı karşılık gelen VM yakalama üzerinde aşağıdaki makaledeki yönergeleri kullanın:

-  PowerShell: [bir Azure VM'den yönetilmeyen bir VM görüntüsü oluşturma](../../../virtual-machines/windows/capture-image-resource.md)
-  Azure CLI: [bir sanal makine veya VHD görüntüsü oluşturma](../../../virtual-machines/linux/capture-image.md)
-  API: [sanal makineler - yakalama](https://docs.microsoft.com/rest/api/compute/virtualmachines/capture)

### <a name="generalize-the-vm-image"></a>VM görüntüyü Genelleştirme

Daha önce genelleştirilmiş bir VHD'den kullanıcı görüntüsü oluşturduğu için bu da genelleştirilmiş.  Yeniden erişim mekanizmanızı karşılık gelen makaleyi seçin.  (Bunu yakalandığında, zaten diskinizi genelleştirildiğinden.)

-  PowerShell: [VM'yi Genelleştirme](https://docs.microsoft.com/azure/virtual-machines/windows/sa-copy-generalized#generalize-the-vm)
-  Azure CLI: [2. adım: sanal makine oluşturma görüntüsü](https://docs.microsoft.com/azure/virtual-machines/linux/capture-image#step-2-create-vm-image)
-  API: [sanal makineler - Generalize](https://docs.microsoft.com/rest/api/compute/virtualmachines/generalize)


## <a name="deploy-a-vm-from-a-user-vm-image"></a>Bir kullanıcı VM görüntüsünden VM dağıtma

Ardından, Azure portalı veya PowerShell kullanarak bir kullanıcı VM görüntüsünden VM dağıtır.

<!-- TD: Recapture following hilited images and replace with red-box. -->

### <a name="deploy-a-vm-from-azure-portal"></a>Azure portalından bir VM dağıtma

Azure portalından, kullanıcı VM'i dağıtmak için aşağıdaki işlemi kullanın.

1.  [Azure portal](https://portal.azure.com) oturum açın.

2.  Tıklayın **yeni** araması **şablon dağıtımı**, ardından **düzenleyicide kendi şablonunuzu oluşturun**.  <br/>
  ![Azure portalında VHD'yi dağıtım şablonu oluşturma](./media/publishvm_021.png)

3. Bunu kopyalayıp [JSON şablonunu](./cpp-deploy-json-template.md) Düzenleyicisi ve tıklatın **Kaydet**. <br/>
  ![Azure portalında VHD'yi dağıtım şablonunu Kaydet](./media/publishvm_022.png)

4. Görüntülenen için parametre değerlerini sağlayın **özel dağıtım** özellik sayfaları.

   <table> <tr> <td valign="top"> <img src="./media/publishvm_023.png" alt="Custom deployment property page 1"> </td> <td valign="top"> <img src="./media/publishvm_024.png" alt="Custom deployment property page 2"> </td> </tr> </table> <br/> 

   |  **Parametre**              |   **Açıklama**                                                            |
   |  -------------              |   ---------------                                                            |
   | Kullanıcı depolama hesabı adı   | Genelleştirilmiş VHD bulunduğu depolama hesabı adı                    |
   | Kullanıcı depolama kapsayıcısı adı | Genelleştirilmiş VHD bulunduğu kapsayıcı adı                          |
   | Genel IP için DNS adı      | Genel IP DNS adı                                                           |
   | Yönetici kullanıcı adı             | Yeni VM için yönetici hesabının kullanıcı adı                                  |
   | Yönetici Parolası              | Yeni VM için yönetici hesabının parolası                                  |
   | İşletim Sistemi Türü                     | VM işletim sistemi: `Windows` \| `Linux`                                    |
   | Abonelik Kimliği             | Seçili abonelik tanımlayıcısı                                      |
   | Konum                    | Dağıtım coğrafi konum                                        |
   | VM Boyutu                     | [Azure VM boyutu](https://docs.microsoft.com/azure/virtual-machines/windows/sizes), örneğin `Standard_A2` |
   | Genel IP adresi adı      | Genel IP adresi adı                                               |
   | VM Adı                     | Yeni bir VM adı                                                           |
   | Sanal ağ adı        | Sanal makine tarafından kullanılan sanal ağ adı                                   |
   | NIC adı                    | Sanal ağ çalıştıran ağ arabirim kartı adı               |
   | VHD URL'si                     | İşletim sistemi diski VHD URL'si tamamlayın                                                     |
   |  |  |
            
5. Bu değerleri sizin sağlamanız sonra tıklayın **satın alma**. 

Azure dağıtım başlayacak: Belirtilen depolama hesabı yolu içinde belirtilen yönetilmeyen VHD ile yeni bir VM oluşturur.  Tıklayarak ilerleme durumunu Azure portalında izleyebilirsiniz **sanal makineler** portalının sol taraftaki.  VM oluşturulduğunda durumu gelen değiştirecek `Starting` için `Running`. 


### <a name="deploy-a-vm-from-powershell"></a>Powershell'den VM dağıtma

Yeni oluşturulan genelleştirilmiş VM görüntüsünden büyük bir VM'yi dağıtmak için aşağıdaki cmdlet'leri kullanın.

``` powershell
    $img = Get-AzureVMImage -ImageName "myVMImage"
    $user = "user123"
    $pass = "adminPassword123"
    $myVM = New-AzureVMConfig -Name "VMImageVM" -InstanceSize "Large" -ImageName $img.ImageName | Add-AzureProvisioningConfig -Windows -AdminUsername $user -Password $pass
    New-AzureVM -ServiceName "VMImageCloudService" -VMs $myVM -Location "West US" -WaitForBoot
```

<!-- TD: The following is a marketplace-publishing article and may be out-of-date.  TD: update and move topic.
For help with issues, see [Troubleshooting common issues encountered during VHD creation](https://docs.microsoft.com/azure/marketplace-publishing/marketplace-publishing-vm-image-creation-troubleshooting) for additional assistance.
-->

## <a name="next-steps"></a>Sonraki adımlar

Sanal makinenizin dağıtıldıktan sonra hazır olduğunuz [VM yapılandırma](./cpp-configure-vm.md).
