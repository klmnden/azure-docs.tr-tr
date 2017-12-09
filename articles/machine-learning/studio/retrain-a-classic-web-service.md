---
title: "Klasik web hizmeti yeniden eğitme | Microsoft Docs"
description: "Program aracılığıyla bir modeli yeniden eğitme ve Azure Machine Learning ile yeni eğitilen modelini kullanmak için web hizmetini güncelleştirmek hakkında bilgi edinin."
services: machine-learning
documentationcenter: 
author: garyericson
manager: raymondlaghaeian
editor: 
ms.assetid: e36e1961-9e8b-4801-80ef-46d80b140452
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: v-donglo
ms.openlocfilehash: 75b1862f288152fa2ff4619f807b86f94dc00e3f
ms.sourcegitcommit: 42ee5ea09d9684ed7a71e7974ceb141d525361c9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/09/2017
---
# <a name="retrain-a-classic-web-service"></a>Klasik web hizmetini yeniden eğitme
Dağıttığınız Tahmine dayalı Web Hizmeti uç noktası Puanlama varsayılandır. Varsayılan uç noktalar özgün eğitim ve puanlama denemeler ile eşitlenmiş tutulur ve bu nedenle varsayılan uç noktası için eğitilen model değiştirilemez. Web hizmeti çağırma için yeni bir uç noktası için web hizmeti eklemeniz gerekir. 

## <a name="prerequisites"></a>Ön koşullar
Eğitim denemenizi ve Tahmine dayalı denemeye gösterildiği gibi ayarlamış olmanız gerekir [yeniden eğitme Machine Learning modellerini programlama aracılığıyla](retrain-models-programmatically.md). 

> [!IMPORTANT]
> Tahmine dayalı denemeye Klasik machine learning web hizmeti dağıtılması gerekir. 
> 
> 

Web hizmetleri dağıtma hakkında ek bilgi için bkz: [bir Azure Machine Learning web hizmetini dağıtma](publish-a-machine-learning-web-service.md).

## <a name="add-a-new-endpoint"></a>Yeni bir uç nokta ekleyin
Dağıttığınız Tahmine dayalı Web hizmeti özgün eğitim ile eşitlenmiş olarak saklanmasını endpoint Puanlama ve denemeler eğitilen modeli Puanlama varsayılan içerir. Web hizmetiniz ile yeni bir eğitilen modeli güncelleştirmek için yeni bir Puanlama uç noktası oluşturmanız gerekir. 

Yeni bir Puanlama uç noktası, eğitilen model ile güncelleştirilebilir Tahmine dayalı Web hizmeti oluşturmak için:

> [!NOTE]
> Tahmine dayalı Web hizmeti için eğitim Web Hizmeti uç noktası ekleme emin olun. Eğitim ve Tahmine dayalı bir Web hizmeti doğru olarak dağıttıysanız, listelenen iki ayrı bir web hizmetleri görmeniz gerekir. Tahmine dayalı Web hizmeti, "[Tahmine dayalı exp.]" bitmelidir.
> 
> 

Yeni bir uç noktası bir web hizmeti olarak eklemek iki yolu vardır:

1. Programlama yoluyla
2. Microsoft Azure Web Hizmetleri Portalı'nı kullanın

