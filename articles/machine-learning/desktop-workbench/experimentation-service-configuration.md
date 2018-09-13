---
title: Azure Machine Learning deneme hizmeti yapılandırma | Microsoft Docs
description: Bu makalede, Azure Machine Learning deneme hizmeti üst düzey bir bakış yönergeleri ile nasıl yapılandırılacağı sağlar.
services: machine-learning
author: gokhanuluderya-msft
ms.author: gokhanu
manager: haining
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.topic: article
ms.date: 09/28/2017
ms.openlocfilehash: e79817ffad139e0a3bcb0ba32b9bc6e5666319d0
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2018
ms.locfileid: "35646765"
---
# <a name="configuring-azure-machine-learning-experimentation-service"></a>Azure Machine Learning deneme hizmeti yapılandırma

## <a name="overview"></a>Genel Bakış
Azure Machine Learning deneme hizmeti kullanılarak Azure Machine Learning yürütme kendi denemeleri yürütün ve yönetim özellikleri çalıştırmak veri bilimcilerine sağlar. Çevik deneme hızlı yineleme için bir çerçeve sunar. Azure Machine Learning Workbench ile başlaması yerel çalışmalar makinenize ve ayrıca bir kolayca yolu GPU veya çalışan Spark HDInsight kümeleri ile uzak veri bilimi Vm'lerini gibi diğer ortamlara ve genişletilmesini sağlar.

Deneme hizmeti, denemelerinizi yalıtılmış, tekrarlanabilir ve tutarlı çalıştırılan sağlamak için tasarlanmıştır. İşlem hedefleri, yürütme ortamlarını yönetmek ve yapılandırmaları Çalıştır yardımcı olur. Azure Machine Learning Workbench yürütme ve çalışma yönetimi yeteneklerini kullanarak, farklı ortamlar arasında kolayca taşıyabilirsiniz. 

Yerel olarak veya yüksek ölçekli bulutta, Workbench projesinde bir Python veya Pyspark'tan betiği yürütebilirsiniz. 

Komut çalıştırabilirsiniz: 

