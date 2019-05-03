---
title: 'Modeli değerlendirme: Modül başvurusu'
titleSuffix: Azure Machine Learning service
description: Eğitilen bir modelin doğruluğunu ölçmek için Azure Machine Learning hizmetinde Evaluate Model modülü kullanmayı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/06/2019
ROBOTS: NOINDEX
ms.openlocfilehash: 40a8247c22da1f7a057e222565ffb2ec4c6b7fb3
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/02/2019
ms.locfileid: "65028748"
---
# <a name="evaluate-model-module"></a>Model modülü değerlendir

Bu makalede bir modül, Azure Machine Learning hizmeti için görsel arabirim (Önizleme).

Eğitilen bir modelin doğruluğunu ölçmek için bu modülü kullanın. Oluşturulan bir modelden puan içeren bir veri kümesi sağlayın ve **Evaluate Model** modülü bir dizi endüstri standardı değerlendirme ölçümleri hesaplar.
  
 Tarafından döndürülen ölçümler **Evaluate Model** değerlendirmesini modelinin türüne bağlıdır:  
  
-   **Sınıflandırma modelleri**    
-   **Regresyon modelleri**    



> [!TIP]
> Modeli değerlendirme için yeni başladıysanız, Dr ile video serisi öneririz. Bir parçası olarak Stephen Elston [makine öğrenme kursunun](https://blogs.technet.microsoft.com/machinelearning/2015/09/08/new-edx-course-data-science-machine-learning-essentials/) EdX öğesinden. 


Kullanmak için üç yol vardır **Evaluate Model** Modülü:

+ Puanları eğitim verileriniz üzerinde oluşturun ve bu puanları temel alarak modeli değerlendirme
+ Puanları modeli oluşturur, ancak bu puanları temel ayrılmış bir sınama kümesi puanlar karşılaştırın
+ Puanları, iki farklı ancak ilişkili model, aynı veri kümesini kullanarak karşılaştırın

## <a name="use-the-training-data"></a>Eğitim verileri kullanma

Bir model değerlendirmek için bir dizi girdi sütunlarını ve puanlarını içeren bir veri kümesi bağlanmanız gerekir.  Başka bir veri varsa, özgün kümenizi kullanabilirsiniz.

1. Connect **veri kümesi Puanlanmış** , çıktı [Score Model](./score-model.md) giriş **Evaluate Model**. 
2. Tıklayın **Evaluate Model** modülü ve değerlendirme puanları oluşturmak için denemeyi çalıştırın.

## <a name="use-testing-data"></a>Test verilerini kullanma

Özgün veri kümeniz, eğitim ve veri kümelerini kullanarak, test içinde ayırmak için machine learning'de yaygın bir senaryo olduğundan [bölünmüş](./split-data.md) modül veya [bölüm ve örnek](./partition-and-sample.md) modülü. 

1. Connect **veri kümesi Puanlanmış** , çıktı [Score Model](score-model.md) giriş **Evaluate Model**. 
2. Sağ giriş sınama verileri içeren verileri bölme modülünün çıkışına bağlayın **Evaluate Model**.
2. Tıklayın **Evaluate Model** modülü ve select **Seçileni Çalıştır** değerlendirme puanları oluşturulacak.

## <a name="compare-scores-from-two-models"></a>İki modellerinden puanları karşılaştırın

Puanları için ikinci bir set de bağlanabilirsiniz **Evaluate Model**.  Puanları, sonuçları bilinen bir paylaşılan değerlendirme kümesi veya bir dizi farklı bir model aynı verilere yönelik sonuçları olabilir.

Sonuçları aynı verileri iki farklı modellerdeki kolayca karşılaştırabilirsiniz çünkü bu yararlı bir özelliktir. Ya da farklı parametrelerle aynı veriler üzerinde iki farklı çalışmalardan puanları karşılaştırma.

1. Connect **veri kümesi Puanlanmış** , çıktı [Score Model](score-model.md) giriş **Evaluate Model**. 
2. İkinci model için Model Puanlama modülü çıkışı için sağ girişine bağlayın **Evaluate Model**.
3. Sağ **Evaluate Model**seçip **Seçileni Çalıştır** değerlendirme puanları oluşturulacak.

## <a name="results"></a>Sonuçlar

Çalıştırdıktan sonra **Evaluate Model**modülü sağ tıklatın ve seçin **değerlendirme sonuçlarını** sonuçları görmek için. Şunları yapabilirsiniz:

+ Diğer araçlarla daha kolay analiz için veri kümesi, sonuçları Kaydet
+ Arabiriminde bir görselleştirme oluşturma

Veri kümeleri için her iki girişleri bağlanırsanız **Evaluate Model**, her iki veri kümesini veya her iki modelleri için ölçümleri sonuçları içerir.
Model veya sol bağlantı noktasına bağlı veri ölçümleri için veri kümesi, ardından, rapor veya doğru bağlantı noktasına bağlı model ilk sunulur.  

Örneğin, aşağıdaki resimde bir karşılaştırma sonuçları arasında aynı verileri, ancak farklı parametrelerle oluşturulmuş iki kümeleme modeli temsil eder.  

![AML&#95;Comparing2Models](media/module/aml-comparing2models.png "AML_Comparing2Models")  

Bu kümeleme modeli olduğundan, değerlendirme sonuçlarını, iki regresyon modelinden puanları karşılaştırıldığında veya iki sınıflandırma modellerini kıyasla daha farklıdır. Ancak, genel sunu aynıdır. 

## <a name="metrics"></a>Ölçümler

Bu bölümde açıklanmaktadır modelleri ile kullanmak için desteklenen belirli türde döndürülen ölçümler **Evaluate Model**:

+ [Sınıflandırma modelleri](#bkmk_classification)
+ [regresyon modelleri](#bkmk_regression)

###  <a name="bkmk_classification"></a> Sınıflandırma modelleri için ölçümleri

Aşağıdaki ölçümler, sınıflandırma modellerini değerlendirirken raporlanır. Modeli karşılaştırmak, değerlendirme için seçtiğiniz ölçüm göre sıralanır.  
  
-   **Doğruluk** sınıflandırma modeli özelliği toplam çalışmaları için doğru sonuçlar oranı olarak ölçer.  
  
-   **Duyarlık** doğru sonuçlar oranı tüm pozitif sonuç olduğunu.  
  
-   **Geri çağırma** modeli tarafından döndürülen tüm doğru sonuçlar bölümüdür.  
  
-   **F-puanı** kesinlik ve geri çağırma 0 ve 1, 1 olduğu ideal F puanı değeri arasında ağırlıklı ortalaması hesaplanır.  
  
-   **AUC** çizilen eğri alanında ile ölçüler pozitif y ekseni ve yanlış pozitif x ekseninde üzerinde true. Bu ölçüm, farklı türlerde modeli sayesinde tek bir sayı sağladığı için kullanışlıdır.  
  
- **Ortalama Günlük kaybı** cezası yanlış sonuçlar için ifade etmek için kullanılan tek bir puan olduğu. İki olasılık dağıtımları – true ve bir modeli arasındaki fark olarak hesaplanır.  
  
- **Günlük kaybı eğitim** rasgele tahmin sınıflandırıcı avantajı temsil eden tek bir puan. Günlük kaybı etiketleri bilinen değerlerle (zemin truth) çıkarır olasılıklar karşılaştırarak modelinizin belirsizlik ölçer. Bir bütün olarak modelin günlük kaybını en aza indirmek istiyorsunuz.

##  <a name="bkmk_regression"></a> Regresyon modelleri için ölçümleri
 
Regresyon modelleri için döndürülen ölçümler genellikle hata miktarı tahmin etmek için tasarlanmıştır.  Model verileri de gözlemlenen ve tahmin edilen değerler arasındaki farkın küçükse uyacak şekilde değerlendirilir. Ancak, Kalanlar desene (herhangi bir tahmin edilen puan ve karşılık gelen gerçek değeri arasındaki farkı), çok olası sapması modelinde hakkında anlayabilirsiniz.  
  
 Aşağıdaki ölçümler, regresyon modelleri değerlendirmesi için rapor edilir. Modeli karşılaştırmak, değerlendirme için seçtiğiniz ölçüm göre sıralanır.  
  
- **Mean Absolute error (MAE)** tahminler elde etmek için gerçek sonuçların ne kadar yakın olan ölçer; bu nedenle, daha düşük bir puan daha iyidir.  
  
- **Kök ortalama karesi alınmış hata (RMSE)** modelinde hata özetleyen tek bir değer oluşturur. Fark karesini tarafından ölçüm fazla öngörü ve eksik tahmin arasındaki farkı yok sayar.  
  
- **Göreli mutlak hata (RAE)** beklenen ve gerçek değerler; arasındaki göreli mutlak fark göreli olduğundan ortalama farkı aritmetik ortalamasını tarafından ayrılmıştır.  
  
- **Göreli karesi alınmış hata (RSE)** benzer şekilde toplam karesi alınmış hata tahmin edilen değerler tarafından gerçek değerlerin toplam karesi alınmış hata bölerek normalleştirir.  
  
- **Sıfır bir hata (MZOE) Anlama** tahmin doğru olup olmadığını gösterir.  Diğer bir deyişle: `ZeroOneLoss(x,y) = 1` olduğunda `x!=y`; Aksi takdirde `0`.
  
- **Katsayısı**genellikle denilen R<sup>2</sup>, 0 ile 1 arasında bir değer olarak Tahmine dayalı model gücünü temsil eder. Sıfır, model rastgele anlamına gelir (hiçbir şey açıklanmıştır); 1 bir mükemmel anlamına gelir. Ancak, dikkat R yorumlama içinde kullanılması gereken<sup>2</sup> değerleri, düşük değerler tamamen normal olabilir ve yüksek değerler, şüpheli olabilir.
  

## <a name="next-steps"></a>Sonraki adımlar

Bkz: [kullanılabilir modül kümesini](module-reference.md) Azure Machine Learning hizmetine. 