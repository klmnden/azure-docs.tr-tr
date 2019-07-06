---
title: "Öğretici: C# ' ta Speech SDK'sı kullanarak amaçlardan tutun konuşma tanıma"
titleSuffix: Azure Cognitive Services
description: Bu öğreticide C# için Konuşma SDK'sını kullanarak konuşmadaki amaçları tanımayı öğreneceksiniz.
services: cognitive-services
author: wolfma61
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: tutorial
ms.date: 07/05/2019
ms.author: wolfma
ms.openlocfilehash: d61141a0955f916b1d4bfeabb22454ec38415cea
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2019
ms.locfileid: "67603231"
---
# <a name="tutorial-recognize-intents-from-speech-using-the-speech-sdk-for-c"></a>Öğretici: Amaçlardan tutun Speech SDK'sı kullanarak konuşma tanımaC#

Bilişsel Hizmetler [Konuşma SDK’sı](~/articles/cognitive-services/speech-service/speech-sdk.md) **amaç tanımayı** sağlamak için [Language Understanding hizmetiyle (LUIS)](https://www.luis.ai/home) tümleştirilmiştir. Amaç, kullanıcının yapmak istediği herhangi bir şeydir: uçak rezervasyonu, hava durumuna bakma veya telefon etme. Kullanıcı kendisine hangi terim doğal geliyorsa onu kullanabilir. LUIS, makine öğrenimini kullanarak kullanıcı isteklerini tanımladığınız amaçlarla eşler.

> [!NOTE]
> LUIS uygulaması tanımak istediğiniz amaçları ve varlıkları tanımlar. Konuşma hizmetini kullanan C# uygulamasından ayrıdır. Bu makalede "app" LUIS uygulaması anlamına gelirken "uygulama" da C# kodu anlamına gelmektedir.

Bu öğreticide, cihazınızın mikrofonu aracılığıyla alınan kullanıcı konuşmalarından amaçları türeten C# konsol uygulamasını geliştirmek için Konuşma SDK’sı kullanırsınız. Şunları öğrenirsiniz:

> [!div class="checklist"]
> * Konuşma SDK'sı NuGet paketine başvuran bir Visual Studio projesi oluşturma
> * Konuşma yapılandırması oluşturma ve amaç tanıyıcısı alma
> * LUIS app’iniz için modeli alma ve size gereken amaçları ekleme
> * Konuşma tanıma için dil belirtme
> * Dosyadan konuşma tanıma
> * Zaman uyumsuz, olay odaklı sürekli tanıma kullanma

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiye başlamadan önce aşağıdakilere sahip olduğunuzdan emin olun.

* LUIS hesabı. [LUIS portalından](https://www.luis.ai/home) ücretsiz bir hesap alabilirsiniz.
* Visual Studio 2017 (herhangi bir sürümü).

## <a name="luis-and-speech"></a>LUIS ve konuşma

LUIS, amaçlardan tutun Konuşma tanımaya konuşma Hizmetleri ile tümleşir. Yalnızca LUIS konuşma Hizmetleri aboneliği gerekmez.

LUIS iki tür anahtar kullanır:

|Anahtar türü|Amaç|
|--------|-------|
|authoring|LUIS app’lerini programlı olarak oluşturmanızı ve değiştirmenizi sağlar|
|endpoint |belirli bir LUIS app’ine erişimi yetkilendirir|

Bu öğreticide uç nokta anahtarı, LUIS anahtarıdır. Bu öğreticide, [Önceden oluşturulmuş Ev otomasyonu app’ini kullanma](https://docs.microsoft.com/azure/cognitive-services/luis/luis-get-started-create-app) başlığı altındaki yönergeleri izleyerek oluşturabildiğiniz örnek Ev Otomasyonu LUIS app kullanılır. Kendiniz bir LUIS app oluşturduysanız, onun yerine kendi oluşturduğunuzu da kullanabilirsiniz.

LUIS app oluşturduğunuzda otomatik olarak bir başlatma anahtarı oluşturulur; böylece, metin sorgularını kullanarak app’i test edebilirsiniz. Bu anahtar, konuşma Hizmetleri Tümleştirme etkinleştirmez ve bu öğreticiyle çalışmaz. Azure panosunda bir LUIS kaynağı oluşturmalı ve bu kaynağı LUIS app’e atamalısınız. Bu öğretici için ücretsiz abonelik katmanını kullanabilirsiniz.

Azure panosunda LUIS kaynağını oluşturduktan sonra [LUIS portalında](https://www.luis.ai/home) oturum açın, Uygulamalarım sayfasında uygulamanızı seçin ve app’in Yönet sayfasına geçin. Son olarak, kenar çubuğunda **Anahtarlar ve Uç Noktalar**’a tıklayın.

![LUIS portalı anahtarları ve uç nokta ayarları](media/sdk/luis-keys-endpoints-page.png)

Anahtarlar ve Uç Nokta ayarları sayfasında:

1. Sayfayı kaydırarak Kaynaklar ve Anahtarlar bölümüne gelin ve **Kaynak ata**’ya tıklayın.
1. **App’inize anahtar atayın** iletişim kutusunda şunları seçin:

    * Kiracı Olarak Microsoft’u seçin.
    * Abonelik Adı altında, kullanmak istediğiniz LUIS kaynağının bulunduğu Azure aboneliğini seçin.
    * Anahtar’ın altında, app ile kullanmak istediğiniz LUIS kaynağını seçin.

Kısa süre içinde yeni abonelik sayfanın altındaki tabloda görüntülenir. Panoya kopyalamak için anahtarın yanındaki simgeye tıklayın. (İstediğiniz anahtarı kullanabilirsiniz.)

![LUIS app abonelik anahtarları](media/sdk/luis-keys-assigned.png)

## <a name="create-a-speech-project-in-visual-studio"></a>Visual Studio’da konuşma projesi oluşturma

[!INCLUDE [Create project](../../../includes/cognitive-services-speech-service-create-speech-project-vs-csharp.md)]

## <a name="add-the-code"></a>Kod ekleme

Visual Studio projesinde `Program.cs` dosyasını açın ve dosyanın başındaki `using` deyim bloğunu şu bildirimlerle değiştirin.

[!code-csharp[Top-level declarations](~/samples-cognitive-services-speech-sdk/samples/csharp/sharedcontent/console/intent_recognition_samples.cs#toplevel)]

Sağlanan `Main()` yönteminin içine şu kodu ekleyin.

```csharp
RecognizeIntentAsync().Wait();
Console.WriteLine("Please press Enter to continue.");
Console.ReadLine();
```

Burada gösterildiği gibi, boş bir zaman uyumsuz `RecognizeIntentAsync()` yöntemi oluşturun.

```csharp
static async Task RecognizeIntentAsync()
{
}
```

Bu yeni yöntemin gövdesine bu kodu ekleyin.

[!code-csharp[Intent recognition by using a microphone](~/samples-cognitive-services-speech-sdk/samples/csharp/sharedcontent/console/intent_recognition_samples.cs#intentRecognitionWithMicrophone)]

Bu yöntemdeki yer tutucuları aşağıda gösterildiği gibi LUIS abonelik anahtarınız, bölgeniz ve uygulama kimliğinizle değiştirin.

|Yer tutucu|Şununla değiştir|
|-----------|------------|
|`YourLanguageUnderstandingSubscriptionKey`|LUIS uç nokta anahtarınız. Önceden belirtildiği gibi, bu bir “başlatma anahtarı” değil, Azure panosundan alınmış bir anahtar olmalıdır. Bunu, [LUIS portalındaki](https://www.luis.ai/home) app’inizin Anahtarlar ve Uç Noktalar sayfasında (Yönet altında) bulabilirsiniz.|
|`YourLanguageUnderstandingServiceRegion`|LUIS aboneliğinizin içinde bulunduğu bölgenin kısa tanımlayıcısı (örneğin, Batı ABD için `westus`). Bkz. [Bölgeler](regions.md).|
|`YourLanguageUnderstandingAppId`|LUIS app kimliği. Bunu, [LUIS portalında](https://www.luis.ai/home) app’inizin Ayarlar sayfasında bulabilirsiniz.|

Yapılan bu değişikliklerle, öğretici uygulamasını derleyebilir (Control-Shift-B) ve çalıştırabilirsiniz (F5). İstendiğinde, bilgisayarınızın mikrofonuna “Işıkları kapat” demeyi deneyin. Sonuç konsol penceresinde görüntülenir.

Aşağıdaki bölümlerde kod açıklaması yer alır.


## <a name="create-an-intent-recognizer"></a>Amaç tanıyıcı oluşturma

Konuşmadaki amaçların tanınmasında ilk adım LUIS uç nokta anahtarınızdan ve bölgenizden bir konuşma yapılandırması oluşturmaktır. Konuşma yapılandırmaları, Konuşma SDK’sının çeşitli özelliklerine yönelik tanıyıcılar oluşturmak için kullanılabilir. Konuşma yapılandırmasında kullanmak istediğiniz aboneliği belirtmek için birden çok yol sağlanır; burada, abonelik anahtarını ve bölgesini alan `FromSubscription` yolunu kullanıyoruz.

> [!NOTE]
> Anahtar ve LUIS aboneliğinizin, bir konuşma Hizmetleri aboneliğin bölge kullanın.

Daha sonra, `new IntentRecognizer(config)` kullanarak bir amaç tanıyıcı oluşturun. Yapılandırma hangi aboneliğin kullanılacağını zaten bildiğinden, tanıyıcıyı oluştururken abonelik anahtarını ve uç noktasını bir kez daha belirtmek gerekmez.

## <a name="import-a-luis-model-and-add-intents"></a>LUIS modelini içeri aktarma ve amaç ekleme

Şimdi de, `LanguageUnderstandingModel.FromAppId()` kullanarak LUIS app’ten modeli içeri aktarın ve tanıyıcının `AddIntent()` yöntemi aracılığıyla tanınmasını istediğiniz LUIS amaçlarını ekleyin. Bu iki adım, kullanıcıların isteklerinde kullanabileceği sözcükleri belirterek konuşma tanımanın doğruluğunu güçlendirir. Uygulamanızda tümünü tanımanız gerekmiyorsa app amaçlarının tümünü eklemeniz gerekmez.

Intents ekleme, üç bağımsız değişken gerektirir: LUIS modeline (oluşturulup oluşturulmadığını ve adlı `model`), hedefi adı ve bir hedefi kimliği Kimlik ve ad arasındaki fark aşağıda gösterilmiştir.

|`AddIntent()` bağımsız değişkeni|Amaç|
|--------|-------|
|intentName |LUIS app’te tanımlandığı şekliyle amacın adı. LUIS amacı adıyla tam olarak eşleşmelidir.|
|intentID    |Konuşma SDK’sı tarafından tanınan amaca atanan kimlik. İstediğiniz kimliği kullanabilirsiniz; LUIS app’te tanımlandığı gibi amaç adına karşılık gelmesi gerekmez. Örneğin, aynı kodla birden çok amaç işleniyorsa, bunlar için aynı kimliği kullanabilirsiniz.|

Giriş Otomasyon LUIS uygulaması iki amacı vardır: biri için bir cihaz ve başka bir cihazın kapatılması için kapatma. Aşağıdaki satırlar bu amaçları tanıyıcıya ekler; `RecognizeIntentAsync()` yöntemindeki üç `AddIntent` satırını bu kodla değiştirin.

```csharp
recognizer.AddIntent(model, "HomeAutomation.TurnOff", "off");
recognizer.AddIntent(model, "HomeAutomation.TurnOn", "on");
```

Tek tek amacı eklemek yerine, ayrıca kullanabileceğiniz `AddAllIntents` tüm hedefleri tanıyıcının bir modele eklemek için yöntemi.

## <a name="start-recognition"></a>Tanımayı başlatma

Oluşturulan tanıyıcıyla ve eklenen amaçlarla tanıma başlayabilir. Konuşma SDK’sı hem tek seferlik hem de sürekli tanımayı destekler.

|Tanıma modu|Çağrılacak yöntemler|Sonuç|
|----------------|-----------------|---------|
|Tek seferlik|`RecognizeOnceAsync()`|Tek konuşmadan sonra, tanınan amacı (varsa) döndürür.|
|Sürekli|`StartContinuousRecognitionAsync()`<br>`StopContinuousRecognitionAsync()`|Birden çok konuşmayı tanır. Sonuçlar sağlandığında olayları (örn., `IntermediateResultReceived`) yayar.|

Öğretici uygulaması tek seferlik modu kullanır ve bu nedenle tanımaya başlamak için `RecognizeOnceAsync()` yöntemini çağırır. Sonuç, tanınan amaç hakkındaki bilgileri içeren `IntentRecognitionResult` nesnesidir. LUIS JSON yanıtı aşağıdaki ifade tarafından ayıklanır:

```csharp
result.Properties.GetProperty(PropertyId.LanguageUnderstandingServiceResponse_JsonResult)
```

Öğretici uygulaması JSON sonucunu ayrıştırmaz, yalnızca konsol penceresinde görüntüler.

![LUIS tanıma sonuçları](media/sdk/luis-results.png)

## <a name="specify-recognition-language"></a>Tanıma dilini belirtme

LUIS varsayılan olarak ABD İngilizcesindeki (`en-us`) amaçları tanır. Konuşma yapılandırmasının `SpeechRecognitionLanguage` özelliğine yerel ayar kodu atayarak başka dillerde de amaçları tanıyabilirsiniz. Örneğin, amaçları Almanca tanımak için tanıyıcıyı oluşturmadan önce öğretici uygulamamıza `config.SpeechRecognitionLanguage = "de-de";` ekleyin. Bkz. [Desteklenen Diller](language-support.md#speech-to-text).

## <a name="continuous-recognition-from-a-file"></a>Dosyadan sürekli tanıma

Aşağıdaki kod, Konuşma SDK’sını kullanarak amaç tanımanın iki ek özelliğini gösterir. Önceden belirtildiği gibi, ilk özellik sonuçlar sağlandığında tanıyıcının olayları gösterdiği sürekli tanımadır. Ardından bu olaylar sağladığınız olay işleyicileri tarafından işlenebilir. Sürekli tanımayla, tanımayı başlatmak için `RecognizeOnceAsync()` yerine tanıyıcının `StartContinuousRecognitionAsync()` seçeneğini çağırırsınız.

Diğer özellik, işlenecek konuşmayı içeren sesin WAV dosyasından okunmasıdır. Amaç tanıyıcı oluşturulduğunda kullanılabilecek ses yapılandırmasını oluşturmayı içerir. Bu dosya 16 kHz örnekleme hızıyla tek kanallı (mono) olmalıdır.

Bu özellikleri denemek için `RecognizeIntentAsync()` yönteminin gövdesini aşağıdaki kodla değiştirin.

[!code-csharp[Intent recognition by using events from a file](~/samples-cognitive-services-speech-sdk/samples/csharp/sharedcontent/console/intent_recognition_samples.cs#intentContinuousRecognitionWithFile)]

Daha önce olduğu gibi LUIS uç nokta anahtarınızı, bölgenizi ve app kimliğinizi dahil etmek, ayrıca Ev Otomasyonu amaçlarını eklemek için kodu düzeltin. `whatstheweatherlike.wav` dosyasını ses dosyanızın adıyla değiştirin. Sonra da derleyin ve çalıştırın.

[!INCLUDE [Download the sample](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]
Samples/csharp/sharedcontent/console klasöründe bu makaledeki kodu bulabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Konuşmayı algılama](how-to-recognize-speech-csharp.md)
