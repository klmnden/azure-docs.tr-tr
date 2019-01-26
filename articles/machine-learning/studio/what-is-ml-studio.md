---
title: Nedir
titleSuffix: Azure Machine Learning Studio
description: Azure Machine Learning Studio, modelleri kullanıma hazır algoritmalar ve modüller kitaplığından hızla oluşturmaya yönelik bir Sürükle ve bırak aracıdır.
keywords: azure machine learning,azure ml, ml studio
services: machine-learning
documentationcenter: ''
author: garyericson
ms.custom: seodec18
ms.author: garye
ms.assetid: e65c8fe1-7991-4a2a-86ef-fd80a7a06269
ms.service: machine-learning
ms.component: studio
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 03/28/2018
ms.openlocfilehash: 606234096314eb73cb32f8fbcc2d5e6e79c25573
ms.sourcegitcommit: 58dc0d48ab4403eb64201ff231af3ddfa8412331
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/26/2019
ms.locfileid: "55077106"
---
# <a name="what-is-azure-machine-learning-studio"></a>Azure Machine Learning Studio nedir?
Microsoft Azure Machine Learning Studio, verilerinizde tahmine dayalı analiz çözümleri oluşturma, test etme ve dağıtma amacıyla kullanabileceğiniz bir işbirliğine dayalı sürükle ve bırak aracıdır. Machine Learning Studio, modelleri özel uygulamalar veya Excel gibi BI araçları tarafından kolayca kullanılabilen web hizmetleri olarak yayımlar.

Machine Learning Studio, verilerinizin veri bilimi, tahmine dayalı analiz ve bulut kaynakları ile buluştuğu yerdir.

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

## <a name="the-machine-learning-studio-interactive-workspace"></a>Machine Learning Studio etkileşimli çalışma alanı
Tahmine dayalı bir analiz modeli geliştirmek için, genellikle bir veya daha çok kaynaktan veri kullanır, çeşitli veri işleme ve istatistik işlevleri aracılığıyla bu verileri dönüştürüp analiz eder ve bir sonuç kümesi oluşturursunuz. Bir modelin bu şekilde geliştirilmesi yinelemeli bir işlemdir. Çeşitli işlevleri ve bunların parametrelerini değiştirirken, eğitilmiş ve verimli bir model elde ettiğinizi düşüneceğiniz ana kadar sonuçlarınız yakınsanır.

**Azure Machine Learning Studio**, tahmine dayalı bir analiz modelini kolayca oluşturma, test etme ve yineleme amacıyla etkileşimli ve görsel bir çalışma alanı sunar. ***Veri kümelerini*** ve analiz ***modüllerini*** etkileşimli bir tuvale sürükleyip bırakır ve bunları birbirine bağlayarak Machine Learning Studio'da çalıştıracağınız bir ***deneme*** oluşturursunuz. Model tasarımınızı yinelemek için, denemeyi düzenleyin, isterseniz bir kopyasını kaydedin ve yeniden çalıştırın. Hazır olduğunuzda, ***eğitim denemenizi*** bir ***tahmine dayalı denemeye*** dönüştürebilir ve ardından modelinize başkaları tarafından erişilebilmesi için bunu bir ***web hizmeti*** olarak yayımlayabilirsiniz.

Programlama gerekmez; tahmine dayalı analiz modelinizi oluşturmak için veri kümelerini ve modülleri görsel olarak bağlamanız yeterlidir.

> [!TIP]
> Machine Learning Studio'nun işlevlerine genel bir bakış sağlayan bir diyagram indirmek ve yazdırmak için bkz. [Azure Machine Learning Studio'nun işlevlerine genel bakış diyagramı](studio-overview-diagram.md).
>
>

![Azure Machine Learning studio diyagramı: Denemeleri oluşturmak, birçok kaynak için veri okuma, puanlanmış veri yazma, model yazma.][ml-studio-overview]

