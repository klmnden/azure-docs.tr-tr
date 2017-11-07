---
title: "Bir VM görüntüsü Azure yığına ekleme | Microsoft Docs"
description: "Kuruluşunuzun özel Windows veya Linux VM görüntü kullanılmak üzere kiracılar için ekleyin."
services: azure-stack
documentationcenter: 
author: SnehaGunda
manager: byronr
editor: 
ms.assetid: e5a4236b-1b32-4ee6-9aaa-fcde297a020f
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/25/2017
ms.author: sngun
ms.openlocfilehash: 520e4dfaadf1d476447a600ef2b3d092b6955a89
ms.sourcegitcommit: 6acb46cfc07f8fade42aff1e3f1c578aa9150c73
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/18/2017
---
# <a name="make-a-custom-virtual-machine-image-available-in-azure-stack"></a>Bir özel sanal makine görüntüsü Azure yığın kullanılabilmesini

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

Azure yığınında işleçleri özel bir sanal makine görüntüleri, kullanıcılar sunabilirsiniz. Bu görüntüleri Azure Resource Manager şablonları tarafından başvurulabilir veya Azure Market kullanıcı arabirimine bir Market öğesi olarak ekleyebilirsiniz. 

## <a name="add-a-vm-image-to-marketplace-by-using-powershell"></a>PowerShell kullanarak Markete bir VM görüntüsü ekleme

Aşağıdaki Önkoşullar, araçtan çalıştırmak [Geliştirme Seti](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-remote-desktop) veya kullanıyorsanız Windows tabanlı bir dış istemci, [VPN üzerinden bağlı](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn):

1. [PowerShell için Azure Yığın Yükle](azure-stack-powershell-install.md).  

2. Karşıdan [Azure yığın ile çalışmak için gereken araçları](azure-stack-powershell-download.md).  

3. VHD biçiminde bir Windows veya Linux işletim sistemi sanal sabit disk görüntüsü hazırlamak (VHDX biçimi kullanmayın).
   
   * Görüntüyü, hazırlama hakkında yönergeler için Windows görüntülerini görmek için [bir Windows VM görüntüsü için Azure Resource Manager dağıtımları için karşıya](../virtual-machines/windows/upload-generalized-managed.md).
   * Linux görüntüleri için bkz: [dağıtmak Linux sanal makineleri Azure yığında](azure-stack-linux.md). Görüntü hazırlayın veya var olan bir Azure yığın Linux görüntüsünü makalesinde açıklanan adımları tamamlayın.  

Azure yığın Market görüntüsü eklemek için aşağıdaki adımları tamamlayın:

1. Connect ve ComputeAdmin modülleri içeri aktarın:
   
   ```powershell
   Set-ExecutionPolicy RemoteSigned

   # Import the Connect and ComputeAdmin modules.
   Import-Module .\Connect\AzureStack.Connect.psm1
   Import-Module .\ComputeAdmin\AzureStack.ComputeAdmin.psm1
   ``` 

2. Azure yığın ortamınız için oturum açın. Olup Azure Active Directory (Azure AD) veya Active Directory Federasyon Hizmetleri (AD FS) kullanarak Azure yığın ortamınızı dağıtıldığına bağlı olarak aşağıdaki betikler birini çalıştırın. (Azure AD Değiştir `tenantName`, `GraphAudience` uç noktasını ve `ArmEndpoint` ortam yapılandırmanızı yansıtacak şekilde değerleri.)

    * **Azure Active Directory**. Aşağıdaki cmdlet'i kullanın:

      ```PowerShell
      # For Azure Stack Development Kit, this value is set to https://adminmanagement.local.azurestack.external. To get this value for Azure Stack integrated systems, contact your service provider.
      $ArmEndpoint = "<Resource Manager endpoint for your environment>"

      # For Azure Stack Development Kit, this value is set to https://graph.windows.net/. To get this value for Azure Stack integrated systems, contact your service provider.
      $GraphAudience = "<GraphAuidence endpoint for your environment>"
      
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

      Login-AzureRmAccount `
        -EnvironmentName "AzureStackAdmin" `
        -TenantId $TenantID 
      ```

   * **Active Directory Federasyon Hizmetleri**. Aşağıdaki cmdlet'i kullanın:
    
        ```PowerShell
        # For Azure Stack Development Kit, this value is set to https://adminmanagement.local.azurestack.external. To get this value for Azure Stack integrated systems, contact your service provider.
        $ArmEndpoint = "<Resource Manager endpoint for your environment>"

        # For Azure Stack Development Kit, this value is set to https://graph.local.azurestack.external/. To get this value for Azure Stack integrated systems, contact your service provider.
        $GraphAudience = "<GraphAuidence endpoint for your environment>"

        # Create the Azure Stack operator's Azure Resource Manager environment by using the following cmdlet:
        Add-AzureRMEnvironment `
          -Name "AzureStackAdmin" `
          -ArmEndpoint $ArmEndpoint

        Set-AzureRmEnvironment `
          -Name "AzureStackAdmin" `
          -GraphAudience $GraphAudience `
          -EnableAdfsAuthentication:$true

        $TenantID = Get-AzsDirectoryTenantId `
          -ADFS 
          -EnvironmentName AzureStackAdmin 

        Login-AzureRmAccount `
          -EnvironmentName "AzureStackAdmin" `
          -TenantId $TenantID 
        ```
    
