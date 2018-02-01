---
title: "Azure Market yayımcı Kılavuzu"
description: "Adım Adım Kılavuzu ve denetim listeleri yeni yayımcılar Azure Marketi'nde yayımlama"
services: marketplace
documentationcenter: 
author: ellacroi
manager: msmbaldwin
editor: 
ms.assetid: e8d228c8-f9e8-4a80-9319-7b94d41c43a6
ms.service: marketplace
ms.workload: 
ms.tgt_pltfrm: 
ms.devlang: 
ms.topic: article
ms.date: 01/18/2018
ms.author: ellacroi
ms.openlocfilehash: 7a980a9a4f0a4fa22e72aa9d6eec83865250a084
ms.sourcegitcommit: ded74961ef7d1df2ef8ffbcd13eeea0f4aaa3219
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2018
---
# <a name="azure-marketplace-publisher-guide"></a>Azure Market yayımcı Kılavuzu

Market yayımcı Kılavuzu'na Hoş Geldiniz. Bu kılavuz, aday ve nasıl AppSource veriş ve Azure Marketi uygulamalarının ve hizmetlerinin işletmelerini Microsoft yöneticileriyle büyümeye listelemek için yararlanacağınızı anlamak varolan yayımcılar yardımcı olmak için tasarlanmıştır. Bu kılavuz sonuna daha her biri bu konular hakkında bilgi edinin ve daha ayrıntılı bilgi nerede bulacağını biliyorsanız:

- Marketi'ndeki katılan yararları nelerdir
- Azure Market ve AppSource nelerdir
- Bu veriş yararlanmak nasıl
- Hangi mağaza teklifler ve hizmetler için doğru 
- Ne tür uygulamaları ve Hizmetleri teklifleri yayımlanabilir
- Her yayımlama seçeneği için teknik ve iş gereksinimleri nelerdir
- Önceden yayımlama varlık denetim listesi
- Bir yayımcı olmaya
- Oluşturma ve yayımlama nereye sunar
- Bir liste en iyi duruma getirme ve sürücü etkisi Go Market kaynaklara kullanma
- Yardım ve Destek yeri AppSource, Azure Marketi hakkında sorular veya bu yayımlama Kılavuzu Market Ekibi temasa  **cloudmarketplace@microsoft.com** . 

## <a name="the-benefits-of-participating-in-marketplace"></a>Marketi'ndeki katılan avantajları

Market Microsoft ile birleşik Git Pazar etkinlikler için başlatma paneli ve iş büyüme flywheel ' dir. Market teklifi Portföy başlatma yükseltme, isteğe bağlı oluşturma ve satış ve pazarlama Eklem kullanarak bulut iş altyapınız merkez olabilir. Market katılan için hiçbir ücretleri vardır. Amacımız, Microsoft müşterilerinin bizim iş ortağı ekosistemi sunduğu en iyi çözümleriyle bağlamaktır.

İşinizi büyümeye Market özelliklerinden yararlanmak:

- **Müşteri adayları ve satış fırsatları oluşturun.** Genişletilmiş bir Portföy çözümleri ile yeni pazarlara Microsoft bulut platformunda girin. Yukarı satış ve çapraz satış Market teklifleri. 
- **Mevcut ve yeni müşterilerle anlaşma boyutunu artırın ve iş değerini artırın.** İş yüklerinin buluta taşırken anlaşma boyutu ve adres Müşteri sorun teşkil edecek noktalar artar. Anlaşma Karlılık eksiksiz çözüm satış tarafından artırın. 
- **Eyleme dönüştürülebilir Öngörüler alın.** Sizi başarınız, bizim başarımızdır. Ne gerçekleştirme bulut iş ortağı Portalı aracılığıyla Öngörüler almak için ne sizi oluşturulur ve kampanya etkinliklerinizi en üst düzeye çıkarmak nasıl.

## <a name="what-are-azure-marketplace-and-appsource"></a>Azure Market ve AppSource nelerdir?

Microsoft iş ortaklarının teklifler listesinde, denemeler etkinleştirmek ve doğrudan Microsoft'un müşterileri ve ekosistemi ile transact izin iki ayrı Market veriş sağlar: [Azure Market] (https://azuremarketplace.microsoft.com) ve [AppSource] () https://appsource.microsoft.com). Bu veriş bulmak için müşteriler deneyin ve uygulamaları satın alın ve Microsoft'un müşterileri ve iş ortağı ekosistemi erişimi artırarak işlerini kendi sayısal dönüşüm hızlandırmak ve yayımcılar yardımcı hizmetler büyümesine izin verir.
 
Market veriş tam olarak ne ihtiyaç duydukları Bul müşterilere yardımcı olmak için izleyiciler ve Microsoft bulut ürünleri hizalanır. Her mağaza tarafından aşağıdaki tabloda özetlenen yayımlama yatırımınızı en üst düzeye çıkarmanıza yardımcı olmak için özelleştirilmiş yayımlama seçeneklerini sunar:


|          |Azure Market |AppSource  |
|---------|---------|---------|
|Hedef kitle     |BT uzmanları, geliştiricilerin (uzmanı rolleri dahil DBAs, SecOps, DevOps, vs.)    | Satırı iş karar verenler (uzmanı rolleri dahil tedarik, üretim, hesap, vs.)      |
|Yerleşik genişletmek için     |Azure         | Azure, Dynamics 365, Office 365, Power BI ve Güç uygulamaları       |
|Çözümler ve Hizmetleri türleri     |  Altyapı çözümleri ve Profesyonel Hizmetler   | Tamamlanmış satırı iş uygulamaları ve Profesyonel Hizmetler        |
|Yayımlama seçenekleri     |  Benimle iletişim, danışmanlık hizmetleri, deneme, sanal makine, çözüm şablonları sunar ve yönetilen uygulamalar       |  Danışmanlık Hizmetleri teklifi veya deneme benimle iletişim       |
|Uygulama Deneyimi     | Azure portalı ve CLI         | Office 365, Dynamics 365, Power BI, Office istemci uygulamaları       |

## <a name="leveraging-these-storefronts"></a>Bu veriş yararlanarak

Her mağaza benzersiz Müşteri gereksinimlerine hizmet etmeyen ve doğru çözüm ya da müşteri kim tabanlı hizmet sunmak izin vermek için rol tarafından hedefleme sağlar.

BT uzmanları ve aracılığıyla bulut geliştiricilerin devreye **Azure Marketi** bulmak için deneyin ve Iaas, SaaS ve PaaS çözüm satın alın:


|Müşteri gereksinimi  |Azure Market |
|---------|---------|
|**İşletme ve teknik gereksinimlerini karşılamak için fazladan bulut platformu işlevselliği talep**     |  Tamamlayıcı uygulamaların ve hizmetlerin Azure üzerinde çalıştırmak için en iyi duruma getirilmiş büyüyen bir Portföy sunar       |
|**Doğru uygulama veya hizmet bulmak için zor bulduğu**    |  Bulmak, deneyin ve çözümleri ve Azure hizmetleri satın almak için bir tek bir noktadan Mağazanız sağlar        |
|**Üçüncü taraf uygulamalar ve hizmetler için ölçeklenebilir dağıtım mekanizması gerekiyor**   | Oluşturma ve üçüncü taraf uygulamalar ve hizmetler için ölçeklenebilir dağıtımlar yapılandırılmasını sağlar        |
|**Yeni uygulamaları ve Hizmetleri Tümleştirme ve var olan çözümler ile çalışmak için gerektirir**  |   Azure üzerinde var olan çözümler ile üçüncü taraf uygulamaları ve Hizmetleri kolayca tümleşir      |

Kullanarak iş kullanıcıları göster **AppSource** bulmak ve iş satır SaaS uygulamaları ve Hizmetleri deneyin: 


|Müşteri gereksinimi  |AppSource  |
|---------|---------|
|**Dynamics 365, Office 365, Power BI ve Güç uygulamaları işlevselliği genişletmek istiyor**   |  Müşterilerinin Microsoft bulut platformu işlevlerini genişletmek için üçüncü taraf uygulamaları ve hizmetleri kullanmasını sağlar       |
|**Doğru uygulama veya hizmet bulmak için zor bulduğu**    |   Bulmak için bir tane Dükkanı ve deneme uygulamaları ve Hizmetleri, eklentiler ve daha fazla bilgi sağlar      |
|**Sektöre özgü iş kolu çözümünü gerekiyor**   | Müşteriler ihtiyaç duydukları ne bulabilmesi için her endüstri için çözümleri sağlar        |
|**İş özgü çözümleri gerektiriyor**    | İş ve müşteri hizmetleri, ik, işlemleri ve çok daha fazlasını dahil olmak üzere iş sorunu her satırı için çözümleri sağlar        |

## <a name="understanding-the-differences-between-storefronts"></a>Veriş arasındaki farklar anlama

Bir mağaza başlatılır teklifiniz hedef kitlesi tanımlamaya seçme: Azure Marketi BT profesyonelleri ve geliştiricilerin ihtiyaçlarını hizalanmış ve AppSource iş kullanıcılara hizalanması. Çözümünüzün her iki izleyicileri hedefliyorsa, yalnızca bir kez hem veriş listesinde yayımlamak gerekir.
 
Her mağaza ek yararları göz önünde bulundurun:

|StoreFront avantajı  |Azure Market  |AppSource   |
|---------|---------|---------|
|**Faturalama esneklik**    | Sanal makineler için 'Kullandıkça Öde' fatura seçenekleri, Microsoft Kurumsal anlaşmalarındaki veya web doğrudan satış modelleri kullanın. Fiyatlandırma seçeneklerini de sınırlı bir süre sonra Ücretli bir aboneliğe dönüştürür promotionally ücretsiz deneyin şimdi aboneliği yanı sıra bir sunum perpetually boş olduğu bir ücretsiz katmanı abonelik içerir. 'Kendi lisansını Getir' etkinleştirme durumda da senaryolarda fatura her iki seçenek için yayımcılar desteklemek için bir seçenek Azure uygulamaları (örn., çözüm şablonu veya yönetilen uygulama) kullanılarak dağıtılan sanal makineler burada sağlanan tüm Azure kaynakları faturalandırılır doğrudan müşteriye | AppSource sorunsuz bir deneme sürümü deneyimi sağlama sunar, ancak şu anda yayımlama seçeneği ticaret etkin sağlamaz; Bu, geçerli sıralama yararlanmanızı ve hiçbir ek yatırım veya değişiklikleri fatura altyapısıyla sağlar        |
|**Diğer ortaklarıyla bağlantı kolaylaştırmak**     |Azure Market şu anda bir hizmet sağlayıcısı veya teslim iş ortakları için teklif bağlamak yayımcı izin vermiyor         |  Bağımsız yazılım satıcıları, sistemleri tümleştiricileri ve yönetilen hizmetler sağlayıcıları belirli uygulama senaryoları için işbirliğine dayalı yeni müşteriler için satış destekleme bağlanabilir      |
|**Otomasyon**     |    Azure Market şu anda bir hizmet sağlayıcısı veya teslim iş ortakları için teklif bağlamak yayımcı izin vermiyor     | SaaS tabanlı veri toplama ve dağıtım senaryolarını otomatikleştirmek için çözüm şablonları kullanın ve eklenti sağlamayla otomatik SaaS yararlanın        |Bağımsız yazılım satıcıları, sistemleri tümleştiricileri ve yönetilen hizmetler sağlayıcıları belirli uygulama senaryoları için işbirliğine dayalı yeni müşteriler için satış destekleme bağlanabilir
|**Birden çok bulut türleri**     |   Genel Bulut ve şirket içi çözümleri Azure yığın aracılığıyla yayımlamak ya da Azure kamu ve bölgesel Bulutlar Çin ve Almanya dahil olmak üzere yayımlama      |    AppSource şu anda Azure yığını, Azure kamu veya bölgesel Bulutlar için destek sağlamaz     |
|**İçerik sunu müşterilere**     |  Çözümünüzü bağlamsal arama (sanal makineler ve çözüm şablonlar) için Azure portal deneyimi kullanılabilmesini       |  Çözümünüzü uygulama deneyimi Microsoft ürünleri için kullanılabilir duruma       |

