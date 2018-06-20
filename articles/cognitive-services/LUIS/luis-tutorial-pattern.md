---
title: HALUK tahminleri - Azure artırmak için desenleri kullanma Öğreticisi | Microsoft Docs
titleSuffix: Azure
description: Bu öğreticide, düzeni amaçlar için HALUK amacını ve varlık tahminleri geliştirmek için kullanın.
services: cognitive-services
author: v-geberr
manager: kamran.iqbal
ms.service: cognitive-services
ms.technology: luis
ms.topic: article
ms.date: 05/07/2018
ms.author: v-geberr;
ms.openlocfilehash: ff5572366be548132b28e5ce03b9595e7f98128c
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36265325"
---
# <a name="tutorial-use-patterns-to-improve-predictions"></a>Öğretici: modelleri tahminleri geliştirmek için kullanın.

Bu öğreticide, desenleri amacını ve varlık tahmin artırmak için kullanın.  

> [!div class="checklist"]
* Bir desen uygulamanıza yardımcı olacak olduğunu belirleme
* Bir desen oluşturma 
* Önceden oluşturulmuş ve özel varlıkları bir desen kullanma 
* Desen tahmin geliştirmeleri doğrulama hakkında
* Bağlam tabanlı varlıkları bulmak için bir varlık için bir rol ekleme
* Serbest biçimli varlıkları bulmak için bir Pattern.any ekleme

Bu makalede, ücretsiz bir gereksinim duyduğunuz [HALUK] [ LUIS] HALUK uygulamanızı yazmak için hesap.

## <a name="import-humanresources-app"></a>İçeri aktarma İnsanKaynakları uygulama
Bu öğretici İnsanKaynakları uygulama alır. Uygulama üç amacı vardır: None, GetEmployeeOrgChart, GetEmployeeBenefits. İki varlık uygulama vardır: önceden oluşturulmuş numarası ve çalışan. Çalışan varlık, bir çalışanın adı ayıklamak için basit bir varlıktır. 

1. Yeni bir HALUK uygulama dosyası oluşturun ve adlandırın `HumanResources.json`. 

2. Aşağıdaki uygulama tanımını dosyaya kopyalayın:

   [!code-json[Add the LUIS model](~/samples-luis/documentation-samples/tutorial-patterns/HumanResources.json?range=1-164 "Add the LUIS model")]

3. HALUK üzerinde **uygulamaları** sayfasında, **alma yeni uygulama**. 

4. İçinde **alma yeni uygulama** iletişim kutusunda `HumanResources.json` 1. adımda oluşturduğunuz dosyası.

5. Seçin **GetEmployeeOrgChart** amacı, ardından değişikliği **varlıklar Görünüm** için **belirteçleri görüntüle**. Birkaç örnek utterances listelenir. Her utterance çalışan varlık bir ad içerir. Her ad farklıdır ve ifade düzenlenmesi için her utterance farklı olduğuna dikkat edin. Bu seviyelerine utterances çeşitli bilgi HALUK yardımcı olur.

    ![Ekran görüntüsü, hedefi sayfasıyla yükseğe varlıklar görünümü](media/luis-tutorial-pattern/utterances-token-view.png)

6. Seçin **eğitmek** uygulama eğitmek için üst gezinti çubuğunda. Yeşil başarı çubuğu bekleyin.

7. Seçin **Test** üst panelinde. Girin `Who does Patti Owens report to?` seçin ENTER. Seçin **incele** test hakkında daha fazla bilgi için utterance altında.
    
    Çalışan adı, Patti Owens henüz bir örnek utterance kullanılan değil. Ne kadar iyi HALUK bu utterance için öğrenilen görmek için bir test budur `GetEmployeeOrgChart` amacını ve çalışan varlığı olmalıdır `Patti Owens`. Sonuç % 50'altında olmalıdır (. 50) için `GetEmployeeOrgChart` hedefi. Hedefi doğru olsa da, puanı düşük olur. Çalışan varlık da doğru olarak tanımlanan `Patti Owens`. Bu ilk tahmin puanı desenleri artırın. 

    ![Ekran görüntüsü, Test paneli](media/luis-tutorial-pattern/original-test.png)

8. Test Masası'nı seçerek kapatın **Test** üst gezinti düğmesini. 

## <a name="patterns-teach-luis-common-utterances-with-fewer-examples"></a>Desenler HALUK daha az örnekleri içeren ortak utterances öğretir.
İnsan Kaynakları etki alanı yapısı nedeniyle, kuruluşta çalışan ilişkiler hakkında isteyen birkaç ortak yolu vardır. Örneğin:

