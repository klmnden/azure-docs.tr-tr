---
title: Hedefleri HALUK uygulamaları ekleme | Microsoft Docs
description: Kullanıcı isteklerini anlamak ve düzgün şekilde tepki uygulamalar yardımcı olmak için hedefleri eklemek için dil anlama (HALUK) kullanın.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.component: language-understanding
ms.topic: article
ms.date: 05/07/2018
ms.author: v-geberr
ms.service: cognitive-services
ms.openlocfilehash: 6e013e994a3bcb60c3104aa10cd7bad1535706f1
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "35355763"
---
# <a name="manage-intents"></a>Hedefleri yönetme 
Ekleme [hedefleri](luis-concept-intent.md) HALUK uygulamanıza soru veya aynı amaçları sahip komutları grupları tanımlayın. 

Ekleme ve gelen, hedefleri yönetme **hedefleri** sayfasında, kullanılabilir **hedefleri** HALUK'ın sol panelinde. 

Aşağıdaki yordam TravelAgent uygulamada "Bookflight" hedefi ekleneceği gösterilmektedir.

## <a name="add-intent"></a>Hedefi ekleyin

1. Uygulamanız (örneğin, TravelAgent) adını tıklayarak açın **My uygulamaları** sayfasında ve ardından **hedefleri** sol panelinde. 
2. Üzerinde **hedefleri** sayfasında, **yeni amacı oluşturma**.

    ![Hedefleri listesi](./media/luis-how-to-add-intents/IntentsList.png)
