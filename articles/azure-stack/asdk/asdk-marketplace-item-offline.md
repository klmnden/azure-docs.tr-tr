---
title: Yerel bir kaynaktan Azure yığın Market öğe ekleme | Microsoft Docs
description: Yerel işletim sistemi görüntüsü Azure yığın Marketinde eklemeyi açıklar.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: PowerShell
ms.topic: tutorial
ms.custom: mvc
ms.date: 04/20/2018
ms.author: jeffgilb
ms.reviewer: misainat
ms.openlocfilehash: a093e60718881b2fe9ca70df7596e8963dc55d9f
ms.sourcegitcommit: 6cf20e87414dedd0d4f0ae644696151e728633b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/06/2018
ms.locfileid: "34808052"
---
# <a name="tutorial-add-an-azure-stack-marketplace-item-from-a-local-source"></a>Öğretici: Azure yığın Market öğesi yerel bir kaynaktan ekleyin.

Bir Azure yığın operatör olarak, kullanıcıların bir sanal makine (VM) dağıtmanıza olanak sağlamak için yapmanız gereken ilk şey bir VM görüntüsü Azure yığın Market eklemektir. Varsayılan olarak, hiçbir şey Azure yığın Market yayımlanır, ancak VM ISO görüntüleri kullanıcılarınız için kullanılabilir duruma getirin karşıya yükleyebilirsiniz. Azure yığın bağlantısı kesilmiş bir senaryo veya senaryoları ile sınırlı bağlantı dağıttıysanız bu seçeneği kullanın.

Bu öğreticide, bir Windows Server 2016 VM görüntüsünü karşıdan Windows Server değerlendirmeleri sayfasında ve Azure yığın Market yükleyin.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Windows Server 2016 VM görüntüsü ekleme
> * VM görüntüsü kullanılabilir olduğunu doğrulayın 
> * Test VM görüntüsü

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için:

