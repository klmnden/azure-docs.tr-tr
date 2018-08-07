---
title: LUIS tahminleri - Azure geliştirmek için pattern.Any varlık kullanma Öğreticisi | Microsoft Docs
titleSuffix: Cognitive Services
description: Bu öğreticide, pattern.any varlık LUIS amacı ve varlık Öngörüler geliştirmek için kullanın.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.technology: luis
ms.topic: article
ms.date: 08/02/2018
ms.author: diberry
ms.openlocfilehash: 1587debecd82072c29d4caffc2b81629b1f52b0e
ms.sourcegitcommit: 9819e9782be4a943534829d5b77cf60dea4290a2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2018
ms.locfileid: "39527373"
---
# <a name="tutorial-improve-app-with-patternany-entity"></a>Öğretici: pattern.any varlığı ile uygulama geliştirin

Bu öğreticide, hedefi ve varlık tahmin artırmak için pattern.any varlık kullanın.  

> [!div class="checklist"]
* Ne zaman ve pattern.any kullanmayı öğrenin
* Pattern.any kullanan düzeni oluşturma
* Tahmin geliştirmeleri doğrulama

[!include[LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="before-you-begin"></a>Başlamadan önce
İnsan Kaynakları uygulamadan yoksa [desen rolleri](luis-tutorial-pattern-roles.md) öğreticide [alma](luis-how-to-start-new-app.md#import-new-app) JSON'a yeni bir uygulama [LUIS](luis-reference-regions.md#luis-website) Web sitesi. İçeri aktarılacak uygulamasını bulunan [LUIS-Samples](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/quickstarts/custom-domain-roles-HumanResources.json) GitHub deposu.

Özgün İnsan Kaynakları uygulamasını tutmak istiyorsanız [Settings](luis-how-to-manage-versions.md#clone-a-version) (Ayarlar) sayfasında sürümü kopyalayıp adını `patt-any` olarak değiştirin. Kopyalama, özgün sürümünüzü etkilemeden farklı LUIS özelliklerini deneyebileceğiniz ideal bir yol sunar. 

## <a name="the-purpose-of-patternany"></a>Pattern.any amacı
Pattern.any varlık burada ifade varlığın varlık utterance geri kalanından sonuna belirlemek zorlaştırır serbest biçimli veri bulmanıza olanak tanır. 

İnsan Kaynakları uygulamasında bu, çalışanların şirket formları yardımcı olur. Forms eklendi [normal ifade öğretici](luis-quickstart-intents-regex-entity.md). Bu öğreticiden form adları form adları gibi iyi biçimlendirilmiş bir form adı ayıklamak için normal bir ifade kullanılan, aşağıdaki tabloda utterance Kalın:

|Konuşma|
|--|
|Nerede olduğunu **HRF 123456**?|
|Kimin yazılan **HRF 123234**?|
|**HRF 456098** Fransızca yayımlanan?|

Ancak, her bir form yukarıdaki tabloda, yanı sıra bir kolay ad gibi kullanılan hem bir biçimlendirilmiş bir ada sahip `Request relocation from employee new to the company 2018 version 5`. 

Konuşma kolay form adı ile şöyle görünür:

|Konuşma|
|--|
|Nerede olduğunu **yeniden konumlandırma çalışan şirket 2018 sürüm 5 yeni Request**?|
|Kimin yazılan **"Request yeniden konumlandırma çalışan şirket 2018 sürüm 5 yeni"**?|
|**Yeniden konumlandırma çalışan şirket 2018 sürüm 5 yeni istek** Fransızca yayımlanan?|

Değişken uzunluğu LUIS varlık sona ereceği hakkında karışıklığa neden olabilir ifadeleri içerir. İçindeki bir desenle Pattern.any varlık kullanarak LUIS form adı doğru ayıklar. böylece form adının sonunda ve başındaki belirtmenizi sağlar.

**Desenler varlıkları değil algılanırsa, daha az örnek konuşma sağlamanıza olanak tanırken desenle eşleşmez.**

## <a name="add-example-utterances-to-the-existing-intent-findform"></a>Mevcut ıntent'e FindForm örnek Konuşma ekleme 
Önceden oluşturulmuş bir anahtar cümlesi varlık oluşturun ve formadı varlık etiketi zor ise kaldırın. 

1. Seçin **derleme** üst gezinti bölmesinden seçip **hedefleri** sol gezinti bölmesinden.

2. Seçin **FindForm** hedefleri listesinde.

3. Bazı örnek Konuşma ekleme:

    |Örnek utterance|
    |--|
    |Formun **laboratuvarda yangın kesilmeler ne yapılacağını** ve onu okuduktan sonra oturum açmak gerek duyan?|
    |Nerede olduğunu **yeniden konumlandırma çalışan şirkete yeni Request** sunucuda?|
    |Kimin yazılan "**ana kampüs sağlık ve Zindelik isteklerinde**" ve en güncel sürümü nedir?|
    |Adlı form bakarak "**Office taşıma isteği fiziksel varlıklar dahil olmak üzere**". |

    Bir Pattern.any varlık LUIS form başlığı form adları çeşitli kullanımları nedeniyle sona ereceği anlaşılması zor olacaktır.

## <a name="create-a-patternany-entity"></a>Pattern.any varlık oluşturma
Varlıklar farklı uzunluktaki Pattern.any varlık ayıklar. Desen Başlangıç ve bitiş varlık işaretlendikleri yalnızca bir düzende çalışır. Bir Pattern.any içerdiğinde, deseni bulabilirsiniz, ayıklar varlıkları yanlış kullanmak bir [açık listesi](luis-concept-patterns.md#explicit-lists) bu sorunu düzeltmek için. 

1. Seçin **varlıkları** sol gezinti bölmesinde.

2. Seçin **yeni varlık Oluştur**, adını `FormName`seçip **Pattern.any** türü. **Done** (Bitti) öğesini seçin. 

    Bir Pattern.any yalnızca bir düzende geçerli olduğundan amaç varlığı etiketi olamaz. 

    Ayıklanan verileri sayı veya datetimeV2 gibi diğer varlıklar dahil etmek istiyorsanız, Pattern.any, yanı sıra sayısı ve datetimeV2 içeren bileşik bir varlık oluşturmanız gerekir.

## <a name="add-a-pattern-that-uses-the-patternany"></a>Pattern.any kullanan bir desen Ekle

1. Seçin **desenleri** sol gezinti bölmesinden.

2. Seçin **FindForm** hedefi.

3. Yeni varlık aşağıdaki şablon konuşma girin:

    |Şablon konuşma|
    |--|
    |Formun ["] {formadı} ["] ve [?], okuduktan sonra oturum açmak gerek duyan|
    |Burada olduğundan ["] {formadı} ["] [?] sunucuda|
    |Kimin yazılan ["] {formadı} ["] ve [?] en güncel sürümü nedir|
    |Adlı form bakarak ["] {formadı} ["] [.]|

    Tek tırnak işaretleri yerine çift tırnak işareti veya soru işareti yerine nokta gibi formun çeşitleri için hesap istiyorsanız, her bir değişim için yeni bir düzen oluşturun.

4. Anahtar cümlesi varlık kaldırılırsa, uygulamaya geri ekleyin. 

## <a name="train-the-luis-app"></a>LUIS uygulamasını eğitme

[!include[LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="test-the-new-pattern-for-free-form-data-extraction"></a>Serbest biçimli veri ayıklama yeni desenini test
1. Seçin **Test** test panelini açmak için üst çubuğunda. 

2. Aşağıdaki utterance girin: 

    `Where is the form Understand your responsibilities as a member of the community and who needs to sign it after I read it?`

3. Seçin **inceleyin** varlık ve amacı test sonuçlarını görmek için sonuç altında.

    Varlık `FormName` amacını belirleme düzeni bulunduktan sonra ilk olarak bulunur. Bir test sonucu burada varlıkları algılanmadı ve bu nedenle düzeni bulunamadı'iniz varsa, daha fazla örnek konuşma amaca (deseni değil) eklemeniz gerekir.

4. Seçerek test paneli kapatmak **Test** üst gezinti düğmesi.

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!include[LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Roller bir desen ile kullanmayı öğrenin](luis-tutorial-pattern-roles.md)