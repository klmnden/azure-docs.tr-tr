---
title: Oluşturma ve Azure yığınında Market öğesi yayımlama | Microsoft Docs
description: Oluşturma ve Azure yığınında Market öğesi yayımlama.
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
ms.date: 05/10/2018
ms.author: brenduns
ms.reviewer: jeffgo
ms.openlocfilehash: 5e0349d6bae9295e7a0ba9f366f84753ebd838c2
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/12/2018
---
# <a name="create-and-publish-a-marketplace-item"></a>Market öğesi oluşturma ve yayımlama

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

## <a name="create-a-marketplace-item"></a>Bir Market öğesi oluşturma
1. [Karşıdan](http://www.aka.ms/azurestackmarketplaceitem) Azure Galerisi Paketleyici aracı ve örnek Azure yığın Market öğesi.
2. Örnek Market öğesi açın ve yeniden adlandırma **SimpleVMTemplate** klasör. (Örneğin, Market öğesi olarak--aynı adı kullanın **Contoso.TodoList**.) Bu klasör içerir:
   
       /Contoso.TodoList/
       /Contoso.TodoList/Manifest.json
       /Contoso.TodoList/UIDefinition.json
       /Contoso.TodoList/Icons/
       /Contoso.TodoList/Strings/
       /Contoso.TodoList/DeploymentTemplates/
3. [Bir Azure Resource Manager şablonu oluşturma](../azure-resource-manager/resource-group-authoring-templates.md) veya Github'dan bir şablon seçin. Market öğesi, bir kaynak oluşturmak için bu şablonu kullanır.
4. Kaynak başarıyla dağıtılabilir emin olmak için Microsoft Azure yığın API'leri şablonla sınayın.
5. Şablonunuzu bir sanal makine görüntüsüne dayalıysa, yönergeleri [Azure yığınına bir sanal makine görüntüsü eklemek](azure-stack-add-vm-image.md).
6. Azure Kaynak Yöneticisi şablonunuzu kaydetmek **/Contoso.TodoList/DeploymentTemplates/** klasör.
7. Market öğesi için metin ve simgeleri seçin. Simgeleri ekleme **simgeleri** klasörünü ve metin eklemek **kaynakları** dosyasını **dizeleri** klasör. Küçük, Orta, büyük ve geniş adlandırma kuralı simgelerini kullanın. Bkz: [Market öğesi kullanıcı Arabirimi başvurusu](#reference-marketplace-item-ui) ayrıntılı açıklaması.
   
   > [!NOTE]
   > Tüm dört simgesi boyutu (küçük, Orta, büyük, geniş) Market öğesi doğru şekilde oluşturmak için gereklidir.
   > 
   > 
8. İçinde **manifest.json** dosya, değişiklik **adı** Market öğesi adı. Aynı zamanda değiştirmeniz **yayımcı** adı veya şirket.
9. Altında **yapıları**, değiştirme **adı** ve **yolu** dahil Azure Resource Manager şablonu için doğru bilgi.
   
         "artifacts": [
            {
                "name": "Type your template name",
                "type": "Template",
                "path": "DeploymentTemplates\\Type your path",
                "isDefault": true
            }
10. Değiştir **My Market öğesi** Market öğesi göründüğü kategori listesi ile.
    
             "categories":[
                 "My Marketplace Items"
              ],
11. İçin diğer tüm düzenlemeleri manifest.json için başvurmak [başvuru: Market öğesi manifest.json](#reference-marketplace-item-manifestjson).
12. Bir .azpkg dosyasına klasörleri paketlemek için bir komut istemi açın ve aşağıdaki komutu çalıştırın:
    
        AzureGalleryPackager.exe package –m <path to manifest.json> -o <output location for the package>
    
    > [!NOTE]
    > Çıkış paketi tam yolu olmalıdır. Örneğin, çıkış yolu C:\MarketPlaceItem\yourpackage.azpkg ise C:\MarketPlaceItem klasörü bulunması gerekir.
    > 
    > 

## <a name="publish-a-marketplace-item"></a>Bir Market öğesi yayımlama
1. Azure Blob depolama alanına, Market öğesi (.azpkg) yükleme için PowerShell veya Azure Storage Gezgini kullanın. Yerel Azure yığın depolama alanına karşıya yükleyebilir veya Azure Storage'a yükler. (Bunu bir geçici paketi için konumdur.) Blob genel olarak erişilebilir olduğundan emin olun.
2. İstemci sanal makinesinde Microsoft Azure yığın ortamında, PowerShell oturumunuzun Hizmet Yöneticisi kimlik bilgilerinizle ayarlandığından emin olun. PowerShell Azure yığınında içinde kimlik doğrulaması hakkında yönergeler bulabilirsiniz [PowerShell ile şablon dağıtma](user/azure-stack-deploy-template-powershell.md).
3. Kullanım **Ekle AzureRMGalleryItem** Market öğesi Azure yığınına yayımlamak için PowerShell cmdlet. Örneğin:
   
       Add-AzureRMGalleryItem -GalleryItemUri `
       https://sample.blob.core.windows.net/gallerypackages/Microsoft.SimpleTemplate.1.0.0.azpkg –Verbose
   
   | Parametre | Açıklama |
   | --- | --- |
   | Subscriptionıd |Yönetici abonelik kimliği PowerShell kullanarak alabilirsiniz. Portalda almak tercih ediyorsanız, sağlayıcı abonelik gidin ve abonelik kimliğini kopyalayın |
   | GalleryItemUri |BLOB URI'si galeri paketinize depolama alanına karşıya yüklendi. |
   | Apiversion |Olarak ayarlanmış **2015-04-01**. |
4. Portalına gidin. Market öğesi--Portalı'nda artık, bir işleç veya bir kullanıcı olarak da görebilirsiniz.
   
   > [!NOTE]
   > Paket görünmesi birkaç dakika sürebilir.
   > 
   > 
5. Market öğesi artık Azure yığın Marketinde kaydedildi. Blob Depolama Birimi konumundan silmeyi seçebilirsiniz.
6. Market öğesi kullanarak kaldırabilirsiniz **Kaldır AzureRMGalleryItem** cmdlet'i. Örnek:
   
        Remove-AzureRMGalleryItem -Name Microsoft.SimpleTemplate.1.0.0  –Verbose
   
   > [!NOTE]
   > Bir öğe kaldırdıktan sonra Market UI hata gösterebilir. Hatayı düzeltmek için tıklatın **ayarları** Portalı'nda. Ardından, seçin **değişiklikleri atmak** altında **Portal özelleştirme**.
   > 
   > 

## <a name="reference-marketplace-item-manifestjson"></a>Başvuru: Market öğesi manifest.json
### <a name="identity-information"></a>Kimlik bilgileri
| Ad | Gerekli | Tür | Kısıtlamalar | Açıklama |
| --- | --- | --- | --- | --- |
| Ad |X |Dize |[A-Za-z0-9]+ | |
| Yayımcı |X |Dize |[A-Za-z0-9]+ | |
| Sürüm |X |Dize |[SemVer v2](http://semver.org/) | |

### <a name="metadata"></a>Meta Veriler
| Ad | Gerekli | Tür | Kısıtlamalar | Açıklama |
| --- | --- | --- | --- | --- |
| DisplayName |X |Dize |Öneri 80 karakter |Portal 80 karakterden uzunsa, öğe adı düzgün biçimde görüntülenmeyebilir. |
| PublisherDisplayName |X |Dize |Öneri 30 karakter |Portal 30 karakterden uzunsa, yayımcı adını düzgün biçimde görüntülenmeyebilir. |
| PublisherLegalName |X |Dize |En fazla 256 karakter | |
| Özet |X |Dize |60 ile 100 karakter | |
| LongSummary |X |Dize |140 ile 256 karakter |Henüz geçerli değil Azure yığınında. |
| Açıklama |X |[HTML](https://auxdocs.azurewebsites.net/en-us/documentation/articles/gallery-metadata#html-sanitization) |500 ile 5.000 karakterleri | |

### <a name="images"></a>Görüntüler
Market aşağıdaki simgeler kullanır:

| Ad | Genişlik | Yükseklik | Notlar |
| --- | --- | --- | --- |
| Geniş |255 px |115 piksel |Her zaman gereklidir |
| Büyük |115 piksel |115 piksel |Her zaman gereklidir |
| Orta |90 piksel |90 piksel |Her zaman gereklidir |
| Küçük |40 piksel |40 piksel |Her zaman gereklidir |
| Ekran görüntüsü |533 piksel |32 piksel |İsteğe bağlı |

### <a name="categories"></a>Kategoriler
Her Market öğesi, öğenin UI portalında göründüğü tanımlayan bir kategorisiyle etiketlenmiş. Varolan kategorilerden birini Azure yığınında seçebilirsiniz (işlem, veri + depolama, vb.) veya yeni bir tane seçin.

### <a name="links"></a>Bağlantılar
Her bir Market öğe çeşitli ek içeriklere bağlantılar içerebilir. Bağlantıları adları ve URI'ler oluşan bir liste olarak belirtilir.

| Ad | Gerekli | Tür | Kısıtlamalar | Açıklama |
| --- | --- | --- | --- | --- |
| DisplayName |X |Dize |En fazla 64 karakter | |
| Uri |X |URI | | |

### <a name="additional-properties"></a>Ek Özellikler
Önceki meta verileri ek olarak aşağıdaki biçimde özel anahtar/değer çifti veri Market yazarlar sağlayabilirsiniz:

| Ad | Gerekli | Tür | Kısıtlamalar | Açıklama |
| --- | --- | --- | --- | --- |
| DisplayName |X |Dize |En çok 25 karakter | |
| Değer |X |Dize |En çok 30 karakter | |

### <a name="html-sanitization"></a>HTML temizleme
HTML sağlayan herhangi bir alan için aşağıdaki öğeleri ve öznitelikleri izin verilir:

H1, h2, h3, h4, h5, p, ol, ul, li, bir [hedef | href], br, güçlü, em, b, t

## <a name="reference-marketplace-item-ui"></a>Başvuru: Market öğesi kullanıcı Arabirimi
Simgeler ve Azure yığın Portalı'nda görülen Market öğesi için metin aşağıdaki gibidir.

### <a name="create-blade"></a>Dikey pencere oluşturma
![Dikey pencere oluşturma](media/azure-stack-marketplace-item-ui-reference/image1.png)

### <a name="marketplace-item-details-blade"></a>Market öğesi Ayrıntılar dikey penceresi
![Market öğesi Ayrıntılar dikey penceresi](media/azure-stack-marketplace-item-ui-reference/image3.png)