```
Who does Mike Jones report to?
Who reports to Mike Jones? 
```

Bu utterances çok kapatmak için her birçok utterance örnekler sağlamadan bağlamsal benzersizliğini belirler. Bir hedefi için bir örüntü ekleyerek, birçok utterance örnekler girmeden amacına için ortak utterance desenler HALUK öğrenir. 

Örnek şablon utterances hedefi arasında şunlar bulunur:

```
Who does {Employee} report to?
Who reports to {Employee}? 
```

Desen normal ifadeyle eşleşen ve makine öğrenme birleşimidir. Ardından, bazı şablonu düzeni öğrenmek HALUK utterance örnekleri sağlayın. Hedefi utterances yanı sıra bu örnekler HALUK amacı hangi utterances uygun anlaşılmasını verin ve, utterance içinde varlıkla bulunduğu. <!--A pattern is specific to an intent. You can't duplicate the same pattern on another intent. That would confuse LUIS, which lowers the prediction score. -->

## <a name="add-the-template-utterances"></a>Şablon utterances Ekle

1. Sol gezinti bölmesinde altında **uygulama performansı**seçin **desenleri** sol gezinti gelen.

2. Seçin **GetEmployeeOrgChart** amacı, ardından aşağıdaki şablonu utterances, bir kerede seçerek girin, sonra her şablon utterance:

    ```
    Does {Employee} have {number} subordinates?
    Does {Employee} have {number} direct reports?
    Who does {Employee} report to?
    Who reports to {Employee}?
    Who is {Employee}'s manager?
    Who are {Employee}'s subordinates?
    ```

    `{Employee}` Sözdizimi hangi varlık olarak iyi olduğu olarak şablon utterance varlık konumdaki işaretler. 

    ![Şablon utterances amacı için girme ekran görüntüsü](./media/luis-tutorial-pattern/enter-pattern.png)

3. Seçin **tren** üst gezinti çubuğunda. Yeşil başarı çubuğu bekleyin.

4. Seçin **Test** üst panelinde. Girin `Who does Patti Owens report to?` metin kutusuna. Select girin. Önceki bölümde test aynı utterance budur. Sonuç daha yüksek olmalıdır `GetEmployeeOrgChart` hedefi. 

    Puan daha iyi sunulmuştur. HALUK birçok örnek sağlamadan hedefi ilgili düzeni öğrendiniz.

    ![Yüksek puan paneliyle ekran görüntüsü, Test sonucu](./media/luis-tutorial-pattern/high-score.png)

    Amaç belirten düzeni bulunur sonra varlık ilk olarak, bulunamadı. Burada varlık algılanmadı ve bu nedenle düzeni bulunamadı test sonucu varsa, daha fazla örnek utterances amacına (desen değil) eklemeniz gerekir. 

5. Test Masası'nı seçerek kapatın **Test** üst gezinti düğmesini.

## <a name="use-an-entity-with-a-role-in-a-pattern"></a>Bir varlık bir rolde bir desen ile kullanma
HALUK uygulama çalışanlar bir konumdan diğerine taşımanıza yardımcı olmak için kullanılır. Bir örnek utterance olan `Move Bob Jones from Seattle to Los Colinas`. Her iki yerde de utterance farklı bir anlamı yoktur. Seattle kaynak konumu ve Los Colinas taşımak için hedef konumu. İki rolleriyle konumu için basit bir varlık oluşturduğunuz düzende aşağıdaki bölümlerde bu konumlardaki birbirinden ayırmak için: kaynak ve hedef. 

### <a name="create-a-new-intent-for-moving-people-and-assets"></a>Kişiler ve varlıklar taşımak için yeni bir amacı oluşturma
Yeni bir taşıma kişiler veya varlıklar hakkında utterances hedefini oluşturun.

1. Seçin **hedefleri** sol gezinti gelen
2. Seçin **yeni amacı oluşturma**
3. Ad yeni hedefi `MoveAssetsOrPeople`
4. Örnek utterances ekleyin:

    ```
    Move Bob Jones from Seattle to Los Colinas
    Move Dave Cooper from Redmond to Seattle
    Move Jim Smith from Toronto to Vancouver
    Move Jill Benson from Boston to London
    Move Travis Hinton from Portland to Orlando
    ```
    ![Örnek utterance MoveAssetsOrPeople amacı için ekran görüntüsü](./media/luis-tutorial-pattern/intent-moveasserts-example-utt.png)

    Örnek utterances amacı yeterli örnekler vermektir. Test konumu varlık algılanan değil ve sonuç olarak düzeni algılandı değil varsa, bu adıma geri dönün ve daha fazla örnek ekleyin. Ardından eğitme ve yeniden sınayın. 

