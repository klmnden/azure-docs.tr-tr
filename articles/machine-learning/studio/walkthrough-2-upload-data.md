---
title: '2. adım: Verileri bir Machine Learning deneme yükleme | Microsoft Docs'
description: "Adım 2 / geliştirme Tahmine dayalı bir çözüm izlenecek yol: karşıya yükleme Azure Machine Learning Studio'ya genel verilere depolanır."
services: machine-learning
documentationcenter: ''
author: heatherbshapiro
ms.author: hshapiro
manager: hjerez
editor: cgronlun
ms.assetid: 9f4bc52e-9919-4dea-90ea-5cf7cc506d85
ms.service: machine-learning
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.openlocfilehash: f482b1273f83f5ae5bb4f1e64609767ee0c5fe32
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="walkthrough-step-2-upload-existing-data-into-an-azure-machine-learning-experiment"></a>Kılavuz Adımı 2: Mevcut verileri bir Azure Machine Learning denemesine yükleme
Bu izlenecek yol ikinci adımdır [Azure Machine learning'de Tahmine dayalı analiz çözümü geliştirme](walkthrough-develop-predictive-solution.md)

1. [Bir Machine Learning çalışma alanı oluşturma](walkthrough-1-create-ml-workspace.md)
2. **Mevcut verileri yükleme**
3. [Yeni bir deneme oluşturma](walkthrough-3-create-new-experiment.md)
4. [Modelleri eğitme ve değerlendirme](walkthrough-4-train-and-evaluate-models.md)
5. [Web hizmetini dağıtma](walkthrough-5-publish-web-service.md)
6. [Web hizmetine erişim](walkthrough-6-access-web-service.md)

- - -
Kredi riski için Tahmine dayalı bir modeli geliştirmek için eğitmek ve modeli test etmek için kullanabileceğiniz veri gerekiyor. Bu kılavuzda UC Irvine Machine Learning depodan "UCI Statlog (Almanca kredi veri) veri kümesi" kullanacağız. Burada bulabilirsiniz:  
<a href="http://archive.ics.uci.edu/ml/datasets/Statlog+(German+Credit+Data)">http://archive.ics.uci.edu/ml/datasets/Statlog+(German+Credit+Data)</a>

Adlı dosyayı kullanacağız **german.data**. Bu dosyayı yerel sabit diskinize yükleyin.  

**German.data** dataset kredi için son 1000 başvuran 20 değişkenlerin satırları içerir. Bu 20 değişkenleri veri kümesi'nin özellikler kümesini temsil eden ( *özelliği vektör*), her kredi başvuran için tanımlayıcı özellikleri sağlar. Her satırda ek bir sütun düşük kredi riski ve 300 yüksek riskli olarak tanımlanan 700 başvuran ile Başvuranın hesaplanan kredi riski temsil eder.

Bu veriler için özellik vektör özniteliklerini açıklamasını UCI sağlar. Bu, finansal bilgileri, kredi geçmişi, çalışma durumu ve kişisel bilgileri içerir. Her başvuran için ikili bir derecelendirme belirli bir düşük olup olmadığını gösteren veya yüksek olmuştur kredi riski. 

Tahmine dayalı bir analiz modeli eğitmek için bu verileri kullanacağız. Biz bittiğinde, modelimizi özelliği vektör yeni bir kullanıcı için kabul edin ve çözemiyorsa düşük veya yüksek kredi riski olup olmadığını tahmin etmek mümkün olması gerekir.  

Burada, ilginç geçiş verilmiştir. Veri kümesi UCI Web sitesinde açıklaması ne değinmektedir biz bir kişinin kredi riski misclassify, maliyetleri.
Model bir yüksek kredi riski gerçekten düşük kredi riski yapılandırabilecek için tahmin, model bir misclassification yaptı.
Ancak ters misclassification finansal kuruluştan beş kez daha pahalıdır: model düşük kredi riski gerçekte bir yüksek kredi riski yapılandırabilecek için tahmin durumunda.

Bu nedenle, böylece bu türdeki misclassification maliyetini beş kez daha yüksek başka bir şekilde misclassifying daha bizim modeli eğitmek istiyoruz.
Bu bizim deneme modelinde eğitimindeki yapmak için bir basit bir yüksek kredi riski birisiyle temsil eden (beş kez) girişler çoğaltarak yoludur. Gerçekte yüksek riskli olduğunuzda model birisi düşük kredi riski olarak misclassifies, daha sonra model, aynı misclassification beş kez kez için her yinelenen yapar. Bu, bu hata eğitim sonuçlarında maliyetini artırır.


## <a name="convert-the-dataset-format"></a>Veri kümesi biçimine dönüştürün
Özgün veri kümesi boş ayrılmış bir biçim kullanır. Virgül ile alanları değiştirerek dataset biz dönüştürme machine Learning Studio'da bir virgülle ayrılmış değer (CSV) dosyası ile daha iyi çalışır.  

Bu verileri dönüştürmek için birçok yolu vardır. Aşağıdaki Windows PowerShell komutunu kullanarak bir yöntemdir:   

    cat german.data | %{$_ -replace " ",","} | sc german.csv  

UNIX azaltılabilir komutunu kullanarak başka bir yöntemdir:  

    sed 's/ /,/g' german.data > german.csv  

Her iki durumda da adlı bir dosya verileri virgülle ayrılmış bir sürümünü oluşturduk **german.csv** , bizim deneme kullanırız.

## <a name="upload-the-dataset-to-machine-learning-studio"></a>Veri kümesi için Machine Learning Studio karşıya yükle
CSV biçimi veri dönüştürüldükten sonra Machine Learning Studio'ya karşıya gerekir. 

1. Machine Learning Studio giriş sayfasını açın ([https://studio.azureml.net](https://studio.azureml.net)). 

2. Menüsünü ![menü][1] penceresinin sol üst köşesinde tıklatın **Azure Machine Learning**seçin **Studio**ve oturum açın.

3. Tıklatın **+ yeni** pencerenin altındaki.

4. Seçin **DATASET**.

5. Seçin **yerel DOSYADAN**.

    ![Yerel bir dosyadan bir veri kümesi ekleyin][2]

6. İçinde **yeni bir veri kümesi karşıya** iletişim kutusunda, tıklatın **Gözat** ve bulma **german.csv** , oluşturduğunuz dosya.

7. Veri kümesi için bir ad girin. Bu kılavuzda "UCI Almanca kredi kartı verileri" çağrısı.

8. Veri türü için **üst bilgi içeren genel CSV dosyası (. nh.csv)**.

9. İsterseniz bir açıklama ekleyin.

10. Tıklatın **Tamam** onay işareti.  

    ![Veri kümesi karşıya yükle][3]

Bir deneme kullandığımız bir veri kümesi modüle veri yükler.

Tıklayarak Studio'ya yüklediğiniz veri kümeleri yönetebilir **veri KÜMELERİ** Studio penceresinin sol sekmesine.

![Veri kümelerini yönetme][4]

Bir deneme diğer veri türleri alma hakkında daha fazla bilgi için bkz: [eğitim verilerinizi Azure Machine Learning Studio'ya içeri](import-data.md).

**Sonraki: [yeni bir deneme oluşturma](walkthrough-3-create-new-experiment.md)**

[1]: media/walkthrough-2-upload-data/menu.png
[2]: media/walkthrough-2-upload-data/add-dataset.png
[3]: media/walkthrough-2-upload-data/upload-dataset.png
[4]: media/walkthrough-2-upload-data/dataset-list.png
