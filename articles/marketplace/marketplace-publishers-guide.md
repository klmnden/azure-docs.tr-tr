---
title: Azure Market ve AppSource yayımcı Kılavuzu
description: Adım Adım Kılavuzu ve denetim listeleri yeni yayımcılar Azure Marketi'nde yayımlama
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
ms.date: 01/18/2018
ms.author: ellacroi
ms.openlocfilehash: e9343b4a0049b2eea30f903159fdeff0ae7ff851
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2018
---
# <a name="azure-marketplace-and-appsource-publisher-guide"></a>Azure Market ve AppSource yayımcı Kılavuzu

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

## <a name="benefits-of-participating-in-the-marketplace"></a>Market katılan avantajları

Azure Market ve AppSource Microsoft ile birleşik Git Pazar etkinlikler için başlatma noktaları ve iş büyüme flywheel kümesidir. Başlatma yükseltme, isteğe bağlı oluşturma ve satış ve pazarlama Eklem kullanarak bulut iş altyapınız merkez Market listelerini hale getirebilirsiniz. Market katılan için hiçbir ücretleri vardır. Amacımız en iyi çözüm ve bizim iş ortağı ekosistemi sunar Hizmetleri ile Microsoft müşterileri bağlamaktır.

İşinizi büyümeye Market özelliklerinden yararlanmak:

- **Müşteri adayları ve satış fırsatları oluşturmak**. Genişletilmiş bir Portföy çözümleri ile yeni pazarlara Microsoft bulut platformunda girin. Yukarı satış ve çapraz satış Market teklifleri. 
- **Var olan ve yeni müşterilerle anlaşma boyutunu artırın ve iş değerini artırın**. Anlaşma boyutu büyümesine ve buluta iş yükleri taşırken, müşteri sorunlarını çözün. Satış uzatmaya ve bu hedef belirli iş yükleri ve endüstri senaryoları eksiksiz çözüm satış anlaşma Karlılık artırın.
- **Eyleme dönüştürülebilir Öngörüler elde**. Sizi başarınız, bizim başarımızdır. Bulut iş ortağı Portalı aracılığıyla, listelerinin performansına Öngörüler alın. Ne gerçekleştiriyor öğrenin, ne sizi oluşturulur ve kampanya etkinliklerinizi en üst düzeye çıkarmak nasıl.

>[!NOTE]
>Office genişleten uygulamalar Öngörüler Office uygulamaları için yayımlama işlemi aracılığıyla erişirsiniz.

## <a name="azure-marketplace-and-appsource-storefronts"></a>Azure Market ve AppSource veriş

