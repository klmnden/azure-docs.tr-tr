---
title: Machine Learning için R dil için hızlı başlangıç Öğreticisi | Microsoft Docs
description: Hızlı bir şekilde tahmin bir çözüm oluşturmak için Azure Machine Learning Studio ile R dili ile çalışmaya başlamak için bu R programlama öğreticiyi kullanın.
keywords: Hızlı Başlangıç, r dil, r programlama dili, r programlama Öğreticisi
services: machine-learning
documentationcenter: ''
author: heatherbshapiro
ms.author: hshapiro
manager: hjerez
editor: cgronlun
ms.assetid: 99a3a0fd-b359-481a-b236-66868deccd96
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/06/2017
ms.openlocfilehash: 231d505e91fc036b30344e2fd9971db8ba2fdf05
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="quickstart-tutorial-for-the-r-programming-language-for-azure-machine-learning"></a>Azure Machine Learning için R programlama diline yönelik hızlı başlangıç öğreticisi

<!-- Stephen F Elston, Ph.D. -->

## <a name="introduction"></a>Giriş
Bu hızlı başlangıç Öğreticisi, Azure Machine Learning R programlama dilini kullanarak genişletme hemen başlamanıza yardımcı olur. Oluşturma, test ve Azure Machine Learning içinde R kod yürütmek için bu R programlama öğreticisini izleyin. Öğreticide çalışırken, Azure Machine Learning R dilini kullanarak eksiksiz bir tahmin çözüm oluşturur.  

Microsoft Azure Machine Learning'de pek çok güçlü machine learning ve veri işleme modüller içerir. Güçlü R dil en yaygın kullanılan analytics, açıklanan. R kullanarak Azure Machine Learning analizi ve veri işleme sonsuza dek, Genişletilebilir Bu birleşimi Azure Machine Learning dağıtım kolaylığı ve ölçeklenebilirlik esneklik ve r, derin analizi sağlar

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

### <a name="forecasting-and-the-dataset"></a>Tahmin ve veri kümesi
Tahmin yaygın olarak kullanılan ve oldukça kullanışlıdır analitik yöntemidir. Ortak macroeconomic değişkenleri tahmin için en iyi stok düzeylerini belirleme Mevsimlik öğelerinin satış tahmin etmeye gelen aralığını kullanır. Tahmin genellikle zaman serisi modelleri ile yapılır.

Zaman serisi veri değerlerin bir zaman dizine sahip verilerdir. Örneğin, her ay ya da her dakika süre dizininin normal, olabilir veya düzensiz. Zaman serisi modeli zaman serisi verilerine dayalıdır. Programlama dili R esnek framework ve kapsamlı analizi için zaman serisi veri içerir.

Bu Hızlı Başlangıç Kılavuzu'nda biz California Süt ürün çalışma ve veri fiyatlandırma. Bu veriler, birkaç süt ürünleri üretim ve sütlü fat, kıyaslama Emtia fiyat aylık bilgi içerir.

Bu makalede, R betiklerini birlikte kullanılan veri [burada indirilen][download]. Bu veriler University Wisconsin gelen bilgilerden başlangıçta oluşturulan http://future.aae.wisc.edu/tab/production.html.

### <a name="organization"></a>Kuruluş
Oluşturma, test ve Azure Machine Learning ortamında analizi ve veri işleme R kod yürütmek öğrenirken size çeşitli adımlarda ilerleyeceğini.  

* İlk biz Azure Machine Learning Studio ortamında R dili kullanılarak temelleri inceleyeceksiniz.
* Ardından çeşitli yönlerini g/ç için veri, R kodu ve Azure Machine Learning ortamında grafik tartışmak için ilerleme.
* Biz ardından tahmin çözümümüzdür ilk bölümü veri temizleme ve dönüştürme için kod oluşturarak oluşturmak.
* Hazırlanan bizim verilerle birkaç veri kümemizde değişkenlerin arasındaki bağıntıları analizini gerçekleştiririz.
* Son olarak, sütlü üretim için Mevsimlik zaman serisi tahmin modeli oluşturacağız.

## <a id="mlstudio"></a>Machine Learning Studio'da R dil ile etkileşim
Bu bölümde bazı temelleri Machine Learning Studio ortamında R programlama dili ile etkileşim alır. R dil özelleştirilmiş analizi ve veri işleme modülleri Azure Machine Learning ortamda oluşturmak için güçlü bir araç sağlar.

Rstudio'dan geliştirmek, test ve küçük ölçekte R kodda hata ayıklama için kullanır. Bu kodu ardından kesme ve yapıştırma içine bir [R betiği yürütün] [ execute-r-script] modülü Machine Learning Studio'da çalıştırılmaya hazır.  

