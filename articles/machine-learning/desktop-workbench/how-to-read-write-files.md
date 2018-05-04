---
title: Büyük veri dosyaları okuma ve yazma | Microsoft Docs
description: Okuma ve Azure Machine Learning denemeleri büyük dosyaları yazma.
services: machine-learning
author: hning86
ms.author: haining
manager: mwinkle
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 09/10/2017
ms.openlocfilehash: 099ff69b396c35730471d684b59115f03ccf67d9
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/19/2018
---
# <a name="persisting-changes-and-working-with-large-files"></a>Değişiklikleri devam ettirmeden ve büyük dosyaları ile çalışma
Azure Machine Learning deneme hizmetiyle yürütme hedefleri çeşitli yapılandırabilirsiniz. Yerel bir yerel bilgisayarda veya yerel bir bilgisayardaki bir Docker kapsayıcısı gibi bazı hedefler. Diğer uzak, Docker kapsayıcısı uzak makinede veya bir Hdınsight kümesi gibi. Daha fazla bilgi için bkz: [Azure Machine Learning genel bakış denemeler yürütme hizmeti](experimentation-service-configuration.md). 

Bir hedef yürütebilir önce işlem hedef proje klasörüne kopyalamanız gerekir. Bu nedenle bile bu amaç için bir yerel geçici klasörü kullanan bir yerel yürütme ile yapmanız gerekir. 

## <a name="execution-isolation-portability-and-reproducibility"></a>Yürütme yalıtımı, taşınabilirlik ve yeniden Üretilebilirlik
Bu tasarım amacı yalıtım, yeniden Üretilebilirlik ve taşınabilirlik yürütme emin olmaktır. Aynı betik iki kez çalıştırırsanız, hedef, aynı veya başka bir işlem aynı sonuçları alırsınız. İlk yürütme sırasında yapılan değişiklikleri ikinci yürütme de etkiler döndürmemelidir. Bu tasarımla, durum bilgisiz hesaplama kaynağı, her tamamladıktan sonra yürütülen işler hiçbir benzeşimi sahip olarak işlem hedefleri davranabilirsiniz.

## <a name="challenges"></a>Zorluklar
Bu tasarım taşınabilirlik ve Yinelenebilirlik avantajları sağlasa da aynı zamanda benzersiz bazı zorluklar getirir.

### <a name="persisting-state-changes"></a>Kalıcı durum değişiklikleri
Komut dosyanızı işlem bağlamı durumunu değiştirirse, sonraki yürütme için değişiklikler kalıcı değildir ve bunlar istemci makineye otomatik olarak yayıldığı değil. 

Komut bir alt klasör oluşturur veya bir dosyayı yazar, daha belirgin olarak bu klasör veya dosyanın yürütme sonrasında proje dizininde mevcut olmaz. Dosyaları geçici bir işlem hedef ortam klasöründe depolanır. Hata ayıklama amacıyla kullanabilir, ancak bunların kalıcı varlığı üzerinde bağlı olamaz.

### <a name="working-with-large-files-in-the-project-folder"></a>Proje klasöründeki büyük dosyaları ile çalışma

Proje klasörünüzdeki tüm büyük dosyalar içeriyorsa, hedef işlem bir yürütme başlangıcında ortam klasörü kopyalandığında gecikme doğurur. Yürütme yerel olarak olsa bile, önlemek için hala gereksiz disk trafiği yoktur. Bu nedenle, biz 25 MB maksimum proje boyutta şu anda cap sağlar.

## <a name="option-1-use-the-outputs-folder"></a>Seçenek 1: Kullanın *çıkarır* klasörü
Bu seçenek aşağıdaki tüm koşullar geçerliyse tercih edilir:
* Komut dosyaları üretir.
* Dosyaları her deneme ile değiştirmek için bekler.
* Bu dosyaların geçmişi tutmak istiyor. 

