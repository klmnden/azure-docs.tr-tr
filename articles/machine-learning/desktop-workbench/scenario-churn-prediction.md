---
title: Azure Machine Learning kullanarak müşteri karmaşıklığı tahmin | Microsoft Docs
description: Azure ML çalışma ekranı kullanarak karmaşası analytics gerçekleştirme.
services: machine-learning
documentationcenter: ''
author: miprasad
manager: kristin.tolle
editor: miprasad
ms.assetid: ''
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: desktop-workbench
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/17/2017
ms.author: miprasad
ms.openlocfilehash: a4e3441e4b7512d60be8ce5433822a95732cd802
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34832399"
---
# <a name="customer-churn-prediction-using-azure-machine-learning"></a>Azure Machine Learning kullanarak müşteri karmaşası tahmin

Ortalama, var olan müşteriler tutma beş kez yenilerini işe alma maliyetini ucuz. Sonuç olarak, pazarlama yöneticiler genellikle kendilerini müşteri karmaşası olasılığını tahmin etmeyi deneyerek ve karmaşıklık oranı en aza indirmek için gerekli olan eylemleri bulma bulun.

Bu çözüm amacı, Azure Machine Learning çalışma ekranı kullanarak Tahmine dayalı karmaşası analytics göstermektir. Bu çözüm için perakende karmaşası Tahmine dayalı veri ardışık geliştirmek için kullanımı kolay bir şablonu sağlar. Şablon, farklı veri kümelerini ve karmaşıklığı farklı tanımları ile kullanılabilir. Uygulamalı örnek amacı şunlardır:

1. Azure Machine Learning çalışma ekranı'nın veri hazırlığı araçları temizleyin ve karmaşıklığı analiz için müşteri ilişkileri verileri alma anlayın.

2. Gürültülü heterojen verileri işlemek için özellik dönüşümünü gerçekleştirin.

3. Üçüncü taraf kitaplıklar tümleştirme (gibi `scikit-learn` ve `azureml`) Bayesian ve karmaşıklığı tahmin etmeye yönelik sınıflandırıcı ağaç tabanlı geliştirilir.

4. Dağıtın.

## <a name="link-of-the-gallery-github-repository"></a>Galeri GitHub deposunun bağlantı
Tüm kod barındırıldığı genel GitHub depo bağlantısını aşağıdadır:

