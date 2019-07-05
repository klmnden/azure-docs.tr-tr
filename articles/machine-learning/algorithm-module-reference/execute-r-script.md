---
title: 'R betiği yürütün: Modül başvurusu'
titleSuffix: Azure Machine Learning service
description: R betiği yürütme modülü R kodu çalıştırmak için Azure Machine Learning hizmetinde kullanmayı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: peterclu
ms.date: 06/01/2019
ROBOTS: NOINDEX
ms.openlocfilehash: bfddcd3db4825ea1875474aa16544aa15412bdea
ms.sourcegitcommit: 6cb4dd784dd5a6c72edaff56cf6bcdcd8c579ee7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2019
ms.locfileid: "67518059"
---
# <a name="execute-r-script"></a>R Betiği yürütme

Bu makalede nasıl kullanılacağını **R betiği yürütme** görsel arabirim denemenizde R kodu çalıştırmak için modülü.

R ile var olan modülleri tarafından gibi şu anda desteklenmeyen görevleri gerçekleştirebilirsiniz: 
- Özel veri dönüştürmeler
- Öngörüler değerlendirmek için kendi ölçümleri'ni kullanın
- Tek başına modülleri görsel arabirim olarak uygulanmadı algoritmaları kullanarak modeller oluşturun

## <a name="r-version-support"></a>R sürüm desteği

Azure Machine Learning hizmeti görsel arabirim r CRAN (Comprehensive R Archive Network) dağıtımını kullanır. Şu anda kullanılan CRAN 3.5.1 sürümüdür.

## <a name="supported-r-packages"></a>Desteklenen R paketleri

100'den fazla paketlerle önceden yüklenmiş R ortamıdır. Tam listesi için konudaki [önceden R paketleri](#pre-installed-r-packages).

Ayrıca aşağıdaki kodu herhangi ekleyebileceğiniz **R betiği yürütme** modülü ve yüklü paketleri görmek için.

```R
azureml_main <- function(dataframe1, dataframe2){
  print("R script run.")
  dataframe1 <- data.frame(installed.packages())
  return(list(dataset1=dataframe1, dataset2=dataframe2))
}
```

## <a name="installing-r-packages"></a>R paketlerini yükleme
Ek R paketleri yüklemek için kullanın `install.packages()` yöntemi. CRAN depo belirttiğinizden emin olun. Paketleri yüklü her biri için **R betiği yürütme** modülü ve diğer arasında paylaşılmaz **R betiği yürütme** modüller.

Bu örnek Zoo yüklenmesi gösterilmektedir:
```R
# R version: 3.5.1
# The script MUST contain a function named azureml_main
# which is the entry point for this module.

# The entry point function can contain up to two input arguments:
#   Param<dataframe1>: a R DataFrame
#   Param<dataframe2>: a R DataFrame
azureml_main <- function(dataframe1, dataframe2){
  print("R script run.")
  
  if(!require(zoo)) install.packages("zoo",repos = "http://cran.us.r-project.org")
  library(zoo)
  # Return datasets as a Named List
  return(list(dataset1=dataframe1, dataset2=dataframe2))
}
```

## <a name="how-to-configure-execute-r-script"></a>R betiği yürütme yapılandırma

**R betiği yürütme** modülü, başlangıç noktası olarak kullanabileceğiniz örnek kodunu içerir. Yapılandırmak için **R betiği yürütme** modülü, giriş ve yürütmek için kod kümesi sağlar.

![R Modülü](media/module/execute-r-script.png)

Görsel arabirim içinde depolanan kümeleri otomatik olarak bu modülü ile yüklendiğinde bir R veri çerçevesine dönüştürülür.

1.  Ekleme **R betiği yürütme** denemenizi modülü.

    > [!NOTE]
    > Geçirilen tüm veri **R betiği yürütme** R modülü dönüştürülür `data.frame` biçimi.

1. Komut dosyası tarafından gerekli tüm girişleri bağlanın. Girişler isteğe bağlıdır ve veri ve ek R kod içerebilir.

    * **DataSet1**: İlk giriş olarak başvuru `dataframe1`. Giriş veri kümesi biçimlendirilmiş bir CSV, TSV, ARFF'ye veya bir Azure Machine Learning veri kümesi bağlanabilir.

    * **Dataset2**: İkinci girdi olarak başvuru `dataframe2`. Bu veri kümesi, CSV, TSV, olarak biçimlendirilmelidir ARFF'ye dosya veya bir Azure Machine Learning veri kümesi olarak.

    * **Betik paketi**: Üçüncü giriş, ZIP dosyaları kabul eder. Daraltılmış dosyayı, birden çok dosya ve birden çok dosya türünü içerebilir.