Ortak kullanım durumları verilmiştir:
* Bir model eğitimi
* Bir veri kümesi oluşturma
* Model eğitim yürütme bir parçası olarak bir resim dosyası olarak bir grafik çizme 

Ayrıca, önceki tarafından üretilmiş bir çıktı dosyası (örneğin, bir model) çalıştırın ve (örneğin, Puanlama) bir sonraki görev kullanılmaya çalıştığında, select arasında çıkışları karşılaştırmak istediğiniz.

Adlı bir klasöre dosyaları yazabilir *çıkarır* göre kök dizini olmasıdır. Klasör deneme hizmeti tarafından özel işleme alır. Herhangi bir şey kodunuzu klasöründe bir model dosyası, veri dosyası veya çizilen görüntü dosyası gibi yürütme sırasında oluşturur (topluca bilinen _yapıları_), ilişkili olduğu Azure Blob Depolama hesabına kopyalanır, Çalışma sona erdikten sonra deneme hesabı. Dosyalar çalıştırma geçmişi kaydının bir parçası haline gelir.

Bir modeldeki depolamak için kod kısa bir örneği burada verilmiştir *çıkarır* klasörü:
```python
import os
import pickle

# m is a scikit-learn model. 
# we serialize it into a mode.plk file under the ./outputs folder.
with open(os.path.join('.', 'outputs', 'model.pkl'), 'wb') as f:    
    pickle.dump(m, f)
```
Tüm yapı göz atarak yükleyebilirsiniz **çıktı dosyaları** belirli bir çalışma ayrıntı sayfasının bölümünde Azure Machine Learning çalışma çalıştırın. Yalnızca Çalıştır'ı seçin ve ardından **karşıdan** düğmesi. Alternatif olarak, girebilirsiniz `az ml asset download` komut satırı arabirimi (CLI) penceresinde komutu.

Daha eksiksiz bir örnek için bkz: `iris_sklearn.py` Python komut dosyası _sınıflandırma Iris_ örnek proje.

## <a name="option-2-use-the-shared-folder"></a>Seçenek 2: paylaşılan klasörünü kullanın
Her çalışmanın dosyaları geçmişini tut gerekmiyorsa bağımsız çalıştırmaları arasında erişilebilen bir paylaşılan klasör kullanarak aşağıdaki senaryolar için harika bir seçenek olabilir: 
- .Csv dosyaları veya bir dizin eğitim veya test veri olarak metin veya resim dosyaları gibi yerel dosyalarından veri yüklemek komut dosyanızı gerekir. 
- Kodunuzu ham verileri işler ve bir sonraki eğitim çalıştırmak için kullanılan metin veya resim dosyalarını featurized eğitim verileri gibi ara sonuçların çıkışı yazar. 
- Bir model komut dosyanızı çıkarır ve sonraki Puanlama kodunuzu modelini seçin ve değerlendirme için kullanmanız gerekir. 

Paylaşılan klasörün yerel olarak seçilen işlem hedefte ömrü olduğunu dikkate almak önemlidir. Bu nedenle, bu seçenek yalnızca tüm bu paylaşılan klasör başvuru betik çalışmalarınız aynı yürütülen hedef işlem ise çalışır ve işlem hedefi çalışmaları arasında geri dönüştürüldüğünde değil.

Paylaşılan klasör özelliğin avantajlarından yararlanarak, okuma veya bir ortam değişkeni tarafından tanımlanan özel bir klasöre yazma `AZUREML_NATIVE_SHARE_DIRECTORY`. 

### <a name="example"></a>Örnek
Bu paylaşım klasöre okuma ve bir metin dosyasına yazma kullanmayı bazı örnek Python kodu şöyledir:
```python
import os

# write to the shared folder
with open(os.environ['AZUREML_NATIVE_SHARE_DIRECTORY'] + 'test.txt', 'wb') as f:
    f.write(“Hello World”)

# read from the shared folder
with open(os.environ['AZUREML_NATIVE_SHARE_DIRECTORY'] + 'test.txt', 'r') as f:
    text = file.read()
```

