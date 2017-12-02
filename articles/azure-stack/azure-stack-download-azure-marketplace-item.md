---
title: "Market öğesi Azure'dan karşıdan | Microsoft Docs"
description: "My Azure yığın dağıtımına Azure'dan ı Market öğesi indirebilirsiniz."
services: azure-stack
documentationcenter: 
author: ErikjeMS
manager: byronr
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 11/30/2017
ms.author: erikje
ms.openlocfilehash: 33b7be4a85723ab03e4c656a8dd28632ad854e29
ms.sourcegitcommit: 80eb8523913fc7c5f876ab9afde506f39d17b5a1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/02/2017
---
# <a name="download-marketplace-items-from-azure-to-azure-stack"></a>Market öğesi Azure'dan Azure yığınına indirin.

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

Azure yığın marketi'ndeki eklemek üzere hangi içerik karar gibi Azure Marketi'nden içerik göz önünde bulundurmalısınız. Azure yığında çalıştırmak için önceden sınanmış Azure Market öğesi seçkin listesinden indirebilirsiniz. Yeni öğeler sık bu listeye eklenir, bu nedenle yeni içerik için geri denetlediğinizden emin olun.

## <a name="download-marketplace-items-in-a-connected-scenario-with-internet-connectivity"></a>Market öğesi bağlı senaryosunda (internet bağlantısı ile) yükle

1. Market öğesi indirmek için öncelikle [Azure yığın Azure ile kaydedin](azure-stack-register.md). 
2. Azure yığın Yönetici portalı'na (https://portal.local.azurestack.external) oturum açın.
3. Bazı Market öğesi büyük olabilir. Yeterli alan sisteminizde tıklayarak olduğundan emin olmak için kontrol edin **kaynak sağlayıcıları** > **depolama**.

    ![](media/azure-stack-download-azure-marketplace-item/image01.png)

4. Tıklatın **daha fazla hizmet** > **Market Yönetim**.

    ![](media/azure-stack-download-azure-marketplace-item/image02.png)

4. Tıklatın **azure'dan Ekle** indirilebilir öğelerinin bir listesini görmek için. Açıklamasını görüntülemek ve yükleme boyutu için listesindeki her bir öğeyi tıklatabilirsiniz.

    ![](media/azure-stack-download-azure-marketplace-item/image03.png)

5. Listede istediğiniz ve ardından öğeyi seçin **karşıdan**. Bu VM görüntüsü, seçili öğe için indirme başlatır. Yükleme süreleri değişir.

    ![](media/azure-stack-download-azure-marketplace-item/image04.png)

6. İndirme tamamlandıktan sonra yeni Market öğesi bir Azure yığın işleç veya kullanıcı olarak dağıtabilirsiniz. Tıklatın **+ yeni**arama yeni Market öğesi için kategorileri arasında ve ardından öğeyi seçin.
7. Tıklatın **oluşturma** yeni indirilen öğesi oluşturma deneyimini açın. Öğenizi dağıtmak için adım adım yönergeleri izleyin.

## <a name="download-marketplace-items-in-a-disconnected-or-a-partially-connected-scenario-with-limited-internet-connectivity"></a>Bağlantısı kesilmiş bir Market öğelerini veya kısmen bağlı bir senaryo (sınırlı internet bağlantısı ile) yükleme

Bağlantısı kesilmiş bir modda (olmadan herhangi bir internet bağlantısı) Azure yığın dağıttığınızda, Azure yığın portalını kullanarak Market öğesi karşıdan yükleyemiyor. Ancak, Market öğesi Internet bağlantısına sahip bir makineye yüklemek için Market dağıtım aracını kullanın ve bunları Azure yığın ortamınız için Aktarım. 

### <a name="prerequisites"></a>Ön koşullar
Market Dağıtım Aracı'nı kullanmadan önce bilgisayarınızda yüklü olduğundan emin olun [Azure yığını, Azure aboneliğinizle kayıtlı](azure-stack-register.md).  

Internet bağlantısı olan makineden gerekli Market öğeleri karşıdan yüklemek için aşağıdaki adımları kullanın:

1. Bir yönetici olarak bir PowerShell konsolu açın ve [Azure yığın belirli PowerShell modüllerini yüklemek](azure-stack-powershell-install.md). Yüklediğinizden emin olun **PowerShell sürüm 1.2.11 ya da daha yüksek**.  

2. Azure yığın kaydetmek için kullanılan Azure hesabı ekleyin. Bunu yapmak için çalıştırın **Add-AzureRmAccount** cmdlet parametre olmadan. Azure hesabı kimlik bilgilerinizi girmeniz istenir ve hesabınızın yapılandırmasına bağlı olarak 2 öğeli kimlik doğrulama kullanmak zorunda kalabilirsiniz.  

3. Birden çok aboneliğiniz varsa, kayıt için kullanılan birini seçmek için aşağıdaki komutu çalıştırın:  

   ```powershell
   Get-AzureRmSubscription -SubscriptionID '<Your Azure Subscription GUID>' | Select-AzureRmSubscription
   $AzureContext = Get-AzureRmContext
   ```

