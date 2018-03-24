---
title: Bir Klasik Azure Machine Learning web hizmetini yeniden eğitme sorunlarını giderme | Microsoft Docs
description: Tanımlamak ve bir Azure Machine Learning Web hizmeti için modeli yeniden eğitme, ortak sorunları aygıtındaki düzeltin.
services: machine-learning
documentationcenter: ''
author: YasinMSFT
ms.author: yahajiza
manager: hjerez
editor: cgronlun
ms.assetid: 75cac53c-185c-437d-863a-5d66d871921e
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 011/01/2017
ms.openlocfilehash: f5b826decd360ea0e8c2394c4205fafcf6cb16d0
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="troubleshooting-the-retraining-of-an-azure-machine-learning-classic-web-service"></a>Bir Klasik Azure Machine Learning web hizmeti yeniden eğitme sorunlarını giderme
## <a name="retraining-overview"></a>Yeniden eğitme genel bakış
Tahmine dayalı denemeye Puanlama web hizmeti olarak dağıttığınızda statik bir modelidir. Yeni veriler kullanılabilir olduğunda ya da kendi veri tüketici API varsa, model retrained gerekir. 

Klasik web hizmeti yeniden eğitme işlemini eksiksiz bir anlatım için bkz [yeniden eğitme Machine Learning modellerini program aracılığıyla](retrain-models-programmatically.md).

## <a name="retraining-process"></a>İşlemi yeniden eğitme
Web hizmeti yeniden eğitme gerektiğinde, bazı ek parçalar eklemeniz gerekir:

* Eğitim denemenizi dağıtılan bir web hizmetidir. Denemeyi olmalıdır bir **Web hizmeti çıkış** modülünün çıkışına bağlı **Train Model** modülü.  
  
    ![Web hizmeti çıkış train model ekleyin.][image1]
* Puanlama web hizmetiniz için eklenen yeni bir uç noktası.  Program aracılığıyla Machine Learning yeniden eğitme modellerinde program aracılığıyla başvurulan örnek kodu kullanarak uç nokta ekleme konu veya Azure Machine Learning Web Hizmetleri Portalı aracılığıyla.

Yeniden eğitme modeli için eğitim Web hizmetinin API Yardım sayfası örnek C# kodundan sonra kullanabilirsiniz. Sonuçları değerlendirilen ve bunlarla memnun sonra eklediğiniz yeni uç nokta kullanarak web hizmeti Puanlama eğitilen modeli güncelleştirin.

Tüm parçaları ile yerinde, model yeniden eğitme için uygulamanız gereken önemli adımlar aşağıdaki gibidir:

1. Eğitim Web hizmeti çağrısı: Toplu yürütme hizmeti (BES için), değil istek yanıtı hizmeti (RRS) çağrıdır. Arama yapmak için API Yardım sayfasında örnek C# kodu kullanabilirsiniz. 
2. Değerlerini bulmak *BaseLocation*, *RelativeLocation*, ve *SasBlobToken*: Bu değerleri eğitim Web hizmetine, çağrısından çıktıda döndürülür. 
   ![yeniden eğitme örnek ve BaseLocation, RelativeLocation ve SasBlobToken değerleri çıktısını gösteriliyor.][image6]
3. İle yeni eğitilen modeli Puanlama web hizmetinden eklenen uç noktasını güncelleyin: Machine Learning yeniden eğitme modellerinde program aracılığıyla, sağlanan örnek kodu kullanarak güncelleştirme eklediğiniz yeni eğitilen modeli Puanlama modeliyle yeni uç noktası Eğitim Web hizmeti.

## <a name="common-obstacles"></a>Ortak engellerini
### <a name="check-to-see-if-you-have-the-correct-patch-url"></a>Düzeltme eki URL'nin doğru olup olmadığını denetleyin
Düzeltme eki, kullanmakta olduğunuz URL Puanlama web hizmetine eklediğiniz yeni Puanlama uç noktasıyla ilişkili bir olmalıdır. Bir düzeltme eki URL'sini elde etmek için çeşitli yöntemler vardır:

**Seçenek 1: programlı şekilde**

Düzeltme eki doğru URL'yi almak için:

1. Çalıştırma [AddEndpoint](https://github.com/raymondlaghaeian/AML_EndpointMgmt/blob/master/Program.cs) örnek kodu.
2. AddEndpoint çıktısından Bul *HelpLocation* değer ve URL'yi kopyalayın.
   
   ![HelpLocation addEndpoint örnek çıktı.][image2]
3. Web hizmeti için Yardım bağlantıları sağlayan sayfaya gitmek için bir tarayıcı URL'sini yapıştırın.
4. Tıklatın **güncelleştirme kaynağı** düzeltme eki Yardım sayfasını açmak için bağlantı.

**Seçenek 2: Azure Machine Learning Web Hizmetleri Portalı'nı kullanın**

1. Oturum [Azure Machine Learning Web Hizmetleri](https://services.azureml.net/) portal.
2. Tıklatın **Web Hizmetleri** veya **Klasik Web Hizmetleri** üstünde.
4. Çalıştığınız Puanlama web hizmeti (web hizmeti varsayılan adını değiştirirseniz alamadık, "[Puanlama Exp içinde.]" sona erer).
5. Tıklatın **+ yeni**.
6. Uç nokta eklendikten sonra uç nokta adına tıklayın.
7. Altında **düzeltme eki** URL'yi tıklatın **API Yardım** düzeltme eki uygulama Yardım sayfasını açın.

> [!NOTE]
> Tahmine dayalı Web hizmeti yerine eğitim Web hizmeti için uç nokta eklediyseniz, tıklattığınızda aşağıdaki hatayı alırsınız **güncelleştirme kaynağı** bağlantı: "özür dileriz değildir, ancak bu özellik kullanılabilir veya desteklenir Bu bağlamı. Bu Web Hizmeti'nin güncelleştirilebilir kaynağı olmayan. Biz rahatsızlıktan dolayı özür dileriz ve bu iş akışı geliştirmeye çalışıyoruz."
> 
> 

Düzeltme eki Yardım sayfası düzeltme eki kullanmalısınız URL içerir ve bu çağrı için kullanabileceğiniz örnek kod sağlar.

![Düzeltme eki URL'si.][image5]

### <a name="check-to-see-that-you-are-updating-the-correct-scoring-endpoint"></a>Doğru Puanlama uç noktası güncelleştiriliyor denetleyin
* Eğitim web hizmeti düzeltme eki değil: Puanlama web hizmetinde düzeltme eki işlemi gerçekleştirilmesi gerekir.
* Web hizmeti varsayılan uç noktada düzeltme eki değil: eklediğiniz yeni Puanlama web hizmeti uç noktası üzerinde düzeltme eki işlemi gerçekleştirilmesi gerekir.

Web Hizmetleri portalını ziyaret ederek uç nokta açıktır hangi web hizmeti doğrulayabilirsiniz. 

> [!NOTE]
> Tahmine dayalı Web hizmeti için eğitim Web Hizmeti uç noktası ekleme emin olun. Eğitim ve Tahmine dayalı bir Web hizmeti doğru olarak dağıttıysanız, listelenen iki ayrı bir web hizmetleri görmeniz gerekir. Tahmine dayalı Web hizmeti, "[Tahmine dayalı exp.]" bitmelidir.
> 
> 

1. Oturum [Azure Machine Learning Web Hizmetleri](https://services.azureml.net/) portal.
2. Tıklatın **Web Hizmetleri** veya **Klasik Web Hizmetleri**.
3. Tahmine dayalı Web hizmetinizi seçin.
4. Yeni uç noktanızı web hizmetine eklendiğini doğrulayın.

### <a name="check-that-your-workspace-is-in-the-same-region-as-the-web-service"></a>Çalışma alanınızı web hizmeti ile aynı bölgede olup olmadığını denetleyin
1. Oturum [Studio makine](https://studio.azureml.net/).
2. En üstte alanlarınızı aşağı açılan listesi tıklatın.

   ![Machine learning bölge UI.][image4]

3. Çalışma alanınızı bulunduğu bölgeyi doğrulayın.

<!-- Image Links -->

[image1]: ./media/troubleshooting-retraining-a-model/ml-studio-tm-connnected-to-web-service-out.png
[image2]: ./media/troubleshooting-retraining-a-model/addEndpoint-output.png
[image3]: ./media/troubleshooting-retraining-a-model/azure-portal-update-resource.png
[image4]: ./media/troubleshooting-retraining-a-model/check-workspace-region.png
[image5]: ./media/troubleshooting-retraining-a-model/ml-help-page-patch-url.png
[image6]: ./media/troubleshooting-retraining-a-model/retraining-output.png
[image7]: ./media/troubleshooting-retraining-a-model/web-services-tab.png
