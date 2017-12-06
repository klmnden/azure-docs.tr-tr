---
title: "Azure Machine Learning deneme hizmeti yapılandırma | Microsoft Docs"
description: "Bu makalede nasıl yapılandırılacağı hakkında yönergeler ile üst düzey bir genel bakış Azure Machine Learning deneme hizmeti sağlar."
services: machine-learning
author: gokhanuluderya-msft
ms.author: gokhanu
manager: haining
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 09/28/2017
ms.openlocfilehash: 26ab8f9ab561cc218f3dcb249741a96d8f14c579
ms.sourcegitcommit: a48e503fce6d51c7915dd23b4de14a91dd0337d8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2017
---
# <a name="configuring-azure-machine-learning-experimentation-service"></a>Azure Machine Learning deneme hizmetini yapılandırma

## <a name="overview"></a>Genel Bakış
Azure Machine Learning deneme hizmeti kullanılarak Azure Machine Learning yürütme kendi denemeler yürütün ve yönetim özellikleri çalıştırmak veri bilimcilerine sağlar. Hızlı yineleme ile Çevik deneme için bir çerçeve sağlar. Azure Machine Learning çalışma ekranı makinenizde başlamak yerel çalıştırmalar sağlar ve uzak veri bilimi VM'ler GPU veya Hdınsight Spark çalıştıran kümeler gibi diğer ortamlara yukarı ve aşağı doğru ölçeklendirme için kolay bir yol sağlar.

Deneme hizmeti denemelerinizi yalıtılmış, yeniden üretilebilir ve tutarlı çalıştırılan sağlamak için oluşturulmuştur. Yürütme Ortamı, işlem hedeflerini yönetme ve çalıştırma yapılandırmalarında yardımcı olur. Azure Machine Learning çalışma ekranı yürütme ve çalıştırma yönetimi yeteneklerini kullanarak, farklı ortamlar arasında kolayca geçiş yapabilirsiniz. 

Yerel olarak veya bulutta ölçekte çalışma ekranı projede Python veya PySpark komut dosyası çalıştırabilirsiniz. 

Komut dosyalarınızı komutunu çalıştırabilirsiniz: 

