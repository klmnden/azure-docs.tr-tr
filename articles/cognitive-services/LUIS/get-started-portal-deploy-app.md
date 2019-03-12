---
title: 'Hızlı Başlangıç: LUIS Portal ile uygulama dağıtma'
titleSuffix: Language Understanding - Azure Cognitive Services
description: Uygulamayı bir sohbet Robotu gibi bir istemci uygulaması utterance Öngörüler dönmek hazır olduğunda uygulamayı tahmin uç noktasına dağıtın gerekir. Bu hızlı başlangıçta, bir uygulama bir tahmin uç nokta kaynağı oluşturma, kaynak uygulamaya atama, uygulama eğitim ve uygulama yayımlama dağıtmayı öğrenin.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: quickstart
ms.date: 03/11/2019
ms.author: diberry
ms.openlocfilehash: 9c6ce826569b9c198e747a7977de3d346b1d3230
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2019
ms.locfileid: "57783138"
---
# <a name="quickstart-deploy-an-app-in-the-luis-portal"></a>Hızlı Başlangıç: LUIS Portalı'nda bir uygulama dağıtma

Uygulamayı bir sohbet Robotu gibi bir istemci uygulaması utterance Öngörüler dönmek hazır olduğunda uygulamayı tahmin uç noktasına dağıtın gerekir. 

Bu hızlı başlangıçta, bir uygulama bir tahmin uç nokta kaynağı oluşturma, kaynak uygulamaya atama, uygulama eğitim ve uygulama yayımlama dağıtmayı öğrenin. 

## <a name="prerequisites"></a>Önkoşullar