3. VM görüntüsü çağırarak eklemek `Add-AzsVMImage` cmdlet'i. İçinde `Add-AzsVMImage` cmdlet'ini belirtin `osType` Windows veya Linux olarak. Yayımcı, teklif, SKU ve VM görüntüsü için sürüm içerir. İzin verilen parametreleri hakkında daha fazla bilgi için bkz: [parametreleri](#parameters). Parametreleri VM görüntüsü başvurmak için Azure Resource Manager şablonları tarafından kullanılır. Aşağıdaki örnek komut dosyasını çağırır:
     
  ```powershell
  Add-AzsVMImage `
    -publisher "Canonical" `
    -offer "UbuntuServer" `
    -sku "14.04.3-LTS" `
    -version "1.0.0" `
    -osType Linux `
    -osDiskLocalPath 'C:\Users\AzureStackAdmin\Desktop\UbuntuServer.vhd' `
  ```

Komutu şunları yapar:

* Azure yığın ortama kimliğini doğrular.
* Yerel VHD yeni oluşturulan geçici depolama hesabına yükler.
* VM görüntüsü VM görüntü deposu ekler.
* Market öğesi oluşturur.

Komut Portalı'nda başarıyla çalıştırdığını doğrulamak için Market gidin. VM görüntüsü kullanılabilir olduğundan emin olun **sanal makineleri** kategorisi.

![VM görüntüsü başarıyla eklendi](./media/azure-stack-add-vm-image/image5.PNG) 

## <a name="remove-a-vm-image-by-using-powershell"></a>PowerShell kullanarak bir VM görüntüsü kaldırma

Karşıya yüklediğiniz sanal makine görüntüsü artık ihtiyacınız olduğunda aşağıdaki cmdlet'i kullanarak marketten silebilirsiniz:

```powershell
Remove-AzsVMImage `
  -publisher "Canonical" `
  -offer "UbuntuServer" `
  -sku "14.04.3-LTS" `
  -version "1.0.0" `
```

## <a name="parameters"></a>Parametreler

| Parametre | Açıklama |
| --- | --- |
| **Yayımcı** |Kullanıcıların görüntüsünü dağıtırken kullanan VM görüntüsü Yayımcı adı kesimi. Örnek **Microsoft**. Boşluk veya diğer özel karakterleri bu alana dahil etmeyin. |
| **Teklif** |VM görüntüsü dağıttığınızda, kullanıcıların kullanan VM görüntüsü teklif adı kesimi. Örnek **Windows Server**. Boşluk veya diğer özel karakterleri bu alana dahil etmeyin. |
| **SKU** |VM görüntüsü dağıttığınızda, kullanıcıların kullanan VM görüntüsü SKU adı kesimi. Örnek **Datacenter2016**. Boşluk veya diğer özel karakterleri bu alana dahil etmeyin. |
| **Sürüm** |VM görüntüsü dağıttığınızda, kullanıcıların kullanan VM görüntüsü sürümü. Bu sürüm biçimindedir *\#.\#.\#*. Örnek **1.0.0**. Boşluk veya diğer özel karakterleri bu alana dahil etmeyin. |
| **osType** |Görüntünün osType aşağıdakilerden biri olması gerekir **Windows** veya **Linux**. |
| **osDiskLocalPath** |İşletim sistemi diski olarak bir VM görüntüsü Azure yığınına karşıya yüklediğiniz VHD yerel yolu. |
| **dataDiskLocalPaths** |VM görüntüsü bir parçası olarak yüklenen veri diskleri için yerel yollar isteğe bağlı bir dizi. |
| **CreateGalleryItem** |Bir öğe Marketi'ndeki oluşturulup oluşturulmayacağını belirler mantıksal bayrak. Varsayılan olarak ayarlanır **doğru**. |
| **Başlık** |Market öğesi görünen adı. Varsayılan olarak ayarlanır `Publisher-Offer-Sku` VM görüntüsü değeri. |
| **Açıklama** |Market öğesi açıklaması. |
| **konum** |VM görüntüsü Burada yayımlanan konumu. Varsayılan olarak, bu değeri ayarlamak **yerel**.|
| **osDiskBlobURI** |(İsteğe bağlı) Bu komut, bir Blob Depolama URI'si de kabul eder için `osDisk`. |
| **dataDiskBlobURIs** |(İsteğe bağlı) Bu komut dosyası ayrıca veri diski için görüntü ekleme için Blob Depolama URI'ler bir dizi kabul eder. |

## <a name="add-a-vm-image-through-the-portal"></a>Portal üzerinden bir VM görüntüsü ekleme

> [!NOTE]
> Bu yöntemle Market öğesi ayrı ayrı oluşturmanız gerekir.

Görüntüleri bir Blob Depolama URI'si tarafından başvurulan kurabilmesi gerekir. (VHDX değil) VHD biçiminde bir Windows veya Linux işletim sistemi görüntüsünü hazırlama ve Azure ya da Azure yığın depolama hesabı görüntüsünü yükleyin. Görüntünüzü zaten Azure ya da Azure yığın Blob depolama alanına karşıya yüklediyseniz, 1. adım atlayabilirsiniz.

1. [Bir Windows VM görüntüsü için Azure Resource Manager dağıtımları için karşıya](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-upload-image/) ya da, bir Linux görüntü için bölümünde açıklanan yönergeleri izleyerek [dağıtmak Linux sanal makineleri Azure yığında](azure-stack-linux.md). Görüntüyü karşıya yüklemeden önce aşağıdaki etmenleri dikkate almak önemlidir:

   * Görüntüyü Azure yığın görüntü deposuna Gönder daha az zaman alır çünkü bir görüntü Azure yığın Blob Depolama Azure Blob depolama alanına yüklemek için daha verimli olur. 
   
   * Karşıya yüklediğiniz zaman [Windows VM görüntüsü](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-upload-image/), değiştirdiğinizden emin olun **Azure'da oturum aç** ile adım [Azure yığın işlecin PowerShell ortamını yapılandırma](azure-stack-powershell-configure-admin.md) adım.  

   * Blob Depolama burada görüntüyü karşıya yükleme URI'si not edin. Blob Depolama URI'si aşağıdaki biçime sahiptir:  *&lt;storageAccount&gt;/&lt;blobContainer&gt;/&lt;targetVHDName&gt;* .vhd.

   * Blob anonim olarak erişilebilir olması için burada VM görüntüsü VHD yüklenen depolama hesabı blob kapsayıcısına gidin. Seçin **Blob**ve ardından **erişim ilkesi**. İsteğe bağlı olarak, bunun yerine kapsayıcı için bir paylaşılan erişim imzası oluşturmak de blob URI'si parçası olarak dahil edebilirsiniz.

   ![Depolama hesabı BLOB'larını gidin](./media/azure-stack-add-vm-image/image1.png)

   ![Ayarlama blob erişimi için ortak](./media/azure-stack-add-vm-image/image2.png)

2. Azure yığınına operatör olarak oturum açın. Menüde seçin **daha fazla hizmet** > **kaynak sağlayıcıları**. Ardından, seçin **işlem** > **VM görüntüleri** > **Ekle**.

3. Altında **bir VM görüntüsü eklemek**, yayımcı, teklif, SKU ve sanal makine görüntüsünün sürümü girin. Bu ad kesimler Resource Manager şablonları VM görüntüsündeki bakın. Seçtiğinizden emin olun **osType** doğru değeri. İçin **OD Disk Blob URİ'si**, burada görüntüyü karşıya Blob URİ'si girin. Ardından, seçin **oluşturma** VM görüntüsü oluşturmaya başlamak için.
   
   ![Görüntü oluşturmaya başla](./media/azure-stack-add-vm-image/image4.png)

   Görüntüyü başarıyla oluşturulduğunda, VM görüntü durumu değişikliklerini **başarılı**.

4. Sanal makine görüntüsü kullanıcı Arabiriminde kullanıcı tüketimi için daha kolay kullanılabilir hale getirmek için iyi bir fikir olduğu [Market öğesi oluşturma](azure-stack-create-and-publish-marketplace-item.md).

## <a name="next-steps"></a>Sonraki adımlar

[Sanal makine sağlama](azure-stack-provision-vm.md)