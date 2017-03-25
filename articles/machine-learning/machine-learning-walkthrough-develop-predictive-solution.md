---
title: "Machine Learning ile kredi riski için tahmine dayalı bir çözüm| Microsoft Belgeleri"
description: "Azure Machine Learning Studio&quot;da kredi riski değerlendirmesi için tahmine dayalı bir analiz çözümün nasıl oluşturulacağını gösteren ayrıntılı bir kılavuz."
keywords: "kredi riski, tahmine dayalı analiz çözümü, risk değerlendirmesi"
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 43300854-a14e-4cd2-9bb1-c55c779e0e93
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/23/2017
ms.author: garye
translationtype: Human Translation
ms.sourcegitcommit: b652c0f817e5e56d4ff50701345b03db634f616c
ms.openlocfilehash: 043c54094f53ae539c833eb8f167201781c677a6
ms.lasthandoff: 02/17/2017


---
# <a name="walkthrough-develop-a-predictive-analytics-solution-for-credit-risk-assessment-in-azure-machine-learning"></a>Kılavuz: Azure Machine Learning Studio'da kredi riski değerlendirmesi için tahmine dayalı bir analiz çözümü geliştirme

Bu kılavuzda, Machine Learning Studio'da tahmine dayalı analiz çözümü geliştirme işleminin ayrıntılarına göz atacağız. Machine Learning Studio'da basit model geliştirecek ve ardından bunu, modelin yeni verileri kullanarak tahmin yapabileceği bir Azure Machine Learning web hizmeti olarak dağıtacağız. 

Bu kılavuzda Machine Learning Studio'yu daha önce en az bir kere kullandığınız ve makine öğrenimi kavramlarıyla ilgili bir fikriniz olduğu kabul edilir. Bununla birlikte, bir uzman olduğunuz da varsayılmaz.

**Azure Machine Learning Studio**'yu daha önce hiç kullanmadıysanız [Azure Machine Learning Studio'da ilk veri bilimi denemenizi oluşturma](machine-learning-create-experiment.md) öğreticisiyle başlamak isteyebilirsiniz. Bu öğretici, Machine Learning Studio’yu ilk kez nasıl kullanacağınızı gösterir. Öğreticide modülleri sürükleyip denemenize bırakma, birbirine bağlama, denemeyi çalıştırma ve sonuçları görme konularında temel bilgiler verilir. Machine Learning Studio’yu kullanmaya başlamanıza yardımcı olabilecek diğer bir araç da hizmetin özelliklerine genel bakış sağlayan bir diyagramdır. Diyagramı şu sayfadan indirip yazdırabilirsiniz: [Azure Machine Learning Studio özelliklerine genel bakış diyagramı](machine-learning-studio-overview-diagram.md).
 
Genel olarak makine öğrenimi alanında yeniyseniz size yardımcı olabilecek bir video serisi önerebiliriz. [Yeni Başlayanlar için Veri Bilimleri](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md) adlı bu seri, gündelik dil ve kavramlarla makine öğrenimine harika bir giriş yapmanızı sağlayabilir.


[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]
 

## <a name="the-problem"></a>Sorun

Bir kişinin kredi başvurusunda verdiği bilgilere dayanarak kredi riskini tahmin etmeniz gerektiğini varsayalım.  

Kredi riski değerlendirmesi karmaşık bir sorun olsa da bu kılavuz için biraz basitleştirebiliriz. Microsoft Azure Machine Learning’i kullanarak nasıl tahmine dayalı analiz çözümü oluşturabileceğinizi gösteren bir örnek olarak kullanacağız. Bunu yapmak için Azure Machine Learning Studio’yu ve bir Machine Learning web hizmetini kullanırız.  

## <a name="the-solution"></a>Çözüm

Bu ayrıntılı kılavuzda, herkesin erişebileceği kredi riski verileriyle çalışmaya başlayarak bu verileri temel alan tahmine dayalı bir model geliştireceğiz ve modeli eğiteceğiz. Daha sonra, modelin başkaları tarafından kredi riski değerlendirmesi amacıyla kullanılabilmesi için modeli bir web hizmeti olarak dağıtacağız.

Bu kredi riski değerlendirme çözümünü oluşturmak için aşağıdaki adımları izleyeceğiz:  

1. [Bir Machine Learning çalışma alanı oluşturma](machine-learning-walkthrough-1-create-ml-workspace.md)
2. [Mevcut verileri yükleme](machine-learning-walkthrough-2-upload-data.md)
3. [Deneme oluşturma](machine-learning-walkthrough-3-create-new-experiment.md)
4. [Modelleri eğitme ve değerlendirme](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5. [Web hizmetini dağıtma](machine-learning-walkthrough-5-publish-web-service.md)
6. [Web hizmetine erişme](machine-learning-walkthrough-6-access-web-service.md)

> [!TIP] 
> Bu kılavuzda geliştirdiğimiz denemenin çalışan bir kopyasını [Cortana Intelligence Galerisi](https://gallery.cortanaintelligence.com)’nde bulabilirsiniz. **[Walkthrough - Credit risk prediction](https://gallery.cortanaintelligence.com/Experiment/Walkthrough-Credit-risk-prediction-1)** (Kılavuz - Kredi riski tahmini) sayfasına gidin ve **Open in Studio** (Studio’da Aç) seçeneğine tıklayarak denemenin bir kopyasını Machine Learning Studio çalışma alanınıza indirin.
> 
> Bu kılavuz, [Galeri](http://gallery.cortanaintelligence.com/)’den de edinebileceğiniz [Binary Classification: Credit risk prediction](http://go.microsoft.com/fwlink/?LinkID=525270) (İkili Sınıflandırma: Kredi riski tahmini) örnek denemesinin basitleştirilmiş bir biçimini temel alır.