## <a name="select-a-publishing-option"></a>Yayımlama Seç seçeneği

Birden çok yayımlama seçeneklerini ve teklif türleri her mağaza destekler: listesi, deneme ve Transact. En iyi uygulama ve hizmet ayrıntıları temsil eden bir teklif türü seçin. Tüm yayımlama seçeneklerini iş ortakları, paylaşım sağlama için erişim sahibi olursunuz. 


|**Yayımlama seçeneği**  | **Teklif türü** | **Mağaza**  |
|---------|---------|---------|
|**Liste**    |    Hizmet danışmanlık benimle iletişim     |  Azure Market, AppSource       |
|**Deneme**   |     Ücretsiz deneme sürümü, SaaS deneme, etkileşimli Demo, Test sürücü    |  Azure Market, AppSource       |
|**Transact**     |   Sanal makine, çözüm şablonu, uygulama yönetilen      |    Azure Market     |

### <a name="list"></a>LİSTE

Kullanım **kişi benim** deneme veya işlem düzeyi Katılım olmadığında yapılabilir. Bu yaklaşımın avantajı, hemen iş flywheel başlatmak için temel anlaşmalar becerilerin geliştirilmesi müşteri adayları almaya başlamak bir çözüm temelinde piyasaya olan yayımcı etkinleştirir ' dir. Ancak, dezavantajı müşteri katılım, diğer teklif türleri ile karşılaştırıldığında sınırlıdır.

Ne zaman teklif oluşur öncelikle Profesyonel Hizmetler (örn., değerlendirmeleri, uygulamaları, Atölyeleri) kullanım **Danışmanlık Hizmetleri** türü sunar. Teklif kapsam, süre ve fiyat düzeltilmesi gereken, tek bir müşteri için olmalıdır ve şirkete gerçekleştirilmesi gerekir.

### <a name="trial"></a>DENEME

Bir deneme sürümü deneyimi sağlayarak müşteriler ve bu nedenle daha zengin bir Etkilenme çözümünüzün sunulan katılım düzeyini artırır. Bir deneme sürümünü satın almadan önce çözümünüzü incelemek müşterilerin sağlar. Bir deneme sürümü deneyimi ile veriş yükseltme yüksek olasılığını sahip olur ve müşteri katılımlar daha zengin ve daha fazla müşteri adaylarını beklemelisiniz.
 
Tüm deneme seçenekleri deneme ortamı ve/veya Azure aboneliği içine yerine, Müşteri'nin veya Azure aboneliğinizde içine dağıtılır. Denemeler müşteri gözetimindeki herhangi bir ek satın alma işlemleri olmadan ve en az olmalıdır varsa, basit bir tamamlamak için ek yapılandırma kullanım örneği. Denemeler ücretsiz destek en az ve deneme süresi boyunca eklemeniz gerekir. Deneme kullanıcılar becerilerin geliştirilmesi ve en iyi sonuçlar için kasıtlı değerlendirme yol boyunca izlenen. Yayımcılar Market müşteri adayları ve yayımcının kendi uygulama Intelligence izlemek ve deneme kullanıcıları yönetmek için kullanmanız önerilir.

3 tipik deneme senaryo vardır:


|**Deneme seçeneği**  |**Başlıca yararları**  |**Gerekirse bu seçeneği belirleyin...**  |
|---------|---------|---------|
|**Ücretsiz deneme sürümü**    |     Ücretli dönüştürmek için otomatik bir yöntem almadan önce ürününüzü denemek bir müşteri sağlar ve Microsoft satış ekipleri ile müşteri ve eklem katılım için kavramlar kanıtı sağlar |     Bir sanal makine çözümünüzü veya çözüm şablonu veya bilgisayarınızı çözümüdür ve, teklif bir çok kiracılı SaaS ürün sunumu SaaS, bir müşteri alınacağı deneyimi ve hızlı bir şekilde, tek bir kiracı varsa, ancak müşteri olarak eklemekte olduğunuz çalışırken bir ilk sahip ' bize Konuk ers|
**Test sürücü**     |     Önceden yapılandırılmış bir ayar yapmasına çözümünüzün Kılavuzlu bir deneyim sağlar ve bunlar satın almadan önce ürününüzü denemek bir müşteri sağlar |   Çözümünüzü bir sanal makine, çözüm şablonu ya da tek bir kiracı SaaS uygulamayla veya OR sahip olmadığınız denemenizi Ücretli teklif dönüştürmek için bir yöntem sağlama karmaşıktır |
|**Etkileşimli Tanıtımı**    |  Kurulum KARMAŞASIZ eylem ürününüzü görmek müşterilerin olanak tanır       |    Çözümünüzü ve deneme süresi elde etmek zor olurdu karmaşık kurulum gerektirir     |

#### <a name="free-trial"></a>Ücretsiz Deneme

Kullanım bir **ücretsiz deneme** ne zaman çözüm ya da uygulama sunar serbest çalışırsanız, SaaS tabanlı deneme. Bu seçenek yüksek kaliteli müşteri adayları, iş flywheel başlamanıza yardımcı ilgilenen müşterilerden sürücüleri. Ücretsiz deneme sınırlı kullanın veya kısa süreli deneme hesapları sunulan ve bir çağrı--yazılımınızı Ücretli kullanılacak dönüştürülmesi hızlandırmak için eylem içermelidir.

#### <a name="test-drive"></a>Test Drive

Kullanım bir **Test sürücü** Iaas ya da SaaS uygulamaları aracılığıyla bir veya daha fazla sanal makine aracılığıyla çözümü dağıtıldığında. Bu yaklaşımın avantajı, bir sanal gereç ya da bir iş ortağı barındırılan 'hiçbir ek ücret ödemeden müşteri değerlendirmesi için çözümün müşteriye Kılavuzlu Tur içinde' couched çözümün tamamında ortamı otomatik sağlama ' dir. Müşteri varolan bir Azure müşteri olması gerekmez, daha yüksek kaliteli oluşturmak için yardımcı yol açar.

Başka avantajları vardır bir **Test sürücü**:

- kullanıcılar tarafından yalnızca Göster teklifleri test sürücülerle için iyileştirilmiştir kullanıcı aramalarını Market'te 27 yüzdesi 
- %38 daha fazla müşteri adayları teklifleri daha test sürücülerle teklifleri oluştur 
- Yeni müşteri edinme Azure Market'te % 36 sınamayı sürdü müşterilerden gelen 
- Ürününüz ortak satış çalışmaları için daha iyi anlamak Microsoft alan satıcılar etkinleştir

#### <a name="interactive-demo"></a>Etkileşimli Tanıtım

Ürününüzün ile Kılavuzlu bir deneyim aracılığıyla müşterilerinize ele bir **etkileşimli Demo**. Bu seçeneğin avantaj olmadan sağlamayı karmaşık karmaşık çözümleri için bir deneme sürümü deneyimi sağlayabilir olmalıdır. Bu seçenek geçici çözüm bir görünüm ile bir müşteri sağlar ve iş flywheel başlatmak için temel anlaşmalar becerilerin geliştirilmesi müşteri adayları almaya başlamak yayımcı sağlar. 

### <a name="transact"></a>TRANSACT

Azure Marketi'nde kullanan bir **sanal makine** müşteri aboneliğe sanal gereç olarak çözüm dağıtıldığında. Sanal makinelerin tam olarak ticaret Kullandıkça Öde veya KLG etkin lisans modelleri ile etkin olduğunu. Microsoft commerce işlem barındırır ve yayımcı adına müşteri bills. Yayımcılar ile Kurumsal Anlaşma dahil olmak üzere Microsoft, Müşteri'nin tercih edilen ödeme ilişkisi yararlanarak yararı alın. 

>[!NOTE]
>Şu anda bir kurumsal anlaşması parasal taahhüt sanal gereç ait Azure kullanım karşı ancak publisher'ın yazılım lisans ücretleri karşı değil kullanılabilir.

