---
title: Bot - C# - v3
titleSuffix: Language Understanding - Azure Cognitive Services
description: C# kullanarak, dil anlama (LUIS) ile tümleşik bir sohbet robotu oluşturun. Bu sohbet Robotu, hızlı bir şekilde bir bot çözümü uygulamak için önceden oluşturulmuş HomeAutomation etki alanını kullanır.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 01/09/2019
ms.author: diberry
ms.openlocfilehash: f23cf78bfca48b3a78e234520d645abdb354038f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60197461"
---
# <a name="luis-bot-in-c-with-the-bot-framework-3x-and-the-azure-web-app-bot"></a>LUIS bot içinde C# ile Bot Framework 3.x ve Azure Web app botu

C# kullanarak, dil anlama (LUIS) ile tümleşik bir sohbet robotu oluşturun. Bu sohbet Robotu, hızlı bir şekilde bir bot çözümü uygulamak için önceden oluşturulmuş HomeAutomation etki alanını kullanır. Bot, Bot Framework ile derlenir 3.x ve Azure Web app botu.

## <a name="prerequisite"></a>Önkoşul

* [HomeAutomation LUIS uygulaması](luis-get-started-create-app.md). Bu LUIS uygulaması eşlemesinden ıntents botun iletişim işleyicilere. 

## <a name="luis-homeautomation-intents"></a>LUIS HomeAutomation hedefleri

| Amaç | Örnek konuşma | Bot işlevi |
|:----:|:----------:|---|
| HomeAutomation.TurnOn | Işıkları aç'ı açın. | Zaman LUIS amaç `HomeAutomation.TurnOn` algılandığında, robot çağırır `OnIntent` iletişim işleyici. Bu iletişim kutusu, burada cihaz kapalıyken üzerinde kullanıcı bir cihazı açın ve bir IOT hizmeti çağrısı ' dir. |
| HomeAutomation.TurnOff | Yatak odası Işıkları aç. | Zaman LUIS amaç `HomeAutomation.TurnOff` algılandığında, robot çağırır `OffIntent` iletişim işleyici. Bu iletişim kutusu, burada cihaz kapatılmış, kullanıcı cihazı kapatıp ve bir IOT hizmeti çağırmak ' dir. |

## <a name="create-a-language-understanding-bot-with-bot-service"></a>Language Understanding bot ile Bot hizmeti oluşturma