5. Bir utterance adınızı sonra son seçtikten sonra çalışan varlık listeden seçim tarafından çalışan varlık örnek utterances içinde varlıkla işaretleyin.

    ![Çalışan varlık ile işaretlenmiş MoveAssetsOrPeople utterances ekran görüntüsü](./media/luis-tutorial-pattern/intent-moveasserts-employee.png)

6. Metni seçin `portland` utterance içinde `move travis hinton from portland to orlando`. Açılan iletişim kutusunda, yeni varlık adı girin `Location`seçip **yeni varlık oluşturma**. Seçin **basit** varlık türü ve select **Bitti**.

    ![Yeni bir konum varlık oluşturma ekran görüntüsü](./media/luis-tutorial-pattern/create-new-location-entity.png)

    Utterances konumu adlarında kalan işaretleyin. 

    ![İşaretlenen tüm varlıkların ekran görüntüsü](./media/luis-tutorial-pattern/moveasset-all-entities-labeled.png)

    Word seçim ve sipariş desenini önceki görüntüde açıktır. Varsa, **değil** düzenleri kullanarak ve size kullanıyor desenleri anlarsınız belirgin bir desen, hedefi üzerinde utterances sahip. 

    Çok çeşitli bir desen yerine utterances bekliyorsanız, bu yanlış örnek utterances olacaktır. Bu durumda, yaygın olarak değişen utterances terim veya word seçim, utterance uzunluğu ve varlık yerleştirme istersiniz. 

<!--TBD: what guidance to move from hier entities to patterns with roles -->
<!--    The [Hierarchical entity quickstart](luis-quickstart-intent-and-hier-entity.md) uses the  same idea of location but uses child entities to find origin and destination locations. 
-->
### <a name="add-role-to-location-entity"></a>Konum varlığa Rol Ekle 
Roller yalnızca desenler için kullanılabilir. Kaynak ve hedef rollerini konumu varlığa ekleyin. 

1. Seçin **varlıklar** ve sol gezinti bölmesindeki, ardından **konumu** varlıkların listesinden.

2. Ekleme `Origin` ve `Destination` varlık rollere.

    ![Yeni bir varlık rolleriyle ekran görüntüsü](./media/luis-tutorial-pattern/location-entity.png)

    Roller üzerinde hedefi utterances mevcut değil çünkü rolleri MoveAssetsOrPeople hedefi sayfasında işaretlenmez. Bunlar yalnızca desen şablon utterances üzerinde mevcut. 

### <a name="add-template-utterances-that-uses-location-and-destination-roles"></a>Konum ve hedef rollerini kullanır şablonu utterances Ekle
Yeni varlığı kullanması şablonu utterances ekleyin.

1. Seçin **desenleri** sol gezinti gelen.

2. Seçin **MoveAssetsOrPeople** hedefi.

3. Yeni varlık kullanarak yeni bir şablon utterance girin `Move {Employee} from {Location:Origin} to {Location:Destination}`. Bir varlık ve şablonu utterance içinde rol için söz dizimi `{entity:role}`.

    ![Yeni bir varlık rolleriyle ekran görüntüsü](./media/luis-tutorial-pattern/pattern-moveassets.png)

4. Uygulama için yeni amacı, varlık ve düzeni eğitmek.

### <a name="test-the-new-pattern-for-role-data-extraction"></a>Yeni düzeni rol veri ayıklama için test etme
Yeni desen ile bir test doğrulayın.

1. Seçin **Test** üst panelinde. 
2. Utterance girin `Move Tammi Carlson from Bellingham to Winthrop`.
3. Seçin **incele** varlık ve amacı için test sonuçları görmek için sonuca altında.

    ![Yeni bir varlık rolleriyle ekran görüntüsü](./media/luis-tutorial-pattern/test-with-roles.png)

    Amaç belirten düzeni bulunur sonra varlıkları ilk olarak, bulunamadı. Burada varlıkları algılanmadı ve bu nedenle düzeni bulunamadı test sonucu varsa, daha fazla örnek utterances amacına (desen değil) eklemeniz gerekir. 

4. Test Masası'nı seçerek kapatın **Test** üst gezinti düğmesini.