### <a name="the-execute-r-script-module"></a>R betiği yürütün Modülü
Machine Learning Studio içinde içinde R komut dosyaları çalıştırılır [R betiği yürütün] [ execute-r-script] modülü. Bir örneği [R betiği yürütün] [ execute-r-script] Machine Learning Studio'da modülü, Şekil 1'de gösterilir.

 ![Programlama dili R: Machine Learning Studio'da seçili R betiği yürütün Modülü][1]

*Şekil 1. Seçili R betiği yürütün modülü gösteren Machine Learning Studio ortam.*

Şekil 1'e başvuran, anahtar bölümleri ile çalışmak için Machine Learning Studio ortamının bazıları bakalım [R betiği yürütün] [ execute-r-script] modülü.

* Denemeyi modülleri Orta bölmede gösterilir.
* Sağ bölmede üst kısmını görüntülemek ve R komut dosyalarınızı düzenlemek için bir pencere içerir.  
* Sağ bölmede alt kısmı bazı özellikleri gösterir [R betiği yürütün][execute-r-script]. Bu bölme üzerindeki uygun noktaları tıklayarak hata ve Çıktı günlükleri görüntüleyebilirsiniz.

Biz doğal olarak, tartışma [R betiği yürütün] [ execute-r-script] bu belgenin geri kalanında daha ayrıntılı.

Karmaşık R işlevleri ile çalışırken, t, düzenleme, test ve Rstudio'dan içinde hata ayıklama öneririz. Tüm yazılım geliştirme olduğu gibi kodunuzu artımlı olarak genişletmek ve küçük basit test çalışmalarını test. Ardından kesme ve işlevlerinizi R betiği penceresine yapıştırın [R betiği yürütün] [ execute-r-script] modülü. Bu yaklaşım, Rstudio'dan tümleşik geliştirme ortamı (IDE) ve Azure Machine Learning gücünü harekete geçirmek sağlar.  

#### <a name="execute-r-code"></a>R kodu yürütme
Bir R kodda [R betiği yürütün] [ execute-r-script] modülü tıklayarak denemeyi çalıştırdığınızda, yürütülecek **çalıştırmak** düğmesi. Yürütme tamamlandığında, bir onay işareti görünür [R betiği yürütün] [ execute-r-script] simgesi.

#### <a name="defensive-r-coding-for-azure-machine-learning"></a>Azure Machine Learning için savunma R kodlama
Azure Machine Learning kullanarak R kodu söyleyin, bir web hizmeti için geliştiriyorsanız, kesinlikle kodunuzu beklenmeyen veri giriş ve özel durumları nasıl ilgilenecektir planlamanız gerekir. Netlik korumak için ı çok denetleme veya özel durum işleme gösterilen kod örnekleri çoğunda in the way of eklemediniz. İlerlemeden gibi ancak ı size çeşitli işlevleri örnekleri R'ın özel durum işleme yeteneği kullanarak sunar.  

Daha eksiksiz bir R özel durum işleme işlenmesi gerekiyorsa, ı defteri listelenen Wickham tarafından uygulanabilir bölümleri okumanız önerilir [ek B - daha fazla bilgi](#appendixb).

#### <a name="debug-and-test-r-in-machine-learning-studio"></a>Hata ayıklama ve Machine Learning Studio'da R test
Yinelemek için ı sınamak ve Rstudio'dan içindeki küçük ölçekte R kodunuzun hatalarını ayıklamak öneririz. Ancak, burada gerekir R kodu sorunların izlemek için durumlar vardır [R betiği yürütün] [ execute-r-script] kendisi. Ayrıca, Machine Learning Studio'da sonuçlarınızı denetleyin iyi bir uygulamadır.

R kodunuzu ve Azure Machine Learning platformunda yürütme çıktısını öncelikle içeren içinde bulunur. Bazı ek bilgiler error.log görülür.  

Machine Learning Studio'da R kodunuzu çalıştırılırken bir hata meydana gelirse, eylem, ilk seyri error.log aramak için olmalıdır. Bu dosya, hatayı düzeltmek ve anlamanıza yardımcı olması için kullanışlı hata iletileri içerebilir. Error.log görüntülemek için tıklatın **hata günlüğü görüntüle** üzerinde **Özellikler bölmesinde** için [R betiği yürütün] [ execute-r-script] hata içeren.

Örneğin, aşağıdaki R kodu tanımlanmamış değişken bir y ile de çalıştırdım bir [R betiği yürütün] [ execute-r-script] Modülü:

    x <- 1.0
    z <- x + y

Bu kod yürütmek bir hata durumuna neden başarısız olur. Tıklayarak **hata günlüğü görüntüle** üzerinde **Özellikler bölmesinde** Şekil 2'de gösterilen görüntüler.

  ![Hata iletisi açılır][2]

*Şekil 2. Hata iletisi açılır.*

R hata iletisini görmek için içeren aramak ihtiyacımız gibi görünüyor. Tıklayın [R betiği yürütün] [ execute-r-script] tıklayın **içeren görüntülemek** üzerinde öğesi **Özellikler bölmesinde** sağındaki. Yeni bir tarayıcı penceresi açar ve ı aşağıdaki konulara bakın.

    [Critical]     Error: Error 0063: The following error occurred during evaluation of R script:
    ---------- Start of error message from R ----------
    object 'y' not found


    object 'y' not found
    ----------- End of error message from R -----------

Bu hata iletisi yok beklenmeyen durumları içerir ve açıkça sorun tanımlar.

R herhangi bir nesnenin değeri incelemek için bu değerleri içeren dosyaya yazdırabilirsiniz. Nesne değerlerini İnceleme kuralları temelde etkileşimli bir R oturumu olduğu gibi aynıdır. Örneğin, bir satıra bir değişken adı yazın, nesnenin değerini içeren dosyanın basılır.  

#### <a name="packages-in-machine-learning-studio"></a>Machine Learning Studio'da paketleri
Azure Machine Learning 350'den önceden yüklenmiş R dil paketleriyle birlikte gelir. Aşağıdaki kodu kullanabilirsiniz [R betiği yürütün] [ execute-r-script] önceden yüklenen paketler listesini almak için modülü.

    data.set <- data.frame(installed.packages())
    maml.mapOutputPort("data.set")

Şu anda bu kodu son satırının anlamadığınız okumaya devam edin. Bu belgenin geri kalanı Java'da Azure Machine Learning ortamında R kullanarak aşağıdakiler ele alınacaktır.

### <a name="introduction-to-rstudio"></a>Rstudio'dan giriş
R için yaygın olarak kullanılan bir IDE Rstudio'dan olduğu Rstudio'dan düzenleme, test ve bu Hızlı Başlangıç Kılavuzu'nda kullanılan R kodu bazıları hata ayıklama için kullanır. R kodu test edilmiş ve hazır olduğunda size kesip Rstudio'dan Düzenleyicisi'nden bir Machine Learning Studio'ya [R betiği yürütün] [ execute-r-script] modülü.  

Masaüstü makinenize yüklü R programlama dili yoksa, bunu şimdi yapın ı öneririz. Açık kaynak R dilinin ücretsiz indirme en kapsamlı R arşiv ağ (CRAN), kullanılabilir [ http://www.r-project.org/ ](http://www.r-project.org/). Windows, Mac OS ve Linux/UNIX için kullanılabilir yüklemeler vardır. Yakındaki bir yansıtma seçin ve yükleme yönergeleri izleyin. Ayrıca, CRAN bol miktarda yararlı analizi ve veri işleme paketleri içerir.

İçin Rstudio'dan yeniyseniz, indirin ve Masaüstü sürümünü yükleyin. Rstudio'dan yüklemeler için Windows, Mac OS ve Linux/UNIX konumunda bulabilirsiniz http://www.rstudio.com/products/RStudio/. Masaüstü makinenizde Rstudio'dan yüklemek için sağlanan yönergeleri izleyin.  

Rstudio'dan öğretici bir giriş şu adresten edinilebilir https://support.rstudio.com/hc/sections/200107586-Using-RStudio.

I içinde Rstudio'dan kullanarak bazı ek bilgiler sağlayan [ek A][appendixa].  

## <a id="scriptmodule"></a>R betiği yürütün modülü ve dışındaki veri al
Bu bölümde içine ve işyeri dışında veri nereden aşağıdakiler ele alınacaktır [R betiği yürütün] [ execute-r-script] modülü. Biz içinde ve dışında okuma çeşitli veri türlerini nasıl ele alınacağını incelenecek [R betiği yürütün] [ execute-r-script] modülü.

Tamamlamak için bu bölümde daha önce indirdiğiniz zip dosyasında kodudur.

### <a name="load-and-check-data-in-machine-learning-studio"></a>Yük ve veri Machine Learning Studio'da denetleyin
#### <a id="loading"></a>Veri kümesi yükleme
Biz yükleyerek başlayacak **csdairydata.csv** Azure Machine Learning Studio dosyasına.

* Azure Machine Learning Studio ortamınızı başlatın.
* Tıklayın **+ yeni** seçin ve ekranın sol alt köşesindeki **Dataset**.
* Seçin **yerel dosyadan**ve ardından **Gözat** dosyayı seçin.
* Seçtiğiniz emin olun **genel CSV dosyası (.csv) üstbilgiyle** veri kümesi türü.
* Onay işaretine tıklayın.
* Veri kümesi karşıya yüklendikten sonra tıklayarak yeni veri kümesi görmelisiniz **veri kümeleri** sekmesi.  

#### <a name="create-an-experiment"></a>Bir deneme oluşturma
Machine Learning Studio'da sahip olduğumuz bazı verileri, çözümleme yapmak için bir deneme oluşturmak gerekir.  

* Tıklayın **+ yeni** alt sol ve select **deneme**, ardından **boş deneme**.
* Seçme ve değiştirme, denemenizi adlandırabilirsiniz **deneme oluşturulan...**  sayfanın üst kısmındaki başlığı. Örneğin, kendisine değiştirme **CA günlük analizi**.
* Deneme sayfanın sol tarafta genişletin **kaydedilen veri kümeleri**ve ardından **My veri kümeleri**. Görmeniz gerekir **cadairydata.csv** daha önce yüklediğiniz.
* Sürükleme ve bırakma **csdairydata.csv dataset** deneme üzerine.
* İçinde **arama öğeleri denemeler** kutusunu sol bölmede, türü üst kısmında [R betiği yürütün][execute-r-script]. Arama listede modülü görürsünüz.
* Sürükleme ve bırakma [R betiği yürütün] [ execute-r-script] , palet modülü.  
* Çıkışına bağlayın **csdairydata.csv dataset** soldaki giriş (**Dataset1**), [R betiği yürütün][execute-r-script].
* **'Kaydet' tıklamayı unutmayın!**  

Bu noktada denemenizi Şekil 3 gibi görünmelidir.

![CA günlük analizi denemeler veri kümesi ve R betiği yürütün Modülü][3]

*Şekil 3. CA günlük analizi veri kümesi ve R betiği yürütün modülü ile deneyin.*

#### <a name="check-on-the-data"></a>Veriyi denetle
Şimdi bizim deneme yüklemiş olduğunuz veri bir görünüme sahiptir. Denemesinde çıktısını tıklatın **cadairydata.csv dataset** seçip **görselleştirmek**. Şekil 4 gibi bir şey görmeniz gerekir.  

![Cadairydata.csv dataset özeti][4]

*Şekil 4. Cadairydata.csv dataset özeti.*

Bu görünümde çok sayıda yararlı bilgiler bakın. Bu veri kümesinin ilk birkaç satır görebiliriz. Biz bir sütun seçerseniz, istatistikleri bölüm sütunu hakkında daha fazla bilgi gösterir. Örneğin, özellik türü satır bize Azure Machine Learning Studio atanmış bir sütun için hangi veri türlerini gösterir. Herhangi bir önemli iş yapmak başlamadan önce bu gibi hızlı bir bakış sahip bir iyi sağlamlık olup olmadığını denetler.

### <a name="first-r-script"></a>İlk R betiği
Azure Machine Learning Studio'da deneme ilk basit bir R betiği oluşturalım. I oluşturduktan ve aşağıdaki komut dosyası içinde Rstudio'dan test.  

    ## Only one of the following two lines should be used
    ## If running in Machine Learning Studio, use the first line with maml.mapInputPort()
    ## If in RStudio, use the second line with read.csv()
    cadairydata <- maml.mapInputPort(1)
    # cadairydata  <- read.csv("cadairydata.csv", header = TRUE, stringsAsFactors = FALSE)
    str(cadairydata)
    pairs(~ Cotagecheese.Prod + Icecream.Prod + Milk.Prod + N.CA.Fat.Price, data = cadairydata)
    ## The following line should be executed only when running in
    ## Azure Machine Learning Studio
    maml.mapOutputPort('cadairydata')

Şimdi bu komut dosyası Azure Machine Learning Studio transfer etmeniz. Yalnızca kesin ve yapıştırın. Ancak, bu durumda, ı my R betiği bir zip dosyası aktarın.

### <a name="data-input-to-the-execute-r-script-module"></a>R betiği yürütün modülü veri girişi
Şimdi girişleri göz sahip [R betiği yürütün] [ execute-r-script] modülü. Bu örnekte, biz California Süt verileri okur [R betiği yürütün] [ execute-r-script] modülü.  

Üç olası girdileri vardır [R betiği yürütün] [ execute-r-script] modülü. Herhangi bir ya da tüm bu girdi, uygulamaya bağlı olarak kullanabilir. Hiçbir girdi hiç alır bir R betiği kullanmak mükemmel uygun olur.  

Her soldan sağa giderek bu girdi bakalım. İmleç giriş yerleştirme ve araç ipucu okuma, her girdi adlarını görebilirsiniz.  

#### <a name="script-bundle"></a>Komut dosyası paket
Komut dosyası paket giriş sağlayan bir zip dosyasına içeriğini geçirmeniz [R betiği yürütün] [ execute-r-script] modülü. R kodunuza ZIP dosyasının içeriğini okumak için aşağıdaki komutlardan birini kullanabilirsiniz.

    source("src/yourfile.R") # Reads a zipped R script
    load("src/yourData.rdata") # Reads a zipped R data file

> [!NOTE]
> Azure Machine Learning değerlendirir zip dosyalarında src varsa gibi / dizini ve bu nedenle, dosya adları bu dizin adı ile önek gerekir. Örneğin, dosyaları zip içeriyorsa, `yourfile.R` ve `yourData.rdata` zip kökünde olarak çözülmesi `src/yourfile.R` ve `src/yourData.rdata` kullanırken `source` ve `load`.
> 
> 

Biz zaten yükleme kümelerinde ele [dataset yüklenirken](#loading). Oluşturulan ve önceki bölümde gösterilen R betiği test sonra aşağıdakileri yapın:

1. R betiği içine Kaydet bir. R dosyası. My komut dosyası "simpleplot. çağırın "R". Aşağıda, içeriği verilmiştir.
   
        ## Only one of the following two lines should be used
        ## If running in Machine Learning Studio, use the first line with maml.mapInputPort()
        ## If in RStudio, use the second line with read.csv()
        cadairydata <- maml.mapInputPort(1)
        # cadairydata  <- read.csv("cadairydata.csv", header = TRUE, stringsAsFactors = FALSE)
        str(cadairydata)
        pairs(~ Cotagecheese.Prod + Icecream.Prod + Milk.Prod + N.CA.Fat.Price, data = cadairydata)
        ## The following line should be executed only when running in
        ## Azure Machine Learning Studio
        maml.mapOutputPort('cadairydata')
2. Zip dosyası oluşturun ve komut dosyanızı Bu zip dosyasına kopyalayın. Windows üzerinde dosyasını sağ tıklatın ve seçin **göndermek**ve ardından **sıkıştırılmış klasörü**. Bu "simpleplot. içeren yeni bir zip dosyası oluşturur R"dosyası.
3. Dosyanıza ekleyin **veri kümeleri** Machine Learning Studio'da türü olarak belirterek **zip**. Zip dosyası, veri kümelerinde görmelisiniz.
4. Zip dosyasından sürükleyip **veri kümeleri** üzerine **ML Studio tuvale**.
5. Çıkışına bağlayın **zip veri** simgesine **betik paket** , giriş [R betiği yürütün] [ execute-r-script] modülü.
6. Tür `source()` zip dosyası adınızı için kod penceresine işleviyle [R betiği yürütün] [ execute-r-script] modülü. My durumda yazdım `source("src/simpleplot.R")`.  
7. Tıklattığınız emin olun **kaydetmek**.

Bu adımları tamamlandıktan sonra [R betiği yürütün] [ execute-r-script] modülü yürütülecek R betiği zip dosyasında deneme çalıştırdığınızda. Bu noktada denemenizi Şekil 5 gibi görünmelidir.

![Daraltılmış R betiği kullanmayı deneyin][6]

*Şekil 5. Daraltılmış R betiği kullanmayı deneyin.*

#### <a name="dataset1"></a>DataSet1
Dataset1 giriş kullanarak R kodunuzu dikdörtgen bir veri tablosu geçirebilirsiniz. Bizim basit bir komut dosyası içinde `maml.mapInputPort(1)` işlevi, bağlantı noktası 1 verileri okur. Bu veriler daha sonra kodunuzda dataframe değişken adı atanır. Bizim basit betik kodu, ilk satırının atama gerçekleştirir.

    cadairydata <- maml.mapInputPort(1)

Tıklayarak denemenizin yürütme **çalıştırmak** düğmesi. Yürütme sona erdiğinde, tıklayın [R betiği yürütün] [ execute-r-script] modülü ve ardından **çıkış Günlüğü Görüntüle** Özellikler bölmesi. Yeni bir sayfa içeren dosyanın içeriğini gösteren tarayıcınızda görüntülenmesi gerekir. Aşağı kaydırın, aşağıdakine benzer bir şey görmeniz gerekir.

    [ModuleOutput] InputDataStructure
    [ModuleOutput]
    [ModuleOutput] {
    [ModuleOutput]  "InputName":Dataset1
    [ModuleOutput]  "Rows":228
    [ModuleOutput]  "Cols":9
    [ModuleOutput]  "ColumnTypes":System.Int32,3,System.Double,5,System.String,1
    [ModuleOutput] }

Sayfa aşağı uzağına aşağıdakine benzer görünecektir sütunları hakkında daha ayrıntılı bilgi olur.

    [ModuleOutput] [1] "Loading variable port1..."
    [ModuleOutput]
    [ModuleOutput] 'data.frame':    228 obs. of  9 variables:
    [ModuleOutput]
    [ModuleOutput]  $ Column 0         : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput]
    [ModuleOutput]  $ Year.Month       : num  1995 1995 1995 1995 1995 ...
    [ModuleOutput]
    [ModuleOutput]  $ Month.Number     : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput]
    [ModuleOutput]  $ Year             : int  1995 1995 1995 1995 1995 1995 1995 1995 1995 1995 ...
    [ModuleOutput]
    [ModuleOutput]  $ Month            : chr  "Jan" "Feb" "Mar" "Apr" ...
    [ModuleOutput]
    [ModuleOutput]  $ Cotagecheese.Prod: num  4.37 3.69 4.54 4.28 4.47 ...
    [ModuleOutput]
    [ModuleOutput]  $ Icecream.Prod    : num  51.6 56.1 68.5 65.7 73.7 ...
    [ModuleOutput]
    [ModuleOutput]  $ Milk.Prod        : num  2.11 1.93 2.16 2.13 2.23 ...
    [ModuleOutput]
    [ModuleOutput]  $ N.CA.Fat.Price   : num  0.98 0.892 0.892 0.897 0.897 ...

Bu sonuçları 228 gözlemleri ve dataframe 9 sütunlarında beklendiği gibi genellikle olur. Sütun adları, R veri türü ve her sütunun bir örnek görebiliriz.

> [!NOTE]
> Bu aynı çıktılar R aygıt çıktısından rahat kullanılabilir [R betiği yürütün] [ execute-r-script] modülü. Çıkışları aşağıdakiler ele alınacaktır [R betiği yürütün] [ execute-r-script] sonraki bölümde modülü.  
> 
> 

#### <a name="dataset2"></a>Dataset2
Dataset2 giriş davranışını, Dataset1 aynıdır. Bu girdi kullanarak R kodunuza ikinci bir dikdörtgen tablo veri geçirebilirsiniz. İşlev `maml.mapInputPort(2)`, 2 bağımsız değişkeniyle bu veri iletmek için kullanılır.  

### <a name="execute-r-script-outputs"></a>R betiği çıkışları yürütme
#### <a name="output-a-dataframe"></a>Bir dataframe çıkış
Kullanarak sonuç Dataset1 bağlantı noktası üzerinden dikdörtgen tablo olarak bir R dataframe içeriğini çıkarabilirsiniz `maml.mapOutputPort()` işlevi. Bizim basit R betiği bu aşağıdaki satırı tarafından gerçekleştirilir.

    maml.mapOutputPort('cadairydata')

Denemeyi çalıştırdıktan sonra sonuç Dataset1 çıkış bağlantı noktasına tıklayın ve ardından tıklatın **Görselleştir**. Şekil 6 gibi bir şey görmeniz gerekir.

![Çıktı California Süt veri Görselleştirme][7]

*Şekil 6. Çıktı California Süt veri görselleştirme.*

Tam olarak beklenen şekilde bu çıkış girişi için aynı arar.  

### <a name="r-device-output"></a>R cihaz çıktısı
Cihaz çıktısı [R betiği yürütün] [ execute-r-script] modülü iletileri ve grafik çıktısı içerir. Her iki standart çıktı ve standart hata iletilerden R R aygıt çıkış bağlantı noktasına gönderilir.  

R aygıt çıktı görüntülemek için bağlantı noktası üzerinde tıklatın ve ardından **Görselleştir**. Biz, standart çıktı ve standart hata Şekil 7'de R betikten bakın.

![Standart çıktı ve standart hata R aygıtı bağlantı noktasından][8]

*Şekil 7. Standart çıktı ve standart hata R aygıtı bağlantı noktasından.*

Bkz: Şekil 8'deki bizim R betiği grafik çıktısı biz kaydırma.  

![R aygıtı bağlantı noktası grafik çıktısı][9]

*Şekil 8. R aygıtı bağlantı noktasından çıktı grafik.*  

## <a id="filtering"></a>Verileri filtreleme ve dönüştürme
Bu bölümde biz bazı temel verileri filtreleme ve California Süt veriler üzerinde dönüştürme işlemleri gerçekleştirir. Bu bölümde sonuna analitik bir model oluşturmak için uygun bir biçim biz veri sahip olur.  

Daha açık belirtmek gerekirse, bu bölümde birkaç ortak veri temizleme ve dönüştürme görevleri gerçekleştiririz: tür dönüştürme, yeni hesaplanan sütunlar ekleme dataframes üzerinde filtreleme ve değer dönüşümler. Bu arka plan gerçek dünyadaki sorunları karşılaştı pek çok Çeşitleme uğraşmanız yardımcı olmalıdır.

Bu bölüm için tam R kod daha önce indirdiğiniz zip dosyasında kullanılabilir.

### <a name="type-transformations"></a>Tür dönüştürmeleri
Biz R koda California Süt verileri okuyabilir göre [R betiği yürütün] [ execute-r-script] modül ihtiyacımız sütunlardaki veriler hedeflenen türünü ve biçimini olduğundan emin olmak.  

R birinden diğerine gerektiği üzere bu veri türleri yüklenen anlamına gelir dinamik olarak yazılan bir dildir. R atomik veri türlerinde sayısal, mantıksal ve karakter içerir. Faktörü türü sıkı şekilde kategorik veri depolamak için kullanılır. İçindeki başvuruların veri türleri hakkında daha fazla bilgi bulabilirsiniz [ek B - daha fazla bilgi](#appendixb).

Tablo veri R bir dış kaynaktan içine okunduğunda sütunlardaki sonuç türleri denetlemek için iyi bir fikir her zaman olduğu. Bir sütun türü karakterinin isteyebilirsiniz, ancak çoğu durumda bu faktör olarak (veya tersi) görünür. Diğer durumlarda bir sütun olması düşündüğünüz sayısal karakter verileri tarafından örneğin temsil edilir bir kayan olarak 1,23 yerine '1,23' nokta sayısı.  

Neyse ki, eşleme mümkün olduğu sürece bir türünü diğerine dönüştürme kolaydır. Örneğin, sayısal bir değer 'Nevada'ya' dönüştürülemiyor, ancak bir faktör (kategorik değişken) dönüştürebilirsiniz. Başka bir örnek olarak, bir karakter '1' ya da bir faktör sayısal 1 dönüştürebilirsiniz.  

Herhangi bir bu dönüştürme için söz dizimi basittir: `as.datatype()`. Bu tür dönüştürme işlevleri arasında şunlar yer alır.

* `as.numeric()`
* `as.character()`
* `as.logical()`
* `as.factor()`

Önceki bölümde giriyoruz sütunların veri türlerini bakarak: türü sayısal, türü karakteri olan'Month ' etiketli sütun dışındaki tüm sütunları olan. Şimdi bu bir faktör dönüştürün ve test sonuçları.  

Scatterplot matris oluşturulur ve 'Month' sütunu için bir faktör dönüştürme bir satır eklenir satır silindi. Denememe t yalnızca R kod kesip kod penceresine [R betiği yürütün] [ execute-r-script] modülü. Ayrıca ZIP dosyasını güncelleştirin ve Azure Machine Learning Studio karşıya ancak bu birkaç adım alır.  

    ## Only one of the following two lines should be used
    ## If running in Machine Learning Studio, use the first line with maml.mapInputPort()
    ## If in RStudio, use the second line with read.csv()
    cadairydata <- maml.mapInputPort(1)
    # cadairydata  <- read.csv("cadairydata.csv", header = TRUE, stringsAsFactors = FALSE)
    ## Ensure the coding is consistent and convert column to a factor
    cadairydata$Month <- as.factor(cadairydata$Month)
    str(cadairydata) # Check the result
    ## The following line should be executed only when running in
    ## Azure Machine Learning Studio
    maml.mapOutputPort('cadairydata')

Şimdi bu kodu yürütür ve R betiği için çıktı günlüğüne bakın. Şekil 9'ilgili verilerin günlüğünden gösterilir.

    [ModuleOutput] [1] "Loading variable port1..."
    [ModuleOutput] 
    [ModuleOutput] 'data.frame':    228 obs. of  9 variables:
    [ModuleOutput] 
    [ModuleOutput]  $ Column 0         : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Year.Month       : num  1995 1995 1995 1995 1995 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Number     : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Year             : int  1995 1995 1995 1995 1995 1995 1995 1995 1995 1995 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month            : Factor w/ 14 levels "Apr","April",..: 6 5 9 1 11 8 7 3 14 13 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Cotagecheese.Prod: num  4.37 3.69 4.54 4.28 4.47 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Icecream.Prod    : num  51.6 56.1 68.5 65.7 73.7 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Milk.Prod        : num  2.11 1.93 2.16 2.13 2.23 ...
    [ModuleOutput] 
    [ModuleOutput]  $ N.CA.Fat.Price   : num  0.98 0.892 0.892 0.897 0.897 ...
    [ModuleOutput] 
    [ModuleOutput] [1] "Saving variable  cadairydata  ..."
    [ModuleOutput] 
    [ModuleOutput] [1] "Saving the following item(s):  .maml.oport1"

*Şekil 9. Bir faktör değişkeniyle dataframe özeti.*

Türü ay için şimdi yazması gerekir '**14 düzeyleri içeren faktörü**'. Yılın yalnızca 12 ay olduğundan bu bir sorundur. Ayrıca, görmek için kontrol edebilirsiniz türünde **Görselleştir** sonuç kümesi bağlantı noktası olduğundan '**Categorical**'.

'Month' sütun sistematik olarak kodlanmış değil, sorunudur. Bazı durumlarda bir ay Nisan adı verilir ve diğerleri Apr kısaltılır. 3 karakter dizesine kırpma tarafından size bu sorunu çözebilirsiniz. Kod satırı aşağıdaki gibi görünür:

    ## Ensure the coding is consistent and convert column to a factor
    cadairydata$Month <- as.factor(substr(cadairydata$Month, 1, 3))

Denemeyi yeniden çalıştırın ve çıktı günlüğüne bakın. Şekil 10'beklenen sonuçları gösterilmektedir.  

    [ModuleOutput] [1] "Loading variable port1..."
    [ModuleOutput] 
    [ModuleOutput] 'data.frame':    228 obs. of  9 variables:
    [ModuleOutput] 
    [ModuleOutput]  $ Column 0         : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Year.Month       : num  1995 1995 1995 1995 1995 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Number     : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Year             : int  1995 1995 1995 1995 1995 1995 1995 1995 1995 1995 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month            : Factor w/ 12 levels "Apr","Aug","Dec",..: 5 4 8 1 9 7 6 2 12 11 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Cotagecheese.Prod: num  4.37 3.69 4.54 4.28 4.47 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Icecream.Prod    : num  51.6 56.1 68.5 65.7 73.7 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Milk.Prod        : num  2.11 1.93 2.16 2.13 2.23 ...
    [ModuleOutput] 
    [ModuleOutput]  $ N.CA.Fat.Price   : num  0.98 0.892 0.892 0.897 0.897 ...
    [ModuleOutput] 
    [ModuleOutput] [1] "Saving variable  cadairydata  ..."
    [ModuleOutput] 
    [ModuleOutput] [1] "Saving the following item(s):  .maml.oport1"

*Şekil 10. Doğru sayıda faktörü düzeyleri ile dataframe özeti.*

Bizim faktörü değişkeni istenen 12 düzeyleri artık sahiptir.

### <a name="basic-data-frame-filtering"></a>Çerçeve temel verileri filtreleme
R dataframes güçlü filtreleme yetenekleri için destek. Veri kümeleri, satır veya sütun üzerinde mantıksal filtreleri kullanarak alt kümelenmiş olabilir. Çoğu durumda, karmaşık filtre ölçütlerini gerekli olacaktır. İçindeki başvuruların [ek B - daha fazla bilgi](#appendixb) dataframes filtreleme kapsamlı örnekleri içerir.  

Bir bit yoktur filtre biz kümemize üzerinde yapmalısınız. Cadairydata dataframe sütunlarında bakarsanız, iki gereksiz sütunları görürsünüz. İlk sütun yalnızca çok kullanışlı değil bir satır numarası tutar. İkinci sütun, Year.Month, yedek bilgiler içerir. Aşağıdaki R kodu kullanarak Biz bu sütunları kolayca dışlayabilirsiniz.

> [!NOTE]
> Şu andan itibaren bu bölümde, ı yalnızca, ı ekleme ek kod gösterecektir [R betiği yürütün] [ execute-r-script] modülü. Her yeni satır ekleyeceğim **önce** `str()` işlevi. I Azure Machine Learning Studio'da my sonuçları doğrulamak için bu işlevi kullanın.
> 
> 

R kodumu aşağıdaki satırı eklediğim [R betiği yürütün] [ execute-r-script] modülü.

    # Remove two columns we do not need
    cadairydata <- cadairydata[, c(-1, -2)]

Denemenizde bu kodu çalıştırmak ve çıkış günlüğü sonucundan denetleyin. Bu sonuçları Şekil 11'de gösterilmiştir.

    [ModuleOutput] [1] "Loading variable port1..."
    [ModuleOutput] 
    [ModuleOutput] 'data.frame':    228 obs. of  7 variables:
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Number     : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Year             : int  1995 1995 1995 1995 1995 1995 1995 1995 1995 1995 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month            : Factor w/ 12 levels "Apr","Aug","Dec",..: 5 4 8 1 9 7 6 2 12 11 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Cotagecheese.Prod: num  4.37 3.69 4.54 4.28 4.47 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Icecream.Prod    : num  51.6 56.1 68.5 65.7 73.7 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Milk.Prod        : num  2.11 1.93 2.16 2.13 2.23 ...
    [ModuleOutput] 
    [ModuleOutput]  $ N.CA.Fat.Price   : num  0.98 0.892 0.892 0.897 0.897 ...
    [ModuleOutput] 
    [ModuleOutput] [1] "Saving variable  cadairydata  ..."
    [ModuleOutput] 
    [ModuleOutput] [1] "Saving the following item(s):  .maml.oport1"

*Şekil 11. İki sütunlarla dataframe özetini kaldırıldı.*

Güzel bir haberimiz var! Biz beklenen sonuçları alın.

### <a name="add-a-new-column"></a>Yeni bir sütun ekleyin
Zaman serisi modelleri oluşturmak için zaman serisi başladığından bu yana ay içeren bir sütun için uygun olacaktır. Yeni bir sütun 'Month.Count' oluşturacağız.

Biz bizim ilk basit işlevi oluşturacak kodu düzenlenmesine yardımcı olmak için `num.month()`. Biz sonra bu işlevi dataframe yeni bir sütun oluşturmak için geçerli olur. Yeni kod aşağıdaki gibidir.

    ## Create a new column with the month count
    ## Function to find the number of months from the first
    ## month of the time series
    num.month <- function(Year, Month) {
      ## Find the starting year
      min.year  <- min(Year)

      ## Compute the number of months from the start of the time series
      12 * (Year - min.year) + Month - 1
    }

    ## Compute the new column for the dataframe
    cadairydata$Month.Count <- num.month(cadairydata$Year, cadairydata$Month.Number)

Şimdi güncelleştirilmiş denemeyi çalıştırın ve sonuçları görmek için çıkış günlüğü kullanın. Şekil 12'de bu sonuçları gösterilmektedir.

    [ModuleOutput] [1] "Loading variable port1..."
    [ModuleOutput] 
    [ModuleOutput] 'data.frame':    228 obs. of  8 variables:
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Number     : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Year             : int  1995 1995 1995 1995 1995 1995 1995 1995 1995 1995 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month            : Factor w/ 12 levels "Apr","Aug","Dec",..: 5 4 8 1 9 7 6 2 12 11 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Cotagecheese.Prod: num  4.37 3.69 4.54 4.28 4.47 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Icecream.Prod    : num  51.6 56.1 68.5 65.7 73.7 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Milk.Prod        : num  2.11 1.93 2.16 2.13 2.23 ...
    [ModuleOutput] 
    [ModuleOutput]  $ N.CA.Fat.Price   : num  0.98 0.892 0.892 0.897 0.897 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Count      : num  0 1 2 3 4 5 6 7 8 9 ...
    [ModuleOutput] 
    [ModuleOutput] [1] "Saving variable  cadairydata  ..."
    [ModuleOutput] 
    [ModuleOutput] [1] "Saving the following item(s):  .maml.oport1"

*Şekil 12. Ek sütunla dataframe özeti.*

Her şey çalışıyor gibi görünüyor. Bizim dataframe beklenen değerlerle yeni bir sütun sahibiz.

### <a name="value-transformations"></a>Değer dönüşümler
Bu bölümde biz bizim dataframe sütunlarının bazıları değerlere bazı basit dönüşümleri gerçekleştirir. R dil neredeyse rastgele değeri dönüşümleri destekler. İçindeki başvuruların [ek B - daha fazla bilgi](#appendixb) kapsamlı örnekler içeriyor.

Bizim dataframe özetlerini değerlerde bakarsanız tek bir şey burada görmeniz gerekir. Daha fazla dondurma sütlü daha İzmir'de oluşturulur? Bu hiçbir mantıklı gibi Hayır, Elbette bize bazıları için dondurma lovers olgunun üzücü olmayabilir. Birimleri farklıdır. Fiyat birimleri BİZE pound, olan 1 M birimlerinde ABD pound, dondurma 1.000 birimlerinde BİZE galon ve cottage Peynir 1.000 ABD pound birimlerinde sütlü olur. Dondurma galon başına yaklaşık 6.5 pound hafiftir varsayıldığında, biz tüm eşit, biriminde 1.000 pound; bu nedenle bu değerleri dönüştürmek için çarpma kolayca yapabilirsiniz.

Tahmin modelimiz için çarpma modeli eğilim ve bu verileri Mevsimlik düzeltilmesi için kullanırız. Bir günlük dönüştürmesi bize bu işlem basitleştirme doğrusal bir model kullanılacak sağlar. Biz çarpanı burada uygulanan aynı işlev günlük dönüşümünde uygulayabilirsiniz.

Aşağıdaki kodda ı yeni bir işlev tanımlayın `log.transform()`ve sayısal değerleri içeren satırları uygulayın. R `Map()` işlevi uygulamak için kullanılan `log.transform()` dataframe seçilen sütunlarda işlevi. `Map()` benzer `apply()` ancak işlevi bağımsız değişken birden fazla listesini sağlar. Çarpanları listesini ikinci bağımsız değişkeni sağlayan Not `log.transform()` işlevi. `na.omit()` İşlevi değil sahibiz eksik veya tanımsız değerler dataframe emin olmak için temizleme biraz kullanılır.

    log.transform <- function(invec, multiplier = 1) {
      ## Function for the transformation, which is the log
      ## of the input value times a multiplier

      warningmessages <- c("ERROR: Non-numeric argument encountered in function log.transform",
                           "ERROR: Arguments to function log.transform must be greate than zero",
                           "ERROR: Aggurment multiplier to funcition log.transform must be a scaler",
                           "ERROR: Invalid time seies value encountered in function log.transform"
                           )

      ## Check the input arguments
      if(!is.numeric(invec) | !is.numeric(multiplier)) {warning(warningmessages[1]); return(NA)}  
      if(any(invec < 0.0) | any(multiplier < 0.0)) {warning(warningmessages[2]); return(NA)}
      if(length(multiplier) != 1) {{warning(warningmessages[3]); return(NA)}}

      ## Wrap the multiplication in tryCatch
      ## If there is an exception, print the warningmessage to
      ## standard error and return NA
      tryCatch(log(multiplier * invec),
               error = function(e){warning(warningmessages[4]); NA})
    }


    ## Apply the transformation function to the 4 columns
    ## of the dataframe with production data
    multipliers  <- list(1.0, 6.5, 1000.0, 1000.0)
    cadairydata[, 4:7] <- Map(log.transform, cadairydata[, 4:7], multipliers)

    ## Get rid of any rows with NA values
    cadairydata <- na.omit(cadairydata)  

Oldukça bit oluşmasını yoktur `log.transform()` işlevi. Bu kod çoğunu bağımsız değişkenleri ile ilgili olası sorunları veya hala hesaplamalar sırasında ortaya çıkabilecek özel durumlar ilgilenmek için denetleniyor. Bu kod yalnızca birkaç satırlık gerçekte hesaplamalar yapın.

Savunma programlama işleme devam etmesini engelleyen tek bir işlev hatasını önlemek için hedefidir. Uzun süre çalışan çözümleme ani bir başarısızlığını kullanıcılar için oldukça can sıkıcı olabilir. Bu durumu önlemek için varsayılan dönüş değerleri aşağı akış işleme zarar sınırlar seçilmelidir. Bir ileti ayrıca bir şeyler yanlış geçtiğini kullanıcıları uyarmak için oluşturulur.

R savunma programlamada için kullanılmaz, bu kod bir bit bunaltıcı görünebilir. I temel adımlarda size yol gösterir:

1. Vektör dört iletilerinin tanımlanır. Bu iletiler, bazı şu kodla oluşan özel durumlar ve olası hatalar hakkında bilgi iletişim kurmak için kullanılır.
2. I BELİRTİLMEYEN değeri her bir olay döndürür. Daha az yan etkileri olabilir diğer birçok olasılık vardır. Vektör sıfır değerini veya özgün giriş vektör, örneğin dönüş.
3. Denetimleri işleve bağımsız değişkenler üzerinde gerçekleştirilir. Her durumda, bir hata algılandığında, varsayılan bir değer döndürülür ve bir ileti tarafından üretilen `warning()` işlevi. Kullanıyorum `warning()` yerine `stop()` tam olarak neyi önlemek çalışıyorum yürütme, ikincisi sonlandıracak gibi. I bu kodu bu durumda olduğu gibi bir yordam stili karmaşık ve belirsiz seemed işlevsel bir yaklaşım yazılmış olduğunu unutmayın.
4. Günlük hesaplamalar kaydırılır `tryCatch()` böylece özel durumları işleme ani bir durmasına neden olmaz. Olmadan `tryCatch()` hataların çoğu bunu yapan bir durdurma sinyali R işlevleri sonucu olarak gerçekleşti.

Bu R kodu denemenizde yürütün ve çıktılar göz içeren dosyanız. Şekil 13'te gösterildiği gibi şimdi günlüğünde, dört sütun dönüştürülmüş değerlerini görürsünüz.

    [ModuleOutput] [1] "Loading variable port1..."
    [ModuleOutput] 
    [ModuleOutput] 'data.frame':    228 obs. of  8 variables:
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Number     : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Year             : int  1995 1995 1995 1995 1995 1995 1995 1995 1995 1995 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month            : Factor w/ 12 levels "Apr","Aug","Dec",..: 5 4 8 1 9 7 6 2 12 11 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Cotagecheese.Prod: num  1.47 1.31 1.51 1.45 1.5 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Icecream.Prod    : num  5.82 5.9 6.1 6.06 6.17 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Milk.Prod        : num  7.66 7.57 7.68 7.66 7.71 ...
    [ModuleOutput] 
    [ModuleOutput]  $ N.CA.Fat.Price   : num  6.89 6.79 6.79 6.8 6.8 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Count      : num  0 1 2 3 4 5 6 7 8 9 ...
    [ModuleOutput] 
    [ModuleOutput] [1] "Saving variable  cadairydata  ..."
    [ModuleOutput] 
    [ModuleOutput] [1] "Saving the following item(s):  .maml.oport1"

*Şekil 13. Dataframe dönüştürülmüş değerleri özeti.*

Değerleri dönüştürülen bakın. Şimdi sütlü üretim büyük ölçüde şimdi günlük ölçekte arıyoruz, geri çağırma tüm diğer Süt ürün üretim aşıyor.

Bu noktada verilerimizi temizlenir ve biz bazı modelleme için hazır olursunuz. Sonuç veri kümesi çıktısı için Özet görselleştirme bakarak bizim [R betiği yürütün] [ execute-r-script] modülü, 'Month' sütundur 'Categorical' 12 benzersiz değerlerle yeniden, biz istediği gibi görürsünüz.

## <a id="timeseries"></a>Zaman serisi nesneleri ve bağıntı çözümleme
Bu bölümde biz birkaç temel R zaman serisi nesneleri keşfedin ve bazı değişkenler arasındaki bağıntıları analiz edin. Amacımız birkaç gecikmelere pairwise bağıntı bilgileri içeren bir dataframe çıkış olmaktır.

Tam R Bu bölümde daha önce indirdiğiniz zip dosyasında kodudur.

### <a name="time-series-objects-in-r"></a>R zaman serisi nesneleri
Yukarıda belirtildiği gibi zaman serisinin zaten zamanına göre dizine veri değerleri bir dizi olduğundan. R zaman serisi nesneleri oluşturmak ve zaman dizini yönetmek için kullanılır. Zaman serisi nesneleri kullanmanın birkaç avantajı vardır. Zaman serisi nesneleri nesneyi kapsüllenmiş zaman serisi dizini değerleri yönetme birçok ayrıntıları boş. Ayrıca, zaman serisinin nesneleri çizdirmek için çok zaman serisi yöntemlerini kullanmayı yazdırma, modelleme, vb. sağlar.

POSIXct zaman serisi sınıfı yaygın olarak kullanılır ve görece daha basittir. Bu zaman serisi, 1 Ocak 1970'ten dönem başından ölçüleri zaman sınıfı. Bu örnekte POSIXct zaman serisi nesneleri kullanacağız. Diğer yaygın olarak kullanılan R zaman serisi nesne sınıfları zoo ve xts, Genişletilebilir zaman serisi içerir.
<!-- Additional information on R time series objects is provided in the references in Section 5.7. [commenting because this section doesn't exist, even in the original] -->

### <a name="time-series-object-example"></a>Zaman serisi nesnesi örneği
Bizim örneğimizde ile başlayalım. Sürükleme ve bırakma bir **yeni** [R betiği yürütün] [ execute-r-script] denemenizi modüle. Var olan sonuç Dataset1 çıkış bağlantı noktasına bağlanmak [R betiği yürütün] [ execute-r-script] Dataset1 modülüne giriş bağlantı noktası yeni [R betiği yürütün] [ execute-r-script] modülü.

Biz örnek ilerledikçe t ilk örnekler için yaptığınız gibi bazı noktalarda ı yalnızca artımlı ek satırlar her adım R kodu gösterir.  

#### <a name="reading-the-dataframe"></a>Dataframe okuma
İlk adım olarak şimdi bir dataframe okuyun ve biz beklenen sonuçları elde emin olun. Aşağıdaki kod, iş yapmanız gerekir.

    # Comment the following if using RStudio
    cadairydata <- maml.mapInputPort(1)
    str(cadairydata) # Check the results

Şimdi, denemeyi çalıştırın. R betiği yürütün şekil günlük Şekil 14 gibi görünmelidir.

    [ModuleOutput] [1] "Loading variable port1..."
    [ModuleOutput] 
    [ModuleOutput] 'data.frame':    228 obs. of  8 variables:
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Number     : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Year             : int  1995 1995 1995 1995 1995 1995 1995 1995 1995 1995 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month            : Factor w/ 12 levels "Apr","Aug","Dec",..: 5 4 8 1 9 7 6 2 12 11 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Cotagecheese.Prod: num  1.47 1.31 1.51 1.45 1.5 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Icecream.Prod    : num  5.82 5.9 6.1 6.06 6.17 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Milk.Prod        : num  7.66 7.57 7.68 7.66 7.71 ...
    [ModuleOutput] 
    [ModuleOutput]  $ N.CA.Fat.Price   : num  6.89 6.79 6.79 6.8 6.8 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Count      : num  0 1 2 3 4 5 6 7 8 9 ...

*Şekil 14. R betiği yürütün modülünde dataframe özeti.*

Bu, beklenen türleri ve biçimde verilerdir. 'Month' sütun türü faktörü olduğunu ve beklenen sayısı sahip olduğunu unutmayın.

#### <a name="creating-a-time-series-object"></a>Zaman serisi nesnesi oluşturma
Bizim dataframe bir zaman serisi nesnesi eklemeniz gerekir. Geçerli kod POSIXct sınıfının yeni bir sütun ekler aşağıdaki ile değiştirin.

    # Comment the following if using RStudio
    cadairydata <- maml.mapInputPort(1)

    ## Create a new column as a POSIXct object
    Sys.setenv(TZ = "PST8PDT")
    cadairydata$Time <- as.POSIXct(strptime(paste(as.character(cadairydata$Year), "-", as.character(cadairydata$Month.Number), "-01 00:00:00", sep = ""), "%Y-%m-%d %H:%M:%S"))

    str(cadairydata) # Check the results

Şimdi, günlüğünü denetleyin. Şekil 15 gibi görünmelidir.

    [ModuleOutput] [1] "Loading variable port1..."
    [ModuleOutput] 
    [ModuleOutput] 'data.frame':    228 obs. of  9 variables:
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Number     : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Year             : int  1995 1995 1995 1995 1995 1995 1995 1995 1995 1995 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month            : Factor w/ 12 levels "Apr","Aug","Dec",..: 5 4 8 1 9 7 6 2 12 11 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Cotagecheese.Prod: num  1.47 1.31 1.51 1.45 1.5 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Icecream.Prod    : num  5.82 5.9 6.1 6.06 6.17 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Milk.Prod        : num  7.66 7.57 7.68 7.66 7.71 ...
    [ModuleOutput] 
    [ModuleOutput]  $ N.CA.Fat.Price   : num  6.89 6.79 6.79 6.8 6.8 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Count      : num  0 1 2 3 4 5 6 7 8 9 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Time             : POSIXct, format: "1995-01-01" "1995-02-01" ...

*Şekil 15. Bir zaman serisi nesnesiyle dataframe özeti.*

Yeni bir sütun aslında POSIXct sınıfının olan özeti görebiliriz.

### <a name="exploring-and-transforming-the-data"></a>Keşfetmek ve verileri dönüştürme
Bu veri kümesi değişkenlerde bazıları inceleyelim. Scatterplot matris hızlı bir bakış üretmek için iyi bir yoludur. Değiştirme `str()` aşağıdaki satırı önceki R koduyla işlevinde.

    pairs(~ Cotagecheese.Prod + Icecream.Prod + Milk.Prod + N.CA.Fat.Price, data = cadairydata, main = "Pairwise Scatterplots of dairy time series")

Bu kodu çalıştırmak ve ne olduğunu gözleyin. R aygıtı bağlantı noktasına üretilen çizim Şekil 16 gibi görünmelidir.

![Seçili değişkenleri Scatterplot Matrisi][17]

*Şekil 16. Seçili değişkenleri Scatterplot matrisi.*

Bu değişkenler ilişkileri bazı garip görünüşlü yapısı yoktur. Belki de bu verilerindeki eğilimleri ve biz değişkenleri standartlaşmış değil, olgu ortaya çıkar.

### <a name="correlation-analysis"></a>Bağıntı çözümleme
Bağıntı analizi yapmak için kimliğinizi XML'deki eğilimi hem değişkenleri standartlaştırmak gerekiyor. Biz yalnızca R kullanabilirsiniz `scale()` merkezleri hem değişkenleri ölçeklendirir işlevi. Bu işlev iyi daha hızlı çalışabilir. Ancak, savunma programing örneği r'de Göster istiyor

`ts.detrend()` Aşağıda gösterilen işlevi bu işlemlerin her ikisini de gerçekleştirir. Aşağıdaki iki kod satırı veri XML'deki eğilimi ve değerlerin standart hale getirme.

    ts.detrend <- function(ts, Time, min.length = 3){
      ## Function to de-trend and standardize a time series

      ## Define some messages if they are NULL  
      messages <- c('ERROR: ts.detrend requires arguments ts and Time to have the same length',
                    'ERROR: ts.detrend requires argument ts to be of type numeric',
                    paste('WARNING: ts.detrend has encountered a time series with length less than', as.character(min.length)),
                    'ERROR: ts.detrend has encountered a Time argument not of class POSIXct',
                    'ERROR: Detrend regression has failed in ts.detrend',
                    'ERROR: Exception occurred in ts.detrend while standardizing time series in function ts.detrend'
      )
      # Create a vector of zeros to return as a default in some cases
      zerovec  <- rep(length(ts), 0.0)

      # The input arguments are not of the same length, return ts and quit
      if(length(Time) != length(ts)) {warning(messages[1]); return(ts)}

      # If the ts is not numeric, just return a zero vector and quit
      if(!is.numeric(ts)) {warning(messages[2]); return(zerovec)}

      # If the ts is too short, just return it and quit
      if((ts.length <- length(ts)) < min.length) {warning(messages[3]); return(ts)}

      ## Check that the Time variable is of class POSIXct
      if(class(cadairydata$Time)[[1]] != "POSIXct") {warning(messages[4]); return(ts)}

      ## De-trend the time series by using a linear model
      ts.frame  <- data.frame(ts = ts, Time = Time)
      tryCatch({ts <- ts - fitted(lm(ts ~ Time, data = ts.frame))},
               error = function(e){warning(messages[5]); zerovec})

      tryCatch( {stdev <- sqrt(sum((ts - mean(ts))^2))/(ts.length - 1)
                 ts <- ts/stdev},
                error = function(e){warning(messages[6]); zerovec})

      ts
    }  
    ## Apply the detrend.ts function to the variables of interest
    df.detrend <- data.frame(lapply(cadairydata[, 4:7], ts.detrend, cadairydata$Time))

    ## Plot the results to look at the relationships
    pairs(~ Cotagecheese.Prod + Icecream.Prod + Milk.Prod + N.CA.Fat.Price, data = df.detrend, main = "Pairwise Scatterplots of detrended standardized time series")

Oldukça bit oluşmasını yoktur `ts.detrend()` işlevi. Bu kod çoğunu bağımsız değişkenleri ile ilgili olası sorunları veya hala hesaplamalar sırasında ortaya çıkabilecek özel durumlar ilgilenmek için denetleniyor. Bu kod yalnızca birkaç satırlık gerçekte hesaplamalar yapın.

Biz zaten savunma programlamada örneği ele alınan [değer dönüşümler](#valuetransformations). Her iki hesaplama blokları kaydırılır `tryCatch()`. Bazı hatalar için özgün giriş vektör döndürülecek mantıklıdır ve diğer durumlarda, ı vektör sıfır değerini döndürür.  

XML'deki eğilimleri belirlemek için kullanılan doğrusal regresyon zaman serisi regresyon olduğuna dikkat edin. Bir zaman serisi nesnesi bir göstergesi olduğu değişkenidir.  

Bir kez `ts.detrend()` tanımlanmış biz bizim dataframe ilgi değişkenleri geçerli. Tarafından oluşturulan sonuç listesi coerce gerekir `lapply()` kullanarak veri dataframe için `as.data.frame()`. Savunma yönlerini nedeniyle `ts.detrend()`, hata işleme değişkenlerinden biri olmayan diğer doğru işlenmesini engelleyebilir.  

Kodu son satırının pairwise scatterplot oluşturur. R kodu çalıştırdıktan sonra sonuçları scatterplot, Şekil 17'de gösterilmektedir.

![XML'deki koleksiyonunuzdaki ve standartlaştırılmış zaman serisinin pairwise scatterplot][18]

*Şekil 17. Pairwise scatterplot XML'deki koleksiyonunuzdaki ve standartlaştırılmış zaman serisinin.*

Bu sonuçları bu Şekil 16'da gösterilen karşılaştırabilirsiniz. Kaldırılan eğilim ve standartlaştırılmış değişkenleri, biz bu değişkenleri arasındaki ilişkileri çok daha az yapısında bakın.

Bağıntıları R ccf nesneler olarak hesaplamak için kod aşağıdaki gibidir.

    ## A function to compute pairwise correlations from a
    ## list of time series value vectors
    pair.cor <- function(pair.ind, ts.list, lag.max = 1, plot = FALSE){
      ccf(ts.list[[pair.ind[1]]], ts.list[[pair.ind[2]]], lag.max = lag.max, plot = plot)
    }

    ## A list of the pairwise indices
    corpairs <- list(c(1,2), c(1,3), c(1,4), c(2,3), c(2,4), c(3,4))

    ## Compute the list of ccf objects
    cadairycorrelations <- lapply(corpairs, pair.cor, df.detrend)  

    cadairycorrelations

Bu kodu çalıştırmak Şekil 18'de gösterilen günlük üretir.

    [ModuleOutput] Loading objects:
    [ModuleOutput]   port1
    [ModuleOutput] [1] "Loading variable port1..."
    [ModuleOutput] [[1]]
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput] Autocorrelations of series 'X', by lag
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput]    -1     0     1 
    [ModuleOutput] 0.148 0.358 0.317 
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput] [[2]]
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput] Autocorrelations of series 'X', by lag
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput]     -1      0      1 
    [ModuleOutput] -0.395 -0.186 -0.238 
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput] [[3]]
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput] Autocorrelations of series 'X', by lag
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput]     -1      0      1 
    [ModuleOutput] -0.059 -0.089 -0.127 
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput] [[4]]
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput] Autocorrelations of series 'X', by lag
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput]    -1     0     1 
    [ModuleOutput] 0.140 0.294 0.293 
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput] [[5]]
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput] Autocorrelations of series 'X', by lag
    [ModuleOutput] 
    [ModuleOutput] 
    [ModuleOutput]     -1      0      1 
    [ModuleOutput] -0.002 -0.074 -0.124 

