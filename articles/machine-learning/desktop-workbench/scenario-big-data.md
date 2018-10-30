---
title: Sunucu iş yükü terabaytlarca veriyi - Azure tahmini | Microsoft Docs
description: Azure Machine Learning Workbench'i kullanarak büyük veriler üzerinde makine öğrenme modeli eğitmek nasıl.
services: machine-learning
documentationcenter: ''
author: daden
manager: mithal
editor: daden
ms.assetid: ''
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/15/2017
ms.author: daden
ROBOTS: NOINDEX
ms.openlocfilehash: 0d3c6b78944d9365d1e7e88ed33aba852b71a9c1
ms.sourcegitcommit: fbdfcac863385daa0c4377b92995ab547c51dd4f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2018
ms.locfileid: "50232027"
---
# <a name="server-workload-forecasting-on-terabytes-of-data"></a>Birkaç terabayt veri üzerinde sunucu iş yükü tahmini

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)] 

Bu makale, veri uzmanları, büyük veri kullanımı gerektiren çözümler geliştirmek için Azure Machine Learning Workbench nasıl kullanabileceğinizi kapsar. Büyük bir veri kümesi bir örnekten başlatma, veri hazırlama, özellik Mühendisliği ve makine öğrenimi yineleme yapmak ve sonra tüm büyük veri kümesini işleme genişletin. 

Machine Learning Workbench aşağıdaki temel özellikleri hakkında bilgi edineceksiniz:
* Hedef işlem arasında kolay geçiş yapma. Farklı işlem hedefleri ayarlayabilir ve bunları deneme kullanın. Bu örnekte işlem hedefler olarak bir Ubuntu DSVM ve Azure HDInsight kümesini kullanın. İşlem hedefleri kaynaklarının bağlı olarak da yapılandırabilirsiniz. Özellikle, daha fazla alt düğüm Spark kümesiyle genişletme sonra deneme çalıştırmalarınızın ' hızlandırmak için Machine Learning Workbench kaynaklarında kullanabilir.
* Geçmiş izleme çalıştırın. Machine Learning Workbench, makine öğrenimi modelleri ve ilgilendiğiniz diğer ölçüm performansını izlemek için kullanabilirsiniz.
* Makine learning modeli kullanıma hazır hale getirme Machine Learning Workbench içinde yerleşik araçlar, eğitilen makine öğrenimi modelinde Azure Container Service'teki bir web hizmeti olarak dağıtmak için kullanabilirsiniz. Web hizmeti REST API çağrıları üzerinden Mini batch Öngörüler almak için de kullanabilirsiniz. 
* Terabaytlarca veri desteği.

