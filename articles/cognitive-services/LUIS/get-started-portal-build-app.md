---
title: 'Hızlı Başlangıç: LUIS portalda yeni bir uygulama oluşturma'
titleSuffix: Language Understanding - Azure Cognitive Services
description: Bu hızlı başlangıçta, LUIS Portalı'nda yeni bir uygulama oluşturun. Uygulama, amaçlarını ve varlıkların temel parçaları oluşturun. Uygulamayı test etmek için tahmin edilen amacını almak için etkileşimli test panelinde bir örnek kullanıcı utterance sağlayarak. Bir uygulama oluşturmak ücretsizdir; Bu, bir Azure aboneliği gerektirmez.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: quickstart
ms.date: 03/11/2019
ms.author: diberry
ms.openlocfilehash: 5ec38c510fedee7f489b3b100285eeb7c75f4561
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64690981"
---
# <a name="quickstart-create-a-new-app-in-the-luis-portal"></a>Hızlı Başlangıç: LUIS portalda yeni bir uygulama oluşturma

Bu hızlı başlangıçta, yeni bir uygulama oluşturun [LUIS portalı](https://www.luis.ai). İlk olarak, bir uygulamanın temel parçaları oluşturun **hedefleri**, ve **varlıkları**. Ardından örnek kullanıcı utterance tahmin edilen hedefi almak için etkileşimli test panelinde sağlayarak uygulama sınayın.

Bir uygulama oluşturmak ücretsizdir ve bir Azure aboneliği gerektirmez. Uygulamanızı dağıtmaya hazır olduğunuzda bkz [uygulama dağıtmak için Hızlı Başlangıç](get-started-portal-deploy-app.md). Bir Azure Bilişsel Hizmet kaynağı oluşturun ve uygulamaya atama gösterir.

## <a name="create-an-app"></a>Uygulama oluşturma

1. Açık [LUIS portalı](https://www.luis.ai) tarayıcı ve oturum açın. İlk kez oturum açarken ise, bir ücretsiz LUIS portal kullanıcı hesabı oluşturmanız gerekir.

1. Seçin **yeni uygulama oluştur** bağlam araç çubuğundan.

   [![LUIS portalında yeni bir uygulama oluşturun](./media/get-started-portal-build-app/create-app-in-portal.png)](./media/get-started-portal-build-app/create-app-in-portal.png#lightbox)

1. Açılır pencerede aşağıdaki ayarlarla uygulamayı yapılandırın ve ardından **Bitti**.

   |Ayar adı| Değer | Amaç|
   |--|--|--|
   |Ad|`myEnglishApp`|Benzersiz LUIS uygulaması adı<br>gerekli|
   |Kültür|**İngilizce**|Kullanıcıların, konuşma, dil **en-us**<br>gerekli|
   |Açıklama|`App made with LUIS Portal`|Uygulama açıklaması<br>isteğe bağlı|
   | | | |

   ![Yeni uygulama ayarları girin](./media/get-started-portal-build-app/create-new-app-settings.png)

## <a name="create-intents"></a>Amaç oluşturma

LUIS uygulaması oluşturduktan sonra hedefleri oluşturmanız gerekir. Amacı, kullanıcıların metin kategorilere ayırmak için bir yoludur. Örneğin, İnsan Kaynakları uygulamasında iki işlev de sahip olabilir. Kişilerin yardımcı olmak için:

 1. Bulma ve işler için geçerlidir
 1. Formları için işleri uygulamak için

Uygulamayı iki farklı kullanıcının _amaçları_ hizalamak için aşağıdaki hedefleri:

|Amaç|Örnek metin kullanıcıdan<br>bilinen bir _utterance_|
|--|--|
|ApplyForJob|`I want to apply for the new software engineering position in Cairo.`|
|FindForm|`Where is the job transfer form hrf-123456?`|

Intents oluşturmak için aşağıdaki adımları uygulayın:

1. Uygulama oluşturulduktan sonra olan **hedefleri** sayfasının **derleme** bölümü. **Create new intent** (Yeni amaç oluştur) öğesini seçin.

   [![Yeni Oluştur hedefi düğmesini seçin](./media/get-started-portal-build-app/create-new-intent-button.png)](./media/get-started-portal-build-app/create-new-intent-button.png#lightbox)

1. Hedefi bir ad girin `FindForm`ve ardından **Bitti**.

   ![FindForm hedefi adını girin](./media/get-started-portal-build-app/create-new-intent-dialog.png)

## <a name="add-an-example-utterance"></a>Bir örnek utterance Ekle

Intents oluşturduktan sonra örnek Konuşma ekleme. Örnek konuşma bir sohbet Robotu veya başka bir istemci uygulama, kullanıcının girdiği metinlerdir. Bunlar, kullanıcının metin amacınıza LUIS ıntent'e eşleyin.

Bu örnek uygulama için `FindForm` hedefi, örnek konuşma form numarası da içerecektir. İstemci uygulamasının utterance içerecek şekilde önemli olduğu için kullanıcının isteği gerçekleştirmek için form sayısının gerekir.

Aşağıdaki örnek 15 konuşma için ekleme `FindForm` hedefi.

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

Tasarım gereği, bu örnek konuşma aşağıdaki yollarla farklılık gösterir:

* utterance uzunluğu
* Noktalama işaretleri
* sözcük seçimi
* Fiili şimdiki (ise, oluştu, olacaktır)
* sözcük sırasını

[![FindForm amaç için örnek konuşma girin](./media/get-started-portal-build-app/add-example-utterance.png)](./media/get-started-portal-build-app/add-example-utterance.png#lightbox)

## <a name="create-a-regular-expression-entity"></a>Bir normal ifade varlık oluşturma

Döndürülen çalışma zamanı tahmin yanıtın bir parçası form numarasını almak için formun bir varlık olarak işaretlenmesi gerekir. Form numarası metni ileri düzeyde yapılandırılmış olduğundan, bir normal ifade varlık kullanmayı işaretlenebilir. Varlık, aşağıdaki adımlarla oluşturun:

1. Seçin **varlıkları** sol taraftaki menüden.

1. Seçin **yeni varlık Oluştur** üzerinde **varlıkları** sayfası.

1. Bir ad girin `Human Resources Form Number`seçin **Regex** varlık yazın ve normal ifade girin `hrf-[0-9]{6}`. Bu giriş sabit karakterleriyle eşleşen `hrf-`ve tam olarak 6 basamak sağlar.

   ![Normal ifade varlık için varlık bilgilerini girin](./media/get-started-portal-build-app/create-regular-expression-entity.png)

1. **Done** (Bitti) öğesini seçin.

## <a name="add-example-utterances-to-the-none-intent"></a>Örnek konuşma hiçbiri hedefi ekleme

**Hiçbiri** hedefi temel amacı ve boş bırakılması gerekir. Bu amaç için uygulama başka bir ıntents için eklediğiniz her 10 örnek konuşma bir utterance içermelidir.

**Hiçbiri** amaç'ın örnek konuşma dışında istemci uygulama etki alanında olmalıdır.

1. Seçin **hedefleri** sol menüsünü ve ardından **hiçbiri** hedefleri listesinde.

1. Aşağıdaki örnek konuşma ıntent'e ekleyin:

   |Hiçbiri hedefi örnek konuşma|
   |--|
   |Barking dogs are annoying (Havlayan köpekler rahatsız eder)|
   |Order a pizza for me (Bana bir pizza söyle)|
   |Penguins in the ocean (Okyanustaki penguenler)|

   Bu İnsan Kaynakları uygulaması için örnek konuşma bu etki alanının dışında ' dir. İnsan Kaynakları etki alanınızı içeren hayvanlar, Yemek veya Okyanusu sonra farklı bir örnek konuşma için kullanması gereken **hiçbiri** hedefi.

## <a name="train-the-app"></a>Uygulamayı eğitme

Sağ üstteki menüde **eğitme** amacı ve varlık uygulamak için uygulamanın geçerli sürüme modeli değiştirir.

## <a name="look-at-the-regular-expression-entity-in-the-example-utterances"></a>Örnek konuşma normal ifade varlıkta bakın

1. Varlık içinde bulunan doğrulayın **FindForm** seçerek hedefi **hedefleri** sol menüden. Ardından **FindForm** hedefi.

   Varlık örnek konuşma göründüğü işaretlenir. Varlık adı yerine orijinal metni görmek isterseniz değiştirme **varlıkları görünümü** araç çubuğundan.

   [![Varlıklar ile işaretlenmiş tüm örnek konuşma](./media/get-started-portal-build-app/all-example-utterances-marked-with-entities.png)](./media/get-started-portal-build-app/all-example-utterances-marked-with-entities.png#lightbox)

## <a name="test-your-new-app-with-the-interactive-test-pane"></a>Yeni uygulamanızı etkileşimli test Bölmesi ile test etme

Etkileşimli kullanın **Test** varlık uygulama henüz görülen henüz yeni konuşma ayıklanır doğrulamak için LUIS portalındaki bölmesinde.

1. Seçin **Test** sağ üst menüsünde.

1. Yeni bir utterance ekleyin ve ardından Enter tuşuna basın:

   ```Is there a form named hrf-234098```

   ![Test Bölmesi'nde yeni utterance test](./media/get-started-portal-build-app/test-new-utterance.png)

   Üst tahmin hedefi olan doğru **FindForm** % 90'ile güvenle (0.977). **İnsan Kaynakları Form numarası** varlık hrf 234098 değeriyle ayıklanır.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu Hızlı Başlangıç ile işiniz bittiğinde ve bir sonraki hızlı açın taşıma olmayan bittiğinde **uygulamalarım** üst gezinti menüsünde. Sonra liste ve select uygulamanın sol onay kutusunu seçin **Sil** listesinin üst bağlam araç çubuğundan.

[![Uygulamalar listemde uygulamayı Sil](./media/get-started-portal-build-app/delete-app.png)](./media/get-started-portal-build-app/delete-app.png#lightbox)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [2. Uygulama dağıtma](get-started-portal-deploy-app.md)
