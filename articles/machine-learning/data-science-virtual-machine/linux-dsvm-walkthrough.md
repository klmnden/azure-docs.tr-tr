---
title: Veri bilimi ile Linux veri bilimi sanal makinede Azure | Microsoft Docs
description: Linux veri bilimi VM ile birkaç genel veri bilimi görevleri gerçekleştirme.
services: machine-learning
documentationcenter: ''
author: gopitk
manager: cgronlun
editor: cgronlun
ms.assetid: 34ef0b10-9270-474f-8800-eecb183bbce4
ms.service: machine-learning
ms.component: data-science-vm
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/16/2018
ms.author: gokuma
ms.openlocfilehash: 59d6b960a40910b8b2fe72f6c3b149608ee8b8ad
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2018
---
# <a name="data-science-with-a-linux-data-science-virtual-machine-on-azure"></a>Veri bilimi ile Linux veri bilimi sanal makinede Azure
Bu kılavuzda, Linux veri bilimi VM ile birçok ortak veri bilimi görevlerinin nasıl gerçekleştirileceğini gösterir. Linux veri bilimi sanal makine (DSVM) veri analizi ve makine öğrenme için yaygın olarak kullanılan bir araç koleksiyonu ile önceden yüklenmiş olan Azure üzerinde kullanılabilir bir sanal makine görüntüdür. Anahtar yazılım bileşenleri içinde listelenen [Linux veri bilimi sanal makine sağlama](linux-dsvm-intro.md) konu. VM görüntüsü yüklemek ve araçların her biri ayrı ayrı yapılandırmak zorunda kalmadan dakika cinsinden veri bilimi yapılması başlamak kolaylaştırır. Kolayca VM gerekirse ölçeklendirmek ve kullanılmadığında durdurun. Bu nedenle bu kaynak, esnek ve ekonomik içindir.