Microsoft iş ortaklarının teklifler listesinde, denemeler etkinleştirmek ve doğrudan Microsoft müşterileri ve ekosistemi ile transact izin iki ayrı Market veriş sağlar: [Azure Marketi](https://azuremarketplace.microsoft.com) ve [AppSource](https://appsource.microsoft.com). Bu veriş bulmak için deneyin ve uygulamaları ve bunların dijital dönüştürme hızlandırmak hizmetleri satın müşteriler etkinleştirin. İşlerini Microsoft müşterileri erişimi artırarak büyür ve iş ortağı ekosistemlerini yayımcılar yardımcı olurlar.
 
Market veriş gereksinim duydukları şeyleri bulmalarına müşterilere yardımcı olmak için izleyiciler ve Microsoft bulut ürünleri hizalanır. Her mağaza yayımlama yatırımınızı en üst düzeye çıkarmanıza yardımcı olmak için özelleştirilmiş yayımlama seçeneklerini sunar. Bu seçenekler aşağıdaki tabloda özetlenmiştir:


|          |Azure Market |AppSource  |
|---------|---------|---------|
|Hedef kitle     |BT uzmanlarının ve geliştiricilerin (DBAs, SecOps, DevOps uzmanı roller içerir)    | İş karar alıcılar (üretim, muhasebe tedarik uzmanı roller içerir)      |
|İle oluşturulan veya genişletmek için     |Azure         | Azure, Dynamics 365, Office 365, Power BI, PowerApps       |
|Çözüm ve hizmet türleri     |  Altyapı çözümleri, Profesyonel Hizmetler   | Tamamlanmış iş bulut uygulamalarını, Office 365 eklentiler, Profesyonel Hizmetler        |
|Yayımlama seçenekleri     |  Me danışmanlık hizmetleri sunmak, kişi deneme, sanal makine, çözüm şablonu, yönetilen uygulama       |  Ücretsiz deneme, şimdi al test sürücüsü, ilgili kişi benim danışmanlık hizmetleri sağlar      |
|Uygulama Deneyimi uygulamalar ve hizmetler kendi uygulama bağlamında kullanıcılara erişim vermek için  | Azure portalı ve Azure CLI         | Office 365, Dynamics 365, Power BI, Office istemci uygulamaları       |

## <a name="using-the-storefronts"></a>Veriş kullanma

Her mağaza benzersiz Müşteri gereksinimlerine işlevi görür. Bu rol için doğru çözüm ya da müşteri kim tabanlı hizmet sunmak için hedefleme sağlar.

BT uzmanları ve bulut geliştiriciler bulmak, deneyin ve Iaas, SaaS ve PaaS çözümleri satın almak için Azure Market üzerinden göster:


|Müşteri gereksinimi  |Azure Market |
|---------|---------|
|**İşletme ve teknik gereksinimlerini karşılamak için fazladan bulut platformu işlevselliği talep**     |  Tamamlayıcı uygulamaların ve hizmetlerin Azure üzerinde çalıştırmak için en iyi duruma getirilmiş büyüyen bir Portföy sunar       |
|**Doğru uygulama veya hizmet bulmak için zor bulduğu**    |  Bulmak, deneyin ve çözümleri ve Azure hizmetleri satın almak için bir tek bir noktadan Mağazanız sağlar        |
|**Üçüncü taraf uygulamalar ve hizmetler için ölçeklenebilir dağıtım mekanizması gerekiyor**   | Oluşturma ve üçüncü taraf uygulamalar ve hizmetler için ölçeklenebilir dağıtımlar yapılandırılmasını sağlar        |
|**Yeni uygulamaları ve Hizmetleri Tümleştirme ve var olan çözümler ile çalışmak için gerektirir**  |   Azure üzerinde var olan çözümler ile üçüncü taraf uygulamaları ve Hizmetleri kolayca tümleşir      |

Sürücü iş sonuçları yardımcı değerine süresini azaltmak için iş kullanıcıları bulmak için deneyin ve iş kolu satır SaaS uygulamaları ve uygulama hizmetleri almak AppSource üzerinde göster: 


|Müşteri gereksinimi  |AppSource  |
|---------|---------|
|**Microsoft ürünleriyle çalışmak işletme çözümleri zaten kullandıkları** | Müşterilerinin Microsoft bulut uygulamaları ve teknolojileri genişletmek için üçüncü taraf uygulamaları ve hizmetleri kullanmasını sağlar       |
|**Doğru çözüm ya da uygulama hizmeti kolayca bulmak için yeteneği.**    |   Bulmak, deneyin ve uygulamaları ve Hizmetleri, eklentiler ve daha fazla bilgi almak için bir tek bir noktadan Mağazanız sağlar      |
|**Kendi özel iş güçlükleri sektöre özgü iş satır çözümleri**   | Adres belirli gereksinimleri arasında çok sayıda sektörü yardımcı olmak için tamamlanmış uçtan uca endüstri çözümleri sağlar     |
|**Verimlilik, verimliliğini ve iş öngörüleri geliştirmeye yardımcı olmak için uygulamalar**    | Uygulamalar için Müşteri Hizmetleri, ik, işlemleri ve çok daha fazlasını dahil olmak üzere iş satırları sağlar        |
| **Uygulamaları kendi benzersiz durumunuza uyarlamak için deneyimli uygulama iş ortağı** | Bir katalog Dynamics 365, Power BI, PowerApps ve üçüncü taraf uygulamalar tahmin edilebilir sonuçlar teslim iş kullanıcıları yardımcı olmak için göre çözümleri için danışmanlık hizmetleri tekliflerinin sağlar |

## <a name="understanding-the-differences-between-storefronts"></a>Veriş arasındaki farklar anlama

Teklifiniz için hedef kitleyi tanımlayan ile mağaza başlatılır seçme. Azure Market, BT uzmanlarının ve geliştiricilerin ihtiyaçlarını hizalanır. AppSource iş Kullanıcıların ihtiyaçlarını hizalanır. Çözümünüzün her iki izleyicileri hedefliyorsa, her iki veriş listesinde yalnızca bir kez yayımlamak gerekir.
 
Her mağaza ek yararları göz önünde bulundurun:

|StoreFront avantajı  |Azure Market  |AppSource   |
|---------|---------|---------|
|**Faturalama esneklik**    | Sanal makineler için Microsoft Kurumsal anlaşmalarındaki (EAs) veya web doğrudan satış modelleri Kullandıkça Öde fatura seçenekleri kullanın. Fiyatlandırma seçeneklerini de bir sunum perpetually boş olduğu bir ücretsiz katmanı abonelik içerir. Ayrıca daha sonra Ücretli bir aboneliğe dönüştürülür, sınırlı bir süre için promotionally ücretsiz deneyin şimdi aboneliği içerirler. Kendi etkinleştirme lisansını yayımcılar desteklemek için bir seçenek de getirin. <br><br>Fatura iki seçenek için sanal makineleri Azure yoluyla uygulamaları (örneğin, bir çözüm şablonu veya yönetilen bir uygulama) dağıtıldığı senaryolarda tüm sağlanmış Azure kaynaklarını doğrudan müşteriye faturalandırılır. | AppSource bir deneme sürümü deneyimi sağlama sunar, ancak şu anda yayımlama seçeneği ticaret etkin sağlamaz. Hiçbir ek yatırım veya değişikliklerle geçerli sıralama ve fatura altyapısını kullanabilirsiniz.        |
|**Diğer ortaklarıyla bağlantı dönüştüğünde, Yönetim**     |Azure Marketi teklifi için bir hizmet sağlayıcısı veya teslim ortakları bağlamak yayımcı şu anda izin vermez, ancak bu işlev içinde 2018 başlatacak.         |  Bağımsız yazılım satıcıları, sistemleri tümleştiricileri ve yönetilen hizmet sağlayıcıları için belirli uygulama senaryoları bağlanabilir. Bu özelliği yeni müşterilere işbirliğine dayalı satış destekler.      |
|**Otomasyon**     |    Azure Marketi teklifi için bir hizmet sağlayıcısı veya teslim ortakları bağlamak yayımcı şu anda izin vermiyor.     | Eklenti sağlamayla otomatik SaaS yararlanın. SaaS tabanlı veri toplama ve dağıtım senaryolarını otomatikleştirmek için çözüm şablonları kullanın.        |Bağımsız yazılım satıcıları, sistemleri tümleştiricileri ve yönetilen hizmet sağlayıcıları belirli uygulama senaryoları için işbirliğine dayalı yeni müşteriler için satış destekleme bağlanabilir.
|**Birden çok bulut türleri**     |   Genel Bulut ve şirket içi çözümleri Azure yığın aracılığıyla yayımlamak veya Azure kamu ve bölgesel Bulutlar, Çin ve Almanya dahil olmak üzere yayımlayabilirsiniz.      |    AppSource şu anda Azure yığını, Azure kamu veya bölgesel Bulutlar için destek sağlamaz.     |
|**İçerik sunu müşterilere**     |  Çözümünüzü bağlamsal arama (sanal makineler ve çözüm şablonlar) için Azure portal deneyiminde kullanılabilir duruma getirin.       |  Daha fazla müşterileri Dynamics 365, Power BI ve Office 365 gibi Microsoft ürünleri için uygulama deneyimi ile ulaşabilirsiniz.    |

## <a name="publishing-options"></a>Yayımlama seçenekleri

Her mağaza birden fazla yayımlama seçeneklerini ve teklif türlerini destekler. En iyi uygulama ve hizmet ayrıntıları temsil eden bir teklif türü seçin. Tüm yayımlama seçeneklerini iş ortakları, paylaşım sağlama için erişim sahibi olursunuz. 

|**Yayımlama seçeneği**  | **Teklif türü** | **Mağaza**  |
|---------|---------|---------|
|**List**    |    Danışmanlık Hizmetleri benimle iletişim     |  Azure Market, AppSource       |
|**Deneme**   |     Ücretsiz deneme sürümü, SaaS deneme, etkileşimli demo, sürücü sınayın.    |  Azure Market, AppSource       |
|**İşlem**     |   Sanal makine, çözüm şablonu, uygulama yönetilen      |    Azure Market     |

### <a name="list"></a>Liste

Deneme düzeyi veya işlem düzeyindeki katılım uygun olmadığı durumlarda kişi benim kullanın. Bu yaklaşımın avantajı, hemen iş flywheel başlatmak için temel anlaşmalar becerilerin geliştirilmesi müşteri adayları almaya başlamak bir pazar çözümü olan yayımcı etkinleştirir ' dir. Ancak, dezavantajı müşteri katılım, diğer teklif türleri ile karşılaştırıldığında sınırlıdır.

>[!IMPORTANT]
>Kişi benim liste türü öneririz yok. Yalnızca durumlarda kullanması gereken bir deneme sürümü deneyimi oluşturmak için bir yol olduğu. Müşteri katılım deneme ve işlem teklifleri ile en iyisidir. Deneme sürümü deneyimi herhangi bir türde varsa, bizim ekleme işlemi için senaryonuza bağlı olarak bu seçeneklerden birini yol gösterecektir.

Teklif kullanım öncelikle Profesyonel Hizmetler (örneğin, değerlendirmeleri, uygulamaları, Atölyeleri) oluşuyorsa danışmanlık hizmetleri türü sunar. Teklif kapsam, süre ve fiyat düzeltilmesi gereken, tek bir müşteri için olmalıdır ve sitesinde gerçekleştirilmesi gerekir.

### <a name="trial"></a>Deneme

Bir deneme sürümü deneyimi sağlayarak müşteriler ve bu nedenle daha zengin bir Etkilenme çözümünüzün sunulan katılım düzeyini artırır. Bir deneme sürümünü satın almadan önce çözümünüzü incelemek müşterilerin sağlar. Bir deneme sürümü deneyimi ile veriş yükseltme yüksek olasılığını sahip olur ve müşteri katılımlar daha zengin ve daha fazla müşteri adaylarını beklemelisiniz.
 
Tüm deneme seçenekleri deneme ortamı ve/veya Azure aboneliği arasında Müşteri'nin veya Azure aboneliğinizde dağıtılır. Denemeler müşteri neden olmadan herhangi bir ek satın alma işlemleri ve en az olmalıdır varsa, basit bir tamamlamak için ek yapılandırma kullanım örneği. Denemeler ücretsiz destek en az ve deneme süresi boyunca eklemeniz gerekir. Deneme kullanıcılar becerilerin geliştirilmesi ve en iyi sonuçlar için kasıtlı değerlendirme yol boyunca izlenen. Yayımcılar Market müşteri adayları ve yayımcının kendi uygulama Intelligence izlemek ve deneme kullanıcıları yönetmek için kullanmanız önerilir.

Üç tipik deneme senaryo vardır:


|**Deneme seçeneği**  |**Başlıca yararları**  |**Gerekirse bu seçeneği belirleyin...**  |
|---------|---------|---------|
|**Ücretsiz deneme sürümü**    |     Ücretli kullanımına dönüştürmek için bir otomatik yöntem ile satın almadan önce ürününüzü denemek bir müşteri sağlar. Ayrıca müşteri ve Microsoft satış ekipleri ile birleşik katılım için prototip sağlamaları sağlar. |     Bir sanal makine ya da çözüm şablonu çözümüdür.<br><br> Çok kiracılı bir SaaS ürün sunar ve sunan bir SaaS, çözümüdür. <br><br>Bir müşteri hale getirmek için ilk çalıştırma deneyimi ve hızlı bir şekilde çalışan var. <br><br>Tek bir kiracı sahip ancak müşteriler Konuk kullanıcı olarak eklemekte olduğunuz.|
**Test sürücü**     |     Bunlar satın almadan önce ürününüzü denemek bir müşteri sağlar. Ayrıca, önceden yapılandırılmış bir ayar yapmasına çözümünüzün Kılavuzlu bir deneyim sağlar. |   Çözümünüzü bir sanal makine, çözüm şablonu ya da tek bir kiracı SaaS uygulamayla veya sağlamak için karmaşıktır. <br><br>Ücretli bir teklife deneme sürümünüzü dönüştürmek için bir yöntem yoktur. |
|**Etkileşimli Tanıtımı**    |  Kurulum KARMAŞASIZ eylem ürününüzü görmek müşterilerin olanak tanır.       |    Çözümünüzü ve deneme süresi elde etmek için zor olabilir karmaşık kurulum gerektirir.     |


#### <a name="free-trial"></a>Ücretsiz deneme

Çözüm ya da uygulama serbest çalışırsanız, SaaS tabanlı deneme önerdiğinde ücretsiz deneme sürümü kullanın. Bu seçenek, iş flywheel başlamanıza yardımcı olması amacıyla ilgilenen müşterilerden, yüksek kaliteli müşteri adayları sürücüleri. Ücretsiz deneme sınırlı kullanım veya sınırlı süre deneme hesapları olarak sunulabilir. Bunlar yazılımınızı Ücretli kullanılacak dönüştürülmesi hızlandırmaya için eylem yapılan bir çağrı içermelidir.

#### <a name="test-drive"></a>Test sürüşü

Çözüm aracılığıyla Iaas veya SaaS uygulamaları aracılığıyla bir veya daha fazla sanal makine dağıtıldığında sınamayı kullanın. Bu yaklaşımın avantajı, bir sanal gereç ya da bir iş ortağı barındırılan "hiçbir ek ücret ödemeden müşteri değerlendirmesi için çözümün müşteriye Kılavuzlu Tur içinde" couched çözümün tamamında ortamı otomatik sağlama ' dir. Müşteri var olan bir Azure müşteri, daha yüksek kaliteli müşteri adayları oluşturmak amacıyla olması gerekmez.

Bir test sürücüye başka avantajları vardır:

- Kullanıcı aramalarını Market'te % 27, kullanıcılar tarafından yalnızca Göster teklifleri test sürücülerle için iyileştirilmiştir. 
- Test sürücülerle teklifleri %38 daha fazla müşteri adayları teklifleri daha oluşturun. 
- Yeni müşteri edinme Market'te % 36 sınamayı sürdü müşterilerden gelen. 
- Test sürücüleri ürününüz ortak satış çalışmaları için daha iyi anlamak Microsoft alan satıcılar sağlar.

#### <a name="interactive-demo"></a>Etkileşimli tanıtım

Etkileşimli bir demo kullanarak ürününüzün Kılavuzlu bir deneyim aracılığıyla müşterilerinize alın. Bu seçeneğin avantaj olmadan sağlamayı karmaşık karmaşık çözümleri için bir deneme sürümü deneyimi sağlayabilir olmalıdır. Bu seçenek müşteriler geçici çözüm bir görünüm sağlar. Ve iş flywheel başlatmak için temel anlaşmalar becerilerin geliştirilmesi müşteri adayları almaya başlamak yayımcılar sağlar. 

### <a name="transaction"></a>İşlem

Azure Marketi'nde kullanan bir *sanal makine* müşterinin aboneliğine sanal gereç olarak çözüm dağıtıldığında. Sanal makinelerin tam olarak ticaret Kullandıkça Öde veya KLG etkin lisans modelleri ile etkin olduğunu. Microsoft commerce işlem barındırır ve yayımcı adına müşteri bills. Yayımcı ile Kurumsal Anlaşma dahil olmak üzere Microsoft, Müşteri'nin tercih edilen ödeme ilişkisi yararlanarak yararı alır. 

>[!NOTE]
>Şu anda bir kurumsal anlaşması parasal taahhüt sanal gereç ait Azure kullanım karşı ancak publisher'ın yazılım lisans ücretleri karşı değil kullanılabilir.

Kullanım bir *Azure çözüm şablonu* ne zaman bir çözüm sanal gereç ötesinde ek dağıtım ve yapılandırma Otomasyon gerektirir. Çözüm şablonları bir veya daha fazla sanal makine kaynakları sağlama otomatik hale getirebilir ve ağ ve depolama kaynaklarını sağlayabilirsiniz. Çözüm şablonları tek sanal makineleri ve tüm Iaas tabanlı çözümü ortamları Otomasyon fayda sağlayabilir. Çözüm şablonları oluşturma hakkında daha fazla bilgi [GitHub](https://github.com/MicrosoftDocs/azure-docs).

Kullanım bir *Azure yönetilen uygulama* ne zaman bir müşterinin aboneliğine--bir sanal makine veya Iaas tabanlı çözümün tamamında dağıtıyorsanız ve yayımcı veya müşteri üçüncü taraf (örneğin, bir SI tarafından yönetilecek çözüm ister ya da MSP). Yönetilen uygulamalar oluşturma hakkında daha fazla bilgi [Azure yönetilen uygulamaları genel bakış](https://docs.microsoft.com/azure/managed-applications/overview). Sık sorulan soruların bir listesi için bkz: [Market SSS](https://azure.microsoft.com/marketplace/faq/).

>[!NOTE]
> Yönetilen uygulamalar Market üzerinden dağıtılabilir olmalıdır. Müşteri iletişimi önemliyse sağlama paylaşımı varsa, ilgilenen müşteriler için ulaşmak olduğunu unutmayın.

### <a name="azure-certified-program"></a>Azure Onaylandı programı

Azure Marketi'nde yayımlanan tüm sanal makineler için Azure Certified program sınanır. Program:

- Müşteriler sanal makineniz Azure platformu ve model satış Market ile uyumlu olmasını sağlar.
- Virüs ve kötü amaçlı yazılım dahil olmak üzere çevrimiçi görüntü güvenliği uyumluluk testleri.
- Doğrulanmış bir çözüm olarak Microsoft Kurumsal müşteriler için yükseltme geliştirmek için teklif düzeyinde badging etkinleştirir.

#### <a name="marketplace-commercial-considerations"></a>Market ticari konuları

Market katılan için hiçbir ücretleri vardır. Liste, deneme ve KLG hareket seçeneklerini kullanarak yayımlama sırasında markette katılan için geliri paylaşımı yok. Daha fazla bilgi için bkz: [Market katılım ilkeleri](https://azure.microsoft.com/support/legal/marketplace/participation-policies/).

#### <a name="pay-as-you-go-and-bring-your-own-license-billing-options"></a>Kullandıkça Öde ve kendi faturalama seçeneklerini lisansını Getir

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

### <a name="single-billing-and-payment-methods"></a>Tek faturalandırma ve ödeme yöntemleri

Seçenek yayımlama işlem kullanmanın önemli bir avantajı, Microsoft erişebileceği tek fatura lisans maliyetleriniz temel Azure kullanım doğrudan müşteriye aynı zamanda ' dir. Bu senaryo, Microsoft Ürün ve sizin adınıza toplar, müşteriyle kendi satın alma ilişkisi oluşturmak için gereksinimini ortadan kaldırır. Bu, zaman ve fatura toplamadığı satış giriş odaklanmak için kaynak tasarrufu yapabilirsiniz.

### <a name="enterprise-agreement"></a>Kurumsal Anlaşma

Microsoft müşterileri, bir Kurumsal Anlaşma bazen dahil olmak üzere Azure kullanım Microsoft ürünleri için ödeme yapmak için kullanın. Bu ödeme seçeneği yazılım lisans ve bulut Hizmetleri en az üç yıllık bir dönem için istediğiniz kuruluşlar için tasarlanmıştır. Müşterilerin bir eylemli ödeme yapmak yerine ödemeler yayılan seçeneğiniz vardır. EA müşteri Kullandıkça Öde işlem listeleme kullandığında, publisher'ın yazılım lisans maliyetleri için fatura döngüsü faturalama üç aylık EA fazlalığı izler.

### <a name="monetary-commitment"></a>Parasal taahhüt 

Tüm Kurumsal Anlaşma müşterileri, Azure'a önceden parasal taahhütte bulunarak Azure'ı anlaşmalarına ekleyebilir. Bu bağlılık çeşitli genel merkezlerinden Azure'un sunduğu bulut hizmetlerini herhangi bir bileşimini kullanarak yıl kullanılır.

## <a name="prerequisites-for-marketplace-publishing"></a>Market yayımlama için Önkoşullar

### <a name="prerequisites-for-all-marketplace-publishing-options"></a>Tüm Market yayımlama seçeneklerini için Önkoşullar


|**Gereksinim**  |**Ayrıntılar**  |**Yayımlama seçeneği**  |
|---------|---------|---------|
|**Katılım ilkeleri**    | Azure Market gözden [katılım ilkeleri](https://azure.microsoft.com/support/legal/marketplace/participation-policies/).       | Liste, deneme, işlem        |
|**Microsoft ile tümleştirme**    | Azure Market sunar kullanın veya Microsoft Azure hizmet türleri işlem, ağ veya depolama gibi genişletir. Bunlar, var olan bir Azure Marketi kategori veritabanları, güvenlik ve ağ gibi uyumlu olmalıdır. Bkz: [tam listesi](https://azuremarketplace.microsoft.com/marketplace/apps).        | Liste, deneme, işlem        |
|**Hedef kitle**    | Azure Market sunar, BT uzmanları, bulut geliştiriciler veya diğer teknik müşteri rolleri olması gerekir.       |  Liste, deneme, işlem 
|**Sağlama Yönetimi**    | Marketten müşteri adayları almak için CRM (Marketo, Microsoft Dynamics veya Salesforce) sağlama verileri kabul etmek için etkinleştirmeniz gerekir.        |   Liste, deneme, işlem      |
|**Gizlilik ilkesini ve kullanım koşulları**     |   Gizlilik İlkesi genel bir URL ile kullanılabilir olması gerekir. Kullanım koşullarını metin olarak yayımlama sırasında girilmesi gerekir.      |   Liste, deneme, işlem      |
|**Destek**     |  Teklifiniz, müşterilerin Yardım bulabileceğiniz bir genel kullanıma açık destek URL'si eklemeniz gerekir. Denemeler için destek ve deneme süresi için hiçbir ek ücret sağlanmalıdır.       |  Deneme, işlem       |

### <a name="prerequisites-specific-to-trial-publishing"></a>Deneme yayımlamaya belirli önkoşulları

|**Gereksinim**  | **Ayrıntılar**  |**Yayımlama seçeneği**  |
|---------|---------|---------|
|**Ücretsiz deneme süresi ve deneme sürümü deneyimi**     |  Bir müşteri uygulamanızı sınırlı bir süre için ücretsiz kullanmanız mümkün olması gerekir.<br><br>Bu, müşteri lisans veya Abonelik ücretlerinin ürününüzü ya da temel alınan Microsoft birinci taraf ürün veya hizmetle maliyetini tabi olmaz anlamına gelir. Tüm deneme seçenekleri publisher'ın Microsoft Ürün aboneliğine dağıtılan olduğundan yayımcı yalnızca deneme maliyet iyileştirmesi ve yönetim denetler.<br><br>Ücretsiz deneme sürümü, etkileşimli demo seçin veya sürücü testi. Seçtiğiniz öğeye olsun, ücretsiz deneme müşteri uygulaması için ek ücret ödemeden denemek için en az bir süre sağlaması gerekir.<br><br>Sınamayı oluşturma işlemine başlamak için ulaşmak cloudmarketplace@microsoft.com. <br><br>Azure Market SaaS deneme karşılaştığında Not kendi Active Directory iş kimlik bilgileriyle oturum açmasını izin vermeniz gerekir. [Daha fazla bilgi edinin](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-devhowto-appsource-certified#appsource-trial-experiences). |   Deneme      | 
| **Kolayca yapılandırılabilir, anahtar teslimi çözümü**    |  Uygulamanızı yapılandırmak ve ayarlamak hızlı ve kolay olmalıdır.       |  Deneme       |
|**Kullanılabilirlik/açık kalma süresi**    |    SaaS uygulaması veya platform en az % 99,9 çalışma süresi olması gerekir.     |    Deneme     |
|**Azure Active Directory**    |    Teklifiniz etkin izin ile çoklu oturum açma (SSO) Azure Active Directory (Azure AD) federe izin vermeniz gerekir.      |  Deneme|

### <a name="prerequisites-specific-to-transaction-publishing"></a>İşlem yayımlamaya belirli önkoşulları


|**Gereksinim**  |**Ayrıntılar** |**Yayımlama seçeneği**  |
|---------|---------|---------|
|**Faturalama ve ölçümü**    |  Sanal makineniz kendi lisansını Getir veya kullanım tabanlı, aylık faturalama desteklemesi gerekir.       |    İşlem    |
|**Azure ile uyumlu sanal sabit disk (VHD)**     |   Sanal makineler yerleşik, [Windows](https://docs.microsoft.com/en-us/azure/marketplace-publishing/marketplace-publishing-vm-image-creation) veya [Linux](https://docs.microsoft.com/en-us/azure/marketplace-publishing/marketplace-publishing-vm-image-creation).    |   İşlem      |

### <a name="prerequisites-specific-to-consulting-services-publishing"></a>Önkoşullar belirli danışmanlık hizmetleri yayımlama


|**Gereksinimleri** |**Ayrıntılar**  |**Yayımlama seçeneği**  |
|---------|---------|---------|
|**Hizmet teklif özelliklerini**     | Danışmanlık hizmetinizi olması gerekir: <br>-Sabit kapsam, sabit süre, sabit fiyat (veya boş) katılım teslim. <br>-Yönelik öncelikle satış öncesi. <br>-Tek bir müşteri sınırlıdır. <br>-Sitesinde yürütülür.        |    Liste     |
|**Danışmanlık Hizmetleri için iş ortağı gereksinimleri**    |   *Yalnızca AppSource*:  <br>- **Dynamics 365 müşteri katılım için**: sahip Gümüş veya altın [bulut Müşteri İlişkileri Yönetimi](https://partner.microsoft.com/en-us/membership/cloud-customer-relationship-management-competency) uzmanlığı. <br>- **Dynamics 365 Finans ve işlemleri Enterprise edition için**: sahip Gümüş veya altın [kurumsal kaynak planlaması](https://partner.microsoft.com/en-us/membership/enterprise-resource-planning-competency) uzmanlığı ve 25.000 bulut işlemleri sonunda 12 ay içinde en az bir gelir. <br>- **Dynamics 365 Finans ve işlemleri, Business edition**: olarak hizmet [bulut Hizmetler Sağlayıcısı (CSP)](https://partner.microsoft.com/en-us/cloud-solution-provider) veya [dijital iş ortağı, kaydı (DPOR)](https://partner.microsoft.com/en-us/membership/digital-partner-of-record) en az bir müşteri için. <br>- **Power BI**: [çözüm ortağı] (file:///C:/Users/ellacroi/Downloads/BI%20Partner%20Program%20Overview%20 & % 20Incentives.pdf) ölçütlere uyan. <br>- **PowerApps**: sahip bir [iş ortağı gösterimi](https://powerapps.microsoft.com/en-us/partner-showcase/) çözümü. |    Liste     |

## <a name="using-azure-active-directory-to-enable-trials"></a>Denemeler etkinleştirmek için Azure Active Directory kullanma
Azure Active Directory olduğu kimlik doğrulamasına olanak tanıyan bir bulut kimliği hizmeti ile bir Microsoft iş veya Okul hesabı endüstri standardı protokolleri kullanılarak: OAuth ve Openıd Connect. Azure AD hakkında daha fazla bilgi almak [ürün Web sayfası](https://www.microsoft.com/en-us/cloud-platform/azure-active-directory-features). 

Microsoft Azure AD ile tüm Market kullanıcıların kimliğini doğrular. Kimliği doğrulanmış bir kullanıcı markette listeleme denemenizi aracılığıyla tıklar ve deneme ortamınıza yönlendirilir, ek bir oturum açma adımı gerek kalmadan doğrudan bir deneme sürümü'nü kullanıcı sağlayabilirsiniz. [Uygulamanızı Azure AD kimlik doğrulama sırasında alan belirteci](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-token-and-claims#sample-tokens) uygulamanızda bir kullanıcı hesabı oluşturmak için kullanabileceğiniz değerli kullanıcı bilgilerini içerir. Ardından, sağlama deneyimi otomatikleştirmek ve dönüştürme olasılığını artırmak. 

Azure AD kullanarak uygulamanızı veya deneme için tek tıklamayla kimlik doğrulamasını etkinleştirmek için:

- Market müşteri deneyimine deneme kolaylaştırır. 
- Kullanıcının etki alanı ya da deneme ortamınıza marketten bile yönlendirildiği zaman bir ürün deneyimi yapısını korur.
- Hiçbir ek oturum açma adımı olduğundan yeniden yönlendirme üzerinde abandonment olasılığını azaltır.
- Azure AD kullanıcıları büyük popülasyonunu dağıtım engelleri azaltır.

### <a name="certify-your-azure-ad-integration-for-the-marketplace-multitenant-applications"></a>Market, Azure AD tümleştirme Onayla: çok müşterili uygulamalar

Azure AD bugün destekliyorsa:

- Uygulamanızı Azure portalında kaydedin.
- Çoklu müşteri mimarisi desteği özelliği, bir tek tıklamayla deneme sürümü deneyimi almak için Azure AD'de etkinleştirin.
- [Daha fazla bilgi edinin](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-integrating-applications).

Azure AD Federasyon SSO için yeni varsa:

- Uygulamanızı Azure portalında kaydedin.
- Azure AD ile SSO kullanarak geliştirmek [Openıd Connect](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-protocols-openid-connect-code) veya [OAuth 2.0](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-protocols-oauth-code).
- Çoklu müşteri mimarisi desteği özelliği, bir tek tıklamayla deneme sürümü deneyimi almak için Azure AD'de etkinleştirin.
- [Daha fazla bilgi edinin](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-devhowto-appsource-certified).

### <a name="certify-your-azure-ad-integration-for-the-marketplace-single-tenant-applications"></a>Market, Azure AD tümleştirme Onayla: tek Kiracı uygulamaları

Tek Kiracı uygulamalar için birden çok seçenek vardır:

- Kullanıcılar, kullanarak Konuk kullanıcılar olarak dizininize eklemek [Azure B2B](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b).
- El ile sağlama denemeler kişi Me aracılığıyla müşteriler için
- Müşteri başına sınamayı geliştirin.
- SSO çok müşterili örnek bir tanıtım uygulaması oluşturun.

## <a name="publishing-processes-by-product-for-office-dynamics-and-power-bi"></a>Office, Dynamics ve Power BI için ürün yayımlama işlemler
Office, Dynamics ve Power BI genişletmek AppSource uygulamalar için bu bölümdeki ürüne özgü belgelere belirli gereksinimleri hakkında daha fazla bilgi edinebilirsiniz. 


|Ürün |Yayımlama bilgileri  |
|---------|---------|
|Office 365     |    Gözden geçirme [işlemi ve yönergeleri yayımlama]( https://docs.microsoft.com/en-us/office/dev/store/submit-to-the-office-store).     |
|Dynamics 365 Finans ve işlemleri  |   İçin Enterprise Edition oluştururken gözden [işlemi ve yönergeleri yayımlama](https://docs.microsoft.com/en-us/dynamics365/unified-operations/dev-itpro/lcs-solutions/lcs-solutions-app-source).      |
|Dynamics 365 müşteri katılım için |Gözden geçirme [işlemi ve yönergeleri yayımlama](https://docs.microsoft.com/en-us/dynamics365/customer-engagement/developer/publish-app-appsource). |
|Power BI   |     Gözden geçirme [işlemi ve yönergeleri yayımlama]( https://docs.microsoft.com/en-us/power-bi/developer/office-store).    |
|Cortana Intelligence     |    Hakkında bilgi edinin [AppSource Cortana](https://docs.microsoft.com/en-us/azure/machine-learning/team-data-science-process/cortana-intelligence-appsource-publishing-guide).     |
|AppSource danışmanlık teklifleri     |  Gözden geçirme [yönergeleri ve teklifiniz gönderme öğrenin](https://smp-cdn-prod.azureedge.net/documents/Microsoft%20AppSource%20Partner%20Listing%20Guidelines.pdf).    |



## <a name="cloud-partner-portal-pre-publishing-checklist-for-the-azure-marketplace"></a>Bulut Azure Marketi için iş ortağı portalını önceden yayımlama denetim listesi

Yayımlama işlemi başlamadan önce bir teklifi oluşturmak için gerekli bileşenleri anlamak faydalıdır. Aşağıdaki yapılar oluşturma teklif yayımlama akışında bulut iş ortağı Portalı'nı tamamlamak için gereklidir. 

### <a name="storefront-details"></a>StoreFront ayrıntıları


|Bu yayımlama yapı gerekir  |Bu teklif türü için  |
|---------|---------|
|**(200 karakterden) ad ve Açıklama (2.000 karakter) sunma**    |  Tümü        |
|**Microsoft iş ortağı ağı (MPN) kimliği**   |  Tümü       |
|**Ülke/bölge kullanılabilirliği**   | Tümü        |
|**Katılım süresi**     |   Danışmanlık Hizmetleri      |
|**Geçerli sektörü, kategoriler ve arama anahtar sözcükleri**     |  Tümü       |
|**Şirket logoları (48 x 48, 216 x 216)**     |  Danışmanlık Hizmetleri       |
|**Ürün genel bakış videosu (isteğe bağlı)**  |  Tümü       |
|**Screenshots (maximum 5, 1280x720)**   |    Tümü     |
|**Pazarlama belgeleri (en fazla 3)**    |  Tümü       |
|**Hedef yol**    |   Tümü      |

### <a name="contacts"></a>Kişiler


|Bu yayımlama yapı gerekir  |Bu teklif türü için  |
|---------|---------|
|**Bilgi (desteği, mühendislik, ticari) başvurun**    |    Tümü     |

### <a name="technical-info"></a>Teknik bilgi


|Bu yayımlama yapı gerekir  |Bu teklif türü için |
|---------|---------|
|**Deneme URL'si**     |  Tüm deneme teklifi türleri       |
|**Desteklenen diller**    |   Tüm deneme teklifi türleri      |
|**Uygulama sürüm numarası ve yayın tarihi**    |   Tüm deneme teklifi türleri      |
|**Destek URL'si**    |   Tüm deneme teklifi türleri, sanal makineler      |
|**Kullanım ve gizlilik ilkesi URL'si koşulları**     |    Tümü     |

### <a name="test-drive"></a>Test sürüşü


|Bu yayımlama yapı gerekir  |Bu teklif türü için  |
|---------|---------|
|**Açıklama ve süresi**     |  Yalnızca test sürücü       |
|**Kullanıcı el ile**     |   Yalnızca test sürücü      |
|**Test drive video (maximum 1)**     |  Yalnızca test sürücü       |
|**Sınama sürücü ülke/bölge kullanılabilirliği**    |   Yalnızca test sürücü      |
|**Azure kaynak grubu adı**   |         |
|**Azure abonelik kimliği**     |  Yalnızca test sürücü       |
|**Azure AD Kiracı kimliği**   |    Yalnızca test sürücü     |
|**Azure AD uygulama kimliği**  |  Yalnızca test sürücü       |
|**Azure AD uygulama anahtarı**     |   Yalnızca test sürücü      |

### <a name="storefrontmarketplace"></a>Mağaza/Market


|Bu yayımlama yapı gerekir  |Bu teklif türü için  |
|---------|---------|
|**Title (maximum 50 characters)**    |  İşlem: sanal makineler, Azure uygulamaları (Çözüm şablonları ve yönetilen uygulamalar)       |
|**Summary (maximum 200 characters)**    |  İşlem: sanal makineler, Azure uygulamaları (Çözüm şablonları ve yönetilen uygulamalar)       |
|**Uzun özeti (en fazla 256 karakter)**     |   İşlem: sanal makineler, Azure uygulamaları (Çözüm şablonları ve yönetilen uygulamalar)      |
|**HTML tabanlı açıklaması (en fazla 3000 karakter)**    |  İşlem: sanal makineler, Azure uygulamaları (Çözüm şablonları ve yönetilen uygulamalar)       |
|**Şirket logoları (40 x 40, 90 x 90, 115 x 115, 255 x 115, 815 x 290)**    |  İşlem: sanal makineler, Azure uygulamaları (Çözüm şablonları ve yönetilen uygulamalar)       |

### <a name="sku"></a>SKU


|Bu yayımlama yapı gerekir  |Bu teklif türü için  |
|---------|---------|
|**Sürüm numarası**     |    İşlem: Azure uygulamaları (Çözüm şablonları ve yönetilen uygulamalar)     |
|**Tüm şablon dosyalarını ve createUIDefinitionFile içeren paket dosyası**   |İşlem: Azure uygulamaları (Çözüm şablonları ve yönetilen uygulamalar)         |
|**İşletim sistemi ayrıntıları**    |   İşlem: sanal makineler      |
|**Bağlantı noktalarını ve protokolleri kullanımda**    |  İşlem: sanal makineler       |
|**Disk sürümü ve Kullanımdaki her VHD için SAS URL'si**   |  İşlem: sanal makineler       |

## <a name="becoming-a-publisher"></a>Bir yayımcı olma

Bu bölümde, adımları açıklanır:

- Azure Market ve AppSource Publisher'da haline gelir.
- Bulut iş ortağı portalına erişin. Yapı, yayımlama ve teklifiniz korumak için bu portalı kullanacaksınız. 

### <a name="process-overview"></a>İşlemine genel bakış


|Market kayıt adımı  |Zaman  |Açıklama  |
|---------|---------|---------|
| Microoft iş ortağı ağı kaydet | 15 dakika | Yayımcı, Microsoft Partner Network (Hesap doğrulama ilk düzeyine sahip ve ek avantajların yanı sıra ve bir Azure Market yayımcı olabilme desteği için MPN) kayıtlı olması gerekir |
|Microsoft kimlik oluşturun     |   15 dakika      |   İş ortaklarının Microsoft ID. olması gerekir Bu Microsoft ID bulut iş ortağı portalına erişmek için kullanılır.       |
|Market Adaylığı form gönderme     |  1-3 gün       |  Market onay işlemini başlatmak üzere Adaylığı form gönderilemedi ortakları gerekir. Form gönderildikten sonra Market hazırlanma ekibi uygulama gözden geçirin ve istek doğrulayın.       |
|Geliştirici Merkezi'nde kaydetme     |    5-10 gün     | Microsoft iş ortağı kayıtlı olduğu ülkede için geçerli vergi numarası geçerli yasal bir varlıkla olduğunu doğrulamak için Microsoft Developer Center'da kayıt gereklidir. Geliştirici Merkezi kayıtlı Microsoft geliştirici olmanız ve bunları Azure Geliştirici programı erişim sağlamak için ortağı olanak sağlar. <br><br>Market Adaylığı form tamamladıysanız henüz 99 kayıt ücret ödemeniz istenir, olduğunu unutmayın. Kullanıma başlama öncesi bu ücret sağlamak için Market Adaylığı formu doldurun ve e-posta yoluyla bir promosyon kodu alırsınız.  |
|Bulut iş ortağı portalında oturum açın     |  15 dakika       |   İş ortağı onayı kendi Adaylığı onaylandıktan Market ekibinden aldıktan sonra erişim için iş ortağı [bulut iş ortağı portalını](https://cloudpartner.azure.com/) etkinleştirilir. İş ortağı Adaylığı formun Microsoft ID yayımcı profillerini bulut iş ortağı portalında oturum açmak için kullanmanız gerekir. Geliştirici Merkezi ile kayıttan sonra iş ortağı Geliştirici Merkezi hesabına yayımlamak için kendi Azure Marketi yayımcı profiliyle ilişkilendirmek gerekir.      |

#### <a name="create-a-microsoft-id"></a>Microsoft kimlik oluşturun

Tüm Market yayımlama işlemi yoluyla, Market hesabını tanımlayan bir e-posta adresi kullanın. Bu e-posta adresi a Microsoft ID kayıtlı olması gerekir ve her ikisi için kullanılan [Microsoft Developer Center'da](https://developer.microsoft.com/) ve [bulut iş ortağı portalını](https://cloudpartner.azure.com/). 

Azure Market ve AppSource Teklifleriniz için yalnızca bir Microsoft ID hesabı olması gerekir. Diğer hizmetlerin veya teklifleri paylaşmadan mu olduğunu öneririz.

Seçilen e-posta adresi şirket etki alanınızda tercihen olmalıdır ve BT ekibi tarafından denetlenir. Bir kimliği oluşturmadan önce yönergeleri gözden geçirmek için aşağıdaki bölümlerde [kılavuzları ve nasıl yapılır](#guidelines-and-how-tos). 

#### <a name="register-in-microsoft-partner-network"></a>Microsoft iş ortağı ağı kaydet 
Bir Azure Marketi veya AppSource yayımcı şirketiniz olma Microsoft ile ortaklık. Microsoft Partner Network (MPN) katılarak, şirketinizin teknik çözümleri geliştirmek ve işinizi genişletmenize yardımcı olmak için çekirdek yararları ayarlamak için erişim sağlama (örn: teknik destek içerir). Bir yayımcı olarak Market katılarak Avantajlarınızı Microsoft iş ortağı ağı içinde tahakkuk. MPN kaydetmek için lütfen ziyaret [Microsoft iş ortağı ağı](https://partner.microsoft.com/en-us/membership/). Şirket içinde MPN zaten kayıtlı değilse doğrulamalıdır. Kaydedildikten sonra MPN kimliğinizi hesabınızı doğrulamak yayımcı profilinizde doğrulamak için isteyeceğiz [bulut iş ortağı portalını](https://cloudpartner.azure.com/). 

#### <a name="submit-the-marketplace-nomination-form"></a>Market Adaylığı form gönderme
Market ekleme işleminin bir parçası olarak Adaylığı form gönderme gerekir. Form uygulamanızı veya hizmet teklifi, şirketinizin bilgilerini ve düzeyde sağlayacağınız desteği hakkında bilgi içerir. 

- [Azure Market Adaylığı formu](http://aka.ms/listonazuremarketplace)   
- [AppSource Adaylığı formu](http://aka.ms/listonappsource)

Formu gönderdikten sonra Market Ekibi uygulama gözden geçirin ve istek doğrulayın. İstek inceledikten sonra bulut iş ortağı portalında onaylanmış bir iş ortağı olmadan için sonraki adımda e-postayla bildirilecek.

#### <a name="register-in-the-developer-center"></a>Geliştirici Merkezi'nde kaydetme

[Microsoft Developer Center'da](https://developer.microsoft.com/) sahip olan uygulamalar yayımlamayı sanal makineler, çözüm şablonları ve Azure ile yönetilen uygulamalar gibi özellikler transact için gereklidir. Bu gereksinim, şirket bilgileri şirketinizin yasal, vergi ve varlıkları bankacılık doğrulamak Microsoft izin verir. Registrant şirketin geçerli temsilcisi olması ve kullanıcıların kimliğini doğrulamak için kendi kişisel bilgilerini sağlamanız gerekir. Kaydetme kişinin şirket için paylaşılan a Microsoft ID kullanmanız gerekir ve aynı hesabı kullanılmalıdır [bulut iş ortağı portalını](https://cloudpartner.azure.com/). 

>[!IMPORTANT]
>Microsoft Developer Center'da hesabı oluşturmak denemeden önce şirketinizin zaten olmayan emin olun.

İşlemi sırasında biz şirket adresi bilgileri, banka hesabı bilgileri toplamak ve bilgi vergi. Bu bilgiler genellikle finans bölümüyle veya işletmeyle ilgili kişilerden alınabilir. Ayrıca, çeşitli aşamaları teklif oluşturulmasını ve dağıtımını tamamlamak için aşağıdaki yayımcı profili bileşenleri tamamlamanız gerekir:


|**Yayımcı profili**  |**Profili başlatmak için**  |**Hazırlama**  |**Liste ve deneme**  |**Transact**
|---------|---------|---------|---------|---------|
|**Şirket kayıt**     | Olması gerekir        |  Olması gerekir       | Olması gerekir        |  Olması gerekir       |
|**Vergi profili kimliği**   |    İsteğe bağlı     |    İsteğe bağlı     |  İsteğe bağlı       | Olması gerekir      |
|**Banka hesabı**     |   İsteğe bağlı      |    İsteğe bağlı     |  İsteğe bağlı       |  Olması gerekir      |

Bu işlem hakkında adım adım açıklama için bkz: [Geliştirici Merkezi'nde kaydetmek yönergeler](#instructions-on-how-to-register-in-the-developer-center). 

#### <a name="sign-in-to-the-cloud-partner-portal"></a>Bulut iş ortağı portalında oturum açın

Adaylığı onaylanmış ve içinde kaydettiğiniz Market Ekibi'nden gelen onay aldıktan sonra [Microsoft iş ortağı ağı](https://partner.microsoft.com/en-us/membership/) ve [Microsoft Developer Center'da](https://dev.windows.com) (IF gerekli), bir hesap erişmek için oluşturulacak [bulut iş ortağı portalını](https://cloudpartner.azure.com). İlk kez oturum açma kimlik bilgileri Adaylığı onay e-postayla dahil edilir. 

Yayımcı profilinizi erişmek için Market hesabınızı (Microsoft ID) kullanın. Bir kez bulut iş ortağı Portalı'nda Microsoft iş ortağı ağı ve Geliştirme Merkezi hesabı (gerekliyse) ilgili Market yayımcı yayımlama profiliyle ilişkilendirmek için son adım olacaktır. Bu ekranın altındaki düğmesiyle yayımcı profilinizde bulut iş ortağı portalında yapılabilir.

Bulut iş ortağı portalının nasıl kullanılacağı hakkında ayrıntılı bilgi için Git [öğrenin](https://cloudpartner.azure.com/#Learn) gözden geçirme ve portal menüsünde **belgelerine** bölümü. 


## <a name="support"></a>Destek

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
|Kayma: [Market kayma katılma](https://join.marketplace.azure.com)    |   Teknik sorunlar iş ortaklarıyla desteklemek için slack ortamı. Şu anda bu ortamda çalışan birden fazla 350 ortakları vardır.        |
|MSDN Forumlarında: [Market](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=DataMarket)     | Microsoft Developer Network forum.         |
|Yığın taşması: [Azure](https://stackoverflow.com/questions/tagged/azure)     |    Yığın taşması ortam çözümleri almak ve her şeyi Azure ve Market ilgili hakkında sorular sormak için:<ul><li>[Azure Market](https://stackoverflow.com/questions/tagged/azure-marketplace)</li><li>[Azure Resource Manager](https://stackoverflow.com/questions/tagged/azure-resource-manager)</li><li>[Azure Sanal Makineler](https://stackoverflow.com/questions/tagged/azure-virtual-machine)</li></ul> |


**Pazarlama kaynakları**

|Destek kanal  |Açıklama  |
|---------|---------|
|E-posta: cosell@microsoft.com    |  Onboarding işlemleri ve ortak satış programı ile ilgili sorular için destek. Pasifik saat diliminde temel.        |
|E-posta: gtm@microsoft.com    |  Git Pazar avantajları ve program sorular için destek. İş saatleri Pasifik saat diliminde ' dir.        |
|E-posta: CEBrand@Microsoft.com     |  Azure logolar ve markalama kullanımı hakkındaki soruların yanıtları.       |

## <a name="guidelines-and-how-tos"></a>Kılavuzları ve nasıl yapılır?

### <a name="guidelines-for-creating-a-microsoft-id-to-manage-an-azure-marketplace-account"></a>Bir Azure Marketi hesabını yönetmek için a Microsoft ID oluşturma yönergeleri

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

Ek Geliştirici hesabı yönergeleri ve güvenlik hakkında daha fazla bilgi için bkz: [bir geliştirici hesabını açma](https://docs.microsoft.com/en-us/windows/uwp/publish/opening-a-developer-account).

### <a name="guidance-for-microsoft-ids-in-an-azure-ad-federated-domain"></a>Bir Azure AD Federasyon etki alanındaki Microsoft IDs Kılavuzu

Şirket hesabınızı üzerinden federe [Azure Active Directory](https://docs.microsoft.com/en-us/azure/active-directory/). Şirket e-posta adresiyle a Microsoft ID oluşturmayı denerseniz hata döndürür. İlk olarak bir hata alırsanız, bu durumda olduğundan emin olmak için BT ekibiniz denetleyin. Bu bilinen bir sorundur ve çözümlemek için çalışıyoruz. 

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
   >Hesap türleri ve seçmek için en iyi olduğu daha iyi anlamak için bkz: [hesap türlerini, konumlarını ve ücretleri](https://docs.microsoft.com/en-us/windows/uwp/publish/account-types-locations-and-fees).

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