*Şekil 18. Ccf listesini ikili bağıntı çözümlemesinden nesneleri.*

Her gecikme için bir bağıntı değer yoktur. Bu bağıntı değerleri hiçbiri önemli olmadığı kadar büyük. Biz biz her bir değişken bağımsız olarak model oluşturabilirsiniz, bu nedenle tamamlanabilmesi.

### <a name="output-a-dataframe"></a>Bir dataframe çıkış
Biz pairwise bağıntıları R ccf nesneleri listesi olarak hesaplanan. Sonuç veri kümesinin çıkış bağlantı noktasına bir dataframe gerçekten gerektirdiğinden bu biraz bir sorunu gösterir. Ayrıca, ccf nesnesi kendisini listesini ve yalnızca değerleri bu listenin ilk öğesindeki çeşitli gecikmelere adresindeki bağıntıları istiyoruz.

Aşağıdaki kod kendilerini listeleridir ccf nesneleri listesinden öteleme değerlerini ayıklar.

    df.correlations <- data.frame(do.call(rbind, lapply(cadairycorrelations, '[[', 1)))

    c.names <- c("correlation pair", "-1 lag", "0 lag", "+1 lag")
    r.names  <- c("Corr Cot Cheese - Ice Cream",
                  "Corr Cot Cheese - Milk Prod",
                  "Corr Cot Cheese - Fat Price",
                  "Corr Ice Cream - Mik Prod",
                  "Corr Ice Cream - Fat Price",
                  "Corr Milk Prod - Fat Price")

    ## Build a dataframe with the row names column and the
    ## correlation data frame and assign the column names
    outframe <- cbind(r.names, df.correlations)
    colnames(outframe) <- c.names
    outframe


    ## WARNING!
    ## The following line works only in Azure Machine Learning
    ## When running in RStudio, this code will result in an error
    #maml.mapOutputPort('outframe')

