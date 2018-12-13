---
title: '6. adım: Web hizmeti - Azure Machine Learning Studio | Microsoft Docs'
description: 'Adım 6 / geliştirme Tahmine dayalı çözüm Kılavuzu: Etkin bir Azure Machine Learning Studio web hizmeti erişim.'
services: machine-learning
documentationcenter: ''
author: garyericson
ms.custom: seodec18
ms.author: garye
editor: cgronlun
ms.assetid: 6a65c89a-40ab-4673-8dd8-8eee0a150e3b
ms.service: machine-learning
ms.component: studio
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/23/2017
ms.openlocfilehash: e0628f6ed39652f3168917e26383b5d3c4a4fa4b
ms.sourcegitcommit: 1c1f258c6f32d6280677f899c4bb90b73eac3f2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2018
ms.locfileid: "53260931"
---
# <a name="walkthrough-step-6-access-the-azure-machine-learning-studio-web-service"></a>Kılavuz adımı 6: Azure Machine Learning Studio web hizmetine erişme

Bu kılavuz, son adımdır [bir Azure Machine learning'de Tahmine dayalı analiz çözümü geliştirin](walkthrough-develop-predictive-solution.md)

1. [Bir Machine Learning çalışma alanı oluşturma](walkthrough-1-create-ml-workspace.md)
2. [Mevcut verileri yükleme](walkthrough-2-upload-data.md)
3. [Yeni bir deneme oluşturma](walkthrough-3-create-new-experiment.md)
4. [Modelleri eğitme ve değerlendirme](walkthrough-4-train-and-evaluate-models.md)
5. [Web hizmetini dağıtma](walkthrough-5-publish-web-service.md)
6. **Web hizmetine erişme**

- - -
Bu kılavuzda önceki adımda size kredi riskini tahmin modelimizi kullanan bir web hizmetini dağıtıldı. Artık kullanıcıların veri göndermek ve sonuçları almak kullanabilirsiniz. 

Web hizmeti, alabilir ve dönüş verileri iki yöntemden biriyle REST API'lerini kullanarak bir Azure web hizmetidir:  

* **İstek/yanıt** - kullanıcı bir HTTP protokolünü kullanarak bir veya daha fazla kredi veri satırlarını hizmetine gönderir ve hizmet yanıt veren bir veya daha fazla sonuç kümeleri.
* **Toplu yürütme** - kullanıcı depolar veya daha fazla satır kredi verileri bir Azure blob ve sonra blob konum hizmetine gönderir. Hizmet giriş blob veri satırı puanlar, başka bir blobun sonuçlarını depolar ve bu kapsayıcı URL'sini döndürür.  

Klasik web hizmetine erişmek için hızlı ve en kolay yollarından biri sayesinde [Azure ML istek-yanıt hizmeti Web uygulaması](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlaspnettemplateforrrs/) veya [Azure ML toplu iş yürütme hizmeti Web uygulaması şablonunu](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlbeswebapptemplate/).

Bu web uygulaması şablonları, web hizmetinizin girdi verilerini ve hangi döndüreceği bildiği bir özel web uygulaması oluşturabilirsiniz. Tek yapmak için ihtiyacınız olan web hizmeti ve veri erişim sağlamak ve şablon geri kalanını yapar.

Web uygulaması şablonları kullanma hakkında daha fazla bilgi için bkz. [web uygulaması şablonu ile bir Azure Machine Learning Web hizmetini kullanma](consume-web-service-with-web-app-template.md).

Ayrıca, R, sağlanan başlangıç kodunu kullanarak web hizmetine erişmek için özel bir uygulama geliştirebilirsiniz C#, ve Python programlama dilleri arasındadır.

Tam Ayrıntılar bulabilirsiniz [bir Azure Machine Learning Web hizmetini kullanma](consume-web-services.md).

