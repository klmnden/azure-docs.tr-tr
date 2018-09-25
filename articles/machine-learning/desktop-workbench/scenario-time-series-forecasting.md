---
title: Enerji talebi zaman serileri tahmini | Microsoft Docs
description: Azure Machine Learning workbench'te enerji talebi zaman serileri tahmini için makine öğrenimini uygulamak nasıl.
services: machine-learning
documentationcenter: ''
author: anta
manager: ireiter
editor: anta
ms.assetid: ''
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/15/2017
ms.author: anta
ROBOTS: NOINDEX
ms.openlocfilehash: 934d37783165c2e57dcabd0ff764747e1ab4b65e
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46946876"
---
# <a name="energy-demand-time-series-forecasting"></a>Enerji talebi zaman serileri tahmini

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)] 




Zaman serisi tahmin sonraki değerleri gözlemler zamana göre sıralı bir dizisi içindeki bir görevdir. Sık karşılaşılan bir sorundur ve sektörlerde birçok uygulama vardır. Örneğin, talebi karşılamak üzere, tedarik zinciri etkili bir şekilde düzenleyebilirsiniz için gelecekteki ürün satış tahmini perakende şirketlerin gerekir. Benzer şekilde, paket teslim şirketlerin iş gücü gereksinimleri ve teslimat rotaları önceden planlayabilmemiz hizmetlerini talep tahmin etmek gerekir. Çoğu durumda, finansal risk yanlış tahminleri önemli olabilir. Bu nedenle tahmini genellikle iş kritik bir etkinlik gereklidir.

Bu örnek, ne zaman serisi tahmini makine öğrenme teknikleri uygulayarak aracılığıyla gerçekleştirilebilir gösterir. Her bir modelleme adımlayın kılavuzluk edilir işlemi dahil olmak üzere:
- Veri hazırlama; temiz ve verileri biçimlendirme
- Makine öğrenimi modellerinden ham zaman serisi verilerinin özelliklerini oluşturma;
- Çeşitli makine öğrenimi modelleri eğitim;
- Performanslarını tutulan genişletme sınama veri kümesinde karşılaştırarak değerlendirme modelleri; ve
- İsteğe bağlı tahminler üretmek için web hizmeti aracılığıyla kullanılabilir hale getirme en iyi modeli faaliyete.

Azure Machine Learning Workbench, her adımda modelleme işlemi kolaylık sağlar: 
- Workbench - kullanılabilir doğrudan Jupyter not defteri ortam - nasıl yapabilirsiniz örnek gösterir. Python kodu daha kolay geliştirme. Model geliştirme süreci daha net bir şekilde markdown ek açıklamalar ve grafikler kullanarak başkalarına açıklanabilir. Bu not defterlerini görüntülenebilir, düzenlenebilir ve doğrudan Workbench'ten yürütülür.
- Eğitilen modelleri kalıcı yapılabilir ve blob depolama alanına yüklenir. Bu, eğitilen model nesneleri izler ve tutulan ve gerektiğinde alınabilir olduklarından emin olmak için veri uzmanı yardımcı olur.
- Ölçümleri etkinleştirme modeli performans puanları kaydını tutmak için veri uzmanı bir Python betiği yürütülürken kaydedilebilir.
- Workbench oturum ölçüm kolayca modeli performans ölçümleri karşılaştırmak veri uzmanı sağlayan özelleştirilebilir tablolar oluşturur. En iyi performansa sahip model bir kolayca belirlenebilir için grafikler otomatik olarak görüntülenir.
- Son olarak, nasıl eğitilen bir modelin bir gerçek zamanlı web hizmeti dağıtma tarafından kullanıma hazır hale getirdiniz örnek gösterir.

## <a name="link-to-the-gallery-github-repository"></a>Galeri GitHub deponuza bağlayın
Bu örnek için gerekli, kod örnekleri dahil olmak üzere tüm malzemeler, bu gerçek dünya senaryosu için ortak GitHub deposu içerir:

