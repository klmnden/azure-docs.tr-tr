---
title: Veri bilimi ile derin öğrenme veri bilimi sanal makinede Azure | Microsoft Docs
description: Derin öğrenme veri bilimi VM ile birkaç genel veri bilimi görevleri gerçekleştirme.
services: machine-learning
documentationcenter: ''
author: gopitk
manager: cgronlun
ms.assetid: 34ef0b10-9270-474f-8800-eecb183bbce4
ms.service: machine-learning
ms.component: data-science-vm
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/16/2018
ms.author: gokuma
ms.openlocfilehash: 2053ed8cc420183d493097eeb2cd2ad93c82c70c
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
ms.locfileid: "32167254"
---
# <a name="using-the-deep-learning-virtual-machine"></a>Sanal makine öğrenme derin kullanma

Derin öğrenme sanal makine'ne (DLVM) sağladıktan sonra bilgisayar görme ve dil anlama gibi etki alanlarındaki AI uygulamaları geliştirmek için derin sinir ağı model oluşturmaya başlayabilirsiniz. 

Derin öğrenme VM'de AI için sağlanan araçları mevcuttur. [Derin öğrenme ve AI çerçeveler sayfası](dsvm-deep-learning-ai-frameworks.md) bu araçlarının nasıl kullanılacağı hakkında ayrıntılar içerir. 

## <a name="deep-learning-tutorials-and-walkthroughs"></a>Öğreticiler ve izlenecek yollar derin öğrenme

Framework tabanlı örnekleri ek olarak, bir dizi kapsamlı izlenecek yollar üzerinde DLVM doğrulanmış de sağlanır. Bu izlenecek etki alanlarında resim ve metin/dil anlama gibi ayrıntılı learning uygulamaları geliştirme hızla başlamanıza yardımcı olur. Daha fazla uçtan uca öğretici farklı etki alanlarında ve teknoloji arasında eklenecek devam eder.   


- [Sinir ağları arasında farklı çerçeveleri çalıştıran](https://github.com/ilkarman/DeepLearningFrameworks): kod bir çerçevesinden geçirme yöntemleri gösteren kapsamlı bir gözden geçirme. Ayrıca, modeli karşılaştırmak ve çerçeveleri arasında zaman performans çalıştırmak nasıl gösterir. 

- [Ürün Görüntüleri içinde algılamak için uçtan uca çözümü oluşturmak için nasıl yapılır kılavuz](https://github.com/Azure/cortana-intelligence-product-detection-from-images): görüntü algılamadır bulun ve görüntüleri içindeki nesneleri sınıflandırmak bir teknik. Bu teknoloji büyük ödül pek çok gerçek hayatta iş alanlarında getirmek için olasılığı vardır. Örneğin, Perakendeciler, müşterinin hangi ürün rafı çekilen belirlemek için bu tekniği kullanabilirsiniz. Bu bilgiler sırayla depoları ürün envanterini yönetmenize yardımcı olur. 

- [Varlık ayıklama PubMed özetleri adlı](https://docs.microsoft.com/azure/machine-learning/preview/scenario-tdsp-biomedical-recognition) yapılandırılmamış metinden Uyuşturucu adları veya Hastalık adları gibi adlandırılmış varlıklar çıkarmayı Bu öğreticide gösterilmiştir. 18 milyon PubMed özetleri üzerinde bir metin gövde katıştırma özel bir word eğitir, varlık ayıklama uzun kısa vadeli bellek (LSTM) yinelenen sinir ağı modelini oluşturmak için bu modeli kullanır ve modeli katıştırma etki alanına özgü Word'ün daha iyi performans gösterir olduğunu gösteren bir Genel word için varlık ayıklama katıştırma.

- [Ses için öğrenme derin](https://blogs.technet.microsoft.com/machinelearning/2018/01/30/hearing-ai-getting-started-with-deep-learning-for-audio-on-azure/) Bu öğretici, üzerinde ses olay algılama derin learning modelini eğitmek gösterilmiştir [Kentsel ses dataset](https://serv.cusp.nyu.edu/projects/urbansounddataset/urbansound8k.html) ve ses verilerle çalışmak nasıl bir bakış sağlar.

- [Metin belgelerin sınıflandırılmasını](https://github.com/anargyri/lstm_han): Bu anlatımda nasıl oluşturulacağı ve iki farklı sinir ağı mimarileri eğitmek gösterilir: hiyerarşik dikkat ağ ve uzun kısa vadeli bellek (LSTM) ağ. Bu sinir ağları metin belgeleri sınıflandırmak için derin öğrenme için Keras API kullanın. Keras olan üç çerçeveleri öğrenme en popüler derin bir ön uç: Microsoft Bilişsel araç seti, TensorFlow ve Theano.

## <a name="next-steps"></a>Sonraki adımlar

[Örnekleri sayfa](dsvm-samples-and-walkthroughs.md) yardımcı olmak için her çerçeveleri VM üzerinde önceden yüklenmiş kod örnekleri işaretçiler hızlı başlama sağlar. 