- Yükleme [Azure yığın uyumlu Azure PowerShell modülleri](asdk-post-deploy.md#install-azure-stack-powershell)
- Karşıdan [Azure yığın araçları](asdk-post-deploy.md#download-the-azure-stack-tools)
- Karşıdan [Windows Server 2106 sanal makine ISO görüntüsü](https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2016) Windows Server değerlendirmeleri sayfasından

## <a name="add-a-windows-server-vm-image"></a>Windows Server VM görüntüsü ekleme
PowerShell kullanarak daha önce indirilmiş bir ISO görüntü ekleyerek, Windows Server 2016 görüntüyü Azure yığın Market yayımlayabilirsiniz. 

Azure yığın bağlantısı kesilmiş bir senaryo veya senaryoları ile sınırlı bağlantı dağıttıysanız bu seçeneği kullanın.

1. [PowerShell için Azure Yığın Yükle](../azure-stack-powershell-install.md).

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

3. Azure yığınına bir operatör olarak oturum açın. Yönergeler için bkz: [oturum açmak için bir işleç olarak Azure yığın](../azure-stack-powershell-configure-admin.md).

   ````PowerShell  
    Add-AzureRmAccount `
      -EnvironmentName "AzureStackAdmin" `
      -TenantId $TenantID
  ````

4. Windows Server 2016 görüntüsü Azure yığın Market ekleyin.

    **Ekle AzsPlatformimage** görüntüsü eklemek için kullanılan cmdlet VM görüntüsü başvurmak için Azure Resource Manager şablonları tarafından kullanılan değerleri belirtir.
    
    Değerler şunlardır:
    
  - **Yayımcı**  
    Örneğin, `Microsoft`  
    Kullanıcıların görüntüsünü dağıtırken kullanan VM görüntüsü Yayımcı adı kesimi. Örnek **Microsoft**. Boşluk veya diğer özel karakterleri bu alana dahil etmeyin.  
  - **Teklif**  
    Örneğin, `WindowsServer`  
    VM görüntüsü dağıttığınızda, kullanıcıların kullanan VM görüntüsü teklif adı kesimi. Örnek **Windows Server**. Boşluk veya diğer özel karakterleri bu alana dahil etmeyin.  
  - **SKU**  
    Örneğin, `Datacenter2016`  
    VM görüntüsü dağıttığınızda, kullanıcıların kullanan VM görüntüsü SKU adı kesimi. Örnek **Datacenter2016**. Boşluk veya diğer özel karakterleri bu alana dahil etmeyin.  
  - **Sürüm**  
    Örneğin, `1.0.0`  
    VM görüntüsü dağıttığınızda, kullanıcıların kullanan VM görüntüsü sürümü. Bu sürüm biçimindedir *\#.\#.\#*. Örnek **1.0.0**. Boşluk veya diğer özel karakterleri bu alana dahil etmeyin.  
  - **osType**  
    Örneğin, `Windows`  
    Görüntünün osType aşağıdakilerden biri olması gerekir **Windows** veya **Linux**. Değiştir *fully_qualified_path_to_ISO* yüklediğiniz Windows Server 2016 ISO yoluna sahip. 
  - **OSUri**  
    Örneğin, `https://storageaccount.blob.core.windows.net/vhds/Ubuntu1404.vhd`  
    Blob depolama URI'si belirtmek için bir `osDisk`. Bu durumda, indirdiğiniz görüntünün depolandığı konumun belirtmeniz gerekecektir.

    Daha fazla bilgi için bkz. PowerShell başvurusu için [Ekle AzsPlatformimage](https://docs.microsoft.com/powershell/module/azs.compute.admin/add-azsplatformimage) cmdlet'i.

    PowerShell ile yükseltilmiş istemi açın ve çalıştırın:

      ````PowerShell  
        Add-AzsPlatformimage -publisher "Microsoft" -offer "WindowsServer" -sku "Datacenter2016" -version "1.0.0” -OSType "Windows" -OSUri "<fully_qualified_path_to_ISO>"
      ````  

## <a name="verify-the-marketplace-item-is-available"></a>Market öğesi kullanılabilir olduğunu doğrulayın
Yeni VM görüntüsü Azure yığın marketi'ndeki kullanılabilir olduğunu doğrulamak için aşağıdaki adımları kullanın.

1. Oturum [ASDK Yönetici portalı](https://adminportal.local.azurestack.external).

2. Seçin **daha fazla hizmet** > **Market Yönetim**. 

3. Windows Server 2016 Datacenter VM görüntüsü başarıyla eklendiğini doğrulayın.

   ![Azure indirilen görüntüden](media/asdk-marketplace-item/azs-marketplace-ws2016.png)

## <a name="test-the-vm-image"></a>Test VM görüntüsü
Bir Azure yığın operatör olarak kullanabileceğiniz [Yönetici portalı](https://adminportal.local.azurestack.external) bir test oluşturmak için görüntüyü doğrulamak için VM başarılı bir şekilde kullanıma sunulmuştur. 

1. Oturum [ASDK Yönetici portalı](https://adminportal.local.azurestack.external).

2. Tıklatın **yeni** > **işlem** > **Windows Server 2016 Datacenter** > **oluşturma**.  
 ![Market görüntüsünden VM oluşturma](media/asdk-marketplace-item/new-compute.png)

3. İçinde **Temelleri** dikey penceresinde, aşağıdaki bilgileri girin ve ardından **Tamam**:
  - **Ad**: test vm 1
  - **Kullanıcı adı**: AdminTestUser
  - **Parola**: AzS TestVM01
  - **Abonelik**: varsayılan sağlayıcı abonelik kabul et
  - **Kaynak grubu**: test vm rg
  - **Konum**: yerel

4. İçinde **bir boyutu seçin** dikey penceresinde tıklatın **A1 standart**ve ardından **seçin**.  

5. İçinde **ayarları** dikey penceresinde, Varsayılanları kabul edin ve tıklatın **Tamam**

6. Sanal makineyi oluşturmak için **Özet** dikey penceresinde **Tamam** seçeneğine tıklayın.  
> [!NOTE]
> Sanal makine dağıtımı tamamlanması birkaç dakika sürer.

7. Yeni VM görüntülemek için tıklatın **sanal makineleri**, ardından arama **test vm 1** ve onun adına tıklayın.
    ![Görüntülenen ilk test VM görüntüsü](media/asdk-marketplace-item/first-test-vm.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme
Başarıyla VM'ye açtığınız ve test görüntüsü düzgün çalıştığını doğruladıktan sonra test kaynak grubunu silmeniz gerekir. Bu sınırlı kaynakları tek bir düğüm ASDK yüklemeleri için kullanılabilir boş.

Artık gerekli olduğunda, aşağıdaki adımları izleyerek kaynak grubu, VM ve tüm ilgili kaynaklar Sil:

1. Oturum [ASDK Yönetici portalı](https://adminportal.local.azurestack.external).
2. Tıklatın **kaynak grupları** > **test vm rg** > **kaynak grubu Sil**.
3. Tür **test vm rg** 'ye tıklayın ve kaynak grubu adı olarak **silmek**.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Windows Server 2016 VM görüntüsü ekleme
> * VM görüntüsü kullanılabilir olduğunu doğrulayın 
> * Test VM görüntüsü

Bir Azure yığın teklif ve planı nasıl oluşturulacağını öğrenmek için sonraki öğretici ilerleyin.

> [!div class="nextstepaction"]
> [Azure yığın Iaas hizmetleri sunar](asdk-offer-services.md)
