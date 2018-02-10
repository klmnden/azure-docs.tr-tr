---
title: "Azure Machine Learning Web Hizmetleri: Dağıtım ve kullanım | Microsoft Docs"
description: "Dağıtma ve web hizmetlerini tüketen kaynaklar."
services: machine-learning
documentationcenter: 
author: garyericson
manager: raymondl
editor: 
ms.assetid: 47635376-d1f4-4ea4-a6af-bd1f99f69a69
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: raymondl
ms.openlocfilehash: 88a61467a79a424670d49e662315cab59ab52d13
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/09/2018
---
# <a name="azure-machine-learning-web-services-deployment-and-consumption"></a>Azure Machine Learning Web Hizmetleri: Dağıtım ve kullanım
Machine learning iş akışları ve modelleri web Hizmetleri olarak dağıtmak için Azure Machine Learning'ı kullanabilirsiniz. Bu web Hizmetleri, tahminlerin gerçek zamanlı veya toplu iş modunda yapmak için Internet üzerinden uygulamalardan machine learning modellerini çağrılacak sonra kullanılabilir. Web hizmetleri RESTful olduğundan, bunları çeşitli programlama dillerini ve .NET ve Java gibi platformları ve Excel gibi uygulamalardan çağırabilirsiniz.

Sonraki bölümlerde izlenecek yollar, kodu ve başlamanıza yardımcı olmak için belgeler için bağlantılar sağlar.

## <a name="deploy-a-web-service"></a>Bir web hizmetini dağıtma

### <a name="with-azure-machine-learning-studio"></a>Azure Machine Learning Studio ile
Machine Learning Studio ve Microsoft Azure Machine Learning Web Hizmetleri Portalı'nı dağıtma ve bir web hizmeti kod yazmadan yönetmenize yardımcı olur.

Aşağıdaki bağlantılar, yeni bir web hizmetini dağıtma hakkında genel bilgi sağlar:

* Azure Resource Manager tabanlı yeni bir web hizmetini dağıtma hakkında bir genel bakış için bkz: [yeni bir web hizmetini dağıtma](publish-a-machine-learning-web-service.md).
* Bir web hizmeti dağıtma hakkında bir kılavuz için bkz: [bir Azure Machine Learning web hizmetini dağıtma](publish-a-machine-learning-web-service.md).
* Oluşturma ve bir web hizmetini dağıtma hakkında tam bir anlatım için bkz: [gözden geçirme adım 1: Machine Learning çalışma alanı oluşturma](walkthrough-1-create-ml-workspace.md).
* Bir web hizmetini dağıtma belirli örnekler için bkz:

  * [İzlenecek yol 5. adım: Azure Machine Learning web hizmetini dağıtma](walkthrough-5-publish-web-service.md)
  * [Bir web hizmeti için birden çok bölgeye dağıtma](how-to-deploy-to-multiple-regions.md)

### <a name="with-web-services-resource-provider-apis-azure-resource-manager-apis"></a>Web Hizmetleri kaynak sağlayıcı API'leri (Azure Resource Manager API'leri)
Web hizmetleri için Azure Machine Learning kaynak sağlayıcısı REST API çağrısı kullanarak dağıtım ve Yönetim web hizmetleri sağlar. Daha fazla bilgi için bkz: [Machine Learning Web hizmeti (REST)](/rest/api/machinelearning/index) başvurusu.

