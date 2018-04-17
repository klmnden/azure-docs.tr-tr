---
title: Azure uygulama Öngörüler kullanım Cohorts | Microsoft docs
description: Farklı ayarlar veya kullanıcılar, oturumlar, olayları veya bir şey ortak olan işlemler Çözümle
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 04/10/2018
ms.author: mbullwin ; daviste
ms.openlocfilehash: 68453499cc6477cc079a342906614e6873938fc8
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="application-insights-cohorts"></a>Uygulama Öngörüler Cohorts

Bir kohort, kullanıcılar, oturumlar, olayları veya bir şey ortak olan işlemler kümesidir. Azure Application Insights'ta cohorts Analytics sorgu tarafından tanımlanır. Kendinizi kullanıcıların belirli bir dizi çözümleme bulmak veya olayları tekrar tekrar cohorts, tam olarak ilgilendiğiniz kümesini ifade etmek için daha fazla esneklik verebilirsiniz varsa.

![Cohorts bölmesi](.\media\app-insights-usage-cohorts\001.png)

## <a name="cohorts-versus-basic-filters"></a>Cohorts temel filtreleri karşılaştırması

Cohorts benzer şekilde filtre olarak kullanılır, ancak bir kohort'ın tanımı özel Analytics sorgularından yerleşik olgu daha uyarlanabilir ve karmaşık olmasını sağlar. Filtreler farklı olarak, diğer ekibinizin üyeleri kaybetmemek için cohorts kaydedebilirsiniz.

Tüm yeni bir özellik, uygulamanızda çalışmış kullanıcıların kohort tanımlayabilirsiniz. Application Insights kaynağınıza kaydedilen bu kohort ile bu belirli kullanıcı grubu, birini tıklatın hemen gelecekte çözümleme kolaylaştırır.

> [!NOTE]
> Oluşturduktan sonra kullanıcılar, oturumlar, olayları ve kullanıcı akar araçlarından cohorts kullanılabilir.

## <a name="example-engaged-users"></a>Örnek: kullanıcıların bağlı

Ekibinizin bağlı bir kullanıcı uygulamanızı beş veya daha fazla kez belirli bir ay içinde kullanan herhangi biri olarak tanımlar. Şimdi bu katılımcı kullanıcıların kohort tanımlayın.

1. Açık **Cohorts** aracı.

2. Tıklatın **Şablon Galerisi** sekmesi. Burada şablonları koleksiyonu için çeşitli cohorts bulabilirsiniz.

3. Seçin **kullanıcıların bağlı** – gün tarafından kullanılan ".

    Bu kohort için üç parametre vardır:
      * **Etkinlikler** imkan sağlayan hangi olayları ve sayfa görünümleri "kullanım" olarak say seçin.
      * **Dönem** ayda bir tanımı.
      * **UsedAtleastCustom** bir şey bağlı olan bir kullanıcı olarak saymak için bir süre içinde kullanmak için ihtiyaç duydukları sayısı.

4. Değişiklik **UsedAtleastCustom** "5 + gün" ve bırakın **süresi** 28 gün varsayılan.

    ![Görüntü](.\media\app-insights-usage-cohorts\003.png)

    Şimdi bu kohort beş ayrı gün son 28 gün içinde hiçbir özel olay ya da sayfa görünümü ile gönderilen tüm kullanıcı kimlikleri temsil eder.

5. **Kaydet**’e tıklayın.

   > [!TIP]
   >  Kohort gibi bir ad verin "Engaged kullanıcıların (5 + gün)" ve "Raporlarım için" kaydedin veya "Bu kohort görmek için bu kullanılıyor Öngörüler kaynağa erişimi olan diğer kişiler istiyorsanız raporları bağlı olarak paylaşılan".

6. Tıklatın **geri Galeriye**.

### <a name="what-can-you-do-with-this-cohort"></a>Bu kohort ile neler yapabileceğiniz?

Açık **kullanıcılar** aracı > içinde **Göster** açılır > Seç altında oluşturduğunuz kohort **ait kullanıcılar...**

Artık kullanıcılar aracını bu kullanıcıların kohort için filtre uygulanır:

![Belirli bir kohort için filtre kullanıcılar bölmesi](.\media\app-insights-usage-cohorts\004.png)

Fark edilecek bazı önemli noktalar:
   * Bu, normal filtreleri yoluyla oluşturulan uygulanamadı bir kümesidir. Tarih mantığı daha gelişmiş.
   * Daha fazla normal filtreleri kullanıcılar aracını kullanarak bu kohort filtreleyebilirsiniz. Bu nedenle kohort 28 günlük windows üzerinde tanımlı olsa da, 30, 60 ya da 90 gün için kullanıcıların aracında zaman aralığı ayarlayabilirsiniz. 

Bu, gibi daha karmaşık sorular sormak sağlar: _nasıl aynı kişilerle son 28 gün içinde bağlı kişiler için son 60 gün içinde davranacağını?_ , aksi takdirde olacaktır Sorgu Oluşturucusu express mümkün.

