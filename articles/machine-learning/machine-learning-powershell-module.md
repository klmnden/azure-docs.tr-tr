---
title: Machine Learning için PowerShell modülü | Microsoft Docs
description: Azure Machine Learning için PowerShell modülü genel önizleme modunda kullanılabilir. İş alanları, denemeler, web hizmetleri ve daha fazlasını oluşturmak ve yönetmek için PowerShell’i kullanın.
keywords: deneme,doğrusal regresyon,makine öğrenimi algoritmaları,makine öğrenimi öğreticisi,tahmine dayalı modelleme teknikleri,veri bilimi deneyi
services: machine-learning
documentationcenter: ''
author: hning86
manager: jhubbard
editor: cgronlun

ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 08/05/2016
ms.author: garye;haining

---
# Microsoft Azure Machine Learning için PowerShell modülü
Azure Machine Learning için PowerShell modülü, Windows PowerShell’i kullanarak iş alanlarını, denemeleri, veri kümelerin, web hizmetlerini ve daha fazlasını yönetmenize imkan tanır.

[https://aka.ms/amlps](https://aka.ms/amlps) adresinden belgelerin tamamını görüntüleyebilir ve modülü tam kaynak koduyla birlikte indirebilirsiniz. 

## Machine Learning PowerShell modülü nedir?
Machine Learning PowerShell modülü, Azure Machine Learning çalışma alanlarını, denemeleri, veri kümelerini, web hizmetlerini ve web hizmeti uç noktalarını Windows PowerShell’den tam olarak yönetmenize imkan tanıyan .NET tabanlı bir DLL modülüdür. Modülle birlikte açıkça ayrılmış bir [C# API katmanı](https://github.com/hning86/azuremlps/blob/master/code/AzureMLSDK.cs) içeren kaynak kodunun tamamını indrebilirsiniz. Bu, kendi .NET projenizden bu DLL’ye başvurabileceğiniz ve Azure Machine Learning’i .NET kodu aracılığıyla yönetebileceğiniz anlamına gelir. Ayrıca, DLL doğrudan en sevdiğiniz istemciden kullanabileceğiniz temel REST API’lere bağımlıdır.

## PowerShell modülü ile ne yapabilirim?
Bu PowerShell modülü ile gerçekleştirebileceğiniz bazı görevler burada açıklanmıştır. Bunlar ve diğer birçok işlev için [belgelerin tamamını](https://aka.ms/amlps) inceleyin.

* Yönetim sertifikası kullanarak yeni bir çalışma alanı sağlama ([New-AmlWorkspace](https://github.com/hning86/azuremlps#new-amlworkspace))
* Bir deneme grafiğini temsil eden bir JSON dosyasını içeri ve dışarı aktarma ([Export-AmlExperimentGraph](https://github.com/hning86/azuremlps#export-amlexperimentgraph) ve [Import-AmlExperimentGraph](https://github.com/hning86/azuremlps#import-amlexperimentgraph))
* Bir deneme çalıştırma ([Start-AmlExperiment](https://github.com/hning86/azuremlps#start-amlexperiment))
* Tahmine daalı bir denemeden bir web hizmeti oluşturma ([New-AmlWebService](https://github.com/hning86/azuremlps#new-amlwebservice))
* Yayımlanmış bir web hizmetinde yeni bir uç nokta oluşturma ([Add-AmlWebServiceEndpoint](https://github.com/hning86/azuremlps#add-amlwebserviceendpoint))
* Bir RRS ve/veya BES web hizmeti uç noktasını çağırma ([Invoke-AmlWebServiceRRSEndpoint](https://github.com/hning86/azuremlps#invoke-amlwebservicerrsendpoint) ve [Invoke-AmlWebServicBESEndpoint](https://github.com/hning86/azuremlps#invoke-amlwebservicebesendpoint))

İşte var olan bir denemeyi çalıştırmak için PowerShell kullanımına küçük bir örnek:

        #Find the first Experiment named “xyz”
        $exp = (Get-AmlExperiment | where Description -eq ‘xyz’)[0]
        #Run the Experiment
        Start-AmlExperiment -ExperimentId $exp.ExperimentId 

Daha ayrıntılı bir kullanım örneği görmek istiyorsanız, PowerShell modülünü kullanarak çok sık istenen bir görevi otomatikleştirmeyle ilgili şu makaleye bakın: [PowerShell kullanarak bir denemeden Machine Learning modelleri ve web hizmeti uç noktaları oluşturma](machine-learning-create-models-and-endpoints-with-powershell.md).

## Nasıl kullanmaya başlayabilirim?
Machine Learning PowerShell’i kullanmaya başlamak için GitHub’dan [yayın paketini](https://github.com/hning86/azuremlps/releases) indirin ve [yükleme yönergelerini](https://github.com/hning86/azuremlps/blob/master/README.md) izleyin. İndirilen/sıkıştırması açılan DLL’nin engelini kaldırmanız ve DLL’yi PowerShell ortamında içeri aktarmanız gerekir. Cmdlet’lerin çoğu bir çalışma alanı kimliği, çalışma alanı yetkilendirme belirteci ve çalışma alanının bulunduğu Azure bölgesini belirtmenizi gerektirir. Bunları sağlamanın en basit yolu, yükleme yönergelerinde ayrıntılı olarak ele alınan bir varsayılan config.json dosyası kullanmaktır. Tabii ki git ağacını kopyalayabilir ve Visual Studio’yu kullanarak kodu yerel olarak değiştirebilir/derleyebilirsiniz.

## Sonraki adımlar
Bu önizleme dönemi boyunca PowerShell modülü geliştirilmeye ve genişletilmeye devam edilecektir. Daha fazla haber ve bilgi için gözünüzü [Cortana Intelligence ve Machine Learning Blog](https://blogs.technet.microsoft.com/machinelearning/)’dan ayırmayın.

<!--HONumber=Sep16_HO3-->


