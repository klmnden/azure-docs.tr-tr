---
title: Azure Stack'te bir VM görüntüsüne ekleyip | Microsoft Docs
description: Kuruluşunuzun özel Windows veya Linux VM görüntüsünü kullanmak kiracılar için ekleyip yeniden açın.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: e5a4236b-1b32-4ee6-9aaa-fcde297a020f
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: PowerShell
ms.topic: get-started-article
ms.date: 06/27/2018
ms.author: mabrigg
ms.reviewer: kivenkat
ms.openlocfilehash: 5c2088ab39e32c049ce867698e84efba759c9a87
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37447345"
---
# <a name="make-a-virtual-machine-image-available-in-azure-stack"></a>Azure Stack'te bir sanal makine görüntüsü kullanılabilmesini

*İçin geçerlidir: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Azure Stack'te, sanal makine görüntüleri kullanıcılarınıza sunabileceğiniz. Bu görüntüler, Azure Resource Manager şablonları tarafından başvurulabilir veya bir Market öğesi olarak Azure Marketi'nde UI ekleyebilirsiniz. Ya da bir görüntü formu genel Azure Marketi kullanın veya kendi özel görüntünüzü ekleyin. Portalı veya Windows PowerShell kullanarak bir VM ekleyebilirsiniz.

## <a name="add-a-vm-image-through-the-portal"></a>Portal üzerinden bir VM görüntüsü ekleme

> [!NOTE]
> Bu yöntem ile Market öğesi ayrı ayrı oluşturmanız gerekir.

Görüntüleri bir blob depolama URI'si başvurulmak üzere kurabilmesi gerekir. (VHDX değil) VHD biçiminde bir Windows veya Linux işletim sistemi görüntüsünü hazırlama ve görüntüyü Azure'da veya Azure Stack'te bir depolama hesabına yükleyin. Görüntünüzü Azure'da veya Azure Stack'te blob depolama için zaten karşıya yüklenmişse, 1. adım atlayabilirsiniz.

