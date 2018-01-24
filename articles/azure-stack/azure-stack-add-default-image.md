---
title: "Varsayılan VM görüntüsü Azure yığın Marketinde ekleme | Microsoft Docs"
description: "Windows Server 2016 VM varsayılan görüntü Azure yığın Marketinde ekleyin."
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: 2849E53F-3D58-48A5-8007-3238FC39F630
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 1/23/2018
ms.author: mabrigg
ms.openlocfilehash: b0b0a4af1d852de516d387697afb2760b967db43
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
# <a name="add-the-windows-server-2016-vm-image-to-the-azure-stack-marketplace"></a>Windows Server 2016 VM görüntüsü Azure yığın Marketinde ekleme

Varsayılan olarak, hiçbir sanal makine görüntü Azure yığın Marketi'nde kullanılabilir. Bir Azure yığın işleç erişmeleri kullanıcılara Market görüntü eklemeniz gerekir. Aşağıdaki yöntemlerden birini kullanarak Azure yığın Marketinde Windows Server 2016 görüntü ekleyebilirsiniz:

* [Azure Marketi'nde görüntüyü indirmeyi](#add-the-image-by-downloading-it-from-the-azure-marketplace). Bağlı bir senaryoda işletim ve Azure yığın örneğinizi Azure ile kaydettiğiniz bu seçeneği kullanın.

* [PowerShell kullanarak görüntü eklemek](#add-the-image-by-using-powershell). Azure yığın bağlantısı kesilmiş bir senaryo veya senaryoları ile sınırlı bağlantı dağıttıysanız bu seçeneği kullanın.

## <a name="add-the-image-by-downloading-it-from-the-azure-marketplace"></a>Azure Marketi'nde yükleyerek görüntüsü ekleme

1. Azure yığın dağıtın ve ardından, Azure yığın Geliştirme Seti için oturum açın.

2. Seçin **daha fazla hizmet** > **Market Yönetim** > **azure'dan Ekle**. 

3. Bulma veya arama **Windows Server 2016 Datacenter** görüntü ve ardından **karşıdan**.

   ![Görüntü Azure'dan indirin](media/azure-stack-add-default-image/download-image.png)

Yükleme tamamlandığında, görüntü altında kullanılabilir **Market Yönetim**. Görüntü ayrıca altında kullanılabilir **işlem** ve yeni sanal makineler oluşturmak kullanılabilir.

## <a name="add-the-image-by-using-powershell"></a>PowerShell kullanarak görüntü ekleme

### <a name="prerequisites"></a>Önkoşullar 

Aşağıdaki Önkoşullar, araçtan çalıştırmak [Geliştirme Seti](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-remote-desktop) veya kullanıyorsanız Windows tabanlı bir dış istemci, [VPN üzerinden bağlı](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn):

1. Yükleme [Azure yığın uyumlu Azure PowerShell modülleri](azure-stack-powershell-install.md).  

2. Karşıdan [Azure yığın ile çalışmak için gereken araçları](azure-stack-powershell-download.md).  

3. Windows Server değerlendirmeleri sayfasında [Windows Server 2016 değerlendirmeyi indirme](https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2016). İstendiğinde, indirme ISO sürümünü seçin. Daha sonra bu makalede açıklanan adımları kullanılır yükleme konumunun yolunu kaydedin. Bu adım Internet bağlantısı gerektirir.  

### <a name="add-the-image-to-the-azure-stack-marketplace"></a>Azure yığın Market görüntüsü ekleme
   
1. Yığın Azure Connect ve ComputeAdmin modülleri aşağıdaki komutları kullanarak içeri aktarın:

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
     -ADFS `
     -EnvironmentName "AzureStackAdmin" 

    Login-AzureRmAccount `
      -EnvironmentName "AzureStackAdmin" `
      -TenantId $TenantID 
    ```
   
3. Windows Server 2016 görüntüsü Azure yığın Marketinde ekleyin. (Değiştir *fully_qualified_path_to_ISO* yüklediğiniz Windows Server 2016 ISO yoluyla.)

    ```PowerShell
    $ISOPath = "<fully_qualified_path_to_ISO>"

    # Add a Windows Server 2016 Evaluation VM image.
    New-AzsServer2016VMImage `
      -ISOPath $ISOPath

    ```

Windows Server 2016 VM görüntüsü en son toplu güncelleştirmeyi olduğundan emin olmak için dahil `IncludeLatestCU` çalıştırdığınızda parametre `New-AzsServer2016VMImage` cmdlet'i. İzin verilen parametreleri hakkında bilgi için `New-AzsServer2016VMImage` cmdlet'ini bkz [parametreleri](#parameters). Görüntüyü Azure yığın Marketinde yayımlama için yaklaşık bir saat sürer. 

## <a name="parameters-for-new-azsserver2016vmimage"></a>Yeni AzsServer2016VMImage için parametreler

### <a name="new-azsserver2016vmimage"></a>AzsServer2016VMImage yeni 

Oluşturur ve yeni bir sunucu 2016 çekirdeği yükler ve veya tam yansımasını ve Market öğesi oluşturur.

| Parametreler | Gerekli | Örnek | Açıklama |
|-----|-----|------|---- |
|ISOPath|Evet| N:\ISO\en_windows_16_x64_dvd | Karşıdan yüklenen Windows Server 2016 ISO tam yolu.|
|Net35|Hayır| True | Windows Server 2016 görüntüde .NET 3.5 çalışma zamanı yükler. Varsayılan olarak, bu değeri ayarlamak **doğru**.|
|Sürüm|Hayır| Tam |  Belirtir **çekirdek**, **tam**, veya **her ikisi de** Windows Server 2016 görüntüler. Varsayılan olarak, bu değeri ayarlamak **tam**.|
|VHDSizeInMB|Hayır| 40,960 | Azure yığın ortamınıza eklenecek VHD görüntüsü boyutu (MB) cinsinden ayarlar. Varsayılan olarak, bu değer 40.960 MB olarak ayarlanır.|
|CreateGalleryItem|Hayır| True | Market öğesi için Windows Server 2016 görüntü oluşturulması gerekip gerekmediğini belirtir. Varsayılan olarak, bu değeri ayarlamak **doğru**.|
|location |Hayır | D:\ | Windows Server 2016 görüntü yayımlanması gerekir konumunu belirtir.|
|IncludeLatestCU|Hayır| False | En son Windows Server 2016 toplu güncelleştirme yeni VHD'ye uygular. En son güncelleştirme ya da sonraki iki seçenekten birini kullanın işaret ettiğinden emin olmak için komut dosyasını denetleyin. |
|CUUri |Hayır | https://yourupdateserver/winservupdate2016 | Belirli bir URİ'den çalıştırmak için toplu güncelleştirme Windows Server 2016 ayarlar. |
|CUPath |Hayır | C:\winservupdate2016 | Yerel bir yoldan çalıştırmaya için toplu güncelleştirme Windows Server 2016 ayarlar. Bu seçenek, bağlantısı kesilmiş bir ortam Azure yığın örneğinde dağıttıysanız yararlıdır.|

## <a name="next-steps"></a>Sonraki adımlar

[Sanal makine sağlama](azure-stack-provision-vm.md)