## <a name="get-started-with-machine-learning-studio"></a>Machine Learning Studio ile çalışmaya başlama
[Machine Learning Studio](https://studio.azureml.net)'ya ilk girişinizde **Giriş** sayfasını görürsünüz. Buradan belgeleri, videoları, web seminerlerini görüntüleyebilir ve diğer değerli kaynakları bulabilirsiniz.

Sol üst menüye tıkladığınızda ![Menü](./media/what-is-ml-studio/menu.png) birkaç seçenek göreceksiniz.

### <a name="cortana-intelligence"></a>Cortana Intelligence
**Cortana Intelligence**'a tıkladığınızda [Cortana Intelligence Suite](https://www.microsoft.com/cloud-platform/cortana-intelligence-suite)'in ana sayfası açılır. Cortana Intelligence Suite, verilerinizi akıllı eylemlere dönüştüren büyük veri ve gelişmiş analiz özellikleri sunan, tam yönetilen bir çözümdür. Müşteri hikayeleri dahil olmak üzere tüm belgeleri görüntülemek için Suite ana sayfasını ziyaret edin.

### <a name="azure-machine-learning-studio"></a>Azure Machine Learning Studio
Burada iki seçenek vardır: Başladığınız sayfa olan **Ana Sayfa** ve **Studio**.

**Studio**'ya tıkladığınızda **Azure Machine Learning Studio** sayfası açılır. Öncelikle Microsoft hesabınızı veya iş ya da okul hesabınızı kullanarak oturum açmanız istenir. Oturum açtıktan sonra, solda şu sekmeleri görürsünüz:

* **PROJELER** - Tek bir projeyi temsil eden denemeler, veri kümeleri, not defterleri ve diğer kaynakların koleksiyonu
* **DENEMELER** - Oluşturup çalıştırdığınız veya taslak olarak kaydettiğiniz denemeler
* **WEB HİZMETLERİ** - Denemelerinizden dağıttığınız web hizmetleri
* **NOT DEFTERLERİ** - Oluşturduğunuz Jupyter not defterleri
* **VERİ KÜMELERİ** - Studio'ya yüklediğiniz veri kümeleri
* **EĞİTİLMİŞ MODELLER** - Denemelerde eğittiğiniz ve Studio'da kaydettiğiniz modeller
* **AYARLAR** - Hesabınızı ve kaynaklarınızı yapılandırmak için kullanabileceğiniz ayarlar koleksiyonu.

### <a name="gallery"></a>Galeri
**Galeri** sekmesine tıkladığınızda **[Azure AI Gallery](http://gallery.cortanaintelligence.com/)** açılır. Galeri, bir veri bilimcileri ve geliştiricileri topluluğunun Cortana Intelligence Suite bileşenleri kullanılarak oluşturduğu çözümleri paylaştığı yerdir.

Galeri hakkında daha fazla bilgi için bkz. [Azure AI Gallery'de çözüm paylaşma ve keşfetme](gallery-how-to-use-contribute-publish.md).

## <a name="components-of-an-experiment"></a>Deneme bileşenleri
Bir deneme, tahmine dayalı bir modeli oluşturmak için birlikte bağladığınız analitik modüllere veri sağlayan veri kümelerinden oluşur. Geçerli bir deneme özellikle şu özelliklere sahiptir:

* Denemenin en az bir veri kümesi ve bir modülü vardır
* Veri kümeleri yalnızca modüllere bağlanabilir
* Modüller veri kümelerine veya diğer modüllere bağlanabilir
* Modüllerin tüm giriş bağlantı noktalarının veri akışına bir tür bağlantısı olması gerekir
* Her bir modül için gereken tüm parametreler ayarlanmalıdır

Bir denemeyi sıfırdan oluşturabilir veya var olan bir örnek denemeyi şablon olarak kullanabilirsiniz. Daha fazla bilgi için bkz. [Örnek denemeleri kopyalayarak yeni makine öğrenimi denemeleri oluşturma](sample-experiments.md).

Basit bir deneme oluşturma örneği için bkz. [Azure Machine Learning Studio'da basit bir deneme oluşturma](create-experiment.md).

Tahmine dayalı bir analiz çözümünün daha kapsamlı bir kılavuzu için bkz. [Azure Machine Learning ile tahmine dayalı bir analiz çözümü geliştirme](walkthrough-develop-predictive-solution.md).

### <a name="datasets"></a>Veri kümeleri
Bir veri kümesi, model oluşturma işleminde kullanılabilmesi için Machine Learning Studio'ya yüklenen verilerdir. Machine Learning Studio'da deneme yapabileceğiniz birçok örnek veri kümesi bulunur ve ihtiyaç duyarsanız daha çok veri kümesi yükleyebilirsiniz. Dahil olan veri kümelerine aşağıda birkaç örnek verilmiştir:

* **Çeşitli otomobiller için MPG verileri** - Otomobiller için silindir, beygir gücü, vb. sayısına göre tanımlanan galon başına mil (MPG) değerleri.
* **Meme kanseri verileri** - Meme kanseri tanılama verileri.
* **Orman yangını verileri** - Kuzey doğu Portekiz'de orman yangını boyutları.

Bir deneme oluştururken, tuval solundaki kullanılabilir veri kümesi listesinden seçebilirsiniz.

Machine Learning Studio'ya dahil olan örnek veri kümelerinin listesi için bkz. [Azure Machine Learning Studio'daki örnek veri kümelerini kullanma](use-sample-datasets.md).

### <a name="modules"></a>Modüller
Bir modül, verilerinizde gerçekleştirebileceğiniz bir algoritmadır. Machine Learning Studio, veri alım işlevlerinden eğitim, puanlama ve doğrulama işlemlerine kadar değişiklik gösteren birçok modüle sahiptir. Dahil olan modüllere aşağıda birkaç örnek verilmiştir:

* [ARFF'ye Dönüştürme][convert-to-arff] - Seri hale getirilmiş .NET veri kümesini Öznitelik-İlişki Dosyası Biçimi'ne (ARFF) dönüştürür.
* [Basit İstatistikleri Hesaplama][elementary-statistics] - Ortalama, standart sapma vb. basit istatistikleri hesaplar.
* [Doğrusal Regresyon][linear-regression] - Çevrimiçi bir gradyan düşüşü tabanlı doğrusal regresyon modeli oluşturur.
* [Model Puanlama][score-model] - Eğitilmiş bir sınıflandırma veya regresyon modelini puanlar.

Bir deneme oluştururken, tuvalin solundaki kullanılabilir modül listesinden seçebilirsiniz.

Bir modül, modülün iç algoritmalarını yapılandırmak için kullanabileceğiniz parametreler kümesine sahip olabilir. Tuvalde bir modül seçtiğinizde, modülün parametreleri tuvalin sağındaki **Özellikler** bölmesinde görüntülenir  Modelinizi ayarlamak için, bu bölmedeki parametreleri değiştirebilirsiniz.

Kullanılabilen büyük makine öğrenimi algoritma kitaplığında gezinme konusunda biraz yardım için bkz. [Microsoft Azure Machine Learning'de algoritma seçme](algorithm-choice.md)

## <a name="deploying-a-predictive-analytics-web-service"></a>Tahmine dayalı analiz web hizmetini dağıtma
Tahmine dayalı analiz modeliniz hazır olduktan sonra, bunu doğrudan Machine Learning Studio'dan bir web hizmeti olarak dağıtabilirsiniz. Bu işlem hakkında daha ayrıntılı bilgi için bkz. [Bir Azure Machine Learning web hizmetini dağıtma](publish-a-machine-learning-web-service.md).

[ml-studio-overview]:./media/what-is-ml-studio/azure-ml-studio-diagram.jpg



## <a name="key-machine-learning-terms-and-concepts"></a>Önemli makine öğrenimi terimleri ve kavramları
Makine öğrenimi terimleri kafanızı karıştırabilir. Size yardımcı olmak açısından önemli terimlerin tanımları aşağıda verilmiştir. Tanımlanmasını istediğiniz başka terim varsa aşağıdaki yorumları kullanarak bize bildirebilirsiniz.

### <a name="data-exploration-descriptive-analytics-and-predictive-analytics"></a>Veri keşfi, açıklayıcı analiz ve tahmine dayalı analiz

**Veri keşfi**, odaklı analiz için özellikler bulmak amacıyla büyük ve genellikle yapılandırılmamış veri kümeleri hakkında bilgi toplama işlemidir.

**Veri madenciliği**, otomatik veri keşfine başvurur.

**Açıklayıcı analiz**, ne olduğunu özetlemek için bir veri kümesini analiz etme işlemidir. Satış raporları, web ölçümleri ve sosyal ağ analizleri gibi iş analizlerinin büyük bir çoğunluğu açıklayıcıdır.

**Tahmine dayalı analiz**, gelecekteki sonuçları öngörmek için geçmiş veya geçerli verilerden model oluşturma işlemidir.

### <a name="supervised-and-unsupervised-learning"></a>Denetimli ve denetimsiz öğrenme
 **Denetimli öğrenme** algoritmaları etiketli verilerle, başka bir deyişle istenen yanıtların örneklerinden oluşan verilerle eğitilir. Örneğin, sahte kredi kartı kullanımını tanımlayan bir model, bilinen sahte ve geçerli ödemelerin veri noktalarıyla etiketlenmiş bir veri kümesinden eğitilir. Machine learning denetimlidir.

 **Denetimsiz öğrenme** hiçbir etiketi olmayan verilerde kullanılır ve amacı verilerdeki ilişkileri bulmaktır. Örneğin, benzer satın alma alışkanlıklarına sahip müşteri demografisi gruplandırmalarını bulmak isteyebilirsiniz.

### <a name="model-training-and-evaluation"></a>Model eğitimi ve değerlendirmesi
Makine öğrenimi modeli, yanıtlamaya çalıştığınız sorunun veya tahmin etmek istediğiniz sonucun bir soyutlamasıdır. Modeller, var olan verilerle eğitilir ve değerlendirilir.

#### <a name="training-data"></a>Eğitim verileri
Bir modeli verilerle eğittiğinizde, bilinen bir veri kümesini kullanırsınız ve en doğru yanıta ulaşmak için gerekli veri özelliklerine bağlı olarak modelde değişiklikler yaparsınız. Azure Machine Learning'de bir model, eğitim verilerini ve puanlama modülü gibi işlev modüllerini işleyen bir algoritma modülünden oluşturulur.

Denetimli öğrenmede bir sahtekarlık algılama modelini eğitiyorsanız sahte veya geçerli olarak etiketlenmiş bir işlem kümesini kullanırsınız. Verilerinizi rastgele ayırıp bir kısmını modeli eğitmek, bir kısmını da modeli test etmek veya değerlendirmek için kullanırsınız.

#### <a name="evaluation-data"></a>Değerlendirme verileri
Bir modeli eğittikten sonra, kalan test verilerini kullanarak modeli değerlendirin. Modelinizin doğru tahmin yapıp yapmadığını anlamanız için sonuçlarını zaten bildiğiniz verileri kullanın.

## <a name="other-common-machine-learning-terms"></a>Diğer ortak makine öğrenimi terimleri
* **algoritma**: Kendi içinde veri işleme, matematik veya otomatik mantık aracılığıyla sorunları çözmek için kullanılan kuralları kümesi.
* **anomali algılama**: Olağan dışı olayları veya değerleri işaretleyen ve yardımcı olan bir model sorunları keşfedin. Örneğin, kredi kartı sahtekarlığı algılama modeli sıra dışı satın alama işlemlerini algılar.
* **Kategorik veriler**: Kategorilere göre düzenlenmiş ve gruplara bölünebilen veriler. Örneğin, otomobiller için kategorik bir veri kümesi yıl, marka, model ve fiyatı belirtebilir.
* **Sınıflandırma**: Veri noktalarını, kategori gruplandırmaları zaten bilinen bir veri kümesi temel alınarak kategoriler halinde düzenlemek için bir model.
* **özellik Mühendisliği**: Bir veri kümesine veri kümesini iyileştirmek ve sonuçları geliştirmek amacıyla ilgili özellikleri ayıklama veya seçme işlemi. Örneğin, uçak bileti ücreti verileri, haftanın günleri ve tatil günlerine göre iyileştirilebilir. Bkz. [Azure Machine Learning'de özellik seçimi ve özellik mühendisliği](../team-data-science-process/create-features.md).
* **Modül**: Bir Machine Learning Studio modelinde, küçük veri kümelerini girmeyi ve düzenlemeyi sağlayan veri girme modülü gibi işlevsel bir parçası. Bir algoritma da Machine Learning Studio'da bir modül türüdür.
* **Model**: Denetimli öğrenme modeli bir makine öğrenimi eğitim verileri, bir algoritma modülü ve Score Model modülü gibi işlevsel modüllerden denemesinin ürünüdür.
* **sayısal veriler**: Ölçümler (sürekli veriler) olarak anlamı veya sayımlar (ayrık veriler) veriler. *Nicel veri* olarak da adlandırılır.
* **bölüm**: Verileri örneklere bölme yöntemi. Daha fazla bilgi için bkz. [Bölüm ve Örnek](https://msdn.microsoft.com/library/azure/dn905960.aspx) 
* **Tahmin**: Tahmin değeri ya da makine öğrenme modeli değerleri bir tahmin ' dir. "Tahmin edilen puan" terimini de görebilirsiniz. Ancak, tahmin edilen puanlar bir modelin son çıktısı değildir. Puanın ardından modelin değerlendirmesi gelir.
* **Regresyon**: Bir değeri tahmin etmeye yönelik bir model, bir yıl temelinde bir araba fiyatını gibi bağımsız değişkenlere bağlı ve olun.
* **puan**: Bir eğitilmiş bir sınıflandırma veya regresyon modelinden oluşturulan tahmin edilen bir değer kullanarak [Model Puanlama Modülü](https://msdn.microsoft.com/library/azure/dn905995.aspx) Machine Learning Studio'da. Sınıflandırma modelleri, tahmin edilen değerin olasılığı için de bir puan döndürür. Bir modelden puan oluşturduktan sonra, [Evaluate Model (Model Değerlendirme) modülünü](https://msdn.microsoft.com/library/azure/dn905915.aspx) kullanarak modelin doğruluğunu değerlendirebilirsiniz.
* **örnek**: Tüm temsilcisi olmaya yönelik bir veri kümesinin bir parçası. Örnekler, rastgele veya veri kümesinin belirli özellikleri temel alınarak seçilebilir.

## <a name="next-steps"></a>Sonraki adımlar
Tahmine dayalı analizin ve makine öğreniminin temellerini, [adım adım öğretici](create-experiment.md) kullanarak ve [örnekler üzerinden giderek](sample-experiments.md) öğrenebilirsiniz.


<!-- Module References -->
[convert-to-arff]: https://msdn.microsoft.com/library/azure/62d2cece-d832-4a7a-a0bd-f01f03af0960/
[elementary-statistics]: https://msdn.microsoft.com/library/azure/3086b8d4-c895-45ba-8aa9-34f0c944d4d3/
[linear-regression]: https://msdn.microsoft.com/library/azure/31960a6f-789b-4cf7-88d6-2e1152c0bd1a/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
