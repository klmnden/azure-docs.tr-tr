---
title: LUIS uygulamalarda hedef ekleme
titleSuffix: Azure Cognitive Services
description: Intents sorularınız ya da aynı amaçları olan komutları gruplarını tanımlamak için LUIS uygulamanızı ekleyin.
services: cognitive-services
author: diberry
manager: cgronlun
ms.component: language-understanding
ms.topic: article
ms.date: 10/24/2018
ms.author: diberry
ms.service: cognitive-services
ms.openlocfilehash: 495b7e99319126b3ee9e655b2d9aa4af940e1d56
ms.sourcegitcommit: 0f54b9dbcf82346417ad69cbef266bc7804a5f0e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/26/2018
ms.locfileid: "50139933"
---
# <a name="add-intents"></a>Hedef ekleme 

Ekleme [hedefleri](luis-concept-intent.md) LUIS uygulamanızı sorularınız ya da bağdaştırıcılar aynı amaca sahip komutları gruplarını tanımlamak için. 

Hedefleri, üst gezinti çubuğundan 's yönetilir **derleme** bölümünden, ardından sol bölmenin **hedefleri**. 

## <a name="create-an-app"></a>Uygulama oluşturma

1. Oturum [LUIS](https://www.luis.ai) portalı.

1. **Yeni uygulama oluştur**'u seçin. 

1. Yeni uygulama adı `MyHumanResourcesApp`. Seçin **İngilizce** kültür. Açıklama isteğe bağlıdır. 

1. **Done** (Bitti) öğesini seçin. 

## <a name="add-intent"></a>Amaç ekleme

1. Uygulama açılır **hedefleri** listesi.

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

![Vurgulanan utterance ile ekran görüntüsü, hedefleri Ayrıntıları sayfası](./media/luis-how-to-add-intents/prediction-discrepancy-intent.png) 

Üst gezinti bölmesinde **eğitme**. Tahmin tutarsızlık sunulmuştur kayboldu.

## <a name="add-a-custom-entity"></a>Özel bir varlık ekleme

Bir amaç için bir utterance eklendikten sonra bir özel varlık oluşturma utterance metni seçebilirsiniz. Özel bir varlık ayıklama, doğru amaç birlikte etiket metni bir yoludur. 

1. Word ' ü seçin `Seattle`, utterance içinde. Metin etrafına köşeli ayraç çizilir ve bir açılan menü görünür. 

    ![Özel varlık oluşturma ekran görüntüsü, hedefleri Ayrıntıları sayfası](./media/luis-how-to-add-intents/create-custom-entity.png) 

    Bu örnek, bir varlık olarak işaretlemek için tek sözcük seçer. Tek çalışır ve ifadeleri varlıklar olarak işaretleyebilirsiniz.

1. Üstteki metin-menüsünün kutusuna `Location`, ardından **yeni varlık Oluştur**. 

    ![Özel varlık adı oluşturma ekran görüntüsü, hedefleri Ayrıntıları sayfası](./media/luis-how-to-add-intents/create-custom-entity-name.png) 

1. İçinde **ne tür bir varlık oluşturmak istiyorsunuz?** varlık oluşturmak için açılır pencere doğrulama **varlık adı** olduğu _konumu_ve **varlık türü**  olduğu _basit_. **Done** (Bitti) öğesini seçin.

## <a name="entity-prediction-discrepancy-errors"></a>Varlık tahmin tutarsızlık hataları 

Varlık kırmızı renkte göstermek için altı çizili olup bir [varlık tahmin tutarsızlık](luis-how-to-add-example-utterances.md#entity-status-predictions). Bu varlığın ilk geçtiği yeri olduğundan yok yeterli örnekler LUIS Yüksek Güvenilirlikli olması için bu metni doğru varlığı ile etiketlenir. Bu uyuşmazlık, uygulamayı eğitildi kaldırılır. 

![Ekran görüntüsü, hedefleri Ayrıntıları sayfası, mavi renkle vurgulandığı özel varlık adı](./media/luis-how-to-add-intents/create-custom-entity-name-blue-highlight.png) 

Metin gösteren bir varlık mavi renkle vurgulanır.  

## <a name="add-a-prebuilt-entity"></a>Önceden oluşturulmuş bir varlık ekleme

Bilgi için [önceden oluşturulmuş varlık](luis-how-to-add-entities.md#add-prebuilt-entity).

## <a name="using-the-contextual-toolbar"></a>Bağlamsal araç çubuğunu kullanma

Bir veya daha fazla örnek konuşma seçiliyken listesinde bir utterance solundaki kutunun işaretlenmesi yoluyla utterance listenin üstündeki araç aşağıdaki eylemleri gerçekleştirmenizi sağlar:

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
* Bir desen Ekle: ortak utterance alıp değiştirilebilir metin ve böylece daha fazla konuşma amacı, gereksinimini azaltarak Ignorable metin işaretlemek bir desen sağlar. 

**Hedefi etiketli** sütun utterance amacı değiştirmenize olanak sağlar.

## <a name="train-your-app-after-changing-model-with-intents"></a>Intents modeliyle değiştirdikten sonra uygulamanızı eğitin

Ekleme, düzenleme veya kaldırma amacı, sonra [eğitme](luis-how-to-train.md) ve [yayımlama](luis-how-to-publish-app.md) uygulamanızı değişikliklerinizi uç nokta sorguları uygulanması. 

## <a name="next-steps"></a>Sonraki adımlar

Ekleme hakkında daha fazla bilgi edinin [örnek konuşma](luis-how-to-add-example-utterances.md) varlıklarla. 