Kod ilk satırının biraz zor olduğu ve bazı açıklama onu anlamanıza yardımcı olabilir. Inside out çalışma şunları sunuyoruz:

1. '**[[**'İşleci' bağımsız değişkeni ile**1**' ccf nesne listesi ilk öğeden gecikmelere adresindeki bağıntıları vektör seçer.
2. `do.call()` İşlevi uygular `rbind()` işlevi listenin öğelerini üzerinden döndürür tarafından `lapply()`.
3. `data.frame()` İşlevi tarafından üretilen sonuç olacak şekilde zorlar `do.call()` bir dataframe için.

Satır adlarını dataframe sütununda olduğunu unutmayın. Bunu korur satır yapılması adları gelen çıktısını alırken [R betiği yürütün][execute-r-script].

Kod çalıştırmasını çıktılar şekil 19'gösterilen zaman ı **Görselleştir** sonuç Dataset noktasındaki çıktı. Satır ilk sütunda tasarlandığı gibi adlardır.

![Bağıntı analiz sonuç çıktısı][20]

*Şekil 19. Bağıntı çözümlemesinden çıktı sonuçları.*

## <a id="seasonalforecasting"></a>Zaman serisi örnek: Mevsimlik tahmin
Bizim veri analizi için uygun bir biçimde sunulmuştur ve değişkenler arasındaki önemli hiçbir bağıntıları vardır belirledik. Şimdi geçmek ve bir zaman serisi modeli tahmin oluşturun. Bu modeli kullanarak biz California sütlü üretim 2013, 12 ay boyunca tahmin.

