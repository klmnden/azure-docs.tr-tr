---
title: URL ve Azure medya Hizmetleri - REST kullanarak akışa bağlı bir uzak dosya kodlama | Microsoft Docs
description: Bir URL'sini temel alarak dosya kodlamak için bu öğreticideki adımları izleyin ve Azure Media Services REST kullanarak içerik akışı.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: tutorial
ms.custom: mvc
ms.date: 04/22/2019
ms.author: juliako
ms.openlocfilehash: 54e52cfc60c42cc48a5c2d33cc6b7e64e75cf27c
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64707657"
---
# <a name="tutorial-encode-a-remote-file-based-on-url-and-stream-the-video---rest"></a>Öğretici: URL'sini temel alarak bir uzak dosya kodlama ve akışını video - REST

Azure Media Services, çok çeşitli tarayıcılar ve cihazlar üzerinde yürütülen biçimlerini kullanarak medya dosyalarınızı kodlayın sağlar. Örneğin, içeriğinizi Apple'ın HLS veya MPEG DASH biçimlerinde akışla göndermek isteyebilirsiniz. Akışla göndermeden önce yüksek kaliteli dijital medya dosyanızı kodlamanız gerekir. Kodlama yönergeleri için bkz. [Kodlama kavramı](encoding-concept.md).

Bu öğretici, REST kullanarak URL'sini temel alarak bir dosya kodlama ve Azure Media Services ile video akışı işlemini göstermektedir. 

![Videoyu yürütme](./media/stream-files-tutorial-with-api/final-video.png)

Bu öğretici şunların nasıl yapıldığını gösterir:    

> [!div class="checklist"]
> * Media Services hesabı oluşturma
> * Media Services API’sine erişim
> * Postman dosyalarını indirme
> * Postman'i yapılandırma
> * Postman kullanarak istek gönderme
> * Akış URL’sini test etme
> * Kaynakları temizleme

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Önkoşullar

- [Bir Media Services hesabı oluşturma](create-account-cli-how-to.md).

    Media Services hesap adını ve kaynak grubu adı için kullanılan değerleri unutmayın emin olun

- AMS REST öğreticilerinden bazılarında gösterilen REST API'lerini yürütmek için [Postman](https://www.getpostman.com/) REST istemcisini yükleyin. 

    Biz **Postman**'ı kullanıyoruz, ancak herhangi bir REST aracı da olabilir. Diğer alternatifler: **Visual Studio Code** REST eklentisiyle veya **Telerik Fiddler**. 

## <a name="download-postman-files"></a>Postman dosyalarını indirme

Postman koleksiyonunu ve ortam dosyalarını içeren bir GitHub deposunu kopyalayın.

 ```bash
 git clone https://github.com/Azure-Samples/media-services-v3-rest-postman.git
 ```

[!INCLUDE [media-services-v3-cli-access-api-include](../../../includes/media-services-v3-cli-access-api-include.md)]

## <a name="configure-postman"></a>Postman'i yapılandırma

Bu bölümde Postman yapılandırılmaktadır.

### <a name="configure-the-environment"></a>Ortamı yapılandırma 

1. **Postman**'ı açın.
2. Ekranın sağ tarafında **Ortamı yönet** seçeneğini belirleyin.

    ![Ortamı yönetme](./media/develop-with-postman/postman-import-env.png)
4. **Ortamı yönet** iletişim kutusunda **İçe aktar**'ı tıklatın.
2. `https://github.com/Azure-Samples/media-services-v3-rest-postman.git` kopyasını oluşturduğunuzda indirilen `Azure Media Service v3 Environment.postman_environment.json` dosyasına gidin.
6. **Azure Media Service v3 Environment** ortamı eklenir.

    > [!Note]
    > Erişim değişkenlerini yukarıdaki **Media Services API'sine erişme** bölümünden aldığınız değerlerle güncelleştirin.

