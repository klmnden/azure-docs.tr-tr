---
title: Büyük veri dosyalarını okuma ve yazma | Microsoft Docs
description: Okuma ve büyük dosyaları Azure Machine Learning denemeleri yazma.
services: machine-learning
author: hning86
ms.author: haining
manager: mwinkle
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.topic: article
ms.date: 09/10/2017
ROBOTS: NOINDEX
ms.openlocfilehash: 4a2dff4dd57bdb0b010bbb4568d796f1e197a728
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46971510"
---
# <a name="persisting-changes-and-working-with-large-files"></a>Değişiklikleri devam ettirmeden ve büyük dosyaları ile çalışma

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)]

Azure Machine Learning denemesi hizmeti ile yürütme hedeflerini çeşitli yapılandırabilirsiniz. Yerel bir yerel bilgisayarda veya yerel bir bilgisayardaki bir Docker kapsayıcısı gibi bazı hedefler. Diğer uzak bir uzak makine veya bir HDInsight kümesi üzerinde bir Docker kapsayıcısı gibi. Daha fazla bilgi için [genel bakış Azure Machine Learning deneme yürütme hizmeti](experimentation-service-configuration.md). 

Bir hedef yürütmeden önce işlem hedef proje klasörüne kopyalamanız gerekir. Bu nedenle bile geçici bir yerel klasör bu amaç için kullanılan bir yerel yürütme ile yapmanız gerekir. 

## <a name="execution-isolation-portability-and-reproducibility"></a>Yeniden üretilebilirliğini yürütme yalıtım ve taşınabilirlik
Bu tasarım amacı, yalıtım, yeniden üretilebilirliğini ve taşınabilirlik yürütme sağlamaktır. Aynı betiği iki kez çalıştırırsanız, hedef, aynı veya başka bir işlem aynı sonuçları alırsınız. İlk yürütme sırasında yapılan değişiklikleri de ikinci yürütme Etkilenme. Bu tasarımla, durum bilgisi olmayan hesaplama kaynakları, her tamamladıktan sonra yürütülen işler için hiçbir benzeşim sahip olarak işlem hedefleri davranabilirsiniz.

## <a name="challenges"></a>Zorluklar
Bu tasarım taşınabilirlik ve yinelenebilirliği sağlasa da bazı benzersiz beraberinde getirir.

### <a name="persisting-state-changes"></a>Kalıcı durum değişiklikleri
Komut dosyanızı işlem bağlamını durumunu değiştirirse, sonraki yürütme için değişiklikler kalıcı değildir ve bunlar istemci makineye otomatik olarak yayılır değil. 

Betiğinizi bir alt klasör oluşturur veya bir dosyaya yazar, daha açık belirtmek gerekirse, klasör veya dosya yürütme sonrasında proje dizininde mevcut olmaz. Dosyaları, işlem hedef ortamı geçici bir klasörde depolanır. Hata ayıklama amacıyla kullanabilir, ancak bunların kalıcı varlığı üzerinde güvenemezsiniz.

### <a name="working-with-large-files-in-the-project-folder"></a>Proje klasöründeki büyük dosyalarla çalışma

Proje klasörünüzdeki tüm büyük dosyaları içeriyorsa, klasör, hedef işlem bir yürütme başına ortamı kopyalandığında gecikme yansıtılmaz. Yürütme yerel olarak olsa bile, yine de gereksiz disk trafiği önlemek için yoktur. Bu nedenle, biz şu anda 25 MB maksimum proje boyut sınırı uygularız.

## <a name="option-1-use-the-outputs-folder"></a>1. seçenek: Kullanma *çıkarır* klasörü
Bu seçenek, aşağıdaki tüm koşullar geçerliyse tercih edilir:
* Komut dosyası üretir.
* Her deneme ile değiştirmek için dosyaları beklediğiniz.
* Bu dosyalar geçmişini tutmak istersiniz. 

Genel kullanım örnekleri şunlardır:
* Modeli
* Bir veri kümesi oluşturma
* Model eğitiminin yürütmeyi bir parçası olarak bir resim dosyası olarak bir grafik çizim 