> [!NOTE]
> Kod örnekleri ve bu örneğe ilgili diğer materyalleri için bkz. [GitHub](https://github.com/Azure/MachineLearningSamples-BigData).
> 

## <a name="use-case-overview"></a>Kullanım örneği genel bakış

Sunucuları üzerindeki iş yükünü tahmin kendi altyapısını yönetme teknoloji şirketler için ortak bir iş gereksinimi var. Altyapı maliyetini azaltmak için az kullanılan sunucuları üzerinde çalışan hizmetleri daha az sayıda makineler üzerinde çalıştırılacak birlikte gruplandırılmalıdır. Aşırı kullanılmasına sunucuları üzerinde çalışan hizmetleri çalıştırmak için daha fazla makine verilmelidir. 

Bu senaryoda, her makine (veya sunucu) için iş yükü tahmin odaklanır. Özellikle, oturum verilerini her sunucuda sunucu iş yükü sınıfı gelecekte tahmin etmek için kullanın. Her sunucunun yükünü düşük, Orta ve yüksek sınıfları rastgele orman sınıflandırıcıda kullanarak sınıflandırdığınız [Apache Spark ML](https://spark.apache.org/docs/2.1.1/ml-guide.html). Makine öğrenimi teknikleri ve bu örnekte iş akışı, diğer benzer sorunlar için kolayca genişletilebilir. 


## <a name="prerequisites"></a>Önkoşullar

Bu örneği çalıştırmak için gereken önkoşullar aşağıdaki gibidir:

* Bir [Azure hesabı](https://azure.microsoft.com/free/) (ücretsiz denemeler kullanılabilir).
* Yüklü bir kopyasını [Azure Machine Learning Workbench](../service/overview-what-is-azure-ml.md). Çalışma alanı oluşturma ve programı yüklemek için bkz: [Hızlı Yükleme Kılavuzu](quickstart-installation.md). Birden fazla aboneliğiniz varsa [geçerli etkin aboneliği olmasını istediğiniz aboneliği ayarlamak](https://docs.microsoft.com/cli/azure/account?view=azure-cli-latest#az_account_set).
* Windows 10 (Bu örnekteki yönergeleri genellikle macOS sistemleri için aynıdır).
* Bir veri bilimi sanal makinesi (DSVM) Linux (Ubuntu), tercihen Doğu ABD bölgesinde veri yeri bulur. Bir Ubuntu DSVM izleyerek sağlayabileceğiniz [bu yönergeleri](https://docs.microsoft.com/azure/machine-learning/data-science-virtual-machine/dsvm-ubuntu-intro). Ayrıca bkz [Bu hızlı başlangıçta](https://ms.portal.azure.com/#create/microsoft-ads.linux-data-science-vm-ubuntulinuxdsvmubuntu). En az 8 çekirdek ve 32 GB bellek ile bir sanal makine kullanmanızı öneririz. 

İzleyin [yönerge](../desktop-workbench/known-issues-and-troubleshooting-guide.md#remove-vm-execution-error-no-tty-present) AML Workbench için VM'de parola olmadan sudoer erişimi etkinleştirmek için.  Kullanmayı da tercih edebilirsiniz [oluşturup AML Workbench'te kullanarak VM için SSH anahtar tabanlı kimlik doğrulaması](experimentation-service-configuration.md#using-ssh-key-based-authentication-for-creating-and-using-compute-targets). Bu örnekte, sanal Makineye erişmek için parola kullanın.  Aşağıdaki tabloda, sonraki adımlara DSVM bilgileri ile kaydedin:

 Alan adı| Değer |  
 |------------|------|
DSVM IP adresi | xxx|
 Kullanıcı adı  | xxx|
 Parola   | xxx|


 Herhangi bir VM ile kullanmayı seçebilirsiniz [Docker altyapısı](https://docs.docker.com/engine/) yüklü.

* Hortonworks Data Platform sürümü 3.6 ve Spark sürümü olan bir HDInsight Spark kümesi 2.1.x, tercihen Doğu ABD bölgesinde veri yeri bulur. Ziyaret [Azure HDInsight Apache Spark kümesi oluşturma](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-provision-linux-clusters) HDInsight kümeleri oluşturma hakkında ayrıntılar için. 16 çekirdek ve 112 GB bellek sahip her çalışana bir üç alt kümesi kullanmanızı öneririz. Veya yalnızca VM türü seçebilirsiniz `D12 V2` baş düğümü ve `D14 V2` çalışan düğümünüz için. Küme dağıtımı, yaklaşık 20 dakika sürer. Küme adı, SSH kullanıcı adı ve bu örnek denemek için parola gerekir. Aşağıdaki tabloda, sonraki adımlar için Azure HDInsight küme bilgileri ile kaydedin:

 Alan adı| Değer |  
 |------------|------|
 Küme adı| xxx|
 Kullanıcı adı  | xxx (varsayılan olarak sshuser)|
 Parola   | xxx|


* Azure Depolama hesabı. İzleyebileceğiniz [bu yönergeleri](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account) oluşturmak için. Ayrıca, adlara sahip iki özel blob kapsayıcı oluşturun `fullmodel` ve `onemonthmodel` bu depolama hesabında. Depolama hesabı, Ara işlem sonuçları ve makine öğrenimi modellerini kaydetmek için kullanılır. Bu örnek denemek için depolama hesabı adını ve erişim anahtarı gerekir. Aşağıdaki tabloda, sonraki adımlar için Azure depolama hesabı bilgileri ile kaydedin:

 Alan adı| Değer |  
 |------------|------|
 Depolama hesabı adı| xxx|
 Erişim anahtarı  | xxx|


Ubuntu DSVM ve önkoşul listesinde oluşturduğunuz Azure HDInsight kümesi, işlem hedeflerdir. Machine Learning Workbench, bağlamında işlem kaynağı, Workbench çalıştığı bilgisayardan farklı olabilir hedeflerdir işlem.   

## <a name="create-a-new-workbench-project"></a>Workbench yeni bir proje oluşturun

Bu örnek bir şablon kullanarak yeni bir proje oluşturun:
1.  Machine Learning Workbench’i açın.
2.  Üzerinde **projeleri** sayfasında **+** oturum açın ve seçin **yeni proje**.
3.  İçinde **yeni proje oluştur** bölmesinde, yeni projeniz için bilgileri doldurun.
4.  İçinde **proje şablonlarında Ara** arama kutusuna **iş yükü tahmin terabaytlarca veri çubuğunda**, şablonu seçin.
5.  **Oluştur**’u seçin.

Önceden oluşturulmuş bir git deposu Workbench projesini izleyerek oluşturabilirsiniz [bu yönerge](./tutorial-classifying-iris-part-1.md).  
Çalıştırma `git status` dosyaların sürüm izleme durumunu denetlemek için.

## <a name="data-description"></a>Veri açıklaması

Bu örnekte kullanılan veri Sentezlenen sunucu iş yükü verilerdir. Doğu ABD bölgesinde publicaly erişilebilir olan bir Azure Blob Depolama hesabında barındırılır. Belirli bir depolama hesabı bilgileri bulunabilir `dataFile` alanını [ `Config/storageconfig.json` ](https://github.com/Azure/MachineLearningSamples-BigData/blob/master/Config/fulldata_storageconfig.json) biçimi "wasb: / /<BlobStorageContainerName>@<StorageAccountName>.blob.core.windows.net/<path>". Verileri doğrudan Blob depolamadan kullanabilirsiniz. Depolama aynı anda birden çok kullanıcı tarafından kullanılıyorsa, kullanabileceğiniz [azcopy](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-linux) daha iyi deneme deneyimi için kendi depolamaya veri yüklemek için. 

Toplam veri boyutu yaklaşık 1 TB'dir. Her dosya, yaklaşık 1-3 GB'tır ve üst bilgi içermeyen CSV dosyası biçiminde olan. Her veri satırının belirli bir sunucudaki bir işlem yükünü temsil eder. Ayrıntılı bilgilerin veri şeması aşağıdaki gibidir:

Sütun numarası | Alan adı| Tür | Açıklama |  
|------------|------|-------------|---------------|
1  | `SessionStart` | Tarih saat |    Oturum başlangıç saati
2  |`SessionEnd`    | Tarih saat | Oturum bitiş saati
3 |`ConcurrentConnectionCounts` | Tamsayı | Eş zamanlı bağlantı sayısı
4 | `MbytesTransferred` | çift | Megabayt cinsinden aktarılan normalleştirilmiş veriler
5 | `ServiceGrade` | Tamsayı |  Hizmet sınıf oturum için
6 | `HTTP1` | Tamsayı|  Oturumu HTTP1 veya HTTP2 kullanır.
7 |`ServerType` | Tamsayı   |Sunucu türü
8 |`SubService_1_Load` | çift |   Subservice 1 yükleme
9 | `SubService_2_Load` | çift |  Subservice 2 yükleme
10 | `SubService_3_Load` | çift |     Subservice 3 yük
11 |`SubService_4_Load` | çift |  Subservice 4 yük
12 | `SubService_5_Load`| çift |      Subservice 5 yük
13 |`SecureBytes_Load`  | çift | Güvenli bayt yük
14 |`TotalLoad` | çift | Sunucu üzerindeki toplam yükü
15 |`ClientIP` | Dize|    İstemci IP adresi
16 |`ServerIP` | Dize|    Sunucu IP adresi



Beklenen veri türlerine yukarıdaki tabloda listelenen unutmayın. Eksik değerleri ve kirli veri sorunları nedeniyle veri türleri, aslında beklendiği gibi olan bir garanti yoktur. Veri işleme bu dikkate almanız gerekir. 


## <a name="scenario-structure"></a>Senaryo yapısı

Bu örnekte dosyalar gibi düzenlenir.

| Dosya adı | Tür | Açıklama |
|-----------|------|-------------|
| `Code` | Klasör | Klasör örnekte tüm kod içerir. |
| `Config` | Klasör | Yapılandırma dosyaları içeren klasör. |
| `Image` | Klasör | Görüntüler için Benioku dosyasını kaydetmek için kullanılan klasör. |
| `Model` | Klasör | Model dosyaları kaydetmek için kullanılan klasör Blob depolama alanından indirilir. |
| `Code/etl.py` | Soubor Pythonu | Veri hazırlama ve özellik Mühendisliği için kullanılan Python dosyası. |
| `Code/train.py` | Soubor Pythonu | Python üç sınıf multi-sınıflandırma modeli eğitmek için kullanılan dosya.  |
| `Code/webservice.py` | Soubor Pythonu | Kullanıma hazır hale getirme için kullanılan Python dosyası.  |
| `Code/scoring_webservice.py` | Soubor Pythonu |  Veri dönüştürme için kullanılan ve web hizmetini çağırmak Python dosyası. |
| `Code/O16Npreprocessing.py` | Soubor Pythonu | Python scoring_webservice.py için verileri ön işleme için kullanılan dosya.  |
| `Code/util.py` | Soubor Pythonu | Azure BLOB'ları yazma ve okuma için kod içeren bir Python dosyası.  
| `Config/storageconfig.json` | JSON dosyası | Ara sonuçlar ve işleme ve eğitim modeli, bir aylık verileri depolayan Azure blob kapsayıcısı için yapılandırma dosyası. |
| `Config/fulldata_storageconfig.json` | JSON dosyası | Ara sonuçlar ve işleme ve eğitim modeli tam bir veri kümesinde depolayan Azure blob kapsayıcısı için yapılandırma dosyası.|
| `Config/webservice.json` | JSON dosyası | Scoring_webservice.py yapılandırma dosyası.|
| `Config/conda_dependencies.yml` | YAML dosyası | Conda bağımlılık dosyası. |
| `Config/conda_dependencies_webservice.yml` | YAML dosyası | Web hizmeti için Conda bağımlılık dosyası.|
| `Config/dsvm_spark_dependencies.yml` | YAML dosyası | Ubuntu DSVM için Spark bağımlılık dosyası. |
| `Config/hdi_spark_dependencies.yml` | YAML dosyası | HDInsight Spark kümesi için Spark bağımlılık dosyası. |
| `README.md` | Markdown dosyası | Benioku markdown dosyası. |
| `Code/download_model.py` | Soubor Pythonu | Azure'dan model dosyaları indirmek için kullanılan bir Python dosyası bir yerel diske blob. |
| `Docs/DownloadModelsFromBlob.md` | Markdown dosyası | Çalıştırılacak hakkında yönergeler içeren bir markdown dosyası `Code/download_model.py`. |



### <a name="data-flow"></a>Veri akışı

Kodda [ `Code/etl.py` ](https://github.com/Azure/MachineLearningSamples-BigData/blob/master/Code/etl.py) verileri genel olarak erişilebilir kapsayıcıdan yükler (`dataFile` alanını [ `Config/storageconfig.json` ](https://github.com/Azure/MachineLearningSamples-BigData/blob/master/Config/fulldata_storageconfig.json)). Veri hazırlama ve özellik Mühendisliği içerir ve Ara bir işlem sonuçları ve modelleri için kendi özel kapsayıcınızı kaydeder. Kodda [ `Code/train.py` ](https://github.com/Azure/MachineLearningSamples-BigData/blob/master/Code/train.py) eğitilen makine öğrenimi modeli özel kapsayıcıya Yazar özel kapsayıcıdan Ara işlem sonuçları yükler ve çok sınıflı sınıflandırma modeli eğitir. 

Bir aylık veri kümesi ve veri kümesinin tamamı üzerinde deneme için başka bir deneme için bir kapsayıcı kullanmanız gerekir. Modeller ve veri Parquet dosyası olarak kaydedilir çünkü her dosya birden çok BLOB'ları içeren kapsayıcı, aslında bir klasöründe bulunur. Sonuçta elde edilen kapsayıcı şu şekilde görünür:

| BLOB ön ek adı | Tür | Açıklama |
|-----------|------|-------------|
| featureScaleModel | Parquet | Sayısal özellikleri için standart scaler modeli. |
| stringIndexModel | Parquet | Sayısal olmayan özellikler için dizin oluşturucu modeli dize.|
| oneHotEncoderModel|Parquet | Kategorik özellikleri için bir hot Kodlayıcı modeli. |
| mlModel | Parquet | Eğitilen makine öğrenimi modeli. |
| bilgi| Python pickle dosyasını | Eğitim başlangıç, eğitim son, süre, zaman damgası için de dahil olmak üzere dönüştürülmüş verileri hakkında bilgiler eğitme ve test bölme ve dizin oluşturma ve sık erişimli bir kodlama için sütun.

Tüm dosyalar ve bloblar önceki tabloda kullanıma hazır hale getirme için kullanılır.


### <a name="model-development"></a>Model geliştirme

#### <a name="architecture-diagram"></a>Mimari diyagramı


Modeli geliştirmek için Machine Learning Workbench'i kullanarak uçtan uca iş akışı aşağıdaki diyagramda gösterilmiştir: ![mimarisi](media/scenario-big-data/architecture.PNG)

Aşağıdaki bölümlerde, model geliştirme Machine Learning Workbench'te uzak işlem hedef işlevler kullanılarak göstereceğiz. Biz öncelikle az miktarda bir örnek verileri yüklemek ve betiği çalıştırın [ `Code/etl.py` ](https://github.com/Azure/MachineLearningSamples-BigData/blob/master/Code/etl.py) hızlı yineleme için bir Ubuntu DSVM üzerinde. İçinde yaptığımız iş biz sınırlandırabilirsiniz [ `Code/etl.py` ](https://github.com/Azure/MachineLearningSamples-BigData/blob/master/Code/etl.py) göre daha hızlı yineleme için fazladan bağımsız değişken geçirme. Sonunda bir HDInsight kümesi ile tam veri geliştirmek için kullanırız.     

[ `Code/etl.py` ](https://github.com/Azure/MachineLearningSamples-BigData/blob/master/Code/etl.py) Dosyasını yükler ve verileri hazırlar ve özellik Mühendisliği gerçekleştirir. Bu, iki bağımsız değişkeni kabul eder:
* Modelleri ve Ara bir işlem sonuçlarını depolamak için Blob Depolama kapsayıcısı için bir yapılandırma dosyası.
* Hata ayıklama yapılandırma bağımsız değişkeni daha hızlı yineleme için.

İlk bağımsız değişken `configFilename`, bir yerel yapılandırma dosyası burada, Blob Depolama bilgilerini depolamak ve verileri yüklemek istediğiniz yeri belirtin. Varsayılan olarak, olduğu [ `Config/storageconfig.json` ](https://github.com/Azure/MachineLearningSamples-BigData/blob/master/Config/storageconfig.json), ve bir aylık verileri çalıştırmak için kullanılacak geçiyor. Biz de [ `Config/fulldata_storageconfig.json` ](https://github.com/Azure/MachineLearningSamples-BigData/blob/master/Config/fulldatastorageconfig.json), hangi Çalıştır tam veri kümesinde kullanmanız gerekir. İçerik yapılandırması aşağıdaki gibidir: 

| Alan | Tür | Açıklama |
|-----------|------|-------------|
| storageAccount | Dize | Azure depolama hesabı adı |
| storageContainer | Dize | Azure depolama hesabında, Ara sonuçlarını depolamak için kapsayıcı |
| Depolama anahtarı | Dize |Azure depolama hesabı erişim anahtarı |
| veri dosyası|Dize | Veri kaynağı dosyaları  |
| süre| Dize | Veri kaynak dosyalarındaki verileri süresi|

İkisini de değiştirin `Config/storageconfig.json` ve `Config/fulldata_storageconfig.json` depolama hesabına, depolama anahtarı ve Ara sonuçlarını depolamak için blob kapsayıcısı yapılandırmak için. Varsayılan olarak, bir aylık verileri çalıştırmak için blob kapsayıcısıdır `onemonthmodel`, ve tam veri kümesi çalıştırmak için blob kapsayıcısı `fullmodel`. Depolama hesabınızda bu iki kapsayıcı oluşturduğunuzdan emin olun. `dataFile` Alanındaki [ `Config/fulldata_storageconfig.json` ](https://github.com/Azure/MachineLearningSamples-BigData/blob/master/Config/fulldatastorageconfig.json) hangi verilerin yüklendiği yapılandırır [ `Code/etl.py` ](https://github.com/Azure/MachineLearningSamples-BigData/blob/master/Code/etl.py). `duration` Alan verileri içeren aralığı yapılandırır. Süre için ONE_MONTH ayarlarsanız, yüklenen veriler için Haziran 2016 verilerin yedi dosyaları arasında yalnızca bir .csv dosyası olmalıdır. Süresi dolu ise, tam veri kümesi (1 TB) yüklenir. Değiştirmeniz gerekmez `dataFile` ve `duration` bu iki yapılandırma dosyalarında.

İkinci bağımsız değişkeni, hata ayıklama ' dir. İçin FILTER_IP ayarı daha hızlı yineleme sağlar. Bu parametrenin kullanılması, betiğinizin hata ayıklamak istiyorsanız faydalıdır.

> [!NOTE]
> Tüm aşağıdaki komutlar, herhangi bir bağımsız değişkenin gerçek değeriyle değiştirin.
> 


#### <a name="model-development-on-the-docker-of-ubuntu-dsvm"></a>Docker'ın Ubuntu DSVM üzerinde model geliştirme

#####  <a name="1-set-up-the-compute-target"></a>1. İşlem hedef ayarlayın

Komut satırı seçerek Machine Learning Workbench uygulamasını başlatın **dosya** > **komut istemini Aç**. Ardından şunu çalıştırın: 

```az ml computetarget attach remotedocker --name dockerdsvm --address $DSVMIPaddress  --username $user --password $password ```

Projenizi aml_config klasöründe bulunan aşağıdaki iki dosya oluşturulur:

-  dockerdsvm.COMPUTE: Bu dosya bir uzaktan yürütme hedef bağlantı ve yapılandırma bilgilerini içerir.
-  dockerdsvm.runconfig: Bu dosya, Workbench uygulaması içinde kullanılan Çalıştır seçeneklerini kümesidir.

Dockerdsvm.runconfig için göz atın ve bu alanların yapılandırmasını aşağıdaki gibi değiştirin:

    PrepareEnvironment: true 
    CondaDependenciesFile: Config/conda_dependencies.yml 
    SparkDependenciesFile: Config/dsvm_spark_dependencies.yml

Çalıştırarak proje ortamı hazırlamalısınız:

```az ml experiment prepare -c dockerdsvm```


İle `PrepareEnvironment` true, Machine Learning Workbench ayarlanırsa, her bir iş gönderdiniz çalışma zamanı ortamı oluşturur. `Config/conda_dependencies.yml` ve `Config/dsvm_spark_dependencies.yml` çalışma zamanı ortamı özelleştirilmesini içerir. Bu iki YMAL dosyalarını düzenleyerek Conda bağımlılıklarını, Spark yapılandırması ve Spark bağımlılıkları her zaman değiştirebilirsiniz. Bu örnekte, eklediğimiz `azure-storage` ve `azure-ml-api-sdk` ek Python paketlerini olarak `Config/conda_dependencies.yml`. Ekledik `spark.default.parallelism`, `spark.executor.instances`, ve `spark.executor.cores` içinde `Config/dsvm_spark_dependencies.yml`. 

#####  <a name="2-data-preparation-and-feature-engineering-on-dsvm-docker"></a>2. Veri hazırlama ve özellik Mühendisliği DSVM docker'da

Betiği çalıştırmak `etl.py` DSVM Docker üzerinde. Yüklenen verilere belirli sunucu IP adreslerine sahip filtreler bir hata ayıklama parametresini kullanın:

```az ml experiment submit -t dockerdsvm -c dockerdsvm ./Code/etl.py ./Config/storageconfig.json FILTER_IP```

Yan bölme için gözatın ve seçin **çalıştırma** çalıştırma geçmişini görmek için `etl.py`. Çalışma süresi yaklaşık iki dakika olduğuna dikkat edin. Kodunuzu yeni özellikleri içerecek şekilde değiştirmeyi planlıyorsanız, ikinci bağımsız değişkeni olarak FILTER_IP daha hızlı yineleme sağlar. Bu adım veri kümesini araştırmak ve yeni özellikler oluşturmak için sorunları, kendi makine öğrenimi ile ilgilenirken birden çok kez çalıştırmak gerekebilir. 

Hangi verilerin yükleneceğini ve hangi verileri işlemek için başka filtre özelleştirilmiş kısıtlaması ile model geliştirmede yineleme sürecinize hız kazandırabilir. Denerken, kodunuzda bir Git deposu için düzenli aralıklarla değişiklikleri kaydetmeniz gerekir. Aşağıdaki kodda kullandık Not `etl.py` özel kapsayıcıya erişimi etkinleştirmek için:

```python
def attach_storage_container(spark, account, key):
    config = spark._sc._jsc.hadoopConfiguration()
    setting = "fs.azure.account.key." + account + ".blob.core.windows.net"
    if not config.get(setting):
        config.set(setting, key)

# attach the blob storage to the spark cluster or VM so that the storage can be accessed by the cluster or VM        
attach_storage_container(spark, storageAccount, storageKey)
```


Ardından, betiği çalıştırmak `etl.py` DSVM docker'da FILTER_IP hata ayıklama parametresi olmadan:

```az ml experiment submit -t dockerdsvm -c dockerdsvm ./Code/etl.py ./Config/storageconfig.json FALSE```

Yan bölme için gözatın ve seçin **çalıştırma** çalıştırma geçmişini görmek için `etl.py`. Çalışma süresi yaklaşık dört dakika olduğuna dikkat edin. Bu adım işlenen sonucunu kapsayıcıya kaydedilir ve train.py eğitimi için yüklenir. Ayrıca, dize dizin oluşturucular, kodlayıcı işlem hatları ve standart scalers özel kapsayıcıya kaydedilir. Bunlar, kullanıma hazır hale getirme içinde kullanılır. 


##### <a name="3-model-training-on-dsvm-docker"></a>3. DSVM docker'da modeli eğitimi

Betiği çalıştırmak `train.py` DSVM Docker üzerinde:

```az ml experiment submit -t dockerdsvm -c dockerdsvm ./Code/train.py ./Config/storageconfig.json```

Bu adım Çalıştır Ara işlem sonuçları yükler `etl.py`ve makine öğrenme modeli eğitir. Bu adım, yaklaşık iki dakika sürer.

Küçük veri çubuğunda deneme başarıyla tamamladıktan sonra deneme tam veri kümesi üzerinde çalışmaya devam edebilirsiniz. Aynı kod kullanarak başlatın ve ardından bağımsız değişkeni ile denemeler yapın ve hedef değişiklikleri işlem.  

####  <a name="model-development-on-the-hdinsight-cluster"></a>HDInsight kümesi üzerinde model geliştirme

##### <a name="1-create-the-compute-target-in-machine-learning-workbench-for-the-hdinsight-cluster"></a>1. İşlem hedef Machine Learning Workbench'te için HDInsight kümesi oluşturma

```az ml computetarget attach cluster --name myhdi --address $clustername-ssh.azurehdinsight.net --username $username --password $password```

Aşağıdaki iki dosyada aml_config klasöründe oluşturulur:
    
-  myhdi.COMPUTE: Bu dosya bir uzaktan yürütme hedef bağlantı ve yapılandırma bilgilerini içerir.
-  myhdi.runconfig: Bu dosya, Workbench uygulaması içinde kullanılan çalıştırma seçenekleri ayarlanır.


Myhdi.runconfig için göz atın ve bu alanların yapılandırmasını aşağıdaki gibi değiştirin:

    PrepareEnvironment: true 
    CondaDependenciesFile: Config/conda_dependencies.yml 
    SparkDependenciesFile: Config/hdi_spark_dependencies.yml

Çalıştırarak proje ortamı hazırlamalısınız:

```az ml experiment prepare -c myhdi```

Bu adım, yedi dakikaya kadar sürebilir.

##### <a name="2-data-preparation-and-feature-engineering-on-hdinsight-cluster"></a>2. Veri hazırlama ve HDInsight kümesi üzerinde özellik Mühendisliği

Betiği çalıştırmak `etl.py`, HDInsight kümesi üzerinde tam verilerle:

```az ml experiment submit -a -t myhdi -c myhdi ./Code/etl.py Config/fulldata_storageconfig.json FALSE```

Görece uzun bir süredir (yaklaşık iki saat) bu iş sürdüğü için kullanabileceğiniz `-a` çıkışının akışını devre dışı bırakmak için. İşi bittiğinde de **çalıştırma geçmişi**, sürücü ve denetleyici günlükleri görüntüleyebilirsiniz. Daha büyük bir kümeye varsa, her zaman yapılandırmalarında yapılandırılmadan `Config/hdi_spark_dependencies.yml` daha fazla örnek veya çekirdek kullanılacak. Örneğin, dört alt düğüm kümesi varsa değerini artırabilir `spark.executor.instances` 5 7. Bu adımda çıktısını görebilirsiniz **fullmodel** depolama hesabınızdaki kapsayıcı. 


##### <a name="3-model-training-on-hdinsight-cluster"></a>3. HDInsight kümesi üzerinde modeli eğitimi

Betiği çalıştırmak `train.py` HDInsight kümesinde:

```az ml experiment submit -a -t myhdi -c myhdi ./Code/train.py Config/fulldata_storageconfig.json```

Görece uzun bir süredir (yaklaşık 30 dakika) bu iş sürdüğü için kullanabileceğiniz `-a` çıkışının akışını devre dışı bırakmak için.

#### <a name="run-history-exploration"></a>Geçmiş incelemesini çalıştırın

Çalıştırma geçmişi, Machine Learning Workbench, deneme izlenmesini sağlayan bir özelliktir. Varsayılan olarak, deneme süresini izler. Tam veri kümesi için taşıdığınızda belirli örneğimizde `Code/etl.py` deneme içinde biz süresi önemli ölçüde arttığına dikkat edin. Ayrıca, izleme amacıyla belirli ölçümleri günlüğe kaydedebilirsiniz. Ölçüm izlemeyi etkinleştirmek için aşağıdaki kod satırlarını Python dosyanızın başında ekleyin:
```python
# import logger
from azureml.logging import get_azureml_logger

# initialize logger
run_logger = get_azureml_logger()
```
Belirli bir ölçüyü izlemek için bir örnek aşağıda verilmiştir:

```python
run_logger.log("Test Accuracy", testAccuracy)
```

Workbench sağ kenar çubuğunda, göz atın **çalıştırmaları** her bir Python dosyası için çalıştırma geçmişini görmek için. Ayrıca, GitHub deponuza gidebilirsiniz. Yeni bir dal, her bir çalıştırmanın betiğinizde yaptığınız değişiklikleri izlemek için "AMLHistory ile" Başlangıç adıyla oluşturulur. 


### <a name="operationalize-the-model"></a>Modeli kullanıma hazır hale getirme

Bu bölümde, bir web hizmeti olarak önceki adımlarda oluşturduğunuz modeli kullanıma hazır hale. Ayrıca iş yükü tahmin etmek için web hizmetini kullanmayı öğrenin. Makine dili kullanıma hazır hale getirme komut satırı arabirimi (Clı'ler) kodun ve bağımlılıkların Docker görüntüleri olarak paketlemek ve modeli yayımlamak için bir kapsayıcıda barındırılan web hizmeti olarak kullanın.

Machine Learning Workbench'te komut satırı istemi Clı'leri çalıştırmak için kullanabilirsiniz.  İzleyerek Ubuntu Linux üzerinde Clı'leri çalıştırabilirsiniz [Yükleme Kılavuzu](./deployment-setup-configuration.md#using-the-cli). 

> [!NOTE]
> Tüm aşağıdaki komutlar, herhangi bir bağımsız değişkenin gerçek değeriyle değiştirin. Bu bölümde tamamlanması yaklaşık 40 dakika sürer.
> 

Benzersiz bir dize olarak kullanıma hazır hale getirme ortamı seçin. Burada, "[benzersiz]" dize seçtiğiniz dizesini temsil etmek için kullanırız.

1. Kullanıma hazır hale getirme ortamı oluşturun ve kaynak grubu oluşturun.

        az ml env setup -c -n [unique] --location eastus2 --cluster -z 5 --yes

   Kapsayıcı hizmeti ortamı olarak kullanarak kullanabileceğiniz Not `--cluster` içinde `az ml env setup` komutu. Üzerinde machine learning modeli faaliyete [Azure Container Service](https://docs.microsoft.com/azure/container-service/kubernetes/container-service-intro-kubernetes). Kullandığı [Kubernetes](https://kubernetes.io/) dağıtım, ölçeklendirme ve kapsayıcılı uygulamaların yönetimini otomatikleştirmek için. Bu komutu çalıştırmak için yaklaşık 20 dakika sürer. Dağıtım başarıyla tamamlandı, kontrol etmek için aşağıdakileri kullanın: 

        az ml env show -g [unique]rg -n [unique]

   Aşağıdaki komutu çalıştırarak oluşturduğunuz bir dağıtım ortamı ayarlayın:

        az ml env set -g [unique]rg -n [unique]

2. Oluşturun ve model Yönetimi hesabı kullanın. Model Yönetimi hesabı oluşturmak için aşağıdaki komutu çalıştırın:

        az ml account modelmanagement create --location  eastus2 -n [unique]acc -g [unique]rg --sku-instances 4 --sku-name S3 

   Model yönetimi, kullanıma hazır hale getirme için aşağıdaki komutu çalıştırarak kullanın:

        az ml account modelmanagement set  -n [unique]acc -g [unique]rg  

   Model Yönetimi hesabı, modelleri ve web hizmetleri yönetmek için kullanılır. Azure portalından oluşturulan yeni model Yönetimi hesabı görebilirsiniz. Modelleri, bildirimler, Docker görüntülerini ve bu model Yönetimi hesabı kullanılarak oluşturulan Hizmetleri görebilirsiniz.

3. Karşıdan yükleme ve modelleri kaydedin.

   ' Deki modellerde indirin **fullmodel** yerel makinenize kodu dizininde kapsayıcı. Parquet veri dosyası adı "vmlSource.parquet." ile yükleme Bir model dosyası değil; Ara işlem bir sonucudur. Ayrıca, Git deposunda bulunan model dosyalarını yeniden kullanabilirsiniz. Daha fazla bilgi için [GitHub](https://github.com/Azure/MachineLearningSamples-BigData/blob/master/Docs/DownloadModelsFromBlob.md). 

   Git `Model` CLI ve kayıt klasöründe modelleri olarak izler:

        az ml model register -m  mlModel -n vmlModel -t fullmodel
        az ml model register -m  featureScaleModel -n featureScaleModel -t fullmodel
        az ml model register -m  oneHotEncoderModel -n  oneHotEncoderModel -t fullmodel
        az ml model register -m  stringIndexModel -n stringIndexModel -t fullmodel
        az ml model register -m  info -n info -t fullmodel

   Her komutun çıkışı, sonraki adımda gerekli olan bir model kimliği verir. Gelecekte kullanım için bir metin dosyasına kaydedin.

4. Web hizmeti için bir bildirim oluşturun.

   Bir bildirim, web hizmeti, machine learning modelleri ve çalışma zamanı ortamı bağımlılıkları kodunu içerir. Git `Code` klasöründe CLI ve şu komutu çalıştırın:

        az ml manifest create -n $webserviceName -f webservice.py -r spark-py -c ../Config/conda_dependencies_webservice.yml -i $modelID1 -i $modelID2 -i $modelID3 -i $modelID4 -i $modelID5

   Çıktı, sonraki adım için bir bildirim kimliği sağlar. 

   İçinde kalmak `Code` dizini ve test webservice.py aşağıdakini çalıştırarak: 

        az ml experiment submit -t dockerdsvm -c dockerdsvm webservice.py

5. Bir Docker görüntüsü oluşturun. 

        az ml image create -n [unique]image --manifest-id $manifestID

   Çıkış bir sonraki adımda bir görüntü kimliği verir, kapsayıcı hizmeti bu docker görüntüsü kullanılır. 

6. Web hizmeti, kapsayıcı hizmeti kümesine dağıtın.

        az ml service create realtime -n [unique] --image-id $imageID --cpu 0.5 --memory 2G

   Bir hizmet kimliği çıktısını verir Yetkilendirme anahtarı alın ve hizmet URL'si için kullanmanız gerekir.

7. Web hizmeti küçük toplu işler üzerinde puanlamak için Python kodu çağırın.

   Yetkilendirme anahtarı almak için aşağıdaki komutu kullanın:

         az ml service keys realtime -i $ServiceID 

   Hizmet Puanlama URL'sini almak için aşağıdaki komutu kullanın:

        az ml service usage realtime -i $ServiceID

   İçeriği Değiştir `./Config/webservice.json` Puanlama URL'sini ve yetkilendirme anahtarını doğru hizmet ile. "Bearer" orijinal dosyasında saklayın ve "xxx" bölümünü değiştirin. 
   
   Proje kök dizinine gidin ve test için Mini toplu aşağıdaki kullanarak Puanlama web hizmeti:

        az ml experiment submit -t dockerdsvm -c dockerdsvm ./Code/scoring_webservice.py ./Config/webservice.json

8. Web hizmeti ölçeklendirin. 

   Daha fazla bilgi için [Azure Container Service kümenizde kullanıma hazır hale getirme ölçeklendirme](how-to-scale-clusters.md).
 

## <a name="next-steps"></a>Sonraki adımlar

Bu örnek, bir machine learning büyük veriler üzerinde modeli eğitmek için Machine Learning Workbench'i kullanma vurgular ve eğitim modeli kullanıma hazır hale getirin. Özellikle, yapılandırma ve farklı işlem hedeflerini kullan ve çalıştırma geçmişi ölçümleri izleme ve farklı çalışmalarında kullanma hakkında bilgi edindiniz.

Çapraz doğrulama ve hyper parametresi ayarlama keşfetmek için kodu genişletebilirsiniz. Çapraz doğrulama ve hyper parametresi ayarlama hakkında daha fazla bilgi edinmek için [bu GitHub kaynak](https://github.com/Azure/MachineLearningSamples-DistributedHyperParameterTuning).  

Zaman serisi tahmin etme hakkında daha fazla bilgi için bkz: [bu GitHub kaynak](https://github.com/Azure/MachineLearningSamples-EnergyDemandTimeSeriesForecasting).