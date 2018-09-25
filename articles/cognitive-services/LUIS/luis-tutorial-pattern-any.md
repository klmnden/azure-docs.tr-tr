---
title: 'Öğretici 5: Pattern.any varlık serbest biçimli metin'
titleSuffix: Azure Cognitive Services
description: Burada verilerin sonuna kolayca utterance kalan kelimelerinizle aklınızı karıştırabilir ve konuşma olduğu iyi biçimlendirilmiş pattern.any varlık konuşma verileri ayıklamak için kullanın.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.technology: language-understanding
ms.topic: article
ms.date: 09/09/2018
ms.author: diberry
ms.openlocfilehash: a5b964877f1c089015812abdf365b22c57267bb0
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "47031383"
---
# <a name="tutorial-5-extract-free-form-data"></a>Öğretici: 5. Serbest biçimli verileri ayıklama

Bu öğreticide, pattern.any varlık konuşma iyi biçimlendirilmiş olduğu ve burada verilerin sonuna kolayca utterance kalan kelimelerinizle aklınızı karıştırabilir konuşma verileri ayıklamak için kullanın. 

Pattern.any varlık burada ifade varlığın varlık utterance geri kalanından sonuna belirlemek zorlaştırır serbest biçimli veri bulmanıza olanak tanır. 

İnsan Kaynakları uygulamasında bu, çalışanların şirket formları yardımcı olur. 

|İfade|
|--|
|Nerede olduğunu **HRF 123456**?|
|Kimin yazılan **HRF 123234**?|
|**HRF 456098** Fransızca yayımlanan?|

Ancak, her bir form yukarıdaki tabloda, yanı sıra bir kolay ad gibi kullanılan hem bir biçimlendirilmiş bir ada sahip `Request relocation from employee new to the company 2018 version 5`. 

Konuşma kolay form adı ile şöyle görünür:

|İfade|
|--|
|Nerede olduğunu **yeniden konumlandırma çalışan şirket 2018 sürüm 5 yeni Request**?|
|Kimin yazılan **"Request yeniden konumlandırma çalışan şirket 2018 sürüm 5 yeni"**?|
|**Yeniden konumlandırma çalışan şirket 2018 sürüm 5 yeni istek** Fransızca yayımlanan?|

Değişken uzunluğu LUIS varlık sona ereceği hakkında karışıklığa neden olabilir sözcükleri içerir. İçindeki bir desenle Pattern.any varlık kullanarak LUIS form adı doğru ayıklar. böylece form adının sonunda ve başındaki belirtmenizi sağlar.

|Şablon utterance örneği|
|--|
|Burada {formadı} [?] olan|
|Kimin {formadı} [?] yazıldı|
|Fransızca [?] {formadı} yayımlanır|

**Bu öğreticide, şunların nasıl yapılır:**

> [!div class="checklist"]
> * Mevcut öğretici uygulamasını kullanma
> * Varolan bir varlık için örnek Konuşma ekleme
> * Pattern.Any varlık oluşturma
> * Desen oluşturmak
> * Eğitim
> * Yeni test düzeni

[!include[LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="use-existing-app"></a>Var olan bir uygulama kullanma
Adlı son öğreticisinde oluşturulan uygulama devam **İnsanKaynakları**. 

Önceki öğreticide İnsanKaynakları uygulamadan yoksa aşağıdaki adımları kullanın:

1.  İndirip kaydedin [uygulama JSON dosyasını](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/tutorials/custom-domain-roles-HumanResources.json).

2. JSON, yeni bir uygulamaya aktarma.

3. Gelen **Yönet** üzerinde bölümünde **sürümleri** sekmesinde sürüm kopyalayın ve adlandırın `patt-any`. Kopyalama, özgün sürümünüzü etkilemeden farklı LUIS özelliklerini deneyebileceğiniz ideal bir yol sunar. Sürüm adı, URL rota bir parçası olarak kullanıldığından, adın bir URL geçerli olmayan karakterler içeremez.

## <a name="add-example-utterances"></a>Örnek Konuşma ekleme 
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

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="test-the-new-pattern-for-free-form-data-extraction"></a>Serbest biçimli veri ayıklama yeni desenini test
1. Seçin **Test** test panelini açmak için üst çubuğunda. 

2. Aşağıdaki utterance girin: 

    `Where is the form Understand your responsibilities as a member of the community and who needs to sign it after I read it?`

3. Seçin **inceleyin** varlık ve amacı test sonuçlarını görmek için sonuç altında.

    Varlık `FormName` amacını belirleme düzeni bulunduktan sonra ilk olarak bulunur. Bir test sonucu burada varlıkları algılanmadı ve bu nedenle düzeni bulunamadı'iniz varsa, daha fazla örnek konuşma amaca (deseni değil) eklemeniz gerekir.

4. Seçerek test paneli kapatmak **Test** üst gezinti düğmesi.

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu öğretici, var olan bir amaç için örnek konuşma eklenen ardından form adı için yeni bir Pattern.any oluşturulan. Ardından öğretici varlık ve yeni örnek Konuşma ile mevcut hedefi için bir düzen oluşturdunuz. Etkileşimli test varlık bulunamadığından deseni ve konuşmanın niyetini tahmin olduğunu göstermiştir. 

> [!div class="nextstepaction"]
> [Roller bir desen ile kullanmayı öğrenin](luis-tutorial-pattern-roles.md)