1. [Resource Manager dağıtımları için azure'a bir Windows VM görüntüsü](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-upload-image/) veya bir Linux görüntüsü için açıklanan yönergeleri [Azure Stack'te dağıtma Linux sanal makineleri](azure-stack-linux.md). Görüntüyü karşıya yüklemeden önce aşağıdaki etmenleri düşünmeniz önemlidir:

   - Azure Stack sabit disk VHD biçimini destekler. Sabit biçim, disk farkı X blob farkı X depolanır. Bu nedenle mantıksal diski dosya içinde doğrusal olarak yapıları. Blob'un sonundaki küçük bir alt bilgi VHD'nin özelliklerini açıklar. Diskinizin giderilip doğrulamak için şunu kullanın [Get-VHD](https://docs.microsoft.com/powershell/module/hyper-v/get-vhd?view=win10-ps) PowerShell komutu.  

    > [!IMPORTANT]
    >  Azure Stack, dinamik disk VHD desteklemez. Bir VM'ye bağlı bir dinamik disk yeniden boyutlandırması VM başarısız durumda bırakır. Bu sorunu gidermek için sanal makinenin disk bir depolama hesabında bir VHD blobunun silmeden VM'yi silin. Dönüştürme, dinamik bir diski VHD'den bir sabit diske ve sanal makine'yeniden oluşturun.

   * Azure Stack blob depolama için Azure blob depolama için Azure Stack görüntü deposuna görüntü gönderebilmeniz için daha az zaman alacağından daha görüntü yüklemek için daha verimlidir.

   * Karşıya yüklerken, [Windows VM görüntüsü](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-upload-image/), değiştirdiğinizden emin olun **Azure'da oturum** ile adım [Azure Stack işlecin PowerShell ortamını yapılandırma](azure-stack-powershell-configure-admin.md) adım.  

   * Blob depolama URI'si görüntünün karşıya yüklersiniz not edin. Blob depolama URI'si aşağıdaki biçime sahiptir: *&lt;storageAccount&gt;/&lt;blobContainer&gt;/&lt;targetVHDName&gt;*.vhd.

   * Blob anonim olarak erişilebilir olması için burada VHD VM görüntüsünü karşıya yüklenen depolama hesabının blob kapsayıcısına gidin. Seçin **Blob**ve ardından **erişim ilkesi**. İsteğe bağlı olarak, bunun yerine kapsayıcı paylaşılan erişim imzası oluşturma de blob URI'si parçası olarak dahil edebilirsiniz.

   ![Depolama hesabının BLOB'ları için Git](./media/azure-stack-add-vm-image/image1.png)

   ![Kümesi blob erişimi için ortak](./media/azure-stack-add-vm-image/image2.png)

2. Azure Stack için operatör olarak oturum açın. Menüde **diğer hizmetler**. Ardından, **işlem** > **VM görüntüleri** > **Ekle**.

3. Altında **bir VM görüntüsü ekleme**, yayımcı, teklif, SKU ve sanal makine görüntüsü sürümü girin. Bu adı kesimlerini Resource Manager şablonlarını sanal makine görüntüsünü bakın. Seçtiğinizden emin olun **osType** doğru değeri. İçin **işletim sistemi diski Blob URİ'si**, burada görüntünün karşıya yüklendiği Blob URI'si girin. Ardından, **Oluştur** VM oluşturmaya başlamak için.

   ![Görüntüyü oluşturmaya başlama](./media/azure-stack-add-vm-image/image4.png)

   Görüntü başarıyla oluşturulduğunda, VM görüntüsü durumu değişerek **başarılı**.

4. Sanal makine görüntüsü kullanıcı Arabiriminde kullanıcı tüketimi için daha kolay kullanılabilir hale getirmek için iyi bir fikir olduğunu [bir Market öğesi oluşturma](azure-stack-create-and-publish-marketplace-item.md).

## <a name="remove-a-vm-image-through-the-portal"></a>Portal üzerinden bir VM görüntüsü Kaldır

1. Yönetim portalında Aç [ https://adminportal.local.azurestack.external ](https://adminportal.local.azurestack.external).

2. Seçin **Market Yönetim**ve ardından silmek istediğiniz VM'yi seçin.

3. **Sil**'e tıklayın.

## <a name="add-a-vm-image-to-the-marketplace-by-using-powershell"></a>PowerShell kullanarak bir VM görüntüsü Market'te Ekle

> [!Note]  
> Şablonlar ve PowerShell dağıtımları, yalnızca Azure kaynak yöneticisi için kullanılabilir olacak bir görüntü tabanlı eklerken. Görüntü kullanılabilir hale getirmek için bir kullanıcılarınızın bir Market öğesi olarak makalesindeki adımları kullanarak Market öğesi yayımlama [oluşturma ve bir Market öğesi yayımlama](https://docs.microsoft.com/en-us/azure/azure-stack/azure-stack-create-and-publish-marketplace-item)

1. [Azure Stack için PowerShell yükleme](azure-stack-powershell-install.md).  

2. Azure Stack için operatör oturum açın. Yönergeler için [Azure Stack operatör olarak oturum açın](azure-stack-powershell-configure-admin.md).

3. PowerShell ile yükseltilmiş istemi açın ve çalıştırın:

  ````PowerShell  
    Add-AzsPlatformimage -publisher "<publisher>" `
      -offer "<offer>" `
      -sku "<sku>" `
      -version "<#.#.#>” `
      -OSType "<ostype>" `
      -OSUri "<osuri>"
  ````

  **Ekle AzsPlatformimage** cmdlet'i, VM görüntüsünün başvurmak için Azure Resource Manager şablonları tarafından kullanılan değerleri belirtir. Değerler şunlardır:
  - **Yayımcı**  
    Örneğin, `Canonical`  
    Bunlar görüntüsünü dağıtırken, kullanıcıların bir VM görüntüsü Yayımcı adı kesimi. Bir örnek **Microsoft**. Bu alanda bir boşluk veya diğer özel karakterleri dahil değildir.  
  - **Teklif**  
    Örneğin, `UbuntuServer`  
    Kullanıcı VM görüntüsü dağıttıklarında kullanan bir VM görüntüsü teklif adı kesimi. Bir örnek **WindowsServer**. Bu alanda bir boşluk veya diğer özel karakterleri dahil değildir.  
  - **SKU**  
    Örneğin, `14.04.3-LTS`  
    Kullanıcı VM görüntüsü dağıttıklarında kullanan bir VM görüntüsü SKU adı kesimi. Bir örnek **Datacenter2016**. Bu alanda bir boşluk veya diğer özel karakterleri dahil değildir.  
  - **Sürüm**  
    Örneğin, `1.0.0`  
    Kullanıcı VM görüntüsü dağıttıklarında kullanan bir VM görüntüsü sürümü. Bu sürüm biçimindedir *\#.\#.\#*. Bir örnek **1.0.0**. Bu alanda bir boşluk veya diğer özel karakterleri dahil değildir.  
  - **osType**  
    Örneğin, `Linux`  
    Görüntünün osType olmalıdır **Windows** veya **Linux**.  
  - **OSUri**  
    Örneğin, `https://storageaccount.blob.core.windows.net/vhds/Ubuntu1404.vhd`  
    Belirtebileceğiniz bir blob depolama URI'si için bir `osDisk`.  

    PowerShell başvurusu için daha fazla bilgi için bkz. [Ekle AzsPlatformimage](https://docs.microsoft.com/powershell/module/azs.compute.admin/add-azsplatformimage) cmdlet'i ve [yeni DataDiskObject](https://docs.microsoft.com/powershell/module/Azs.Compute.Admin/New-DataDiskObject) cmdlet'i.

## <a name="add-a-custom-vm-image-to-the-marketplace-by-using-powershell"></a>PowerShell kullanarak özel bir VM görüntüsü Market'te Ekle

1. [Azure Stack için PowerShell yükleme](azure-stack-powershell-install.md).

  ```PowerShell  
    # Create the Azure Stack operator's Azure Resource Manager environment by using the following cmdlet:
    Add-AzureRMEnvironment `
      -Name "AzureStackAdmin" `
      -ArmEndpoint $ArmEndpoint

    Set-AzureRmEnvironment `
      -Name "AzureStackAdmin" `
      -GraphAudience $GraphAudience

    $TenantID = Get-AzsDirectoryTenantId `
      -AADTenantName "<myDirectoryTenantName>.onmicrosoft.com" `
      -EnvironmentName AzureStackAdmin

    Add-AzureRmAccount `
      -EnvironmentName "AzureStackAdmin" `
      -TenantId $TenantID
  ```

2. Kullanıyorsanız **Active Directory Federasyon Hizmetleri**, aşağıdaki cmdlet'i kullanın:

  ```PowerShell
  # For Azure Stack Development Kit, this value is set to https://adminmanagement.local.azurestack.external. To get this value for Azure Stack integrated systems, contact your service provider.
  $ArmEndpoint = "<Resource Manager endpoint for your environment>"

  # For Azure Stack Development Kit, this value is set to https://graph.local.azurestack.external/. To get this value for Azure Stack integrated systems, contact your service provider.
  $GraphAudience = "<GraphAuidence endpoint for your environment>"

  # Create the Azure Stack operator's Azure Resource Manager environment by using the following cmdlet:
  Add-AzureRMEnvironment `
    -Name "AzureStackAdmin" `
    -ArmEndpoint $ArmEndpoint
    ```

3. Azure Stack için operatör oturum açın. Yönergeler için [Azure Stack operatör olarak oturum açın](azure-stack-powershell-configure-admin.md).

4. Küresel Azure veya Azure Stack'te kendi özel VM görüntüsü depolamak için bir depolama hesabı oluşturun. Yönergeler için bkz [hızlı başlangıç: yükleme, indirme ve Azure portalını kullanarak blobları listeleme](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal).

5. (VHDX değil) VHD biçiminde bir Windows veya Linux işletim sistemi görüntüsü hazırlama, görüntünün depolama hesabınıza yükleyin ve burada VM görüntüsü PowerShell tarafından alınabilir URİ'sini Al.  

  ````PowerShell  
    Add-AzureRmAccount `
      -EnvironmentName "AzureStackAdmin" `
      -TenantId $TenantID
  ````

6. (İsteğe bağlı) Veri diskleri bir dizi VM görüntüsü bir parçası olarak karşıya yükleyebilirsiniz. New-DataDiskObject cmdlet'ini kullanarak, veri diskleri oluşturun. Yükseltilmiş isteminden PowerShell'i açın ve çalıştırın:

  ````PowerShell  
    New-DataDiskObject -Lun 2 `
    -Uri "https://storageaccount.blob.core.windows.net/vhds/Datadisk.vhd"
  ````

7. PowerShell ile yükseltilmiş istemi açın ve çalıştırın:

  ````PowerShell  
    Add-AzsPlatformimage -publisher "<publisher>" -offer "<offer>" -sku "<sku>" -version "<#.#.#>” -OSType "<ostype>" -OSUri "<osuri>"
  ````

    Microsoft PowerShell Ekle AzsPlatformimage cmdlet'ini ve yeni DataDiskObject cmdlet'i hakkında daha fazla bilgi için bkz. [Azure Stack operatörü modülü belgeleri](https://docs.microsoft.com/powershell/module/).

## <a name="remove-a-vm-image-by-using-powershell"></a>PowerShell kullanarak bir VM görüntüsü Kaldır

Karşıya yüklediğiniz sanal makine görüntüsü, artık gerektiğinde, aşağıdaki cmdlet'i kullanarak marketten silebilirsiniz:

1. [Azure Stack için PowerShell yükleme](azure-stack-powershell-install.md).

2. Azure Stack için operatör oturum açın.

3. PowerShell ile yükseltilmiş istemi açın ve çalıştırın:

  ````PowerShell  
  Remove-AzsPlatformImage `
    -publisher "<publisher>" `
    -offer "<offer>" `
    -sku "<sku>" `
    -version "<version>" `
  ````
  **Remove-AzsPlatformImage** cmdlet'i, VM görüntüsünün başvurmak için Azure Resource Manager şablonları tarafından kullanılan değerleri belirtir. Değerler şunlardır:
  - **Yayımcı**  
    Örneğin, `Canonical`  
    Bunlar görüntüsünü dağıtırken, kullanıcıların bir VM görüntüsü Yayımcı adı kesimi. Bir örnek **Microsoft**. Bu alanda bir boşluk veya diğer özel karakterleri dahil değildir.  
  - **Teklif**  
    Örneğin, `UbuntuServer`  
    Kullanıcı VM görüntüsü dağıttıklarında kullanan bir VM görüntüsü teklif adı kesimi. Bir örnek **WindowsServer**. Bu alanda bir boşluk veya diğer özel karakterleri dahil değildir.  
  - **SKU**  
    Örneğin, `14.04.3-LTS`  
    Kullanıcı VM görüntüsü dağıttıklarında kullanan bir VM görüntüsü SKU adı kesimi. Bir örnek **Datacenter2016**. Bu alanda bir boşluk veya diğer özel karakterleri dahil değildir.  
  - **Sürüm**  
    Örneğin, `1.0.0`  
    Kullanıcı VM görüntüsü dağıttıklarında kullanan bir VM görüntüsü sürümü. Bu sürüm biçimindedir *\#.\#.\#*. Bir örnek **1.0.0**. Bu alanda bir boşluk veya diğer özel karakterleri dahil değildir.  
    
    Remove-AzsPlatformImage cmdlet'i hakkında daha fazla bilgi için bkz. Microsoft PowerShell [Azure Stack operatörü modülü belgeleri](https://docs.microsoft.com/powershell/module/).

## <a name="next-steps"></a>Sonraki adımlar

[Sanal makine sağlama](azure-stack-provision-vm.md)
