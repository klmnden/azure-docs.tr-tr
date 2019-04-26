---
title: Azure coğrafi yapay zeka veri bilimi sanal makinesi - kullanarak | Microsoft Docs
description: Coğrafi AI veri bilimi sanal makinesi verilerini çözümleme ve Jeo-uzamsal verileri temel alan modelleri oluşturmak için kullanmayı öğrenin.
keywords: derin öğrenme yapay ZEKA, veri bilimi araçları, veri bilimi sanal makinesi, Jeo-uzamsal analiz
services: machine-learning
documentationcenter: ''
author: gopitk
manager: cgronlun
ms.custom: seodec18
ms.assetid: ''
ms.service: machine-learning
ms.subservice: data-science-vm
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/05/2018
ms.author: gokuma
ms.openlocfilehash: 6e6737e928ece820ea9119d8dfe1d7cf22477646
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60406579"
---
# <a name="using-the-geo-artificial-intelligence-data-science-virtual-machine"></a>Coğrafi yapay zeka veri bilimi sanal makinesi kullanma

Coğrafi AI veri bilimi sanal makinesi, Jeo-uzamsal bilgi kullanan yapay ZEKA uygulamaları için model derleme analiz için veri alma ve veri denetimi gerçekleştirmek için kullanın. Coğrafi AI veri bilimi sanal makinesi hazırladıktan ve Arcgıs Pro Arcgıs hesabınızla oturum sonra çevrimiçi Arcgıs Arcgıs masaüstü ile etkileşim kurma başlatabilirsiniz. Python arabirimleri ve coğrafi veri bilimi sanal makinesi üzerinde önceden yapılandırılmış bir R dili köprüsü Arcgıs da erişebilirsiniz. Zengin yapay ZEKA uygulamaları oluşturmak için makine öğrenimi ve derin öğrenme çerçeveleri ve diğer veri bilimi sanal makinesi üzerinde kullanılabilir olan veri bilimi yazılım ile birleştirin.  


## <a name="configuration-details"></a>Yapılandırma ayrıntıları

Python Kitaplığı [arcpy](https://pro.arcgis.com/en/pro-app/arcpy/main/arcgis-pro-arcpy-reference.htm), alışkın olduğu Arcgıs arabirimiyle bulunduğu veri bilimi sanal makinesi genel kök conda ortamının yüklü ```c:\anaconda```. 

- Python'ı komut istemi'nde çalıştırıyorsanız, çalıştırma ```activate``` conda kök Python ortamına etkinleştirmek için. 
- Bir IDE ya da Jupyter Not Defteri kullanıyorsanız, ortam veya doğru conda ortamında olduğundan emin olmak için çekirdek seçebilirsiniz. 

Arcgıs R köprüsüne adlı bir R kitaplık olarak yüklü [arcgisbinding](https://github.com/R-ArcGIS/r-bridge) ana Microsoft r server tek başına örneğini bulunan ```C:\Program Files\Microsoft\ML Server\R_SERVER```. Visual Studio, RStudio ve Jupyter zaten bu R ortam kullanmak üzere önceden yapılandırılmış ve erişimi ```arcgisbinding``` R kitaplığı. 


## <a name="geo-ai-data-science-vm-samples"></a>Coğrafi AI veri bilimi VM örnekleri

ML ve derin öğrenme temel veri bilimi sanal makinesi örneklerini framework tabanlı ek olarak, Jeo-uzamsal örnekler kümesi de coğrafi AI veri bilimi sanal makinesi bir parçası olarak sağlanır. Bu örnekler, Jeo-uzamsal veri ile Arcgıs yazılım yapay ZEKA uygulamaları geliştirme hemen başlayabileceğiniz yardımcı olabilir. 


1. [Python ile Jeo-uzamsal analiz ile belirtilen](https://github.com/Azure/DataScienceVM/blob/master/Notebooks/ArcGIS/Python%20walkthrough%20ArcGIS%20Data%20analysis%20and%20ML.ipynb): Tarafından sağlanan Arcgıs Python arabirimi kullanarak Jeo-uzamsal veri ile nasıl çalışılacağını gösteren bir tanıtıcı örnek [arcpy](https://pro.arcgis.com/en/pro-app/arcpy/main/arcgis-pro-arcpy-reference.htm) kitaplığı. Ayrıca, geleneksel makine öğrenimi Jeo-uzamsal verilerle birleştirmek ve içinde Arcgıs harita üzerinde sonucun görselleştirilmesi nasıl gösterir. 

2. [R ile Jeo-uzamsal analiz ile belirtilen](https://github.com/Azure/DataScienceVM/blob/master/Notebooks/ArcGIS/R%20walkthrough%20ArcGIS%20Data%20analysis%20and%20ML.ipynb): R tarafından sağlanan Arcgıs arabirimi kullanarak Jeo-uzamsal veri ile nasıl çalışılacağını gösteren bir tanıtım örnek [arcgisbinding](https://github.com/R-ArcGIS/r-bridge) kitaplığı. 

3. [Piksel düzeyinde land kullanmak sınıflandırma](https://github.com/Azure/pixel_level_land_classification): Havadan bir girdi olarak kabul edip land kapak etiketi döndürür derin sinir ağı modelinin nasıl oluşturulacağını gösteren öğretici. "Forested" veya "su" örnekler land kapak etiketleri Model görüntüdeki gibi her piksel etiketini döndürür. Microsoft'un açık kaynaklı kullanarak oluşturulmuş model [Cognitive Toolkit (CNTK)](https://www.microsoft.com/en-us/cognitive-toolkit/) derin öğrenme. 


## <a name="next-steps"></a>Sonraki adımlar

Veri bilimi sanal makinesi kullanan ek örnekleri şuradan ulaşılabilir:

* [Örnekler](dsvm-samples-and-walkthroughs.md)

