---
title: R kullanmaya başlama
titleSuffix: Azure Machine Learning Studio
description: Tahmin çözümünü oluşturmak için Azure Machine Learning Studio ile R dili ile çalışmaya başlamak için R programlama Bu öğreticiyi kullanın.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: xiaoharper
ms.author: amlstudiodocs
ms.custom: previous-author=heatherbshapiro, previous-ms.author=hshapiro
ms.date: 03/01/2019
ms.openlocfilehash: 5c4fa2260b00043e016748010528926b1b9d74a3
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64726553"
---
# <a name="getting-started-with-the-r-programming-language-in-azure-machine-learning-studio"></a>R programlama dili Azure Machine Learning Studio'da kullanmaya başlama

<!-- Stephen F Elston, Ph.D. -->

## <a name="introduction"></a>Giriş

Bu öğreticide Azure Machine Learning Studio'da R programlama dilini kullanarak genişletme başlamanıza yardımcı olur. Oluşturun, test ve Studio içinde R kod yürütmek için bu R programlama öğreticiyi izleyin. Öğreticide çalışırken, Studio'da R dili kullanarak eksiksiz bir tahmin çözümü oluşturur.  

Microsoft Azure Machine Learning Studio, çok sayıda güçlü makine öğrenimi ve veri işleme modüller içerir. Güçlü R dili en yaygın kullanılan analytics'in açıklanan. Sonsuza dek, analiz ve veri işleme Studio'da r kullanarak uzatılabilir Esneklik ve ayrıntılı analizlerle r'ın bu birleşim Studio'nun dağıtım kolaylığı ve ölçeklenebilirlik sağlar

### <a name="forecasting-and-the-dataset"></a>Tahmini ve veri kümesi

Tahmini bir yaygın olarak çalıştırılan ve oldukça faydalı analitik yöntemidir. Ortak satış macroeconomic değişkenleri tahmin etmek için en iyi stok düzeylerini belirlemek dönemsel öğelerinin tahmin gelen aralığını kullanır. Tahmin, genellikle zaman serisi modelleri ile gerçekleştirilir.

Zaman serisi verileri, değerlerin bir zaman dizini olan verilerdir. Örneğin, her ay ya da her dakika süre dizininin normal, olabilir veya düzensiz. Zaman serisi modelindeki zaman serisi verileri temel alır. R programlama dili, bir esnek framework ve kapsamlı analizi için zaman serisi verilerini içerir.

Bu kılavuzda biz California Süt üretim ile çalışma ve veri fiyatlandırma. Bu veriler, Süt birden çok ürünlerin üretim ve sütlü fat Kıyaslama ticari fiyatı aylık bilgi içerir.

