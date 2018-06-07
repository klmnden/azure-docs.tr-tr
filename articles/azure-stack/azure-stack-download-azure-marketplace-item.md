---
title: Market öğesi Azure'dan karşıdan | Microsoft Docs
description: Bulut operatörü, my Azure yığın dağıtımına Azure Market öğesi karşıdan yükleyebilirsiniz.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/16/2018
ms.author: brenduns
ms.reviewer: jeffgo
ms.openlocfilehash: 5d403f7c1d0fff466f6c0fb9942ec777ab820eab
ms.sourcegitcommit: 680964b75f7fff2f0517b7a0d43e01a9ee3da445
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34604541"
---
# <a name="download-marketplace-items-from-azure-to-azure-stack"></a>Market öğesi Azure'dan Azure yığınına indirin.

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

Bir bulut operatörü olarak Azure Marketi'nden öğeleri indirin ve Azure yığınında kullanılabilmesini. Seçebileceğiniz önceden test edilmiş ve desteklenen Azure yığın ile çalışmak için Azure Market öğesi seçkin listesinden öğelerdir. Ek öğeler sık bu listeye eklenir, böylece yeni içerik için geri denetlemeye devam. 

Azure Marketi'nde bağlamak için iki senaryo vardır: 

- **Bağlı bir senaryo** -Azure yığın ortamınız internet'e bağlı olması gerekir. Azure yığın Portalı'nı bulun ve öğeleri karşıdan yüklemek için kullanın. 
- **Bağlantısı kesilmiş veya kısmen bağlı bir senaryo** -Market öğesi indirmek için Market Dağıtım Aracı'nı kullanarak Internet'e erişmek gerektirir. Ardından, bağlantısı kesilmiş Azure yığın yüklemenizi karşıdan yüklemeleri aktarın. Bu senaryo, PowerShell kullanır.

Bkz: [Azure Market öğesi Azure yığınının](azure-stack-marketplace-azure-items.md) indirebilirsiniz Market öğeleri listesi.


## <a name="connected-scenario"></a>Bağlı senaryosu
Azure yığın Internet'e bağlanıyorsa, Market öğesi indirmek için Yönetim Portalı'nı kullanabilirsiniz.

