---
title: "Bir Machine Learning web hizmeti bir web uygulaması şablonu ile kullanma | Microsoft Docs"
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
ms.author: garye;raymondl
ms.openlocfilehash: 1c182403409966923440f359cb2514af7b7df9f3
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="consume-an-azure-machine-learning-web-service-with-a-web-app-template"></a>Bir Azure Machine Learning web hizmetini web uygulaması şablonu ile kullanma

Bir kez artık Tahmine dayalı modelde geliştirilen ve Machine Learning Studio kullanarak bir Azure web hizmeti dağıtılmış veya R veya Python gibi araçları kullanarak, bir REST API kullanarak kullanıma hazır hale getirilmiş modeli erişebilir.

Bir REST API'sini kullanmanın ve web hizmetine erişmek için çeşitli yöntemler vardır. Örneğin, C#, R veya Python web hizmetini dağıttığınızda sizin için oluşturulan örnek kodu kullanarak bir uygulama yazabilirsiniz (kullanılabilir [Machine Learning Web Hizmetleri portalı](https://services.azureml.net/quickstart) veya web hizmeti panosunda Machine Learning Studio'da). Veya sizin için aynı anda oluşturulan örnek Microsoft Excel çalışma kitabını kullanabilirsiniz.

Ancak, web hizmetine erişmek için hızlı ve kolay şekilde şablonlarıyla Web App içinde kullanılabilir [Azure Web uygulaması Market](https://azure.microsoft.com/marketplace/web-applications/all/).

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

## <a name="the-azure-machine-learning-web-app-templates"></a>Azure makine öğrenimi Web Uygulama Şablonları
Web uygulaması şablonlarında kullanılabilir Azure Marketi web hizmetinizin giriş verilerini ve beklenen sonuçları bildiği bir özel web uygulaması oluşturabilirsiniz. Yapmanız gereken tek şey web uygulaması, web hizmeti ve verilerinizi erişmenizi ve şablon geri kalan yapar.

İki şablonlar kullanılabilir:

* [Azure ML istek-yanıt hizmeti Web uygulaması şablonu](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlaspnettemplateforrrs/)
* [Azure ML toplu iş yürütme hizmeti Web uygulaması şablonu](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlbeswebapptemplate/)

Her bir şablon web hizmetiniz için API URI ve anahtarınızı kullanarak bir örnek ASP.NET uygulaması oluşturur ve Azure web sitesine olarak dağıtır. İstek-yanıt hizmeti (RRS) şablonu tek bir sonuç almak için web hizmeti için tek bir veri satırı göndermenize olanak sağlayan bir web uygulaması oluşturur. Toplu yürütme hizmeti (BES) şablonu veri birden çok sonuç almak için çok sayıda satırı göndermenize olanak sağlayan bir web uygulaması oluşturur.

Hiçbir kodlama, bu şablonları kullanmak için gereklidir. Yalnızca API anahtarı ve URI sağlayın ve şablon uygulamayı sizin için oluşturur.

API anahtarı ve istek URI'si için bir web hizmeti almak için:

1. İçinde [Web Hizmetleri portalı](https://services.azureml.net/quickstart), yeni bir web hizmeti için tıklatın **Web Hizmetleri** üstünde. Veya bir Klasik web hizmeti tıklatın **Klasik Web Hizmetleri**.
2. Erişmek istediğiniz web hizmeti tıklatın.
3. Klasik web hizmeti, erişmek istediğiniz uç noktasına tıklayın.
4. Tıklatın **Tüket** üstünde.
5. Kopya **birincil** veya **ikincil anahtar** ve kaydedin.
6. İstek-yanıt hizmeti (RRS) şablonu oluşturuyorsanız, kopyalama **istek-yanıt** URI ve kaydedin. Bir toplu iş yürütme hizmeti (BES) şablonu oluşturuyorsanız, kopyalama **toplu istekleri** URI ve kaydedin.


## <a name="how-to-use-the-request-response-service-rrs-template"></a>İstek-yanıt hizmeti (RRS) şablonunu kullanma
Aşağıdaki çizimde gösterildiği gibi RRS web uygulaması şablonu kullanmak için aşağıdaki adımları izleyin.

![RRS web şablonu kullanmak için işlem][image1]


<!--    ![API Key][image3] -->

<!-- This value will look like this:
   
        https://ussouthcentral.services.azureml.net/workspaces/<workspace-id>/services/<service-id>/execute?api-version=2.0&details=true
   
    ![Request URI][image4] -->

1. Gidin [Azure portal](https://portal.azure.com), **oturum açma**, tıklatın **yeni**, aramak ve seçmek **Azure ML istek-yanıt hizmeti Web uygulaması**, ardından **oluşturma**. 
   
   * Web uygulamanızı benzersiz bir ad verin. Web uygulamasının URL'sini ve ardından bu adı olacaktır `.azurewebsites.net.` Örneğin,`http://carprediction.azurewebsites.net.`
   * Azure aboneliği ve web hizmetinizi altında çalıştığı hizmetleri seçin.
   * **Oluştur**'a tıklayın.
     
     ![Web uygulaması oluşturma][image5]

4. Azure web uygulaması dağıtma tamamlandığında tıklatın **URL** üzerinde web uygulaması Ayarları sayfasında Azure'da veya bir web tarayıcısında URL'sini girin. Örneğin, `http://carprediction.azurewebsites.net.`
5. Web uygulaması ilk kez çalıştırdığında, ister **API gönderme URL'sini** ve **API anahtarı**.
   Daha önce kaydettiğiniz değerleri girin (**istek URI'si** ve **API anahtarı**sırasıyla).
     
     Tıklatın **gönderme**.
     
     ![POST URI ve API anahtarını girin][image6]

6. Web uygulaması görüntüler kendi **Web Uygulama Yapılandırması** geçerli web hizmeti ayarları sayfası. Burada web uygulama tarafından kullanılan ayarları için değişiklik yapabilirsiniz.
   
   > [!NOTE]
   > Buradaki ayarlar yalnızca bunları bu web uygulaması için değiştirilir. Web hizmetinizin varsayılan ayarları değiştirmez. Örneğin, değiştirirseniz **açıklama** burada Machine Learning Studio'da web hizmeti panosunda görüntülenen açıklama değişmez.
   > 
   > 
   
    İşiniz bittiğinde tıklatın **değişiklikleri kaydetmek**ve ardından **giriş sayfasına Git**.

7. Giriş sayfasından web hizmetiniz göndermek için değerler girebilirsiniz. Tıklatın **gönderme** zaman işiniz bittiğinde ve bir sonuç döndürdü.

Geri dönmek istiyorsanız **yapılandırma** sayfasında, Git `setting.aspx` sayfa web uygulaması. Örneğin: `http://carprediction.azurewebsites.net/setting.aspx.` - bu sayfaya erişim ve ayarları güncelleştirmek için gereken API anahtarı yeniden girmeniz istenir.

Durdurmak, yeniden başlatın veya başka bir web uygulaması gibi Azure portalında web uygulaması silme. Çalıştığı sürece ev web adresine göz atın ve yeni değerler girin.

## <a name="how-to-use-the-batch-execution-service-bes-template"></a>Toplu yürütme hizmeti (BES) şablonunu kullanma
Oluşturulan web uygulaması, birden çok sayıda veri göndermek ve birden çok sonuç almanıza izin verir ancak bu RR şablon olarak aynı şekilde BES web uygulaması şablonu kullanabilirsiniz.

Bir toplu iş yürütme web hizmeti için giriş değerleri, Azure depolama veya bir yerel dosya gelebilir; sonuçları bir Azure depolama kapsayıcısında depolanır.
Bu nedenle, web uygulaması tarafından döndürülen sonuçları tutmak için bir Azure depolama kapsayıcısı gerekir ve giriş verilerinizi hazır hale getirmek gerekir.

![BES web şablonu kullanmak için işlem][image2]

1. Git dışında RRS şablonu ettirilmesi BES web uygulaması oluşturmak için aynı yordamı izleyin [Azure ML toplu iş yürütme hizmeti Web uygulaması şablonu](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlbeswebapptemplate/) Azure Marketi BES şablona açın ve tıklatın **Web uygulaması oluşturma**.

2. Depolanan sonuçları istediğiniz yeri belirtmek için web uygulama giriş sayfasında hedef kapsayıcı bilgileri girin. Ayrıca, web uygulaması giriş değerleri, yerel bir dosya veya bir Azure depolama kapsayıcısı nereden belirtin.
   Tıklatın **gönderme**.
   
    ![Depolama birimi bilgileri][image7]

Web uygulaması iş durumunu içeren bir sayfa görüntülenir.
İş tamamlandığında sonuçları Azure blob depolama alanındaki konumunu verilmesi. Ayrıca yerel bir dosyaya sonuçları yükleme seçeneğiniz vardır.

## <a name="for-more-information"></a>Daha fazla bilgi için
Daha fazla bilgi edinmek için...

* machine learning denemeyi Machine Learning Studio ile bkz [Azure Machine Learning Studio'da ilk denemenizi oluşturma](create-experiment.md)
* machine learning denemenizi bir web hizmeti olarak dağıtmak için bkz: nasıl [bir Azure Machine Learning web hizmetini dağıtma](publish-a-machine-learning-web-service.md)
* web hizmetinize erişmek için diğer yollarını görme [bir Azure Machine Learning Web hizmetini kullanma](consume-web-services.md)

[image1]: media/consume-web-service-with-web-app-template/rrs-web-template-flow.png
[image2]: media/consume-web-service-with-web-app-template/bes-web-template-flow.png
[image3]: media/consume-web-service-with-web-app-template/api-key.png
[image4]: media/consume-web-service-with-web-app-template/post-uri.png
[image5]: media/consume-web-service-with-web-app-template/create-web-app.png
[image6]: media/consume-web-service-with-web-app-template/web-service-info.png
[image7]: media/consume-web-service-with-web-app-template/storage.png