* Yerel bilgisayarınızda Workbench tarafından yüklenen Python (3.5.2) ortamı
* Yerel bilgisayardaki bir Docker kapsayıcısı içinde Conda Python ortamı
* Sahibi olduğunuz ve uzak Linux makinesinde yönetme bir Python ortamı
* Uzak Linux makine üzerinde bir Docker kapsayıcısı içinde Conda Python ortamı. Bir [Ubuntu tabanlı DSVM azure'da] (örneğin)https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-ads.linux-data-science-vm-ubuntu)
* [HDInsight için Spark](https://azure.microsoft.com/services/hdinsight/apache-spark/) azure'da

>[!IMPORTANT]
>Azure Machine Learning deneme hizmeti şu anda Python 3.5.2 ve Spark 2.1.11 Python ve Spark'a çalışma zamanı sürümleri sırasıyla destekler. 


### <a name="key-concepts-in-experimentation-service"></a>Deneme hizmeti temel kavramları
Azure Machine Learning deneme yürütme aşağıdaki kavramları anlamak önemlidir. Sonraki bölümlerde ayrıntılı olarak bu kavramlar kullanmayı ele alır. 

#### <a name="compute-target"></a>Hedef işlem
A _hedef işlem_ programınızı, masaüstü, uzak Docker gibi bir VM veya bir küme üzerinde yürütmek konumu belirtir. Bir işlem hedefine adreslenebilir ve sizin tarafınızdan erişilebilir olması gerekir. Workbench, işlem hedeflerini oluşturmak ve bunları Workbench uygulaması ve CLI kullanarak yönetme olanağı sağlar. 

_az ml computetarget ekleme_ CLI komutu çalıştırmalarınızı kullanabileceğiniz bir işlem hedefine oluşturmanıza olanak sağlar.

Desteklenen işlem hedefleri şunlardır:
* Workbench tarafından yüklü bilgisayarınızda yerel Python (3.5.2) ortamı.
* Bilgisayarınızda yerel bir Docker
* Kullanıcı tarafından yönetilen, Python ortamını uzak Linux Ubuntu vm'lerinde. Örneğin, bir [azure'da Ubuntu tabanlı DSVM](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-ads.linux-data-science-vm-ubuntu)
* Uzak Docker Ubuntu Linux vm'lerinde. Örneğin, bir [azure'da Ubuntu tabanlı DSVM](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-ads.linux-data-science-vm-ubuntu)
* [HDInsight Spark kümesi için](https://azure.microsoft.com/services/hdinsight/apache-spark/) azure'da

Deneme hizmeti şu anda Python 3.5.2 destekler ve Python olarak 2.1.11 Spark ve çalışma zamanı sürümleri, sırasıyla Spark. 

>[!IMPORTANT]
> Docker'ı olan Windows Vm'leri çalıştırma **değil** uzak işlem hedefleri olarak desteklenir.

#### <a name="execution-environment"></a>Yürütme Ortamı
_Yürütme ortamı_ çalışma zamanı yapılandırma ve Workbench'te programı çalıştırmak için gerekli bağımlılıkların tanımlar.

Workbench varsayılan çalışma zamanı üzerinde çalıştırıyorsanız, sevdiğiniz araçları ve paket yöneticilerini kullanarak yerel yürütme ortamı yönettiğiniz. 

Conda HDInsight tabanlı yürütmeleri yanı sıra yerel Docker ve uzak bir Docker yürütme yönetmek için kullanılır. Bu hedefleri işlem için yürütme ortamı yapılandırması aracılığıyla yönetilir **Conda_dependencies.yml** ve **Spark_dependencies.yml dosyaları**. Bu dosyaları **aml_config** klasörün içinde projenizin.

**Yürütme ortamları için desteklenen çalışma zamanları şunlardır:**
* Python 3.5.2
* Spark 2.1.11

### <a name="run-configuration"></a>Çalıştırma yapılandırma
Ek olarak işlem hedef ve yürütme ortamı, Azure Machine Learning tanımlayın ve değiştirmek için bir çerçeve sağlar. *yapılandırmaları Çalıştır*. Denemenizi yürütmeleri, yinelemeli deney bir parçası olarak farklı yapılandırma gerektirebilir. Farklı parametre aralıkları farklı veri kaynakları kullanarak ve spark parametreleri ayarlama Süpürme. Deneme hizmeti çalıştırma yapılandırmaları yönetmek için bir çerçeve sunar.

Çalışan _az ml computetarget ekleme_ komutu, iki dosyada üretir, **aml_config** projenizdeki klasöre: ".compute" ve "Bu kuralı aşağıdaki .runconfig": _< your_ computetarget_name > .compute_ ve _< your_computetarget_name > .runconfig_. Bir işlem hedefine oluşturduğunuzda .runconfig dosya size kolaylık olması için otomatik olarak oluşturulur. Oluşturabilir ve diğer çalıştırma yapılandırmaları kullanarak yönetme _az ml runconfigurations_ CLI komutu. Ayrıca, oluşturabilir ve bunları dosya sisteminizde Düzenle.

Ayrıca workbench'teki çalıştırma yapılandırma ortam değişkenlerini belirtmenize olanak sağlar. Ortam değişkenlerini belirtme ve aşağıdaki bölümde .runconfig dosyanıza ekleyerek bunları kodunuzda kullanın. 

```
EnvironmentVariables:
    "EXAMPLE_ENV_VAR1": "Example Value1"
    "EXAMPLE_ENV_VAR2": "Example Value2"
```

Kodunuzda bu ortam değişkenlerine erişebilir. Örneğin, bu phyton kod parçacığı "EXAMPLE_ENV_VAR1" adlı ortam değişkeni yazdırır
```
print(os.environ.get("EXAMPLE_ENV_VAR1"))
```

_**Aşağıdaki şekilde ilk deneme çalıştırmak için üst düzey akışı gösterilmektedir.**_
![](media/experimentation-service-configuration/experiment-execution-flow.png)

## <a name="experiment-execution-scenarios"></a>Deneme yürütme senaryoları
Bu bölümde, yürütme senaryolarına inceleyin ve Azure Machine Learning denemeleri, özellikle bir deneme yerel olarak uzak bir VM üzerinde ve bir HDInsight kümesi üzerinde çalışan çalışma şeklini hakkında bilgi edinin. Bu bölüm, denemelerinizi yürütme için bir işlem hedefine oluşturulmasından itibaren bir kılavuz değildir.

>[!NOTE]
>Bu makalenin geri kalanında için kavramlarını ve özelliklerini göstermek için CLI (komut satırı arabirimi) komutları kullanıyoruz. Burada açıklanan özellikler, Workbench'ten de kullanılabilir.

## <a name="launching-the-cli"></a>CLI'yı başlatma
CLI'yi başlatma için kolay bir yol Workbench ve giderek bir proje açıldığında **Dosya--> komut istemini Aç**.

![](media/experimentation-service-configuration/opening-cli.png)

Bu komut, geçerli proje klasöründe betikleri çalıştırma için komutları girebileceğiniz bir terminal penceresi başlatılır. Bu bir terminal penceresinde, Workbench tarafından yüklenen Python 3.5.2 ortamı ile yapılandırılır.

>[!NOTE]
> Yürüttüğünüzde herhangi _az ml_ komutu komut penceresinden Azure karşı kimlik doğrulaması gerekir. CLI bağımsız kimlik doğrulaması önbellek sonra Masaüstü uygulamasını kullanır ve böylece Workbench'i açtıktan CLI ortamınıza doğrulanır anlamına gelmez. Kimlik doğrulamak için aşağıdakileri kullanın. adımlar. Yalnızca belirtecin süresi dolduğunda bu adımları yineleyin. böylece, kimlik doğrulama belirtecini yerel olarak bir süreliğine önbelleğe alınır. Belirtecin süresi dolduğunda ya da kimlik doğrulama hataları görüyorsanız aşağıdaki komutları yürütün:

```
# to authenticate 
$ az login

# to list subscriptions
$ az account list -o table

# to set current subscription to a particular subscription ID 
$ az account set -s <subscription_id>

# to verify your current Azure subscription
$ az account show
```

>[!NOTE] 
>Çalıştırdığınızda _az ml_ komutu içinde bir proje klasörü, proje üzerinde bir Azure Machine Learning deneme hesabına ait olduğundan emin olun _geçerli_ Azure aboneliği. Aksi takdirde yürütme hatalarla karşılaşabilirsiniz.


## <a name="running-scripts-and-experiments"></a>Betikleri çalıştırıp denemeler
Workbench ile Python yürütebilir ve PySpark betiklerini çeşitli işlem hedeflerini kullanarak _az ml denemeyi gönderme_ komutu. Bu komut, bir çalıştırma yapılandırma tanımı gerektirir. 

Workbench, işlem hedefi oluşturmak, ancak ek çalıştırma yapılandırmaları kullanarak oluşturabileceğiniz bir karşılık gelen runconfig dosyası oluşturur _az ml runconfiguration oluşturma_ komutu. Çalıştırma yapılandırma dosyalarını el ile düzenleyebilirsiniz.

Workbench'te deneyimi deneme parçası çalıştırılmakta olan çalıştırma yapılandırmaları gösterin. 

>[!NOTE]
>Çalıştırma yapılandırma dosyasını hakkında daha fazla bilgi [deneme yürütme yapılandırması başvuru](experimentation-service-configuration-reference.md) bölümü.

## <a name="running-a-script-locally-on-workbench-installed-runtime"></a>Betik Workbench yüklü çalışma zamanı üzerinde yerel olarak çalıştırma
Workbench, doğrudan Workbench tarafından yüklenen Python 3.5.2 çalışma zamanı karşı betikleri çalıştırmanıza olanak sağlar. Bu varsayılan çalışma zamanı, Workbench Kurulum zamanında yüklenir ve Azure Machine Learning kitaplıkları ve bağımlılıklar içerir. Çalıştırma sonuçları ve yerel yürütme için yapılar hala bulutta çalıştırma geçmişi hizmeti kaydedilir.

Docker tabanlı yürütmeleri bu yapılandırmadır _değil_ Conda tarafından yönetilir. Workbench'i Python ortamınız için paket bağımlılıkları el ile sağlamanız gerekir.

Betiğinizi Workbench tarafından yüklenen Python ortamında yerel olarak çalıştırmak için aşağıdaki komutu çalıştırabilirsiniz. 

```
$az ml experiment submit -c local myscript.py
```

Varsayılan Python ortamı yolunu Workbench CLI penceresine aşağıdaki komutu yazarak bulabilirsiniz.
```
$ conda env list
```

>[!NOTE]
>Yerel olarak PySpark ortamları doğrudan yerel Spark karşı çalışan şu anda **değil** desteklenir. Workbench, PySpark betikleri yerel Docker üzerinde çalışan desteklemiyor. Azure Machine Learning temel Docker görüntüsüne önceden yüklenmiş 2.1.11 Spark ile birlikte gelir. 

_**Yerel yürütme için bir Python betiği genel bakış:**_
![](media/experimentation-service-configuration/local-native-run.png)

## <a name="running-a-script-on-local-docker"></a>Betik üzerinde yerel Docker çalıştırma
Ayrıca projelerinizi bir Docker kapsayıcısı üzerinde yerel makinenizde deneme hizmeti çalıştırabilirsiniz. Workbench sağlayan Azure Machine Learning kitaplıkları ve de temel bir Docker görüntüsü olarak yürütmeleri yerel Spark kolaylaştırmak için Spark 2.1.11 çalışma zamanı. Docker yerel makinede çalışır durumda olması gerekir.

Yerel Docker Python veya Pyspark'tan betiğinizi çalıştırmak için CLI aşağıdaki komutları yürütebilir.

```
$az ml experiment submit -c docker myscript.py
```
or
```
az ml experiment submit --run-configuration docker myscript.py
```

Üzerinde yerel Docker yürütme ortamını kullanarak Azure Machine Learning temel Docker görüntüsü hazırlanır. Workbench, ilk zaman ve yer paylaşımları için conda_dependencies.yml dosyanızda belirtilen paket ile çalıştırarak bu görüntüyü indirir. Bu işlem ilk yavaş çalışmasına neden olur ancak sonraki çalıştırmaları önbelleğe alınmış katmanları yeniden Workbench sayesinde daha hızlı olacaktır. 

>[!IMPORTANT]
>Çalıştırmanız gereken _az ml deneme hazırlama - c docker_ önce ilk çalıştırmak için Docker görüntüsünü hazırlamak için komutu. Ayrıca **PrepareEnvironment** parametresini docker.runconfig dosyanızdaki true. Bu eylem, ortamınızın çalıştırma yürütmeyi bir parçası olarak otomatik olarak hazırlar.  

>[!NOTE]
>Bir PySpark betiği Spark üzerinde çalıştırılan ise spark_dependencies.yml conda_dependencies.yml ek olarak da kullanılır.

Komut bir Docker görüntüsü üzerinde çalışan, aşağıdaki avantajları sunar:

1. Bu, diğer yürütme ortamlarında kodunuzu güvenilir bir şekilde yürütülebilir sağlar. Bir Docker kapsayıcısı üzerinde çalışırken keşfedin ve taşınabilirlik etkileyebilecek herhangi bir yerel başvurular önlemenize yardımcı olur. 

2. Kodu çalışma zamanları ve yükleme ve yapılandırma, Apache Spark gibi bunları yüklemek zorunda kalmadan karmaşık çerçeveleri hızlı bir şekilde test olanak tanır.


_**Bir Python betiği için yerel bir Docker yürütme genel bakış:**_
![](media/experimentation-service-configuration/local-docker-run.png)

## <a name="running-a-script-on-a-remote-docker"></a>Uzak bir Docker betik çalıştırma
Bazı durumlarda, yerel makinenizde kullanılabilir kaynakları istenen modeli eğitmek için yeterli olmayabilir. Bu durumda, deneme hizmeti kullanılarak uzak Docker yürütme daha güçlü Vm'lerde Python veya Pyspark'tan komut dosyalarınızı çalıştırmak kolay bir yol sağlar. 

Uzak VM aşağıdaki gereksinimleri karşılaması gerekir:
* Uzak VM, Ubuntu Linux çalışıyor olması gerekir ve SSH üzerinden erişilebilir olması gerekir. 
* Uzak VM Docker'ın çalışıyor olması gerekir.

>[!IMPORTANT]
> Docker, Windows Vm'leri çalıştırma **değil** uzak işlem hedefleri olarak desteklenen


Her iki işlem hedef tanımı oluşturma ve yapılandırma için Docker tabanlı uzak yürütme çalıştırmak için aşağıdaki komutu kullanabilirsiniz.

```
az ml computetarget attach remotedocker --name "remotevm" --address "remotevm_IP_address" --username "sshuser" --password "sshpassword" 
```

İşlem hedefini yapılandırdıktan sonra betiğinizi çalıştırmak için aşağıdaki komutu kullanabilirsiniz.
```
$ az ml experiment submit -c remotevm myscript.py
```
>[!NOTE]
>Göz önünde bulundurun, yürütme ortamı içinde conda_dependencies.yml belirtimleri kullanarak yapılandırılır. PySpark framework .runconfig dosyasında belirtilirse spark_dependencies.yml de kullanılır. 

İşlem yerel Docker için çalışır, böylece benzer bir yürütme deneyimini beklemelisiniz uzak Vm'lerde Docker oluşturma işlemi tam olarak aynıdır.

>[!TIP]
>İlk çalıştırma için Docker görüntüsünü oluşturarak yükleyebilirseniz gecikme süresi önlemek isterseniz, işlem hedef betiğinizi çalıştırmadan önce hazırlamak için aşağıdaki komutu kullanabilirsiniz. az ml deneme - c remotedocker hazırlama

_**Uzak sanal yürütme bir Python betiği için genel bakış:**_
![](media/experimentation-service-configuration/remote-vm-run.png)

## <a name="running-a-script-on-a-remote-vm-targeting-user-managed-environments"></a>Kullanıcı tarafından yönetilen ortamlarını hedefleyen uzak bir VM'de betik çalıştırma
Deneme hizmeti, kullanıcının kendi Python ortamı içinde uzak bir Ubuntu sanal makinesi üzerinde bir komut dosyası çalıştırarak da destekler. Bu yürütme için kendi ortamınızı yönetmek ve Azure Machine Learning özellikleri kullanmaya devam etmenizi sağlar. 

Kendi ortamınızda betiğinizi çalıştırmak için aşağıdaki adımları izleyin.
* Uzak bir Ubuntu VM veya bağımlılıklarınızı yükleme bir DSVM üzerinde Python ortamınızı hazırlayın.
* Azure Machine Learning gereksinimleri aşağıdaki komutu kullanarak yükleyin.

```
pip install -I --index-url https://azuremldownloads.azureedge.net/python-repository/preview --extra-index-url https://pypi.python.org/simple azureml-requirements
```

>[!TIP]
>Bazı durumlarda, sudo modunda sizin ayrıcalıklarınıza bağlı olarak bu komutu çalıştırmanız gerekebilir. 
```
sudo pip install -I --index-url https://azuremldownloads.azureedge.net/python-repository/preview --extra-index-url https://pypi.python.org/simple azureml-requirements
```
 
* Hem işlem hedef tanımı hem de uzak VM yürütme kullanıcı tarafından yönetilen çalıştırmalar için bir çalıştırma yapılandırma oluşturmak için aşağıdaki komutu kullanın.
```
az ml computetarget attach remote --name "remotevm" --address "remotevm_IP_address" --username "sshuser" --password "sshpassword" 
```
>[!NOTE]
>"UserManagedEnvironment" parametresi bu true .compute yapılandırma dosyasındaki ayarlar.

* .Compute dosyanızda yürütülebilir, Python çalışma zamanı konumunu ayarlayın. Yürütülebilir python tam yoluna başvurmanız gerekir. 
```
pythonLocation: python3
```

İşlem hedefini yapılandırdıktan sonra betiğinizi çalıştırmak için aşağıdaki komutu kullanabilirsiniz.
```
$ az ml experiment submit -c remotevm myscript.py
```

>[!NOTE]
> Bir DSVM üzerinde çalışırken aşağıdaki komutları kullanmanız gerekir

Doğrudan DSVM'ın genel python ortamda çalıştırmak istiyorsanız, bu komutu çalıştırın.
```
sudo /anaconda/envs/py35/bin/pip install <package>
```


## <a name="running-a-script-on-an-hdinsight-cluster"></a>Bir betiği bir HDInsight kümesinde çalıştırma
HDInsight, Apache Spark'ı destekleyen büyük veri analizi için popüler bir platformdur. Workbench, deneme HDInsight Spark kümeleri kullanarak büyük verileri sağlar. 

>[!NOTE]
>HDInsight kümesi birincil depolama olarak Azure Blob kullanmalıdır. Azure Data Lake depolamanın kullanılması henüz desteklenmemektedir.

İşlem hedefi oluşturmak ve çalıştırmak için aşağıdaki komutu kullanarak bir HDInsight Spark kümesi yapılandırması:

```
$ az ml computetarget attach cluster --name "myhdi" --address "<FQDN or IP address>" --username "sshuser" --password "sshpassword"  
```

>[!NOTE]
>IP adresi yerine FQDN kullanın ve HDI Spark kümenizin adlı _foo_, sürücü düğümde adlı SSH uç noktası olan _foo-ssh.azurehdinsight.net_. Unutmayın **-ssh** sonek sunucu adı için FQDN kullanırken _--adresi_ parametresi.


İşlem bağlamı oluşturduktan sonra PySpark betiğini yürütmek için aşağıdaki komutu çalıştırabilirsiniz.

```
$ az ml experiment submit -c myhdi myscript.py
```

Workbench hazırlar ve Conda kullanarak HDInsight kümesinde yürütme ortamı yönetir. Yapılandırma tarafından yönetilir _conda_dependencies.yml_ ve _spark_dependencies.yml_ yapılandırma dosyaları. 

Bu modda denemeleri yürütmek için HDInsight kümesine SSH erişmeniz gerekir. 

>[!NOTE]
>HDInsight Spark kümelerinde çalışan Linux (Ubuntu Python/PySpark 3.5.2 ve Spark 2.1.11) desteklenen yapılandırmadır.

_**Bir PySpark betiği HDInsight tabanlı yürütülmek genel bakış**_
![](media/experimentation-service-configuration/hdinsight-run.png)


## <a name="running-a-script-on-gpu"></a>GPU üzerinde betik çalıştırma
GPU üzerinde komut çalıştırmak için bu makaledeki yönergeleri izleyebilirsiniz:[Azure Machine Learning'de GPU kullanma](how-to-use-gpu.md)

## <a name="using-ssh-key-based-authentication-for-creating-and-using-compute-targets"></a>Oluşturup kullanarak işlem hedefleri için SSH anahtar tabanlı kimlik doğrulaması kullanma
Azure Machine Learning Workbench, oluşturma ve SSH anahtar tabanlı kimlik doğrulamanın yanı sıra kullanıcı adı/parola tabanlı Şeması'nı kullanarak işlem hedeflerini kullanmanıza olanak sağlar. Remotedocker veya küme işlem hedef kullandığınızda, bu özelliği kullanabilirsiniz. Bu düzenin kullandığınızda, Workbench bir ortak/özel anahtar çifti oluşturur ve ortak anahtarı geri bildirir. Ortak anahtar için kullanıcı adınızı ~/.ssh/authorized_keys dosyaları ekleyin. Azure Machine Learning Workbench ardından kullanan ssh erişmek ve bu işlem hedef üzerinde yürütülen anahtar tabanlı kimlik doğrulaması. Çalışma alanı için bir anahtar deposunda özel anahtarı işlem hedefi için kaydettikten sonra diğer kullanıcıların çalışma alanının işlem hedef işlem hedefi oluşturmak için sağlanan kullanıcı adını sağlayarak aynı şekilde kullanabilirsiniz.  

Bu işlevselliği kullanmak için aşağıdaki adımları izleyin. 

- Aşağıdaki komutlardan birini kullanarak işlem hedefi oluşturmak.

```
az ml computetarget attach remotedocker --name "remotevm" --address "remotevm_IP_address" --username "sshuser" --use-azureml-ssh-key
```
or
```
az ml computetarget attach remotedocker --name "remotevm" --address "remotevm_IP_address" --username "sshuser" -k
```
- İliştirilmiş işlem hedef ~/.ssh/authorized_keys dosya Workbench tarafından oluşturulan ortak anahtarı ekleyin. 

>[!IMPORTANT]
>İşlem hedef işlem hedefi oluşturmak için kullanılan aynı kullanıcı adı kullanarak bağlanmanız gerekir. 

- Artık, hazırlama ve SSH anahtar tabanlı kimlik doğrulaması kullanarak işlem hedefini kullanabilirsiniz.

```
az ml experiment prepare -c remotevm
```

## <a name="next-steps"></a>Sonraki adımlar
* [Oluşturma ve Azure Machine Learning'i yükleme](../service/quickstart-installation.md)
* [Model Yönetimi](model-management-overview.md)