1. İçinde **R betiği** metin kutusu, geçerli R betiğini yapıştırın veya yazın.

    Çalışmaya başlamanıza yardımcı olmak için **R betiği** metin kutusuna, düzenleme veya değiştirin, örnek kod ile önceden doldurulmuş haldedir.

    * Betik adlı bir işlev içermelidir `azureml_main`, bu modülü için giriş noktası olduğu.

    * En fazla iki giriş bağımsız değişkeni giriş noktası işlevi içerebilir: `Param<dataframe1>` ve `Param<dataframe2>`
 
    > [!NOTE]
    >  Mevcut R kodunu bir görsel arabirim denemesine çalıştırmak için küçük değişiklikler gerekebilir. Örneğin, kodunuzda kullanmadan önce CSV biçiminde sağladığınız girdi verilerini açıkça bir veri kümesine dönüştürülmelidir. R programlama diline kullanılan veri ve sütun türleri ayrıca bazı açılardan visual arabiriminde kullanılan veri ve sütun türünden farklı.

1.  **Rastgele bir tohum**: R ortamın içinde rastgele çekirdek değeri kullanılacak bir değer yazın. Bu parametre çağırmakla eşdeğerdir `set.seed(value)` R kod.  

1. Denemeyi çalıştırın.  

## <a name="results"></a>Sonuçlar

**R betiği yürütme** modülleri, birden çok çıktı döndürebilir, ancak R veri çerçevesi sağlanmalıdır. Veri çerçevelerini görsel arabirim veri kümelerine diğer modüller ile uyumluluk için otomatik olarak dönüştürülür.

Standart iletileri ve R hatalarından modülün günlüğüne döndürülür.

## <a name="sample-scripts"></a>Örnek komut dosyaları

Özel R betiği kullanarak denemenizi genişletme birçok yolu vardır.  Bu bölümde, ortak görevler için örnek kodu sağlıyor.


### <a name="add-r-script-as-an-input"></a>R betiği girdi olarak Ekle

**R betiği yürütme** modülü girdi olarak rastgele R betiklerinin destekler. Bunu yapmak için çalışma alanınıza ZIP dosyasının bir parçası olarak yüklenmelidir.

1. Çalışma alanınızı R kodu içeren bir ZIP dosyası karşıya yüklemek için tıklayın **yeni**, tıklayın **veri kümesi**ve ardından **yerel dosyadan** ve **Zip dosyası**seçeneği.  

1. Sıkıştırılmış dosya olarak kullanılabilir olduğunu doğrulamak **kaydedilmiş veri kümeleri** listesi.

1.  Veri kümesine bağlanmak **betik paketi** giriş bağlantı noktası.

1. ZIP dosyasında yer alan tüm dosyaları çalışma zamanı deneme sırasında kullanılabilir. 

    Paket betiğin bir dizin yapısına içeriyorsa, yapısı korunur. Ancak, dizinin önüne eklediğinizden kodunuzu alter **. / betik paketini** yolu.

### <a name="process-data"></a>Veri işleme

Aşağıdaki örnek, ölçeklendirme ve girdi verilerini Normalleştir gösterilmektedir:

```R
# R version: 3.5.1
# The script MUST contain a function named azureml_main
# which is the entry point for this module.
# The entry point function can contain up to two input arguments:
#   Param<dataframe1>: a R DataFrame
#   Param<dataframe2>: a R DataFrame
azureml_main <- function(dataframe1, dataframe2){
  print("R script run.")
  # If a zip file is connected to the third input port, it is
  # unzipped under "./Script Bundle". This directory is added
  # to sys.path.
  series <- dataframe1$width
  # find the maximum and minimum values of width column in dataframe1
  max_v <- max(series)
  min_v <- min(series)
  # calculate the scale and bias
  scale <- max_v - min_v
  bias <- min_v / dis
  # apply min-max normalizing
  dataframe1$width <- dataframe1$width / scale - bias
  dataframe2$width <- dataframe2$width / scale - bias
  # Return datasets as a Named List
  return(list(dataset1=dataframe1, dataset2=dataframe2))
}
 ```

### <a name="read-a-zip-file-as-input"></a>Giriş olarak bir ZIP dosyası okunamıyor

Bu örnek bir veri kümesi bir ZIP dosyasına bir giriş olarak kullanmayı gösterir **R betiği yürütme** modülü.

1. Veri dosyası CSV biçiminde oluşturun ve "mydatafile.csv" olarak adlandırın.
1. Bir ZIP dosyası oluşturun ve CSV dosyası arşive ekleyin.
1. Sıkıştırılmış dosya, Azure Machine Learning çalışma alanınıza yükleyin. 
1. Sonuçta elde edilen veri kümesine bağlanmak **ScriptBundle** , giriş, **R betiği yürütme** modülü.
1. Sıkıştırılmış dosyayı CSV verileri okumak için aşağıdaki kodu kullanarak.

