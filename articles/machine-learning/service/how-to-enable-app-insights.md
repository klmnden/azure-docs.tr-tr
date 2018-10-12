---
title: Üretimde Azure Machine Learning hizmeti için Application ınsights'ı etkinleştir
description: Bir Azure Kubernetes hizmetinde dağıtılan Azure Machine Learning hizmeti için Application Insights'ı ayarlamayı öğrenin
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: marthalc
author: marthalc
ms.date: 10/01/2018
ms.openlocfilehash: 45871ab515c7ffd9520b1d77d3fd1e77abcc29ef
ms.sourcegitcommit: 4eddd89f8f2406f9605d1a46796caf188c458f64
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2018
ms.locfileid: "49114576"
---
# <a name="monitor-your-azure-machine-learning-models-in-production-with-application-insights"></a>Azure Machine Learning Modellerinizi üretimde Application Insights ile izleme

Bu makalede, nasıl ayarlanacağını öğrenin **Application Insights** için **Azure Machine Learning** hizmeti. Etkinleştirildikten sonra Application Insights izleme olanağı sağlar:
* Oranları, yanıt süreleri ve hata oranları iste
* Bağımlılık oranları, yanıt süreleri ve hata oranları
* Özel durumlar

Application Insights hakkında daha fazla bilgi [burada](../../application-insights/app-insights-overview.md). 

## <a name="prerequisites"></a>Önkoşullar
* Azure aboneliği. Aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.
* Bir Azure Machine Learning çalışma alanı, yüklü Python için betikleri ve Azure Machine Learning SDK'sını içeren yerel bir dizin. Kullanarak şu önkoşul olarak gerekenleri edinin öğrenin [bir geliştirme ortamı yapılandırma](how-to-configure-environment.md) belge.
* Azure Kubernetes Service (AKS) dağıtılması için eğitilen makine öğrenme modeli. Yoksa, bkz. [görüntü sınıflandırma modeli eğitme](tutorial-train-models-with-aml.md) öğretici.
* Bir [AKS kümesi](how-to-deploy-to-aks.md).

## <a name="enable--disable-in-the-portal"></a>Portalda devre dışı bırak & etkinleştir

Etkinleştirebilir ve Azure portalında Application ınsights'ı devre dışı bırakın.

### <a name="enable"></a>Etkinleştirme

1. İçinde [Azure portalında](https://portal.azure.com), kendi çalışma alanını açın.

1. Dağıtımlarınızı gidin ve Application Insights'ı etkinleştirmek için istediğiniz hizmeti seçin.

   [![Dağıtımları](media/how-to-enable-app-insights/Deployments.PNG)](./media/how-to-enable-app-insights/Deployments.PNG#lightbox)

3. Tıklayın **Düzenle** gidin **Gelişmiş ayarlar**.

   [![Düzenle](media/how-to-enable-app-insights/Edit.PNG)](./media/how-to-enable-app-insights/Edit.PNG#lightbox)

4. İçinde **Gelişmiş ayarlar** seçin **etkinleştirme Application Insights tanılama**.

   [![Düzenle](media/how-to-enable-app-insights/AdvancedSettings.png)](./media/how-to-enable-app-insights/AdvancedSettings.png#lightbox)

1. Seçin **güncelleştirme** değişiklikleri uygulamak için ekranın alt kısmındaki. 

### <a name="disable"></a>Devre Dışı Bırak
Azure portalında Application ınsights'ı devre dışı bırakmak için aşağıdaki adımları uygulayın:

1. Adresinden Azure portalında oturum açın https://portal.azure.com.
1. Çalışma alanınıza gidin.
1. Seçin **dağıtımları**, ardından **hizmet Seç**, ardından **Düzenle**.

   [![Düzenle](media/how-to-enable-app-insights/Edit.PNG)](./media/how-to-enable-app-insights/Edit.PNG#lightbox)

1. İçinde **Gelişmiş ayarlar**, seçeneği seçmemelisiniz **etkinleştirme Appınsights tanılamasını**. 

   [![Seçeneğinin işaretini kaldırın](media/how-to-enable-app-insights/uncheck.png)](./media/how-to-enable-app-insights/uncheck.png#lightbox)

1. Seçin **güncelleştirme** değişiklikleri uygulamak için ekranın alt kısmındaki. 

## <a name="enable--disable-from-the-sdk"></a>SDK'sından devre dışı bırak & etkinleştir

### <a name="update-a-deployed-service"></a>Dağıtılan bir hizmette güncelleştir
1. Hizmet çalışma belirlemek (ws = çalışma alanınızın adı)

    ```python
    aks_service= Webservice(ws, "my-service-name")
    ```
2. Hizmet uygulamanızın güncelleştirme ve Application Insights'ı etkinleştirin. 

    ```python
    aks_service.update(enable_app_insights=True)
    ```

### <a name="log-custom-traces-in-your-service"></a>Özel günlük izlemelerini hizmetinizde
Günlük izlemeleri özel istiyorsanız izleyeceği [AKS için standart dağıtım işlemini](how-to-deploy-to-aks.md) ve şunları yapacaksınız:

1. Puanlama dosyası, yazdırma ifadeleri ekleyerek güncelleştirin.
    
    ```python
    print ("model initialized" + time.strftime("%H:%M:%S"))
    ```

2. Aks yapılandırmasını güncelleştirin.
    
    ```python
    aks_config = AksWebservice.deploy_configuration(enable_app_insights=True)
    ```

3. [Görüntü oluşturun ve dağıtın.](how-to-deploy-to-aks.md)  

### <a name="disable-tracking-in-python"></a>Python'da izleme devre dışı bırak

Application ınsights'ı devre dışı bırakmak için aşağıdaki kodu kullanın:

```python 
## replace <service_name> with the name of the web service
<service_name>.update(enable_app_insights=False)
```
    

## <a name="evaluate-data"></a>Veri değerlendir
Hizmetinizin, Azure Machine Learning hizmeti çalışma alanında, aynı kaynak grubu içinde Application Insights hesabınızdaki veriler.
Bunu görüntülemek için:
1. Kaynak grubunuzda Git [Azure portalında](https://portal.azure.com) ve Application Insights kaynağınıza'ı tıklatın. 
2. **Genel bakış** sekmesi ölçümleri hizmetiniz için temel kümesini gösterir.

   [![Genel bakış](media/how-to-enable-app-insights/overview.png)](./media/how-to-enable-app-insights/overview.png#lightbox)

3. İçine aramak için özel izleme'yi tıklatın. **Analytics**.
4. Şema bölümündeki tıklayarak **izlemeleri** ardından **çalıştırma** sorgunuzu. Verileri tablo biçiminde aşağı aşağıda görünür olmalıdır ve özel çağrılarınızı Puanlama dosyanızdaki eşlemelisiniz. 

   [![Özel izlemeler](media/how-to-enable-app-insights/logs.png)](./media/how-to-enable-app-insights/logs.png#lightbox)

Tıklayın [burada](../../application-insights/app-insights-overview.md) Application Insights'ı kullanma hakkında daha fazla bilgi için.
    

## <a name="example-notebook"></a>Örneğin not defteri

[00. alma Started/13.enable-app-insights-in-production-service.ipynb](https://github.com/Azure/MachineLearningNotebooks/tree/master/01.getting-started/13.enable-app-insights) Not Defteri, bu makaledeki kavramları göstermektedir.  Bu not alın:
 
[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Sonraki adımlar
Modellerinizi üretimde şirket verilerini de toplayabilirsiniz. Makaleyi okuyun [üretimde modelleri için veri toplama](how-to-enable-data-collection.md) 
