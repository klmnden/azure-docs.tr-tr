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
ms.date: 09/16/2016
ms.author: garye
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: aa64dc7f5bb3e928aac30987b0904435c603829c


---
# <a name="walkthrough-develop-a-predictive-analytics-solution-for-credit-risk-assessment-in-azure-machine-learning"></a>Kılavuz: Azure Machine Learning Studio'da kredi riski değerlendirmesi için tahmine dayalı bir analiz çözümü geliştirme
Bir kişinin kredi uygulamasında verdiği bilgilere dayanarak kredi riskini tahmin etmeniz gerektiğini varsayalım.  

Kredi riski değerlendirmesi kuşkusuz karmaşık bir sorundur ancak sorunun parametrelerini biraz basitleştirelim. Ardından, böyle bir tahmine dayalı analiz çözümü oluşturmak için Machine Learning Studio ve Machine Learning web hizmeti ile Microsoft Azure Machine Learning'i nasıl kullanabileceğinize bir örnek olarak bunu kullanabiliriz.  

Bu ayrıntılı kılavuzda Machine Learning Studio'da tahmine dayalı bir analiz modeli geliştirme ve ardından bunu bir Azure Machine Learning web hizmeti olarak dağıtma sürecini izleyeceğiz. Genel kullanıma açık kredi riski verileriyle başlayacağız, bu verilere göre tahmine dayalı bir model eğitip geliştireceğiz ve ardından başkaları tarafından kredi riski değerlendirmesi için kullanılabilecek bir web hizmeti olarak modeli dağıtacağız.

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

<!-- -->

> [!TIP]
> Machine Learning Studio'nun işlevlerine genel bir bakış sağlayan bir diyagram indirmek ve yazdırmak için bkz. [Azure Machine Learning Studio'nun işlevlerine genel bakış diyagramı](machine-learning-studio-overview-diagram.md).
> 
> 

Bir kredi riski değerlendirme çözümü oluşturmak için bu adımları izleyeceğiz:  

1. [Bir Machine Learning çalışma alanı oluşturma](machine-learning-walkthrough-1-create-ml-workspace.md)
2. [Mevcut verileri yükleme](machine-learning-walkthrough-2-upload-data.md)
3. [Yeni bir deneme oluşturma](machine-learning-walkthrough-3-create-new-experiment.md)
4. [Modelleri eğitme ve değerlendirme](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5. [Web hizmetini dağıtma](machine-learning-walkthrough-5-publish-web-service.md)
6. [Web hizmetine erişme](machine-learning-walkthrough-6-access-web-service.md)

Bu kılavuz, [Cortana Intelligence Galerisi](http://gallery.cortanaintelligence.com/)'ndeki [İkili Sınıflandırma: Kredi riski tahmini](http://go.microsoft.com/fwlink/?LinkID=525270) örnek denemesinin basitleştirilmiş bir biçimini temel alır.




<!--HONumber=Nov16_HO2-->


