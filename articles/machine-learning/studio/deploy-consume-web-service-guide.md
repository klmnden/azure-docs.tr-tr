---
title: Dağıtım ve kullanım
titleSuffix: Azure Machine Learning Studio
description: Azure Machine Learning Studio, machine learning iş akışları ve Modellerinizi web Hizmetleri olarak dağıtmak için kullanabilirsiniz. Bu web Hizmetleri, ardından, makine öğrenme modellerini uygulamalardan gerçek zamanlı olarak veya toplu iş modunda tahminleri yapmak için internet üzerinden çağırmak için de kullanılabilir.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: xiaoharper
ms.author: amlstudiodocs
ms.custom: seodec18
ms.date: 04/19/2017
ms.openlocfilehash: ad58b914c22c112a83c18a7c0377b567424e4efd
ms.sourcegitcommit: 8313d5bf28fb32e8531cdd4a3054065fa7315bfd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2019
ms.locfileid: "59046028"
---
# <a name="azure-machine-learning-studio-web-services-deployment-and-consumption"></a>Azure Machine Learning Studio Web Hizmetleri: Dağıtım ve kullanım

Azure Machine Learning Studio, machine learning iş akışları ve Modellerinizi web Hizmetleri olarak dağıtmak için kullanabilirsiniz. Bu web Hizmetleri, ardından, makine öğrenme modellerini uygulamalardan gerçek zamanlı olarak veya toplu iş modunda tahminleri yapmak için Internet üzerinden çağırmak için de kullanılabilir. RESTful web hizmetleri olduğu için bunları çeşitli programlama dilleri ve platformları, .NET ve Java gibi ve Excel gibi uygulamaları çağırabilirsiniz.

Sonraki bölümlerde, kılavuzlar, kod ve başlamanıza yardımcı olan belgeler için bağlantılar sağlar.

## <a name="deploy-a-web-service"></a>Bir web hizmetini dağıtma

### <a name="with-azure-machine-learning-studio"></a>Azure Machine Learning Studio ile

Studio portalı ve Microsoft Azure Machine Learning Web Hizmetleri portalını dağıtma ve kod yazmaya gerek kalmadan bir web hizmetini yönetmenize yardımcı olur.

Aşağıdaki bağlantılar, yeni bir web hizmeti dağıtma hakkında genel bilgiler sağlar:

