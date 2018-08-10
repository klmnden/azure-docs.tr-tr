---
title: Bir PowerShell ile yeni Azure Machine Learning web hizmetini yeniden eğitme | Microsoft Docs
description: Program aracılığıyla bir modeli yeniden eğitme ve Azure Machine Learning Machine Learning Yönetimi PowerShell cmdlet'lerini kullanarak yeni eğitim modeli kullanmak için web hizmetini güncelleştirmek hakkında bilgi edinin.
services: machine-learning
documentationcenter: ''
author: YasinMSFT
ms.author: yahajiza
manager: hjerez
editor: cgronlun
ms.assetid: 3953a398-6174-4d2d-8bbd-e55cf1639415
ms.service: machine-learning
ms.component: studio
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/28/2017
ms.openlocfilehash: 069a3022cf9b6423b95e8f9f35686965d2654be7
ms.sourcegitcommit: 4de6a8671c445fae31f760385710f17d504228f8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/08/2018
ms.locfileid: "39631087"
---
# <a name="retrain-a-new-resource-manager-based-web-service-using-the-machine-learning-management-powershell-cmdlets"></a>Machine Learning Yönetimi PowerShell cmdlet'lerini kullanarak yeni Resource Manager tabanlı web hizmetini yeniden eğitme
Yeni bir web hizmetini yeniden eğitme, Tahmine dayalı web hizmeti tanımının yeni eğitim modeli başvuru güncelleştirin.

## <a name="prerequisites"></a>Önkoşullar
Gösterildiği ve eğitim denemesini öngörücü bir denemeye ayarlamalısınız [yeniden eğitme Machine Learning modellerini programlama aracılığıyla](retrain-models-programmatically.md).

> [!IMPORTANT]
> Bir Azure Resource Manager tabanlı (yeni) machine learning web hizmeti Tahmine dayalı denemeye dağıtılması gerekir.
> Yeni bir web hizmetini dağıtmak için yeterli olan aboneliği, web hizmetini dağıtma olması gerekir. Daha fazla bilgi için [Azure Machine Learning Web Hizmetleri portalını kullanarak bir Web hizmetini yönetme](manage-new-webservice.md).

Web hizmetlerini dağıtma hakkında ek bilgi için bkz. [bir Azure Machine Learning web hizmetini dağıtma](publish-a-machine-learning-web-service.md).

