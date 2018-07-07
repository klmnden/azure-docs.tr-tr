---
title: LUIS tahminleri - Azure geliştirmek için desenler kullanma Öğreticisi | Microsoft Docs
titleSuffix: Azure
description: Bu öğreticide, hedefleri için düzeni LUIS amacı ve varlık Öngörüler geliştirmek için kullanın.
services: cognitive-services
author: v-geberr
manager: kamran.iqbal
ms.service: cognitive-services
ms.technology: luis
ms.topic: article
ms.date: 05/07/2018
ms.author: v-geberr;
ms.openlocfilehash: 12105829f62b988760d3bbf18000466fd27b9aff
ms.sourcegitcommit: 11321f26df5fb047dac5d15e0435fce6c4fde663
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/06/2018
ms.locfileid: "37888341"
---
# <a name="tutorial-use-patterns-to-improve-predictions"></a>Öğretici: desenleri Öngörüler geliştirmek için kullanın.

Bu öğreticide, hedefi ve varlık tahmin artırmak için desenleri kullanın.  

> [!div class="checklist"]
* Bir desen uygulamanıza yardımcı olduğunu belirleme
* Bir düzen oluşturma 
* İçindeki bir desenle önceden oluşturulmuş ve özel varlıklar kullanma 
* Desen tahmin geliştirmeleri doğrulama
* Bağlamsal tabanlı varlıkları bulmak için bir varlığa bir rolü ekleme
* Serbest biçimli varlıkları bulmak için bir Pattern.any ekleme

Bu makale için kendi LUIS uygulamanızı yazma amacıyla ücretsiz bir [LUIS](luis-reference-regions.md) hesabına ihtiyacınız olacak.

## <a name="import-humanresources-app"></a>İçeri aktarma İnsanKaynakları uygulama
Bu öğreticide, bir İnsanKaynakları uygulaması içeri aktarır. Uygulamanın üç amacı vardır: hiçbiri, GetEmployeeOrgChart, GetEmployeeBenefits. İki varlık uygulama vardır: önceden oluşturulmuş sayısı ve çalışan. Çalışan varlık, bir çalışanın adı ayıklamak için basit bir varlıktır. 

1. Yeni bir LUIS uygulaması dosya oluşturun ve adlandırın `HumanResources.json`. 

2. Aşağıdaki uygulama tanımını dosyaya kopyalayın:

   [!code-json[Add the LUIS model](~/samples-luis/documentation-samples/tutorial-patterns/HumanResources.json?range=1-164 "Add the LUIS model")]

3. LUIS üzerinde **uygulamaları** sayfasında **alma yeni uygulama**. 

4. İçinde **alma yeni uygulama** iletişim kutusunda `HumanResources.json` 1. adımda oluşturulan dosya.

5. Seçin **GetEmployeeOrgChart** amacı ve ardından değişiklik **varlıkları görünümü** için **belirteçler görünümü**. Birkaç örnek konuşma listelenir. Her utterance çalışan bir varlık adları içeriyor. Her ad farklıdır ve ifadeyi yerleşimini her utterance için farklı olduğuna dikkat edin. Bu seviyelerine LUIS konuşma geniş kapsamlı bilgi edinin yardımcı olur.

    ![Varlıkları görünüm açılıp sayfasıyla amacıyla ekran görüntüsü](media/luis-tutorial-pattern/utterances-token-view.png)

6. Seçin **eğitme** uygulama geliştirmek için üst gezinti çubuğunda. Yeşil bir başarı çubuğunun tamamlanmasını bekleyin.

7. Seçin **Test** üst panelinde. Girin `Who does Patti Owens report to?` seçin ENTER. Seçin **inceleyin** test hakkında daha fazla bilgi için utterance altında.
    
    Çalışan adı, Patti Owens henüz bir örnek utterance kullanılan değil. Bu, ne kadar iyi LUIS bu utterance için öğrenilen görmek için bir test `GetEmployeeOrgChart` amacı ve çalışan varlığı olmalıdır `Patti Owens`. Sonucu %50 olmalıdır (. 50) için `GetEmployeeOrgChart` hedefi. Amaç doğru olsa da, puan düşüktür. Çalışan varlık da doğru olarak tanımlanan `Patti Owens`. Bu ilk tahmin puanı desenleri artırın. 

    ![Ekran görüntüsü, Test paneli](media/luis-tutorial-pattern/original-test.png)

8. Seçerek test paneli kapatmak **Test** üst gezinti düğmesi. 

