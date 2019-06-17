---
title: Oluşturma ve Portalı'nda denemeleri keşfedin
titleSuffix: Azure Machine Learning service
description: Oluşturma ve otomatik makine öğrenimi denemelerini portalında yönetme hakkında bilgi edinin
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: tsikiksr
author: tsikiksr
manager: cgronlun
ms.reviewer: nibaccam
ms.date: 05/02/2019
ms.openlocfilehash: a2a281fda9272fb794692becb0ca08f3cf791458
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65990150"
---
# <a name="create-and-explore-automated-machine-learning-experiments-in-the-azure-portal-preview"></a>Oluşturma ve otomatik makine öğrenimi denemelerini (Önizleme) Azure portalında keşfedin

 Bu makalede, oluşturma, çalıştırma ve otomatik makine öğrenimi denemelerini tek satırlık bir kod olmadan Azure portalında keşfedin öğrenin. Otomatik makine öğrenimi belirli verileriniz için hızla bir machine learning modeli oluşturmak için kullanılacak en iyi algoritmayı seçme işlemini otomatikleştirir. [Otomatik machine learning hakkında daha fazla bilgi edinin](concept-automated-ml.md).

 Daha fazla kod tabanlı bir deneyim tercih ederseniz, ayrıca [otomatik, makine öğrenimi denemelerini python'da yapılandırma](how-to-configure-auto-train.md) ile [Azure Machine Learning SDK'sı](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py).

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Azure aboneliğiniz yoksa başlamadan önce ücretsiz bir hesap oluşturun. Deneyin [Azure Machine Learning hizmetinin ücretsiz veya Ücretli sürümüne](https://aka.ms/AMLFree) bugün.

* Bir Azure Machine Learning hizmeti çalışma alanı. Bkz: [bir Azure Machine Learning hizmeti çalışma alanı oluşturma](https://docs.microsoft.com/azure/machine-learning/service/setup-create-workspace).

## <a name="get-started"></a>başlarken

Çalışma alanınızın sol bölmesine gidin. Machine Learning otomatik yazma (Önizleme) bölümü altında seçin.

![Azure portalında gezinme bölmesi](media/how-to-create-portal-experiments/nav-pane.png)

 Bu otomatik Machine Learning ile tüm denemeler yapmak, ilk kez kullanıyorsanız, aşağıdaki görürsünüz:

![Azure portal deneme giriş sayfası](media/how-to-create-portal-experiments/landing-page.png)

Aksi takdirde, otomatik machine learning panonuzu otomatik, makine öğrenimi denemelerini, SDK'sı ile oluşturulan dahil olmak üzere bir bakış görürsünüz. Burada filtreleyebilirsiniz ve tarihe göre çalıştırmalarınızı keşfedin, adı denemeler yapın ve çalıştırma durumu.

![Azure portal deneme Panosu](media/how-to-create-portal-experiments/dashboard.png)

## <a name="create-an-experiment"></a>Deneme oluşturma

Aşağıdaki formu doldurmak için deneme oluşturma düğmesini seçin.

![Deneme formu oluşturma](media/how-to-create-portal-experiments/create-exp-name-compute.png)

1. Deneme adınızı girin.

1. Profil oluşturma verilerini ve eğitim işine yönelik bir işlem seçin. Açılır menüde, var olan hesaplar listesi kullanılabilir. Yeni bir işlem oluşturmak için adım 3'ndaki yönergeleri izleyin.

1. Açmak için yeni bir işlem düğme Oluştur'u seçin, bölmenin altındaki ve bu deneme için işlem bağlamınızın yapılandırın.

    ![Deneme için yeni işlem oluşturma](media/how-to-create-portal-experiments/create-new-compute.png)

    Alan|Açıklama
    ---|---
    İşlem adı| İşlem bağlamınızın tanımlayan benzersiz bir ad girin.
    Sanal makine boyutu| İşlem için sanal makine boyutu seçin.
    Ek ayarlar| *En düşük düğüm*: İşlem için düğüm sayısı alt sınırı girin. AML işlem düğüm sayısı alt sınırı 0'dır. Profil oluşturma verilerini etkinleştirmek için 1 veya daha fazla düğüm olmalıdır. <br> *En fazla düğüm*: İşlem için en fazla düğüm sayısını girin. 6 düğümleri AML bir işlem için varsayılandır.

      Yeni işlem oluşturulmasını başlatmak için **Oluştur**. Bu işlem birkaç dakika sürebilir.

      >[!NOTE]
      > İşlem adınızı seçin/oluşturduğunuz işlem olduğunu bildirecektir *etkinleştirilmiş profil oluşturma*. (Daha fazla ayrıntı için 7b, profil oluşturma verilerini bakın).

1. Verileriniz için bir depolama hesabı seçin. Genel Önizleme, yalnızca yerel dosya karşıya yükleme ve Azure Blob Depolama hesaplarını destekler.

1. Bir depolama kapsayıcısı seçin.

1. Bir veri dosyası, depolama kapsayıcısından seçin veya yerel bilgisayarınızdan bir dosyasını kapsayıcıya yükleyin.

    ![Deneme için verileri dosyası seçin](media/how-to-create-portal-experiments/select-file.png)

1. Bu deneme için verilerinizi daha fazla yapılandırmak için Önizleme ve profili sekmelerini kullanın.

    1. Önizleme sekmesinde verilerinizi üst bilgileri içerip içermediğini belirtmek ve eğitim kullanma (sütun) özellikleri seçin **dahil edilen** her özellik sütununda düğmeleri geçin.

        ![Veri önizlemesi](media/how-to-create-portal-experiments/data-preview.png)

    1. Profili sekmesinde görüntüleyebileceğiniz [veri profili](#profile) özelliği, hem de dağıtım, türü ve her Özet istatistikleri (ortalama, ORTANCA, en yüksek/dak ve benzeri).

        ![Veri profili sekmesi](media/how-to-create-portal-experiments/data-profile.png)

        >[!NOTE]
        > Aşağıdaki hata iletisini işlem bağlamınızın ise görünür **değil** etkinleştirilmiş profil oluşturma: *Profil oluşturma verilerini, yalnızca çalışmakta olan işlem hedeflerine yönelik kullanılabilir*.

1. Eğitim işi türünü seçin: Sınıflandırma, regresyon veya tahmin.

1. Hedef sütun seçin. Öngörüler yapmak istediğiniz sütun.

1. Tahmin için:
    1. Saat sütununu seçin: Bu sütun için kullanılacak saat verilerini içerir.
    1. Tahmin horizon seçin: Ne kadar zaman birimlerini (dakika/saat/gün/hafta/ay/yıl) modeli için geleceği tahmin etmeyi mümkün olacaktır gösterir. Daha fazla model, daha az doğru olacak geleceğe tahmin etmek için gereklidir. [Daha fazla hakkında tahmini ve tahmin öğrenin horizon](https://docs.microsoft.com/azure/machine-learning/service/how-to-auto-train-forecast#configure-experiment).

1. (İsteğe bağlı) Gelişmiş ayarlar: eğitim işini daha iyi denetlemek için kullanabileceğiniz ek ayarlar.

    Gelişmiş ayarlar|Açıklama
    ------|------
    Birincil Metrik| Model Puanlama için kullanılan ana ölçümünü. [Model ölçümleri hakkında daha fazla bilgi](https://docs.microsoft.com/azure/machine-learning/service/how-to-configure-auto-train#explore-model-metrics).
    Sonlandırma kriteri| Aşağıdaki ölçütleri karşılandığında, eğitim işine tam tamamlanmadan önce sona erer. <br> *Eğitim işi süresi (dakika)* : Eğitim işini çalıştırmak izin vermek için ne kadar.  <br> *En yüksek yineleme sayısı*: Eğitim işi test etmek için işlem hatları (Yineleme) sayısı. İş belirtilen yineleme sayısını birden fazla çalışmaz. <br> *Ölçüm puanı eşiği*:  Tüm işlem hatları için en düşük ölçüm puanı. Bu, erişmek istediğiniz bir tanımlanmış hedef ölçüm varsa, daha fazla zaman eğitim işini daha harcadığınız değil, sağlar.
    Ön işleme| Etkinleştirme veya devre dışı otomatik makine öğrenimi tarafından yapılan ön işleme seçin. Ön işleme otomatik veri temizleme, hazırlama ve dönüştürme yapay özellikleri oluşturulacak içerir. [Ön işleme hakkında daha fazla bilgi](#preprocess).
    Doğrulama| Eğitim işi kullanmak için çapraz doğrulama seçeneklerden birini seçin. [Daha fazla doğrulama bilgi](https://docs.microsoft.com/azure/machine-learning/service/how-to-configure-auto-train#cross-validation-split-options).
    Eşzamanlılık| Çok çekirdekli işlem kullanırken istediğiniz çok çekirdekli sınırları seçin.
    Engellenen algoritması| Algoritmalar ve eğitim işinden çıkarmak istediğiniz seçin.

   ![Gelişmiş ayarları formu](media/how-to-create-portal-experiments/advanced-settings.png)

> [!NOTE]
> Bilgi araç ipucu alanları hakkında daha fazla bilgi için tıklayın.

<a name="profile"></a>

### <a name="data-profiling"></a>Profil oluşturma veri

Veri kümeniz ML kullanıma hazır olup olmadığını doğrulamak için veri kümesi arasında çok sayıda çeşitli Özet istatistikleri elde edebilirsiniz. Sayısal olmayan sütunlar için min, Maks ve hata sayısı gibi yalnızca temel istatistikleri içerirler. Sayısal sütunları için kendi istatistiksel dakika ve tahmini quantiles da gözden geçirebilirsiniz. Özellikle, bizim veri profili içerir:

* **Özellik**: özetlenen sütun adı.

* **Profili**: satır içi görselleştirme tabanlı çıkarımı yapılan tür üzerinde. Örneğin, ondalık (sayısal) histogramlar benzetilir ancak dizeler, Boole ve tarihleri değer sayıları olacaktır. Bu, veri dağıtımını hızlı bir anlayış kazanmak sağlar.

* **Dağıtım türü**: bir sütunda türleri bir satır içi değer sayısı. Null değerlere kendi türü olduğundan, bu görselleştirmeyi tek ya da eksik değerleri algılamak için faydalıdır.

* **Tür**: çıkarsanan sütun türü. Olası değerler şunlardır: dizeler, Boole, tarihleri ve ondalık basamak.

* **Min**: en düşük değer sütunun. Boş girişleri türü yok (örneğin, Boole) devralınan sıralama özellikler için görünür.

* **En fazla**: sütun maksimum değer. "Min gibi" boş girişleri ilgisiz türlerine sahip özellikler için görünür.

* **Sayısı**: sütun eksik ve eksik giriş toplam sayısı.

* **Sayısı eksik**: sütun eksik olmayan giriş sayısı. Bunlar "eksik yok sayma." katkıda bulunan olmayan şekilde boş dizeleri ve hataları değerler olarak kabul edilir olduğunu unutmayın.

* **Quantiles** (aralıklarla 0.1, 1, 5, 25, 50, 75, 95, 99 ve % 99,9): bir fikir veri dağıtımını sağlamak için her quantile yaklaşık değerlerinde. Boş girişleri, ilgisiz türlerine sahip özellikler için görünür.

* **Ortalama**: sütun aritmetik ortalaması. Boş girişleri, ilgisiz türlerine sahip özellikler için görünür.

* **Standart sapma**: sütunun standart sapması. Boş girişleri, ilgisiz türlerine sahip özellikler için görünür.

* **Varyans**: sütun sapması. Boş girişleri, ilgisiz türlerine sahip özellikler için görünür.

* **Komutunu**: komutunu sütun. Boş girişleri, ilgisiz türlerine sahip özellikler için görünür.

* **Basıklık**: basıklığını sütun. Boş girişleri, ilgisiz türlerine sahip özellikler için görünür.

<a name="preprocess"></a>

### <a name="advanced-preprocessing"></a>Gelişmiş ön işleme

Denemelerinizi yapılandırırken, Gelişmiş ayarını etkinleştirebilirsiniz `Preprocess`. Bunu yapmak, aşağıdaki veri ön işleme ve özellik kazandırma sayesinde adımları otomatik olarak gerçekleştirilir böylece anlamına gelir.

|Ön işleme&nbsp;adımları| Açıklama |
| ------------- | ------------- |
|Yüksek bir kardinalite veya herhangi bir fark özellik bırakın|Bu özellikler eksik, tüm satırlar boyunca veya son derece yüksek kardinalite (örneğin, karma, kimlikleri veya GUID'leri) ile aynı değeri tüm değerleri de dahil olmak üzere, eğitim ve doğrulama kümelerinden bırakın.|
|Eksik değerleri impute|Sütundaki değerlerin ortalamasını ile sayısal özellikler için impute.<br/><br/>Kategorik özellikler için en sık kullanılan değerle impute.|
|Ek özellikler oluşturma|DateTime özellikleri: Yıl, ay, gün, haftanın günü, yıl, Çeyrek, yıl, saat, dakika, saniye haftanın günü.<br/><br/>Metin özellikleri: Terim sıklığı unigrams, BI-gram ve üç karakter gram göre.|
|Dönüştürme ve kodlama |Az sayıda benzersiz değere sayısal özelliklerle kategorik özelliklerini dönüştürülür.<br/><br/>Sık erişimli bir kodlama için düşük kardinalite kategorik gerçekleştirilir; yüksek kardinalite için biri-sık erişimli-hash kodlama.|
|Word katıştırma|Metin özelliği vektörleri metin belirteçlerin önceden eğitilmiş bir modeli kullanarak cümle vektörleri dönüştürür Oluşturucu. Bir belgedeki her sözcüğün katıştırma vektör birlikte bir belge özelliği vektör üretmek için toplanır.|
|Hedef kodlamaları|Kategorik özellikler için her kategori regresyon sorunları ve sınıf olasılığa sınıflandırma sorunlar için her sınıf için ortalama hedef değer ile eşler. Çapraz doğrulama sıklığa dayalı ağırlığı ve k Katlama sığdırma seyrek veri kategorilere göre neden gürültü ve eşleme üzerinden azaltmak için uygulanır.|
|Hedef metin kodlaması|Metin girişi için sözcükleri paketi ile üst üste doğrusal bir model, her sınıf olasılığını oluşturmak için kullanılır.|
|Kanıt (WoE) ağırlığı|WoE bağıntı hedef sütunu için kategorik sütunlar olarak hesaplar. Sınıfının vs sınıf olasılıklar oranını günlük olarak hesaplanır. Bu adım, bir sayısal özellik sütunu sınıfı başına çıkarır ve açıkça eksik değerleri ve aykırı işleme impute ihtiyacını ortadan kaldırır.|
|Küme uzaklığı|Tüm sayısal sütunlarda k-ortalamaları kümeleme modeli eğitir.  Çıkışlar k yeni özellikler, her örnek için her kümenin kütle merkezi uzaklığı içeren küme başına yeni bir sayısal özellik.|

## <a name="run-experiment-and-view-results"></a>Denemeyi çalıştırma ve sonuçları görüntüleme

Denemeyi çalıştırmak için Başlat'a tıklayın. Denemeyi Hazırlama işlemi birkaç dakika sürer.

### <a name="view-experiment-details"></a>Deneme ayrıntılarını görüntüle

Deneme Hazırlık aşaması tamamlandıktan sonra Çalıştır ayrıntı ekranı görürsünüz. Bu size, tam oluşturulan modellerin listesini sağlar. Varsayılan olarak, en yüksek puanlar modeli, parametrelere dayalı listesinin üstünde. Daha fazla model eğitim işini çalışır gibi grafik ve yineleme listesi eklenir. Şu ana kadar üretilen modelleri için hızlı ölçümler karşılaştırması almak için yineleme grafik kullanın.

Eğitim işleri her işlem hattı çalıştırma tamamlanması biraz sürebilir.

![Çalıştırma ayrıntıları Panosu](media/how-to-create-portal-experiments/run-details.png)

### <a name="view-training-run-details"></a>Çalıştırma Ayrıntıları görünümünü eğitim

Herhangi bir eğitim gibi performans ölçümlerini ve dağılım grafikleri çalıştırma ayrıntılarını görmek için çıkış modelleri detayına gidin. [Grafikler hakkında daha fazla bilgi](https://docs.microsoft.com/azure/machine-learning/service/how-to-track-experiments#understanding-automated-ml-charts).

![Yineleme ayrıntılarını](media/how-to-create-portal-experiments/iteration-details.png)

## <a name="deploy-model"></a>Model dağıtma

Eldeki en iyi modeli oluşturduktan sonra yeni veri tahmin etmek için bir web hizmeti olarak dağıtma zamanı geldi.

Otomatik ML model dağıtımı ile kod yazmadan yardımcı olur:

1. Dağıtım için birkaç seçeneğiniz vardır. 
    1. Deneme için seçin ölçüm ölçütleri temel alarak en iyi modeli dağıtmak istiyorsanız ayarladığınız **en iyi modeli dağıtma** gelen **çalıştırma ayrıntıları** sayfası.

        ![Dağıt düğmesi modeli](media/how-to-create-portal-experiments/deploy-model-button.png)

    1. Bir modele yineleme dağıtmak istiyorsanız, kendi özel çalışma ayrıntı sayfası açın ve seçmek için model üzerinde detaya **Model dağıtma**.

        ![Dağıt düğmesi modeli](media/how-to-create-portal-experiments/deploy-model-button2.png)

1. Modeli kaydedin hizmete ilk adımdır. "Modeli Kaydet"'i seçin ve kayıt işleminin tamamlanması için bekleyin.

    ![Model dikey dağıtma](media/how-to-create-portal-experiments/deploy-model-blade.png)

1. Model kaydedildiğinde, Puanlama betik (scoring.py) ve dağıtım sırasında kullanılacak ortamı betik (condaEnv.yml) indirmek mümkün olacaktır.

1. Puanlama betiği ve ortam betik indirildiğinde, Git **varlıklar** seçin ve sol Gezinti Bölmesi Dikey penceresinde **modelleri**.

    ![Gezinti bölmesinde modelleri](media/how-to-create-portal-experiments/nav-pane-models.png)

1. Kaydettiğiniz model seçin ve "Görüntü Oluştur" seçeneğini belirleyin.

    Çalıştırma kimliği, yineleme sayısı aşağıdaki biçimde içerecektir açıklamasını tarafından modeli tanımlayabilirsiniz: *< Run_ID > _ < Iteration_number > _Model*

    ![Modelleri: Görüntü oluşturma](media/how-to-create-portal-experiments/model-create-image.png)

1. Görüntü için bir ad girin. 
1. Seçin **Gözat** daha önce indirdiğiniz Puanlama dosyası (scoring.py) yüklemek için "Puanlama dosyası" kutusunun yanındaki düğmeyi.

1. Seçin **Gözat** daha önce indirdiğiniz ortam dosyası (condaEnv.yml) yüklemek için "Conda dosya" kutusunun yanındaki düğmeyi.

    Kendi Puanlama betiği ve conda dosyası kullanmak, yapabilir ek dosyaları karşıya yükleme. [Puanlama betiği hakkında daha fazla bilgi](https://docs.microsoft.com/azure/machine-learning/service/how-to-deploy-and-where#script).

      >[!Important]
      > Dosya adları gerekir altında 32 karakter olmalı ve başlamalı ve alfasayısal karakterler ile bitmelidir. Tire, alt çizgi, nokta ve arasında alfasayısal karakterler içerebilir. Alanları izin verilmez.

    ![Görüntü oluşturma](media/how-to-create-portal-experiments/create-image.png)

1. Görüntü oluşturma işlemini başlatmak için "Oluştur" düğmesini seçin. Bu kez tamamlanması birkaç dakika sürer, üst çubukta bir ileti görürsünüz.
1. "Görüntüler" sekmesine gidin, dağıtmak istediğiniz görüntüyü yanındaki onay kutusunu işaretleyin ve "dağıtım oluştur" seçeneğini belirleyin. [Dağıtımları hakkında daha fazla bilgi](https://docs.microsoft.com/azure/machine-learning/service/how-to-deploy-and-where).

    Dağıtım için 2 seçenek vardır.
     + Azure Container Instance (ACI) - bu amacı yerine uygun ölçekte operasyonel dağıtımı test etmek için daha kullanılır. En az bir çekirdek için değerleri girin emin _CPU rezervasyon kapasitesi_ve en az bir gigabayt (GB) için _bellek ayırma kapasitesi_
     + Azure Kubernetes Service'i (AKS)) - Bu seçenek, uygun ölçekte dağıtım için kullanılabilir. Hazır bir AKS göre işlem gerekir.

     ![Görüntüler: Dağıtım oluşturma](media/how-to-create-portal-experiments/images-create-deployment.png)

1. İşiniz bittiğinde **Oluştur**’u seçin. Modeli dağıtma, her işlem hattı çalıştırma tamamlanması birkaç dakika sürebilir.

1. İşte bu kadar! Tahminler üretmek için bir işletimsel web Hizmetim var.

## <a name="next-steps"></a>Sonraki adımlar

* [Otomatik machine learning hakkında daha fazla bilgi edinin](concept-automated-ml.md) ve Azure Machine Learning.
* [Bir web hizmetinin nasıl kullanılacağı hakkında bilgi edinin](https://docs.microsoft.com/azure/machine-learning/service/how-to-consume-web-service).
