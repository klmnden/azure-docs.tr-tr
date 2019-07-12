---
title: Yönetilen hizmetler teklifi Azure Marketinde yayımlama
description: Ekleme müşteriler için Azure kaynak yönetimi temsilcisi bir yönetilen hizmet teklifi yayımlama hakkında bilgi edinin.
author: JnHs
ms.author: jenhayes
ms.service: lighthouse
ms.date: 07/11/2019
ms.topic: overview
manager: carmonm
ms.openlocfilehash: bb2f26a170bbd60eb927bd00f6def7d033fafee9
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67809851"
---
# <a name="publish-a-managed-services-offer-to-azure-marketplace"></a>Yönetilen hizmetler teklifi Azure Marketinde yayımlama

Bu makalede, bir genel veya özel bir yönetilen hizmetler teklife yayımlanacağını öğreneceksiniz [Azure Marketi](https://azuremarketplace.microsoft.com) kullanarak [bulut iş ortağı portalı](https://cloudpartner.azure.com/), yapılacak bir teklif satın alan müşteriler etkinleştirme için Azure kaynak yönetimi temsilcisi. 

> [!NOTE]
> Geçerli bir sahip olması [hesap iş ortağı Merkezi'nde](https://docs.microsoft.com/azure/marketplace/partner-center-portal/create-account) oluşturma ve bu teklifleri yayımlama. Zaten bir hesabınız yoksa [kaydolma işlemini](https://aka.ms/joinmarketplace) bir hesap iş ortağı Merkezi'nde oluşturup ticari Market programına kaydolma adımlarda size yol gösterecektir. Microsoft iş ortağı ağı (MPN) Kimliğiniz olacak [otomatik olarak ilişkili](https://docs.microsoft.com/azure/billing/billing-partner-admin-link-started) teklifler sayesinde, etkisi müşterilerle yaşadığımız izlemek için yayımlayın.
>
> Bir teklifi Azure Marketinde yayımlama istemiyorsanız, müşterilerinizin el ile Azure Resource Manager şablonları kullanarak yapabilirsiniz. Daha fazla bilgi için bkz. [yerleşik bir müşteri Azure kaynak yönetimi için temsilci](onboard-customer.md).

Yönetilen hizmetleri yayımlama teklif başka türde bir teklifi Azure Market'te yayımlama için benzerdir. Bu işlem hakkında bilgi edinmek için [Azure Market ve Appsource'ta yayımlama Kılavuzu](https://docs.microsoft.com/azure/marketplace/marketplace-publishers-guide) ve [Azure yönetmek ve AppSource Market teklifleri](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/manage-offers/cpp-manage-offers).

> [!IMPORTANT]
> Her bir yönetilen Hizmetleri teklifi planda içeren bir **bildirim ayrıntılarını** bölümünde tanımladığınız yerlerde Azure Active Directory (Azure AD) varlıkları kiracınızdaki temsilci kaynak grupları ve/veya abonelikler için erişimi olacaktır Bu planı satın almış olan müşteriler. Tüm grubuna (veya kullanıcı veya hizmet sorumlusu) buraya dahil planı satın her müşteri için aynı izinlere sahip olacağını bilmeniz önemlidir. Her müşteri ile çalışan için farklı gruplara atamak için her müşteri için özeldir ayrı, özel bir plan yayımlamak gerekir.

## <a name="create-your-offer-in-the-cloud-partner-portal"></a>Bulut iş ortağı Portalı'nda teklifinizi oluşturun

1. Oturum [bulut iş ortağı portalı](https://cloudpartner.azure.com/).
2. Soldaki menüden **yeni teklif**, ardından **yönetilen Hizmetleri**.
3. Gördüğünüz bir **Düzenleyicisi** bölüm doldurmak dört bölümden teklifiniz için: **Teklif ayarları**, **planları**, **Market**, ve **Destek**. Bu bölümleri tamamlayın hakkında yönergeler için okumaya devam edin.

## <a name="enter-offer-settings"></a>Teklif ayarları girin

İçinde **teklif ayarları** bölümünde, şu bilgileri sağlayın:

|Alan  |Açıklama  |
|---------|---------|
|**Teklif kimliği**     | (İçinde yayımcı profilinizle) teklifiniz için benzersiz bir tanımlayıcı. Bu kimlik yalnızca küçük harf alfasayısal karakterler, tire ve alt çizgi, en çok 50 karakter içerebilir. Teklif kimliği müşterilere ürün URL'leri ve faturalandırma raporlarını gibi yerde görünür olabileceğini göz önünde bulundurun. Teklifi yayımladıktan sonra bu değeri değiştiremezsiniz.        |
|**Yayımcı kimliği**     | Bir Teklifle ilişkili yayımcı kimliği. Birden fazla yayımcı kimliği varsa, bu teklif için kullanmak istediğiniz birini seçebilirsiniz.       |
|**Name**     | Müşteriler için teklifinizin Azure Market'te ve Azure portalında gördüğü adı (en çok 50 karakter). Müşteriler anlayacaksınız tanınabilir bir marka adı kullanın — kendi Web sitesi aracılığıyla bu teklif yükseltme,'ı adını buraya kullandığınızdan emin olun.        |

İşiniz bittiğinde, select **Kaydet**. Oturum geçmeye hazır artık **planları** bölümü.

## <a name="create-plans"></a>Planları oluşturun

Her bir teklifin (bazen SKU'ları da adlandırılır) bir veya daha fazla plan olması gerekir. Belirli müşterilerin sınırlı bir kitle için belirli bir plana özelleştirmek için ya da farklı özellik kümeleri farklı fiyatlarla desteklemek için birden çok planı ekleyebilirsiniz. Müşteriler, üst teklif altında kullanılabilir planları görüntüleyebilir.

Oluşturmak istediğiniz her plan için planlar bölümünde seçin **yeni Plan**. Enter bir **Plan kimliği**. Bu kimlik yalnızca küçük harf alfasayısal karakterler, tire ve alt çizgi, en çok 50 karakter içerebilir. Plan kimliği, müşterilere ürün URL'leri ve faturalandırma raporlarını gibi yerde görülebilir. Teklifi yayımladıktan sonra bu değeri değiştiremezsiniz.

Ardından, aşağıdaki bölümlerde tamamlamak **planlama ayrıntıları** bölümü:

|Alan  |Açıklama  |
|---------|---------|
|**Başlık**     | Görüntü için plan için kolay ad. En fazla 50 karakter uzunluğunda.        |
|**Özet**     | Plan için görünen başlık altında birleştiren açıklaması. En fazla 100 karakter uzunluğunda.        |
|**Açıklama**     | Açıklama metni planın daha ayrıntılı bir açıklama sağlar.         |
|**Faturalandırma modeli**     | Burada gösterilen 2 faturalandırma modeli vardır, ancak seçmeniz gerekir **kendi lisansını Getir** için yönetilen hizmetler sunar. Bu, müşterileriniz için doğrudan bu teklife ilgili maliyetleri faturalandırır ve Microsoft size ücretler doldurmaz anlamına gelir.   |
|**Bu, özel bir Plan mi?**     | SKU özel veya genel olup olmadığını belirtir. Varsayılan değer **Hayır** (Genel). Bu seçimi değiştirmeden bırakırsanız, planınızı belirli müşterilere (veya belirli bir sayıda müşteriler için); kısıtlı olmaz bir genel planda yayımladıktan sonra daha sonra bunu özel değiştiremezsiniz. Bu planı yalnızca belirli müşterilere kullanılabilir hale getirmek üzere seçin **Evet**. Bunu yaptığınızda, kullandıkları abonelik kimliklerini sağlayarak müşterileri belirlemek gerekir. Bu, girilen tek tek (için 10 adede kadar abonelikleri) veya (en fazla 20.000 abonelikleri için) bir .csv dosyası yükleyerek olabilir. Test edebilir ve teklif doğrulamak için burada kendi aboneliklerini eklediğinizden emin olun. Daha fazla bilgi için [özel SKU'ları ve planları](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal-orig/cloud-partner-portal-azure-private-skus).  |

Son olarak, tamamlamak **bildirim ayrıntılarını** bölümü. Bu, müşteri kaynaklarını yönetmek için yetkilendirme bilgileri içeren bir bildirimi oluşturur. Burada sağladığınız bilgileri temsilci eklemek için gerekli müşterileriniz için Azure kaynak yönetimi. Yukarıda belirtildiği gibi bu izinleri satın alma planı, her müşteri için uygulanacak belirli bir müşteri erişimi sınırlandırmak istiyorsanız, kendi özel kullanım için özel bir plan yayımlama gerekecek.

- İlk olarak sağlayan bir **sürüm** bildirimi. Biçimini kullanın *n.n.n* (örneğin, 1.2.5).
- Ardından, girin, **Kiracı kimliği**. (Yani, müşterilerinizin kaynaklarını yönetmek için çalışacak, Kiracı), kuruluşunuzun Azure Active Directory Kiracı kimliği ile ilişkili bir GUID budur. Bu kullanışlı yoksa, Azure portalının sağ üst taraftaki hesap adınızın üzerine gelin veya seçerek bulabilirsiniz **dizini Değiştir**. 
- Son olarak, bir veya daha fazla Ekle **yetkilendirme** planınıza girdileri. Yetkilendirmeleri erişebileceği kaynakları ve planı satın almış olan müşteriler için abonelikler varlıklar tanımlayın. Temsil edilen Azure kaynak Yönetimi'ni kullanarak müşteri adına kaynaklara erişmek için bu bilgileri sağlamanız gerekir.
  Her yetkilendirme için aşağıdakileri sağlayın. Ardından seçebilirsiniz **yeni yetkilendirme** gerektiği pek çok daha fazla kullanıcı/rol tanımı eklemek için.
  - **Azure AD nesnesi kimliği**: Bir kullanıcı, kullanıcı grubu veya belirli izinler (rol tanımı tarafından açıklandığı gibi), müşterilerinizin kaynaklarına verilecek uygulama Azure AD tanımlayıcısı.
  - **Azure AD nesne görünen adı**: Müşterinin bu yetkilendirme amacını anlamalarına yardımcı olmak için bir kolay ad. Müşteri, kaynakları yetkilendirirken bu adı görür.
  - **Rol tanımı**: Kullanılabilir birini seçin listeden Azure AD yerleşik rolleri. Bu rolün izinleri belirler, kullanıcı **Azure AD nesnesi kimliği** alan müşterilerinizin kaynaklara sahip olur. Bu roller hakkında daha fazla bilgi için bkz. [yerleşik roller](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles).
  - **Atanabilir rolleri**: Kullanıcı erişimi Yöneticisi'nde seçtiyseniz **rol tanımı** için bu yetkilendirme, bir veya daha fazla atanabilir rolleri buraya ekleyebilirsiniz. Kullanıcı **Azure AD nesnesi kimliği** alan bunlar atayamazsınız olacaktır **atanabilir rolleri** için [yönetilen kimlikleri](https://docs.microsoft.com/azure/managed-applications/publish-managed-identity). Normalde kullanıcı erişimi yöneticisi rolü ile ilişkili hiçbir diğer izinler bu kullanıcı için geçerli olduğunu unutmayın. (Bu kullanıcının rol tanımı için kullanıcı erişimi Yöneticisi seçmediyseniz, bu alanın etkisi yoktur.)

> [!TIP]
> Çoğu durumda, bir Azure AD kullanıcı grubuna veya hizmet sorumlusu yerine bir dizi bireysel kullanıcı hesapları için izinleri atamak istersiniz. Bu ekleme veya güncelleştirme ve erişim gereksinimleriniz değiştiğinde planı yeniden yayımlamak zorunda kalmadan bireysel kullanıcılar için erişim kaldırmanıza olanak sağlar.

Ekleme planlarını bittiğinde seçin **Kaydet**, ardından Devam **Market** bölümü.

## <a name="provide-marketplace-text-and-images"></a>Market metin ve görüntüleri sağlayın

**Market** bölümüdür, müşteriler Azure Marketi hem de Azure portalında görürsünüz görüntüleri ve metin sağlarsınız.

Aşağıdaki alanlar için bilgileri sağlayın **genel bakış** bölümü:

|Alan  |Açıklama  |
|---------|---------|
|**Başlık**     |  Teklif, genellikle uzun, biçimsel adı başlığı. Bu konu başlığı, Market'te göze çarpacak şekilde görüntülenir. En fazla 50 karakter uzunluğunda. Çoğu durumda, bu aynı olmalıdır **adı** girdiğiniz **teklif ayarları** bölümü.       |
|**Özet**     | Amaç veya teklifiniz işlevini kısa. Bu, genellikle başlığı görüntülenir. En fazla 100 karakter uzunluğunda.        |
|**Uzun özeti**     | Teklifinizin işlevi ve amacı daha uzun bir özeti. En fazla 256 karakter uzunluğunda.        |
|**Açıklama**     | Teklifinizi hakkında daha fazla bilgi. Bu alan en fazla 3000 karakterden oluşabilir ve basit bir HTML biçimlendirmeyi destekler.        |
|**Pazarlama tanımlayıcısı**     | Benzersiz bir URL kullanımı kolay tanımlayıcı. Bu teklif için Market URL'lerde kullanılır. Örneğin, yayımcı kimliğiniz *contoso* ve pazarlama tanımlayıcınızı *örnek uygulaması*, teklifinizi Azure Marketi'nde URL'si olacaktır *https://azuremarketplace.microsoft.com/marketplace/apps/contoso.sampleApp* .        |
|**Önizleme abonelik kimlikleri**     | Bir ila 100 abonelik tanımlayıcıları ekleyin. Bu abonelikle ilişkilendirilmiş müşteriler Canlı geçmeden önce Azure Marketi'nde teklif görüntülemeniz mümkün olacaktır. Kullanılabilir müşterilere hale önce teklifinizin Azure Market'te nasıl göründüğünü önizleme için kendi abonelikler buraya dahil öneririz.  (Microsoft destek ve mühendislik ekipleri Ayrıca bu Önizleme süresi boyunca teklifin görüntülemeniz mümkün olacaktır.)   |
|**Faydalı bağlantılar**     | URL'leri ilgili belgeleri gibi kullanıcınıza teklifiniz için sürüm notları, SSS sayfaları, vs.        |
|**Önerilen kategorileri (en fazla 5)**     | Teklifiniz için uygulanacak bir veya daha fazla kategori (en fazla beş tane). Bu kategoriler, müşterilerin teklife Azure Marketi ve Azure portalı Bul yardımcı olur.        |

İçinde **pazarlama Yapıtları** bölümünde, logoları ve diğer varlıkları teklifinizle gösterilecek yükleyebilirsiniz. Ekran görüntüleri isteğe bağlı olarak yükleyebilir veya teklifiniz, müşterilere yardımcı olabilecek videoların bağlantıları anlama.

Dört logosu boyutları gereklidir: **Küçük (40 x 40)** , **Orta (90 x 90)** , **büyük (115 x 115)** , ve **geniş (255 x 155)** . Logolar için aşağıdaki yönergeleri izleyin:

- Azure tasarımının basit bir renk paleti vardır. Logonuzdaki birincil ve ikincil renklerinin sayısını sınırlandırın.
- Portalın tema renkleri siyah ve beyazdır. Bu renkleri logonuzun arka plan rengi olarak kullanmayın. Logonuzun portalda öne çıkmasını sağlayan bir renk kullanın. Basit birincil renkleri öneririz.
- Saydam arka plan kullanıyorsanız logosu ve metin beyaz, olmadığını unutmayın siyah veya mavi.
- Logonuzun genel görünümü düz olmalı ve gradyanlardan kaçınmalıdır. Logoda gradyan arka plan kullanmayın.
- Şirket veya marka adınız dahil olmak üzere logoya metin yerleştirmeyin.
- Logonuzun esnetilmediğinden emin olun.

**Hero (815 x 290)** logosudur isteğe bağlıdır ancak önerilir. Hero logosu dahil ederseniz, aşağıdaki yönergeleri izleyin:

- Kullanmayın hero logo herhangi bir metni içeren ve boş alanı 415 piksel logosu sağ tarafında ayırdığınızdan emin olun. Bu program aracılığıyla gömülecektir metin öğelerini yer bırakmak için gereklidir: Yayımcı görünen adınızı, planı başlık uzun özeti sağlar.
- Hero logo arka plan, siyah, beyaz veya saydam olmayabilir. Arka plan rengini beyaz ekli metinleri görüntülenecek çünkü çok açık olmadığından emin olun.
- Teklifinizle bir hero simgesi yayımladıktan sonra bu (farklı bir sürüme sahip isterseniz güncelleştirebilirsiniz rağmen) kaldırılamıyor.

İçinde **sağlama Yönetim** bölümünde seçebileceğiniz adaylarınızı depolanacağı, CRM sistemine isterseniz. 

Son olarak, sağlayın, **gizlilik ilkesi URL'si** ve **kullanım koşullarını** içinde **yasal** bölümü. Ayrıca burada kullanılıp kullanılmayacağını belirtebilirsiniz [standart sözleşme](https://docs.microsoft.com/azure/marketplace/standard-contract) Bu teklif için.

Oturum geçmeden önce değişikliklerinizi kaydetmek mutlaka **Destek** bölümü.

## <a name="add-support-info"></a>Destek bilgisi ekleme

İçinde **Destek** bölümünde, bir mühendislik birimi ilgili kişisi ve müşteri destek hizmetlerine adı, e-posta ve telefon numarası girin. Ayrıca sağlamanız gerekir URL'lerini destekleyen. Microsoft, iş hakkında sizinle iletişim kurmak ve sorunları desteklemek gerektiğinde bu bilgileri kullanabilir.

Bu bilgileri ekledikten sonra seçin **kaydedin.**

## <a name="publish-your-offer"></a>Teklifinizi yayımlayın

Tüm sağladığınız bilgi mutlu sonra sonraki adımınız teklifi Azure Marketinde yayımlama sağlamaktır. Seçin **Yayımla** teklifinizi sürecini başlatmak için düğmeye Canlı. Bu işlem hakkında daha fazla bilgi için bkz. [yayımlama Azure Market ve AppSource teklifler](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/manage-offers/cpp-publish-offer).

## <a name="next-steps"></a>Sonraki adımlar

- Hakkında bilgi edinin [kiracılar arası yönetim deneyimleri](../concepts/cross-tenant-management-experience.md).
- [Görüntüleme ve yönetme müşteriler](view-manage-customers.md) giderek **müşterilerimin** Azure portalında.
