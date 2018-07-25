---
title: Desen şablonları LUIS uygulamaları ekleme | Microsoft Docs
titleSuffix: Azure
description: Language Understanding (LUIS) uygulamalarında tahmin doğruluğunu artırmak için desen şablonları eklemeyi öğrenin.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.technology: luis
ms.topic: article
ms.date: 06/08/2018
ms.author: diberry;
ms.openlocfilehash: bf1931355fd873eaeac6c313b70717dfa99814c6
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2018
ms.locfileid: "39222605"
---
# <a name="how-to-add-patterns-to-improve-prediction-accuracy"></a>Nasıl tahmin doğruluğunu artırmak için düzenleri ekleyin
Bir LUIS uygulaması konuşma uç noktası aldıktan sonra kullanmak [kavramı](luis-concept-patterns.md) sözcük sırasını ve sözcük seçim içindeki bir desenle açığa konuşma için tahmin doğruluğunu artırmak için desenleri. Kullanım desenlerini [varlıkları](luis-concept-entity-types.md) ve belirli bir desene söz dizimini kullanarak verileri ayıklamak için kullanıcı rolleri. 

## <a name="add-template-utterance-to-create-pattern"></a>Desen oluşturmak için şablon utterance Ekle
1. Adını seçerek uygulamanızı açın **uygulamalarım** sayfasında ve ardından **desenleri** sol bölmede altında **uygulama performansını**.

    ![Desen listesinin ekran görüntüsü](./media/luis-how-to-model-intent-pattern/patterns-1.png)

2. Desen için doğru hedefini seçin. 

    ![Hedefi seçin](./media/luis-how-to-model-intent-pattern/patterns-2.png)

3. Şablon metin şablonu utterance yazın ve Enter'ı seçin. Varlık adı girmek istediğiniz zaman doğru deseni varlık sözdizimini kullanın. Varlık sözdizimi ile başlayan `{`. Varlıklar görüntüler listesi. Doğru varlığı seçin ve ardından Enter'ı seçin. 

    ![Varlık deseni için ekran görüntüsü](./media/luis-how-to-model-intent-pattern/patterns-3.png)

    Varlığınız bir rol varsa, tek bir iki nokta rolüyle belirtmek `:`, varlık adı sonra gibi `{Location:Origin}`. Rolleri varlıkların listesini bir liste görüntüler. Rolü seçin ve ardından Enter'ı seçin. 

    ![Rolü içeren varlığın ekran görüntüsü](./media/luis-how-to-model-intent-pattern/patterns-4.png)

    Doğru varlık seçtikten sonra deseni girdikten ve ardından Enter'ı seçin. Girme desenleri, işiniz bittiğinde [eğitme](luis-how-to-train.md) uygulamanızı.

    ![Girilen deseninin her iki türdeki varlık ile ekran görüntüsü](./media/luis-how-to-model-intent-pattern/patterns-5.png)

## <a name="search-patterns"></a>Arama desenleri
Arama, bazı belirli bir metni içeren kalıpları bulmasına olanak tanır.  

1. Büyüteç simgesini seçin.

    ![Arama aracı simgesinin vurgulandığı ekran görüntüsü, desenleri sayfası](./media/luis-how-to-model-intent-pattern/search-icon.png)

    Arama metni desenleri listenin en sağ üst köşedeki arama kutusuna yazın ve Enter tuşuna basın. Desenler liste yalnızca arama metninizi dahil olmak üzere desenlerini görüntülemek için güncelleştirilir.

    ![Vurgulanan arama kutusundaki arama metnini, ekran düzenleri sayfası](./media/luis-how-to-model-intent-pattern/search-text.png)

    Aramayı iptal et ve desenleri tam listesini geri yükleme için yazdığınız arama metni silin.

<!-- TBD: should I be able to click on the magnifying glass again to close the search box? It doesn't reset the list. -->

## <a name="edit-a-pattern"></a>Bir desen Düzenle
1. Bir desen düzenlemek için üç noktayı seçin (***...*** ) bu deseni için satırın sağ ucunda düğmesine ve ardından **Düzenle**. 

    ![Menü öğesi deseni satır Düzenle ekran görüntüsü](./media/luis-how-to-model-intent-pattern/patterns-three-dots.png) 

2. Herhangi bir değişiklik metin kutusuna girin. İşiniz bittiğinde, select girin. Düzenleme düzenlerini, işleminiz tamamlandığında [eğitme](luis-how-to-train.md) uygulamanızı.

    ![Desen düzenleme işleminin ekran görüntüsü](./media/luis-how-to-model-intent-pattern/edit-pattern.png)

## <a name="reassign-individual-pattern-to-different-intent"></a>Farklı bir amaç için ayrı ayrı desen yeniden atama

Farklı bir hedefi tek bir desen yeniden atamak için desen metnin sağına hedefi liste kutusunu seçin ve farklı bir hedefi seçin.

![Farklı bir amaç için ayrı ayrı desen yeniden atama ekran görüntüsü](./media/luis-how-to-model-intent-pattern/reassign-individual-pattern.png)

## <a name="reassign-several-patterns-to-different-intent"></a>Farklı bir hedefi için çeşitli desenlerden yeniden atama

