---
title: '2. adım: Verileri bir Machine Learning denemesine yükleme | Microsoft Docs'
description: "Adım 2 / geliştirme Tahmine dayalı çözüm Kılavuzu: karşıya yükleme, Azure Machine Learning Studio'ya genel veri depolanan."
services: machine-learning
documentationcenter: ''
author: ericlicoding
ms.custom: (previous ms.author=hshapiro, author=heatherbshapiro)
ms.author: amlstudiodocs
manager: hjerez
editor: cgronlun
ms.assetid: 9f4bc52e-9919-4dea-90ea-5cf7cc506d85
ms.service: machine-learning
ms.component: studio
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/23/2017
ms.openlocfilehash: 81f0cd108075d39c4694163736ad391cf938f958
ms.sourcegitcommit: fa758779501c8a11d98f8cacb15a3cc76e9d38ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2018
ms.locfileid: "52263715"
---
# <a name="walkthrough-step-2-upload-existing-data-into-an-azure-machine-learning-experiment"></a>Kılavuz Adımı 2: Mevcut verileri bir Azure Machine Learning denemesine yükleme
Bu kılavuz, ikinci adımıdır [bir Azure Machine learning'de Tahmine dayalı analiz çözümü geliştirin](walkthrough-develop-predictive-solution.md)

1. [Bir Machine Learning çalışma alanı oluşturma](walkthrough-1-create-ml-workspace.md)
2. **Mevcut verileri yükleme**
3. [Yeni bir deneme oluşturma](walkthrough-3-create-new-experiment.md)
4. [Modelleri eğitme ve değerlendirme](walkthrough-4-train-and-evaluate-models.md)
5. [Web hizmetini dağıtma](walkthrough-5-publish-web-service.md)
6. [Web hizmetine erişme](walkthrough-6-access-web-service.md)

- - -
Kredi riski için Tahmine dayalı bir modeli geliştirmek için eğitmek ve sonra da modeli test etmek için kullanabileceğiniz veri ihtiyacımız var. Bu kılavuz için UC Irvine Machine Learning depodan "UCI Statlog (Alman kredi veri) veri kümesi" kullanacağız. Burada bulabilirsiniz:  
<a href="http://archive.ics.uci.edu/ml/datasets/Statlog+(German+Credit+Data)">http://archive.ics.uci.edu/ml/datasets/Statlog+(German+Credit+Data)</a>

Adlı dosyayı kullanacağız **german.data**. Bu dosya yerel sabit sürücünüze indirin.  

**German.data** veri kümesi iade için son 1000 Başvuranlar için 20 değişkenlerin satırları içerir. 20 Bu değişkenleri temsil eden veri kümesinin özellikler kümesi ( *özellik vektör*), her iade başvuran için tanımlayıcı özellikleri sağlar. Her satırda başka bir sütuna Başvuranın hesaplanan kredi riski, düşük kredi riskini ve 300 bir yüksek riskli tanımlanmış 700 başvuran ile temsil eder.

UCI Web sitesi öznitelikleri özellik vektör bu veriler için bir açıklama sağlar. Bu, finansal bilgi, kredi geçmişi, iş durumu ve kişisel bilgileri içerir. Her başvuran için ikili bir derecelendirme düşük olup olmadığını gösteren belirli veya yüksek olan kredi riski. 

Tahmine dayalı analiz modeli eğitmek için bu verileri kullanacağız. Modelimiz, işimiz bittiğinde, isterse bir düşük veya yüksek kredi riski mi olduğunu tahmin etmek ve yeni bir kullanıcı için bir özellik vektör kabul olmalıdır.  

İlginç bir sürpriz aşağıda verilmiştir. Açıklama UCI Web sitesinde veri kümesinin ne bahsetmeleri bir kişinin kredi riski misclassify biz, bunu maliyetlerini.
Model gerçekten düşük kredi riski kişi için yüksek kredi riskini tahmin, modeli bir misclassification yaptı.
Ancak ters misclassification Finans kurumu için beş kat daha maliyetlidir: model yüksek kredi riski aslında bir kişi için düşük kredi riskini tahmin durumunda.

Bu nedenle bu türdeki misclassification maliyetini beş kat daha yüksek değerinden başka bir şekilde misclassifying böylece, modeli eğitmek istiyoruz.
Bu bizim denemede modeli eğitimindeki yapmak için bir basit bir yüksek kredi riski biriyle temsil eden (beş) girişler çoğaltarak yoludur. Yüksek riskli gerçekten olduklarında model birisi düşük kredi riski olarak misclassifies, daha sonra modeli, aynı misclassification beş kat kez her yinelemesi için yapar. Bu, bu hata eğitim sonuçlarında maliyetini artırır.


## <a name="convert-the-dataset-format"></a>Veri kümesi biçimine dönüştürün
Özgün veri kümesinden boşluk ayrılmış bir biçim kullanır. Veri kümesi alanları virgül ile değiştirerek biz dönüştürme machine Learning Studio'da daha iyi bir virgülle ayrılmış değer (CSV) dosyası ile çalışır.  

Bu verileri dönüştürmek için birçok yolu vardır. Aşağıdaki Windows PowerShell komutunu kullanarak bir yoludur:   

    cat german.data | %{$_ -replace " ",","} | sc german.csv  

UNIX sed komutunu kullanarak başka bir yoludur:  

    sed 's/ /,/g' german.data > german.csv  

Her iki durumda da adlı dosyadaki verileri virgülle ayrılmış bir sürümünü oluşturduk **german.csv** , bizim deneme kullanabiliriz.

## <a name="upload-the-dataset-to-machine-learning-studio"></a>Machine Learning Studio'ya veri kümesini karşıya yükleme
CSV biçiminde verilerin dönüştürüldükten sonra Machine Learning Studio'ya karşıya gerekir. 

1. Machine Learning Studio'ya giriş sayfasını açın ([https://studio.azureml.net](https://studio.azureml.net)). 

2. Menüye tıklayarak ![menü][1] penceresinin sol üst köşesinde tıklayın **Azure Machine Learning**seçin **Studio**ve oturum açın.

3. Tıklayın **+ yeni** pencerenin alt kısmındaki.

4. Seçin **veri KÜMESİ**.

5. Seçin **yerel DOSYADAN**.

    ![Yerel bir dosyadan bir veri kümesi Ekle][2]

6. İçinde **yeni bir veri kümesi karşıya** iletişim kutusunda, tıklayın **Gözat** ve bulma **german.csv** oluşturduğunuz dosya.

7. Veri kümesi için bir ad girin. Bu kılavuz için çağrı "UCI Almanca kredi kartı verileri".

8. Veri türü için **üst bilgi içeren genel bir CSV dosyası (. nh.csv)**.

9. İsterseniz bir açıklama ekleyin.

10. Tıklayın **Tamam** işaretleyin.  

    ![Veri kümesi karşıya yükleme][3]

Bu, bir deneme kullanabiliriz bir veri kümesi modüle verileri yükler.

Tıklayarak Studio'ya yüklediğiniz veri kümelerini yönetebilirsiniz **veri KÜMELERİ** Studio penceresinin sol tarafında için sekmesinde.

![Veri kümelerini Yönet][4]

Bir denemenin diğer veri türleri içeri aktarma hakkında daha fazla bilgi için bkz. [eğitim verilerinizi Azure Machine Learning Studio'ya alma](import-data.md).

**Sonraki: [yeni bir deneme oluşturma](walkthrough-3-create-new-experiment.md)**

[1]: media/walkthrough-2-upload-data/menu.png
[2]: media/walkthrough-2-upload-data/add-dataset.png
[3]: media/walkthrough-2-upload-data/upload-dataset.png
[4]: media/walkthrough-2-upload-data/dataset-list.png
