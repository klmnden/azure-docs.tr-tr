---
title: Bir VM görüntüsü Azure yığınına ekleyip | Microsoft Docs
description: Kuruluşunuzun özel Windows veya Linux VM görüntü kullanılmak üzere kiracılar için ekleyip yeniden açın.
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
ms.date: 05/10/2018
ms.author: mabrigg
ms.reviewer: kivenkat
ms.openlocfilehash: 39708248160b029185b64ed927a453562e1003f2
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/12/2018
---
# <a name="make-a-virtual-machine-image-available-in-azure-stack"></a>Bir sanal makine görüntüsü Azure yığın kullanılabilmesini

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

Azure yığınında, sanal makine görüntülerini kullanıcılarınıza sunabilirsiniz. Bu görüntüleri Azure Resource Manager şablonları tarafından başvurulabilir veya Azure Market kullanıcı arabirimine bir Market öğesi olarak ekleyebilirsiniz. Ya da bir görüntü formu genel Azure Marketi kullanın veya kendi özel görüntünüzü ekleyin. Portalı veya Windows PowerShell'i kullanarak bir VM'i ekleyebilirsiniz.

## <a name="add-a-vm-image-through-the-portal"></a>Portal üzerinden bir VM görüntüsü ekleme

> [!NOTE]
> Bu yöntemle Market öğesi ayrı ayrı oluşturmanız gerekir.

Görüntüleri bir blob depolama URI'si tarafından başvurulan kurabilmesi gerekir. (VHDX değil) VHD biçiminde bir Windows veya Linux işletim sistemi görüntüsünü hazırlama ve Azure ya da Azure yığın depolama hesabı görüntüsünü yükleyin. Görüntünüzü zaten Azure ya da Azure yığın blob depolama alanına karşıya yüklediyseniz, 1. adım atlayabilirsiniz.

