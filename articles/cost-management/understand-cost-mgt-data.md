---
title: Azure maliyet Yönetimi verilerine anlama | Microsoft Docs
description: Bu makalede, hangi verilerin Azure maliyet Yönetimi'nde bulunur ve ne sıklıkta, toplanan, gösterilen ve kapalı işlenir daha iyi anlamanıza yardımcı olur.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 07/01/2019
ms.topic: conceptual
ms.service: cost-management
manager: micflan
ms.custom: ''
ms.openlocfilehash: 44b95c92f51ca9782fca492f3dec3142087ecc91
ms.sourcegitcommit: 66237bcd9b08359a6cce8d671f846b0c93ee6a82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67797013"
---
# <a name="understand-cost-management-data"></a>Maliyet Yönetimi verilerini anlama

Bu makale, Azure maliyet Yönetimi'nde bulunan Azure maliyet ve kullanım verileri daha iyi anlamanıza yardımcı olur. Bu, ne sıklıkta veri toplanan, gösterilen ve kapalı işleneceğini açıklar. Aylık Azure kullanım için faturalandırılırsınız. Faturama olan aylık dönemler döngüsü başlangıç ve bitiş tarihleri abonelik türüne göre farklılık rağmen. Maliyet Yönetimi veri değişir kullanım ne sıklıkta alan farklı etkenlere bağlı olarak. Verileri işlemek için ne kadar sürer ve Azure Hizmetleri kullanımı faturalandırma sistemine ne sıklıkta yayma gibi faktörleri içerir.

Maliyet yönetimi, tüm kullanım ve satın alma işlemleri, ayırmaları ve üçüncü taraf teklifleri, Kurumsal Anlaşma (EA) hesapları için de dahil olmak üzere içerir. Microsoft Müşteri Sözleşmesi (MCA) hesapları ve tek tek abonelikleri Kullandıkça Öde tarifesine göre ile yalnızca Azure ve Market Hizmetleri kullanımınızdan içerir. Destek ve diğer maliyetleri dahil değildir. Maliyet tahmini fatura oluşturulana kadar ve krediler faktör değildir.

## <a name="supported-microsoft-azure-offers"></a>Desteklenen Microsoft Azure teklifleri

