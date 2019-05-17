---
title: Market ticari işlem özellikleri ve önemli noktalar | Azure
description: Bu makalede, bir teklif türü için Transact fiyatlandırma, faturalandırma, faturalama ve ödeme konuları açıklanır.
services: Azure, Marketplace, Compute, Storage, Networking, Transact Offer Type
author: yijenj
manager: nuno costa
ms.service: marketplace
ms.topic: article
ms.date: 10/29/2018
ms.author: yijenj
ms.openlocfilehash: 5dc311538055d9ecb7aba21f95d819a914cd1db7
ms.sourcegitcommit: be9fcaace62709cea55beb49a5bebf4f9701f7c6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2019
ms.locfileid: "65826561"
---
# <a name="azure-marketplace-commercial-transaction-capabilities-and-considerations"></a>Azure Market ticari işlem özelliklerini ve dikkat edilmesi gerekenler

Azure Marketi yayımlama seçeneklerini bulut yazılım ve hizmet sağlayıcıları müşteriler ile bağlantı için benzersiz yollar sunar. Bu makalede, Azure Marketi şu ticaret ile ilgili konularda ele alınmaktadır:

* Market Yayımlama seçenekleri
* Genel bir bakış transact
* Faturalandırma modelleri transact
* Gereksinimleri transact

## <a name="marketplace-publishing-options"></a>Market Yayımlama seçenekleri

Aşağıdaki Yayımlama seçenekleri, Azure Market yayımcıları için kullanılabilir.

### <a name="list--trial-publishing-options"></a>& Liste deneme Yayımlama seçenekleri

Azure Market'teki deneme yayımlama seçeneklerini ve listesindeki yayımcılar yararlanabilir tanıtım ve kullanıcı edinme amaçlar. Liste veya deneme yayımlama seçeneklerini Microsoft doğrudan yayımcının yazılım lisans işlemlere katılmasına değil ve ilişkili işlem ücret yoktur. Yayımcılar dahil ancak bunlarla sınırlı olmamak üzere yazılım lisans işlemin tüm yönlerini desteklemek için sorumlu: siparişi yerine getirme, kullanım ölçümü, faturalandırma, faturalama, ödeme ve koleksiyonu. Liste ve deneme yayımlama seçeneklerini yayımcılar yayımcı yazılım lisans ücretleri müşteriden toplanan %100 tutun. 

### <a name="transact-publishing-option"></a>Yayımlama seçeneği transact

Liste ve deneme yayımlama seçeneklerini ek olarak, Azure Marketi yayımcılarının transact yayımlama seçeneği kullanılabilir.   Microsoft'un küresel olarak kullanılabilir ticaret özellikleri avantajlarından yararlanır. Bu seçenek, Microsoft yayımcı adına konak bulut Market işlemleri sağlar.

## <a name="transact-general-overview"></a>Genel bir bakış transact

Yayımlama seçeneği transact kullanırken, Microsoft üçüncü taraf yazılım satışı ve bazı teklif türleri müşterinin Azure aboneliğine dağıtımını sağlar. Yayımcı, Azure altyapı ücretleri Faturalaması dikkate almanız gerekir ve faturalama seçerken yayımcının kendi yazılım lisans ücretleri, modeli ve Azure Marketi'nde türü sunar.

Azure Marketi'nde Transact yayımlama seçeneği şu anda aşağıdaki teklif türleri için desteklenir: Sanal makineler, Azure uygulamaları veya SaaS uygulamaları.

![[Azure Market'te ilgilenen kurumsal deneyimidir]](./media/marketplace-publishers-guide/Transact-enterprise-deals.png)

### <a name="billing-infrastructure-costs"></a>Faturalandırma altyapı maliyetleri

**Sanal makineler ve Azure uygulamaları için**

Sanal makineleri ve Azure uygulamaları için Azure altyapı kullanım ücretleri müşterinin Azure aboneliğinize faturalandırılır.  Altyapı kullanım ücretleri ücretlendirilir ve gelen yazılım sağlayıcısının lisans ücretleri müşterinin faturasında ayrı olarak sunulur.

**SaaS uygulamaları için**

SaaS uygulamaları için yayımcı kullanım ücretleri Azure altyapı ve yazılım lisans ücretleri için tek maliyeti öğesi olarak dikkate alması gerekir.  Müşteri için sabit bir ücret gösterilir. Azure altyapı kullanım yönetilir ve iş ortağı doğrudan faturalandırılır.  Gerçek altyapı kullanım ücretleri müşteri tarafından görülmez.  Yayımcılar Azure altyapı kullanım ücretleri, yazılım lisans fiyatına paket genellikle tercih etme.  Yazılım Lisans ücretleri ölçülen olmayan veya tüketim temelli.

## <a name="transact-billing-models"></a>Faturalandırma modelleri transact

Kullanılan işlem seçeneğine bağlı olarak, yayımcının lisans ücretini şu şekilde görüntülenebilir:  

* Ücretsiz: Yazılım lisansları için ücret alınmaz. 

* Kendi lisansınızı getirin (BYOL): Yazılım lisansları için herhangi bir geçerli ücreti yayımcı ve müşteri arasında doğrudan yönetilir. Microsoft, yalnızca Azure altyapı kullanım ücretleri geçirir. (Sanal makineler ve Azure uygulamaları yalnızca.)

* Kullandıkça Öde: Yazılım Lisans ücretleri, bir saatlik, kullanılan Azure altyapı harcamasına bağlı tarife çekirdek başına (vCPU) olarak sunulur. Bu, yalnızca Azure uygulamaları ve sanal makineler için geçerlidir.

* Abonelik fiyatlandırması (site tabanlı): Yazılım Lisans ücretleri aylık, yinelenen bir ücret sunulur.  Bu, yalnızca Azure uygulamaları – yönetilen uygulamalara ve SaaS uygulamaları için geçerlidir.

* Ücretsiz yazılım denemesi: 30 gün veya 90 gün için yazılım lisansı için ücret alınmaz.

### <a name="free-and-bring-your-own-license-byol-pricing"></a>Ücretsiz ve-kendi-lisansını getir (KLG) fiyatlandırma

Ücretsiz veya Getir-kendi lisansını işlem teklifi yayımlama sırasında Microsoft satış işlemi, yazılım lisans ücretleri için etkinleştirme bir rol oynar değil. Liste ve deneme yayımlama seçeneklerini gibi yayımcı yazılım lisansı ücretlerini %100 tutar. 

### <a name="pay-as-you-go-and-subscription-site-based-pricing"></a>Kullandıkça Öde ve abonelik (site tabanlı) fiyatlandırması

Microsoft, bir Kullandıkça Öde veya abonelik işlem teklifi yayımlama sırasında gerekli teknolojiyi ve Hizmetleri işlemi yazılım lisans satın alma işlemleri, döndürür ve yansıtma sağlar. Bu senaryoda, yayımcı, bu amaçlar için bir aracı görev yapacak Microsoft yetkisi verir. Yayımcı, Microsoft yazılım lisans işlem, satıcı, sağlayıcı, dağıtımcı ve lisans veren olarak belirtimlerine korurken kolaylaştırmak sağlar.

Microsoft, hüküm ve koşullar hem Azure marketi, hem de publisher'ın son kullanıcı lisans sözleşmesi subjecting müşterilerin sipariş, lisans ve yayımcı yazılımı sağlar (bulut iş ortağı portalı bakın). Yayımcılar, Market teklifi, son kullanıcı lisans sözleşmesi sağlamanız gerekir.

Market ile işlendi siparişler, tek bir faturada aynı fatura yöntemi olarak müşterinin Azure altyapı maliyetleri Azure aboneliğinde müşteri için faturalandırılırsınız. Müşteriler, Azure aboneliğinin faturalandırma için kullanılan tercih edilen Faturalama yöntemi ve ödeme aracı kullanabilir.

### <a name="free-software-trials"></a>Ücretsiz yazılım deneme

Yayımlama senaryoları için transact publisher 30 gün boyunca veya 90 günlük ücretsiz kullanılabilir lisans yazılım yapabilirsiniz; İskonto bu özellik, iş ortağı çözümü kullanarak yönetilen Azure altyapı kullanımı maliyetini içermez.

### <a name="private-offers"></a>Özel teklifler

Kullanmanın yanı sıra türleri sunar ve faturalama modelleri için bir teklif paraya çevirin, yayımcılar transact çözüm teklif fiyatlandırması ile belirlenen, anlaşma özgü, tam ve özel yapılandırmalar özelleştirilmiş bir görüntü kullanarak özel bir sürümünü. Özel teklifler tüm tarafından desteklenir 3 transact Yayımlama seçenekleri.

Bu fiyatlandırma seçeneği, daha yüksek veya genel olarak görüntülenen fiyatlandırma daha düşük olabilir.  Özel teklifler indirim için kullanılabilir veya bir premium bir teklif için ekleyin. Özel teklifler bir veya daha fazla müşterilere Azure aboneliğini teklif düzeyinde listeleme beyaz tarafından kullanılabilir hale getirilebilir.

### <a name="examples"></a>Örnekler

**Kullandıkça Öde** 

* Kullandıkça Öde seçeneği etkinleştirirseniz, aşağıdaki yapısı maliyet vardır.

|Lisansınızı maliyeti  | saat başına 1,00 $  |
|---------|---------|
|Azure kullanım maliyeti (D1/1-çekirdek)    |   saat başına 0.14     |
|*Müşteri, Microsoft tarafından faturalandırılır*    |  *saat başına 1,14*       |

* Microsoft, bu senaryoda, yayımlanmış VM görüntülerinden birini kullanmak için saat başına 1,14 düzenler.

|Microsoft faturalar  | saat başına 1,14  |
|---------|---------|
|Microsoft, lisans maliyeti 80 oranında öder|   saat başına 0,80 $     |
|Microsoft, lisans maliyeti %20 tutar.  |  saat başına $0,20       |
|Microsoft Azure kullanım maliyeti %100 tutar. | saat başına 0.14 |

**Kendi lisansınızı getirin (BYOL)**

* KLG seçeneği etkinleştirirseniz, aşağıdaki yapısı maliyet sahip.

|Lisansınızı maliyeti  | Lisans ücreti anlaşma ve sizin tarafınızdan faturalandırılır  |
|---------|---------|
|Azure kullanım maliyeti (D1/1-çekirdek)    |   saat başına 0.14     |
|*Müşteri, Microsoft tarafından faturalandırılır*    |  *saat başına 0.14*       |

* Microsoft, bu senaryoda, yayımlanmış VM görüntülerinden birini kullanmak için saat başına 0.14 düzenler.

|Microsoft faturalar  | saat başına 0.14  |
|---------|---------|
|Microsoft Azure kullanım maliyeti tutar.    |   saat başına 0.14     |
|Microsoft, lisans maliyeti %0 tutar.   |  saat başına 0,00 ABD Doları       |

**SaaS uygulaması aboneliğiniz (satış Azure üzerinden)**

Bu seçeneği Microsoft satmak üzere yapılandırılmalıdır ve kullanarak fiyatlandırılır veya daha fazla sabit fiyat aylık planlar teklif düzeyinde tanımlanan.

* Ardından Azure seçeneği aracılığıyla satış etkinleştirirseniz, aşağıdaki yapısı maliyet sağlayın.

|Lisansınızı maliyeti       | Aylık 100,00 $  |
|--------------|---------|
|Azure kullanım maliyeti (D1/1-çekirdek)    | Doğrudan yayımcıya, müşteri faturalandırılır |
|*Müşteri, Microsoft tarafından faturalandırılır*    |  *Aylık 100,00 $ (Not: Yayımcı tahakkuk veya doğrudan altyapısı maliyetlerin lisans ücreti hesabı gerekir)*  |

* Bu senaryoda, Microsoft yazılım lisansı için 100,00 $ düzenler ve 80.00 out yayımcıya öder.
* Sınırlı bir Market hizmeti ücreti için iş ortaklarıyla Haziran 2020 kadar Mayıs 2019'nden bir SaaS azaltılmış işlem ücreti sunar görürsünüz. Bu senaryoda, Microsoft yazılım lisansı için 100,00 $ düzenler ve 90.00 out yayımcıya öder.

|Microsoft faturalar  | Aylık 100,00 $  |
|---------|---------|
|Microsoft, lisans maliyeti 80 oranında öder <br> \* Microsoft herhangi bir tam SaaS uygulamaları için lisans maliyetinizin %90 öder   |   80.00 başına aylık <br> \* 90.00 başına aylık    |
|Microsoft, lisans maliyeti %20 tutar. <br> \* Microsoft, herhangi bir tam SaaS uygulamaları için lisans maliyeti %10 tutar.  |  20,00 başına aylık <br> \* $10.00     |

* **Sınırlı bir Market hizmeti ücreti:** Belirli ticari Market'e yayımlamak SaaS ürünler, Microsoft, Market hizmeti ücreti %20 değerinden (Microsoft yayımcı anlaşması'nda açıklandığı gibi) % 10 azaltır.  Nitelemek ürününüzün sırayla ürünlerinizi en az biri Microsoft tarafından olarak ya da IP belirlenmesi gerekir ortak satışa hazır ya da IP ortak satış önceliklendirilir. Bu sınırlı bir Market hizmeti ücreti ay için almak için uygunluk en az beş (5) iş günü, takvim ayının sonundan önce karşılanması gerekir. Daha az Market hizmeti ücreti VM'ler, yönetilen uygulamalar veya ticari Marketimizden kullanıma sunulan diğer ürünler için geçerli değildir.  Bu sınırlı bir Market hizmeti ücreti 31 Mayıs 2019 ve 30 Haziran 2020 arasındaki Microsoft tarafından toplanan lisans ücretleri tam teklifler için kullanıma sunulacaktır.  Bu tarihten sonra Market hizmeti ücreti, normal tutara döndürür.

### <a name="customer-invoicing-payment-billing-and-collections"></a>Müşteri faturalama, ödeme ve faturalama koleksiyonları

**Faturalama ve ödeme**

Yayımcı müşteri Faturalama yöntemi tercih edilen abonelik veya DÖNÜŞTÜREBİLMEMİZ yazılım lisansı ücretlerini sunmak için kullanabilirsiniz.

**Kurumsal Anlaşma** 

Müşteri faturalama tercih edilen yöntem Microsoft Kurumsal anlaşması varsa, yazılım lisans ücretleri, bu bir maliyet dökümü, tüm özel Azure kullanım maliyetleri ayrı olarak Faturalama yöntemi kullanılarak faturalandırılır.

**Kredi kartı ve aylık fatura** 

Müşteriler ayrıca kredi kartı ve bir aylık fatura kullanarak ödeme yapabilirsiniz. Bu durumda, yazılım lisans ücretleri yalnızca Kurumsal Anlaşma senaryo gibi bir maliyet dökümü, tüm özel Azure kullanım maliyetleri ayrı olarak faturalandırılır.

Örneğin, müşterinin kredi kartıyla satın alıyorsa:

|Açıklama    |    Date  |
|----------|----------|
|Siparişi dönemi   | 15 Ağu 2018'den itibaren-30 Ağu 2018 |
|Terim bitiş (ay)   | 30 Ağu 2018 |
|Fatura tarihi | 1 Eylül 2018'den |
|Müşteri ödeme tarihi | 1 Eylül 2018'den |
|Süre (kredi kartı yalnızca 30 gün) tutmak | 1 Eylül 2018'den itibaren – 30 Eylül 2018'e |
|Toplama dönemi başlangıç | 1 Eylül 2018'den |
|Toplama süresi sonu (en fazla 30 gün) | 30 Eylül 2018'e |
|Ödeme hesaplama tarihi (aylık 15'inden) | 1 Ekim 2018'den |
|Ödeme tarihi | 15 Ekim 2018 |

Bir Kurumsal Sözleşmesi'ni kullanarak müşterinin satın alıyorsa:

| Açıklama |    Date  |
|----------|----------|
|Siparişi dönemi | 15 Ağu 2018'den itibaren-30 Ağu 2018 |
|Terim bitiş (üç ay) | 30 Eylül 2018'e |
|Fatura tarihi | 15 Ekim 2018 |
|Süre (kredi kartı yalnızca 30 gün) tutmak | yok |
|Toplama dönemi başlangıç | 15 Ekim 2018 |
|Toplama süresi sonu (en fazla 90 gün) | 15 Ocak 2019 |
|Müşteri ödeme tarihi | 30 Aralık 2018'e |
|Ödeme hesaplama tarihi (aylık 15'inden) | 15 Ocak 2019 |
|Ödeme tarihi | 15 Şubat 2019 |

**Ücretsiz KREDİLERİ ve parasal taahhüt** 

Bazı müşteriler, Azure, Kurumsal Anlaşma parasal taahhüdü ile ön ödeme veya Azure ile kullanmak için ücretsiz krediler sağlanmış olan bırakmayı. KREDİLERİ, Azure kullanımı için ödenecek kullanılabilir olsa da, yayımcı yazılım lisansı ücretlerini ödemek için kullanılamaz.

**Faturalandırma ve koleksiyonları** 

Yayımcı yazılım lisansı faturalandırma, faturalama müşteri seçili yöntemi kullanılarak sunulur ve faturalandırma zaman çizelgesi izler. Bir Kurumsal Anlaşma yerinde olmayan müşteriler Market yazılım lisanslarında aylık olarak faturalandırılır. Bir kurumsal anlaşması olan müşteriler, üç aylık olarak sunulan bir fatura aylık olarak faturalandırılır.

Abonelik veya Kullandıkça Öde fiyatlandırma modelleri seçildiğinde, Microsoft yayımcı aracı görev yapar ve faturalama, ödeme ve koleksiyon tüm yönlerini sorumludur.

### <a name="publisher-payout-and-reporting"></a>Yayımcı ödeme ve Raporlama

* Bir aracı Microsoft tarafından toplanan yazılım lisans ücretleri aksi belirtilmediği sürece bir % 20 işlem ücreti tabi olan ve yayımcı ödeme zamanında düşülür.

* Kurumsal Anlaşma kullanarak müşteriler genellikle satın veya Kullandıkça Öde sözleşmesi bir kredi kartı etkin. Faturalandırma, faturalama, koleksiyon ve ödeme zamanlamanızda sözleşme türünü belirler.

>[!NOTE] 
>Tüm raporlar ve Öngörüler için yayımlama seçeneği transact bulut iş ortağı portalı Insights bölümünü kullanılabilir.

#### <a name="billing-questions-and-support"></a>Faturalama soruları ve Destek

Daha fazla bilgi ve yasal ilkeleri için bkz. [yayımcı anlaşması](https://cloudpartner.azure.com/Content/Unversioned/PublisherAgreement2.pdf) (bulut iş ortağı Portalı'nda kullanılabilir).

Faturalama soruları hakkında Yardım almak için [bir destek olayı oluşturmak](https://support.microsoft.com/getsupport?wf=0&tenant=classiccommercial&oaspworkflow=start_1.0.0.0&pesid=16230&forceorigin=esmc&ccsid=636764613233453423) ve kullanılan teklif türüne bağlı olarak sanal makineler veya Web Apps (SaaS uygulamaları olarak da bilinir) seçin.

## <a name="transact-requirements"></a>Gereksinimleri transact

Farklı bir teklif türleri için transact gereksinimleri, bu bölümde ele alınmıştır.

### <a name="requirements-for-all-offer-types"></a>Türleri için tüm gereksinimleri sunar

**Geliştirme Merkezi ve Microsoft hesabı** 

* Seçeneği, teklifinizi bakılmaksızın yayımlama transact fiyatlandırma modeli için bir geliştirici merkezinde hem de bir Microsoft hesabı gereklidir.
* Geliştirme Merkezi hesabı, Microsoft yayımcının adınıza müşteriden ücreti tahsil yayımcıya buna göre ödeme yapmanız için gereken tüm ilgili Finans bilgilerini tutar.
* Her iki hesap arasında aynı kuruluş veya Microsoft oturum açma ayrıntıları kullanabilir ancak Geliştirme Merkezi bulut iş ortağı portalı hesaptan ayrı bir hesaptır. Yayımcı transact yayımlama seçeneğini kullanmak için bulut iş ortağı portalına erişim kaydolma yanı sıra Geliştirme Merkezi hesabı oturum açma işlemi tamamlamanız gerekir.

*Bu hesaplar ayarlama hakkında daha fazla bilgi için bkz. [bulut Market yayımcısı haline](https://docs.microsoft.com/azure/marketplace/become-publisher).*

### <a name="requirements-for-specific-offer-types"></a>Belirli teklif türleri için gereksinimleri

Transact yayımlama seçeneği yalnızca aşağıdaki Market teklifi türleriyle kullanılabilir: 

**Sanal Makine** 

Ücretsiz, getirin-kendi lisansını veya kullandıkça-as-you-Öde fiyatlandırma-modellerinden seçin ve teklif düzeyinde tanımlanan SKU'ları olarak sunar. Müşterinin Azure faturalarınız Microsoft yayımcı yazılım lisansı ücretlerinden ayrı olarak temel alınan Azure altyapı ücretlerini gösterir. Azure altyapı ücretleri yazılımın kullanımı, yayımcı tarafından yönlendirilir.

**Azure uygulamaları için: Çözüm şablonu veya yönetilen uygulama** 

Bir veya daha fazla sanal makine ve toplam sanal makine fiyatlandırması üzerinden çeken hazırlamanız gerekir. Fiyatlandırma modeli olarak bunun yerine sanal makine fiyatlandırma sabit fiyat aylık bir aboneliğe tek bir plan üzerinde yönetilen uygulamalar için seçilebilir. Her iki durumda da, Azure altyapı kullanım ücretleri müşteri için ayrı ayrı yazılım lisans ücretleri, ancak aynı fatura ekstresi geçirilir.

## <a name="next-steps"></a>Sonraki adımlar

* Seçme ve yapılandırma teklifinizin sonlandırmak için Teklif türü bölümünde yayımlama seçeneklerini uygunluk gereksinimlerini gözden geçirin.
* Yayımlama modelleri, çözümünüzü bir teklif türüne ve yapılandırmayı eşlemelerini nasıl ilişkin örnekler için mağaza tarafından gözden geçirin.
* Market yayımcısı haline gelir ve oturum açın [bulut iş ortağı portalı](https://cloudpartner.azure.com) oluşturmak ve teklifinizi yapılandırmak için.
