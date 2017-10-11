---
title: "Azure Market uygulamalarda yönetilen | Microsoft Docs"
description: "Azure açıklar yönetilen Market üzerinden kullanılabilir uygulamalar."
services: azure-resource-manager
author: ravbhatnagar
manager: rjmax
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 07/09/2017
ms.author: gauravbh; tomfitz
ms.openlocfilehash: 58ac7665abf7e75a43bb0b92bdf6f41005c3efe8
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2017
---
# <a name="azure-managed-applications-in-the-marketplace"></a>Market'te Azure yönetilen uygulamalar

 MSP'ler, ISV ve sistem tümleştiricileri (SIS) Azure kullanabileceğiniz yönetilen tüm Azure Marketi müşterilere çözümleri sunmak için uygulamalar. Bu tür çözümler, Bakım ve müşteriler için ek yükü bakım azaltın. Yayımcılar, altyapı ve yazılım Market üzerinden satmak. Yönetilen uygulamaların bunlar Hizmetleri ve işletimsel destek ekleyebilirsiniz. Daha fazla bilgi için bkz: [yönetilen uygulama genel bakış](managed-application-overview.md).

Bu makalede, nasıl bir MSP, ISV veya SI Marketi'nde uygulama yayımlama ve müşterilere geniş çapta kullanılabilir hale açıklanmaktadır.

## <a name="prerequisites-for-publishing-a-managed-application"></a>Yönetilen bir uygulamayı yayımlamak için Önkoşullar

Market listesindeki için Önkoşullar:

* Teknik

    *  Temel yapısını ve Azure Resource Manager şablonları sözdizimi hakkında daha fazla bilgi için bkz: [Azure Resource Manager şablonları](resource-group-authoring-templates.md).
    *  Tam veri şablonunun çözümleri görüntülemek için bkz: [Azure hızlı başlangıç şablonlarını](https://azure.microsoft.com/en-us/documentation/templates/) veya [Hızlı Başlangıç şablonu deposu](https://github.com/azure/azure-quickstart-templates).
    *  Market üzerinden uygulamanızı dağıtmak müşteriler arabirimi oluşturma hakkında daha fazla bilgi için bkz: [bir kullanıcı arabirimi tanımı dosyası oluşturma](managed-application-createuidefinition-overview.md).

* Yedeğin (iş gereksinimlerini)

    *   Şirketiniz veya onun yan burada satış Marketi tarafından desteklenen bir ülkede bulunmalıdır.
    *   Ürünü Marketi tarafından desteklenen faturalama modelleri ile uyumlu şekilde lisansına sahip olması gerekir.
    *   Teknik Destek kullanılabilir müşterilere bir ticari koşulların elverdiği oranda makul şekilde yapmaktan sorumlu. Destek Ücretli, boş veya topluluk desteklemez.
    *   Yazılım ve üçüncü taraf yazılım bağımlılıkları lisansı sağlamaktan sorumlu.
    *   Teklifinizle Market ve Azure portalını listelenmiş ölçütlerini karşılayan içerik sağlamanız gerekir.
    *   Azure Market katılım ilkeleri ve yayımcı Sözleşmesi koşullarını kabul etmeniz gerekir.
    *   Kullanım koşulları, Microsoft gizlilik bildirimi ve Microsoft Azure sertifikalı Program sözleşmesi uymak kabul etmeniz gerekir.

## <a name="create-a-new-azure-application-offer"></a>Yeni bir Azure uygulama teklifi oluşturma

Önkoşulları karşılaması sonra yönetilen uygulamayı teklifiniz oluşturmaya hazırsınız. Bir teklif ve bir SKU hızlı bir genel bakış atalım.

### <a name="offer"></a>Sunduğu

Teklif yönetilen bir uygulama için bir sınıf bir yayımcıdan sunumu ürünün karşılık gelir. Market kullanılabilir hale getirmek istediğiniz çözüm/uygulama yeni bir tür varsa, bunu yeni bir teklif ayarlayabilirsiniz. Bir teklif, SKU'ları koleksiyonudur. Her teklif Market'te kendi varlık olarak görünür.

### <a name="sku"></a>SKU

Bir SKU en küçük purchasable bir teklif birimidir. Aynı ürün sınıfı (teklif) içinde bir SKU arasında ayırt etmek için kullanabilirsiniz:

* Desteklenen farklı özellikleri.
* Teklif yönetilen yönetilmeyen mı.
* Desteklenen faturalama modelleri.

Bir SKU Market'te üst teklif altında görüntülenir. Azure portalında purchasable kendi varlık olarak görünür.

### <a name="set-up-an-offer"></a>Bir teklif ayarlayın

1. Oturum [bulut iş ortağı portalına](https://cloudpartner.azure.com/).

2. Sol gezinti bölmesinde seçin **+ yeni teklif** > **Azure uygulamaları**.

    ![Yeni teklif](./media/managed-application-author-marketplace/newOffer.png)

3. Sol tarafta görünür form doldurmak **Düzenleyicisi** görünümü. Gerekli alanları ile kırmızı yıldız işareti (*) işaretlenir.

    ![Teklif ayarları](./media/managed-application-author-marketplace/newOffer_OfferSettings.png)

    Dört ana formlar, yönetilen bir uygulama oluşturmak için kullanılır:

    a. Teklif ayarları

    b. SKU'ları

    c. Market

    d. Destek

Bu form, aşağıdaki bölümlerde daha ayrıntılı açıklanmıştır.

## <a name="offer-settings-form"></a>Teklif ayarları formu
Teklif ayarlarını belirtmek için temel bu formu kullanın.

1. Doldurmak **teklif ayarları** formu. Farklı alanları şunlardır:

    a. **Teklif kodu**: Yayımcı profilindeki teklif bu benzersiz tanımlayıcı tanımlar. Bu kimliği ürün URL'ler, Resource Manager şablonları görünür ve faturalama raporlar. Yalnızca küçük harf alfasayısal karakterler veya tire (-) birleştirilebilir. Kimliği, bir tire bitemez. Buna ait sınırlı en çok 50 karakter. Bu alan, bir teklif Canlı göründükten sonra kilitlendi.

    b. **Yayımcı kimliği**: Bu teklif altında yayımlamak istediğiniz yayımcı profilini seçmek için bu açılan listeyi kullanın. Bu alan, bir teklif Canlı göründükten sonra kilitlendi.

    c. **Ad**: teklifiniz için bu görünen ad Market ve Portalı'nda görünür. En çok 50 karakter olabilir. Ürününüzün tanınabilir bir marka adını ekleyin. Nasıl pazarlama olmadığı sürece, şirketinizin adını buraya dahil etmeyin. Kendi Web sitesinde bu teklif pazarlama, adı tam olarak, Web sitenizde şu şekilde görünür durumda olduğundan emin olun.

2. Seçin **kaydetmek** ilerleme durumunuzu kaydetmek için. 

## <a name="skus-form"></a>SKU'ları formu
Sonraki adım, teklifiniz için SKU'ları eklemektir.

1. Seçin **SKU'ları** > **yeni SKU**. 

    ![Yeni SKU seçin](./media/managed-application-author-marketplace/newOffer_skus.png)

2. Girin bir **SKU kimliği**. Bir teklif içinde SKU için benzersiz bir tanımlayıcı SKU kimliğidir. Bu kimliği ürün URL'ler, Resource Manager şablonları görünür ve faturalama raporlar. Yalnızca küçük harf alfasayısal karakterler veya tire (-) birleştirilebilir. Kimliği, tire ve buna ait en çok 50 karakter sınırlı bitemez. Bu alan, bir teklif Canlı göründükten sonra kilitlendi. Bir teklif içinde birden çok SKU olabilir. Bir SKU yayımlamayı düşündüğünüz her görüntü için gerekir.

3. Doldurmak **SKU ayrıntıları** aşağıdaki formda bölümü:

    ![Yeni SKU sağlayın](./media/managed-application-author-marketplace/newOffer_newsku.png)

    Aşağıdaki alanları doldurun:
    
    a. **Başlık**: Bu SKU için bir başlık girin. Bu öğe için galerisinde bu başlığı görüntülenir.

    b. **Özet**: kısa bir özeti için bu SKU girin. Bu metin başlığı altında görüntülenir.

    c. **Açıklama**: SKU hakkında ayrıntılı bir açıklama girin.

    d. **SKU tür**: izin verilen değerler: **yönetilen uygulamayı** ve **çözüm şablonları**. Bu durumda, seçin **yönetilen uygulamayı**.

4. Doldurmak **Paket ayrıntılarını** aşağıdaki formda bölümü:

    ![Paket](./media/managed-application-author-marketplace/newOffer_newsku_package.png)

    Aşağıdaki alanları doldurun:

    a. **Geçerli sürüm**: karşıya yüklediğiniz paket için bir sürümü girin. Şu biçimde olmalıdır `{number}.{number}.{number}{number}`.

    b. **Bir paket dosyası seçmek**: Bu paket bir .zip dosyasına sıkıştırılmış aşağıdaki dosyaları içerir:
    * **applianceMainTemplate.json**: Çözüm/uygulama dağıtmak için kullanılan dağıtım şablon dosyası. Dağıtım şablonu dosyaları oluşturma hakkında daha fazla bilgi için bkz: [, ilk Azure Resource Manager şablonu oluşturma](resource-manager-create-first-template.md).
    * **appliancecreateUIDefinition.json**: Bu dosya bu çözümü/uygulama sağlamak için kullanılan kullanıcı arabirimi oluşturmak için Azure portal tarafından kullanılır. Daha fazla bilgi için bkz: [CreateUiDefinition ile çalışmaya başlama](managed-application-createuidefinition-overview.md).
    * **mainTemplate.json**: Bu şablon dosyası, yalnızca Microsoft.Solution/appliances kaynak içeriyor. MainTemplate dosyası, aşağıdaki özellikleri içerir:

        *  **tür**: kullanım **Market** Market'te yönetilen uygulamalar için.
        *  **ManagedResourceGroupId**: applianceMainTemplate.json içinde tanımlanan tüm kaynaklara dağıtıldığı müşterinin aboneliğini bu kaynak grubunda bulunuyor.
        *  **PublisherPackageId**: Bu dize paketi benzersiz olarak tanımlar. Değer biçiminde sağlayın `{publisherId}.{OfferId}.{SKUID}.{PackageVersion}`.

Elde **Teklif kimliği** ve **yayımcı kimliği** Yayımlama Portalı, aşağıdaki resimde gösterildiği gibi:

![Teklif kimliği](./media/managed-application-author-marketplace/UniqueString_pubid_offerid.png)
        
Elde **SKU kimliği**aşağıdaki görüntüde gösterildiği gibi:

![SKU KİMLİĞİ](./media/managed-application-author-marketplace/UniqueString_skuid.png)
        
Paket elde **sürüm**aşağıdaki görüntüde gösterildiği gibi:

![Paket sürümü](./media/managed-application-author-marketplace/UniqueString_packageversion.png)
    
  Değerini önceki örneklerde dayalı **PublisherPackageId** olan `azureappliance-test.ravmanagedapptest.ravpreviewmanagedsku.1.0.0`.

  Örnek mainTemplate.json:

  ```json
  {
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
      "storageAccountNamePrefix": {
        "type": "string",
        "metadata": {
          "description": "Specify the name of the storage account"
        }
      },
      "storageAccountType": {
        "type": "string"
      }
    },
    "variables": {
      "managedResourceGroup": "[concat(resourceGroup().id,uniquestring(resourceGroup().id))]"
    },
    "resources": [{
      "type": "Microsoft.Solutions/appliances",
      "apiVersion": "2016-09-01-preview",
      "name": "[concat(parameters('storageAccountNamePrefix'), '-', 'managed')]",
      "location": "[resourceGroup().location]",
      "kind": "marketplace",
      "properties": {
        "managedResourceGroupId": "[variables('managedResourceGroup')]",
        "PublisherPackageId":"azureappliancetest.ravmanagedapptest.ravpreviewmanagedsku.1.0.0",
        "parameters": {
          "storageAccountName": {
            "value": "[parameters('storageAccountNamePrefix')]"
          },
          "storageAccountType": {
            "value": "[parameters('storageAccountType')]"
          }
        }
      }
    }],
    "outputs": {

    }
  }
  ```

Bu paket, herhangi bir iç içe geçmiş şablonları veya bu uygulama başarıyla hazırlamak için gereken komut dosyaları içermelidir. MainTemplate.json applianceMainTemplate.json applianceCreateUIDefinition.json dosyaları ve kök klasörde mevcut olması gerekir.

* **Yetkilerini**: Bu özellik müşterilerin Aboneliklerde kimlerin erişimi ve kaynaklara erişim düzeyini alır tanımlar. Yayımcı, uygulama müşteri adına yönetmek için kullanabilirsiniz.
* **Principalıd**: Bu özellik bir kullanıcının, kullanıcı grubu veya Müşteri'nin aboneliğindeki kaynaklar belirli izinleri verildi uygulama Azure Active Directory (Azure AD) tanımlayıcısıdır. Rol tanımı izinleri açıklar. 
* **Rol tanımı**: Bu özellik Azure AD tarafından sağlanan tüm yerleşik rol tabanlı erişim denetimi (RBAC) rollerini listesidir. Kaynakları müşteri adına yönetmek üzere kullanmak en uygun olan rolü seçebilirsiniz.

Birden çok yetkilerini ekleyebilirsiniz. Bir AD kullanıcı grubu oluşturun ve kendi Kimliğini belirtin öneririz **Principalıd**. Bu şekilde, kullanıcı grubu SKU güncelleştirmeye gerek olmadan daha fazla kullanıcı ekleyebilirsiniz.

RBAC hakkında daha fazla bilgi için bkz: [Azure portalında RBAC ile çalışmaya başlama](../active-directory/role-based-access-control-what-is.md).

## <a name="marketplace-form"></a>Market formu

Market form üzerinde göster alanların ister [Azure Marketi](https://azuremarketplace.microsoft.com) ve [Azure portal](https://portal.azure.com/).

### <a name="preview-subscription-ids"></a>Önizleme abonelik kimlikleri

Azure aboneliği yayımlandıktan sonra teklif erişebileceği kimlikleri listesini girin. Bunu yapmadan önce önizleme uygulanan teklif test etmek için bu beyaz listelenen abonelikleri kullanabileceğiniz Canlı. İş ortağı portalında en fazla 100 abonelikleri beyaz listesi derleyebilirsiniz.

### <a name="suggested-categories"></a>Önerilen kategorileri

Teklifiniz en iyi ile ilişkilendirilebilir listesinden en fazla beş kategorilerini seçin. Bu kategoriler kullanılabilir olan ürün kategorilerini teklifiniz eşlemek için kullanılır [Azure Marketi](https://azuremarketplace.microsoft.com) ve [Azure portal](https://portal.azure.com/).

#### <a name="azure-marketplace"></a>Azure Market

Aşağıdaki alanları, yönetilen uygulamanızın özetini görüntüler:

![Market özeti](./media/managed-application-author-marketplace/publishvm10.png)

**Genel bakış** aşağıdaki alanları, yönetilen uygulamanızın görüntüler sekmesi:

![Market’e genel bakış](./media/managed-application-author-marketplace/publishvm11.png)

**Planları + fiyatlandırma** aşağıdaki alanları, yönetilen uygulamanızın görüntüler sekmesi:

![Market planları](./media/managed-application-author-marketplace/publishvm15.png)

#### <a name="azure-portal"></a>Azure portalına

Aşağıdaki alanları, yönetilen uygulamanızın özetini görüntüler:

![Portal özeti](./media/managed-application-author-marketplace/publishvm12.png)

Genel bakış, yönetilen uygulamanızın için aşağıdaki alanları görüntüler:

![Portal genel bakış](./media/managed-application-author-marketplace/publishvm13.png)

#### <a name="logo-guidelines"></a>Logo yönergeleri

Bulut iş ortağı Portalı'nda karşıya logo aşağıdaki yönergeleri izleyin:

*   Azure tasarım basit renk paletini sahiptir. Logonuzun ikincil renkleri ve birincil sayısını sınırlayın.
*   Tema renkleri portalı beyaz ve siyah. Bu renkler arka plan rengi olarak logonuzun için kullanmayın. Logonuzun portalında belirgin hale getirir bir renk kullanın. Basit birincil renkleri öneririz. *Saydam arka plan kullanırsanız, logo ve metin beyaz, olduğundan emin olun siyah veya mavi.*
*   Gradyan arka planı logosunu kullanmayın.
*   Metin, logo, bile, şirket veya marka adı yerleştirmeyin. Logonuzun Görünüm ve yapısını düz ve gradyan olmaması gerekir.
*   Logo uzatılmış olmadığından emin olun.

#### <a name="hero-logo"></a>Kahramanı logosu

Kahramanı logosu isteğe bağlıdır. Yayımcı kahramanı logosu yüklemek değil seçebilirsiniz. Kahramanı simgesi yüklendikten sonra silinemez. O anda iş ortağı kahramanı simgeler Market yönergeleri izlemeniz gerekir.

Kahramanı logo simgesini için aşağıdaki yönergeleri izleyin:

*   Yayımcı görünen adı, planı başlık ve uzun Özet teklif beyaz görüntülenir. Bu nedenle, açık bir renk kahramanı simgesi arka plan için kullanmayın. Siyah, beyaz veya saydam arka plan kahramanı simgelerini izin verilmiyor.
*   Teklif listelenen sonra yayımcı görüntüleme adı, planı başlık, uzun Özet teklif ve **oluşturma** düğmesi program aracılığıyla kahramanı logosunun içinde katıştırılmış. Sonuç olarak, kahramanı logosu tasarlarken herhangi bir metin girmeyin. Metni program aracılığıyla bu alana dahil olduğundan boş alanı sağ tarafta bırakın. Metin için boş alan sağdaki 415 x 100 piksel olmalıdır. Soldan 370 piksel uzakta bulunur.

    ![Kahramanı logosu örneği](./media/managed-application-author-marketplace/publishvm14.png)

## <a name="support-form"></a>Form desteği

Doldurmak **Destek** kişiler şirketinizden desteğiyle formu. Bu bilgileri, kişiler ve müşteri destek ilgili kişisi mühendislik.

## <a name="publish-an-offer"></a>Bir teklifi yayımlama

Tüm bölümleri doldurduktan sonra seçin **Yayımla** teklifiniz müşteriler için kullanılabilir hale getirir işlemini başlatmak üzere.

## <a name="next-steps"></a>Sonraki adımlar

* Yönetilen uygulamaların giriş için bkz: [yönetilen uygulama genel bakış](managed-application-overview.md).
* Marketten bir yönetilen uygulamayı kullanma hakkında daha fazla bilgi için bkz: [tüketen Azure Market uygulamalarda yönetilen](managed-application-consume-marketplace.md).
* Hizmet Kataloğu yönetilen uygulama yayımlama hakkında daha fazla bilgi için bkz: [oluşturma ve bir hizmet Kataloğu yönetilen uygulamayı yayımlayın](managed-application-publishing.md).
* Hizmet Kataloğu yönetilen uygulama kullanma hakkında daha fazla bilgi için bkz: [bir hizmet Kataloğu yönetilen uygulama tüketen](managed-application-consumption.md).
