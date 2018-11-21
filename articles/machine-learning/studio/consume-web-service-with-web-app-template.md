---
title: Bir web uygulaması şablonunu kullanarak bir Machine Learning web hizmetini kullanma | Microsoft Docs
description: Web uygulaması şablonu, Azure Marketi'nde Azure Machine learning'de Tahmine dayalı web servisini kullanın.
keywords: Web hizmeti, kullanıma hazır hale getirme, REST API, makine öğrenimi
services: machine-learning
documentationcenter: ''
author: ericlicoding
ms.custom: (previous ms.author=yahajiza, author=YasinMSFT)
ms.author: amlstudiodocs
manager: hjerez
editor: cgronlun
ms.assetid: e0d71683-61b9-4675-8df5-09ddc2f0d92d
ms.service: machine-learning
ms.component: studio
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2017
ms.openlocfilehash: 39add830d44cac43e620a4c13d20e282f3d59e47
ms.sourcegitcommit: fa758779501c8a11d98f8cacb15a3cc76e9d38ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2018
ms.locfileid: "52264226"
---
# <a name="consume-an-azure-machine-learning-web-service-by-using-a-web-app-template"></a>Bir web uygulaması şablonunu kullanarak bir Azure Machine Learning web hizmetini kullanma

Tahmine dayalı bir model geliştirmektir ve kullanarak bir Azure web hizmeti olarak dağıtacağız:
- Azure Machine Learning Studio'da.
- R veya Python gibi araçlar. 

REST API kullanarak bundan sonra çalışır hale getirilen modeli erişebilirsiniz.