### <a name="prerequisites"></a>Önkoşullar
Azure yığın dağıtımınızı olması Internet bağlantısına sahip ve olması [Azure Active Directory'ye](azure-stack-register.md).

### <a name="use-the-portal-to-download-marketplace-items"></a>Market öğesi indirmek üzere portalı kullanın  
1. Azure yığın Yönetici portalında oturum açın.

2.  Kullanılabilir depolama alanı Market öğesi indirilmeden önce gözden geçirin. Daha sonra yüklemek için öğeler seçtiğinizde, indirme boyutu, kullanılabilir depolama kapasitesinin karşılaştırabilirsiniz. Kapasitesi sınırlı ise, seçeneklerini göz önünde bulundurun [kullanılabilir alan yönetme](azure-stack-manage-storage-shares.md#manage-available-space). 

    Kullanılabilir alan gözden geçirmek için **bölge Yönetimi** keşfedin ve gidin istediğiniz bölgeyi seçin **kaynak sağlayıcıları** > **depolama**.

    ![Depolama alanını gözden geçirin](media/azure-stack-download-azure-marketplace-item/storage.png)  

    
3. Azure yığın Market açın ve Azure'a bağlanın. Bunu yapmak için seçin **Market Yönetim**ve ardından **azure'dan Ekle**.

    ![Azure'dan Ekle](media/azure-stack-download-azure-marketplace-item/marketplace.png)

    Portal, Azure Marketi'nden İndirilebilecek öğeleri listesini görüntüler. Açıklaması ve indirme boyutuna da dahil olmak üzere, ilgili ek bilgileri görüntülemek için her bir öğede tıklatabilirsiniz. 

    ![Market listesi](media/azure-stack-download-azure-marketplace-item/image03.png)

4. Seçin ve ardından öğeyi **karşıdan**. Yükleme süreleri değişir.

    ![İletiyi Yükle](media/azure-stack-download-azure-marketplace-item/image04.png)

    İndirme tamamlandıktan sonra yeni Market öğesi bir Azure yığın işleç veya kullanıcı olarak dağıtabilirsiniz.

5. Karşıdan yüklenen öğe dağıtmak için seçin **+ yeni**ve ardından yeni Market öğesi için kategorileri arasında arayın. Sonraki dağıtım işlemini başlatmak için öğeyi seçin. İşlemi farklı Market öğesi için değişir. 

## <a name="disconnected-or-a-partially-connected-scenario"></a>Bağlantısı kesilmiş veya kısmen bağlı senaryosu

Azure yığın bağlantısız modda ve internet bağlantısı olmadan ise, PowerShell kullanın ve *Market dağıtım aracı* Market öğesi internet bağlantısına sahip bir makineye yüklemek için. Ardından Azure yığın ortamınıza öğeleri aktarın. Bağlantısı kesilmiş bir ortamda Azure yığın portalını kullanarak Market öğesi indiremez. 

Market dağıtım aracı ayrıca bağlı bir senaryoda kullanılabilir. 

Bu senaryo iki bölümü vardır:
- **1. Kısım:** Azure Marketi'nden indirin. İnternet erişimi olan bilgisayarda PowerShell yapılandırmak, Dağıtım Aracı'nı indirin ve öğeleri form Azure Marketi indirin.  
- **2. Kısım:** karşıya yükleyin ve Azure yığın Marketinde yayımlama. Azure yığınına bunları içeri aktarın ve bunları Azure yığın Marketinde yayımlama Azure yığın ortamınıza indirilen dosyaları taşıyın.  


### <a name="prerequisites"></a>Önkoşullar
- Azure yığın dağıtımınızı olmalıdır [Azure Active Directory'ye](azure-stack-register.md).  

- Internet bağlantısına sahip bilgisayarda olmalıdır **Azure yığın PowerShell modülü sürümü 1.2.11** ya da daha yüksek. Henüz yoksa, [Azure yığın belirli PowerShell modüllerini yüklemek](azure-stack-powershell-install.md).  

- İndirilen Market öğesi alınmasıyla etkinleştirmek için [Azure yığın işleci için PowerShell ortamı](azure-stack-powershell-configure-admin.md) yapılandırılması gerekir.  

- Bilmeniz gereken bir [depolama hesabı](azure-stack-manage-storage-accounts.md) Azure yığınındaki (Bu bir depolama blob) genel olarak erişilebilir bir kapsayıcı vardır. Kapsayıcı Market öğelerini galeri dosyaları için geçici depolama alanı olarak kullanın. Depolama hesapları ve kapsayıcıları bilmiyorsanız, bkz: [BLOB'lar - Azure portal ile çalışmak](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal) Azure belgelerinde.

- Market Dağıtım Aracı'nı ilk yordam sırasında yüklenir. 

### <a name="use-the-marketplace-syndication-tool-to-download-marketplace-items"></a>Market öğesi indirmek için Market Dağıtım Aracı'nı kullanın

1. Internet bağlantısı olan bir bilgisayarda, yönetici olarak bir PowerShell konsolu açın.

2. Azure yığın kaydetmek için kullanılan Azure hesabı ekleyin. Çalıştırma PowerShell'de hesabı eklemek için `Add-AzureRmAccount` parametre olmadan. Azure hesabı kimlik bilgilerinizi girmeniz istenir ve hesabınızın yapılandırmasına bağlı olarak 2 Etmenli kimlik doğrulaması kullanmanız gerekebilir.

3. Birden çok aboneliğiniz varsa, kayıt için kullanılan birini seçmek için aşağıdaki komutu çalıştırın:  

   ```PowerShell  
   Get-AzureRmSubscription -SubscriptionID '<Your Azure Subscription GUID>' | Select-AzureRmSubscription
   $AzureContext = Get-AzureRmContext
   ```

4. Aşağıdaki komut dosyasını kullanarak Market dağıtım aracının en son sürümünü yükleyin:  

   ```PowerShell
   # Download the tools archive.
   [Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12 
   invoke-webrequest https://github.com/Azure/AzureStack-Tools/archive/master.zip `
     -OutFile master.zip

   # Expand the downloaded files.
   expand-archive master.zip `
     -DestinationPath . `
     -Force

   # Change to the tools directory.
   cd .\AzureStack-Tools-master

   ```

5. Dağıtım modülü içe aktarın ve sonra aşağıdaki komutu çalıştırarak Aracı'nı başlatın. Değiştir *hedef klasör yolu* Azure Marketi'nden dosyaları depolamak için bir konum.   

   ```PowerShell  
   Import-Module .\Syndication\AzureStack.MarketplaceSyndication.psm1

   Sync-AzSOfflineMarketplaceItem `
     -destination "Destination folder path" `
     -AzureTenantID $AzureContext.Tenant.TenantId `
     -AzureSubscriptionId $AzureContext.Subscription.Id  
   ```

6. Araç çalıştığında Azure hesabı kimlik bilgilerinizi girmeniz istenir. Azure yığın kaydetmek için kullanılan Azure hesabınızda oturum açın. Oturum açma başarılı olduktan sonra aşağıdaki görüntüyle kullanılabilir Market öğeleri listesi gibi bir ekran görmeniz gerekir.  

   ![Azure Market öğelerini açılan](media/azure-stack-download-azure-marketplace-item/image05.png)

7. Karşıdan yükle ve Not istediğiniz öğeyi seçin *sürüm*. (Basılı tutarak birden çok görüntü seçebilirsiniz *Ctrl* anahtar.) Bakacağınız *sürüm* sonraki yordama öğesinde aktardığınızda. 
   
   Kullanarak görüntüleri listesini filtreleyebilirsiniz **ölçüt eklemek** seçeneği.

8. Seçin **Tamam**ve ardından gözden geçirin ve yasal koşulları kabul edin. 

9. İndirme geçen süre öğesi boyutuna bağlıdır. Yükleme tamamlandıktan sonra öğeyi komut dosyasında belirtilen klasöründe kullanılabilir. Bir VHD dosyası (sanal makineler) indirme içerir veya bir. ZIP dosyası (sanal makine uzantıları). Galeri paketinde ayrıca içerir *.azpkg* biçimi. (A *.azpkg* paketi bir *.zip* dosyası.)
 

### <a name="import-the-download-and-publish-to-azure-stack-marketplace"></a>İndirme alma ve Azure yığın Marketinde yayımlama
1. Sanal makine görüntülerini veya çözüm şablonları dosyalarını [daha önce indirilen](#use-the-marketplace-syndication-tool-to-download-marketplace-items) Azure yığın ortamınız için yerel olarak kullanılabilir hale getirilmelidir.  

2. İçeri aktarın. VHD dosyaları Azure yığınına. Başarılı bir şekilde bir sanal makine (VM) görüntüsünü içeri aktarmak için VM hakkında aşağıdaki bilgileri olması gerekir:
   - *Sürüm*, önceki yordamın 7. adımında belirtildiği gibi.
   - Sanal makineleri için değerleri *yayımcı*, *teklif*, ve *sku*. Bu değerleri almak için bir kopyasını yeniden adlandırmak **.azpkg** dosya uzantısını değiştirmek için dosyayı **.zip**. Ardından bir metin Düzenleyicisi'ni açmak için kullanabileceğiniz **DeploymentTemplates\CreateUiDefinition.json**. .Json dosyasında bulun *Imagereference* Market öğesi için bu değerleri içeren bölüm. Aşağıdaki örnek, bu bilgileri nasıl göründüğünü gösterir:

     ```json  
     "imageReference": {  
        "publisher": "MicrosoftWindowsServer",  
        "offer": "WindowsServer",  
        "sku": "2016-Datacenter-Server-Core"  
      }
     ```  

   Görüntü kullanarak Azure yığınına içeri **Ekle AzsPlatformimage** cmdlet'i. Bu cmdlet'i kullanırken değiştirdiğinizden emin olun *yayımcı*, *teklif*ve İçeri aktarmakta olduğunuz görüntünün değerlerle diğer parametre değerleri. Alma *yayımcı*, *sunar*, ve *sku* birlikte AZPKG dosyası indirilir ve hedef konumda metin dosyası görüntüden değerleri . 

   Aşağıdaki örnek komut dosyasında, Windows Server 2016 Datacenter - Sunucu Çekirdeği sanal makine için değerler kullanılır. 

   ```PowerShell  
   Add-AzsPlatformimage `
    -publisher "MicrosoftWindowsServer" `
    -offer "WindowsServer" `
    -sku "2016-Datacenter-Server-Core" `
    -osType Windows `
    -Version "2016.127.20171215" `
    -OsDiskLocalPath "C:\AzureStack-Tools-master\Syndication\Windows-Server-2016-DatacenterCore-20171215-en.us-127GB.vhd" `
   ```
   **Çözüm şablonları hakkında:** bazı şablonlar küçük 3 MB içerebilir. VHD dosya adı ile **fixed3.vhd**. Bu dosyayı Azure yığınına içeri gerek yoktur. Fixed3.vhd.  Bu dosyayı Azure Marketi'nde yayımlama gereksinimlerini karşılamak üzere bazı çözüm şablonları ile birlikte gelir.

   Şablonları tanımını gözden geçirin ve yükleyebilir ve çözüm şablonla çalışması için gerekli VHD gibi ek gereksinimler içeri aktarın.

3. Market öğesi paketi (.azpkg dosyası) Azure yığın Blob depolama alanına yüklemek için Yönetim Portalı'nı kullanın. Daha sonra öğe yayımlayabilirsiniz şekilde karşıya yükleme paketinin Azure yığın Azure yığın Market sağlar.

   Karşıya yükleme, genel olarak erişilebilir kapsayıcısı ile bir depolama hesabına sahip olmasını gerektirir (bu senaryonun önkoşulları bakın)   
   1. Azure yığın Yönetim Portalı'nda Git **daha fazla hizmet** > **depolama hesapları**.  
   
   2. Aboneliğiniz ve altında bir depolama hesabı seçin **BLOB hizmeti**seçin **kapsayıcıları**.  
      ![BLOB hizmeti](media/azure-stack-download-azure-marketplace-item/blob-service.png)  
   
   3. Kullanın ve ardından istediğiniz kapsayıcıyı seçin **karşıya** açmak için **karşıya yükleme blob** bölmesi.  
      ![kapsayıcı](media/azure-stack-download-azure-marketplace-item/container.png)  
   
   4. Depolama alanına yüklemek ve ardından istediğiniz dosyaları karşıya yükleme blob bölmesinde Gözat **karşıya**.  
      ![upload](media/azure-stack-download-azure-marketplace-item/upload.png)  

   5. Karşıya yüklediğiniz dosyaları kapsayıcı bölmesinde görünür. Bir dosya seçin ve ardından URL'yi kopyalayın **Blob özellikleri** bölmesi. Market öğesi Azure yığınına içe aktardığınızda bu URL sonraki adımda kullanacaksınız.  Aşağıdaki görüntüde kapsayıcısıdır *blob test depolama* ve dosya *Microsoft.WindowsServer2016DatacenterServerCore ARM.1.0.801.azpkg*.  URL dosya *https://testblobstorage1.blob.local.azurestack.external/blob-test-storage/Microsoft.WindowsServer2016DatacenterServerCore-ARM.1.0.801.azpkg*.  
      ![BLOB özellikleri](media/azure-stack-download-azure-marketplace-item/blob-storage.png)  

4.  Market öğesi kullanarak Azure yığınına yayımlamak için PowerShell'i kullanma **Ekle AzsGalleryItem** cmdlet'i. Örneğin:  
    ```PowerShell  
    Add-AzsGalleryItem `
     -GalleryItemUri "https://mystorageaccount.blob.local.azurestack.external/cont1/Microsoft.WindowsServer2016DatacenterServerCore-ARM.1.0.801.azpkg" `
     –Verbose
    ```
5. Bir galeri öğesi yayımladıktan sonra ondan görüntüleyebilirsiniz **daha fazla hizmet** > **Market**.  Karşıdan bir çözüm şablonu ise, bu çözüm şablon için bağımlı tüm VHD görüntüsü eklediğinizden emin olun.  
  ![Görünüm Market](media/azure-stack-download-azure-marketplace-item/view-marketplace.png)  

> [!NOTE]
> Azure yığın PowerShell 1.3.0 in çıkışıyla artık sanal makine uzantıları ekleyebilirsiniz.

Örneğin:

````PowerShell
Add-AzsVMExtension -Publisher "Microsoft" -Type "MicroExtension" -Version "0.1.0" -ComputeRole "IaaS" -SourceBlob "https://github.com/Microsoft/PowerShell-DSC-for-Linux/archive/v1.1.1-294.zip" -SupportMultipleExtensions -VmOsType "Linux"
````

## <a name="next-steps"></a>Sonraki adımlar
[Oluşturma ve bir Market öğesi yayımlama](azure-stack-create-and-publish-marketplace-item.md)