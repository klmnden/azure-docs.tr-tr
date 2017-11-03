---
title: "Koşulları ekleme ve iş akışları - Azure Logic Apps başlatın | Microsoft Docs"
description: "İş akışları Azure Logic Apps içinde koşullu mantık, Tetikleyiciler, Eylemler ve parametreleri ekleyerek çalışma şeklini denetler."
author: stepsic-microsoft-com
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: e4e24de4-049a-4b3a-a14c-3bf3163287a8
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/28/2017
ms.author: LADocs; stepsic
ms.openlocfilehash: e632c48ed31e82536db55a9c54438bece0c38fd4
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="use-logic-apps-features"></a>Logic Apps özelliklerini kullanma

İçinde bir [önceki konu](../logic-apps/logic-apps-create-a-logic-app.md), ilk mantıksal uygulamanızı oluşturuldu. Mantığı uygulamanızın iş akışını denetlemek için mantıksal uygulamanızı çalıştırın ve Diziler, koleksiyonlar ve toplu verileri işlemek nasıl için farklı yollar belirtebilirsiniz. Bu öğeleri mantığı uygulama akışınızda içerebilir:

* Koşullar ve [switch ifadeleri](../logic-apps/logic-apps-switch-case.md) belirli koşulların karşılanıp karşılanmadığını bağlı olarak farklı eylemler çalıştırmak mantıksal uygulamanızı sağlar.

* [Döngüler](../logic-apps/logic-apps-loops-and-scopes.md) adımları tekrar tekrar çalıştırmak mantıksal uygulamanızı sağlar. Kullandığınızda Örneğin, Eylemler bir dizi yineleyebilirsiniz bir **For_each** döngü. Ya da kullanırken bir koşul yerine getirilene kadar Eylemler yineleyebilirsiniz bir **kadar** döngü.

* [Kapsamları](../logic-apps/logic-apps-loops-and-scopes.md) let gruplandırma Eylemler dizisi birlikte, örneğin, özel durum işleme uygulamak için.

* [Debatching](../logic-apps/logic-apps-loops-and-scopes.md) kullandığınızda bir dizi öğeleri için ayrı iş akışlarını başlatmak mantıksal uygulamanızı sağlar **SplitOn** komutu.

Bu konu, mantıksal uygulamanızı oluşturmaya yönelik diğer kavramları sunar:

* Varolan bir mantıksal uygulama düzenlemek için kod görünümü
* Bir iş akışı başlatma seçenekleri

## <a name="conditions-run-steps-only-after-meeting-a-condition"></a>Koşullar: yalnızca bir koşul yerine getirdikten sonra adımları çalıştırın

Mantıksal uygulamanızı yalnızca veri belirli ölçütleri karşıladığında adımlarını çalıştırmak için iş akışı içinde belirli alanlar veya değerler karşı verileri karşılaştıran bir koşul ekleyebilirsiniz.

Örneğin, bir Web sitesinin RSS akışı gönderileri için çok fazla e-posta gönderen bir mantıksal uygulama olduğunu varsayalım. Bir koşul ekleyebilirsiniz, böylece yalnızca belirli bir kategoriye yeni posta ait olduğunda mantıksal uygulamanızı e-posta gönderir.