Bir REST API'sini kullanmanın ve web hizmetine erişmek için çeşitli yollar vardır. Örneğin, bir uygulama yazabileceğiniz C#, R veya Python'da web hizmetini dağıttığınızda sizin için oluşturulan örnek kodu kullanarak. (Örnek kodu [Machine Learning Web Hizmetleri portalında](https://services.azureml.net/quickstart) veya Machine Learning Studio web hizmeti panosundaki.) Veya aynı anda sizin için oluşturulan örnek Microsoft Excel çalışma kitabını kullanabilirsiniz.

Ancak, web hizmetine erişmek için hızlı ve en kolay yollarından biri kullanılabilir web uygulaması şablonları sayesinde [Azure Marketi](https://azure.microsoft.com/marketplace/web-applications/all/).

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

## <a name="azure-machine-learning-web-app-templates"></a>Azure Machine Learning web uygulaması şablonları
Web uygulaması şablonları kullanılabilir Azure Marketi'nde web hizmetinizin girdi verilerini ve beklenen sonuçları bildiği bir özel web uygulaması oluşturabilirsiniz. Tek yapmak için ihtiyacınız olan verileri ve web hizmeti web uygulaması erişimi vermek ve şablon geri kalanını yapar.

İki şablonları mevcuttur:

* [Azure ML istek-yanıt hizmeti Web uygulaması şablonu](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlaspnettemplateforrrs/)
* [Azure ML toplu iş yürütme hizmeti Web uygulaması şablonu](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlbeswebapptemplate/)

Her şablon API URI'sini kullanarak örnek bir ASP.NET uygulaması oluşturur ve web hizmetiniz için anahtar. Şablon, daha sonra uygulamanızı bir Web sitesi olarak Azure'a dağıtır. 

İstek-yanıt hizmeti (RRS) şablonu, tek bir sonuç almak için web hizmeti için tek bir satır veri göndermek için kullanabileceğiniz bir web uygulaması oluşturur. Toplu yürütme hizmeti (BES) şablon birçok sayıda birden çok sonuç almak için veri göndermek için kullanabileceğiniz bir web uygulaması oluşturur.

Kodlama, bu şablonları kullanmak için gereklidir. URI ve API anahtarı yalnızca sağlayın ve uygulamayı sizin için bir şablon oluşturur.

API anahtarı alın ve istek URI'si için bir web hizmeti için:

1. İçinde [Web Hizmetleri portalını](https://services.azureml.net/quickstart)seçin **Web Hizmetleri** en üstünde. Veya bir Klasik web hizmeti için seçin **Klasik Web Hizmetleri**.
2. Erişmek istediğiniz web hizmeti seçin.
3. Klasik web hizmeti, erişmek istediğiniz uç nokta seçin.
4. Seçin **Tüket** en üstünde.
5. Birincil veya ikincil anahtarı kopyalayın ve kaydedin.
6. Bir RRS şablonu oluşturuyorsanız, kopyalama **istek-yanıt** URI ve kaydedin. Bir BES şablonu oluşturuyorsanız, kopyalama **toplu istekleri** URI ve kaydedin.


## <a name="how-to-use-the-request-response-service-template"></a>İstek-yanıt hizmeti şablonunu kullanma
Aşağıdaki diyagramda gösterildiği gibi RRS web uygulaması şablonu kullanmak için aşağıdaki adımları izleyin.

![RRS web şablonu kullanmak için işlem][image1]


<!--    ![API Key][image3] -->

<!-- This value will look like this:
   
        https://ussouthcentral.services.azureml.net/workspaces/<workspace-id>/services/<service-id>/execute?api-version=2.0&details=true
   
    ![Request URI][image4] -->

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Seçin **yeni**, arayın ve seçin **Azure ML istek-yanıt hizmeti Web uygulaması**ve ardından **Oluştur**. 
3. İçinde **Oluştur** bölmesi:
   
   * Web uygulamanıza benzersiz bir ad verin. Ardından bu ad web uygulamasının URL'si olacaktır **. azurewebsites.net**. Bir örnek **http://carprediction.azurewebsites.net**.
   * Web hizmetinizin altında çalışan hizmetleri ve Azure aboneliği seçin.
   * **Oluştur**’u seçin.
     
   ![Web uygulaması oluşturma][image5]

4. Azure web uygulaması dağıtımı tamamlandığında, seçin **URL** üzerinde web uygulaması Ayarları sayfasında Azure'da veya bir web tarayıcısında URL'sini girin. Örneğin, **http://carprediction.azurewebsites.net**.
5. Web uygulamasını ilk kez çalıştırdığında, sizin için soran **API Post URL'si** ve **API anahtarı**. Daha önce kaydettiğiniz değerleri girin (sırasıyla istek URI ve API anahtarı). Seçin **gönderme**.
     
   ![POST URI ve API anahtarını girin][image6]

6. Web uygulaması görüntüler, **Web Uygulama Yapılandırması** geçerli web hizmeti ayarları sayfası. Burada web uygulamasını kullanan ayarlarına değişiklik yapabilirsiniz.
   
   > [!NOTE]
   > Buradaki ayarlar yalnızca bunları bu web uygulaması için değiştirilir. Bu, web hizmetiniz varsayılan ayarlarını değiştirmez. Örneğin, metin değiştirirseniz **açıklama** Burada, Machine Learning Studio web hizmeti panosunda gösterilen açıklama değişmez.
   > 
   > 
   
    İşiniz bittiğinde **değişiklikleri kaydetmek**ve ardından **giriş sayfasına Git**.

7. Giriş sayfasından, web hizmetine göndermek için değerleri girebilirsiniz. Seçin **Gönder** zaman işiniz ve sonuç döndürülür.

Geri dönmek istiyorsanız **yapılandırma** sayfasında, Git **setting.aspx** web uygulamasının sayfası. Örneğin, Git **http://carprediction.azurewebsites.net/setting.aspx**. API anahtarı yeniden girmeniz istenir. Bu sayfaya erişim ve ayarları güncelleştirmek için gerekir.

Durdurma, yeniden başlatma veya başka bir web uygulaması gibi Azure portalında web uygulamasını silmek. Çalıştığı sürece, ev web adresine göz atın ve yeni değerler girin.

## <a name="how-to-use-the-batch-execution-service-template"></a>Toplu yürütme hizmeti şablonunu kullanma
RRS şablon olarak aynı şekilde BES web uygulaması şablonunu kullanabilirsiniz. Oluşturduğunuz web uygulamasını birden çok sayıda veri göndermek ve birden çok sonuç almak için kullanabileceğiniz farktır.

Toplu yürütme web hizmeti giriş değerlerini Azure depolama veya yerel bir dosyaya gelebilir. Sonuçları bir Azure depolama kapsayıcısında depolanır. Bu nedenle, web uygulaması döndüren sonuçlarını tutmak için bir Azure depolama kapsayıcısı gerekir. Giriş verileriniz hazır hale getirmek gerekir.

![BES web şablonu kullanmak için işlem][image2]

1. BES web uygulamasını hem RRS şablonu oluşturmak için aynı yordamı izleyin. Ancak bu durumda, Git [Azure ML toplu iş yürütme hizmeti Web uygulaması şablonunu](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlbeswebapptemplate/) Azure Marketi'nde BES şablonu açmak için. Seçin **Web uygulaması oluşturma**.

2. Depolanan sonuçların istediğiniz belirtmek için web uygulamasının giriş sayfasında hedef kapsayıcı bilgileri girin. Ayrıca web uygulamasının giriş değerleri nereden belirtin: yerel bir dosyaya veya bir Azure depolama kapsayıcısında.
   Seçin **gönderme**.
   
   ![Depolama bilgileri][image7]

Web uygulaması, iş durumunu içeren bir sayfa görüntüler. İş tamamlandığında, Azure Blob Depolama alanında sonuçları konumunu alma. Ayrıca yerel bir dosyaya sonuçları yükleme seçeneğiniz vardır.

## <a name="for-more-information"></a>Daha fazla bilgi edinmek için
Hakkında daha fazla bilgi edinmek için:

* Bkz: Machine Learning Studio ile machine learning denemesi oluşturma [Azure Machine Learning Studio'da ilk denemenizi oluşturma](create-experiment.md).
* Machine learning denemenizi bir web hizmeti olarak dağıtma nasıl [bir Azure Machine Learning web hizmetini dağıtma](publish-a-machine-learning-web-service.md).
* Bkz: web hizmetine erişmek için diğer yolları [bir Azure Machine Learning web hizmetini kullanma](consume-web-services.md).

[image1]: media/consume-web-service-with-web-app-template/rrs-web-template-flow.png
[image2]: media/consume-web-service-with-web-app-template/bes-web-template-flow.png
[image3]: media/consume-web-service-with-web-app-template/api-key.png
[image4]: media/consume-web-service-with-web-app-template/post-uri.png
[image5]: media/consume-web-service-with-web-app-template/create-web-app.png
[image6]: media/consume-web-service-with-web-app-template/web-service-info.png
[image7]: media/consume-web-service-with-web-app-template/storage.png