Tahmin modelimizi eğilim bileşeni ve Mevsimlik bileşeni olmak üzere iki bileşenden sahip olur. Tam tahmin bu iki bileşenin ürünüdür. Bu model türüne çarpma model olarak bilinir. Alternatif bir toplama modelidir. Bir günlük dönüşüm zaten bu çözümleme tractable yapar ilgi değişkenlere uyguladınız.

Tam R Bu bölümde daha önce indirdiğiniz zip dosyasında kodudur.

### <a name="creating-the-dataframe-for-analysis"></a>Analiz için dataframe oluşturma
Başlangıç ekleyerek bir **yeni** [R betiği yürütün] [ execute-r-script] denemenizi modülü. Bağlantı **sonuç Dataset** var olan çıktı [R betiği yürütün] [ execute-r-script] modülüne **Dataset1** yeni modülünün giriş. Sonuç Şekil 20 gibi görünmelidir.

![Eklenen yeni R betiği yürütün modülüyle deneme][21]

*Şekil 20. Eklenen yeni R betiği yürütün modülüyle deneme.*

Olarak bağıntı çözümlemesi biz yalnızca tamamlandı, biz POSIXct zaman serisi nesne içeren bir sütun eklemeniz gerekir. Aşağıdaki kod, yalnızca bu yapın.

    # If running in Machine Learning Studio, uncomment the first line with maml.mapInputPort()
    cadairydata <- maml.mapInputPort(1)

    ## Create a new column as a POSIXct object
    Sys.setenv(TZ = "PST8PDT")
    cadairydata$Time <- as.POSIXct(strptime(paste(as.character(cadairydata$Year), "-", as.character(cadairydata$Month.Number), "-01 00:00:00", sep = ""), "%Y-%m-%d %H:%M:%S"))

    str(cadairydata)

