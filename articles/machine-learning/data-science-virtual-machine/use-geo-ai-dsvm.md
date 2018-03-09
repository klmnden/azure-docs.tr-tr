---
title: "Coğrafi yapay zeka veri bilimi kullanarak sanal makine - Azure | Microsoft Docs"
description: "Azure üzerinde bir coğrafi AI sanal makine kullanma"
keywords: "derin öğrenme, AI, veri bilimi araçları, veri bilimi sanal makine, Jeo-uzamsal analizi"
services: machine-learning
documentationcenter: 
author: gopitk
manager: cgronlun
ms.assetid: 
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/05/2018
ms.author: gokuma; bradsev;
ms.openlocfilehash: f87b56ad2b19b10bca1e797f0a5aef5c2a92a5dd
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/08/2018
---
# <a name="using-the-geo-artificial-intelligence-data-science-virtual-machine"></a>Coğrafi yapay zeka veri bilimi sanal makine kullanma

Coğrafi AI veri bilimi VM analiz için veri alma, wrangling veri gerçekleştirmek ve Jeo-uzamsal bilgi tüketen AI uygulamalar için model oluşturmak için kullanın. Coğrafi AI veri bilimi VM'i sağlayan ve ArcGIS Pro ArcGIS hesabınızla oturum sonra çevrimiçi ArcGis ArcGIS masaüstü ile etkileşimde başlatabilirsiniz. Python arabirimleri ve coğrafi veri bilimi VM önceden yapılandırılmış bir R dil köprüsü ArcGIS de erişebilirsiniz. Zengin AI uygulamaları geliştirmek için makine öğrenme ve derin çerçeveler ve veri bilimi VM kullanılabilir diğer veri Bilim yazılımı öğrenme ile birleştirin.  


## <a name="configuration-details"></a>Yapılandırma ayrıntıları

Python Kitaplığı [arcpy](http://pro.arcgis.com/en/pro-app/arcpy/main/arcgis-pro-arcpy-reference.htm), olduğu için kullanılan ArcGIS arabirimiyle bulunduğu konum veri bilimi VM genel kök conda ortamının yüklü ```c:\anaconda```. 

- Bir komut isteminde Python çalıştırıyorsanız çalıştırın ```activate``` conda kök Python ortamı etkinleştirmek için. 
- IDE veya Jupyter Not Defteri kullanıyorsanız, ortam veya doğru conda ortamında olduğundan emin olmak için çekirdek seçebilirsiniz. 

Adlı bir R kitaplık olarak ArcGIS R köprüsüne yüklü [arcgisbinding](https://github.com/R-ArcGIS/r-bridge) server tek başına örneğini bulunan ana Microsoft R içinde ```C:\Program Files\Microsoft\ML Server\R_SERVER```. Visual Studio, Rstudio'dan ve Jupyter zaten bu R ortamı kullanmak üzere önceden yapılandırılmış ve erişebilir ```arcgisbinding``` R kitaplığı. 


## <a name="geo-ai-data-science-vm-samples"></a>Coğrafi AI veri bilimi VM örnekleri

ML ve temel veri bilimi VM örneklerini framework tabanlı öğrenme derin ek olarak, Jeo-uzamsal örnekler kümesi de coğrafi AI veri bilimi VM bir parçası olarak sağlanır. Bu örnekler, geliştirme Jeo-uzamsal veri ve ArcGIS yazılım kullanılarak AI uygulamaların hızla başlamanıza yardımcı olabilir. 


1. [Python ile Jeo-uzamsal analytics ile belirtildiği](https://github.com/Azure/DataScienceVM/blob/master/Notebooks/ArcGIS/Python%20walkthrough%20ArcGIS%20Data%20analysis%20and%20ML.ipynb): tarafından sağlanan ArcGIS Python arabirimi kullanarak Jeo-uzamsal veri ile nasıl çalışılacağını gösteren bir giriş örnek [arcpy](http://pro.arcgis.com/en/pro-app/arcpy/main/arcgis-pro-arcpy-reference.htm) kitaplığı. Ayrıca nasıl geleneksel machine learning Jeo-uzamsal verilerle birleştirmek ve ArcGIS eşlemesinde sonucuna görselleştirmek gösterir. 

2. [R Jeo-uzamsal analytics ile belirtildiği](https://github.com/Azure/DataScienceVM/blob/master/Notebooks/ArcGIS/R%20walkthrough%20ArcGIS%20Data%20analysis%20and%20ML.ipynb): tarafından sağlanan ArcGIS R arabirimi kullanarak Jeo-uzamsal veri ile nasıl çalışılacağını gösteren bir giriş örnek [arcgisbinding](https://github.com/R-ArcGIS/r-bridge) kitaplığı. 

3. [Piksel düzeyinde kara sınıflandırma kullanmak](https://github.com/Azure/pixel_level_land_classification): havadan bir görüntü girişi olarak kabul eder ve bir kara kapak etiketi döndüren bir derin sinir ağı model oluşturmak nasıl gösterir bir öğretici. "Forested" veya "su" örnekler kara kapak etiketleri Model görüntüde böyle her piksel etiketi döndürür. Model Microsoft'un açık kaynaklı kullanılarak oluşturulan [Bilişsel Araç Seti (CNTK)](https://www.microsoft.com/en-us/cognitive-toolkit/) derin öğrenme çerçevesi. Bu örnek ayrıca üzerinde eğitim ölçeğini gösterilmektedir [Azure Batch AI](https://docs.microsoft.com/azure/batch-ai/) ve ArcGIS Pro yazılım modeli tahminleri kullanabilir. 


## <a name="next-steps"></a>Sonraki adımlar

Veri bilimi VM kullanmak ek örnekleri kullanılabilir şunlardır:

* [Örnekler](dsvm-samples-and-walkthroughs.md)

