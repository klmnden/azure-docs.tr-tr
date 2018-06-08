---
title: Enerji talep zaman serisi tahmin | Microsoft Docs
description: Azure Machine Learning çalışma ekranındaki enerji talep zaman serisi tahmin etmek öğrenme makine uygulamak nasıl.
services: machine-learning
documentationcenter: ''
author: anta
manager: ireiter
editor: anta
ms.assetid: ''
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: desktop-workbench
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/15/2017
ms.author: anta
ms.openlocfilehash: 19e62364599ec9f1d0571e6f6e35205b0bb93b4b
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34832542"
---
# <a name="energy-demand-time-series-forecasting"></a>Enerji talep zaman serisi tahmin


Zaman serisi tahmin gözlemleri zaman sıralı bir dizi gelecekteki değerleri tahmin etmeye görevdir. Sık karşılaşılan bir sorundur ve birçok sektörün uygulamalar vardır. Örneğin, perakende şirketler talebi karşılamak üzere kendi tedarik zinciri etkili bir şekilde düzenleyebilirsiniz şekilde gelecekteki ürün satış tahmin gerekir. Benzer şekilde, paket teslim şirketler iş gücü gereksinimlerini ve teslimat rotaları önceden planlayabilirsiniz şekilde hizmetlerini talep tahmin etmeniz gerekir. Çoğu durumda, yanlış tahminleri finansal risklerini önemli olabilir. Bu nedenle, tahmin işlemi genellikle bir iş kritik etkinliğidir.

Bu örnek, ne zaman serisi tahmin machine learning tekniklerini uygulayarak aracılığıyla gerçekleştirilebilir gösterir. Modelleme her adımda kılavuzluk edilir işlemi dahil olmak üzere:
- Veri hazırlama'temizleyin ve verilerin biçimlendirilmesi için;
- Ham zaman serisi veri modelinden öğrenme makinenin özelliklerini oluşturma;
- Çeşitli makine öğrenimi modellerini eğitim;
- Modelleri tutulan çıkış sınama veri kümesi üzerinde kendi performansını karşılaştırarak değerlendirme; ve
- İsteğe bağlı tahminleri oluşturmak için bir web hizmeti üzerinden kullanılabilir hale getirme en iyi modeli faaliyete geçirmeye yönelik.

Azure Machine Learning çalışma ekranı her adım modelleme işlemi yardımları: 
- Örnek çalışma ekranı - kullanılabilir doğrudan Jupyter not defteri ortam - nasıl yapabilirsiniz gösterir Python kodu daha kolay geliştirme. Model geliştirme sürecinin daha net bir şekilde markdown ek açıklamalar ve grafikler kullanarak başkalarına açıklanabilir. Bu dizüstü bilgisayarlar, düzenlenmesi ve doğrudan çalışma ekranından yürütülen görüntülenebilir.
- Eğitilmiş modeller kalıcı ve blob depolama alanına yüklenir. Bu, eğitilen model nesneleri izlemek ve saklama ve gerektiğinde alınabilir olduklarından emin olmak için veri Bilimcisi yardımcı olur.
- Ölçümleri modeli performans puanları kaydını tutmak veri Bilimcisi etkinleştirme bir Python betiği yürütülürken kaydedilebilir.
- Çalışma ekranı kolayca modeli performans ölçümleri karşılaştırmak veri Bilimcisi izin vererek oturum ölçümleri özelleştirilebilir tablolar oluşturur. Model gerçekleştirme en iyi kolayca belirlenebilir şekilde grafikler otomatik olarak görüntülenir.
- Son olarak, nasıl bir modeli gerçek zamanlı web hizmetinde dağıtarak operationalized örnek göstermektedir.

## <a name="link-to-the-gallery-github-repository"></a>Galeri GitHub deponuza bağlayın
Bu gerçek dünya senaryoları için ortak GitHub depo kod örnekleri, bu örnek için gerekli dahil olmak üzere tüm malzemeleri içerir:

[https://github.com/Azure/MachineLearningSamples-EnergyDemandTimeSeriesForecasting](https://github.com/Azure/MachineLearningSamples-EnergyDemandTimeSeriesForecasting)


## <a name="use-case-overview"></a>Kullanım örneği'ne genel bakış

Bu senaryo, bir enerji kılavuz gelecekteki yükü tahmin etmek için hedef olduğu tahmin enerji isteğe bağlı odaklanır. İşleçler hakkında bir kılavuz tüketilen enerji ve kendisine sağlanan enerji arasındaki iyi dengeyi bulundurması gerekecek enerji kesimdeki şirketler için önemli iş işlemi olur. Kılavuza sağlanan çok fazla güç enerji veya teknik hataları kaybı neden olabilir. Ancak, çok az güç sağlanırsa, müşterilerin güç olmadan bırakarak blackouts yol açabilir. Genellikle, kılavuz işleçler kılavuzuna enerji kaynağı yönetmek ve yük bakiyeye tutmak için kısa vadeli kararları alabilir. Enerji isteğe bağlı olarak doğru bir kısa vadeli tahmin, bu nedenle işleci güvenle bu kararlar almak gereklidir.

Bu senaryo bir machine learning çözüm tahmin enerji talep yapımı ayrıntılarını verir. Çözüm, ortak bir veri kümesinden alınan üzerinde eğitildi [New York bağımsız sistem işleci (NYISO)](http://www3.dps.ny.gov/W/PSCWeb.nsf/All/298372E2CE4764E885257687006F39DF?OpenDocument), New York durumu için güç kılavuz çalışır. Veri kümesi, beş yıl döneminde saatlik New York şehrinde güç talep verilerini içerir. Saatlik aynı saat diliminde New York şehrinde hava koşulları içeren ek bir veri kümesini alındığı [darksky.net](https://darksky.net).

## <a name="prerequisites"></a>Önkoşullar

- Bir [Azure hesabı](https://azure.microsoft.com/free/) (ücretsiz deneme kullanılabilir).
- Yüklü bir kopyasını [Azure Machine Learning çalışma ekranı](../service/overview-what-is-azure-ml.md) aşağıdaki [hızlı başlangıç Yükleme Kılavuzu](../service/quickstart-installation.md) programı yüklemek ve bir çalışma alanı oluşturmak için.
- Bu örnek, Windows 10 ile Azure ML çalışma ekranı çalıştığını varsayar [Docker altyapısına](https://www.docker.com/) yerel olarak yüklü. MacOS kullanıyorsanız, büyük ölçüde aynı yönergelerdir.
- Bu konuda açıklandığı gibi Azure Machine Learning ayarlanmış bir yerel dağıtım ortamı ve bir model yönetim hesabı ile yüklü Operationalization oluşturulan [Kılavuzu](./model-management-configuration.md).
- Bu örnek 0.20.3 sürüme Pandas yükleme güncelleştirmenin gerektirdiği veya üstü ve matplotlib yükleyin. Tıklatın *komut istemini açın* gelen *dosya* menüde çalışma ekranı ve bu bağımlılıklar yüklemek için aşağıdaki komutları çalıştırın:

    ```
    conda install "pandas>=0.21.1"
    ```
    
## <a name="create-a-new-workbench-project"></a>Yeni bir çalışma ekranı projesi oluşturma

Bu örnek bir şablon kullanarak yeni bir proje oluşturun:
1.  Açık Azure Machine Learning çalışma ekranı
2.  Üzerinde **projeleri** sayfasında, **+** oturum ve seçin **yeni proje**
3.  İçinde **yeni proje oluştur** bölmesinde, yeni projeniz için bilgileri doldurun
4.  İçinde **arama proje şablonları** arama kutusu, "Enerji talep zaman serisi tahmin" yazın ve şablonu seçin
5.  **Oluştur**'a tıklayın


## <a name="data-description"></a>Veri açıklaması

İki veri kümesi ile bu örnek sağlanır ve kullanılarak yüklenen `1-data-preparation.ipynb` Not: `nyc_demand.csv` ve `nyc_weather.csv`.

**nyc_demand.csv** saatlik 2012 2017 yıldır New York şehrinde enerji talep değerlerini içerir. Verileri aşağıdaki basit yapıya sahiptir:

| zaman damgası | İsteğe bağlı |
| --- | --- |
| 2012-01-01 00:00:00 | 4937.5 |
| 2012-01-01 01:00:00 | 4752.1 |
| 2012-01-01 02:00:00 | 4542.6 |
| 2012-01-01 03:00:00 | 4357.7 |

İsteğe bağlı, megawatt-saat (MWh) değerlerdir. Bir grafik Temmuz 2017 7 günlük sürede üzerinden enerji talebin aşağıdadır:

![Enerji isteğe bağlı](./media/scenario-time-series-forecasting/energy_demand.png  "enerji isteğe bağlı")

**nyc_weather.csv** saatlik 2012 2017 yıllar içinde New York şehrinde hava durumu değerlerini içerir:

| zaman damgası | precip | Temp
| --- | --- | --- |
| 2012-01-01 00:00:00 | 0.0 | 46.13 |
| 2012-01-01 01:00:00 | 0.01 | 45.89 |
| 2012-01-01 02:00:00 | 0.05 | 45.04 |
| 2012-01-01 03:00:00 | 0.02 | 45.03 |

*precip* precipitation düzeyini yüzde ölçüsüdür. *Temp* (sıcaklık) değerleri boyutlandırılan sağlayacak şekilde tüm değerler [0, 100] aralığındaki ayrılır.

## <a name="scenario-structure"></a>Senaryo yapısı

Azure Machine Learning çalışma ekranındaki ilk kez bu projeyi açtığınızda gidin *dosyaları* sol taraftaki bölmesi. Aşağıdaki kod dosyaları ile bu örnek verilmiştir:
- `1-data-preparation.ipynb` -Bu Jupyter not defteri indirir ve modelleme için hazırlamak için verileri işler. Bu, çalışacak ilk dizüstü bilgisayardır.
- `2-linear-regression.ipynb` -Bu Not eğitim verileri bir doğrusal regresyon modeli eğitir.
- `3-ridge.ipynb` -çıkıntı regresyon modeli eğitir.
- `4-ridge-poly2.ipynb` -derece 2 polinom özelliklerini bir çıkıntı regresyon modeli eğitir.
- `5-mlp.ipynb` -çok katmanlı perceptron sinir ağı eğitir.
- `6-dtree.ipynb` -karar ağacı modeli eğitir.
- `7-gbm.ipynb` -Gradyan boosted makine modeli eğitir.
- `8-evaluate-model.py` -Bu komut dosyası eğitilen modelini yükler ve tutulan çıkış test veri kümesinde tahminlerde için kullanır. Farklı modelleri performansını karşılaştırılabilir şekilde modeli değerlendirme ölçümleri üretir.
- `9-forecast-output-exploration.ipynb` -Bu Dizüstü modelleri öğrenme makinesi tarafından oluşturulan tahminleri görselleştirmeleri üretir.
- `10-deploy-model.ipynb` -eğitilmiş bir tahmin modeli gerçek zamanlı web hizmetinde nasıl operationalized bu not gösterir.
- `evaluate-all-models.py` -Bu komut tüm eğitilmiş modeller değerlendirir. Çalışan bir alternatif sunar `8-evaluate-model.py` ayrı ayrı her biri için eğitilmiş model.

### <a name="1-data-preparation-and-cleaning"></a>1. Veri hazırlama ve temizleme

Örnek çalıştıran başlatmak için önce tıklayın `1-data-preparation.ipynb` not defteri önizleme modunda açın. Tıklayın ***Start not defteri Server*** Jupyter not defteri sunucu ve Python çekirdek makinenizde başlatmak için. Çalışma ekranının içinden defterlerinden ya da çalıştırabilir veya tarayıcınızı giderek kullanabileceğiniz [ http://localhost:8888 ](http://localhost:8888). Çekirdek ile değiştirmelisiniz Not *PROJECT_NAME yerel*. Bunu yapmak *çekirdek* not defterinin altında menüde *değişiklik çekirdek*. Tuşuna ***üst karakter + Enter*** tıklayın veya tek tek not defteri hücrelerde kodu çalıştırmak *tümünü Çalıştır* içinde *hücre* tüm not çalıştırmak için menüsü.

Not defteri verileri Azure üzerinde barındırılan bir blob depolama kapsayıcısını gelen ilk yükler. Veri sonra makinenizde depolanan `AZUREML_NATIVE_SHARE_DIRECTORY`. Bu konumda herhangi defterlerinden erişilebilir veya çalışma ekranından Çalıştır komut dosyaları bu nedenle veri ve eğitilmiş modeller depolamak için uygun bir yerdir.

Veriler yüklendikten sonra not defteri verileri zaman serisinde aralıkları doldurma ve eksik değerleri ilişkilendirme tarafından girerek temizler. Bu şekilde zaman serisi veri temizleme modeli eğitmek için kullanılabilir veri miktarını en üst düzeye çıkarır.

Birkaç modeli özelliklerini Temizlenen ham verilerden oluşturulur. Bu özellikler iki gruba kategorilere ayrılabilir:

- **Özellikler güdümlü zaman** türetilmiş *zaman damgası* değişkeni örneğin *ay*, *dayofweek* ve *saat*.
- **Özellikler lagged** gerçek isteğe bağlı veya sıcaklık değerlerin gölgeye zaman değerlerdir. Modeldeki ardışık dönemleri arasındaki koşullu bağımlılıkları yakalamak için bu özellikleri hedefleyin.

Sonuçta elde edilen özelliği dataset tahmin modelleri geliştirirken kullanılabilir.

### <a name="2-model-development"></a>2. Model geliştirme

İlk modelleme yaklaşım Basit doğrusal regresyon modelini olabilir. `2-linear-regression.ipynb` Not defteri gelecekteki enerji isteğe bağlı bir doğrusal regresyon tahmin modeli eğitmek nasıl gösterir. Özellikle, model enerji isteğe bağlı bir saat tahmin etmek için Eğitilecek (*t + 1*) geçerli süre öncesinde (*t*). Biz gelecekteki zaman aralıkları için tahminleri oluşturmak için test zaman bu 'tek adımlı' model yinelemeli ancak uygulayabilirsiniz *t + n*.

Bir modeli eğitmek için üç ayrı adımdan oluşur, Tahmine dayalı bir ardışık düzen oluşturulur:

- **Bir veri dönüşümünün**: giriş verileri için gerekli biçimleri makine öğrenme algoritmasını bağlı olarak değişebilir. Bu durumda, doğrusal regresyon modeli bir hot kodlanacak kategorik değişkenler gerektirir.
- **Çapraz doğrulama yordamı A**: Makine öğrenimi modeli deneme ayarlanması gereken bir veya daha fazla hyperparameters genellikle sahip olur. Çapraz doğrulama en iyi parametre değerleri kümesi bulmak için kullanılabilir. Model art arda eğitilmiş ve veri kümesi farklı Katlama üzerinde değerlendirilir. En iyi da arasında çapraz doğrulama Katlama ortalaması çalışırken en iyi modeli performansı elde etmek parametreleridir. Bu işlem daha ayrıntılı olarak anlatılmıştır `2-linear-regression.ipynb`.
- **Son model eğitim**: model hyperparameters en iyi kümesini kullanarak tüm veri kümesi üzerinde eğitildi.

Biz 6 farklı modelleri bir dizi not defterlerini numaralı 2-7'de kullanıma sunuyoruz. Her dizüstü bilgisayar farklı bir model eğitir ve depolar `AZUREML_NATIVE_SHARE_DIRECTORY` konumu. Bu senaryo için geliştirilen tüm modelleri eğitilmiş sonra biz değerlendirmek ve bunları sonraki bölümde decribed olarak karşılaştırır.

### <a name="3-model-evaluation-and-comparison"></a>3. Model değerlendirme ve karşılaştırma

Gelecekteki zaman aralıkları için tahminleri yapmak için size bir modeli kullanabilirsiniz. Bu modeller tutulan çıkış sınama veri kümesi üzerinde değerlendirmek en iyisidir. Aynı görünmeyen veri farklı modellerde değerlendirerek size oldukça kendi performansını karşılaştırır ve en iyi modeli tanımlamak. Bu senaryoda, gelecekte birden çok zaman adımları tahmin etmek için eğitilen model yinelemeli olarak uygulanır. Özellikle, biz tahminleri altı saate kadar Oluştur *t + 6* geçerli saatte öncesinde *t*. Bu Öngörüler her zaman aralığında gözlemlenen gerçek talebe göre değerlendirilir.

Bir model değerlendirmek için açık `8-evaluate-model.py` betikten *dosyaları* çalışma ekranı bölmesinde. Denetleyin *yapılandırma Çalıştır* ayarlanır **yerel** ve model ad alanının içine yazın *bağımsız değişkenleri* alan. Model adı tam olarak eşleşmesi gerekiyor *model_adı* değişkenini model eğitilmiş dizüstü bilgisayar başlangıcında. Örneğin, eğitilen doğrusal regresyon modeli değerlendirmek için "linear_regression" girin. Tüm modelleri eğitilen sonra alternatif olarak, bunları bir komutla çalıştırarak değerlendirebilmeniz `evaluate-all-models.py`. Bu komut dosyasını çalıştırmak için çalışma ekranından bir komut istemi açın ve aşağıdaki komutu yürütün:

```
python evaluate-all-models.py
```
`8-evaluate-model.py` Komut dosyası, aşağıdaki işlemleri gerçekleştirir:

- Disk, eğitilen model ardışık düzen yükler
- Test veri kümesinde tahminleri yapar
- Model performans ölçümleri hesaplar ve bunları günlüğe kaydeder
- Veri kümesi tahminleri test kaydeder `AZUREML_NATIVE_SHARE_DIRECTORY` sonraki denetleme ve analiz için
- Eğitilen model ardışık düzene kaydeder *çıkarır* klasör.

> [!Note]
> Modele kaydetme *çıkarır* klasörü otomatik olarak kopyalar model nesnesi, deneme hesabınızla ilişkilendirilmiş Azure Blob Storage hesaba. Bu, her zaman makine değiştirin veya içeriği işlem olsa bile kaydedilmiş model nesnesi blobundan alamadı anlamına gelir.

Gidin *çalıştırma geçmişi* bölmesinde ve tıklayarak `8-evaluate-model.py`. Grafikleri birkaç modeli performans ölçümleri görüntüleme görürsünüz:

- **BANA**: tahmin hata anlamına gelir. Bu tahmini ortalama sapması yorumlanacak.
- **MPE**: [ortalama yüzdesini hata](https://en.wikipedia.org/wiki/Mean_percentage_error) (ME gerçek isteğe bağlı bir yüzdesi olarak ifade edilen)
- **MSE**: [karesi alınmış hata anlama](https://en.wikipedia.org/wiki/Mean_squared_error)
- **RMSE**: kök ortalama karesi alınmış hata (MSE kare kökünü)
- **MAPE**: [mutlak yüzdesi hata anlama](https://en.wikipedia.org/wiki/Mean_absolute_percentage_error)
- **sMAPE**: [simetrik ortalama mutlak yüzdesi hata](https://en.wikipedia.org/wiki/Symmetric_mean_absolute_percentage_error)
- **MAPE_base**: MAPE temelinin tahmin tahmin son bilinen talep değeri eşittir.
- **MdRAE**: ORTANCA göreli mutlak hata. Tahmin hatası taban Medyan oranını hata tahmin. Bir değer değerinden tahmin taban daha iyi gerçekleştiriyor 1 anlamına gelir.

Yukarıdaki tüm ölçümleri başvurmak *t + 1* tahmin. Yukarıdaki ölçümlerin yanı sıra ile ilgili ölçümleri kümesi de görürsünüz *_horizon* soneki. Bu süre tahmin aralığındaki tüm nokta üzerinden ortalaması ölçümüdür *t + 1* süresini *t + 6*.

Ölçümleri grafik alanında görüntülenmiyor, sağ üst köşesindeki dişli simgenin tıklayın. İlgilendiğiniz modeli performans ölçümleri seçildiğinden emin olun. Ölçümleri de grafikleri bir tabloda görüntülenir. Bu tablo yeniden özelleştirilebilir tıklatarak dişli takımından olabilir. Tablo en iyi modeli tanımlamak için bir performans ölçümü göre sıralayın. Tahmin performansın dönemden nasıl değişeceğini görmeniz ilginizi *t + 1* için *t + 6*, tablonun modelde girişini tıklatın. Grafikler, MAPE görüntüleme MPE ve MdRAE ölçümleri tahmin dönemi boyunca altında gösterilen *görselleştirmeleri*.

Tahmin modelleri değerlendirirken, çıktı tahminleri görselleştirmek çok kullanışlı olabilir. Bu, üretilen tahmin gerçekçi görünür olup olmadığını belirlemek için veri Bilimcisi yardımcı olur. Örneğin, tahmin kötü belirli dönemlerde gerçekleştirirse, tahmin sorunlarını tanımlamak için de yardımcı olabilir. `9-forecast-output-exploration.ipynb` Not defteri sınama veri kümesi için oluşturulan tahminleri görselleştirmeleri oluşturacak.

### <a name="4-deployment"></a>4. Dağıtım

En iyi modeli gerçek zamanlı web hizmeti olarak dağıtarak operationalized. Bu web Hizmeti'nin, ardından yeni veriler kullanılabilir olduğunda isteğe bağlı tahminleri oluşturmak için çağrılabilir. Bu senaryoda, yeni bir tahmini enerji isteğe bağlı sonraki bir saat içinde tahmin etmek için her saat oluşturulacak gerekir. Bu görevi gerçekleştirmek için bir web hizmeti olabilir dağıtılan belirli bir saat süre giriş olarak bir dizi özelliklerin alır ve tahmin edilen talep çıktısı olarak döndürür.

Bu örnekte, bir web hizmeti bir Windows 10 makineye dağıtılır. Bu konuda ayarlanan yerel dağıtım için önkoşul adımlarını tamamladığınızdan emin olun [Başlangıç Kılavuzu](https://github.com/Azure/Machine-Learning-Operationalization/blob/master/documentation/getting-started.md) Operationalization CLI için. Yerel ortamınıza ve model yönetim hesabı oluşturduktan sonra çalıştırmak `10-deploy-model.ipynb` web hizmeti dağıtmak için dizüstü bilgisayar.

## <a name="conclusion"></a>Sonuç

Bu örnek nasıl çözüm tahmin enerji talep tahmin amacıyla bir uçtan uca zaman serisi oluşturulacağını gösterir. Bu örnekte incelediniz ilkeleri birçoğu, diğer tahmin senaryoları ve sektörlerde genişletilebilir.

Bu senaryoda, nasıl Azure Machine Learning çalışma ekranı veri Bilimcisi ölçüm günlüğe kaydetme özellikleri ve Jupyter not defteri ortamı gibi yararlı özellikleri ile gerçek dünya çözümleri geliştirirken yardımcı olabilecek gösterir. Örnek ayrıca okuyucu nasıl bir model operationalized ve Azure Machine Learning Operationalization CLI kullanarak dağıtılmış size rehberlik eder. Dağıtıldığında, web hizmeti API'sine geliştiriciler veya veri mühendisleri tahmin modeli daha geniş bir veri ardışık düzenine tümleştirmenize olanak sağlar.
