---
title: Nedir
titleSuffix: Azure Machine Learning Studio
description: Azure Machine Learning Studio, modelleri kullanıma hazır algoritmalar ve modüller kitaplığından hızla oluşturmaya yönelik bir Sürükle ve bırak aracıdır.
services: machine-learning
documentationcenter: ''
author: garyericson
ms.custom: seodec18
ms.author: garye
ms.assetid: e65c8fe1-7991-4a2a-86ef-fd80a7a06269
ms.service: machine-learning
ms.subservice: studio
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 04/20/2019
ms.openlocfilehash: dd1eaa95a23deed0bf2098995be43402c605defc
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/02/2019
ms.locfileid: "65024211"
---
# <a name="what-is-azure-machine-learning-studio"></a>Azure Machine Learning Studio nedir?
Microsoft Azure Machine Learning Studio, verilerinizde tahmine dayalı analiz çözümleri oluşturma, test etme ve dağıtma amacıyla kullanabileceğiniz bir işbirliğine dayalı sürükle ve bırak aracıdır. Machine Learning Studio, modelleri özel uygulamalar veya Excel gibi BI araçları tarafından kolayca kullanılabilen web hizmetleri olarak yayımlar.

Machine Learning Studio, verilerinizin veri bilimi, tahmine dayalı analiz ve bulut kaynakları ile buluştuğu yerdir.


## <a name="the-machine-learning-studio-interactive-workspace"></a>Machine Learning Studio etkileşimli çalışma alanı
Tahmine dayalı analiz modeli geliştirmek için genellikle bir veri kullanın veya daha fazla kaynağı dönüştürme, çeşitli veri işleme ve istatistik işlevleri aracılığıyla bu verileri analiz edin ve bir sonuç kümesi oluşturur. Bir modelin bu şekilde geliştirilmesi yinelemeli bir işlemdir. Çeşitli işlevleri ve bunların parametrelerini değiştirirken, eğitilmiş ve verimli bir model elde ettiğinizi düşüneceğiniz ana kadar sonuçlarınız yakınsanır.

**Azure Machine Learning Studio**, tahmine dayalı bir analiz modelini kolayca oluşturma, test etme ve yineleme amacıyla etkileşimli ve görsel bir çalışma alanı sunar. ***Veri kümelerini*** ve analiz ***modüllerini*** etkileşimli bir tuvale sürükleyip bırakır ve bunları birbirine bağlayarak Machine Learning Studio'da çalıştıracağınız bir ***deneme*** oluşturursunuz. Model tasarımınızı yinelemek için, denemeyi düzenleyin, isterseniz bir kopyasını kaydedin ve yeniden çalıştırın. Hazır olduğunuzda, ***eğitim denemenizi*** bir ***tahmine dayalı denemeye*** dönüştürebilir ve ardından modelinize başkaları tarafından erişilebilmesi için bunu bir ***web hizmeti*** olarak yayımlayabilirsiniz.

Programlama gerekmez; tahmine dayalı analiz modelinizi oluşturmak için veri kümelerini ve modülleri görsel olarak bağlamanız yeterlidir.

![Azure Machine Learning studio diyagramı: Denemeleri oluşturmak, birçok kaynak için veri okuma, puanlanmış veri yazma, model yazma.](./media/what-is-ml-studio/azure-ml-studio-diagram.jpg)

## <a name="download-the-machine-learning-studio-overview-diagram"></a>Machine Learning Studio'ya genel bakış diyagramını indirme
**Microsoft Azure Machine Learning Studio İşlevlerine Genel Bakış** diyagramını indirin ve Machine Learning Studio işlevlerine üst düzey bir genel bakış elde edin. Diyagramı yakınınızda tutmak için tabloid boyutunda (11 x 17 inç) yazdırabilirsiniz.