1. İçinde [Azure portal](https://portal.azure.com), bulma ve mantığı Uygulama Tasarımcısı'nda mantıksal uygulamanızı açın.

2. Bir koşul istediğiniz iş akışı konumu ekleyin. 

   Mantıksal uygulama iş akışı içinde varolan adımlar arasındaki koşul eklemek için üzerinde oku koşulu eklemek istediğiniz işaretçiyi. 
   Seçin **artı** (**+**), ardından **bir koşul eklemek**. Örneğin:

   ![Mantıksal uygulama koşul Ekle](./media/logic-apps-use-logic-app-features/add-condition.png)

   > [!NOTE]
   > Geçerli iş akışınızı sonunda bir koşul eklemek istiyorsanız, mantıksal uygulamanızı alt kısmına gidin ve seçin **+ yeni adım**.

3. Şimdi koşul tanımlayın. Değerlendirme, işlemi gerçekleştirmek için ve hedef değer veya alan istediğiniz kaynak alanı belirtin. Var olan alanları, koşulu eklemek için seçin **Ekle dinamik içerik listesi**.

   Örneğin:

   ![Koşul temel modunda düzenleme](./media/logic-apps-use-logic-app-features/edit-condition-basic-mode.png)

   Tam koşul şöyledir:

   ![Tam koşul](./media/logic-apps-use-logic-app-features/edit-condition-basic-mode-2.png)

   > [!TIP]
   > Kodda koşulu tanımlamak için tercih **Gelişmiş modda Düzenle**. Örneğin:
   > 
   > ![Kod koşulunda Düzenle](./media/logic-apps-use-logic-app-features/edit-condition-advanced-mode.png)

4. Altında **Evet IF** ve **IF Hayır**, gerçekleştirme adımları temel koşul karşılanır karşılanmaz ekleyebilirsiniz.

   Örneğin:

   ![Evet ve hiçbir yol koşulu](./media/logic-apps-use-logic-app-features/condition-yes-no-path.png)

   > [!TIP]
   > Varolan eylemlere sürükleyebilirsiniz **Evet IF** ve **IF Hayır** yollar.

5. İşiniz bittiğinde, mantıksal uygulamanızı kaydedin.

Şimdi yalnızca gönderileri koşulunuz karşıladığında e-postaları alın.

## <a name="repeat-actions-over-a-list-with-foreach"></a>Eylemler forEach ile bir liste üzerinden yineleme

ForEach döngüsü üzerinden bir eylem yinelenecek dizisini belirtir. Bir dizi değil akış başarısız olur. Örneğin, bir dizi iletileri çıkarır action1 varsa ve her ileti göndermek istediğiniz eyleminizi özelliklerinde bu forEach deyimi içerebilir:`forEach : "@action('action1').outputs.messages"`

## <a name="edit-the-code-definition-for-a-logic-app"></a>Bir mantıksal uygulama kodu tanımını düzenleme

Mantıksal Uygulama Tasarımcısı'nı sahip olsa da, bir mantıksal uygulama tanımlayan kodu doğrudan düzenleyebilirsiniz.

1. Komut çubuğunda seçin **kod görünümü**.

    Bir tam Düzenleyicisi'ni açar ve düzenlediğiniz tanımını gösterir.

    ![Kod Görünümü](media/logic-apps-use-logic-app-features/codeview.png)

    Metin Düzenleyicisi'nde kopyalayın ve aynı mantıksal uygulama içinde veya arasında mantıksal uygulamalar eylemleri herhangi bir sayıda yapıştırın. 
    Ayrıca kolayca eklemenize veya tüm bölümleri tanımından kaldırın ve ayrıca tanımları başkalarıyla paylaşabilirsiniz.

2. Yaptığınız düzenlemeleri kaydetmek üzere seçim yapın **kaydetmek**.

## <a name="parameters"></a>Parametreler

Bazı Logic Apps özellikleri yalnızca kod görünümünde, örneğin, parametreleri sağlanır. Parametre değerleri mantıksal uygulamanızı boyunca yeniden kolay hale getirir. Örneğin, çeşitli eylemler kullanmak istediğiniz bir e-posta adresi varsa, bu e-posta adresi parametre olarak tanımlamanız gerekir.

Çok değişmesi olasılığı olan değerler çekmek için iyi parametreleridir. Bunlar, özellikle farklı ortamlarda parametreleri geçersiz kılmak gerektiğinde kullanışlıdır. Ortamına bağlı parametreleri geçersiz kılmak öğrenmek için bkz: [Yazar mantıksal uygulama tanımları](../logic-apps/logic-apps-author-definitions.md) ve [REST API belgeleri](https://docs.microsoft.com/rest/api/logic).

Bu örnek için sorgu Terime parametreleri kullanabilmesi için var olan mantıksal uygulamanızı güncelleştirme gösterilmiştir.

1. Kod görünümünde Bul `parameters : {}` nesne ve ekleme bir `currentFeedUrl` nesnesi:

        "currentFeedUrl" : {
            "type" : "string",
            "defaultValue" : "http://rss.cnn.com/rss/cnn_topstories.rss"
        }

2. Git `When_a_feed-item_is_published` eylemi bulma `queries` bölümünde ve sorgu değerle değiştirin:`"feedUrl": "#@{parameters('currentFeedUrl')}"` 

    İki veya daha fazla katılmak için de kullanabilirsiniz `concat` işlevi. 
    Örneğin, `"@concat('#',parameters('currentFeedUrl'))"` yukarıdaki gibi aynı şekilde çalışır.

3.  İşiniz bittiğinde seçin **kaydetmek**. 

    Web sitesinin RSS yoluyla farklı bir URL geçirerek akışı değiştirebileceğiniz artık `currentFeedURL` nesnesi.

Daha fazla bilgi edinmek [mantıksal uygulama tanımları yazma nasıl](../logic-apps/logic-apps-author-definitions.md).

## <a name="start-logic-app-workflows"></a>Mantıksal uygulama iş akışlarını başlatmak

Mantıksal uygulamanızı tanımlanan iş akışı başlatma için farklı seçeneğiniz vardır. Bir iş akışı isteğe bağlı her zaman başlayabilmeniz için [Azure portal].

### <a name="recurrence-triggers"></a>Yineleme Tetikleyicileri

Bir yineleme tetikleyici, belirttiğiniz bir zaman aralığında çalışır. Tetikleyici koşullu mantık olduğunda, tetikleyici iş akışını çalıştırmak gerekip gerekmediğini belirler. İş akışı döndürerek çalışmalı tetikleyici gösteren bir `200` durum kodu. İş akışını çalıştırmak gerekmez, tetikleyici döndürür bir `202` durum kodu.

### <a name="callback-using-rest-apis"></a>REST API'lerini kullanarak geri çağırma

Bir iş akışını başlatmak için Hizmetler bir mantıksal uygulama uç noktası çağırabilirsiniz. Bu tür bir mantıksal uygulama isteğe bağlı başlatmayı seçin **Şimdi Çalıştır** komut çubuğunda. Bkz: [Tetikleyiciler olarak uygulama uç noktaları mantığı çağırarak iş akışlarını başlatmak](../logic-apps/logic-apps-http-endpoint.md). 

<!-- Shared links -->
[Azure portal]: https://portal.azure.com

## <a name="next-steps"></a>Sonraki adımlar

* [Switch deyimleri](../logic-apps/logic-apps-switch-case.md) 
* [Döngüler, kapsamlar ve ayırma](../logic-apps/logic-apps-loops-and-scopes.md)
* [Mantıksal uygulama tanımları yazma](../logic-apps/logic-apps-author-definitions.md)