Aşağıdaki bilgiler, şu anda desteklenen gösterir [Microsoft Azure'un sunduğu](https://azure.microsoft.com/support/legal/offer-details/) Azure maliyet Yönetimi'nde. Bir Azure teklifi, sahip olduğunuz Azure aboneliğinin türüdür. Başlangıç maliyet Yönetimi'nde veriler kullanılabilir **bulunan veri** tarih. Bir abonelik teklif değişirse, maliyetleri teklif değişiklik tarihinden önce kullanılamaz. 

| **Kategori**  | **Teklif adı** | **Kota kimliği** | **Teklif numarası** | **Kullanılabilir veri** |
| --- | --- | --- | --- | --- |
| **Azure Almanya** | [Azure Almanya Kullandıkça Öde](https://azure.microsoft.com/offers/ms-azr-de-0003p)      | PayAsYouGo_2014-09-01 | MS-AZR-DE-0003P | 2 Ekim 2018<sup>2</sup> |
| **Azure Devlet Kurumları** | Azure kamu Enterprise                                                         | EnterpriseAgreement_2014-09-01 | MS-AZR-USGOV-0017P | Mayıs 2014<sup>1</sup> |
| **Kurumsal Anlaşma (EA)** | Kurumsal Geliştirme ve Test                                                        | MSDNDevTest_2014-09-01 | MS-AZR-0148P | Mayıs 2014<sup>1</sup> |
| **Kurumsal Anlaşma (EA)** | [Microsoft Azure Kurumsal](https://azure.microsoft.com/offers/enterprise-agreement-support-upgrade) | EnterpriseAgreement_2014-09-01 | MS-AZR-0017P | Mayıs 2014<sup>1</sup> |
| **Microsoft Müşteri Sözleşmesi** | [Microsoft Azure-planı](https://azure.microsoft.com/offers/ms-azr-0017g) | EnterpriseAgreement_2014-09-01 | Yok | Mar 2019<sup>3</sup> |
| **Microsoft Müşteri Sözleşmesi** | [Microsoft Azure geliştirme ve Test planlama](https://azure.microsoft.com/offers/ms-azr-0148g) | MSDNDevTest_2014-09-01 | Yok | Mar 2019<sup>3</sup> |
| **Microsoft Geliştirici Ağı (MSDN)** | [MSDN platformları](https://azure.microsoft.com/offers/ms-azr-0062p)<sup>4</sup> | MSDN_2014-09-01 | MS-AZR-0062P | 2 Ekim 2018<sup>2</sup> |
| **Kullandıkça Öde** | [Kullandıkça Öde](https://azure.microsoft.com/offers/ms-azr-0003p)                  | PayAsYouGo_2014-09-01 | MS-AZR-0003P | 2 Ekim 2018<sup>2</sup> |
| **Kullandıkça Öde** | [Kullandıkça Öde geliştirme ve Test](https://azure.microsoft.com/offers/ms-azr-0023p)         | MSDNDevTest_2014-09-01 | MS-AZR-0023P | 2 Ekim 2018<sup>2</sup> |
| **Kullandıkça Öde** | [Microsoft iş ortağı ağı](https://azure.microsoft.com/offers/ms-azr-0025p)      | MPN_2014-09-01 | MS-AZR-0025P | 2 Ekim 2018<sup>2</sup> |
| **Kullandıkça Öde** | [Ücretsiz deneme sürümü](https://azure.microsoft.com/offers/ms-azr-0044p)<sup>4</sup>         | FreeTrial_2014-09-01 | MS-AZR-0044P | 2 Ekim 2018<sup>2</sup> |
| **Kullandıkça Öde** | [Open ile Azure](https://azure.microsoft.com/offers/ms-azr-0111p)<sup>4</sup>      | AzureInOpen_2014-09-01 | MS-AZR-0111P | 2 Ekim 2018<sup>2</sup> |
| **Kullandıkça Öde** | [Öğrenciler için Azure](https://azure.microsoft.com/offers/ms-azr-0170p)<sup>4</sup> | AzureForStudents_2018-01-01 | MS-AZR-0170P | 2 Ekim 2018<sup>2</sup> |
| **Kullandıkça Öde** | Azure Pass<sup>4</sup>                                                            | AzurePass_2014-09-01 | MS-AZR - 0120P, MS - AZR - 0122P, MS-AZR - 0125P, MS - AZR - 0128P - MS-AZR - 0130P | 2 Ekim 2018<sup>2</sup> |
| **Visual Studio** | [Visual Studio Enterprise – MPN](https://azure.microsoft.com/offers/ms-azr-0029p)<sup>4</sup>     | MPN_2014-09-01 | MS-AZR-0029P | 2 Ekim 2018<sup>2</sup> |
| **Visual Studio** | [Visual Studio Professional](https://azure.microsoft.com/offers/ms-azr-0059p)<sup>4</sup>         | MSDN_2014-09-01 | MS-AZR-0059P | 2 Ekim 2018<sup>2</sup> |
| **Visual Studio** | [Visual Studio Test Professional](https://azure.microsoft.com/offers/ms-azr-0060p)<sup>4</sup>    | MSDNDevTest_2014-09-01 | MS-AZR-0060P | 2 Ekim 2018<sup>2</sup> |
| **Visual Studio** | [Visual Studio Enterprise](https://azure.microsoft.com/offers/ms-azr-0063p)<sup>4</sup>           | MSDN_2014-09-01 | MS-AZR-0063P | 2 Ekim 2018<sup>2</sup> |
| **Visual Studio** | [Visual Studio Enterprise: BizSpark](https://azure.microsoft.com/offers/ms-azr-0064p)<sup>4</sup> | MSDN_2014-09-01 | MS-AZR-0064P | 2 Ekim 2018<sup>2</sup> |

_<sup>**1** </sup> Mayıs 2014'ten önce daha fazla veri için ziyaret [Azure Enterprise portal](https://ea.azure.com)._

_<sup>**2** </sup> veri 2 Ekim 2018 tarihinden önce ziyaret edin [Azure hesap Merkezi](https://account.azure.com/subscriptions)._

_<sup>**3** </sup> Microsoft Müşteri anlaşmalarını Mar 2019 içinde çalışmaya ve bu noktadan önce tüm geçmiş verisi yok._

_<sup>**4** </sup> kredi tabanlı ve ödeme ödemeli abonelikler için geçmiş verileri faturanızı değil aynı. Bkz: [geçmiş verileri eşleşmiyor olabilir fatura](#historical-data-might-not-match-invoice) aşağıda._

Aşağıdaki tablo, henüz desteklenmeyen teklifler gösterir.

| Category  | **Teklif adı** | **Kota kimliği** | **Teklif numarası** |
| --- | --- | --- | --- |
| **Azure Almanya** | [Azure Almanya Kullandıkça Öde](https://azure.microsoft.com/offers/ms-azr-de-0003p) | PayAsYouGo_2014-09-01 | MS-AZR-DE-0003P |
| **Bulut çözümü sağlayıcısı (CSP)** | Microsoft Azure                                    | CSP_2015-05-01 | MS-AZR-0145P |
| **Bulut çözümü sağlayıcısı (CSP)** | Azure kamu CSP                               | CSP_2015-05-01 | MS-AZR-USGOV-0145P |
| **Bulut çözümü sağlayıcısı (CSP)** | Microsoft Bulut Almanya için CSP’de Azure Almanya   | CSP_2015-05-01 | MS-AZR-DE-0145P |
| **Kullandıkça Öde**                 | Öğrenciler için Azure Başlangıç | DreamSpark_2015-02-01 | MS-AZR - 0144P |
| **Kullandıkça Öde**                 | [Microsoft Azure Sponsorluğu](https://azure.microsoft.com/offers/ms-azr-0036p/) | Sponsored_2016-01-01 | MS-AZR-0036P |
| **Destek planları** | Standart destek                    | Default_2014-09-01 | MS-AZR-0041P |
| **Destek planları** | Profesyonel doğrudan desteği         | Default_2014-09-01 | MS-AZR-0042P |
| **Destek planları** | Geliştirici Desteği                   | Default_2014-09-01 | MS-AZR-0043P |
| **Destek planları** | Almanya destek planı                | Default_2014-09-01 | MS-AZR-DE-0043P |
| **Destek planları** | Azure kamu standart destek   | Default_2014-09-01 | MS-AZR-USGOV-0041P |
| **Destek planları** | Azure kamu profesyonel doğrudan desteği | Default_2014-09-01 | MS-AZR-USGOV-0042P |
| **Destek planları** | Azure kamu Geliştirici Desteği  | Default_2014-09-01 | MS-AZR-USGOV-0043P |

## <a name="determine-your-offer-type"></a>Teklif türünüz belirleme
Veriler için bir abonelik göremiyor ve aboneliğinizi desteklenen teklifleri altında kalırsa belirlemek istiyorsanız, aboneliğinizin desteklenip desteklenmediğini doğrulayabilirsiniz. Bir Azure aboneliği desteklendiğini doğrulamak için oturum [Azure portalında](https://portal.azure.com). Ardından **tüm hizmetleri** sol menü bölmesinde. Hizmetler listesinde seçin **abonelikleri**. Abonelik listesi menüde, doğrulamak istediğiniz aboneliğe tıklayın. Aboneliğiniz genel bakış sekmesinde gösterilen ve gördüğünüz **teklif** ve **Teklif kimliği**. Aşağıdaki resimde bir örnek gösterilir.

![Abonelik genel bakış sekmesinin teklifi ve teklif kimliği gösteren örnek](./media/understand-cost-mgt-data/offer-and-offer-id.png)

## <a name="costs-included-in-cost-management"></a>Maliyet Yönetimi'nde bulunan maliyetleri

Aşağıdaki tablolarda, maliyet Yönetimi'nde değil ya da eklenmiştir. Bu verileri görüntüleyin. Fatura oluşturulana kadar tüm maliyetleri tahmin edilir. Ücretsiz ve ön ödemeli krediler gösterilen maliyetleri dahil değildir.

**Maliyet ve kullanım verileri**

| **Dahil edilen** | **Dahil değil** |
| --- | --- |
| Azure hizmet kullanımı<sup>5</sup>        | Destek ücretleri - daha fazla bilgi için bkz: [fatura açıklanan terimleri](../billing/billing-understand-your-invoice.md). |
| Market kullanım teklifi<sup>6</sup> | Vergi - daha fazla bilgi için [fatura açıklanan terimleri](../billing/billing-understand-your-invoice.md). |
| Market satın<sup>6</sup>      | Krediler - daha fazla bilgi için [fatura açıklanan terimleri](../billing/billing-understand-your-invoice.md). |
| Rezervasyon satın alma işlemleri<sup>7</sup>      |  |

_<sup>**5** </sup> azure hizmet kullanımı üzerinde ayırma temel alır ve fiyatlarını görüştü._

_<sup>**6** </sup> Market kullanım ve satın alma Kullandıkça Öde için MSDN, mevcut değildir ve Visual Studio, şu anda sunar._

_<sup>**7** </sup> rezervasyon satın alma işlemleri yalnızca Kurumsal Sözleşme (EA) hesapları için kullanılabilir şu anda._

**Meta verileri**

| **Dahil edilen** | **Dahil değil** |
| --- | --- |
| Kaynak etiketleri<sup>8</sup> | kaynak grubu etiketleri |

_<sup>**8** </sup> kaynak etiketleri kullanım her hizmetinden yayıldığını olarak uygulanır ve geçmiş kullanımı için geriye dönük olarak kullanılamaz._

## <a name="rated-usage-data-refresh-schedule"></a>Derecelendirilmiş kullanım Veri Yenileme zamanlaması

Maliyet ve kullanım verileri Azure portalında maliyet Yönetimi + faturalandırma içinde kullanılabilir ve [API'leri destekleme](index.yml). Maliyetleri gözden geçirirken, aşağıdaki noktaları göz önünde bulundurun:

- Geçerli fatura dönemi için tahmini ücretleri altı kat gün güncelleştirilir.
- Daha fazla kullanım tabi olarak, geçerli fatura dönemi için tahmini ücretleri değiştirebilirsiniz.
- Her güncelleştirme toplanır ve tüm satır öğeleri ve önceki güncelleştirme bilgileri içerir.
- Azure sonlandırır veya _kapatır_ geçerli fatura dönemi yukarı fatura dönemi sona erdikten sonra 72 saate (üç takvim günü).

Aşağıdaki örnekler, faturalandırma nasıl son gösterir.

Faturalama ayı 31 Mart sona ererse Kurumsal Anlaşma (EA) abonelikleri – ücretleri yukarı daha sonra 72 saat güncelleştirilir tahmin. Bu örnekte, gece yarısı (UTC) 4 Nisan tarafından.

Faturalama ayının 15 Mayıs, tahmini ücretleri yukarı daha sonra 72 saat güncelleştirilir sonra sona ererse Kullandıkça Öde abonelikleri –. Bu örnekte, gece yarısı (UTC) Mayıs 19 tarafından.

### <a name="rerated-data"></a>Rerated veri

Kullanıp [maliyet Yönetimi API'leri](index.yml), verileri almak için Power BI veya Azure portalında yeniden derecelendirilmiş ve fatura kapatılana kadar sonuç olarak, değiştirmek için geçerli fatura döneminin ücretleri bekler.

## <a name="usage-data-update-frequency-varies"></a>Kullanım verileri güncelleştirme sıklığını değişir

Maliyet Yönetimi'nde tahakkuk kullanım verilerinizin kullanılabilirliğini birkaç dahil olmak üzere bir dizi etkene bağlıdır:

- Ne sıklıkla (örneğin, depolama, bilgi işlem, CDN ve SQL) Azure Hizmetleri kullanımı gösterin.
- Derecelendirme altyapıyı kullanım verilerini işlemek ve maliyet Yönetimi işlem hatları için geçen süre.

Bazı hizmetler, kullanım diğerlerinden daha sık gösterin. Bu nedenle, bazı hizmetleri için maliyet Yönetimi'nde veri verileri daha az sık yayma diğer hizmetleri hemen görebilirsiniz. Genellikle, kullanım Hizmetleri için maliyet Yönetimi'nde görüntülenmesi 8-24 saat sürer. Güncelleştirmeleri birikmeli olduğundan daha fazla kullanım ücretler gibi açık bir ay yenilenir için bu verilere göz önünde bulundurun.

## <a name="historical-data-might-not-match-invoice"></a>Geçmiş verileri fatura aynı olmamalıdır

Geçmiş verileri kredi tabanlı ve önceden ödeme teklifleri, faturanızı eşleşmeyebilir. Bazı Azure Kullandıkça Öde, MSDN ve Visual Studio Azure KREDİLERİ ve Gelişmiş ödemeleri için fatura uyguladığınız sunar. Bununla birlikte, maliyet Yönetimi'nde gösterilen geçmiş verileri yalnızca, tahmini tüketim ücretleri alır. Maliyet Yönetimi geçmiş verileri, ödemeler ve krediler içermez. Sonuç olarak, aşağıdaki teklifler için gösterilen geçmiş verileri tam olarak faturanızı ile eşleşmeyebilir.

- (MS-AZR - 0170P) Öğrenciler için Azure
- Açık (MS-AZR - 0111P)
- Azure geçişi (MS-AZR - 0120P, MS - AZR - 0123P, MS - AZR - 0125P, MS - AZR - 0128P, MS - AZR - 0129P)
- Ücretsiz deneme sürümü (MS-AZR - 0044P)
- MSDN (MS-AZR-0062P)
- Visual Studio (MS-AZR - 0029P, MS - AZR - 0059P, MS - AZR - 0060P, MS - AZR - 0063P, MS - AZR - 0064P)

## <a name="see-also"></a>Ayrıca bkz.

- Maliyet yönetimi için zaten ilk hızlı tamamlamadıysanız, hem okuma [maliyetleri başlamanızı](quick-acm-cost-analysis.md).