>[!Note]
> Çalıştırma sonrasında çıkış klasöründeki izlenen dosyasının en büyük boyutu 512 MB'dir. Bu komut çıktı klasöründe 512 MB değerinden daha büyük bir dosya oluşturur, burada toplanmaz anlamına gelir. 

Ayrıca, bir önceki tarafından üretilmiş bir çıktı dosyası (örneğin, bir model) çalıştırın ve bir sonraki göreve (örneğin, Puanlama) kullanın çalıştığında, select arasında çıkışları karşılaştırmak istediğiniz.

Adlı bir klasör dosyaları yazabilir *çıkarır* kök dizinine göreli olmasıdır. Klasör, deneme hizmeti tarafından özel olarak değerlendirilmesi alır. Herhangi bir şey betiğinizi klasöründe bir model dosyası, veri dosyası veya çizilen bir görüntü dosyası gibi yürütme sırasında oluşturur (toplu adıyla _yapıtları_), ilişkili olduğu Azure Blob Depolama hesabına kopyalanır, Deneme hesabı çalışma sona erdikten sonra. Dosyaları, çalıştırma geçmişi kaydına bir parçası haline gelir.

Bir model depolamak için kod kısa bir örnek *çıkarır* klasörü:
```python
import os
import pickle

# m is a scikit-learn model. 
# we serialize it into a mode.plk file under the ./outputs folder.
with open(os.path.join('.', 'outputs', 'model.pkl'), 'wb') as f:    
    pickle.dump(m, f)
```
Tüm yapıt göz atarak indirebileceğiniz **Çıkış dosyalarını** çalıştırmalardan sayfanın belirli bir bölümünde Azure Machine Learning Workbench'te çalıştırın. Yalnızca Çalıştır'ı seçin ve ardından **indirme** düğmesi. Alternatif olarak, girdiğiniz `az ml asset download` komut satırı arabirimi (CLI) penceresinde komutu.

Daha eksiksiz bir örnek için bkz: `iris_sklearn.py` Python betik içinde _Iris sınıflandırma_ örnek proje.

## <a name="option-2-use-the-shared-folder"></a>2. seçenek: paylaşılan klasörü kullanın
Her bir çalıştırmanın dosyalarının geçmişini tutmak üzere gerekmiyorsa, bağımsız çalıştırmaları arasında erişilebilen paylaşılan bir klasörü kullanarak aşağıdaki senaryolar için harika bir seçenek olabilir: 
- Verileri .csv dosyaları ya da eğitim veya test verisi olarak metin veya resim dosyaları dizini yerel dosyaları yüklemenizi betiğinizi gerekir. 
- Betiğinizi ham verileri işler ve özellikleri tespit eğitim verileri bir sonraki eğitime çalıştırmak için kullanılan metin veya görüntü dosyaları gibi ara sonuçlar kullanıma yazar. 
- Betiğinizi modelinin ölçeğini çıkarır ve sonraki Puanlama betiğinizi çekme modelini ve değerlendirme için kullanmanız gerekir. 

Paylaşılan klasörün yerel olarak, seçtiğiniz işlem hedefte ömrü olduğunu unutmayın. Bu nedenle, bu seçenek yalnızca bu paylaşılan klasöre başvuru betik çalıştırmalarınızı aynı yürütülen tüm işlem hedefi varsa çalışır ve işlem hedef çalışmaları arasında geri değildir.

Paylaşılan klasör özelliğin avantajlarından yararlanarak, okuma veya yazma için bir ortam değişkeni tarafından tanımlanan özel bir klasör `AZUREML_NATIVE_SHARE_DIRECTORY`. 

### <a name="example"></a>Örnek
Bu paylaşım klasörü için bir metin dosyasını okuma ve yazma için kullanılan bazı örnek Python kodu şu şekildedir:
```python
import os

# write to the shared folder
with open(os.environ['AZUREML_NATIVE_SHARE_DIRECTORY'] + 'test.txt', "w") as f1:
    f1.write(“Hello World”)

# read from the shared folder
with open(os.environ['AZUREML_NATIVE_SHARE_DIRECTORY'] + 'test.txt', "r") as f2:
    text = f2.read()
```

