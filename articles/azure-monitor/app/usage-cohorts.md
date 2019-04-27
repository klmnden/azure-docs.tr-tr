---
title: Azure Application Insights kullanım kohortlar | Microsoft Docs
description: Farklı ayarlar veya kullanıcılar, oturumlar, olayları veya bir ortak olan işlemler analiz edin
services: application-insights
documentationcenter: ''
author: NumberByColors
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 04/10/2018
ms.reviewer: mbullwin
ms.pm_owner: daviste;NumberByColors
ms.author: daviste
ms.openlocfilehash: a575161be0a339973f9c59cc13c2320b38939d04
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60785123"
---
# <a name="application-insights-cohorts"></a>Application Insights kohortlar

Bir kohort, kullanıcılar, oturumlar, olayları veya bir ortak sahip işlemleri kümesidir. Azure Application Insights'ta bir analitik sorgu tarafından kohortlar tanımlanır. Belirli bir analiz etmek için sahip olduğu durumlarda kullanıcıları veya olayları tekrar tekrar kohortlar, tam olarak ilgilendiğiniz kümesini ifade etmek için daha fazla esneklik verebilirsiniz.

![Kohortlar bölmesi](./media/usage-cohorts/001.png)

## <a name="cohorts-versus-basic-filters"></a>Temel filtreler karşı kohortlar

Kohortlar filtrelere benzer şekillerde kullanılır. Ancak çok daha uyarlanabilir ve karmaşık bulunmaları kohortlar tanımları özel analiz sorgularından oluşturulmuştur. Filtreler farklı olarak, diğer takım üyeleriniz bunları yeniden kullanabilmesi kohortlar kaydedebilirsiniz.

Uygulamanızı yeni bir özellik çalışmış tüm kullanıcılar kohortu tanımlayabilir. Application Insights kaynağınızı bu kohort kaydedebilirsiniz. Belirli kullanıcıların kaydedilmiş bu Grup gelecekte çözümlemek kolaydır.

> [!NOTE]
> Bunlar oluşturduktan sonra kullanıcılar, oturumlar, olayları ve kullanıcı akışları araçlarından kohortlar kullanılabilir.

## <a name="example-engaged-users"></a>Örnek: Bağlı kullanıcılar

Ekibinizin bağlı bir kullanıcı uygulamanızı beş veya daha fazla kez belirli bir ay içinde kullanan herkes olarak tanımlar. Bu bölümde, bu katılımcı kullanıcılar kohortu tanımlayın.

1. Kohortlar Aracı'nı açın.

2. Seçin **Şablon Galerisi** sekmesi. Şablonları koleksiyonu için çeşitli kohortlar görürsünüz.

3. Seçin **katılımcı kullanıcılar--gün tarafından kullanılan**.

    Bu kohort için üç parametresi vardır:
    * **Etkinlikleri**burada "kullanımı" hangi olay ve sayfa görüntüleme sayısı seçin
    * **Dönem**, bir ay tanımı.
    * **UsedAtLeastCustom**, sayı sürelerinin kullanıcılar sorun bir süre içinde bağlı olarak saymak için kullanmanız gerekir.

4. Değişiklik **UsedAtLeastCustom** için **5 + gün**, bırakıp **süresi** varsayılan 28 gün.

    ![Bağlı kullanıcılar](./media/usage-cohorts/003.png)

    Bu kohort tüm özel olay veya sayfa görüntüleme 5 üzerinde gönderilen tüm kullanıcı kimlikleri temsil eder. Şimdi gün öncesi ayrı 28.

5. **Kaydet**’i seçin.

   > [!TIP]
   > Kohortunuzu gibi bir ad verin "Engaged kullanıcılar (5 + gün)." "Raporlarım için" kaydedin veya "raporlar bu kohort görmek için bu Application Insights kaynağına erişimi olan diğer kişiler isteyip istemediğinize bağlı olarak paylaşılan".

6. Seçin **Galerisine dön**.

### <a name="what-can-you-do-by-using-this-cohort"></a>Bu kohort kullanarak neler?

Kullanıcılar aracını açın. İçinde **Göster** açılan kutusunda, oluşturduğunuz altında kohort **ait kullanıcılar**.

Artık kullanıcılar aracı, bu kullanıcıların kohortu için filtrelenmiştir:

![Belirli bir kohort için filtrelenmiş kullanıcılar bölmesi](./media/usage-cohorts/004.png)

Fark gereken bazı önemli noktalar:

* Bu normal filtrelerden oluşturulamıyor. Daha gelişmiş tarih mantığı.
* Bu kohort kullanıcılar Aracı'nda normal filtreleri kullanarak daha fazla filtre uygulayabilirsiniz. Bu nedenle kohort 28 günlük dönemler halinde windows üzerinde tanımlanmış olsa da, 30, 60 ya da 90 gün olacak şekilde kullanıcılar aracı zaman aralığında ayarlayabilirsiniz.

