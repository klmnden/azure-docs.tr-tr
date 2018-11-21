---
title: Azure Machine Learning hizmeti için Application Insights'ı etkinleştir
description: Azure Machine Learning hizmeti aracılığıyla dağıtılan hizmetler için Application Insights'ı ayarlamayı öğrenin
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: marthalc
author: marthalc
ms.date: 10/01/2018
ms.openlocfilehash: 9e0f07e744aaf5f1c35666b40285937dce6dd4de
ms.sourcegitcommit: 8d88a025090e5087b9d0ab390b1207977ef4ff7c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2018
ms.locfileid: "52275063"
---
# <a name="monitor-your-azure-machine-learning-models-with-application-insights"></a>Azure Machine Learning Modellerinizi Application Insights ile izleme

Bu makalede, Azure Application ınsights'ı, Azure Machine Learning hizmeti için ayarlama konusunda bilgi edinin. Application Insights izleme olanağı sağlar:
* Oranları, yanıt süreleri ve hata oranları isteyin.
* Bağımlılık oranları, yanıt süreleri ve hata oranları.
* Özel durumlar.

[Application Insights hakkında daha fazla bilgi](../../application-insights/app-insights-overview.md). 

>[!NOTE]
> Bu makalede kod Azure Machine Learning SDK sürüm 0.1.74 ile test edilmiştir


## <a name="prerequisites"></a>Önkoşullar
* Azure aboneliği. Aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://aka.ms/AMLfree) oluşturun.
* Bir Azure Machine Learning çalışma alanı, yüklü Python için betikleri ve Azure Machine Learning SDK'sını içeren yerel bir dizin. Bu Önkoşullar alma hakkında bilgi için bkz: [bir geliştirme ortamı yapılandırma](how-to-configure-environment.md).
* Azure Kubernetes Service (AKS) veya Azure Container örneği (ACI) dağıtılması için eğitilen makine öğrenme modeli. Yoksa, bkz. [görüntü sınıflandırma modeli eğitme](tutorial-train-models-with-aml.md) öğretici.


## <a name="enable-and-disable-from-the-sdk"></a>Etkinleştirme ve SDK'sından devre dışı

### <a name="update-a-deployed-service"></a>Dağıtılan bir hizmette güncelleştir
1. Hizmet çalışma alanınızdaki belirleyin. Değeri `ws` çalışma alanınızın adıdır.

    ```python
    from azureml.core.webservice import Webservice
    aks_service= Webservice(ws, "my-service-name")
    ```
2. Hizmet uygulamanızın güncelleştirme ve Application Insights'ı etkinleştirin. 

    ```python
    aks_service.update(enable_app_insights=True)
    ```

### <a name="log-custom-traces-in-your-service"></a>Özel günlük izlemelerini hizmetinizde
Günlük izlemeleri özel istiyorsanız için standart dağıtım işlemini izleyin [AKS](how-to-deploy-to-aks.md) veya [ACI](how-to-deploy-to-aci.md) . Daha sonra:

1. Puanlama dosyası, yazdırma ifadeleri ekleyerek güncelleştirin.
    
    ```python
    print ("model initialized" + time.strftime("%H:%M:%S"))
    ```

2. Hizmet yapılandırmasını güncelleştirin.
    
    ```python
    config = Webservice.deploy_configuration(enable_app_insights=True)
    ```

3. Bir görüntü oluşturun ve üzerinde dağıtım [AKS](how-to-deploy-to-aks.md) veya [ACI](how-to-deploy-to-aci.md).  

### <a name="disable-tracking-in-python"></a>Python'da izleme devre dışı bırak

Application ınsights'ı devre dışı bırakmak için aşağıdaki kodu kullanın:

```python 
## replace <service_name> with the name of the web service
<service_name>.update(enable_app_insights=False)
```
    
## <a name="enable-and-disable-in-the-portal"></a>Etkinleştirme ve portalda devre dışı

Etkinleştirebilir ve Azure portalında Application ınsights'ı devre dışı bırakın.