3. İçinde **yeni amacı oluşturma** iletişim kutusu, amacı "BookFlight" olarak adlandırın ve tıklatın türü **Bitti**.

    ![Hedefi ekleyin](./media/luis-how-to-add-intents/Addintent-dialogbox.png)

    Yeni eklenen hedefinin hedefi Ayrıntıları sayfasında [utterances eklemek](#add-an-utterance-on-intent-page).

## <a name="rename-intent"></a>Hedefi yeniden adlandırma

1. Üzerinde **hedefi** sayfasında, yeniden adlandırma simgesini ![yeniden adlandırmak hedefi](./media/luis-how-to-add-intents/Rename-Intent-btn.png) hedefi adının yanındaki. 

2. Üzerinde **hedefi** sayfasında, geçerli hedefi adı bir iletişim kutusu gösterilir. Hedefi adını düzenleyin ve ENTER tuşuna basın. Yeni bir ad kaydedilmiş ve hedefi sayfasında görüntülenir.

    ![Hedefi Düzenle](./media/luis-how-to-add-intents/EditIntent-dialogbox.png)

## <a name="delete-intent"></a>Hedefi silme
Hiçbiri hedefi dışında bir amaç silerken, tüm utterances Yok'a hedefi eklemeyi seçebilirsiniz. Silerek yerine utterances taşımanız gerekirse, bu yararlıdır.   

1. Üzerinde **hedefi** sayfasında, **silme hedefi** hedefi adının yanındaki düğmesi. 

    ![Hedefi düğmesi silme](./media/luis-how-to-add-intents/DeleteIntent.png)

2. Onay iletişim kutusunda "Tamam" düğmesini tıklatın.

<!--
    TBD: waiting for confirmation about which delete dialog is going to be in //BUILD

    ![Delete Intent Dialog](./media/luis-how-to-add-intents/DeleteIntent-Confirmation.png)
-->


## <a name="add-an-utterance-on-intent-page"></a>Hedefi sayfasında bir utterance ekleyin

Hedefi sayfasında beklediğiniz, kullanıcılardan gibi ilgili utterance girin `book 2 adult business tickets to Paris tomorrow on Air France` hedefi adı ve ENTER tuşuna basın altındaki metin kutusuna. 
 
>[!NOTE]
>HALUK tüm utterances küçük harflere dönüştürür.

![Vurgulanan utterance ile ekran görüntüsü, hedefleri Ayrıntılar sayfası](./media/luis-how-to-add-intents/add-new-utterance-to-intent.png) 

Utterances geçerli amacı için utterances listesine eklenir. Bir utterance eklendikten sonra [herhangi bir varlık etiketi](luis-how-to-add-example-utterances.md) utterances içinde ve [eğitmek](luis-how-to-train.md) uygulamanızı. 

## <a name="create-a-pattern-from-an-utterance"></a>Bir utterance bir düzeni oluşturma
Bkz: [Ekle düzeni hedefi veya varlık sayfasında mevcut utterance gelen](luis-how-to-model-intent-pattern.md#add-pattern-from-existing-utterance-on-intent-or-entity-page).

## <a name="edit-an-utterance-on-intent-page"></a>Bir utterance hedefi sayfasında Düzenle

Bir utterance düzenlemek için bu utterance çizgi sağ ucunda üç noktaya (...) simgesini seçin ve ardından **Düzenle**. Metin değiştirme sonra klavyedeki Enter tuşuna basın.

![Vurgulanan üç noktaya simgesiyle ekran görüntüsü, hedefleri Ayrıntılar sayfası](./media/luis-how-to-add-intents/edit-utterance.png) 

## <a name="reassign-utterances-on-intent-page"></a>Hedefi sayfasında utterances yeniden atama
Bir veya daha fazla utterances amacı, başka bir amaç için atayarak değiştirebilirsiniz. 

Tek bir utterance utterance's satırın sağ uç farklı bir hedefi için yeniden atamak için doğru hedefi adı altında seçin **hedefi etiketli** sütun. Utterance geçerli amacı 's utterance listesinden kaldırılır. 

![Bir utterance's amacıyla Seçili Labeled hedefi sütununun altında ekran BookFlight hedefi sayfası](./media/luis-how-to-add-intents/reassign-1-utterance.png)

Birkaç utterances amacı değiştirmek için utterances solundaki onay kutularını seçin ve ardından **yeniden hedefi**. Doğru hedefi listeden seçin.

![İşaretli utterance ve yeniden atama hedefi düğmesi vurgulanmış hedefi sayfasıyla BookFlight, ekran görüntüsü](./media/luis-how-to-add-intents/delete-several-utterances.png) 

## <a name="delete-utterances-on-intent-page"></a>Hedefi sayfasında utterances Sil

Bir utterance silmek için bu utterance çizgi sağ ucunda üç noktaya (...) simgesini seçin ve ardından **silmek**. Utterance listesi ve HALUK uygulama kaldırılır.

![Vurgulanan Sil seçeneğiyle ekran görüntüsü, hedefleri Ayrıntılar sayfası](./media/luis-how-to-add-intents/delete-utterance-ddl.png)

Birkaç utterances silmek için:

1. Utterances solundaki onay kutularını seçin ve ardından **silmek utterances (s)**. 

    ![Ekran görüntüsü, hedefleri Ayrıntıları sayfası, utterances denetlenir ve utterance(s) düğmesi vurgulanmış Sil](./media/luis-how-to-add-intents/delete-several-utterances.png)

2. Seçin **Bitti** içinde **silmek utterances?** açılan iletişim.

## <a name="search-in-utterances-on-intent-page"></a>Hedefi sayfasında utterances içinde arama
Bir amacı, metin (kelimeler ve ifadeler) içeren utterances için arama yapabilirsiniz. Örneğin, belirli bir sözcük içeren bir hata görebilirsiniz ve bu belirli word dahil tüm örnekler bulmak istediğiniz. 

1. Araç çubuğunda Büyüteç simgesini seçin.

    ![Ekran görüntüsü, hedefleri sayfasıyla vurgulanmış Büyüteç arama simgesi](./media/luis-how-to-add-intents/magnifying-glass.png)

2. Arama metin kutusuna görünür. Sözcük veya tümcecik utterances listenin en sağ üst köşedeki arama kutusuna yazın. Utterances arama metninizi içeren utterances görüntülemek için güncelleştirmeleri listeleyin. 

    ![Arama metin kutusuna vurgulanmış olan ekran görüntüsü, hedefleri sayfası](./media/luis-how-to-add-intents/search-textbox.png)

    Aramayı iptal etmek ve utterances tam listesini geri yükleme için yazdığınız arama metni silin. Arama metin kutusuna kapatmak için araç çubuğunda Büyüteç simgesine yeniden seçin.

## <a name="prediction-discrepancy-errors-on-intent-page"></a>Hedefi sayfadaki tahmin tutarsızlık hataları
Bir utterance amacına içinde seçilen amacını ve tahmin puanı arasında bir farklılık olabilir. HALUK puanı etrafında kırmızı kutu ile bu farklılık gösterir. 

![Vurgulanan tahmin tutarsızlık puanı BookFlight hedefi ekran sayfası](./media/luis-how-to-add-intents/score-discrepancy.png) 

## <a name="filter-by-intent-prediction-discrepancy-errors-on-intent-page"></a>Filtre ölçütü hedefi sayfadaki hedefi tahmin tutarsızlık hataları
Yalnızca bir hedefi tahmin tutarsızlık ile utterances utterance listesine filtre uygulamak için gelen geçiş **Tümünü Göster** için **yalnızca hatalar** araç. 

## <a name="filter-by-entity-type-on-intent-page"></a>Hedefi sayfasında varlık türüne göre filtrele
Kullanım **varlık filtreleri** utterances varlık tarafından filtre uygulamak için araç çubuğunda açılır. 

![Varlık Türü Filtresi vurgulanmış olan ekran görüntüsü, hedefleri sayfası](./media/luis-how-to-add-intents/filter-by-entities.png) 

Filtreyi kaldırmak için bu sözcük veya tümceciği araç çubuğunun altında mavi filtre kutusuyla seçin.  
<!-- TBD: waiting for ux fix - bug in ux of prebuit entity number -- when filtering by it, it doesn't show the list -->

## <a name="switch-to-token-view-on-intent-page"></a>Hedefi sayfasında belirteci görünümüne geç
İki durumlu **belirteçleri görüntüle** varlık türü adları yerine belirteçleri görüntülemek için. De kullanabilirsiniz klavyede **denetim + E** görünümüne geçiş yapmak için. 

![Ekran görüntüsü, BookFlight amacıyla, belirteç vurgulanmış görünümü](./media/luis-how-to-add-intents/toggle-tokens-view.png)

## <a name="train-your-app-after-changing-model-with-intents"></a>Hedefleri modeliyle değiştirdikten sonra uygulamanızı eğitme
Eklemek, düzenlemek veya hedefleri, kaldırdıktan sonra [eğitmek](luis-how-to-train.md) ve [yayımlama](PublishApp.md) uygulamanız için uç nokta sorguları etkilemek yaptığınız değişiklikleri. 

## <a name="next-steps"></a>Sonraki adımlar

Uygulamanıza hedefleri eklendikten sonra sonraki göreviniz eklemeye başlamak için olan [örnek utterances](luis-how-to-add-example-utterances.md) eklediğiniz amaçlar için. 