## <a name="patterns-teach-luis-common-utterances-with-fewer-examples"></a>Desenler LUIS daha az sayıda örnek ile ortak konuşma öğretin.
İnsan Kaynakları etki alanı yapısı nedeniyle, kuruluşların çalışan ilişkiler hakkında soran birkaç genel yol vardır. Örneğin:

```
Who does Mike Jones report to?
Who reports to Mike Jones? 
```

Bu konuşma yakın her birçok utterance örneği sağlamadan bağlamsal benzersizlik belirleyin. Bir amaç için bir düzen ekleyerek, birçok utterance örneği sağlamadan LUIS bir amaç için ortak utterance düzenlerini öğrenir. 

Örnek şablon konuşma niyetini şunlar:

```
Who does {Employee} report to?
Who reports to {Employee}? 
```

Desen, normal ifade eşleştirme ve makine öğrenimi birleşimidir. Ardından, bazı şablonu LUIS deseni öğrenmek için utterance örnekler sağlar. Hedefi konuşma yanı sıra bu örnekleri LUIS amaç hangi konuşma uygun daha iyi anlamasını ve utterance içinde varlık mevcut olduğu,. <!--A pattern is specific to an intent. You can't duplicate the same pattern on another intent. That would confuse LUIS, which lowers the prediction score. -->

## <a name="add-the-template-utterances"></a>Şablon Konuşma ekleme

1. Sol gezinti bölmesindeki altında **uygulama performansını**seçin **desenleri** sol gezinti bölmesinden.

2. Seçin **GetEmployeeOrgChart** amacı, aşağıdaki şablon konuşma, bir kerede enter, seçtikten sonra her şablon utterance girin:

    ```
    Does {Employee} have {number} subordinates?
    Does {Employee} have {number} direct reports?
    Who does {Employee} report to?
    Who reports to {Employee}?
    Who is {Employee}'s manager?
    Who are {Employee}'s subordinates?
    ```

    `{Employee}` Söz dizimi hangi varlık olarak da öyledir olarak şablon utterance içindeki varlık konum işaretler. 

    ![Şablon konuşma amacı için girme ekran görüntüsü](./media/luis-tutorial-pattern/enter-pattern.png)

3. Seçin **eğitme** üst gezinti çubuğunda. Yeşil bir başarı çubuğunun tamamlanmasını bekleyin.

4. Seçin **Test** üst panelinde. Girin `Who does Patti Owens report to?` metin kutusuna. Select girin. Önceki bölümde test aynı utterance budur. Sonuç için daha yüksek olmalıdır `GetEmployeeOrgChart` hedefi. 

    Sonuç artık çok daha iyidir. LUIS, birçok örneği sağlamadan ıntent'e ilgili deseni öğrendiniz.

    ![Yüksek puan paneliyle ekran görüntüsü, Test sonucu](./media/luis-tutorial-pattern/high-score.png)

    Desen bulunursa, amacını belirten sonra varlık ilk olarak bulunur. Bir test sonucu burada varlık algılanmadı ve bu nedenle düzeni bulunamadı'iniz varsa, daha fazla örnek konuşma amaca (deseni değil) eklemeniz gerekir. 

5. Seçerek test paneli kapatmak **Test** üst gezinti düğmesi.

## <a name="use-an-entity-with-a-role-in-a-pattern"></a>Bir desende rolüne sahip bir varlık kullanın
LUIS uygulaması, çalışanların bir konumdan diğerine taşımanıza yardımcı olmak için kullanılır. Bir örnek utterance olduğu `Move Bob Jones from Seattle to Los Colinas`. Utterance her bir konumdaki farklı bir anlama sahiptir. Seattle kaynak konumu ve Los Colinas taşımak için hedef konum. İki rol ile bir varlığın konum için oluşturduğunuz desen aşağıdaki bölümlerde, bu konumlardaki ayırt etmek için: kaynak ve hedef. 

### <a name="create-a-new-intent-for-moving-people-and-assets"></a>Kişiler ve varlıklar taşımak için yeni bir hedefi oluşturma
Yeni bir amaç için taşıma kişiler veya varlıklar hakkında herhangi bir konuşma oluşturun.

1. Seçin **hedefleri** sol gezinti bölmesinden
2. Seçin **yeni hedefi oluşturma**
3. Yeni hedefi adı `MoveAssetsOrPeople`
4. Örnek Konuşma ekleme:

    ```
    Move Bob Jones from Seattle to Los Colinas
    Move Dave Cooper from Redmond to Seattle
    Move Jim Smith from Toronto to Vancouver
    Move Jill Benson from Boston to London
    Move Travis Hinton from Portland to Orlando
    ```
    ![Örnek utterance MoveAssetsOrPeople hedefi için ekran görüntüsü](./media/luis-tutorial-pattern/intent-moveasserts-example-utt.png)

    Örnek konuşma amacı, yeterli örnekler vermektir. Test konumu varlık algılanan değildir ve sonuç olarak desenini algılanan değil, bu adıma geri dönün ve daha fazla örnek ekleyin. Ardından eğitme ve yeniden test edin. 

