---
title: Azure Machine Learning ile müşteri karmaşıklığı tahmini | Microsoft Docs
description: Azure ML Workbench kullanarak karmaşıklığı analizi gerçekleştirme.
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
ms.date: 09/17/2017
ms.author: miprasad
ms.openlocfilehash: 7c7b50098cfd1bcac534156dd905b37affab80bd
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2018
ms.locfileid: "35648222"
---
# <a name="customer-churn-prediction-using-azure-machine-learning"></a>Azure Machine Learning ile müşteri karmaşıklığı tahmini

Ortalama olarak, mevcut müşterilerin beş kat daha yeni bir tane işe alma maliyetini. Sonuç olarak, Pazarlama yöneticileri genellikle kendilerini müşteri dalgalanması olasılığını tahmin etmeye ve dalgalanma oranını en aza indirmek için gerekli eylemleri bulmaya çalışır bulun.

Amacı, bu çözüm, Azure Machine Learning Workbench'i kullanarak Tahmine dayalı karmaşası analytics göstermektir. Bu çözüm, değişim sıklığı Tahmine dayalı veri işlem hatları için Perakendeciler geliştirmek için kullanımı kolay bir şablonu sağlar. Şablonu farklı veri kümelerini ve değişim farklı tanımları ile kullanılabilir. Uygulamalı örnek amacı şudur:

1. Temiz ve müşteri karmaşıklığı analizi için ilişki veri almak için Azure Machine Learning Workbench'in veri hazırlama araçları anlayın.

2. Gürültülü fazla heterojen veri işlemek için özellik dönüşüm gerçekleştirin.

3. Üçüncü taraf kitaplıklar tümleştirme (gibi `scikit-learn` ve `azureml`) Bayes ve ağaç tabanlı sınıflandırıcılar, değişim sıklığı tahmin etmeye yönelik geliştirme.

4. Dağıtın.

## <a name="link-of-the-gallery-github-repository"></a>Galeri GitHub deposunun bağlantısı
Tüm kod barındırıldığı ortak GitHub deposu bağlantısı aşağıda verilmiştir:

[https://github.com/Azure/MachineLearningSamples-ChurnPrediction](https://github.com/Azure/MachineLearningSamples-ChurnPrediction)

## <a name="use-case-overview"></a>Kullanım örneği genel bakış
Şirketler, müşteri kaybını yönetmek için bir etkin stratejisine ihtiyaç duyar. Müşteri karmaşası müşteriler bir hizmet kullanımını durdurmak, rakip bir hizmete geçme, hizmetinde bir daha düşük katmanlı deneyime geçiş veya hizmetle etkileşimini azaltmayı içerir.

Bu kullanım örneğinde önleyecek özel destek hizmetleri kampanyaları oluşturun ve hizmeti geliştirmek için vadede olasılığı müşterileri belirlemek için Fransız bir telekomünikasyon şirketinin turuncu gelen verileri arayın.

Telekom şirketlerinden rekabetçi olan bir pazarda karşı karşıyadır. Birçok operatörler olasılığı kaybederek müşterilerden gelir kaybedersiniz. Bu nedenle müşteri dalgalanması doğru bir şekilde belirleyebilmek büyük bir rekabet üstünlüğü olabilir.

Telekomünikasyon müşteri kaybı faktörlerini bazıları şunlardır:

* Algılanan sıklıkla yaşanan servis kesintileri
* Çevrimiçi/perakende mağazalarda yetersiz müşteri hizmeti deneyimleri
* Rakip operatörlerden (daha iyi bir aile planı, veri planı, vb.) alınan sunar.

Bu çözümde Tahmine dayalı bir müşteri karmaşıklığı modeli için Telekom şirketlerinden oluşturmanın somut bir örnek kullanacağız.

## <a name="prerequisites"></a>Önkoşullar

* Bir [Azure hesabı](https://azure.microsoft.com/free/) (ücretsiz denemeler kullanılabilir)

* Yüklü bir kopyasını [Azure Machine Learning Workbench](../service/overview-what-is-azure-ml.md) aşağıdaki [hızlı başlangıç Yükleme Kılavuzu](../service/quickstart-installation.md) programı yüklemek ve bir çalışma alanı oluşturmak için

* Docker altyapısı yüklü ve yerel olarak çalışan varsa getirmek için en iyisidir. Aksi takdirde küme seçeneğini kullanın ancak bir Azure Container Service (ACS) çalıştıran pahalı olabileceğini unutmayın.

* Bu çözüm, Azure Machine Learning Workbench ile Docker altyapısının yerel olarak yüklü Windows 10'da çalıştığını varsayar. MacOS kullanıyorsanız, yönerge büyük ölçüde aynıdır.

## <a name="create-a-new-workbench-project"></a>Workbench yeni bir proje oluşturun

Bu örnekte, şablon olarak kullanarak yeni bir proje oluşturun:
1.  Açık bir Azure Machine Learning Workbench'i
2.  Üzerinde **projeleri** sayfasında **+** açıp seçmek **yeni proje**
3.  İçinde **yeni proje oluştur** bölmesinde, yeni projeniz için bilgileri girin
4.  İçinde **proje şablonlarında Ara** arama kutusuna, "Customer Churn Prediction" yazın ve şablonu seçin
5.  **Oluştur**'a tıklayın

## <a name="data-description"></a>Veri açıklaması

Çözümde kullanılan veri kümesi tarafından SIDKDD 2009 rekabet ' dir. Çağrıldığı `CATelcoCustomerChurnTrainingSample.csv` ve bulunan [ `data` ](https://github.com/Azure/MachineLearningSamples-ChurnPrediction/tree/master/data) klasör. Veri kümesi anonimleştirilmiştir ve daha heterojen gürültülü veri (/ kategorik sayısal değişkenler) Fransızca Telekom şirketten turuncu oluşur.

Yakala değişkenleri müşteri demografik bilgiler, çağrı istatistikleri (örneğin, ortalama çağırma süresi çağrısı hata oranı, vb.), bilgi, uyumlu istatistikleri sözleşme. Değişim sıklığı değişkendir ikili (0 - değil değişim sıklığı, 1 - karmaşıklığı).

## <a name="scenario-structure"></a>Senaryo yapısı

Klasör yapısı aşağıdaki gibi düzenlenir:

__veri__: çözümde kullanılan veri kümesini içeren  

__docs__: uygulamalı laboratuvarların tümünü içerir

Çözümü yürütmek için uygulamalı laboratuvarlar sırası aşağıdaki gibidir:
1. Veri hazırlama:, veri hazırlama veri klasöründeki ilgili ana dosyasıdır `CATelcoCustomerChurnTrainingSample.csv`
2. Modelleme ve değerlendirme: Model oluşturma ve değerlendirme Kök klasörde ilgili ana dosyasıdır `CATelcoCustomerChurnModeling.py`
3. Modelleme ve değerlendirme .dprep olmadan: kök klasöründe bu görev için ana dosyasıdır `CATelcoCustomerChurnModelingWithoutDprep.py`
4. Kullanıma hazır hale getirme:, Sık karşılaşılan için ana dosyaları modeline (`model.pkl`) ve `churn_schema_gen.py`

| Sipariş verme| Dosya Adı | Kullanıcılarla ilgili dosyalar |
|--|-----------|------|
| 1 | [`DataPreparation.md`](https://github.com/Azure/MachineLearningSamples-ChurnPrediction/blob/master/docs/DataPreparation.md) | 'data/CATelcoCustomerChurnTrainingSample.csv' |
| 2 | [`ModelingAndEvaluation.md`](https://github.com/Azure/MachineLearningSamples-ChurnPrediction/blob/master/docs/ModelingAndEvaluation.md) | 'CATelcoCustomerChurnModeling.py' |
| 3 | [`ModelingAndEvaluationWithoutDprep.md`](https://github.com/Azure/MachineLearningSamples-ChurnPrediction/blob/master/docs/ModelingAndEvaluationWithoutDprep.md) | 'CATelcoCustomerChurnModelingWithoutDprep.py' |
| 4 | [`Operationalization.md`](https://github.com/Azure/MachineLearningSamples-ChurnPrediction/blob/master/docs/Operationalization.md) | 'model.pkl'<br>'churn_schema_gen.py' |

Laboratuvarlar yukarıda açıklanan sıralı bir şekilde izleyin.

## <a name="conclusion"></a>Sonuç
Bu uygulamalı karmaşıklığı tahmini Azure Machine Learning Workbench'i kullanarak gerçekleştirme senaryosu gösterilmiştir. İlk veri veri hazırlama araçlarını kullanarak özellik Mühendisliği tarafından izlenen gürültülü ve heterojen veri işlemek için temizleme gerçekleştirdiğimiz. Biz açık kaynak machine learning araçları oluşturun ve bir sınıflandırma modeli değerlendirmek için kullanılan, sonra yerel bir docker kapsayıcısı üretim için hazır hale getirme modeli dağıtmak için kullanılır.
