---
title: 'Hızlı Başlangıç: LUIS portal ile uygulama dağıtma'
titleSuffix: Language Understanding - Azure Cognitive Services
description: Uygulamayı bir sohbet Robotu gibi bir istemci uygulaması utterance Öngörüler dönmek hazır olduktan sonra LUIS uygulamanızı tahmin uç noktasına dağıtmayı öğrenin. Bu hızlı başlangıçta nasıl tahmin uç nokta kaynağı oluşturma, kaynak uygulamaya atama, uygulama eğitim ve uygulama yayımlama bir uygulamayı dağıtmak size kılavuzluk eder.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: quickstart
ms.date: 03/11/2019
ms.author: diberry
ms.openlocfilehash: 9a54cad9212bdb514f7742121909221863ba75e3
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64713513"
---
# <a name="quickstart-deploy-an-app-in-the-luis-portal"></a>Hızlı Başlangıç: LUIS Portalı'nda bir uygulama dağıtma

LUIS uygulamanızı utterance Öngörüler, bir istemci uygulaması (örneğin, bir sohbet Robotu) geri dönmek hazır olduğunda uygulamayı tahmin uç noktaya dağıtmanız gerekebilir.

Bu hızlı başlangıçta, bir uygulamayı nasıl dağıtacağınızı öğrenin. Tahmin uç nokta kaynak oluşturma, kaynak uygulamaya atamak, uygulamayı eğitme ve uygulamayı yayımlayın.

## <a name="prerequisites"></a>Önkoşullar

