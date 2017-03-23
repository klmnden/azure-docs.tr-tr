---
title: Azure Machine Learning Studio nedir? | Microsoft Belgeleri
description: "Kullanıma hazır bir algoritmalar ve modüller kitaplığından hızla model oluşturmaya yönelik bir sürükle ve bırak aracı olan Azure ML Studio&quot;ya genel bakış."
keywords: azure machine learning,azure ml, ml studio
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: e65c8fe1-7991-4a2a-86ef-fd80a7a06269
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/20/2017
ms.author: garye
translationtype: Human Translation
ms.sourcegitcommit: 9e738c4e5f43ae6c939f7c6da90c258498943e73
ms.openlocfilehash: b8115f1fb72b0ba89fd0c8afa3358878a0fab92b
ms.lasthandoff: 12/14/2016


---
# <a name="what-is-azure-machine-learning-studio"></a>Azure Machine Learning Studio nedir?
Microsoft Azure Machine Learning Studio, verilerinizde tahmine dayalı analiz çözümleri oluşturma, test etme ve dağıtma amacıyla kullanabileceğiniz bir işbirliğine dayalı sürükle ve bırak aracıdır. Machine Learning Studio, modelleri özel uygulamalar veya Excel gibi BI araçları tarafından kolayca kullanılabilen web hizmetleri olarak yayımlar.

Machine Learning Studio, verilerinizin veri bilimi, tahmine dayalı analiz ve bulut kaynakları ile buluştuğu yerdir.

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="the-machine-learning-studio-interactive-workspace"></a>Machine Learning Studio etkileşimli çalışma alanı
Tahmine dayalı bir analiz modeli geliştirmek için, genellikle bir veya daha çok kaynaktan veri kullanır, çeşitli veri işleme ve istatistik işlevleri aracılığıyla bu verileri dönüştürüp analiz eder ve bir sonuç kümesi oluşturursunuz. Bir modelin bu şekilde geliştirilmesi yinelemeli bir işlemdir. Çeşitli işlevleri ve bunların parametrelerini değiştirirken, eğitilmiş ve verimli bir model elde ettiğinizi düşüneceğiniz ana kadar sonuçlarınız yakınsanır.

**Azure Machine Learning Studio**, tahmine dayalı bir analiz modelini kolayca oluşturma, test etme ve yineleme amacıyla etkileşimli ve görsel bir çalışma alanı sunar. ***Veri kümelerini*** ve analiz ***modüllerini*** etkileşimli bir tuvale sürükleyip bırakır ve bunları birbirine bağlayarak Machine Learning Studio'da çalıştıracağınız bir ***deneme*** oluşturursunuz. Model tasarımınızı yinelemek için, denemeyi düzenleyin, isterseniz bir kopyasını kaydedin ve yeniden çalıştırın. Hazır olduğunuzda, ***eğitim denemenizi*** bir ***tahmine dayalı denemeye*** dönüştürebilir ve ardından modelinize başkaları tarafından erişilebilmesi için bunu bir ***web hizmeti*** olarak yayımlayabilirsiniz.

Programlama gerekmez; tahmine dayalı analiz modelinizi oluşturmak için veri kümelerini ve modülleri görsel olarak bağlamanız yeterlidir.

> [!TIP]
> Machine Learning Studio'nun işlevlerine genel bir bakış sağlayan bir diyagram indirmek ve yazdırmak için bkz. [Azure Machine Learning Studio'nun işlevlerine genel bakış diyagramı](machine-learning-studio-overview-diagram.md).
> 
> 

![Azure ML Studio diyagramı: Deneme oluşturma, birçok kaynak için veri okuma, puanlanmış veri yazma, model yazma.][ml-studio-overview]

