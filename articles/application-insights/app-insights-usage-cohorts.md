---
title: Azure Application Insights kullanım cohorts | Microsoft Docs
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
ms.openlocfilehash: f8d566f552c86f749b914ffed70512209ad76ab7
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
---
# <a name="application-insights-cohorts"></a>Uygulama Öngörüler cohorts

Bir kohort, kullanıcılar, oturumlar, olayları veya bir şey ortak olan işlemler kümesidir. Azure Application Insights'ta cohorts analytics sorgu tarafından tanımlanır. Belirli bir analiz etmek için sahip olduğu durumda kullanıcıları ayarlamak veya olayları tekrar tekrar cohorts, tam olarak ilgilendiğiniz kümesini ifade etmek için daha fazla esneklik verebilirsiniz.

![Cohorts bölmesi](.\media\app-insights-usage-cohorts\001.png)

## <a name="cohorts-versus-basic-filters"></a>Cohorts temel filtreleri karşılaştırması

Cohorts filtreleri için benzer şekilde kullanılır. Ancak çok daha uyarlanabilir ve karmaşık bulunmaları cohorts tanımları özel analytics sorgularından oluşturulur. Filtreler farklı olarak, diğer ekibinizin üyeleri kaybetmemek için cohorts kaydedebilirsiniz.

Tüm yeni bir özellik, uygulamanızda çalışmış kullanıcıların kohort tanımlayabilirsiniz. Application Insights kaynağınıza bu kohort kaydedebilirsiniz. Belirli kullanıcılar bu kaydedilmiş grubunu gelecekte analiz kolaydır.

> [!NOTE]
> Bunlar oluşturduktan sonra kullanıcılar, oturumlar, olayları ve kullanıcı akar araçlarından cohorts kullanılabilir.

## <a name="example-engaged-users"></a>Örnek: kullanıcıların bağlı

Ekibinizin bağlı bir kullanıcı uygulamanızı beş veya daha fazla kez belirli bir ay içinde kullanan herhangi biri olarak tanımlar. Bu bölümde, bu katılımcı kullanıcıların kohort tanımlayın.

1. Cohorts Aracı'nı açın.

2. Seçin **Şablon Galerisi** sekmesi. Şablonları koleksiyonu için çeşitli cohorts bakın.

3. Seçin **katılımcı kullanıcıları--gün tarafından kullanılan**.

    Bu kohort için üç parametre vardır:
    * **Etkinlikler**burada hangi olayları ve sayfa görünümleri "kullanım" olarak say seçin
    * **Dönem**, ayda bir tanımı.
    * **UsedAtleastCustom**, sayı sürelerinin kullanıcılar bir şey bir dönem içinde bağlı olarak saymak için kullanmanız gerekir.

4. Değişiklik **UsedAtleastCustom** için **5 + gün**, bırakıp **süresi** 28 gün varsayılan.

    ![Katılımcı kullanıcılar](.\media\app-insights-usage-cohorts\003.png)

    Bu kohort 5 herhangi özel olay ya da sayfa görünümü ile gönderilen tüm kullanıcı kimlikleri temsil eden artık gün öncesi ayrı 28.

5. **Kaydet**’i seçin.

   > [!TIP]
   >  Kohort gibi bir ad verin "Engaged kullanıcıların (5 + gün)." "Raporlarım için" kaydetmek veya "raporları, bu kohort görmek için bu Application Insights kaynağına erişimi olan diğer kişiler mi istediğinize bağlı olarak paylaşılan".

6. Seçin **geri Galeriye**.

### <a name="what-can-you-do-by-using-this-cohort"></a>Bu kohort kullanarak neler?

Kullanıcıların Aracı'nı açın. İçinde **Göster** açılır kutusunda, oluşturduğunuz altında kohort seçin **ait kullanıcılar**.

Artık kullanıcılar aracını bu kullanıcıların kohort için filtre uygulanır:

![Belirli bir kohort için filtre kullanıcılar bölmesi](.\media\app-insights-usage-cohorts\004.png)

Fark edilecek bazı önemli noktalar:
* Normal filtreleri yoluyla bu kümesi oluşturulamıyor. Tarih mantığı daha gelişmiş.
* Bu kohort kullanıcılar aracında normal filtreleri kullanarak daha fazla filtre uygulayabilirsiniz. Bu nedenle kohort 28 günlük windows üzerinde tanımlı rağmen 30, 60 ya da 90 gün için kullanıcıların aracında zaman aralığı ayarlayabilirsiniz.

Bu filtreler Sorgu Oluşturucusu express imkansız daha karmaşık soruları destekler. Örnek _son 28 gün içinde bağlı kişiler. Aynı kişilerle son 60 gün içinde nasıl davranacağını?_

