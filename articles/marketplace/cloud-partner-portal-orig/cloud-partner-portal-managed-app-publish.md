---
title: Azure Marketi'nde Azure bir yönetilen uygulama yayımlama
description: Azure Marketi'nde Azure bir yönetilen uygulama yayımlama
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: qianw211
manager: pbutlerm
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 09/13/2018
ms.author: pbutlerm
ms.openlocfilehash: bc044c8b59c939163336ecab01546fc26a7a2643
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2018
ms.locfileid: "48811616"
---
<a name="publish-an-azure-managed-application-to-azure-marketplace"></a>Azure Market'te Azure tarafından yönetilen uygulama yayımlama 
========================================================

Bu makalede, bir yönetilen uygulamayı teklifi Azure Marketinde yayımlama dahil çeşitli adımları listelenir.

<a name="pre-requisites-for-publishing-a-managed-application"></a>Yönetilen bir uygulama yayımlamak için ön koşullar 
---------------------------------------------------

Azure Marketi'nde listeleme için Önkoşullar

1.  Teknik

    -   [Azure Resource Manager şablonları yazma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authoring-templates)

    -   Azure hızlı başlangıç şablonları:

        -   <https://azure.microsoft.com/documentation/templates/>

        -   <https://github.com/azure/azure-quickstart-templates>

    -   UI tanımı oluşturma

        -   [Kullanıcı arabirimi tanım dosyası oluşturma](https://docs.microsoft.com/azure/azure-resource-manager/managed-application-createuidefinition-overview)

2.  Teknik olmayan (iş gereksinimlerini)

    -   Şirketinizin (veya yan kuruluşunun), Azure Marketi tarafından desteklenen bir satış kaynak ülkede bulunmalıdır

    -   Ürününüzü Azure Marketi tarafından desteklenen faturalandırma modelleri ile uyumlu bir şekilde lisanslanması gerekir

    -   Müşterileriniz için teknik destek sağlamaktan sorumludur: ücretsiz, ücretli veya topluluk desteği aracılığıyla.

    -   Yazılım ve tüm bağımlılıkları üçüncü taraf yazılım lisansı sağlamaktan sorumlu olursunuz.

    -   Teklifinizin Azure Market'te ve Azure Yönetim Portalı'nda listelenmesi ölçütlerini karşılayan içeriği sağlamanız gerekir

    -   Azure Marketi katılım ilkeleri yayımcı sözleşmesi ve koşulları kabul etmelisiniz

    -   Kullanım koşulları, Microsoft gizlilik bildirimi ve Microsoft Azure sertifikası Program Sözleşmesi ile uyumlu kabul etmesi gerekir.

<a name="how-to-create-a-new-azure-application-offer"></a>Yeni bir Azure uygulaması teklif oluşturma 
-------------------------------------------

Tüm önkoşulların karşılandığını sonra yönetilen uygulamayı teklifinizi yazma başlamaya hazır olursunuz. Biz başlamadan önce bir teklif ve SKU hızlı bir genel bakış

**Teklif**

Bir Azure uygulaması teklif sunan bir yayımcıdan ürün sınıfının karşılık gelir. Yeni bir çözüm/Azure Market'te yayımlanmasına uygulaması varsa, yeni bir teklif, Git yoludur. Teklif bir SKU koleksiyonudur. Her teklif, Azure Market'te kendi varlık olarak görünür.

**SKU**

Bir SKU satın alınabilir en küçük birim bir tekliftir. Aynı ürün sınıf içinde sırada (teklif), SKU'ları desteklenen farklı özellikleri, teklifin yönetilen veya yönetilmeyen olup ve faturalandırma modelleri arasında ayırt etmenize olanak sağlar.

Farklı faturalama modellerini desteklemek istiyorsanız, birden çok SKU'ları ekleyin: kendi lisansını getir (KLG gibi), Kullandıkça Öde (PAYG), vs. 

Birden çok SKU'nun her SKU ayarlayın ve hesabın fiyatı farklı bir özellik desteklediğinde ekleyin.

Azure.com satın alınabilir kendi varlıkta olarak gösterilir ancak bir SKU üst teklifi Azure Marketi altında gösterilir.

1.  Oturum [bulut iş ortağı portalı](http://cloudpartner.azure.com).

2.  Sol gezinti çubuğundan tıklayarak \"+ yeni teklif\" seçip \"Azure uygulamalarını\".

    ![Yeni Teklif](./media/cloud-partner-portal-publish-managed-app/newOffer.png)

3.  Yeni bir teklif \"Düzenleyicisi\" görünümü artık açılır ve yazma işlemini başlatabilirsiniz.

4.  \"Forms\" doldurulması gereken içinde soldaki görünür \"Düzenleyicisi\" görünümü. Her \"form\" doldurulması için alan kümesi oluşur. Alanları bir kırmızı yıldız ile işaretlenmiş gerekir (\*).

    > Yönetilen bir uygulama yazmak için 4 ana forms vardır.

    -   Teklif ayarları

    -   SKU'lar

    -   Market

    -   Destek

<a name="how-to-fill-out-the-offer-settings-form"></a>Teklif ayarları formu doldurun nasıl 
---------------------------------------

Teklif ayarları formu teklif ayarlarını belirtmek için temel bir biçimidir.
Farklı alanlar, aşağıda açıklanmıştır.

**Teklif kimliği**

Bir yayımcı profilinde teklif için benzersiz bir tanımlayıcı.
Resource Manager şablonları, ürün URL'leri görünür ve faturalandırma raporlar. Yalnızca alfasayısal karakterden küçük veya tire (-) izin verilir. Tire bitiş veya en çok 50 karakter aşıyor. Bu alan, teklif etkin hale gelir sonra kilitli.

**Yayımcı kimliği**

Bu açılan listeyi, bu teklif altında yayımlamak istediğiniz yayımcı profilinizle seçmenize olanak sağlar. Bu alan, teklif etkin hale gelir sonra kilitli.

**Ad**

Teklifiniz için görünen adı. Azure Marketi'nde hem de Azure portalında görünür adı. En fazla 50 karakter olabilir. Bir kılavuz ürününüzün tanınabilir bir marka adı eklemektir. Şirketinizin adını buraya nasıl pazarlanmadığından olmadığı sürece dahil değildir. Bu teklif kendi Web pazarlama, adı tam olarak nasıl siteniz görünür olduğundan emin olun.

Tıklayarak \"Kaydet\" ilerlemenizi kaydetmek için. Teklifiniz için SKU'ları eklemek için sonraki adım olacaktır.

<a name="how-to-create-skus"></a>SKU'ları oluşturma 
------------------

Tıklayarak \"SKU'ları\" formu. Bir seçenek için burada gördüğünüz \"bir SKU ekleyin\" hangi girmenize olanak tanıyacak öğesine tıklayarak bir \"SKU kimliği\".

![Yeni Teklif SKU'ları](./media/cloud-partner-portal-publish-managed-app/newOffer_skus.png)

\"SKU kimliği\" SKU teklif içinde benzersiz tanımlayıcısıdır. Ürün URL'ler, ARM şablonları, bu kimliği görünmez ve faturalandırma raporlar. Yalnızca küçük harfli alfasayısal karakterler veya tirelerden (-) oluşabilir.
Tire bitemez ve en çok 50 karakter olabilir. Bu alan, teklif etkin hale gelir sonra kilitli. Bir teklif içinde birden çok SKU’ya sahip olabilirsiniz. Bir SKU yayımlamayı planlama için her görüntü ihtiyacınız vardır.

Bir SKU eklendikten sonra SKU'ları içinde listesinde görünür \"SKU'ları\" formu. SKU adı, ilgili SKU ayrıntılarını almak için tıklayın. Aşağıda, bazı alanlar için bazı ayrıntılar verilmiştir.

Tıklayarak sonra \"yeni SKU\", aşağıdaki formu doldurmanız gerekir:

![Yeni Teklif - yeni SKU](./media/cloud-partner-portal-publish-managed-app/newOffer_newsku.png)

### <a name="how-to-fill-sku-details-section"></a>SKU ayrıntıları bölümü dolduracak şekilde nasıl 

**Başlık** -bu Sku için bir başlık sağlar. Bu öğe için galeride gösterilir ne budur.

**Özet** -bu sku için kısa bir Özet sağlar. Bu metin, sağ başlığı gösterir.

**Açıklama** -SKU hakkında ayrıntılı bir açıklama sağlar.

**SKU türü** -izin verilen değerler \"yönetilen uygulamayı\" ve \"çözüm şablonları\". Bu örnekte, seçersiniz \"yönetilen uygulamayı\".

### <a name="how-to-fill-package-details-section"></a>Paket Ayrıntıları bölümü dolduracak şekilde nasıl 

Paket bölümü doldurulması gereken aşağıdaki alanlar vardır.

![Yeni Teklif - yeni SKU paketi](./media/cloud-partner-portal-publish-managed-app/newOffer_newsku_package.png)

**Geçerli sürümü** -karşıya yükleme paket için sürümü sağlar.
Biçiminde olmalıdır.

**Dosya paketini** -paket bir .zip dosyasına sıkıştırılmış aşağıdaki dosyaları içerir.

applianceMainTemplate.json - çözüm/uygulamayı dağıtmak ve içinde tanımlanan kaynakları oluşturmak için kullanılan dağıtım şablonu dosyası. Yazar dağıtım şablonu dosyaları burada nasıl daha fazla ayrıntı bulabilirsiniz- <https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-create-first-template>

applianceCreateUIDefinition.json - bu dosya, bu çözüm/uygulama sağlamak için kullanıcı arabirimi oluşturmak için azure.com adresinden portala tarafından kullanılır. Bu dosyayı şurada oluşturma hakkında daha fazla ayrıntı bulabilirsiniz- <https://docs.microsoft.com/azure/azure-resource-manager/managed-application-createuidefinition-overview>

mainTemplate.json - yalnızca Microsoft.Solution/appliances kaynağı içeren şablon dosyası. Bu kaynak dikkat edilmesi gereken önemli özellikleri şunlardır:

-   \"tür\" -değeri olmalıdır \"Market\" Market'te yönetilen uygulama senaryosu söz konusu olduğunda
-   \"ManagedResourceGroupId\": Müşteri kaynak grubunda\'applianceMainTemplate.json içinde tanımlanan tüm kaynakların dağıtılacağı s abonelik.
-   \"Publisherpackageıd\": paketi benzersiz olarak tanımlayan bir dize. 

Değeri şu şekilde oluşturulması gerekir - bir birleşimi olan \[Publisherıd\].\[ OfferId\]-preview\[SKUID\].\[ PackageVersion\].
Bu değerlerin her birini aşağıda gösterildiği gibi elde edilebilir.

Bu paketin herhangi bir iç içe geçmiş şablonlar içermesi veya bu uygulama başarıyla sağlamak için gereken betikler. MainTemplate.json ve applianceMainTemplate.json applianceCreateUIDefinition.json, kök klasörde mevcut olması gerekir.

**Yetkilendirmeleri** -bu özellik müşterilerin abonelik içindeki kaynaklara erişim kimlerin ve hangi erişim düzeyini tanımlar. Bu uygulamayı müşteri adına yönetmek yayımcı sağlar.

-   Principalıd - kullanıcı, kullanıcı grubu veya belirli izinler müşteri aboneliğindeki kaynaklar üzerinde (rol tanımı tarafından açıklandığı gibi) verilecek uygulama Azure Active directory tanımlayıcısı.

-   Rol tanımı - Azure AD tarafından sağlanan tüm yerleşik RBAC rolleri listesi. Rol en uygun kaynakları müşteri adına yönetmek izin veren seçebilirsiniz.

Birden çok yetkilendirmeleri eklenebilir. Ancak, bir Active Directory kullanıcı grubu oluşturun ve kendi kimliği belirtmek için önerilir \"Principalıd\."  Bu SKU güncelleştirme gerek kalmadan daha fazla kullanıcı kullanıcı grubuna eklenmesine olanak sağlar.

RBAC hakkında daha fazla bilgi burada bulunabilir- <https://docs.microsoft.com/azure/active-directory/role-based-access-control-what-is>

<a name="marketplace-form"></a>Market formu
----------------

Üzerinde görüntülenecek alanlar için bir azure uygulaması teklif Market formda ister [Azure Marketi](https://azuremarketplace.microsoft.com) ve [Azure portalı](https://portal.azure.com/). Aşağıda, bazı alanlar için bazı ayrıntılar verilmiştir.

#### <a name="preview-subscription-ids"></a>Önizleme abonelik kimlikleri

Burada yayımlandıktan sonra teklifin erişmesini istediğiniz Azure abonelik kimlikleri listesini girin. Bu teknik listelenen abonelikler test bunu yapmadan önce önizlenen teklifini sağlayacak Canlı. İş ortağı portalı beyaz liste 100 abonelikleri kadar olanak tanır.

#### <a name="suggested-categories"></a>Önerilen kategorileri

En fazla beş kategoriye teklifinizi en iyi ile ilişkilendirilebilen sağlanan listeden seçin. Seçili kategorilerdeki teklifinizi, bulunan ürün kategorilerini eşlemek için kullanılan [Azure Marketi](https://azuremarketplace.microsoft.com) ve [Azure portalı](https://portal.azure.com/).

İçinde bu formda sağladığınız veriler gösterilir yerlerden bazıları aşağıda verilmiştir.

##### <a name="azure-marketplace"></a>Azure Market

![publishvm10](./media/cloud-partner-portal-publish-managed-app/publishvm10.png)

![publishvm11](./media/cloud-partner-portal-publish-managed-app/publishvm11.png)

![publishvm15](./media/cloud-partner-portal-publish-managed-app/publishvm15.png)

##### <a name="portal-at-azurecom"></a>Azure.com portalında

![publishvm12](./media/cloud-partner-portal-publish-managed-app/publishvm12.png)

![publishvm13](./media/cloud-partner-portal-publish-managed-app/publishvm13.png)

##### <a name="logo-guidelines"></a>Logo yönergeleri

Bulut iş ortağı Portalı'nda karşıya logo izlemelidir yönergeleri aşağıda:

-   Azure tasarımının basit bir renk paleti vardır. Logonuzu düşük tutmak için ikincil renk ve birincil sayısı.

-   Azure.com adresinden portala tema rengini beyaz ve siyah. Bu renkler, logolar arka plan rengi kullanmaktan kaçının. Azure.com, logolar portalında belirgin olarak yapacağınız bazı renk kullanın.
    Basit birincil renkleri öneririz. **Saydam arka plan kullanıyorsanız, logoları ve metin beyaz, siyah veya mavi olmadığından emin olun.**

-   Arka plan gradyan logosunu kullanmayın.

-   Metin, hatta şirket, yerleştirmekten kaçının veya adına logo marka.
    Logonuzu Görünüm ve yapısını olmalıdır \'düz\' gradyanlar kaçınmalısınız.

-   Logo uzatma kaçının.

##### <a name="hero-logo"></a>Hero logosu

Hero logosu isteğe bağlıdır. Yayımcı Hero logoyu karşıya yükleyin değil seçebilirsiniz. Ancak bir kez karşıya yüklenen hero simge silinemiyor. Bu durumda, iş ortağı Hero simgeler için Azure Marketi yönergelere uyması gerekir.

###### <a name="guidelines-for-the-hero-logo-icon"></a>Hero logosu simgesi için yönergeler

-   Yayımcı görünen adı, planı başlık ve uzun Özet teklif beyaz renkte görüntülenir. Herhangi bir ışık rengi Hero simgesinin arka planda kaçının. Siyah, beyaz ve saydam arka planlar için Hero simgeler izin verilmez.

-   Bu teklif listelenen, yayımcı görünen adı, planı başlık, uzun Özet teklif ve Oluştur düğmesine içinde Hero logosu programlı olarak katıştırılır. Hero logosu tasarlarken, herhangi bir metin girmeniz gerekmez. Görünen adı, planı başlık, teklif uzun özeti ve vb. yayımcı için sağdaki boş boşluk bırakın. Bunlar, programlı olarak dahil edilir.
    Metin 415 x 100 sağdaki ve uzaklığı ile 370 alanları boş soldan piksel.

![publishvm14](./media/cloud-partner-portal-publish-managed-app/publishvm14.png)

<a name="support-form"></a>Destek formu
------------

Destek formu doldurun sonraki biçimidir. Bu form için destek kişileri, şirketten ister.  Mühendislik iletişim bilgilerini ve müşteri destek iletişim bilgileri bazı örnekler verilmiştir.

<a name="how-to-publish-an-offer"></a>Bir teklifi yayımlama
-----------------------

Teklifinizi drafted, sonraki adım, Azure Marketi'nde teklif yayımlamak sağlamaktır. Tıklayın \"Yayımla\" teklifinizi sürecini Başlat düğmesi Canlı.
