---
title: Görsel arabirim
titleSuffix: Azure Machine Learning service
description: Koşulları, kavramlar ve Azure Machine Learning hizmeti için (Önizleme) görsel arabirim oluşturan iş akışı hakkında bilgi edinin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: sgilley
author: sdgilley
ms.date: 05/15/2019
ms.openlocfilehash: be07e0f3438ea93312d4eb440e7e63b8f98e11b8
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67077370"
---
# <a name="what-is-the-visual-interface-for-azure-machine-learning-service"></a>Azure Machine Learning hizmeti için görsel arabirim nedir? 

Azure Machine Learning hizmeti için görsel arabirim (Önizleme), veri hazırlama, eğitme, test, dağıtma, yönetme ve makine öğrenimi modelleri, kod yazmaya gerek kalmadan izleme sağlar.

Programlama gerekmeden, görsel olarak bağlandığınız [veri kümeleri](#dataset) ve [modülleri](#module) modelinizi oluşturmak için.

Görsel bir arabirim kullanır, Azure Machine Learning hizmeti [çalışma](concept-workspace.md) için:

+ Yapıtları yazma [deneme](#experiment) çalışma alanı içinde çalışır.
+ Erişim [veri kümeleri](#dataset).
+ Kullanım [işlem kaynaklarını](#compute) çalışma denemeyi çalıştırın. 
+ Kayıt [modelleri](concept-azure-machine-learning-architecture.md#models).
+ [Dağıtma](#deployment) modeller web Hizmetleri işlem çalışma alanındaki kaynakları olarak.

![Görsel arabirim genel bakış](media/ui-concept-visual-interface/overview.png)

## <a name="workflow"></a>İş akışı

Görsel arabirim hızlıca oluşturun, test ve model üzerinde yinelemek için etkileşimli ve görsel bir tuval sunar. 

+ Sürükle ve bırak [modülleri](#module) tuvale.
+ Modüller birbirine bağlayarak bir [deneme](#experiment).
+ Machine Learning hizmeti çalışma alanının bilgi işlem kaynağını kullanarak denemeyi çalıştırın.
+ Denemeyi düzenleyebilir ve yeniden çalıştırarak model tasarımınızı yinelemek.
+ Hazır olduğunuzda, dönüştürmek, **eğitim denemesini** için bir **Tahmine dayalı denemeye**.
+ [Dağıtma](#deployment) Tahmine dayalı denemeye bir Web hizmeti, modelinize başkaları tarafından erişilebilir.

## <a name="experiment"></a>Deneme

Bir denemeyi sıfırdan oluşturabilir veya var olan bir örnek denemeyi şablon olarak kullanın.  Bir deney her çalıştırdığınızda yapıtları çalışma alanınızda depolanır.

Bir deney, veri kümeleri ve bir modeli oluşturmak için birbirine bağlama analitik modüllere oluşur. Geçerli bir deneme özellikle şu özelliklere sahiptir:

* Veri kümeleri yalnızca modüllere bağlanabilir.
* Modüller veri kümelerine veya diğer modüllere bağlanabilir.
* Modüllerin tüm giriş bağlantı noktalarının veri akışına bir tür bağlantısı olması gerekir.
* Gerekli tüm parametreleri her bir modül için ayarlamanız gerekir.

Örneği basit bir deneme için bkz: [hızlı başlangıç: Hazırlama ve Azure Machine Learning'de kod yazmaya gerek kalmadan verileri görselleştirme](ui-quickstart-run-experiment.md).

Tahmine dayalı analiz çözümünün daha kapsamlı bir kılavuzu için bkz. [Öğreticisi: Görsel arabirim ile otomobil fiyatını tahmin](ui-tutorial-automobile-price-train-score.md).

## <a name="dataset"></a>Veri kümesi

Bir veri kümesi model oluşturma işleminde kullanılacak görsel arabirim için yüklenen verilerdir. Birçok örnek veri kümesi ile denemenizi dahil edilir ve ihtiyaç duyarsanız daha çok veri kümesi yükleyebilirsiniz.

## <a name="module"></a>Modül

Bir modül, verilerinizde gerçekleştirebileceğiniz bir algoritmadır. Görsel arabirim veri alım işlevlerinden eğitim, Puanlama ve doğrulama işlemleri için değişiklik gösteren birçok modüle sahiptir.

Bir modül, modülün iç algoritmalarını yapılandırmak için kullanabileceğiniz parametreler kümesine sahip olabilir. Tuvalde bir modül seçtiğinizde modülün parametreleri tuvalin sağındaki Özellikler bölmesinde görüntülenir. Modelinizi ayarlamak için, bu bölmedeki parametreleri değiştirebilirsiniz.

![Modülü özellikleri](media/ui-concept-visual-interface/properties.png)

Bazı Yardım makine öğrenimi algoritma kitaplığı gezinmek için bkz: [algoritma ve modül başvurusu genel bakış](../algorithm-module-reference/module-reference.md)

## <a name="compute"></a> İşlem kaynakları

Denemeyi veya ana bilgisayar, dağıtılan Modellerinizi web Hizmetleri olarak çalıştırmak için çalışma alanınızı kaynaklardan işlem kullanın. Desteklenen işlem hedefleri şunlardır:


| Hedef işlem | Eğitim | Dağıtım |
| ---- |:----:|:----:|
| Azure Machine Learning işlem | ✓ | |
| Azure Kubernetes Service | | ✓ |

Hedefleri, Machine Learning için bağlı işlem [çalışma](concept-workspace.md). Çalışma alanınızda işlem hedeflerinizi yönetmek [Azure portalında](https://portal.azure.com).

## <a name="deployment"></a>Dağıtım

Tahmine dayalı analiz modeliniz hazır olduktan sonra bunu visual arabiriminden doğrudan bir web hizmeti olarak dağıtın.

Web Hizmetleri, uygulama ve, Puanlama modeli arasında bir arabirim sağlar. Bir dış uygulama, gerçek zamanlı Puanlama modeli ile iletişim kurabilir. Bir web hizmeti çağrısı bir dış uygulamaya tahmin sonuçlarını döndürür. Bir web hizmetine çağrı yapmak için web hizmeti dağıtılırken oluşturulan bir API anahtarını geçirirsiniz. Web hizmeti bir web programlama projeleri için popüler bir mimari seçimi olan REST'i temel alır.

Modelinizi öğrenmek için bkz: [Öğreticisi: Görsel arabirim ile makine öğrenme modeli dağıtma](ui-tutorial-automobile-price-deploy.md).

## <a name="next-steps"></a>Sonraki adımlar

* Tahmine dayalı analiz temellerini öğrenin ve makine öğrenimi ile [hızlı başlangıç: Hazırlama ve Azure Machine Learning'de kod yazmaya gerek kalmadan verileri görselleştirme](ui-quickstart-run-experiment.md).
* Örneklerden birini kullanın ve ihtiyaçlarınızı paketine değiştirin:
    * [Örnek 1 - regresyon: Tahmin](ui-sample-regression-predict-automobile-price-basic.md)
    * [2 - regresyon. örnek: Tahmin ve algoritmaları karşılaştırın](ui-sample-regression-predict-automobile-price-compare-algorithms.md)
    * [3 - sınıflandırma. örnek: Kredi riskini tahmin](ui-sample-classification-predict-credit-risk-basic.md)
    * [4 - sınıflandırma. örnek: (Maliyet hassas) kredi riskini tahmin](ui-sample-classification-predict-credit-risk-cost-sensitive.md)
    * [5 - sınıflandırma. örnek: Değişim sıklığı, deneyde ve yukarı satış tahmin edin](ui-sample-classification-predict-churn.md)