Farklı bir hedefi için çeşitli desenlerden yeniden atamak için her desen solundaki onay kutusunu işaretleyin veya üst onay kutusunu işaretleyin. **Yeniden atama hedefi** seçeneği, araç çubuğundaki görüntüler. Desenler için doğru hedefini seçin. 

![Farklı bir hedefi için çeşitli desenlerden yeniden atama ekran görüntüsü](./media/luis-how-to-model-intent-pattern/reassign-many-patterns.png)

## <a name="delete-a-single-pattern"></a>Tek bir düzeni Sil

1. Bir desen silmek için üç noktayı seçin (***...*** ) bu deseni için satırın sağ ucunda düğmesine ve ardından **Sil**. 

    ![Utterance Sil ekran görüntüsü](./media/luis-how-to-model-intent-pattern/patterns-three-dots-ddl.png)

2. Seçin **Tamam** silme işlemini onaylamak için.

    ![Ekran görüntüsü, silme onayı](./media/luis-how-to-model-intent-pattern/confirm-delete.png)

## <a name="delete-several-patterns"></a>Çeşitli desenlerden Sil

1. Çeşitli desenlerden silmek için her desen solundaki onay kutusunu işaretleyin veya üst onay kutusunu işaretleyin. **Sil (s) desenlerini** seçeneği, araç çubuğundaki görüntüler. Seçin **Sil (s) desenlerini**.  

    ![Çeşitli desenlerden silme işleminin ekran görüntüsü](./media/luis-how-to-model-intent-pattern/delete-many-patterns.png)

2. **Sil desenleri** onay iletişim kutusu görüntülenir. Seçin **Tamam** silme işlemini tamamlamak için.

    ![Çeşitli desenlerden silme işleminin ekran görüntüsü](./media/luis-how-to-model-intent-pattern/delete-many-patterns-confirmation.png)

## <a name="filter-pattern-list-by-entity"></a>Varlık tarafından filtre deseni listesi

Desenlerinin listesi tarafından belirli bir varlığa filtre uygulamak için seçim **varlık filtreleri** desenleri üstündeki araç çubuğundan. 

![Varlık tarafından desenleri filtreleme ekran görüntüsü](./media/luis-how-to-model-intent-pattern/filter-entities-1.png)

Filtre uygulandıktan sonra varlık adı araç çubuğunun altında görünür. 

## <a name="filter-pattern-list-by-intent"></a>Filtre deseni listeyi hedefi

Belirli bir hedefi tarafından desenleri listesini filtrelemek için seçin **amaç filtreleri** desenleri üstündeki araç çubuğundan. 

![Hedefi tarafından desenleri filtreleme ekran görüntüsü](./media/luis-how-to-model-intent-pattern/filter-intents-1.png)

Filtre uygulandıktan sonra hedefi adı araç çubuğunun altında görünür. 

## <a name="remove-entity-or-intent-filter"></a>Varlık veya hedefi Filtreyi Kaldır
Desen listesinin filtre uygulandığında varlık veya hedefi adı araç çubuğu altında görünür. Filtreyi kaldırmak için bir ad seçin.

![Varlık tarafından filtrelenen desenlerinin ekran görüntüsü](./media/luis-how-to-model-intent-pattern/filter-entities-2.png)

Filtre kaldırılır ve tüm desenleri görüntüler. 

## <a name="add-pattern-from-existing-utterance-on-intent-or-entity-page"></a>Desen hedefi veya varlık sayfasında mevcut utterance ekleyin
Her iki mevcut bir utterance desen oluşturabilirsiniz **hedefi** veya **varlık** sayfası. Herhangi bir amaç veya varlık sayfadaki tüm konuşma utterance düzeyi seçenekleri erişimi gibi sağlama sağ sütunla bir listesinde görüntülenen **Düzenle**, **Sil**, ve **deseniolarakEkle**.

1. Utterance seçilen satırdaki üç noktayı seçin (***...*** ) düğmesini utterance sağında ve **deseni olarak Ekle**.

    [![](./media/luis-how-to-model-intent-pattern/add-pattern-from-utterance.png "Konuşma Tablo Ekle düzendeki Seçenekler menüsünde vurgulanmış ekran görüntüsü")](./media/luis-how-to-model-intent-pattern/add-pattern-from-utterance.png)

2. Şunlara göre desenini değiştirir [sözdizimi kurallarına](luis-concept-patterns.md#pattern-syntax). Seçtiğiniz utterance varlıklarla sahipse, bu zaten doğru sözdizimi desende varlıklardır.

    ![Varlık tarafından filtrelenen desenlerinin ekran görüntüsü](./media/luis-how-to-model-intent-pattern/confirm-patterns-modal.png)

## <a name="train-your-app-after-changing-model-with-patterns"></a>Model desenleri ile değiştirdikten sonra uygulamanızı eğitin
Sonra ekleme, düzenleme, kaldırmak veya bir desen yeniden atama [eğitme](luis-how-to-train.md) ve [yayımlama](luis-how-to-publish-app.md) uygulamanız için uç nokta sorguları etkilemek yaptığınız değişiklikleri. 

## <a name="next-steps"></a>Sonraki adımlar

* Bilgi nasıl [desen yapı](luis-tutorial-pattern.md) bir pattern.any ve roller ile.
* Bilgi nasıl [eğitme](luis-how-to-train.md) uygulamanızı.