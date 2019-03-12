---
title: 'Hızlı Başlangıç: LUIS portalı ile yeni bir uygulama oluşturma'
titleSuffix: Language Understanding - Azure Cognitive Services
description: Bu hızlı başlangıçta, LUIS Portalı'nda yeni bir uygulama oluşturun. Uygulama, amaçlarını ve varlıkların temel parçaları oluşturmak ve test etmek için tahmin edilen amacını almak için etkileşimli test panelinde bir örnek kullanıcı utterance sağlayarak. Bir uygulama oluşturmak ücretsizdir; Bu, bir Azure aboneliği gerektirmez.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: quickstart
ms.date: 03/11/2019
ms.author: diberry
ms.openlocfilehash: e97be28261d28c2a72e507adcdac0248691745c7
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2019
ms.locfileid: "57786870"
---
# <a name="quickstart-create-a-new-app-in-the-luis-portal"></a>Hızlı Başlangıç: LUIS portalda yeni bir uygulama oluşturma

Bu hızlı başlangıçta, yeni bir uygulama oluşturma [LUIS portalı](https://www.luis.ai). Bir uygulamanın temel parçaları oluşturmak **hedefleri** ve **varlıkları**, tahmin edilen hedefi almak için etkileşimli test panelinde bir örnek kullanıcı utterance sağlayarak sınayın.

Bir uygulama oluşturmak ücretsizdir; Bu, bir Azure aboneliği gerektirmez. Uygulamanızı dağıtmak hazır olduğunuzda, sonra Azure Bilişsel Hizmet kaynağı oluşturup uygulamaya atayın. Bu dağıtım işlemi bulunduğu [sonraki hızlı](get-started-portal-deploy-app.md).

## <a name="create-app"></a>Uygulama oluştur 

1. Açık [LUIS portalı](https://www.luis.ai) tarayıcı ve oturum açın. Bu, ilk kez oturum açarken ise, bir ücretsiz LUIS portal kullanıcı hesabı oluşturmanız gerekir.

1. Seçin **yeni uygulama oluştur** bağlam araç çubuğundan.

    [![LUIS portalında yeni bir uygulama oluşturun](./media/get-started-portal-build-app/create-app-in-portal.png)](./media/get-started-portal-build-app/create-app-in-portal.png#lightbox)

1. Açılır pencerede aşağıdaki ayarlarla uygulamayı yapılandırma ardından seçin **Bitti**.

    |Ayar adı| Değer | Amaç|
    |--|--|--|
    |Ad|`myEnglishApp`|Benzersiz LUIS uygulaması adı<br>gerekli|
    |Kültür|**İngilizce**|Kullanıcıların, konuşma, dil **en-us**<br>gerekli|
    |Açıklama|`App made with LUIS Portal`|Uygulama açıklaması<br>isteğe bağlı|

    ![Yeni uygulama ayarları girin](./media/get-started-portal-build-app/create-new-app-settings.png)


## <a name="create-intent"></a>Amaç oluşturma 

Bu uygulama oluşturulduktan sonra sonraki adıma ıntents oluşturmaktır. Amacı, kullanıcıların metin kategorilere ayırmak için bir yoludur. İki işleve olan bir insan kaynakları uygulaması varsa: ilk bulun ve projeler için geçerli kolaylaştıracak ve ikinci başvurmak için formları bulmak için işler, bu iki farklı _amaçları_ hizalamak için aşağıdaki hedefleri:

|Amaç|Örnek metin kullanıcıdan<br>bilinen bir _utterance_|
|--|--|
|ApplyForJob|`I want to apply for the new software engineering position in Cairo.`|
|FindForm|`Where is the job transfer form hrf-123456?`|

1. Uygulama oluşturulduktan sonra olan **hedefleri** sayfasının **derleme** bölümü. **Create new intent** (Yeni amaç oluştur) öğesini seçin. 

    [![Yeni Oluştur hedefi düğmesini seçin](./media/get-started-portal-build-app/create-new-intent-button.png)](./media/get-started-portal-build-app/create-new-intent-button.png#lightbox)

1. Hedefi bir ad girin `FindForm` seçip **Bitti**.

    ![FindForm hedefi adını girin](./media/get-started-portal-build-app/create-new-intent-dialog.png)

## <a name="add-example-utterance"></a>Örnek utterance Ekle 

Hedefi oluşturduktan sonra sonraki adıma örnek konuşma eklemektir. Sohbet Robotu veya kullanıcının metin amacınıza LUIS ıntent'e eşleşen başka bir istemci uygulama kullanıcı tarafından girilen metni, budur. 

Bu örnek uygulama için `FindForm` hedefi, örnek konuşma utterance içinde önemli bilgileri istemci uygulaması fullfil sırada kullanıcının isteği gerektiğini form sayısının da içerecektir. 

Aşağıdaki beş örnek konuşma için ekleme `FindForm` hedefi. 

|#|Örnek konuşmalar|
|--|--|
|1|İçin Hrf 123456 aranıyor|
|2|İnsan Kaynakları form hrf 234591 nerede?|
|3|hrf-345623 olduğu|
|4|Bana Gönder hrf 345794 mümkündür|
|5|Dahili bir iş için uygulanacak hrf 234695 ihtiyacım var?|
|6|Yöneticim hrf 234091 ile bir proje için uygulama bilmeniz gerekiyor mu|
|7|Hrf 234918 nereye gönderebilirim? Alındığı bir e-posta yanıtı alabilir miyim?|
|8|Hrf 234555|
|9|Hrf 234987 ne zaman güncelleştirildiği?|
|10|Form hrf-876345 için mühendislik konumlarına uygulamak için kullanabilirim|
|11|My açık isteği için gönderilen hrf 765234 yeni bir sürümü neydi?|
|12|Uluslararası işleri hrf 234234 kullanıyor musunuz?|
|13|Hrf 234598 yazım hatası|
|14|Yeni gereksinimleri Hrf 234567 düzenlenecek|
|15|Hrf 123456, hrf 123123 hrf 234567|

Bu örnek konuşma amaç, aşağıdaki yollarla değişir:

* utterance uzunluğu
* Noktalama işaretleri
* sözcük seçimi
* Fiili şimdiki (ise, oluştu, olacaktır)
* sözcük sırasını

[![FindForm amaç için örnek konuşma girin](./media/get-started-portal-build-app/add-example-utterance.png)](./media/get-started-portal-build-app/add-example-utterance.png#lightbox)

## <a name="create-regular-expression-entity"></a>Normal ifade varlık oluşturma 

Döndürülen çalışma zamanı tahmin yanıtın bir parçası form numarasını almak için formun bir varlık olarak işaretlenmesi gerekir. Form numarası metni ileri düzeyde yapılandırılmış olduğundan, bir normal ifade varlık kullanmayı işaretlenebilir. Aşağıdaki adımlarla varlık oluşturun. 

1. Seçin **varlıkları** sol gezinti menüsünde. 

1. Seçin **yeni varlık Oluştur** varlıkları sayfasında.

1. Bir ad girin `Human Resources Form Number`seçin **Regex** varlık yazın ve normal ifade girin `hrf-[0-9]{6}`. Bu sabit karakterleriyle eşleşen `hrf-`ve tam olarak 6 basamak sağlar. 

    ![Normal ifade varlık için varlık bilgilerini girin](./media/get-started-portal-build-app/create-regular-expression-entity.png)

1. **Done** (Bitti) öğesini seçin. 

## <a name="add-example-utterances-to-none-intent"></a>Hedefi hiçbiri için örnek Konuşma ekleme

**Hiçbiri** hedefi temel amacı ve boş bırakılması gerekir. Bu amaç amacı uygulamanın geri kalanında 1 utterance her 10 olması gerekir. 

**Hiçbiri** amaç'ın örnek konuşma dışında istemci uygulama etki alanında olmalıdır. 

1. Seçin **hedefleri** seçip sol menüden **hiçbiri** hedefleri listesinde.

1. Aşağıdaki örnek konuşma ıntent'e ekleyin:

    |Hiçbiri hedefi örnek konuşma|
    |--|
    |Barking dogs are annoying (Havlayan köpekler rahatsız eder)|
    |Order a pizza for me (Bana bir pizza söyle)|
    |Penguins in the ocean (Okyanustaki penguenler)|

    Bu İnsan Kaynakları uygulaması için örnek konuşma bu etki alanının dışında ' dir. İnsan Kaynakları etki alanınız, hayvanlar, Yemek veya Okyanusu gibi daha sonra bu örnek konuşma için kullanılmamalıdır, **hiçbiri** hedefi. 

## <a name="train-the-app"></a>Uygulamayı eğitme

Sağ üst gezinti bölmesinde **eğitme** amacı ve varlık uygulamak için uygulamanın geçerli sürüme modeli değiştirir. 

## <a name="look-at-the-regular-expression-entity-in-the-example-utterances"></a>Örnek konuşma normal ifade varlıkta bakın

1. Varlık içinde bulunan doğrulayın **FindForm** seçerek hedefi **hedefleri** seçip sol menüden **FindForm** hedefi. 

    Varlık örnek konuşma göründüğü işaretlenir. Varlık adı yerine orijinal metni görmek isterseniz değiştirme **varlıkları görünümü** araç çubuğundan.

    [![Varlıklar ile işaretlenmiş tüm örnek konuşma](./media/get-started-portal-build-app/all-example-utterances-marked-with-entities.png)](./media/get-started-portal-build-app/all-example-utterances-marked-with-entities.png#lightbox)

## <a name="test-your-new-app-with-the-interactive-test-pane"></a>Yeni uygulamanızı etkileşimli test Bölmesi ile test etme

Etkileşimli kullanın **Test** varlık doğrulamak için LUIS portalındaki bölmesinde, uygulama henüz görülen henüz yeni konuşma ayıklanır.

1. Seçin **Test** sağ üst menüsünde.

1. Yeni bir utterance ekleyin, sonra Enter tuşuna basın:

    ```Is there a form named hrf-234098```

    ![Test Bölmesi'nde yeni utterance test](./media/get-started-portal-build-app/test-new-utterance.png)

    Üst tahmin hedefi olan doğru **FindForm** % 90'ile güvenle (0.977) ve **İnsan Kaynakları Form numarası** varlık hrf 234098 değeriyle ayıklanır. 

## <a name="clean-up-resources"></a>Kaynakları temizleme
Açın sonraki hızlı hareket eden değil ve bu hızlı başlangıç ile işiniz bittiğinde **uygulamalarım** üst gezinti menüsünde. Ardından listeden uygulamanın sol taraftaki onay kutusunu seçin ve seçin **Sil** listesinin üst bağlam araç çubuğundan. 

[![Uygulamalar listemde uygulamayı Sil](./media/get-started-portal-build-app/delete-app.png)](./media/get-started-portal-build-app/delete-app.png#lightbox)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [2. Uygulama dağıtma](get-started-portal-deploy-app.md)