[https://github.com/Azure/MachineLearningSamples-EnergyDemandTimeSeriesForecasting](https://github.com/Azure/MachineLearningSamples-EnergyDemandTimeSeriesForecasting)


## <a name="use-case-overview"></a>Kullanım örneği genel bakış

Enerji talebini tahmin gelecekteki enerji şebekesi yükü tahmin etmek için hedef olduğu Bu senaryoda odaklanır. İşleçler bir kılavuzda kullanılan enerji ve kendisine sağlanan enerji arasındaki hassas dengeyi sürdürmeniz gerekir, enerji sektörü şirket için bir iş açısından kritik işlem aynıdır. Kılavuza verilen çok güç enerji veya teknik hatalar atık neden olabilir. Ancak, çok az güç sağlanmazsa blackouts bırakarak power olmayan müşteriler, açabilir. Genellikle, kılavuz işleçler kılavuzuna enerji tedariğini yönetmek ve yük dengesi tutmak için kısa vadeli kararlar alabilir. Bir doğru kısa süreli bir enerji talebini tahmin, bu nedenle bu kararları güvenle operatör için önemlidir.

Bu senaryo, bir makine öğrenimi enerji talebini tahmin çözümü oluşumu ayrıntıları. Çözüm üzerinde ortak bir veri kümesinden eğitilir [New York bağımsız sistem işleci (benzetim yöntemiyle NYISO'nun)](http://www3.dps.ny.gov/W/PSCWeb.nsf/All/298372E2CE4764E885257687006F39DF?OpenDocument), New York durumu için güç kılavuz çalışır. Veri kümesi, beş yıllık bir dönem boyunca saatlik New York City power istek verilerini içerir. Hava koşulları da oturan aynı süre boyunca saatlik içeren ek bir veri kümesi alındığı [darksky.net](https://darksky.net).

## <a name="prerequisites"></a>Önkoşullar

- Bir [Azure hesabı](https://azure.microsoft.com/free/) (ücretsiz denemeler kullanılabilir).
- Yüklü bir kopyasını [Azure Machine Learning Workbench](../service/overview-what-is-azure-ml.md) aşağıdaki [hızlı başlangıç Yükleme Kılavuzu](quickstart-installation.md) programı yüklemek ve bir çalışma alanı oluşturun.
- Bu örnek, Windows 10 ile Azure ML Workbench çalıştığını varsayar [Docker altyapısı](https://www.docker.com/) yerel olarak yüklü. MacOS kullanıyorsanız, yönergeleri büyük ölçüde aynıdır.
- Azure Machine Learning model Yönetimi hesabı ile ayarlanmış bir yerel dağıtım ortamı ile yüklü kullanıma hazır hale getirme oluşturulan açıklandığı [Kılavuzu](./model-management-configuration.md).
- Bu örnek Pandas yükleme 0.20.3 sürüme güncelleştirme gerektirir ya da daha yüksek ve girerek matplotlib kitaplığını yükleyin. Tıklayın *komut istemini Aç* gelen *dosya* menüde Workbench ve bu bağımlılıkları yüklemek için aşağıdaki komutları çalıştırın:

    ```
    conda install "pandas>=0.21.1"
    ```
    
## <a name="create-a-new-workbench-project"></a>Workbench yeni bir proje oluşturun

Bu örnekte, şablon olarak kullanarak yeni bir proje oluşturun:
1.  Açık bir Azure Machine Learning Workbench'i
2.  Üzerinde **projeleri** sayfasında **+** açıp seçmek **yeni proje**
3.  İçinde **yeni proje oluştur** bölmesinde, yeni projeniz için bilgileri girin
4.  İçinde **proje şablonlarında Ara** arama kutusuna "Enerji talebi zaman serisi tahmin" yazın ve şablonu seçin
5.  **Oluştur**'a tıklayın


## <a name="data-description"></a>Veri açıklaması

İki veri kümesi kullanılarak indirilir ve bu örnekle sağlanan `1-data-preparation.ipynb` Not: `nyc_demand.csv` ve `nyc_weather.csv`.

**nyc_demand.csv** saatlik 2012-2017 yılları için New York City enerji talebi değerlerini içerir. Veriler aşağıdaki basit bir yapıya sahiptir:

| Zaman damgası | İsteğe bağlı |
| --- | --- |
| 2012-01-01 00:00:00 | 4937.5 |
| 2012-01-01 01:00:00 | 4752.1 |
| 2012-01-01 02:00:00 | 4542.6 |
| 2012-01-01 03:00:00 | 4357.7 |

İsteğe bağlı, megawatt-saat içinde (MWh) değerlerdir. Enerji talebini Temmuz 2017'de bir 7 günlük dönemdeki bir grafiğini aşağıdadır:

![Enerji talebi](./media/scenario-time-series-forecasting/energy_demand.png  "enerji talebi")

**nyc_weather.csv** saatlik 2012-2017 yıllar içinde New York City hava durumu değerlerini içerir:

| Zaman damgası | precip | Temp
| --- | --- | --- |
| 2012-01-01 00:00:00 | 0.0 | 46.13 |
| 2012-01-01 01:00:00 | 0.01 | 45.89 |
| 2012-01-01 02:00:00 | 0.05 | 45.04 |
| 2012-01-01 03:00:00 | 0.02 | 45.03 |

*precip* Yağış düzeyini yüzdesi ölçüsüdür. *Temp* (sıcaklık) değerleri boyutlandırılan tüm değerler [0, 100] aralığında kalan şekilde.

## <a name="scenario-structure"></a>Senaryo yapısı

Bu proje, Azure Machine Learning Workbench uygulamasında ilk kez açtığınızda, gitmek *dosyaları* bölmesi sol tarafta. Aşağıdaki kod dosyaları ile bu örnek verilmiştir:
- `1-data-preparation.ipynb` -Bu Jupyter not defteri indirir ve modelleme için hazırlamak için verileri işler. Çalıştırılacak ilk not defteri budur.
- `2-linear-regression.ipynb` -Bu not defteri eğitim verileri bir doğrusal regresyon modeli eğitir.
- `3-ridge.ipynb` -bir ridge regresyon modeli eğitir.
- `4-ridge-poly2.ipynb` -bir polinom özellikleri biri derece 2 ridge regresyon modeli eğitir.
- `5-mlp.ipynb` -çok katmanlı perceptron sinir ağı eğitir.
- `6-dtree.ipynb` -bir karar ağacı modeli eğitir.
- `7-gbm.ipynb` -Gradyan artırmalı makine modeli eğitir.
- `8-evaluate-model.py` -Bu betik, eğitilen modeli yükler ve tutulan genişletme test veri kümesinde tahminlerde bulunmak üzere kullanır. Farklı modellerdeki performansını karşılaştırılabilir için model değerlendirme ölçümleri üretir.
- `9-forecast-output-exploration.ipynb` -Bu not defteri görselleştirmeleri, makine öğrenimi modellerini tarafından oluşturulan tahminlerin üretir.
- `10-deploy-model.ipynb` -nasıl eğitilen bir tahmin modeli bir gerçek zamanlı web hizmetinde kullanıma hazır hale getirdiniz bu not defteri gösterir.
- `evaluate-all-models.py` -Bu komut tüm eğitilen modelleri değerlendirir. Çalışan bir alternatif sunar `8-evaluate-model.py` eğitilen her biri için ayrı ayrı modeli.

### <a name="1-data-preparation-and-cleaning"></a>1. Veri hazırlama ve temizleme

Örnek çalıştırmaya başlamak için önce tıklayarak `1-data-preparation.ipynb` önizleme modunda not defterini açın. Tıklayarak ***Başlat Notebook sunucusu*** Jupyter notebook sunucusu ve Python çekirdek makinenizde başlatmak için. Defterlerinden workbench'teki çalıştırabilir ya da veya tarayıcınızı giderek kullanabileceğiniz [ http://localhost:8888 ](http://localhost:8888). Çekirdek değiştiğine dikkat edin *PROJECT_NAME yerel*. Bu işlemi yapabileceğiniz *çekirdek* menüsü altında not defterinde *değişiklik çekirdek*. Tuşuna ***shift + Enter*** kodu ayrı bir not defteri hücrelerde çalıştırmak veya *tümünü Çalıştır* içinde *hücre* tüm not defterlerini çalıştırmak için menü.

Not defterini ilk verileri Azure üzerinde barındırılan bir blob depolama kapsayıcısına indirir. Veriler makinenizde depolanır `AZUREML_NATIVE_SHARE_DIRECTORY`. Bu konumda hiçbir Not defterlerinden erişilebilir veya Workbench'ten çalıştırılan betikler kadar veri ve eğitilen modelleri depolamak için iyi bir yerdir.

Veriler yüklendikten sonra not defterini verileri zaman serisi boşlukları doldurma ve eksik değerleri ilişkilendirme girerek temizler. Bu şekilde zaman serisi verilerini temizleme modellerin eğitimi için kullanılabilir veri miktarını en üst düzeye çıkarır.

Bazı model özellikleri, temizlenen ham verilerden oluşturulur. Bu özellikler, iki gruplar halinde kategorilere ayrılabilir:

- **Özellik temelli zaman** türetilmiştir *zaman damgası* değişkeni örn *ay*, *dayofweek* ve *saat*.
- **Özellikleri geciktirmiştir** gerçek isteğe bağlı veya sıcaklık değerlerinin saat kaydırılacağı uzaklık değerleri. Bu özellikler arasındaki art arda süreler modelinde koşullu bağımlılıkları yakalamak için hedeflenir.

Sonuçta elde edilen özelliği veri kümesi, tahmin modellerinin geliştirirken kullanılabilir.

### <a name="2-model-development"></a>2. Model geliştirme

İlk bir modelleme yaklaşımı, bir Basit doğrusal regresyon modeli olabilir. `2-linear-regression.ipynb` Not defteri için gelecekteki enerji talebini tahmin doğrusal regresyon modeli eğitmek nasıl gösterir. Özellikle, model bir saat enerji talebini tahmin etmek için Eğitilecek (*t + 1*) geçerli süre önüne (*t*). Biz, gelecekteki zaman dönemlerinde tahminler üretmek için test zaman bu 'tek adımlı' model yinelemeli olarak ancak uygulayabilirsiniz *t + n*.

Bir modeli eğitmek için üç ayrı adımdan oluşur, Tahmine dayalı bir işlem hattı oluşturulur:

- **Veri dönüştürme**: gerekli biçimleri giriş verileri için makine öğrenme algoritmasını bağlı olarak değişebilir. Bu durumda, doğrusal regresyon modelinin bir seyrek kodlanacak kategorik değişkenler gerektirir.
- **Çapraz doğrulama yordamı bir**: Makine öğrenme modeli deneme ayarlanması gereken bir veya daha fazla hiperparametreleri genellikle olacaktır. Çapraz doğrulama, parametre değerlerini en uygun kümesini bulmak için kullanılabilir. Model sürekli eğitim ve veri kümesinin farklı hatları üzerinde değerlendirilir. En iyi parametreleri arasında çapraz doğrulama hatları ortalama, en iyi bir model performansı elde etmek olanlardır. Bu işlem daha ayrıntılı olarak açıklanan `2-linear-regression.ipynb`.
- **Son modelin eğitim**: model hiperparametreleri en iyi kümesini kullanarak tüm veri kümesinde çalıştırılır.

Not defterlerini numaralı 2-7'de bir dizi 6 farklı modeller ekledik. Her bir dizüstü bilgisayar farklı bir model eğitir ve depolar `AZUREML_NATIVE_SHARE_DIRECTORY` konumu. Bu senaryo için geliştirilen tüm modelleri eğitim almış sonra biz değerlendirmek ve sonraki bölümde decribed olarak karşılaştırın.

### <a name="3-model-evaluation-and-comparison"></a>3. Model değerlendirme ve karşılaştırma

Gelecekteki zaman dönemlerinde tahminleri yapmak için eğitilen bir modelin kullanabiliriz. Bu modeller tutulan genişletme sınama veri kümesinde değerlendirmek idealdir. Aynı görünmeyen bir veri kümesini farklı modelleri değerlendirerek oldukça performanslarını karşılaştırma ve en iyi modeli belirleyin. Bu senaryoda, biz gelecekte birden çok zaman adımı tahmin etmek için eğitilen modeli yinelemeli olarak uygulanır. Tahminleri altı saate kadar özellikle, oluşturduğumuz *t + 6* geçerli saatte önüne *t*. Bu tahminler, her bir zaman aralığı için gözlemlenen gerçek talebe göre değerlendirilir.

Bir model değerlendirilecek açın `8-evaluate-model.py` gelen betik *dosyaları* Workbench bölmesinde. Bu maddeyi *yapılandırmayı Çalıştır* ayarlanır **yerel** ve model adını yazın *bağımsız değişkenleri* alan. Model adı tam olarak eşleşmesi gerekiyor *model_adı* değişkenini modeli Eğitimi, dizüstü bilgisayar başında. Örneğin, eğitilen doğrusal regresyon modelinin değerlendirilecek "linear_regression" girin. Tüm modelleri eğitilen sonra alternatif olarak, bunları bir komutla çalıştırarak değerlendirebilirsiniz `evaluate-all-models.py`. Bu betik çalıştırmak için Workbench uygulamasını bir komut istemi açın ve aşağıdaki komutu yürütün:

```
python evaluate-all-models.py
```
`8-evaluate-model.py` Komut dosyası, aşağıdaki işlemleri gerçekleştirir:

- Eğitilen modelin işlem hattı diskten yükler.
- Test veri kümesinde tahminlerde
- Model performansı ölçümlerini hesaplar ve bunları günlüğe kaydeder
- Test veri kümesini Öngörüler için kaydeder `AZUREML_NATIVE_SHARE_DIRECTORY` sonraki inceleme ve çözümleme
- Eğitilen modelin işlem hattına kaydeder *çıkarır* klasör.

> [!Note]
> Modele kaydetme *çıkarır* klasörü otomatik olarak kopyalar model nesnesi, deneme hesabınızla ilişkili Azure Blob Depolama hesabına. Başka bir deyişle, her zaman bile makine değiştirin veya işlem bağlamında blobundan kaydedilmiş model nesnesi almak mümkün olacaktır.

Gidin *çalıştırma geçmişi* bölmesi ve tıklayarak `8-evaluate-model.py`. Birkaç model performansı ölçümlerini görüntüleme grafikleri görürsünüz:

- **BANA**: hata tahminin anlamına gelir. Bu tahmini ortalama sapması yorumlanabilir.
- **MPE**: [ortalama yüzdesi hata](https://en.wikipedia.org/wiki/Mean_percentage_error) (BANA gerçek isteğe bağlı bir yüzdesi olarak ifade edilen)
- **MSE**: [karesi alınmış hata anlama](https://en.wikipedia.org/wiki/Mean_squared_error)
- **RMSE**: kök ortalama karesi alınmış hata (MSE kare kökünü)
- **MAPE**: [mutlak yüzdesi hata anlama](https://en.wikipedia.org/wiki/Mean_absolute_percentage_error)
- **sMAPE**: [simetrik ortalama mutlak yüzdesi hata](https://en.wikipedia.org/wiki/Symmetric_mean_absolute_percentage_error)
- **MAPE_base**: temelinin MAPE tahmin tahmin son bilinen bir isteğe bağlı değerin değerine eşittir.
- **MdRAE**: ORTANCA göreli mutlak hata. ORTANCA taban tahmin hata oranı hata tahmin. Yüksek bir değer daha az tahmin temel daha iyi gerçekleştiriyor 1 anlamına gelir.

Yukarıdaki tüm ölçümler başvurmak *t + 1* tahmin edin. Yukarıdaki ölçümlerin yanı sıra, bir dizi ile ilgili ölçümleri de görürsünüz *_horizon* soneki. Tüm dönem dönem tahmin aralığında üzerinden ortalaması alınan ölçüm budur *t + 1* dönemle *t + 6*.

Ölçümler, grafik alanında görüntülenmiyor, sağ üst köşedeki dişli simgesine tıklayın. İlgilendiğiniz model performansı ölçümlerini işaretli olduğundan emin olun. Ölçüm grafikleri aşağıda tabloda de görüntülenir. Bu tablo yeniden özelleştirilebilir üzerinde dişli simgesine tıklayarak olabilir. Bir performans ölçümü tarafından en iyi modeli belirleyin kullanılarak tablo sıralanır. Tahmini performans dönemden nasıl değişeceğini görmeniz ilgilenen *t + 1* için *t + 6*, tablonun modelde girdiye tıklayın. Altında gösterilen MAPE görüntüleyen grafikleri, tahmin dönemi boyunca MPE ve MdRAE ölçümleri *görselleştirmeler*.

Tahmin modellerini değerlendirirken, çıkış Öngörüler görselleştirmek çok kullanışlı olabilir. Bu, üretilen tahmin gerçekçi görünür olup olmadığını belirlemek için veri uzmanı yardımcı olur. Örneğin, hava durumu tahminini kötü belirli süreler uyguluyorsa, hava durumu tahminini sorunlarını tanımlamak için de yardımcı olabilir. `9-forecast-output-exploration.ipynb` Not Defteri, test veri kümesi için oluşturulan tahminlerin görselleştirmeler üretir.

### <a name="4-deployment"></a>4. Dağıtım

En iyi modeli dağıtarak gerçek zamanlı web hizmeti olarak kullanıma hazır hale getirdiniz. Bu web hizmetini, ardından yeni veriler kullanılabilir olduğunda isteğe bağlı tahminlerini oluşturmak için çağrılabilir. Bu senaryoda, yeni bir tahmini saatte sonraki bir saat içinde enerji talebini tahmin etmek için oluşturulması gereken. Bu görevi gerçekleştirmek için bir web hizmeti olabilir özellikleri bir dizi belirli bir saat süre girdi olarak alır ve tahmin edilen talep çıktı olarak verir.

Bu örnekte, bir web hizmeti için bir Windows 10 makinesi dağıtılır. Bu ayarla, yerel dağıtım için önkoşul adımlarını tamamladığınızdan emin olun [Başlangıç Kılavuzu](https://github.com/Azure/Machine-Learning-Operationalization/blob/master/documentation/getting-started.md) kullanıma hazır hale getirme CLI. Yerel ortamınıza ve model Yönetimi hesabı ayarladığınızda, çalıştırılır `10-deploy-model.ipynb` web hizmeti dağıtmak için Not Defteri.

## <a name="conclusion"></a>Sonuç

Bu örnek nasıl tahmin çözümü enerji talebini tahmin amacıyla bir uçtan uca zaman serisi oluşturulacağını gösterir. Bu örnekte incelediniz ilkelerin çoğunu diğer tahmin senaryoları ve sektörler için genişletilebilir.

Bu senaryoda, nasıl Azure Machine Learning Workbench bir veri Bilimcisi ölçüm günlüğe kaydetme özellikleri ve Jupyter not defteri ortamı gibi yararlı özellikleri ile gerçek dünya çözümleri geliştirmenize yardımcı olabilecek gösterilmektedir. Örnek ayrıca okuyucu nasıl bir modeli kullanıma hazır hale getirdiniz ve Azure Machine Learning operasyonel hale getirme CLI kullanılarak dağıtılan size yol gösterir. Dağıtıldıktan sonra geliştiriciler veya veri mühendisleri tahmin modelini daha geniş bir veri işlem hattı ile tümleştirmek web hizmeti API'si sağlar.