5. Ardından Soyadı adı içinde bir utterance seçip çalışan varlık listeden örnek konuşma varlıklarda çalışan varlığı ile işaretleyin.

    ![Konuşma ile çalışan entity işaretlenmiş MoveAssetsOrPeople içinde ekran görüntüsü](./media/luis-tutorial-pattern/intent-moveasserts-employee.png)

6. Metni seçin `portland` utterance içinde `move travis hinton from portland to orlando`. Açılan iletişim kutusunda, yeni varlık adını `Location`seçip **yeni varlık Oluştur**. Seçin **basit** varlık türü **Bitti**.

    ![Yeni bir konum varlık oluşturma işleminin ekran görüntüsü](./media/luis-tutorial-pattern/create-new-location-entity.png)

    Konuşma konum adlarını rest işaretleyin. 

    ![İşaretlenmiş tüm varlıkların ekran görüntüsü](./media/luis-tutorial-pattern/moveasset-all-entities-labeled.png)

    Word seçim ve sipariş desenini, önceki görüntüde açıktır. Varsa, **değil** düzenleri kullanarak ve size kullanıyor desenleri anlarsınız belirgin bir desen, konuşma amacı üzerinde sahip. 

    Çok çeşitli konuşma, bir desen yerine bekliyorsanız bu yanlış örnek konuşma olacaktır. Bu durumda, terim veya word seçim, utterance uzunluğu ve varlık yerleştirme çok çeşitlilik gösteren konuşma istersiniz. 

<!--TBD: what guidance to move from hier entities to patterns with roles -->
<!--    The [Hierarchical entity quickstart](luis-quickstart-intent-and-hier-entity.md) uses the  same idea of location but uses child entities to find origin and destination locations. 
-->
### <a name="add-role-to-location-entity"></a>Konum varlığa rolü ekleme 
Roller yalnızca desenleri için kullanılabilir. Kaynak ve hedef rolleri konumu varlığa ekleyin. 

1. Seçin **varlıkları** sol gezinti bölmesinde, ardından **konumu** varlıkların listesinden.

2. Ekleme `Origin` ve `Destination` varlığa rolleri.

    ![Yeni varlık rolleri ile ekran görüntüsü](./media/luis-tutorial-pattern/location-entity.png)

    Roller üzerinde hedefi konuşma mevcut olduğundan MoveAssetsOrPeople hedefi sayfada rolleri işaretlenmedi. Bunlar yalnızca deseni şablon konuşma üzerinde mevcut. 

### <a name="add-template-utterances-that-uses-location-and-destination-roles"></a>Konum ve hedef rolleri kullanan şablon Konuşma ekleme
Yeni varlık kullanan bir şablonu konuşma ekleyin.

1. Seçin **desenleri** sol gezinti bölmesinden.

2. Seçin **MoveAssetsOrPeople** hedefi.

3. Yeni bir varlık kullanarak yeni bir şablon utterance girin `Move {Employee} from {Location:Origin} to {Location:Destination}`. Bir varlık veya şablon utterance içinde rol için söz dizimi `{entity:role}`.

    ![Yeni varlık rolleri ile ekran görüntüsü](./media/luis-tutorial-pattern/pattern-moveassets.png)

4. Uygulama için yeni amacı, varlık ve düzeni eğitin.

### <a name="test-the-new-pattern-for-role-data-extraction"></a>Yeni rol veri ayıklama desenini test
Yeni desen, bir test ile doğrulayın.

1. Seçin **Test** üst panelinde. 
2. Utterance girin `Move Tammi Carlson from Bellingham to Winthrop`.
3. Seçin **inceleyin** varlık ve amacı test sonuçlarını görmek için sonuç altında.

    ![Yeni varlık rolleri ile ekran görüntüsü](./media/luis-tutorial-pattern/test-with-roles.png)

    Desen bulunursa, amacını belirten sonra varlıkları ilk olarak bulunur. Bir test sonucu burada varlıkları algılanmadı ve bu nedenle düzeni bulunamadı'iniz varsa, daha fazla örnek konuşma amaca (deseni değil) eklemeniz gerekir. 

4. Seçerek test paneli kapatmak **Test** üst gezinti düğmesi.

