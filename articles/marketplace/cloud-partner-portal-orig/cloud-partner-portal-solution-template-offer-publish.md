---
title: Bir Azure çözümü şablonu | Microsoft Docs
description: Bir çözüm şablonu, Azure Market'te yayımlayın.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: dan-wesley
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 11/15/2018
ms.author: pbutlerm
ms.openlocfilehash: 333eebfa1bae919c43164572c63f2de4f7251fe0
ms.sourcegitcommit: fa758779501c8a11d98f8cacb15a3cc76e9d38ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2018
ms.locfileid: "52261626"
---
# <a name="publish-a-solution-template-to-azure-marketplace"></a>Bir çözüm şablonu, Azure Market'te yayımlama

Bu makalede, bir çözüm şablonu yayımlama adımları Azure Marketi'nde teklif sağlar.

## <a name="prerequisites"></a>Önkoşullar

Aşağıdaki teknik ve teknik gereksinimleri, bir çözüm şablonu Azure Market'te listelemek için geçerlidir.

### <a name="technical"></a>Teknik

- [Azure Resource Manager şablonları anlama](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authoring-templates).
- Azure hızlı başlangıç şablonları:
    - [Azure Hızlı Başlangıç şablonu belgeleri](https://azure.microsoft.com/documentation/templates/)
    - [Github'da Azure hızlı başlangıç belgeleri](https://github.com/azure/azure-quickstart-templates)
 - [Azure portal kullanıcı arabirimi dosyası oluşturma](https://docs.microsoft.com/azure/azure-resource-manager/managed-application-createuidefinition-overview)
 - Etkinleştirme [müşteri kullanım attribution](./../azure-partner-customer-usage-attribution.md) Azure'da Azure müşteri dağıtımları yazılım kullanımını izlemenize yardımcı olması için.

### <a name="non-technical-business-requirements"></a>Teknik olmayan (iş gereksinimlerini)

- Azure Marketi tarafından desteklenen ülkeden bir satış şirketinizin (veya yan kuruluşunun) bulunmalıdır.
- Ürününüzün Azure Marketi tarafından desteklenen faturalandırma modelleri ile uyumlu bir şekilde lisanslanmalıdır.
- Teknik Destek müşterilere ticari açıdan makul bir şekilde sağlama yoksa, ücretsiz, ücretli veya topluluk desteği aracılığıyla sorumlu.
- Yazılımınızı ve üçüncü taraf yazılım bağımlılıkları lisansı sağlamaktan sorumlu.
- Teklifinizin Azure Market'te ve Azure Portalı'nda listelenmesi ölçütlerini karşılayan içerik sağlar.
- Azure Marketi katılım ilkeleri ve yayımcı Sözleşmesi koşullarını kabul etmiş olursunuz.
- Kullanım Koşulları, Microsoft Gizlilik Bildirimi ve Microsoft Azure Sertifikalı Program Sözleşmesi’ne uymayı kabul edin.

## <a name="before-you-begin"></a>Başlamadan önce

Tüm önkoşulların yerine getirdikten sonra çözüm şablonu teklifinizi yazma başlayabilirsiniz. Başlamadan önce aşağıdaki teklif ve SKU bilgileri gözden geçirin.

**Teklif**

Bir Azure uygulaması teklif sunan bir yayımcıdan ürün sınıfının karşılık gelir. Azure Marketi'nde kullanılabilir olmasını istediğiniz çözüm/uygulama için yeni bir tür varsa, yeni bir teklif için en iyi yaklaşımdır. Teklif bir SKU koleksiyonudur. Her teklif, Azure Market'te kendi varlık olarak görünür.

**SKU**

SKU, bir teklife ilişkin en küçük satın alınabilir birimdir. Aynı ürün sınıf içinde sırada (teklif), SKU'ları farklı desteklenen özellikler arasında ayırt etmenize olanak sağlar. Örneğin, teklif yönetilen veya yönetilmeyen ve farklı faturalama modellerini desteklenir.

Birden çok SKU'ları, aşağıdaki senaryolarda ekleyin:
- Kendi lisansını getir (KLG) ya da Kullandıkça Öde (PAYG) gibi farklı faturalama modellerini desteklemek istediğiniz.
- Her SKU farklı özellik kümesini destekler ve her bir özellik kümesi hesabın fiyatı farklıdır.

Bir SKU üst teklifi Azure Marketi bölümünde gösterilir ve Azure Portalı'nda satın alınabilir kendi varlık olarak gösterilir.

## <a name="to-create-a-new-offer"></a>Yeni bir teklif oluşturmak için

1.  Oturum [bulut iş ortağı portalı](http://cloudpartner.azure.com/).

2.  Sol gezinti çubuğunda **+ yeni teklif**ve ardından **Azure uygulamalarını**.

    ![Yeni bir teklif oluşturun](./media/cloud-partner-portal-publish-managed-app/newOffer.png)

3.  Altında **yeni teklif**seçin **Düzenleyicisi**.

    ![Yeni Teklif Düzenleyicisi](./media/cloud-partner-portal-publish-managed-app/newOffer_OfferSettings.png)

4.  Altında **Düzenleyicisi**, aşağıdaki görünümleri bilgileri sağlarız:
    - Teklif ayarları
    - SKU'lar
    - Market
    - Destek

Her görünüm alanlarını doldurmak size bir dizi içeriyor. Gerekli alanları bir kırmızı yıldız gösterilir (\*)

## <a name="to-configure-offer-settings"></a>Teklif ayarları yapılandırmak için

1. Aşağıdaki yapılandırma **Teklif kimliği** alanları ayarları sunar.

    **Teklif kimliği**

     Bir yayımcı profilinde teklif için benzersiz bir tanımlayıcı. Bu kimliği ürün URL'leri, Azure Resource Manager şablonları, görünür ve faturalandırma bildirir. Yalnızca küçük harf alfasayısal karakterler veya tire (-) kullanabilirsiniz. Kimliği tire bitemez ve en çok 50 karakter olabilir. 
    >[!Note]
    >Bir teklif etkin durumda olduğunda bu alan kilitli.

    **Yayımcı kimliği**

    Yayımcı profil için bir açılır liste. Bu teklif altında yayımlamak istediğiniz profili seçin. 
    >[!Note]
    >Bir teklif etkin durumda olduğunda bu alan kilitli.

    **Ad**

    Teklifiniz için görünen adı. Bu ad, Azure Marketi'nde hem de Azure portalında gösterilir. En fazla 50 karakter olabilir. Teklif adı için aşağıdaki yönergeleri kullanın:
    -  Ürününüz için tanınabilir bir marka adı ekleyin. 
    - Nasıl teklif pazarlanmadığından olmadığı sürece, şirketinizin adını buraya dahil değildir.
    - Kendi Web sitesi bu teklif pazarlama, kendi Web sitenizin adıyla aynı adı olduğundan emin olun.

2. Seçin **Kaydet** teklif ayarları tamamlanması.
teklifiniz için.

## <a name="to-create-skus"></a>SKU'ları oluşturmak için
------------------

1. Seçin **SKU'ları**. 

    ![Yeni SKU](./media/cloud-partner-portal-publish-managed-app/newOffer_skus.png)

    SKU kimliği, teklif içinde SKU için benzersiz bir tanımlayıcıdır. Bu kimlik; ürün URL’leri, Kaynak Yöneticisi şablonları ve faturalandırma raporlarında görünürdür. SKU kimliği:
    - Yalnızca en çok 50 karakter olabilir.
    - Yalnızca küçük harf alfasayısal karakterler veya tire (-) oluşturulabilir.
    - Kimlik tire ile bitemez.

    >[!Note]
    >Bir SKU eklendikten sonra SKU'ları görünümünde SKU'lar listesinde görünür. SKU ayrıntıları görmek için SKU Adı'nı seçin. 

2. Seçin **yeni SKU** aşağıdaki ekran görüntüsünde gösterilen bilgiler sağlamak için. 

    ![SKU ayrıntıları](./media/cloud-partner-portal-publish-managed-app/newOffer_newsku.png)


### <a name="sku-settings"></a>SKU ayarları

Aşağıdaki SKU ayarları sağlayın.

- **Başlık** -SKU için bir başlık. Bu konu başlığı, bu öğe için galerisinde görüntülenir.
- **Özet** - bir Özet açıklaması sku'sunun kısa. (En fazla 100 karakterdir.)
- **Açıklama** - ayrıntılı SKU açıklaması.
- **SKU türü** -bu değerleri içeren bir açılan listedeki: "Yönetilen uygulama (Önizleme)" ve "Çözüm şablonu". Bu senaryo için seçin **çözüm şablonu**.
- **Bulut kullanılabilirlik** -SKU konumu. Varsayılan değer **genel Azure**.

### <a name="package-details"></a>Paket Ayrıntıları

SKU ayarları tamamladıktan sonra aşağıdaki Paket ayrıntılarını sağlayın.

![Paket Ayrıntıları](./media/cloud-partner-portal-publish-managed-app/newOffer_newsku_ST_package.png)

- **Geçerli sürümü** -karşıya yükleyeceğiniz paketin sürümü. Sürüm etiketleri X.Y.Z, burada X, Y ve Z tamsayılardır biçiminde olmalıdır.
- **Dosya paketini** -bu paket bir .zip dosyasına kaydedilir aşağıdaki dosyaları içerir.
    -   MainTemplate.json - çözüm/uygulamayı dağıtmak ve çözüm için tanımlanan kaynakları oluşturmak için kullanılan dağıtım şablonu dosyası. Daha fazla bilgi için [nasıl dağıtım şablonu dosyaları yazma](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-create-first-template)
    -   createUIDefinition.json - bu dosya, bu çözüm/uygulama sağlamak için kullanıcı arabirimi oluşturmak için Azure portal tarafından kullanılır. Daha fazla bilgi için [yönetilen uygulamanız için oluşturma Azure portal kullanıcı arabirimi](https://docs.microsoft.com/azure/azure-resource-manager/managed-application-createuidefinition-overview)

    >[!IMPORTANT]
    >Bu paket, herhangi bir iç içe geçmiş şablonlar veya bu uygulama sağlamak için gereken komut dosyaları içermelidir. MainTemplate.json ve createUIDefinition.json kök klasöründe olması gerekir.

## <a name="to-configure-the-marketplace"></a>Market yapılandırmak için

Teklif için görüntülenen alanları yapılandırmak için Market görünümünü kullanın [Azure Marketi](https://azuremarketplace.microsoft.com) ve [Azure portalında](https://portal.azure.com/).

### <a name="preview-subscription-ids"></a>Önizleme abonelik kimlikleri

Teklif yayımlandığında teklif erişmesini istediğiniz Azure abonelik kimlikleri listesi. Bu teknik listelenen abonelikler test bunu yapmadan önce önizlenen teklifini izin Canlı. İş ortağı portalı beyaz liste 100 abonelikleri kadar olanak tanır.

### <a name="suggested-categories"></a>Önerilen kategorileri

En fazla beş kategoriye teklifinizi en iyi ile ilişkilendirilebilen sağlanan listeden seçin. Seçili kategorilerdeki teklifinizi, bulunan ürün kategorilerini eşlemek için kullanılan [Azure Marketi](https://azuremarketplace.microsoft.com) ve [Azure portalı](https://portal.azure.com/).

Aşağıdaki örnekler, Azure Marketi'nde hem de Azure portalında Market bilgileri gösterir.

**Azure Market**

![publishvm10](./media/cloud-partner-portal-publish-managed-app/publishvm10.png)


![publishvm11](./media/cloud-partner-portal-publish-managed-app/publishvm11.png)


![publishvm15](./media/cloud-partner-portal-publish-managed-app/publishvm15.png)


**Azure portal**


![publishvm12](./media/cloud-partner-portal-publish-managed-app/publishvm12.png)


![publishvm13](./media/cloud-partner-portal-publish-managed-app/publishvm13.png)


### <a name="logo-guidelines"></a>Logo yönergeleri

Bulut iş ortağı portalına karşıya logo için aşağıdaki yönergeleri izleyin:

-   Azure tasarımının basit bir renk paleti vardır. Logonuzu düşük tutmak için ikincil renk ve birincil sayısı.

-   Azure portal'ın Tema renkleri beyaz ve siyah. Bu renkler, logolar arka plan rengi kullanmaktan kaçının. Logo, Azure portalında belirgin hale getirir bir renk kullanın. Basit birincil renkleri öneririz.

    >[!Note] 
    >Saydam arka plan kullanıyorsanız, ardından logoları/metin beyaz, olmayan emin olun, siyah veya mavi.

-   Logoda gradyan arka plan kullanmayın.

-   Metin logosunu yerleştirmekten kaçının. Bu kılavuz, şirketiniz veya marka adı içerir. Logonuzu Görünüm ve yapısını olmalıdır *düz* gradyanlar kaçınmalısınız.

-   Logo esnetilmiş olmamalıdır.

#### <a name="hero-logo"></a>Hero logosu

Hero logosu isteğe bağlıdır. Yayımcı Hero logoyu karşıya yükleyin değil seçebilirsiniz. Ancak, logoyu karşıya yüklendikten sonra silinemez. İş ortağı Hero simgeler için Azure Marketi yönergelere uyması gerekir.

#### <a name="guidelines-for-the-hero-logo-icon"></a>Hero logosu simgesi için yönergeler

-   Beyaz renkte bir yazı tipi kullanarak yayımcı görünen adı, planı başlık ve teklife ilişkin uzun özeti görüntülenir. Açık bir renk arka planda kullanmaktan kaçının. Siyah, beyaz ve saydam arka planlar için Hero simgeler izin verilmez.

-   Yayımcı görünen adı, plan teklife ilişkin listede, başlık, uzun Özet teklif ve Oluştur düğmesine program aracılığıyla içinde Hero logosu katıştırılır. Hero logosu tasarlarken, herhangi bir metin girmeyin. Logo sağ tarafındaki bir boşluk bırakın. Bu alanı 415 x 100 piksel olmalı ve 370 tarafından uzaklık piksel soldan.

![publishvm14](./media/cloud-partner-portal-publish-managed-app/publishvm14.png)

## <a name="to-configure-support"></a>Desteğini yapılandırmak için

Aşağıdaki bilgileri sağlamak için destek görünümünü kullanın:

- Mühendislik gibi şirketiniz kişilerden destekler.
- Müşteri desteği kişiler.

## <a name="to-publish-the-offer"></a>Teklif yayımlamak için

Son adım, teklif yayımlamaktır. **Yayımla**’yı seçin.