Bu makalede, R betikleri ile birlikte kullanılan verileri indirilebileceğini [MachineLearningSamples-not defterlerini/studio-samples](https://github.com/Azure-Samples/MachineLearningSamples-Notebooks/tree/master/studio-samples). Veri dosyasındaki `cadairydata.csv` ilk olarak University Wisconsin gelen bilgilerden oluşturulan [ https://dairymarkets.com ](https://dairymarkets.com).

### <a name="organization"></a>Kuruluş

Oluşturma, test ve Azure Machine Learning Studio ortamında analiz ve veri işleme R kod yürütme öğrenirken size çeşitli adımlarda ilerleyeceğine.  

* İlk biz Azure Machine Learning Studio ortamında R dilini kullanmanın temellerini inceleyeceksiniz.
* Ardından verilerin, R kodunu ve Azure Machine Learning Studio ortamında grafikler için g/ç çeşitli yönlerini tartışmak için ilerleme.
* Biz ardından tahmin Çözümümüzü ilk bölümünü verileri temizlemek ve dönüştürmek için kod oluşturarak oluşturmak.
* Hazırlanmış verilerimizi ile biz birkaç veri kümemizdeki değişkenleri arasında bağıntılar analizini yapar.
* Son olarak, Dönemsel zaman serisi tahmin modeli sütlü üretim için oluşturacağız.

## <a id="mlstudio"></a>Machine Learning Studio'da R dili ile etkileşim kurma

Bu bölümde Machine Learning Studio ortamında olan R programlama dili ile etkileşim kurmanın bazı temel alır. R dili, özelleştirilmiş analytics ve Azure Machine Learning Studio ortamı içinde veri işleme modüller oluşturmak için güçlü bir araç sağlar.

RStudio geliştirmek, test ve küçük ölçekli R kodu hata ayıklama için kullanır. Bu kodu ardından kesme ve yapıştırma içine bir [R betiği yürütme] [ execute-r-script] modülü Machine Learning Studio'da çalışmaya hazır.  

### <a name="the-execute-r-script-module"></a>R betiği yürütme Modülü

Machine Learning Studio, R betikleri içinde çalıştırılan [R betiği yürütme] [ execute-r-script] modülü. Örneği [R betiği yürütme] [ execute-r-script] Machine Learning Studio'da bir modül, Şekil 1'de gösterilmiştir.

 ![R programlama dili: Machine Learning Studio'da seçili R betiği yürütme Modülü](./media/r-quickstart/fig1.png)

*Şekil 1. Machine Learning Studio ortam seçili R betiği yürütme modülü gösteriliyor.*

Şekil 1'e başvuran, bazı önemli noktalarından biri ile çalışmak için Machine Learning Studio ortam göz atalım [R betiği yürütme] [ execute-r-script] modülü.

* Denemeyi modülleri, Orta bölmede gösterilir.
* Sağ bölmenin üst kısmındaki görüntülemek ve R betikleriniz düzenlemek için bir pencere içerir.  
* Sağ bölmede alt bölümünde bazı özelliklerini gösterir [R betiği yürütme][execute-r-script]. Bu bölme uygun spot seçerek hata ve çıkış günlükleri görüntüleyebilirsiniz.

Biz, görüştükten olacak [R betiği yürütme] [ execute-r-script] daha ayrıntılı olarak bu makalenin geri kalanında.

Karmaşık R işlevleri ile çalışırken, ı, düzenleme, test ve RStudio içinde hata ayıklama öneririz. Herhangi bir yazılım geliştirme ile kodunuzu kademeli olarak genişletin ve küçük basit test çalışmalarında test ederken. Ardından kesip işlevlerinizi R betik penceresine yapıştırmanız [R betiği yürütme] [ execute-r-script] modülü. Bu yaklaşım, hem RStudio tümleşik geliştirme ortamı (IDE) hem de Azure Machine Learning Studio'nun gücünden yararlanın olanak tanır.  

#### <a name="execute-r-code"></a>R kodunu yürütün

Herhangi bir R kodu [R betiği yürütme] [ execute-r-script] modülü seçerek denemeyi çalıştırdığınızda yürütülecek **çalıştırma** düğmesi. Yürütme tamamlandığında bir onay işareti görünür [R betiği yürütme] [ execute-r-script] simgesi.

#### <a name="defensive-r-coding-for-azure-machine-learning"></a>Azure Machine Learning için savunma R kodlama

Azure Machine Learning Studio'yu kullanarak, örneğin bir web hizmeti için R kodu geliştiriyorsanız, özel durumları ve beklenmeyen veri girişi ile kodunuzu nasıl ilgilenecektir kesinlikle planlamanız gerekir. Netlik sağlamak için ı çok denetimi veya özel durum işleme gösterilen kod örnekleri çoğu in the way of eklemediniz. Biz devam ederken ancak miyim size işlevleri çeşitli örneklerini kullanarak R'ın özel durum işleme yeteneği sunar.  

R özel durum işleme daha eksiksiz bir işlemden ihtiyacınız varsa, aşağıda listelenen Wickham kitabı geçerli bölümlerini okumak önerim [daha fazla okuma](#appendixb).

#### <a name="debug-and-test-r-in-machine-learning-studio"></a>Machine Learning Studio'da R test ve hata ayıklama

Yinelemek için ı test ve küçük bir ölçekte RStudio içinde R kodunuzdaki hataları ayıklamanıza öneririz. Ancak, burada ihtiyaç duyacağınız içinde R kod sorunların izlemek için durumlar vardır [R betiği yürütme] [ execute-r-script] kendisi. Ayrıca, Machine Learning Studio'da sonuçlarınızı denetleyin iyi bir uygulamadır.

R kodunuzun ve Azure Machine Learning Studio platformunda yürütme çıktısını içeren öncelikli olarak bulunur. Bazı ek bilgiler error.log görülür.  

R kodunuzu çalıştırırken, Machine Learning Studio'da bir hata meydana gelirse, ilk kursunuzun eyleminin error.log aramak için olmalıdır. Bu dosya, hatayı düzeltmek ve anlamanıza yardımcı olması için kullanışlı hata iletileri içerebilir. Error.log görüntülemek için seçin **hata günlüğü görüntüle** üzerinde **Özellikler bölmesinde** için [R betiği yürütme] [ execute-r-script] hata içeren.

Örneğin, aşağıdaki R kodu ile tanımlanmamış bir değişken y, buna çalıştırdım bir [R betiği yürütme] [ execute-r-script] Modülü:

```R
x <- 1.0
z <- x + y
```

Bu kod yürütmek bir hata durumuna neden başarısız oluyor. Seçme **hata günlüğü görüntüle** üzerinde **Özellikler bölmesinde** Şekil 2'de gösterilen görüntüler.

  ![Hata iletisi açılır penceresi](./media/r-quickstart/fig2.png)

*Şekil 2. Hata iletisi açılır.*

R hata iletisini görmek için içeren aramak ihtiyacımız gibi görünüyor. Seçin [R betiği yürütme] [ execute-r-script] seçip **içeren görüntülemek** üzerinde öğesi **Özellikler bölmesinde** sağ. Yeni bir tarayıcı penceresi açılır ve aşağıdaki görüyorum.

    [Critical]     Error: Error 0063: The following error occurred during evaluation of R script:
    ---------- Start of error message from R ----------
    object 'y' not found


    object 'y' not found
    ----------- End of error message from R -----------

Bu hata iletisi artık sürprizle karşılaşmazsınız içerir ve sorun açıkça tanımlar.

R ile herhangi bir nesnenin değerini incelemek için bu değerleri içeren dosyayı yazdırabilir. Nesne değerlerini İnceleme için temel olarak R etkileşimli bir oturum ile aynı kurallardır. Örneğin, bir satıra bir değişken adı yazarsanız, nesnenin değerini içeren dosyaya yazdırılır.  

#### <a name="packages-in-machine-learning-studio"></a>Machine Learning Studio'da paketleri

Studio ile önceden yüklenmiş 350'in üzerinde R dil paketleri gelir. Aşağıdaki kodda kullanabileceğiniz [R betiği yürütme] [ execute-r-script] önceden yüklenmiş paketler listesini almak için modülü.

```R
data.set <- data.frame(installed.packages())
maml.mapOutputPort("data.set")
```

Şu anda bu kodu son satırının anlamıyorsanız, okumaya devam edin. Bu makalenin geri kalanında kapsamlı olarak R Studio ortamı ele alınacaktır.

### <a name="introduction-to-rstudio"></a>RStudio giriş

RStudio r için yaygın olarak kullanılan bir ıde'dir RStudio düzenleme, test ve bu kılavuzda kullanılan R kodunu bazı hata ayıklama için kullanır. R kodu, test edilmiş ve hazır olduğunda, yalnızca Kes ve bir Machine Learning Studio'ya RStudio düzenleyiciye yapıştırın [R betiği yürütme] [ execute-r-script] modülü.  

Masaüstü makinenizde yüklü olan R programlama dili yoksa, artık bunu ben önerilir. Açık kaynak R diliyle ücretsiz olarak karşıdan en kapsamlı R arşiv ağ (CRAN) konumunda kullanılabilir [ https://www.r-project.org/ ](https://www.r-project.org/). Windows, Macos ve Linux/UNIX için kullanılabilen yüklemeleri vardır. Yakındaki bir yansıtma'ı seçip indirme yönergeleri izleyin. Ayrıca, çok sayıda kullanışlı analiz ve veri işleme paketleri CRAN içerir.

RStudio için yeni başladıysanız, indirme ve Masaüstü sürümünü yüklemeniz gerekir. RStudio yüklemeler için Windows, Mac OS ve Linux/UNIX bulabilirsiniz http://www.rstudio.com/products/RStudio/. Masaüstü makinenizde Rstudio'yu yükleme için sağlanan yönergeleri izleyin.  

RStudio öğretici bir giriş kullanılabilir [RStudio IDE kullanarak](https://support.rstudio.com/hc/sections/200107586-Using-RStudio).

Ben de RStudio kullanma ile ilgili bazı ek bilgiler sunar [RStudio belgeler için kılavuz](#appendixa) aşağıda.  

## <a id="scriptmodule"></a>R betiği yürütme modülün içine ve dışına veri alma

Bu bölümde Küme içi ve dışı veri alma nasıl ele alınacaktır [R betiği yürütme] [ execute-r-script] modülü. Şimdi küme içi ve dışı okuma çeşitli veri türlerini işlemek nasıl gözden [R betiği yürütme] [ execute-r-script] modülü.

Bu bölüm için tam kodu [MachineLearningSamples-not defterlerini/studio-samples](https://github.com/Azure-Samples/MachineLearningSamples-Notebooks/tree/master/studio-samples).

### <a name="load-and-check-data-in-machine-learning-studio"></a>Yük ve Machine Learning Studio'da data denetimi

#### <a id="loading"></a>Dataset yükleme

Yükleyerek başlayacağız **csdairydata.csv** Azure Machine Learning Studio'ya dosya.

1. Azure Machine Learning Studio ortamınızı başlatın.
1. Seçin **+ yeni** seçin ve ekranın sol alt köşesindeki **veri kümesi**.
1. Seçin **yerel dosyadan**, ardından **Gözat** dosyayı seçin.
1. Seçtiğinizden emin olun **genel CSV dosyası (.csv) olan üstbilgiyle** veri kümesi türü.
1. Onay işaretini seçin.
1. Veri kümesi karşıya yüklendikten sonra seçerek yeni veri kümesi görürsünüz **veri kümeleri** sekmesi.  

#### <a name="create-an-experiment"></a>Deneme oluşturma

Machine Learning Studio'da sahip olduğumuz bazı veriler, analiz yapmak için bir deneme oluşturmak gerekir.  

1. Seçin **+ yeni** en düşük seçeneğini ve **deneme**, ardından **boş deneme**.
1. Seçme ve değiştirme, denemenizi adlandırabilirsiniz **deneme oluşturuldu...**  sayfanın üst kısmındaki başlık. Örneğin, kendisine değiştirme **CA günlük analizi**.
1. Deneme sayfanın sol tarafında genişletmek **kaydedilmiş veri kümeleri**, ardından **My veri kümeleri**. Görmelisiniz **cadairydata.csv** daha önce yüklediğiniz.
1. Sürükle ve bırak **csdairydata.csv dataset** denemeyi sürükleyin.
1. İçinde **arama öğeleri deneme** kutusunun sol bölmesinde, türü en üstündeki [R betiği yürütme][execute-r-script]. Arama listede modülü görürsünüz.
1. Sürükle ve bırak [R betiği yürütme] [ execute-r-script] modülü, modül paleti.  
1. Çıkışını **csdairydata.csv dataset** en soldaki giriş (**Dataset1**), [R betiği yürütme][execute-r-script].
1. **'Kaydet' seçilecek unutmayın!**  

Bu noktada denemenizi Şekil 3 gibi görünmelidir.

![CA günlük analizi veri kümesini ve R betiği yürütme modülü ile denemeler yapın](./media/r-quickstart/fig3.png)

*Şekil 3. CA günlük analizi veri kümesini ve R betiği yürütme modülü ile denemeler yapın.*

#### <a name="check-on-the-data"></a>Veriyi denetle

Diyelim ki bizim denemenin yüklendik veri göz vardır. Deneme çıkışı seçin **cadairydata.csv dataset** seçip **görselleştirme**. Şekil 4 gibi bir şey görmeniz gerekir.  

![Veri kümesinin cadairydata.csv özeti](./media/r-quickstart/fig4.png)

*Şekil 4. Cadairydata.csv dataset özeti.*

Bu görünümde birçok yararlı bilgi görüyoruz. Bu veri kümesinin ilk birkaç satır görebiliriz. Bir sütun seçtiğinizde, istatistikleri bölüm sütunu hakkında daha fazla bilgi gösterir. Örneğin, özellik türü satır bize atanmış bir sütun için Azure Machine Learning Studio'da hangi veri türlerini gösterir. Herhangi bir önemli iş yapmak başlamadan önce bu gibi hızlı bir bakış sahip bir iyi sağlamlık olup olmadığını denetler.

### <a name="first-r-script"></a>İlk R betiği

Azure Machine Learning Studio'da deneme amaçlı ilk basit bir R betiği oluşturalım. Ben oluşturduktan ve aşağıdaki betiği RStudio içinde test edilmiş.  

```R
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
```

Şimdi bu betiği Azure Machine Learning Studio'ya aktarma gerekecek. Yalnızca kesin ve yapıştırın. Ancak, bu durumda, miyim my R betiği bir zip dosyasına aktarın.

### <a name="data-input-to-the-execute-r-script-module"></a>R betiği yürütme modülüne veri girişi

Şimdi girişleri göz sahip [R betiği yürütme] [ execute-r-script] modülü. Bu örnekte biz California Süt verileri okuyacak [R betiği yürütme] [ execute-r-script] modülü.  

İçin üç olası girişler vardır [R betiği yürütme] [ execute-r-script] modülü. Bağlı olarak uygulamanızı herhangi bir ya da bu girişlerin tümünü kullanabilir. Ayrıca, hiçbir giriş hiç aldığı bir R betiği kullanmak mükemmel bir şekilde uygun olur.  

Soldan sağa doğru giden, bu girişlerin her göz atalım. Girişlerin her biri adlarını giriş imleci yerleştirerek ve araç ipucu okuyarak görebilirsiniz.  

#### <a name="script-bundle"></a>Betik paketi

Betik paketi girişi sağlayan bir zip dosyası olarak içeriği geçirmeniz [R betiği yürütme] [ execute-r-script] modülü. R kodunuza ZIP dosyasının içeriğini okumak için aşağıdaki komutlardan birini kullanabilirsiniz.

```R
source("src/yourfile.R") # Reads a zipped R script
load("src/yourData.rdata") # Reads a zipped R data file
```

> [!NOTE]
> Azure Machine Learning Studio zip dosyaları src içine oldukları gibi davranır / dizin, bu nedenle, dosya adı bu dizin adı ön eki gerekir. Örneğin, zip dosyaları içeren `yourfile.R` ve `yourData.rdata` zip dosyasının kökünde, bu dosyaların başvuracağını `src/yourfile.R` ve `src/yourData.rdata` kullanırken `source` ve `load`.

Yükleme veri kümelerinde zaten ele aldığımız [için veri kümesi](#loading). Oluşturulan ve önceki bölümde gösterilenle R betiği test sonra aşağıdakileri yapın:

1. R betiği içine Kaydet bir. R dosyası. Betik dosyamı "simpleplot. çağrı "R". İçeriği aşağıda verilmiştir.

   ```R
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
   ```

1. Bir zip dosyası oluşturun ve betiğinizi Bu zip dosyasına kopyalayın. Windows üzerinde dosyaya sağ tıklayın ve seçin **göndermek**, ardından **sıkıştırılmış klasörü**. Bu "simpleplot. içeren yeni bir ZIP dosyası oluşturur R"dosyası.

1. Dosyanıza ekleyin **veri kümeleri** Machine Learning Studio'da türü olarak belirterek **zip**. Şimdi, veri kümeleri zip dosyası görmeniz gerekir.

1. Zip dosyasından sürükleyip **veri kümeleri** üzerine **ML Studio tuval**.

1. Çıkışını **zip veri** simgesine **betik paketi** , giriş [R betiği yürütme] [ execute-r-script] modülü.

1. Tür `source()` işlevi için kod penceresine zip dosyası adınızla [R betiği yürütme] [ execute-r-script] modülü. My durumda yazmış olduğum `source("src/simpleplot.R")`.  

1. Seçtiğinizden emin olun **Kaydet**.

Bu adımlar tamamlandıktan sonra [R betiği yürütme] [ execute-r-script] modülü yürütülecek R betiğini zip dosyasında deneme çalıştırıldığında. Bu noktada denemenizi Şekil 5 gibi görünmelidir.

![Sıkıştırılmış bir R betiği kullanarak denemeler yapın](./media/r-quickstart/fig6.png)

*Şekil 5. Sıkıştırılmış bir R betiği kullanarak denemeler yapın.*

#### <a name="dataset1"></a>DataSet1

Dikdörtgen bir veri tablosu Dataset1 giriş kullanarak R kodunuzdaki geçirebilirsiniz. Basit betiğimizi içinde `maml.mapInputPort(1)` işlevi, 1 bağlantı noktasından verileri okur. Bu veriler daha sonra kodunuzda bir dataframe değişken adı atanır. Basit betiğimizi kodun ilk satırını atamayı gerçekleştirir.

```R
cadairydata <- maml.mapInputPort(1)
```

Denemenizi seçerek yürütme **çalıştırma** düğmesi. Yürütme sona erdiğinde seçin [R betiği yürütme] [ execute-r-script] modülü ve ardından **çıkış Günlüğü Görüntüle** özellikleri bölmesinde. Yeni bir sayfa içeren dosyanın içeriğini gösteren tarayıcınızda görüntülenmesi gerekir. Aşağı kaydırırsanız aşağıdaki gibi görmeniz gerekir.

    [ModuleOutput] InputDataStructure
    [ModuleOutput]
    [ModuleOutput] {
    [ModuleOutput]  "InputName":Dataset1
    [ModuleOutput]  "Rows":228
    [ModuleOutput]  "Cols":9
    [ModuleOutput]  "ColumnTypes":System.Int32,3,System.Double,5,System.String,1
    [ModuleOutput] }

Sayfa aşağı küçüldükleri aşağıdaki gibi görünür sütunlar hakkında daha ayrıntılı bilgi olur.

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

Bu sonuçlar çoğunlukla 228 gözlemleri ve 9 sütunlarda veri çerçevesi ile beklendiği gibi olur. Sütun adları, R veri türü ve her bir sütunun bir örnek görebiliriz.

> [!NOTE]
> Bu aynı çıktılar R cihazında çıktısından rahatça kullanılabilir [R betiği yürütme] [ execute-r-script] modülü. Çıktıları ele alınacaktır [R betiği yürütme] [ execute-r-script] sonraki bölümde modülü.  

#### <a name="dataset2"></a>Dataset2

Aynı olan Dataset1 Dataset2 giriş, davranıştır. Bu giriş kullanarak R kodunuzdaki bir ikinci dikdörtgen bir veri tablosu geçirebilirsiniz. İşlev `maml.mapInputPort(2)`, 2 bağımsız değişkeniyle, bu veri iletmek için kullanılır.  

### <a name="execute-r-script-outputs"></a>R betiği çıkışları yürütün

#### <a name="output-a-dataframe"></a>Bir dataframe çıktı

Kullanarak sonucu Dataset1 bağlantı noktası üzerinden dikdörtgen tablo olarak R dataframe içeriğini çıkarabilirsiniz `maml.mapOutputPort()` işlevi. Basit R betiğimizi bu aşağıdaki satırı tarafından gerçekleştirilir.

```
maml.mapOutputPort('cadairydata')
```

Denemeyi çalıştırdıktan sonra sonuç Dataset1 çıkış bağlantı noktasını seçin ve ardından **Görselleştir**. Şekil 6 gibi'bir şey görmeniz gerekir.

![Çıkış California Süt veri Görselleştirme](./media/r-quickstart/fig7.png)

*Şekil 6. Çıkış California Süt veri görselleştirme.*

Bu çıkış, tam olarak beklenen şekilde giriş, aynı arar.  

### <a name="r-device-output"></a>R cihaz çıktısı

Cihaz çıktısı [R betiği yürütme] [ execute-r-script] modülü iletileri ve grafik çıkış içerir. Hem standart çıktı ve standart hata iletilerini gelen R R cihazında çıkış bağlantı noktasına gönderilir.  

R cihazında çıkışı görüntülemek için bağlantı noktasını seçin ve ardından **Görselleştir**. Standart çıktı ve standart hata Şekil 7'de R betikten görüyoruz.

![Standart çıktı ve standart hata R cihazında bağlantı noktasından](./media/r-quickstart/fig8.png)

*Şekil 7. Standart çıktı ve standart hata R cihazında bağlantı noktasından.*

Bkz: Şekil 8'de R betiğimizi grafik çıktısı biz kaydırma.  

![Grafik çıktısı R cihazında bağlantı noktası](./media/r-quickstart/fig9.png)

*Şekil 8. Grafik R cihazında bağlantı noktasından çıktı.*  

## <a id="filtering"></a>Verileri filtreleme ve dönüştürme

Bu bölümde biz bazı temel veri filtreleme ve California Süt veriler üzerinde dönüşüm işlemleri gerçekleştirir. Bu bölümün sonunda biz verileri bir analitik model oluşturmak için uygun bir biçimde olacaktır.  

Daha açık belirtmek gerekirse, bu bölümde biz birkaç ortak veri temizleme ve dönüştürme görevleri gerçekleştireceksiniz: değer dönüştürmeleri ve tür dönüştürme, yeni hesaplanan sütunlar ekleyerek veri çerçevelerini üzerinde filtreleme. Bu arka plan birçok çeşitlemeleri gerçek sorunla başa çıkmanıza yardımcı olmalıdır.

Bu bölüm için tam R kodu [MachineLearningSamples-not defterlerini/studio-samples](https://github.com/Azure-Samples/MachineLearningSamples-Notebooks/tree/master/studio-samples).

### <a name="type-transformations"></a>Tür dönüştürmeleri

Biz R kodun içine California Süt verileri okuyabilir göre [R betiği yürütme] [ execute-r-script] modülü, ihtiyacımız sütunlardaki verileri hedeflenen türünü ve biçimini olduğundan emin olmak.  

R gerektiği gibi başka bir veri türleri zorlanır anlamına gelir dinamik olarak yazılmış bir dildir. R atomik veri türü sayısal, mantıksal ve karakter içerir. Faktörü türü sütunları ise kategorik veriler sıkı bir şekilde depolamak için kullanılır. Başvuruları veri türleri hakkında daha fazla bilgi bulabilirsiniz [daha fazla okuma](#appendixb) aşağıda.

Bir dış kaynaktan R içine tablo verisi okurken, her zaman sütunlardaki elde edilen türlerini denetlemek için iyi bir fikirdir. Bir sütun türü bir karakterin isteyebilirsiniz, ancak çoğu durumda bu ya da tam tersi öğe olarak görünür. Diğer durumlarda bir sütun olması düşündüğünüz sayısal karakter verileri tarafından örneğin gösterilir '1,23' yerine 1,23 olarak kayan nokta sayısı.  

Neyse ki, eşleme mümkün olduğu sürece bir türden diğerine dönüştürme kolaydır. Örneğin, bir sayısal değerde 'Nevada'ya' dönüştürülemiyor, ancak bir faktör (kategorik değişken) dönüştürebilirsiniz. Başka bir örnek olarak, bir karakter '1' ya da bir faktör, sayısal 1 dönüştürebilirsiniz.  

Bu dönüştürmeleri hiçbiri için söz dizimi basittir: `as.datatype()`. Bu tür dönüştürme işlevleri arasında şunlar yer alır.

* `as.numeric()`
* `as.character()`
* `as.logical()`
* `as.factor()`

Biz önceki bölümde giriş sütunların veri türlerini bakarak: türü sayısal, türü karakter ' Month' etiketli bir sütun dışında tüm sütunları ilgilidir. Şimdi bu dönüştürme için bir faktör ve test sonuçları.  

Ben dağılım grafiği matris oluşturulan ve 'Month' sütunu için bir faktör dönüştürme bir satır eklenir satır sildiniz. My deneme miyim yalnızca Kes ve R kodu kod penceresine yapıştırın [R betiği yürütme] [ execute-r-script] modülü. Ayrıca zip dosyasını güncelleştirin ve Azure Machine Learning Studio'da karşıya, ancak bu adımları götürür.  

```R
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
```

Şimdi bu kod yürütün ve R betiği için çıktı günlüğüne bakın. Şekil 9'da ilgili verileri günlüğünden gösterilir.

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

*Şekil 9. Veri çerçevesi ile bir faktör değişken özeti.*

Ay türü artık yazmalıdır '**14 düzey çakışabilmektedir faktörü**'. Yılın yalnızca 12 ay sonra bir sorun budur. Ayrıca, görmek için kontrol edebilirsiniz türünde **Görselleştir** sonuç kümesini bağlantı noktasıdır '**kategorik**'.

'Month' sütunu sistematik olarak kodlanmış olmayan bir sorundur. Bazı durumlarda bir ay Nisan denir ve diğer Apr kısaltılır. 3 karakter dizesine kırpma tarafından Biz bu sorunu çözebilirsiniz. Kod satırı aşağıdaki gibi görünür:

```R
## Ensure the coding is consistent and convert column to a factor
cadairydata$Month <- as.factor(substr(cadairydata$Month, 1, 3))
```

Denemeyi yeniden çalıştırmanız ve çıkış günlüğü görüntüleyin. Şekil 10'da beklenen sonuçları gösterilmektedir.  

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

*Şekil 10. Veri çerçevesi ile faktörü düzeyleri doğru sayıda özeti.*

İstenen 12 düzeyleri faktörü değişkeni artık var.

### <a name="basic-data-frame-filtering"></a>Çerçeve temel verileri filtreleme

Güçlü filtreleme yetenekleri R veri çerçevelerini destekler. Veri kümeleri, satırlar veya sütunlar üzerinde mantıksal filtrelerini kullanarak alt kümelenmiş olabilir. Çoğu durumda, karmaşık filtre ölçütlerini gerekli olacaktır. Başvuruları [daha fazla okuma](#appendixb) aşağıda dataframes filtreleme kapsamlı örnekler içerir.  

Bir bit olduğundan filtre kümemizi üzerinde yapmamız gerekir. Sütunları cadairydata dataframe bakarsanız, iki gereksiz sütunları görürsünüz. İlk sütun, yalnızca çok kullanışlı olmayan bir satır numarası tutar. İkinci sütunda, Year.Month, yedekli bilgiler içerir. Aşağıdaki R kodunu kullanarak Biz bu sütunların kolayca dışlayabilirsiniz.

> [!NOTE]
> Artık bu bölümde, ı yalnızca, ı ekleme ek kod gösterir [R betiği yürütme] [ execute-r-script] modülü. Her yeni satır Ekle **önce** `str()` işlevi. Azure Machine Learning Studio'da sonuçlarımı doğrulamak için bu işlevi kullanıyorum.

R kodumu aşağıdaki satırı ekleyebilirim [R betiği yürütme] [ execute-r-script] modülü.

```R
# Remove two columns we do not need
cadairydata <- cadairydata[, c(-1, -2)]
```

Denemenizde bu kodu çalıştırmak ve sonucu çıktı günlüğüne bakın. Şekil 11'de bu sonuçları gösterilmektedir.

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

*Şekil 11. İki sütunlu bir veri çerçevesi özetini kaldırıldı.*

Güzel bir haberimiz var! Biz, beklenen sonuçları alın.

### <a name="add-a-new-column"></a>Yeni bir sütun ekleme

Zaman serisi modelleri oluşturmak için zaman serisi başladığından bu yana ay içeren bir sütun kullanışlı olacaktır. 'Month.Count' yeni bir sütun oluşturacağız.

Size sunduğumuz ilk basit işlevi oluşturur kodu düzenlenmesine yardımcı olmak için `num.month()`. Biz, ardından veri çerçevesi içinde yeni bir sütun oluşturmak için bu işlevi uygulanır. Yeni kod aşağıdaki gibidir.

```R
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
```

Şimdi güncelleştirilmiş denemeyi çalıştırma ve sonuçları görmek için çıkış günlüğü kullanın. Şekil 12'deki bu sonuçları gösterilmektedir.

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

*Şekil 12. Veri çerçevesi ile ek sütun özeti.*

Her şey çalışıyor gibi görünüyor. Bizim veri çerçevesi içinde beklenen değerlerle yeni bir sütun sahibiz.

### <a name="value-transformations"></a>Değer dönüştürmeleri

Bu bölümde biz bazı bizim dataframe sütunlarını değerlere göre bazı basit dönüşümleri gerçekleştirir. R dili neredeyse rastgele değer dönüştürmeleri destekler. Başvuruları [daha fazla okuma](#appendixb) aşağıda kapsamlı örnekler içerir.

Bizim dataframe özetlerini değerleri bakarsanız tek bir şey burada görmeniz gerekir. Daha fazla ICE cream sütlü daha California'da oluşturulur? Bu hiçbir mantıklı gibi Hayır, Elbette bazı kalanımız ICE cream lovers bu olgu üzücü olmayabilir. Birimleri farklıdır. Fiyat kalanımız birimlerinde pound, olan 1 M birimleriyle ABD pound, ICE cream 1.000 birimlerinde BİZE galon ve cottage peynirlerine ayırıyor 1.000 ABD pound birimlerinde sütlü olur. ICE cream galon başına yaklaşık 6.5 pound ağırlıklandıran varsayıldığında, biz bu değerleri tüm eşit birimleri 1.000 pound cinsinden şekilde dönüştürmek için çarpma kolayca yapabilirsiniz.

Tahmin modelimiz için çarpma modeli eğilim ve bu verileri dönemsel düzeltilmesi için kullanırız. Günlük dönüştürme bu sürecini kolaylaştırır bir Doğrusal model kullandık olanak sağlıyor. Biz çarpan uygulandığı aynı işlevde günlük dönüşümü uygulayabilirsiniz.

Aşağıdaki kodda, ı yeni bir işlev tanımla `log.transform()`ve sayısal değerler içeren satırları uygulayabilirsiniz. R `Map()` işlevi uygulamak için kullanılan `log.transform()` seçilen sütunlarda veri çerçevesini işlevi. `Map()` benzer `apply()` ancak birden fazla işlev bağımsız değişkenlerinin listesi için verir. Çarpanları listesini ikinci bağımsız değişkeni sağladığı Not `log.transform()` işlevi. `na.omit()` İşlevi değil sahibiz eksik veya tanımlanmamış değerler veri çerçevesi emin olmak için bir temizleme bit olarak kullanılır.

```R
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
```

Tam anlamıyla bir bit gerçekleşmesini içinde yoktur `log.transform()` işlevi. Bu kod çoğunu bağımsız değişkenleriyle olası sorunları veya hala hesaplamalar sırasında oluşabilecek özel durumları uğraşmanızı denetleniyor. Bu kod yalnızca birkaç satır gerçekten hesaplamalar gerçekleştirin.

Hata işleme devam etmesini engelleyen tek bir işlevin amacı, savunma programlama önlemektir. Uzun süre çalışan analiz ani bir hata, kullanıcılar için oldukça can sıkıcı olabilir. Bu durumu önlemek için varsayılan dönüş değerleri, aşağı akış işleme zarar sınırlar seçilmelidir. Bir ileti, bir sorun oluştu, kullanıcıları uyarmak üzere de oluşturulur.

R ile savunma programlama için kullanılmaz, bu kod biraz zor görünebilir. Ben temel adımlarda size yol gösterir:

1. Dört iletileri oluşan bir vektörü tanımlanır. Bu iletiler, bazı olası hataları ve şu kodla oluşabilecek özel durumları hakkında bilgi iletişim kurmak için kullanılır.
2. Ben, her örneği için ad değeri döndürür. Daha az yan etkileri olabilir diğer birçok olasılık vardır. Bir vektör sıfır ya da özgün giriş vektör, örneğin geri.
3. Denetimler, işleve bağımsız değişkenler üzerinde çalıştırılır. Her durumda, bir hata algılandığında, varsayılan değeri döndürülür ve tarafından üretilen bir ileti `warning()` işlevi. Kullanmakta olduğum `warning()` yerine `stop()` gibi ikinci tam olarak ne önlemek çalışıyorum yürütme sona erer. Ben bu kod bu örnekte olduğu gibi bir yordam stili karmaşık ve belirsiz olduğu görülüyor işlevsel bir yaklaşım yazdığınızı unutmayın.
4. Günlük hesaplamalar sarmalanır `tryCatch()` böylece özel durumları işleme ani bir durmasına neden olmaz. Olmadan `tryCatch()` yapan bir durdurma sinyali işlevleri sonucunda R tarafından gerçekleştirilen en hataları.

Denemenizde bu R kodunu yürütün ve çıktılar göz içeren dosyanız. Şekil 13'te gösterildiği gibi artık günlük dört sütun dönüştürülen değerler görürsünüz.

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

*Şekil 13. Dönüştürülmüş veri çerçevesi değerleri özeti.*

Dönüştürülen değerler görürüz. Artık sütlü üretim size artık günlük ölçekli olarak aradığınız geri çağırma tüm diğer Süt ürün üretim, büyük ölçüde aşıyor.

Bu noktada verilerimizi temizlenir ve bazı modelleme için hazırız. Sonuç veri kümesini çıktısı için Özet görselleştirme bakarak bizim [R betiği yürütme] [ execute-r-script] modülü, 'Month' sütunu olan 'Kategorik' 12 benzersiz değerlerle yeniden istediğimiz şekilde görürsünüz.

## <a id="timeseries"></a>Zaman serisi nesneleri ve bağıntı analizi

Bu bölümde ediyoruz birkaç temel R zaman serisi nesneleri keşfedin ve bazı değişkenler arasındaki bağıntıları analiz edin. Hedefimiz, birkaç aksamalar ikili bağıntı bilgileri içeren bir dataframe çıkış sağlamaktır.

Bu bölüm için tam R kodu [MachineLearningSamples-not defterlerini/studio-samples](https://github.com/Azure-Samples/MachineLearningSamples-Notebooks/tree/master/studio-samples).

### <a name="time-series-objects-in-r"></a>R ile zaman serisi nesneleri

Önce de belirtildiği gibi zaman serileri zamanına göre dizine veri değerlerini bir dizi değildir. R zaman serisi nesneleri oluşturmak ve zaman dizini yönetmek için kullanılır. Zaman serisi nesneleri kullanarak çeşitli avantajları vardır. Zaman serisi nesneleri nesnesinde kapsüllenir zaman serisi dizin değerleri yönetme birçok ayrıntılarından boş. Ayrıca, zaman serisi nesneleri çizim için çoğu zaman serisi yöntemlerini kullanmayı yazdırma, modelleme, vb. sağlar.

POSIXct zaman serisi sınıfı oldukça basittir ve yaygın olarak kullanılır. Bu zaman serisi ölçümler zaman dönem, 1 Ocak 1970 başlangıcından sınıfı. Bu örnekte POSIXct zaman serisi nesneleri kullanacağız. Diğer yaygın olarak kullanılan R zaman serisi nesne sınıfları şunlardır: zoo ve xts, Genişletilebilir zaman serisi.

### <a name="time-series-object-example"></a>Zaman serisi nesnesi örneği

Bizim örneğimizde ile başlayalım. Sürükle ve bırak bir **yeni** [R betiği yürütme] [ execute-r-script] denemenizi modüle. Varolan sonucu Dataset1 çıkış bağlantı noktasına bağlanmak [R betiği yürütme] [ execute-r-script] Dataset1 modülüne giriş bağlantı noktasına yeni [R betiği yürütme] [ execute-r-script] modülü.

Örnek ilerledikçe miyim ilk örnek için yaptığınız gibi bazı noktalarda miyim yalnızca artımlı ek R kod satırlarını her bir adımı gösterilir.  

#### <a name="reading-the-dataframe"></a>Veri çerçevesi okuma

İlk adım olarak şimdi bir dataframe okuyun ve beklenen sonuçları aldığımız emin olun. Aşağıdaki kod, iş yapmanız gerekir.

```R
# Comment the following if using RStudio
cadairydata <- maml.mapInputPort(1)
str(cadairydata) # Check the results
```

Şimdi, denemeyi çalıştırın. Yeni bir R betiği yürütme şekil günlüğü Şekil 14 gibi görünmelidir.

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

*Şekil 14. R betiği yürütme modülünde dataframe özeti.*

Bu, beklenen türleri ve biçimi verilerdir. 'Month' sütun türü faktörü ve düzeyleri beklenen sayıda olduğundan unutmayın.

#### <a name="creating-a-time-series-object"></a>Zaman serisi nesnesi oluşturma

Zaman serisi nesnesi için sunduğumuz dataframe eklemek ihtiyacımız var. Geçerli kod POSIXct sınıfının yeni bir sütun ekler aşağıdaki ile değiştirin.

```R
# Comment the following if using RStudio
cadairydata <- maml.mapInputPort(1)

## Create a new column as a POSIXct object
Sys.setenv(TZ = "PST8PDT")
cadairydata$Time <- as.POSIXct(strptime(paste(as.character(cadairydata$Year), "-", as.character(cadairydata$Month.Number), "-01 00:00:00", sep = ""), "%Y-%m-%d %H:%M:%S"))

str(cadairydata) # Check the results
```

Şimdi, günlüğünü denetleyin. Bu Şekil 15 gibi görünmelidir.

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

*Şekil 15. Veri çerçevesi ile zaman serisi nesnesi özeti.*

Yeni bir sütun aslında POSIXct sınıfıdır özeti görebiliriz.

### <a name="exploring-and-transforming-the-data"></a>Keşfetmek ve verileri dönüştürme

Bu veri kümesinde değişkenlerinin bazıları araştıralım. Bir dağılım grafiği matrisi, hızlı bir görünüm oluşturmak için iyi bir yoludur. Değiştirme `str()` aşağıdaki satırı önceki R koduyla işlevi.

```R
pairs(~ Cotagecheese.Prod + Icecream.Prod + Milk.Prod + N.CA.Fat.Price, data = cadairydata, main = "Pairwise Scatterplots of dairy time series")
```

Bu kodu çalıştırmak ve ne olacağına bakalım. R cihazında bağlantı noktalarından üretilen çizim Şekil 16 gibi görünmelidir.

![Seçilen değişkenleri dağılım grafiği Matrisi](./media/r-quickstart/fig17.png)

*Şekil 16. Seçilen değişkenleri matrisi dağılım grafiği.*

Bu değişkenler ilişkileri bazı garip görünüşlü yapısı vardır. Belki de bu veriler, eğilimleri ve biz değişkenleri standartlaşmış değil, olgu ortaya çıkar.

### <a name="correlation-analysis"></a>Bağıntı analizi

Bağıntı analiz gerçekleştirmek için XML'deki eğilimi hem değişkenlerin standart hale getirmek ihtiyacımız var. R yalnızca kullanabiliriz `scale()` merkezleri hem değişkenleri ölçeklenen, işlev. Bu işlev de daha hızlı çalışabilir. Ancak, savunma ölçeklenebilirliğinden örneği r'de göstermek istiyorsunuz

`ts.detrend()` Aşağıda gösterilen işlev bu işlemlerin her ikisi de gerçekleştirir. Aşağıdaki iki kod satırlarını XML'deki veri eğilimi ve değerlerin standart hale getirin.

```R
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
```

Tam anlamıyla bir bit gerçekleşmesini içinde yoktur `ts.detrend()` işlevi. Bu kod çoğunu bağımsız değişkenleriyle olası sorunları veya hala hesaplamalar sırasında oluşabilecek özel durumları uğraşmanızı denetleniyor. Bu kod yalnızca birkaç satır gerçekten hesaplamalar gerçekleştirin.

Zaten değer dönüştürmeleri savunma programlamada bir örneği ele almıştık. Her iki hesaplama bloğu içinde sarmalanmış `tryCatch()`. Bazı hatalar için özgün giriş vektör döndürülecek mantıklı ve diğer durumlarda, ı sıfır, bir vektör döndürür.  

Kümenin doğrusal regresyonunu XML'deki eğilimleri belirleme için kullanılan bir zaman serisi gerileme olduğuna dikkat edin. Zaman serisi nesnesi tahmin unsuru değişkendir.  

Bir kez `ts.detrend()` tanımlanan değişkenler bizim dataframe gösterdiğiniz ilgi için biz bunu uygulayabilirsiniz. Biz tarafından oluşturulan sonuç listesini coerce gerekir `lapply()` kullanarak verileri veri çerçevesi için `as.data.frame()`. Savunma yönlerini nedeniyle `ts.detrend()`, işlem değişkenlerden biri başarısız olduğunda, diğerleri doğru işlenmesini engellemez.  

Son kod satırının, ikili bir dağılım grafiği oluşturur. R kodunu çalıştırdıktan sonra dağılım grafiği sonuçlarını şekil 17'de gösterilmektedir.

![XML'deki koleksiyonunuzdaki ve standartlaştırılmış bir zaman serisinin ikili dağılım grafiği](./media/r-quickstart/fig18.png)

*Şekil 17. XML'deki koleksiyonunuzdaki ve standartlaştırılmış bir zaman serisinin ikili dağılım grafiği.*

Bu Şekil 16'da gösterilen bu sonuçları karşılaştırabilirsiniz. Kaldırılan eğilim ve standartlaştırılmış değişkenleri ile bu değişkenler arasındaki ilişkiler çok daha az yapısında görüyoruz.

Bağıntılar R ccf nesneler olarak hesaplamak için kod aşağıdaki gibidir.

```R
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
```

Bu kodu çalıştırmadan Şekil 18'de gösterilen günlük üretir.

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

*Şekil 18. Ccf listesini ikili bağıntı analiz nesneleri.*

Her gecikmesi için bir bağıntı değer yoktur. Bu bağıntı değerleri hiçbiri önemli kadar büyük değil. Biz size her bir değişken bağımsız olarak modelleyebilir, bu nedenle tamamlanabilmesi.

### <a name="output-a-dataframe"></a>Bir dataframe çıktı
Biz, R ccf nesnelerin bir listesini ikili bağıntılar hesaplanan. Sonuç veri kümesinin çıkış bağlantı noktasına bir dataframe gerçekten gerektirir. Bu sorunun bir bit sunar. Daha fazla ccf nesnenin kendisi bir liste değildir ve yalnızca değerleri bu liste öğesinin ilk öğesinin çeşitli aksamalar, bağıntılar istiyoruz.

Aşağıdaki kodu, kendilerini listeleridir ccf nesnelerin listeden gecikme değerleri ayıklar.

```R
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
## The following line works only in Azure Machine Learning Studio
## When running in RStudio, this code will result in an error
#maml.mapOutputPort('outframe')
```

Kodun ilk satırını biraz zor ve bazı açıklaması, anlamanıza yardımcı olabilir. Inside out çalışma şunları sunuyoruz:

1. '**[[**'Bağımsız değişkeni işlecini'**1**' aksamalar, bağıntılar vektörü ccf nesne listesinin ilk öğeyi seçer.
2. `do.call()` İşlevi uygular `rbind()` işlevi listedeki öğeleri üzerinde döndürür tarafından `lapply()`.
3. `data.frame()` İşlevi tarafından üretilen sonuç olacak şekilde zorlar `do.call()` bir veri çerçevesi için.

Satır adları bir veri çerçevesi sütunda olduğunu unutmayın. Elde edilen çıktı, adları bunu korur satır yapılması [R betiği yürütme][execute-r-script].

Şekil 19'gösterilen çıkışı üretir kodu çalıştıran, ı **Görselleştir** sonuç veri kümesi bağlantı noktasında çıktı. Satır ilk sütunda, beklendiği gibi adlarıdır.

![Sonuç çıktısı bağıntı analiz](./media/r-quickstart/fig20.png)

*Şekil 19. Bağıntı analiz çıktı sonuçları.*

## <a id="seasonalforecasting"></a>Zaman serisi örnek: dönemsel tahmin

Verilerimizi artık analiz için uygun bir biçimde ve değişkenleri arasında önemli hiçbir bağıntılar vardır belirledik. Şimdi ilerleyelim ve bir zaman serisi tahmin modeli eğitir oluşturun. Bu modeli kullanarak biz California sütlü üretim 2013 12 ayı için tahmin.

Tahmin modelimizi, iki bileşen, bir eğilim bileşeni ve dönemsel bileşen olacaktır. Tam tahmin bu iki bileşenin ürünüdür. Bu tür bir model çarpma model olarak bilinir. Alternatif eklenebilir bir modeldir. Günlük dönüştürme zaten bu analiz tractable getiren ilgi değişkenlere uyguladınız.

Bu bölüm için tam R kodu [MachineLearningSamples-not defterlerini/studio-samples](https://github.com/Azure-Samples/MachineLearningSamples-Notebooks/tree/master/studio-samples).

### <a name="creating-the-dataframe-for-analysis"></a>Analiz için veri çerçevesi oluşturma

Başlangıç ekleyerek bir **yeni** [R betiği yürütme] [ execute-r-script] denemenizi modülü. Connect **sonuç veri kümesini** varolan çıkış [R betiği yürütme] [ execute-r-script] modülüne **Dataset1** yeni modülünün giriş. Sonuç Şekil 20 gibi görünmelidir.

![Eklenen yeni R betiği yürütme modülü ile deneme](./media/r-quickstart/fig21.png)

*Şekil 20. Eklenen yeni R betiği yürütme modülü ile deneme.*

Olarak yeni tamamladığımız, bağıntı analiziyle POSIXct zaman serisi nesnesi içeren bir sütun eklemek ihtiyacımız var. Aşağıdaki kod, yalnızca bu yapar.

```R
# If running in Machine Learning Studio, uncomment the first line with maml.mapInputPort()
cadairydata <- maml.mapInputPort(1)

## Create a new column as a POSIXct object
Sys.setenv(TZ = "PST8PDT")
cadairydata$Time <- as.POSIXct(strptime(paste(as.character(cadairydata$Year), "-", as.character(cadairydata$Month.Number), "-01 00:00:00", sep = ""), "%Y-%m-%d %H:%M:%S"))

str(cadairydata)
```

Bu kodu çalıştırmak ve günlüğüne bakın. Sonuç, Şekil 21 gibi görünmelidir.

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

*Şekil 21. Bir veri çerçevesi özeti.*

Bu sonuç ile çözümlememiz başlatmak hazırız.

### <a name="create-a-training-dataset"></a>Bir eğitim veri kümesi oluşturma

Oluşturulan veri çerçevesi ile size bir eğitim veri kümesi oluşturmanız gerekir. Bu veriler, 2013 yılının son 12 dışında gözlemleri tüm test kümemizi olduğu içerir. Aşağıdaki alt kümelerini dataframe kod ve çizimler Süt üretimini ve fiyat değişkenlerin oluşturur. Ben bu dört üretim çizimleri oluşturmak ve değişkenleri fiyatı. Anonim bir işlevdir bazı artırmaktadır çizim için tanımlayın ve diğer iki bağımsız değişkenlerle listesi boyunca yineleme yapmak için kullanılan `Map()`. Düşünmek, bir döngü ince burada hakkında deneyimli olduğunuzu için doğru. Ancak, R miyim işlevsel yaklaşım gösteren işlevsel bir dildir olduğundan.

```R
cadairytrain <- cadairydata[1:216, ]

Ylabs  <- list("Log CA Cotage Cheese Production, 1000s lb",
               "Log CA Ice Cream Production, 1000s lb",
               "Log CA Milk Production 1000s lb",
               "Log North CA Milk Milk Fat Price per 1000 lb")

Map(function(y, Ylabs){plot(cadairytrain$Time, y, xlab = "Time", ylab = Ylabs, type = "l")}, cadairytrain[, 4:7], Ylabs)
```

Kod çalıştırma, zaman serisi Şekil 22'de gösterilen R cihazında çıktısından çizer dizi üretir. Zaman ekseni tarihleri yöntemi seriyi çizmek zaman iyi bir yararı birimleri cinsinden olduğunu unutmayın.

![Zaman serisi çizimleri California Süt üretimini ve fiyat verilerin ilk](./media/r-quickstart/unnamed-chunk-161.png)

![Zaman serisi çizimleri California Süt üretimini ve fiyat veri saniyesi](./media/r-quickstart/unnamed-chunk-162.png)

![Zaman serisi çizimleri California Süt üretimini ve fiyat verilerin bir kısmını](./media/r-quickstart/unnamed-chunk-163.png)

![Zaman serisi çizimleri California Süt üretimini ve fiyat veri, dördüncü](./media/r-quickstart/unnamed-chunk-164.png)

*Şekil 22. Zaman serisi çizimleri California Süt üretim ve fiyat verileri.*

### <a name="a-trend-model"></a>Eğilim modeli

Zaman serisi nesne ve verileri göz sahip oluşturulduktan sonra California sütlü üretim verileri için bir eğilim modeli oluşturmak başlayalım. Zaman serisi regresyonla biz bunu yapabilirsiniz. Ancak, size en fazla bir Eğim gerekir ve doğru bir şekilde gözlemlenen eğilimi eğitim verilerini modellemek için ıntercept çizim gelen temizleyin.

Verilerin küçük ölçekli göz önünde bulundurulduğunda, ı RStudio eğilimi için model derleme Kes ve elde edilen modeli Azure Machine Learning Studio'ya yapıştırın. RStudio, bu tür bir etkileşimli analiz için etkileşimli bir ortam sağlar.

Bir ilk deneme ben bir Polinom gerileme powers kadar 3 ile deneyin. Bu tür modelleri aşırı sığdırma gerçek olma tehlikesi yoktur. Bu nedenle, üst sıra koşulları kaçınmanız en iyisidir. `I()` İşlevi boşmuş içeriği yorumu ('olduğundan' içeriği yorumlar) ve tam anlamıyla yorumlanan bir işlev bir regresyon denklemde yazmanıza olanak sağlar.

```R
milk.lm <- lm(Milk.Prod ~ Time + I(Month.Count^2) + I(Month.Count^3), data = cadairytrain)
summary(milk.lm)
```

Bu, aşağıdaki oluşturur.

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

P değerlerden (`Pr(>|t|)`) bu çıkış, kare terim önemli olmayabilir görebiliriz. Kullanacağım `update()` kare terim bırakarak bu modeli değiştirmek için işlevi.

```R
milk.lm <- update(milk.lm, . ~ . - I(Month.Count^2))
summary(milk.lm)
```

Bu, aşağıdaki oluşturur.

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

Bu, daha iyi görünüyor. Önemli olan tüm koşulları. Ancak, 2e-16 değer varsayılan değerdir ve aşırı ciddiye alınmamalıdır.  

Sağlamlık test, bir zaman serisi çizim California Süt üretim veri gösterilen eğilim eğrinin ile olalım. Azure Machine Learning Studio'da aşağıdaki kodu eklediğiniz [R betiği yürütme] [ execute-r-script] model oluşturmak ve bir çizim yapmak için model (RStudio değil). Sonuç, Şekil 23'te gösterilir.

```R
milk.lm <- lm(Milk.Prod ~ Time + I(Month.Count^3), data = cadairytrain)

plot(cadairytrain$Time, cadairytrain$Milk.Prod, xlab = "Time", ylab = "Log CA Milk Production 1000s lb", type = "l")
lines(cadairytrain$Time, predict(milk.lm, cadairytrain), lty = 2, col = 2)
```

![California sütlü üretim verileri ile gösterilen eğilim modeli](./media/r-quickstart/unnamed-chunk-18.png)

*Şekil 23. California sütlü üretim verileri ile gösterilen eğilim modeli.*

Veri eğilimi modeli oldukça iyi en uygun gibi görünüyor. Daha fazla değil gözükmüyor atlayarak sığdırma kanıtı olarak model eğrisindeki tek wiggles gibi.  

### <a name="seasonal-model"></a>Dönemsel modeli

Elle içinde bir eğilim modeliyle push ve dönemsel etkileri eklemek ihtiyacımız var. Yılın ayını Doğrusal model işlevsiz bir değişken olarak aydan aya etkisi yakalamak için kullanacağız. Bir modele faktörü değişkenleri yapılırsa, kesme noktası'nin olmayan hesaplanan gerekir unutmayın. Bunu yaparsanız, formül aşırı belirtilir ve R için istenen faktör bırak ancak ıntercept terimi tutun.

Biz tatmin edici eğilim modeli olduğundan kullanabiliriz `update()` durumda yeni HÜKÜMLERİN mevcut modele eklemek için işlevi. -1 güncelleştirme formülde ıntercept terimi bırakır. RStudio içinde şu anda devam etmesini:

```R
milk.lm2 <- update(milk.lm, . ~ . + Month - 1)
summary(milk.lm2)
```

Bu, aşağıdaki oluşturur.

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

Model artık ıntercept bir dönemi kapsar ve 12 ay önemli faktörler olan görüyoruz. Bu, tam olarak görmek istedik olur.

Dönemsel modeli nasıl çalıştığını görmek için başka bir zaman serisi çizim California Süt üretim veri olalım. Azure Machine Learning Studio'da aşağıdaki kodu eklediğiniz [R betiği yürütme] [ execute-r-script] model oluşturmak ve bir çizim yapmak için.

```R
milk.lm2 <- lm(Milk.Prod ~ Time + I(Month.Count^3) + Month - 1, data = cadairytrain)

plot(cadairytrain$Time, cadairytrain$Milk.Prod, xlab = "Time", ylab = "Log CA Milk Production 1000s lb", type = "l")
lines(cadairytrain$Time, predict(milk.lm2, cadairytrain), lty = 2, col = 2)
```

Azure Machine Learning Studio'da bu kodu çalıştırmadan şekil 24'teki çizim üretir.

![Dönemsel etkileri de dahil olmak üzere modeliyle California sütlü üretim](./media/r-quickstart/unnamed-chunk-20.png)

*Şekil 24. Dönemsel etkileri de dahil olmak üzere modeliyle California sütlü üretim.*

Şekil 24'de gösterilen veri Sığdır yerine encouraging. Hem eğilimi hem de mevsimsel etkisi (aylık değişim) makul arayın.

Modelimiz, başka bir onay, şimdi Kalanlar göz vardır. Aşağıdaki kodu bizim iki modeli tahmin edilen değerleri hesaplar, Dönemsel modelin Kalanlar hesaplar ve ardından bu Kalanlar eğitim verileri çizer.

```R
## Compute predictions from our models
predict1  <- predict(milk.lm, cadairydata)
predict2  <- predict(milk.lm2, cadairydata)

## Compute and plot the residuals
residuals <- cadairydata$Milk.Prod - predict2
plot(cadairytrain$Time, residuals[1:216], xlab = "Time", ylab ="Residuals of Seasonal Model")
```

Kalan çizim şekil 25'gösterilir.

![Eğitim verileri yönelik dönemsel modelin Kalanlar](./media/r-quickstart/unnamed-chunk-21.png)

*Şekil 25. Dönemsel modeli için eğitim verileri Kalanlar.*

Bu Kalanlar makul arayın. Hiçbir modelimiz için hesaba katmaz 2008-2009 krizden etkisini dışında özel bir yapı yoktur özellikle iyi.

Şekil 25'gösterilen çizimi, Kalanlar içinde herhangi bir bağımlı desenleri algılamak için faydalıdır. Açık bir yaklaşım, hesaplama ve kullandım çizme Kalanlar zaman sırada çizim yerleştirir. Öte yandan, ı çizilen, `milk.lm$residuals`, çizim zaman sırada bulunması gereken değil.

Ayrıca `plot.lm()` tanılama çizimler bir dizi oluşturmak için.

```R
## Show the diagnostic plots for the model
plot(milk.lm2, ask = FALSE)
```

Bu kod, Şekil 26 ' gösterilen tanılama çizimler bir dizi oluşturur.

![İlk dönemsel modeli için tanılama çizimler](./media/r-quickstart/unnamed-chunk-221.png)

![Dönemsel modeli için tanılama çizimleri saniyesi](./media/r-quickstart/unnamed-chunk-222.png)

![Dönemsel modeli için tanılama çizimleri, üçüncü](./media/r-quickstart/unnamed-chunk-223.png)

![Dönemsel modeli için tanılama çizimleri, dördüncü](./media/r-quickstart/unnamed-chunk-224.png)

*Şekil 26. Tanılama dönemsel modelini çizer.*

Bu çizimler, ancak hiçbir şey harika sorunu neden tanımlanmış birkaç yüksek oranda etkili noktalar vardır. Normal Q-Q çizim Kalanlar normal için Doğrusal model için önemli bir varsayım Kapat daha da görebiliriz.

### <a name="forecasting-and-model-evaluation"></a>Tahmin ve model değerlendirme

Bizim örneğimizde tamamlamak için yalnızca bir şey daha vardır. Tahminlerini hesaplamak ve hataya karşı gerçek veriler ölçmek ihtiyacımız var. Bizim tahmin 2013 12 ay boyunca olacaktır. Biz bu tahmin eğitim veri kümemizdeki bir parçası değil gerçek veriler için bir hata ölçü hesaplayabilirsiniz. Ayrıca, biz performansı test verilerinin 12 ay eğitim veri 18 yıl karşılaştırabilirsiniz.  

Birkaç ölçüm, zaman serisi modelleri performansını ölçmek için kullanılır. Örneğimizde kök Ortalama kare (RMS) hata kullanacağız. Aşağıdaki işlev iki seriler arasında RMS hata hesaplar.  

```R
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
```

Olduğu gibi `log.transform()` "değer dönüştürmeleri" bölümünde ele aldığımız işlevi birçok hata denetimi ve özel durum kurtarma kodu Bu işlevde yoktur. İşe ilkeleri aynıdır. İçinde sarmalanmış iki yerde çalışmanın `tryCatch()`. İlk olarak, zaman serisi exponentiated,, çünkü değerleri günlükleriyle çalışıyoruz. İkinci olarak, gerçek RMS hata hesaplanır.  

RMS hata ölçmek için bir işlev ile donatılmış, şimdi oluşturun ve RMS hataları içeren bir dataframe çıktı. Tek başına bir eğilim modeli için hüküm ve sezona yönelik faktörleri tam modelin dahil edilir. Aşağıdaki kod, oluşturulmuş iki Doğrusal Model kullanarak işi yapar.

```R
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
```

Bu kodu çalıştırmadan şekil 27 sonuç veri kümesinin çıkış bağlantı noktasında gösterilen bir çıktı üretir.

![RMS hataları modellerine yönelik karşılaştırması](./media/r-quickstart/fig26.png)

*Şekil 27. RMS hataları modellerine yönelik karşılaştırması.*

Bu sonuçlardan modele dönemsel Etkenler ekleme RMS hata önemli ölçüde azaltır olduğunu görüyoruz. Çok edilebileceği RMS eğitim verileri için bir bit'den az tahmin için bir hatadır.

## <a id="appendixa"></a>RStudio belgeler için kılavuz

RStudio oldukça iyi belgelenmiştir. Bazı önemli bölümleri başlamanıza yardımcı olmak için RStudio belgelerine bağlantılar aşağıda verilmiştir.

* **Proje oluşturma** -düzenleyebilir ve Rstudio'yu kullanarak R kodunuzdaki projelere yönetin. Bkz: [kullanarak projeleri](https://support.rstudio.com/hc/articles/200526207-Using-Projects) Ayrıntılar için. Ben, aşağıdaki yönergeleri izleyin ve bu makalede R kod örnekleri için bir proje oluşturun öneririz.  
* **Düzenleme ve R kodu yürüten** -RStudio düzenleme ve R kodunu yürütmek için tümleşik bir ortam sağlar. Bkz: [düzenleme ve kod yürütülürken](https://support.rstudio.com/hc/articles/200484448-Editing-and-Executing-Code) Ayrıntılar için.
* **Hata ayıklama** -RStudio güçlü hata ayıklama özellikleri içerir. Bkz: [RStudio ile hata ayıklama](https://support.rstudio.com/hc/articles/200713843-Debugging-with-RStudio) bu özellikler hakkında daha fazla bilgi için. Kesme noktası özellikleri sorun giderme hakkında daha fazla bilgi için bkz. [kesme noktası sorunlarını giderme](https://support.rstudio.com/hc/articles/200534337-Breakpoint-Troubleshooting).

## <a id="appendixb"></a>Daha fazla bilgi

R programlama Bu öğretici, Azure Machine Learning Studio ile R dili kullanmak ihtiyacınız olan temel kavramları kapsar. R ile ilgili bilgi sahibi değilseniz, iki tanıtımları CRAN'de kullanılabilir:

* [Yeni başlayanlar için R](https://cran.r-project.org/doc/contrib/Paradis-rdebuts_en.pdf) Emmanuel Paradis tarafından başlatmak için iyi bir yerdir.  
* [R giriş](https://cran.r-project.org/doc/manuals/R-intro.html) W. n tarafından Venables et. Al. biraz daha derinlik geçer.

Başlamanıza yardımcı olabilecek R bir çok kitap mevcuttur. Birkaç faydalı bulabilirim şunlardır:

* **R programlama resim: Bir tur, istatistiksel yazılım tasarımı** Norman Matloff tarafından r programlama mükemmel bir giriş niteliğindedir  
* **R Kılavuzu** Paul Teetor tarafından r kullanarak bir sorun ve çözümü yaklaşım sağlar.  
* **R uygulamada** Robert Kabacoff tarafından başka bir kullanışlı tanıtım defteridir. Yardımcısı [hızlı R Web sitesi](https://www.statmethods.net/) faydalı bir kaynaktır.
* **R Inferno** Patrick Burns ile r'de programlamada karşılaşılan zor ve zor konular sayısı ile ilgilenen yayımladım yanıtlamaya yönelik esprili defteridir Ücretsiz kitap kullanılabilir [R Inferno](https://www.burns-stat.com/documents/books/the-r-inferno/).
* R ile Gelişmiş konular hakkında ayrıntılı bir inceleme istediğiniz kitabın göz varsa **Gelişmiş R** Hadley Wickham tarafından. Bu kitap'ın çevrimiçi sürümünü ücretsiz kullanılabilir [ http://adv-r.had.co.nz/ ](http://adv-r.had.co.nz/).

R zaman serisi paketleri kataloğunu bulunabilir [CRAN görev görünümü: Zaman serisi analiz](https://cran.r-project.org/web/views/TimeSeries.html). Paketleri serisi nesnesi hakkında bilgi için belirli bir zaman, paketin belgelerine başvurmanız gerekir.

Kitap **tanıtım zaman serisi** Paul Cowpertwait ve Andrew Metcalfe R R kullanarak zaman serisi analiz için giriş bilgileri sağlar. Çok fazla teorik metinleri R örnekleri sağlar.

Bazı harika internet kaynaklar aşağıda verilmiştir:

* DataCamp tarayıcınız video dersler ve kodlama alıştırmalar alışık olduğunuz, R öğretir. Etkileşimli öğreticileri son R teknikleri ve paketleri vardır. Ücretsiz katılın [R için etkileşimli öğretici](https://www.datacamp.com/courses/introduction-to-r).
* [Eksiksiz bir kılavuz olan R programlama öğrenin](https://www.programiz.com/r-programming) Programiz öğesinden.
* Hızlı [R öğretici](https://www.cyclismo.org/tutorial/R/) Clarkson University'den Kelly siyah olarak.
* Vardır adresinde listelenmiş 60 R kaynakları üzerinden [veri becerilerinizi geliştirmek için üst R dil kaynakları](https://www.computerworld.com/article/2497464/business-intelligence-60-r-resources-to-improve-your-data-skills.html).

<!-- Module References -->
[execute-r-script]: /azure/machine-learning/studio-module-reference/execute-r-script