* [Azure aboneliği](https://azure.microsoft.com/free).
* Tamamlamak [önceki portalı hızlı başlangıcı](get-started-portal-build-app.md) veya [indirin ve uygulamayı içeri aktarma](https://github.com/Azure-Samples/cognitive-services-language-understanding/blob/master/documentation-samples/quickstarts/in-portal/build-portal-app.json). 


## <a name="create-endpoint-resource"></a>Uç nokta kaynağı oluşturma

Tahmin uç nokta kaynağı, Azure portalında oluşturulur. Bu kaynak, yalnızca uç nokta tahmin sorguları için kullanılmalıdır. Bu kaynak, uygulama geliştirme değişiklikler için kullanmayın. 

1. Oturum  **[Azure portalında](https://ms.portal.azure.com/)**. 

1. Yeşili **+** üst sol panelde oturum ve arama `Cognitive Services` Market'te seçin. 

1. Abonelik, aşağıdaki ayarlarla yapılandırın:

    |Ayar|Değer|Amaç|
    |--|--|--|
    |Ad|`my-cognitive-service-resource`|Azure kaynak adı. Kaynak LUIS Portalı'nda uygulama atadığınızda, bu adı gerekir.|
    |Abonelik|Aboneliğiniz|Hesabınızla ilişkili abonelikleri birini seçin.|
    |Konum|**Batı ABD**|Bu kaynak için bir azure bölgesi.|
    |Fiyatlandırma katmanı|**S0**|Fiyatlandırma katmanına bu kaynak için varsayılan.|
    |Kaynak grubu|`my-cognitive-service-resource-group`|Tüm bilişsel hizmet kaynaklarınız için yeni bir kaynak grubu oluşturun. Kaynaklar ile işiniz bittiğinde, aboneliğinizi temizlemek için kaynak grubunu silebilirsiniz. | 

    ![Azure API seçim](./media/get-started-portal-deploy-app/create-cognitive-services-resource.png) 

1. Seçin **Oluştur** Azure kaynağını oluşturmak için. 

    Sonraki bölümde, bu yeni kaynak LUIS portalında LUIS uygulaması bağlanma hakkında bilgi edinin. 

## <a name="assign-the-resource-key-to-the-luis-app-in-the-luis-portal"></a>Kaynak anahtarı LUIS Portalı'nda LUIS uygulama atama

LUIS için yeni bir kaynak oluşturmak her durumda, LUIS uygulaması için kaynak atamanız gerekir. Atandıktan sonra yeni bir kaynak oluşturmadığınız sürece bu adımı tekrar yapmanız gerekmez. Uygulamanızı bölgeleri genişletin veya daha yüksek bir sayı tahmin sorguları desteklemek için yeni bir kaynak oluşturabilir. 

1. Oturum [LUIS portalı](https://www.luis.ai), seçin **myEnglishApp** uygulamalar listesinden uygulama.

1. Seçin **Yönet** seçip sağ üst menüsünde **anahtarları ve uç noktaları**.

1. LUIS eklemek için seçin **Kaynak Ata +**.

    [![Uygulamanıza bir kaynak atayın](./media/get-started-portal-deploy-app/assign-resource-button.png)](./media/get-started-portal-deploy-app/assign-resource-button.png#lightbox)

1. Kiracı, abonelik ve kaynak adınız ve ardından seçin **atama kaynak**. 

    ![Uygulamanıza bir kaynak atayın](./media/get-started-portal-deploy-app/assign-resource.png)

1. Tabloya yeni satır bulun ve uç nokta URL'sini kopyalayın. LUIS API'si uç nokta tahmin için bir HTTP GET isteği yapmak için doğru şekilde oluşturulur. 

## <a name="train-and-publish-the-app"></a>Uygulamayı eğitme ve yayımlama 

Eğitim zaman, test etmek hazır. Tahmin uç nokta çalışma zamanını şuradan istemci uygulamaları için kullanılabilir olması için şu anda eğitilen sürümünü istediğiniz zaman uygulama yayımlamanız gerekir. 

1. Uygulama deneyimsiz ise seçin **eğitme** üstten sağ menü.

1. Seçin **Yayımla** üstten sağ menü. Varsayılan ortam ayarlarını kabul edin ve seçin **Yayımla**.

1. Yeşil bir başarı bildirim çubuğu tarayıcı penceresinin en üstünde göründüğünde seçin **uç noktalar listesine bakın** bağlantı. 

    ![Tarayıcıda başarıyla yayımlandı uygulama bildirim çubuğu](./media/get-started-portal-deploy-app/successfully-published-notification.png)

1. Sayfasına yönlendirileceksiniz **anahtarları ve uç nokta ayarları** sayfası. Atanmış kaynaklar ve karşılık gelen uç nokta URL'leri listesidir sayfanın alt kısmında. 

1. Yeni kaynak adınız ile ilişkili uç nokta URL'sini seçin. Bunu yapmak için doğru şekilde oluşturulmuş bir URL ile bir web tarayıcısı açılır bir **alma** isteği tahmin uç çalışma zamanı. 

1. `q=` URL'nin sonunda kısaltması olduğundan **sorgu** ve burada kullanıcının utterance GET isteğine eklenir. Sonra `q=`, önceki hızlı başlangıcın sonunda kullanılan aynı kullanıcı utterance girin:

    ```Is there a form named hrf-234098```

    Tarayıcı, istemci uygulamanın alacağı JSON'un yanıttır:

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

    Bu yanıt varsayılandan daha fazla bilgi veren **Test** önceki öğreticide bölmesi. Bu düzeyde Test bölmesini bilgilerinin görmek istiyorsanız, uygulamayı yayımlanması gerekir. Test Bölmesi'nde seçip **Karşılaştır yayımlanan**. Kullanım **Göster JSON görünümü** yayımlanmış test bölmesinde aynı JSON önceki adıma bakın. Bu, üzerinde çalıştığınız geçerli uygulama ve uç noktaya yayımlanan uygulama karşılaştırmanızı sağlar.

    [![Şu anda yayımlanan uygulama sürümü düzenleme karşılaştırın](./media/get-started-portal-deploy-app/compare-test-pane.png)](./media/get-started-portal-deploy-app/compare-test-pane.png#lightbox)


## <a name="clean-up-resources"></a>Kaynakları temizleme
Bu hızlı başlangıcı tamamladığınızda, seçin **uygulamalarım** üst gezinti menüsünde. Ardından listeden uygulamanın sol taraftaki onay kutusunu seçin ve seçin **Sil** listesinin üst bağlam araç çubuğundan. 

[![Uygulamalar listemde uygulamayı Sil](./media/get-started-portal-build-app/delete-app.png)](./media/get-started-portal-build-app/delete-app.png#lightbox)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Ortak hedefleri ve varlıkları tanımlama](/luis-tutorial-prebuilt-intents-entities.md)