* Alma bir [Azure aboneliği](https://azure.microsoft.com/free).
* Tamamlamak [önceki portalı hızlı başlangıcı](get-started-portal-build-app.md) veya [indirin ve uygulamayı içeri aktarma](https://github.com/Azure-Samples/cognitive-services-language-understanding/blob/master/documentation-samples/quickstarts/in-portal/build-portal-app.json).

## <a name="create-the-endpoint-resource"></a>Uç nokta kaynağı oluşturma

Azure portalında'de tahmin uç nokta kaynağı oluşturun. Bu kaynak, yalnızca uç nokta tahmin sorguları için kullanılmalıdır. Bu kaynak, uygulama geliştirme değişiklikler için kullanmayın.

1. [Azure Portal](https://ms.portal.azure.com/) oturum açın.

1. Yeşili **+** sol panelde oturum açın. Arama `Cognitive Services` Market'te ve bu seçeneği belirleyin.

1. Abonelik, aşağıdaki ayarlarla yapılandırın:

   |Ayar|Değer|Amaç|
   |--|--|--|
   |Ad|`my-cognitive-service-resource`|Azure kaynak adı. Kaynak LUIS Portalı'nda uygulama atadığınızda, bu adı gerekir.|
   |Abonelik|Aboneliğiniz|Hesabınızla ilişkili abonelikleri birini seçin.|
   |Location|**Batı ABD**|Bu kaynak için bir Azure bölgesi.|
   |Fiyatlandırma katmanı|**S0**|Fiyatlandırma katmanına bu kaynak için varsayılan.|
   |Kaynak grubu|`my-cognitive-service-resource-group`|Tüm bilişsel hizmet kaynaklarınız için yeni bir kaynak grubu oluşturun. Kaynaklar ile işiniz bittiğinde, aboneliğinizi temizlemek için kaynak grubunu silebilirsiniz. |
   | | | |

   ![Azure API seçim](./media/get-started-portal-deploy-app/create-cognitive-services-resource.png)

1. Seçin **Oluştur** Azure kaynağını oluşturmak için.

   Sonraki bölümde, bu yeni kaynak LUIS portalında LUIS uygulaması bağlanma hakkında bilgi edinin.

## <a name="assign-the-resource-key-to-the-luis-app-in-the-luis-portal"></a>Kaynak anahtarı LUIS Portalı'nda LUIS uygulama atama

LUIS için yeni bir kaynak oluşturmak her durumda, LUIS uygulaması için kaynak atamanız gerekir. Atandıktan sonra yeni bir kaynak oluşturmadığınız sürece bu adımı tekrar yapmanız gerekmez. Uygulamanızı bölgeleri genişletin veya daha yüksek bir sayı tahmin sorguları desteklemek için yeni bir kaynak oluşturabilir.

1. Oturum [LUIS portalı](https://www.luis.ai) ve **myEnglishApp** uygulamalar listesinden uygulama.

1. Seçin **Yönet** seçin ve sağ üst menüsünde **anahtarları ve uç noktaları**.

1. LUIS eklemek için seçin **Kaynak Ata +**.

   [![Uygulamanıza bir kaynak atayın](./media/get-started-portal-deploy-app/assign-resource-button.png)](./media/get-started-portal-deploy-app/assign-resource-button.png#lightbox)

1. Kiracı, abonelik ve kaynak adınızı seçin. Seçin **atama kaynak**.

   ![Uygulamanıza bir kaynak atayın](./media/get-started-portal-deploy-app/assign-resource.png)

1. Tabloya yeni satır bulun ve uç nokta URL'sini kopyalayın. Doğru sağlamak üzere oluşturulmuş olan bir `HTTP GET` LUIS API'si uç noktasına bir tahmin için istek.

## <a name="train-and-publish-the-app"></a>Uygulamayı eğitme ve yayımlama

Uygulamayı test etmek hazır olduğunuzda eğitin. Tahmin uç nokta çalışma zamanını şuradan istemci uygulamaları için kullanılabilir olması için şu anda eğitilen sürümünü istediğiniz zaman, uygulamayı yayımlayın.

1. Uygulama deneyimsiz ise seçin **eğitme** sağ üst köşedeki menüden.

1. Seçin **Yayımla** menüsünde. Varsayılan ortam ayarlarını kabul edin ve seçin **Yayımla**.

1. Yeşil bir başarı bildirim çubuğu tarayıcı penceresinin en üstünde göründüğünde seçin **uç noktalar listesine bakın**.

   ![Tarayıcıda başarıyla yayımlandı uygulama bildirim çubuğu](./media/get-started-portal-deploy-app/successfully-published-notification.png)

1. Üzerinde **anahtarları ve uç nokta ayarları** sayfasında, alt kısmında atanan kaynaklara ve karşılık gelen uç nokta URL'lerinin listesini bulun.

1. Yeni kaynak adınız ile ilişkili uç nokta URL'sini seçin. Bu eylem yapmak için doğru şekilde oluşturulmuş bir URL ile bir web tarayıcısı açılır bir `GET` istek tahmin uç çalışma zamanı.

1. `q=` URL'nin sonunda kısaltması olduğundan **sorgu** ve burada kullanıcının utterance GET isteğine eklenir. Sonra `q=`, önceki hızlı başlangıcın sonunda kullanılan aynı kullanıcı utterance girin:

    ```Is there a form named hrf-234098```

    Tarayıcı, istemci uygulamanın alacağı aynı JSON yanıtı gösterilir:

    ```JSON
    {
    "query": "Is there a form named hrf-234098",
    "topScoringIntent": {
        "intent": "FindForm",
        "score": 0.9768753
    },
    "intents": [
        {
        "intent": "FindForm",
        "score": 0.9768753
        },
        {
        "intent": "None",
        "score": 0.0216071066
        }
    ],
    "entities": [
        {
        "entity": "hrf-234098",
        "type": "Human Resources Form Number",
        "startIndex": 22,
        "endIndex": 31
        }
      ]
    }
    ```

    Bu yanıt, önceki öğreticide varsayılan test bölmesini daha fazla bilgi sağlar. Bu düzeyde test bölmesindeki bilgileri görmek için uygulamayı yayımlamanız gerekir. Uygulamayı yayımladıktan sonra seçin **Karşılaştır yayımlanan** test bölmesinde. Kullanım **Göster JSON görünümü** yayınlanan test bölmesinde aynı JSON önceki adıma bakın. Bu şekilde, uç nokta için yayımlanmış bir uygulamayla, üzerinde çalıştığınız geçerli uygulama karşılaştırabilirsiniz.

    [![Şu anda yayımlanan uygulama sürümü düzenleme karşılaştırın](./media/get-started-portal-deploy-app/compare-test-pane.png)](./media/get-started-portal-deploy-app/compare-test-pane.png#lightbox)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu hızlı başlangıcı tamamladığınızda, seçin **uygulamalarım** üst gezinti menüsünde. Listeden uygulamanın onay kutusunu seçin ve ardından **Sil** listesinin üst bağlam araç çubuğundan.

[![Uygulamalar listemde uygulamayı Sil](./media/get-started-portal-build-app/delete-app.png)](./media/get-started-portal-build-app/delete-app.png#lightbox)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Ortak hedefleri ve varlıkları tanımlama](luis-tutorial-prebuilt-intents-entities.md)