<!-- [Machine Learning Web Service (REST)](https://msdn.microsoft.com/library/azure/mt767538.aspx) reference. -->


### <a name="with-powershell-cmdlets"></a>PowerShell cmdlet'leri ile
Azure Machine Learning kaynak sağlayıcısı web hizmetleri için dağıtım ve Yönetim web hizmetleri PowerShell cmdlet'lerini kullanarak etkinleştirir.

Cmdlet'lerini kullanmak için ilk kez Azure hesabınızdan PowerShell ortamında kullanarak oturum gerekir [Add-AzureRmAccount](https://msdn.microsoft.com/library/mt619267.aspx) cmdlet'i. Resource Manager, temel alan PowerShell komutlarını nasıl bilginiz varsa bkz [Azure PowerShell kullanarak Azure Resource Manager ile](../../azure-resource-manager/powershell-azure-resource-manager.md#log-in-to-your-azure-account).

Tahmine dayalı denemenizi dışarı aktarmak için kullanın [Bu örnek kod](https://github.com/ritwik20/AzureML-WebServices). Koddan .exe dosyasını oluşturduktan sonra yazabilirsiniz:

    C:\<folder>\GetWSD <experiment-url> <workspace-auth-token>

Uygulama çalıştıran bir web hizmeti JSON şablonu oluşturur. Bir web hizmeti dağıtmak için şablon kullanmak için aşağıdaki bilgileri eklemeniz gerekir:

* Depolama hesabı adı ve anahtar

    Depolama hesabının adını almak ve gelen anahtar [Azure portal](https://portal.azure.com/).
* Taahhüt plan kimliği

    Plan Kimliğinden alabilirsiniz [Azure Machine Learning Web Hizmetleri](https://services.azureml.net) oturum açma ve bir plan adı tıklatarak portal.

JSON şablonunu alt olarak eklemediğiniz *özellikleri* düğüm aynı düzeyde *MachineLearningWorkspace* düğümü.

Bir örneği aşağıda verilmiştir:

    "StorageAccount": {
            "name": "YourStorageAccountName",
            "key": "YourStorageAccountKey"
    },
    "CommitmentPlan": {
        "id": "subscriptions/YouSubscriptionID/resourceGroups/YourResourceGroupID/providers/Microsoft.MachineLearning/commitmentPlans/YourPlanName"
    }

Daha fazla ayrıntı için örnek kod ve aşağıdaki makalelere bakın:

* [Azure Machine Learning cmdlet'lerini](https://msdn.microsoft.com/library/azure/mt767952.aspx) MSDN'de başvurusu
* Örnek [izlenecek](https://github.com/raymondlaghaeian/azureml-webservices-arm-powershell/blob/master/sample-commands.txt) github'da

## <a name="consume-the-web-services"></a>Web hizmetlerini kullanma
### <a name="from-the-azure-machine-learning-web-services-ui-testing"></a>Azure Machine Learning Web Hizmetleri kullanıcı Arabirimi (test)
Azure Machine Learning Web Hizmetleri portalında web hizmetiniz test edebilirsiniz. Bu istek-yanıt hizmeti (RRS) sınama içerir ve toplu yürütme hizmeti (BES) arabirimleri.

* [Yeni bir web hizmeti dağıtma](publish-a-machine-learning-web-service.md)
* [Bir Azure Machine Learning web hizmetini dağıtma](publish-a-machine-learning-web-service.md)
* [İzlenecek yol 5. adım: Azure Machine Learning web hizmetini dağıtma](walkthrough-5-publish-web-service.md)

### <a name="from-excel"></a>Excel'den
Web hizmeti tüketen Excel şablonunu indirebilirsiniz:

* [Bir Azure Machine Learning web hizmetini Excel'den kullanma](consuming-from-excel.md)
* [Azure Machine Learning Web Hizmetleri için Excel Eklentisi](excel-add-in-for-web-services.md)

### <a name="from-a-rest-based-client"></a>REST tabanlı bir istemciden
Azure Machine Learning Web Hizmetleri RESTful API'lerini ' dir. .NET, Python, R, Java, vb. gibi çeşitli platformlarda bu API'lerden kullanabilir. **Tüket** sayfasında web hizmetiniz için [Microsoft Azure Machine Learning Web Hizmetleri portalı](https://services.azureml.net) başlamanıza yardımcı olabilecek örnek koduna sahip. Daha fazla bilgi için bkz. [Azure Machine Learning web hizmetini kullanma](consume-web-services.md).
