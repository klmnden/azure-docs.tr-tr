---
title: Azure Market ve AppSource yayımcı Kılavuzu
description: Uygulama ve hizmet yayımcılar için Azure Marketi ve AppSource nelerdir genel bakış
services: Marketplace, Compute, Storage, Networking, Blockchain, Security
documentationcenter: ''
author: ellacroi
manager: msmbaldwin
editor: ''
ms.assetid: e8d228c8-f9e8-4a80-9319-7b94d41c43a6
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 05/09/2018
ms.author: ellacroi
ms.openlocfilehash: 30847ff20abf6654e58a0e72a12f04dcd88d5871
ms.sourcegitcommit: 909469bf17211be40ea24a981c3e0331ea182996
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
---
# <a name="azure-marketplace-and-appsource-publisher-guide"></a>Azure Market ve AppSource yayımcı Kılavuzu

## <a name="overview"></a>Genel Bakış

Hoş Geldiniz [Azure Marketi](https://azuremarketplace.microsoft.com) ve [AppSource](https://appsource.microsoft.com) yayımcı Kılavuzu. Bu kılavuz, yeni ve mevcut yayımcılar Azure Marketi ve AppSource veriş kullanma, uygulamalarının ve hizmetlerinin yayımlama ve Microsoft yöneticileriyle işletmelerini büyümeye nasıl anlamanıza yardımcı olmak için tasarlanmıştır. 

Bu kılavuz sonuna daha her biri bu konular hakkında bilmeniz ve daha ayrıntılı bilgi nerede bulacağını biliyorsanız:

- Market veriş birinde dökümün avantajları nelerdir
- Veriş kullanma
- Hangi mağaza teklifler ve hizmetler için doğru 
- Ne tür uygulamaları ve Hizmetleri teklifleri yayımlanabilir
- Yayımlama seçeneklerinin her biri için teknik ve iş gereksinimleri nelerdir
- Önceden yayımlama varlık denetim listesi oluşturma
- Bir yayımcı olmaya
- Oluşturma ve yayımlama nereye sunar
- Bir liste en iyi duruma getirme ve sürücü etkisi Git Pazar kaynaklara kullanma
- Yardım ve Destek alınacağı

Azure Market, AppSource veya yayımlama bu kılavuz hakkında sorular için Market Ekibi başvurun cloudmarketplace@microsoft.com. 

## <a name="benefits"></a>Avantajlar

**Market katılan avantajları**

Azure Market ve AppSource Microsoft ve bir ortak satış hazır ortaklığı Fırsat için kapısı birleşik Go Market etkinliklerle Git'i markalarıdır. Başlatma yükseltme, isteğe bağlı oluşturma ve satış ve pazarlama Eklem kullanarak bulut iş altyapınız ve flywheel iş büyüme için bir merkez Market listelerini hale getirebilirsiniz. Market katılan için hiçbir ücretleri vardır. Amacımız en iyi çözüm ve bizim iş ortağı ekosistemi sunar Hizmetleri ile Microsoft müşterileri bağlamaktır.

İşinizi büyümeye Market özelliklerinden yararlanmak:

- **Müşteri adayları ve satış fırsatları oluşturmak**. Genişletilmiş bir Portföy çözümleri ile yeni pazarlara Microsoft bulut platformunda girin. Yukarı satış ve çapraz satış Market teklifleri. 
- **Var olan ve yeni müşterilerle anlaşma boyutunu artırın ve iş değerini artırın**. Anlaşma boyutu büyümesine ve buluta iş yükleri taşırken, müşteri sorunlarını çözün. Satış uzatmaya ve bu hedef belirli iş yükleri ve endüstri senaryoları eksiksiz çözüm satış anlaşma Karlılık artırın.
- **Eyleme dönüştürülebilir Öngörüler elde**. Sizi başarınız, bizim başarımızdır. Bulut iş ortağı Portalı aracılığıyla, listelerinin performansına Öngörüler alın. Ne gerçekleştiriyor öğrenin, ne sizi oluşturulur ve kampanya etkinliklerinizi en üst düzeye çıkarmak nasıl.

>[!NOTE]
>Office genişleten uygulamalar Öngörüler Office uygulamaları için yayımlama işlemi aracılığıyla erişirsiniz.

## <a name="storefronts"></a>Veriş

Microsoft iş ortaklarının teklifler listesinde, denemeler etkinleştirmek ve doğrudan Microsoft müşterileri ve ekosistemi ile transact izin iki ayrı Market veriş sağlar: [Azure Marketi](https://azuremarketplace.microsoft.com) ve [AppSource](https://appsource.microsoft.com). Bu veriş bulmak için deneyin ve uygulamaları ve bunların dijital dönüştürme hızlandırmak hizmetleri satın müşteriler etkinleştirin. İşlerini Microsoft müşterileri erişimi artırarak büyür ve iş ortağı ekosistemlerini yayımcılar yardımcı olurlar.
 
Her mağaza yayımlama yatırımınızı en üst düzeye çıkarmanıza yardımcı olmak için özelleştirilmiş yayımlama seçeneklerini sunar. Bu seçenekler aşağıdaki tabloda özetlenmiştir:


|          |Azure Market |AppSource  |
|---------|---------|---------|
|Hedef kitle     |BT uzmanlarının ve geliştiricilerin (DBAs, SecOps, DevOps uzmanı roller içerir)    | İş karar alıcılar (üretim, muhasebe tedarik uzmanı roller içerir)      |
|İle oluşturulan veya genişletmek için     |Azure         | Azure, Dynamics 365, Office 365, Power BI, PowerApps       |
|Çözüm ve hizmet türleri     |  Altyapı çözümleri, Profesyonel Hizmetler   | Tamamlanmış iş bulut uygulamalarını, Office 365 eklentiler, Profesyonel Hizmetler        |
|Yayımlama seçenekleri     |  Me danışmanlık hizmetleri sunmak, kişi deneme, sanal makine, çözüm şablonu, yönetilen uygulama       |  Ücretsiz deneme, şimdi al test sürücüsü, ilgili kişi benim danışmanlık hizmetleri sağlar      |
|Uygulama Deneyimi uygulamalar ve hizmetler kendi uygulama bağlamında kullanıcılara erişim vermek için  | Azure portalı ve Azure CLI         | Office 365, Dynamics 365, Power BI, Office istemci uygulamaları       |

## <a name="how-to-publish-on-cloud-marketplace"></a>Bulut Market'te yayımlama

Bir bulut Market yayımcı olma bir kolay üç adımlık bir işlemdir:
1.  Liste türü sağ teklifiniz için belirleme
2.  Bir bulut Market yayımcı hale için kaydolun
3.  Teklif ve listeleme türüne göre gerekli teknik ve içerik ön koşullar tamamlayın


**1.    Sağ teklifiniz için liste türünü belirleme**

Her mağaza birden fazla yayımlama seçeneklerini ve teklif türlerini destekler. En iyi uygulama ve hizmet ayrıntıları temsil eden bir teklif türü seçin. Tüm yayımlama seçeneklerini iş ortakları, paylaşım sağlama için erişim sahibi olursunuz.

|**Yayımlama seçeneği**  | **Teklif türü** | **Mağaza**  |
|---------|---------|---------|
|**Liste**    |    Danışmanlık Hizmetleri benimle iletişim     |  Azure Market, AppSource       |
|**Deneme**   |     Ücretsiz deneme sürümü, SaaS deneme, etkileşimli demo, sürücü sınayın.    |  Azure Market, AppSource       |
|**İşlem**     |   Sanal makine, çözüm şablonu, yönetilen uygulama, kapsayıcıları, SaaS abonelikleri      |    Azure Market     |



**2.    Bir bulut iş ortağı yayımcı olur**

Bizim bulut Market'te yayımcı olarak kaydetmek için aşağıdaki adımları izleyin. Microsoft ve seçilen liste türü, mevcut engagement bağlı olarak bazı adımlar gerekli olabilir: 

|Market kayıt adımı  |Zaman  |Açıklama  |
|---------|---------|---------|
| 1. Microsoft iş ortağı ağı kaydet | 15 dakika | Resmi bir Microsoft iş ortağı haline gelir ve başka avantajları almak ve bir Azure Market yayımcı olabilme desteği için Microsoft Partner Network (MPN) katılın. MPN kaydetmek için lütfen Microsoft iş ortağı ağı ziyaret edin ve "Kaydet"'i tıklatın Kuruluşunuzun varolan bir üyelik, (varsa), kayıt sırasında katılmak kuramaz. Kaydedildikten sonra kuruluşunuzun MPN Kimliğini not alın: Yayımcı profilinizde bulut iş ortağı portalını (adım 3) etkinleştirmek de isteyeceğiz.      |
|2. Microsoft kimlik oluşturun     |   15 dakika      |  Bu Microsoft ID bulut iş ortağı portalına erişmek için kullanılır. Bu e-posta adresi a Microsoft ID kayıtlı olması gerekir ve hem bulut iş ortağı Portalı'nı (adım 3) hem de Microsoft Developer Center'da (4. adım) için kullanılır. Seçilen e-posta adresi şirket etki alanınızda tercihen olmalıdır ve BT ekibi tarafından denetlenir. Bir kimliği oluşturmadan önce yönergeleri için kılavuzları ve nasıl yapılır bölümleri gözden geçirin. |
|3. Market Adaylığı form gönderme     |  1-3 gün       | Kuruluşunuzun bir Microsoft bulut Market yayımcı olmasını belirler. Form kuruluşunuz, ilk uygulama veya hizmet teklifi yayımlamak istediğiniz ve düzeyde sağlayacağınız desteği hakkında bilgi içerir. <ul><li>[Azure Market Adaylığı formu](http://aka.ms/listonazuremarketplace)</li><li>[AppSource Adaylığı formu](http://aka.ms/listonappsource)</li></ul> Formu gönderdikten sonra Market Ekibi uygulama gözden geçirin ve istek doğrulayın. İstek inceledikten sonra onaylanmış bir iş ortağı olmadan için sonraki adımda e-postayla bildirilecek ve bulut iş ortağı portalına erişim, burada ilk teklif listenizi tamamlamak ve ek oluşturma sunar. Onaylanırsa, aynı zamanda Microsoft Development Center (4. adım) kayıt ücret feragat etmiş bir promosyon kodu alırsınız. |
|4. Geliştirici Merkezi'nde kaydetme     |    5-10 gün     | Microsoft Developer Center'da Market'te sanal makineler, çözüm şablonları ve Azure ile yönetilen uygulamalar gibi özellikleri sahip olan uygulamalar yayımlamayı transact için gereklidir. Bu gereksinim, şirket bilgileri şirketinizin yasal, vergi ve varlıkları bankacılık doğrulamak Microsoft izin verir. Registrant geçerli temsilcisi kuruluşun olmalıdır ve kimliğini doğrulamak için kişisel bilgileri sağlamanız gerekir. Kaydetme kişi a Microsoft şirket için paylaşılan ID (2. adım) kullanmanız gerekir ve aynı hesabı bulut iş ortağı Portalı'nda kullanılmalıdır. <ul>Market Adaylığı form tamamladıysanız henüz 99 kayıt ücret ödemeniz istenir, olduğunu unutmayın. Kullanıma başlama öncesi bu ücret sağlamak için Market Adaylığı formu doldurun ve e-posta yoluyla bir promosyon kodu alırsınız. Önemli: bir Microsoft Developer Center'da hesabı oluşturmayı denemeden önce şirketinizin zaten olmayan emin olun. Bu işlemi adım adım açıklama için geliştirici Merkezi'nde kaydetme hakkında yönergeler bakın.</ul>   |
|5. Bulut iş ortağı portalında oturum açın     |  15 dakika       |  Adaylığı onaylandıktan sonra kaydettiğiniz [Microsoft iş ortağı ağı](https://partner.microsoft.com/en-us/membership/) ve [Microsoft Developer Center'da](https://dev.windows.com/), erişmek için bir hesap oluşturulacak [bulut İş ortağı portalı](https://cloudpartner.azure.com/). İlk kez oturum açma kimlik bilgileri Adaylığı onay e-postayla dahil edilir. Bulut iş ortağı portalının nasıl kullanılacağı hakkında ayrıntılı bilgi için Git [öğrenin](https://cloudpartner.azure.com/#Learn) bir belge bölümü gözden geçirin ve portal menüsünde.    |

**3.    Teklif ve türü Önkoşullar listeleme tamamlayın:**

Teknik ve pazarlama içerik gereksinimleri mağaza, Teklif türü ve liste türüne göre değişir. Toplantı emin olmak için aşağıdaki özellikleri gözden geçirin:
-   StoreFront gereksinimleri: Azure Market ve AppSource
-   Liste türü gereksinimleri: Listesi, deneme ve Transact
-   Teklif türü gereksinimleri: 
 -  Uygulama – sanal makine, kapsayıcı ya da SaaS
 -  Teklif danışmanlık

## <a name="support"></a>Destek

**Destek**

Destek seçenekleri için Azure Marketi listesidir:

**Azure Market genel sorgular**

|Destek kanal |Açıklama |
|---------|---------|
|E-posta: cloudmarketplace@microsoft.com     |  Onboarding desteği dağıtım listesi. Keşif oturumları ve iş ortakları ile Mimari Tasarım oturumları ayarlama ekleme istekleri için kullanılır.        |

**Azure Market destek yayımlama**

|Destek kanal  |Açıklama  |
|---------|---------|
|E-posta: azurecertified@microsoft.com |   İş ortakları Azure Marketi'nde uygulama yayımlama için destek. İş saatleri Pasifik saat diliminde ' dir.      |
|E-posta: AzureMarketOnboard@microsoft.com |   Azure Marketi Çözüm Adaylığı form ve işlem için destek. İş saatleri Pasifik saat diliminde ' dir.      |
|E-posta: Amp-testdrive@microsoft.com |   Sürücüleri sınamak için hazırlanma erişimi'ni kullanın. İş saatleri Pasifik saat diliminde ' dir. | 

**Azure Market portal desteği**

|Destek kanal  |Açıklama  |
|---------|---------|
|E-posta: [desteği](https://go.microsoft.com/fwlink/?linkid=844975)    |   Market yayımlama portalında desteği. Her zaman, gece güne ve destek sağlanır.        |

**Teknik Destek**

|Destek kanal  |Açıklama  |
|---------|---------|
|MSDN Forumlarında: [Market](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=DataMarket)     | Microsoft Developer Network forum.         |
|Yığın taşması: [Azure](https://stackoverflow.com/questions/tagged/azure)     |    Yığın taşması ortam çözümleri almak ve her şeyi Azure ve Market ilgili hakkında sorular sormak için:<ul><li>[Azure Market](https://stackoverflow.com/questions/tagged/azure-marketplace)</li><li>[Azure Resource Manager](https://stackoverflow.com/questions/tagged/azure-resource-manager)</li><li>[Azure Sanal Makineler](https://stackoverflow.com/questions/tagged/azure-virtual-machine)</li></ul> |


**Pazarlama kaynakları**

|Destek kanal  |Açıklama  |
|---------|---------|
|E-posta: cosell@microsoft.com    |  Onboarding işlemleri ve ortak satış programı ile ilgili sorular için destek. Pasifik saat diliminde temel.        |
|E-posta: gtm@microsoft.com    |  Git Pazar avantajları ve program sorular için destek. İş saatleri Pasifik saat diliminde ' dir.        |
|E-posta: CEBrand@Microsoft.com     |  Azure logolar ve markalama kullanımı hakkındaki soruların yanıtları.       |


### <a name="go-to-market-benefits"></a>Git Pazar avantajları

Azure Market ve AppSource çözümleri müşterilerin milyonlarca göstermek yayımcılar sağlar. Yeni Market listelerinde farkındalığı teklifinizin bizim müşteri ekosistemi yardımcı olmak için ücretsiz Go Market avantajları kümesini otomatik olarak sunulur.

Market mağaza kullanımınız teklifinize listeleme Microsoft ve bir ortak satış hazır ortaklığı Fırsat için kapısı birleşik Go Market etkinlikler için Git'i olur. Tüm yeni listeleri no Go Market avantajları farkındalığı teklifinizin bizim müşteri ekosistemi yardımcı olmak için maliyet kümesini otomatik olarak sunulur. Pazara Çıkış avantajları, iş ortaklarına çözümlerinin tanınmasında ve satış sağlamalarında yardımcı olmak için markamızdan, kanallarımızdan ve ekosistemimizden yararlanmak üzere tasarlanmış çeşitli ortak pazarlama ve ortak satış etkinlikleri sunar. İş ortağının bir şey yapması gerekmez. Bir teklif yayımlanır yayımlanmaz, Pazara Çıkış ekibimiz teslim işlemini başlatmak üzere iş ortağına ulaşır.
Bizim GTM avantajları ve Market işinizde büyümeye yolları hakkında daha fazla bilgi için lütfen ziyaret [GTM avantajları MPN sitesinde](https://partner.microsoft.com/en-US/reach-customers/gtm).


### <a name="requirements-by-listing-type"></a>Liste türü tarafından gereksinimleri

#### <a name="prerequisites-for-marketplace-publishing"></a>Market yayımlama için Önkoşullar

**AppSource: Yayımlama seçenekleri önkoşulları**

|**Gereksinim**  |**Ayrıntılar**  |**Gerekli ya da önerilen**  |
|---------|---------|---------|
|**Azure Active Directory (AAD)**    |  Uygulamanız Azure Active Directory Federasyon çoklu oturum açmaya izin vermelidir (AAD Federasyon SSO) ile etkin onay. AAD etkinleştirme konusunda bilgi SSO Federasyon için buraya gidin     |    Gerekli   |
|**Microsoft bulut hizmetleriyle tümleştirme**    |  Uygulamanızı nesnelerin interneti gibi Microsoft Power BI, Cortana Intelligence ya da Microsoft Azure Hizmetleri gibi diğer Microsoft Cloud services ile tümleştirmeniz       |    Önerilen    |
|**Hedef kitle**    |  İş kullanıcılar ve işletme sahipleri için AppSource teklifleri olmalıdır     |    Gerekli   |
|**İş için bir hizmet (SaaS) uygulaması olarak yazılım**    |  Uygulamanızın olması gerekir: <ul><li>İş SaaS uygulama</li><li>İş sürecini odaklanmış</li><li>İşletme müşterileri için hedeflenmiş</li><li>Kullanıcılar kendi iş kimlik bilgileriyle (kullanıcı adı ve parola) oturum etkinleştir</li></ul>       |    Gerekli    |
|**Ücretsiz deneme süresi ve deneme sürümü deneyimi**    |  Bir müşteri uygulamanızı sınırlı bir süre için ücretsiz kullanmanız mümkün olması gerekir. Ücretsiz deneme sürümünüzü iki biçimlerden birini gerçekleştirebilirsiniz: <ul><li>Müşteriler deneme sürümünden AppSource içinde başlatabilmesi için bir "Deneme" seçeneği sağlar</li><li>İstek müşteriler "Deneme" AppSource içinde vardır</li></ul> Her iki yöntemde, uygulama için ek ücret ödemeden denemek için en az bir süre müşteri ücretsiz deneme sağlaması gerekir.      |    Gerekli    |
|**Kolayca yapılandırılabilir, anahtar teslimi çözümü**    |  Uygulamanızı kolay ve hızlı yapılandırıp (özelleştirme gerekli) kurulumu olmalıdır     |    Gerekli   |
|**Sağlama Yönetimi**    |  Marketten müşteri adayları almak için CRM (Marketo, Microsoft Dynamics veya Salesforce) sağlama verileri kabul etmek için etkinleştirmeniz gerekir.       |    Gerekli    |
|**Gizlilik ilkesini ve kullanım koşulları**    |  Gizlilik İlkesi genel bir URL ile kullanılabilir olması gerekir. Kullanım koşullarını metin olarak yayımlama sırasında girilmesi gerekir.     |    Gerekli   |
|**Destek**    |  Teklifiniz, müşterilerin Yardım bulabileceğiniz bir genel kullanıma açık destek URL'si eklemeniz gerekir. Denemeler için destek ve deneme süresi için hiçbir ek ücret sağlanmalıdır.     |    Gerekli   |

**Azure Market: seçenekleri yayımlamak için Önkoşullar**

|**Gereksinim**  |**Ayrıntılar**  |**Yayımlama seçeneği**  |
|---------|---------|---------|
|**Katılım ilkeleri**    | Azure Market gözden [katılım ilkeleri](https://azure.microsoft.com/support/legal/marketplace/participation-policies/).       | Liste, deneme, işlem        |
|**Microsoft ile tümleştirme**    | Azure Market sunar kullanın veya Microsoft Azure hizmet türleri işlem, ağ veya depolama gibi genişletir. Bunlar, var olan bir Azure Marketi kategori veritabanları, güvenlik ve ağ gibi uyumlu olmalıdır. Bkz: [tam listesi](https://azuremarketplace.microsoft.com/marketplace/apps).        | Liste, deneme, işlem        |
|**Hedef kitle**    | Azure Market sunar, BT uzmanları, bulut geliştiriciler veya diğer teknik müşteri rolleri olması gerekir.       |  Liste, deneme, işlem 
|**Sağlama Yönetimi**    | Marketten müşteri adayları almak için CRM (Marketo, Microsoft Dynamics veya Salesforce) sağlama verileri kabul etmek için etkinleştirmeniz gerekir.        |   Liste, deneme, işlem      |
|**Gizlilik ilkesini ve kullanım koşulları**     |   Gizlilik İlkesi genel bir URL ile kullanılabilir olması gerekir. Kullanım koşullarını metin olarak yayımlama sırasında girilmesi gerekir.      |   Liste, deneme, işlem      |
|**Destek**     |  Teklifiniz, müşterilerin Yardım bulabileceğiniz bir genel kullanıma açık destek URL'si eklemeniz gerekir. Denemeler için destek ve deneme süresi için hiçbir ek ücret sağlanmalıdır.       |  Deneme, işlem       |

#### <a name="prerequisites-specific-to-trial-publishing"></a>Önkoşullar özel deneme yayımlama

|**Gereksinim**  | **Ayrıntılar**  |**Yayımlama seçeneği**  |
|---------|---------|---------|
|**Ücretsiz deneme süresi ve deneme sürümü deneyimi**     |  Bir müşteri uygulamanızı sınırlı bir süre için ücretsiz kullanmanız mümkün olması gerekir.<br><br>Bu, müşteri lisans veya Abonelik ücretlerinin ürününüzü ya da temel alınan Microsoft birinci taraf ürün veya hizmetle maliyetini tabi olmaz anlamına gelir. Tüm deneme seçenekleri publisher'ın Microsoft Ürün aboneliğine dağıtılan olduğundan yayımcı yalnızca deneme maliyet iyileştirmesi ve yönetim denetler.<br><br>Ücretsiz deneme sürümü, etkileşimli demo seçin veya sürücü testi. Seçtiğiniz öğeye olsun, ücretsiz deneme müşteri uygulaması için ek ücret ödemeden denemek için en az bir süre sağlaması gerekir.<br><br>Sınamayı oluşturma işlemine başlamak için ulaşmak amp-testdrive@microsoft.com. <br><br>Azure Market SaaS deneme karşılaştığında Not kendi Active Directory iş kimlik bilgileriyle oturum açmasını izin vermeniz gerekir. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-appsource-certified#appsource-trial-experiences). |   Deneme      | 
| **Kolayca yapılandırılabilir, anahtar teslimi çözümü**    |  Uygulamanızı yapılandırmak ve ayarlamak hızlı ve kolay olmalıdır.       |  Deneme       |
|**Kullanılabilirlik/açık kalma süresi**    |    SaaS uygulaması veya platform en az % 99,9 çalışma süresi olması gerekir.     |    Deneme     |
|**Azure Active Directory**    |    Teklifiniz etkin izin ile çoklu oturum açma (SSO) Azure Active Directory (Azure AD) federe izin vermeniz gerekir.      |  Deneme|

#### <a name="prerequisites-specific-to-transaction-publishing"></a>Önkoşullar özel işlem yayımlama

|**Gereksinim**  |**Ayrıntılar** |**Yayımlama seçeneği**  |
|---------|---------|---------|
|**Faturalama ve ölçümü**    |  Sanal makineniz kendi lisansını Getir veya kullanım tabanlı, aylık faturalama desteklemesi gerekir.       |    İşlem    |
|**Azure ile uyumlu sanal sabit disk (VHD)**     |   Sanal makineler yerleşik, [Windows](https://docs.microsoft.com/azure/marketplace-publishing/marketplace-publishing-vm-image-creation) veya [Linux](https://docs.microsoft.com/azure/marketplace-publishing/marketplace-publishing-vm-image-creation).    |   İşlem      |

#### <a name="prerequisites-specific-to-transaction-publishing-for-containers"></a>İşlem yayımlama kapsayıcıları için Önkoşullar özgü


|**Gereksinim**  |**Ayrıntılar** |**Yayımlama seçeneği**  |
|---------|---------|---------|
|**Faturalama ve ölçümü**   |  Kapsayıcı ya da desteklemelidir boş veya faturalama modelleri kendi lisansını getir.       |  İşlem       |
|**Docker tabanlı görüntü**    |   Kapsayıcı görüntüleri Docker görüntü biçimi dayalı ve Azure kapsayıcı defterlerinden çekilen gerekir.      |  İşlem       |

#### <a name="prerequisites-specific-to-transation-publishing-for-saas-app-subscriptions"></a>Önkoşullar özel SaaS uygulama abonelikler için işlem yayımlama

|**Gereksinim**  |**Ayrıntılar** |**Yayımlama seçeneği**  |
|---------|---------|---------|
|**Faturalama ve ölçümü**    |   Teklifiniz aylık bir düz hızında fiyatlandırılır. Kullanım tabanlı fiyatlandırma ve kullanım tabanlı "true li" özellikleri şu anda desteklenmiyor.      |   İşlem      |
|**İptali**  |   Herhangi bir zamanda müşteri tarafından iptal edilebilen teklifidir.      |   İşlem      |
|**İşlem giriş sayfası**     |   Azure ortak markalı işlem giriş sayfası, kullanıcıların oluşturmak ve SaaS hizmet hesaplarını yönetmek bir ana bilgisayar.      |    İşlem     |
|**SaaS abonelik API**    |   SaaS oluşturmak, güncelleştirmek ve bir kullanıcı hesabı ve hizmet planını silmek için abonelik ile etkileşim kurabilen bir hizmeti kullanıma sunar. 24 saat içinde desteklenen kritik API değişiklikleri gerekir. Kritik olmayan API değişiklikleri düzenli olarak kullanıma sunulacaktır.      |     İşlem    |

### <a name="prerequisites-specific-to-consulting-services-publishing"></a>Danışmanlık için Önkoşullar belirli hizmetleri yayımlama

|**Gereksinimleri** |**Ayrıntılar**  |**Yayımlama seçeneği**  |
|---------|---------|---------|
|**Hizmet teklif özelliklerini**     | Danışmanlık hizmetinizi olması gerekir: <br>-Sabit kapsam, sabit süre, sabit fiyat (veya boş) katılım teslim. <br>-Yönelik öncelikle satış öncesi. <br>-Tek bir müşteri sınırlıdır. <br>-Sitesinde yürütülür.        |    Liste     |
|**Danışmanlık Hizmetleri için iş ortağı gereksinimleri**    |   *Yalnızca AppSource*:  <br>- **Dynamics 365 müşteri katılım için**: sahip Gümüş veya altın [bulut Müşteri İlişkileri Yönetimi](https://partner.microsoft.com/en-us/membership/cloud-customer-relationship-management-competency) uzmanlığı. <br>- **Dynamics 365 Finans ve işlemleri Enterprise edition için**: sahip Gümüş veya altın [kurumsal kaynak planlaması](https://partner.microsoft.com/en-us/membership/enterprise-resource-planning-competency) uzmanlığı ve 25.000 bulut işlemleri sonunda 12 ay içinde en az bir gelir. <br>- **Dynamics 365 Finans ve işlemleri, Business edition**: olarak hizmet [bulut Hizmetler Sağlayıcısı (CSP)](https://partner.microsoft.com/en-us/cloud-solution-provider) veya [dijital iş ortağı, kaydı (DPOR)](https://partner.microsoft.com/en-us/membership/digital-partner-of-record) en az bir müşteri için. <br>- **Power BI**: [çözüm ortağı] (file:///C:/Users/ellacroi/Downloads/BI%20Partner%20Program%20Overview%20 & % 20Incentives.pdf) ölçütlere uyan. <br>- **PowerApps**: sahip bir [iş ortağı gösterimi](https://powerapps.microsoft.com/en-us/partner-showcase/) çözümü. |    Liste     |
|**Danışmanlık Hizmetleri için iş ortağı gereksinimleri**    |  *Yalnızca Azure Market*: <br>İş ortakları için de gereklidir bir **Gümüş veya gold uzmanlığına** kendi hizmet ilgili alanda. Uygun yetkinlikleri aşağıda listelenmiştir:</br><br><ul><li> **Uzmanlığı:** bulut platformu ve altyapı. <br>**Çözüm alanı:** bulut platformu, veri merkezi</li><br><li>**Uzmanlığı:** uygulama geliştirme ve ISV <br>**Çözüm alanı:** uygulama geliştirme, uygulama tümleştirmesi, DevOps</li><br><li>**Uzmanlığı:** veri yönetimi ve analizi <br>**Çözüm alanı:** veri analizi, bir veri platformu </li></br></ul>Daha fazla bilgi için bkz: [Microsoft iş ortağı ağı üzerinden yetkinlikleri](https://partner.microsoft.com/en-US/membership/competencies).</br><br>Listeleme hakkında daha fazla bilgi için bkz: [Azure Marketi Danışmanlık Hizmetleri](https://docs.microsoft.com/en-us/azure/marketplace/consulting-services) |    Liste     | 


#### <a name="cloud-partner-portal-pre-publishing-checklist-for-the-azure-marketplace"></a>Denetim listesi için Azure Market önceden yayımlama bulut iş ortağı portalı

Yayımlama işlemi başlamadan önce bir teklifi oluşturmak için gerekli bileşenleri anlamak faydalıdır. Aşağıdaki yapılar oluşturma teklif yayımlama akışında bulut iş ortağı Portalı'nı tamamlamak için gereklidir. 

**StoreFront ayrıntıları**

|Bu yayımlama yapı gerekir  |Bu teklif türü için  |
|---------|---------|
|**(200 karakterden) ad ve Açıklama (2.000 karakter) sunma**    |  Tümü        |
|**Microsoft iş ortağı ağı (MPN) kimliği**   |  Tümü       |
|**Ülke/bölge kullanılabilirliği**   | Tümü        |
|**Katılım süresi**     |   Danışmanlık Hizmetleri      |
|**Geçerli sektörü, kategoriler ve arama anahtar sözcükleri**     |  Tümü       |
|**Şirket logoları (48 x 48, 216 x 216)**     |  Danışmanlık Hizmetleri       |
|**Ürün genel bakış videosu (isteğe bağlı)**  |  Tümü       |
|**Ekran görüntüleri (en fazla 5, 1280 x 720)**   |    Tümü     |
|**Pazarlama belgeleri (en fazla 3)**    |  Tümü       |
|**Hedef yol**    |   Tümü      |

**Kişiler**

|Bu yayımlama yapı gerekir  |Bu teklif türü için  |
|---------|---------|
|**Bilgi (desteği, mühendislik, ticari) başvurun**    |    Tümü     |


**Teknik bilgi**

|Bu yayımlama yapı gerekir  |Bu teklif türü için |
|---------|---------|
|**Deneme URL'si**     |  Tüm deneme teklifi türleri       |
|**Desteklenen diller**    |   Tüm deneme teklifi türleri      |
|**Uygulama sürüm numarası ve yayın tarihi**    |   Tüm deneme teklifi türleri      |
|**Destek URL'si**    |   Tüm deneme teklifi türleri, sanal makineler      |
|**Kullanım ve gizlilik ilkesi URL'si koşulları**     |    Tümü     |


**Test sürücü**

|Bu yayımlama yapı gerekir  |Bu teklif türü için  |
|---------|---------|
|**Açıklama ve süresi**     |  Yalnızca test sürücü       |
|**Kullanıcı el ile**     |   Yalnızca test sürücü      |
|**Test sürücü video (maksimum 1)**     |  Yalnızca test sürücü       |
|**Sınama sürücü ülke/bölge kullanılabilirliği**    |   Yalnızca test sürücü      |
|**Azure kaynak grubu adı**   |         |
|**Azure abonelik kimliği**     |  Yalnızca test sürücü       |
|**Azure AD Kiracı kimliği**   |    Yalnızca test sürücü     |
|**Azure AD uygulama kimliği**  |  Yalnızca test sürücü       |
|**Azure AD uygulama anahtarı**     |   Yalnızca test sürücü      |


**Mağaza/Market**

|Bu yayımlama yapı gerekir  |Bu teklif türü için  |
|---------|---------|
|**Title (maximum 50 characters)**    |  İşlem: sanal makineler, Azure uygulamaları (Çözüm şablonları ve yönetilen uygulamalar), kapsayıcıları, SaaS abonelikleri       |
|**Özet (en fazla 200 karakter)**    |  İşlem: sanal makineler, Azure uygulamaları (Çözüm şablonları ve yönetilen uygulamalar), kapsayıcıları, SaaS abonelikleri       |
|**Uzun özeti (en fazla 256 karakter)**     |   İşlem: sanal makineler, Azure uygulamaları (Çözüm şablonları ve yönetilen uygulamalar), kapsayıcıları, SaaS abonelikleri      |
|**HTML tabanlı açıklaması (en fazla 3000 karakter)**    |  İşlem: sanal makineler, Azure uygulamaları (Çözüm şablonları ve yönetilen uygulamalar), kapsayıcıları, SaaS abonelikleri      |
|**Şirket logoları (40 x 40, 90 x 90, 115 x 115, 255 x 115, 815 x 290)**    |  İşlem: sanal makineler, Azure uygulamaları (Çözüm şablonları ve yönetilen uygulamalar), kapsayıcıları, SaaS abonelikleri     |


**SKU**

|Bu yayımlama yapı gerekir  |Bu teklif türü için  |
|---------|---------|
|**Sürüm numarası**     |    İşlem: Azure uygulamaları (Çözüm şablonları ve yönetilen uygulamalar)     |
|**Tüm şablon dosyalarını ve createUIDefinitionFile içeren paket dosyası**   |İşlem: Azure uygulamaları (Çözüm şablonları ve yönetilen uygulamalar)         |
|**İşletim sistemi ayrıntıları**    |   İşlem: sanal makineler      |
|**Bağlantı noktalarını ve protokolleri kullanımda**    |  İşlem: sanal makineler       |
|**Disk sürümü ve Kullanımdaki her VHD için SAS URL'si**   |  İşlem: sanal makineler       |
|**Abonelik kimliği, kaynak grubu adı, kayıt defteri adı, havuz adı, kullanıcı adı, parola ve görüntü etiketler (isteğe bağlı) dahil olmak üzere azure kapsayıcı kayıt defteri (ACR) görüntü deposu ayrıntıları** | İşlem: kapsayıcıları |


#### <a name="using-azure-active-directory-to-enable-trials"></a>Denemeler etkinleştirmek için Azure Active Directory kullanma
Azure Active Directory olduğu kimlik doğrulamasına olanak tanıyan bir bulut kimliği hizmeti ile bir Microsoft iş veya Okul hesabı endüstri standardı protokolleri kullanılarak: OAuth ve Openıd Connect. Azure AD hakkında daha fazla bilgi almak [ürün Web sayfası](https://www.microsoft.com/en-us/cloud-platform/azure-active-directory-features). 

Microsoft Azure AD ile tüm Market kullanıcıların kimliğini doğrular. Kimliği doğrulanmış bir kullanıcı markette listeleme denemenizi aracılığıyla tıklar ve deneme ortamınıza yönlendirilir, ek bir oturum açma adımı gerek kalmadan doğrudan bir deneme sürümü'nü kullanıcı sağlayabilirsiniz. [Uygulamanızı Azure AD kimlik doğrulama sırasında alan belirteci](https://docs.microsoft.com/azure/active-directory/develop/active-directory-token-and-claims#sample-tokens) uygulamanızda bir kullanıcı hesabı oluşturmak için kullanabileceğiniz değerli kullanıcı bilgilerini içerir. Ardından, sağlama deneyimi otomatikleştirmek ve dönüştürme olasılığını artırmak. 

Azure AD kullanarak uygulamanızı veya deneme için tek tıklamayla kimlik doğrulamasını etkinleştirmek için:

- Market müşteri deneyimine deneme kolaylaştırır. 
- Kullanıcının etki alanı ya da deneme ortamınıza marketten bile yönlendirildiği zaman bir ürün deneyimi yapısını korur.
- Hiçbir ek oturum açma adımı olduğundan yeniden yönlendirme üzerinde abandonment olasılığını azaltır.
- Azure AD kullanıcıları büyük popülasyonunu dağıtım engelleri azaltır.

**Market, Azure AD tümleştirme Onayla: çok müşterili uygulamalar**

Azure AD bugün destekliyorsa:

- Uygulamanızı Azure portalında kaydedin.
- Çoklu müşteri mimarisi desteği özelliği, bir tek tıklamayla deneme sürümü deneyimi almak için Azure AD'de etkinleştirin.
- [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications).

Azure AD Federasyon SSO için yeni varsa:

- Uygulamanızı Azure portalında kaydedin.
- Azure AD ile SSO kullanarak geliştirmek [Openıd Connect](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-openid-connect-code) veya [OAuth 2.0](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-oauth-code).
- Çoklu müşteri mimarisi desteği özelliği, bir tek tıklamayla deneme sürümü deneyimi almak için Azure AD'de etkinleştirin.
- [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-appsource-certified).

**Market, Azure AD tümleştirme Onayla: tek Kiracı uygulamaları**

Tek Kiracı uygulamalar için birden çok seçenek vardır:

- Kullanıcılar, kullanarak Konuk kullanıcılar olarak dizininize eklemek [Azure B2B](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b).
- El ile sağlama denemeler kişi Me aracılığıyla müşteriler için
- Müşteri başına sınamayı geliştirin.
- SSO çok müşterili örnek bir tanıtım uygulaması oluşturun.


### <a name="become-a-cloud-marketplace-publisher"></a>Bir bulut Market yayımcı olur

#### <a name="becoming-a-publisher"></a>Bir yayımcı olma

Bu bölümde, adımları açıklanır: <ul><li>Azure Market ve AppSource Publisher'da haline gelir.</li><li>Bulut iş ortağı portalına erişin. Yapı, yayımlama ve teklifiniz korumak için bu portalı kullanacaksınız.</li></ul>

**İşlemine genel bakış**

|Market kayıt adımı  |Zaman  |Açıklama  |
|---------|---------|---------|
| 1. Microsoft iş ortağı ağı kaydet | 15 dakika | Resmi bir Microsoft iş ortağı haline gelir ve başka avantajları almak ve bir Azure Market yayımcı olabilme desteği için Microsoft Partner Network (MPN) katılın. MPN kaydetmek için lütfen Microsoft iş ortağı ağı ziyaret edin ve "Kaydet"'i tıklatın Kuruluşunuzun varolan bir üyelik, (varsa), kayıt sırasında katılmak kuramaz. Kaydedildikten sonra kuruluşunuzun MPN Kimliğini not alın: Yayımcı profilinizde bulut iş ortağı portalını (adım 3) etkinleştirmek de isteyeceğiz.      |
|2. Microsoft kimlik oluşturun     |   15 dakika      |  Bu Microsoft ID bulut iş ortağı portalına erişmek için kullanılır. Bu e-posta adresi a Microsoft ID kayıtlı olması gerekir ve hem bulut iş ortağı Portalı'nı (adım 3) hem de Microsoft Developer Center'da (4. adım) için kullanılır. Seçilen e-posta adresi şirket etki alanınızda tercihen olmalıdır ve BT ekibi tarafından denetlenir. Bir kimliği oluşturmadan önce yönergeleri için kılavuzları ve nasıl yapılır bölümleri gözden geçirin. |
|3. Market Adaylığı form gönderme     |  1-3 gün       | Kuruluşunuzun bir Microsoft bulut Market yayımcı olmasını belirler. Form kuruluşunuz, ilk uygulama veya hizmet teklifi yayımlamak istediğiniz ve düzeyde sağlayacağınız desteği hakkında bilgi içerir. <ul><li>[Azure Market Adaylığı formu](http://aka.ms/listonazuremarketplace)</li><li>[AppSource Adaylığı formu](http://aka.ms/listonappsource)</li></ul> Formu gönderdikten sonra Market Ekibi uygulama gözden geçirin ve istek doğrulayın. İstek inceledikten sonra onaylanmış bir iş ortağı olmadan için sonraki adımda e-postayla bildirilecek ve bulut iş ortağı portalına erişim, burada ilk teklif listenizi tamamlamak ve ek oluşturma sunar. Onaylanırsa, aynı zamanda Microsoft Development Center (4. adım) kayıt ücret feragat etmiş bir promosyon kodu alırsınız. |
|4. Geliştirici Merkezi'nde kaydetme     |    5-10 gün     | Microsoft Developer Center'da Market'te sanal makineler, çözüm şablonları ve Azure ile yönetilen uygulamalar gibi özellikleri sahip olan uygulamalar yayımlamayı transact için gereklidir. Bu gereksinim, şirket bilgileri şirketinizin yasal, vergi ve varlıkları bankacılık doğrulamak Microsoft izin verir. Registrant geçerli temsilcisi kuruluşun olmalıdır ve kimliğini doğrulamak için kişisel bilgileri sağlamanız gerekir. Kaydetme kişi a Microsoft şirket için paylaşılan ID (2. adım) kullanmanız gerekir ve aynı hesabı bulut iş ortağı Portalı'nda kullanılmalıdır. <ul>Market Adaylığı form tamamladıysanız henüz 99 kayıt ücret ödemeniz istenir, olduğunu unutmayın. Kullanıma başlama öncesi bu ücret sağlamak için Market Adaylığı formu doldurun ve e-posta yoluyla bir promosyon kodu alırsınız. Önemli: bir Microsoft Developer Center'da hesabı oluşturmayı denemeden önce şirketinizin zaten olmayan emin olun. Bu işlemi adım adım açıklama için geliştirici Merkezi'nde kaydetme hakkında yönergeler bakın.</ul>   |
|5. Bulut iş ortağı portalında oturum açın     |  15 dakika       |  Adaylığı onaylandıktan sonra kaydettiğiniz [Microsoft iş ortağı ağı](https://partner.microsoft.com/en-us/membership/) ve [Microsoft Developer Center'da](https://dev.windows.com/), erişmek için bir hesap oluşturulacak [bulut İş ortağı portalı](https://cloudpartner.azure.com/). İlk kez oturum açma kimlik bilgileri Adaylığı onay e-postayla dahil edilir. Bulut iş ortağı portalının nasıl kullanılacağı hakkında ayrıntılı bilgi için Git [öğrenin](https://cloudpartner.azure.com/#Learn) bir belge bölümü gözden geçirin ve portal menüsünde.    |


#### <a name="guidelines-and-how-tos"></a>Kılavuzları ve nasıl yapılır?

**Bir Azure Marketi hesabını yönetmek için a Microsoft ID oluşturma yönergeleri**

Bir şirket hesabı oluştururken, hesap açılan Microsoft hesabı oturum açarak hesabınıza erişmek birden çok kişi gerekecekse bu yönergeleri izleyin.

>[!IMPORTANT]
>Birden çok kullanıcıların Geliştirici Merkezi hesabınızda erişmesine izin vermek için Azure Active Directory rolleri bireysel kullanıcılara atamak için kullanmanızı öneririz. Bu kullanıcılar hesap oturum bireysel oturum açarak erişebilir Azure AD kimlik. Daha fazla bilgi için bkz: [Microsoft IDs kılavuzunda Azure AD etki alanı Federasyon](#guidance-for-microsoft-ids-in-an-azure-ad-federated-domain). Microsoft hesabınız, şirketinizin etki alanı, ancak tek bir ait bir e-posta adresi kullanarak oluşturun. windowsapps@fabrikam.com bunun bir örneğidir.

- Bu Microsoft hesabını geliştiricileri en küçük olası sayısına erişimi sınırlayın.
- Geliştirici hesabını erişmesi gereken herkes içeren bir şirket e-posta dağıtım listesini ayarlamak ve güvenlik bilgilerinizi bu e-posta adresi ekleyin. Bu listede gerektiğinde güvenlik kodlarını almak ve Microsoft hesabınızın güvenlik bilgilerini yönetmek için tüm çalışanlar sağlar. Bir dağıtım listesi ayarlama uygun değilse, tek tek e-posta hesabının sahibi erişmek ve istendiğinde güvenlik kodu paylaşmak kullanılabilir olması gerekir. (Yeni güvenlik bilgileri hesabına veya yeni bir CİHAZDAN erişilmelidir eklendiğinde, örneğin, sahibi sorulur.)
- Uzantı gerektirmez ve anahtar takım üyeleri için erişilebilir olan bir şirket telefon numarası ekleyin.
- Genel olarak, şirketinizin Geliştirici hesabınızda oturum açmak için güvenilen cihazları kullanın geliştiriciler vardır. Tüm anahtar ekip üyelerinin bu güvenilen cihazlara erişimi olmalıdır. Bu erişim birisi hesabına erişirken gönderilmek üzere güvenlik kodlarını gereksinimini azaltır.
- Güvenilir olmayan bir Bilgisayardan hesabına erişime ihtiyacınız varsa, en fazla beş geliştiriciler bu erişimi sınırlayın. İdeal olarak, bu geliştiriciler hesabına erişmek ve bu da aynı coğrafi paylaşan ve ağ konumu makinelerden.
- Şirketinizin güvenlik bilgileri sık gözden [Yönetim sayfasında](https://account.live.com/proofs/Manage) tüm geçerli olduğundan emin olmak için.

Geliştirici hesabınızda öncelikle Güvenilen bilgisayarlarından erişilmesi. Hesap hafta başına oluşturulan kodlarını sayısına bir sınır olduğundan bu önemlidir. Güvenilen bilgisayar kullanarak, en sorunsuz oturum açma deneyimi sağlar.

Ek Geliştirici hesabı yönergeleri ve güvenlik hakkında daha fazla bilgi için bkz: [bir geliştirici hesabını açma](https://docs.microsoft.com/windows/uwp/publish/opening-a-developer-account).

### <a name="guidance-for-microsoft-ids-in-an-azure-ad-federated-domain"></a>Bir Azure AD Federasyon etki alanındaki Microsoft IDs Kılavuzu

Şirket hesabınızı üzerinden federe [Azure Active Directory](https://docs.microsoft.com/azure/active-directory/). Şirket e-posta adresiyle a Microsoft ID oluşturmayı denerseniz hata döndürür. İlk olarak bir hata alırsanız, bu durumda olduğundan emin olmak için BT ekibiniz denetleyin. Bu bilinen bir sorundur ve çözümlemek için çalışıyoruz. 

Geçici bir çözüm olarak, yeni bir e-posta adresi oluşturun olan öneririz @outlook.com etki alanı ve bir kural oluşturun. Şu adımları uygulayın:

1. Git [kaydolma sayfasına](https://signup.live.com/signup) seçip **yeni bir e-posta adresi alın**.


2. Yeni bir e-posta adresi oluşturun ve bir parola girin. Bu adım outlook.com hizmetinde yeni a Microsoft ID ve bir e-posta kutularına oluşturur. Hesap oluşturulduktan kadar kayıt işlemine devam edin.

   >[!IMPORTANT]
   >Geliştirici Merkezi'nde kayıt için kullanacağınız e-posta kimliği veya dağıtım listesi bir Microsoft hesabı olarak kayıtlı olduğundan emin olun. (Kişiler tarafından gönderilen bağımlılığı kaldırmak için bir dağıtım listesi öneririz.) Aksi durumda, [kaydettirmek](https://signup.live.com/signup?uaid=e479342fe2824efeb0c3d92c8f961fd3&lic=1).
   >
   >Ayrıca, *herhangi bir e-posta kimliği Microsoft şirket etki alanı altında* Geliştirici Merkezi kaydı için kullanılamaz.

3. Bu Hesapla Microsoft ID oluşturduktan sonra hesabınızda oturum açın [posta kutusu](https://outlook.live.com/owa/). [Kural ileten bir e-posta oluşturma](https://support.office.com/en-us/article/Use-rules-in-Outlook-Web-App-to-automatically-forward-messages-to-another-account-1433e3a0-7fb0-4999-b536-50e05cb67fed) tüm e-postaları Azure Marketi hesabını yönetmek için etki alanında oluşturulan e-posta adresi bu posta kutusuna taşımak için.

Son adımı tamamladıktan sonra Outlook e-posta hesabınıza etki alanınızdaki Microsoft ID tüm e-postaları/iletişimler gönderir. Kullanmanız gereken @outlook.com e-posta adresi Microsoft Developer Center hem bulut iş ortağı portalını kimlik doğrulaması.

### <a name="instructions-on-how-to-register-in-the-developer-center"></a>Geliştirici Merkezi'nde kaydetmek yönergeler

1. Yeni bir Internet Explorer InPrivate veya Incognito gözatma oturumu Chrome, kişisel hesabına açmadınız emin olmak için açın.
2. Git [Geliştirici Merkezi](http://dev.windows.com/registration?accountprogram=azure) kendiniz bir satıcı olarak kaydetmek için.
3. Tamamlamak **hesabınızı korumamıza yardımcı olun** Sihirbazı'nı, ama telefon numarası veya e-posta adresi aracılığıyla kimliğinizi doğrular.

   !["Hesabınızı korumamıza yardımcı olun" Sihirbazı'nda telefon bilgisi kutuları](./media/marketplace-publishers-guide/registerdevcenteremail.png)

4. İçinde **kayıt hesap bilgisi** bölümünde, aşağı açılan menüden hesabı ülke/bölge seçin.

   ![Ülke/bölge için kutusuyla hesabı bilgileri](./media/marketplace-publishers-guide/devcenterregistrationaccountinfo.png)
   
   >[!WARNING]
   >Hizmetlerinizi Azure Market'te satmak için kayıtlı varlığınız onaylanan "Satış Yeri" ülkelerde birinden olduğundan emin olun. Ödeme ve vergi nedeniyle kısıtlamadır. Daha fazla bilgi için bkz: [Market katılım ilkeleri](https://azure.microsoft.com/support/legal/marketplace/participation-policies/).

5. İçin **hesap türü**seçin **şirket** ve ardından **sonraki** düğmesi.

   >[!IMPORTANT]
   >Hesap türleri ve seçmek için en iyi olduğu daha iyi anlamak için bkz: [hesap türlerini, konumlarını ve ücretleri](https://docs.microsoft.com/windows/uwp/publish/account-types-locations-and-fees).

6. İçin **yayımcı görünen adı**, görünen ad (tipik olarak, şirketinizin adı) girin.

   >[!TIP]
   >Teklifiniz listelenen sonra Geliştirici Merkezi'ne girilen yayımcı görünen ad Azure Marketi'nde görüntülenmez. Ancak, kayıt işlemini tamamlamak için bu kutuyu doldurmanız gerekir.

7. İçin **kişi bilgisi**, hesap doğrulamayı bilgilerini girin.

   >[!IMPORTANT]
   >Doğru kişi bilgilerini sağlamanız gerekir. Doğrulama işlemi Geliştirici Merkezi'nde, şirketinizin onaylamak için kullanır.

8. İçin **şirket onaylayanı**, onaylayan kişi bilgilerini girin. Onaylayan bir hesap Geliştirici Merkezi'nde kuruluşunuz adına oluşturma yetkiniz doğrulayabilirsiniz kullanıcıdır. Seçin **sonraki** bitirdiğinizde.

   ![Vurgulanmış bölümleri "hesap bilgisi" sayfası](./media/marketplace-publishers-guide/devcenterregistrationpayment.png)

9. Hesabınız için ödeme yapmak için Ödeme bilgilerinizi girin. Kayıt maliyetini kapsayan bir promosyon kodu varsa, burada girebilirsiniz. Aksi takdirde, kredi kartı bilgilerini veya desteklenen pazarda PayPal bilgileri sağlar. İşiniz bittiğinde seçin **sonraki**.

   ![Geliştirici Merkezi ödeme bilgileri](./media/marketplace-publishers-guide/devcenterregistrationpayment2.png)

10. Hesap bilgilerinizi gözden geçirin ve her şeyin doğru olduğundan emin olun. Ardından, okuyun ve hüküm ve koşulları Microsoft Azure Market yayımcı anlaşmasının kabul edin. Okuma ve bu koşulları kabul gösteren kutuyu seçin.
11. Seçin **son** kaydınızı doğrulamak için. E-posta adresinizi bir onay iletisi göndereceğiz.
12. Yalnızca boş teklifleri yayımlama planlıyorsanız seçin **bulut iş ortağı Portalı'na Git**.

    Ticari (işlem) teklifleri--yayımlama planlıyorsanız, örneğin, sanal makine saatlik bir fatura modeliyle--sunar seçin **hesap bilgilerinizi güncelleştirmeniz**. Ardından, Banka ve vergi bilgileri Geliştirici Merkezi hesabınızda sonraki bölümde açıklandığı gibi doldurmanız gerekir.

#### <a name="add-bank-and-tax-information"></a>Banka ve vergi bilgilerini ekleme

Ticari teklifleri satın alma için yayımlamak isterseniz, ödeme ekleyin ve bilgi vergi ve geliştirici Merkezi'nde doğrulama için gönderme gerekir. Yayımlarsanız yalnızca boş veya KLG sunar, bu bilgileri eklemeniz gerekmez. Daha sonra ekleyebilirsiniz, ancak vergi bilgileri doğrulamak için biraz zaman alabilir. Ticari teklifleri satın alma için sunacaktır biliyorsanız, mümkün olan en kısa sürede eklemenizi öneririz.

>[!IMPORTANT]
>Ticari (işlem) tekliflere banka ve vergi bilgilerini tamamlanana kadar üretime Teklifleriniz gönderemezsiniz.

Banka bilgilerini eklemek için:
1. Microsoft Developer Center Microsoft hesabınızla oturum açın.
2. Seçin **ödeme hesap** sol menüde. Altında **ödeme yöntemi seçin**seçin **banka hesabı** veya **PayPal**.

   >[!IMPORTANT]
   >Müşterilerin Market satın alması ticari teklifleri varsa, bu bu satın alma işlemleri için ödeme almanıza hesabıdır.

3. Ödeme bilgileri girin ve seçin **kaydetmek** memnun olduğunuzda.

   >[!IMPORTANT]
   >Güncelleştirme veya ödeme hesabınızda değişiklik, aynı yukarıdaki adımları izleyin, ancak yeni bilgilerle geçerli bilgilerini değiştirmek gerekiyorsa. Ödeme hesabınızı değiştirme, ödemeler tek bir ödeme döngüsü tarafından gecikmeye yol açabilir. Bu gecikme, ödeme hesabınızı ilk ayarladığınızda komutlarında yaptığımız gibi hesabı değişikliğini doğrulamak ihtiyacımız oluşur. Hesabınızı doğrulandıktan sonra devam ödenen için tam tutar. Son ödeme geçerli ödeme için bir döngü eklenir.

4. **İleri**’yi seçin. 

Vergi bilgilerini eklemek için:

1. Oturum [Microsoft Developer Center'da](https://dev.windows.com) Microsoft hesabınıza (gerekirse).
2. Seçin **vergi profili** sol menüde.
3. Üzerinde **, vergi formu ayarlama** sayfasında ülke veya bölge kalıcı residency olduğu seçin ve ardından ülke veya bölge birincil vatandaşlığa benzer tutun yeri seçin. **İleri**’yi seçin.
4. Vergi ayrıntılarını girin ve ardından **sonraki**.

Geliştirici Merkezi kayıt sorunları varsa bir destek bileti oturum:

1. Git [destek sayfası](https://developer.microsoft.com/windows/support).
2. Altında **bize**seçin **bir olay gönderme** düğmesi.

   !["Bir olay Gönder" düğmesi](./media/marketplace-publishers-guide/devcentersubmitincident.png)

3. Seçin **Yardım Dev Center'a** olarak **sorun türü**seçip **Yayımla ve uygulamaları yönetme** olarak **kategori**. Bundan sonra seçin **Başlat e-posta** düğmesi.   
4. Oturum açma sayfasında oturum açmak için herhangi bir Microsoft hesabı kullanın. Bir Microsoft hesabınız yoksa, bağlantıyı kullanarak bir tane oluşturun. 
5. Sorun ayrıntıları doldurun ve seçerek bilet gönderme **gönderme** düğmesi.

#### <a name="billing-options"></a>Faturalama seçenekleri

**Market ticari konuları**

Market katılan için hiçbir ücretleri vardır. Liste, deneme ve KLG hareket seçeneklerini kullanarak yayımlama sırasında markette katılan için geliri paylaşımı yok. Daha fazla bilgi için bkz: [Market katılım ilkeleri](https://azure.microsoft.com/support/legal/marketplace/participation-policies/).

**Kullandıkça Öde ve kendi lisans faturalama seçeneklerini Getir**

Kullandıkça Öde işlem yayımlama seçeneği olarak kullandığınızda, lisans gelir: kullanım tabanlı yazılımınızı % 80 / % 20 arasında Microsoft, sırasıyla paylaşılan. Tek bir teklif Kullandıkça Öde ve kendi lisansını Getir faturalama modelleri fiyatlandırılır ve teklif düzeyinde ayrı SKU'ları bulunabilir. Bu, teklifte bulut iş ortağı portalında yapılandırabilirsiniz.

Aşağıdaki örnek göz önünde bulundurun.

İsteğe bağlı olarak Kullandıkça Öde etkinleştirirseniz:


|Maliyet lisansı   | saat başına 1.00 $        |
|---------|---------|
|Azure kullanım maliyeti (1/D1-çekirdek)     | saat başına 0.14  |
|**Müşteri Microsoft tarafından faturalandırılır**    | **saat başına 1,14**       |

Bu senaryoda, Microsoft, yayımlanan sanal makine görüntüsü kullanımı için saat başına 1,14 ödemenizi işler.


|**Microsoft faturalar** |**saat başına 1,14**  |
|---------|---------|
|Microsoft lisans maliyetinizi % 80'i ödeyen | saat başına 0,80        |
|Microsoft lisans maliyetinizi % 20 tutar    | saat başına 0,20        |
|Microsoft Azure kullanım maliyeti tutar     |   saat başına 0.14      |

Buna karşılık, etkinleştirirseniz, isteğe bağlı olarak kendi lisansını Getir:

|Maliyet lisansı     | Üzerinde anlaşılan ve yayımcı tarafından faturalandırılır lisans ücreti        |
|---------|---------|
|Azure kullanım maliyeti (1/D1-çekirdek)    | saat başına 0.14         |
|**Müşteri Microsoft tarafından faturalandırılır**     | **saat başına 0.14**        |

Bu senaryoda, Microsoft, yayımlanan sanal makine görüntüsü kullanımı için saat başına 0.14 ödemenizi işler. 

|**Microsoft faturalar**    |   **saat başına 0.14**      |
|---------|---------|
|Microsoft Azure kullanım maliyeti tutar     |    saat başına 0.14     |
|Microsoft lisans maliyetinizi %0 tutar     |  saat başına 0,00       |

**Tek faturalandırma ve ödeme yöntemleri**

Seçenek yayımlama işlem kullanmanın önemli bir avantajı, Microsoft erişebileceği tek fatura lisans maliyetleriniz temel Azure kullanım doğrudan müşteriye aynı zamanda ' dir. Bu senaryo, Microsoft Ürün ve sizin adınıza toplar, müşteriyle kendi satın alma ilişkisi oluşturmak için gereksinimini ortadan kaldırır. Bu, zaman ve fatura toplamadığı satış giriş odaklanmak için kaynak tasarrufu yapabilirsiniz.

**Kurumsal Anlaşma**

Microsoft müşterileri, bir Kurumsal Anlaşma bazen dahil olmak üzere Azure kullanım Microsoft ürünleri için ödeme yapmak için kullanın. Bu ödeme seçeneği yazılım lisans ve bulut Hizmetleri en az üç yıllık bir dönem için istediğiniz kuruluşlar için tasarlanmıştır. Müşterilerin bir eylemli ödeme yapmak yerine ödemeler yayılan seçeneğiniz vardır. EA müşteri Kullandıkça Öde işlem listeleme kullandığında, publisher'ın yazılım lisans maliyetleri için fatura döngüsü faturalama üç aylık EA fazlalığı izler.

**Parasal taahhüt** 

Tüm Kurumsal Anlaşma müşterileri, Azure'a önceden parasal taahhütte bulunarak Azure'ı anlaşmalarına ekleyebilir. Bu bağlılık çeşitli genel merkezlerinden Azure'un sunduğu bulut hizmetlerini herhangi bir bileşimini kullanarak yıl kullanılır.


### <a name="azure-marketplace-vs-appsource"></a>Azure Market vs. AppSource

Her mağaza benzersiz Müşteri gereksinimlerine işlevi görür. Bu rol için doğru çözüm ya da müşteri kim tabanlı hizmet sunmak için hedefleme sağlar.

BT uzmanları gerçekleştirmesine ve bulut aracılığıyla geliştiricilerin **Azure Marketi** bulmak için deneyin ve Iaas, SaaS ve PaaS çözüm satın alın:


|**Müşteri gereksinimi**  |**Azure Market** |
|---------|---------|
|**İşletme ve teknik gereksinimlerini karşılamak için fazladan bulut platformu işlevselliği talep**     |  Tamamlayıcı uygulamaların ve hizmetlerin Azure üzerinde çalıştırmak için en iyi duruma getirilmiş büyüyen bir Portföy sunar       |
|**Doğru uygulama veya hizmet bulmak için zor bulduğu**    |  Bulmak, deneyin ve çözümleri ve Azure hizmetleri satın almak için bir tek bir noktadan Mağazanız sağlar        |
|**Üçüncü taraf uygulamalar ve hizmetler için ölçeklenebilir dağıtım mekanizması gerekiyor**   | Oluşturma ve üçüncü taraf uygulamalar ve hizmetler için ölçeklenebilir dağıtımlar yapılandırılmasını sağlar        |
|**Yeni uygulamaları ve Hizmetleri Tümleştirme ve var olan çözümler ile çalışmak için gerektirir**  |   Azure üzerinde var olan çözümler ile üçüncü taraf uygulamaları ve Hizmetleri kolayca tümleşir      |

Üzerinde iş kullanıcıları göster **AppSource** bulmak için deneyin ve iş kolu satır SaaS uygulamaları ve sürücü iş sonuçları yardımcı değerine süresini azaltmak için uygulama hizmetleri alabilirsiniz: 


|**Müşteri gereksinimi**  |**AppSource**  |
|---------|---------|
|**Microsoft ürünleriyle çalışmak işletme çözümleri zaten kullandıkları** | Müşterilerinin Microsoft bulut uygulamaları ve teknolojileri genişletmek için üçüncü taraf uygulamaları ve hizmetleri kullanmasını sağlar       |
|**Doğru çözüm ya da uygulama hizmeti kolayca bulmak için yeteneği.**    |   Bulmak, deneyin ve uygulamaları ve Hizmetleri, eklentiler ve daha fazla bilgi almak için bir tek bir noktadan Mağazanız sağlar      |
|**Kendi özel iş güçlükleri sektöre özgü iş satır çözümleri**   | Adres belirli gereksinimleri arasında çok sayıda sektörü yardımcı olmak için tamamlanmış uçtan uca endüstri çözümleri sağlar     |
|**Verimlilik, verimliliğini ve iş öngörüleri geliştirmeye yardımcı olmak için uygulamalar**    | Uygulamalar için Müşteri Hizmetleri, ik, işlemleri ve çok daha fazlasını dahil olmak üzere iş satırları sağlar        |
| **Uygulamaları kendi benzersiz durumunuza uyarlamak için deneyimli uygulama iş ortağı** | Bir katalog Dynamics 365, Power BI, PowerApps ve üçüncü taraf uygulamalar tahmin edilebilir sonuçlar teslim iş kullanıcıları yardımcı olmak için göre çözümleri için danışmanlık hizmetleri tekliflerinin sağlar |

#### <a name="understanding-the-differences-between-storefronts"></a>Veriş arasındaki farklar anlama

Teklifiniz için hedef kitleyi tanımlayan ile mağaza başlatılır seçme. Çözümünüzün her iki izleyicileri hedefliyorsa, her iki veriş listesinde yalnızca bir kez yayımlamak gerekir.
 
Her mağaza ek yararları göz önünde bulundurun:

|StoreFront avantajı  |Azure Market  |AppSource   |
|---------|---------|---------|
|**Faturalama esneklik**    | Sanal makineler için Microsoft Kurumsal anlaşmalarındaki (EAs) veya web doğrudan satış modelleri Kullandıkça Öde fatura seçenekleri kullanın. Fiyatlandırma seçeneklerini de bir sunum perpetually boş olduğu bir ücretsiz katmanı abonelik içerir. Ayrıca daha sonra Ücretli bir aboneliğe dönüştürülür, sınırlı bir süre için promotionally ücretsiz deneyin şimdi aboneliği içerirler. Kendi etkinleştirme lisansını yayımcılar desteklemek için bir seçenek de getirin. <br><br>Fatura iki seçenek için sanal makineleri Azure yoluyla uygulamaları (örneğin, bir çözüm şablonu veya yönetilen bir uygulama) dağıtıldığı senaryolarda tüm sağlanmış Azure kaynaklarını doğrudan müşteriye faturalandırılır. | AppSource bir deneme sürümü deneyimi sağlama sunar, ancak şu anda yayımlama seçeneği ticaret etkin sağlamaz. Hiçbir ek yatırım veya değişikliklerle geçerli sıralama ve fatura altyapısını kullanabilirsiniz.        |
|**Diğer ortaklarıyla bağlantı dönüştüğünde, Yönetim**     |Azure Marketi teklifi için bir hizmet sağlayıcısı veya teslim ortakları bağlamak yayımcı şu anda izin vermez, ancak bu işlev içinde 2018 başlatacak.         |  Bağımsız yazılım satıcıları, sistemleri tümleştiricileri ve yönetilen hizmet sağlayıcıları için belirli uygulama senaryoları bağlanabilir. Bu özelliği yeni müşterilere işbirliğine dayalı satış destekler.      |
|**Otomasyon**     |    Azure Marketi teklifi için bir hizmet sağlayıcısı veya teslim ortakları bağlamak yayımcı şu anda izin vermiyor.     | Eklenti sağlamayla otomatik SaaS yararlanın. SaaS tabanlı veri toplama ve dağıtım senaryolarını otomatikleştirmek için çözüm şablonları kullanın.        |Bağımsız yazılım satıcıları, sistemleri tümleştiricileri ve yönetilen hizmet sağlayıcıları belirli uygulama senaryoları için işbirliğine dayalı yeni müşteriler için satış destekleme bağlanabilir.
|**Birden çok bulut türleri**     |   Genel Bulut ve şirket içi çözümleri Azure yığın aracılığıyla yayımlamak veya Azure kamu ve bölgesel Bulutlar, Çin ve Almanya dahil olmak üzere yayımlayabilirsiniz.      |    AppSource şu anda Azure yığını, Azure kamu veya bölgesel Bulutlar için destek sağlamaz.     |
|**İçerik sunu müşterilere**     |  Çözümünüzü bağlamsal arama (sanal makineler ve çözüm şablonlar) için Azure portal deneyiminde kullanılabilir duruma getirin.       |  Daha fazla müşterileri Dynamics 365, Power BI ve Office 365 gibi Microsoft ürünleri için uygulama deneyimi ile ulaşabilirsiniz.    |