4. Aşağıdaki komut dosyasını kullanarak Market dağıtım aracının en son sürümünü yükleyin:  

   ```PowerShell
   # Download the tools archive.
   invoke-webrequest https://github.com/Azure/AzureStack-Tools/archive/vnext.zip `
     -OutFile vnext.zip

   # Expand the downloaded files.
   expand-archive vnext.zip `
     -DestinationPath . `
     -Force

   # Change to the tools directory.
   cd \AzureStack-Tools-vnext

   ```

5. Dağıtım modülü içe aktarın ve aşağıdaki komutları çalıştırarak Aracı'nı başlatın:  

   ```powershell
   Import-Module .\ Syndication\AzureStack.MarketplaceSyndication.psm1

   Sync-AzSOfflineMarketplaceItem `
     -destination “<Destination folder path>” `
     -AzureTenantID $AzureContext.Tenant.TenantId `
     -AzureSubscriptionId $AzureContext.Subscription.Id  
   ```

6. Araç çalıştığında Azure hesabı kimlik bilgilerinizi girmeniz istenir. Azure yığın kaydetmek için kullanılan Azure hesabınızda oturum açın. Oturum açma başarılı, sonra aşağıdaki ekranı kullanılabilir Market öğesi listesini görmelisiniz.  

   ![Azure Market öğelerini açılan](./media/azure-stack-download-azure-marketplace-item/image05.png)

7. Yüklemek istediğiniz görüntüyü seçin (birden fazla görüntü Ctrl tuşunu basılı tutarak seçebilirsiniz) ve resim sürümünü not edin, bu sürümü bir sonraki bölümde görüntüyü içe aktarmak için kullanacağınız > tıklatın **Tamam** > yasal koşulları kabul tıklayarak **Evet**. Kullanarak görüntüleri listesini filtreleyebilirsiniz **ölçüt eklemek** seçeneği. İndirme görüntünün boyutuna bağlı olarak biraz uzun sürebilir. Bir kez resim yüklemelerini daha önce sağlanan hedef yolu kullanılabilir. İndirme Azpkg biçimde VHD dosyasını ve galeri öğeleri içerir.  

### <a name="import-the-image-and-publish-it-to-azure-stack-marketplace"></a>Görüntü alma ve Azure yığın marketinde yayımlama

1. Görüntü & galeri paketi indirdikten sonra bunları ve içeriği çıkarılabilir disk sürücüsüne AzureStack araçları vnext klasörüne kaydedin ve Azure yığın ortamına kopyalayın (için yerel olarak herhangi bir yere gibi kopyalayabilirsiniz: "C:\MarketplaceImages".)   

2. Görüntü almadan önce Azure yığın işlecin ortamına açıklanan adımları kullanarak bağlamanız gerekir [Azure yığın işlecin PowerShell ortamını yapılandırma](azure-stack-powershell-configure-admin.md).  

3. Görüntü, Add-AzsVMImage cmdlet'ini kullanarak Azure yığınına içeri aktarın. Bu cmdlet'i kullanırken, yayımcı, teklif ve diğer parametre değerlerini İçeri aktarmakta olduğunuz görüntü değerlerle değiştirdiğinizden emin olun. 6. adımda önceki bölümde "publisher", "Teklif" ve "sku" değerlerini daha önce indirdiğiniz Azpkg dosyasının Imagereference nesnesinden görüntüsünün ve "Sürüm" değerini alabilir.

   ```json
   "imageReference": {
      "publisher": "MicrosoftWindowsServer",
      "offer": "WindowsServer",
      "sku": "2016-Datacenter-Server-Core"
    }
   ```

   Parametre değerlerini değiştirin ve aşağıdaki komutu çalıştırın:

   ```powershell
   Import-Module .\ComputeAdmin\AzureStack.ComputeAdmin.psm1

   Add-AzsVMImage `
    -publisher "MicrosoftWindowsServer" `
    -offer "WindowsServer" `
    -sku "2016-Datacenter-Server-Core" `
    -osType Windows `
    -Version "2017.09.25" `
    -OsDiskLocalPath "C:\AzureStack-Tools-master\Syndication\Microsoft.WindowsServer2016DatacenterServerCore-ARM-Eval.2017.09.25.vhd" `
    -CreateGalleryItem $False `
    -Location Local 
   ```

4. Market öğesi karşıya yüklemek için kullanım portal (. Azpkg) Azure yığın Blob Depolama. Yerel Azure yığın depolama alanına karşıya yükleyebilir veya Azure Storage'a yükler. (Bunu bir geçici paketi için konumdur.) Blob genel olarak erişilebilir olduğundan emin olun ve URI not edin.  

5. Market öğesi kullanarak Azure yığınına yayımlamak **Ekle AzureRMGalleryItem**. Örneğin:

   ```powershell
   Add-AzsGalleryItem `
     -GalleryItemUri "https://mystorageaccount.blob.local.azurestack.external/cont1/Microsoft.WindowsServer2016DatacenterServerCore-ARM.1.0.2.azpkg" `
     –Verbose
   ```

6. Galeri öğesi yayımlandıktan sonra buradan görüntüleyebilirsiniz **yeni** > **Market** bölmesi.  

   ![Market](./media/azure-stack-download-azure-marketplace-item/image06.png)

## <a name="next-steps"></a>Sonraki adımlar

[Oluşturma ve bir Market öğesi yayımlama](azure-stack-create-and-publish-marketplace-item.md)