## <a name="use-a-patternany-entity-to-find-free-form-entities-in-a-pattern"></a>İçindeki bir desenle serbest biçimli varlıkları bulmak için bir Pattern.any varlık kullanın
Bu İnsanKaynakları uygulama çalışanların şirket formları da yardımcı olur. Formları çoğunun uzunluğu değişen başlıkları vardır. Değişken uzunlukta HALUK form adı bittiği hakkında karışıklığa neden olabilir tümcecikleri içerir. Kullanarak bir **Pattern.any** varlık desende HALUK form adı doğru ayıklar şekilde başlangıcını ve bitişini form adını belirtmenize olanak verir. 

### <a name="create-a-new-intent-for-the-form"></a>Form için yeni bir amacı oluşturma
Formlar için aradığınız utterances için yeni bir hedefi oluşturun.

1. Seçin **hedefleri** sol gezinti gelen.

2. Seçin **yeni amacı oluşturma**.

3. Yeni hedefi adı `FindForm`.

4. Bir örnek utterance ekleyin.

    ```
    `Where is the form What to do when a fire breaks out in the Lab and who needs to sign it after I read it?`
    ```

    ![Yeni bir varlık rolleriyle ekran görüntüsü](./media/luis-tutorial-pattern/intent-findform.png)

    Form başlık `What to do when a fire breaks out in the Lab`. Utterance formun konumunu soran ve ayrıca bu dosyayı okuma çalışan doğrulama imzalamak gerek duyan isteyen. Pattern.any varlık, burada formu başlığı sona erer ve utterance bir varlık olarak form başlığı ayıklamak anlamak zor olurdu.

### <a name="create-a-patternany-entity-for-the-form-title"></a>Form başlık Pattern.any varlık oluştur
Pattern.any varlık değişen uzunluğu varlıklar için sağlar. Desen başlangıcını ve bitişini varlık işaretler olduğundan yalnızca bir düzende çalışır. Bir Pattern.any içerdiğinde, düzeni görürseniz, ayıklar varlıklar yanlış kullanan bir [açık listesi](luis-concept-patterns.md#explicit-lists) bu sorunu düzeltmek için. 

1. Seçin **varlıklar** sol gezinti bölmesinde.

2. Seçin **yeni varlık oluşturma**. 

3. Varlık adı `FormName` türüyle **Pattern.any**. Belirli Bu öğretici için herhangi bir rol için varlık eklemek gerekmez.

    ![Varlık adı ve varlık türü için iletişim kutusunun görüntüsü](./media/luis-tutorial-pattern/create-entity-pattern-any.png)

### <a name="add-a-pattern-that-uses-the-patternany"></a>Pattern.any kullanan bir deseni ekleme

1. Seçin **desenleri** sol gezinti gelen.

2. Seçin **FindForm** hedefi.

3. Yeni varlık kullanarak bir şablon utterance girin `Where is the form {FormName} and who needs to sign it after I read it?`

    ![Pattern.any varlık kullanarak şablonu utterance ekran görüntüsü](./media/luis-tutorial-pattern/pattern.any-template-utterance.png)

4. Uygulama için yeni amacı, varlık ve düzeni eğitmek.

### <a name="test-the-new-pattern-for-free-form-data-extraction"></a>Yeni düzeni serbest biçimli veri ayıklama için test etme
1. Seçin **Test** test panelini açmak için üst çubuğundan. 

2. Utterance girin `Where is the form Understand your responsibilities as a member of the community and who needs to sign it after I read it?`.

3. Seçin **incele** varlık ve amacı için test sonuçları görmek için sonuca altında.

    ![Pattern.any varlık kullanarak şablonu utterance ekran görüntüsü](./media/luis-tutorial-pattern/test-pattern.any-results.png)

    Amaç belirten düzeni bulunur sonra varlık ilk olarak, bulunamadı. Burada varlıkları algılanmadı ve bu nedenle düzeni bulunamadı test sonucu varsa, daha fazla örnek utterances amacına (desen değil) eklemeniz gerekir.

4. Test Masası'nı seçerek kapatın **Test** üst gezinti düğmesini.

## <a name="clean-up-resources"></a>Kaynakları temizleme
Artık gerektiğinde HALUK uygulamayı silin. Bunu yapmak için uygulama adının sağ tarafındaki üç nokta menüsüne (...) uygulama listesinde seçin **silmek**. Açılan iletişim kutusunda **Delete uygulama?** seçin **Tamam**.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Tümcecik listesi tahmin geliştirmek için kullanın](luis-tutorial-interchangeable-phrase-list.md)

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions