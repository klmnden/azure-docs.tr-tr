---
title: Pattern.Any varlık
titleSuffix: Azure Cognitive Services
description: Konuşmaların iyi biçimlendirilmiş olduğu ve verilerin sonunun konuşmadaki diğer sözcüklerle karıştırılma ihtimalinin yüksek olduğu konuşmalardan veri ayıklamak için pattern.any varlığını kullanın.
services: cognitive-services
ms.custom: seodec18
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: tutorial
ms.date: 12/21/2018
ms.author: diberry
ms.openlocfilehash: 8ab24d478efa0d0006cff618d7760d4396d0e45e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60495290"
---
# <a name="tutorial-extract-free-form-data-with-patternany-entity"></a>Öğretici: Serbest biçimli Pattern.any varlık verileriyle ayıklayın

Bu öğreticide konuşmaların iyi biçimlendirilmiş olduğu ve verilerin sonunun konuşmadaki diğer sözcüklerle karıştırılma ihtimalinin yüksek olduğu konuşmalardan veri ayıklamak için pattern.any varlığını kullanacaksınız. 

**Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:**

> [!div class="checklist"]
> * Örnek uygulamayı içeri aktarma
> * Var olan varlığa örnek konuşma ekleme
> * Pattern.any varlığını oluşturma
> * Desen oluşturma
> * Eğitim
> * Yeni deseni test etme

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="using-patternany-entity"></a>Pattern.Any varlık kullanma

pattern.any varlığı, ifade nedeniyle varlığın sonunun konuşmanın geri kalanından ayrılmasının zor olduğu durumlarda serbest biçimli verileri bulmanızı sağlar. 

Bu İnsan Kaynakları uygulaması, çalışanların şirket formlarını bulmasına yardımcı olmaktadır. 

|İfade|
|--|
|Where is **HRF-123456**? (HRF-123456 nerede?)|
|Who authored **HRF-123234**? (HRF-123234'ü kim yazdı?)|
|**HRF-456098** is published in French? (HRF-456098 Fransızca mı yayımlandı?)|

Ancak her formun hem bir biçimlendirilmiş adı (yukarıdaki tabloda kullanılan) hem de kolay adı (`Request relocation from employee new to the company 2018 version 5` gibi) vardır. 

Formun kolay adı konuşmada şu şekilde olur:

|İfade|
|--|
|Where is **Request relocation from employee new to the company 2018 version 5**? (Yeni şirket çalışanı taşınma talep formu 2018 sürüm 5 nerede?)|
|Who authored **"Request relocation from employee new to the company 2018 version 5"**? (Yeni şirket çalışanı taşınma talep formu 2018 sürüm 5'i kim yazdı?)|
|**Request relocation from employee new to the company 2018 version 5** is published in French? (Yeni şirket çalışanı taşınma talep formu 2018 sürüm 5 Fransızca mı yayımlandı?)|

Sözcüklerin uzunluklarının değişmesi, LUIS'in varlığın sonunu belirleme konusunda karışıklık yaşamasına neden olabilir. Pattern.any varlığını bir desen içinde kullanmak, form adının başını ve sonunu belirterek LUIS'in form adını doğru şekilde ayıklamasına yardımcı olmanızı sağlar.

|Konuşma şablonu örneği|
|--|
|Where is {FormName}[?] ({FormName} nerede[?])|
|Who authored {FormName}[?] ({FormName} kim yazdı[?])|
|{FormName} is published in French[?] ({FormName} Fransızca mı yayımlandı[?])|

## <a name="import-example-app"></a>Örnek uygulamayı içeri aktarma
Son öğreticide oluşturulan **HumanResources** adlı uygulamayla devam edin. 

Aşağıdaki adımları kullanın:

1.  [Uygulama JSON dosyasını](https://github.com/Azure-Samples/cognitive-services-language-understanding/blob/master/documentation-samples/tutorials/custom-domain-pattern-roles-HumanResources.json) indirip kaydedin.

2. JSON'ı yeni bir uygulamaya içeri aktarın.

3. **Yönet** bölümünde **Sürümler** sekmesinde sürümü kopyalayın ve `patt-any` olarak adlandırın. Kopyalama, özgün sürümünüzü etkilemeden farklı LUIS özelliklerini deneyebileceğiniz ideal bir yol sunar. Sürüm adı, URL rotasının bir parçası olarak kullanıldığından ad bir URL'de geçerli olmayan herhangi bir karakter içeremez.

## <a name="add-example-utterances"></a>Örnek konuşmalar ekleme 
FormName varlığını oluşturup etiketlemek zor geliyorsa önceden oluşturulmuş keyPhrase varlığını kaldırın. 

1. Üst gezinti bölmesinden **Build** (Derle) öğesini, sol gezinti bölmesinden de **Intents** (Amaçlar) öğesini seçin.

2. Amaç listesinden **FindForm** öğesini seçin.

3. Birkaç örnek konuşma ekleyin:

    |Örnek konuşma|
    |--|
    |Where is the form **What to do when a fire breaks out in the Lab** and who needs to sign it after I read it? (Laboratuvarda yangın çıktığında yapılacak işlemler formu nerede ve ben okuduktan sonra kimin imzalaması gerekiyor?)|
    |Where is **Request relocation from employee new to the company** on the server? (Yeni şirket çalışanı taşınma talep formu sunucuda nerede?)|
    |Who authored "**Health and wellness requests on the main campus**" and what is the most current version? ("Ana kampüsteki sağlık ve sağlıklı yaşam istekleri"ni kim yazdı ve en güncel sürümü nedir?)|
    |I'm looking for the form named "**Office move request including physical assets**". ("Fiziksel eşyalar dahil olmak üzere ofis taşıma isteği" adlı formu arıyorum.) |

    Form adlarındaki değişik kullanımlar nedeniyle Pattern.any varlığı olmadan LUIS'in form başlığının sonunu belirlemesi zor olacaktır.

## <a name="create-a-patternany-entity"></a>Pattern.any varlığı oluşturma
Pattern.any varlığı farklı uzunluklardaki varlıkları ayıklar. Desen varlığın başını ve sonunu belirlediğinden yalnızca bir desen içinde çalışır. Pattern.any içerdiğinde deseninizin varlıkları yanlış ayıkladığını fark ederseniz bu sorunu gidermek için [açık liste](luis-concept-patterns.md#explicit-lists) kullanın. 

1. Sol gezinti panelinden **Entities** (Varlıklar) öğesini seçin.

2. **Create new entity** (Yeni varlık oluştur) öğesini seçin, `FormName` adını girin ve tür olarak **Pattern.any** seçin. **Done** (Bitti) öğesini seçin. 

    Pattern.any yalnızca bir desende geçerli olduğundan bu varlığı bir amaçta etiketleyemezsiniz. 

    Ayıklanan verilere number veya datetimeV2 biçiminde diğer varlıkların da dahil edilmesini istiyorsanız Pattern.any ile number ve datetimeV2 varlıklarını da içeren bir birleşik varlık oluşturmanız gerekir.

## <a name="add-a-pattern-that-uses-the-patternany"></a>Pattern.any kullanan bir desen ekleme

1. Sol gezinti bölmesinden **Patterns** (Desenler) öğesini seçin.

2. **FindForm** amacını seçin.

3. Aşağıdaki yeni varlığı kullanan konuşma şablonlarını girin:

    |Konuşma şablonları|
    |--|
    |Where is the form ["]{FormName}["] and who needs to sign it after I read it[?] (["]{FormName}["] formu nerede ve ben okuduktan sonra kimin imzalaması gerekiyor[?])|
    |Where is ["]{FormName}["] on the server[?] (["]{FormName}["] sunucuda nerede[?])|
    |Who authored ["]{FormName}["] and what is the most current version[?] (["]{FormName}["] kim yazdı ve en güncel sürümü nedir[?])|
    |I'm looking for the form named ["]{FormName}["][.] (["]{FormName}["] adlı formu arıyorum[.])|

    Çift tırnak yerine tek tırnak veya soru işareti yerine nokta gibi form farklılıklarının dikkate alınmasını istiyorsanız her farklı kullanım için yeni bir desen oluşturun.

4. keyPhrase varlığını kaldırdıysanız uygulamaya geri ekleyin. 

## <a name="train-the-luis-app"></a>LUIS uygulamasını eğitme

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="test-the-new-pattern-for-free-form-data-extraction"></a>Serbest biçimli metin ayıklama için yeni deseni test etme
1. Üst çubuktan **Test**'i seçerek test panelini açın. 

2. Aşağıdaki konuşmayı girin: 

    `Where is the form Understand your responsibilities as a member of the community and who needs to sign it after I read it?`

3. Varlık ve amaç test sonuçlarını görmek için sonucun altındaki **Inspect** (Denetle) öğesini seçin.

    Önce `FormName` varlığı bulunmuş, ardından amacı belirten desen bulunmuştur. Test sonucunda varlıklar algılanmadıysa ve bu nedenle desen bulunmadıysa amaçla (desenle değil) ilgili daha fazla örnek konuşma eklemeniz gerekir.

4. Üst gezinti çubuğundan **Test** düğmesini seçerek test panelini kapatın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide var olan bir amaca örnek konuşmalar ekleyip form adı için yeni bir Pattern.any varlığı oluşturdunuz. Daha sonra öğreticide yeni örnek konuşmalar ve varlıkla var olan amaç için bir desen oluşturdunuz. Etkileşimli test sonucunda varlık bulunduğu için desenin ve amacının tahmin edildiğini gördünüz. 

> [!div class="nextstepaction"]
> [Desen ile rol kullanmayı öğrenin](luis-tutorial-pattern-roles.md)