### <a name="programmatically-add-an-endpoint"></a>Program aracılığıyla bir uç nokta ekleme
Bu konuda sağlanan örnek kodu kullanarak Puanlama uç noktalar ekleyebilirsiniz [github deposunu](https://github.com/raymondlaghaeian/AML_EndpointMgmt/blob/master/Program.cs).

### <a name="use-the-microsoft-azure-web-services-portal-to-add-an-endpoint"></a>Bir uç noktası eklemek için Microsoft Azure Web Hizmetleri Portalı'nı kullanın
1. Machine Learning Studio'da Web Hizmetleri sol gezinti sütunu,'ı tıklatın.
2. Web hizmeti Pano altındaki tıklatın **yönetin uç noktaları Önizleme**.
3. **Ekle**'ye tıklayın.
4. Bir ad ve yeni uç noktası için bir açıklama yazın. Günlüğe kaydetme düzeyi ve örnek verileri etkinleştirilip etkinleştirilmediğini seçin. Günlüğe kaydetme hakkında daha fazla bilgi için bkz: [Machine Learning web hizmetleri için günlüğe kaydetmeyi etkinleştirmek](web-services-logging.md).

## <a name="update-the-added-endpoints-trained-model"></a>Eklenen uç noktanın eğitilen model güncelleştir
Yeniden eğitme işlemini tamamlamak için eklediğiniz yeni uç nokta eğitilen modelini güncelleştirmeniz gerekir.

Örnek kodu kullanarak uç nokta eklediyseniz, bu tarafından tanımlanan Yardım URL'si konumunu içeren *HelpLocationURL* çıktı değeri.

Yol URL'sini almak için:

1. URL'sini kopyalayıp tarayıcınıza yapıştırın.
2. Güncelleştirme kaynağı bağlantısına tıklayın.
3. PATCH isteği gönderme URL'sini kopyalayın. Örneğin:
   
     DÜZELTME EKİ URL: https://management.azureml.net/workspaces/00bf70534500b34rebfa1843d6/webservices/af3er32ad393852f9b30ac9a35b/endpoints/newendpoint2

Daha önce oluşturduğunuz Puanlama uç nokta güncelleştirmek için eğitilen modeli şimdi kullanabilirsiniz.

Aşağıdaki örnek kod, nasıl kullanılacağını gösterir *BaseLocation*, *RelativeLocation*, *SasBlobToken*ve düzeltme eki URL uç noktasını güncelleyin.

    private async Task OverwriteModel()
    {
        var resourceLocations = new
        {
            Resources = new[]
            {
                new
                {
                    Name = "Census Model [trained model]",
                    Location = new AzureBlobDataReference()
                    {
                        BaseLocation = "https://esintussouthsus.blob.core.windows.net/",
                        RelativeLocation = "your endpoint relative location", //from the output, for example: “experimentoutput/8946abfd-79d6-4438-89a9-3e5d109183/8946abfd-79d6-4438-89a9-3e5d109183.ilearner”
                        SasBlobToken = "your endpoint SAS blob token" //from the output, for example: “?sv=2013-08-15&sr=c&sig=37lTTfngRwxCcf94%3D&st=2015-01-30T22%3A53%3A06Z&se=2015-01-31T22%3A58%3A06Z&sp=rl”
                    }
                }
            }
        };

        using (var client = new HttpClient())
        {
            client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", apiKey);

            using (var request = new HttpRequestMessage(new HttpMethod("PATCH"), endpointUrl))
            {
                request.Content = new StringContent(JsonConvert.SerializeObject(resourceLocations), System.Text.Encoding.UTF8, "application/json");
                HttpResponseMessage response = await client.SendAsync(request);

                if (!response.IsSuccessStatusCode)
                {
                    await WriteFailedResponse(response);
                }

                // Do what you want with a successful response here.
            }
        }
    }

*Apikey ile yapılan* ve *endpointUrl* çağrı uç nokta panodan elde edilebilir için.

Değeri *adı* parametresinde *kaynakları* Tahmine dayalı denemeye kaydedilmiş eğitilen modele kaynak adı eşleşmelidir. Kaynak adı almak için:

1. Oturum [Klasik Azure portalı](https://manage.windowsazure.com).
2. Soldaki menüde tıklatın **Machine Learning**.
3. Adı altında çalışma alanına tıklayın ve ardından **Web Hizmetleri**.
4. Adı altında tıklatın **Census modeli [Tahmine dayalı exp.]** .
5. Eklediğiniz yeni uç noktasına tıklayın.
6. Uç nokta Panoda tıklatın **güncelleştirme kaynağı**.
7. Web hizmeti için güncelleştirme kaynağı API belgelerine sayfasında bulabilirsiniz **kaynak adı** altında **güncelleştirilebilir kaynakları**.

Uç noktası güncelleniyor bitirmeden SAS belirteci süresi dolarsa, yeni bir belirteç almak için iş kimliğine sahip bir GET gerçekleştirmeniz gerekir.

Kod başarıyla çalıştırıldı, yeni uç nokta yaklaşık 30 saniye içinde retrained modelini kullanarak başlamanız gerekir.

## <a name="summary"></a>Özet
Yeniden eğitme API'lerini kullanarak, Tahmine dayalı bir Web hizmeti gibi senaryoları etkinleştirme eğitilen modelini güncelleştirebilirsiniz:

* Yeni verilerle yeniden eğitme düzenli modeli.
* Kullanıcıların kendi verilerini kullanarak modeli yeniden eğitme yapmalarına izin amacı ile dağıtım müşteriler için bir modelin.

## <a name="next-steps"></a>Sonraki adımlar
[Bir Azure Machine Learning Klasik web hizmeti yeniden eğitme sorunlarını giderme](troubleshooting-retraining-models.md)

