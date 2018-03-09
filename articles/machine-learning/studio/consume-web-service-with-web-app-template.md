---
title: "Bir web uygulaması şablonu kullanarak bir Machine Learning web hizmetini kullanma | Microsoft Docs"
description: "Azure Machine Learning Tahmine dayalı web hizmetinde kullanmak için Azure Marketi'nde bir web uygulaması şablonu kullanın."
keywords: "Web hizmeti, operationalization, REST API makine öğrenme"
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: e0d71683-61b9-4675-8df5-09ddc2f0d92d
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2017
ms.author: raymondl
ms.openlocfilehash: f7efa647fa6afc247509cd4a52066c0459f75ca3
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/08/2018
---
# <a name="consume-an-azure-machine-learning-web-service-by-using-a-web-app-template"></a>Bir web uygulaması şablonu kullanarak bir Azure Machine Learning web hizmetini kullanma

Tahmine dayalı bir modeli geliştirmek ve kullanarak bir Azure web hizmeti olarak dağıtabilirsiniz:
- Azure Machine Learning Studio.
- R veya Python gibi araçlar. 

Bir REST API kullanarak bundan sonra kullanıma hazır hale getirilmiş modeli erişebilirsiniz.

Bir REST API'sini kullanmanın ve web hizmetine erişmek için çeşitli yöntemler vardır. Örneğin, web hizmetini dağıttığınızda sizin için oluşturulan örnek kodu kullanarak C#, R veya Python bir uygulama yazabilirsiniz. (Örnek kod kullanılabilir [Machine Learning Web Hizmetleri portalı](https://services.azureml.net/quickstart) veya web hizmeti panosunda Machine Learning Studio'da.) Veya sizin için aynı anda oluşturulan örnek Microsoft Excel çalışma kitabını kullanabilirsiniz.