Bu işlem, Azure Machine Learning cmdlet'leri yüklemiş olması gerekir. Machine Learning cmdlet'leri yükleme hakkında bilgi için bkz. [Azure Machine Learning cmdlet'leri](https://docs.microsoft.com/powershell/module/azurerm.machinelearning/) MSDN başvuru.

Yeniden eğitme çıktısı, aşağıdaki bilgileri kopyalandı:

* BaseLocation
* RelativeLocation

Adımlar şunlardır:

1. Azure Resource Manager hesabınızda oturum açın.
2. Web servis tanımı Al
3. Web hizmet tanımı JSON olarak dışarı aktarma
4. JSON ilearner blob başvurusunu güncelleştirin.
5. Web hizmet tanımı JSON dosyasını içe
6. Yeni Web hizmeti tanımıyla web hizmetini güncelleştirmek

## <a name="sign-in-to-your-azure-resource-manager-account"></a>Azure Resource Manager hesabınızda oturum açın
İlk Azure hesabınızı PowerShell kullanarak ortam içinde için kaydolmalısınız [Connect-AzureRmAccount](/powershell/module/azurerm.profile/connect-azurermaccount) cmdlet'i.

## <a name="get-the-web-service-definition"></a>Web servis tanımı Al
Ardından, Web hizmeti çağrısı yaparak alın [Get-AzureRmMlWebService](https://docs.microsoft.com/powershell/module/azurerm.machinelearning/get-azurermmlwebservice) cmdlet'i. Web hizmet tanımı eğitilmiş modelinin web hizmeti bir iç temsiline ve doğrudan değiştirilebilir değil. Web hizmeti tanımı, Tahmine dayalı denemeye ve, eğitim denemesini almakta olduğunu doğrulayın.

    $wsd = Get-AzureRmMlWebService -Name 'RetrainSamplePre.2016.8.17.0.3.51.237' -ResourceGroupName 'Default-MachineLearning-SouthCentralUS'

Mevcut bir web hizmetini kaynak grubu adını belirlemek için aboneliğinizde web hizmetleri görüntülemek için herhangi bir parametre olmadan Get-AzureRmMlWebService cmdlet'ini çalıştırın. Web hizmeti bulun ve ardından, web hizmeti kimliğini arayın Kaynak grubu adını dördüncü kimliği hemen sonra öğedir *resourceGroups* öğesi. Aşağıdaki örnekte kaynak grubu adı varsayılan MachineLearning SouthCentralUS ' dir.

    Properties : Microsoft.Azure.Management.MachineLearning.WebServices.Models.WebServicePropertiesForGraph
    Id : /subscriptions/<subscription ID>/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/RetrainSamplePre.2016.8.17.0.3.51.237
    Name : RetrainSamplePre.2016.8.17.0.3.51.237
    Location : South Central US
    Type : Microsoft.MachineLearning/webServices
    Tags : {}

Alternatif olarak, mevcut bir web hizmetini kaynak grubu adını belirlemek için Microsoft Azure Machine Learning Web Hizmetleri portalında oturum açın. Web hizmeti seçin. Kaynak grubu adı beşinci web hizmetinin URL'sini hemen sonra öğesidir *resourceGroups* öğesi. Aşağıdaki örnekte kaynak grubu adı varsayılan MachineLearning SouthCentralUS ' dir.

    https://services.azureml.net/subscriptions/<subcription ID>/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/RetrainSamplePre.2016.8.17.0.3.51.237


## <a name="export-the-web-service-definition-as-json"></a>Web hizmet tanımı JSON olarak dışarı aktarma
Yeni eğitilen kullanmak için eğitilen modeli tanımına değiştirmek için Model, ilk kullanmanız gerekir [dışarı aktarma AzureRmMlWebService](https://docs.microsoft.com/powershell/module/azurerm.machinelearning/export-azurermmlwebservice) JSON biçiminde bir dosyaya vermek için cmdlet'i.

    Export-AzureRmMlWebService -WebService $wsd -OutputFile "C:\temp\mlservice_export.json"

## <a name="update-the-reference-to-the-ilearner-blob-in-the-json"></a>JSON ilearner blob başvurusunu güncelleştirin.
[Eğitilen model] varlıkları bulun, güncelleştirme *URI* değerini *locationInfo* düğümle ilearner blob URI'si. URI birleştirerek oluşturulur *BaseLocation* ve *RelativeLocation* çağrı yeniden eğitme BES çıktısından. Bu yeni eğitim modeli başvuru yolu güncelleştirir.

     "asset3": {
        "name": "Retrain Samp.le [trained model]",
        "type": "Resource",
        "locationInfo": {
          "uri": "https://mltestaccount.blob.core.windows.net/azuremlassetscontainer/baca7bca650f46218633552c0bcbba0e.ilearner"
        },
        "outputPorts": {
          "Results dataset": {
            "type": "Dataset"
          }
        }
      },

## <a name="import-the-json-into-a-web-service-definition"></a>Web hizmet tanımı JSON dosyasını içe
Kullanmalısınız [alma AzureRmMlWebService](https://docs.microsoft.com/powershell/module/azurerm.machinelearning/import-azurermmlwebservice) geri bir Web hizmeti Web hizmeti tanımını güncelleştirmek için kullanabileceğiniz tanımı içinde değiştirilen bir JSON dosyası dönüştürmek için cmdlet'i.

    $wsd = Import-AzureRmMlWebService -InputFile "C:\temp\mlservice_export.json"


## <a name="update-the-web-service-with-new-web-service-definition"></a>Yeni Web hizmeti tanımıyla web hizmetini güncelleştirmek
Son olarak, kullandığınız [güncelleştirme AzureRmMlWebService](https://docs.microsoft.com/powershell/module/azurerm.machinelearning/update-azurermmlwebservice) Web hizmet tanımını güncelleştirmeye yönelik cmdlet'i.

    Update-AzureRmMlWebService -Name 'RetrainSamplePre.2016.8.17.0.3.51.237' -ResourceGroupName 'Default-MachineLearning-SouthCentralUS'  -ServiceUpdates $wsd

## <a name="summary"></a>Özet
Machine Learning PowerShell Yönetimi cmdlet'lerini kullanarak Tahmine dayalı Web hizmeti gibi senaryoları etkinleştiren eğitilen modelini güncelleştirebilirsiniz:

* Yeni veriler ile yeniden eğitme düzenli modeli.
* Bunları kendi verilerini kullanarak modeli yeniden eğitme izin vermek amacıyla müşteriler için bir model dağıtımı.

