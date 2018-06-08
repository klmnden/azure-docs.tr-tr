---
title: Sunucu iş yükü verilerini - Azure terabayt üzerinde tahmin | Microsoft Docs
description: Azure Machine Learning çalışma ekranı kullanarak büyük veri üzerinde makine öğrenimi modeli eğitmek nasıl.
services: machine-learning
documentationcenter: ''
author: daden
manager: mithal
editor: daden
ms.assetid: ''
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: desktop-workbench
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/15/2017
ms.author: daden
ms.openlocfilehash: 450c033fbce3544cdc17ddc6d47ff726b01a4d3e
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34832671"
---
# <a name="server-workload-forecasting-on-terabytes-of-data"></a>Birkaç terabayt veri üzerinde sunucu iş yükü tahmini

Bu makalede, veri bilimcilerine kullanması büyük veri çözümleri geliştirmek için Azure Machine Learning çalışma ekranı nasıl kullanabileceğinizi yer almaktadır. Büyük bir veri kümesi örnekten başlatmak, veri hazırlığı, özellik Mühendisliği ve machine learning yinelemek ve büyük veri kümesinin tamamını işlemine genişletir. 

Machine Learning çalışma ekranı aşağıdaki anahtar özellikleri hakkında bilgi edineceksiniz:
* Arasında kolayca geçiş işlem hedefler. Farklı işlem hedefleri ayarlayabilir ve bunları deneme kullanabilirsiniz. Bu örnekte, işlem hedefleri olarak bir Ubuntu DSVM ve Azure Hdınsight kümesini kullanın. İşlem hedefleri kaynaklar kullanılabilirliğini bağlı olarak da yapılandırabilirsiniz. Özellikle, daha fazla alt düğüm Spark kümesiyle ölçeğini sonra deneme çalıştırmalarını hızlandırmak için Machine Learning çalışma ekranı aracılığıyla kaynakları kullanabilir.
* Geçmiş izleme çalıştırın. Machine Learning çalışma ekranı, machine learning modellerini ve diğer ölçümleri ilgi performansını izlemek için kullanabilirsiniz.
* Makine öğrenimi modeline operationalization. Azure kapsayıcı hizmeti bir web hizmeti olarak modeli learning eğitilen makineyi dağıtmak için Machine Learning çalışma ekranının içinden yerleşik araçlarını kullanabilirsiniz. Web hizmeti, REST API çağrıları aracılığıyla Mini toplu Öngörüler almak için de kullanabilirsiniz. 
* Terabayt veri desteği.