Daha eksiksiz bir örnek için bkz: *iris_sklearn_shared_folder.py* dosyasını _sınıflandırma Iris_ örnek proje.

Bu özelliği kullanabilmek için önce kümesinde sahip *.compute* dosya hedeflenen yürütme bağlamı temsil eden bazı basit yapılandırmaları *aml_config* klasör. Klasöre gerçek yol işlem hedef seçtiğiniz ve yapılandırdığınız değer bağlı olarak değişebilir.

### <a name="configure-local-compute-context"></a>Yerel işlem bağlamını yapılandırın

Bir yerel işlem bağlamında bu özelliği etkinleştirmek için yalnızca ekleyin, *.compute* temsil eden aşağıdaki satırı dosya _yerel_ ortam (genellikle adlı *local.compute*).
```
# local.runconfig
...
nativeSharedDirectory: ~/.azureml/share
...
```

Yol ~/.azureml/share varsayılan temel klasör yoludur. Bunu çalıştırmak komut dosyası tarafından erişilebilen herhangi yerel bir mutlak yol değiştirebilirsiniz. Deneme hesabı adını, çalışma alanı adı ve proje adı paylaşılan dizinin tam yolu olur temel dizin adına otomatik olarak eklenir. Örneğin, dosyaları yazılan (ve alınır) önceki kullanırsanız aşağıdaki yolu varsayılan değer:

```
# on Windows
C:\users\<username>\.azureml\share\<exp_acct_name>\<workspace_name>\<proj_name>\

# on macOS
/Users/<username>/.azureml/share/<exp_acct_name>/<workspace_name>/<proj_name>/
```

### <a name="configure-the-docker-compute-context-local-or-remote"></a>Docker işlem bağlamını (yerel veya uzak) yapılandırın
Docker işlem bağlamı bu özelliği etkinleştirmek için aşağıdaki iki satırı, yerel veya uzak Docker eklemelisiniz *.compute* dosya.

```
# docker.compute
...
sharedVolumes: true
nativeSharedDirectory: ~/.azureml/share
...
```
>[!IMPORTANT]
>**SharedVolumes** özelliği ayarlanmalıdır *true* kullandığınızda `AZUREML_NATIVE_SHARE_DIRECTORY` paylaşılan klasöre erişmek için ortam değişkeni. Aksi takdirde yürütme başarısız olur.

