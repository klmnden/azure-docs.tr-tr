---
title: Azure Market öğelerini indirme | Microsoft Docs
description: Bulut operatörü, Azure Stack dağıtımım için Azure'dan Market öğelerini indirme.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 02/14/2019
ms.author: sethm
ms.reviewer: unknown
ms.lastreviewed: 12/10/2018
ms.openlocfilehash: 9bf261918bdbdf3ee06ad28a037d5bb8a3631a20
ms.sourcegitcommit: 0dd053b447e171bc99f3bad89a75ca12cd748e9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58487643"
---
# <a name="download-marketplace-items-from-azure-to-azure-stack"></a>Azure Stack için Azure Market öğelerini indirme

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Bulut operatörü olarak, öğeleri Azure Market'ten indirebilir ve bunları Azure Stack'te kullanılabilir duruma getirin. Önceden test ve Azure Stack ile çalışmak için desteklenen Azure Market öğelerinin seçkin bir listeden seçim yapabilirsiniz öğeler görüntülenir. Ek öğeleri genellikle bu listeye eklenir, böylece yeni içerik için geri denetlemeye devam. 

Azure Marketi'nde bağlamak için iki senaryo vardır: 

- **Bağlı bir senaryo** -Azure Stack ortamınız internet'e bağlı olması gerekir. Bulmak ve öğeleri indirmek için Azure Stack portalını kullanın. 
- **Bağlantısı kesilmiş veya kısmen bağlı bir senaryo** -Market öğelerini indirme için Market Dağıtım Aracı'nı kullanarak İnternet'e erişmek gerektirir. Ardından, bağlantısı kesilmiş Azure Stack yüklemenizi indirmelerinizi aktarın. Bu senaryo, PowerShell kullanır.

Bkz: [Azure Stack için Azure Market öğeleri](azure-stack-marketplace-azure-items.md) indirebilirsiniz Market öğelerinin listesi.


## <a name="connected-scenario"></a>Bağlı senaryosu
Azure Stack internet'e bağlanır, Market öğelerini indirme için Yönetim Portalı'nı kullanabilirsiniz.

