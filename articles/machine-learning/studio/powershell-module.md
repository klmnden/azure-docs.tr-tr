---
title: Machine Learning Studio - Azure PowerShell Modülü | Microsoft Docs
description: Azure Machine Learning için PowerShell modülü genel önizleme modunda kullanılabilir. Oluşturmak ve çalışma alanlarını, denemeleri, web hizmetleri ve yönetmek için PowerShell'i kullanın.
keywords: deneme,doğrusal regresyon,makine öğrenimi algoritmaları,makine öğrenimi öğreticisi,tahmine dayalı modelleme teknikleri,veri bilimi deneyi
services: machine-learning
documentationcenter: ''
author: ericlicoding
ms.custom: (previous ms.author=haining, author=hning86)
ms.author: amlstudiodocs
manager: mwinkle
editor: cgronlun
ms.assetid: a9001cc2-3aa0-47e1-b175-1f76408ba1d1
ms.service: machine-learning
ms.component: studio
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/15/2017
ms.openlocfilehash: 3191ff845f72c87b85fdd414716ed9a00b022d06
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/26/2018
ms.locfileid: "52312036"
---
# <a name="powershell-module-for-azure-machine-learning-studio"></a>Azure Machine Learning Studio için PowerShell Modülü
Azure Machine Learning için PowerShell modülü, çalışma alanlarını, denemeleri, veri kümeleri, Klasik web hizmetleri ve daha fazla yönetmek için Windows PowerShell'i kullanma olanak tanıyan güçlü bir araçtır.

Belgeleri görüntüleyin ve modülü tam kaynak koduyla birlikte, indirme [ https://aka.ms/amlps ](https://aka.ms/amlps). 

> [!NOTE]
> Azure Machine Learning PowerShell modülü şu anda Önizleme modundadır. Modülü geliştirilmeye ve genişletilmiş Bu önizleme dönemi boyunca devam eder. Gözünüzü [Cortana Intelligence ve Machine Learning Web günlüğü](https://blogs.technet.microsoft.com/machinelearning/) Haberler ve bilgiler.

## <a name="what-is-the-machine-learning-powershell-module"></a>Machine Learning PowerShell modülü nedir?
Machine Learning PowerShell modülü bir. Azure Machine Learning çalışma alanlarını, denemeleri, veri kümeleri, Klasik web hizmetleri ve klasik web hizmeti uç noktalarını Windows Powershell'den tam olarak yönetmenize olanak tanıyan NET tabanlı DLL modülü. 

Modülle birlikte düzgün bir şekilde ayrılmış içeren tam kaynak kodunu indirebilirsiniz [ C# API katmanı](https://github.com/hning86/azuremlps/blob/master/code/AzureMLSDK.cs). Kendi .NET projenizden bu DLL'ye başvuru ve Azure Machine Learning'i .NET kodu aracılığıyla yönetebileceğiniz. Ayrıca, DLL doğrudan, en sevdiğiniz istemciden kullanabileceğiniz temel REST API'leri bağlıdır.

## <a name="what-can-i-do-with-the-powershell-module"></a>PowerShell modülü ile ne yapabilirim?
Bu PowerShell modülü ile gerçekleştirebileceğiniz bazı görevler burada açıklanmıştır. Bunlar ve diğer birçok işlev için [belgelerin tamamını](https://aka.ms/amlps) inceleyin.

* Yönetim sertifikası kullanarak yeni bir çalışma alanı sağlama ([New-AmlWorkspace](https://github.com/hning86/azuremlps#new-amlworkspace))
* Bir deneme grafiğini temsil eden bir JSON dosyasını içeri ve dışarı aktarma ([Export-AmlExperimentGraph](https://github.com/hning86/azuremlps#export-amlexperimentgraph) ve [Import-AmlExperimentGraph](https://github.com/hning86/azuremlps#import-amlexperimentgraph))
* Bir deneme çalıştırma ([Start-AmlExperiment](https://github.com/hning86/azuremlps#start-amlexperiment))
* Tahmine daalı bir denemeden bir web hizmeti oluşturma ([New-AmlWebService](https://github.com/hning86/azuremlps#new-amlwebservice))
* Bir uç nokta üzerinde yayımlanan web hizmeti oluşturma ([Add-AmlWebServiceEndpoint](https://github.com/hning86/azuremlps#add-amlwebserviceendpoint))
* Bir RRS ve/veya BES web hizmeti uç noktasını çağırma ([Invoke-AmlWebServiceRRSEndpoint](https://github.com/hning86/azuremlps#invoke-amlwebservicerrsendpoint) ve [Invoke-AmlWebServicBESEndpoint](https://github.com/hning86/azuremlps#invoke-amlwebservicebesendpoint))

İşte var olan bir denemeyi çalıştırmak için PowerShell kullanımına küçük bir örnek:

        #Find the first Experiment named “xyz”
        $exp = (Get-AmlExperiment | where Description -eq ‘xyz’)[0]
        #Run the Experiment
        Start-AmlExperiment -ExperimentId $exp.ExperimentId 

Sık istenen bir görevi otomatik hale getirmek için PowerShell modülünü kullanma hakkında daha ayrıntılı bir kullanım örneği için şu makaleye bakın: [çok sayıda Machine Learning modeli ve web hizmeti uç noktaları PowerShell kullanarak bir denemeden oluşturma](create-models-and-endpoints-with-powershell.md).

## <a name="how-do-i-get-started"></a>Nasıl kullanmaya başlayabilirim?
Machine Learning PowerShell’i kullanmaya başlamak için GitHub’dan [yayın paketini](https://github.com/hning86/azuremlps/releases) indirin ve [yükleme yönergelerini](https://github.com/hning86/azuremlps/blob/master/README.md) izleyin. Yönergeler, indirilen/sıkıştırması açılan DLL'nin engelini kaldırmanız ve PowerShell ortamında içeri aktarma işlemleri açıklanmaktadır. Cmdlet’lerin çoğu bir çalışma alanı kimliği, çalışma alanı yetkilendirme belirteci ve çalışma alanının bulunduğu Azure bölgesini belirtmenizi gerektirir. Bir varsayılan config.json dosyası aracılığıyla değerlerini sağlamak için en basit yoludur. Yönergeler Ayrıca bu dosya yapılandırma işlemleri açıklanmaktadır. 

İsteyip istemediğinizi git ağacı kopyalayabilirsiniz kodu değiştirin ve yerel olarak Visual Studio'yu kullanarak derleyin.

## <a name="next-steps"></a>Sonraki adımlar
Tüm belgeler için PowerShell modülü bulabilirsiniz [ https://aka.ms/amlps ](https://aka.ms/amlps). 

Gerçek hayattaki bir senaryoda modülü kullanma genişletilmiş bir örneği için ayrıntılı kullanım örneğinin denetleyin [çok sayıda Machine Learning modeli ve web hizmeti uç noktaları PowerShell kullanarak bir denemeden oluşturma](create-models-and-endpoints-with-powershell.md).