1. [Bir Windows VM görüntüsü için Azure Resource Manager dağıtımları için karşıya](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-upload-image/) ya da, bir Linux görüntü için bölümünde açıklanan yönergeleri izleyerek [dağıtmak Linux sanal makineleri Azure yığında](azure-stack-linux.md). Görüntüyü karşıya yüklemeden önce aşağıdaki etmenleri dikkate almak önemlidir:

   - Azure yığını sabit disk VHD biçimini destekler. Bu disk uzaklığı X X blob uzaklığı depolanan için sabit bir biçime doğrusal olarak dosyanın içinde mantıksal disk yapıları. Blob sonunda küçük altbilgi VHD özelliklerini açıklar. Diskinizin giderilip doğrulamak için kullanın [Get-VHD](https://docs.microsoft.com/powershell/module/hyper-v/get-vhd?view=win10-ps) PowerShell komutu.  

    > [!IMPORTANT]
    >  Azure yığın dinamik disk VHD desteklemez. Bir VM'ye ekli dinamik bir disk yeniden boyutlandırma VM başarısız durumda bırakır. Bu sorunu azaltmak için VM'in disk, VHD blob depolama hesabındaki silmeden VM silin. Dönüştürme, bir dinamik disk VHD'den bir sabit diske ve sanal makine'yeniden oluşturun.

   * Görüntüyü Azure yığın görüntü deposuna Gönder daha az zaman alır çünkü için Azure blob depolama daha Azure yığın blob depolama birimine görüntüyü karşıya daha etkilidir.

   * Karşıya yüklediğiniz zaman [Windows VM görüntüsü](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-upload-image/), değiştirdiğinizden emin olun **Azure'da oturum aç** ile adım [Azure yığın işlecin PowerShell ortamını yapılandırma](azure-stack-powershell-configure-admin.md) adım.  

   * Blob depolama burada görüntüyü karşıya yükleme URI'si not edin. Blob depolama URI'si aşağıdaki biçime sahiptir: *&lt;storageAccount&gt;/&lt;blobContainer&gt;/&lt;targetVHDName&gt;*.vhd.

   * Blob anonim olarak erişilebilir olması için burada VM görüntüsü VHD yüklenen depolama hesabı blob kapsayıcısına gidin. Seçin **Blob**ve ardından **erişim ilkesi**. İsteğe bağlı olarak, bunun yerine kapsayıcı için bir paylaşılan erişim imzası oluşturmak de blob URI'si parçası olarak dahil edebilirsiniz.

   ![Depolama hesabı BLOB'larını gidin](./media/azure-stack-add-vm-image/image1.png)

   ![Ayarlama blob erişimi için ortak](./media/azure-stack-add-vm-image/image2.png)

2. Azure yığınına operatör olarak oturum açın. Menüde seçin **daha fazla hizmet** > **kaynak sağlayıcıları**. Ardından, seçin **işlem** > **VM görüntüleri** > **Ekle**.

3. Altında **bir VM görüntüsü eklemek**, yayımcı, teklif, SKU ve sanal makine görüntüsünün sürümü girin. Bu ad kesimler Resource Manager şablonları VM görüntüsündeki bakın. Seçtiğinizden emin olun **osType** doğru değeri. İçin **işletim sistemi Disk Blob URİ'si**, burada görüntüyü karşıya Blob URİ'si girin. Ardından, seçin **oluşturma** VM görüntüsü oluşturmaya başlamak için.

   ![Görüntü oluşturmaya başla](./media/azure-stack-add-vm-image/image4.png)

   Görüntüyü başarıyla oluşturulduğunda, VM görüntü durumu değişikliklerini **başarılı**.

4. Sanal makine görüntüsü kullanıcı Arabiriminde kullanıcı tüketimi için daha kolay kullanılabilir hale getirmek için iyi bir fikir olduğu [Market öğesi oluşturma](azure-stack-create-and-publish-marketplace-item.md).

## <a name="remove-a-vm-image-through-the-portal"></a>Bir VM görüntüsü portal üzerinden Kaldır

1. Yönetim portalında açmak [ https://adminportal.local.azurestack.external ](https://adminportal.local.azurestack.external).

2. Seçin **Market Yönetim**ve ardından silmek istediğiniz VM seçin.

3. **Sil**'e tıklayın.

## <a name="add-a-vm-image-to-the-marketplace-by-using-powershell"></a>PowerShell kullanarak bir VM görüntüsü Market'te ekleme

1. [PowerShell için Azure Yığın Yükle](azure-stack-powershell-install.md).  

2. Azure yığınına bir operatör olarak oturum açın. Yönergeler için bkz: [oturum açmak için bir işleç olarak Azure yığın](azure-stack-powershell-configure-admin.md).

3. PowerShell ile yükseltilmiş istemi açın ve çalıştırın:

  ````PowerShell  
    Add-AzsPlatformimage -publisher "<publisher>" `
      -offer "<offer>" `
      -sku "<sku>" `
      -version "<#.#.#>” `
      -OSType "<ostype>" `
      -OSUri "<osuri>"
  ````

  **Ekle AzsPlatformimage** cmdlet VM görüntüsü başvurmak için Azure Resource Manager şablonları tarafından kullanılan değerleri belirtir. Değerler şunlardır:
  - **Yayımcı**  
    Örneğin, `Canonical`  
    Kullanıcıların görüntüsünü dağıtırken kullanan VM görüntüsü Yayımcı adı kesimi. Örnek **Microsoft**. Boşluk veya diğer özel karakterleri bu alana dahil etmeyin.  
  - **Teklif**  
    Örneğin, `UbuntuServer`  
    VM görüntüsü dağıttığınızda, kullanıcıların kullanan VM görüntüsü teklif adı kesimi. Örnek **Windows Server**. Boşluk veya diğer özel karakterleri bu alana dahil etmeyin.  
  - **SKU**  
    Örneğin, `14.04.3-LTS`  
    VM görüntüsü dağıttığınızda, kullanıcıların kullanan VM görüntüsü SKU adı kesimi. Örnek **Datacenter2016**. Boşluk veya diğer özel karakterleri bu alana dahil etmeyin.  
  - **Sürüm**  
    Örneğin, `1.0.0`  
    VM görüntüsü dağıttığınızda, kullanıcıların kullanan VM görüntüsü sürümü. Bu sürüm biçimindedir *\#.\#.\#*. Örnek **1.0.0**. Boşluk veya diğer özel karakterleri bu alana dahil etmeyin.  
  - **osType**  
    Örneğin, `Linux`  
    Görüntünün osType aşağıdakilerden biri olması gerekir **Windows** veya **Linux**.  
  - **OSUri**  
    Örneğin, `https://storageaccount.blob.core.windows.net/vhds/Ubuntu1404.vhd`  
    Blob depolama URI'si belirtmek için bir `osDisk`.  

    Add-AzsPlatformimage cmdlet'i hakkında daha fazla bilgi için bkz: Microsoft PowerShell [Azure yığın işleci modülünün belgelerine](https://docs.microsoft.com/powershell/module/).

## <a name="add-a-custom-vm-image-to-the-marketplace-by-using-powershell"></a>PowerShell kullanarak özel bir VM görüntüsü Market'te ekleme

1. [PowerShell için Azure Yığın Yükle](azure-stack-powershell-install.md).

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

3. Azure yığınına bir operatör olarak oturum açın. Yönergeler için bkz: [oturum açmak için bir işleç olarak Azure yığın](azure-stack-powershell-configure-admin.md).

4. Genel Azure veya Azure yığın özel VM görüntüsü depolamak için bir depolama hesabı oluşturun. Yönergeler için bkz [hızlı başlangıç: karşıya yükleme, indirme ve Azure portalını kullanarak listesi BLOB'ları](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal).

5. VHD biçiminde (VHDX değil) Windows veya Linux işletim sistemi görüntüsünü hazırlama, depolama hesabınıza görüntüyü karşıya yükleme ve burada VM görüntüsü PowerShell tarafından alınabilecek URI'sini Al.  

  ````PowerShell  
    Add-AzureRmAccount `
      -EnvironmentName "AzureStackAdmin" `
      -TenantId $TenantID
  ````

6. (İsteğe bağlı) Veri diskleri bir dizi VM görüntüsü bir parçası olarak yükleyebilirsiniz. Yeni DataDiskObject cmdlet'ini kullanarak, veri diski oluşturun. Yükseltilmiş isteminden PowerShell'i açın ve çalıştırın:

  ````PowerShell  
    New-DataDiskObject -Lun 2 `
    -Uri "https://storageaccount.blob.core.windows.net/vhds/Datadisk.vhd"
  ````

7. PowerShell ile yükseltilmiş istemi açın ve çalıştırın:

  ````PowerShell  
    Add-AzsPlatformimage -publisher "<publisher>" -offer "<offer>" -sku "<sku>" -version "<#.#.#>” -OSType "<ostype>" -OSUri "<osuri>"
  ````

    Add-AzsPlatformimage cmdlet'ini ve yeni DataDiskObject cmdlet'i hakkında daha fazla bilgi için bkz: Microsoft PowerShell [Azure yığın işleci modülünün belgelerine](https://docs.microsoft.com/powershell/module/).

## <a name="remove-a-vm-image-by-using-powershell"></a>PowerShell kullanarak bir VM görüntüsü kaldırma

Karşıya yüklediğiniz sanal makine görüntüsü artık ihtiyacınız olduğunda aşağıdaki cmdlet'i kullanarak marketten silebilirsiniz:

1. [PowerShell için Azure Yığın Yükle](azure-stack-powershell-install.md).

2. Azure yığınına bir operatör olarak oturum açın.

3. PowerShell ile yükseltilmiş istemi açın ve çalıştırın:

  ````PowerShell  
  Remove-AzsPlatformImage `
    -publisher "<publisher>" `
    -offer "<offer>" `
    -sku "<sku>" `
    -version "<version>" `
  ````
  **Kaldır AzsPlatformImage** cmdlet VM görüntüsü başvurmak için Azure Resource Manager şablonları tarafından kullanılan değerleri belirtir. Değerler şunlardır:
  - **Yayımcı**  
    Örneğin, `Canonical`  
    Kullanıcıların görüntüsünü dağıtırken kullanan VM görüntüsü Yayımcı adı kesimi. Örnek **Microsoft**. Boşluk veya diğer özel karakterleri bu alana dahil etmeyin.  
  - **Teklif**  
    Örneğin, `UbuntuServer`  
    VM görüntüsü dağıttığınızda, kullanıcıların kullanan VM görüntüsü teklif adı kesimi. Örnek **Windows Server**. Boşluk veya diğer özel karakterleri bu alana dahil etmeyin.  
  - **SKU**  
    Örneğin, `14.04.3-LTS`  
    VM görüntüsü dağıttığınızda, kullanıcıların kullanan VM görüntüsü SKU adı kesimi. Örnek **Datacenter2016**. Boşluk veya diğer özel karakterleri bu alana dahil etmeyin.  
  - **Sürüm**  
    Örneğin, `1.0.0`  
    VM görüntüsü dağıttığınızda, kullanıcıların kullanan VM görüntüsü sürümü. Bu sürüm biçimindedir *\#.\#.\#*. Örnek **1.0.0**. Boşluk veya diğer özel karakterleri bu alana dahil etmeyin.  
    
    Remove-AzsPlatformImage cmdlet'i hakkında daha fazla bilgi için bkz: Microsoft PowerShell [Azure yığın işleci modülünün belgelerine](https://docs.microsoft.com/powershell/module/).

## <a name="next-steps"></a>Sonraki adımlar

[Sanal makine sağlama](azure-stack-provision-vm.md)