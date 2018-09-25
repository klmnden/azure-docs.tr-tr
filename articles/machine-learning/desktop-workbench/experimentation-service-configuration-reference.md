---
title: Azure Machine Learning deneme hizmeti yapılandırma dosyaları
description: Bu belge, Azure ML deneme hizmeti için yapılandırma ayarlarını ayrıntıları.
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
ROBOTS: NOINDEX
ms.openlocfilehash: abce80528479ba180783dbab604d4c836ddb7f1e
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46950786"
---
# <a name="azure-machine-learning-experimentation-service-configuration-files"></a>Azure Machine Learning deneme hizmeti yapılandırma dosyaları

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)]

Bir betiği Azure Machine Learning (Azure ML) Workbench'te çalıştırdığınızda, yürütme davranışını dosyalarında denetlenir **aml_config** klasör. Bu klasör proje klasörü kökünde bulunur. İstenen sonuca, yürütme için iyi bir şekilde elde etmek için bu dosyaların içeriğini anlamak önemlidir.

Bu klasör altındaki ilgili dosyalar şunlardır:
- conda_dependencies.yml
- spark_dependencies.yml
- Hedef dosya işlem
    - \<Hedef adı işlem > .compute
- yapılandırma dosyalarını çalıştır
    - \<Yapılandırma adı çalıştırın > .runconfig

>[!NOTE]
>Genellikle işlem hedef dosya varsa ve oluşturduğunuz her işlem hedefi için yapılandırma dosyasını çalıştırın. Ancak, bağımsız olarak bu dosyaların oluşturulması ve birden fazla çalıştırma yapılandırma dosyası aynı işlem hedefe işaret eden sahip.