## <a name="get-started-with-machine-learning-studio"></a>Machine Learning Studio ile çalışmaya başlama
[Machine Learning Studio](https://studio.azureml.net)'ya ilk girişinizde **Giriş** sayfasını görürsünüz. Buradan belgeleri, videoları, web seminerlerini görüntüleyebilir ve diğer değerli kaynakları bulabilirsiniz.

Sol üst menüye tıkladığınızda ![Menü](media/machine-learning-what-is-ml-studio/menu.png) birkaç seçenek göreceksiniz.

### <a name="cortana-intelligence"></a>Cortana Intelligence
**Cortana Intelligence**'a tıkladığınızda [Cortana Intelligence Suite](https://www.microsoft.com/cloud-platform/cortana-intelligence-suite)'in ana sayfası açılır. Cortana Intelligence Suite, verilerinizi akıllı eylemlere dönüştüren büyük veri ve gelişmiş analiz özellikleri sunan, tam yönetilen bir çözümdür. Müşteri hikayeleri dahil olmak üzere tüm belgeleri görüntülemek için Suite ana sayfasını ziyaret edin.

### <a name="azure-machine-learning"></a>Azure Machine Learning
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
**Galeri** sekmesine tıkladığınızda **[Cortana Intelligence Galerisi](http://gallery.cortanaintelligence.com/)** açılır. Galeri, bir veri bilimcileri ve geliştiricileri topluluğunun Cortana Intelligence Suite bileşenleri kullanılarak oluşturduğu çözümleri paylaştığı yerdir.

Galeri hakkında daha fazla bilgi için bkz. [Cortana Intelligence Galerisi'nde çözüm paylaşma ve keşfetme](machine-learning-gallery-how-to-use-contribute-publish.md).

## <a name="components-of-an-experiment"></a>Deneme bileşenleri
Bir deneme, tahmine dayalı bir modeli oluşturmak için birlikte bağladığınız analitik modüllere veri sağlayan veri kümelerinden oluşur. Geçerli bir deneme özellikle şu özelliklere sahiptir:

* Denemenin en az bir veri kümesi ve bir modülü vardır
* Veri kümeleri yalnızca modüllere bağlanabilir
* Modüller veri kümelerine veya diğer modüllere bağlanabilir
* Modüllerin tüm giriş bağlantı noktalarının veri akışına bir tür bağlantısı olması gerekir
* Her bir modül için gereken tüm parametreler ayarlanmalıdır

Bir denemeyi sıfırdan oluşturabilir veya var olan bir örnek denemeyi şablon olarak kullanabilirsiniz. Daha fazla bilgi için bkz. [Yeni denemeler oluşturmak için örnek denemeleri kullanma](machine-learning-sample-experiments.md).

Basit bir deneme oluşturma örneği için bkz. [Azure Machine Learning Studio'da basit bir deneme oluşturma](machine-learning-create-experiment.md).

Tahmine dayalı bir analiz çözümünün daha kapsamlı bir kılavuzu için bkz. [Azure Machine Learning ile tahmine dayalı bir analiz çözümü geliştirme](machine-learning-walkthrough-develop-predictive-solution.md).

### <a name="datasets"></a>Veri kümeleri
Bir veri kümesi, model oluşturma işleminde kullanılabilmesi için Machine Learning Studio'ya yüklenen verilerdir. Machine Learning Studio'da deneme yapabileceğiniz birçok örnek veri kümesi bulunur ve ihtiyaç duyarsanız daha çok veri kümesi yükleyebilirsiniz. Dahil olan veri kümelerine aşağıda birkaç örnek verilmiştir:

* **Çeşitli otomobiller için MPG verileri** - Otomobiller için silindir, beygir gücü, vb. sayısına göre tanımlanan galon başına mil (MPG) değerleri.
* **Meme kanseri verileri** - Meme kanseri tanılama verileri.
* **Orman yangını verileri** - Kuzey doğu Portekiz'de orman yangını boyutları.

Bir deneme oluştururken, tuval solundaki kullanılabilir veri kümesi listesinden seçebilirsiniz.

Machine Learning Studio'ya dahil olan örnek veri kümelerinin listesi için bkz. [Azure Machine Learning Studio'daki örnek veri kümelerini kullanma](machine-learning-use-sample-datasets.md).

### <a name="modules"></a>Modüller
Bir modül, verilerinizde gerçekleştirebileceğiniz bir algoritmadır. Machine Learning Studio, veri alım işlevlerinden eğitim, puanlama ve doğrulama işlemlerine kadar değişiklik gösteren birçok modüle sahiptir. Dahil olan modüllere aşağıda birkaç örnek verilmiştir:

* [ARFF'ye Dönüştürme][convert-to-arff] - Seri hale getirilmiş .NET veri kümesini Öznitelik-İlişki Dosyası Biçimi'ne (ARFF) dönüştürür.
* [Basit İstatistikleri Hesaplama][elementary-statistics] - Ortalama, standart sapma vb. basit istatistikleri hesaplar.
* [Doğrusal Regresyon][linear-regression] - Çevrimiçi bir gradyan düşüşü tabanlı doğrusal regresyon modeli oluşturur.
* [Model Puanlama][score-model] - Eğitilmiş bir sınıflandırma veya regresyon modelini puanlar.

Bir deneme oluştururken, tuvalin solundaki kullanılabilir modül listesinden seçebilirsiniz.  

Bir modül, modülün iç algoritmalarını yapılandırmak için kullanabileceğiniz parametreler kümesine sahip olabilir. Tuvalde bir modül seçtiğinizde, modülün parametreleri tuvalin sağındaki **Özellikler** bölmesinde görüntülenir  Modelinizi ayarlamak için, bu bölmedeki parametreleri değiştirebilirsiniz.

Kullanılabilen büyük makine öğrenimi algoritma kitaplığında gezinme konusunda biraz yardım için bkz. [Microsoft Azure Machine Learning'de algoritma seçme](machine-learning-algorithm-choice.md)

## <a name="deploying-a-predictive-analytics-web-service"></a>Tahmine dayalı analiz web hizmetini dağıtma
Tahmine dayalı analiz modeliniz hazır olduktan sonra, bunu doğrudan Machine Learning Studio'dan bir web hizmeti olarak dağıtabilirsiniz. Bu işlem hakkında daha ayrıntılı bilgi için bkz. [Bir Azure Machine Learning web hizmetini dağıtma](machine-learning-publish-a-machine-learning-web-service.md).

[ml-studio-overview]:./media/machine-learning-what-is-ml-studio/azure-ml-studio-diagram.jpg

<!-- Module References -->
[convert-to-arff]: https://msdn.microsoft.com/library/azure/62d2cece-d832-4a7a-a0bd-f01f03af0960/
[elementary-statistics]: https://msdn.microsoft.com/library/azure/3086b8d4-c895-45ba-8aa9-34f0c944d4d3/
[linear-regression]: https://msdn.microsoft.com/library/azure/31960a6f-789b-4cf7-88d6-2e1152c0bd1a/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/

