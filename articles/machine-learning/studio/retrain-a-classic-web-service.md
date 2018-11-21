---
title: Klasik web hizmetini yeniden eğitme | Microsoft Docs
description: Program aracılığıyla bir modeli yeniden eğitme ve Azure Machine Learning'de yeni eğitim modeli kullanmak için web hizmetini güncelleştirmek hakkında bilgi edinin.
services: machine-learning
documentationcenter: ''
author: ericlicoding
ms.custom: (previous ms.author=yahajiza, author=YasinMSFT)
ms.author: amlstudiodocs
manager: hjerez
editor: cgronlun
ms.assetid: e36e1961-9e8b-4801-80ef-46d80b140452
ms.service: machine-learning
ms.component: studio
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.openlocfilehash: bd8fb59390bc54e5819183d13f16a557d62ec7ce
ms.sourcegitcommit: fa758779501c8a11d98f8cacb15a3cc76e9d38ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2018
ms.locfileid: "52260758"
---
# <a name="retrain-a-classic-web-service"></a>Klasik web hizmetini yeniden eğitme
Dağıttığınız Tahmine dayalı Web Hizmeti uç noktası Puanlama varsayılandır. Varsayılan uç noktalar özgün eğitim ve puanlama denemeleri ile eşitlenmiş olarak tutulur ve bu nedenle varsayılan uç nokta için eğitilen model değiştirilemez. Web hizmetini yeniden eğitme için web hizmetine yeni bir uç noktası eklemeniz gerekir. 

## <a name="prerequisites"></a>Önkoşullar
Size bir eğitim denemesini ve Tahmine dayalı denemeye gösterildiği ayarlamış olmanız gerekir [yeniden eğitme Machine Learning modellerini programlama aracılığıyla](retrain-models-programmatically.md). 

> [!IMPORTANT]
> Tahmine dayalı denemeye Klasik machine learning web hizmeti dağıtılması gerekir. 
> 
> 

Web hizmetlerini dağıtma hakkında ek bilgi için bkz. [bir Azure Machine Learning web hizmetini dağıtma](publish-a-machine-learning-web-service.md).

## <a name="add-a-new-endpoint"></a>Yeni bir uç nokta Ekle
Özgün eğitim ile eşitlenmiş olarak tutulur, uç nokta Puanlama ve denemeleri eğitilen model Puanlama varsayılan dağıttığınız Tahmine dayalı Web hizmeti içerir. Web hizmetiniz için yeni bir eğitilen modeli güncelleştirmek için yeni bir Puanlama uç noktası oluşturmanız gerekir. 

Yeni bir Puanlama uç noktası, eğitilen modeli ile güncelleştirilebilir Tahmine dayalı Web hizmeti oluşturmak için:

> [!NOTE]
> Tahmine dayalı Web hizmeti için eğitim Web Hizmeti uç noktası ekleme emin olun. Hem eğitim hem de bir Tahmine dayalı Web hizmeti doğru olarak dağıttıysanız, listelenen iki ayrı web hizmetlerini görmelisiniz. Tahmine dayalı Web hizmeti "[Tahmine dayalı ifade ile.]" ile bitmelidir.
> 
> 

Yeni bir uç noktası için bir web hizmeti olarak Ekle iki yolu vardır:

1. Programlı olarak
2. Microsoft Azure Web Hizmetleri portalını kullanma

### <a name="programmatically-add-an-endpoint"></a>Program aracılığıyla bir uç nokta ekleme
Bu konuda sağlanan örnek kodu kullanarak Puanlama uç noktalar ekleyebilirsiniz [github deposu](https://github.com/hning86/azuremlps#add-amlwebserviceendpoint).

### <a name="use-the-microsoft-azure-web-services-portal-to-add-an-endpoint"></a>Bir uç nokta eklemek için Microsoft Azure Web Hizmetleri portalını kullanma
1. Machine Learning Studio'da, sol gezinti sütununda, Web Hizmetleri.
2. Web hizmeti Pano altındaki tıklatın **yönetin uç noktaları Önizleme**.
3. **Ekle**'ye tıklayın.
4. Bir ad ve yeni uç nokta için bir açıklama yazın. Günlüğe kaydetme düzeyini ve örnek veriler etkin olup olmadığını seçin. Günlüğe kaydetme hakkında daha fazla bilgi için bkz. [Machine Learning web hizmetleri için günlüğe kaydetmeyi etkinleştirme](web-services-logging.md).

## <a name="update-the-added-endpoints-trained-model"></a>Eklenen uç noktanın eğitilen modeli güncelleştirme
Yeniden eğitme işlemini tamamlamak için eğitilen modeli eklemiş olduğunuz yeni uç nokta güncelleştirmeniz gerekir.

Örnek kodu kullanarak uç nokta eklediyseniz, bu tarafından tanımlanan Yardım URL'si konumunu içerir *HelpLocationURL* çıktı değeri.

Yol URL'si almak için:

1. URL'sini kopyalayıp tarayıcınıza yapıştırın.
2. Güncelleştirme kaynağı bağlantıya tıklayın.
3. PATCH isteği POST URL'sini kopyalayın. Örneğin:
   
     DÜZELTME URL'Sİ: https://management.azureml.net/workspaces/00bf70534500b34rebfa1843d6/webservices/af3er32ad393852f9b30ac9a35b/endpoints/newendpoint2

Artık, daha önce oluşturduğunuz Puanlama uç noktasını güncelleştirmek için eğitilen modeli kullanabilirsiniz.

Aşağıdaki örnek kod, nasıl kullanılacağını gösterir *BaseLocation*, *RelativeLocation*, *SasBlobToken*ve URL uç noktasını güncelleştirmek için düzeltme eki.

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

*ApiKey* ve *endpointUrl* için uç nokta panodan arama alınabilir.

Değerini *adı* parametresinde *kaynakları* Tahmine dayalı denemeye kaydedilmiş eğitilen modele kaynak adı ile eşleşmelidir. Kaynak adı almak için:

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Sol menüde **Machine Learning**.
3. Adı altında çalışma alanınızı tıklayın ve ardından **Web Hizmetleri**.
4. Adı altında tıklatın **Görselleştirmenizdeki modeli [Tahmine dayalı exp.]** .
5. Eklediğiniz yeni uç noktaya tıklayın.
6. Uç nokta panosunda **kaynak güncelleştirme**.
7. Web hizmeti için güncelleştirme kaynağı API belgeleri sayfasında bulabilirsiniz **kaynak adı** altında **güncelleştirilebilir kaynakları**.

Değeriniz SAS belirtecinizle uç noktası güncelleniyor sonlandırmadan önce dolarsa, yeni bir belirteç almak için iş kimliğine sahip bir GET gerçekleştirmeniz gerekir.

Kod başarıyla çalıştıktan sonra yeni uç nokta yaklaşık 30 saniye içinde retrained modeli kullanarak başlamanız gerekir.

## <a name="summary"></a>Özet
Yeniden eğitme API'lerini kullanarak, bir Tahmine dayalı Web hizmeti gibi senaryoları etkinleştiren eğitilen modelini güncelleştirebilirsiniz:

* Yeni veriler ile yeniden eğitme düzenli modeli.
* Bunları kendi verilerini kullanarak modeli yeniden eğitme izin vermek amacıyla müşteriler için bir model dağıtımı.

## <a name="next-steps"></a>Sonraki adımlar
[Sorun giderme bir Azure Machine Learning Klasik web hizmetini yeniden eğitme](troubleshooting-retraining-models.md)

