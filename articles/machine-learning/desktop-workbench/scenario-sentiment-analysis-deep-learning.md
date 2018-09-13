---
title: Derin öğrenme ile Azure Machine Learning kullanarak yaklaşım analizi | Microsoft Docs
description: Azure ML Workbench ile ayrıntılı öğrenme kullanarak yaklaşım analizi gerçekleştirme.
services: machine-learning
documentationcenter: ''
author: miprasad
manager: kristin.tolle
editor: miprasad
ms.assetid: ''
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/20/2018
ms.author: miprasad
ms.openlocfilehash: 97e3a621e291935db2e0c70eb2b596e77c7bffb7
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2018
ms.locfileid: "35651358"
---
# <a name="sentiment-analysis-using-deep-learning-with-azure-machine-learning"></a>Derin öğrenme ile Azure Machine Learning kullanarak yaklaşım analizi

Yaklaşım analizi, doğal dil işleme bölgedeki iyi bilinen bir görevdir. Metinleri bir dizi göz önünde bulundurulduğunda, bir yandan bu metinlerdeki belirlemektir. Amacı, bu çözüm, elde edilen film incelemeleri duyarlılığı tahmin etmede derin öğrenme kullanmaktır.

Çözüm bulunur https://github.com/Azure/MachineLearningSamples-SentimentAnalysis

## <a name="link-to-the-gallery-github-repository"></a>Galeri GitHub deponuza bağlayın

Ortak GitHub deposu için bu bağlantıyı izleyin:

[https://github.com/Azure/MachineLearningSamples-SentimentAnalysis](https://github.com/Azure/MachineLearningSamples-SentimentAnalysis)

## <a name="use-case-overview"></a>Kullanım örneği genel bakış

Fırsatları kendi duyguları ve alışkanlıklarından herhangi bir zamanda herhangi bir şey ve her şeyi ifade etmek müşteriler için çok sayıda veri patlaması ve mobil cihazları çoğalan oluşturdunuz. Bu fikir ya da "yaklaşım" incelemeleri biçiminde sosyal kanallar aracılığıyla oluşturulan, Sohbetleri, paylaşımları, beğeni, twitter, vb. Yaklaşım için ürün ve hizmetlerini geliştirmek, daha bilinçli kararlar alın ve daha iyi markalar kendi Yükselt isteyen işletmelerin çok yararlı olabilir.

Yaklaşım analizi değeri almak için işletmelerin geniş yapılandırılmamış sosyal veri depolarını eyleme dönüştürülebilir içgörülerle Madencilik özelliği olmalıdır. Bu örnekte, biz ayrıntılı öğrenme modelleri, film incelemeleri AMLWorkbench kullanarak yaklaşım analizi gerçekleştirmek için geliştirin

## <a name="prerequisites"></a>Önkoşullar

* Bir [Azure hesabı](https://azure.microsoft.com/free/) (ücretsiz denemeler kullanılabilir).

* Yüklü bir kopyasını [Azure Machine Learning Workbench](../service/overview-what-is-azure-ml.md) aşağıdaki [hızlı başlangıç Yükleme Kılavuzu](../service/quickstart-installation.md) programı yüklemek ve bir çalışma alanı oluşturun.

* Docker altyapısı yüklü ve yerel olarak çalışan varsa getirmek için en iyisidir. Aksi durumda, küme seçeneğini kullanabilirsiniz. Ancak, bir Azure Container Service (ACS) çalıştırmak pahalı olabilir.

* Bu çözüm, Azure Machine Learning Workbench ile Docker altyapısının yerel olarak yüklü Windows 10'da çalıştığını varsayar. Bir macOS üzerinde yönerge büyük ölçüde aynıdır.

## <a name="data-description"></a>Veri açıklaması

Bu örnek için kullanılan veri kümesini elle oluşturulmuş küçük bir veri kümesidir ve bulunan [veri klasörü](https://github.com/Azure/MachineLearningSamples-SentimentAnalysis/tree/master/data).

İlk sütun film incelemeleri ve ikinci sütun, duygu (0 - negatif ve 1 - pozitif) içerir. Veri kümesi yalnızca tanıtım amacıyla kullanılır ancak genellikle güçlü duyarlılığı puanlarını almak için büyük bir veri kümesi gerekir. Örneğin, [IMDB film incelemeleri yaklaşım sınıflandırma problemi](https://keras.io/datasets/#datasets ) Keras bir veri kümesi (pozitif veya negatif) tarafından yaklaşım etiketli IMDB, 25.000 filmler incelemelerinden oluşur. Bu Laboratuvar amacınıza, derin öğrenme ile AMLWorkbench kullanarak yaklaşım analizi gerçekleştirme göstermektir.

## <a name="scenario-structure"></a>Senaryo yapısı

Klasör yapısı aşağıdaki gibi düzenlenir:

1. Kök klasöründe AMLWorkbench kullanarak yaklaşım analizi için ilgili tüm kodu.
2. Veri: çözümde kullanılan veri kümesini içeren
3. docs: uygulamalı laboratuvarların tümünü içerir

Çözümü yürütmek için uygulamalı laboratuvarlar sırası aşağıdaki gibidir:

| Sipariş verme| Dosya Adı | İlgili dosyalar |
|--|-----------|------|
| 1 | [`SentimentAnalysisDataPreparation.md`](https://github.com/Azure/MachineLearningSamples-SentimentAnalysis/blob/master/docs/SentimentAnalysisDataPreparation.md) | 'data/sampleReviews.txt' |
| 2 | [`SentimentAnalysisModelingKeras.md`](https://github.com/Azure/MachineLearningSamples-SentimentAnalysis/blob/master/docs/SentimentAnalysisModelingKeras.md) | 'SentimentExtraction.py' |
| 3 | [`SentimentAnalysisOperationalization.md`](https://github.com/Azure/MachineLearningSamples-SentimentAnalysis/blob/master/docs/SentimentAnalysisOperationalization.md) | 'Operaionalization' |

## <a name="conclusion"></a>Sonuç

Conclusion, bu çözüm derin öğrenmeyi kullanarak Azure Machine Learning Workbench ile ilgili yaklaşım analizi gerçekleştirmek için sunar. HDF5 modelleri kullanarak da çalışır.
