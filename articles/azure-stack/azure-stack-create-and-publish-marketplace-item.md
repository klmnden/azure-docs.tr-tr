---
title: Oluşturma ve Azure Stack'te bir Market öğesi yayımlama | Microsoft Docs
description: Oluşturma ve Azure Stack'te bir Market öğesi yayımlama.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/14/2018
ms.author: brenduns
ms.reviewer: jeffgo
ms.openlocfilehash: 101686149c0e3faaf442c58f4002cbbfe0e72eaa
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "35647226"
---
# <a name="create-and-publish-a-marketplace-item"></a>Market öğesi oluşturma ve yayımlama

*İçin geçerlidir: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

## <a name="create-a-marketplace-item"></a>Bir Market öğesi oluşturma
1. [İndirme](http://www.aka.ms/azurestackmarketplaceitem) Azure galeri Paketleyici araç ve örnek Azure Stack Marketini öğesi.
2. Örnek Market öğesi açın ve yeniden adlandırma **SimpleVMTemplate** klasör. (Market öğesi--aynı adı kullanın, örneğin, **Contoso.TodoList**.) Bu klasör içerir:
   
       /Contoso.TodoList/
       /Contoso.TodoList/Manifest.json
       /Contoso.TodoList/UIDefinition.json
       /Contoso.TodoList/Icons/
       /Contoso.TodoList/Strings/
       /Contoso.TodoList/DeploymentTemplates/
3. [Bir Azure Resource Manager şablonu oluşturma](../azure-resource-manager/resource-group-authoring-templates.md) ya da Github'dan bir şablon seçin. Market öğesi, bir kaynak oluşturmak için bu şablonu kullanılmaktadır.

    > [!Note]  
    > Ürün anahtarları, parola veya müşteri olarak tanımlanabilir bilgileri Azure Resource Manager şablonu gibi herhangi bir gizli anahtar asla sabit kod. Şablon json dosyaları galeride yayımlandıktan sonra kimlik doğrulama gerek kalmadan erişilebilir.  Tüm gizli Store [Key Vault](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-keyvault-parameter) ve bunları şablonda çağırın.

4. Kaynak başarıyla dağıtılabilir emin olmak için Microsoft Azure Stack API'leri ile şablon test edin.
5. Şablonunuzu bir sanal makine görüntüsüne dayanıyorsa, yönergeleri [Azure Stack'e sanal makine görüntüsü ekleme](azure-stack-add-vm-image.md).
6. Azure Resource Manager şablonunuzu kaydetmek **/Contoso.TodoList/DeploymentTemplates/** klasör.
7. Simge ve metin, Market öğesi için seçin. Simgeleri eklemek **simgeler** klasöründe ve metin eklemek **kaynakları** dosyası **dizeleri** klasör. Küçük, Orta, büyük ve geniş bir adlandırma kuralı simgelerini kullanın. Bkz: [Market öğesi kullanıcı Arabirimi başvurusu](#reference-marketplace-item-ui) ayrıntılı bir açıklaması için.
   
   > [!NOTE]
   > Tüm dört simgesi boyutu (küçük, Orta, büyük geniş) Market öğesi doğru şekilde oluşturmak için gereklidir.
   > 
   > 
8. İçinde **manifest.json** dosya, değişiklik **adı** , Market öğesi adı. Ayrıca **yayımcı** ad ya da şirket için.
9. Altında **yapıtları**, değiştirme **adı** ve **yolu** dahil Azure Resource Manager şablonu için doğru bilgileri için.
   
         "artifacts": [
            {
                "name": "Type your template name",
                "type": "Template",
                "path": "DeploymentTemplates\\Type your path",
                "isDefault": true
            }
10. Değiştirin **My Market öğesi** , Market öğesi göründüğü kategori listesi ile.
    
             "categories":[
                 "My Marketplace Items"
              ],
11. İçin diğer tüm düzenlemeleri manifest.json için başvurmak [başvuru: Market öğesi bazıları manifest.json](#reference-marketplace-item-manifestjson).
12. Klasörleri bir .azpkg dosyasına paketlemek için bir komut istemi açın ve aşağıdaki komutu çalıştırın:
    
        AzureGalleryPackager.exe package –m <path to manifest.json> -o <output location for the package>
    
    > [!NOTE]
    > Çıkış paketi tam yolu olmalıdır. Örneğin, çıkış yolunu C:\MarketPlaceItem\yourpackage.azpkg ise, ' % s'klasörü C:\MarketPlaceItem mevcut olması gerekir.
    > 
    > 

## <a name="publish-a-marketplace-item"></a>Bir Market öğesi yayımlama
1. PowerShell veya Azure Depolama Gezgini, Market öğesi (.azpkg) Azure Blob depolama alanına yüklemek için kullanın. Yerel Azure Stack depolama alanına yükleyin ya da Azure Storage'a yükler. (Bu, paket için geçici bir konuma dir.) Blob genel olarak erişilebilir olduğundan emin olun.
2. İstemci sanal makinesinde Microsoft Azure Stack ortamında, Hizmet Yöneticisi kimlik bilgilerinizle PowerShell oturumunuzu ayarlandığını doğrulayın. Azure Stack'te PowerShell'de kimlik doğrulaması hakkında yönergeler bulabilirsiniz [bir şablonu PowerShell ile dağıtma](user/azure-stack-deploy-template-powershell.md).
3. Kullandığınızda [PowerShell 1.3.0]( azure-stack-powershell-install.md) veya daha sonra kullanabileceğiniz **Ekle AzsGalleryItem** Azure Stack'e Market öğesi yayımlama için PowerShell cmdlet'i. PowerShell 1.3.0 kullanılmadan önce cmdlet'ini kullanın **Ekle AzureRMGalleryitem** yerine **Ekle AzsGalleryItem**.  Örneğin, PowerShell 1.3.0 kullanırken ya da daha sonra:
   
       Add-AzsGalleryItem -GalleryItemUri `
       https://sample.blob.core.windows.net/gallerypackages/Microsoft.SimpleTemplate.1.0.0.azpkg –Verbose
   
   | Parametre | Açıklama |
   | --- | --- |
   | Subscriptionıd |Yönetici aboneliğin kimliği. PowerShell kullanarak alabilirsiniz. Portalda almak tercih ediyorsanız, sağlayıcı aboneliğe gidip abonelik kimliğini kopyalayın |
   | GalleryItemUri |BLOB depolama alanına zaten yüklendi, Galeri paketi için URI. |
   | Apiversion |Yap **2015-04-01**. |
4. Portala gidin. Market öğesi--Portalı'nda artık, bir işleç veya bir kullanıcı olarak da görebilirsiniz.
   
   > [!NOTE]
   > Paket görünmesi birkaç dakika sürebilir.
   > 
   > 
5. Market öğeniz artık Azure Stack Marketini için kaydedildi. Blob Depolama konumunuzdan silmeyi seçebilirsiniz.
    > [!Caution]  
    > Tüm varsayılan galeri yapıtlar ve özel galeri yapıtlarınızı artık aşağıdaki URL'ler altında kimlik doğrulaması olmadan erişilebilir:  
`https://adminportal.[Region].[external FQDN]:30015/artifact/20161101/[Template Name]/DeploymentTemplates/Template.json`  
`https://portal.[Region].[external FQDN]:30015/artifact/20161101/[Template Name]/DeploymentTemplates/Template.json`  
`https://systemgallery.blob.[Region].[external FQDN]/dev20161101-microsoft-windowsazure-gallery/[Template Name]/UiDefinition.json`

6. Bir Market öğesi kullanarak kaldırabilirsiniz **Remove-AzureRMGalleryItem** cmdlet'i. Örnek:
   
        Remove-AzureRMGalleryItem -Name Microsoft.SimpleTemplate.1.0.0  –Verbose
   
   > [!NOTE]
   > Bir öğe kaldırdıktan sonra Market UI bir hata gösterebilir. Hatayı düzeltmek için tıklatın **ayarları** portalında. Ardından, **değişiklikleri atmak** altında **portalı özelleştirme**.
   > 
   > 

## <a name="reference-marketplace-item-manifestjson"></a>Başvurusu: Market öğesi manifest.json
### <a name="identity-information"></a>Kimlik bilgileri
| Ad | Gerekli | Tür | Kısıtlamalar | Açıklama |
| --- | --- | --- | --- | --- |
| Ad |X |Dize |[A-Za-z0-9]+ | |
| Yayımcı |X |Dize |[A-Za-z0-9]+ | |
| Sürüm |X |Dize |[SemVer v2](http://semver.org/) | |

### <a name="metadata"></a>Meta Veriler
| Ad | Gerekli | Tür | Kısıtlamalar | Açıklama |
| --- | --- | --- | --- | --- |
| DisplayName |X |Dize |Öneri 80 karakter |Portal 80 karakterden daha uzunsa, öğe adı düzgün görüntülenmeyebilir. |
| PublisherDisplayName |X |Dize |Öneri 30 karakter |Portalda 30 karakterden uzunsa, yayımcı adını düzgün bir şekilde görüntülenmeyebilir. |
| PublisherLegalName |X |Dize |En fazla 256 karakter | |
| Özet |X |Dize |60-100 karakter | |
| LongSummary |X |Dize |140 ile 256 karakter |Henüz geçerli değil Azure Stack'te. |
| Açıklama |X |[HTML](https://auxdocs.azurewebsites.net/en-us/documentation/articles/gallery-metadata#html-sanitization) |500 ila 5.000 karakter | |

### <a name="images"></a>Görüntüler
Market, aşağıdaki simgeleri kullanır:

| Ad | Genişlik | Yükseklik | Notlar |
| --- | --- | --- | --- |
| Geniş |255 px |115 piksel |Her zaman gerekli |
| Büyük |115 piksel |115 piksel |Her zaman gerekli |
| Orta |90 piksel |90 piksel |Her zaman gerekli |
| Küçük |40 piksel |40 piksel |Her zaman gerekli |
| Ekran görüntüsü |533 piksel |32 piksel |İsteğe bağlı |

### <a name="categories"></a>Kategoriler
Her bir Market öğesi öğesi kullanıcı Arabirimi portalda göründüğü tanımlayan bir kategorisiyle etiketlenmiş. Azure Stack'te mevcut kategorilerden birini seçebilirsiniz (işlem, veri + depolama, vb.) veya yeni bir tane seçin.

### <a name="links"></a>Bağlantılar
Her bir Market öğesi çeşitli ek içeriklere bağlantılar içerebilir. Bağlantılar, adları ve bir URI'leri listesi olarak belirtilir.

| Ad | Gerekli | Tür | Kısıtlamalar | Açıklama |
| --- | --- | --- | --- | --- |
| DisplayName |X |Dize |En fazla 64 karakter | |
| Uri |X |URI | | |

### <a name="additional-properties"></a>Ek Özellikler
Önceki meta veriler ek olarak aşağıdaki biçimde özel bir anahtar/değer çifti verileri Market yazarlar sağlayabilirsiniz:

| Ad | Gerekli | Tür | Kısıtlamalar | Açıklama |
| --- | --- | --- | --- | --- |
| DisplayName |X |Dize |En çok 25 karakter | |
| Değer |X |Dize |En çok 30 karakter | |

### <a name="html-sanitization"></a>HTML temizleme
HTML sağlayan herhangi bir alan için aşağıdaki öğelere ve özniteliklere izin verilir:

H1, h2, h3, h4, h5, p, ol, ul, li, bir [hedef | href], br, güçlü, em, b, ı

## <a name="reference-marketplace-item-ui"></a>Başvuru: Market öğesi kullanıcı Arabirimi
Simgeler ve Azure Stack portalında görüldüğü gibi Market öğesi için metin aşağıdaki gibidir.

### <a name="create-blade"></a>Dikey pencere oluşturma
![Dikey pencere oluşturma](media/azure-stack-marketplace-item-ui-reference/image1.png)

### <a name="marketplace-item-details-blade"></a>Market öğesi ayrıntıları dikey penceresi
![Market öğesi ayrıntıları dikey penceresi](media/azure-stack-marketplace-item-ui-reference/image3.png)