* Python (3.5.2'de) ortamı, yerel bilgisayarınızda çalışma ekranı tarafından yüklenmiş.
* Yerel bilgisayardaki bir Docker kapsayıcısı içinde Conda Python ortamı
* Bir uzak Linux makinesinde Docker kapsayıcısı içinde Conda Python ortamı. Örneğin, bir [Ubuntu tabanlı DSVM Azure ile ilgili](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-ads.linux-data-science-vm-ubuntu)
* [Hdınsight için Spark](https://azure.microsoft.com/services/hdinsight/apache-spark/) Azure ile ilgili

>[!IMPORTANT]
>Azure Machine Learning deneme hizmeti şu anda Python 3.5.2'de ve Spark 2.1.11 Python ve Spark çalışma zamanı sürümleri sırasıyla destekler. 


### <a name="key-concepts-in-experimentation-service"></a>Deneme hizmetindeki temel kavramları
Azure Machine Learning deneme yürütmesinde aşağıdaki kavramları anlamanız önemlidir. Sonraki bölümlerde ayrıntılı olarak bu kavramlar kullanmayı tartışın. 

#### <a name="compute-target"></a>Hedef işlem
A _hedef işlem_ programınızı Masaüstü, uzak Docker gibi bir VM veya bir küme üzerinde çalıştırmak konumu belirtir. Bir işlem hedef adreslenebilir ve sizin tarafınızdan erişilebilir olması gerekir. Çalışma ekranı işlem hedefleri oluşturma ve bunları çalışma ekranı uygulama ve CLI kullanarak yönetme olanağı sağlar. 

_az ml computetarget attach_ CLI komutta çalışmalarınız kullanabileceğiniz bir işlem hedef oluşturmanıza olanak sağlar.

Desteklenen işlem hedefleri şunlardır:
* Çalışma ekranı tarafından yüklü bilgisayarınızdaki yerel Python (3.5.2'de) ortamı.
* Bilgisayarınızda yerel Docker
* Uzak Docker Ubuntu Linux sanal makineleri üzerinde. Örneğin, bir [Ubuntu tabanlı DSVM Azure ile ilgili](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-ads.linux-data-science-vm-ubuntu)
* [Hdınsight Spark kümesi için](https://azure.microsoft.com/services/hdinsight/apache-spark/) Azure ile ilgili

Deneme hizmeti şu anda Python 3.5.2'de destekler ve Python olarak 2.1.11 Spark ve çalışma zamanı sürümlerini sırasıyla Spark. 

>[!IMPORTANT]
> Docker olan Windows VM'ler çalışan **değil** uzak işlem hedefleri olarak desteklenir.

#### <a name="execution-environment"></a>Yürütme Ortamı
_Yürütme ortamı_ çalıştırma yapılandırmasını ve çalışma ekranı içinde programı çalıştırmak için gerekli bağımlılıkların tanımlar.

Çalışma ekranı varsayılan çalışma zamanı üzerinde çalıştırıyorsanız, sık kullandığınız araçları ve paket yöneticileri kullanarak yerel yürütme ortamı yönetin. 

Conda Hdınsight tabanlı yürütmeleri yanı sıra yerel Docker ve uzak Docker yürütmeleri yönetmek için kullanılır. Bu hedefleri işlem için yürütme ortamı yapılandırması üzerinden yönetilir **Conda_dependencies.yml** ve **Spark_dependencies.yml dosyaları**. Bu dosyaları **aml_config** projenizi bir klasöre.

**Yürütme ortamlar için desteklenen çalışma zamanları şunlardır:**
* Python 3.5.2'de
* Spark 2.1.11

### <a name="run-configuration"></a>Çalışma yapılandırması
İşlem hedef ve yürütme ortamı yanı sıra, Azure Machine Learning tanımlayın ve değiştirmek için bir çerçeve sağlar *yapılandırmaları çalıştırma*. Denemenizi yürütmeleri yinelemeli deneme bir parçası olarak farklı yapılandırma gerekebilir. Farklı parametre aralıkları, farklı veri kaynakları kullanarak ve spark parametreleri ayarlama yerleştirmez. Deneme hizmeti çalıştırma yapılandırmaları yönetmek için bir çerçeve sağlar.

Çalışan _az ml computetarget attach_ komutu, iki dosyada üretir, **aml_config** projenizdeki klasöre: bir .compute ve bu kural aşağıdaki .runconfig: _< your_ computetarget_name > .compute_ ve _< your_computetarget_name > .runconfig_. Bir işlem hedef oluşturduğunuzda .runconfig dosyası size kolaylık olması için otomatik olarak oluşturulur. Oluşturma ve çalıştırma diğer yapılandırmaları kullanarak yönetme _az ml runconfigurations_ CLI komutu. Ayrıca, oluşturabilir ve bunları, dosya sisteminde düzenleyebilirsiniz.

Çalışma ekranı çalıştırma yapılandırmasında ortam değişkenlerini belirtmenize olanak sağlar. Ortam değişkenleri belirtin ve bunları .runconfig dosyanızda aşağıdaki bölümde ekleyerek kodunuzda kullanabilirsiniz. 

```
EnvironmentVariables:
"EXAMPLE_ENV_VAR1": "Example Value1"
"EXAMPLE_ENV_VAR2": "Example Value2"
```

Bu ortam değişkenleri kodunuzda erişilebilir. Örneğin, "EXAMPLE_ENV_VAR1" adlı ortam değişkeni bu phyton kod parçacığını yazdırır
```
print(os.environ.get("EXAMPLE_ENV_VAR1"))
```

_**Aşağıdaki şekilde ilk deneme çalıştırmak için üst düzey akışı gösterilmektedir.**_
![](media/experimentation-service-configuration/experiment-execution-flow.png)

## <a name="experiment-execution-scenarios"></a>Deneme yürütme senaryoları
Bu bölümde, yürütme senaryolarına daha yakından inceleyin ve Azure Machine Learning denemeleri, özellikle bir denemeyi yerel olarak Hdınsight kümesi ve uzaktan bir VM üzerinde çalışan çalışma şeklini hakkında bilgi edinin. Bu bölüm denemelerinizi yürütülmesi için bir işlem hedefi oluşturma başlangıç bir kılavuz değildir.

>[!NOTE]
>Bu makalede geri kalanı için kavramlarını ve özelliklerini göstermek için CLI (komut satırı arabirimi) komutları kullanıyoruz. Burada açıklanan özellikleri çalışma ekranından de kullanılabilir.

## <a name="launching-the-cli"></a>CLI başlatma
Çalışma ekranı ve giderek bir proje CLI başlatmak için kolay bir yol açıyor **Dosya--> komut istemini açın**.

![](media/experimentation-service-configuration/opening-cli.png)

Bu komut, komut dosyaları geçerli proje klasöründe yürütülecek komut girebilirsiniz bir terminal penceresi başlatır. Bu bir terminal penceresi, çalışma ekranı tarafından yüklenen Python 3.5.2'de ortamı ile yapılandırılır.

>[!NOTE]
> Çalıştırdığınızda herhangi _az ml_ komutu komut penceresinde karşı Azure kimliğinizin doğrulanması gerekiyor. Bir bağımsız kimlik doğrulama önbelleğini sonra masaüstü uygulaması CLI kullanır ve bu nedenle çalışma ekranına oturum açmayı CLI ortamınızda doğrulanır anlamına gelmez. Kimlik doğrulaması yapmak için aşağıdaki adımları izleyin. Yalnızca belirtecin süresi dolduğunda bu adımları yinelemeniz gerekir böylece kimlik doğrulama belirteci yerel olarak bir süre önbelleğe alınır. Belirtecin süresi dolduğunda veya kimlik doğrulama hataları görüyorsanız, aşağıdaki komutları çalıştırın:

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
>Çalıştırdığınızda _az ml_ komutu proje klasör içinde proje üzerinde bir Azure Machine Learning deneme hesabına ait olduğundan emin olun _geçerli_ Azure aboneliği. Aksi takdirde yürütme hatalarla karşılaşabilirsiniz.


## <a name="running-scripts-and-experiments"></a>Komut dosyaları ve denemeler çalıştıran
Çalışma ekranı ile Python yürütebilir ve çeşitli PySpark betikleri kullanarak hedeflerini işlem _az ml deneme gönderme_ komutu. Bu komut, bir çalışma yapılandırma tanımı gerekiyor. 

Çalışma ekranı oluşturur karşılık gelen bir .runconfig dosyası bir işlem hedef oluşturmak, ancak kullanarak ek çalışma yapılandırmaları oluşturabilirsiniz _az ml runconfiguration oluşturma_ komutu. Çalışma yapılandırma dosyalarını el ile de düzenleyebilirsiniz.

İçinde çalışma ekranı deneyimi deneme parçası çalışacak şekilde çalıştırma yapılandırmaları gösterin. 

>[!NOTE]
>Çalışma yapılandırma dosyasında hakkında daha fazla bilgiyi [deneme yürütme yapılandırma başvurusu](experimentation-service-configuration-reference.md) bölümü.

## <a name="running-a-script-locally-on-workbench-installed-runtime"></a>Betik çalışma zamanında çalışma ekranı yüklü yerel olarak çalıştırma
Çalışma ekranı doğrudan çalışma ekranı yüklü Python 3.5.2'de çalışma zamanı karşı komut dosyaları çalıştırmanıza olanak sağlar. Bu varsayılan çalışma zamanı, çalışma ekranı Kurulum aynı anda yüklenir ve Azure Machine Learning kitaplıkları ve bağımlılıklar içerir. Çalıştırma sonuçları ve yapıları yerel yürütmeleri için hala çalıştırma geçmişi hizmet olarak bulutta kaydedilir.

Docker tabanlı yürütmeleri bu yapılandırmadır _değil_ Conda tarafından yönetilir. Yerel çalışma ekranı Python ortamınız için paket bağımlılıkları el ile sağlamak gerekir.

Komut çalışma ekranı yüklü Python ortamında yerel olarak çalıştırmak için aşağıdaki komutu çalıştırabilirsiniz. 

```
$az ml experiment submit -c local myscript.py
```

Varsayılan Python ortamı yoluna çalışma ekranı CLI penceresinde aşağıdaki komutu yazarak bulabilirsiniz.
```
$ conda env list
```

>[!NOTE]
>Yerel olarak PySpark ortamları doğrudan yerel Spark karşı çalışan şu anda **değil** desteklenir. Çalışma ekranı yerel Docker üzerinde çalışan PySpark komut desteklemiyor. Azure Machine Learning temel Docker görüntü önceden yüklenmiş 2.1.11 Spark ile birlikte gelir. 

_**Yerel yürütme bir Python komut dosyası için genel bakış:**_
![](media/experimentation-service-configuration/local-native-run.png)

## <a name="running-a-script-on-local-docker"></a>Betik üzerinde yerel Docker çalıştırma
Ayrıca projelerinizi Docker kapsayıcısı üzerinde yerel makinenizde deneme hizmeti aracılığıyla çalıştırabilirsiniz. Çalışma ekranı sağlar ve de Azure Machine Learning kitaplıkları ile gelir temel bir Docker görüntü yerel Spark yürütmeleri kolaylaştırmak için Spark 2.1.11 çalışma zamanı olarak. Docker zaten yerel makinede çalışıyor olması gerekir.

Python veya PySpark betik üzerinde yerel Docker çalıştırmak için CLI aşağıdaki komutları çalıştırabilirsiniz.

```
$az ml experiment submit -c docker myscript.py
```
or
```
az ml experiment submit --run-configuration docker myscript.py
```

Azure Machine Learning temel Docker görüntü kullanarak yerel Docker yürütme ortamda hazırlanır. Çalışma ekranı ilk zaman ve yer paylaşımları için conda_dependencies.yml dosyasında belirtilen paket ile çalıştırıldığında, bu görüntüyü yükler. Bu işlem ilk daha yavaş çalışma yapar ancak sonraki çalışmalarını önemli ölçüde önbelleğe alınmış katmanları yeniden çalışma ekranı sayesinde hızlıdır. 

>[!IMPORTANT]
>Çalıştırmanız gereken _az ml deneme hazırlama - c docker_ önce ilk çalıştırma için Docker görüntüsünü hazırlamak için komutu. Ayrıca ayarlayabilirsiniz **PrepareEnvironment** parametresini docker.runconfig dosyanızdaki true. Bu eylem, ortamınızın çalışma yürütme bir parçası olarak otomatik olarak hazırlar.  

>[!NOTE]
>Bir PySpark betik Spark üzerinde çalışıyorsa, spark_dependencies.yml de conda_dependencies.yml ek olarak kullanılır.

Komut dosyalarınızı Docker görüntüde çalıştıran, aşağıdaki avantajları sunar:

1. Kodunuzu diğer yürütme ortamlarda güvenilir bir şekilde yürütülebilecek sağlar. Docker kapsayıcısı üzerinde çalışan bulmak ve taşınabilirlik etkileyebilecek herhangi bir yerel başvurusu önlemenize yardımcı olur. 

2. Hızlı bir şekilde kodu çalışma zamanları ve yükleme ve yapılandırma, Apache Spark gibi kendiniz yüklemek zorunda kalmadan karmaşık çerçeveler test imkan tanır.


_**Yerel Docker yürütme bir Python komut dosyası için genel bakış:**_
![](media/experimentation-service-configuration/local-docker-run.png)

## <a name="running-a-script-on-a-remote-docker"></a>Uzak bir Docker betik çalıştırma
Bazı durumlarda, yerel makinenizde kullanılabilir kaynakları istenen modeli eğitmek için yeterli olmayabilir. Bu durumda, deneme hizmeti kullanılarak uzaktan Docker yürütme daha güçlü Vm'lerinde, Python veya PySpark komut dosyalarını çalıştırmak kolay bir yol sağlar. 

Uzak VM aşağıdaki gereksinimleri karşılaması gerekir:
* Uzak VM Ubuntu Linux çalıştırılması gerekiyor ve SSH erişilebilir olması gerekir. 
* Uzak VM çalıştıran Docker olmalıdır.

>[!IMPORTANT]
> Windows sanal makineleri çalıştığı Docker **değil** uzak işlem hedefleri olarak desteklenen


Her iki işlem hedef tanımı oluşturun ve uzak Docker tabanlı yürütmeleri için yapılandırma çalıştırmak için aşağıdaki komutu kullanabilirsiniz.

```
az ml computetarget attach remotedocker --name "remotevm" --address "remotevm_IP_address" --username "sshuser" --password "sshpassword" 
```

İşlem hedef yapılandırdıktan sonra kodunuzu çalıştırmak için aşağıdaki komutu kullanabilirsiniz.
```
$ az ml experiment submit -c remotevm myscript.py
```
>[!NOTE]
>Bu yürütme göz önünde bulundurmanız ortam içinde conda_dependencies.yml belirtimlerini kullanarak yapılandırılır. PySpark framework .runconfig dosyasında belirtilmişse spark_dependencies.yml de kullanılır. 

İşlem için yerel Docker çalışır, benzer bir yürütme deneyimi beklemeniz gerekir böylece Uzaktan VM'ler için Docker oluşturma işlemi tam olarak aynıdır.

>[!TIP]
>İlk çalıştırma için Docker görüntü oluşturma tarafından sunulan gecikme önlemek tercih ederseniz, komut yürütülmeden önce işlem hedef hazırlamak için aşağıdaki komutu kullanabilirsiniz. az ml deneme - c remotedocker hazırlama


_**Uzak vm yürütme bir Python komut dosyası için genel bakış:**_
![](media/experimentation-service-configuration/remote-vm-run.png)


## <a name="running-a-script-on-an-hdinsight-cluster"></a>Bir Hdınsight kümesine betik çalıştırma
Hdınsight Apache Spark destekleyen büyük veri analizi için popüler bir platformdur. Çalışma ekranı Hdınsight Spark kümeleri kullanarak büyük veri üzerinde deneme sağlar. 

>![NOT] HDInsight kümesi birincil depolama olarak Azure Blob kullanmalıdır. Azure Data Lake depolamanın kullanılması henüz desteklenmemektedir.

Bir işlem hedef oluşturun ve aşağıdaki komutu kullanarak Hdınsight Spark kümesinde yapılandırmanın çalıştırın:

```
$ az ml computetarget attach cluster --name "myhdi" --address "<FQDN or IP address>" --username "sshuser" --password "sshpassword"  
```

>[!NOTE]
>Bir IP adresi yerine FQDN kullanıyorsanız ve HDI Spark kümenizin adlı _foo_, SSH uç noktası adlı sürücü düğümde olan _foo-ssh.azurehdinsight.net_. Unutmayın **-ssh** için FQDN kullanırken sunucu adı sonek _--adres_ parametresi.


İşlem bağlamı olduktan sonra PySpark kodunuzu çalıştırmak için aşağıdaki komutu çalıştırabilirsiniz.

```
$ az ml experiment submit -c myhdi myscript.py
```

Çalışma ekranı hazırlar ve yürütme ortamı Conda kullanarak Hdınsight kümesinde yönetir. Yapılandırma tarafından yönetilir _conda_dependencies.yml_ ve _spark_dependencies.yml_ yapılandırma dosyaları. 

Bu modda denemeler yürütmek için Hdınsight kümesi için SSH erişmeniz gerekir. 

>[!NOTE]
>Çalışan Linux (Python/PySpark 3.5.2'de ve Spark 2.1.11 Ubuntu) Hdınsight Spark kümeleri desteklenen yapılandırmadır.

_**Hdınsight tabanlı yürütme PySpark komut dosyası için genel bakış**_
![](media/experimentation-service-configuration/hdinsight-run.png)


## <a name="running-a-script-on-gpu"></a>GPU üzerinde bir komut dosyası çalıştırma
Komut dosyalarınızı GPU üzerinde çalıştırmak için bu makaledeki yönergeleri takip edebilirsiniz:[GPU Azure Machine Learning ile kullanma](how-to-use-gpu.md)

## <a name="using-ssh-key-based-authentication-for-creating-and-using-compute-targets"></a>Oluşturma ve işlem hedefleri kullanmak için SSH anahtarı tabanlı kimlik doğrulama kullanılarak
Azure Machine Learning çalışma ekranı oluşturmak ve SSH anahtarı tabanlı kimlik doğrulaması kullanıcı adı/parola tabanlı düzeni yanı sıra kullanarak işlem hedeflerini kullanmanıza olanak sağlar. Remotedocker veya küme işlem hedef olarak kullanırken, bu özelliği kullanabilirsiniz. Bu düzen kullandığınızda, çalışma ekranı ortak/özel anahtar çifti oluşturur ve ortak anahtar geri raporlar. Ortak anahtar için kullanıcı adınızı ~/.ssh/authorized_keys dosyaları ekleyin. Azure Machine Learning çalışma ekranı sonra kullanır ssh erişmek ve bu işlem hedefte yürütme anahtar tabanlı kimlik doğrulaması. Çalışma alanı için bir anahtar deposunda işlem hedef özel anahtarı kaydedildikten sonra diğer kullanıcıların çalışma alanının işlem hedef işlem hedef oluşturmak için sağlanan kullanıcı adını sağlayarak aynı şekilde kullanabilirsiniz.  

Bu işlevselliği kullanmak için aşağıdaki adımları izleyin. 

- Aşağıdaki komutlardan birini kullanarak bir işlem hedef oluşturun.

```
az ml computetarget attach remotedocker --name "remotevm" --address "remotevm_IP_address" --username "sshuser" --use-azureml-ssh-key
```
or
```
az ml computetarget attach remotedocker --name "remotevm" --address "remotevm_IP_address" --username "sshuser" -k
```
- Ekli işlem hedefte ~/.ssh/authorized_keys dosyaya çalışma ekranı tarafından oluşturulan ortak anahtarı ekleyin. 

[!IMPORTANT] İşlem hedef oluşturmak için kullanılan aynı kullanıcı adı kullanarak işlem hedef bağlanmanız gerekir. 

- Şimdi, hazırlamak ve SSH anahtar tabanlı kimlik doğrulaması kullanarak işlem hedef kullanın.

```
az ml experiment prepare -c remotevm
```

## <a name="next-steps"></a>Sonraki adımlar
* [Oluşturun ve Azure Machine Learning yükleyin](quickstart-installation.md)
* [Model Yönetimi](model-management-overview.md)