Daha eksiksiz bir örnek için bkz: *iris_sklearn_shared_folder.py* dosyası _Iris sınıflandırma_ örnek proje.

Bu özelliği kullanabilmeniz için önce kümesinde sahip *.compute* dosya hedeflenen bir yürütme bağlamındaki gösteren bazı basit yapılandırmaları *aml_config* klasör. Gerçek klasörünün yolunu, seçtiğiniz işlem hedef ve değerin yapılandırdığınız bağlı olarak değişebilir.

### <a name="configure-local-compute-context"></a>Yerel işlem bağlamını yapılandırın

Bir yerel işlem bağlamında bu özelliği etkinleştirmek için basitçe ekleyin, *.compute* gösteren aşağıdaki satırı dosya _yerel_ ortam (genellikle *local.compute*).
```
# local.runconfig
...
nativeSharedDirectory: ~/.azureml/share
...
```

Yol ~/.azureml/share varsayılan temel klasör yoludur. Çalıştırılacak betiği tarafından erişilebilir olduğundan yerel mutlak yolu değiştirebilirsiniz. Deneme hesabı adı, çalışma alanı adı ve proje adı, paylaşılan dizinin tam yolu haline gelir temel dizin adı için otomatik olarak eklenir. Örneğin, dosyaları yazılan (ve alınan) önceki kullanırsanız aşağıdaki yol varsayılan değer:

```
# on Windows
C:\users\<username>\.azureml\share\<exp_acct_name>\<workspace_name>\<proj_name>\

# on macOS
/Users/<username>/.azureml/share/<exp_acct_name>/<workspace_name>/<proj_name>/
```

### <a name="configure-the-docker-compute-context-local-or-remote"></a>Docker işlem bağlamını (yerel veya uzak) yapılandırma
Bir Docker işlem bağlamı bu özelliği etkinleştirmek için aşağıdaki iki satırı, yerel veya uzak Docker eklemelisiniz *.compute* dosya.

```
# docker.compute
...
sharedVolumes: true
nativeSharedDirectory: ~/.azureml/share
...
```
>[!IMPORTANT]
>**SharedVolumes** özelliği ayarlanmalıdır *true* kullandığınızda `AZUREML_NATIVE_SHARE_DIRECTORY` paylaşılan klasöre erişmek için ortam değişkeni. Aksi takdirde, yürütme başarısız olur.