Bu kodu çalıştırmak ve günlüğüne bakın. Sonuç şekil 21 gibi görünmelidir.

    [ModuleOutput] [1] "Loading variable port1..."
    [ModuleOutput] 
    [ModuleOutput] 'data.frame':    228 obs. of  9 variables:
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Number     : int  1 2 3 4 5 6 7 8 9 10 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Year             : int  1995 1995 1995 1995 1995 1995 1995 1995 1995 1995 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month            : Factor w/ 12 levels "Apr","Aug","Dec",..: 5 4 8 1 9 7 6 2 12 11 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Cotagecheese.Prod: num  1.47 1.31 1.51 1.45 1.5 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Icecream.Prod    : num  5.82 5.9 6.1 6.06 6.17 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Milk.Prod        : num  7.66 7.57 7.68 7.66 7.71 ...
    [ModuleOutput] 
    [ModuleOutput]  $ N.CA.Fat.Price   : num  6.89 6.79 6.79 6.8 6.8 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Month.Count      : num  0 1 2 3 4 5 6 7 8 9 ...
    [ModuleOutput] 
    [ModuleOutput]  $ Time             : POSIXct, format: "1995-01-01" "1995-02-01" ...

*Şekil 21. Dataframe özeti.*

Bu sonuçla biz bizim analiz başlatmaya hazırsınız.

### <a name="create-a-training-dataset"></a>Bir eğitim veri kümesi oluşturma
Oluşturulan dataframe ile biz bir eğitim veri kümesi oluşturmanız gerekir. Bu verileri tüm 2013 yılın son 12 dışında gözlemleri test kümemize olduğu içerecektir. Aşağıdaki alt kümelerini dataframe kod ve çizimler Süt üretim ve fiyat değişkenlerin oluşturur. I bu çizimler dört üretim oluşturun ve değişkenleri fiyat. Adsız bir işlev bazı güçlendirir çizim için tanımlamak ve diğer iki bağımsız değişken listesiyle üzerinden yineleme yapmak için kullanılan `Map()`. Size, düşünüyor varsa bir döngü ince burada çalışılan için doğru. Ancak, R ı işlevsel bir yaklaşım gösteren işlevsel bir dili olduğundan.

    cadairytrain <- cadairydata[1:216, ]

    Ylabs  <- list("Log CA Cotage Cheese Production, 1000s lb",
                   "Log CA Ice Cream Production, 1000s lb",
                   "Log CA Milk Production 1000s lb",
                   "Log North CA Milk Milk Fat Price per 1000 lb")

    Map(function(y, Ylabs){plot(cadairytrain$Time, y, xlab = "Time", ylab = Ylabs, type = "l")}, cadairytrain[, 4:7], Ylabs)

