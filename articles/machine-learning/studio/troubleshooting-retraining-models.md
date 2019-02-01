---
title: Bir Machine Learning Studio Klasik web hizmetini yeniden eğitme sorunlarını giderme
titleSuffix: Azure Machine Learning Studio
description: Tanımlamak ve bir Azure Machine Learning Studio web hizmeti için modeli yeniden eğitme karşılaşılan genel sorunları düzeltin.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: article
author: ericlicoding
ms.author: amlstudiodocs
ms.custom: previous-ms.author=yahajiza, previous-author=YasinMSFT
ms.date: 11/01/2017
ms.openlocfilehash: 6cde9d929c52984c95669554aa1153c2bdf21131
ms.sourcegitcommit: fea5a47f2fee25f35612ddd583e955c3e8430a95
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2019
ms.locfileid: "55512161"
---
# <a name="troubleshooting-the-retraining-of-an-azure-machine-learning-studio-classic-web-service"></a>Yeniden eğitme bir Azure Machine Learning Studio Klasik web hizmeti sorunlarını giderme
## <a name="retraining-overview"></a>Yeniden eğitme genel bakış
Bir Puanlama web hizmeti olarak öngörücü bir denemeye dağıtırken statik bir modeldir. Yeni veriler kullanılabilir olduğunda ya da kendi veri tüketici API'si varsa, model eğitilebileceği gerekir. 

Klasik web hizmetini yeniden eğitme sürecinin tamamını bir kılavuz için bkz. [yeniden eğitme Machine Learning modellerini programlı](retrain-models-programmatically.md).

## <a name="retraining-process"></a>İşlem yeniden eğitme
Web hizmetini yeniden eğitme gerektiğinde, bazı ek parçaları eklemeniz gerekir:

* Eğitim deneme dağıtılan bir web hizmetidir. Denemeyi olmalıdır bir **Web hizmeti çıkış** modülünün çıkışına bağlı **modeli eğitme** modülü.  
  
    ![Web hizmeti çıkış için modeli eğitme ekleyin.][image1]
* Puanlama web hizmetiniz için eklenen yeni bir uç noktası.  Programlama yoluyla yeniden eğitme Machine Learning modelleri programlı olarak başvurulan örnek kodu kullanarak uç nokta ekleyebilirsiniz konu veya Azure Machine Learning Web Hizmetleri portalından.

Örnek daha sonra kullanabileceğiniz C# kod modeli yeniden eğitme eğitim Web Service'in API Yardım sayfasında. Sonuçları değerlendirilen ve bunlarla memnun olana sonra eğitilen model Puanlama web hizmeti eklemiş olduğunuz yeni uç nokta kullanarak güncelleştirin.

Tüm parçaları ile yerinde, modeli yeniden eğitme için gerçekleştirmeniz gereken önemli adımlar aşağıdaki gibidir:

1. Eğitim Web hizmetini çağırın:  Toplu yürütme hizmeti (BES için), değil istek yanıt hizmeti (RRS) çağrısıdır. Örnek kullanabileceğiniz C# kod arama yapmak için API Yardım sayfasında. 
2. Değerlerini bulma *BaseLocation*, *RelativeLocation*, ve *SasBlobToken*: Bu değerler, eğitim Web hizmetine çağrı çıkışında döndürülen. 
   ![yeniden eğitme örnek ve BaseLocation RelativeLocation ve SasBlobToken değerlerin çıkışını gösteriliyor.][image6]
3. Yeni eğitim modeli ile eklenen uç noktasından Puanlama web hizmeti güncelleştirin: Machine Learning yeniden eğitme modelleri programlı olarak sağlanan örnek kodu kullanarak, yeni eğitim modeli Puanlama modeli için eğitim Web hizmetinden eklediğiniz yeni uç nokta güncelleştirin.

## <a name="common-obstacles"></a>Sık karşılaşılan engelleri
### <a name="check-to-see-if-you-have-the-correct-patch-url"></a>Düzeltme eki URL'nin doğru olup olmadığını denetleyin
Düzeltme eki kullandığınız URL bir Puanlama web hizmeti için eklenen yeni Puanlama uç noktası ile ilişkili olması gerekir. Düzeltme URL'si almak için çeşitli yollarla vardır:

**1. seçenek: Program aracılığıyla**

Düzeltme eki URL'sini doğru almak için:

1. Çalıştırma [AddEndpoint](https://github.com/raymondlaghaeian/AML_EndpointMgmt/blob/master/Program.cs) örnek kodu.
2. AddEndpoint çıktısından Bul *HelpLocation* değeri ve URL'yi kopyalayın.
   
   ![HelpLocation addEndpoint örnek çıktı.][image2]
3. URL, Yardım bağlantıları için web hizmeti sağlayan bir sayfaya gitmek için bir tarayıcıya yapıştırın.
4. Tıklayın **kaynak güncelleştirme** düzeltme eki Yardım sayfasını açmak için bağlantı.

**2. seçenek: Azure Machine Learning Web Hizmetleri portalını kullanma**

1. Oturum [Azure Machine Learning Web Hizmetleri](https://services.azureml.net/) portalı.
2. Tıklayın **Web Hizmetleri** veya **Klasik Web Hizmetleri** en üstünde.
4. Birlikte çalıştığınız Puanlama web hizmeti (web hizmeti varsayılan adını değiştirirseniz olmadı "[Puanlama ifade içinde.]" sonunda).
5. Tıklayın **+ yeni**.
6. Uç nokta eklendikten sonra uç nokta adına tıklayın.
7. Altında **düzeltme eki** URL'yi tıklatın **API Yardım** düzeltme eki uygulama Yardım sayfasını açın.

> [!NOTE]
> Tahmine dayalı Web hizmeti yerine eğitim Web hizmeti için uç nokta eklediyseniz tıkladığınızda aşağıdaki hatayı alırsınız **kaynak güncelleştirme** bağlantı: "Üzgünüz, ancak bu özellik desteklenmez ve bu bağlamda kullanılamaz. Bu Web hizmetini güncelleştirilebilir hiçbir kaynak vardır. Biz Verdiğimiz rahatsızlık için özür dileriz ve bu iş akışı geliştirme konusunda durmaksızın çalışıyoruz."
> 
> 

Düzeltme eki yardım sayfasına düzeltme eki kullanmalısınız URL içerir ve onu çağırmak için kullanabileceğiniz örnek kodu sağlar.

![Düzeltme URL'si.][image5]

### <a name="check-to-see-that-you-are-updating-the-correct-scoring-endpoint"></a>Doğru Puanlama uç noktası güncelleştiriliyor denetleyin
* Eğitim web hizmeti eki değil: Puanlama web hizmeti düzeltme eki işlemi gerçekleştirilmesi gerekir.
* Varsayılan uç nokta web Service'i düzeltme değil: Eklediğiniz yeni Puanlama web hizmeti uç noktası üzerinde düzeltme eki işlemi gerçekleştirilmesi gerekir.

Hangi web hizmeti Web Hizmetleri portalını ziyaret ederek uç nokta açıktır doğrulayabilirsiniz. 

> [!NOTE]
> Tahmine dayalı Web hizmeti için eğitim Web Hizmeti uç noktası ekleme emin olun. Hem eğitim hem de bir Tahmine dayalı Web hizmeti doğru olarak dağıttıysanız, listelenen iki ayrı web hizmetlerini görmelisiniz. Tahmine dayalı Web hizmeti "[Tahmine dayalı ifade ile.]" ile bitmelidir.
> 
> 

1. Oturum [Azure Machine Learning Web Hizmetleri](https://services.azureml.net/) portalı.
2. Tıklayın **Web Hizmetleri** veya **Klasik Web Hizmetleri**.
3. Tahmine dayalı Web hizmetinizi seçin.
4. Yeni uç noktanız için web hizmeti eklendiğini doğrulayın.

### <a name="check-that-your-workspace-is-in-the-same-region-as-the-web-service"></a>Web hizmeti ile aynı bölgede çalışma alanınız olup olmadığını denetleyin
1. Oturum [Machine Learning Studio'da](https://studio.azureml.net/).
2. Çalışma alanlarının aşağı açılan listenin en üstünde, tıklayın.

   ![Machine learning bölge kullanıcı Arabirimi.][image4]

3. Çalışma alanınızı bulunduğu bölgeyi doğrulayın.

<!-- Image Links -->

[image1]: ./media/troubleshooting-retraining-a-model/ml-studio-tm-connected-to-web-service-out.png
[image2]: ./media/troubleshooting-retraining-a-model/addEndpoint-output.png
[image3]: ./media/troubleshooting-retraining-a-model/azure-portal-update-resource.png
[image4]: ./media/troubleshooting-retraining-a-model/check-workspace-region.png
[image5]: ./media/troubleshooting-retraining-a-model/ml-help-page-patch-url.png
[image6]: ./media/troubleshooting-retraining-a-model/retraining-output.png
[image7]: ./media/troubleshooting-retraining-a-model/web-services-tab.png
