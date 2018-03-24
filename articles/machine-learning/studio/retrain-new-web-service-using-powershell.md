---
title: PowerShell ile yeni Azure Machine Learning web hizmetine yeniden eğitme | Microsoft Docs
description: Program aracılığıyla bir modeli yeniden eğitme ve makine öğrenme yönetim PowerShell cmdlet'lerini kullanarak Azure Machine Learning ile yeni eğitilen modelini kullanmak için web hizmetini güncelleştirmek hakkında bilgi edinin.
services: machine-learning
documentationcenter: ''
author: YasinMSFT
ms.author: yahajiza
manager: hjerez
editor: cgronlun
ms.assetid: 3953a398-6174-4d2d-8bbd-e55cf1639415
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/28/2017
ms.openlocfilehash: 7fa93e138bc9feb66c200597119bb12dbaf00480
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="retrain-a-new-resource-manager-based-web-service-using-the-machine-learning-management-powershell-cmdlets"></a>Machine Learning yönetim PowerShell cmdlet'lerini kullanarak yeni Resource Manager temelli web hizmeti yeniden eğitme
Yeni bir web hizmeti yeniden eğitme, yeni eğitilen model başvurmak için Tahmine dayalı web hizmeti tanımının güncelleştirin.  

## <a name="prerequisites"></a>Önkoşullar
Gösterildiği gibi bir eğitim denemenizi ve Tahmine dayalı denemeye ayarlamalısınız [yeniden eğitme Machine Learning modellerini programlama aracılığıyla](retrain-models-programmatically.md). 

> [!IMPORTANT]
> Tahmine dayalı denemeye bir Azure Kaynak Yöneticisi'ni (yeni) dayalı machine learning web hizmeti dağıtılması gerekir. Yeni bir web hizmeti dağıtmak için yeterli izinleri olan Abonelikteki, web hizmetini dağıtma olmalıdır. Daha fazla bilgi için bkz: [Azure Machine Learning Web Hizmetleri Portalı'nı kullanarak bir Web hizmetini yönetmek](manage-new-webservice.md). 

Web hizmetleri dağıtma hakkında ek bilgi için bkz: [bir Azure Machine Learning web hizmetini dağıtma](publish-a-machine-learning-web-service.md).