Kodu çalıştırmak Şekil 22'de gösterilen R aygıt çıktısından serisi çizer zaman serisi üretir. Zaman ekseni tarihleri, yöntemi seriyi çizmek zaman iyi bir yararı birimlerinde olduğuna dikkat edin.

![Zaman serisi çizimleri California Süt üretim ve fiyat verilerinin ilk](./media/r-quickstart/unnamed-chunk-161.png)

![Zaman serisi çizimleri California Süt üretim ve fiyat veri saniyesi](./media/r-quickstart/unnamed-chunk-162.png)

![Zaman serisi çizimleri California Süt üretim ve fiyat veri üçüncü](./media/r-quickstart/unnamed-chunk-163.png)

![Zaman serisi çizimleri California Süt üretim ve fiyat veri dördüncü](./media/r-quickstart/unnamed-chunk-164.png)

*Şekil 22. Zaman serisi çizimleri California Süt üretim ve fiyat verileri.*

### <a name="a-trend-model"></a>Eğilim modeli
Zaman serisi nesne ve verileri gördüğünüze oluşturulduktan sonra California sütlü üretim verileri için bir eğilim modeli oluşturmak başlayalım. Zaman serisi regresyon ile biz bunu yapabilirsiniz. Ancak, biz en fazla bir Eğim ihtiyaç ve doğru şekilde eğitim verileri gözlemlenen eğilimi modellemek için müdahale çizim gelen temizleyin.

Verilerin küçük ölçekli verildiğinde, t Rstudio'dan eğilimi için model oluşturmak ve ardından kesebilir ve sonuç olarak oluşan model Azure Machine Learning yapıştırabilirsiniz. Rstudio'dan etkileşimli analiz bu tür için etkileşimli bir ortam sağlar.

Bir ilk girişimi ı polinom regresyon powers kadar 3 ile çalışacaktır. Bu tür modelleri aşırı sığdırma, gerçek bir tehlike yoktur. Bu nedenle, yüksek sipariş terimler önlemek en iyisidir. `I()` İşlevi engeller içeriği yorumu ('olduğundan' içeriği yorumlar) ve bir regresyon denklemi tam anlamıyla yorumlanan işlevi yazmanızı sağlar.

    milk.lm <- lm(Milk.Prod ~ Time + I(Month.Count^2) + I(Month.Count^3), data = cadairytrain)
    summary(milk.lm)

Aşağıdaki oluşturur.

    ##
    ## Call:
    ## lm(formula = Milk.Prod ~ Time + I(Month.Count^2) + I(Month.Count^3),
    ##     data = cadairytrain)
    ##
    ## Residuals:
    ##      Min       1Q   Median       3Q      Max
    ## -0.12667 -0.02730  0.00236  0.02943  0.10586
    ##
    ## Coefficients:
    ##                   Estimate Std. Error t value Pr(>|t|)
    ## (Intercept)       6.33e+00   1.45e-01   43.60   <2e-16 ***
    ## Time              1.63e-09   1.72e-10    9.47   <2e-16 ***
    ## I(Month.Count^2) -1.71e-06   4.89e-06   -0.35    0.726
    ## I(Month.Count^3) -3.24e-08   1.49e-08   -2.17    0.031 *  
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ##
    ## Residual standard error: 0.0418 on 212 degrees of freedom
    ## Multiple R-squared:  0.941,    Adjusted R-squared:  0.94
    ## F-statistic: 1.12e+03 on 3 and 212 DF,  p-value: <2e-16

P değerlerinden (Pr (> | t |)) bu çıktısında squared terim önemli olmayabilir görebiliriz. Kullanacağım `update()` squared terim bırakarak bu modeli değiştirmek için işlevi.

    milk.lm <- update(milk.lm, . ~ . - I(Month.Count^2))
    summary(milk.lm)

Aşağıdaki oluşturur.

    ##
    ## Call:
    ## lm(formula = Milk.Prod ~ Time + I(Month.Count^3), data = cadairytrain)
    ##
    ## Residuals:
    ##      Min       1Q   Median       3Q      Max
    ## -0.12597 -0.02659  0.00185  0.02963  0.10696
    ##
    ## Coefficients:
    ##                   Estimate Std. Error t value Pr(>|t|)
    ## (Intercept)       6.38e+00   4.07e-02   156.6   <2e-16 ***
    ## Time              1.57e-09   4.32e-11    36.3   <2e-16 ***
    ## I(Month.Count^3) -3.76e-08   2.50e-09   -15.1   <2e-16 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ##
    ## Residual standard error: 0.0417 on 213 degrees of freedom
    ## Multiple R-squared:  0.941,  Adjusted R-squared:  0.94
    ## F-statistic: 1.69e+03 on 2 and 213 DF,  p-value: <2e-16

Bu, daha iyi görünür. Tüm koşullarını önemlidir. Ancak, 2e-16 değeri varsayılan değerdir ve çok ciddi alınmamalıdır.  