* Azure Resource Manager'a bağlı yeni bir web hizmeti dağıtma hakkında genel bir bakış için bkz. [yeni bir web hizmetini dağıtma](publish-a-machine-learning-web-service.md).
* Bir web hizmeti dağıtma hakkında kılavuz için bkz. [bir Azure Machine Learning web hizmetini dağıtma](publish-a-machine-learning-web-service.md).
* İle bir web hizmeti oluşturma ve dağıtma konusunda tam bir kılavuz için başlangıç [öğretici 1: Kredi riskini tahmin](tutorial-part1-credit-risk.md).
* Bir web hizmetini dağıtma belirli örnekler için bkz:

  * [3. Öğretici: Kredi riski model dağıtma](tutorial-part3-credit-risk-deploy.md)
  * [Bir web hizmetini birden fazla bölgeye dağıtma](/azure/machine-learning/studio/publish-a-machine-learning-web-service#multi-region)

### <a name="with-web-services-resource-provider-apis-azure-resource-manager-apis"></a>Web Hizmetleri kaynak sağlayıcı API'leri (Azure Resource Manager API'leri)

Web hizmetleri için Azure Machine Learning Studio kaynak sağlayıcısı REST API çağrıları kullanarak dağıtım ve Yönetim web hizmetleri sağlar. Daha fazla bilgi için [Machine Learning Web hizmeti (REST)](/rest/api/machinelearning/index) başvuru.

<!-- [Machine Learning Web Service (REST)](https://msdn.microsoft.com/library/azure/mt767538.aspx) reference. -->

### <a name="with-powershell-cmdlets"></a>PowerShell cmdlet'leri ile

Azure Machine Learning Studio web hizmetleri kaynak sağlayıcı, PowerShell cmdlet'lerini kullanarak, dağıtım ve Yönetim web hizmetleri sağlar.

Cmdlet'leri kullanmak için önce Azure hesabınıza PowerShell ortamında kullanarak oturum açmalısınız [Connect AzAccount](/powershell/module/az.profile/connect-azaccount) cmdlet'i. Resource Manager temel alan PowerShell komutlarını çağıran nasıl aralıklarıyla bkz [Azure PowerShell kullanarak Azure Resource Manager ile](../../azure-resource-manager/manage-resources-powershell.md).

Tahmine dayalı denemenizi dışarı aktarmak için kullanın [Bu örnek kod](https://github.com/ritwik20/AzureML-WebServices). Koddan .exe dosyasını oluşturduktan sonra şunu yazabilirsiniz:

    C:\<folder>\GetWSD <experiment-url> <workspace-auth-token>

Uygulamayı çalıştıran bir web hizmeti JSON şablonu oluşturur. Bir web hizmetini dağıtmak için şablonu kullanmak için aşağıdaki bilgileri eklemeniz gerekir:

* Depolama hesabı adı ve anahtarı

    Depolama hesabı adını alın ve gelen anahtar [Azure portalında](https://portal.azure.com/).
* Taahhüt plan kimliği

    Planı Kimliğinden alabilirsiniz [Azure Machine Learning Web Hizmetleri](https://services.azureml.net) portalı oturum açma ve bir plan adı tıklatarak.

JSON şablonu altı olarak eklemediğiniz *özellikleri* düğüm aynı düzeyde *MachineLearningWorkspace* düğümü.

Bir örneği aşağıda verilmiştir:

    "StorageAccount": {
            "name": "YourStorageAccountName",
            "key": "YourStorageAccountKey"
    },
    "CommitmentPlan": {
        "id": "subscriptions/YouSubscriptionID/resourceGroups/YourResourceGroupID/providers/Microsoft.MachineLearning/commitmentPlans/YourPlanName"
    }

Ek ayrıntılar için örnek kod ve aşağıdaki makalelere bakın:

* [Azure Machine Learning Studio cmdlet'leri](https://docs.microsoft.com/powershell/module/az.machinelearning) MSDN'de başvurusu
* Örnek [izlenecek](https://github.com/raymondlaghaeian/azureml-webservices-arm-powershell/blob/master/sample-commands.txt) github'da

## <a name="consume-the-web-services"></a>Web hizmetlerini kullanma

### <a name="from-the-azure-machine-learning-web-services-ui-testing"></a>Azure Machine Learning Web Hizmetleri kullanıcı Arabirimi (test)

Azure Machine Learning Web Hizmetleri portalında web hizmetini test edebilirsiniz. Toplu yürütme hizmeti (BES) arabirimleri ve bu istek-yanıt hizmeti (RRS) test içerir.

* [Yeni bir web hizmeti dağıtma](publish-a-machine-learning-web-service.md)
* [Azure Machine Learning web hizmeti dağıtma](publish-a-machine-learning-web-service.md)
* [3. Öğretici: Kredi riski model dağıtma](tutorial-part3-credit-risk-deploy.md)

### <a name="from-excel"></a>Excel'den

Web hizmeti bir Excel şablonu indirebilirsiniz:

* [Bir Azure Machine Learning web hizmetini Excel'den kullanma](consuming-from-excel.md)
* [Azure Machine Learning Web Hizmetleri için Excel Eklentisi](excel-add-in-for-web-services.md)

### <a name="from-a-rest-based-client"></a>REST tabanlı bir istemciden

Azure Machine Learning Web Hizmetleri, RESTful apı'lerdir. Bu API'ler, .NET, Python, R, Java, vb. gibi çeşitli platformlarından kullanabilir. **Tüket** sayfasında web hizmetiniz için [Microsoft Azure Machine Learning Web Hizmetleri portalında](https://services.azureml.net) başlamanıza yardımcı olabilecek örnek koduna sahiptir. Daha fazla bilgi için bkz. [Azure Machine Learning web hizmetini kullanma](consume-web-services.md).