1. İçinde [Azure portalında](https://portal.azure.com)seçin **yeni kaynak Oluştur** üstteki soldaki menüde.

    ![Azure portalında yeni kaynak oluştur](./media/luis-tutorial-cscharp-web-bot/bot-service-creation.png)

2. Arama kutusuna arama **Web App Botu**. 

    ![Web app botu kaynak türü seçin](./media/luis-tutorial-cscharp-web-bot/bot-service-selection.png)

3. Web App Botu pencerede **Oluştur**.

4. İçinde **Bot hizmeti**, gerekli bilgileri sağlayın ve tıklayın **Oluştur**. Bu, oluşturur ve Azure bot hizmeti ve LUIS uygulaması dağıtır. Kullanmak istiyorsanız [konuşma Hazırlama işlemi](https://docs.microsoft.com/bot-framework/bot-service-manage-speech-priming), gözden [bölge gereksinimleri](troubleshooting.md#what-luis-regions-support-bot-framework-speech-priming) botunuzun oluşturmadan önce. 
   * Ayarlama **uygulama adı** botunuzun kişinin adı. Botunuzun (örneğin, mynotesbot.azurewebsites.net) buluta dağıtıldığında alt etki alanı adı kullanılır. <!-- This name is also used as the name of the LUIS app associated with your bot. Copy it to use later, to find the LUIS app associated with the bot. -->
   * Aboneliği seçin [kaynak grubu](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview), App service planı ve [konumu](https://azure.microsoft.com/regions/).
   * İçin **Bot şablon**seçin:
       * **SDK'sı v3**
       * **C#**
       * **Dil anlama**
   * Seçin **LUIS uygulama konumu**. Bu yazma, [bölge](luis-reference-regions.md) uygulama oluşturulur.
   * Yasal bildirimi için onay kutusunu seçin. Yasal bildirimin koşullarını onay kutusunu ' dir.

     ![Bot Hizmeti](./media/luis-tutorial-cscharp-web-bot/bot-service-setting-callout-template.png)


5. Bot hizmeti dağıtıldığını doğrulayın.
    * Bildirimleri (Azure portalının üst kenarı bulunan zil simgesi) tıklayın. Bildirim değişiklikleri **dağıtım başlatıldı** için **dağıtım başarılı**.
    * Bildirim değişiklikler yapıldıktan sonra **dağıtım başarılı**, tıklayın **kaynağa Git** bu bildirim.

> [!Note]
> Bu web uygulama bot oluşturma işlemini yeni bir LUIS uygulaması sizin için da oluşturmuştur. Bu olduğundan eğitilmiş ve sizin için yayımlanan. 

## <a name="try-the-default-bot"></a>Varsayılan bot deneyin

Bot seçerek dağıtıldığını doğrulayın **bildirimleri** onay kutusu. Bildirimleri değişir **dağıtım sürüyor...**  için **dağıtım başarılı**. Tıklayın **kaynağa Git** botun kaynakları açmak için düğmeyi.

Bot dağıtıldıktan sonra tıklayın **Test Web sohbeti** Web Chat bölmesinde açmak için. "Web Chat hello" yazın.

  ![Bot Web Chat test edin.](./media/luis-tutorial-cscharp-web-bot/bot-service-web-chat.png)

Bot "Karşılama sınırına ulaştınız. diyerek yanıt verir. Bununla birlikte,: hello ".  Bu yanıt bot, bir ileti aldı ve varsayılan olarak oluşturulan LUIS uygulaması geçirilen onaylar. Bu varsayılan LUIS uygulaması Karşılama hedefi algılandı. Sonraki adımda, varsayılan LUIS uygulaması yerine daha önce oluşturduğunuz LUIS uygulaması için robot bağlanırsınız.

## <a name="connect-your-luis-app-to-the-bot"></a>İçin robot LUIS uygulamanızı bağlayın

Açık **uygulama ayarları** ve düzenleme **LuisAppId** LUIS uygulamanızı uygulama Kimliğini içeren alan. Batı ABD dışındaki bir bölgede HomeAutomation LUIS uygulamanızı oluşturduğunuz değiştirmek gerekirse **LuisAPIHostName** de. **LuisAPIKey** şu anda geliştirme anahtarınızı ayarlandı. Trafiğiniz ücretsiz katmanı kotayı aşarsa Bu uç nokta anahtarınızı değiştirin. 

  ![Azure'da LUIS uygulama kodunu güncelleştir](./media/luis-tutorial-cscharp-web-bot/bot-service-app-settings.png)

> [!Note]
> LUIS uygulama kimliği yoksa, [giriş Otomasyon uygulama](luis-get-started-create-app.md), oturum [LUIS](luis-reference-regions.md) Azure'da oturum açmak için kullandığınız hesabın aynısını kullanarak Web sitesi. 
> 1. Tıklayarak **uygulamalarım**. 
> 2. Amaç ve varlıkları HomeAutomation etki alanından içeren daha önce oluşturduğunuz, LUIS uygulaması bulun.
> 3. İçinde **ayarları** LUIS uygulaması için sayfasında, bulmak ve uygulama kimliği kopyalayın. Olduğundan emin olmak [eğitilen](luis-interactive-test.md) ve [yayımlanan](luis-how-to-publish-app.md). 
> 
> [!WARNING]
> Uygulama kimliği veya LUIS anahtarınızı silerseniz, robot çalışmayı durdurur.

## <a name="modify-the-bot-code"></a>Bot kodu değiştirin

1. Tıklayın **derleme** ve ardından **açık çevrimiçi Kod Düzenleyicisi**.

   ![Açık çevrimiçi Kod Düzenleyicisi](./media/luis-tutorial-cscharp-web-bot/bot-service-build.png)

2. Sağ tıklayın `build.cmd` ve **konsolundan çalıştırın** uygulamayı oluşturun. Hizmet sizin için otomatik olarak gerçekleştiren birkaç yapı adım vardır. İle işiniz bittiğinde, derleme tamamlandıktan "başarıyla tamamlandı."

3. Kod Düzenleyicisi'nde açın `/Dialogs/BasicLuisDialog.cs`. Bunu, aşağıdaki kodu içerir:

   [!code-csharp[Default BasicLuisDialog.cs](~/samples-luis/documentation-samples/tutorial-web-app-bot/csharp/Default_BasicLuisDialog.cs "Default BasicLuisDialog.cs")]

## <a name="change-code-to-homeautomation-intents"></a>HomeAutomation hedefleri için kodu değiştirin


1. Üç hedefi öznitelikleri ve yöntemlerini kaldırın **Karşılama**, **iptal**, ve **yardımcı**. Bu hedefleri HomeAutomation önceden oluşturulmuş etki alanında kullanılmaz. Tutmaya dikkat **hiçbiri** hedefi özniteliği ve yöntemi. 

2. Bağımlılıklar, diğer bağımlılıkları olan dosyanın üstüne ekleyin:

   [!code-csharp[Dependencies](~/samples-luis/documentation-samples/tutorial-web-app-bot/csharp/BasicLuisDialog.cs?range=4-5&dedent=8 "dependencies")]

3. Dizeleri en üstündeki yönetmek için sabitleri ekleyin `BasicLuisDialog` sınıfı:

   [!code-csharp[Add Intent and Entity Constants](~/samples-luis/documentation-samples/tutorial-web-app-bot/csharp/BasicLuisDialog.cs?range=23-32&dedent=8 "Add Intent and Entity Constants")]

4. Yeni hedefleri için kod ekleme `HomeAutomation.TurnOn` ve `HomeAutomation.TurnOff` içinde `BasicLuisDialog` sınıfı:

   [!code-csharp[Add Intents](~/samples-luis/documentation-samples/tutorial-web-app-bot/csharp/BasicLuisDialog.cs?range=61-71&dedent=8 "Add Intents")]

5. LUIS içinde bulunan tüm varlıkları almak için kod ekleyin `BasicLuisDialog` sınıfı:

   [!code-csharp[Collect entities](~/samples-luis/documentation-samples/tutorial-web-app-bot/csharp/BasicLuisDialog.cs?range=34-53&dedent=8 "Collect entities")]

6. Değişiklik **ShowLuisResult** yönteminde `BasicLuisDialog` puanı round, varlıkları toplamak ve sohbet Robotu yanıt iletisini görüntülemek için sınıf:

   [!code-csharp[Display message in chatbot](~/samples-luis/documentation-samples/tutorial-web-app-bot/csharp/BasicLuisDialog.cs?range=73-83&dedent=8 "Display message in chatbot")]

## <a name="build-the-bot"></a>Bot oluşturma
Kod Düzenleyicisi'nde sağ `build.cmd` seçip **konsolundan çalıştırın**.

![Web bot oluşturma](./media/luis-tutorial-cscharp-web-bot/bot-service-build-run-from-console.png)

Kod görünümü, bir terminal penceresi yapının sonuçlarını ve ilerleme durumunu gösteren değiştirilir.

![Web botu derleme başarısı](./media/luis-tutorial-cscharp-web-bot/bot-service-build-success.png)

> [!TIP]
> Robot adı üst mavi çubuğunu seçin ve markanızı için alternatif bir yöntem olan **Kudu konsolu aç**. Konsolu açılır **D:\home**. 
> 
> Dizinine **D:\home\site\wwwroot** yazarak: `cd site\wwwroot`
>
> Derleme betiği yazarak çalıştırın: `build.cmd`

## <a name="test-the-bot"></a>Bot test

Azure portalında tıklayarak **Test Web sohbeti içinde** bot test etmek için. Türü iletileri gibi "kapatma ışıkları Aç" ve "my heater kapatma" ona eklediğiniz ıntents çağırmak için.

   ![HomeAutomation bot Web Chat test edin.](./media/luis-tutorial-cscharp-web-bot/bot-service-chat-results.png)

> [!TIP]
> LUIS uygulamanızı, botun kod değişiklik olmadan yeniden eğitebilir. Bkz: [örnek Konuşma ekleme](https://docs.microsoft.com/azure/cognitive-services/LUIS/add-example-utterances) ve [eğitme ve LUIS uygulamanızı test etme](https://docs.microsoft.com/azure/cognitive-services/LUIS/luis-interactive-test). 

## <a name="download-the-bot-to-debug"></a>Hata ayıklamak için robot indirin
Botunuzun çalışmıyorsa, projeyi yerel makinenize indirmek ve devam et [hata ayıklama](https://docs.microsoft.com/bot-framework/bot-service-debug-bot). 

## <a name="learn-more-about-bot-framework"></a>Bot Framework hakkında daha fazla bilgi edinin
Daha fazla bilgi edinin [Bot Framework](https://dev.botframework.com/) ve [3.x](https://github.com/Microsoft/BotBuilder) ve [4.x](https://github.com/Microsoft/botbuilder-dotnet) SDK'ları.

## <a name="next-steps"></a>Sonraki adımlar

Bot hizmeti iletişim kutuları için işleme ve LUIS hedefleri ekleme **yardımcı**, **iptal**, ve **Karşılama** amacı. Unutmayın eğitmek, yayımlama ve [derleme](#build-the-bot) web app botu. LUIS hem bot aynı hedefleri olması gerekir.

Daha fazla bilgi bkz [örnekleri](https://github.com/Microsoft/AI) damıtarak konuşma bağlamında kullanılabilen bot ile. 

> [!div class="nextstepaction"]
> [Hedef ekleme](./luis-how-to-add-intents.md)
> [konuşma Hazırlama işlemi](https://docs.microsoft.com/bot-framework/bot-service-manage-speech-priming)

<!-- tested on Win10 -->