### <a name="prerequisites"></a>Önkoşullar
Azure Stack dağıtımınıza internet bağlantınız ve olması gerekir [Azure Active Directory'ye](azure-stack-register.md).

### <a name="use-the-portal-to-download-marketplace-items"></a>Market öğelerini indirme için portalı kullanma  
1. Azure Stack Yönetici portalında oturum açın.

2.  Market öğesi indirmeden önce kullanılabilir depolama alanını inceleyin. Daha sonra yüklemek için öğeler seçtiğinizde, indirme boyutu, kullanılabilir depolama kapasitesi karşılaştırabilirsiniz. Kapasitesi sınırlı ise, seçeneklerini göz önünde bulundurun [kullanılabilir alanı yönetme](azure-stack-manage-storage-shares.md#manage-available-space). 

    Kullanılabilir alan gözden geçirmek için **bölge Yönetimi** keşfedin ve ardından gitmek istediğiniz bölgeyi seçin **kaynak sağlayıcıları** > **depolama**.

    [![Depolama alanını inceleyin](media/azure-stack-download-azure-marketplace-item/storagesm.png "depolama alanını inceleyin")](media/azure-stack-download-azure-marketplace-item/storage.png#lightbox)

    
3. Azure Stack Marketini açın ve Azure'a bağlanın. Bunu yapmak için **Market Yönetim**ve ardından **Ekle azure'dan**.

    [![Azure'dan ekleme](media/azure-stack-download-azure-marketplace-item/marketplacesm.png "Azure'dan Ekle")](media/azure-stack-download-azure-marketplace-item/marketplace.png#lightbox)

    Portal, Azure Market'ten indirilebilir öğelerin listesini görüntüler. Açıklaması ve bunu yapanlar hakkında ek bilgi indirme boyutu da dahil olmak üzere görüntülemek için her bir öğede tıklayabilirsiniz. 

    [![Market listesi](media/azure-stack-download-azure-marketplace-item/image03sm.png "Market listesi")](media/azure-stack-download-azure-marketplace-item/image03.png#lightbox)

4. Seçin ve ardından öğeyi **indirme**. Yükleme süreleri değişir.

    [![İleti indirme](media/azure-stack-download-azure-marketplace-item/image04.png "yükleme iletisi")](media/azure-stack-download-azure-marketplace-item/image04.png#lightbox)

    İndirme tamamlandıktan sonra yeni Market öğesi, bir Azure Stack operatörü veya kullanıcı olarak dağıtabilirsiniz.

5. İndirilen öğesi dağıtmak için seçebileceğiniz **+ kaynak Oluştur**ve ardından yeni Market öğesi kategorileri arasından arayın. Sonraki dağıtım işlemini başlatmak için öğeyi seçin. İşlemi farklı bir Market öğesi için değişir. 

## <a name="disconnected-or-a-partially-connected-scenario"></a>Bağlantısı kesilmiş veya kısmen bağlı senaryosu

Azure Stack'i bağlantısız bir modda ve internet bağlantısı olmadan, PowerShell kullanın ve *Market dağıtım aracı* Market öğesi internet bağlantısına sahip bir makineye yüklemek için. Öğeleri daha sonra Azure Stack ortamınıza aktarın. Azure Stack portalını kullanarak Market öğesi bağlantısı kesilmiş bir ortamda indiremez. 

Market Dağıtım Aracı'nı, bağlı bir senaryoda de kullanılabilir. 

Bu senaryo iki bölümü vardır:
- **1. Bölüm:** Azure Market'ten indirin. İnternet erişimi olan bilgisayarda PowerShell yapılandırmak, Dağıtım Aracı'nı indirin ve ardından öğeleri form Azure Marketi'nde indirin.  
- **2. Bölüm:** Karşıya yükleme ve Azure Stack Market'te yayımlayın. Azure Stack'e aktarın ve sonra Azure Stack Marketi'nde içerik yayımlamak Azure Stack ortamınıza indirilen dosyaları Taşı.  


### <a name="prerequisites"></a>Önkoşullar
- Azure Stack dağıtımınıza olmalıdır [Azure Active Directory'ye](azure-stack-register.md).  

- İnternet bağlantısı olan bilgisayar olmalıdır **Azure Stack PowerShell modülü sürüm 1.2.11** veya üzeri. Henüz yoksa, [Azure Stack belirli PowerShell modüllerini yükleyin](azure-stack-powershell-install.md).  

- İndirilen Market öğesinin içeri aktarma özelliğini etkinleştirme [Azure Stack operatörü için PowerShell ortamı](azure-stack-powershell-configure-admin.md) yapılandırılması gerekir.  

- Olmalıdır bir [depolama hesabı](azure-stack-manage-storage-accounts.md) Azure stack'teki (olan bir depolama blobu) ortak olarak erişilebilen bir kapsayıcı vardır. Market öğelerini galeri dosyaları için geçici depolama alanı olarak kapsayıcısını kullanın. Kapsayıcılar ve depolama hesapları ile ilgili bilgi sahibi değilseniz bkz [BLOB'lar - Azure portal ile çalışmak](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal) Azure belgeleri.

- Market Dağıtım Aracı'nı ilk yordam sırasında yüklenir. 

- Yükleyebileceğiniz [AzCopy](../storage/common/storage-use-azcopy.md) en iyi indirme için performans, ancak bu zorunlu değildir.

### <a name="use-the-marketplace-syndication-tool-to-download-marketplace-items"></a>Market öğelerini indirme için Market Dağıtım Aracı'nı kullanın

1. Internet bağlantısı olan bir bilgisayarda, yönetici olarak bir PowerShell konsolu açın.

2. Azure Stack kaydetmek için kullanılan bir Azure hesabı ekleyin. PowerShell'de çalıştırın, hesabı eklemek için `Add-AzureRmAccount` hiçbir parametre olmadan. Azure hesabı kimlik bilgilerinizi girmeniz istenir ve hesabınızın yapılandırmasına bağlı olarak 2 faktörlü kimlik doğrulaması kullanmanız gerekebilir.

3. Birden fazla aboneliğiniz varsa, kayıt için kullanılmakta seçmek için aşağıdaki komutu çalıştırın:  

   ```powershell  
   Get-AzureRmSubscription -SubscriptionID '<Your Azure Subscription GUID>' | Select-AzureRmSubscription
   $AzureContext = Get-AzureRmContext
   ```

4. Aşağıdaki betiği kullanarak Market Dağıtım Aracı'nı en son sürümünü yükleyin:  

   ```powershell
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

5. Dağıtım modülü içeri aktarın ve ardından aşağıdaki komutları çalıştırarak Aracı'nı başlatın. Değiştirin `Destination folder path` Azure Marketi'nden de dosyaları depolamak için bir konum.   

   ```powershell  
   Import-Module .\Syndication\AzureStack.MarketplaceSyndication.psm1

   Export-AzSOfflineMarketplaceItem -Destination "Destination folder path in quotes" 
   ```

6. Araç çalıştığında, kullanılabilir Market öğelerinin listesini içeren aşağıdaki görüntüye benzer bir ekran görmeniz gerekir:

   [![Azure Market öğeleri açılan](media/azure-stack-download-azure-marketplace-item/image05.png "Azure Market öğeleri")](media/azure-stack-download-azure-marketplace-item/image05.png#lightbox)

7. İndirip Not istediğiniz öğeyi seçin *sürüm*. Basılı tutabilirsiniz *Ctrl* birden fazla görüntü seçmek için anahtar. Başvuru *sürüm* içeri aktardığınızda sonraki yordamda öğesi. 
   
   Kullanarak ayrıca görüntülerin listesini filtreleyebilirsiniz **Ölçüt Ekle** seçeneği.

8. Seçin **Tamam**, daha sonra gözden geçirin ve yasal koşulları kabul edin. 

9. İndirme gereken süre, öğe boyutuna bağlıdır. İndirme tamamlandıktan sonra öğesi, betikte belirttiğiniz klasöründe kullanılabilir. İndirme, VHD dosyası (sanal makineler için) veya bir .zip dosyası (sanal makine uzantıları) içerir. Bir galeri paketi içerebilir *.azpkg* biçimi yalnızca bir .zip dosyasıdır.

10. İndirme başarısız olursa, aşağıdaki PowerShell cmdlet'ini yeniden çalıştırarak yeniden deneyebilirsiniz:

    ```powershell
    Export-AzSOfflineMarketplaceItem -Destination "Destination folder path in quotes”
    ```

    Yeniden denemeden önce indirme başarısız ürün klasörü kaldır. Örneğin, yükleme komut dosyası için indirirken başarısız olursa `D:\downloadFolder\microsoft.customscriptextension-arm-1.9.1`, kaldırma `D:\downloadFolder\microsoft.customscriptextension-arm-1.9.1` klasörünü ve ardından cmdlet'i yeniden çalıştırın.
 
### <a name="import-the-download-and-publish-to-azure-stack-marketplace-1811-and-higher"></a>İndirme içeri aktarma ve yayımlama Azure Stack Marketini (1811 ve üzeri)

1. Dosyaları taşımanız [daha önce indirilen](#use-the-marketplace-syndication-tool-to-download-marketplace-items) yerel olarak Azure Stack ortamınıza kullanılabilir durumda. İçeri aktarma işlemi gerçekleştirmek için Aracı'nı kullanmanız gerekir çünkü Market Dağıtım Aracı'nı da Azure Stack ortamınıza kullanılabilir olması gerekir.

   Aşağıdaki resimde, bir klasör yapısı örneği gösterilmektedir. `D:\downloadfolder` indirilen Market öğeleri içerir. Her bir Market öğesi alt klasördür (örneğin, `microsoft.custom-script-linux-arm-2.0.3`), adlandırılan ürün kimliğine göre Her bir alt klasörü Market öğenin indirilen içeriktir.

   [![Market indirme dizin yapısı](media/azure-stack-download-azure-marketplace-item/mp1sm.png "Market indirme dizin yapısı")](media/azure-stack-download-azure-marketplace-item/mp1.png#lightbox)

2. Bölümündeki yönergeleri [bu makalede](azure-stack-powershell-configure-admin.md) Azure Stack operatörü PowerShell oturumu yapılandırmak için. 

3. Dağıtım modülü içeri aktarın ve ardından aşağıdaki komutu çalıştırarak Market Dağıtım Aracı'nı başlatın:

   ```powershell
   $credential = Get-Credential -Message "Enter the azure stack operator credential:"
   Import-AzSOfflineMarketplaceItem -origin "marketplace content folder" -AzsCredential $credential
   ```

   `-origin` Parametresi tüm indirilen ürünleri; içeren üst düzey klasörü belirtir, `"D:\downloadfolder"`.

   `-AzsCredential` Parametresi isteğe bağlıdır. Süresi dolmuşsa erişim belirtecini yenilemek için kullanılır. Varsa `-AzsCredential` parametresi belirtilmezse ve belirteç süre sonu, işleci kimlik bilgilerini girmek için bir uyarı alırsınız.

    > [!Note]  
    > AD FS kullanıcı kimlikleri ile etkileşimli kimlik doğrulaması yalnızca destekler. Bir kimlik bilgisi nesnesi gerekiyorsa, bir hizmet sorumlusu (SPN) kullanmanız gerekir. Azure Stack ve AD FS ile hizmet sorumlusu, Kimlik Yönetimi Hizmeti ayarlama hakkında daha fazla bilgi için bkz. [Yönet AD FS için hizmet sorumlusu](azure-stack-create-service-principals.md#manage-service-principal-for-ad-fs).

4. Betik başarıyla tamamlandıktan sonra öğeyi Azure Stack Market'te kullanılabilir olması gerekir.

### <a name="import-the-download-and-publish-to-azure-stack-marketplace-1809-and-lower"></a>İndirme içeri aktarma ve Azure Stack Marketini (1809 ve daha düşük) yayımlama

1. Sanal makine görüntüleri veya sahip olduğunuz çözüm şablonları dosyalarını [daha önce indirilen](#use-the-marketplace-syndication-tool-to-download-marketplace-items) Azure Stack ortamınıza yerel olarak kullanılabilir hale getirilmelidir.  

2. Market öğesi paket (.azpkg dosyası) ve sanal sabit disk görüntüsü (.vhd dosyası) Azure Stack Blob Depolama'ya yüklemek için yönetim portalını kullanın. Paket karşıya ve disk dosyaları kullanılabilir hale getirir bunları Azure Stack için Azure Stack Marketini daha sonra öğeyi yayımlayabilmeniz.

   Karşıya yükleme, genel olarak erişilebilir kapsayıcısı ile bir depolama hesabına sahip olmasını gerektirir (bu senaryonun önkoşulları bakın).  
   1. Azure Stack Yönetim Portalı'nda Git **tüm hizmetleri** altındaki **veri + depolama** kategorisi seçin **depolama hesapları**.  
   
   2. Aboneliğinizde ve altında bir depolama hesabı seçin **BLOB hizmeti**seçin **kapsayıcıları**.  
      [![BLOB hizmeti](media/azure-stack-download-azure-marketplace-item/blob-service.png "Blob hizmeti")](media/azure-stack-download-azure-marketplace-item/blob-service.png#lightbox)  
   
   3. Kullanın ve ardından istediğiniz kapsayıcıyı seçin **karşıya** açmak için **blobu karşıya yükleme** bölmesi.  
      [![Kapsayıcı](media/azure-stack-download-azure-marketplace-item/container.png "kapsayıcı")](media/azure-stack-download-azure-marketplace-item/container.png#lightbox)  
   
   4. Depolama alanına yüklemek ve ardından diski ve paket dosyaları karşıya yükleme blob bölmesinde, Gözat **karşıya**: [![Karşıya yükleme](media/azure-stack-download-azure-marketplace-item/uploadsm.png "karşıya yükleme")](media/azure-stack-download-azure-marketplace-item/upload.png#lightbox)  

   5. Karşıya dosya kapsayıcı bölmesinde görünür. Bir dosya seçin ve ardından URL'den kopyalayın **Blob özellikleri** bölmesi. Azure Stack'e Market öğesi içeri aktardığınızda bu URL'yi bir sonraki adımda kullanacaksınız.  Aşağıdaki görüntüde kapsayıcısıdır *blob test depolama* ve dosya *Microsoft.WindowsServer2016DatacenterServerCore ARM.1.0.801.azpkg*.  Dosyanın URL'si *https://testblobstorage1.blob.local.azurestack.external/blob-test-storage/Microsoft.WindowsServer2016DatacenterServerCore-ARM.1.0.801.azpkg*.  
      [![BLOB özellikleri](media/azure-stack-download-azure-marketplace-item/blob-storagesm.png "Blob özellikleri")](media/azure-stack-download-azure-marketplace-item/blob-storage.png#lightbox)  

3. VHD görüntüsünü kullanarak Azure Stack'e içeri **Ekle AzsPlatformimage** cmdlet'i. Bu cmdlet'i kullandığınızda değiştirin *yayımcı*, *teklif*ve görüntünün aldığınız değerlerle diğer parametre değerleri. 

   Alabileceğiniz *yayımcı*, *teklif*, ve *sku* AZPKG dosya yüklemeleri metin dosyasından görüntüyü değerleri. Metin dosyasını, hedef konumda depolanır. *Sürüm* öğesi önceki yordamda Azure'dan indirirken belirtilen sürüm değerdir. 
 
   Aşağıdaki örnek betik, Windows Server 2016 Datacenter - Sunucu Çekirdeği sanal makine için değerler kullanılır. Değeri *- Osuri* öğesi için blob depolama konumuna örnek yoludur.

   Bu betik bir alternatif olarak, kullandığınız [bu makalede açıklanan yordamı](azure-stack-add-vm-image.md#add-a-vm-image-through-the-portal) içeri aktarmak için. Azure portalını kullanarak VHD görüntüsü.

   ```powershell  
   Add-AzsPlatformimage `
    -publisher "MicrosoftWindowsServer" `
    -offer "WindowsServer" `
    -sku "2016-Datacenter-Server-Core" `
    -osType Windows `
    -Version "2016.127.20171215" `
    -OsUri "https://mystorageaccount.blob.local.azurestack.external/cont1/Microsoft.WindowsServer2016DatacenterServerCore-ARM.1.0.801.vhd"  
   ```

   **Çözüm şablonları hakkında:** Bazı şablonlar, küçük bir 3 MB dahil edebilirsiniz. VHD dosya adı ile **fixed3.vhd**. Azure Stack için bu dosyayı içeri gerek yoktur. Fixed3.vhd.  Bu dosya, Azure Marketi'nde yayımlama gereksinimlerini karşılamak için bazı çözüm şablonları ile birlikte gelir.

   Şablonları açıklamayı gözden geçirin, indirin ve ardından ek gereksinimler ile çözüm şablonu çalışmak için gereken VHD'ler gibi içeri aktarın.  
   
   **Uzantıları hakkında:** Sanal makine görüntü uzantılarını ile çalışırken, aşağıdaki parametreleri kullanın:
   - *Yayımcı*
   - *Tür*
   - *Sürüm*  

   Seçeneğini kullanmaz *teklif* uzantıları.   


4.  PowerShell kullanarak Azure Stack'e Market öğesi yayımlama kullanmasını **Ekle AzsGalleryItem** cmdlet'i. Örneğin:  
    ```powershell  
    Add-AzsGalleryItem `
     -GalleryItemUri "https://mystorageaccount.blob.local.azurestack.external/cont1/Microsoft.WindowsServer2016DatacenterServerCore-ARM.1.0.801.azpkg" `
     –Verbose
    ```
5. Bir galeri öğesi yayımladıktan sonra artık kullanmak kullanılabilir. Galeri öğesi yayımlandığında onaylamak için Git **tüm hizmetleri**ve ardından altındaki **genel** kategorisi seçin **Market**.  İndirme işleminiz bir çözüm şablonu, bu çözüm şablonu herhangi bir bağımlı VHD görüntü eklediğinizden emin olun.  
  [![Görünüm Market](media/azure-stack-download-azure-marketplace-item/view-marketplacesm.png "görünümü Market")](media/azure-stack-download-azure-marketplace-item/view-marketplace.png#lightbox)  

Azure Stack PowerShell 1.3.0'ın yayınlanmasıyla birlikte, artık sanal makine uzantıları ekleyebilirsiniz. Örneğin:

```powershell
Add-AzsVMExtension -Publisher "Microsoft" -Type "MicroExtension" -Version "0.1.0" -ComputeRole "IaaS" -SourceBlob "https://github.com/Microsoft/PowerShell-DSC-for-Linux/archive/v1.1.1-294.zip" -SupportMultipleExtensions -VmOsType "Linux"
```

## <a name="next-steps"></a>Sonraki adımlar

[Bir Market öğesi oluşturma ve yayımlama](azure-stack-create-and-publish-marketplace-item.md)