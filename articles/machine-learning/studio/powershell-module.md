---
title: Machine Learning Studio için PowerShell modülleri
titleSuffix: Azure Machine Learning Studio
description: Oluşturmak ve Azure Machine Learning Studio çalışma alanları, denemeler, web hizmetleri ve daha fazlasını yönetmek için PowerShell'i kullanın.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: xiaoharper
ms.author: amlstudiodocs
ms.date: 04/25/2019
ms.openlocfilehash: bee42f8a9582908963c0eef95a2fd04742cd425e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65205672"
---
# <a name="powershell-modules-for-azure-machine-learning-studio"></a>Azure Machine Learning Studio için PowerShell modülleri

PowerShell modüllerini kullanarak Studio kaynakları ve çalışma alanları, veri kümeleri ve web Hizmetleri gibi varlıklar programlı olarak yönetebilirsiniz.

Üç Powershell modüllerini kullanarak Studio kaynaklarla etkileşim kurabilirsiniz:

* [Azure PowerShell Az](#az-rm) rağmen farklı cmdlet adları ile AzureRM, tüm işlevleri içerir, 2018 ' yayınlandı
* [AzureRM](#az-rm) , 2016, PowerShell Az tarafından değiştirilen çıkan
* [Azure Machine Learning PowerShell Klasik](#classic) 2016'da sunulan

Bu PowerShell modülleri bazı benzerlikler olmakla birlikte, her belirli senaryolar için tasarlanmıştır. Bu makalede PowerShell modülleri ve hangilerini seçmeniz karar vermenize yardımcı arasındaki farklar açıklanmaktadır.  

Denetleme [destek tablo](#support-table) hangi kaynakların her modülü tarafından desteklenen görmek için aşağıdaki. 

## <a name="az-rm"></a> Azure PowerShell Az hem de AzureRM

Az artık Azure ile etkileşim kurmak için hedeflenen PowerShell modülü ve tüm önceki AzureRM işlevselliğini içerir. AzureRM hata düzeltmeleri almaya devam eder, ancak hiçbir yeni cmdlet'leri veya özellikleri alır.  Her ikisi de az hem de AzureRM kullanılarak dağıtılan çözümleri yönetme **Azure Resource Manager** dağıtım modeli. Studio çalışma alanları ve "Yeni" Studio web hizmetleri bu kaynaklar içerir. 

PowerShell Klasik Az ya da AzureRM yanı sıra hem "Yeni" ve "Klasik" kaynak türleri kapsayacak şekilde yüklenebilir. Ancak, Az ve aynı anda yüklü AzureRM önerilmez. AzureRM az arasında karar vermek için Microsoft tüm gelecekteki dağıtımlar için Az önerir.  Az AzureRM ve geçiş yolu karşılaştırması hakkında daha fazla bilgi [Azure PowerShell Az giriş](https://docs.microsoft.com/powershell/azure/new-azureps-module-az).

Az ile çalışmaya başlamak için izleyin [yükleme yönergeleri için Azure Az](https://docs.microsoft.com/powershell/azure/install-az-ps).

## <a name="classic"></a> PowerShell Klasik

Studio [Klasik PowerShell Modülü](https://aka.ms/amlps) kullanılarak dağıtılan kaynaklara yönetmenize imkan tanır **Klasik dağıtım modeli**. Bu kaynaklar Studio kullanıcı varlıklar, "Klasik" web hizmetleri ve "Klasik" bir web hizmeti uç noktalarını içerir.

Ancak, Microsoft Dağıtım ve kaynakların yönetimini basitleştirmek için gelecekteki tüm kaynaklar için Resource Manager dağıtım modelini kullanmasını önerir. Dağıtım modelleri hakkında daha fazla bilgi edinmek istiyorsanız, bkz. [Azure Resource Manager ve klasik dağıtım](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-deployment-model) makalesi.

PowerShell Klasik ile çalışmaya başlamak için Yükle [yayın paketi](https://github.com/hning86/azuremlps/releases) GitHub ve izleme [yüklemesi için yönergeler](https://github.com/hning86/azuremlps/blob/master/README.md). Yönergeler, indirilen/sıkıştırması açılan DLL'nin engelini kaldırmanız ve PowerShell ortamında içeri aktarma işlemleri açıklanmaktadır.

PowerShell Klasik Az ya da AzureRM yanı sıra hem "Yeni" ve "Klasik" kaynak türleri kapsayacak şekilde yüklenebilir.

## <a name="support-table"></a> PowerShell desteği tablo


| | **Az** |  **PowerShell klasik** |
| --- | --- | --- |
| Çalışma alanları oluşturma/silme | [Resource Manager şablonları](https://docs.microsoft.com/azure/machine-learning/studio/deploy-with-resource-manager-template) |  |
| Çalışma alanı taahhüt planları yönetme | [Yeni AzMlCommitmentPlan](https://docs.microsoft.com/powershell/module/az.machinelearning/new-azmlcommitmentplan) | |
| Çalışma alanı kullanıcıları yönetme |  | [AmlWorkspaceUsers ekleyin](https://github.com/hning86/azuremlps#add-amlworkspaceusers)|
| Web hizmetlerini yönetme | [Yeni AzMlWebService](https://docs.microsoft.com/powershell/module/az.machinelearning/new-azmlwebservice) <br>("Yeni" web Hizmetleri)|| [Yeni-AmlWebService](https://github.com/hning86/azuremlps#manage-classic-web-service) <br>("Klasik" web Hizmetleri) |
| Web Hizmeti uç noktaları/anahtarları yönetme |  [Get-AzMlWebServiceKey](https://docs.microsoft.com/powershell/module/az.machinelearning/get-azmlwebservicekey)|  [Add-AmlWebServiceEndpoint](https://github.com/hning86/azuremlps#manage-classic-web-servcie-endpoint)|
| Kullanıcı veri kümesi ve eğitilen modelleri Yönet| | [Get-AmlDataset](https://github.com/hning86/azuremlps#manage-user-assets-dataset-trained-model-transform) |
| Kullanıcı deneyimleri yönetme |  | [Start-AmlExperiment](https://github.com/hning86/azuremlps#manage-experiment) |
| Özel modüller yönetme | | [Yeni AmlCustomModule](https://github.com/hning86/azuremlps#manage-custom-module) |


## <a name="next-steps"></a>Sonraki adımlar
Bu PowerShell modülünü tam belgelere bakın:
* [PowerShell klasik](https://aka.ms/amlps)
* [Azure PowerShell Az](https://docs.microsoft.com/powershell/module/az.machinelearning/#machine_learning)
