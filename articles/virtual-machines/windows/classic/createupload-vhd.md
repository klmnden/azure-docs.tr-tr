---
title: "Powershell kullanarak bir VM görüntüsü oluşturup | Microsoft Docs"
description: "Oluşturma ve klasik dağıtım modeli ve Azure Powershell kullanılarak genelleştirilmiş bir Windows Server yansıma (VHD) yükleme hakkında bilgi edinme."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 8c4a08fe-7714-4bf0-be87-c728a7806d3f
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/23/2017
ms.author: cynthn
ms.openlocfilehash: c2540120bcb1eca9f4ba62c7dbc0675343bf4f99
ms.sourcegitcommit: ce934aca02072bdd2ec8d01dcbdca39134436359
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/08/2017
---
# <a name="create-and-upload-a-windows-server-vhd-to-azure"></a>Bir Windows Server VHD’si oluşturup Azure’a yükleme
Bu makalede, sanal makineler oluşturmak için kullanabileceğiniz şekilde kendi genelleştirilmiş VM görüntüsü bir sanal sabit disk (VHD) nasıl yükleneceğini gösterir. Diskleri ve Microsoft Azure içinde VHD'leri hakkında daha fazla ayrıntı için [hakkında diskleri ve sanal makineler için VHD'ler](../about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

> [!IMPORTANT]
> Azure oluşturmak ve kaynaklarla çalışmak için iki farklı dağıtım modeli vardır: [Resource Manager ve klasik](../../../resource-manager-deployment-model.md). Bu makalede, Klasik dağıtım modeli kullanarak yer almaktadır. Microsoft, yeni dağıtımların çoğunun Resource Manager modelini kullanmasını önerir. Ayrıca [karşıya](../upload-generalized-managed.md) Resource Manager modelini kullanarak bir sanal makine.
> [!INCLUDE [virtual-machines-common-classic-createportal](../../../../includes/virtual-machines-classic-portal.md)]

## <a name="prerequisites"></a>Ön koşullar
Bu makalede, sahip olduğunuz varsayılmaktadır:

* **Bir Azure aboneliği** -yoksa, şunları yapabilirsiniz [ücretsiz bir Azure hesabı açabilirsiniz](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).
* **[Microsoft Azure PowerShell](/powershell/azure/overview)**  -yüklü ve aboneliğinizi kullanmak üzere yapılandırılmış Microsoft Azure PowerShell modülü vardır.
* **A. VHD dosyasını** - desteklenen Windows işletim sistemi bir .vhd dosyası depolanır ve bir sanal makineye bağlı. VHD çalışan sunucu rollerini Sysprep tarafından desteklenip desteklenmediğini denetleyin. Daha fazla bilgi için bkz: [sunucu rolleri için Sysprep desteği](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles).

    > [!IMPORTANT]
    > VHDX biçimi, Microsoft Azure'da desteklenmiyor. Disk Hyper-V Yöneticisi'ni kullanarak VHD biçimine Dönüştür veya [Convert-VHD cmdlet](http://technet.microsoft.com/library/hh848454.aspx). Ayrıntılar için bu bkz [blog gönderisi](http://blogs.msdn.com/b/virtual_pc_guy/archive/2012/10/03/using-powershell-to-convert-a-vhd-to-a-vhdx.aspx).

## <a name="step-1-prep-the-vhd"></a>1. adım: VHD hazırla
Azure'a VHD karşıya yüklemeden önce Sysprep aracını kullanılarak genelleştirilmiş gerekiyor. Bir görüntü olarak kullanılacak VHD hazırlar. Sysprep hakkında daha fazla ayrıntı için bkz: [kullanım Sysprep nasıl: Giriş](http://technet.microsoft.com/library/bb457073.aspx). VM yedeklemesi, Sysprep çalıştırılmadan önce yedekleyin.

İşletim sisteminin yüklü olduğu sanal makine'ne aşağıdaki yordamı tamamlayın:

1. İşletim sistemi için oturum açın.
2. Yönetici olarak bir komut istemi penceresi açın. Dizinine değiştirin **%windir%\system32\sysprep**ve ardından çalıştırın `sysprep.exe`.

    ![Bir komut istemi penceresi açın](./media/createupload-vhd/sysprep_commandprompt.png)
3. **Sistem Hazırlama aracı** iletişim kutusu görüntülenir.

   ![Sysprep Başlat](./media/createupload-vhd/sysprepgeneral.png)
4. İçinde **Sistem Hazırlama aracı**seçin **girin sistem ilk çalıştırma deneyimi (OOBE)** emin olun **Generalize** denetlenir.
5. İçinde **kapatma seçenekleri**seçin **kapatma**.
6. **Tamam** düğmesine tıklayın.

## <a name="step-2-create-a-storage-account-and-a-container"></a>2. adım: bir depolama hesabı ve kapsayıcı oluşturma
.Vhd dosyasını karşıya yüklemek için bir yer alacak şekilde Azure depolama hesabı gerekir. Bu adım bir hesap oluşturmak veya var olan bir hesaptan gereksinim duyduğunuz bilgileri almak nasıl gösterir. Değişkenleri değiştirin &lsaquo; köşeli &rsaquo; kendi bilgilerle.

1. Oturum Aç

    ```powershell
    Add-AzureAccount
    ```

2. Azure aboneliğiniz ayarlayın.

    ```powershell
    Select-AzureSubscription -SubscriptionName <SubscriptionName>
    ```

3. Yeni bir depolama hesabı oluşturun. Depolama hesabı adının benzersiz olması gerekir 3-24 karakter. Ad, harf ve sayıların herhangi bir birleşimini olabilir. Ayrıca "Doğu ABD" gibi bir konum belirtmeniz gerekir

    ```powershell
    New-AzureStorageAccount –StorageAccountName <StorageAccountName> -Location <Location>
    ```

4. Yeni depolama hesabı varsayılan olarak ayarlayın.

    ```powershell
    Set-AzureSubscription -CurrentStorageAccountName <StorageAccountName> -SubscriptionName <SubscriptionName>
    ```

5. Yeni bir kapsayıcı oluşturun.

    ```powershell
    New-AzureStorageContainer -Name <ContainerName> -Permission Off
    ```

## <a name="step-3-upload-the-vhd-file"></a>3. adım: .vhd dosyasını karşıya yükle
Kullanım [Ekle AzureVhd](https://docs.microsoft.com/en-us/powershell/module/azure/add-azurevhd) VHD yüklemek için.

Önceki adımda kullanılan Azure PowerShell penceresinde aşağıdaki komutu yazın ve değişkenleri değiştirin &lsaquo; köşeli &rsaquo; kendi bilgilerle.

```powershell
Add-AzureVhd -Destination "https://<StorageAccountName>.blob.core.windows.net/<ContainerName>/<vhdName>.vhd" -LocalFilePath <LocalPathtoVHDFile>
```

## <a name="step-4-add-the-image-to-your-list-of-custom-images"></a>4. adım: resim özel resimler listenize ekleyin.
Kullanım [Ekle AzureVMImage](https://docs.microsoft.com/en-us/powershell/module/azure/add-azurevmimage) görüntü kendi özel görüntülerinizi listesine eklemek için cmdlet.

```powershell
Add-AzureVMImage -ImageName <ImageName> -MediaLocation "https://<StorageAccountName>.blob.core.windows.net/<ContainerName>/<vhdName>.vhd" -OS "Windows"
```

## <a name="next-steps"></a>Sonraki adımlar
Artık [özel bir VM oluşturma](createportal.md) karşıya yüklediğiniz görüntüsü kullanarak.