Ancak, web hizmetine erişmek için hızlı ve kolay şekilde şablonlarıyla web app içinde kullanılabilir [Azure Marketi](https://azure.microsoft.com/marketplace/web-applications/all/).

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

## <a name="azure-machine-learning-web-app-templates"></a>Azure Machine Learning web uygulama şablonları
Web uygulaması şablonlarında kullanılabilir Azure Marketi web hizmetinizin giriş verilerini ve beklenen sonuçları bildiği bir özel web uygulaması oluşturabilirsiniz. Yapmanız gereken tek şey web uygulaması, web hizmeti ve verilerinizi erişmenizi ve şablon geri kalan yapar.

İki şablonlar kullanılabilir:

* [Azure ML istek-yanıt hizmeti Web uygulaması şablonu](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlaspnettemplateforrrs/)
* [Azure ML toplu iş yürütme hizmeti Web uygulaması şablonu](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlbeswebapptemplate/)

API URI'sini kullanarak her şablon bir örnek ASP.NET uygulaması oluşturur ve web hizmetiniz için anahtar. Şablon sonra Azure uygulamanızı bir Web sitesi olarak dağıtır. 

İstek-yanıt hizmeti (RRS) şablonu tek bir sonuç almak için web hizmeti için tek bir satır veri göndermek için kullanabileceğiniz bir web uygulaması oluşturur. Toplu yürütme hizmeti (BES) şablonu pek çok sayıda birden çok sonuç almak için veri göndermek için kullanabileceğiniz bir web uygulaması oluşturur.

Hiçbir kodlama, bu şablonları kullanmak için gereklidir. Yalnızca API anahtarı ve URI sağlayın ve şablon uygulamayı sizin için oluşturur.

API anahtarını almak ve istek URI'si için bir web hizmeti için:

1. İçinde [Web Hizmetleri portalında](https://services.azureml.net/quickstart)seçin **Web Hizmetleri** üstünde. Veya bir Klasik web hizmeti için seçin **Klasik Web Hizmetleri**.
2. Erişmek istediğiniz web hizmeti seçin.
3. Klasik web hizmeti için erişmek istediğiniz uç nokta seçin.
4. Seçin **Tüket** üstünde.
5. Birincil veya ikincil anahtarı kopyalayın ve kaydedin.
6. Bir RRS şablonu oluşturuyorsanız, kopyalama **istek-yanıt** URI ve kaydedin. Bir BES şablonu oluşturuyorsanız, kopyalama **toplu istekleri** URI ve kaydedin.


## <a name="how-to-use-the-request-response-service-template"></a>İstek-yanıt hizmeti şablonunu kullanma
Aşağıdaki çizimde gösterildiği gibi RRS web uygulaması şablonu kullanmak için aşağıdaki adımları izleyin.

![RRS web şablonu kullanmak için işlem][image1]


<!--    ![API Key][image3] -->

<!-- This value will look like this:
   
        https://ussouthcentral.services.azureml.net/workspaces/<workspace-id>/services/<service-id>/execute?api-version=2.0&details=true
   
    ![Request URI][image4] -->

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Seçin **yeni**, aramak ve seçmek **Azure ML istek-yanıt hizmeti Web uygulaması**ve ardından **oluşturma**. 
3. İçinde **oluşturma** bölmesi:
   
   * Web uygulamanızı benzersiz bir ad verin. Web uygulamasının URL'sini ve ardından bu adı olacaktır **. azurewebsites.net**. Örnek **http://carprediction.azurewebsites.net**.
   * Azure aboneliği ve web hizmetinizi altında çalıştığı hizmetleri seçin.
   * **Oluştur**’u seçin.
     
   ![Web uygulaması oluşturma][image5]

4. Azure web uygulaması dağıtma sona erdiğinde seçin **URL** üzerinde web uygulaması Ayarları sayfasında Azure'da veya bir web tarayıcısında URL'sini girin. Örneğin **http://carprediction.azurewebsites.net**.
5. Web uygulaması ilk kez çalıştırdığında, sizin için ister **API gönderme URL'sini** ve **API anahtarı**. Daha önce kaydettiğiniz değerleri girin (URI ve API anahtarını sırasıyla istek). Seçin **gönderme**.
     
   ![POST URI ve API anahtarını girin][image6]

6. Web uygulaması görüntüler kendi **Web Uygulama Yapılandırması** geçerli web hizmeti ayarları sayfası. Burada web uygulamasını kullanan ayarlarına değişiklik yapabilirsiniz.
   
   > [!NOTE]
   > Buradaki ayarlar yalnızca bunları bu web uygulaması için değiştirilir. Web hizmetinizin varsayılan ayarları değiştirmez. Örneğin, metnin değiştirirseniz **açıklama** Burada, Machine Learning Studio'da web hizmeti panosunda görüntülenen açıklama değişmez.
   > 
   > 
   
    İşiniz bittiğinde seçin **değişiklikleri kaydetmek**ve ardından **giriş sayfasına Git**.

7. Giriş sayfasından web hizmetiniz göndermek için değerler girebilirsiniz. Seçin **gönderme** zaman işiniz bittiğinde ve bir sonuç döndürdü.

Geri dönmek istiyorsanız **yapılandırma** sayfasında, Git **setting.aspx** sayfa web uygulaması. Örneğin, Git **http://carprediction.azurewebsites.net/setting.aspx**. API anahtarı yeniden girmeniz istenir. Bu sayfaya erişim ve ayarları güncelleştirmek için gerekir.

Durdurmak, yeniden başlatın veya başka bir web uygulaması gibi Azure portalında web uygulaması silme. Çalıştığı sürece, ev web adresine göz atın ve yeni değerler girin.

## <a name="how-to-use-the-batch-execution-service-template"></a>Toplu yürütme hizmeti şablonunu kullanma
RRS şablon olarak aynı şekilde BES web uygulaması şablonu kullanabilirsiniz. Oluşturulan web uygulaması birden çok sayıda veri göndermek ve birden çok sonuç almak için kullanabileceğiniz farktır.

Bir toplu iş yürütme web hizmeti için giriş değerleri, Azure Storage veya bir yerel dosya gelebilir. Sonuçları bir Azure depolama kapsayıcısında depolanır. Bu nedenle, web uygulaması döndürür sonuçları tutmak için bir Azure depolama kapsayıcısı gerekir. Giriş verilerinizi hazır hale getirmek gerekir.

![BES web şablonu kullanmak için işlem][image2]

1. RRS şablon ettirilmesi BES web uygulaması oluşturmak için aynı yordamı izleyin. Ancak bu durumda, Git [Azure ML toplu iş yürütme hizmeti Web uygulaması şablonu](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlbeswebapptemplate/) Azure Marketi'nde BES şablonu açın. Seçin **Web uygulaması oluşturma**.

2. Depolanan sonuçları istediğiniz yeri belirtmek için web uygulamanızın giriş sayfasında hedef kapsayıcı bilgileri girin. Ayrıca, web uygulaması giriş değerleri nereden belirtin: yerel bir dosya veya bir Azure depolama kapsayıcısı.
   Seçin **gönderme**.
   
   ![Depolama birimi bilgileri][image7]

Web uygulaması iş durumunu içeren bir sayfa görüntülenir. İş tamamlandığında, Azure Blob Depolama sonuçlarında konumunu alır. Ayrıca yerel bir dosyaya sonuçları yükleme seçeneğiniz vardır.

## <a name="for-more-information"></a>Daha fazla bilgi edinmek için
Daha fazla bilgi edinmek için:

* Machine learning denemeyi Machine Learning Studio ile bkz [Azure Machine Learning Studio'da ilk denemenizi oluşturma](create-experiment.md).
* Machine learning denemenizi bir web hizmeti olarak dağıtmak için bkz: nasıl [bir Azure Machine Learning web hizmetini dağıtma](publish-a-machine-learning-web-service.md).
* Web hizmetinize erişmek için diğer yollarını görme [bir Azure Machine Learning web hizmeti kullanmak nasıl](consume-web-services.md).

[image1]: media/consume-web-service-with-web-app-template/rrs-web-template-flow.png
[image2]: media/consume-web-service-with-web-app-template/bes-web-template-flow.png
[image3]: media/consume-web-service-with-web-app-template/api-key.png
[image4]: media/consume-web-service-with-web-app-template/post-uri.png
[image5]: media/consume-web-service-with-web-app-template/create-web-app.png
[image6]: media/consume-web-service-with-web-app-template/web-service-info.png
[image7]: media/consume-web-service-with-web-app-template/storage.png
