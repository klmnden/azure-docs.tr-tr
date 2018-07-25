---
title: Hedef LUIS uygulamaları ekleme | Microsoft Docs
description: Intents kullanıcı isteklerini anlamak ve düzgün bir şekilde tepki uygulamaların eklemek için Language Understanding (LUIS) kullanın.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.component: language-understanding
ms.topic: article
ms.date: 05/07/2018
ms.author: diberry
ms.service: cognitive-services
ms.openlocfilehash: 0ebf15ea49467674ab3c56aa7983131593cf5c9a
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2018
ms.locfileid: "39225895"
---
# <a name="manage-intents"></a>Intents yönetme 
Ekleme [hedefleri](luis-concept-intent.md) LUIS uygulamanızı sorularınız ya da aynı amaçları olan komutları gruplarını tanımlamak için. 

Ekleme ve yönetme, amaçlardan tutun **hedefleri** sayfasında, kullanılabilir **hedefleri** LUIS'ın sol panelde. 

Aşağıdaki yordam TravelAgent uygulamada "Bookflight" hedefi ekleme göstermektedir.

## <a name="add-intent"></a>Hedefi ekleyin

1. Uygulamanız (örneğin, TravelAgent) adını tıklayarak açın **uygulamalarım** sayfasında ve ardından **hedefleri** sol bölmesinde. 
2. Üzerinde **hedefleri** sayfasında **yeni hedefi oluşturma**.

    ![Intents listesi](./media/luis-how-to-add-intents/IntentsList.png)