## <a name="example-events-cohort"></a>Örnek: olayları kohort

Olayların cohorts de yapabilirsiniz. Şimdi kohort sayfa görünümleri ve olayların tanımlayın ve ardından diğer Araçları'ndan kullanılması hakkında bilgi. Bu ekibinizin göz önünde bulundurur olayları kümesini tanımlamak kullanışlı olabilir _etkin kullanım_, ya da belirli yeni bir özellik için ilgili olayları kümesini tanımlamak için.

1. Açık **Cohorts** aracı.

2. Tıklatın **Şablon Galerisi** sekmesi. Burada şablonları koleksiyonu için çeşitli cohorts bulabilirsiniz.

3. Seçin **olayları Seçici**.

    ![Ekran olaylar Seçici](.\media\app-insights-usage-cohorts\006.png)

4. İçinde **etkinlikleri** açılan listesinde, kohort olmasını istediğiniz olayları seçin

5. Kohort kaydedin ve bir ad verin.

## <a name="example-active-users-where-you-modify-query"></a>Örnek: sorguyu değiştirin burada etkin kullanıcılar

Önceki iki cohorts bırakmalar kullanarak tanımlanmıştır. Ancak biz de cohorts toplam esneklik için analitik sorguları ile tanımlayabilirsiniz. Görelim İngiltere kullanıcıların kohort oluşturarak nasıl.

![Cohorts aracını kullanarak taramasını animasyonlu resmi](.\media\app-insights-usage-cohorts\cohorts0001.gif)

1. Açık **Cohorts** aracı > tıklatın **Şablon Galerisi** sekmesini > Seç **boş kullanıcılar kohort**.

    ![Boş kullanıcılar Kohort](.\media\app-insights-usage-cohorts\001.png)

    Üç bölüm bulunur:
       * Burada, daha ayrıntılı kohort başkaları için takımınızın açıklayabilirsiniz Markdown metin bölümü.

       * Gibi kendi parametrelerinizi yapmak için kullanabileceğiniz parametreler bölümünden **etkinlikleri** ve diğer bırakmalar önceki iki örneklerle.

       * Analytics sorgusu kullanılarak kohort tanımlamak için kullandığınız bir sorgu bölüm.

    Sorgu bölümünde, [Analytics sorgusu yazma](https://docs.loganalytics.io/index) belirli kümesini tanımlamak istediğiniz kohort açıklayan satırları seçer. Cohorts aracı ekler sonra örtük olarak bir "| USER_ID tarafından özetlemek"yan tümcesi sorgulanamıyor. Sorgunuzun sonuçlarını döndürmektir emin olabilmeniz Bu tablo sorguda aşağıda önizlemesini görebilirsiniz.

    > [!NOTE]
    > Sorgu görmüyorsanız, eninden olun ve sorgu ortaya çıkarmak için bölümü yeniden boyutlandırmayı deneyin. Bu bölümün başında animasyonlu .gif yeniden boyutlandırma davranışı gösterir.

2. Aşağıdaki sorgu düzenleyicisine kopyalayıp yapıştırın:

    ```KQL
    union customEvents, pageViews
    | where client_CountryOrRegion == "United Kingdom"
    ```

3. Tıklatın **sorgusu**. Kullanıcı kimliklerini tabloda görünen görmeniz gerekir. Değilse, uygulamanız kullanıcıların sahip olduğu bir ülke değiştirin.

4. Kaydedin ve kohort olarak adlandırın.

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

_Kullanıcıların belirli bir ülkeden kohort tanımlamış olabilirsiniz. Bu kohort yalnızca söz konusu ülkenin kullanıcılar aracında bir filtre ayarı için kullanıcıların aracında karşılaştırdığınızda, farklı sonuçlar bakın. Neden?_

Cohorts ve filtreleri farklıdır. (Yukarıdaki örnekteki gibi tanımlanır) İngiltere kullanıcıların kohort varsa ve filtre ayarına sonuçlarını karşılaştırın varsayalım "ülke veya bölge İngiltere =."

* Kohort sürüm en az bir olay İngiltere geçerli zaman aralığı içinde gönderilen kullanıcıların tüm olayları gösterir. Ülke veya bölgeye göre bölerseniz, birçok ülke ve bölgelerden büyük olasılıkla görürsünüz.
* Filtreler sürümü yalnızca İngiltere olayları gösterir. Ülke veya bölgeye göre bölerseniz, yalnızca İngiltere göreceğiniz ise.

## <a name="learn-more"></a>Daha fazla bilgi edinin
- [Analytics sorgu dili](https://go.microsoft.com/fwlink/?linkid=856587)
- [Kullanıcılar, Oturumlar, Etkinlikler](app-insights-usage-segmentation.md)
- [Kullanıcı Akışları](app-insights-usage-flows.md)
- [Kullanıma genel bakış](app-insights-usage-overview.md)