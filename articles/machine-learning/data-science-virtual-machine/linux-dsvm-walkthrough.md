---
title: Linux veri bilimi Sanal Makinesi'ni kullanmayı öğrenin
titleSuffix: Azure
description: Linux veri bilimi sanal makinesi ile çeşitli genel veri bilimi görevlerini gerçekleştirme.
services: machine-learning
documentationcenter: ''
author: gopitk
manager: cgronlun
editor: cgronlun
ms.custom: seodec18
ms.assetid: 34ef0b10-9270-474f-8800-eecb183bbce4
ms.service: machine-learning
ms.subservice: data-science-vm
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/16/2018
ms.author: gokuma
ms.openlocfilehash: 6e8883870cc0f035df5122e91449f04203836218
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60516901"
---
# <a name="data-science-with-a-linux-data-science-virtual-machine-on-azure"></a>İle bir Linux veri bilimi sanal makinesi Azure üzerinde veri bilimi
Bu izlenecek yol, Linux veri bilimi sanal makinesi ile çeşitli genel veri bilimi görevlerini gerçekleştirmek nasıl gösterir. Linux veri bilimi sanal makinesi (DSVM) veri analizi ve makine öğrenimi için yaygın olarak kullanılan araçları koleksiyonu ile önceden yüklenmiş olan Azure üzerinde kullanılabilir bir sanal makine görüntüsüdür. Anahtar yazılım bileşenleri içinde listelenen [Linux veri bilimi sanal makinesi sağlama](linux-dsvm-intro.md) konu. VM görüntüsü, yüklemek ve araçların her biri ayrı ayrı yapılandırmak zorunda kalmadan, dakikalar içinde veri bilimi yapmaya başlayın kolaylaştırır. Kolayca VM'yi, gerekirse ölçeği ve kullanımda olmadığında durdurun. Bu nedenle bu kaynak, esnek ve maliyet açısından verimli içindir.

Bu izlenecek yolda gösterilen veri bilimi görevleri konusunda özetlenen adımları [Team Data Science Process](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/overview). Bu işlem, takımları, veri uzmanları, akıllı uygulamalar oluşturma yaşam döngüsü üzerinde etkin bir biçimde işbirliği sağlayan veri bilimi için sistematik bir yaklaşım sunar. Veri bilimi işlemi ayrıca bir kişi tarafından izlenebilir veri bilimi için yinelemeli bir çerçeve sağlar.