**Diyagramı buradan indirin: [Microsoft Azure Machine Learning Studio işlevlerine genel bakış](https://download.microsoft.com/download/C/4/6/C4606116-522F-428A-BE04-B6D3213E9E52/ml_studio_overview_v1.1.pdf)**
![Microsoft Azure Machine Learning Studio işlevlerine genel bakış](./media/what-is-ml-studio/ml_studio_overview_v1.1.png)

## <a name="get-started-with-machine-learning-studio"></a>Machine Learning Studio ile çalışmaya başlama
Machine Learning Studio'da ilk girdiğinizde] (https://studio.azureml.net) gördüğünüz **giriş** sayfası. Buradan belgeleri, videoları, web seminerlerini görüntüleyebilir ve diğer değerli kaynakları bulabilirsiniz.

Sol üst menüye tıkladığınızda ![Menü](./media/what-is-ml-studio/menu.png) birkaç seçenek göreceksiniz.
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
**Galeri** sekmesine tıkladığınızda **[Azure AI Gallery](https://gallery.azure.ai/)** açılır. Galeri, bir veri bilimcileri ve geliştiricileri topluluğunun Cortana Intelligence Suite bileşenleri kullanılarak oluşturduğu çözümleri paylaştığı yerdir.

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

Tahmine dayalı analiz çözümü oluşturmak daha kapsamlı kılavuzu için bkz [Azure Machine Learning Studio'da öngörülebilir bir çözüm geliştirin](tutorial-part1-credit-risk.md).

### <a name="datasets"></a>Veri kümeleri
Bir veri kümesi, model oluşturma işleminde kullanılabilmesi için Machine Learning Studio'ya yüklenen verilerdir. Machine Learning Studio'da deneme yapabileceğiniz birçok örnek veri kümesi bulunur ve ihtiyaç duyarsanız daha çok veri kümesi yükleyebilirsiniz. Dahil olan veri kümelerine aşağıda birkaç örnek verilmiştir:

* **Çeşitli otomobiller için MPG verileri** - Otomobiller için silindir, beygir gücü, vb. sayısına göre tanımlanan galon başına mil (MPG) değerleri.
* **Meme kanseri verileri** - Meme kanseri tanılama verileri.
* **Orman yangını verileri** - Kuzey doğu Portekiz'de orman yangını boyutları.

Bir deneme oluştururken, tuvalin solundaki kullanılabilir veri kümesi listesinden seçebilirsiniz.

Machine Learning Studio'ya dahil olan örnek veri kümelerinin listesi için bkz. [Azure Machine Learning Studio'daki örnek veri kümelerini kullanma](use-sample-datasets.md).

### <a name="modules"></a>Modüller
Bir modül, verilerinizde gerçekleştirebileceğiniz bir algoritmadır. Machine Learning Studio, veri alım işlevlerinden eğitim, puanlama ve doğrulama işlemlerine kadar değişiklik gösteren birçok modüle sahiptir. Dahil olan modüllere aşağıda birkaç örnek verilmiştir:

* [ARFF'ye Dönüştürme][convert-to-arff] - Seri hale getirilmiş .NET veri kümesini Öznitelik-İlişki Dosyası Biçimi'ne (ARFF) dönüştürür.
* [Basit İstatistikleri Hesaplama][elementary-statistics] - Ortalama, standart sapma vb. basit istatistikleri hesaplar.
* [Doğrusal Regresyon][linear-regression] - Çevrimiçi bir gradyan düşüşü tabanlı doğrusal regresyon modeli oluşturur.
* [Model Puanlama][score-model] - Eğitilmiş bir sınıflandırma veya regresyon modelini puanlar.

Bir deneme oluştururken, tuvalin solundaki kullanılabilir modül listesinden seçebilirsiniz.

Bir modül, modülün iç algoritmalarını yapılandırmak için kullanabileceğiniz parametreler kümesine sahip olabilir. Tuvalde bir modül seçtiğinizde, modülün parametreleri tuvalin sağındaki **Özellikler** bölmesinde görüntülenir  Modelinizi ayarlamak için, bu bölmedeki parametreleri değiştirebilirsiniz.

Bazı Yardım büyük makine öğrenimi algoritma kitaplığı gezinmek için bkz: [Microsoft Azure Machine Learning Studio için algoritma seçme](algorithm-choice.md).

## <a name="deploying-a-predictive-analytics-web-service"></a>Tahmine dayalı analiz web hizmetini dağıtma
Tahmine dayalı analiz modeliniz hazır olduktan sonra, bunu doğrudan Machine Learning Studio'dan bir web hizmeti olarak dağıtabilirsiniz. Bu işlem hakkında daha ayrıntılı bilgi için bkz. [Bir Azure Machine Learning web hizmetini dağıtma](publish-a-machine-learning-web-service.md).

<a name="compare"></a>
## <a name="how-is-machine-learning-studio-different-from-azure-machine-learning-service"></a>Ne Machine Learning Studio, Azure Machine Learning hizmetinden farklıdır?

[Azure Machine Learning hizmeti](../service/overview-what-is-azure-ml.md) hem SDK'lar sağlar **- ve -** hızla veri hazırlama için bir görsel interface(preview) eğitme ve makine öğrenimi modelleri dağıtın. Bu görsel bir arabirim (Önizleme) Studio'ya benzer bir Sürükle ve bırak deneyimi sağlar. Ancak, özel işlem platform Studio, farklı görsel arabirim kendi işlem kaynakları kullanır ve Azure Machine Learning hizmetinde tam olarak tümleşiktir.

Hızlı bir karşılaştırması aşağıdadır.

|| Machine Learning Studio | Azure Machine Learning hizmeti:<br/>Görsel arabirim|
|---| --- | --- |
|| Genel kullanıma (GA) | Önizleme aşamasında|
|Modüller için arabirimi| Many | İlk dizi popüler modülleri|
|Eğitim işlem hedefleri| Özel işlem hedefi, yalnızca CPU desteği| Azure Machine Learning işlem, GPU veya CPU destekler.<br/>(Desteklenen SDK'yı diğer hesaplar)|
|Dağıtım işlem hedefleri| Özel web hizmeti biçimi, özelleştirilemeyen | Kurumsal güvenlik seçenekleri ve Azure Kubernetes hizmeti. <br/>([Diğer hesaplar](../service/how-to-deploy-and-where.md) desteklenen SDK'sı) |
|Otomatik model eğitiminin ve hiper parametre ayarı | Hayır | Henüz visual arabiriminde. <br/> (SDK ve Azure Portalı'nda desteklenmiyor.) | 

Görsel arabirim (Önizleme) ile denemeye [hızlı başlangıç: Hazırlama ve kod yazmaya gerek kalmadan verileri görselleştirin](../service/ui-quickstart-run-experiment.md)

> [!NOTE]
> Modelleri Studio'da oluşturulan dağıtılmış veya Azure Machine Learning hizmeti tarafından yönetilir. Ancak, oluşturulan ve dağıtılan hizmet visual arabiriminde modelleri Azure Machine Learning hizmeti çalışma yönetilebilir.

## <a name="free-trial"></a>Ücretsiz deneme sürümü

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]


## <a name="next-steps"></a>Sonraki adımlar
Tahmine dayalı analiz temellerini öğrenin ve makine öğreniminin bir [adım adım hızlı](create-experiment.md) ve [örnekler üzerinden giderek](sample-experiments.md).

<!-- Module References -->
[convert-to-arff]: https://msdn.microsoft.com/library/azure/62d2cece-d832-4a7a-a0bd-f01f03af0960/
[elementary-statistics]: https://msdn.microsoft.com/library/azure/3086b8d4-c895-45ba-8aa9-34f0c944d4d3/
[linear-regression]: https://msdn.microsoft.com/library/azure/31960a6f-789b-4cf7-88d6-2e1152c0bd1a/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
