---
title: Oluşturma ve model yönetimi için işlem hedeflerini kullan
titleSuffix: Azure Machine Learning service
description: (Hedef işlem) eğitim ortamları için makine öğrenme modeli eğitimi yapılandırın. Eğitim ortamlar arasında kolayca geçiş yapabilirsiniz. Eğitim yerel olarak başlatın. Ölçeği genişletmek gerekiyorsa, bir bulut tabanlı bir işlem hedefine geçiş yapın.
services: machine-learning
author: heatherbshapiro
ms.author: hshapiro
ms.reviewer: sgilley
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.date: 01/07/2019
ms.custom: seodec18
ms.openlocfilehash: 3edc1c2bd328cd6e7b7991ff2b5438b8899a0ce7
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66160474"
---
# <a name="set-up-compute-targets-for-model-training"></a>İşlem hedeflerine yönelik model eğitiminin ayarlama 

Azure Machine Learning hizmeti ile kaynakları veya ortamlar için olarak anılan, çeşitli modelinizi eğitmek [ __hedefleri işlem__](concept-azure-machine-learning-architecture.md#compute-target). İşlem hedefi, bir yerel makineye veya bir Azure Machine Learning işlem, Azure HDInsight veya uzak bir sanal makine gibi bir bulut kaynağı olabilir.  Model dağıtımı için işlem hedefleri açıklandığı gibi oluşturabilirsiniz ["nerede ve nasıl Modellerinizi dağıtmak"](how-to-deploy-and-where.md).

Oluşturun ve Azure Machine Learning SDK'sı, Azure portal veya Azure CLI kullanarak bir işlem hedefine yönetin. Başka bir hizmete (örneğin, bir HDInsight kümesi) oluşturulan işlem hedefleri varsa, Azure Machine Learning hizmeti çalışma alanınıza ekleyerek kullanabilirsiniz.
 
Bu makalede, model yönetimi için çeşitli bilgisayar hedefine kullanmayı öğrenin.  Aynı iş akışının tüm işlem hedeflerine yönelik adımları izleyin:
1. __Oluşturma__ zaten yoksa, işlem hedefi.
2. __Ekleme__ çalışma alanınıza işlem hedefi.
3. __Yapılandırma__ işlem hedef böylece betiğinizin tarafından gereken Python ortamını ve paket bağımlılıkları içerir.


>[!NOTE]
> Bu makalede kod, Azure Machine Learning SDK 1.0.6 sürümünü ile test edilmiştir.

## <a name="compute-targets-for-training"></a>Eğitim hedefleri işlem

Azure Machine Learning hizmeti farklı işlem hedef arasında değişen desteğe sahiptir. Az miktarda veriniz üzerinde dev/deneme ile tipik model geliştirme yaşam döngüsü başlatır. Bu aşamada, yerel bir ortamı kullanmanızı öneririz. Örneğin, yerel bilgisayarınıza veya bulut tabanlı bir VM. Büyük veri kümeleri üzerinde eğitim ölçeğini veya dağıtılmış eğitimi yapmak gibi bir Farklı Çalıştır gönderdiğiniz her zaman bu daralttığında tek veya çok node küme oluşturmak için Azure Machine Learning işlem kullanmanızı öneririz. Çeşitli senaryolarda olarak değişiklik gösterebilir destek aşağıda ayrıntılarıyla olsa da, kendi işlem kaynağı ekleyebilirsiniz:


|Eğitim için hedef işlem| GPU hızlandırma | Otomatik<br/> Hiper parametre ayarı | Otomatik<br/> makine öğrenimi | Azure Machine Learning işlem hatlarını |
|----|:----:|:----:|:----:|:----:|
|[Yerel bilgisayar](#local)| Belki de | &nbsp; | ✓ | &nbsp; |
|[Azure Machine Learning işlem](#amlcompute)| ✓ | ✓ | ✓ | ✓ |
|[Uzak VM](#vm) | ✓ | ✓ | ✓ | ✓ |
|[Azure Databricks](how-to-create-your-first-pipeline.md#databricks)| &nbsp; | &nbsp; | ✓ | ✓ |
|[Azure Data Lake Analytics'i](how-to-create-your-first-pipeline.md#adla)| &nbsp; | &nbsp; | &nbsp; | ✓ |
|[Azure HDInsight](#hdinsight)| &nbsp; | &nbsp; | &nbsp; | ✓ |
|[Azure Batch](#azbatch)| &nbsp; | &nbsp; | &nbsp; | ✓ |

**Tüm hedefler için birden fazla eğitim işleri yeniden kullanılabilir işlem**. Örneğin, uzak bir VM çalışma alanınıza eklediğiniz sonra birden çok iş için kullanabilirsiniz.

> [!NOTE]
> Azure Machine Learning işlem oluşturulabilir kalıcı bir kaynak olarak veya bir farklı çalıştır istediğinizde dinamik olarak oluşturulur. Bu şekilde oluşturulan işlem hedefleri yeniden kullanmak için eğitim çalıştırma tamamlandıktan sonra işlem hedef çalışma tabanlı olarak oluşturulmasını kaldırır.

## <a name="whats-a-run-configuration"></a>Bir çalıştırma yapılandırma nedir?

Eğitim, yerel bilgisayarınızda başlatmak ve daha sonra farklı işlem hedefte Bu eğitim betiğini çalıştırmak için yaygındır. Azure Machine Learning hizmeti ile kodunuzu değiştirmek zorunda kalmadan kodunuzu çeşitli işlem hedefler üzerinde çalıştırabilirsiniz. 

Tek yapmak için ihtiyacınız olan ortam için her işlem hedefini tanımlayan bir **çalıştırma yapılandırmasını**.  Ardından, farklı işlem hedefte eğitim denemenizi çalıştırmak istediğinizde, bu işlem için çalışma yapılandırması belirtin. 

Daha fazla bilgi edinin [denemeleri gönderme](#submit) bu makalenin sonunda.

### <a name="manage-environment-and-dependencies"></a>Ortamı ve bağımlılıkları yönetin

Bir çalıştırma yapılandırması oluşturduğunuzda, işlem hedef bağımlılıkları ve ortamı yönetmek nasıl karar vermeniz gerekir. 

#### <a name="system-managed-environment"></a>Sistem tarafından yönetilen ortamı

Sistem tarafından yönetilen bir ortamda istediğiniz zaman kullanmak [Conda](https://conda.io/docs/) Python ortamı ve kod bağımlılıkları, yönetilecek. Sistem tarafından yönetilen bir ortamda, varsayılan ve en yaygın seçimi tarafından kabul edilir. Özellikle hedefleyen yapılandıramazsınız, uzak işlem hedeflerde yararlıdır. 

Tek yapmak için ihtiyacınız olan her paket bağımlılık kullanarak belirttiğiniz [CondaDependency sınıfı](https://docs.microsoft.com/python/api/azureml-core/azureml.core.conda_dependencies.condadependencies?view=azure-ml-py) ardından Conda adlı bir dosya oluşturur **conda_dependencies.yml** içinde **aml_config** Paket bağımlılıklarını ve eğitim denemenizi gönderdiğinizde Python ortamınızı kümeleri listesi ile çalışma dizini. 

Yeni bir ortam ilk kurulumu gerekli bağımlılıkları boyutuna bağlı olarak birkaç dakika sürebilir. Kurulum süresi, paketlerin listesi değişmeden kaldığı sürece, yalnızca bir kez gerçekleşir.
  
Aşağıdaki kod örneği scikit gerektiren sistem tarafından yönetilen bir ortamda gösterir-öğrenin:
    
[!code-python[](~/aml-sdk-samples/ignore/doc-qa/how-to-set-up-training-targets/runconfig.py?name=run_system_managed)]

#### <a name="user-managed-environment"></a>Kullanıcı tarafından yönetilen ortamı

Kullanıcı tarafından yönetilen bir ortam için ortamınızı ayarlarken ve işlem hedef eğitim betiğinizi gereken her paket yükleme için sorumlu olursunuz. Eğitim ortamınızı zaten yapılandırılmışsa (gibi yerel makinenizde), ayarlayarak Kurulum adımı atlayabilirsiniz `user_managed_dependencies` true. Conda ortamınızı denetlemez ve her şeyi sizin için yükler.

Aşağıdaki kod, kullanıcı tarafından yönetilen bir ortamda eğitim çalıştırmalarının yapılandırma örneği gösterilmektedir:

[!code-python[](~/aml-sdk-samples/ignore/doc-qa/how-to-set-up-training-targets/runconfig.py?name=run_user_managed)]
  
## <a name="set-up-compute-targets-with-python"></a>Python ile işlem hedeflerini ayarlama

Kullanım bu yapılandırmak için aşağıdaki bölümlerde işlem hedefleri:

* [Yerel bilgisayar](#local)
* [Azure Machine Learning işlem](#amlcompute)
* [Uzak sanal makineler](#vm)
* [Azure HDInsight](#hdinsight)


### <a id="local"></a>Yerel bilgisayar

1. **Oluşturma ve ekleme**: Yerel bilgisayarınıza eğitim ortamı olarak kullanmak üzere bir işlem hedefine eklemek veya oluşturmak için gerek yoktur.  

1. **Yapılandırma**:  Yerel bilgisayarınızda bir işlem hedef olarak kullandığınızda, eğitim kod çalışmasında, [geliştirme ortamı](how-to-configure-environment.md).  Bu ortamda zaten Python paketlerini ihtiyacınız varsa, kullanıcı tarafından yönetilen ortamı kullanın.

 [!code-python[](~/aml-sdk-samples/ignore/doc-qa/how-to-set-up-training-targets/local.py?name=run_local)]

İşlem bağlı ve çalıştırma yapılandırılmış göre sonraki adım olarak [eğitim çalıştırma gönderme](#submit).

### <a id="amlcompute"></a>Azure Machine Learning işlem

Azure Machine Learning işlem kolayca tek veya çok düğümlü bir işlem oluşturmak kullanıcıya izin veren bir yönetilen işlem altyapısıdır. İşlem, çalışma alanı bölge içinde çalışma alanınızda diğer kullanıcılarla paylaşılabilir bir kaynak oluşturulur. Bir iş gönderilir ve bir Azure sanal ağında koyabilirsiniz işlem otomatik olarak ölçeklendirilebilir. İşlem, bir kapsayıcı ortamında yürütür ve model bağımlılıklarınızı paketleri bir [Docker kapsayıcısı](https://www.docker.com/why-docker).

Azure Machine Learning işlem eğitim işlemin CPU veya GPU işlem düğümü bulutta bir küme dağıtmak için kullanabilirsiniz. GPU'ları içeren VM boyutları hakkında daha fazla bilgi için bkz. [GPU için iyileştirilmiş sanal makine boyutları](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-gpu).

Azure Machine Learning işlemi, ayrılan çekirdek sayısı gibi varsayılan sınırlara sahiptir. Daha fazla bilgi için [Azure kaynaklarını yönetin ve istek kotalarını](https://docs.microsoft.com/azure/machine-learning/service/how-to-manage-quotas).


Bir farklı çalıştır zamanladığınızda isteğe bağlı veya bir kalıcı kaynak olarak bir Azure Machine Learning işlem ortamı oluşturabilirsiniz.

#### <a name="run-based-creation"></a>Çalıştırma tabanlı oluşturma

Azure Machine Learning işlem işlem hedefi çalışma zamanında oluşturabilirsiniz. İşlem, çalıştırmak için otomatik olarak oluşturulur. İşlem, çalıştırma tamamlandıktan sonra otomatik olarak silinir. 

> [!NOTE]
> Max kullanmak için düğüm sayısını belirtmek için normalde ayarlarsınız `node_count` düğüm sayısı. Şu anda (04/04/2019) çalışmasını engelleyen bir hata. Geçici çözüm olarak, `amlcompute._cluster_max_node_count` çalıştırma yapılandırma özelliği. Örneğin, `run_config.amlcompute._cluster_max_node_count = 5`.

> [!IMPORTANT]
> Azure Machine Learning işlem çalışma tabanlı oluşturulması şu anda Önizleme aşamasındadır. Otomatik hiper parametre ayarı kullanın ya da machine learning otomatik çalıştırma tabanlı olarak oluşturulmasını kullanmayın. Hiper parametre ayarı veya otomatik makine öğrenimi kullanmak için oluşturun bir [kalıcı işlem](#persistent) bunun yerine hedef.

1.  **Oluşturma, ekleme ve yapılandırma**: Çalıştırma tabanlı olarak oluşturulmasını oluşturma, ekleme ve işlem hedef çalışma yapılandırması ile yapılandırmak için gerekli tüm adımları gerçekleştirir.  

  [!code-python[](~/aml-sdk-samples/ignore/doc-qa/how-to-set-up-training-targets/amlcompute.py?name=run_temp_compute)]


İşlem bağlı ve çalıştırma yapılandırılmış göre sonraki adım olarak [eğitim çalıştırma gönderme](#submit).

#### <a id="persistent"></a>Kalıcı işlem

Bir kalıcı Azure Machine Learning işlem işleri arasında yeniden kullanılabilir. İşlem, çalışma alanındaki diğer kullanıcılarla paylaşılabilir ve işleri arasında tutulur.

1. **Oluşturma ve ekleme**: Python'da kalıcı bir Azure Machine Learning işlem kaynak oluşturmak için belirtin **vm_size** ve **max_nodes** özellikleri. Azure Machine Learning, daha sonra diğer özellikler için akıllı Varsayılanları kullanır. İşlem daralttığında kullanılan değil sıfır düğümleri gösteriyor.   Gerektiğinde işlerinizi çalıştırmak için adanmış VM'ler oluşturulur.
    
    * **vm_size**: Azure Machine Learning işlem tarafından oluşturulan düğümler VM ailesi.
    * **max_nodes**: Otomatik ölçeklendirme, Azure Machine Learning işlem iş çalıştırıldığında en fazla düğüm maksimum sayısı.
    
   [!code-python[](~/aml-sdk-samples/ignore/doc-qa/how-to-set-up-training-targets/amlcompute2.py?name=cpu_cluster)]

   Azure Machine Learning işlem oluşturduğunuzda, bazı gelişmiş özellikler yapılandırabilirsiniz. Özellikleri aboneliğinizde sabit boyutlu ya da mevcut bir Azure sanal ağ içindeki kalıcı bir küme oluşturmanıza imkan tanır.  Bkz: [AmlCompute sınıfı](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.amlcompute.amlcompute?view=azure-ml-py
    ) Ayrıntılar için.
    
   Ya da oluşturabilir ve Azure Machine Learning işlem bir kalıcı kaynak ekleme [Azure portalında](#portal-create).

1. **Yapılandırma**: Kalıcı işlem hedefi için bir çalıştırma yapılandırması oluşturun.

   [!code-python[](~/aml-sdk-samples/ignore/doc-qa/how-to-set-up-training-targets/amlcompute2.py?name=run_amlcompute)]

İşlem bağlı ve çalıştırma yapılandırılmış göre sonraki adım olarak [eğitim çalıştırma gönderme](#submit).


### <a id="vm"></a>Uzak sanal makineler

Azure Machine Learning, ayrıca kendi işlem kaynağı getiren ve çalışma alanınıza eklenmesini destekler. Azure Machine Learning hizmetini erişilebilir olduğu sürece bir kaynak türü bir rasgele uzak VM ' dir. Kaynak, bir Azure VM, kuruluş veya şirket içi uzak bir sunucuda olabilir. Özellikle, verilen IP adresini ve kimlik bilgilerini (kullanıcı adı ve parola veya SSH anahtarı), tüm erişilebilir VM'ler uzaktan çalıştırmalar için kullanabilirsiniz.

Bir Docker kapsayıcısı, zaten var olan bir Python ortamını veya sistem tarafından oluşturulan conda ortamda kullanabilirsiniz. Bir Docker kapsayıcısında yürütmek için VM'de çalışan bir Docker altyapısının olmalıdır. Yerel makinenize daha esnek, bulut tabanlı geliştirme/deneme ortamı istediğinizde, bu işlev özellikle yararlıdır.

Azure veri bilimi sanal makinesi (DSVM), bu senaryo için tercih ettiğiniz Azure VM olarak kullanın. Bu, önceden yapılandırılmış bir veri bilimi ve yapay ZEKA geliştirme ortamında Azure vm'dir. VM, araç ve çerçeve tam yaşam döngüsü makine öğrenimi geliştirme için seçkin bir seçenek sunar. Azure Machine Learning ile DSVM'sini kullanma hakkında daha fazla bilgi için bkz. [geliştirme ortamını yapılandırma](https://docs.microsoft.com/azure/machine-learning/service/how-to-configure-environment#dsvm).

1. **Oluşturma**: Modelinizi eğitmek için kullanmadan önce bir DSVM oluşturma. Bu kaynak oluşturmak için bkz [Linux (Ubuntu) için veri bilimi sanal makinesi sağlama](https://docs.microsoft.com/azure/machine-learning/data-science-virtual-machine/dsvm-ubuntu-intro).

    > [!WARNING]
    > Azure Machine Learning yalnızca Ubuntu çalıştıran sanal makineleri destekler. Bir VM oluşturmak veya mevcut bir VM'yi seçin, Ubuntu kullanan bir VM seçmeniz gerekir.

1. **Ekleme**: Mevcut bir sanal makine işlem hedefi olarak eklemek için sanal makine için tam etki alanı adı (FQDN), kullanıcı adı ve parola sağlamalısınız. Bu örnekte değiştirin \<fqdn > Genel VM'nin genel IP adresi veya FQDN ile. Değiştirin \<username > ve \<parola > SSH kullanıcı adı ve parolayla VM için.

   ```python
   from azureml.core.compute import RemoteCompute, ComputeTarget

   # Create the compute config 
   compute_target_name = "attach-dsvm"
   attach_config = RemoteCompute.attach_configuration(address = "<fqdn>",
                                                    ssh_port=22,
                                                    username='<username>',
                                                    password="<password>")

   # If you authenticate with SSH keys instead, use this code:
   #                                                  ssh_port=22,
   #                                                  username='<username>',
   #                                                  password=None,
   #                                                  private_key_file="<path-to-file>",
   #                                                  private_key_passphrase="<passphrase>")

   # Attach the compute
   compute = ComputeTarget.attach(ws, compute_target_name, attach_config)

   compute.wait_for_completion(show_output=True)
   ```

   DSVM çalışma alanınıza eklemek veya [Azure portalını kullanarak](#portal-reuse).

1. **Yapılandırma**: DSVM işlem hedefi için bir çalıştırma yapılandırması oluşturun. Docker ve conda oluşturmak ve eğitim ortamı DSVM'nin yapılandırmak için kullanılır.

   [!code-python[](~/aml-sdk-samples/ignore/doc-qa/how-to-set-up-training-targets/dsvm.py?name=run_dsvm)]


İşlem bağlı ve çalıştırma yapılandırılmış göre sonraki adım olarak [eğitim çalıştırma gönderme](#submit).

### <a id="hdinsight"></a>Azure HDInsight 

Azure HDInsight, büyük veri analizi için popüler bir platformdur. Apache Spark, modelinizi eğitmek için kullanılan platform sağlar.

1. **Oluşturma**:  Modelinizi eğitmek için kullanmadan önce HDInsight kümesi oluşturun. HDInsight kümesinde bir Spark oluşturmak için bkz: [HDInsight Spark kümesi oluşturma](https://docs.microsoft.com/azure/hdinsight/spark/apache-spark-jupyter-spark-sql). 

    Kümeyi oluşturduğunuzda, bir SSH kullanıcı adı ve parola belirtmeniz gerekir. Bir işlem hedefi olarak HDInsight'ı kullanmaya gerek duyduğunuzda, bu değerleri not alın.
    
    Küme oluşturulduktan sonra ana bilgisayar adı ile bağlanmak \<clustername >-ssh.azurehdinsight.net, burada \<clustername >, küme için sağlanan addır. 

1. **Ekleme**: Bir HDInsight kümesi işlem hedefi olarak eklemek için ana bilgisayar adı, kullanıcı adı ve parola HDInsight kümesi için sağlamanız gerekir. Aşağıdaki örnek, bir küme çalışma alanınıza eklemek için SDK'sını kullanır. Bu örnekte değiştirin \<clustername > değerini kümenizin adıyla. Değiştirin \<username > ve \<parola > SSH kullanıcı adı ve parola küme için.

   ```python
   from azureml.core.compute import ComputeTarget, HDInsightCompute
   from azureml.exceptions import ComputeTargetException

   try:
    # if you want to connect using SSH key instead of username/password you can provide parameters private_key_file and private_key_passphrase
    attach_config = HDInsightCompute.attach_configuration(address='<clustername>-ssh.azureinsight.net', 
                                                          ssh_port=22, 
                                                          username='<ssh-username>', 
                                                          password='<ssh-pwd>')
    hdi_compute = ComputeTarget.attach(workspace=ws, 
                                       name='myhdi', 
                                       attach_configuration=attach_config)

   except ComputeTargetException as e:
    print("Caught = {}".format(e.message))

   hdi_compute.wait_for_completion(show_output=True)
   ```

   HDInsight kümesi çalışma alanınıza eklemek veya [Azure portalını kullanarak](#portal-reuse).

1. **Yapılandırma**: HDI işlem hedefi için bir çalıştırma yapılandırması oluşturun. 

   [!code-python[](~/aml-sdk-samples/ignore/doc-qa/how-to-set-up-training-targets/hdi.py?name=run_hdi)]


İşlem bağlı ve çalıştırma yapılandırılmış göre sonraki adım olarak [eğitim çalıştırma gönderme](#submit).


### <a id="azbatch"></a>Azure Batch 

Azure Batch, büyük ölçekli paralel ve yüksek performanslı hesaplama (HPC) uygulamalarını bulutta verimli bir şekilde çalıştırmak için kullanılır. AzureBatchStep bir Azure Machine Learning işlem hattı, bir Azure Batch havuzu makinelerin iş göndermek için kullanılabilir.

Azure Batch işlem hedefi olarak eklemek için Azure Machine Learning SDK'yı kullanın ve aşağıdaki bilgileri sağlayın:

-   **Azure Batch işlem adı**: Çalışma alanı içinde bir işlem için kullanılacak bir kolay ad
-   **Azure Batch hesabı adı**: Azure Batch hesabı adı
-   **Kaynak grubu**: Azure Batch hesabını içeren kaynak grubu.

Aşağıdaki kod, Azure Batch işlem hedefi olarak eklemek gösterilmektedir:

```python
from azureml.core.compute import ComputeTarget, BatchCompute
from azureml.exceptions import ComputeTargetException

batch_compute_name = 'mybatchcompute' # Name to associate with new compute in workspace

# Batch account details needed to attach as compute to workspace
batch_account_name = "<batch_account_name>" # Name of the Batch account
batch_resource_group = "<batch_resource_group>" # Name of the resource group which contains this account

try:
    # check if the compute is already attached
    batch_compute = BatchCompute(ws, batch_compute_name)
except ComputeTargetException:
    print('Attaching Batch compute...')
    provisioning_config = BatchCompute.attach_configuration(resource_group=batch_resource_group, account_name=batch_account_name)
    batch_compute = ComputeTarget.attach(ws, batch_compute_name, provisioning_config)
    batch_compute.wait_for_completion()
    print("Provisioning state:{}".format(batch_compute.provisioning_state))
    print("Provisioning errors:{}".format(batch_compute.provisioning_errors))

print("Using Batch compute:{}".format(batch_compute.cluster_resource_id))
```

## <a name="set-up-compute-in-the-azure-portal"></a>Azure Portalı'nda işlem ayarlama

Azure portalında bir çalışma alanınız ile ilişkili olan işlem hedefleri erişebilirsiniz.  Portala kullanabilirsiniz:

* [Görünüm işlem hedeflerini](#portal-view) çalışma alanınıza bağlı
* [İşlem hedefi oluşturmak](#portal-create) çalışma alanınızdaki
* [İşlem hedefi ekleme](#portal-reuse) dışında çalışma oluşturuldu

Bir hedef oluşturulur ve çalışma alanınıza bağlı sonra onu çalıştırması yapılandırmanızdaki kullanacaksınız bir `ComputeTarget` nesnesi: 

```python
from azureml.core.compute import ComputeTarget
myvm = ComputeTarget(workspace=ws, name='my-vm-name')
```

### <a id="portal-view"></a>İşlem hedefleri görüntüle


Çalışma alanınız için işlem hedefleri görmek için aşağıdaki adımları kullanın:

1. Gidin [Azure portalında](https://portal.azure.com) ve, çalışma alanını açın. 
1. Altında __uygulamaları__seçin __işlem__.

    ![Görünüm işlem sekmesi](./media/how-to-set-up-training-targets/azure-machine-learning-service-workspace.png)

### <a id="portal-create"></a>İşlem hedefi oluşturmak

İşlem hedeflerin listesini görüntülemek için önceki adımları izleyin. Ardından işlem hedefi oluşturmak için aşağıdaki adımları kullanın: 

1. İşlem hedefi eklemek için artı işaretini (+) seçin.

    ![İşlem hedefi ekleyin](./media/how-to-set-up-training-targets/add-compute-target.png) 

1. İşlem hedefi için bir ad girin. 

1. Seçin **Machine Learning işlem** kullanılmak üzere işlem türü olarak __eğitim__. 

    >[!NOTE]
    >Azure Machine Learning işlemi Azure portalında oluşturabileceğiniz yalnızca yönetilen-işlem kaynağıdır.  Oluşturulduktan sonra diğer tüm işlem kaynaklarını eklenebilir.

1. Formu doldurun. Gerekli özellikleri için değerleri sağlayın, özellikle **VM ailesi**ve **en fazla düğüme** hesaplamayı dönmesi için kullanılacak.  

    ![Formu doldurun](./media/how-to-set-up-training-targets/add-compute-form.png) 

1. __Oluştur__’u seçin.


1. İşlem hedef listeden seçerek oluşturma işleminin durumunu görüntüleyin:

    ![Oluşturma işlemi durumunu görüntülemek için bir işlem hedef seçin](./media/how-to-set-up-training-targets/View_list.png)

1. Ardından işlem hedef ayrıntılarına bakın: 

    ![Bilgisayar hedef ayrıntıları görüntüleyin](./media/how-to-set-up-training-targets/compute-target-details.png) 



### <a id="portal-reuse"></a>İşlem hedefleri ekleme

Azure Machine Learning hizmeti çalışma dışında oluşturulan işlem hedefleri kullanmak için bunları eklemeniz gerekir. İşlem hedefi eklemek çalışma alanınızı kullanılmasını sağlar.

İşlem hedeflerin listesini görüntülemek için daha önce açıklanan adımları izleyin. Ardından, işlem hedefi eklemek için aşağıdaki adımları kullanın: 

1. İşlem hedefi eklemek için artı işaretini (+) seçin. 
1. İşlem hedefi için bir ad girin. 
1. İçin eklemek için işlem türünü seçin __eğitim__:

    > [!IMPORTANT]
    > Tüm işlem, türleri, Azure portalından eklenebilir. Eğitim için şu anda eklenebilecek işlem türleri şunlardır:
    >
    > * Uzak VM
    > * Azure Databricks (için machine learning işlem hatlarını kullanın)
    > * Azure Data Lake Analytics (için machine learning işlem hatlarını kullanın)
    > * Azure HDInsight

1. Formu doldurun ve gerekli özellikleri için değerler sağlayın.

    > [!NOTE]
    > Microsoft, SSH anahtarları, parolalara göre daha güvenlidir kullanmanızı önerir. Parola deneme yanılma saldırılarına karşı savunmasızdır. SSH anahtarlarını, şifreleme imzalarını kullanır. Azure sanal makineler ile kullanmak için SSH anahtarları oluşturma hakkında daha fazla bilgi için aşağıdaki belgelere bakın:
    >
    > * [Oluşturma ve Linux veya Macos'ta SSH anahtarlarını kullanma](https://docs.microsoft.com/azure/virtual-machines/linux/mac-create-ssh-keys)
    > * [Oluşturma ve Windows üzerinde SSH anahtarlarını kullanma](https://docs.microsoft.com/azure/virtual-machines/linux/ssh-from-windows)

1. Seçin __ekleme__. 
1. İşlem hedef listeden seçerek iliştirme işlemi durumunu görüntüleyin.

## <a name="set-up-compute-with-the-cli"></a>CLI ile işlem ayarlama

Kullanarak çalışma ile ilişkilendirilen işlem hedefleri erişebileceğiniz [CLI uzantısını](reference-azure-machine-learning-cli.md) Azure Machine Learning hizmeti için.  CLI'yı kullanabilirsiniz:

* Yönetilen işlem hedefi oluşturmak
* Güncelleştirme yönetilen işlem hedefi
* Bir yönetilmeyen işlem hedefi ekleme

Daha fazla bilgi için [kaynak yönetimi](reference-azure-machine-learning-cli.md#resource-management).

## <a id="submit"></a>Çalıştırma eğitim gönderin

Bir çalıştırma yapılandırma oluşturduktan sonra denemenizi çalıştırmak için kullanın.  Eğitim çalıştırma göndermek için kod desenini işlem hedefleri tüm türleri için de aynıdır:

1. Çalıştırmak için bir deneme oluşturma
1. Çalıştırma gönderin.
1. Çalıştırmak için bekleyin.

> [!IMPORTANT]
> Eğitim çalıştırma gönderdiğinizde, eğitim komut dosyalarınızı içeren dizine anlık görüntüsünü oluşturulur ve işlem hedefine gönderildi. Ayrıca, çalışma alanınızda denemeler bir parçası olarak depolanır. Dosyaları değiştirin ve çalıştırma gönderme yalnızca değiştirilen dosyaların yeniden yüklenecek.
>
> Anlık görüntüde bulunan dosyaların önlemek için oluşturma bir [.gitignore](https://git-scm.com/docs/gitignore) veya `.amlignore` dosya dizin ve dosyaları ekleyin. `.amlignore` Dosyası aynı sözdizimini kullanır ve olarak desenleri [.gitignore](https://git-scm.com/docs/gitignore) dosya. Her iki dosya varsa, `.amlignore` dosya önceliklidir.
> 
> Daha fazla bilgi için [anlık görüntüleri](concept-azure-machine-learning-architecture.md#snapshot).

### <a name="create-an-experiment"></a>Deneme oluşturma

İlk olarak, bir denemeyi çalışma alanınızda oluşturun.

[!code-python[](~/aml-sdk-samples/ignore/doc-qa/how-to-set-up-training-targets/local.py?name=experiment)]

### <a name="submit-the-experiment"></a>Denemeyi gönderme

İle deneme gönderme bir `ScriptRunConfig` nesne.  Bu nesne içerir:

* **kaynak_dizin**: Eğitim komut dosyanızı içeren kaynak dizini
* **betik**: Eğitim betiği tanımlayın
* **run_config**: Eğitim gerçekleşeceği sırayla tanımlayan çalıştırma yapılandırmasına.

Örneğin, kullanılacak [yerel hedef](#local) yapılandırma:

[!code-python[](~/aml-sdk-samples/ignore/doc-qa/how-to-set-up-training-targets/local.py?name=local_submit)]

Farklı işlem hedefte farklı bir çalıştırma yapılandırma gibi kullanarak çalıştırmak için aynı denemeyi geçiş [amlcompute hedef](#amlcompute):

[!code-python[](~/aml-sdk-samples/ignore/doc-qa/how-to-set-up-training-targets/amlcompute2.py?name=amlcompute_submit)]

Veya, şunları yapabilirsiniz:

* İle deneme gönderme bir `Estimator` nesne gösterildiği [estimators Train ML modelleriyle](how-to-train-ml-models.md). 
* Bir denemeyi göndermek [CLI uzantısını kullanarak](reference-azure-machine-learning-cli.md#experiments).

## <a name="github-tracking-and-integration"></a>GitHub izleme ve tümleştirme

Kaynak dizini yerel bir Git deposu olduğu çalıştırma eğitim başlattığınızda, depo bilgilerini çalıştırma geçmişinde depolanır. Örneğin, geçerli işleme kimliği depo için geçmiş bir parçası olarak günlüğe kaydedilir.

## <a name="notebook-examples"></a>Not Defteri örnekleri

Bu not defterlerini eğitim çeşitli işlem hedefleri olan örnekler için bkz:
* [Yardım-How-to-kullanın-azureml/eğitimi](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training)
* [öğreticiler/img-sınıflandırma-bölüm 1-training.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/tutorials/img-classification-part1-training.ipynb)

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Sonraki adımlar

* [Öğretici: Bir model eğitip](tutorial-train-models-with-aml.md) bir modeli eğitmek için yönetilen işlem hedefi kullanır.
* Bilgi edinmek için nasıl [hiperparametreleri verimli bir şekilde ayarlamak](how-to-tune-hyperparameters.md) daha iyi modelleri oluşturmak için.
* Eğitilen bir modelin aldıktan sonra bilgi [nasıl ve nerede modelleri dağıtma](how-to-deploy-and-where.md).
* Görünüm [RunConfiguration sınıfı](https://docs.microsoft.com/python/api/azureml-core/azureml.core.runconfig.runconfiguration?view=azure-ml-py) SDK başvurusu.
* [Azure Machine Learning hizmeti ile Azure sanal ağları kullanın.](how-to-enable-virtual-network.md)