Analiz ediyoruz [spambase](https://archive.ics.uci.edu/ml/datasets/spambase) bu kılavuzda açıklanan veri kümesi. İstenmeyen posta ya da (olmadıkları istenmeyen posta anlamına gelir) ham, işaretlenmiş e-postalar bir dizi budur ve bazı İstatistikler e-postaları içeriğine de içerir. Dahil edilen istatistikleri sonraki ancak bir bölümde ele alınmıştır.

## <a name="prerequisites"></a>Önkoşullar
Linux veri bilimi sanal makinesi kullanabilmeniz için önce aşağıdakilere sahip olmanız gerekir:

* Bir **Azure aboneliği**. Zaten bir yoksa, bkz. [ücretsiz Azure hesabınızı hemen oluşturun](https://azure.microsoft.com/free/).
* A [ **Linux veri bilimi sanal makinesi**](https://azure.microsoft.com/marketplace/partners/microsoft-ads/linux-data-science-vm). Bu VM sağlama hakkında daha fazla bilgi için bkz: [Linux veri bilimi sanal makinesi sağlama](linux-dsvm-intro.md).
* [X2Go](https://wiki.x2go.org/doku.php) bilgisayarınızda yüklü ve XFCE oturum açıldı. Yükleme ve yapılandırma hakkında bilgi için bir **X2Go istemci**, bkz: [yükleme ve yapılandırma X2Go istemci](linux-dsvm-intro.md#installing-and-configuring-x2go-client).
* Hakkında gfx.xrender.enabled bayrağı daha yumuşak kaydırma deneyimi için geçiş: sanal makineleri FireFox tarayıcısı yapılandırmada. [Buradan daha fazla bakın. ](https://www.reddit.com/r/firefox/comments/4nfmvp/ff_47_unbearable_slow_over_remote_x11/). Ayrıca geçiş göz önünde bulundurun *mousewheel.enable_pixel_scrolling* false. [Buradaki yönergeleri.](https://support.mozilla.org/en-US/questions/981140)
* Bir **AzureML hesabı**. Yeni hesap için kaydolun biri yoksa, [AzureML giriş sayfası](https://studio.azureml.net/). Başlamanıza yardımcı olmak için ücretsiz kullanım katman mevcuttur.

## <a name="download-the-spambase-dataset"></a>Spambase veri kümesini indirin
[Spambase](https://archive.ics.uci.edu/ml/datasets/spambase) yalnızca 4601 örnekler içeren veri görece küçük bir veri kümesidir. Bu, veri bilimi sanal makinesi olarak anahtar özelliklerinden bazılarını korur, kaynak gereksinimlerini büyüklükteki gösteren kullanılacak uygun bir boyutudur.

> [!NOTE]
> Bu izlenecek yol, bir D2 v2 ölçekli Linux veri bilimi sanal Makinesi'nin (CentOS sürüm) oluşturuldu. Bu boyut DSVM yordamları bu kılavuzda işleyebilir.
>
>

Daha fazla depolama alanı gerekiyorsa, ek diskler oluşturma ve bunları sanal Makinenize ekleyin. Sunucu yeniden boyutlandırma nedeniyle daralıp veya kapatılmış olsa bile verilerine korunacak şekilde kalıcı Azure depolama, bu diskleri kullanın. Bir disk ekleyin ve sanal Makinenize eklemek için yönergeleri izleyin. [bir Linux VM'ye disk ekleme](../../virtual-machines/linux/add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Azure komut satırı DSVM üzerinde zaten yüklü arabirimi (Azure CLI), aşağıdaki adımları kullanın. Bu nedenle bu yordamları tamamen VM'den kendisini yapılabilir. Depolama alanını artırmak için başka bir seçenek kullanmaktır [Azure dosyaları](../../storage/files/storage-how-to-use-files-linux.md).

Verileri indirmek için bir terminal penceresi açın ve şu komutu çalıştırın:

    wget https://archive.ics.uci.edu/ml/machine-learning-databases/spambase/spambase.data

İndirilen dosyayı şimdi üst bilgi yok. başka bir dosya oluşturun bir başlık satırı yok. Uygun üst bilgilerle bir dosya oluşturmak için şu komutu çalıştırın:

    echo 'word_freq_make, word_freq_address, word_freq_all, word_freq_3d,word_freq_our, word_freq_over, word_freq_remove, word_freq_internet,word_freq_order, word_freq_mail, word_freq_receive, word_freq_will,word_freq_people, word_freq_report, word_freq_addresses, word_freq_free,word_freq_business, word_freq_email, word_freq_you, word_freq_credit,word_freq_your, word_freq_font, word_freq_000, word_freq_money,word_freq_hp, word_freq_hpl, word_freq_george, word_freq_650, word_freq_lab,word_freq_labs, word_freq_telnet, word_freq_857, word_freq_data,word_freq_415, word_freq_85, word_freq_technology, word_freq_1999,word_freq_parts, word_freq_pm, word_freq_direct, word_freq_cs, word_freq_meeting,word_freq_original, word_freq_project, word_freq_re, word_freq_edu,word_freq_table, word_freq_conference, char_freq_semicolon, char_freq_leftParen,char_freq_leftBracket, char_freq_exclamation, char_freq_dollar, char_freq_pound, capital_run_length_average,capital_run_length_longest, capital_run_length_total, spam' > headers

Komutu ile birlikte iki dosyayı gösterip birleştirir:

    cat spambase.data >> headers
    mv headers spambaseHeaders.data

Veri kümesi her e-posta istatistikleri çeşitli sahiptir:

* Sütunları ister ***word\_freq\_WORD*** eşleşen e-posta sözcükleri yüzdesini belirtmek *WORD*. Örneğin, varsa *word\_freq\_olun* %1 e-postadaki tüm bir kelimelerin bundan sonra 1 ' dir *olun*.
* Sütunları ister ***char\_freq\_CHAR*** olan tüm e-posta karakterleri yüzdesini belirtmek *CHAR*.
* ***büyük\_çalıştırma\_uzunluğu\_uzun*** büyük harf dizisini uzun uzunluğu.
* ***büyük\_çalıştırma\_uzunluğu\_ortalama*** büyük harf tüm dizileri ortalama uzunluğu.
* ***büyük\_çalıştırma\_uzunluğu\_toplam*** büyük harf tüm dizileri toplam uzunluğu.
* ***istenmeyen posta*** veya e-posta istenmeyen posta kabul olup olmadığını belirtir (1 istenmeyen-posta, 0 = değil istenmeyen posta =).

## <a name="explore-the-dataset-with-microsoft-r-open"></a>Microsoft R Open kümesiyle keşfedin
Şimdi verileri incelemek ve r ile bazı temel makine yapın Veri bilimi sanal makinesi ile birlikte gelen [Microsoft R Open](https://mran.revolutionanalytics.com/open/) önceden yüklenmiş. R'ın bu sürümünde çok iş parçacıklı matematik kitaplıkları, çeşitli tek iş parçacıklı sürümleri daha iyi performans sunar. Microsoft R Open de yeniden üretilebilirliğini CRAN Paket Deposu anlık görüntüsünü kullanarak sağlar.

Bu kılavuzda kullanılan kod örnekleri kopyalarını almak için kopyalama **Azure-Machine-Learning-veri bilimi** git, sanal makinede önceden yüklenmiş olduğu kullanarak depo. Git komut satırından çalıştırın:

    git clone https://github.com/Azure/Azure-MachineLearning-DataScience.git

Bir terminal penceresi açın ve R etkileşimli konsol ile yeni bir R oturumu başlatmak veya RStudio makinede önceden kullanın.


Ortamı ayarlayın ve verileri içeri aktarmak için çalıştırın:

    data <- read.csv("spambaseHeaders.data")
    set.seed(123)

Her sütun hakkında özet istatistikleri görmek için:

    summary(data)

Verileri farklı bir görünüm için:

    str(data)

Bu, her bir değişken ve ilk birkaç değer türü veri kümesini gösterir.

*İstenmeyen posta* sütun bir tamsayı olarak okundu, ancak gerçekte kategorik olan değişken (veya faktörü). Türünü ayarlamak için:

    data$spam <- as.factor(data$spam)

Bazı keşif analizi yapmak için [ggplot2](https://ggplot2.tidyverse.org/) paketini, sanal makinede zaten yüklü R için popüler bir grafik kitaplığı. , Daha önce gösterilen özet verileri biz Özet istatistikleri ünlem işareti karakter sıklığı temel unutmayın. Şimdi bu frekansları burada aşağıdaki komutları kullanarak Çiz:

    library(ggplot2)
    ggplot(data) + geom_histogram(aes(x=char_freq_exclamation), binwidth=0.25)

Sıfır çubuğu, çizim eğriltme olduğundan, şimdi bunu kurtulun:

    email_with_exclamation = data[data$char_freq_exclamation > 0, ]
    ggplot(email_with_exclamation) + geom_histogram(aes(x=char_freq_exclamation), binwidth=0.25)

Önemsiz olmayan bir yoğunluklu ilginç görünüyor 1 yukarıda yoktur. Yalnızca bu verilere göz atalım:

    ggplot(data[data$char_freq_exclamation > 1, ]) + geom_histogram(aes(x=char_freq_exclamation), binwidth=0.25)

Ardından istenmeyen posta vs ham göre böl:

    ggplot(data[data$char_freq_exclamation > 1, ], aes(x=char_freq_exclamation)) +
    geom_density(lty=3) +
    geom_density(aes(fill=spam, colour=spam), alpha=0.55) +
    xlab("spam") +
    ggtitle("Distribution of spam \nby frequency of !") +
    labs(fill="spam", y="Density")

Bu örnekler diğer sütunlara benzer çizimler içinde yer alan verileri araştırmak için yapmanızı etkinleştirmeniz gerekir.

## <a name="train-and-test-an-ml-model"></a>Eğitme ve ML model test etme
Şimdi şimdi birkaç veri kümesinde span veya ham içeren olarak e-postaları sınıflandırmak için makine öğrenimi modellerini eğitin. Biz karar ağacı modeli ve bu bölümde bir rastgele orman modeli eğitmek ve sonra kendi tahmin doğruluğunu test.

> [!NOTE]
> Aşağıdaki kodda kullanılan rpart (yinelemeli bölümleme ve regresyon ağaçlarında) paketi üzerinde veri bilimi sanal makinesi zaten yüklü.
>
>

Şimdi veri kümesini eğitim ve test kümeleri halinde bölme ilk:

    rnd <- runif(dim(data)[1])
    trainSet = subset(data, rnd <= 0.7)
    testSet = subset(data, rnd > 0.7)

' İ tıklatın ve ardından e-postaları sınıflandırmak için bir karar şeması oluştur.

    require(rpart)
    model.rpart <- rpart(spam ~ ., method = "class", data = trainSet)
    plot(model.rpart)
    text(model.rpart)

Sonuç şu şekildedir:

![1](./media/linux-dsvm-walkthrough/decision-tree.png)

Eğitim kümesinde ne kadar iyi gerçekleştirdiği belirlemek için aşağıdaki kodu kullanın:

    trainSetPred <- predict(model.rpart, newdata = trainSet, type = "class")
    t <- table(`Actual Class` = trainSet$spam, `Predicted Class` = trainSetPred)
    accuracy <- sum(diag(t))/sum(t)
    accuracy

Ne kadar iyi belirlemek için test kümesinde gerçekleştirir:

    testSetPred <- predict(model.rpart, newdata = testSet, type = "class")
    t <- table(`Actual Class` = testSet$spam, `Predicted Class` = testSetPred)
    accuracy <- sum(diag(t))/sum(t)
    accuracy

Ayrıca bir rastgele orman modeli deneyelim. Rastgele ormanları karar ağaçları çok sayıda eğitmek ve ayrı ayrı karar ağaçları tüm sınıflandırmaları modu bir sınıfı çıktı. Bunlar bir eğitim veri kümesi overfit işlemciler karar ağacı modeli için olarak yaklaşımı öğrenmek, daha güçlü bir makine sağlarlar.

    require(randomForest)
    trainVars <- setdiff(colnames(data), 'spam')
    model.rf <- randomForest(x=trainSet[, trainVars], y=trainSet$spam)

    trainSetPred <- predict(model.rf, newdata = trainSet[, trainVars], type = "class")
    table(`Actual Class` = trainSet$spam, `Predicted Class` = trainSetPred)

    testSetPred <- predict(model.rf, newdata = testSet[, trainVars], type = "class")
    t <- table(`Actual Class` = testSet$spam, `Predicted Class` = testSetPred)
    accuracy <- sum(diag(t))/sum(t)
    accuracy


## <a name="deploy-a-model-to-azure-machine-learning-studio"></a>Azure Machine Learning Studio'da model dağıtma
[Azure Machine Learning Studio](https://studio.azureml.net/) , Tahmine dayalı analiz modellerini Derleme ve dağıtma kolay bir bulut hizmetidir. Azure Machine Learning Studio'nun iyi özelliklerinden herhangi bir web hizmeti olarak R işlevi yayımlamak için olmasıdır. Azure Machine Learning studio R paketi dağıtım sağ DSVM bizim R oturum yapmak daha kolay hale getirir.

Önceki bölümde karar ağacı kodu dağıtmak için Azure Machine Learning Studio'da oturum açmanız gerekir. Çalışma alanı Kimliğiniz ve bir yetkilendirme belirteci oturum açmanız gerekir. Bu değerleri bulmak ve onlarla Azure Machine Learning değişkenlerini başlatmak için:

Seçin **ayarları** sol menüdeki. Not, **çalışma alanı kimliği**. ![2](./media/linux-dsvm-walkthrough/workspace-id.png)

Seçin **yetkilendirme belirteçleri** Not ve ek yükü menüden, **birincil yetkilendirme belirteci**.![ 3](./media/linux-dsvm-walkthrough/workspace-token.png)

Yük **AzureML** paketini ve ardından belirteci ve çalışma alanı Kimliğiniz ile değişkenlerin değerlerini R oturumunuzda DSVM'nin ayarlayın:

    if(!require("AzureML")) install.packages("AzureML")
    require(AzureML)
    wsAuth = "<authorization-token>"
    wsID = "<workspace-id>"


Şimdi bu gösteride uygulamak daha kolay hale getirmek için model basitleştirin. Karar ağacının kök en yakın üç değişkenin seçin ve yalnızca bu üç değişkenin kullanarak yeni bir ağaç oluşturun:

    colNames <- c("char_freq_dollar", "word_freq_remove", "word_freq_hp", "spam")
    smallTrainSet <- trainSet[, colNames]
    smallTestSet <- testSet[, colNames]
    model.rpart <- rpart(spam ~ ., method = "class", data = smallTrainSet)

Özellikleri bir girdi olarak alır ve tahmin edilen değerler döndüren bir tahmin işlevi ihtiyacımız:

    predictSpam <- function(newdata) {
      predictDF <- predict(model.rpart, newdata = newdata)
      return(colnames(predictDF)[apply(predictDF, 1, which.max)])
    }


AzureML kullanarak predictSpam işlevi yayımlama **publishWebService** işlevi:

    spamWebService <- publishWebService(ws, fun = predictSpam, name="spamWebService", inputSchema = smallTrainSet, data.frame=TRUE)


Bu işlev alır **predictSpam** işlev, adlı bir web hizmeti oluşturur **spamWebService** girdileri ve çıktıları ile tanımlanan ve yeni uç nokta hakkında bilgi verir.

Kendi API uç noktası dahil olmak üzere en son yayımlanan web hizmeti, ayrıntılarını görüntülemek ve erişim anahtarları komutu:

    s<-tail(services(ws, name = "spamWebService"), 1)
    ep <- endpoints(ws,s)
    ep

İlk denemenin test 10 satır ayarlayın:

    consume(ep, smallTestSet[1:10, ])


## <a name="use-other-tools-available"></a>Diğer araçları kullanma
Kalan bölümler, Linux veri bilimi sanal makinesi üzerinde yüklü araçlardan bazıları kullanmayı göstermektedir. Ele alınan araçlarının listesi aşağıda verilmiştir:

* XGBoost
* Python
* Jupyterhub
* Çıngırağı
* PostgreSQL ve Squirrel SQL
* SQL Server veri ambarı

## <a name="xgboost"></a>XGBoost
[XGBoost](https://xgboost.readthedocs.org/en/latest/) , hızlı ve doğru artırmalı ağaç uygulaması sağlayan bir araçtır.

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

XGBoost, python veya bir komut satırından de çağırabilirsiniz.

## <a name="python"></a>Python
Python kullanarak geliştirme için Anaconda Python 2.7 ve 3.5 dağıtımlar DSVM yüklenmiş.

> [!NOTE]
> Anaconda dağıtım içerir [Conda](https://conda.pydata.org/docs/index.html), özel ortamlarda farklı sürümleri ve/veya bunların yüklü paketleri olan Python oluşturmak için kullanılabilir.
>
>

Şimdi bazı spambase kümesinin okuyun ve Destek vektör makinelerle scikit e-postaları sınıflandırmak-öğrenin:

    import pandas
    from sklearn import svm
    data = pandas.read_csv("spambaseHeaders.data", sep = ',\s*')
    X = data.ix[:, 0:57]
    y = data.ix[:, 57]
    clf = svm.SVC()
    clf.fit(X, y)

Tahminde bulunmak için:

    clf.predict(X.ix[0:20, :])

R modeli daha önce yayımladığımızda yaptığımız gibi AzureML uç nokta yayımlama işlemini göstermek için daha basit bir model üç değişkenin olalım.

    X = data[["char_freq_dollar", "word_freq_remove", "word_freq_hp"]]
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
> Bu, yalnızca python 2.7 için kullanılabilir ve 3.5 üzerinde henüz desteklenmiyor. Çalıştırma **/anaconda/bin/python2.7**.
>
>

## <a name="jupyterhub"></a>Jupyterhub
Anaconda dağıtım DSVM bir Jupyter not defteri ile Python, R ya da Julia kod ve analiz paylaşmak için bir platformlar arası ortamda gelir. Jupyter not defteri JupyterHub erişilir. Yerel Linux kullanıcı adı ve parolayı kullanarak oturum ***https://\<VM DNS adı veya IP adresi\>: 8000 /***. JupyterHub için tüm yapılandırma dosyalarını dizininde bulunan **/etc/jupyterhub**.

> [!NOTE]
> Python Paket Yöneticisi'ni (aracılığıyla `pip` komut) geçerli çekirdek Jupyter not defterinden aşağıdaki komutu kod hücresine örneğin kullanılabilir:
>   ```python
>    import sys
>    ! {sys.executable} -m pip install numpy -y
>   ```
> 
> 
> 
> [!NOTE]
> Conda yükleyici kullanılacak (aracılığıyla `conda` komut) geçerli çekirdek Jupyter not defterinden aşağıdaki komutu kod hücresine örneğin kullanılabilir:
>   ```python
>    import sys
>    ! {sys.prefix}/bin/conda install --yes --prefix {sys.prefix} numpy
>   ```

Birkaç örnek not defterleri VM üzerinde zaten yüklü:

* Bkz: [IntroToJupyterPython.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Data-Science-Virtual-Machine/Samples/Notebooks/IntroToJupyterPython.ipynb) örnek Python not defteri için.
* Bkz: [IntroTutorialinR](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Data-Science-Virtual-Machine/Samples/Notebooks/IntroTutorialinR.ipynb) örneği **R** dizüstü bilgisayar.
* Bkz: [IrisClassifierPyMLWebService](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Data-Science-Virtual-Machine/Samples/Notebooks/IrisClassifierPyMLWebService.ipynb) başka bir örnek için **Python** dizüstü bilgisayar.

> [!NOTE]
> Julia diline Linux veri bilimi sanal makinesi üzerinde komut satırından da kullanılabilir.
>
>

## <a name="rattle"></a>Çıngırağı
[Rattle](https://cran.r-project.org/web/packages/rattle/index.html) (R analitik aracı için bilgi kolayca) grafiksel bir R veri madenciliği için aracıdır. Yük, keşfedin ve veri dönüştürme ve derleme ve modellerin değerlendirmesi daha kolay hale getirir, sezgisel bir arabirim var.  Makaleyi [Rattle: R için bir veri madenciliği GUI](https://journal.r-project.org/archive/2009-2/RJournal_2009-2_Williams.pdf) özelliklerini gösteren bir kılavuz sağlar.

Yükleyin ve aşağıdaki komutlarla Çıngırağı başlatın:

    if(!require("rattle")) install.packages("rattle")
    require(rattle)
    rattle()

> [!NOTE]
> DSVM'nin yüklemesi gerekli değildir. Ancak, yüklediğinde, ek paketler yüklemek için Çıngırağı isteyebilir.
>
>

Çıngırağı için sekmesinde tabanlı bir arabirim kullanır. Sekmeler çoğu karşılık adımlarda [Data Science Process](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/), veri yükleme veya araştırma gibi. Veri bilimi işlemi soldan sağa sekmeler arasında akar. Ancak son sekmeyi R komutlar Çıngırağı tarafından çalıştırılır ve günlüğü içeriyor.

Yük ve veri kümesini yapılandırmak için:

* Dosya yüklemek için işaretleyin **veri** sekmesini, ardından
* Yanındaki seçicisinde **Filename** ve **spambaseHeaders.data**.
* Dosya yüklenemiyor. seçin **yürütme** düğmelerinin üst satırında. Girdi, bir hedef veya başka türde değişken ve benzersiz değerlerin sayısını olup olmadığını, belirtilen veri türü dahil olmak üzere her bir sütunun özeti görmeniz gerekir.
* Çıngırağı doğru tanımladı **istenmeyen posta** hedefi olarak bir sütun. İstenmeyen posta sütunu seçin ve ardından ayarlamak **hedef veri türü** için **Categoric**.

Verileri araştırmak için:

* Seçin **Araştır** sekmesi.
* Tıklayın **Özet**, ardından **yürütme**değişken türleri hakkında bazı bilgiler ve bazı Özet istatistikleri görmek için.
* Her değişken hakkında istatistikler diğer türlerini görüntülemek için gibi diğer seçenekleri Seç **açıklayın** veya **Temelleri**.

**Araştır** sekmesi de çok sayıda bilgilendirici çizimleri oluşturmak olanak tanır. Verilerin bir histogram çizmek için:

* Seçin **dağıtımları**.
* Denetleme **Histogram** için **word_freq_remove** ve **word_freq_you**.
* **Yürüt**’ü seçin. Her iki yoğunluklu çizimleri sözcük "," çok daha sık "kaldırmak daha" e-postalarda göründüğünü NET olduğu bir tek bir grafik penceresinde görmeniz gerekir.

Bağıntı çizimler, ayrıca ilginç. Oluşturmak için:

* Seçin **bağıntı** olarak **türü**, ardından
* **Yürüt**’ü seçin.
* En fazla 40 değişkenler önerir Çıngırağı sizi uyarır. Seçin **Evet** çizim görüntülemek için.

Gündeme bazı ilginç bağıntılar vardır: "teknoloji" kesin bağıntılı "HP" ve "Laboratuvarları" Örneğin. Veri kümesi bağışçılarının alan kodu 650 olduğundan "650" de ilişkilendirilir.

Sözcükler arasındaki bağıntıları sayısal değerlerini İncele penceresinde kullanılabilir. Örneğin, "teknoloji" olumsuz "sizin" ile ilişkilendirilir olduğunu unutmayın ve "para" ilginç.

Bazı yaygın sorunlar işlemek için veri kümesi Çıngırağı dönüştürebilirsiniz. Örneğin, bu özellikleri yeniden ölçeklendirmek, eksik değerleri impute, aykırı değerleri işlemek ve değişkenleri veya gözlemleri eksik verileri kaldırmak sağlar. Çıngırağı gözlemleri ve/veya değişkenleri arasında ilişkilendirme kuralları da tanımlayabilirsiniz. Bu sekme, bu tanıtıcı kılavuz için kapsam dışına:.

Çıngırağı ayrıca küme analiz gerçekleştirebilirsiniz. Şimdi çıkış okunmasını kolaylaştırmak için bazı özellikler hariç tutun. Üzerinde **veri** sekmesini, **Yoksay** yanındaki her bir değişken bu on öğeleri hariç:

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

Ardından dönün **küme** sekmesini, **KMeans**, ayarlayıp *küme sayısı* 4. Ardından **yürütme**. Sonuçlar, çıkış penceresinde görüntülenir. Bir küme yüksek sıklık düzeyi "george" ve "hp" sahiptir ve büyük olasılıkla bir meşru bir iş e-posta.

Basit karar ağacı makine öğrenme modeli oluşturmak için:

* Seçin **modeli** sekmesinde
* Seçin **ağaç** olarak **türü**.
* Seçin **yürütme** çıkış penceresindeki metin biçiminde ağaç görüntülenecek.
* Seçin **çizmek** grafik sürümünü görüntülemek için düğme. Bunu elde daha önce kullanarak ağacına oldukça benzer *rpart*.

Çıngırağı iyi özelliklerinden biri çeşitli makine öğrenme yöntemleri çalışır ve hızlı bir şekilde değerlendirmek için olmasıdır. Yordam şöyledir:

* Seçin **tüm** için **türü**.
* **Yürüt**’ü seçin.
* Bittikten sonra tek tıklayabilirsiniz **türü**gibi **SVM**ve sonuçları görüntüleyin.
* Modelleri kullanarak doğrulama performansını de karşılaştırabilirsiniz **değerlendir** sekmesi. Örneğin, **hata matris** seçimi gösterilir karışıklık matrisi, genel hata ve ortalama sınıf hatası her model için doğrulama kümesinde.
* Ayrıca ROC eğrilerini, duyarlılığı analiz gerçekleştirmek ve diğer türleri model değerlendirme yapın.

İşiniz bittiğinde modeller oluşturma seçeneğini belirleyin **günlük** Çıngırağı tarafından oturumunuz sırasında çalıştırmak R kodunu görüntülemek için sekmesinde. Seçebileceğiniz **dışarı** kaydetmek için düğme.

> [!NOTE]
> Çıngırağı geçerli sürümde hata yoktur. Komut dosyasını değiştirin veya sonraki adımlarınızı yinelenecek kullanmak için bir # karakteri önüne Ekle *bu günlüğünü dışarı aktar...*  metin günlüğü.
>
>

## <a name="postgresql--squirrel-sql"></a>PostgreSQL ve Squirrel SQL
DSVM yüklü PostgreSQL ile birlikte gelir. PostgreSQL, bir karmaşık, açık kaynaklı ilişkisel veritabanı hizmetidir. Bu bölümde, istenmeyen posta kümemizi PostgreSQL yükleme ve sorgulayın gösterilmektedir.

Verileri yüklemek önce parola kimlik doğrulamasını localhost izin vermeniz gerekir. Bir komut isteminde:

    sudo gedit /var/lib/pgsql/data/pg_hba.conf

Alt yapılandırma dosyası izin verilen bağlantı ayrıntılarını birden fazla satır şunlardır:

    # "local" is for Unix domain socket connections only
    local   all             all                                     trust
    # IPv4 local connections:
    host    all             all             127.0.0.1/32            ident
    # IPv6 local connections:
    host    all             all             ::1/128                 ident

Biz bir kullanıcı adı ve parolayı kullanarak oturum için md5 ident yerine kullanmak için "IPv4 yerel bağlantılar" satırı değiştirin:

    # IPv4 local connections:
    host    all             all             127.0.0.1/32            md5

Ve postgres hizmetini yeniden başlatın:

    sudo systemctl restart postgresql

PostgreSQL, yerleşik postgres kullanıcı için bir etkileşimli terminal psql başlatmak için bir isteminden aşağıdaki komutu çalıştırın:

    sudo -u postgres psql

Şu anda olarak oturum açtınız ve parola verin Linux hesabıyla aynı kullanıcı adı kullanarak yeni bir kullanıcı hesabı oluşturun:

    CREATE USER <username> WITH CREATEDB;
    CREATE DATABASE <username>;
    ALTER USER <username> password '<password>';
    \quit

Psql için kullanıcı daha sonra oturum açın:

    psql

Ve yeni bir veritabanına veri içeri aktarın:

    CREATE DATABASE spam;
    \c spam
    CREATE TABLE data (word_freq_make real, word_freq_address real, word_freq_all real, word_freq_3d real,word_freq_our real, word_freq_over real, word_freq_remove real, word_freq_internet real,word_freq_order real, word_freq_mail real, word_freq_receive real, word_freq_will real,word_freq_people real, word_freq_report real, word_freq_addresses real, word_freq_free real,word_freq_business real, word_freq_email real, word_freq_you real, word_freq_credit real,word_freq_your real, word_freq_font real, word_freq_000 real, word_freq_money real,word_freq_hp real, word_freq_hpl real, word_freq_george real, word_freq_650 real, word_freq_lab real,word_freq_labs real, word_freq_telnet real, word_freq_857 real, word_freq_data real,word_freq_415 real, word_freq_85 real, word_freq_technology real, word_freq_1999 real,word_freq_parts real, word_freq_pm real, word_freq_direct real, word_freq_cs real, word_freq_meeting real,word_freq_original real, word_freq_project real, word_freq_re real, word_freq_edu real,word_freq_table real, word_freq_conference real, char_freq_semicolon real, char_freq_leftParen real,char_freq_leftBracket real, char_freq_exclamation real, char_freq_dollar real, char_freq_pound real, capital_run_length_average real, capital_run_length_longest real, capital_run_length_total real, spam integer);
    \copy data FROM /home/<username>/spambase.data DELIMITER ',' CSV;
    \quit

Şimdi, şimdi verileri araştırın ve kullanarak bazı sorguları **Squirrel SQL**, JDBC sürücüsü aracılığıyla veritabanları ile etkileşime olanak tanıyan grafik bir araç.

Başlamak için Squirrel SQL uygulamaları Menüsü'nden başlatın. Sürücüyü ayarlamak için:

* Seçin **Windows**, ardından **görüntülemek sürücüleri**.
* Sağ **PostgreSQL** seçip **değiştirme sürücü**.
* Seçin **yolu'ekstra sınıf**, ardından **ekleme**.
* Girin ***/usr/share/java/jdbcdrivers/postgresql-9.4.1208.jre6.jar*** için **dosya adı** ve
* **Aç**'ı seçin.
* Liste sürücüleri seçin ve ardından **org.postgresql.Driver** içinde **sınıf adı**seçip **Tamam**.

Yerel sunucusuyla bağlantıyı ayarlamak için:

* Seçin **Windows**, ardından **diğer adlarını görüntüleyin.**
* Seçin **+** yeni bir diğer ad düğmesini seçin.
* Adlandırın *istenmeyen posta veritabanı*, seçin **PostgreSQL** içinde **sürücü** açılır.
* URL'sini ayarlamak *jdbc:postgresql://localhost/spam*.
* Girin, *kullanıcıadı* ve *parola*.
* **Tamam** düğmesine tıklayın.
* Açmak için **bağlantı** penceresinde çift ***istenmeyen posta veritabanı*** diğer adı.
* **Bağlan**’ı seçin.

Bazı sorgular çalıştırmak için:

* Seçin **SQL** sekmesi.
* Basit bir sorgu girin `SELECT * from data;` sorgu metin kutusuna SQL sekmenin üstünde.
* Tuşuna **Ctrl-Enter** çalıştırmak için. Varsayılan olarak, ilk 100 satırı sorgunuzdan Squirrel SQL döndürür.

Bu verileri araştırmak için çalıştırabileceğiniz çok daha fazla sorgu yok. Örneğin, word sıklığını nasıl yaptığını *olun* istenmeyen posta ve ham farklıdır?

    SELECT avg(word_freq_make), spam from data group by spam;

Veya sık içeren e-posta özelliklerini nelerdir *3B*?

    SELECT * from data order by word_freq_3d desc;

Yüksek bir oluşumunu olan çoğu e-postaları *3B* olan görünüşe göre istenmeyen, böylece e-postaları sınıflandırmak için Tahmine dayalı bir model oluşturmaya yönelik kullanışlı bir özelliği olabilir.

Bir PostgreSQL veritabanına depolanmış verilerle machine learning gerçekleştirmek istiyorsanız, kullanmayı [MADlib](https://madlib.incubator.apache.org/).

## <a name="sql-server-data-warehouse"></a>SQL Server veri ambarı
Azure SQL Veri Ambarı, hem ilişkisel hem de ilişkisel olmayan çok geniş hacimlerdeki verileri işleyebilen, bulut tabanlı bir genişletme veritabanıdır. Daha fazla bilgi için [Azure SQL veri ambarı nedir?](../../sql-data-warehouse/sql-data-warehouse-overview-what-is.md)

Veri ambarı'na bağlanmak ve tablo oluşturmak için bir komut isteminden aşağıdaki komutu çalıştırın:

    sqlcmd -S <server-name>.database.windows.net -d <database-name> -U <username> -P <password> -I

Ardından sqlcmd isteminde:

    CREATE TABLE spam (word_freq_make real, word_freq_address real, word_freq_all real, word_freq_3d real,word_freq_our real, word_freq_over real, word_freq_remove real, word_freq_internet real,word_freq_order real, word_freq_mail real, word_freq_receive real, word_freq_will real,word_freq_people real, word_freq_report real, word_freq_addresses real, word_freq_free real,word_freq_business real, word_freq_email real, word_freq_you real, word_freq_credit real,word_freq_your real, word_freq_font real, word_freq_000 real, word_freq_money real,word_freq_hp real, word_freq_hpl real, word_freq_george real, word_freq_650 real, word_freq_lab real,word_freq_labs real, word_freq_telnet real, word_freq_857 real, word_freq_data real,word_freq_415 real, word_freq_85 real, word_freq_technology real, word_freq_1999 real,word_freq_parts real, word_freq_pm real, word_freq_direct real, word_freq_cs real, word_freq_meeting real,word_freq_original real, word_freq_project real, word_freq_re real, word_freq_edu real,word_freq_table real, word_freq_conference real, char_freq_semicolon real, char_freq_leftParen real,char_freq_leftBracket real, char_freq_exclamation real, char_freq_dollar real, char_freq_pound real, capital_run_length_average real, capital_run_length_longest real, capital_run_length_total real, spam integer) WITH (CLUSTERED COLUMNSTORE INDEX, DISTRIBUTION = ROUND_ROBIN);
    GO

Bcp ile veri kopyalayın:

    bcp spam in spambaseHeaders.data -q -c -t  ',' -S <server-name>.database.windows.net -d <database-name> -U <username> -P <password> -F 1 -r "\r\n"

> [!NOTE]
> İndirilen dosyadaki satır sonları için Windows stili olsa da, bu nedenle, bcp - r bayrağıyla söyleyin seçmeliyiz bcp UNIX stili bekliyor.
>
>

Ve sqlcmd ile sorgulama:

    select top 10 spam, char_freq_dollar from spam;
    GO

Squirrel SQL ile de sorgulayabilir. İçinde bulunan Microsoft MSSQL Server JDBC sürücüsü kullanarak PostgreSQL için benzer adımları ***/usr/share/java/jdbcdrivers/sqljdbc42.jar***.

## <a name="next-steps"></a>Sonraki adımlar
Azure veri bilimi işlemi oluşturan görevler rehberlik konuları genel bakış için bkz. [Team Data Science Process](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/overview).

Team Data Science Process belirli senaryolar için adımları gösteren diğer uçtan uca izlenecek yollar açıklaması için bkz: [Team Data Science Process Kılavuzu](../team-data-science-process/walkthroughs.md). İzlenecek yollar, ayrıca bir iş akışı veya işlem hattı akıllı bir uygulama oluşturmak için bulut ve şirket içi araçları ve Hizmetleri birleştirme işlemini göstermektedir.
