---
title: C# için Azure Bot Oluşturucu SDK'yı kullanarak bir bot HALUK tümleştirileceğini | Microsoft Docs
description: Bot Framework kullanarak bir HALUK uygulaması ile tümleşik bir bot oluşturun.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 03/06/2018
ms.author: v-geberr
ms.openlocfilehash: b3283880ebb116e5397c38d722a0790cff414f38
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37111931"
---
# <a name="web-app-bot-using-the-luis-template-for-c"></a>Web uygulaması HALUK şablonu için C# kullanarak Bot

Tümleşik dil anlama ile chatbot oluşturun.

## <a name="prerequisite"></a>Önkoşul

* [HomeAutomation HALUK uygulama](luis-get-started-create-app.md). Bu HALUK uygulama eşlemesinden hedefleri bot kişinin iletişim işleyicilere. 

## <a name="luis-homeautomation-intents"></a>HALUK HomeAutomation hedefleri

| Hedefi | Örnek utterance | Bot işlevi |
|:----:|:----------:|---|
| HomeAutomation.TurnOn | Üzerinde ışık açın. | Zaman HALUK hedefi `HomeAutomation.TurnOn` algılandığında bot çağırır `OnIntent` iletişim işleyici. Bu iletişim kutusu, burada bir IOT bir aygıtı açın ve aygıtın açık üzerinde kullanıcıya bildir hizmete çağrı ' dir. |
| HomeAutomation.TurnOff | Yatak ışık devre dışı bırakın. | Zaman HALUK hedefi `HomeAutomation.TurnOff` algılandığında bot çağırır `OffIntent` iletişim işleyici. Bu iletişim kutusu, burada bir aygıtı kapatın ve kullanıcı aygıt kapatılmış olduğunu bildirmek üzere bir IOT hizmet çağırırdı ' dir. |

## <a name="create-a-language-understanding-bot-with-bot-service"></a>Bir dil anlama bot Bot hizmeti ile oluşturma