[https://github.com/Azure/MachineLearningSamples-ChurnPrediction](https://github.com/Azure/MachineLearningSamples-ChurnPrediction)

## <a name="use-case-overview"></a>Kullanım örneği'ne genel bakış
Şirketler, müşteri karmaşası yönetmek için etkili bir strateji gerekir. Müşteri karmaşası hizmet kullanımını durdurmak, bir rakip hizmetine geçiş yapma, alt katmanlı deneyimini hizmetinde yapma veya katılım hizmetiyle azaltma müşterileri içerir.

Bu kullanım örneğinde şirketten hizmeti geliştirmek ve müşterileri Yardım özel destek hizmetleri kampanyalar oluşturmak için yakın vadede karmaşıklığı olasılığı olan müşterileri belirlemek için Fransızca telekomünikasyon turuncu veri arayın.

Telekomünikasyon şirketler rekabet Pazar karşılaşıyor. Çok sayıda hizmet sağlayıcılar karmaşıklığı için postpaid müşterilerden gelir kaybedersiniz. Bu nedenle özelliği müşteri karmaşası doğru bir şekilde tanımlamak için çok büyük bir rekabet avantajı olabilir.

Telekomünikasyon müşteri karmaşası katkıda bulunan Etkenler bazıları şunlardır:

* Algılanan sık hizmet kesilmelerini
* Çevrimiçi/perakende mağazaları kötü müşteri hizmeti deneyimleri
* (Daha iyi ailesi planı, veri planı, vb.) rakip diğer hizmet sağlayıcılar önerileri.

Bu çözümde Tahmine dayalı bir müşteri telekomünikasyon şirketler için karmaşası model oluşturmanın somut bir örnek kullanacağız.

## <a name="prerequisites"></a>Önkoşullar

* Bir [Azure hesabı](https://azure.microsoft.com/free/) (ücretsiz deneme kullanılabilir)

* Yüklü bir kopyasını [Azure Machine Learning çalışma ekranı](../service/overview-what-is-azure-ml.md) aşağıdaki [hızlı başlangıç Yükleme Kılavuzu](../service/quickstart-installation.md) programı yüklemek ve bir çalışma alanı oluşturmak için

* Yüklü ve yerel olarak çalışan Docker altyapısına varsa operationalization için en iyisidir. Aksi durumda, küme seçeneğini kullanır ancak Azure kapsayıcı Hizmeti'ni (ACS) çalıştıran pahalı olabileceğini unutmayın.

* Bu çözüm, Azure Machine Learning çalışma ekranı Docker altyapısına yerel olarak yüklü Windows 10'çalıştığını varsayar. MacOS kullanıyorsanız talimat büyük ölçüde aynıdır.

## <a name="create-a-new-workbench-project"></a>Yeni bir çalışma ekranı projesi oluşturma

Bu örnek bir şablon kullanarak yeni bir proje oluşturun:
1.  Açık Azure Machine Learning çalışma ekranı
2.  Üzerinde **projeleri** sayfasında, **+** oturum ve seçin **yeni proje**
3.  İçinde **yeni proje oluştur** bölmesinde, yeni projeniz için bilgileri doldurun
4.  İçinde **arama proje şablonları** arama kutusu, "Müşteri karmaşıklığı tahmin" yazın ve şablonu seçin
5.  **Oluştur**'a tıklayın

## <a name="data-description"></a>Veri açıklaması

Veri çözümde kullanılan SIDKDD 2009 rekabet kümesidir. Çağrılır `CATelcoCustomerChurnTrainingSample.csv` ve bulunan [ `data` ](https://github.com/Azure/MachineLearningSamples-ChurnPrediction/tree/master/data) klasör. Dataset turuncu Fransızca telekomünikasyon şirketten heterojen gürültülü veriler (sayısal/kategorik değişkenler) oluşur ve anonim.

Yakala değişkenleri müşteri demografik bilgileri, çağrı istatistikleri (örneğin, Ortalama çağrı süresi, çağrı hata oranı, vb.), bilgi, uyumlu istatistikleri sözleşme. Karmaşası değişken ikili (0 - değil karmaşıklığı, 1 - karmaşıklığı).

## <a name="scenario-structure"></a>Senaryo yapısı

Klasör yapısı şu şekilde düzenlenir:

__veri__: çözümde kullanılan veri kümesi içerir  

__belgeleri__: tüm uygulamalı labs içerir

Çözüm taşımak için uygulamalı Labs sırasını aşağıdaki gibidir:
1. Veri hazırlama:, veri hazırlığı veri klasöründe ilgili ana dosyasıdır `CATelcoCustomerChurnTrainingSample.csv`
2. Model oluşturma ve değerlendirme: Model oluşturma ve değerlendirme kök klasöründe ilgili ana dosyasıdır `CATelcoCustomerChurnModeling.py`
3. Model oluşturma ve değerlendirme .dprep olmadan: kök klasöründe bu görev için ana dosyasıdır `CATelcoCustomerChurnModelingWithoutDprep.py`
4. Operationalization: Ana deloyment için model dosyalardır (`model.pkl`) ve `churn_schema_gen.py`

| Sıra| Dosya Adı | Realted dosyaları |
|--|-----------|------|
| 1 | [`DataPreparation.md`](https://github.com/Azure/MachineLearningSamples-ChurnPrediction/blob/master/docs/DataPreparation.md) | 'data/CATelcoCustomerChurnTrainingSample.csv' |
| 2 | [`ModelingAndEvaluation.md`](https://github.com/Azure/MachineLearningSamples-ChurnPrediction/blob/master/docs/ModelingAndEvaluation.md) | 'CATelcoCustomerChurnModeling.py' |
| 3 | [`ModelingAndEvaluationWithoutDprep.md`](https://github.com/Azure/MachineLearningSamples-ChurnPrediction/blob/master/docs/ModelingAndEvaluationWithoutDprep.md) | 'CATelcoCustomerChurnModelingWithoutDprep.py' |
| 4 | [`Operationalization.md`](https://github.com/Azure/MachineLearningSamples-ChurnPrediction/blob/master/docs/Operationalization.md) | 'model.pkl'<br>'churn_schema_gen.py' |

Yukarıda açıklanan sıralı şekilde Labs izleyin.

## <a name="conclusion"></a>Sonuç
Bu aktarır senaryo Azure Machine Learning çalışma ekranı kullanarak karmaşası tahmin gerçekleştirme gösterilmektedir. Biz öncelikle veri veri hazırlığı araçlarını kullanarak özellik mühendislik tarafından izlenen gürültülü ve heterojen verileri işlemek için temizleme gerçekleştirilir. Biz açık kaynaklı makine öğrenimi araçları oluşturmak ve bir sınıflandırma modeli değerlendirmek için kullanılan, sonra yerel docker kapsayıcısı üretim için hazır hale getirme modeli dağıtmak için kullanılır.