## <a name="use-a-patternany-entity-to-find-free-form-entities-in-a-pattern"></a>Pattern.any varlık içindeki bir desenle serbest biçimli varlıkları bulmak için kullanın
Bu İnsanKaynakları uygulama, çalışanların şirket formları da yardımcı olur. Formları birçoğu, uzunluğu değişen başlıklar sahiptir. Değişken uzunluğu LUIS form adı sona ereceği hakkında karışıklığa neden olabilir ifadeleri içerir. Kullanarak bir **Pattern.any** desenindeki varlık LUIS form adı doğru ayıklar. Bu nedenle başlangıcını ve bitişini form adını belirtmenize olanak verir. 

### <a name="create-a-new-intent-for-the-form"></a>Form için yeni bir hedefi oluşturma
Formlar için aradığınız konuşma için yeni bir hedefi oluşturun.

1. Seçin **hedefleri** sol gezinti bölmesinden.

2. **Create new intent** (Yeni amaç oluştur) öğesini seçin.

3. Yeni amacı ad `FindForm`.

4. Bir örnek utterance ekleyin.

    ```
    `Where is the form What to do when a fire breaks out in the Lab and who needs to sign it after I read it?`
    ```

    ![Yeni varlık rolleri ile ekran görüntüsü](./media/luis-tutorial-pattern/intent-findform.png)

    Form başlığı `What to do when a fire breaks out in the Lab`. Utterance formun konumunu isteyen ve ayrıca dosyayı okuma çalışan doğrulama imzalamalarını gerek duyan isteyen. Pattern.any varlık, burada form başlığı sona erer ve bir utterance varlık olarak form başlığı ayıklamak anlaşılması zor olacaktır.

### <a name="create-a-patternany-entity-for-the-form-title"></a>Form başlığı için bir Pattern.any varlık oluşturma
Pattern.any varlık için varlıklar farklı uzunluktaki sağlar. Desen Başlangıç ve bitiş varlık işaretlendikleri yalnızca bir düzende çalışır. Bir Pattern.any içerdiğinde, deseni bulabilirsiniz, ayıklar varlıkları yanlış kullanmak bir [açık listesi](luis-concept-patterns.md#explicit-lists) bu sorunu düzeltmek için. 

1. Seçin **varlıkları** sol gezinti bölmesinde.

2. **Create new entity** (Yeni varlık oluştur) öğesini seçin. 

3. Varlık adı `FormName` türüyle **Pattern.any**. Belirli Bu öğretici için varlığa herhangi bir rol eklemek gerekmez.

    ![Varlık adı ve varlık türü için iletişim kutusunun görüntüsü](./media/luis-tutorial-pattern/create-entity-pattern-any.png)

### <a name="add-a-pattern-that-uses-the-patternany"></a>Pattern.any kullanan bir desen Ekle

1. Seçin **desenleri** sol gezinti bölmesinden.

2. Seçin **FindForm** hedefi.

3. Yeni varlık kullanan bir şablonu utterance girin `Where is the form {FormName} and who needs to sign it after I read it?`

    ![Şablon utterance pattern.any varlık kullanarak ekran görüntüsü](./media/luis-tutorial-pattern/pattern.any-template-utterance.png)

4. Uygulama için yeni amacı, varlık ve düzeni eğitin.

### <a name="test-the-new-pattern-for-free-form-data-extraction"></a>Serbest biçimli veri ayıklama yeni desenini test
1. Seçin **Test** test panelini açmak için üst çubuğunda. 

2. Utterance girin `Where is the form Understand your responsibilities as a member of the community and who needs to sign it after I read it?`.

3. Seçin **inceleyin** varlık ve amacı test sonuçlarını görmek için sonuç altında.

    ![Şablon utterance pattern.any varlık kullanarak ekran görüntüsü](./media/luis-tutorial-pattern/test-pattern.any-results.png)

    Desen bulunursa, amacını belirten sonra varlık ilk olarak bulunur. Bir test sonucu burada varlıkları algılanmadı ve bu nedenle düzeni bulunamadı'iniz varsa, daha fazla örnek konuşma amaca (deseni değil) eklemeniz gerekir.

4. Seçerek test paneli kapatmak **Test** üst gezinti düğmesi.

## <a name="clean-up-resources"></a>Kaynakları temizleme
İhtiyacınız kalmadıysa LUIS uygulamasını silebilirsiniz. Bunu yapmak için üç noktayı seçin (***...*** ) sağında bulunan uygulama listesinde uygulama adı, seçin **Sil**. Açılan **Delete app?** (Uygulama silinsin mi?) iletişim kutusunda **Ok** (Tamam) öğesini seçin.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [LUIS uygulamaları için en iyi uygulamaları öğrenin](luis-concept-best-practices.md)