7. Seçili dosyaya çift tıklayın ve [API'ye erişim](#access-the-media-services-api) adımlarını izleyerek aldığınız değerleri girin.
8. İletişim kutusunu kapatın.
9. Aşağı açılan listeden **Azure Media Service v3 Environment** ortamını seçin.

    ![Ortamı seçme](./media/develop-with-postman/choose-env.png)
   
### <a name="configure-the-collection"></a>Koleksiyonu yapılandırma

1. Koleksiyon dosyasını içe aktarmak için **İçe Aktar**'ı tıklatın.
1. `https://github.com/Azure-Samples/media-services-v3-rest-postman.git` kopyasını oluşturduğunuzda indirilen `Media Services v3.postman_collection.json` dosyasına gidin
3. **Media Services v3.postman_collection.json** dosyasını seçin.

    ![Dosya içe aktarma](./media/develop-with-postman/postman-import-collection.png)

## <a name="send-requests-using-postman"></a>Postman kullanarak istek gönderme

Bu bölümde, dosyanızı akışla aktarabilmeniz için kodlama ve URL oluşturma ile ilgili istekler göndereceğiz. Özellikle de aşağıdaki istekleri göndereceğiz:

1. Hizmet Sorumlusu Kimlik Doğrulaması için Azure AD Belirteci alma
2. Çıktı varlığı oluşturma
3. Oluşturma bir **dönüştürme**
4. Oluşturma bir **işi**
5. Oluşturma bir **akış Bulucusu**
6. Liste yollarını **akış Bulucu**

> [!Note]
>  Bu öğretici, tüm kaynakları benzersiz adlarla oluşturmakta olduğunuzu varsayar.  

### <a name="get-azure-ad-token"></a>Azure AD Belirteci alma 

1. Postman sol penceresinde, seçin "1. adım: AAD kimlik doğrulaması belirteci alma".
2. Sonra, "Hizmet Sorumlusu Kimlik Doğrulaması için Azure AD Belirteci alma"'yı seçin.
3. **Gönder**’e basın.

    Aşağıdaki **POST** işlemi gönderilir.

    ```
    https://login.microsoftonline.com/:tenantId/oauth2/token
    ```

4. Yanıt belirteç ile gelir ve "AccessToken" ortam değişkenini belirteç değerine ayarlar. "AccessToken"'ı ayarlayan kodu görmek için **Testler** sekmesine tıklayın. 

    ![AAD belirteci alma](./media/develop-with-postman/postman-get-aad-auth-token.png)

### <a name="create-an-output-asset"></a>Çıktı varlığı oluşturma

Çıktı [Varlığı](https://docs.microsoft.com/rest/api/media/assets), kodlama işinizin sonucunu depolar. 

1. Postman'ın sol penceresinde "Varlıklar"'ı seçin.
2. Ardından "Varlık oluşturma veya güncelleştirme"'yi seçin.
3. **Gönder**’e basın.

    * Aşağıdaki **PUT** işlemi gönderilir:

        ```
        https://management.azure.com/subscriptions/:subscriptionId/resourceGroups/:resourceGroupName/providers/Microsoft.Media/mediaServices/:accountName/assets/:assetName?api-version={{api-version}}
        ```
    * İşlemin gövdesi aşağıdaki gibidir:

        ```json
        {
        "properties": {
            "description": "My Asset",
            "alternateId" : "some GUID"
         }
        }
        ```

### <a name="create-a-transform"></a>Dönüşüm oluşturma

Media Services’te içerik kodlarken veya işlerken, kodlama ayarlarını bir tarif olarak ayarlamak yaygın bir modeldir. Daha sonra bu tarifi bir videoya uygulamak üzere bir **İş** gönderirsiniz. Her yeni video için yeni işleri göndererek, kitaplığınızda tüm videoları için söz konusu tarif uyguladığınızı. Media Services içinde tarif, **Dönüşüm** olarak adlandırılır. Daha fazla bilgi için [Dönüşümler ve İşler](transform-concept.md) konusuna bakın. Bu öğreticide açıklanan örnek, videoyu çeşitli iOS ve Android cihazlarına akışla aktarmak için kodlayan bir tarifi tanımlar. 

Yeni bir [Dönüşüm](https://docs.microsoft.com/rest/api/media/transforms) örneği oluştururken çıktı olarak neyi üretmesi istediğinizi belirtmeniz gerekir. Gerekli parametre bir **TransformOutput** nesnesidir. Her **TransformOutput** bir **Ön ayar** içerir. **Ön ayar**, video ve/veya ses işleme işlemlerinin istenen **TransformOutput** nesnesini oluşturmak üzere kullanılacak adım adım yönergelerini açıklar. Bu makalede açıklanan örnek, **AdaptiveStreaming** adlı yerleşik bir Ön Ayar kullanır. Ön Ayar, giriş çözünürlüğü ve bit hızını temel alarak, giriş videosunu otomatik olarak oluşturulan bir bit hızı basamağına (bit hızı-çözünürlük çiftleri) kodlar ve her bir bit hızı-çözünürlük çiftine karşılık gelen H.264 video ve AAC sesi ile ISO MP4 dosyaları üretir. Bu Ön Ayar hakkında bilgi için bkz. [otomatik oluşturulan bit hızı basamağı](autogen-bitrate-ladder.md).

Yerleşik bir EncoderNamedPreset ön ayarını veya özel ön ayarları kullanabilirsiniz. 

> [!Note]
> Bir [Dönüşüm](https://docs.microsoft.com/rest/api/media/transforms) oluştururken, önce **Get** yöntemini kullanarak zaten bir tane olup olmadığını denetlemelisiniz. Bu öğretici, dönüştürmeyi benzersiz bir adla oluşturmakta olduğunuzu varsayar.

1. Postman'in sol penceresinde "Kodlama ve Analiz"'i seçin.
2. Ardından, "Dönüşüm Oluşturma"'yı seçin.
3. **Gönder**’e basın.

    * Aşağıdaki **PUT** işlemi gönderilir.

        ```
        https://management.azure.com/subscriptions/:subscriptionId/resourceGroups/:resourceGroupName/providers/Microsoft.Media/mediaServices/:accountName/transforms/:transformName?api-version={{api-version}}
        ```
    * İşlemin gövdesi aşağıdaki gibidir:

        ```json
        {
            "properties": {
                "description": "Standard Transform using an Adaptive Streaming encoding preset from the library of built-in Standard Encoder presets",
                "outputs": [
                    {
                    "onError": "StopProcessingJob",
                "relativePriority": "Normal",
                    "preset": {
                        "@odata.type": "#Microsoft.Media.BuiltInStandardEncoderPreset",
                        "presetName": "AdaptiveStreaming"
                    }
                    }
                ]
            }
        }
        ```

### <a name="create-a-job"></a>Bir iş oluşturma

Burada [İş](https://docs.microsoft.com/rest/api/media/jobs), oluşturulan **Dönüşümü** belirli bir video girdisine veya ses içeriğine uygulamak için Media Services'e gönderilen istektir. **İş** giriş videosunun konumu ve çıktının konumu gibi bilgileri belirtir.

Bu örnekte, işin girdisini üzerinde bir HTTPS URL'si alır ("https: \/ /nimbuscdn-nimbuspm.streaming.mediaservices.windows.net/2b533311-b215-4409-80af-529c3e853622/").

1. Postman'in sol penceresinde "Kodlama ve Analiz"'i seçin.
2. Ardından "İş Oluşturma veya Güncelleştirme"'yi seçin.
3. **Gönder**’e basın.

    * Aşağıdaki **PUT** işlemi gönderilir.

        ```
        https://management.azure.com/subscriptions/:subscriptionId/resourceGroups/:resourceGroupName/providers/Microsoft.Media/mediaServices/:accountName/transforms/:transformName/jobs/:jobName?api-version={{api-version}}
        ```
    * İşlemin gövdesi aşağıdaki gibidir:

        ```json
        {
        "properties": {
            "input": {
            "@odata.type": "#Microsoft.Media.JobInputHttp",
            "baseUri": "https://nimbuscdn-nimbuspm.streaming.mediaservices.windows.net/2b533311-b215-4409-80af-529c3e853622/",
            "files": [
                    "Ignite-short.mp4"
                ]
            },
            "outputs": [
            {
                "@odata.type": "#Microsoft.Media.JobOutputAsset",
                "assetName": "testAsset1"
            }
            ]
        }
        }
        ```

İşin tamamlanması biraz sürüyor ve tamamlandığında bildirim almak istiyorsunuz. İşin ilerleme durumunu görmek için Event Grid'in kullanılmasını öneririz. Event Grid yüksek kullanılabilirlik, tutarlı performans ve dinamik ölçek için tasarlanmıştır. Event Grid ile uygulamalarınız neredeyse tüm Azure hizmetleri ve özel kaynaklardan gelen olayları takip edip bu olaylara yanıt verebilir. Basit, HTTP tabanlı reaktif olay işleme özelliği, olayların akıllı filtrelenmesi ve yönlendirilmesi sayesinde etkili çözümler oluşturmanıza yardımcı olur.  Bkz. [Olayları özel bir web uç noktasına yönlendirme](job-state-events-cli-how-to.md).

**İş** genellikle şu durumlardan geçer: **Zamanlanmış**, **sıraya alınan**, **işleme**, **tamamlandı** (son durumu). İş bir hatayla karşılaştıysa **Hata** durumunu alırsınız. İş iptal edilme sürecindeyse **İptal Ediliyor** ve **İptal Edildi** durumunu alırsınız.

#### <a name="job-error-codes"></a>İş hata kodları

Bkz: [hata kodları](https://docs.microsoft.com/rest/api/media/jobs/get#joberrorcode).

### <a name="create-a-streaming-locator"></a>Akış bulucusu oluşturma

Kodlama işi tamamlandıktan sonra sonraki adım video çıktısında olmaktır **varlık** kayıttan yürütme için istemciler tarafından kullanılabilir. Bunu iki adımda gerçekleştirebilirsiniz: ilk olarak, oluşturun bir [akış Bulucu](https://docs.microsoft.com/rest/api/media/streaminglocators)ve ikinci olarak, istemcilerin kullandığı akış URL'leri oluşturun. 

Oluşturma işlemi bir **akış Bulucu** yayımlama denir. Varsayılan olarak, **akış Bulucu** API çağrılarını hemen sonra geçerli olduğunu ve isteğe bağlı bir başlangıç ve bitiş zamanlarını yapılandırmadığınız sürece silinene kadar sürer. 

Oluştururken bir [akış Bulucu](https://docs.microsoft.com/rest/api/media/streaminglocators), istenen belirtmeniz gerekecektir **StreamingPolicyName**. Bu örnekte, temiz (şifrelenmemiş) içeriğin akışını yapacağınız için önceden tanımlı temiz akış ilkesi **PredefinedStreamingPolicy.ClearStreamingOnly** kullanılmaktadır.

> [!IMPORTANT]
> Özel [StreamingPolicy](https://docs.microsoft.com/rest/api/media/streamingpolicies)’yi kullanırken Media Service hesabınız için bu tür ilkelerin sınırlı bir kümesini tasarlamanız ve aynı şifreleme seçenekleri ve protokoller gerekli olduğunda StreamingLocators için bunları kullanmanız gerekir. 

Medya hizmeti hesabınızı bir kota sayısı olan **akış ilke** girdileri. Yeni oluşturduğunuz değil **akış ilke** her **akış Bulucu**.

1. Postman'in sol penceresinde "Akış İlkeleri"'ni seçin.
2. Ardından, "Akış Bulucusu Oluşturma"'yı seçin.
3. **Gönder**’e basın.

    * Aşağıdaki **PUT** işlemi gönderilir.

        ```
        https://management.azure.com/subscriptions/:subscriptionId/resourceGroups/:resourceGroupName/providers/Microsoft.Media/mediaServices/:accountName/streamingPolicies/:streamingPolicyName?api-version={{api-version}}
        ```
    * İşlemin gövdesi aşağıdaki gibidir:

        ```json
        {
            "properties":{
            "assetName": "{{assetName}}",
            "streamingPolicyName": "{{streamingPolicyName}}"
            }
        }
        ```

### <a name="list-paths-and-build-streaming-urls"></a>Yolları listeleme ve akış URL'lerini derleme

#### <a name="list-paths"></a>Yolları listeleme

Şimdi [akış Bulucu](https://docs.microsoft.com/rest/api/media/streaminglocators) olmuştur oluşturulmuş akış URL'lerini alabilirsiniz

1. Postman'in sol penceresinde "Akış İlkeleri"'ni seçin.
2. Ardından, "Yolları Listele"'yi seçin.
3. **Gönder**’e basın.

    * Aşağıdaki **POST** işlemi gönderilir.

        ```
        https://management.azure.com/subscriptions/:subscriptionId/resourceGroups/:resourceGroupName/providers/Microsoft.Media/mediaServices/:accountName/streamingLocators/:streamingLocatorName/listPaths?api-version={{api-version}}
        ```
        
    * İşlemin gövdesi yoktur:
        
4. Akış için kullanmak istediğiniz yollardan birini not edin; sonraki bölümde kullanacaksınız. Burada aşağıdaki yollar döndürülür:
    
    ```
    "streamingPaths": [
        {
            "streamingProtocol": "Hls",
            "encryptionScheme": "NoEncryption",
            "paths": [
                "/cdb80234-1d94-42a9-b056-0eefa78e5c63/Ignite-short.ism/manifest(format=m3u8-aapl)"
            ]
        },
        {
            "streamingProtocol": "Dash",
            "encryptionScheme": "NoEncryption",
            "paths": [
                "/cdb80234-1d94-42a9-b056-0eefa78e5c63/Ignite-short.ism/manifest(format=mpd-time-csf)"
            ]
        },
        {
            "streamingProtocol": "SmoothStreaming",
            "encryptionScheme": "NoEncryption",
            "paths": [
                "/cdb80234-1d94-42a9-b056-0eefa78e5c63/Ignite-short.ism/manifest"
            ]
        }
    ]
    ```

#### <a name="build-the-streaming-urls"></a>Akış URL'lerini derleme

Bu bölümde HLS akış URL'sini derleyeceğiz. URL'ler aşağıdaki değerlerden oluşur:

1. Veri gönderme protokolü. Burada "https".

    > [!NOTE]
    > Oynatıcı bir https sitesinde barındırılıyorsa, "https" URL’sini güncelleştirdiğinizden emin olun.

2. StreamingEndpoint'in konak adı. Burada "amsaccount-usw22.streaming.media.azure.net".

    Ana bilgisayar adını almak için aşağıdaki alma işlemi kullanabilirsiniz:
    
    ```
    https://management.azure.com/subscriptions/00000000-0000-0000-0000-0000000000000/resourceGroups/amsResourceGroup/providers/Microsoft.Media/mediaservices/amsaccount/streamingEndpoints/default?api-version={{api-version}}
    ```
    
3. Önceki bölümde (Yolları listeleme) aldığınız yol.  

Sonuç olarak, aşağıdaki HLS URL'si derlenir

```
https://amsaccount-usw22.streaming.media.azure.net/cdb80234-1d94-42a9-b056-0eefa78e5c63/Ignite-short.ism/manifest(format=m3u8-aapl)
```

## <a name="test-the-streaming-url"></a>Akış URL’sini test etme


> [!NOTE]
> Emin **akış uç noktası** istediğiniz gelen akış çalışıyor.

Bu makalede, akışı test etmek için Azure Media Player kullanılmaktadır. 

1. Bir web tarayıcısı açın ve [https://aka.ms/azuremediaplayer/](https://aka.ms/azuremediaplayer/) sayfasına gidin.
2. **URL:** kutusuna derlediğiniz URL'yi yapıştırın. 
3. **Oynatıcıyı Güncelleştir** düğmesine basın.

Azure Media Player, test için kullanılabilir, ancak üretim ortamında kullanılmamalıdır. 

## <a name="clean-up-resources-in-your-media-services-account"></a>Media Services hesabınızdaki kaynakları temizleme

Genel olarak, yeniden kullanmak için planlama nesneler dışında her şeyi temizlemeniz gerekir (genellikle yeniden kullanılacak **dönüştüren**, ve açık kalır **akış bulucuları**vb..). Deneme sonrasında hesabınızın temiz olmasını istiyorsanız, yeniden kullanmayı planlamadığınız kaynakları silmeniz gerekir.  

Bir kaynağı silmek için, silmek istediğiniz kaynağın altından "Sil ..." işlemini seçin.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu öğreticide oluşturduğunuz Media Services ve depolama hesapları dahil olmak üzere, kaynak grubunuzdaki kaynaklardan herhangi birine artık ihtiyacınız yoksa kaynak grubunu silebilirsiniz.  

Aşağıdaki CLI komutunu yürütün:

```azurecli
az group delete --name amsResourceGroup
```

## <a name="provide-feedback"></a>Geri bildirimde bulunma

Kullanıma [Azure Media Services topluluğu](media-services-community.md) soru sorun, görüşlerinizi ve medya hizmetleri hakkında güncelleştirmeler almak farklı yollarını görmek için makaleyi.

## <a name="next-steps"></a>Sonraki adımlar

Videonuzu karşıya yükleme, kodlama ve akışla aktarma hakkında bilgi edindiğinize göre aşağıdaki makaleye bakabilirsiniz: 

> [!div class="nextstepaction"]
> [Videoları analiz etme](analyze-videos-tutorial-with-api.md)
