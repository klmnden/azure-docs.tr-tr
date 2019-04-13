---
title: Hedef ekleme
titleSuffix: Language Understanding - Azure Cognitive Services
description: Intents sorularınız ya da aynı amaçları olan komutları gruplarını tanımlamak için LUIS uygulamanızı ekleyin.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.subservice: language-understanding
ms.topic: article
ms.date: 04/01/2019
ms.author: diberry
ms.service: cognitive-services
ms.openlocfilehash: ed180563ea6138b3b4bab6092b39eeacf9dbf840
ms.sourcegitcommit: 1c2cf60ff7da5e1e01952ed18ea9a85ba333774c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/12/2019
ms.locfileid: "59521751"
---
# <a name="add-intents-to-determine-user-intention-of-utterances"></a>Konuşma kullanıcı amacınıza belirlemek için hedef ekleme

Ekleme [hedefleri](luis-concept-intent.md) LUIS uygulamanızı sorularınız ya da bağdaştırıcılar aynı amaca sahip komutları gruplarını tanımlamak için. 

Hedefleri, üst gezinti çubuğundan 's yönetilir **derleme** bölümünden, ardından sol bölmenin **hedefleri**. 

## <a name="add-intent"></a>Amaç ekleme

1. **Intents** (Amaçlar) sayfasında **Create new intent** (Yeni amaç oluştur) öğesini seçin.

1. İçinde **yeni hedefi oluşturma** iletişim kutusunda, hedefi adı girin `GetEmployeeInformation`, tıklatıp **Bitti**.

    ![Hedefi ekleyin](./media/luis-how-to-add-intents/Addintent-dialogbox.png)

## <a name="add-an-example-utterance"></a>Bir örnek utterance Ekle

Örnek konuşma metin kullanıcı sorularınız ya da komutları örnekleridir. Language Understanding (LUIS) öğretmeyi bir amaç için örnek konuşma eklemeniz gerekir.

1. Üzerinde **GetEmployeeInformation** hedefi Ayrıntıları sayfasında, beklediğiniz kullanıcılarınızdan gelen gibi ilgili bir utterance girin `Does John Smith work in Seattle?` metin kutusuna aşağıdaki hedefi adını ve Enter tuşuna basın.
 
    ![Vurgulanan utterance ile ekran görüntüsü, hedefleri Ayrıntıları sayfası](./media/luis-how-to-add-intents/add-new-utterance-to-intent.png) 

    LUIS, tüm konuşma küçük harfe dönüştürür ve kısa çizgi gibi belirteçleri boşluk ekler.

## <a name="intent-prediction-discrepancy-errors"></a>Hedefi tahmin tutarsızlık hataları 

Bir hedefi olarak bir utterance seçili amaç ve tahmin puanı arasında bir hedefi tahmin uyumsuzluk olabilir. LUIS çevresinde ile bu farklılık gösteren **hedefi etiketli** örnek utterance satırda. 

![Ekran görüntüsü, hedefleri Ayrıntıları sayfası, utterance tahmin tutarsızlık hataları](./media/luis-how-to-add-intents/prediction-discrepancy-intent.png) 

Üst gezinti bölmesinde **eğitme**. Tahmin tutarsızlık sunulmuştur kayboldu.

> [!Note]
> Bir sözcük veya tümcecik örnek utterance içinde altında kırmızı bir çizgi olduğunda bir [varlık tahmin hata](luis-how-to-add-example-utterances.md#entity-status-predictions) oluştu. Düzeltmeniz gerekir. 

## <a name="add-a-custom-entity"></a>Özel bir varlık ekleme

Bir amaç için bir utterance eklendikten sonra bir özel varlık oluşturma utterance metni seçebilirsiniz. Özel bir varlık ayıklama, doğru amaç birlikte etiket metni bir yoludur. 

Bkz: [varlık eklemek için utterance](luis-how-to-add-example-utterances.md) daha fazla bilgi için.

## <a name="entity-prediction-discrepancy-errors"></a>Varlık tahmin tutarsızlık hataları 

Varlık kırmızı renkte göstermek için altı çizili olup bir [varlık tahmin tutarsızlık](luis-how-to-add-example-utterances.md#entity-status-predictions). Bu varlığın ilk geçtiği yeri olduğundan yok yeterli örnekler LUIS Yüksek Güvenilirlikli olması için bu metni doğru varlığı ile etiketlenir. Bu uyuşmazlık, uygulamayı eğitildi kaldırılır. 

![Ekran görüntüsü, hedefleri Ayrıntıları sayfası, mavi renkle vurgulandığı özel varlık adı](./media/luis-how-to-add-intents/create-custom-entity-name-blue-highlight.png) 

Metin gösteren bir varlık mavi renkle vurgulanır.  

## <a name="add-a-prebuilt-entity"></a>Önceden oluşturulmuş bir varlık ekleme

Bilgi için [önceden oluşturulmuş varlık](luis-how-to-add-entities.md#add-a-prebuilt-entity-to-your-app).

## <a name="using-the-contextual-toolbar"></a>Bağlamsal araç çubuğunu kullanma

Bir veya daha fazla örnek konuşma sol tarafındaki bir utterance kutuyu işaretleyerek listesinde seçili olduğunda utterance listenin üstündeki araç aşağıdaki eylemleri gerçekleştirmenizi sağlar:

* Yeniden atama hedefi: farklı eylemlerinize utterance(s) Taşı
* Utterance(s) Sil
* Varlık filtreleri: yalnızca filtrelenen varlık içeren konuşma Göster
* Tümünü Göster / yalnızca hataları: tahmin hatalı konuşma ya da tüm konuşma göstermek
* Varlıkları/belirteç görünümü: varlık adları ile varlıkları görünümünü göster veya utterance ham metni göster
* Büyüteç: belirli bir metni içeren konuşma arayın

## <a name="working-with-an-individual-utterance"></a>Tek bir utterance ile çalışma

Aşağıdaki eylemleri utterance sağındaki üç nokta menüsünden bir bireysel utterance gerçekleştirilebilir:

* Düzen: utterance metnini değiştirme
* Sil: utterance amacından kaldırın. Utterance hala istiyorsanız taşımak için daha iyi bir yöntem olan **hiçbiri** hedefi. 
* Bir desen Ekle: Bir desen, bir ortak utterance alıp değiştirilebilir metin ve böylece daha fazla konuşma amacı, gereksinimini azaltarak Ignorable metin işaretlemek sağlar. 

**Hedefi etiketli** sütun utterance amacı değiştirmenize olanak sağlar.

## <a name="train-your-app-after-changing-model-with-intents"></a>Intents modeliyle değiştirdikten sonra uygulamanızı eğitin

Ekleme, düzenleme veya kaldırma amacı, sonra [eğitme](luis-how-to-train.md) ve [yayımlama](luis-how-to-publish-app.md) uygulamanızı değişikliklerinizi uç nokta sorguları uygulanması. 

## <a name="next-steps"></a>Sonraki adımlar

Ekleme hakkında daha fazla bilgi edinin [örnek konuşma](luis-how-to-add-example-utterances.md) varlıklarla. 