```R
azureml_main <- function(dataframe1, dataframe2){
  print("R script run.")
  mydataset<-read.csv("./Script Bundle/mydatafile.csv",encoding="UTF-8");  
  # Return datasets as a Named List
  return(list(dataset1=mydataset, dataset2=dataframe2))
}
```

### <a name="replicate-rows"></a>Satır çoğaltma

Bu örnek, örnek dengelemek için bir veri kümesindeki pozitif kayıtları çoğaltmak gösterilmektedir:

```R
azureml_main <- function(dataframe1, dataframe2){
  data.set <- dataframe1[dataframe1[,1]==-1,]  
  # positions of the positive samples
  pos <- dataframe1[dataframe1[,1]==1,]
  # replicate the positive samples to balance the sample  
  for (i in 1:20) data.set <- rbind(data.set,pos)  
  row.names(data.set) <- NULL
  # Return datasets as a Named List
  return(list(dataset1=data.set, dataset2=dataframe2))
}
```

### <a name="pass-r-objects-between-execute-r-script-modules"></a>R betiği yürütme modülleri arasında geçişi R nesneleri

Örnekleri arasında R nesnelerini geçirebilirsiniz **R betiği yürütme** iç seri hale getirme mekanizmasını kullanarak modülü. Bu örnek adlı R nesne taşımak istediğiniz varsayar `A` iki **R betiği yürütme** modüller.

1. İlk ekleme **R betiği yürütme** Modülü aşağıdaki kod, tür ve deneme **R betiği** serileştirilmiş nesne oluşturmak için metin kutusunu `A` modülü bir sütunda veri tablosu çıktı olarak:  
  
    ```R
    azureml_main <- function(dataframe1, dataframe2){
      print("R script run.")
      # some codes generated A
      
      serialized <- as.integer(serialize(A,NULL))  
      data.set <- data.frame(serialized,stringsAsFactors=FALSE)

      return(list(dataset1=data.set, dataset2=dataframe2))
    }
    ```

    R ile verileri seri hale getirme işlevi çıkardığından tamsayı türüne açık dönüştürme yapılır `Raw` biçiminde görsel arabirim tarafından desteklenmiyor.

1. İkinci bir örneğini ekleme **R betiği yürütme** modülü ve önceki modülünün çıkış bağlantı noktasına bağlayın.

1. Aşağıdaki kodu yazın **R betiği** nesne ayıklamak için metin kutusu `A` giriş veri tablosundan. 

    ```R
    azureml_main <- function(dataframe1, dataframe2){
      print("R script run.")
      A <- unserialize(as.raw(dataframe1$serialized))  
      # Return datasets as a Named List
      return(list(dataset1=dataframe1, dataset2=dataframe2))
    }
    ```

## <a name="pre-installed-r-packages"></a>Önceden yüklü R paketleri

Önceden yüklü R paketleri kullanılabilir geçerli listesi:

|              |            | 
|--------------|------------| 
| Paket      | Version    | 
| askpass      | 1.1        | 
| assertthat   | 0.2.1      | 
| backports    | 1.1.4      | 
| temel         | 3.5.1      | 
| base64enc    | 0.1-3      | 
| BH           | 1.69.0-1   | 
| bindr        | 0.1.1      | 
| bindrcpp     | 0.2.2      | 
| bitops       | 1.0-6      | 
| başlatma         | 1.3-22     | 
| broom        | 0.5.2      | 
| callr        | 3.2.0      | 
| Giriş işaretini        | 6.0-84     | 
| caTools      | 1.17.1.2   | 
| cellranger   | 1.1.0      | 
| sınıf        | 7.3-15     | 
| CLI          | 1.1.0      | 
| clipr        | 0.6.0      | 
| Küme      | 2.0.7-1    | 
| codetools    | 0.2-16     | 
| renk uzayı   | 1.4-1      | 
| Derleyici     | 3.5.1      | 
| Kreyon       | 1.3.4      | 
| Curl         | 3.3        | 
| Data.Table   | 1.12.2     | 
| datasets     | 3.5.1      | 
| DBI          | 1.0.0      | 
| dbplyr       | 1.4.1      | 
| digest       | 0.6.19     | 
| dplyr        | 0.7.6      | 
| e1071        | 1.7-2      | 
| değerlendir     | 0.14       | 
| fansi        | 0.4.0      | 
| forcats      | 0.3.0      | 
| foreach      | 1.4.4      | 
| Yabancı      | 0.8-71     | 
| FS           | 1.3.1      | 
| gdata        | 2.18.0     | 
| Genel türler     | 0.0.2      | 
| ggplot2      | 3.2.0      | 
| glmnet       | 2.0-18     | 
| birleştirici         | 1.3.1      | 
| gower        | 0.2.1      | 
| gplots       | 3.0.1.1    | 
| Grafik     | 3.5.1      | 
| grDevices    | 3.5.1      | 
| Kılavuz         | 3.5.1      | 
| gtable       | 0.3.0      | 
| gtools       | 3.8.1      | 
| haven        | 2.1.0      | 
| highr        | 0.8        | 
| hms          | 0.4.2      | 
| htmltools    | 0.3.6      | 
| httr         | 1.4.0      | 
| ipred        | 0.9-9      | 
| Yineleyiciler    | 1.0.10     | 
| jsonlite     | 1.6        | 
| KernSmooth   | 2.23-15    | 
| knitr        | 1,23       | 
| Etiketleme     | 0.3        | 
| Kafes      | 0.20-38    | 
| Lava         | 1.6.5      | 
| lazyeval     | 0.2.2      | 
| lubridate    | 1.7.4      | 
| magrittr     | 1.5        | 
| Markdown     | 1          | 
| YIĞIN         | 7.3-51.4   | 
| Matrix       | 1.2-17     | 
| Yöntemleri      | 3.5.1      | 
| mgcv         | 1.8-28     | 
| MIME         | 0.7        | 
| ModelMetrics | 1.2.2      | 
| modelr       | 0.1.4      | 
| munsell      | 0.5.0      | 
| nlme         | 3.1-140    | 
| nnet         | 7.3-12     | 
| numDeriv     | 2016.8-1.1 | 
| openssl      | 1.4        | 
| parallel     | 3.5.1      | 
| Sütun       | 1.4.1      | 
| pkgconfig    | 2.0.2      | 
| plogr        | 0.2.0      | 
| plyr         | 1.8.4      | 
| prettyunits  | 1.0.2      | 
| processx     | 3.3.1      | 
| prodlim      | 2018.04.18 | 
| İlerleme durumu     | 1.2.2      | 
| ps           | 1.3.0      | 
| purrr        | 0.3.2      | 
| quadprog     | 1.5-7      | 
| quantmod     | 0.4-15     | 
| R6           | 2.4.0      | 
| RandomForest | 4.6-14     | 
| RColorBrewer | 1.1-2      | 
| Rcpp         | 1.0.1      | 
| RcppRoll     | 0.3.0      | 
| readr        | 1.3.1      | 
| readxl       | 1.3.1      | 
| yemek tarifleri      | 0.1.5      | 
| rematch      | 1.0.1      | 
| reprex       | 0.3.0      | 
| reshape2     | 1.4.3      | 
| reticulate   | 1.12       | 
| rlang        | 0.4.0      | 
| rmarkdown    | 1.13       | 
| ROCR         | 1.0-7      | 
| rpart        | 4.1-15     | 
| rstudioapi   | 0.1        | 
| rvest        | 0.3.4      | 
| ölçekler       | 1.0.0      | 
| selectr      | 0.4-1      | 
| Uzamsal      | 7.3-11     | 
| eğrileri      | 3.5.1      | 
| SQUAREM      | 2017.10-1  | 
| İstatistikleri        | 3.5.1      | 
| stats4       | 3.5.1      | 
| stringi      | 1.4.3      | 
| stringr      | 1.3.1      | 
| acil ihtiyaç     | 2.44-1.1   | 
| sys          | 3,2        | 
| tcltk        | 3.5.1      | 
| tibble       | 2.1.3      | 
| tidyr        | 0.8.3      | 
| tidyselect   | 0.2.5      | 
| tidyverse    | 1.2.1      | 
| saat tarih     | 3043.102   | 
| tinytex      | 0.13       | 
| araçlar        | 3.5.1      | 
| tseries      | 0.10-47    | 
| TTR          | 0.23-4     | 
| Utf8         | 1.1.4      | 
| Yardımcı programları        | 3.5.1      | 
| vctrs        | 0.1.0      | 
| viridisLite  | 0.3.0      | 
| yatay çizgi      | 0.3-2      | 
| withr        | 2.1.2      | 
| xfun         | 0.8        | 
| xml2         | 1.2.0      | 
| xts          | 0.11-2     | 
| yaml         | 2.2.0      | 
| zeallot      | 0.1.0      | 
| Zoo          | 1.8-6      | 

## <a name="next-steps"></a>Sonraki adımlar

Bkz: [kullanılabilir modül kümesini](module-reference.md) Azure Machine Learning hizmetine. 