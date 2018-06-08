---
title: '6. adım: Machine Learning Web hizmetine erişim | Microsoft Docs'
description: '6. adımını geliştirme Tahmine dayalı bir çözüm izlenecek yol: etkin bir Azure Machine Learning Web hizmetine erişim.'
services: machine-learning
documentationcenter: ''
author: YasinMSFT
ms.author: yahajiza
manager: hjerez
editor: cgronlun
ms.assetid: 6a65c89a-40ab-4673-8dd8-8eee0a150e3b
ms.service: machine-learning
ms.component: studio
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/23/2017
ms.openlocfilehash: be63a04d0fe71039b19eb6bc0678a319f622a811
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34836713"
---
# <a name="walkthrough-step-6-access-the-azure-machine-learning-web-service"></a>Kılavuz Adımı 6: Azure Machine Learning web hizmetine erişim

Bu izlenecek yol son adımdır [Azure Machine learning'de Tahmine dayalı analiz çözümü geliştirme](walkthrough-develop-predictive-solution.md)

1. [Bir Machine Learning çalışma alanı oluşturma](walkthrough-1-create-ml-workspace.md)
2. [Mevcut verileri yükleme](walkthrough-2-upload-data.md)
3. [Yeni bir deneme oluşturma](walkthrough-3-create-new-experiment.md)
4. [Modelleri eğitme ve değerlendirme](walkthrough-4-train-and-evaluate-models.md)
5. [Web hizmetini dağıtma](walkthrough-5-publish-web-service.md)
6. **Web hizmetine erişim**

- - -
Bu kılavuzda önceki adımda bizim kredi riskini tahmin modelini kullanan bir web hizmeti dağıttığımız. Artık kullanıcıların veri göndermek ve sonuçları almak kullanabilirsiniz. 

Web hizmetini alabilen ve dönüş verileri iki yoldan biriyle REST API'lerini kullanarak bir Azure web hizmetidir:  

* **İstek/yanıt** - kullanıcı, bir HTTP protokolünü kullanarak hizmete kredi veri bir veya daha fazla satırı gönderir ve hizmeti bir veya daha fazla sonuç kümeleri yanıt verir.
* **Toplu yürütme** - kullanıcı depolar veya daha fazla satır kredi Azure blob ve blob konumu hizmetine gönderir. Hizmet giriş blob veri tüm satırları puanlar, başka bir blob'a sonuçları depolar ve bu kapsayıcı URL'sini döndürür.  

Klasik web hizmetine erişmek için hızlı ve kolay yolunu [Azure ML istek-yanıt hizmeti Web uygulaması](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlaspnettemplateforrrs/) veya [Azure ML toplu iş yürütme hizmeti Web uygulaması şablonu](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlbeswebapptemplate/).

Bu web uygulaması şablonlar web hizmetinizin giriş verilerini ve ne döndürülecek bilir bir özel web uygulaması oluşturabilirsiniz. Yapmanız gereken tek şey web hizmeti ve veri erişim sağlar ve geri kalan şablon yapar.

Web uygulama şablonları kullanma hakkında daha fazla bilgi için bkz: [bir Azure Machine Learning Web hizmeti bir web uygulaması şablonu kullanmak](consume-web-service-with-web-app-template.md).

Ayrıca sizin için R, C# ve Python programlama dili sağlanan Başlatıcı kodu kullanarak web hizmetine erişmek için özel bir uygulama geliştirebilirsiniz.

Tam ayrıntıları bulabilirsiniz [bir Azure Machine Learning Web hizmeti kullanmak nasıl](consume-web-services.md).

