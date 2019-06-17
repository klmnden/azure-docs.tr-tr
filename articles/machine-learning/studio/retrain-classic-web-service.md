---
title: Klasik bir web hizmetini tekrar eğitip dağıtma
titleSuffix: Azure Machine Learning Studio
description: Modeli yeniden eğitme ve Azure Machine Learning Studio'da eğitim yeni modeli kullanmak için bir Klasik web hizmetini güncelleştirmek hakkında bilgi edinin.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: peterclu
ms.author: amlstudiodocs
ms.custom: seodec18, previous-ms.author=yahajiza, previous-author=YasinMSFT
ms.date: 02/14/2019
ms.openlocfilehash: b636883ee1f08fa0fb6d080b6980cd07553dde1b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65234054"
---
# <a name="retrain-and-deploy-a-classic-studio-web-service"></a>Yeniden eğitme ve klasik Studio web hizmeti dağıtma

Makine öğrenimi modelleri yeniden eğitme doğru ve göre en alakalı verileri kullanılabilir kalmasını sağlamak için bir yoludur. Bu makalede Klasik Studio web hizmeti yeniden eğitme yapmayı gösterir. Yeni bir Studio web hizmetini yeniden eğitme hakkında bir kılavuz için [bu nasıl yapılır makalesine bakın.](retrain-machine-learning-model.md)

## <a name="prerequisites"></a>Önkoşullar

Bu makalede, özel olarak yeniden eğitme deneme hem öngörücü bir denemeye zaten sahip olduğunuzu varsayar. Bu adımları açıklanmıştır [yeniden eğitme ve makine öğrenme modeli dağıtın.](/azure/machine-learning/studio/retrain-machine-learning-model) Ancak, yeni bir web hizmeti olarak makine öğrenimi modelinizi dağıtmak yerine, Tahmine dayalı denemeye Klasik web hizmeti olarak dağıtır.
     
## <a name="add-a-new-endpoint"></a>Yeni bir uç nokta Ekle

Özgün eğitim ile eşitlenmiş olarak tutulur, uç nokta Puanlama ve denemeleri eğitilen model Puanlama varsayılan dağıttığınız Tahmine dayalı web hizmeti içerir. Web hizmetiniz için yeni bir eğitilen modeli güncelleştirmek için yeni bir Puanlama uç noktası oluşturmanız gerekir.

Yeni bir uç noktası için bir web hizmeti olarak Ekle iki yolu vardır:

* Programlı olarak
* Azure Web Hizmetleri portalını kullanma

> [!NOTE]
> Tahmine dayalı Web hizmeti için eğitim Web Hizmeti uç noktası ekleme emin olun. Hem eğitim hem de bir Tahmine dayalı Web hizmeti doğru olarak dağıttıysanız, listelenen iki ayrı web hizmetlerini görmelisiniz. Tahmine dayalı Web hizmeti "[Tahmine dayalı ifade ile.]" ile bitmelidir.
>

### <a name="programmatically-add-an-endpoint"></a>Program aracılığıyla bir uç nokta ekleme