1. İçinde [Azure portal](https://portal.azure.com)seçin **yeni kaynak oluşturmak** üst soldaki menüde.

    ![Yeni kaynak oluştur](./media/luis-tutorial-cscharp-web-bot/bot-service-creation.png)

2. Arama kutusuna arama **Web uygulaması Bot**. 

    ![Yeni kaynak oluştur](./media/luis-tutorial-cscharp-web-bot/bot-service-selection.png)

3. Web uygulaması Bot penceresinde **oluşturma**.

4. İçinde **Bot hizmet**, gerekli bilgileri sağlayın ve tıklayın **oluşturma**. Bu oluşturur ve bot hizmet ve HALUK uygulama için Azure dağıtır. Kullanmak istiyorsanız, [konuşma Hazırlama işlemi](https://docs.microsoft.com/bot-framework/bot-service-manage-speech-priming), gözden [bölge gereksinimleri](luis-resources-faq.md#what-luis-regions-support-bot-framework-speech-priming) , bot oluşturmadan önce. 
    * Ayarlama **uygulama adı** bot kişinin adı. Bot (örneğin, mynotesbot.azurewebsites.net) buluta dağıtıldığında alt etki alanı adı kullanılır. <!-- This name is also used as the name of the LUIS app associated with your bot. Copy it to use later, to find the LUIS app associated with the bot. -->
    * Abonelik seç [kaynak grubu](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview), uygulama hizmeti planı ve [konumu](https://azure.microsoft.com/regions/).
    * Seçin **dil anlama (C#)** için şablon **Bot şablonu** alan.
    * Seçin **HALUK uygulamasının konumuna**. Yazma budur [bölge] [ LUIS] uygulama oluşturulur.
    * Yasal bildirim için onay kutusunu seçin. Yasal bildirim koşullarını onay kutusu var.

    ![Boz Hizmeti](./media/luis-tutorial-cscharp-web-bot/bot-service-setting-callout-template.png)


5. Bot hizmet dağıtıldığını doğrulayın.
    * Bildirimleri (Azure portalı üst kenarına bulunan zil simgesine) tıklayın. Bildirim değişiklikleri **dağıtım başladı** için **dağıtım başarılı**.
    * Bildirim değişiklikler yapıldıktan sonra **dağıtım başarılı**, tıklatın **kaynağa gidin** bu bildirim.

> [!Note]
> Bu web uygulaması bot oluşturma işlemi aynı zamanda yeni bir HALUK uygulaması için oluşturduğunuz. Bunu olduğundan eğitilmiş ve sizin için yayımlanan. 

## <a name="try-the-default-bot"></a>Varsayılan bot deneyin

Bot seçerek dağıtıldığını doğrulayın **bildirimleri** onay kutusu. Bildirimleri değiştirir **dağıtımı devam ediyor...**  için **dağıtım başarılı**. Tıklatın **kaynağa gidin** bot ait kaynakları açmak için düğmeye.

Bot dağıtıldığında, tıklatın **Web sohbeti testinde** Web sohbet bölmesini açmak için. "Web sohbet hello" yazın.

  ![Web sohbet bot test](./media/luis-tutorial-cscharp-web-bot/bot-service-web-chat.png)

Bot "Tebrik ulaştınız. söyleyerek yanıt verir. Belirtilmektedir: hello ".  Bu yanıt bot, ileti aldı ve varsayılan olarak oluşturulan HALUK uygulama geçirilen onaylar. Bu varsayılan HALUK uygulama Tebrik hedefi algıladı. Sonraki adımda HALUK uygulama varsayılan yerine daha önce oluşturduğunuz HALUK App bot bağlanırsınız.

## <a name="connect-your-luis-app-to-the-bot"></a>Bot HALUK uygulamanızı bağlama

Açık **uygulama ayarları** ve düzenleme **LuisAppId** HALUK uygulamanızı uygulama Kimliğini içeren alan. Batı ABD dışındaki bir bölgede HomeAutomation HALUK uygulamanızı oluşturduysanız, değiştirmeye ihtiyaç **LuisAPIHostName** de. **LuisAPIKey** geliştirme anahtarınızı ayarlanmış. Ücretsiz katmanı kota trafiğinizi aştığında, bu uç nokta anahtarınızı değiştirin. 

  ![Azure HALUK uygulama Kimliğini güncelleştir](./media/luis-tutorial-cscharp-web-bot/bot-service-app-settings.png)

> [!Note]
> HALUK uygulama Kimliğini yoksa [giriş Otomasyon uygulama](luis-get-started-create-app.md), oturum [HALUK](luis-reference-regions.md) Azure'da oturum açma için kullandığınız aynı hesabı kullanarak Web sitesi. 
> 1. Tıklayın **uygulamalarım**. 
> 2. Hedefleri ve HomeAutomation etki alanından varlıkları içeren daha önce oluşturduğunuz, HALUK uygulamayı bulun.
> 3. İçinde **ayarları** sayfasında HALUK, bulmak ve uygulama kimliği kopyalayın. Olmasına dikkat edin [eğitilen](interactive-test.md) ve [yayımlanan](PublishApp.md). 

    > [!WARNING]
    > If you delete your app ID or LUIS key, the bot will stop working.

## <a name="modify-the-bot-code"></a>Bot kodunu değiştirme

1. Tıklatın **yapı** ve ardından **açık çevrimiçi Kod düzenleyicisinde**.

   ![Açık çevrimiçi Kod Düzenleyicisi](./media/luis-tutorial-cscharp-web-bot/bot-service-build.png)

2. Sağ tıklayın `build.cmd` ve **konsolundan çalıştırma** uygulamanızı oluşturmak için. Hizmet sizin için otomatik olarak tamamlar birkaç yapı adım vardır. İle bittiğinde, yapı tamamlandıktan "başarıyla tamamlandı."

3. Kod düzenleyicisinde açın `/Dialogs/BasicLuisDialog.cs`. Aşağıdaki kod içerir:

   [!code-csharp[Default BasicLuisDialog.cs](~/samples-luis/documentation-samples/tutorial-web-app-bot/csharp/Default_BasicLuisDialog.cs "Default BasicLuisDialog.cs")]

## <a name="change-code-to-homeautomation-intents"></a>HomeAutomation amaçlar için kodunu değiştir


1. Üç hedefi öznitelikleri ve yöntemleri kaldırmak **selamlama**, **iptal**, ve **yardımcı**. Bu hedefleri HomeAutomation önceden oluşturulmuş etki alanında kullanılmaz. Sakladığınızdan emin olun **hiçbiri** hedefi özniteliği ve yöntemi. 

2. Bağımlılıklar diğer bağımlılıkları olan dosyanın en üstüne ekleyin:

   [!code-csharp[Dependencies](~/samples-luis/documentation-samples/tutorial-web-app-bot/csharp/BasicLuisDialog.cs?range=4-5&dedent=8 "dependencies")]

3. Dizeleri en üstündeki yönetmek için sabitleri ekleyin `BasicLuisDialog ` sınıfı:

   [!code-csharp[Add Intent and Entity Constants](~/samples-luis/documentation-samples/tutorial-web-app-bot/csharp/BasicLuisDialog.cs?range=23-32&dedent=8 "Add Intent and Entity Constants")]

4. Yeni amaçlar için kod ekleme `HomeAutomation.TurnOn` ve `HomeAutomation.TurnOff` içinde `BasicLuisDialog ` sınıfı:

   [!code-csharp[Add Intents](~/samples-luis/documentation-samples/tutorial-web-app-bot/csharp/BasicLuisDialog.cs?range=61-71&dedent=8 "Add Intents")]

5. HALUK içinde bulunan tüm varlıkları almak için kodu ekleyin `BasicLuisDialog ` sınıfı:

   [!code-csharp[Collect entities](~/samples-luis/documentation-samples/tutorial-web-app-bot/csharp/BasicLuisDialog.cs?range=34-53&dedent=8 "Collect entities")]

6. Değişiklik **ShowLuisResult** yönteminde `BasicLuisDialog ` sınıfı puanı round, varlıkları toplamak ve chatbot yanıt iletisini görüntülemek için:

   [!code-csharp[Display message in chatbot](~/samples-luis/documentation-samples/tutorial-web-app-bot/csharp/BasicLuisDialog.cs?range=73-83&dedent=8 "Display message in chatbot")]

## <a name="build-the-bot"></a>Bot derleme
Kod Düzenleyicisi'nde sağ tıklayın `build.cmd` seçip **konsolundan çalıştırma**.

![Web bot derleme ](./media/luis-tutorial-cscharp-web-bot/bot-service-build-run-from-console.png)

Kod görünümünde ilerleme durumunu ve sonuçlarını derleme gösteren bir terminal penceresi ile değiştirilir.

![Web bot Yapı Başarı](./media/luis-tutorial-cscharp-web-bot/bot-service-build-success.png)

> [!TIP]
> Üst mavi çubuğunu bot adını seçin ve seçmek için bot oluşturmak için alternatif bir yöntemi olan **açık Kudu Konsolu**. İçin konsolu açar **D:\home**. 
> 
> Dizinine değiştirin **D:\home\site\wwwroot** yazarak: `cd site\wwwroot`
>
> Yapı betik yazarak çalıştırın: `build.cmd`

## <a name="test-the-bot"></a>Bot test

Azure portalında tıklayın **Test Web sohbet** bot test etmek için. İletileri yazın LIKE "üzerinde ışık kapatın" ve "devre dışı my ısıtıcı" kendisine eklenmiş hedefleri çağırmak için.

   ![Web sohbet HomeAutomation bot test](./media/luis-tutorial-cscharp-web-bot/bot-service-chat-results.png)

> [!TIP]
> HALUK uygulamanızı bot's kod kendilerine herhangi bir değişiklik olmadan yeniden eğitme. Bkz: [örnek utterances ekleme](https://docs.microsoft.com/azure/cognitive-services/LUIS/add-example-utterances) ve [eğitmek ve HALUK uygulamanızı test](https://docs.microsoft.com/azure/cognitive-services/LUIS/interactive-test). 

## <a name="download-the-bot-to-debug"></a>Hata ayıklama için bot indirin
Bot çalışmıyorsa, projeyi yerel makinenize indirmek ve devam et [hata ayıklama](https://docs.microsoft.com/bot-framework/bot-service-debug-bot#debug-an-azure-app-service-web-app-c-bot). 

## <a name="learn-more-about-bot-framework"></a>Bot Framework hakkında daha fazla bilgi edinin
Daha fazla bilgi edinmek [Bot Framework](https://dev.botframework.com/) ve [3.x](https://github.com/Microsoft/BotBuilder) ve [4.x](https://github.com/Microsoft/botbuilder-dotnet) SDK'ları.

## <a name="next-steps"></a>Sonraki adımlar

HALUK amaçları ve işleme Bot hizmet iletişim kutularına ekleme **yardımcı**, **iptal**, ve **selamlama** amaçlar. Eğitmek unutmayın yayımlama ve [yapı](#build-the-bot) web uygulaması bot. HALUK ve bot aynı hedefleri olması gerekir.

> [!div class="nextstepaction"]
> [Hedefleri Ekle](./luis-how-to-add-intents.md)
> [konuşma Hazırlama işlemi](https://docs.microsoft.com/bot-framework/bot-service-manage-speech-priming)


<!-- Links -->
[Github-BotFramework-Emulator-Download]: https://aka.ms/bot-framework-emulator
[Github-LUIS-Samples]: https://github.com/Microsoft/LUIS-Samples
[Github-LUIS-Samples-cs-hotel-bot]: https://github.com/Microsoft/LUIS-Samples/tree/master/bot-integration-samples/hotel-finder/csharp
[Github-LUIS-Samples-cs-hotel-bot-readme]: https://github.com/Microsoft/LUIS-Samples/blob/master/bot-integration-samples/hotel-finder/csharp/README.md
[BFPortal]: https://dev.botframework.com/
[RegisterInstructions]: https://docs.microsoft.com/bot-framework/portal-register-bot
[BotFramework]: https://docs.microsoft.com/bot-framework/
[AssignedEndpointDoc]: https://docs.microsoft.com/azure/cognitive-services/LUIS/manage-keys
[VisualStudio]: https://www.visualstudio.com/
[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions
<!-- tested on Win10 -->