Bu işlem, Azure Machine Learning cmdlet'leri yüklü olmasını gerektirir. Machine Learning cmdlet'lerini yükleme hakkında bilgi için bkz: [Azure Machine Learning cmdlet'leri](https://msdn.microsoft.com/library/azure/mt767952.aspx) başvuru konusuna bakın.

Aşağıdaki bilgileri yeniden eğitme çıkışı kopyalanan:

* BaseLocation
* RelativeLocation

Adımlar şunlardır:

1. Azure Resource Manager hesabınızda oturum açın.
2. Web hizmeti tanımının Al
3. Web hizmeti tanımının JSON olarak dışarı aktarma
4. JSON ilearner blob'una referansı güncelleştirin.
5. Bir Web hizmeti tanımının JSON içe
6. Yeni Web hizmeti tanımının web hizmetiyle güncelleştirme

## <a name="sign-in-to-your-azure-resource-manager-account"></a>Azure Resource Manager hesabınızda oturum açın
İlk PowerShell kullanarak ortam içinde Azure hesabınızdan için kaydolmalısınız [Add-AzureRmAccount](https://msdn.microsoft.com/library/mt619267.aspx) cmdlet'i.

## <a name="get-the-web-service-definition"></a>Web hizmeti tanımının Al
Ardından, Web hizmeti çağırarak alma [Get-AzureRmMlWebService](https://msdn.microsoft.com/library/mt619267.aspx) cmdlet'i. Web hizmeti tanımının bir iç web hizmetinin eğitilen modeli gösterimini ve doğrudan değiştirilebilir değil. Tahmine dayalı denemenizi ve değil eğitim denemenizi için Web hizmeti tanımının alırsınız emin olun.

    $wsd = Get-AzureRmMlWebService -Name 'RetrainSamplePre.2016.8.17.0.3.51.237' -ResourceGroupName 'Default-MachineLearning-SouthCentralUS'

Var olan bir web hizmetini kaynak grubu adını belirlemek için web Hizmetleri, aboneliğinizde görüntülemek için herhangi bir parametre olmadan Get-AzureRmMlWebService cmdlet'ini çalıştırın. Web hizmeti bulun ve ardından, web hizmeti kimliğini arayın Kaynak grubunun adını dördüncü kimliği hemen sonra öğedir *resourceGroups* öğesi. Aşağıdaki örnekte, kaynak grubu adı varsayılan MachineLearning SouthCentralUS ' dir.

    Properties : Microsoft.Azure.Management.MachineLearning.WebServices.Models.WebServicePropertiesForGraph
    Id : /subscriptions/<subscription ID>/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/RetrainSamplePre.2016.8.17.0.3.51.237
    Name : RetrainSamplePre.2016.8.17.0.3.51.237
    Location : South Central US
    Type : Microsoft.MachineLearning/webServices
    Tags : {}

Alternatif olarak, var olan bir web hizmetini kaynak grubu adını belirlemek için Microsoft Azure Machine Learning Web Hizmetleri portalında oturum açın. Web hizmeti seçin. Kaynak grubu adı beşinci web hizmetinin URL'sini hemen sonra öğesidir *resourceGroups* öğesi. Aşağıdaki örnekte, kaynak grubu adı varsayılan MachineLearning SouthCentralUS ' dir.

    https://services.azureml.net/subscriptions/<subcription ID>/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/RetrainSamplePre.2016.8.17.0.3.51.237


## <a name="export-the-web-service-definition-as-json"></a>Web hizmeti tanımının JSON olarak dışarı aktarma
Yeni eğitilen kullanılacak eğitilen modelini tanımına değiştirmek için Model, ilk kullanmalısınız [verme AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767935.aspx) JSON biçiminde bir dosyaya dışarı aktarmak için cmdlet.

    Export-AzureRmMlWebService -WebService $wsd -OutputFile "C:\temp\mlservice_export.json"

## <a name="update-the-reference-to-the-ilearner-blob-in-the-json"></a>JSON ilearner blob'una referansı güncelleştirin.
[Eğitilen model] varlıkları bulun, güncelleştirme *URI* değeri *locationInfo* ilearner blob URI'si düğümle. URI birleştirerek oluşturulan *BaseLocation* ve *RelativeLocation* çağrısı yeniden eğitme BES çıktısından. Bu yeni eğitilen model başvuru yolu güncelleştirir.

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

## <a name="import-the-json-into-a-web-service-definition"></a>Bir Web hizmeti tanımının JSON içe
Kullanmalısınız [alma AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767925.aspx) değiştirilmiş JSON dosyasını geri bir Web hizmeti, Web hizmeti tanımının güncelleştirmek için kullanabileceğiniz tanımının dönüştürmek için cmdlet.

    $wsd = Import-AzureRmMlWebService -InputFile "C:\temp\mlservice_export.json"


## <a name="update-the-web-service-with-new-web-service-definition"></a>Yeni Web hizmeti tanımının web hizmetiyle güncelleştirme
Son olarak, kullandığınız [güncelleştirme AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767922.aspx) Web hizmeti tanımının güncelleştirmek için cmdlet.

    Update-AzureRmMlWebService -Name 'RetrainSamplePre.2016.8.17.0.3.51.237' -ResourceGroupName 'Default-MachineLearning-SouthCentralUS'  -ServiceUpdates $wsd

## <a name="summary"></a>Özet
Machine Learning PowerShell Yönetimi cmdlet'lerini kullanarak, Tahmine dayalı bir Web hizmeti gibi senaryoları etkinleştirme eğitilen modelini güncelleştirebilirsiniz:

* Yeni verilerle yeniden eğitme düzenli modeli.
* Kullanıcıların kendi verilerini kullanarak modeli yeniden eğitme yapmalarına izin amacı ile dağıtım müşteriler için bir modelin.