Sağlamlık test olarak bir zaman serisi çizim California Süt üretim verilerin gösterildiği eğilimi eğri ile olalım. Aşağıdaki kod Azure Machine Learning eklediğiniz [R betiği yürütün] [ execute-r-script] model oluşturmak ve bir çizim yapmak için model (Rstudio'dan değil). Sonuç şekil 23'te gösterilir.

    milk.lm <- lm(Milk.Prod ~ Time + I(Month.Count^3), data = cadairytrain)

    plot(cadairytrain$Time, cadairytrain$Milk.Prod, xlab = "Time", ylab = "Log CA Milk Production 1000s lb", type = "l")
    lines(cadairytrain$Time, predict(milk.lm, cadairytrain), lty = 2, col = 2)

![California sütlü üretim verileri ile gösterilen eğilimi modeli](./media/r-quickstart/unnamed-chunk-18.png)

*Şekil 23. California sütlü üretim verileriyle gösterilen eğilimi modeli.*

Eğilim model veri oldukça iyi uygun gibi görünüyor. Ayrıca, değil gözükmüyor atlayarak sığdırma kanıtı modeli eğrisindeki tek wiggles gibi.  

### <a name="seasonal-model"></a>Mevsimlik modeli
Bir eğilim modelinde ile elle push ve Mevsimlik efektleri eklemek ihtiyacımız. Yılın ayını Doğrusal model kukla bir değişkende olarak ay ay etkisi yakalamak için kullanacağız. Bir modele faktörü değişkenleri aldığımızda, ıntercept'in değil hesaplanan gerekir unutmayın. Bunu yaparsanız, formül aşırı belirtilir ve R istenen Etkenler birini bırakın ancak ıntercept terim tutun.

Biz tatmin edici eğilimi modeline sahip olduğundan biz kullanabilir `update()` yeni koşulları varolan modele eklemek için işlev. Güncelleştirme formülünde -1 ıntercept terim bırakır. İçinde Rstudio'dan şu anda devam:

    milk.lm2 <- update(milk.lm, . ~ . + Month - 1)
    summary(milk.lm2)

Aşağıdaki oluşturur.

    ##
    ## Call:
    ## lm(formula = Milk.Prod ~ Time + I(Month.Count^3) + Month - 1,
    ##     data = cadairytrain)
    ##
    ## Residuals:
    ##      Min       1Q   Median       3Q      Max
    ## -0.06879 -0.01693  0.00346  0.01543  0.08726
    ##
    ## Coefficients:
    ##                   Estimate Std. Error t value Pr(>|t|)
    ## Time              1.57e-09   2.72e-11    57.7   <2e-16 ***
    ## I(Month.Count^3) -3.74e-08   1.57e-09   -23.8   <2e-16 ***
    ## MonthApr          6.40e+00   2.63e-02   243.3   <2e-16 ***
    ## MonthAug          6.38e+00   2.63e-02   242.2   <2e-16 ***
    ## MonthDec          6.38e+00   2.64e-02   241.9   <2e-16 ***
    ## MonthFeb          6.31e+00   2.63e-02   240.1   <2e-16 ***
    ## MonthJan          6.39e+00   2.63e-02   243.1   <2e-16 ***
    ## MonthJul          6.39e+00   2.63e-02   242.6   <2e-16 ***
    ## MonthJun          6.38e+00   2.63e-02   242.4   <2e-16 ***
    ## MonthMar          6.42e+00   2.63e-02   244.2   <2e-16 ***
    ## MonthMay          6.43e+00   2.63e-02   244.3   <2e-16 ***
    ## MonthNov          6.34e+00   2.63e-02   240.6   <2e-16 ***
    ## MonthOct          6.37e+00   2.63e-02   241.8   <2e-16 ***
    ## MonthSep          6.34e+00   2.63e-02   240.6   <2e-16 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ##
    ## Residual standard error: 0.0263 on 202 degrees of freedom
    ## Multiple R-squared:     1,    Adjusted R-squared:     1
    ## F-statistic: 1.42e+06 on 14 and 202 DF,  p-value: <2e-16

Biz, model artık bir kesme noktası terim sahiptir ve 12 ay önemli faktörler olan bakın. Bu, tam olarak biz görmek istediğinizi olur.

Mevsimlik modeli ne kadar iyi çalıştığını görmek için başka bir zaman serisi çizim California Süt üretim verisi olalım. Aşağıdaki kod Azure Machine Learning eklediğiniz [R betiği yürütün] [ execute-r-script] model oluşturmak ve bir çizim yapmak için.

    milk.lm2 <- lm(Milk.Prod ~ Time + I(Month.Count^3) + Month - 1, data = cadairytrain)

    plot(cadairytrain$Time, cadairytrain$Milk.Prod, xlab = "Time", ylab = "Log CA Milk Production 1000s lb", type = "l")
    lines(cadairytrain$Time, predict(milk.lm2, cadairytrain), lty = 2, col = 2)

Azure Machine Learning ile bu kodu çalıştırmak şekil 24'te gösterilen çizim üretir.

![California sütlü üretim Mevsimlik efektler dahil modeli](./media/r-quickstart/unnamed-chunk-20.png)

*Şekil 24. California sütlü üretim Mevsimlik efektler dahil modeli.*

Şekil 24'de gösterilen veri Sığdır yerine encouraging. Eğilim ve Mevsimlik etkisi (aylık değişim) makul arayın.

Bizim model üzerinde başka bir onay, şirketinizdeki Kalanlar göz vardır. Aşağıdaki kod iki bizim modeli tahmin edilen değerleri hesaplar, Kalanlar Mevsimlik modeli için hesaplar ve eğitim verileri için bu Kalanlar çizer.

    ## Compute predictions from our models
    predict1  <- predict(milk.lm, cadairydata)
    predict2  <- predict(milk.lm2, cadairydata)

    ## Compute and plot the residuals
    residuals <- cadairydata$Milk.Prod - predict2
    plot(cadairytrain$Time, residuals[1:216], xlab = "Time", ylab ="Residuals of Seasonal Model")

Kalan çizim şekil 25 gösterilir.

![Eğitim verileri Mevsimlik modelinin Kalanlar](./media/r-quickstart/unnamed-chunk-21.png)

*Şekil 25. Eğitim verileri Mevsimlik modelinin Kalanlar.*

Bu Kalanlar makul arayın. Bizim modeli için hesaplamaz 2008-2009 recession etkisini dışında hiçbir belirli yapısı yoktur özellikle iyi.

Şekil 25 gösterilen çizim Kalanlar herhangi zamana bağımlı desenleri algılamak için yararlı olur. Açık bir yaklaşım, hesaplama ve kullandım çizme çizim zaman siparişte Kalanlar yerleştirir. Diğer taraftan, ı çizilen değilse, `milk.lm$residuals`, çizim süre sırada olabilirdi değil.

Aynı zamanda `plot.lm()` bir dizi tanı çizimleri üretmek için.

    ## Show the diagnostic plots for the model
    plot(milk.lm2, ask = FALSE)

Bu kod, bir dizi şekil 26'da gösterilen tanılama çizimleri üretir.

![Tanılama çizimleri Mevsimlik modeli için ilk](./media/r-quickstart/unnamed-chunk-221.png)

![Tanılama çizimleri ikinci Mevsimlik modeli için](./media/r-quickstart/unnamed-chunk-222.png)

![Tanılama çizimleri Mevsimlik modeli için üçüncü](./media/r-quickstart/unnamed-chunk-223.png)

![Mevsimlik modeli için tanılama çizimleri dördüncü](./media/r-quickstart/unnamed-chunk-224.png)

*Şekil 26. Tanılama Mevsimlik modelini çizer.*

Bu çizimler, ancak hiçbir şey mükemmel sorunu neden tanımlanan yüksek etkili birkaç nokta vardır. Normal Q-Q çizim Kalanlar normalde dağıtılmış için doğrusal modeller için önemli bir varsayım Kapat Ayrıca, görebiliriz.

### <a name="forecasting-and-model-evaluation"></a>Tahmin ve model değerlendirme
Bizim örneğimizde tamamlamak için yapmak için tek daha fazla şey yoktur. Tahminleri işlem ve hata karşı gerçek veri ölçmek gerekir. Bizim tahmin 2013 12 ay için olacaktır. Biz bu tahmin eğitim kümemize parçası değil gerçek veri için bir hata ölçü hesaplayabilirsiniz. Ayrıca, biz 18 yıllık test verilerinin 12 ay için eğitim verilerini performansına karşılaştırabilirsiniz.  

Ölçümleri sayısı zaman serisi modelleri performansını ölçmek için kullanılır. Örneğimizde kök Ortalama kare (RMS) hata kullanacağız. Aşağıdaki işlevi iki serinin arasındaki RMS hatası hesaplar.  

    RMS.error <- function(series1, series2, is.log = TRUE, min.length = 2){
      ## Function to compute the RMS error or difference between two
      ## series or vectors

      messages <- c("ERROR: Input arguments to function RMS.error of wrong type encountered",
                    "ERROR: Input vector to function RMS.error is too short",
                    "ERROR: Input vectors to function RMS.error must be of same length",
                    "WARNING: Funtion rms.error has received invald input time series.")

      ## Check the arguments
      if(!is.numeric(series1) | !is.numeric(series2) | !is.logical(is.log) | !is.numeric(min.length)) {
        warning(messages[1])
        return(NA)}

      if(length(series1) < min.length) {
        warning(messages[2])
        return(NA)}

      if((length(series1) != length(series2))) {
           warning(messages[3])
        return(NA)}

      ## If is.log is TRUE exponentiate the values, else just copy
      if(is.log) {
        tryCatch( {
          temp1 <- exp(series1)
          temp2 <- exp(series2) },
          error = function(e){warning(messages[4]); NA}
        )
      } else {
        temp1 <- series1
        temp2 <- series2
      }

     ## Compute predictions from our models
    predict1  <- predict(milk.lm, cadairydata)
    predict2  <- predict(milk.lm2, cadairydata)

    ## Compute the RMS error in a dataframe
      tryCatch( {
        sqrt(sum((temp1 - temp2)^2) / length(temp1))},
        error = function(e){warning(messages[4]); NA})
    }

İle `log.transform()` biz "dönüşümleri değer" bölümünde açıklanan işlevi oldukça çok sayıda hata denetimi ve özel durum kurtarma kodu Bu işlevde yoktur. İşe ilkeleri aynıdır. İş sarmalanmış iki yerde yapılır `tryCatch()`. İlk olarak, şu değerleri günlükleriyle çalışma olduğundan zaman serisi exponentiated, ' dir. İkinci olarak, gerçek bir RMS hata hesaplanır.  

RMS hatası ölçmek için bir işlev ile donatılmış, şimdi oluşturmak ve RMS hata içeren bir dataframe çıktı. Tek başına eğilimi modeli koşulları ve Mevsimlik Etkenler tam modeliyle dahil edilir. Aşağıdaki kod, biz oluşturulan iki doğrusal modeller kullanarak iş yapar.

    ## Compute the RMS error in a dataframe
    ## Include the row names in the first column so they will
    ## appear in the output of the Execute R Script
    RMS.df  <-  data.frame(
    rowNames = c("Trend Model", "Seasonal Model"),
      Traing = c(
      RMS.error(predict1[1:216], cadairydata$Milk.Prod[1:216]),
      RMS.error(predict2[1:216], cadairydata$Milk.Prod[1:216])),
      Forecast = c(
        RMS.error(predict1[217:228], cadairydata$Milk.Prod[217:228]),
        RMS.error(predict2[217:228], cadairydata$Milk.Prod[217:228]))
    )
    RMS.df

    ## The following line should be executed only when running in
    ## Azure Machine Learning Studio
    maml.mapOutputPort('RMS.df')

Bu kodu çalıştırmak sonuç veri kümesinin çıkış bağlantı noktasında 27 şekil gösterilen çıkışı üretir.

![Modeller için RMS hataları karşılaştırması][26]

*Şekil 27. Karşılaştırma modelleri için RMS hata sayısı.*

Bu sonuçlarından Mevsimlik Etkenler Modeli'ne ekleme RMS hatası önemli ölçüde azaltır, bkz. Çok edilebileceği RMS eğitim verileri için bir bit değerinden tahmini hatasıdır.

## <a id="appendixa"></a>Ek A: Kılavuzu'na Rstudio'dan
Bu ekte ı başlamanıza yardımcı olmak için Rstudio'dan belgelerine anahtar bölümlerine bazı bağlantılar sağlayacak şekilde Rstudio'dan oldukça iyi belgelenmiştir.

1. Proje oluşturma
   
   Düzenleyebilir ve Rstudio'dan kullanarak R kodunuzu projelere yönetin. Projeleri kullanan belgeleri şu adreste bulunabilir: https://support.rstudio.com/hc/articles/200526207-Using-Projects.
   
   I bu yönergeleri izleyin ve bu belgede R kod örnekleri için bir proje oluşturun öneririz.  
2. Düzenleme ve R kodu çalıştırma
   
   Rstudio'dan düzenleme ve R kod yürütmek için tümleşik bir ortam sağlar. Belgeler bulunabilir https://support.rstudio.com/hc/articles/200484448-Editing-and-Executing-Code.
3. Hata ayıklama
   
   Rstudio'dan güçlü hata ayıklama özellikleri içerir. Bu özellikler için belgesidir adresindeki https://support.rstudio.com/hc/articles/200713843-Debugging-with-RStudio.
   
   Kesme noktası sorun giderme özellikleri adresinde belgelenen https://support.rstudio.com/hc/articles/200534337-Breakpoint-Troubleshooting.

## <a id="appendixb"></a>Ek B: Daha fazla bilgi
Bu R programlama öğretici R dil Azure Machine Learning Studio ile kullanmak ihtiyacınız olan temel bilgiler yer almaktadır. R ile bilmiyorsanız, iki tanıtımları CRAN üzerinde kullanılabilir:

* R Emmanuel Paradis tarafından yeni başlayanlar için başlangıç için iyi bir yerdir http://cran.r-project.org/doc/contrib/Paradis-rdebuts_en.pdf.  
* W. n R giriş Venables et. Al. konumundaki biraz daha kapsamlı giden http://cran.r-project.org/doc/manuals/R-intro.html.

Başlamanıza yardımcı olabilecek R birçok books vardır. Yararlı bulabilirim birkaç şunlardır:

* R programlama resim: bir tur, istatistiksel yazılım Norman Matloff tarafından r programlamada mükemmel giriş tasarımdır  
* R Kılavuzu Paul Teetor tarafından r kullanarak bir sorun ve çözümü yaklaşım sağlar  
* Eylem Robert Kabacoff tarafından R başka yararlı tanıtım Rehberi ' dir. Yardımcı hızlı R Web sitesi yararlı kaynaktır http://www.statmethods.net/.
* R Inferno CAN yanıklara tarafından r'de programlamada karşılaştı hassas ve zor konular sayısıyla ilgilidir şaşırtıcı esprili bir kitap olduğu Kitap ücretsiz adresten edinilebilir http://www.burns-stat.com/documents/books/the-r-inferno/.
* Derinlemesine Gelişmiş konular R, içine istiyorsanız Gelişmiş R Hadley Wickham tarafından kitap bakmak gerekir. Bu kitap çevrimiçi sürümünü ücretsiz adresten edinilebilir http://adv-r.had.co.nz/.

Zaman serisi çözümleme için CRAN görev görünümünde Kataloğu R zaman serisi paketlerin bulunabilir: http://cran.r-project.org/web/views/TimeSeries.html. Paketleri serisi nesne belirli bir zaman hakkında bilgi için bu paket için belgelerine başvurmalıdır.

Paul Cowpertwait ve Barış Metcalfe r Giriş zaman serisi defteri R zaman serisi analize kullanmaya giriş bilgileri sağlar. Çok fazla teorik metinleri R örnekleri sağlar.

Harika bazı Internet kaynakların:

* DataCamp: DataCamp video dersleri ve kodlama alıştırmaları tarayıcınızla rahatlık içinde R öğretir. En son R teknikleri ve paketleri etkileşimli öğreticileri vardır. Ücretsiz etkileşimli R adresindeki Öğreticisi https://www.datacamp.com/courses/introduction-to-r
* Bir alma üzerinde r ile Programiz gelen Kılavuzu https://www.programiz.com/r-programming
* Hızlı bir R öğretici Kelly siyah Clarkson University gelen tarafından http://www.cyclismo.org/tutorial/R/
* R adresinde listelenmiş 60 + kaynakları http://www.computerworld.com/article/2497464/business-intelligence-60-r-resources-to-improve-your-data-skills.html

<!--Image references-->
[1]: ./media/r-quickstart/fig1.png
[2]: ./media/r-quickstart/fig2.png
[3]: ./media/r-quickstart/fig3.png
[4]: ./media/r-quickstart/fig4.png
[5]: ./media/r-quickstart/fig5.png
[6]: ./media/r-quickstart/fig6.png
[7]: ./media/r-quickstart/fig7.png
[8]: ./media/r-quickstart/fig8.png
[9]: ./media/r-quickstart/fig9.png
[10]: ./media/r-quickstart/fig10.png
[11]: ./media/r-quickstart/fig11.png
[12]: ./media/r-quickstart/fig12.png
[13]: ./media/r-quickstart/fig13.png
[14]: ./media/r-quickstart/fig14.png
[15]: ./media/r-quickstart/fig15.png
[16]: ./media/r-quickstart/fig16.png
[17]: ./media/r-quickstart/fig17.png
[18]: ./media/r-quickstart/fig18.png
[19]: ./media/r-quickstart/fig19.png
[20]: ./media/r-quickstart/fig20.png
[21]: ./media/r-quickstart/fig21.png
[22]: ./media/r-quickstart/fig22.png

[26]: ./media/r-quickstart/fig26.png

<!--links-->
[appendixa]: #appendixa
[download]: https://azurebigdatatutorials.blob.core.windows.net/rquickstart/RFiles.zip


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