## <a name="condadependenciesyml"></a>conda_dependencies.yml
Bu dosya bir [conda ortam dosyası](https://conda.io/docs/using/envs.html#create-environment-file-by-hand) kodunuzu bağımlı paketler ve Python çalışma zamanı sürümünü belirtir. Azure ML Workbench bir Docker kapsayıcısı veya HDInsight küme içinde bir kod yürütüldüğünde, oluşturduğu bir [conda ortam](https://conda.io/docs/using/envs.html) çalıştırılacak betiği için. 

Bu dosyada, komut dosyanız için gereken Python paketlerini belirtin. Azure ML deneme hizmeti listenize göre bağımlılıklar conda ortamı oluşturur. Burada listelenen paketleri gibi kanallar aracılığıyla yürütme altyapısı tarafından erişilebilir olmalıdır:

* [continuum.io](https://anaconda.org/conda-forge/repo)
* [PyPI](https://pypi.python.org/pypi)
* Genel olarak erişilebilen bir uç noktaya (URL)
* ya da yerel dosya yolu
* diğerleri yürütme altyapısı tarafından erişilebilir

>[!NOTE]
>HDInsight küme üzerinde çalışan, Azure ML Workbench belirli çalıştırmak için bir conda ortamı oluşturur. Bu, farklı kullanıcılar aynı küme üzerindeki farklı python ortamları çalışmasını sağlar.  

İşte bir örnek tipik bir **conda_dependencies.yml** dosya.
```yaml
name: project_environment
dependencies:
  # Python version
  - python=3.5.2
  
  # some conda packages
  - scikit-learn
  - cryptography
  
  # use pip to install some more packages
  - pip:
     # a package in PyPi
     - azure-storage
     
     # a package hosted in a public URL endpoint
     - https://cntk.ai/PythonWheel/CPU-Only/cntk-2.1-cp35-cp35m-win_amd64.whl
     
     # a wheel file available locally on disk (this only works if you are executing against local Docker target)
     - C:\temp\my_private_python_pkg.whl
```

Azure ML Workbench kullandığı aynı conda ortamı sürece derlenmeden **conda_dependencies.yml** aynı kalır. Bağımlılıklarınızı değiştirirseniz ortamınızı yeniden oluşturur.

>[!NOTE]
>Yürütmeyi hedefliyorsanız _yerel_ işlem bağlamında, **conda_dependencies.yml** dosyasıdır **değil** kullanılır. Azure ML Workbench'i Python ortamınız için Paket bağımlılıklarını elle yüklenmesi gerekir.

## <a name="sparkdependenciesyml"></a>spark_dependencies.yml
Bir PySpark betiği ve yüklenmesi gereken Spark paketleri gönderdiğinizde bu dosya, Spark uygulaması adını belirtir. Ayrıca, bir ortak Maven deposu ve bunun yanı sıra bu Maven depolarında bulunabilir Spark paketlerini de belirtebilirsiniz.

Örnek aşağıda verilmiştir:

```yaml
configuration:
  # Spark application name
  "spark.app.name": "ClassifyingIris"
  
repositories:
  # Maven repository hosted in Azure CDN
  - "https://mmlspark.azureedge.net/maven"
  
  # Maven repository hosted in spark-packages.org
  - "https://spark-packages.org/packages"
  
packages:
  # MMLSpark package hosted in the Azure CDN Maven
  - group: "com.microsoft.ml.spark"
    artifact: "mmlspark_2.11"
    version: "0.5"
    
  # spark-sklearn packaged hosted in the spark-packages.org Maven
  - group: "databricks"
    artifact: "spark-sklearn"
    version: "0.2.0"
```

>[!NOTE]
>Çalışan boyutu ve çekirdek gibi parametreleri ayarlama küme spark_dependecies.yml dosyasındaki "yapılandırma" bölümüne gidin 

>[!NOTE]
>Python ortamında, komut dosyası yürütme, *spark_dependencies.yml* dosya göz ardı edilir. Yalnızca Spark (Docker veya HDInsight kümesindeki ya) karşı çalıştırıyorsanız kullanılır.

## <a name="run-configuration"></a>Çalıştırma yapılandırma
Belirli bir çalıştırma yapılandırma belirtmek için .compute dosyası ve .runconfig dosyası gerekir. Bunlar genellikle bir CLI komutu kullanarak oluşturulur. Ayrıca, olanları çıkmadan kopyalama, yeniden adlandırmak ve düzenleyebilir.

```azurecli
# create a compute target pointing to a VM via SSH
$ az ml computetarget attach remotedocker -n <compute target name> -a <IP address or FQDN of VM> -u <username> -w <password>

# create a compute context pointing to an HDI cluster head-node via SSH
$ az ml computetarget attach cluster -n <compute target name> -a <IP address or FQDN of HDI cluster> -u <username> -w <password> 
```

Bu komut dosyalarının belirtilen işlem hedef bir çift oluşturur. İşlem hedef adlı varsayalım _foo_. Bu komut oluşturur _foo.compute_ ve _foo.runconfig_ içinde **aml_config** klasör.

>[!NOTE]
> _yerel_ veya _docker_ çalıştırma yapılandırma dosyalarının rastgele için adları. Azure ML Workbench, size kolaylık olması için boş bir proje oluşturduğunuzda, bu iki yapılandırmaları Çalıştır ekler. Yeniden adlandırabilir miyim "<run configuration name>.runconfig" dosyaları ile proje şablonu yayımlamayın veya istediğiniz adla yeni bir tane oluşturun.

### <a name="compute-target-namecompute"></a>\<Hedef adı işlem > .compute
_\<Hedef adı işlem > .compute_ bağlantı ve yapılandırma bilgileri için işlem hedef dosyayı belirtir. Ad-değer çifti listesidir. Desteklenen ayarlar şunlardır:

**tür**: işlem ortamını türü. Desteklenen değerler şunlardır:
  - yerel
  - Uzak
  - Docker
  - remotedocker
  - küme

**baseDockerImage**: Python/PySpark betiğini çalıştırmak için kullanılan bir Docker görüntüsü. Varsayılan değer _microsoft/mmlspark:plus-0.7.91_. Ayrıca başka bir görüntü destekliyoruz: _microsoft/mmlspark:plus-gpu-0.7.91_, size sağlayan GPU erişimi ana bilgisayarın (GPU mevcutsa).

**adresi**: IP adresi veya FQDN (tam etki alanı adı) sanal makinesi veya HDInsight küme baş düğüm.

**Kullanıcı adı**: sanal makine ya da HDInsight baş düğüm erişmek için SSH kullanıcı adı.

**Parola**: SSH bağlantısını için şifreli parola.

**sharedVolumes**: Bu yürütme altyapısı, Docker kullanması gereken sinyal bayrağı paylaşılan proje dosyaları sürekli teslim için Toplu özellik. Bu bayrağı açık olan Docker erişip bunları kopyalamak zorunda kalmadan doğrudan projeleri olduğundan yürütme hızlandırabilirsiniz. Ayarlamak en iyi _false_ Windows üzerindeki Docker güvenilir olmayan için birimi paylaşan beri Docker altyapısı Windows üzerinde çalışıp çalışmadığını. Ayarlayın _true_ macOS veya Linux üzerinde çalışıyorsa.

**nvidiaDocker**: Bu bayrak ayarlandığında, _true_, kullanılacak Azure ML deneme hizmeti söyler _NVIDIA docker_ komutu normal aksine _docker_Docker görüntüsünü başlatmak için komutu. _NVIDIA docker_ GPU donanım erişimi için Docker kapsayıcı sağlar. GPU yürütme Docker kapsayıcısında çalıştırmak istiyorsanız gerekli bir ayardır. Yalnızca Linux ana destekler _NVIDIA docker_. Örneğin, azure'da Linux tabanlı DSVM ile birlikte gelen _NVIDIA docker_. _NVIDIA docker_ şu andan itibaren Windows üzerinde desteklenmiyor.

**nativeSharedDirectory**: Bu özellik, temel dizin belirtir (örneğin: _~/.azureml/share/_) dosyaları arasında paylaşılması için kaydedilebileceği aynı işlem hedefi üzerinde çalışır. Bu ayar, bir Docker kapsayıcısı üzerinde çalışırken kullanılıyorsa _sharedVolumes_ ayarlanmalıdır true. Aksi takdirde, yürütme başarısız olur.

**userManagedEnvironment**: Bu özellik bu işlem hedef doğrudan kullanıcı tarafından yönetilen veya deneme hizmeti yönetilen olup olmadığını belirtir.  

**pythonLocation**: Bu özellik işlem hedef kullanıcının programı çalıştırmak için kullanılacak python çalışma zamanını konumunu belirtir. 

### <a name="run-configuration-namerunconfig"></a>\<Yapılandırma adı çalıştırın > .runconfig
_\<Yapılandırma adı çalıştırın > .runconfig_ Azure ML deneme yürütme davranışını belirtir. Çalıştırma geçmişini izleme gibi yürütme davranışını yapılandırabilir veya ne yanı sıra diğer birçok kullanmak için hedef işlem. Çalıştırma yapılandırma dosyalarının adlarını Azure ML Workbench masaüstü uygulamasında yürütme bağlamı açılan listeyi doldurmak için kullanılır.

**ArgumentVector**: Bu bölümde bu yürütme ve betik parametreleri için bir parçası olarak çalıştırılacak betiği belirtir. Örneğin, aşağıdaki kod parçacığı varsa, "<run configuration name>.runconfig" dosyası 

```
 "ArgumentVector":[
  - "myscript.py"
  - 234
  - "-v" 
 ] 
```
_"az ml denemeyi gönderme foo.runconfig"_ komutuyla otomatik olarak çalıştırır _myscript.py_ 234 bir parametre ve kümeleri geçirerek dosya verbose bayrağı.

**Hedef**: Bu parametrenin adıdır _.compute_ dosya _runconfig_ dosya başvuruları. Genel olarak işaret _foo.compute_ dosya ancak düzenleyebilir, farklı işlem hedefe işaret edecek şekilde.

**Ortam değişkenlerini**: Bu bölümde, çalışmalarına bir parçası olarak ortam değişkenlerini ayarlamak kullanıcıların sağlar. Kullanıcı ortam değişkenlerini şu biçimde ad-değer çiftleri kullanarak belirtebilirsiniz:
```
EnvironmentVariables:
  "EXAMPLE_ENV_VAR1": "Example Value1"
  "EXAMPLE_ENV_VAR2": "Example Value2"
```

Bu ortam değişkenleri, kullanıcı kodunda erişilebilir. Örneğin, bu Python kodu "EXAMPLE_ENV_VAR" adlı ortam değişkeni yazdırır
```
print(os.environ.get("EXAMPLE_ENV_VAR1"))
```

**Framework**: Bu özellik, Azure ML Workbench betiği çalıştırmak için bir Spark oturumu açması gereken belirtir. Varsayılan değer _PySpark_. Ayarlayın _Python_ işi daha az ek yük ile daha hızlı başlatma yardımcı olabilecek PySpark kodu çalıştırıyorsanız değil.

**CondaDependenciesFile**: Bu özellik conda ortam bağımlılıkları belirtir bir dosyaya işaret *aml_config* klasör. Varsa kümesine _null_, varsayılan işaret **conda_dependencies.yml** dosya.

**SparkDependenciesFile**: Bu özellik Spark bağımlılıkları belirtir bir dosyaya işaret **aml_config** klasör. Ayarlanmış _null_ varsayılan ve onu işaret varsayılan **spark_dependencies.yml** dosya.

**PrepareEnvironment**: ayarlandığında, bu özellik _true_, ilk çalıştırma bir parçası olarak belirtilen conda bağımlılıklarını göre conda ortamı hazırlama deneme hizmeti söyler. Bu özellik etkin ise yalnızca, bir Docker ortamında yürütme. Karşı çalıştırıyorsanız, bu ayarın hiçbir etkisi bir _yerel_ ortam. 

**TrackedRun**: Bu bayrağı deneme hizmeti Azure ML Workbench çalıştırma geçmişi altyapı çalıştırmasında izlemek gerekip gerekmediğini gösterir. Varsayılan değer _true_. 

**UseSampling**: _UseSampling_ veri kaynakları için etkin örnek veri kümelerini çalıştırmak için kullanılıp kullanılmayacağını belirtir. Varsa kümesine _false_, veri kaynakları alma ve veri deposundan okuma tam verileri kullanabilirsiniz. Varsa kümesine _true_, etkin örnekleri kullanılır. Kullanıcılar **DataSourceSettings** etkin örnek geçersiz kılmak isterseniz kullanmak için hangi belirli örnek veri kümesi belirtmek için. 

**DataSourceSettings**: Bu yapılandırma bölümü, veri kaynağı ayarlarını belirtir. Bu bölümde, kullanıcı, belirli bir veri kaynağı için mevcut hangi veri örneği çalıştırma bir parçası olarak kullanıldığını belirtir. 

Şu yapılandırma ayarı "MySample" adlı bu örnek "gelen veriKaynağım ' a" adlı bir veri kaynağı için kullanıldığını belirtir
```
DataSourceSettings:
    MyDataSource.dsource:
    Sampling:
    Sample: MySample
```

**DataSourceSubstitutions**: kullanıcı bir veri kaynağından diğerine kendi kodunda değişiklik yapmadan geçin istediğinde, veri kaynağı değişimler kullanılabilir. Örneğin, kullanıcılar örneklenen aşağı, yerel bir dosyadan veri kaynağı başvurusu değiştirerek Azure Blobu'nda depolanan özgün, büyük veri kümesine geçiş yapabilirsiniz. Bir değiştirme'kullanıldığında, Azure ML Workbench veri kaynakları ve veri hazırlık paketlerini, alternatif veri kaynağı başvurarak çalıştırır.

Aşağıdaki örnekte Azure ML veri kaynakları ve veri hazırlık paketlerini "mylocal.datasource" başvuru "myremote.dsource" ile değiştirir. 
 
```
DataSourceSubstitutions:
    mylocal.dsource: myremote.dsource
```

Yukarıdaki değiştirme bağlı olarak, aşağıdaki kod örneği artık "myremote.dsource" "mylocal.dsource yerine" kullanıcılar kendi kodunda değişiklik okur.
```
df = datasource.load_datasource('mylocal.dsource')
```
## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi edinin [deneme hizmeti yapılandırması](experimentation-service-configuration.md).