Kullanım bir **Azure çözüm şablonu** ne zaman bir çözüm sanal gereç ötesinde ek dağıtım ve yapılandırma Otomasyon gerektirir. Çözüm şablonları bir veya daha fazla sanal makine kaynakları sağlama otomatik hale getirebilir ve ayrıca ağ ve depolama kaynaklarını sağlayabilirsiniz. Çözüm şablonları tüm Iaas tabanlı çözümü ortamları yanı sıra tek sanal makineleri üzerinde Otomasyon fayda sağlayabilir. Çözüm şablonları oluşturma hakkında daha fazla bilgi [burada](https://github.com/MicrosoftDocs/azure-docs).

Yönetilen bir Azure uygulama yayımcı ya da müşteri bir 3. taraf tarafından örneğin bir SI veya MSP yönetilecek çözüm istediğinde sanal makine ya da Iaas tabanlı çözümün tamamında bir müşterinin aboneliğini dağıtırken kullanın. Daha fazla bilgi edinmek [yönetilen uygulamalar burada derleme](https://docs.microsoft.com/azure/managed-applications/overview). Sık sorulan soruların bir listesi için bkz: [Azure Market SSS](https://azure.microsoft.com/marketplace/faq/).

### <a name="azure-certified"></a>Azure Sertifikası

Azure Marketi'nde yayımlanan tüm sanal makineler için test edildiğini **Azure sertifikalı** program. Program müşteriler, sanal makinenizi Azure platformu ile uyumludur ve model satış Market çevrimiçi görüntü güvenliği uygunluk testleri virüs ve kötü amaçlı yazılım dahil olmak üzere sağlar ve teklif geliştirmek için düzeyinde badging sağlar sağlar. doğrulanmış bir çözüm olarak yükseltme Microsoft Kurumsal müşteriler için.

#### <a name="marketplace-commercial-considerations"></a>Market ticari konuları

Marketi'ndeki katılan için hiçbir ücretleri vardır. Liste, deneme ve KLG Transact seçenekleri kullanarak yayımlarken Marketi'ndeki katılan için geliri paylaşımı yok. Daha fazla ayrıntı için bkz: bizim [Market katılım ilkeleri](https://azure.microsoft.com/support/legal/marketplace/participation-policies/).

#### <a name="pay-as-you-go-and-bring-your-own-license-billing-options"></a>Kullandıkça Öde ve Getir bilgisayarınızı-kendi-lisans faturalama seçenekleri

Seçenek yayımlama Kullandıkça Öde Transact kullanıldığında gelir lisans kullanım tabanlı yazılımınızı paylaşılan % 80 / % 20 ve Microsoft arasında sırasıyla. Tek bir teklif Kullandıkça Öde ve Getir bilgisayarınızı kendi lisans faturalama modellere fiyatlandırılır ve teklif düzeyinde ayrı SKU'ları birlikte bulunabilir. Bu teklifiniz bulut iş ortağı Portalı'nda yapılandırılabilir.

Bu örneği göz önünde bulundurun:

İsteğe bağlı olarak Kullandıkça Öde etkinleştirirseniz:


|Lisans maliyetinizi   | saat başına 1.00 $        |
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

|Lisans maliyetinizi     | Üzerinde anlaşılan ve yayımcı tarafından faturalandırılır lisans ücreti        |
|---------|---------|
|Azure kullanım maliyeti (1/D1-çekirdek)    | saat başına 0.14         |
|**Müşteri Microsoft tarafından faturalandırılır**     | **saat başına 0.14**        |

Bu senaryoda, Microsoft 0.14 yayımlanan sanal makine görüntüsü kullanımı için saat başına ödemenizi işler. 

|**Microsoft faturalar**    |   **saat başına 0.14**      |
|---------|---------|
|Microsoft Azure kullanım maliyeti tutar     |    saat başına 0.14     |
|Microsoft lisans maliyetinizi %0 tutar     |  saat başına 0,00       |

### <a name="single-billing-and-payment-methods"></a>Tek faturalandırma ve ödeme yöntemleri

Seçenek yayımlama Transact kullanmanın önemli bir avantajı Microsoft 'fatura tek' lisans maliyetleriniz temel Azure kullanım doğrudan müşteriye aynı zamanda dir. Bu senaryo, Microsoft Ürün ve sizin adınıza toplar, müşteriyle kendi satın alma ilişkisi oluşturmak için gereksinimini ortadan kaldırır. Bu, zaman ve fatura toplamadığı satış giriş odaklanmak için kaynak tasarrufu yapabilirsiniz.

### <a name="enterprise-agreement"></a>Kurumsal Anlaşma

Microsoft müşterileri bir Kurumsal Anlaşma (Kurumsal Sözleşme) dahil olmak üzere Azure kullanım Microsoft ürünleri için ödeme yapmak bazen kullanın. Bu ödeme seçeneği yazılım lisans ve bulut Hizmetleri en az üç yıllık bir dönem için istediğiniz kuruluşlar için tasarlanmıştır. Müşterilerin bir eylemli ödeme yerine ödemeler yayılan seçeneğiniz vardır. EA müşteri Kullandıkça Öde Transact listeleme kullandığında, publisher'ın yazılım lisans maliyetleri için fatura üç aylık EA aşma faturalama döngüsü izler.

### <a name="monetary-commitment"></a>Parasal Taahhüt 

Tüm Kurumsal Anlaşma müşterileri, Azure'a önceden parasal taahhütte bulunarak Azure'ı anlaşmalarına ekleyebilir. Bu taahhüt, yıl boyunca Azure'un küresel veri merkezleri aracılığıyla sunduğu birçok çeşit bulut hizmetinin herhangi bir birleşimi kullanılarak tüketilir.

## <a name="prerequisites-for-marketplace-publishing"></a>Market yayımlama için Önkoşullar

### <a name="for-all-marketplace-publishing-options"></a>Tüm Market Yayımlama seçenekleri


|**Gereksinim**  |**Ayrıntılar**  |**Yayımlama seçeneği**  |
|---------|---------|---------|
|**Katılım ilkeleri**    |  Gözden geçirme Azure Market yayımcı sözleşmesi [here](file:///C:/Users/ellacroi/Downloads/Microsoft%20Marketplace%20Publisher%20Agreement.pdf). Azure Market katılım ilkelerini [burada] (https://azure.microsoft.com/support/legal/marketplace/participation-policies/) gözden geçirin.       | Liste, deneme, Transact        |
|**Microsoft ile tümleştirme**    | Azure Market sunar, yararlanan ya da işlem, ağ veya depolama gibi Microsoft Azure Hizmetleri genişletmek ve var olan bir Azure Marketi kategori veritabanları, güvenlik, ağ, vb. gibi Hizala. Tam listeyi Bul [burada](https://azuremarketplace.microsoft.com/marketplace/apps).        | Liste, deneme, Transact        |
|**Hedef kitle**    | Azure Market sunar, BT uzmanları, bulut geliştiriciler veya diğer teknik müşteri rolleri olması gerekir.       |  Liste, deneme, Transact 
|**Sağlama Yönetimi**    | Marketten müşteri adayları almak için CRM (Marketo, Microsoft Dynamics veya Salesforce) sağlama verileri kabul etmek için etkinleştirmeniz gerekir.        |   Liste, deneme, Transact      |
|**Gizlilik ilkesini ve kullanım koşulları**     |   Gizlilik İlkesi genel bir URL ile kullanılabilir olmalı ve yayımlama sırasında metin olarak, kullanım koşulları girmeniz gerekir.      |   Liste, deneme, Transact      |
|**Destek**     |  Teklifiniz, müşterilerin Yardım bulabileceğiniz bir genel kullanıma açık destek URL'si eklemeniz gerekir. Denemeler için destek için deneme süresi hiçbir ek ücret sağlanmalıdır.       |  Denemeyi Transact       |

### <a name="prerequisites-specific-to-trial-publishing"></a>Deneme yayımlama için belirli önkoşulları

|**Gereksinim**  | **Ayrıntılar**  |**Yayımlama seçeneği**  |
|---------|---------|---------|
|**Ücretsiz deneme süresi ve deneme sürümü deneyimi**     |  Bir müşteri uygulamanızı sınırlı bir süre için ücretsiz kullanmanız mümkün olması gerekir.<br>Bu, müşteri lisans veya Abonelik ücretlerinin ürününüzü ya da temel alınan Microsoft birinci taraf ürün veya hizmetle maliyetini tabi olmaz anlamına gelir. Tüm deneme seçenekleri publisher'ın Microsoft Ürün aboneliğine dağıtılan olduğundan, en iyi duruma getirme deneme maliyet ve yönetim yalnızca yayımcı tarafından denetlenir.<br>Ücretsiz deneme sürümü, etkileşimli demo veya Test sürücü seçebilirsiniz. Seçtiğiniz öğeye olsun, ücretsiz deneme müşteri uygulaması için ek ücret ödemeden denemek için en az bir süre sağlaması gerekir.<br> Ulaşmak cloudmarketplace@microsoft.com sınamayı oluşturma işlemine başlamak için. Not: Kimlik bilgileri Azure Market deneme deneyimleri kendi Active Directory ile oturum açmasını izin vermelidir SaaS çalışır. [Daha fazla bilgi edinin.](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-devhowto-appsource-certified#appsource-trial-experiences) |   Deneme      | 
| **Kolayca yapılandırılabilir, anahtar teslimi çözümü**    |  Uygulamanızı yapılandırmak ve ayarlamak hızlı ve kolay olmalıdır.       |  Deneme       |
|**Kullanılabilirlik/açık kalma süresi**    |    SaaS uygulaması veya platform en az % 99,9 çalışma süresi olması gerekir.     |    Deneme     |
|**Azure Active Directory**    |    Azure Active Directory Federasyon çoklu oturum açma teklifiniz izin vermesi gerekir (AAD Federasyon SSO) ile etkin onay.      |  Deneme|

### <a name="prerequisites-specific-to-transact-publishing"></a>Yayımlama olmaya özel Önkoşullar


|**Gereksinim**  |**Ayrıntılar** |**Yayımlama seçeneği**  |
|---------|---------|---------|
|**Faturalama ve ölçümü**    |  Sanal makine ya da kendi lisansını Getir desteklemesi gerekir veya kullanım tabanlı, aylık faturalama.       |    Transact    |
|**Azure ile uyumlu sanal sabit disk (VHD)**     |   [Windows] (https://docs.microsoft.com/en-us/azure/marketplace-publishing/marketplace-publishing-vm-image-creation) sanal makineleri yerleşik veya [Linux] (https://docs.microsoft.com/en-us/azure/marketplace-publishing/marketplace-publishing-vm-image-creation)    |   Transact      |

### <a name="prerequisites-specific-to-consulting-services-publishing"></a>Danışmanlık için Önkoşullar belirli hizmetleri yayımlama


|**Gereksinimleri** |**Ayrıntılar**  |**Yayımlama seçeneği**  |
|---------|---------|---------|
|**Hizmet özellikleri sunan**     | Danışmanlık hizmetinizi süresi, sabit bir sabit kapsam, fiyat sabit olarak teslim edilen (veya boş) olmalıdır katılım, yönelik, öncelikle satış öncesi tek bir müşteri için sınırlı ve şirkete gerçekleştirilen        |    Liste     |
|**Danışmanlık Hizmetleri için ortak gereksinimler**    |   **Yalnızca AppSource.**  Dynamics 365 müşteri katılım için [Gümüş veya altın bulut müşteri ilişkileri yönetimi uzmanlığı](https://partner.microsoft.com/en-us/membership/cloud-customer-relationship-management-competency). Dynamics 365 Finans ve işlemleri Enterprise edition için: Gümüş veya altın [kuruluş kaynak planlama] (https://partner.microsoft.com/en-us/membership/enterprise-resource-planning-competency) uzmanlığı ve en düşük gelir, $25K bulutta İşlem sonunda 12 ay içinde. Dynamics 365 Finans ve işlemleri, Business edition: olarak hizmet [bulut Hizmetler Sağlayıcısı (CSP)](https://partner.microsoft.com/en-us/cloud-solution-provider) veya [dijital iş ortağı, kaydı (DPOR)](https://partner.microsoft.com/en-us/membership/digital-partner-of-record) en az bir müşteri için. Power BI: [çözüm ortağı] (file:///C:/Users/ellacroi/Downloads/BI%20Partner%20Program%20Overview%20 & % 20Incentives.pdf) ölçütlere uygun olmalıdır. PowerApps: [iş ortağı gösterimi] (https://powerapps.microsoft.com/en-us/partner-showcase/) çözümünün var |    Liste     |

## <a name="using-azure-active-directory-to-enable-trials"></a>Denemeler etkinleştirmek için Azure Active Directory kullanma
Azure Active Directory (AAD) olan bir Microsoft iş veya Okul kimlik doğrulamasına olanak tanıyan bir bulut kimlik hizmeti endüstri standardı protokoller kullanarak hesap: OAuth ve Openıd Connect. AAD hakkında daha fazla bilgi [burada](https://www.microsoft.com/en-us/cloud-platform/azure-active-directory-features). 

Microsoft kullanıcıların kimliğini doğrulayan tüm Market aad'ye, bu nedenle kimliği doğrulanmış bir kullanıcı, deneme listesindeki Market üzerinden tıkladığında ve yeniden yönlendirilir deneme ortamınıza, hazırlayabilirsiniz kullanıcının doğrudan bir deneme sürümü'nü gerek kalmadan bir ek oturum açma adımı. [Uygulamanızı AAD kimlik doğrulaması sırasında alan belirteci] (https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-token-and-claims#sample-tokens) oluşturmak için kullanabileceğiniz değerli kullanıcı bilgilerini içeren bir kullanıcı hesabı, uygulamanızda sağlama deneyimi otomatikleştirmek ve dönüştürme olasılığını artırmak etkinleştirme. 

AAD kullanarak uygulamanızı veya deneme için 1-tıklatma kimlik doğrulamasını etkinleştirmek için:

- Market müşteri deneyimine deneme kolaylaştırır 
- 'Ürün Deneyimi' yapısını tutar bile zaman kullanıcının Market'ten, etki alanı veya deneme ortamı yönlendirildiği
- Ek bir oturum açma adımı olmadığından yeniden yönlendirme üzerinde abandonment olasılığını azaltır
- AAD kullanıcılarını büyük popülasyonunu dağıtım engelleri azaltır

### <a name="certifying-your-azure-active-directory-integration-for-marketplace"></a>Market için Azure Active Directory tümleştirmesini onaylıyor

Çok Kiracılı uygulamalar için:

AAD bugün destekliyorsa

- Azure portalında uygulamanızı kaydetme
- 'Tek tıklatmayla' deneme sürümü deneyimi almak aad'de çoklu kiracı destek özelliğini etkinleştirin
- [Şuradan daha fazla bilgi edinin](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-integrating-applications)

AAD Federasyon SSO yeniyseniz

- Azure portalında uygulamanızı kaydetme
- [Openıd Connect] (https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-protocols-openid-connect-code) kullanarak AAD SSO'su geliştirmek veya [OAuth 2.0](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-protocols-oauth-code)
- 'Tek tıklatmayla' deneme sürümü deneyimi almak aad'de çoklu kiracı destek özelliğini etkinleştirin
- [Şuradan daha fazla bilgi edinin](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-devhowto-appsource-certified)

Tek Kiracı uygulamalar için:

Tek bir kiracı uygulamalar için birden çok seçenek vardır:

- Kullanıcıların [Azure B2B] kullanarak Konuk kullanıcılar olarak dizininize eklemek (https://docs.microsoft.com/en-us/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b)
- El ile sağlama denemeler 'Kişi benim' aracılığıyla müşteriler için
- Geliştirme bir müşteri 'Test sürücü' başına
- Çok kiracılı örnek Tanıtım uygulamasını SSO ile derleme

## <a name="cloud-partner-portal-pre-publishing-checklist"></a>Denetim listesi önceden yayımlama bulut iş ortağı portalı

Yayımlama işlemini başlatmadan önce bir teklifi oluşturmak için gerekli bileşenleri anlamak faydalıdır. Aşağıdaki yapılar oluşturma teklif yayımlama akışında bulut iş ortağı Portalı'nı tamamlamak için gereklidir. 

### <a name="storefront-details"></a>StoreFront ayrıntıları


|Bu yapı yayımlama gerekir  |Bu teklif türü için  |
|---------|---------|
|**Ad sunar (200 karakter) ve Açıklama (2000 karakter)**    |  Tümü        |
|**MPN kimliği ve yetkinlikleri**   |  Danışmanlık Hizmetleri       |
|**Ülke/bölge kullanılabilirliği**   | Tümü        |
|**Katılım süresi**     |   Danışmanlık Hizmetleri      |
|**Geçerli sektörü, kategoriler ve arama anahtar sözcükleri**     |  Tümü       |
|**Şirket logoları (48 x 48, 216 x 216)**     |  Danışmanlık Hizmetleri       |
|**Ürün genel bakış videosu (isteğe bağlı)**  |  Tümü       |
|**Ekran görüntüleri (Maks 5, 1280 x 720)**   |    Tümü     |
|**Pazarlama belgeleri (Max3)**    |  Tümü       |
|**Hedef yol**    |   Tümü      |

### <a name="contacts"></a>Kişiler


|Bu yapı yayımlama gerekir  |Bu teklif türü için  |
|---------|---------|
|**Bilgi (desteği, mühendislik, ticari) başvurun**    |    Tümü     |

### <a name="technical-info"></a>Teknik bilgi


|Bu yapı yayımlama gerekir  |Bu teklif türü için |
|---------|---------|
|**Deneme URL'si**     |  Tüm deneme teklifi türleri       |
|**Desteklenen diller**    |   Tüm deneme teklifi türleri      |
|**Uygulamanın sürüm numarasını ve yayın tarihi**    |   Tüm deneme teklifi türleri      |
|**Destek URL'si**    |   Tüm deneme teklifi türleri, sanal makineler      |
|**Kullanım ve gizlilik ilkesi URL'si koşulları**     |    Tümü     |

### <a name="test-drive"></a>Test Drive


|Bu yapı yayımlama gerekir  |Bu teklif türü için  |
|---------|---------|
|**Açıklama ve süresi**     |  Yalnızca test sürücü       |
|**Kullanıcı el ile**     |   Yalnızca test sürücü      |
|**Test sürücü Video (en çok 1)**     |  Yalnızca test sürücü       |
|**Test sürücü ülke/bölge kullanılabilirliği**    |   Yalnızca test sürücü      |
|**Azure kaynak grubu adı**   |         |
|**Azure abonelik kimliği**     |  Yalnızca test sürücü       |
|**Azure AD Kiracı kimliği**   |    Yalnızca test sürücü     |
|**Azure AD uygulama kimliği**  |  Yalnızca test sürücü       |
|**Azure AD uygulama anahtarı**     |   Yalnızca test sürücü      |

### <a name="storefrontmarketplace"></a>Mağaza/Market


|Bu yapı yayımlama gerekir  |Bu teklif türü için  |
|---------|---------|
|**Başlık (en çok 50 karakter)**    |  Transact ' sanal makineleri, Azure uygulamaları (Çözüm şablonları ve yönetilen uygulamalar)       |
|**Özet (en fazla 200 karakter)**    |  Transact ' sanal makineleri, Azure uygulamaları (Çözüm şablonları ve yönetilen uygulamalar)       |
|**Uzun özeti (en fazla 256 karakter)**     |   Transact ' sanal makineleri, Azure uygulamaları (Çözüm şablonları ve yönetilen uygulamalar)      |
|**HTML tabanlı açıklaması (en çok 3000 karakter)**    |  Transact ' sanal makineleri, Azure uygulamaları (Çözüm şablonları ve yönetilen uygulamalar)       |
|**Şirket logoları (40 x 40, 90 x 90, 115 x 115, 255 x 115, 815 x 290)**    |  Transact ' sanal makineleri, Azure uygulamaları (Çözüm şablonları ve yönetilen uygulamalar)       |

### <a name="sku"></a>SKU


|Bu yapı yayımlama gerekir  |Bu teklif türü için  |
|---------|---------|
|**Sürüm numarası**     |    Transact ' Azure uygulamaları (Çözüm şablonları ve yönetilen uygulamalar)     |
|**Tüm şablon dosyalarını ve createUIDefinitionFile içeren paket dosyası**   |Transact ' Azure uygulamaları (Çözüm şablonları ve yönetilen uygulamalar)         |
|**İşletim sistemi ayrıntıları**    |   Transact ' sanal makineler      |
|**Bağlantı noktalarını ve protokolleri kullanımda**    |  Transact ' sanal makineler       |
|**Kullanımdaki her VHD için disk sürümü ve SAS URL'si**   |  Transact ' sanal makineler       |

## <a name="become-a-publisher"></a>Bir yayımcı olur

Bu bölümde, biz adımlarda açıklanmaktadır: Azure Marketi ve AppSource; Publisher'da olacak ve oluşturmak için kullanacağı, bulut iş ortağı portalına erişmek için yayımlama ve teklifiniz korumak. 

### <a name="process-overview"></a>İşlemine genel bakış


|Market kayıt adımları  |Zaman  |Açıklama  |
|---------|---------|---------|
|Microsoft kimlik oluşturun     |   15 dakika      |   İş ortakları için iş ortağı tanımlamak için kullanılan a Microsoft ID olması gerekir. Bu Microsoft ID bulut iş ortağı portalına erişmek için kullanılır.       |
|Market Adaylığı formu     |  1-3 gün       |  Market onay işlemini başlatmak için Adaylığı Form gönderilemedi ortakları gerekir. Form gönderildikten sonra Market hazırlanma ekibi uygulama gözden geçirin ve istek doğrulayın.       |
|Geliştirici Merkezi'nde kaydetme     |    5-10 gün     | Microsoft iş ortağı kayıtlı olduğu ülkede için geçerli bir vergi numarası geçerli yasal bir varlıkla olduğunu doğrulamak için Microsoft Developer Center'da kayıt gereklidir. Geliştirici Merkezi kayıtlı Microsoft Developer ve bunları Azure Geliştirici programı erişim sağlamak iş ortağı olanak sağlar. <br><br>*Not: Market Adaylığı Form tamamlamadıysanız, 99 kayıt ücret ödemeniz istenir. Kullanıma başlama öncesi bu ücret sağlamak için Market Adaylığı formu doldurun ve e-posta yoluyla bir promosyon kodu alırsınız.*  |
|Bulut iş ortağı portalında oturum açın     |  15 dakika       |   İş ortağı onayı Market kendi Adaylığı onaylandıktan ekibinden aldıktan sonra ortak erişim [bulut iş ortağı portalını](https://cloudpartner.azure.com/) etkinleştirilir. İş ortağı Microsoft bulut iş ortağı portalında yayımcı profillerini içine Adaylığı biçimde oturum açmak için kullanılan ID kullanmanız gerekir. Geliştirici Merkezi ile kaydedildikten sonra iş ortağı Geliştirici Merkezi hesabına kendi Azure Market yayımcı yayımlama profiliyle ilişkilendirmek gerekir.      |

#### <a name="create-a-microsoft-id"></a>Microsoft kimlik oluşturun

Tüm Market yayımlama işlemi yoluyla, Market hesabını tanımlayan bir e-posta adresini kullanır. Bu e-posta adresi a Microsoft ID kayıtlı olması gerekir ve her ikisi için kullanılacak [Microsoft Developer Center'da](https://developer.microsoft.com/) ve [bulut iş ortağı portalını](https://cloudpartner.azure.com/). Azure Market ve AppSource Teklifleriniz için yalnızca bir Microsoft ID hesabınızın olması gerekir ve diğer hizmetlerle paylaşılmayan veya sunar öneririz.

Seçilen e-posta adresi şirket etki alanınızda tercihen olmalıdır ve BT ekibi tarafından denetlenir. Lütfen ek gözden geçirin: a Microsoft ID Market hesabı yönet ve ek oluşturma yönergeleri: AAD etki alanlarında federe kimlik oluşturma yönergeleri önceki için Microsoft IDs Kılavuzu 

#### <a name="submit-the-marketplace-nomination-form"></a>Market Adaylığı Form Gönderme
Market ekleme işleminin bir parçası olarak Adaylığı form gönderme uygulamanızı veya hizmet teklifi, şirketinizin bilgilerini ve düzeyde, sağlama desteği hakkında bilgi gönderme yapmanız gerekir.  
Form gönderildikten sonra Market Ekibi uygulama gözden geçirin ve istek doğrulayın. İstek inceledikten sonra onaylanmış bir iş ortağı bulut iş ortağı portalında olmasını tamamlanması gereken sonraki adımda e-postayla bildirilecek. Lütfen, Adaylığı gönderin:

Azure Market Adaylığı: http://aka.ms/listonazuremarketplace   
AppSource Adaylığı: http://aka.ms/listonappsource

#### <a name="register-in-the-developer-center"></a>Geliştirici Merkezi'nde kaydetme

[Microsoft Developer Center'da](https://developer.microsoft.com/) şirketinizin bilgilerini kaydetmek için kullanılır. Registrant şirketin geçerli temsilcisi olması ve kullanıcıların kimliğini doğrulamak için kendi kişisel bilgilerini sağlamanız gerekir. Kaydetme kişinin şirket için paylaşılan a Microsoft ID kullanmanız gerekir ve aynı hesabı kullanılmalıdır [bulut iş ortağı portalını](https://cloudpartner.azure.com/). 

>[!IMPORTANT]
>Oluşturmak çalışmadan önce şirketinizin bir Microsoft Developer Center'da hesabı zaten yok emin olmak için kontrol etmeniz gerekir.

İşlemi sırasında biz şirket adresi bilgileri, banka hesabı bilgileri toplamak ve bilgi vergi. Bu bilgiler genellikle finans bölümüyle veya işletmeyle ilgili kişilerden alınabilir. Ayrıca, çeşitli aşamaları teklif oluşturulmasını ve dağıtımını ilerleme için aşağıdaki yayımcı profili bileşenleri tamamlamanız gerekir:


|**Yayımcı profili**  |**Profili başlatmak için**  |**Hazırlama**  |**Liste ve deneme**  |**Transact**
|---------|---------|---------|---------|---------|
|**Şirket kayıt**     | **Olması gerekir**        |  **Olması gerekir**       | **Olması gerekir**        |  **Olması gerekir**       |
|**Vergi profili kimliği**   |    İsteğe bağlı     |    İsteğe bağlı     |  İsteğe bağlı       | **Olması gerekir**      |
|**Banka hesabı**     |   İsteğe bağlı      |    İsteğe bağlı     |  İsteğe bağlı       |  **Olması gerekir**      |

Eke bakın: Bu işlemi adım adım açıklama için Geliştirici Merkezi'ne kaydetmek yönergeler. 

#### <a name="log-in-to-the-cloud-partner-portal"></a>Bulut iş ortağı portalında oturum açın

Adaylığı onaylanmış ve içinde kaydettiğiniz Market Ekibi'nden gelen onay aldıktan sonra [Microsoft Developer Center'da](https://dev.windows.com), erişmek için bir hesap oluşturulacak [bulut İş ortağı portalı](https://cloudpartner.azure.com). İlk kez oturum açma kimlik bilgileri Adaylığı onay e-postayla dahil edilir. 

Yayımcı profilinizi erişmek için Market hesabınızı (Microsoft ID) kullanın. Bir kez bulut iş ortağı Portalı'nda Geliştirici Merkezi hesabına ilgili Market yayımcı yayımlama profiliyle ilişkilendirmek için son adım olacaktır. Bu ekranın altındaki düğmesiyle yayımcı profilinizde bulut iş ortağı portalında yapılabilir.

Bulut iş ortağı portalının nasıl kullanılacağı hakkında ayrıntılı bilgi için lütfen [öğrenin](https://cloudpartner.azure.com/#Learn) menü portal ve bir belge bölümü tıklatın. 

## <a name="how-to-grow-your-business-with-marketplace"></a>Market ile işinizi büyümeye nasıl

Pazarlama'ndaki en iyi yöntemler aşağıdaki Git Pazar ve Microsoft ortak Sell girişimleri ile başarılı kaydınızı yanı sıra iş Avantajlarınızı Market üzerinden en üst düzeye ayarladığınız yardımcı olur. [Microsoft Partner Network (MPN)](https://partner.microsoft.com/membership) ağ geçidiniz tümüdür Market dışı ilgili pazarlama ve programlama kaynakları. 

Uygulama yayın ve müşteri merkezli talep oluşturma ve iş ortağı katılım Yardım sürücü müşteri büyüme, işiniz için taahhüdünün kalitesi. Bu etkinliklere Microsoft Git Pazar iş artırmasına yardımcı olur ve özellik anahtar çözümleri Market veriş arasında. 

Bu bölümde, bir teklif için pazarlama en iyi yöntemler aşağıdaki denetim listesini göre açıklanmaktadır:

- My listeleme sürücü trafiği ve katılım için iyileştirilmiş.
- I my Web sitesinde Mesajlaşma my Pazaryeri listesi sürücü trafiği için bir benzersiz giriş sayfası yararlanarak oluşturdunuz.
- Böylece müşterilerin, Azure üzerinde çalışırken my teklif beklenmez ı bir Test sürücü veya diğer deneme oluşturdunuz.
- I planlanması ve kendi pazarlama ve promosyonlar Kampanyalar farkındalığı ve katılım için oluşturulmuş.
- İsteğe bağlı müşteri adaylarına ilişkin oluşturma, test veya Uygulamam dağıtmak için birisi görevi gören her zaman, adını ve ilgili kişi bilgilerini alması için bunları etkin.
- İlgili öğrenilen ve bana kullanılabilir olan iş ortağı kaynaklarla bağlı [Microsoft Partner Network (MPN)](https://partner.microsoft.com/membership).

### <a name="create-a-great-listing"></a>Harika bir liste oluşturma

Market, listesindeki ilk etkileşim potansiyel müşteri ile bazen olabilir. Tüm ilk izlenim gibi sağlam hale getirmek istediğiniz ve bir şey kitlenizi izlemek istiyor. Bu ilk izlenim marketi'ndeki harika olmasına yardımcı olmak için yapabileceğiniz temel bazı şeyler vardır!

- **Bulunabilir:** , alıcı aramak için anahtar sözcükleri ve koşulları kullanma, teklif açıklamasını yazın. 
- **Görsel olması:** görüntüleri ve videolar anahtar özelliklerinizi kullanıcıları göster Yardım ve deneyiminizi göstermeye yardımcı olabilir. Ne size yardımcı olur, değer teklifinde teslim veya, alıcının üst sorularını gösterebilir hakkında düşünün.
- **Bir deneyim sağlamak:** müşteriler bunlar satın almadan önce denemek ister. Tanıtımlar denemeler, müşteri adayları oluşturulur ve daha fazla müşteri anlaşmalar sağlama için test sürücüleri kanıtlamak. Daha güçlü deneme sürümü deneyimi, daha güçlü, oluşturacaksınız sağlama sağlayabilirsiniz. Biz sınamayı sonucu (ortalama) adaylarını kapalı % 40 anlaşmalar buldunuz.
- **Bilgi kitlenizi Yardım:** genel bakış alanınızı açık ve basit tutmak için öneririz, ancak aynı zamanda yeterince olduğundan ürününüzle ilgili ek kaynaklar için işaret edecek şekilde yer. Ürününüzü ne yaptığını ve nasıl müşterinizin ihtiyaçlarına uygun olan hakkında kısa ileti göndermek için bu alanı kullanma; Daha fazla bilgi için ek malzemeleri yönlendirmek korkutmasın. Bu öğrenim malzemelerini veya bağlantılar tutarlı bir şekilde eğitimi şekilde stratejisi pazarlama içeriğinizi izleyebilir, ek yol açar.
- **Derecelendirmeleri & incelemeler yararlanın:** Let müşterilerinize satmak ürününüzü sizin için. Müşteri hakları satış büyük sürücü olabilir ve ürününüzle ilgili daha fazla bilgi edinin önce alıcıların nereye görülür. Birden çok güçlü incelemeler de arama sonuçlarında ve anahtar öne çıkan alanlarına teklifiniz kabartma yardımcı olur.

### <a name="build-a-great-landing-page"></a>Derleme harika giriş sayfası
Şirket Web sitesinin giriş sayfasında, isteğe bağlı nesil etkinliklerden Azure Marketi'nde listeleme, Market ağ geçidi ' dir. 

Hedeflerinizi belirlemekle başlayın. Market çözümleri için hedef müşteri olan ve hangi eylemini almak istediğiniz karar verin. Örnek eylemleri 'Test sürücü bizim çözüm' olur ya da 'bir çözüm şimdi alın.' Giriş sayfanız arasında birden çok pazarlama taktiği herhangi bir şey olayları, Web yayınları ve sosyal medya gelen teknik incelemeler, teknik eğitim oturumları ve basın duyuruları için de kullanılabilir. Daha tutarlı, Mesajlaşma ve çalıştığınız arama eylemi için çözümünüzün bulmak için daha kolay olacaktır.

Bir kampanya planı hazır olduğunda, bu en iyi uygulamaları izleyin ve ne giriş sayfanız verimliliğini en üst düzeye çıkarmak için sayfayı oluştururken önlemek göz önünde bulundurun: 


|En iyi uygulama  |Kaçının gerekenler  |
|---------|---------|
|**Çözümünüzü çözdü hangi müşteri sorun iyileştirir ve durum yapmak için Azure nasıl yararlanın**    |  Çözüm ayrıca Azure birlikte çalıştığı yolları iyileştirir başarısız       |
|**Kısa, kolay unutmayın URL'yi oluşturun**    |    Uzun URL'ler etkileyici değil ve bulmak sabit     |
|**İlgili visual içeriği ekleyin: Müşteri referans video ya da çözüm mimarisi olan en iyi uygulamalar**   |   Çok fazla metin kullanarak ince ayar ve sizinle keşfetme durdurma kitlenizi yapabilirsiniz      |
|**Market katalog sayfası ziyaretçileri doğrudan Temizle yapılan bir çağrı eylem oluşturma**    |   Sayfada çok sayıda bağlantılar veya olası Eylemler sahip       |
|**Bir üst bilgi veya açıkça sonuçları bölüm aramanız eyleme yerleştirme**    |  Metin paragrafta listeleme, Market bağlantılara katıştırma       |
|**En iyi anahtar sözcükleri araştırın ve sayfa arama için en iyi duruma getirme**    | Ürün adı varsayarak yüksek arama derecelendirmeleri oluşturur        |
|**Anahtar sözcükleri, reklam kampanyası yararlanan**    |  Birçok farklı anahtar arasında web özellikleri kullanarak reklam yatırımlarınızı dilute       |
|**İlgili ürün adları ve anahtar sözcükler 'üzerinde 'Katlama yerleştirin**     | Kullanıcıların hangi ürün ya da çözüm birtakım sergileyen görmek için kaydırmanız        |
|**Çözümünüzü doğrulamak için marka görüntülerin (örneğin Azure Onaylandı *) kullanın ve yönergelerine uygun olarak Microsoft markalama**    |    Beklemediğiniz onaylanan Microsoft marka görüntülerin kullanma     |

* Daha fazla bilgi edinmek [Azure sertifikalı rozet](https://azure.microsoft.com/support/legal/marketplace/certified-guidelines/ ). [Microsoft Partner Network (MPN)](https://partner.microsoft.com/en-us/membership/how-it-works) üyeleri aracılığıyla markalama için ek kaynaklar erişebilir [marka Merkezi](https://microsoft.sharepoint.com/teams/brandcentral) ve erişim [logosu Oluşturucu](https://logobuilder.partner.microsoft.com) aracı. Birleştirme hakkında bilgi için burayı tıklatın [MPN](https://partner.microsoft.com/en-us/membership/how-it-works). 

### <a name="promoting-your-new-offer"></a>Yeni teklifiniz yükseltme

#### <a name="building-an-effective-marketing-campaign"></a>Etkin pazarlama kampanyası oluşturma
Bir pazarlama kampanyası tanıtım etkinlikler dizisidir veya taktiği pazarlama kitlenizi istenen eylem ya da sonucu yürüten adresindeki hedefler. Kampanyanızı tasarlamadan önce aşağıdakileri yapmalısınız:

#### <a name="know-your-audience"></a>Hedef kitlenizi tanıyın

İlk olarak, kimin alıcı olduğunu doğrulayın ve etkileyen kimdir? Taktiği ve her grup için eylem çağrılarını farklı olabilir. Bu değerlendirme sorular sorun:

- Ne kadar denetim alıcı satın alma karar var mı? 
- Ne kadar etkisini etkileyen var mı? 
- Ne etkileyen etkiler? 
- Bunlar bütçe veya hangi çözümün çekilir etkilemek? 

Bu soruların yanıtlarını bilerek, nerede, ABD Doları kullanacağınızı ve, ABD Doları dağıtma hakkında kararlar almanıza yardımcı olur.

#### <a name="define-where-your-audience-learns"></a>Burada kitlenizi öğrenir tanımlayın

Alıcıların kendi gezisine aşamalardaki % 90'ını bir Market ziyaret zamanına göre ' dir. Alıcılar bu kadar yol karar verme işleminde çözümleri hakkında öğrenme ve seçenekleri önceden değerlendirmek alın. Burada alıcılar ve etkileyici öğrenin olması amaçlayan bir kampanya tasarlamak istersiniz. Her izleyici her endüstri, dikey veya kategori için farklıdır. Kitlenizi çevrimiçi ticari gösterir, e-posta aracılığıyla bilgi vermez veya güvenilen danışmanlar sohbet sosyal medya üzerinden? Nerede ve nasıl kitlenizi öğrenir bağlı olarak etkinlikleri tasarlama ve buna göre pazarlama dolar dağıtmak istersiniz. Bu taktiği birleşimi kampanya stratejinizi olur.

#### <a name="create-clear-campaign-goals"></a>Clear Kampanya hedefler

Market kampanyanızın başarı tanımlayın ve Temizle KPI'ları oluşturmak gerekir. Farklı bitiş hedefle birden çok kampanyaları. Elbette, tüm satış büyümeye istiyoruz. Ultimate end-artan gelir veya müşteri edinme hedeftir. Ancak, pazarlama kampanyalarınızın hedeflere diğer satın alma döngüsü aşamalarında bağlı.

Örneği için yeni ürününüzü bizim Market başlatılan değilse, odak en iyi hedef kitle eğitim ve sağlama oluşturma harcanan bulabilirsiniz. Başarı Market listeden oluşturulan müşteri adayları sayısına göre tanımlanabilir. Bu durumda, taktiği pazarlama (ve giriş sayfasında) müşteriler Market listenizi çizim odaklanmak.

Bir deneme ayarlanmış marketi'ndeki sahip ve ürününüzü katılım ve satın alma önce deneyimi belirli bir düzeyde gerektirdiğini biliyorsanız, deneme sayısı indirilen kampanya amacınız kalmasına neden olabilir. Bu durumda, kampanya taktiği CTA keskin Market bir deneme ve odaklanmak. 

Ürün veya kategori daha iyi bilinen ve satın alma özellikleri deneme adımı atlayın ve hedef kitleye doğrudan pazarında 'hemen satın alın' bağlantıyı doğrudan karar verebilir Market ayarlanan varsa.

Bu durumda doğru sürücü eylem Market satın alma artan ve bir daha da olgun upselling üzerinde kampanya çabalarınız odaklanmaya karar verebilir teklifiniz 's geçmişinde müşteri tabanı noktası. Taktiği odaklanmak 'şimdi marketi'ndeki satın almak için ' müşteriler görünümü teşvik eder. KPI Market üzerinden oluşturulan gelir olabilir.

Bu hedefe ne olursa olsun teklifiniz 's olgunluk ve bu hedefin odaklanan kaldığını ve tümleşik pazarlama taktiği bir dizi eşleme, kuruluşunuzun hedeflerine hizalı kampanya verimliliğini en üst düzeye çıkarma için anahtardır.

Azure Marketi'nde yeni Publisher'da olan bir parçası olarak, bazı ücretsiz Market GTM avantajları sunuyoruz. Önemli ölçüde avantajlar kampanya stratejinizi içinde yararlanan konusunda düşünmelisiniz. Market kampanya hedeflerinizi ve istediğiniz hedef kitle eyleminizi bilmeniz Pazarlama ekibimiz olanak tanır. Biz bu sonuçlara planınıza çalışmak için özelleştirebilirsiniz.

Şablonlar, web içeriği, eğitim ve iş ziyaretiniz yükseltmek için araçlar da dahil olmak üzere ek Git Pazar desteği için [www. MicrosoftGoToMarket.com](https://www.MicrosoftGoToMarket.com) oluşturma ve en iyi yöntemler pazarlama kampanyası ek içerik için ziyaret [akıllı iş ortağı pazarlama](https://partner.microsoft.com/en-US/smart-partner-marketing), Microsoft iş ortağı ağı, bir program.

#### <a name="marketplace-gtm-benefits"></a>Market GTM avantajları

Yeni Market listelerinde ücretsiz Market GTM avantajları almak uygun hale gelir. Listelenen sonra uzmanlarıyla pazarlama ekibimizin size başlatmak için bu etkinlikler ulaşın. Size ulaşmak sonra bizimle bulunmaya olması dışında bir şey yok. 

Sağladığımız etkinlikleri çözüm durumunuzu bizim markette bağlı olarak farklılık gösterir. Avantajları teklifleri için bir deneme sürümü deneyimi ile gelir veya Market içinde özellikleri transact önemli ölçüde artırır.

Bu etkinlikler etkisini en üst düzeye çıkarmak için başlatma planınız yürütmek hazır olmasını öneririz. Giriş sayfanız çoğu bu taktiği yararlanan isteyebilirsiniz. OCP katalog (bir ticari ortak katalog) Microsoft Partner Network üyeleri bir avantajı olmadığını unutmayın. 

![Market GTM avantajları](./media/marketplace-publishers-guide/marketplace-gtm-promotion.png)

Şablonlar, web içeriği, eğitim ve işinizin yükseltmek için Araçlar desteği için ziyaret [için Microsoft Git Market](https://www.microsoftgotomarket.com).

#### <a name="enable-lead-sharing"></a>Sağlama paylaşımını etkinleştir

Market müşterilerinizin kişi bilgilerini alması için bunları sağlama yönetim Market teklifiniz etkin olduğundan emin olun. Bu müşteri adayları temel Destek Hizmetleri için talep oluşturma kampanyalar, satış hareketlerin alan satış personeli ve nasıl teklifiniz gerçekleştirme hakkında bilgi sağlar. 

Bu müşteri adayları kullanmak için en iyi uygulamalar şunlardır:

- Müşteri adayları niteleme ve satış fırsatları olarak Puanlama
- Bunları potansiyel satışlar girmek için eğitimi
- Bu genel pazarlama kampanyası stratejinize çabayla hizalayın

Bu müşteri adayları çok hedeflenen kullanıcı ilgi Market teklifiniz ve teknoloji göstermek ve böylece müşteriler üst düzeyde riskli olabilecek bulmak için bir yol bağlı olarak değerlendirilmelidir. Bir sağlama Marketi'ndeki oluşturulduğunda, benzer bir sağlama Microsoft alan satıcı CRM oluşturulur. 

Ancak, Market müşteri adayları birlikte satış programının bir özellik olan Microsoft seller tam müşteri adaylarını farklıdır. Ortak satış program erişme hakkında bilgi için aşağıya bakın. 

#### <a name="promote-your-business-through-microsoft"></a>Microsoft işletmenize Yükselt

Birçok kişi ve takımlar ortaklarımızın desteklemek ve bizimle satış sahip uyuşmazlık azaltmak için tek amacı olan Microsoft içinde vardır. Bizim Market listelediğiniz göre bizim Market programları ve kaynaklara erişim açtığınız. 

En fazla oturum açmış henüz [Microsoft Partner Network (MPN)](https://partner.microsoft.com), bu ilk adım olmalıdır. MPN Microsoft gezinmek için rehberlik sunar ' öğesinden yeni iş fırsatlarını ekipleri ve çözümleri hakkında bilgi için iş ortakları ile bağlanma ve, skillset artırılmasına yardımcı olmak için eğitim.
Daha fazla iş ortağı avantajları erişimi açmak için alabilir en iyi sonraki adımlar ve kaynaklar:

1.  Dengeleme, [çekirdek avantajları](https://partner.microsoft.com/en-US/membership/core-benefits) bizim Microsoft iş ortağı ağı bir parçası olarak, bir dizi zamandan tasarruf yardımcı olabilecek çekirdek avantajları almak ve yeteneklerinizi, daha iyi güçlendirmek sırasında para hizmet müşteriler ve yapı bağlantıları tam iş potansiyelinize ulaşmak.

2.  Kazanma, [bulut platformu uzmanlığı](https://partner.microsoft.com/en-us/membership/cloud-platform-competency) bir uzmanlığı getirisi yanı sıra kendiniz Microsoft'un korunmalarını iş ortağı ağı içinde ayırt olanak tanır Pazar teknik uzmanlığı ve müşteri başarılı göstermektedir. Bir uzmanlığı getirisi Ayrıca ortak satış gibi birçok anahtar iş ortağı programı için ön koşuldur.

3.  Hale [ortak satış hazır](https://partner.microsoft.com/en-US/reach-customers/promote-your-business) Bu program doğrudan Microsoft satıcılar ve hedef müşteriye fırsatları üzerindeki diğer iş ortakları ile birlikte çalışın ve planlama hesap olanak sağlar. Çözümünüzü kendi çözüm kataloğunda bizim satıcıları için görünür hale gelir ve işbirliği ve sizinle kazanma bizim satıcıları ödülünü.

#### <a name="merchandising"></a>Satış 
Yayımlama işleminin bir parçası olarak ne tür bir teklif oluşturmak için kabul ve Azure Marketi'nde teklifiniz için bir kategori seçin fırsatı vardı. Olası müşteriler için doğru şekilde görünmesi için çözümünüz için doğru bir kategori seçtiğinizden emin olun. 

Deneme ve Transact işlevselliği etkinleştirdiğinizde, Azure Marketi'nde, öne çıkan uygulamalar için uygun olur. Market GTM avantajları öne çıkan uygulamalar Go Market Avantajlarınızı bağlamında nasıl uyduğunu anlamanıza gözden geçirin. 

Öne çıkan uygulamalar dayalı üzerinde ve uygulamaları en iyi müşteri deneyimi katılım pazarlama kullanıcı ve yararlanır yüksek kaliteli için iş ortağı sağlamak seçilir. Bu listenin netlik, teknoloji güvenilirliğini ve müşteriler platform kullanımı büyüme ve yüksek kaliteli pazarlama malzemeleri oluşturma ile katılım düzeyine içerir. 

Öne çıkan uygulamanızın olasılığını en üst düzeye çıkarmak, Market teklifi başarılı şekilde yatırım ve büyük müşteri deneyimine teklifinizin emin olmak için aşağıdaki yaklaşımlardan göz önünde bulundurun: 

- Pazarlama yapıtları görüntüleme ve karşıya doğru olduğundan emin olun
- Katılma [Microsoft iş ortağı ağı](https://partner.microsoft.com/membership) ve iş ortağı ekosistemi ile göster
- Yüksek kaliteli talep oluşturma Kampanyalar oluşturarak yüksek kaliteli trafiği teklifiniz Azure markette sürücü
- Tüm Azure çözümleri ve uygulamalarını Azure Marketi'nde kullanılabilir olduğundan emin olun
- İsteğe bağlı olarak sağlayarak yüksek kaliteli Müşteri Hizmetleri yürüten ve ürün üzerinde zamanında güncelleştirmeleri sağlayarak Azure Marketi Teklifleriniz tüketiminin Büyüt

## <a name="analytics-and-reporting"></a>Analiz ve Raporlama

Bulut iş ortağı portalının Öngörüler bölümünde teklifiniz 's performans üst düzey bir genel bakış görürsünüz. Bu bölümde raporları şunlardır:  
- Siparişleri Özet anlık görüntü
- Kullanım
- Dağıtımlar
- Müşteri eğilimleri Öngörüler giriş sayfasındaki
- Ayrıntılı siparişler, kullanım ve müşteri verileri
- Siparişleri ve aylık özet veya altı aylık eğilimi görünüm olarak gösterilen kullanımı
- Kullanım/siparişleri birkaç ölçüte göre dilimlenebilir

Müşterilerinizi karşılaştırma ve, satıcıları dengelemek için ayrıntılı raporlar müşteri bilgileri, şirket adı ve coğrafi konumuna posta kodu gibi gösterir. Aşağıdaki listede, müşterilerinizin hakkında sağladığımız özel öznitelikler içerir:
- Satıcı
- Ad
- Soyadı
- E-posta
- Şirket Adı
- İşlem tarihi
- Abonelik Adı
- Azure abonelik kimliği (yalnızca PAYG Müşteriler)
- Bulut örneği adı
- Sıra sayısı
- Müşteri Ülke
- Müşteri Şehir
- Müşteri iletişimi kültür
- Müşteri posta kodu

Bu raporlar bilgileri için en iyi uygulama, kendi iç verilerle bağdaştırılması ve pazarlama kampanyası eylemlerinizi öncelik yardımcı olmak için kullanmaktır. 

Bulut iş ortağı portalı Öngörüler Analytics bölümü, uygulama Ayrıntıları sayfasında trafiği görmenize olanak sağlayan bir Power BI göre zengin bir Pano sağlar. Bu Pano için yeni özellikler devam eden bir şekilde alınıyor. Ayrıca bulut iş ortağı portalı içinde Microsoft Campaigns, bir mekanizma olarak kampanyaların ayarlama ve bunları portalın içinde izleme belgesidir.

## <a name="getting-support"></a>Destek alma

Destek seçenekleri için Azure Marketi listesidir:

**Azure Market genel sorgular:**
|Destek kanal |Açıklama |
|---------|---------|
|E-posta:cloudmarketplace@microsoft.com     |  Onboarding desteği dağıtım listesi. Ekleme istekleri, Keşif oturumları ve iş ortakları ile Mimari Tasarım oturumları (ADS) ayarlamak için kullanılır.        |

**Azure Market destek yayımlama:**

|Destek kanal  |Açıklama  |
|---------|---------|
|E-posta: 
azurecertified@microsoft.com |   İş ortakları Azure Marketi'nde yayımlama uygulamaları için desteği sağlar. İş saatleri Pasifik saat diliminde.      |
|E-posta:
AzureMarketOnboard@microsoft.com |   Azure Market çözüm Adaylığı Form ve işlem için destek sağlar. İş saatleri Pasifik saat diliminde.      |
|E-posta: 
Amp-testdrive@microsoft.com |   Test sürücüleri ekleme erişim sağlar. İş saatleri Pasifik saat diliminde. | 

**Azure Market Portal desteği:**

|Destek kanal  |Açıklama  |
|---------|---------|
|E-posta [desteği](https://go.microsoft.com/fwlink/?linkid=844975)    |   Market yayımlama portalında desteği. 7/24 destek sağlanır.        |

**Teknik Destek**


|Destek kanal  |Açıklama  |
|---------|---------|
|Kayma: [Market kayma katılma](https://join.marketplace.azure.com)    |   Teknik sorunlar iş ortaklarıyla desteklemek için slack ortamı. Bu ortamda çalışmakta orada 350 + ortakları.        |
|MSDN Forumlarında: [Market](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=DataMarket)     | Microsoft Developer Network forum.         |
|StackOverflow: [Azure](https://stackoverflow.com/questions/tagged/azure)     |    StackOverflow Web sitesi çözümleri almak ve her şeyi Azure ve AMP ilgili hakkında sorular sormak için bir ortam sağlamak birden çok bölümü vardır:<ul><li>StackOverflow: [Azure Market](https://stackoverflow.com/questions/tagged/azure-marketplace)</li><li>StackOverflow: [Azure Kaynak Yöneticisi](https://stackoverflow.com/questions/tagged/azure-resource-manager)</li><li>StackOverflow: [Azure sanal makineler](https://stackoverflow.com/questions/tagged/azure-virtual-machine)</li></ul> |


**Pazarlama kaynakları**

|Destek kanal  |Açıklama  |
|---------|---------|
|E-posta:cosell@microsoft.com    |  Onboarding işlemleri ve ortak satış programı ile ilgili sorular için destek sağlar. Pasifik saat diliminde temel.        |
|E-posta:gtm@microsoft.com    |  Git Pazar avantajları ve program sorular için destek sağlar. İş saatleri Pasifik saat diliminde.        |
|E-posta:CEBrand@Microsoft.com     |  Azure logolar ve markalama için marka kullanımı hakkındaki soruları yanıtlar.       |

## <a name="guidelines-and-how-tos"></a>Kılavuzları ve nasıl yapılır?

### <a name="guidelines-for-creating-a-microsoft-id-to-manage-an-azure-marketplace-account"></a>Azure Market hesabını yönetmek için a Microsoft Id oluşturma yönergeleri

Hesap açılan Microsoft hesabı oturum açarak hesabınıza erişmek birden çok kişi gerekecekse bir şirket hesabı oluştururken bu yönergeleri izleyin.

>[!IMPORTANT]
>Birden çok kullanıcıların Geliştirici Merkezi hesabınızda erişmesine izin vermek, Azure Active Directory hesabı oturum bireysel oturum açarak erişebilir tek tek kullanıcılar rolleri atamak için kullanmanızı öneririz Azure AD kimlik. Daha fazla bilgi için lütfen inceleyin [AAD Federasyon etki alanlarıyla Kılavuzu](#guidance-with-aad-federated-domains). Şirketinizin etki alanı, ancak tek individual'for örnek ait bir e-posta adresi kullanarak Microsoft hesabınızı oluşturmak windowsapps@fabrikam.com.

- Bu Microsoft hesabını geliştiricileri en küçük olası sayısına erişimi sınırlayın.
- Geliştirici hesabını erişmesi gereken herkes içeren bir şirket e-posta dağıtım listesini ayarlamak ve güvenlik bilgilerinizi bu e-posta adresi ekleyin. Bu listede gerektiğinde güvenlik kodlarını almak ve Microsoft hesabınızın güvenlik bilgilerini yönetmek için tüm çalışanlar sağlar. Bir dağıtım listesi oluşturarak uygun değilse, tek tek e-posta hesabının sahibi erişmek ve güvenlik kodunu (örneğin, yeni güvenlik bilgileri hesabınıza eklendiğinde veya yeni bir CİHAZDAN zaman erişilmelidir) istendiğinde paylaşmak kullanılabilir olması gerekir.
- Uzantı gerektirmez ve anahtar takım üyeleri için erişilebilir olan bir şirket telefon numarası ekleyin.
- Genel olarak, güvenilen cihazları şirketinizin Geliştirici hesabınızla oturum açmak için kullandığınız geliştiriciler vardır. Tüm anahtar ekip üyelerinin bu güvenilen cihazlara erişimi olmalıdır. Bu hesap erişirken gönderilmek üzere güvenlik kodlarını gereksinimini azaltır.
- Güvenilir olmayan bir Bilgisayardan hesabına erişime ihtiyacınız varsa, en fazla beş geliştiriciler bu erişimi sınırlayın. İdeal olarak, bu geliştiriciler hesabına erişmek ve bu da aynı coğrafi paylaşan ve ağ konumu makinelerden.
- Şirketinizin güvenlik bilgisi en sık gözden [https://account.live.com/proofs/Manage](https://account.live.com/proofs/Manage) tüm geçerli olduğundan emin olmak için.

Geliştirici hesabınızda öncelikle Güvenilen bilgisayarlarından erişilmesi. Hesap hafta başına oluşturulan kodlarını sayısına bir sınır olduğundan bu önemlidir. Ayrıca, en sorunsuz oturum açma deneyimi sağlar.
Ek Geliştirici hesabı yönergeleri ve güvenlik hakkında daha fazla bilgi için tıklatın [burada](https://docs.microsoft.com/en-us/windows/uwp/publish/opening-a-developer-account).

### <a name="guidance-for-microsoft-ids-in-aad-federated-domain"></a>Etki alanı aad'de Microsoft kimlikleri için yönergeler Federasyon

Şirket hesabınızı kullanarak Federasyon [Azure Active Directory](https://docs.microsoft.com/en-us/azure/active-directory/), ve bir şirket e-posta adresiyle a Microsoft ID oluşturmayı denerseniz bir hata döndürecektir. İlk olarak bir hata alırsanız, bu durumda olduğundan emin olmak için BT ekibiniz denetleyin. Bu bilinen bir sorundur ve çözümlemek için çalışıyoruz. Geçici çözüm aşağıda verilmiştir:

Yeni bir e-posta adresi oluşturun öneririz  **@outlook.com**  etki alanı. Şu adımları uygulayın:

1. Git [https://signup.live.com/signup](https://signup.live.com/signup) seçip **yeni bir e-posta adresi alın**


2. Yeni bir e-posta adresi oluşturun ve bir parola girin. Bu yeni a Microsoft ID ve bir e-posta kutularına outlook.com hizmetinde oluşturur. Hesap oluşturulduktan kadar kayıt işlemine devam edin.

>[!IMPORTANT]
>Geliştirme Merkezi'nde kayıt için kullanacağınız (bir dağıtım listesi kişilere bağımlılığı kaldırmak için önerilir) e-posta kimliği veya dağıtım listesi konumunda olduğundan, ilk bir Microsoft hesabı kayıtlı emin olun. Aksi takdirde, daha sonra lütfen bu kullanarak kaydettirin [bağlantı](https://signup.live.com/signup?uaid=e479342fe2824efeb0c3d92c8f961fd3&lic=1).

Ayrıca, **Microsoft şirket etki alanı altında herhangi bir e-posta kimliği** Dev Center kaydı için kullanılamaz.

Microsoft ID bu hesapla oluşturulduktan sonra hesabında oturum açma [posta kutusu](https://outlook.live.com/owa/) ve tüm e-postaları üzerinde bu posta kutusu e-posta adresi taşımak için bir e-posta iletme kuralı oluşturma Azure Marketi yönetmek için etki alanında oluşturulan hesabı. Bu başvuruda [bağlantı](https://support.office.com/en-us/article/Use-rules-in-Outlook-Web-App-to-automatically-forward-messages-to-another-account-1433e3a0-7fb0-4999-b536-50e05cb67fed) Outlook.com'da bir e-posta iletme kuralı oluşturma hakkında.

Bu son adım tamamlandıktan sonra tüm e-postaları/iletişimlerinden etki alanınızdaki e-posta hesabınıza gönderilen Microsoft ID sahip olur. Kullanmanız gerekecektir @outlook.com e-posta adresi Microsoft Developer Center'da ve bulut iş ortağı portalına hem de kimlik doğrulaması.

### <a name="instructions-on-how-to-register-in-the-development-center"></a>Geliştirme Merkezi'nde kaydetmek yönergeler

1. Yeni bir Internet Explorer InPrivate veya Incognito gözatma oturumu Chrome, kişisel hesabına açmadınız emin olmak için açın.
2. Git [http://dev.windows.com/registration?accountprogram=azure](http://dev.windows.com/registration?accountprogram=azure) kendiniz bir satıcı geliştirme Merkezi'ndeki olarak kaydetmek için. Lütfen devam etmeden önce aşağıdaki önemli not okuyun.

   ![Geliştirici Merkezi e-posta](./media/marketplace-publishers-guide/registerdevcenteremail.png)

3. Telefon numarası veya e-posta adresi aracılığıyla kimliğinizi doğrular "hesabınızı korumamıza yardımcı olun" Sihirbazı tamamlayın.
4. "Kayıt hesabı bilgi" bölümünde seçin, **hesap ülke/bölge** açılan menüden.

   ![Hesap bilgileri](./media/marketplace-publishers-guide/devcenterregistrationaccountinfo.png)
   
   >[!WARNING]
   >"Satış Yeri" ülkelerde: hizmetlerinizi Azure Market'te satmak için kayıtlı varlığınız onaylanan 'satış yeri' ülkelerin yukarıdaki birinden olması gerekir. Ödeme ve vergi nedeniyle kısıtlamadır. Daha fazla bilgi için [Market katılım ilkeleri] https://azure.microsoft.com/support/legal/marketplace/participation-policies/ bakın.

5. "Hesap türünüz" olarak seçin **şirket** ve ardından **sonraki** düğmesi.

   >[!IMPORTANT]
   >Hesap türleri ve seçmek için en iyi olduğu daha iyi anlamak için lütfen sayfayı görüntülemek [hesap türlerini, konumlarını ve ücretleri](https://docs.microsoft.com/en-us/windows/uwp/publish/account-types-locations-and-fees).

6. Girin **yayımcı görünen adı**, tipik olarak, şirketinizin adı.

   >[!TIP]
   >Teklifiniz listelenen gider sonra geliştirme Merkezi'ne girilen yayımcı görünen ad Azure Marketi'nde görüntülenmez. Ancak bu kayıt işlemini tamamlamak için doldurulması gerekir.

7. Girin **kişi bilgisi** Hesap doğrulama için.

   >[!IMPORTANT]
   >Bu şirket için bizim doğrulama işlemi Geliştirici Merkezi'nde onaylanması için kullanılır çünkü doğru kişi bilgilerini sağlamanız gerekir.

8. Kişi bilgilerini girin **şirket onaylayanı**. Şirket onaylayanı geliştirme Merkezi'nde kuruluşunuz adına bir hesap oluşturmak için yetkilendirilmiş olduğunuzu doğrulayın kullanıcıdır. Tıklayın **sonraki** taşımak için **"Ödeme bölüm"** işiniz bittiğinde.

   ![Geliştirici Merkezi ödeme](./media/marketplace-publishers-guide/devcenterregistrationpayment.png)

9. Hesabınız için ödeme yapmak için Ödeme bilgilerinizi girin. Kayıt maliyetini kapsayan bir promosyon kodu varsa, burada girebilirsiniz. Aksi takdirde, kredi kartı bilgileri veya desteklenen pazarda PayPal sağlar. İşiniz bittiğinde, tıklatın **sonraki** üzerinde taşımayı **"Gözden geçirme ekran."**

   ![Geliştirici Merkezi ödeme](./media/marketplace-publishers-guide/devcenterregistrationpayment2.png)

10. Hesap bilgilerinizi gözden geçirmek ve her şeyin doğru olduğundan emin olun. Ardından, okuma ve hüküm ve Microsoft Azure Market yayımcı Sözleşmesi koşullarını kabul edin. Okuma ve bu koşulları kabul belirtmek için onay kutusunu işaretleyin.
11. Tıklatın **son** kaydınızı doğrulamak için. E-posta adresinizi bir onay iletisi göndereceğiz.
12. Yalnızca boş teklifleri yayımlama planlıyorsanız tıklatın **Git [bulut iş ortağı portalını](https://cloudpartner.azure.com)**.

Ticari (TRANSACT) teklifleri--yayımlama planlıyorsanız, örn., sanal makine saatlik faturalandırma modeliyle--sunar tıklatın **hesap bilgilerinizi güncelleştirmeniz** burada doldurmanız gerekir vergi ve bankacılık bilgilerini, geliştirici Merkezi hesabı.

Vergi ve banka bilgilerinizi daha sonra güncelleştirme tercih ederseniz, sonraki bölüme taşıyabilirsiniz. 

>[!IMPORTANT]
>Ticari (TRANSACT) teklifleri durumunda vergi ve banka hesabı bilgileri tamamlamadan üretime Teklifleriniz itme mümkün olmaz.

#### <a name="add-tax-and-banking-information"></a>Vergi ve bilgi bankacılık ekleme

Ticari teklifleri satın alma için yayımlamak isterseniz, ödeme ekleyin ve bilgi vergi ve geliştirici Merkezi'nde doğrulama için gönderme gerekir. Yayımlarsanız yalnızca boş veya bu bilgileri eklemek gerekmez sonra KLG sunar. Daha sonra ekleyebilirsiniz, ancak vergi bilgileri doğrulamak için biraz zaman alabilir. Ticari teklifleri satın alma için sunacaktır biliyorsanız, mümkün olan en kısa sürede eklemenizi öneririz.

**Banka bilgileri**
1. Microsoft Developer Center Microsoft hesabınızla oturum açın.
2.  Tıklatın **ödeme hesap** soldaki menüde altında **ödeme yöntemi seçin** tıklatın **banka hesabı** veya **PayPal**.

   >    [!IMPORTANT]
   >    Müşterilerin Market satın alması ticari teklifleri varsa, bu bu satın alma işlemleri için ödeme almanıza hesabıdır.

3. Ödeme bilgilerini girin ve memnun olduğunuzda Kaydet'i tıklatın.

   >    [!IMPORTANT]
   >    Güncelleştirmek veya ödeme hesabınızı değiştirmek gerekiyorsa, yukarıda yeni bilgiyle geçerli bilgilerinizi değiştirmeyi aynı adımları izleyin. Ödeme hesabınızı değiştirme, ödemeler tek bir ödeme döngüsü tarafından gecikmeye yol açabilir. Bu gecikme, ödeme hesabınızı ilk ayarladığınızda komutlarında yaptığımız gibi hesabı değişikliğini doğrulamak ihtiyacımız oluşur. Hesabınızı doğrulandıktan sonra devam ödenen için tam tutar; son ödeme geçerli ödeme için bir döngü eklenir.

4. Tıklatın **sonraki.** 

**Vergi bilgileri**

1. Oturum [Microsoft Developer Center'da](https://dev.windows.com) Microsoft hesabınıza (gerekirse).
2. Tıklatın **vergi profili** soldaki menüde.
3. Üzerinde **, vergi formu ayarlama** sayfasında ülke veya bölge kalıcı residency olduğu seçin ve ardından ülke veya bölge birincil vatandaşlığa benzer tutun yeri seçin. **İleri**’ye tıklayın.
4. Vergi ayrıntılarını girin ve ardından **sonraki.**

   >    [!WARNING]
   >    Microsoft Developer Center'da hesabınızda vergi ve banka hesabı bilgileri tamamlamadan, ticari sunmaktadır üretime itme mümkün olmaz.

Lütfen bir destek bileti Geliştirici Merkezi kayıt ile ilgili sorununuz olursa, aşağıdaki gibi oturum:

1. İçin destek bağlantı https://developer.microsoft.com/windows/support gidin
2. Altında **bize** bölümünde, düğmeyi tıklatın **bir olay gönderme** ekran görüntüsü aşağıda gösterildiği gibi.
3. "Yardım ile Geliştirme Merkezi" olarak seçin **sorun türü** ve "Yayımla uygulamaları ve yönetme" olarak **kategori**. Bundan sonra "Başlangıç e-posta." düğmesini tıklatın

   ![Geliştirici Merkezi sorunu](./media/marketplace-publishers-guide/devcentersubmitincident.png)

4. Bir oturum açma sayfası sağlanacaktır. Tüm Microsoft hesabı oturum açma kullanın. Bir Microsoft hesabınız yoksa, bu bağlantıyı kullanarak bir tane oluşturun. 
5. Sorun ayrıntıları doldurun ve tıklayarak bilet gönderme **gönderme** düğmesi.