3. İçinde **yeni hedefi oluşturma** iletişim kutusu, amacı "BookFlight" adını verin ve tıklayın türü **Bitti**.

    ![Hedefi ekleyin](./media/luis-how-to-add-intents/Addintent-dialogbox.png)

    Yeni eklenen hedefinin hedefi ayrıntıları sayfasındaki [Konuşma ekleme](#add-an-utterance-on-intent-page).

## <a name="rename-intent"></a>Amacı yeniden adlandır

1. Üzerinde **hedefi** sayfasında, yeniden adlandırma simgesi ![amacı Yeniden Adlandır](./media/luis-how-to-add-intents/Rename-Intent-btn.png) hedefi adının yanında. 

2. Üzerinde **hedefi** sayfasında, geçerli hedefi adı iletişim kutusunda gösterilir. Hedefi adı düzenleyin ve enter tuşuna basın. Yeni bir ad kaydedildi ve hedefi sayfasında görüntülenir.

    ![Hedefi Düzenle](./media/luis-how-to-add-intents/EditIntent-dialogbox.png)

## <a name="delete-intent"></a>Hedefi silme
Hiçbiri hedefi dışında bir amaç silerken konuşma hiçbiri hedefi eklemek seçebilirsiniz. Konuşma silerek yerine taşımanız gerekirse, bu yararlıdır.   

1. Üzerinde **hedefi** sayfasında **silme hedefi** hedefi adının yanındaki düğmeyi. 

    ![Hedefi düğmesi silme](./media/luis-how-to-add-intents/DeleteIntent.png)

2. Onay iletişim kutusunda "Tamam" düğmesine tıklayın.

<!--
    TBD: waiting for confirmation about which delete dialog is going to be in //BUILD

    ![Delete Intent Dialog](./media/luis-how-to-add-intents/DeleteIntent-Confirmation.png)
-->


## <a name="add-an-utterance-on-intent-page"></a>Hedefi sayfasında bir utterance ekleyin

Beklediğiniz kullanıcılarınızdan gelen gibi ilgili bir utterance hedefi sayfasında girin `book 2 adult business tickets to Paris tomorrow on Air France` metin kutusuna aşağıdaki hedefi adını ve Enter tuşuna basın. 
 
>[!NOTE]
>LUIS tüm konuşma küçük harfe dönüştürür.

![Vurgulanan utterance ile ekran görüntüsü, hedefleri Ayrıntıları sayfası](./media/luis-how-to-add-intents/add-new-utterance-to-intent.png) 

Konuşma geçerli amaç için konuşma listesine eklenir. Bir utterance eklendikten sonra [herhangi bir varlık etiketi](luis-how-to-add-example-utterances.md) konuşma içinde ve [eğitme](luis-how-to-train.md) uygulamanızı. 

## <a name="create-a-pattern-from-an-utterance"></a>Bir utterance bir düzen oluşturma
Bkz: [hedefi veya varlık sayfasında mevcut utterance Ekle deseni](luis-how-to-model-intent-pattern.md#add-pattern-from-existing-utterance-on-intent-or-entity-page).

## <a name="edit-an-utterance-on-intent-page"></a>Hedefi sayfasında bir utterance Düzenle

Bir utterance düzenlemek için üç noktayı seçin (***...*** ) bu utterance satırını sağ ucunda düğmesini ve ardından **Düzenle**. Metin değiştirme sonra klavyedeki Enter tuşuna basın.

![Üç nokta düğmesi vurgulanmış ekran görüntüsü, hedefleri Ayrıntıları sayfası](./media/luis-how-to-add-intents/edit-utterance.png) 

## <a name="reassign-utterances-on-intent-page"></a>Konuşma niyetini sayfasında yeniden atama
Bir veya daha fazla konuşma amacı, başka bir amaç için atama yoluyla değiştirebilirsiniz. 

Tek bir utterance utterance ait satır sonuna doğru farklı bir amaç için yeniden atamak için doğru hedefi adı altında seçin **hedefi etiketli** sütun. Utterance geçerli amaç 's utterance listesinden kaldırılır. 

![Seçili Labeled hedefi sütununun altında bir utterance'nın amacı, ekran BookFlight hedefi sayfası](./media/luis-how-to-add-intents/reassign-1-utterance.png)

Çeşitli konuşma amacı değiştirmek için konuşma solundaki onay kutularını seçin ve ardından **yeniden atama hedefi**. Doğru amaç listeden seçin.

![İşaretli bir utterance ve yeniden atama hedefi düğmesi vurgulanmış ekran görüntüsü, BookFlight hedefi sayfası](./media/luis-how-to-add-intents/delete-several-utterances.png) 

## <a name="delete-utterances-on-intent-page"></a>Konuşma niyetini sayfasında Sil

Bir utterance silmek için üç noktayı seçin (***...*** ) bu utterance satırını sağ ucunda düğmesini ve ardından **Sil**. Utterance listesi ve LUIS uygulaması kaldırılır.

![Sil seçeneğinin vurgulandığı ile ekran görüntüsü, hedefleri Ayrıntıları sayfası](./media/luis-how-to-add-intents/delete-utterance-ddl.png)

Çeşitli konuşma silmek için:

1. Konuşma solundaki onay kutularını seçin ve ardından **Sil (s) konuşma**. 

    ![Ekran görüntüsü, hedefleri Ayrıntıları sayfası, konuşma, iade ve utterance(s) düğmesi vurgulanmış Sil](./media/luis-how-to-add-intents/delete-several-utterances.png)

2. Seçin **Bitti** içinde **konuşma Sil?** açılır iletişim kutusu.

## <a name="search-in-utterances-on-intent-page"></a>Konuşma niyetini sayfasında içinde arama
Bir amaca, metin (sözcük ve tümcecikleri) içeren konuşma için arama yapabilirsiniz. Örneğin, belirli bir sözcüğün içeren bir hata görebilirsiniz ve bu belirli bir sözcüğün dahil tüm örnekleri bulmak istediğiniz. 

1. Araç çubuğunda Büyüteç simgesini seçin.

    ![Ekran görüntüsü, hedefleri sayfası Büyüteç vurgulanan arama simgesini](./media/luis-how-to-add-intents/magnifying-glass.png)

2. Arama metin kutusu görünür. Konuşma listenin en sağ üst köşedeki arama kutusuna bir sözcük veya tümcecik yazın. Konuşma arama metninizi içeren konuşma görüntülemek için güncelleştirmeleri listeleyin. 

    ![Vurgulanan arama metin kutusuna ekran görüntüsü, hedefleri sayfası](./media/luis-how-to-add-intents/search-textbox.png)

    Aramayı iptal et ve konuşma tam listesini geri yükleme için yazdığınız arama metni silin. Arama metin kutusuna kapatmak için araç çubuğunda Büyüteç simgesini yeniden seçin.

## <a name="prediction-discrepancy-errors-on-intent-page"></a>Hedefi sayfasında tahmin tutarsızlık hataları
Bir hedefi olarak bir utterance seçili amaç ve tahmin puanı arasında bir tutarsızlık olabilir. LUIS çevresinde puanı ile bu farklılık gösterir. 

![Vurgulanan tahmin tutarsızlık puanı BookFlight hedefi ekran sayfası](./media/luis-how-to-add-intents/score-discrepancy.png) 

## <a name="filter-by-intent-prediction-discrepancy-errors-on-intent-page"></a>Hedefi tahmin tutarsızlık hataları hedefi sayfasında göre filtreleme
Yalnızca bir hedefi tahmin tutarsızlık ile Konuşma utterance listesini filtrelemek için değiştirin **Tümünü Göster** için **yalnızca hataları** araç. 

## <a name="filter-by-entity-type-on-intent-page"></a>Hedefi sayfasında varlık türüne göre filtrele
Kullanım **varlık filtreleri** konuşma varlığa filtre uygulamak için araç çubuğundaki açılır. 

![Ekran görüntüsü, hedefleri sayfasıyla vurgulanmış varlık türü filtresi](./media/luis-how-to-add-intents/filter-by-entities.png) 

Filtreyi kaldırmak için bu sözcük veya tümceciği araç çubuğunun altında mavi bir filtre kutusu seçin.  
<!-- TBD: waiting for ux fix - bug in ux of prebuit entity number -- when filtering by it, it doesn't show the list -->

## <a name="switch-to-token-view-on-intent-page"></a>Hedefi sayfasında belirteci görünümüne geçin
İki durumlu **belirteçleri görünümü** varlık türü adları yerine belirteçler görüntülemek için. De kullanabilirsiniz klavyede **denetim + E** görünüme geçiş için. 

![Belirteç görünümle vurgulanmış ekran görüntüsü, BookFlight hedefi](./media/luis-how-to-add-intents/toggle-tokens-view.png)

## <a name="train-your-app-after-changing-model-with-intents"></a>Intents modeliyle değiştirdikten sonra uygulamanızı eğitin
Ekleme, düzenleme veya kaldırma amacı, sonra [eğitme](luis-how-to-train.md) ve [yayımlama](luis-how-to-publish-app.md) uygulamanız için uç nokta sorguları etkilemek yaptığınız değişiklikleri. 

## <a name="next-steps"></a>Sonraki adımlar

Intents uygulamanıza ekledikten sonra sonraki göreviniz eklemeye başlamak için olan [örnek konuşma](luis-how-to-add-example-utterances.md) eklediğiniz hedefleri için. 