Bu konuda sağlanan örnek kodu kullanarak Puanlama uç noktalar ekleyebilirsiniz [GitHub deposu](https://github.com/hning86/azuremlps#add-amlwebserviceendpoint).

### <a name="use-the-azure-web-services-portal-to-add-an-endpoint"></a>Bir uç nokta eklemek için Azure Web Hizmetleri portalını kullanma

1. Machine Learning Studio'da, sol gezinti sütununda, Web Hizmetleri.
1. Web hizmeti Pano altındaki tıklatın **yönetin uç noktaları Önizleme**.
1. **Ekle**'yi tıklatın.
1. Bir ad ve yeni uç nokta için bir açıklama yazın. Günlüğe kaydetme düzeyini ve örnek veriler etkin olup olmadığını seçin. Günlüğe kaydetme hakkında daha fazla bilgi için bkz. [Machine Learning web hizmetleri için günlüğe kaydetmeyi etkinleştirme](web-services-logging.md).

## <a name="update-the-added-endpoints-trained-model"></a>Eklenen uç noktanın eğitilen modeli güncelleştirme

### <a name="retrieve-patch-url"></a>Düzeltme eki URL'sini alın

### <a name="option-1-programmatically"></a>1\. seçenek: Programlı olarak

Düzeltme eki URL'sini doğru programlı olarak almak için şu adımları izleyin:

1. Çalıştırma [AddEndpoint](https://github.com/raymondlaghaeian/AML_EndpointMgmt/blob/master/Program.cs) örnek kodu.
1. AddEndpoint çıktısından Bul *HelpLocation* değeri ve URL'yi kopyalayın.

   ![HelpLocation addEndpoint örnek çıktı.](./media/retrain-classic/addEndpoint-output.png)
1. URL, Yardım bağlantıları için web hizmeti sağlayan bir sayfaya gitmek için bir tarayıcıya yapıştırın.
1. Tıklayın **kaynak güncelleştirme** düzeltme eki Yardım sayfasını açmak için bağlantı.

### <a name="option-2-use-the-azure-machine-learning-web-services-portal"></a>2\. seçenek: Azure Machine Learning Web Hizmetleri portalını kullanma

Doğru düzeltme eki using the web portal URL almak için aşağıdaki adımları izleyin:

1. Oturum [Azure Machine Learning Web Hizmetleri](https://services.azureml.net/) portalı.
1. Tıklayın **Web Hizmetleri** veya **Klasik Web Hizmetleri** en üstünde.
1. İle çalışırken Puanlama web hizmeti (web hizmeti varsayılan adını değiştirirseniz olmadı "[Puanlama ifade içinde.]" sonunda).
1. Tıklayın **+ yeni**.
1. Uç nokta eklendikten sonra uç nokta adına tıklayın.
1. Altında **düzeltme eki** URL'yi tıklatın **API Yardım** düzeltme eki uygulama Yardım sayfasını açın.

> [!NOTE]
> Tahmine dayalı Web hizmeti yerine eğitim Web hizmeti için uç nokta eklediyseniz tıkladığınızda aşağıdaki hatayı alırsınız **kaynak güncelleştirme** bağlantı: "Üzgünüz, ancak bu özellik desteklenmez ve bu bağlamda kullanılamaz. Bu Web hizmetini güncelleştirilebilir hiçbir kaynak vardır. Biz Verdiğimiz rahatsızlık için özür dileriz ve bu iş akışı geliştirme konusunda durmaksızın çalışıyoruz."
>

Düzeltme eki yardım sayfasına düzeltme eki kullanmalısınız URL içerir ve onu çağırmak için kullanabileceğiniz örnek kodu sağlar.

![Düzeltme URL'si.](./media/retrain-classic/ml-help-page-patch-url.png)

### <a name="update-the-endpoint"></a>Uç noktası güncellenemedi

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
1. Sol menüde **Machine Learning**.
1. Adı altında çalışma alanınızı tıklayın ve ardından **Web Hizmetleri**.
1. Adı altında tıklatın **Görselleştirmenizdeki modeli [Tahmine dayalı exp.]** .
1. Eklediğiniz yeni uç noktaya tıklayın.
1. Uç nokta panosunda **kaynak güncelleştirme**.
1. Web hizmeti için güncelleştirme kaynağı API belgeleri sayfasında bulabilirsiniz **kaynak adı** altında **güncelleştirilebilir kaynakları**.

Değeriniz SAS belirtecinizle uç noktası güncelleniyor sonlandırmadan önce dolarsa, yeni bir belirteç almak için iş Kimliğine sahip bir GET gerçekleştirmeniz gerekir.

Kod başarıyla çalıştıktan sonra yeni uç nokta yaklaşık 30 saniye içinde retrained modeli kullanarak başlamanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Web hizmetlerini yönetme veya birden çok denemeleri çalıştırması izlemek hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

* [Web Hizmetleri portalını keşfedin](manage-new-webservice.md)
* [Deneme yinelemelerini yönetme](manage-experiment-iterations.md)