1. İçinde [Azure portalında](https://portal.azure.com), kendi çalışma alanını açın.

1. Üzerinde **dağıtımları** sekmesinde, Application Insights'ı etkinleştirmek istediğiniz hizmeti seçin.

   [![Dağıtımları sekmesinde hizmetlerin listesi](media/how-to-enable-app-insights/Deployments.PNG)](./media/how-to-enable-app-insights/Deployments.PNG#lightbox)

3. **Düzenle**’yi seçin.

   [![Düzenle düğmesi](media/how-to-enable-app-insights/Edit.PNG)](./media/how-to-enable-app-insights/Edit.PNG#lightbox)

4. İçinde **Gelişmiş ayarlar**seçin **etkinleştirme Appınsights tanılamasını** onay kutusu.

   [![Seçili onay kutusu için tanılamayı etkinleştirme](media/how-to-enable-app-insights/AdvancedSettings.png)](./media/how-to-enable-app-insights/AdvancedSettings.png#lightbox)

1. Seçin **güncelleştirme** değişiklikleri uygulamak için ekranın alt kısmındaki. 

### <a name="disable"></a>Devre Dışı Bırak
1. İçinde [Azure portalında](https://portal.azure.com), kendi çalışma alanını açın.
1. Seçin **dağıtımları**, hizmet seçip **Düzenle**.

   [![Düzenle düğmesi](media/how-to-enable-app-insights/Edit.PNG)](./media/how-to-enable-app-insights/Edit.PNG#lightbox)

1. İçinde **Gelişmiş ayarlar**temizleyin **etkinleştirme Appınsights tanılamasını** onay kutusu. 

   [![Temizlenmiş onay kutusu için tanılamayı etkinleştirme](media/how-to-enable-app-insights/uncheck.png)](./media/how-to-enable-app-insights/uncheck.png#lightbox)

1. Seçin **güncelleştirme** değişiklikleri uygulamak için ekranın alt kısmındaki. 
 

## <a name="evaluate-data"></a>Veri değerlendir
Application ınsights'ı hesabınız, Azure Machine Learning hizmeti aynı kaynak grubunda hizmetinizin veriler depolanır.
Bunu görüntülemek için:
1. Machine Learning hizmeti çalışma alanınızda Git [Azure portalında](https://portal.azure.com) ve Application Insights bağlantıya tıklayın.

    [![AppInsightsLoc](media/how-to-enable-app-insights/AppInsightsLoc.png)](./media/how-to-enable-app-insights/AppInsightsLoc.png#lightbox)

1. Seçin **genel bakış** hizmetiniz için ölçümleri temel kümesini görmek için sekmesinde.

   [![Genel bakış](media/how-to-enable-app-insights/overview.png)](./media/how-to-enable-app-insights/overview.png#lightbox)

3. İçinde özel izlemeler bakmak için seçtikten **Analytics**.
4. Şema bölümünde **izlemeleri**. Ardından **çalıştırma** sorgunuzu çalıştırılacak. Verileri tablo biçiminde görünür olmalıdır ve özel çağrılarınızı Puanlama dosyanızdaki eşlemelisiniz. 

   [![Özel izlemeler](media/how-to-enable-app-insights/logs.png)](./media/how-to-enable-app-insights/logs.png#lightbox)

Application Insights'ı kullanma hakkında daha fazla bilgi için bkz. [Application Insights nedir?](../../application-insights/app-insights-overview.md).
    

## <a name="example-notebook"></a>Örneğin not defteri

[00. alma Started/13.enable-app-insights-in-production-service.ipynb](https://github.com/Azure/MachineLearningNotebooks/tree/master/01.getting-started/13.enable-app-insights) Not Defteri, bu makaledeki kavramları göstermektedir.  Bu not alın:
 
[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Sonraki adımlar
Modellerinizi üretimde şirket verilerini de toplayabilirsiniz. Makaleyi okuyun [üretimde modelleri için veri toplama](how-to-enable-data-collection.md). 


## <a name="other-references"></a>Diğer başvurular
* [Kapsayıcılar için Azure İzleyici](https://docs.microsoft.com/azure/monitoring/monitoring-container-insights-overview?toc=%2fazure%2fmonitoring%2ftoc.json)