Bu paylaşılan klasörü olarak her zaman bir Docker kapsayıcısı içinde çalışan kodu görür */azureml-share /*. Klasör yolu, Docker kapsayıcı tarafından görülen şekilde yapılandırılabilir değildir. Bu klasör adı, kodunuzda kullanmayın. Bunun yerine, her zaman ortam değişkeni adı kullanın `AZUREML_NATIVE_SHARE_DIRECTORY` bu klasörü belirtmek için. Docker konağı üzerindeki yerel bir klasöre eşlendi makine veya işlem bağlamında. Bu yerel klasörle temel dizinini yapılandırılabilir değeri `nativeSharedDirectory` ayarı *.compute* dosya. Varsayılan değeri kullanırsanız, konak makinesinde, paylaşılan klasörün yerel yol aşağıda verilmiştir:
```
# Windows
C:\users\<username>\.azureml\share\<exp_acct_name>\<workspace_name>\<proj_name>\

# macOS
/Users/<username>/.azureml/share/<exp_acct_name>/<workspace_name>/<proj_name>/

# Ubuntu Linux
/home/<username>/.azureml/share/<exp_acct_name>/<workspace_name>/<proj_name>/
```

>[!NOTE]
>Yerel işlem bağlamını veya yerel bir Docker işlem bağlamı olup paylaşılan klasör yerel diskteki yolu aynıdır. Başka bir deyişle, hatta dosyaları yerel çalıştırma ve yerel bir Docker çalıştırma arasında paylaşabilirsiniz.

Giriş verileri bu klasörleri doğrudan koyun ve yerel veya Docker machine çalışır, devam edebilirsin, bekler. Ayrıca, yerel ya da Docker çalıştırmaları dosyalarını bu klasöre yazmak ve yürütme yaşam döngüsü geri kalan beklediğiniz dosyaları bu klasörde kalıcı.

Daha fazla bilgi için [Azure Machine Learning Workbench yürütme yapılandırma dosyalarını](experimentation-service-configuration-reference.md).

>[!NOTE]
>`AZUREML_NATIVE_SHARE_DIRECTORY` Ortam değişkeni, bir HDInsight işlem bağlamda desteklenmiyor. Ancak, okuma ve ekli blob depolama alanına yazmak için açıkça mutlak bir Azure Blob Depolama yol kullanarak aynı sonucu elde etmek kolaydır.

## <a name="option-3-use-external-durable-storage"></a>Seçenek 3: dış dayanıklı depolama kullanma

Dış dayanıklı depolama, yürütme sırasında durumu kalıcı hale getirmek için kullanabilirsiniz. Bu seçenek, aşağıdaki senaryolarda kullanışlıdır:
- Girişinizi zaten hedef işlem ortamından erişilebilen dayanıklı depolama alanında depolanır.
- Dosyaları, çalıştırma geçmişi kayıtları bir parçası olması gerekmez.
- Dosyaları tarafından yürütme çeşitli işlem ortamlarında paylaşılması gerekir.
- Dosyaların işlem bağlamını varlığını sürdürmesini mümkün olması gerekir.

Bu tür bir yaklaşım, Python veya Pyspark'tan kodunuzu Azure Blob depolamadan kullanmaktır. Kısa bir örnek aşağıda verilmiştir:

```python
from azure.storage.blob import BlockBlobService
from azure.storage.blob.models import PublicAccess
import glob
import os

ACCOUNT_NAME = "<your blob storage account name>"
ACCOUNT_KEY = "<account key>"
CONTAINER_NAME = "<container name>"

blob_service = BlockBlobService(account_name=ACCOUNT_NAME, account_key=ACCOUNT_KEY)

## Create a new container if necessary, or use an existing one
blob_service.create_container(CONTAINER_NAME, fail_on_exist=False, public_access=PublicAccess.Container)

# df is a pandas DataFrame
df.to_csv('mydata.csv', sep='\t', index=False)

# Export the mydata.csv file to Blob storage.
for name in glob.iglob('mydata.csv'):
    blob_service.create_blob_from_path(CONTAINER_NAME, 'single_file.csv', name)
```

HDI Spark çalışma zamanı rastgele herhangi bir Azure Blob Depolama eklemek için kısa bir örnek aşağıda verilmiştir:
```python
def attach_storage_container(spark, account, key):
    config = spark._sc._jsc.hadoopConfiguration()
    setting = "fs.azure.account.key." + account + ".blob.core.windows.net"
    if not config.get(setting):
        config.set(setting, key)
 
attach_storage_container(spark, "<storage account name>", "<storage key>”)
```

## <a name="conclusion"></a>Sonuç
Azure Machine Learning, hedef işlem bağlamına tüm proje klasörüne kopyalayarak betikleri yürütüldüğünden, özel büyük giriş, çıkış ve Ara dosyaları dikkatli olun. Büyük dosya işlemleri için aracılığıyla erişilebilen paylaşılan klasör özel çıkış klasörünü kullanabilirsiniz `AZUREML_NATIVE_SHARE_DIRECTORY` ortam değişkeninde ya da dış dayanıklı depolama. 

## <a name="next-steps"></a>Sonraki adımlar
- Gözden geçirme [Azure Machine Learning Workbench yürütme yapılandırma dosyalarını](experimentation-service-configuration-reference.md) makalesi.
- Bkz. nasıl [Iris sınıflandırma](tutorial-classifying-iris-part-1.md) öğretici projesinin eğitilen bir modeli kalıcı hale getirmek için çıktı klasörü kullanır.
