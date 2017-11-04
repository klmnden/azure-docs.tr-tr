---
title: "Bir Klasik Azure Machine Learning web hizmetini yeniden eğitme sorunlarını giderme | Microsoft Docs"
description: "Tanımlamak ve bir Azure Machine Learning Web hizmeti için modeli yeniden eğitme, ortak sorunları aygıtındaki düzeltin."
services: machine-learning
documentationcenter: 
author: VDonGlover
manager: raymondl
editor: 
ms.assetid: 75cac53c-185c-437d-863a-5d66d871921e
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: v-donglo
ms.openlocfilehash: 85cf9175bb4a5f253c7b47b2edc3ac8b00616ba2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="troubleshooting-the-retraining-of-an-azure-machine-learning-classic-web-service"></a>Bir Azure Machine Learning Klasik Web hizmeti yeniden eğitme sorunlarını giderme
## <a name="retraining-overview"></a>Yeniden eğitme genel bakış
Tahmine dayalı denemeye Puanlama web hizmeti olarak dağıttığınızda statik bir modelidir. Yeni veriler kullanılabilir olduğunda ya da kendi veri tüketici API varsa, model retrained gerekir. 

Klasik Web hizmeti yeniden eğitme işlemini eksiksiz bir anlatım için bkz [yeniden eğitme Machine Learning modellerini program aracılığıyla](retrain-models-programmatically.md).

## <a name="retraining-process"></a>İşlemi yeniden eğitme
Web hizmeti yeniden eğitme gerektiğinde, bazı ek parçalar eklemeniz gerekir:

* Eğitim denemenizi dağıtılan bir Web hizmetidir. Denemeyi olmalıdır bir **Web hizmeti çıkış** modülünün çıkışına bağlı **Train Model** modülü.  
  
    ![Web hizmeti çıkış train model ekleyin.][image1]
* Puanlama Web hizmetiniz için eklenen yeni bir uç noktası.  Program aracılığıyla Machine Learning yeniden eğitme modellerinde program aracılığıyla başvurulan örnek kodu kullanarak uç nokta ekleme konu veya Klasik Azure Portalı aracılığıyla.

Yeniden eğitme modeli için eğitim Web hizmetinin API Yardım sayfası örnek C# kodundan sonra kullanabilirsiniz. Sonuçları değerlendirilen ve bunlarla memnun sonra eklediğiniz yeni uç nokta kullanarak web hizmeti Puanlama eğitilen modeli güncelleştirin.

Tüm parçaları ile yerinde, model yeniden eğitme için uygulamanız gereken önemli adımlar aşağıdaki gibidir:

1. Eğitim Web hizmeti çağrısı: Toplu yürütme hizmeti (BES için), değil istek yanıtı hizmeti (RRS) çağrıdır. Arama yapmak için API Yardım sayfasında örnek C# kodu kullanabilirsiniz. 
2. Değerlerini bulmak *BaseLocation*, *RelativeLocation*, ve *SasBlobToken*: Bu değerleri eğitim Web hizmetine, çağrısından çıktıda döndürülür. 
   ![yeniden eğitme örnek ve BaseLocation, RelativeLocation ve SasBlobToken değerleri çıktısını gösteriliyor.][image6]
3. İle yeni eğitilen modeli Puanlama web hizmetinden eklenen uç noktasını güncelleyin: Machine Learning yeniden eğitme modellerinde program aracılığıyla, sağlanan örnek kodu kullanarak güncelleştirme eklediğiniz yeni eğitilen modeli Puanlama modeliyle yeni uç noktası Eğitim Web hizmeti.

## <a name="common-obstacles"></a>Ortak engellerini
### <a name="check-to-see-if-you-have-the-correct-patch-url"></a>Düzeltme eki URL'nin doğru olup olmadığını denetleyin
Düzeltme eki, kullanmakta olduğunuz URL Puanlama Web hizmetine eklediğiniz yeni Puanlama uç noktasıyla ilişkili bir olmalıdır. Bir düzeltme eki URL'sini elde etmek için çeşitli yöntemler vardır:

**Seçenek 1: programlı şekilde**

Düzeltme eki doğru URL'yi almak için:

1. Çalıştırma [AddEndpoint](https://github.com/raymondlaghaeian/AML_EndpointMgmt/blob/master/Program.cs) örnek kodu.
2. AddEndpoint çıktısından Bul *HelpLocation* değer ve URL'yi kopyalayın.
   
   ![HelpLocation addEndpoint örnek çıktı.][image2]
3. Web hizmeti için Yardım bağlantıları sağlayan sayfaya gitmek için bir tarayıcı URL'sini yapıştırın.
4. Tıklatın **güncelleştirme kaynağı** düzeltme eki Yardım sayfasını açmak için bağlantı.

**Seçenek 2: Klasik Azure portalını kullanın**

1. [Klasik Azure portalında](https://manage.windowsazure.com) oturum açın.
2. Machine Learning sekmesini açın. ![Makine leaning sekmesini tıklatın.][image4]
3. Çalışma alanı adınız ardından **Web Hizmetleri**.
4. Birlikte çalıştığınız Puanlama Web hizmeti tıklatın. (Web hizmeti varsayılan adını değiştirmezseniz bu [Puanlama Exp içinde.] sona erer.)
5. Tıklatın **uç nokta ekleme**.
6. Uç nokta eklendikten sonra uç nokta adına tıklayın. Ardından **güncelleştirme kaynağı** düzeltme eki uygulama Yardım sayfasını açın.

> [!NOTE]
> Tahmine dayalı Web hizmeti yerine eğitim Web hizmeti için uç nokta eklediyseniz, tıklattığınızda aşağıdaki hatayı alırsınız **güncelleştirme kaynağı** bağlantı: özür dileriz, ancak bu özellik desteklenmiyor veya bu kullanılabilir değil bağlamı. Bu Web Hizmeti'nin güncelleştirilebilir kaynağı olmayan. Biz rahatsızlıktan dolayı özür dileriz ve bu iş akışı geliştirmeye çalışıyoruz.
> 
> 

![Yeni bir uç nokta Pano.][image3]

Düzeltme eki Yardım sayfası düzeltme eki kullanmalısınız URL içerir ve bu çağrı için kullanabileceğiniz örnek kod sağlar.

![Düzeltme eki URL'si.][image5]

### <a name="check-to-see-that-you-are-updating-the-correct-scoring-endpoint"></a>Doğru Puanlama uç noktası güncelleştiriliyor denetleyin
* Eğitim Web hizmeti düzeltme eki değil: Puanlama Web hizmetinde düzeltme eki işlemi gerçekleştirilmesi gerekir.
* Web hizmeti varsayılan uç noktada düzeltme eki değil: eklediğiniz yeni Puanlama Web Hizmeti uç noktası üzerinde düzeltme eki işlemi gerçekleştirilmesi gerekir.

Klasik Azure portalını ziyaret ederek uç nokta açıktır hangi Web hizmeti doğrulayabilirsiniz. 

> [!NOTE]
> Tahmine dayalı Web hizmeti için eğitim Web Hizmeti uç noktası ekleme emin olun. Eğitim ve Tahmine dayalı bir Web hizmeti doğru olarak dağıttıysanız, listelenen iki ayrı Web Hizmetleri görmeniz gerekir. Tahmine dayalı Web hizmeti, "[Tahmine dayalı exp.]" bitmelidir.
> 
> 

1. [Klasik Azure portalında](https://manage.windowsazure.com) oturum açın.
2. Machine Learning sekmesini açın. ![Machine learning çalışma alanı UI.][image4]
3. Çalışma alanınızı seçin.
4. Tıklatın **Web Hizmetleri**.
5. Tahmine dayalı Web hizmetinizi seçin.
6. Yeni uç noktanızı Web hizmetine eklendiğini doğrulayın.

### <a name="check-the-workspace-that-your-web-service-is-in-to-ensure-it-is-in-the-correct-region"></a>Web hizmeti doğru bölgede olduğundan emin olmak için bulunduğu çalışma denetleyin
1. [Klasik Azure portalında](https://manage.windowsazure.com) oturum açın.
2. Machine Learning menüsünden seçin.
   ![Machine learning bölge UI.][image4]
3. Çalışma alanınızı konumunu doğrulayın.

<!-- Image Links -->

[image1]: ./media/troubleshooting-retraining-a-model/ml-studio-tm-connnected-to-web-service-out.png
[image2]: ./media/troubleshooting-retraining-a-model/addEndpoint-output.png
[image3]: ./media/troubleshooting-retraining-a-model/azure-portal-update-resource.png
[image4]: ./media/troubleshooting-retraining-a-model/azure-portal-machine-learning-tab.png
[image5]: ./media/troubleshooting-retraining-a-model/ml-help-page-patch-url.png
[image6]: ./media/troubleshooting-retraining-a-model/retraining-output.png
[image7]: ./media/troubleshooting-retraining-a-model/web-services-tab.png