## <a name="example-events-cohort"></a>Örnek: Olayları kohort

Olayların cohorts de yapabilirsiniz. Bu bölümde, sayfa görünümleri ve olayların kohort tanımlayın. Daha sonra bunları diğer Araçları'ndan nasıl kullanacağınızı bakın. Bu kohort ekibinizin göz önünde bulundurur olayların kümesini tanımlayabilir _etkin kullanım_ veya belirli yeni bir özellik için ilgili kümesi.

1. Cohorts Aracı'nı açın.

2. Seçin **Şablon Galerisi** sekmesi. Şablonları koleksiyonu için çeşitli cohorts görürsünüz.

3. Seçin **olayları Seçici**.

    ![Olayları Seçici ekran görüntüsü](.\media\app-insights-usage-cohorts\006.png)

4. İçinde **etkinlikleri** açılır kutusunda, kohort olmasını istediğiniz olayları seçin.

5. Kohort kaydedin ve bir ad verin.

## <a name="example-active-users-where-you-modify-a-query"></a>Örnek: bir sorguyu değiştirin burada etkin kullanıcılar

Önceki iki cohorts açılan kutuları kullanarak tanımlanmıştır. Ancak, toplam esneklik için analitik sorguları kullanarak cohorts tanımlayabilirsiniz. Görmek için nasıl kohort kullanıcıların İngiltere oluşturun.

![Cohorts aracını kullanarak taramasını animasyonlu resmi](.\media\app-insights-usage-cohorts\cohorts0001.gif)

1. Cohorts Aracı'nı açın, **Şablon Galerisi** sekmesini tıklatın ve seçin **boş kullanıcılar kohort**.

    ![Boş kullanıcılar kohort](.\media\app-insights-usage-cohorts\001.png)

    Üç bölüm bulunur:
    * Burada, daha ayrıntılı kohort başkaları için takımınızın açıklayan bir Markdown metin bölümü.

    * Burada yaptığınız kendi parametreleri gibi Parametreler bölümünde, **etkinlikleri** ve diğer açılan kutuları önceki iki örneklerle.

    * Kohort analytics sorgusu kullanarak tanımladığınız bir sorgu bölüm.

    Sorgu bölümünde, [analytics sorgusu yazma](https://docs.loganalytics.io/index). Sorgu belirli kümesini tanımlamak istediğiniz kohort açıklayan satırları seçer. Cohorts aracı ekler sonra örtük olarak bir "| USER_ID tarafından özetlemek"yan tümcesi sorgulanamıyor. Sorgunuzun sonuçlarını döndürmektir emin olabilmeniz bu verileri bir tablo sorguda aşağıda önizlemesini görebilirsiniz.

    > [!NOTE]
    > Sorgu görmüyorsanız, eninden olun ve sorgu ortaya çıkarmak için bölümü yeniden boyutlandırmayı deneyin. Bu bölümün başında animasyonlu .gif yeniden boyutlandırma davranışı gösterir.

2. Kopyalayın ve sorgu düzenleyicisine aşağıdaki metni yapıştırın:

    ```KQL
    union customEvents, pageViews
    | where client_CountryOrRegion == "United Kingdom"
    ```

3. Seçin **sorgusu**. Kullanıcı kimliklerini tabloda görünen görmüyorsanız, uygulamanız kullanıcıların sahip olduğu bir ülke değiştirin.

4. Kaydedin ve kohort olarak adlandırın.

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

_Kullanıcıların belirli bir ülkeden kohort tanımlamış olabilirsiniz. Yalnızca bu ülkeye göre bir filtre ayarlamak için kullanıcıların aracında bu kohort karşılaştırdığınızda, farklı sonuçlar bakın. Neden?_

Cohorts ve filtreleri farklıdır. (Önceki örnekte gibi tanımlanan) İngiltere kullanıcıların kohort varsa ve filtre ayarına sonuçlarını karşılaştırın varsayalım "ülke veya bölge İngiltere =."

* Kohort sürümü, bir veya daha fazla olayları İngiltere geçerli zaman aralığı içinde gönderilen kullanıcıların tüm olayları gösterir. Ülke veya bölgeye göre bölerseniz, birçok ülke ve bölgelerden olasılıkla bakın.
* Filtreler sürümü yalnızca İngiltere olayları gösterir. Ancak ülke veya bölgeye göre bölerseniz, yalnızca İngiltere bakın.

## <a name="learn-more"></a>Daha fazla bilgi edinin
- [Analytics sorgu dili](https://go.microsoft.com/fwlink/?linkid=856587)
- [Kullanıcılar, oturumlar, olayları](app-insights-usage-segmentation.md)
- [Kullanıcı akışlar](app-insights-usage-flows.md)
- [Kullanıma genel bakış](app-insights-usage-overview.md)