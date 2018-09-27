---
title: Machine Learning ile kredi riski için tahmine dayalı bir çözüm| Microsoft Belgeleri
description: Azure Machine Learning Studio'da kredi riski değerlendirmesi için tahmine dayalı bir analiz çözümün nasıl oluşturulacağını gösteren ayrıntılı bir kılavuz.
keywords: kredi riski, tahmine dayalı analiz çözümü, risk değerlendirmesi
services: machine-learning
documentationcenter: ''
author: heatherbshapiro
ms.author: hshapiro
ms.assetid: 43300854-a14e-4cd2-9bb1-c55c779e0e93
ms.service: machine-learning
ms.component: studio
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/23/2017
ms.openlocfilehash: 9d2cfeb780687e1bf0e61173b3550e26f4019115
ms.sourcegitcommit: d1aef670b97061507dc1343450211a2042b01641
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/27/2018
ms.locfileid: "47394159"
---
# <a name="walkthrough-develop-a-predictive-analytics-solution-for-credit-risk-assessment-in-azure-machine-learning"></a>Kılavuz: Azure Machine Learning Studio'da kredi riski değerlendirmesi için tahmine dayalı bir analiz çözümü geliştirme

Bu kılavuzda, Machine Learning Studio'da tahmine dayalı analiz çözümü geliştirme işleminin ayrıntılarına göz atacağız. Machine Learning Studio'da basit model geliştirecek ve ardından bunu, modelin yeni verileri kullanarak tahmin yapabileceği bir Azure Machine Learning web hizmeti olarak dağıtacağız. 

Bu kılavuzda Machine Learning Studio'yu daha önce en az bir kere kullandığınız ve makine öğrenimi kavramlarıyla ilgili bir fikriniz olduğu kabul edilir. Bununla birlikte, bir uzman olduğunuz da varsayılmaz.

**Azure Machine Learning Studio**'yu daha önce hiç kullanmadıysanız [Azure Machine Learning Studio'da ilk veri bilimi denemenizi oluşturma](create-experiment.md) öğreticisiyle başlamak isteyebilirsiniz. Bu öğretici, Machine Learning Studio’yu ilk kez nasıl kullanacağınızı gösterir. Öğreticide modülleri sürükleyip denemenize bırakma, birbirine bağlama, denemeyi çalıştırma ve sonuçları görme konularında temel bilgiler verilir. Machine Learning Studio’yu kullanmaya başlamanıza yardımcı olabilecek diğer bir araç da hizmetin özelliklerine genel bakış sağlayan bir diyagramdır. Diyagramı şu sayfadan indirip yazdırabilirsiniz: [Azure Machine Learning Studio özelliklerine genel bakış diyagramı](studio-overview-diagram.md).
 
Genel olarak makine öğrenimi alanında yeniyseniz size yardımcı olabilecek bir video serisi önerebiliriz. [Yeni Başlayanlar için Veri Bilimleri](data-science-for-beginners-the-5-questions-data-science-answers.md) adlı bu seri, gündelik dil ve kavramlarla makine öğrenimine harika bir giriş yapmanızı sağlayabilir.


[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]
 

## <a name="the-problem"></a>Sorun

Bir kişinin kredi başvurusunda verdiği bilgilere dayanarak kredi riskini tahmin etmeniz gerektiğini varsayalım.  

Kredi riski değerlendirmesi karmaşık bir sorun olsa da bu kılavuz için biraz basitleştirebiliriz. Microsoft Azure Machine Learning’i kullanarak nasıl tahmine dayalı analiz çözümü oluşturabileceğinizi gösteren bir örnek olarak kullanacağız. Bunu yapmak için Azure Machine Learning Studio’yu ve bir Machine Learning web hizmetini kullanırız.  

## <a name="the-solution"></a>Çözüm

Bu ayrıntılı kılavuzda, herkesin erişebileceği kredi riski verileriyle çalışmaya başlayarak bu verileri temel alan tahmine dayalı bir model geliştireceğiz ve modeli eğiteceğiz. Daha sonra, modelin başkaları tarafından kredi riski değerlendirmesi amacıyla kullanılabilmesi için modeli bir web hizmeti olarak dağıtacağız.

Bu kredi riski değerlendirme çözümünü oluşturmak için aşağıdaki adımları izleyeceğiz:  

1. [Bir Machine Learning çalışma alanı oluşturma](walkthrough-1-create-ml-workspace.md)
2. [Mevcut verileri yükleme](walkthrough-2-upload-data.md)
3. [Deneme oluşturma](walkthrough-3-create-new-experiment.md)
4. [Modelleri eğitme ve değerlendirme](walkthrough-4-train-and-evaluate-models.md)
5. [Web hizmetini dağıtma](walkthrough-5-publish-web-service.md)
6. [Web hizmetine erişme](walkthrough-6-access-web-service.md)

> [!TIP] 
> Bu kılavuzda geliştirdiğimiz denemenin çalışan bir kopyasını [Azure AI Gallery](https://gallery.cortanaintelligence.com)’de bulabilirsiniz. **[Walkthrough - Credit risk prediction](https://gallery.cortanaintelligence.com/Experiment/Walkthrough-Credit-risk-prediction-1)** (Kılavuz - Kredi riski tahmini) sayfasına gidin ve **Open in Studio** (Studio’da Aç) seçeneğine tıklayarak denemenin bir kopyasını Machine Learning Studio çalışma alanınıza indirin.
> 
> Bu kılavuz, [Galeri](http://gallery.cortanaintelligence.com/)’den de edinebileceğiniz [Binary Classification: Credit risk prediction](http://go.microsoft.com/fwlink/?LinkID=525270) (İkili Sınıflandırma: Kredi riski tahmini) örnek denemesinin basitleştirilmiş bir biçimini temel alır.