Bu paylaşılan klasörü olarak Docker kapsayıcısı içinde her zaman çalışan kodu görür */azureml-share /*. Klasör yolu, Docker kapsayıcısı tarafından görülen yapılandırılabilir değildir. Bu klasör adı kodunuzda kullanmayın. Bunun yerine, her zaman ortam değişkeni adı kullanın `AZUREML_NATIVE_SHARE_DIRECTORY` bu klasörü belirtmek için. Docker ana bilgisayardaki yerel bir klasöre eşlenmiş makine veya işlem bağlamı. Bu yerel klasör temel dizini yapılandırılabilir değeri `nativeSharedDirectory` ayarı *.compute* dosya. Varsayılan değeri kullanırsanız ana makinede, paylaşılan klasörün yerel yol aşağıda verilmiştir:
```
# Windows
C:\users\<username>\.azureml\share\<exp_acct_name>\<workspace_name>\<proj_name>\

# macOS
/Users/<username>/.azureml/share/<exp_acct_name>/<workspace_name>/<proj_name>/

# Ubuntu Linux
/home/<username>/.azureml/share/<exp_acct_name>/<workspace_name>/<proj_name>/
```

>[!NOTE]
>Yerel işlem bağlamı veya yerel bir Docker işlem bağlamı olup yerel diskteki paylaşılan klasörün yolunu aynıdır. Bu dosyalar yerel çalıştırma ve yerel Docker çalıştırma arasında bile paylaşabilirsiniz anlamına gelir.

Giriş verisi doğrudan bu klasörlerde yerleştirin ve yerel veya Docker çalıştığı makinede bunu seçebilirsiniz olduğunu bekler. Yerel ya da Docker çalıştırır dosyalarını bu klasöre yazma ve yürütme yaşam döngüsü sürdüren beklediğiniz dosyaları bu klasörde kalıcı.

Daha fazla bilgi için bkz: [Azure Machine Learning çalışma ekranı yürütme yapılandırma dosyalarını](experimentation-service-configuration-reference.md).

>[!NOTE]
>`AZUREML_NATIVE_SHARE_DIRECTORY` Ortam değişkeni bir Hdınsight işlem bağlamda desteklenmiyor. Ancak, okuma ve ekli blob depolama alanına yazmak için açıkça bir mutlak Azure Blob Depolama yolu kullanarak aynı sonucu elde etmek kolaydır.

## <a name="option-3-use-external-durable-storage"></a>Seçenek 3: dış dayanıklı depolama kullanma

Yürütme sırasında durumu devam ettirmek için dış dayanıklı depolamayı kullanabilirsiniz. Bu seçenek, aşağıdaki senaryolarda kullanışlıdır:
- Giriş verilerinizi zaten hedef işlem ortamından erişilebilen dayanıklı depolama alanında depolanır.
- Dosyaları çalıştırma geçmişi kayıtları bir parçası olması gerekmez.
- Dosyaları tarafından yürütmeleri çeşitli işlem ortamlar genelinde paylaşılan gerekir.
- Dosyaları işlem bağlamı varlığını sürdürmesini kurabilmesi gerekir.

Bu tür bir yaklaşım, Python veya PySpark kodunuzdan Azure Blob Depolama kullanmaktır. Kısa bir örnek aşağıda verilmiştir:

```python
from azure.storage.blob import BlockBlobService
import glob
import os

ACCOUNT_NAME = "<your blob storage account name>"
ACCOUNT_KEY = "<account key>"
CONTAINER_NAME = "<container name>"

blob_service = BlockBlobService(account_name=ACCOUNT_NAME, account_key=ACCOUNT_KEY)

## Create a new container if necessary, or use an existing one
my_service.create_container(CONTAINER_NAME, fail_on_exist=False, public_access=PublicAccess.Container)

# df is a pandas DataFrame
df.to_csv('mydata.csv', sep='\t', index=False)

# Export the mydata.csv file to Blob storage.
for name in glob.iglob('mydata.csv'):
    blob_service.create_blob_from_path(CONTAINER_NAME, 'single_file.csv', name)
```

Aşağıda, HDI Spark çalışma zamanına herhangi bir rastgele Azure Blob Depolama alanı eklemek için kısa bir örnek verilmiştir:
```python
def attach_storage_container(spark, account, key):
    config = spark._sc._jsc.hadoopConfiguration()
    setting = "fs.azure.account.key." + account + ".blob.core.windows.net"
    if not config.get(setting):
        config.set(setting, key)
 
attach_storage_container(spark, "<storage account name>", "<storage key>”)
```

## <a name="conclusion"></a>Sonuç
Azure Machine Learning, hedef işlem bağlamı için tüm proje klasörünü kopyalayarak betikleri çalıştırdığından, Ara dosyaları büyük giriş ve çıkış ile özel dikkat edin. Büyük dosya işlemleri için özel çıkış klasörü aracılığıyla erişilebilen paylaşılan klasör kullanabilirsiniz `AZUREML_NATIVE_SHARE_DIRECTORY` ortam değişkeni ya da harici dayanıklı depolama. 

## <a name="next-steps"></a>Sonraki adımlar
- Gözden geçirme [Azure Machine Learning çalışma ekranı yürütme yapılandırma dosyalarını](experimentation-service-configuration-reference.md) makalesi.
- Bkz: nasıl [sınıflandırma Iris](tutorial-classifying-iris-part-1.md) öğretici project eğitilen model kalıcı hale getirmek için çıkış klasörü kullanır.
