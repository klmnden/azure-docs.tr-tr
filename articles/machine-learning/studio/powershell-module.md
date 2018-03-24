---
title: Machine Learning için PowerShell Modülü | Microsoft Docs
description: Azure Machine Learning için PowerShell modülü genel önizleme modunda kullanılabilir. Çalışma alanları, denemeler, web hizmetleri ve daha fazlasını oluşturmak ve yönetmek için PowerShell kullanın.
keywords: deneme,doğrusal regresyon,makine öğrenimi algoritmaları,makine öğrenimi öğreticisi,tahmine dayalı modelleme teknikleri,veri bilimi deneyi
services: machine-learning
documentationcenter: ''
author: hning86
ms.author: haining
manager: mwinkle
editor: cgronlun
ms.assetid: a9001cc2-3aa0-47e1-b175-1f76408ba1d1
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/15/2017
ms.openlocfilehash: c9553e372f4d1cb5c60935fae5a7af61806ea6d4
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="powershell-module-for-microsoft-azure-machine-learning"></a>Microsoft Azure Machine Learning için PowerShell modülü
Azure Machine Learning için PowerShell modülü çalışma alanları, denemeler, veri kümeleri, Klasik web hizmetleri ve daha fazlasını yönetmek için Windows PowerShell'i kullanmanıza olanak sağlayan güçlü bir araçtır.

Belgeleri görüntülemek ve konumundaki tam kaynak kodu ile birlikte modülünü indirin [ https://aka.ms/amlps ](https://aka.ms/amlps). 

> [!NOTE]
> Azure Machine Learning PowerShell modülü şu anda Önizleme modundadır. Modül geliştirilmiş ve bu Önizleme dönemi boyunca genişletilmiş devam eder. Takip [Cortana Intelligence ve makine öğrenme Blog](https://blogs.technet.microsoft.com/machinelearning/) Haberler ve bilgiler için.

## <a name="what-is-the-machine-learning-powershell-module"></a>Machine Learning PowerShell modülü nedir?
Machine Learning PowerShell modülüdür bir. Azure Machine Learning çalışma alanları, denemeler, veri kümeleri, Klasik web hizmetleri ve klasik web hizmeti uç noktalarını Windows Powershell'den tam olarak yönetmenize olanak sağlayan ağ tabanlı DLL modülü. 

Modül birlikte düzgün bir şekilde ayrılmış içeren tam kaynak kodu indirebilirsiniz [C# API katmanı](https://github.com/hning86/azuremlps/blob/master/code/AzureMLSDK.cs). Bu DLL kendi .NET projeden başvuru ve Azure Machine Learning .NET kod üzerinden yönetebilirsiniz. Buna ek olarak, DLL doğrudan sık kullanılan istemcinizden kullanabileceğiniz temel REST API'leri bağlıdır.

## <a name="what-can-i-do-with-the-powershell-module"></a>PowerShell modülü ile ne yapabilirim?
Bu PowerShell modülü ile gerçekleştirebileceğiniz bazı görevler burada açıklanmıştır. Bunlar ve diğer birçok işlev için [belgelerin tamamını](https://aka.ms/amlps) inceleyin.

* Yönetim sertifikası kullanarak yeni bir çalışma alanı sağlama ([New-AmlWorkspace](https://github.com/hning86/azuremlps#new-amlworkspace))
* Bir deneme grafiğini temsil eden bir JSON dosyasını içeri ve dışarı aktarma ([Export-AmlExperimentGraph](https://github.com/hning86/azuremlps#export-amlexperimentgraph) ve [Import-AmlExperimentGraph](https://github.com/hning86/azuremlps#import-amlexperimentgraph))
* Bir deneme çalıştırma ([Start-AmlExperiment](https://github.com/hning86/azuremlps#start-amlexperiment))
* Tahmine daalı bir denemeden bir web hizmeti oluşturma ([New-AmlWebService](https://github.com/hning86/azuremlps#new-amlwebservice))
* Yayımlanan web hizmetini temel bir uç noktası oluşturma ([Ekle AmlWebServiceEndpoint](https://github.com/hning86/azuremlps#add-amlwebserviceendpoint))
* Bir RRS ve/veya BES web hizmeti uç noktasını çağırma ([Invoke-AmlWebServiceRRSEndpoint](https://github.com/hning86/azuremlps#invoke-amlwebservicerrsendpoint) ve [Invoke-AmlWebServicBESEndpoint](https://github.com/hning86/azuremlps#invoke-amlwebservicebesendpoint))

İşte var olan bir denemeyi çalıştırmak için PowerShell kullanımına küçük bir örnek:

        #Find the first Experiment named “xyz”
        $exp = (Get-AmlExperiment | where Description -eq ‘xyz’)[0]
        #Run the Experiment
        Start-AmlExperiment -ExperimentId $exp.ExperimentId 

Sık istenen görevini otomatikleştirmek için PowerShell modülünü kullanarak bu makalede daha kapsamlı bir kullanım örneği için bkz: [birçok Machine Learning modellerini ve web hizmeti uç noktalarını PowerShell kullanarak bir deneme oluşturma](create-models-and-endpoints-with-powershell.md).

## <a name="how-do-i-get-started"></a>Nasıl kullanmaya başlayabilirim?
Machine Learning PowerShell’i kullanmaya başlamak için GitHub’dan [yayın paketini](https://github.com/hning86/azuremlps/releases) indirin ve [yükleme yönergelerini](https://github.com/hning86/azuremlps/blob/master/README.md) izleyin. Yönergeleri, indirilen/sıkıştırması açılmış DLL engelini kaldırmak ve PowerShell ortamınıza içeri aktarın açıklamaktadır. Cmdlet’lerin çoğu bir çalışma alanı kimliği, çalışma alanı yetkilendirme belirteci ve çalışma alanının bulunduğu Azure bölgesini belirtmenizi gerektirir. Değerlerini sağlamak için en basit yolu, varsayılan config.json dosyasıdır. Yönergeler Ayrıca bu dosyayı yapılandırılması açıklanmaktadır. 

Ve, git ağaç kopyalamak istiyorsanız kodu değiştirin ve yerel olarak Visual Studio'yu kullanarak derleyin.

## <a name="next-steps"></a>Sonraki adımlar
PowerShell modülü tam belgelerine bulabilirsiniz [ https://aka.ms/amlps ](https://aka.ms/amlps). 

Gerçek dünya senaryoda modülü kullanmak nasıl genişletilmiş bir örneği için ayrıntılı kullanım örneği denetleyin [birçok Machine Learning modellerini ve web hizmeti uç noktalarını PowerShell kullanarak bir deneme oluşturma](create-models-and-endpoints-with-powershell.md).
