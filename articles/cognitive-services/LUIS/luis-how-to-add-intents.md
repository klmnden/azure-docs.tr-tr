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
ms.openlocfilehash: 0c42ab44ba317888b982ba7c72f78be4ca73d93c
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65148161"
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

<a name="#intent-prediction-discrepancy-errors"></a>

## <a name="intent-prediction-errors"></a>Hedefi tahmin hataları 

Bir örnek utterance bir amacı, hedefi tahmin hata yer şu anda örnek utterance amaç ve eğitim sırasında belirlenen tahmin hedefi arasında olabilir. 

Utterance tahmin hataları bulmak ve bunları düzeltmek için kullanın **filtre** seçeneğin **değerlendirme** yanlış ve Unclear seçenekleri Sunucusu'yla birlikte **görünümü** seçeneği**Ayrıntılı görünümü**. 

![Utterance tahmin hataları bulmak ve bunları düzeltmek için filtre seçeneğini kullanın.](./media/luis-how-to-add-intents/find-intent-prediction-errors.png)

Filtreler ve görünüm uygulanır ve örnek konuşma hatalarla olduğunda, örnek utterance liste konuşma ve sorunları gösterir.

![! [Filtreler ve görünüm uygulanır ve örnek konuşma hatalarla olduğunda, örnek utterance liste konuşma ve sorunları gösterir.] (. / media/luis-how-to-add-intents/find-errors-in-utterances.png)](./media/luis-how-to-add-intents/find-errors-in-utterances.png#lightbox)

Her satır için bu iki puanları farkı olan en yakın müsabık'ın puanı örnek utterance geçerli eğitim 's tahmin puanı gösterir. 

### <a name="fixing-intents"></a>Intents düzeltme

Hedefi tahmin hataların nasıl düzeltileceğini öğrenmek için kullanın [Özet Panosu](luis-how-to-use-dashboard.md). Özet panosu için etkin sürüme ait son eğitim analizini sağlar ve modelinizi düzeltmek için en çok istenen önerilerden sunar.  

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
