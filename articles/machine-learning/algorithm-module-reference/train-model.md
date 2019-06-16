---
title: 'Modeli eğitme: Modül başvurusu'
titleSuffix: Azure Machine Learning service
description: Nasıl kullanacağınızı öğrenin **modeli eğitme** modülü Azure Machine Learning hizmetinde bir sınıflandırma veya regresyon modelini eğitmek için.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ROBOTS: NOINDEX
ms.openlocfilehash: 29d0f698456b83c1520a92bc7df47b26540325f4
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65028118"
---
# <a name="train-model-module"></a>Train Model Modülü

Bu makalede bir modül, Azure Machine Learning hizmeti için görsel arabirim (Önizleme).

Bir sınıflandırma veya regresyon modelini eğitmek için bu modülü kullanın. Eğitim gerçekleştikten sonra model tanımlı ve parametrelerini ayarlayın ve ekli veri gerektirir. Ayrıca **modeli eğitme** yeni verilerle birlikte var olan bir modeli yeniden eğitme için. 

## <a name="how-the-training-process-works"></a>Eğitim işlem nasıl çalışır?

Azure Machine Learning, oluşturma ve machine learning modeli kullanarak genellikle üç adım, bir işlemdir. 

1. Bir model, belirli türde bir algoritma seçme ve kendi parametrelerini veya hiperparametreleri tanımlama yapılandırın. Aşağıdaki modeli türlerinden herhangi birini seçin: 

    + **Sınıflandırma** modelleri, karmaşık sinir ağları, karar ağaçları ve karar ormanları ve diğer algoritmalar göre.
    + **Regresyon** modelleri, standart doğrusal regresyon içerebilir veya sinir ağları ve Bayes regresyon dahil olmak üzere, diğer algoritmalar kullanın.  

2. Etiketlenir ve veri algoritması ile uyumlu olan bir veri kümesi sağlar. Hem verileri hem de modele bağlanmak **modeli eğitme**.

    Hangi eğitim üreten bir belirli ikili verilerden öğrenilen istatistiksel düzenleri kapsülleyen iLearner biçimdir. Doğrudan değiştiremez veya bu biçim okuyun; Ancak, diğer modüllerin bu eğitilen modeli kullanabilirsiniz. 
    
    Modelin özelliklerine yönelik da görüntüleyebilirsiniz. Daha fazla bilgi için sonuçlar bölümüne bakın.

3. Alıştırma tamamlandıktan sonra eğitilen model biri ile kullanın [modülleri Puanlama](./score-model.md)yeni veri tahminlerde bulunmak için.

## <a name="how-to-use-train-model"></a>Nasıl kullanılacağını **modeli eğitme**  
  
1.  Azure Machine Learning'de model sınıflandırma veya regresyon modelinden yapılandırın.
    
2. Ekleme **modeli eğitme** modülünü deneme.  Bu modül altında bulabilirsiniz **Machine Learning** kategorisi. Genişletin **eğitme**ve ardından sürükleyin **modeli eğitme** denemenizi modüle.
  
3.  Sol giriş deneyimsiz modu ekleyin. Eğitim veri kümesi eklemek için sağ girişine **modeli eğitme**.

    Eğitim veri kümesi, bir etiket sütun içermelidir. Herhangi bir satır etiketsiz göz ardı edilir.
  
4.  İçin **etiket sütun**, tıklayın **Sütun seçiciyi Başlat**ve modeli için eğitim kullanabileceğiniz sonuçları içeren tek bir sütun seçin.
  
    - Sınıflandırma sorunlar için etiket sütunu ya da içermelidir **kategorik** değerleri veya **ayrık** değerleri. Bazı örnekler, bir Evet/Hayır derecelendirme, Hastalık sınıflandırma kod veya adı veya bir gelir grubu olabilir.  Noncategorical bir sütun seçerseniz, modül eğitim sırasında bir hata döndürür.
  
    -   Regresyon sorunlar için etiket sütunu içermelidir **sayısal** yanıt değişkeni temsil eden veriler. İdeal olarak sürekli bir ölçek sayısal veri temsil eder. 
    
    Örnekleri bir kredi risk puanı öngörülen zamanında hatası için bir sabit sürücü veya tahmin edilen bir çağrı merkezi çağrı sayısı için belirli gün veya saat olabilir.  Sayısal bir sütun seçmezseniz, bir hata alabilirsiniz.
  
    -   Kullanılacak etiket sütunu belirtmezseniz, Azure Machine Learning veri kümesinin meta verileri kullanarak uygun etiketi sütun olduğu Infer dener. Yanlış sütun seçer, bunu düzeltmek için Sütun seçiciyi kullanın.
  
    > [!TIP] 
    > Sütun Seçici üzerinden sorun yaşıyorsanız, bkz [kümesindeki sütunları seçme](./select-columns-in-dataset.md) için ipuçları. Bazı yaygın senaryolar ve kullanımına yönelik ipuçları açıklanır **kuralları ile** ve **ada göre** seçenekleri.
  
5.  Denemeyi çalıştırın. Çok fazla veri varsa, bu işlem biraz sürebilir.

## <a name="bkmk_results"></a> Sonuçları

Sonra modeli eğitilir:

+ Model parametreleri ve özellik ağırlıkları görüntülemek için seçin ve çıkış sağ **Görselleştir**.
+ Diğer denemeleri modelini kullanmak için model sağ tıklayıp **Modeli Kaydet**. Model için bir ad yazın. 

    Bu, modeli deneme yinelenen çalıştırıcıları tarafından güncelleştirilmemiş bir anlık görüntü olarak kaydeder.
+ Yeni değerleri modelini kullanmak için ona bağlanın [Score Model](./score-model.md) modülü, yeni giriş verileriyle birlikte.


## <a name="next-steps"></a>Sonraki adımlar

Bkz: [kullanılabilir modül kümesini](module-reference.md) Azure Machine Learning hizmetine. 