Bu filtreler Sorgu Oluşturucu express mümkün olan daha karmaşık sorular destekler. Bir örnek _son 28 gün içinde bağlı kişiler. Aynı kişilerle son 60 gün içindeki nasıl davranacağını?_

## <a name="example-events-cohort"></a>Örnek: Olaylar kohortu

Olayların kohortlar de yapabilirsiniz. Bu bölümde, tanımladığınız bir kohort olayları ve sayfa. Ardından, bunları diğer araçları kullanma hakkında bilgi. Bu kohort takımınızın göz önünde bulundurur olayları kümesini tanımlayabilir _etkin kullanım_ veya bir belirli yeni özellik ile ilgili bir set.

1. Kohortlar Aracı'nı açın.

2. Seçin **Şablon Galerisi** sekmesi. Şablonları koleksiyonu için çeşitli kohortlar görürsünüz.

3. Seçin **olayları Seçici**.

    ![Olayları Seçici ekran görüntüsü](./media/usage-cohorts/006.png)

4. İçinde **etkinlikleri** açılan kutusunda, kohorttaki olmasını istediğiniz olayları seçin.

5. Kohort kaydedin ve bir ad verin.

## <a name="example-active-users-where-you-modify-a-query"></a>Örnek: Burada bir sorguyu değiştirdiğinizde etkin kullanıcılar

Önceki iki kohortlar açılır kutuları kullanarak tanımlanmadı. Ancak, toplam esneklik için analytics sorgularını kullanarak kohortlar tanımlayabilirsiniz. Görmek için kullanıcıların kohortu İngiltere ' oluşturma.

![Kohortlar aracın kullanımı walking animasyonlu resmi](./media/usage-cohorts/cohorts0001.gif)

1. Kohortlar Aracı'nı açın, **Şablon Galerisi** sekmesine tıklayın ve **boş kullanıcılar kohortu**.

    ![Boş kullanıcılar kohortu](./media/usage-cohorts/001.png)

    Üç bölüm bulunur:
   * Burada, daha ayrıntılı bir kohort başkaları için ekibiniz açıklayan bir Markdown metin bölümü.

   * Burada yaptığınız kendi parametreleri gibi bir parametreler bölümü **etkinlikleri** ve diğer açılır kutuları önceki iki örnekte yer.

   * Kohort bir analytics sorgusunu kullanarak tanımladığınız bir sorgu bölümü.

     Sorgu bölümünde, [bir analytics sorgusunu yazmak](/azure/kusto/query). Sorgu, belirli bir kümesini tanımlamak istediğiniz kohort açıklayan satırları seçer. Kohortlar aracı ekler ardından örtük olarak bir "| user_Id değerini tanımlayarak özetleme"yan tümcesi için sorgu. Sorgunuzu sonuçlar döndürüyor emin olabilmeniz bu verileri bir tablo, sorguda aşağıdaki önizlemesini görebilirsiniz.

     > [!NOTE]
     > Sorgu görmüyorsanız, uzun yapın ve sorgu ortaya çıkarmak için bölüm yeniden boyutlandırmayı deneyin. Bu bölümün başında animasyonlu .gif, yeniden boyutlandırma davranışını gösterir.

2. Kopyalayın ve sorgu Düzenleyicisi'ne aşağıdaki metni yapıştırın:

    ```KQL
    union customEvents, pageViews
    | where client_CountryOrRegion == "United Kingdom"
    ```

3. Seçin **sorgusu**. Kullanıcı kimliklerini tablosunda görünür görmüyorsanız, uygulamanız kullanıcıların sahip olduğu bir ülke için değiştirin.

4. Kaydet ve kohort adı.

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

_Ben bir ülkeden kullanıcılar kohortu tanımladınız. Ben bir filtre söz konusu ülkeye göre ayarlamak için kullanıcıların aracında bu kohort karşılaştırdığınızda, farklı sonuçlar konusuna bakın. Neden?_

Kohortlar ve filtreleri farklıdır. Birleşik Krallık (önceki örnekteki gibi tanımlanır) kullanıcıların kohortu sahip ve filtre ayarı için sonuçları karşılaştırma varsayalım "ülke veya bölge Birleşik Krallık =."

* Kohort sürümü, Birleşik Krallık geçerli zaman aralığı içinde bir veya daha fazla olay gönderen kullanıcıların tüm olayları gösterir. Ülke veya bölgeye göre bölerseniz, birçok ülke ve bölgelerden büyük olasılıkla bakın.
* Filtreler sürümü yalnızca Birleşik Krallık olayları gösterir. Ancak, ülkeye veya bölgeye göre bölme, Birleşik Krallık bakın.

## <a name="learn-more"></a>Daha fazla bilgi edinin

* [Analytics sorgu dili](https://go.microsoft.com/fwlink/?linkid=856587)
* [Kullanıcılar, oturumlar, etkinlikler](usage-segmentation.md)
* [Kullanıcı akışları](usage-flows.md)
* [Kullanıma genel bakış](usage-overview.md)