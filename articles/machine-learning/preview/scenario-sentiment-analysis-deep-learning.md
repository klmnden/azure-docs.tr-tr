---
title: "Azure Machine Learning ile derin öğrenme kullanarak düşünceleri çözümleme | Microsoft Docs"
description: "Azure ML çalışma ekranı ile derin öğrenme kullanarak düşünceleri çözümlemesi gerçekleştirme."
services: machine-learning
documentationcenter: 
author: miprasad
manager: kristin.tolle
editor: miprasad
ms.assetid: 
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/20/2018
ms.author: miprasad
ms.openlocfilehash: 3b0a5bfc911f3edf91367cbf4fde907cbf98e114
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2018
---
# <a name="sentiment-analysis-using-deep-learning-with-azure-machine-learning"></a>Azure Machine Learning ile derin öğrenme kullanarak düşünceleri çözümlemesi

Düşünceleri analiz doğal dil işleme bölgedeki iyi bilinen bir görevdir. Metinleri kümesi düşünüldüğünde AIM metnin düşünceleri belirlemektir. Bu çözüm amacı derin öğrenme film yorumlarını gelen düşünceleri etmede kullanmaktır.

Çözüm https://github.com/Azure/MachineLearningSamples-SentimentAnalysis bulunur

## <a name="link-to-the-gallery-github-repository"></a>Galeri GitHub deponuza bağlayın

Genel GitHub depo bu bağlantıyı izleyin:

[https://github.com/Azure/MachineLearningSamples-SentimentAnalysis](https://github.com/Azure/MachineLearningSamples-SentimentAnalysis)

## <a name="use-case-overview"></a>Kullanım örneği'ne genel bakış

Veri patlaması ve mobil aygıtların artışı fırsatlar müşterilerin kendi duyguları ve alışkanlıklarınızı herhangi bir şey ve her şeyi hakkında herhangi bir zamanda ifade çok sayıda oluşturdunuz. Bu fikir veya "düşünceleri" incelemeler biçiminde sosyal kanalları üzerinden oluşturulan, Sohbetleri, paylaşımları, yöntemlerine, vb. tweet'leri. Düşünceleri ürün ve hizmetlerini geliştirmek, daha fazla bilgiye dayalı kararlar ve daha iyi kendi markalar yükseltmek için arayan işletmeler için çok yararlı olabilir.

Düşünceleri çözümlemesinden değeri almak için işletmelerin yapılandırılmamış sosyal veriler büyük depoları eyleme dönüştürülebilir Öngörüler için benim özelliği olmalıdır. Bu örnekte, biz AMLWorkbench kullanarak film yorumlarını düşünceleri analizini gerçekleştirmeye yönelik ayrıntılı learning modellerini geliştireceksiniz

## <a name="prerequisites"></a>Önkoşullar

* Bir [Azure hesabı](https://azure.microsoft.com/free/) (ücretsiz deneme kullanılabilir).

* Yüklü bir kopyasını [Azure Machine Learning çalışma ekranı](./overview-what-is-azure-ml.md) aşağıdaki [hızlı başlangıç Yükleme Kılavuzu](./quickstart-installation.md) programı yüklemek ve bir çalışma alanı oluşturmak için.

* Yüklü ve yerel olarak çalışan Docker altyapısına varsa operationalization için en iyisidir. Aksi durumda, küme seçeneğini kullanabilirsiniz. Ancak, Azure kapsayıcı Hizmeti'ni (ACS) çalıştıran pahalı olabilir.

* Bu çözüm, Azure Machine Learning çalışma ekranı Docker altyapısına yerel olarak yüklü Windows 10'çalıştığını varsayar. Bir macOS üzerinde talimat büyük ölçüde aynıdır.

## <a name="data-description"></a>Veri açıklaması

Bu örnek için kullanılan veri kümesi küçük bir yandan hazırlanmış veri kümesi ve bulunan [veri klasörü](https://github.com/Azure/MachineLearningSamples-SentimentAnalysis/tree/master/data).

İlk sütun film yorumlarını ve bunların düşünceleri (0 - negatif ve 1 - pozitif) ikinci sütun içerir. Veri kümesi yalnızca tanıtım amacıyla kullanılır ancak genellikle sağlam düşünceleri puanları almak için büyük bir veri kümesi gerekir. Örneğin, [IMDB film incelemeleri düşünceleri sınıflandırma sorunu](https://keras.io/datasets/#datasets ) tarafından düşünceleri (olumlu veya olumsuz) etiketli IMDB gelen 25.000 filmler incelemeleri, bir veri kümesinin Keras oluşur. Bu Laboratuvarın amacı, size derin öğrenme AMLWorkbench ile kullanarak düşünceleri çözümlemesi gerçekleştirme göstermektir.

## <a name="scenario-structure"></a>Senaryo yapısı

Klasör yapısı şu şekilde düzenlenir:

1. Kök klasöründe AMLWorkbench kullanarak düşünceleri analiz ilgili tüm kodu.
2. Veri: çözümde kullanılan veri kümesi içerir
3. belgeler: tüm uygulamalı labs içerir

Çözüm taşımak için uygulamalı Labs sırasını aşağıdaki gibidir:

| Sıra| Dosya Adı | İlişkili dosyaları |
|--|-----------|------|
| 1 | [`SentimentAnalysisDataPreparation.md`](https://github.com/Azure/MachineLearningSamples-SentimentAnalysis/blob/master/docs/SentimentAnalysisDataPreparation.md) | 'data/sampleReviews.txt' |
| 2 | [`SentimentAnalysisModelingKeras.md`](https://github.com/Azure/MachineLearningSamples-SentimentAnalysis/blob/master/docs/SentimentAnalysisModelingKeras.md) | 'SentimentExtraction.py' |
| 4 | [`SentimentAnalysisOperationalization.md`](https://github.com/Azure/MachineLearningSamples-SentimentAnalysis/blob/master/docs/SentimentAnalysisOperationalization.md) | 'Operaionalization' |

## <a name="conclusion"></a>Sonuç

Conclusion, bu çözüm Azure Machine Learning çalışma ekranı ile düşünceleri analizi yapmak için derin öğrenme kullanmaya sunar. HDF5 modelleri kullanarak da faaliyete.