> [!NOTE]
> Kod örnekleri ve bu örnek için ilgili diğer malzemeleri için bkz: [GitHub](https://github.com/Azure/MachineLearningSamples-BigData).
> 

## <a name="use-case-overview"></a>Kullanım örneği'ne genel bakış

Sunucuları üzerindeki iş yükünü tahmin kendi altyapısını yönetmek teknolojisi şirketler için ortak bir iş gereksinimi var. Altyapı maliyetini azaltmak için kullanılan sunucuları üzerinde çalışan hizmetleri makineler daha küçük bir sayı çalıştırmak için birlikte gruplandırılmalıdır. Aşırı kullanılmasına sunucuları üzerinde çalışan hizmetleri çalıştırmak için daha fazla makine verilmelidir. 

Bu senaryoda, her makine (veya sunucu) için iş yükü tahmini odaklanır. Özellikle, oturum verilerini her bir sunucuda sunucu iş yükü sınıfının gelecekte tahmin etmek için kullanın. Her sunucu yükü düşük, Orta ve yüksek sınıfları rastgele orman sınıflandırıcıda kullanarak sınıflandırdığınız [Apache Spark ML](https://spark.apache.org/docs/2.1.1/ml-guide.html). Makine öğrenimi teknikleri ve iş akışı bu örnekte, benzer diğer sorunlar için kolayca genişletilebilir. 


## <a name="prerequisites"></a>Önkoşullar

Bu örneği çalıştırmak için gereken önkoşullar aşağıdaki gibidir:

* Bir [Azure hesabı](https://azure.microsoft.com/free/) (ücretsiz deneme kullanılabilir).
* Yüklü bir kopyasını [Azure Machine Learning çalışma ekranı](../service/overview-what-is-azure-ml.md). Programı yüklemek ve bir çalışma alanı oluşturmak için bkz: [hızlı başlangıç Yükleme Kılavuzu'na](../service/quickstart-installation.md). Birden çok aboneliğiniz varsa, [geçerli etkin aboneliğinizin olmasını istediğiniz aboneliği ayarlamak](https://docs.microsoft.com/cli/azure/account?view=azure-cli-latest#az_account_set).
* Windows 10 (Bu örnekte yönergeleri genellikle macOS sistemleri için aynıdır).
* Bir veri bilimi sanal makine (DSVM) Linux (Ubuntu), tercihen Doğu ABD bölgede burada verileri bulur. İzleyerek bir Ubuntu DSVM sağlayabilirsiniz [bu yönergeleri](https://docs.microsoft.com/azure/machine-learning/data-science-virtual-machine/dsvm-ubuntu-intro). Ayrıca bkz [Bu Hızlı Başlangıç](https://ms.portal.azure.com/#create/microsoft-ads.linux-data-science-vm-ubuntulinuxdsvmubuntu). En az 8 çekirdek ve 32 GB bellek bir sanal makine kullanmanızı öneririz. 

İzleyin [yönerge](../service/known-issues-and-troubleshooting-guide.md#remove-vm-execution-error-no-tty-present) AML çalışma ekranı için VM parola daha az sudoer erişimini etkinleştirmek için.  Kullanmayı tercih edebileceğiniz [oluşturmak ve VM AML çalışma ekranı içinde kullanmak için SSH anahtar tabanlı kimlik doğrulaması](experimentation-service-configuration.md#using-ssh-key-based-authentication-for-creating-and-using-compute-targets). Bu örnekte, VM erişmek için parola kullanın.  Aşağıdaki tabloda, sonraki adımlara DSVM bilgileri ile kaydedin:

 Alan adı| Değer |  
 |------------|------|
DSVM IP adresi | xxx|
 Kullanıcı adı  | xxx|
 Parola   | xxx|


 Tüm VM ile kullanmayı tercih edebileceğiniz [Docker altyapısına](https://docs.docker.com/engine/) yüklü.

* Hdınsight Spark kümesi, Hortonworks veri platformu sürümü 3.6 ve Spark sürüm ile 2.1.x tercihen Doğu ABD bölgede burada verileri bulur. Ziyaret [Azure Hdınsight'ta Apache Spark kümesi oluşturma](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-provision-linux-clusters) Hdınsight kümeleri oluşturma hakkında ayrıntılar için. 16 çekirdek ve bellek 112 GB olan her çalışan ile üç alt küme kullanmanızı öneririz. Veya yalnızca VM türünü seçebilirsiniz `D12 V2` baş düğüm için ve `D14 V2` değerini çalışan düğümünüz için. Küme dağıtımı yaklaşık 20 dakika sürer. Küme adı, SSH kullanıcı adı ve bu örnek denemek için parola gerekir. Aşağıdaki tabloda, sonraki adımlar için Azure Hdınsight kümesi bilgileri ile kaydedin:

 Alan adı| Değer |  
 |------------|------|
 Küme adı| xxx|
 Kullanıcı adı  | xxx (varsayılan olarak sshuser)|
 Parola   | xxx|


* Azure Depolama hesabı. İzleyebileceğiniz [bu yönergeleri](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account) oluşturmak için. Ayrıca, iki özel blob kapsayıcıları adlarıyla oluşturma `fullmodel` ve `onemonthmodel` bu depolama hesabında. Depolama hesabı Ara işlem sonuçları ve makine öğrenimi modellerinin oluşturulmasına kaydetmek için kullanılır. Bu örnek denemek için depolama hesabı adı ve erişim anahtarı gerekir. Aşağıdaki tabloda, sonraki adımlar için Azure depolama hesabı bilgileri ile kaydedin:

 Alan adı| Değer |  
 |------------|------|
 Depolama hesabı adı| xxx|
 Erişim anahtarı  | xxx|


Ubuntu DSVM ve önkoşul listesinde oluşturulan Azure Hdınsight kümesini işlem hedefleri şunlardır. Hangi çalışma ekranı çalıştığı bilgisayardan farklı olabilir Machine Learning çalışma ekranı, bağlamında işlem kaynak hedefleri olan işlem.   

## <a name="create-a-new-workbench-project"></a>Yeni bir çalışma ekranı projesi oluşturma

Bu örnek bir şablon kullanarak yeni bir proje oluşturun:
1.  Machine Learning Workbench’i açın.
2.  Üzerinde **projeleri** sayfasında, **+** oturum ve seçin **yeni proje**.
3.  İçinde **yeni proje oluştur** bölmesinde, yeni projeniz için bilgileri doldurun.
4.  İçinde **arama proje şablonları** arama kutusuna **iş yükü tahmin terabayt verileri**ve şablonu seçin.
5.  **Oluştur**’u seçin.

Aşağıdaki önceden oluşturulmuş git deposunu bir çalışma ekranı proje oluşturabilirsiniz [bu yönergeyi](./tutorial-classifying-iris-part-1.md).  
Çalıştırma `git status` dosyaların sürüm izleme durumunu incelemek için.

## <a name="data-description"></a>Veri açıklaması

Bu örnekte kullanılan veri birleştirilen sunucu iş yükü verilerdir. Doğu ABD bölgesinde genel olarak erişilebilir olan bir Azure Blob Depolama hesabı içinde barındırılır. Belirli bir depolama hesabı bilgilerini bulunabilir `dataFile` alanını [ `Config/storageconfig.json` ](https://github.com/Azure/MachineLearningSamples-BigData/blob/master/Config/fulldata_storageconfig.json) biçiminde "wasb: / /<BlobStorageContainerName>@<StorageAccountName>.blob.core.windows.net/<path>". Verileri doğrudan Blob Depolama kullanabilirsiniz. Depolama aynı anda birden çok kullanıcı tarafından kullanılıyorsa, kullanabileceğiniz [azcopy](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-linux) daha iyi deneme deneyimi için kendi depolama alanına veri yüklemek için. 

Toplam veri boyutu yaklaşık 1 TB'tır. Her dosya yaklaşık 1-3 GB ve üst bilgi içermeyen CSV dosya biçiminde. Her veri satırının belirli bir sunucu üzerinde bir işlem yükünü temsil eder. Veri şeması ilgili ayrıntılı bilgileri aşağıdaki gibidir:

Sütun numarası | Alan adı| Tür | Açıklama |  
|------------|------|-------------|---------------|
1  | `SessionStart` | Tarih saat |    Oturum başlangıç saati
2  |`SessionEnd`    | Tarih saat | Oturum bitiş saati
3 |`ConcurrentConnectionCounts` | Tamsayı | Eşzamanlı bağlantı sayısı
4 | `MbytesTransferred` | Çift | Megabayt cinsinden aktarılan normalleştirilmiş verileri
5 | `ServiceGrade` | Tamsayı |  Oturumu için hizmet notu
6 | `HTTP1` | Tamsayı|  HTTP1 veya HTTP2 oturumu kullanır
7 |`ServerType` | Tamsayı   |Sunucu türü
8 |`SubService_1_Load` | Çift |   Subservice 1 yük
9 | `SubService_2_Load` | Çift |  Subservice 2 yükleme
10 | `SubService_3_Load` | Çift |     Subservice 3 yükleme
11 |`SubService_4_Load` | Çift |  Subservice 4 yükleme
12 | `SubService_5_Load`| Çift |      Subservice 5 yükleme
13 |`SecureBytes_Load`  | Çift | Güvenli bayt yükleme
14 |`TotalLoad` | Çift | Sunucusundaki toplam yükü
15 |`ClientIP` | Dize|    İstemci IP adresi
16 |`ServerIP` | Dize|    Sunucu IP adresi



Beklenen veri türleri yukarıdaki tabloda listelenen unutmayın. Eksik değerleri ve kirli veri sorunları nedeniyle olan veri türleri gerçekte beklendiği gibi garantisi yoktur. Veri işleme bu dikkate almanız gerekir. 


## <a name="scenario-structure"></a>Senaryo yapısı

Bu örnekte dosyaları şu şekilde düzenlenmiştir.

| Dosya adı | Tür | Açıklama |
|-----------|------|-------------|
| `Code` | Klasör | Klasör örnekteki tüm kod içerir. |
| `Config` | Klasör | Klasör yapılandırma dosyalarını içerir. |
| `Image` | Klasör | Görüntüler için Benioku dosyasını kaydetmek için kullanılan klasör. |
| `Model` | Klasör | Model dosyaları kaydetmek için kullanılan klasör Blob depolama alanından indirilir. |
| `Code/etl.py` | Python dosyası | Veri hazırlama ve özellik Mühendisliği için kullanılan Python dosyası. |
| `Code/train.py` | Python dosyası | Üç sınıfı multi-sınıflandırma modeli eğitmek için kullanılan Python dosyası.  |
| `Code/webservice.py` | Python dosyası | Operationalization için kullanılan Python dosyası.  |
| `Code/scoring_webservice.py` | Python dosyası |  Veri dönüştürme için kullanılan ve web hizmeti çağırma Python dosyası. |
| `Code/O16Npreprocessing.py` | Python dosyası | Scoring_webservice.py verileri ön işlemek için kullanılan Python dosyası.  |
| `Code/util.py` | Python dosyası | Okuma ve Azure BLOB'ları yazmak için kod içeren Python dosyası.  
| `Config/storageconfig.json` | JSON dosyası | İşleme ve eğitim modeli ve Ara sonuçların bir aylık verileri depolayan Azure blob kapsayıcısı için yapılandırma dosyası. |
| `Config/fulldata_storageconfig.json` | JSON dosyası | Ara sonuçların ve işleme ve eğitim modeli tam veri kümesinde depolayan Azure blob kapsayıcısı için yapılandırma dosyası.|
| `Config/webservice.json` | JSON dosyası | Scoring_webservice.py için yapılandırma dosyası.|
| `Config/conda_dependencies.yml` | YAML dosyası | Conda bağımlılık dosyası. |
| `Config/conda_dependencies_webservice.yml` | YAML dosyası | Web hizmeti Conda bağımlılık dosyası.|
| `Config/dsvm_spark_dependencies.yml` | YAML dosyası | Ubuntu DSVM için Spark bağımlılık dosyası. |
| `Config/hdi_spark_dependencies.yml` | YAML dosyası | Hdınsight Spark kümesi için Spark bağımlılık dosyası. |
| `README.md` | Markdown dosyası | Benioku markdown dosyası. |
| `Code/download_model.py` | Python dosyası | Azure'dan modeli dosyaları indirmek için kullanılan Python dosyası yerel diske blob. |
| `Docs/DownloadModelsFromBlob.md` | Markdown dosyası | Çalıştırma hakkında yönergeler içeren markdown dosyası `Code/download_model.py`. |



### <a name="data-flow"></a>Veri akışı

Kodda [ `Code/etl.py` ](https://github.com/Azure/MachineLearningSamples-BigData/blob/master/Code/etl.py) verileri genel olarak erişilebilir kapsayıcıdan yükler (`dataFile` alanını [ `Config/storageconfig.json` ](https://github.com/Azure/MachineLearningSamples-BigData/blob/master/Config/fulldata_storageconfig.json)). Veri hazırlama ve özellik Mühendisliği içerir ve kendi özel kapsayıcıya modelleri ve Ara İşlem sonuçlarını kaydeder. Kodda [ `Code/train.py` ](https://github.com/Azure/MachineLearningSamples-BigData/blob/master/Code/train.py) özel kapsayıcıdan Ara işlem sonuçları yükler, çok sınıfı sınıflandırma modeli eğitir ve eğitilen machine learning modelini özel kapsayıcıya yazar. 

Deneme bir aylık veri kümesi ve başka bir deneme tam veri kümesi üzerinde bir kapsayıcı kullanmanız gerekir. Veri ve modelleri Parquet dosyası olarak kaydedilir çünkü her dosya birden çok BLOB'ları içeren kapsayıcı, aslında bir klasöründe bulunur. Sonuçta elde edilen kapsayıcı şu şekilde görünür:

| BLOB önek adı | Tür | Açıklama |
|-----------|------|-------------|
| featureScaleModel | Parquet | Sayısal özellikleri için standart scaler modeli. |
| stringIndexModel | Parquet | Sayısal olmayan özellikler için dizin oluşturucu modeli dize.|
| oneHotEncoderModel|Parquet | Kategorik özellikleri için bir hot Kodlayıcı modeli. |
| mlModel | Parquet | Eğitilmiş makine öğrenimi modeline. |
| bilgi| Python pickle dosyası | Eğitim başlangıç, eğitimi bitiş olayı, süre, için zaman damgası dahil olmak üzere dönüştürülmüş verileri hakkında bilgiler train-test bölme ve sütunlar dizin oluşturma ve bir hot kodlama için.

Tüm dosyaları ve önceki tabloda BLOB'lar operationalization için kullanılır.


### <a name="model-development"></a>Model geliştirme

#### <a name="architecture-diagram"></a>Mimari diyagramı


Aşağıdaki diyagramda modeli geliştirmek için Machine Learning çalışma ekranı kullanma uçtan uca iş akışı gösterilmektedir: ![mimarisi](media/scenario-big-data/architecture.PNG)

Aşağıdaki bölümlerde, modeli geliştirme Machine Learning çalışma ekranı içinde uzak işlem hedef işlevselliğini kullanarak gösteriyoruz. Biz ilk örnek verileri az miktarda yük ve komut dosyasını çalıştırmak [ `Code/etl.py` ](https://github.com/Azure/MachineLearningSamples-BigData/blob/master/Code/etl.py) bir Ubuntu DSVM hızlı yineleme için üzerinde. Bunu yapmadan iş biz sınırlandırabilirsiniz [ `Code/etl.py` ](https://github.com/Azure/MachineLearningSamples-BigData/blob/master/Code/etl.py) göre daha hızlı yineleme için ek bağımsız değişken geçirme. Sonunda, Hdınsight kümesi ile tam veri eğitmek için kullanırız.     

[ `Code/etl.py` ](https://github.com/Azure/MachineLearningSamples-BigData/blob/master/Code/etl.py) Dosya yükler ve veri hazırlar ve özellik Mühendisliği gerçekleştirir. Bu, iki bağımsız değişken kabul eder:
* Ara işlem sonuçları ve modelleri depolamak için Blob Depolama kapsayıcısı için bir yapılandırma dosyası.
* Hata ayıklama yapılandırması için bir bağımsız değişken daha hızlı yineleme.

İlk bağımsız değişken `configFilename`, bir yerel yapılandırma dosyası burada, Blob Depolama bilgilerini depolamak ve verileri yüklemek istediğiniz yeri belirtin. Varsayılan olarak, olmasından [ `Config/storageconfig.json` ](https://github.com/Azure/MachineLearningSamples-BigData/blob/master/Config/storageconfig.json), ve Çalıştır bir aylık verileri kullanılacak geçiyor. Biz de dahil [ `Config/fulldata_storageconfig.json` ](https://github.com/Azure/MachineLearningSamples-BigData/blob/master/Config/fulldatastorageconfig.json), hangi Çalıştır tam veri kümesi üzerinde kullanmanız gerekir. Yapılandırma içeriği aşağıdaki gibidir: 

| Alan | Tür | Açıklama |
|-----------|------|-------------|
| StorageAccount | Dize | Azure depolama hesabı adı |
| storageContainer | Dize | Ara Sonuçların depolanacağı Azure depolama hesabı kapsayıcısında |
| Depolama anahtarı | Dize |Azure depolama hesabı erişim anahtarı |
| Veri dosyası|Dize | Veri kaynağı dosyaları  |
| süre| Dize | Veri kaynağı dosyaları verilerde süresi|

Her ikisi de değiştirme `Config/storageconfig.json` ve `Config/fulldata_storageconfig.json` depolama hesabı, depolama anahtarı ve Ara sonuçların depolamak için blob kapsayıcısı yapılandırmak için. Varsayılan olarak, blob kapsayıcısı çalıştırmak bir aylık verileri için olan `onemonthmodel`, ve tam veri kümesini çalıştırmak için blob kapsayıcı `fullmodel`. Depolama hesabınızı bu iki kapsayıcı oluşturduğunuzdan emin olun. `dataFile` Alanındaki [ `Config/fulldata_storageconfig.json` ](https://github.com/Azure/MachineLearningSamples-BigData/blob/master/Config/fulldatastorageconfig.json) hangi verilerin yüklendiği yapılandırır [ `Code/etl.py` ](https://github.com/Azure/MachineLearningSamples-BigData/blob/master/Code/etl.py). `duration` Alan verileri içeren aralığı yapılandırır. Süre için ONE_MONTH ayarlarsanız, yüklenen veriler için Haziran 2016 verilerin yedi dosyalar arasında yalnızca bir .csv dosyası olmalıdır. Süresi tam ise tam veri kümesi (1 TB) yüklenir. Değiştirmeniz gerekmez `dataFile` ve `duration` bu iki yapılandırma dosyalarında.

İkinci bağımsız değişkeni Hata Ayıkla'dır. FILTER_IP için ayarı daha hızlı yineleme sağlar. Bu parametrenin kullanımı, komut dosyası hata ayıklama istediğinizde yararlıdır.

> [!NOTE]
> Tüm aşağıdaki komutlar, herhangi bir bağımsız değişken gerçek değeriyle değiştirin.
> 


#### <a name="model-development-on-the-docker-of-ubuntu-dsvm"></a>Ubuntu Docker DSVM modeli geliştirme

#####  <a name="1-set-up-the-compute-target"></a>1. İşlem hedefi ayarlama

Komut satırı seçerek Machine Learning çalışma ekranından başlayın **dosya** > **komut istemini açın**. Ardından çalıştırın: 

```az ml computetarget attach remotedocker --name dockerdsvm --address $DSVMIPaddress  --username $user --password $password ```

Aşağıdaki iki dosyayı projenize aml_config klasöründe oluşturulur:

-  dockerdsvm.COMPUTE: Bu dosya bir uzaktan yürütme hedef için bağlantı ve yapılandırma bilgileri içerir.
-  dockerdsvm.runconfig: Bu dosya çalışma ekranı uygulama içinde kullanılan çalıştırma seçenekleri kümesidir.

Dockerdsvm.runconfig için göz atın ve bu alanlar yapılandırmasını aşağıdaki gibi değiştirin:

    PrepareEnvironment: true 
    CondaDependenciesFile: Config/conda_dependencies.yml 
    SparkDependenciesFile: Config/dsvm_spark_dependencies.yml

Çalıştırarak proje ortamını hazırlayın:

```az ml experiment prepare -c dockerdsvm```


İle `PrepareEnvironment` true olarak Machine Learning çalışma ekranı bir işi göndermek her çalışma zamanı ortamı oluşturur. `Config/conda_dependencies.yml` ve `Config/dsvm_spark_dependencies.yml` çalışma zamanı ortamı özelleştirme içerir. Bu iki YMAL dosyaları düzenleyerek Conda bağımlılıkları, Spark yapılandırma ve Spark bağımlılıkları her zaman değiştirebilirsiniz. Bu örnekte, eklediğimiz `azure-storage` ve `azure-ml-api-sdk` ek Python paketlerini olarak `Config/conda_dependencies.yml`. Ayrıca eklediğimiz `spark.default.parallelism`, `spark.executor.instances`, ve `spark.executor.cores` içinde `Config/dsvm_spark_dependencies.yml`. 

#####  <a name="2-data-preparation-and-feature-engineering-on-dsvm-docker"></a>2. Veri hazırlama ve özellik Mühendisliği DSVM Docker üzerinde

Komut dosyasını çalıştırmak `etl.py` DSVM Docker üzerinde. Belirli sunucu IP adresleri ile yüklenen verilere filtre hata ayıklama parametresini kullanın:

```az ml experiment submit -t dockerdsvm -c dockerdsvm ./Code/etl.py ./Config/storageconfig.json FILTER_IP```

Yan Masası'na giderek seçin **çalıştırmak** çalıştırma geçmişini görmek için `etl.py`. Çalışma zamanında yaklaşık iki dakika olduğuna dikkat edin. Yeni özellikler için kodunu değiştirmeniz planlıyorsanız, ikinci bağımsız değişken olarak FILTER_IP sağlayarak daha hızlı yineleme sağlar. Bu adım yeni özellikler oluşturmak veya veri kümesi keşfedin sorunları, öğrenme kendi makineyle ilgilenirken birden çok kez çalıştırmanız gerekebilir. 

Hangi verilerin yükleneceğini ve hangi verileri işlemek için daha fazla filtreleme üzerinde özelleştirilmiş kısıtlamasıyla modeli geliştirmede yineleme işlemini hızlandırabilir. Denerken, kodunuzda Git deposuna düzenli aralıklarla değişiklikleri kaydetmeniz gerekir. Aşağıdaki kodda kullandık Not `etl.py` özel kapsayıcı erişimi etkinleştirmek için:

```python
def attach_storage_container(spark, account, key):
    config = spark._sc._jsc.hadoopConfiguration()
    setting = "fs.azure.account.key." + account + ".blob.core.windows.net"
    if not config.get(setting):
        config.set(setting, key)

# attach the blob storage to the spark cluster or VM so that the storage can be accessed by the cluster or VM        
attach_storage_container(spark, storageAccount, storageKey)
```


Ardından, komut dosyasını çalıştırmak `etl.py` DSVM Docker FILTER_IP hata ayıklama parametresi olmadan üzerinde:

```az ml experiment submit -t dockerdsvm -c dockerdsvm ./Code/etl.py ./Config/storageconfig.json FALSE```

Yan Masası'na giderek seçin **çalıştırmak** çalıştırma geçmişini görmek için `etl.py`. Çalışma zamanında yaklaşık dört dakika olduğuna dikkat edin. Bu adım işlenen sonucu kapsayıcıya kaydedilir ve train.py eğitim için yüklenir. Ayrıca, dize dizin oluşturucular, kodlayıcı ardışık düzen ve standart scalers özel kapsayıcıya kaydedilir. Bunlar operationalization kullanılır. 


##### <a name="3-model-training-on-dsvm-docker"></a>3. DSVM Docker üzerinde eğitim modeli

Komut dosyasını çalıştırmak `train.py` DSVM Docker üzerinde:

```az ml experiment submit -t dockerdsvm -c dockerdsvm ./Code/train.py ./Config/storageconfig.json```

Bu adım çalıştırma Ara işlem sonuçları yükler `etl.py`ve makine öğrenimi modeline eğitir. Bu adım yaklaşık iki dakika sürer.

Deneme küçük verileri başarıyla tamamladıktan sonra tam veri kümesi üzerinde deneme çalıştırmaya devam edebilirsiniz. Aynı kodu kullanarak başlatın ve ardından bağımsız değişkeniyle denemeler ve hedef değişiklikleri işlem.  

####  <a name="model-development-on-the-hdinsight-cluster"></a>Hdınsight kümesinde modeli geliştirme

##### <a name="1-create-the-compute-target-in-machine-learning-workbench-for-the-hdinsight-cluster"></a>1. İşlem hedef Hdınsight kümesi için Machine Learning çalışma ekranı oluşturma

```az ml computetarget attach cluster --name myhdi --address $clustername-ssh.azurehdinsight.net --username $username --password $password```

Aşağıdaki iki dosyalar aml_config klasöründe oluşturulur:
    
-  myhdi.COMPUTE: Bu dosya bir uzaktan yürütme hedef bağlantı ve yapılandırma bilgilerini içerir.
-  myhdi.runconfig: Bu dosya çalışma ekranı uygulama içinde kullanılan çalıştırma seçenekleri ayarlayın.


Myhdi.runconfig için göz atın ve bu alanlar yapılandırmasını aşağıdaki gibi değiştirin:

    PrepareEnvironment: true 
    CondaDependenciesFile: Config/conda_dependencies.yml 
    SparkDependenciesFile: Config/hdi_spark_dependencies.yml

Çalıştırarak proje ortamını hazırlayın:

```az ml experiment prepare -c myhdi```

Bu adım yedi dakikaya kadar sürebilir.

##### <a name="2-data-preparation-and-feature-engineering-on-hdinsight-cluster"></a>2. Veri hazırlama ve özellik Mühendisliği Hdınsight kümesinde

Komut dosyasını çalıştırmak `etl.py`, Hdınsight kümesinde tam verilerle:

```az ml experiment submit -a -t myhdi -c myhdi ./Code/etl.py Config/fulldata_storageconfig.json FALSE```

Bu işi oldukça uzun bir süre (yaklaşık iki saat) süresi için kullanabileceğiniz `-a` çıkış akış devre dışı bırakmak için. Ne zaman iş yapılır, buna **çalıştırma geçmişi**, sürücü ve denetleyici günlükleri görüntüleyebilirsiniz. Daha büyük bir küme varsa, her zaman yapılandırmalarında yapılandırabilirsiniz `Config/hdi_spark_dependencies.yml` daha fazla örneği veya çekirdek kullanılacak. Örneğin, dört alt düğüm kümeniz varsa, değeri artırabilirsiniz `spark.executor.instances` 7 5. Bu adımda çıktısını görebileceğim **fullmodel** depolama hesabınızdaki kapsayıcı. 


##### <a name="3-model-training-on-hdinsight-cluster"></a>3. Hdınsight kümesi üzerinde eğitim modeli

Komut dosyasını çalıştırmak `train.py` Hdınsight kümesinde:

```az ml experiment submit -a -t myhdi -c myhdi ./Code/train.py Config/fulldata_storageconfig.json```

Bu işi oldukça uzun bir süredir (yaklaşık 30 dakika) süresi için kullanabileceğiniz `-a` çıkış akış devre dışı bırakmak için.

#### <a name="run-history-exploration"></a>Geçmiş araştırması çalıştırın

Çalıştırma geçmişi, Machine Learning çalışma ekranındaki, deneme izlemeyi sağlayan bir özelliktir. Varsayılan olarak, deneme süresi izler. İçin tam veri kümesi için taşıdığınızda belirli örneğimizde `Code/etl.py` deneme, biz süresini önemli ölçüde artırır dikkat edin. Ayrıca, izleme amacıyla belirli ölçümleri oturum açabilir. Ölçüm izlemeyi etkinleştirmek için Python dosyanızı head için kod aşağıdaki satırları ekleyin:
```python
# import logger
from azureml.logging import get_azureml_logger

# initialize logger
run_logger = get_azureml_logger()
```
Belirli bir ölçüyü izlemek için örnek aşağıda verilmiştir:

```python
run_logger.log("Test Accuracy", testAccuracy)
```

Çalışma ekranı sağ kenar çubuğunda Gözat **çalışır** her Python dosyası için çalıştırma geçmişini görmek için. GitHub deponuza de gidebilirsiniz. Yeni bir dal, her çalıştırmayı betiğinizde yaptığınız değişiklikleri izlemek için "AMLHistory," ile başlayan ada sahip oluşturulur. 


### <a name="operationalize-the-model"></a>Model faaliyete

Bu bölümde, bir web hizmeti olarak önceki adımlarda oluşturduğunuz modeli faaliyete. Ayrıca iş yükü tahmin etmek için web hizmeti kullanmayı öğrenin. Makine dili operationalization komut satırı arabirimlerinden (CLIs) kapsayıcılı web hizmeti olarak kodu ve bağımlılıklarını Docker görüntüleri olarak paketini ve modeli yayımlamak için kullanın.

CLIs çalıştırmak için Machine Learning çalışma ekranı komut satırı isteminde kullanın.  İzleyerek CLIs Ubuntu Linux üzerinde de çalıştırabilirsiniz [Yükleme Kılavuzu](./deployment-setup-configuration.md#using-the-cli). 

> [!NOTE]
> Tüm aşağıdaki komutlar, herhangi bir bağımsız değişken gerçek değeriyle değiştirin. Bu bölümde tamamlanması yaklaşık 40 dakika sürer.
> 

Benzersiz bir dize olarak operationalization ortamını seçin. Burada, "[benzersiz]" dize seçtiğiniz dizesini temsil etmesi için kullanırız.

1. Operationalization ortamını oluşturun ve kaynak grubu oluşturun.

        az ml env setup -c -n [unique] --location eastus2 --cluster -z 5 --yes

   Kapsayıcı hizmeti ortamı olarak kullanarak kullanabileceğinizi unutmayın `--cluster` içinde `az ml env setup` komutu. Machine learning modelini faaliyete geçirebilirsiniz [Azure kapsayıcı hizmeti](https://docs.microsoft.com/azure/container-service/kubernetes/container-service-intro-kubernetes). Kullandığı [Kubernetes](https://kubernetes.io/) dağıtımını, ölçeklendirme ve kapsayıcılı uygulamaların yönetimini otomatikleştirmek için. Bu komutu çalıştırmak için yaklaşık 20 dakika sürer. Dağıtım başarılı bir şekilde sona erdi denetlemek için aşağıdakileri kullanın: 

        az ml env show -g [unique]rg -n [unique]

   Aşağıdaki komutu çalıştırarak, yeni oluşturduğunuz hesapla dağıtım ortamı ayarlayın:

        az ml env set -g [unique]rg -n [unique]

2. Oluşturun ve bir model yönetim hesabı kullanın. Bir model yönetim hesabı oluşturmak için şu komutu çalıştırın:

        az ml account modelmanagement create --location  eastus2 -n [unique]acc -g [unique]rg --sku-instances 4 --sku-name S3 

   Model yönetimi, aşağıdaki komutu çalıştırarak operationalization için kullanın:

        az ml account modelmanagement set  -n [unique]acc -g [unique]rg  

   Bir model yönetim hesabı modelleri ve web hizmetleri yönetmek için kullanılır. Azure portalından, yeni bir model yönetim hesabı oluşturuldu görebilirsiniz. Modeller, bildirimler, Docker görüntüler ve bu model yönetim hesabı kullanılarak oluşturulan Hizmetleri görebilirsiniz.

3. Karşıdan yükle ve modelleri kaydedin.

   Modellerinde karşıdan **fullmodel** yerel makinenize kodu dizininde kapsayıcı. Adı "vmlSource.parquet." parquet veri dosyasıyla yüklemeyin Bir model dosyası değil; Bu, bir ara işlem sonucu olacaktır. Git deposunda bulunan modeli dosyalar da yeniden kullanabilirsiniz. Daha fazla bilgi için bkz: [GitHub](https://github.com/Azure/MachineLearningSamples-BigData/blob/master/Docs/DownloadModelsFromBlob.md). 

   Git `Model` CLI ve kayıt klasöründe modelleri olarak izler:

        az ml model register -m  mlModel -n vmlModel -t fullmodel
        az ml model register -m  featureScaleModel -n featureScaleModel -t fullmodel
        az ml model register -m  oneHotEncoderModel -n  oneHotEncoderModel -t fullmodel
        az ml model register -m  stringIndexModel -n stringIndexModel -t fullmodel
        az ml model register -m  info -n info -t fullmodel

   Her komutun çıktısı, sonraki adımda gerekli olan bir model kimliği verir. Gelecekte kullanım için bir metin dosyasına kaydedin.

4. Web hizmeti için bir bildirim oluşturun.

   Bir bildirim web hizmeti, machine learning modellerini ve çalışma zamanı ortamı bağımlılıkları kodunu içerir. Git `Code` klasöründe CLI ve aşağıdaki komutu çalıştırın:

        az ml manifest create -n $webserviceName -f webservice.py -r spark-py -c ../Config/conda_dependencies_webservice.yml -i $modelID1 -i $modelID2 -i $modelID3 -i $modelID4 -i $modelID5

   Çıkış bir sonraki adımda bildirim kimliği verir. 

   De Kal `Code` dizin ve test edebilirsiniz webservice.py aşağıdakini çalıştırarak: 

        az ml experiment submit -t dockerdsvm -c dockerdsvm webservice.py

5. Bir Docker görüntüsü oluşturun. 

        az ml image create -n [unique]image --manifest-id $manifestID

   Çıkış bir sonraki adımda bir görüntü kimliği verir, bu docker görüntü kapsayıcı Hizmeti'nde kullanılır. 

6. Web hizmeti için kapsayıcı hizmeti kümesini dağıtma.

        az ml service create realtime -n [unique] --image-id $imageID --cpu 0.5 --memory 2G

   Bir hizmet kimliği çıktısı verir Yetkilendirme anahtarının almak ve hizmet URL'si için kullanmanız gerekir.

7. Web hizmeti mini-toplu Puanlama amacıyla Python kodu çağırın.

   Yetkilendirme anahtarını almak için aşağıdaki komutu kullanın:

         az ml service keys realtime -i $ServiceID 

   Puanlama URL'sini hizmeti almak için aşağıdaki komutu kullanın:

        az ml service usage realtime -i $ServiceID

   İçeriği değiştirmek `./Config/webservice.json` URL'sini ve yetkilendirme anahtarını Puanlama sağ hizmetiyle. Özgün dosyada "Bearer" tutmak ve "xxx" bölümü değiştirin. 
   
   Projenizi kök dizinine gidin ve aşağıdakileri kullanarak Puanlama Mini toplu işlemi için web hizmeti test:

        az ml experiment submit -t dockerdsvm -c dockerdsvm ./Code/scoring_webservice.py ./Config/webservice.json

8. Web hizmeti ölçeklendirin. 

   Daha fazla bilgi için bkz: [operationalization Azure kapsayıcı hizmeti kümenizde ölçeklendirme](how-to-scale-clusters.md).
 

## <a name="next-steps"></a>Sonraki adımlar

Bu örnek bir machine learning büyük veri modelini eğitmek için Machine Learning çalışma ekranı kullanmayı vurgular ve eğitilen model faaliyete. Özellikle, yapılandırmak ve farklı işlem hedefleri kullanın ve ölçümleri izleme geçmişi çalıştırın ve farklı çalıştırmalarını kullanma hakkında bilgi edindiniz.

Çapraz doğrulama ve parametre hyper ayarlama keşfetmek için kod genişletebilirsiniz. Çapraz doğrulama ve parametre hyper ayarlama hakkında daha fazla bilgi için bkz: [bu GitHub kaynak](https://github.com/Azure/MachineLearningSamples-DistributedHyperParameterTuning).  

Zaman serisi tahmin hakkında daha fazla bilgi için bkz: [bu GitHub kaynak](https://github.com/Azure/MachineLearningSamples-EnergyDemandTimeSeriesForecasting).
