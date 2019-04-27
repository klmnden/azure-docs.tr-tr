---
title: Web analizi | Microsoft Docs
description: .
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: pbutlerm
manager: Ricardo.Villalobos
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 09/13/2018
ms.author: pbutlerm
ms.openlocfilehash: 89cc8c4bffe910de0861d7f44925a10df3811fdb
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60745705"
---
<a name="web-analytics"></a>Web analizi
=============

Bu makalede, işletmenizi büyütün, öğrenin ve en iyi Web analizi kullanma hakkında yönergeler sağlar. Şu anda bu öngörüleri sekme AppSource herhangi bir teklif için kullanılabilir.

Oluşturulan ve yayımlanan teklifinizi göre Yolculuğunuzun sonraki bölümü izlemek ve ölçmek için olan kendi\' başarılı. İle **Web analizi**, tam olarak ne kadar iyi her tekliflerin Market'te yapıyor görme olanağı ekledik. Yolculuğunuza başlamak için yeni analiz sekmesinde görmek için bulut iş ortağı portalının sol tarafındaki Öngörüler sayfasına gidin.

![WebAnalytics sayfası](./media/si-getting-started/WebAnalytics1.png)

Microsoft Power BI ile oluşturulmuş ve her tekliflerinize görmenize olanak sağlayan, yayımcı Kimliğiniz için zengin bir Pano görürsünüz\' verileri günlük olarak yenilenir.

<a name="microsoft-campaigns"></a>**Microsoft kampanyaları**
-----------------------

Tekliflerinizi büyütün ve büyüme tekliflerinize izlemek için kullanma olanağı sağladık **Microsoft kampanyaları** bulut iş ortağı portalı. Kampanyalar için müşterilerin uygulama ayrıntıları sayfasına gönderen farklı kanallar izlemenize imkan tanıyacak Market'e yeni desteklenen bir özelliğidir.

### <a name="how-to-make-a-campaign"></a>**Bir kampanya yapma**

Kampanyaları açıklamak için basit, özel bir word/terimi gölünüzdeki URL'nizi Market'te Uygulama Ayrıntıları sayfasında ekliyorsanız yoldur. Google, Bing, LinkedIn ve birçok diğer sitelere bir reklam oluşturun, istenen sitenize kendi sitesinden bağlantı teşvik edin.

Genel olarak, sürücü yeni müşteriler, ürünle yardımcı olmak için bu çalışmaların olan ve her biri kanallarınızın nasıl çalıştığını başarısını ölçmek için önemlidir. Bu, kampanyaları burada devreye girer.

Kendi kampanyanızı oluşturmak için iki yolunuz vardır.

1. Sorgu parametresi URL'nizi ekleyin **mktcmpid** açıklayan kampanya nedir ve hangi sayfası/olayı bu müşterilerin geldiğini.

Örneğin kullanabilirsiniz: <https://appsource.microsoft.com/product/dynamics-365/contoso.offername?mktcmpid=NewCampaign>

1. (Gelişmiş): URL'de bizim desteklenen, genel kampanya kimlikleri birini kullanın. Otomatik olarak bu ek etiketleri tanımak için kuralı destekliyoruz, kullanmanız gereken ek başvuru etiketlerle destekleme olmasını istiyoruz:
    
    1. **utm\_kampanyası**
    2. **utm\_kaynak**
    3. **başvuru**
    4. **src**

Örneğin kullanabilirsiniz: <https://appsource.microsoft.com/product/dynamics-365/contoso.offername?utm_campaign=NewCampaign>

Birden çok birleşimine sahip seçtiğiniz daha fazla müşteri nereden geldiğini (e-posta, blog, sosyal medya kaynağı, vb.) gibi bir kampanya için trafiği yönlendiren birden çok kaynağı tanımlamak için bu kampanyanın kimlikleri.

Örneğin:

1. Bülten başvuran:  <https://appsource.microsoft.com/product/dynamics-365/contoso.offername?mktcmpid=NewCampaign&src=newsletter>
2. LinkedIn başvuran:  <https://appsource.microsoft.com/product/dynamics-365/contoso.offername?mktcmpid=NewCampaign&src=LinkedIn>

### <a name="ensuring-campaigns-pass-through-all-your-pages"></a>**Tüm sayfaları kalmasını sağlama kampanyaları geçirin**

Kampanyalarınızın müşterileri Market'te göndermeye devam eder, trafiği yürüten bir ara sayfası sahip olduğu bir senaryoda olabilir. Market'te gönderdiğiniz son URL ilk kampanyanızı kimlikleri geçirmek önemlidir.

Örnek aşağıda verilmiştir:

1. Pazarlama çalışan sürücü trafiği şirket için Google'dan reklam satın\'s giriş sayfası <https://contoso.com>. Bu giriş sayfası olan bir \"deneyin my ürün\" giden bağlantı <https://appsource.com>.
2. Bir kullanıcı ad tıklar ve kendi şirket gölünüzdeki\'s giriş sayfası.
    1.  Başvuru URL google.com =
    2.  Giriş sayfası URL'si = <https://contoso.com/?utm_campaign=MyCampaignAdName&utm_source=MySourceAdName>
3. Kullanıcı \"deneyin my ürün\" bağlamak ve Appsource'ta geçer.
    1. Başvuru URL =  <https://contoso.com/?utm_campaign=MyCampaignAdName&utm_source=MySourceAdName>
    2. Giriş sayfası URL'si (**bu URL'yi utm olduğundan emin olun\_kampanya ve utm\_bu URL'ye eklenen kaynak**) = [ https://appsource.microsoft.com/en-us/product/dynamics-365/contoso.offername? **utm\_kampanya MyCampaignAdName = & utm\_kaynak MySourceAdName =**](https://appsource.microsoft.com/en-us/product/dynamics-365/contoso.offername?utm_campaign=MyCampaignAdName&utm_source=MySourceAdName)

<a name="how-to-evaluate-the-success-of-a-campaign"></a>Bir kampanya başarısını değerlendirme
-----------------------------------------

### <a name="page-visits-by-campaign"></a>**Sayfasını ziyaret kampanyaya göre**

![WebAnalytics2](./media/si-getting-started/WebAnalytics2.png)

Her günlük, sayfa ziyareti kaynaklarına kampanyaya göre dökümüdür.

### <a name="conversion-rate-by-campaign"></a>**Kampanyaya göre dönüştürme oranı**

![WebAnalytics3](./media/si-getting-started/WebAnalytics3.png)

Benzer şekilde, tüm teklifinizin dönüştürme oranı dökümünü farklı kampanyalarınızın nasıl yaptığını görebilirsiniz. Bu grafikte nasıl göstereceğiz. Bu grafik nereden yüksek dönüştürme oranı kampanyalarınızın geldiğini belirlemenize yardımcı olmalıdır.

### <a name="distribution-by-campaign"></a>**Kampanyaya göre dağılım**

![WebAnalytics4](./media/si-getting-started/WebAnalytics4.png)

Benzer şekilde, müşterilerinizin konumundaki etki alanları nasıl baktığımızda, bu grafik, verilerinizin kullanıcıları Market'te altında geliyor ve kampanya başına dağıtım görmenize olanak sağlar. \_NoCampaign bunlar Market'te gittiğinizde, müşterinin bir kampanya kimliği URL'de olmadığını anlamına gelir.

<a name="next-steps"></a>**Sonraki Adımlar**
--------------

Tekliflerinizi başarı izleme olanağı olduğuna göre kendi kampanyaları oluşturun geçirmenizi istiyoruz.

Soru/özellik istekleriniz varsa, bunları sağ üst köşede bulunan geri bildirim, aracılığıyla paylaşın.

![Bulut iş ortağı Portalı'nda geri bildirim](./media/si-getting-started/WebAnalytics5.png)