Bu kılavuzda gösterilen veri bilimi görevleri konusunda özetlenen adımları [takım veri bilimi işlemi](https://azure.microsoft.com/documentation/learning-paths/data-science-process/). Bu işlem için etkili bir şekilde akıllı uygulamaları oluşturma yaşam döngüsü üzerinde işbirliği yapmak için veri bilimcilerine ekiplerin sağlayan veri bilimi sistematik bir yaklaşım sunar. Veri bilimi işlemi da bir kişi tarafından izlenebilir veri bilimi için yinelemeli bir çerçeve sağlar.

Biz analiz [spambase](https://archive.ics.uci.edu/ml/datasets/spambase) bu kılavuzda veri kümesi. Bu e-postaların istenmeyen posta ya da (olmadıkları istenmeyen posta anlamına gelir) ham, olarak işaretlenmiş kümesidir ve ayrıca e-posta içeriğini bazı istatistiklerle içerir. Dahil edilen istatistikleri sonraki ancak bir bölüm içinde ele alınmıştır.

## <a name="prerequisites"></a>Önkoşullar
Linux veri bilimi sanal makine kullanmadan önce aşağıdakilere sahip olmanız gerekir:

* Bir **Azure aboneliği**. Zaten bir yoksa, bkz: [ücretsiz Azure hesabınızı bugün oluşturmak](https://azure.microsoft.com/free/).
* A [ **Linux veri bilimi VM**](https://azure.microsoft.com/marketplace/partners/microsoft-ads/linux-data-science-vm). Bu VM sağlama hakkında daha fazla bilgi için bkz: [Linux veri bilimi sanal makine sağlama](linux-dsvm-intro.md).
* [X2Go](http://wiki.x2go.org/doku.php) bilgisayarınızda yüklü ve bir XFCE oturumu açılır. Yükleme ve yapılandırma hakkında bilgi için bir **X2Go istemci**, bkz: [yükleme ve yapılandırma X2Go istemci](linux-dsvm-intro.md#installing-and-configuring-x2go-client).
* Daha sorunsuz bir kaydırma deneyim için hakkında gfx.xrender.enabled bayrağı Değiştir: VM'ler FireFox tarayıcıda yapılandırma. [Daha fazla buraya bakın. ](https://www.reddit.com/r/firefox/comments/4nfmvp/ff_47_unbearable_slow_over_remote_x11/). Geçiş de dikkate *mousewheel.enable_pixel_scrolling* false. [Burada yönergeler.](https://support.mozilla.org/en-US/questions/981140)
* Bir **AzureML hesap**. Zaten, yeni bir kayıt yoksa, [AzureML giriş sayfası](https://studio.azureml.net/). Başlamanıza yardımcı olmak için ücretsiz kullanım katmanı yoktur.

## <a name="download-the-spambase-dataset"></a>Spambase dataset indirin
[Spambase](https://archive.ics.uci.edu/ml/datasets/spambase) veri kümesi yalnızca 4601 örnekleri içeren veri görece küçük kümesidir. Bu, veri bilimi VM şekliyle anahtar özelliklerinden bazıları tutmasını kaynak gereksinimlerini uygun gösteren sırasında kullanmak için uygun bir boyutudur.

> [!NOTE]
> Bu kılavuz bir D2 v2 ölçekli Linux veri bilimi sanal makinede oluşturuldu. Bu boyut DSVM yordamları bu kılavuzda işleyebilir.
>
>

Daha fazla depolama alanı gerekiyorsa, ek diskleri oluşturun ve sizin VM'e ekleyin. Bu diskleri kalıcı Azure depolama kullandığından, sunucu yeniden boyutlandırma nedeniyle sağlama veya kapatılmış olsa bile verilerini korunur. Bir disk ekleyin ve VM'nize eklemek için'ndaki yönergeleri izleyin [bir Linux VM için bir disk eklemek](../../virtual-machines/linux/add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Azure komut satırı DSVM üzerinde önceden yüklenmiş arabirimi (Azure CLI), aşağıdaki adımları kullanın. Bu nedenle bu yordamları tamamen sanal makineden kendisini yapılabilir. Depolama artırmak için başka bir seçenek kullanmaktır [Azure dosyaları](../../storage/files/storage-how-to-use-files-linux.md).

Veri yüklemek için bir terminal penceresi açın ve şu komutu çalıştırın:

    wget http://archive.ics.uci.edu/ml/machine-learning-databases/spambase/spambase.data

İndirilen Dosya sağlandığından üstbilgi sahip başka bir dosya oluşturmak bir başlık satırı yok. Uygun üst bilgilerle bir dosya oluşturmak için şu komutu çalıştırın:

    echo 'word_freq_make, word_freq_address, word_freq_all, word_freq_3d,word_freq_our, word_freq_over, word_freq_remove, word_freq_internet,word_freq_order, word_freq_mail, word_freq_receive, word_freq_will,word_freq_people, word_freq_report, word_freq_addresses, word_freq_free,word_freq_business, word_freq_email, word_freq_you, word_freq_credit,word_freq_your, word_freq_font, word_freq_000, word_freq_money,word_freq_hp, word_freq_hpl, word_freq_george, word_freq_650, word_freq_lab,word_freq_labs, word_freq_telnet, word_freq_857, word_freq_data,word_freq_415, word_freq_85, word_freq_technology, word_freq_1999,word_freq_parts, word_freq_pm, word_freq_direct, word_freq_cs, word_freq_meeting,word_freq_original, word_freq_project, word_freq_re, word_freq_edu,word_freq_table, word_freq_conference, char_freq_semicolon, char_freq_leftParen,char_freq_leftBracket, char_freq_exclamation, char_freq_dollar, char_freq_pound, capital_run_length_average,capital_run_length_longest, capital_run_length_total, spam' > headers

Ardından komutu ile birlikte iki dosyaları birleştirme:

    cat spambase.data >> headers
    mv headers spambaseHeaders.data

Veri kümesi istatistikleri çeşitli türlerde her e-postaya sahiptir:

* Sütunları ister ***word\_freq\_WORD*** eşleşen sözcükleri e-postadaki yüzdesini belirtmek *WORD*. Örneğin, varsa *word\_freq\_olun* %1 e-posta içindeki tüm sözcükleri bundan sonra 1 ' dir *olun*.
* Sütunları ister ***char\_freq\_CHAR*** olan tüm e-posta karakter yüzdesini belirtmek *CHAR*.
* ***büyük\_çalıştırmak\_uzunluğu\_uzun*** büyük harf bir dizi uzun uzunluğu.
* ***büyük\_çalıştırmak\_uzunluğu\_ortalama*** büyük harf ve tüm dizilerini ortalama uzunluğu.
* ***büyük\_çalıştırmak\_uzunluğu\_toplam*** büyük harf ve tüm dizilerini toplam uzunluğu.
* ***istenmeyen posta*** veya e-posta istenmeyen posta kabul olup olmadığını gösterir (1 istenmeyen posta, 0 = değil istenmeyen posta =).

## <a name="explore-the-dataset-with-microsoft-r-open"></a>Microsoft R açık ile dataset keşfedin
Şimdi verileri incelemek ve bazı temel makine r ile öğrenme yapın Veri bilimi VM ile birlikte gelen [Microsoft R açık](https://mran.revolutionanalytics.com/open/) önceden yüklenmiş. Birden çok iş parçacıklı matematik kitaplıkları R bu sürümündeki çeşitli tek iş parçacıklı sürümleri daha iyi performans sunar. Microsoft R açık de yeniden Üretilebilirlik CRAN Paket Deposu görüntüsünü kullanarak sağlar.

Bu kılavuzda kullanılan kod örnekleri kopyalarını almak için kopyalama **Azure-Machine-Learning-Data-Bilim** git, VM önceden yüklenmiş olduğu kullanarak deposu. Git komut satırından çalıştırın:

    git clone https://github.com/Azure/Azure-MachineLearning-DataScience.git

Bir terminal penceresi açın ve yeni bir R oturumu R etkileşimli konsol ile başlatın.

> [!NOTE]
> Bu gibi durumlarda, Rstudio'dan da için aşağıdaki yordamları kullanabilirsiniz. Rstudio'dan yüklemek için bu bir terminal komutunu yürütün: `./Desktop/DSVM\ tools/installRStudio.sh`
>
>

Veri içeri aktarın ve ortamını ayarlamak için çalıştırın:

    data <- read.csv("spambaseHeaders.data")
    set.seed(123)

Her sütunun hakkındaki özet istatistikleri görmek için:

    summary(data)

Verileri farklı bir görünüm için:

    str(data)

Bu, her bir değişken ve ilk birkaç değerleri türü veri kümesini gösterir.

*İstenmeyen posta* sütun bir tamsayı olarak okundu, ancak gerçekte kategorik olan değişkeni (veya faktörü). Türünü ayarlamak için:

    data$spam <- as.factor(data$spam)

Keşif biraz analiz yapma kullanmak [ggplot2](http://ggplot2.org/) paketini, VM üzerinde zaten yüklü R için popüler bir grafik kitaplığı. , Daha önce görüntülenen özet verileri biz Özet istatistikleri ünlem işareti karakteri sıklığını sahip olduğunu unutmayın. Şimdi bu sıklıklarını burada aşağıdaki komutlarla çizimi:

    library(ggplot2)
    ggplot(data) + geom_histogram(aes(x=char_freq_exclamation), binwidth=0.25)

Şimdi sıfır çubuğu çizim eğriltme olduğundan, bunu kurtulun:

    email_with_exclamation = data[data$char_freq_exclamation > 0, ]
    ggplot(email_with_exclamation) + geom_histogram(aes(x=char_freq_exclamation), binwidth=0.25)

Önemsiz olmayan yoğunluğu ilginç görünüyor 1 yukarıda yoktur. Bu verileri yalnızca bakalım:

    ggplot(data[data$char_freq_exclamation > 1, ]) + geom_histogram(aes(x=char_freq_exclamation), binwidth=0.25)

Ardından istenmeyen posta vs ham tarafından böl:

    ggplot(data[data$char_freq_exclamation > 1, ], aes(x=char_freq_exclamation)) +
    geom_density(lty=3) +
    geom_density(aes(fill=spam, colour=spam), alpha=0.55) +
    xlab("spam") +
    ggtitle("Distribution of spam \nby frequency of !") +
    labs(fill="spam", y="Density")

Bu örnekler, içerdikleri verileri araştırmak için benzer çizimleri diğer sütunların yapmak etkinleştirmeniz gerekir.

## <a name="train-and-test-an-ml-model"></a>Eğitme ve test ML modeli
Şimdi şimdi birkaç span veya ham içeren olarak dataset postalarda sınıflandırmak için makine öğrenimi modellerini eğitmek. Biz karar ağacı modeli ve bu bölümdeki rastgele orman modeli eğitmek ve kendi tahminleri kendi doğruluğunu test edin.

> [!NOTE]
> Aşağıdaki kod içinde kullanılan rpart (özyinelemeli bölümlendirme ve regresyon ağaçlarında) paketi veri bilimi VM üzerinde zaten yüklü.
>
>

Öncelikle, şimdi dataset eğitim ve test ayarlar böl:

    rnd <- runif(dim(data)[1])
    trainSet = subset(data, rnd <= 0.7)
    testSet = subset(data, rnd > 0.7)

Ve e-postaları sınıflandırmak için karar ağacı oluşturun.

    require(rpart)
    model.rpart <- rpart(spam ~ ., method = "class", data = trainSet)
    plot(model.rpart)
    text(model.rpart)

Sonuç şöyledir:

![1](./media/linux-dsvm-walkthrough/decision-tree.png)

Ne kadar iyi eğitim kümesinde gerçekleştirip belirlemek için aşağıdaki kodu kullanın:

    trainSetPred <- predict(model.rpart, newdata = trainSet, type = "class")
    t <- table(`Actual Class` = trainSet$spam, `Predicted Class` = trainSetPred)
    accuracy <- sum(diag(t))/sum(t)
    accuracy

Ne kadar iyi belirlemek için test kümesinde gerçekleştirir:

    testSetPred <- predict(model.rpart, newdata = testSet, type = "class")
    t <- table(`Actual Class` = testSet$spam, `Predicted Class` = testSetPred)
    accuracy <- sum(diag(t))/sum(t)
    accuracy

Ayrıca bir rastgele orman modeli deneyelim. Rastgele ormanları karar ağaçları çok sayıda eğitmek ve tüm ayrı ayrı karar ağaçları sınıflandırmaları modunun bir sınıf çıkış. Bir eğitim veri kümesi overfit işlemciler karar ağacı modeli için düzeltirken yaklaşım öğrenme daha güçlü bir makine sağlarlar.

    require(randomForest)
    trainVars <- setdiff(colnames(data), 'spam')
    model.rf <- randomForest(x=trainSet[, trainVars], y=trainSet$spam)

    trainSetPred <- predict(model.rf, newdata = trainSet[, trainVars], type = "class")
    table(`Actual Class` = trainSet$spam, `Predicted Class` = trainSetPred)

    testSetPred <- predict(model.rf, newdata = testSet[, trainVars], type = "class")
    t <- table(`Actual Class` = testSet$spam, `Predicted Class` = testSetPred)
    accuracy <- sum(diag(t))/sum(t)
    accuracy


## <a name="deploy-a-model-to-azure-ml"></a>Azure ML model dağıtma
[Azure Machine Learning Studio](https://studio.azureml.net/) (AzureML), derleme ve Tahmine dayalı analiz modelleriniz dağıtma daha kolay hale getirir bir bulut hizmetidir. AzureML iyi özelliklerini bir web hizmeti olarak herhangi bir R işlev yayımlama yeteneğini biridir. AzureML R paketi dağıtım sağ DSVM bizim R oturum yapmak kolaylaştırır.

Önceki bölümde karar ağacı koddan dağıtmak için Azure Machine Learning Studio'da oturum açmak gerekir. Çalışma alanı Kimliğinizi ve bir yetki belirteci oturum açmak için gerekir. Bu değerleri bulmak ve onlarla AzureML değişkenlerini başlatmak için:

Seçin **ayarları** sol menüdeki. Not, **çalışma alanı kimliği**. ![2](./media/linux-dsvm-walkthrough/workspace-id.png)

Seçin **yetkilendirme belirteçleri** Not ve genel gider menüden, **birincil yetkilendirme belirteci**.![ 3](./media/linux-dsvm-walkthrough/workspace-token.png)

Yük **AzureML** paketini ve belirteç ve çalışma alanı Kimliğiniz ile değişkenlerin değerleri üzerinde DSVM R oturumunuzda ayarlayın:

    require(AzureML)
    wsAuth = "<authorization-token>"
    wsID = "<workspace-id>"


Şimdi uygulamak bu tanıtımı kolaylaştırmak için model basitleştirin. Kök yakın karar ağacında üç değişkenleri seçin ve yalnızca bu üç değişkenin kullanarak yeni bir ağaç yapılandırın:

    colNames <- c("char_freq_dollar", "word_freq_remove", "word_freq_hp", "spam")
    smallTrainSet <- trainSet[, colNames]
    smallTestSet <- testSet[, colNames]
    model.rpart <- rpart(spam ~ ., method = "class", data = smallTrainSet)

Özellikleri bir girdi olarak alır ve tahmin edilen değerler döndüren bir tahmin işlevinde ihtiyacımız var:

    predictSpam <- function(char_freq_dollar, word_freq_remove, word_freq_hp) {
        predictDF <- predict(model.rpart, data.frame("char_freq_dollar" = char_freq_dollar,
        "word_freq_remove" = word_freq_remove, "word_freq_hp" = word_freq_hp))
        return(colnames(predictDF)[apply(predictDF, 1, which.max)])
    }

AzureML kullanmaya predictSpam işlevi yayımlama **publishWebService** işlevi:

    spamWebService <- publishWebService("predictSpam",
        "spamWebService",
        list("char_freq_dollar"="float", "word_freq_remove"="float","word_freq_hp"="float"),
        list("spam"="int"),
        wsID, wsAuth)

Bu işlev alır **predictSpam** işlev, adlı bir web hizmeti oluşturur **spamWebService** girişleri ve çıkışları ile tanımlanan ve yeni uç noktası hakkında bilgi verir.

Yayımlanan web hizmetinde, kendi API uç noktası gibi ayrıntılarını görüntülemek ve erişim anahtarları komutuyla:

    spamWebService[[2]]

İlk denemenin testinin 10 satır ayarlayın:

    consumeDataframe(spamWebService$endpoints[[1]]$PrimaryKey, spamWebService$endpoints[[1]]$ApiLocation, smallTestSet[1:10, 1:3])


## <a name="use-other-tools-available"></a>Kullanılabilir diğer araçlarını kullanma
Kalan bölümleri bazı Linux veri bilimi VM'de yüklü Araçları'nın nasıl kullanıldığını gösterir. Açıklanan araçları listesi aşağıdadır:

* XGBoost
* Python
* Jupyterhub
* Çıngırağı
* PostgreSQL & Squirrel SQL
* SQL Server veri ambarı

## <a name="xgboost"></a>XGBoost
[XGBoost](https://xgboost.readthedocs.org/en/latest/) hızlı ve doğru boosted ağaç uygulaması sağlayan bir araçtır.

    require(xgboost)
    data <- read.csv("spambaseHeaders.data")
    set.seed(123)

    rnd <- runif(dim(data)[1])
    trainSet = subset(data, rnd <= 0.7)
    testSet = subset(data, rnd > 0.7)

    bst <- xgboost(data = data.matrix(trainSet[,0:57]), label = trainSet$spam, nthread = 2, nrounds = 2, objective = "binary:logistic")

    pred <- predict(bst, data.matrix(testSet[, 0:57]))
    accuracy <- 1.0 - mean(as.numeric(pred > 0.5) != testSet$spam)
    print(paste("test accuracy = ", accuracy))

XGBoost, python veya bir komut satırından da çağırabilirsiniz.

## <a name="python"></a>Python
Python kullanarak geliştirme için Anaconda Python dağıtımları 2.7 ve 3.5 DSVM yüklenmiş.

> [!NOTE]
> Anaconda dağıtım içeren [Conda](http://conda.pydata.org/docs/index.html), farklı sürümlerini ve/veya bunları yüklü olan paketleri sahip Python için özel ortamlar oluşturmak için kullanılabilir.
>
>

Şimdi spambase dataset bazıları okuyun ve Destek vektör scikit makinelerinizde ile e-postaları sınıflandırmak-öğrenin:

    import pandas
    from sklearn import svm    
    data = pandas.read_csv("spambaseHeaders.data", sep = ',\s*')
    X = data.ix[:, 0:57]
    y = data.ix[:, 57]
    clf = svm.SVC()
    clf.fit(X, y)

Tahminleri yapmak için:

    clf.predict(X.ix[0:20, :])

Biz R modeli önceden yayımlandığında yaptığımız gibi bir AzureML uç nokta yayımlama göstermek için daha basit bir model üç değişkenleri olalım.

    X = data.ix[["char_freq_dollar", "word_freq_remove", "word_freq_hp"]]
    y = data.ix[:, 57]
    clf = svm.SVC()
    clf.fit(X, y)

Model için AzureML yayımlamak için:

    # Publish the model.
    workspace_id = "<workspace-id>"
    workspace_token = "<workspace-token>"
    from azureml import services
    @services.publish(workspace_id, workspace_token)
    @services.types(char_freq_dollar = float, word_freq_remove = float, word_freq_hp = float)
    @services.returns(int) # 0 or 1
    def predictSpam(char_freq_dollar, word_freq_remove, word_freq_hp):
        inputArray = [char_freq_dollar, word_freq_remove, word_freq_hp]
        return clf.predict(inputArray)

    # Get some info about the resulting model.
    predictSpam.service.url
    predictSpam.service.api_key

    # Call the model
    predictSpam.service(1, 1, 1)

> [!NOTE]
> Bu, yalnızca python 2.7 için kullanılabilir ve 3.5 henüz desteklenmiyor. Çalıştırma **/anaconda/bin/python2.7**.
>
>

## <a name="jupyterhub"></a>Jupyterhub
DSVM Anaconda dağıtımlarında Jupyter not defteri ile Python, R veya Jale kodunu ve analiz paylaşmak için platformlar arası ortamda gelir. Jupyter not defteri JupyterHub erişilir. Yerel Linux kullanıcı adı ve parola kullanarak oturum ***https://\<VM DNS adı veya IP adresi\>: 8000 /***. Tüm yapılandırma dosyalarını JupyterHub için dizinde bulunan **/etc/jupyterhub**.

> [!NOTE]
> Python Paket Yöneticisi'ni (aracılığıyla `pip` komutu) bir Jupyter not defteri geçerli Çekirdeği'nde, aşağıdaki komutu kod hücrede örneğin kullanılabilir:
```python
   import sys
   ! {sys.executable} -m pip install numpy -y
```
>
>

> [!NOTE]
> Conda yükleyici kullanmak için (aracılığıyla `conda` komutu) bir Jupyter not defteri geçerli Çekirdeği'nde, aşağıdaki komutu kod hücrede örneğin kullanılabilir:
```python
   import sys
   ! {sys.prefix}/bin/conda install --yes --prefix {sys.prefix} numpy
```
>
>

Birkaç örnek not defterlerini VM üzerinde zaten yüklenir:

* Bkz: [IntroToJupyterPython.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Data-Science-Virtual-Machine/Samples/Notebooks/IntroToJupyterPython.ipynb) bir örnek Python not defteri için.
* Bkz: [IntroTutorialinR](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Data-Science-Virtual-Machine/Samples/Notebooks/IntroTutorialinR.ipynb) bir örnek için **R** dizüstü bilgisayar.
* Bkz: [IrisClassifierPyMLWebService](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Data-Science-Virtual-Machine/Samples/Notebooks/IrisClassifierPyMLWebService.ipynb) başka bir örnek için **Python** dizüstü bilgisayar.

> [!NOTE]
> Jale dil, Linux veri bilimi VM üzerinde komut satırından da mevcuttur.
>
>

## <a name="rattle"></a>Çıngırağı
[Rattle](https://cran.r-project.org/web/packages/rattle/index.html) (R analitik aracı için bilgi kolayca) grafiksel bir R veri araştırma için aracıdır. Yük, keşfetme, veri dönüştürme ve yapı ve modelleri değerlendirmek daha kolay hale getirir sezgisel bir arabirim vardır.  Makaleyi [Rattle: bir veri madenciliği GUI r](https://journal.r-project.org/archive/2009-2/RJournal_2009-2_Williams.pdf) özelliklerini gösteren bir kılavuz sağlar.

Yükleyin ve aşağıdaki komutlarla Çıngırağı başlatın:

    if(!require("rattle")) install.packages("rattle")
    require(rattle)
    rattle()

> [!NOTE]
> Yükleme DSVM üzerinde gerekli değildir. Ancak, yüklediğinde, ek paketleri yüklemek için Çıngırağı isteyebilir.
>
>

Çıngırağı sekmesini tabanlı bir arabirim kullanır. Sekmeleri çoğunu karşılık adımlarda [veri bilimi işlemi](https://azure.microsoft.com/documentation/learning-paths/data-science-process/)veri yükleme veya araştırma gibi. Veri bilimi işlemi soldan sağa sekmeler arasında akar. Ancak son sekmesi tarafından Çıngırağı çalıştırmak R komutların günlüğünü de içerir.

Yük ve veri kümesi yapılandırmak için:

* Dosyayı yüklemek için seçin **veri** sekmesinde, ardından
* Seçici seçin **Filename** ve **spambaseHeaders.data**.
* Dosya yüklenemedi. seçin **yürütme** düğmelerinin üst satırda. Bir giriş, bir hedef veya başka bir değişken türü ve benzersiz değerlerin sayısını olup olmadığını, belirtilen veri türü de dahil olmak üzere her bir sütunun özetini görmeniz gerekir.
* Çıngırağı doğru tanımlanan **istenmeyen posta** sütunu hedef olarak. İstenmeyen posta sütunu seçin, ardından ayarlamak **hedef veri türü** için **Categoric**.

Verileri araştırmak için:

* Seçin **Araştır** sekmesi.
* Tıklatın **Özet**, ardından **yürütme**, değişken türleri hakkında bazı bilgiler ve bazı Özet istatistikleri görmek için.
* Her bir değişken hakkındaki istatistiklerdir diğer türlerini görüntülemek için diğer seçenekleri gibi seçin **açıklayın** veya **Temelleri**.

**Araştır** sekmesi ayrıca birçok ayrıntılı çizimleri oluşturmak olanak tanır. Verilerin bir histogram çizmek için:

* Seçin **dağıtımları**.
* Denetleme **Histogram** için **word_freq_remove** ve **word_freq_you**.
* Seçin **yürütme**. Word "," çok daha sık Kaldır"den" postalarda göründüğünü Temizle olduğu bir tek grafik penceresinde, her iki yoğunluğu çizimleri görmeniz gerekir.

Bağıntı çizimler, ayrıca ilginç. Oluşturmak için:

* Seçin **bağıntı** olarak **türü**, ardından
* Seçin **yürütme**.
* Çıngırağı, en çok 40 değişkenleri önerir sizi uyarır. Seçin **Evet** çizim görüntülemek için.

Gündeme bazı ilginç bağıntıları vardır: "teknolojisinin" kesinlikle bağıntılı "HP" ve "labs" Örneğin. Veri kümesi bağışta alan kodu 650 olduğundan "650" de kesinlikle ilişkilendirilir.

Sözcükler arasındaki bağıntıları sayısal değerleri Araştır penceresi için kullanılabilir. Örneğin, "teknolojisinin" olumsuz "sizin" bağıntılı olduğunu unutmayın ve "para" ilginç olacaktır.

Çıngırağı bazı yaygın sorunlar işlemek için veri kümesi dönüştürebilirsiniz. Örneğin, bu özellikleri yeniden Ölçeklendir, eksik değerleri impute, aykırı değerlerini işlemek ve değişkenlerin ya da eksik verilerle gözlemleri kaldırmanızı sağlar. Çıngırağı gözlemleri ve/veya değişkenleri arasındaki ilişkilendirme kuralları da tanımlayabilirsiniz. Bu sekme, bu tanıtıcı kılavuz için kapsam dışına:.

Çıngırağı küme analiz de gerçekleştirebilirsiniz. Şimdi çıktının okunmasını kolaylaştırmak için bazı özellikler çıkar. Üzerinde **veri** sekmesinde, seçin **Yoksay** her bir değişken on bu öğeler dışında yanındaki:

* word_freq_hp
* word_freq_technology
* word_freq_george
* word_freq_remove
* word_freq_your
* word_freq_dollar
* word_freq_money
* capital_run_length_longest
* word_freq_business
* istenmeyen posta

Daha sonra geri dönerek **küme** sekmesinde, seçin **KMeans**ve *küme sayısı* 4. Ardından **yürütme**. Sonuçlar çıktı penceresinde görüntülenir. Bir küme yüksek sıklığı "Hasan" ve "hp" vardır ve büyük olasılıkla yasal iş e-posta olur.

Basit karar ağacı makine öğrenimi modeli oluşturmak için:

* Seçin **modeli** sekmesi
* Seçin **ağaç** olarak **türü**.
* Seçin **yürütme** ağaç çıktı penceresinde metin biçiminde görüntülemek için.
* Seçin **çizin** grafik sürümünü görüntülemek için düğmeye. Bunu elde daha önce kullanarak ağacına oldukça benzer *rpart*.

İyi özelliklerinden Çıngırağı biri birkaç makine öğrenme yöntemlerini çalıştırma ve bunları hızlı bir şekilde değerlendirmek yeteneğidir. Yordam aşağıdaki gibidir:

* Seçin **tüm** için **türü**.
* Seçin **yürütme**.
* Sona erdikten sonra tüm tek tıklatabilirsiniz **türü**gibi **SVM**ve sonuçları görüntüleyin.
* Kullanılarak ayarlanan doğrulama modellerinde performansını de karşılaştırabilirsiniz **değerlendir** sekmesi. Örneğin, **hata matris** seçimi gösterir, karışıklığı matrisi, genel hata ve ortalama sınıfı hata her model için doğrulama ayarlayın.
* Ayrıca ROC eğriler çizme, duyarlılık çözümlemesi ve diğer türleri modeli değerlendirmeleri yapın.

Model oluşturmaya yaptıktan sonra seçeneğini **günlük** Çıngırağı tarafından oturumunuz sırasında çalışma R kodu görüntülemek için. Seçebileceğiniz **verme** kaydetmeyi düğmesi.

> [!NOTE]
> Çıngırağı geçerli sürümde bir hata var. Komut dosyasını değiştirin veya daha sonra buluncaya için kullanmak için bir # karakteri önüne yerleştirin *... Bu günlüğünü Dışarı Aktar * günlük metin.
>
>

## <a name="postgresql--squirrel-sql"></a>PostgreSQL & Squirrel SQL
DSVM yüklü PostgreSQL ile birlikte gelir. PostgreSQL Gelişmiş, açık kaynak ilişkisel veritabanıdır. Bu bölümde, istenmeyen posta kümemize PostgreSQL yüklemek ve ardından sorgu gösterilmektedir.

Verileri yüklemeden önce localhost parola kimlik doğrulamasını izin vermeniz gerekiyor. Bir komut isteminde:

    sudo gedit /var/lib/pgsql/data/pg_hba.conf

Yapılandırma dosyası altına izin verilen bağlantılar ayrıntı birden çok satıra şunlardır:

    # "local" is for Unix domain socket connections only
    local   all             all                                     trust
    # IPv4 local connections:
    host    all             all             127.0.0.1/32            ident
    # IPv6 local connections:
    host    all             all             ::1/128                 ident

Biz bir kullanıcı adı ve parola kullanarak oturum için md5 plainname yerine kullanmak için "IPv4 yerel bağlantılar" satırı değiştirin:

    # IPv4 local connections:
    host    all             all             127.0.0.1/32            md5

Ve postgres hizmetini yeniden başlatın:

    sudo systemctl restart postgresql

Etkileşimli terminal yerleşik postgres kullanıcı olarak, PostgreSQL için psql başlatmak için bir isteminden aşağıdaki komutu çalıştırın:

    sudo -u postgres psql

Şu anda olarak oturum açtınız ve parola verin Linux hesabıyla aynı kullanıcı adı kullanarak yeni bir kullanıcı hesabı oluşturun:

    CREATE USER <username> WITH CREATEDB;
    CREATE DATABASE <username>;
    ALTER USER <username> password '<password>';
    \quit

Psql için kullanıcı olarak sonra oturum açın:

    psql

Ve yeni bir veritabanına veri içeri aktarın:

    CREATE DATABASE spam;
    \c spam
    CREATE TABLE data (word_freq_make real, word_freq_address real, word_freq_all real, word_freq_3d real,word_freq_our real, word_freq_over real, word_freq_remove real, word_freq_internet real,word_freq_order real, word_freq_mail real, word_freq_receive real, word_freq_will real,word_freq_people real, word_freq_report real, word_freq_addresses real, word_freq_free real,word_freq_business real, word_freq_email real, word_freq_you real, word_freq_credit real,word_freq_your real, word_freq_font real, word_freq_000 real, word_freq_money real,word_freq_hp real, word_freq_hpl real, word_freq_george real, word_freq_650 real, word_freq_lab real,word_freq_labs real, word_freq_telnet real, word_freq_857 real, word_freq_data real,word_freq_415 real, word_freq_85 real, word_freq_technology real, word_freq_1999 real,word_freq_parts real, word_freq_pm real, word_freq_direct real, word_freq_cs real, word_freq_meeting real,word_freq_original real, word_freq_project real, word_freq_re real, word_freq_edu real,word_freq_table real, word_freq_conference real, char_freq_semicolon real, char_freq_leftParen real,char_freq_leftBracket real, char_freq_exclamation real, char_freq_dollar real, char_freq_pound real, capital_run_length_average real, capital_run_length_longest real, capital_run_length_total real, spam integer);
    \copy data FROM /home/<username>/spambase.data DELIMITER ',' CSV;
    \quit

Şimdi, şimdi verileri araştırırken kullanarak bazı sorgular çalıştırmak **Squirrel SQL**, JDBC sürücüsü aracılığıyla veritabanlarıyla etkileşim olanak sağlayan bir grafik aracıdır.

Başlamak için uygulamaları menüsünden Squirrel SQL başlatın. Sürücüsünü kurmak için:

* Seçin **Windows**, ardından **görüntülemek sürücüleri**.
* Sağ **PostgreSQL** seçip **değiştirmek sürücü**.
* Seçin **yolu'fazladan sınıf**, ardından **eklemek**.
* Girin ***/usr/share/java/jdbcdrivers/postgresql-9.4.1208.jre6.jar*** için **dosya adı** ve
* Seçin **açık**.
* Liste sürücüleri seçin ve ardından **org.postgresql.Driver** içinde **sınıf adı**seçip **Tamam**.

Yerel sunucu bağlantısını ayarlamak için:

* Seçin **Windows**, ardından **diğer adlar görüntüleyin.**
* Seçin **+** düğmesi yeni bir diğer ad yapın.
* Bu ad *istenmeyen posta veritabanı*, seçin **PostgreSQL** içinde **sürücü** açılır.
* URL'sini ayarlamak *jdbc:postgresql://localhost/spam*.
* Girin, *kullanıcıadı* ve *parola*.
* **Tamam**’a tıklayın.
* Açmak için **bağlantı** penceresinde çift ***istenmeyen posta veritabanı*** diğer adı.
* **Bağlan**’ı seçin.

Bazı sorgular çalıştırmak için:

* Seçin **SQL** sekmesi.
* Basit bir sorgu girin `SELECT * from data;` sorgu metin kutusuna SQL sekmenin üstünde.
* Tuşuna **Ctrl-Enter** çalıştırmak için. Varsayılan olarak Squirrel SQL sorgunuzdan ilk 100 satırı döndürür.

Bu verileri araştırmak için çalıştırabilir pek çok daha fazla sorguları vardır. Örneğin, word sıklığını nasıl mu *olun* istenmeyen ve ham arasında farklı?

    SELECT avg(word_freq_make), spam from data group by spam;

Veya sık içeren e-posta özelliklerini nelerdir *3B*?

    SELECT * from data order by word_freq_3d desc;

Yüksek bir oluşumunu sahip çoğu e-postaları *3B* olan görünüşe göre istenmeyen posta göndermek, e-postaları sınıflandırmak için Tahmine dayalı bir model oluşturmak için yararlı bir özellik olması.

Bir PostgreSQL veritabanında depolanan verilerle machine learning gerçekleştirmek istiyorsanız, kullanmayı [MADlib](http://madlib.incubator.apache.org/).

## <a name="sql-server-data-warehouse"></a>SQL Server veri ambarı
Azure SQL Veri Ambarı, hem ilişkisel hem de ilişkisel olmayan çok geniş hacimlerdeki verileri işleyebilen, bulut tabanlı bir genişletme veritabanıdır. Daha fazla bilgi için bkz: [Azure SQL Data Warehouse nedir?](../../sql-data-warehouse/sql-data-warehouse-overview-what-is.md)

Veri ambarına bağlanmak ve tablo oluşturmak için bir komut isteminden aşağıdaki komutu çalıştırın:

    sqlcmd -S <server-name>.database.windows.net -d <database-name> -U <username> -P <password> -I

Ardından sqlcmd isteminde:

    CREATE TABLE spam (word_freq_make real, word_freq_address real, word_freq_all real, word_freq_3d real,word_freq_our real, word_freq_over real, word_freq_remove real, word_freq_internet real,word_freq_order real, word_freq_mail real, word_freq_receive real, word_freq_will real,word_freq_people real, word_freq_report real, word_freq_addresses real, word_freq_free real,word_freq_business real, word_freq_email real, word_freq_you real, word_freq_credit real,word_freq_your real, word_freq_font real, word_freq_000 real, word_freq_money real,word_freq_hp real, word_freq_hpl real, word_freq_george real, word_freq_650 real, word_freq_lab real,word_freq_labs real, word_freq_telnet real, word_freq_857 real, word_freq_data real,word_freq_415 real, word_freq_85 real, word_freq_technology real, word_freq_1999 real,word_freq_parts real, word_freq_pm real, word_freq_direct real, word_freq_cs real, word_freq_meeting real,word_freq_original real, word_freq_project real, word_freq_re real, word_freq_edu real,word_freq_table real, word_freq_conference real, char_freq_semicolon real, char_freq_leftParen real,char_freq_leftBracket real, char_freq_exclamation real, char_freq_dollar real, char_freq_pound real, capital_run_length_average real, capital_run_length_longest real, capital_run_length_total real, spam integer) WITH (CLUSTERED COLUMNSTORE INDEX, DISTRIBUTION = ROUND_ROBIN);
    GO

Bcp ile veri kopyalayın:

    bcp spam in spambaseHeaders.data -q -c -t  ',' -S <server-name>.database.windows.net -d <database-name> -U <username> -P <password> -F 1 -r "\r\n"

> [!NOTE]
> İndirilen Dosya, satır sonları Windows stili olsa da - r bayrağıyla söyleyin bcp ihtiyacımız şekilde bcp UNIX stilinde bekliyor.
>
>

Ve sorgu sqlcmd ile:

    select top 10 spam, char_freq_dollar from spam;
    GO

Ayrıca Squirrel SQL ile sorgulayabilir. İçinde bulunan Microsoft MSSQL Server JDBC sürücüsü kullanarak PostgreSQL için benzer adımları ***/usr/share/java/jdbcdrivers/sqljdbc42.jar***.

## <a name="next-steps"></a>Sonraki adımlar
Azure veri bilimi işlemi oluşturan görevler size yol konuları genel bakış için bkz: [takım veri bilimi işlemi](http://aka.ms/datascienceprocess).

Belirli senaryolar için takım veri bilimi işlemdeki adımlar gösteren diğer uçtan uca talimatlara açıklaması için bkz: [takım veri bilimi süreci gözden geçirmeleri](../team-data-science-process/walkthroughs.md). İzlenecek yollar da Bulut ve şirket içi araçları ve Hizmetleri bir iş akışı veya akıllı bir uygulama oluşturmak için ardışık düzen birleştirmek nasıl gösterilmektedir.
