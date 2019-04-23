---
title: Sürümde yenilikler nelerdir?
titleSuffix: Azure Machine Learning service
description: Azure Machine Learning hizmeti ve makine öğrenimi için en son güncelleştirmeleri öğrenin ve Python SDK'ları veri hazırlama.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
ms.author: larryfr
author: Blackmist
ms.date: 04/08/2019
ms.custom: seodec18
ms.openlocfilehash: 7fc0d3a2e29a2aaa06d88f25828ff676d615939d
ms.sourcegitcommit: c884e2b3746d4d5f0c5c1090e51d2056456a1317
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "60149574"
---
# <a name="azure-machine-learning-service-release-notes"></a>Azure Machine Learning hizmeti sürüm notları

Bu makalede, Azure Machine Learning hizmet sürümleri hakkında bilgi edinin.  Her bir SDK tam bir açıklaması için başvuru belgelerini ziyaret edin:
+ Azure Machine Learning'ın [ **Python için ana SDK'sı**](https://aka.ms/aml-sdk)
+ Azure Machine Learning [ **veri hazırlama SDK'sı**](https://aka.ms/data-prep-sdk)

## <a name="2019-04-17"></a>2019-04-17

### <a name="azure-machine-learning-data-prep-sdk-v112"></a>Azure Machine Learning veri hazırlama SDK v1.1.2

Not: Veri hazırlığı Python SDK'sını yükleme artık `numpy` ve `pandas` paketleri. Bkz: [yükleme yönergeleri güncelleştirildi](https://aka.ms/aml-data-prep-installation).

+ **Yeni Özellikler**
  + Pivot dönüştürme artık kullanabilirsiniz.
    + Nasıl yapılır kılavuzunda: [Pivot not defteri](https://aka.ms/aml-data-prep-pivot-nb)
  + Yerel işlevlerde artık normal ifadeler kullanabilirsiniz.
    + Örnekler:
      + `dflow.filter(dprep.RegEx('pattern').is_match(dflow['column_name']))`
      + `dflow.assert_value('column_name', dprep.RegEx('pattern').is_match(dprep.value))`
  + Artık `to_upper`  ve `to_lower`  ifade dilindeki işlevleri.
  + Artık, bir veri profilinde her bir sütunun benzersiz değerlerin sayısını da görebilirsiniz.
  + Bazı yaygın olarak kullanılan okuyucu adımları için şimdi de geçirebilirsiniz `infer_column_types` bağımsız değişken. Bu ayarlanırsa `True`, veri hazırlığı, algılamak ve sütun türleri otomatik olarak dönüştürmek deneyecek.
    + `inference_arguments` artık kullanılmıyor.
  + Artık çağırabilirsiniz `Dataflow.shape`.

+ **Hata düzeltmeleri ve geliştirmeleri**
  + `keep_columns` artık ek isteğe bağlı bağımsız değişken kabul eden `validate_column_exists`, varsa denetler sonucunu `keep_columns` hiçbir sütun içerir.
  + (Bu bir dosyadan okunan) okuyucu adımların tümünü artık ek isteğe bağlı bağımsız değişken kabul `verify_exists`.
  + Pandas dataframe okunuyor ve veri profilleri alma performansı İyileştirildi.
  + Başarısız olan tek bir dizin bir tek bir veri akışı adımından dilimleme olduğu bir hata düzeltildi.

## <a name="2019-04-15"></a>2019-04-15

### <a name="azure-portal"></a>Azure Portal
+ **Yeni Özellikler**
  + Şimdi, var olan bir uzak işlem kümesi üzerinde varolan bir komut dosyasını yeniden gönderebilirsiniz. 
  + Artık yeni parametrelerle yayımlanan bir işlem hattı işlem hatları sekmesinde çalıştırabilirsiniz. 
  + Çalıştırma ayrıntılarını yeni bir anlık görüntü dosyası Görüntüleyici şimdi destekler. Belirli bir çalıştırma gönderildiğinde bir anlık görüntü dizininin görüntüleyebilirsiniz. Not Defteri çalıştırma başlatmak için gönderildi de indirebilirsiniz.
   + Şimdi, Azure Portalı'ndan üst çalıştırmaları iptal edebilirsiniz.

## <a name="2019-04-08"></a>2019-04-08

### <a name="azure-machine-learning-sdk-for-python-v1023"></a>Azure Machine SDK için Python v1.0.23 Learning

+ **Yeni Özellikler**
  + Azure Machine Learning SDK'sı artık Python 3.7 destekler.
  + Azure Machine Learning DNN Estimators artık yerleşik çoklu sürüm desteği sağlar. Örneğin, `TensorFlow`  estimator artık kabul eden bir `framework_version` parametresi ve kullanıcıları '1.10' veya '1.12' sürümünü belirtebilirsiniz. Geçerli SDK sürümünüzü tarafından desteklenen sürümlerinin listesi için çağrı `get_supported_versions()` istenen framework sınıfında (örneğin `TensorFlow.get_supported_versions()`).
  En son SDK'sı sürümü tarafından desteklenen sürümlerinin bir listesi için bkz. [DNN Estimator belgeleri](https://docs.microsoft.com/en-us/python/api/azureml-train-core/azureml.train.dnn?view=azure-ml-py).

### <a name="azure-machine-learning-data-prep-sdk-v111"></a>Azure Machine Learning veri hazırlama SDK v1.1.1

+ **Yeni Özellikler**
  + Birden çok veri deposu/DataPath/DataReference kaynakları read_ * dönüşümler kullanarak okuyabilir.
  + Sütunları yeni bir sütun oluşturmak için aşağıdaki işlemleri gerçekleştirebilirsiniz: bölme, modül, güç, floor, uzunluğu.
  + Veri hazırlığı artık Azure ML tanılama Suite'in bir parçası ve tanılama bilgileri varsayılan olarak oturum açar.
    + Bu devre dışı bırakmak için bu ortam değişkenini true olarak ayarlayın: DISABLE_DPREP_LOGGER

+ **Hata düzeltmeleri ve geliştirmeleri**
  + Yaygın olarak kullanılan sınıfları ve işlevleri için geliştirilmiş kod belgeleri.
  + Excel dosyaları okunamadı auto_read_file içinde bir hata düzeltildi.
  + Read_pandas_dataframe klasöründe üzerine seçeneği eklendi.
  + Geliştirilmiş performans dotnetcore2 bağımlılık yükleme ve Fedora 27/28 ve Ubuntu 1804 için destek eklendi.
  + Azure BLOB'ları okuma performansını geliştirdik.
  + Sütun türü algılama artık uzun türünde sütunlar destekler.
  + Python datetime nesneler yerine zaman damgası olarak bazı tarih değerlerini görüntüleyen hata düzeltildi.
  + Double yerine tamsayı olarak bazı türü sayıları görüntülendiği hata düzeltildi.

  
## <a name="2019-03-25"></a>2019-03-25

### <a name="azure-machine-learning-sdk-for-python-v1021"></a>Azure Machine SDK için Python v1.0.21 Learning

+ **Yeni Özellikler**
  + *Azureml.core.Run.create_children* yöntemi sağlayan birden çok alt düşük gecikme süreli oluşturulmasını tek bir çağrı ile çalışır.

### <a name="azure-machine-learning-data-prep-sdk-v110"></a>Azure Machine Learning veri hazırlama SDK v1.1.0

+ **Bozucu değişiklikler**
  + Veri hazırlık paketi kavramı, kullanım dışı bırakıldı ve artık desteklenmiyor. Tek bir pakette birden çok veri akışı kalıcı yerine, veri akışlarını ayrı ayrı kalıcı hale getirebilirsiniz.
    + Nasıl yapılır kılavuzunda: [Açma ve kaydetme veri akışlarını not defteri](https://aka.ms/aml-data-prep-open-save-dataflows-nb)

+ **Yeni Özellikler**
  + Veri hazırlığı, belirli bir anlam türüyle eşleşmesi ve buna göre bölme sütunlar artık tanıyabilirsiniz. Şu anda desteklenen STypes içerir: e-posta adresi, coğrafi koordinatlar vardır (enlem ve boylam), IPv4 ve IPv6 adresleri, ABD telefon numarası ve ABD posta kodu.
    + Nasıl yapılır kılavuzunda: [Anlam türleri not defteri](https://aka.ms/aml-data-prep-semantic-types-nb)
  + Veri hazırlığı, iki sayısal sütunları sonuç bir sütun oluşturmak için aşağıdaki işlemleri artık destekliyor: çıkarma, çarpma, bölme, modül ve.
  + Çağırabilirsiniz `verify_has_data()` çalıştırıldığında veri akışı kayıtları neden olup olmadığını denetlemek için bir veri akışı üzerinde.

+ **Hata düzeltmeleri ve geliştirmeleri**
  + Artık bir histogramda için sayısal bir sütun profilleri kullanmak için depo sayısını belirtebilirsiniz.
  + `read_pandas_dataframe` Dönüşüm artık DataFrame dize - olmasını gerektiriyor veya bayt - yazılan sütun adları.
  + Bir hata düzeltildi `fill_nulls` dönüştürme, burada değerleri doğru doldurulmamış sahipse sütunu eksik.

## <a name="2019-03-11"></a>2019-03-11

### <a name="azure-machine-learning-sdk-for-python-v1018"></a>Azure Machine SDK için Python v1.0.18 Learning

 + **Değişiklikleri**
   + Azureml tensorboard paket azureml contrib tensorboard değiştirir.
   + Bu sürümle birlikte oluşturma sırasında yönetilen hesaplama kümenizde (amlcompute), bir kullanıcı hesabı ayarlayabilirsiniz. Bu, yalnızca bu özellikleri sağlama yapılandırmada geçirerek yapılabilir. Daha fazla bilgi bulabilirsiniz [SDK başvuru belgeleri](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.amlcompute.amlcompute?view=azure-ml-py#provisioning-configuration-vm-size-----vm-priority--dedicated---min-nodes-0--max-nodes-none--idle-seconds-before-scaledown-none--admin-username-none--admin-user-password-none--admin-user-ssh-key-none--vnet-resourcegroup-name-none--vnet-name-none--subnet-name-none--tags-none--description-none-).

### <a name="azure-machine-learning-data-prep-sdk-v1017"></a>Azure Machine Learning veri hazırlama SDK v1.0.17

+ **Yeni Özellikler**
  + Artık ifade dilini kullanarak bir sonuç sütun oluşturmak için iki sayısal sütunlar ekleyerek destekler.

+ **Hata düzeltmeleri ve geliştirmeleri**
  + Belgeler ve parametre random_split için İyileştirildi.
  
## <a name="2019-02-27"></a>2019-02-27

### <a name="azure-machine-learning-data-prep-sdk-v1016"></a>Azure Machine Learning veri hazırlama SDK v1.0.16

+ **Hata düzeltmesi**
  + Bir hizmet sorumlusu kaynaklandı kimlik doğrulama sorunu düzeltildi API değişikliği.

## <a name="2019-02-25"></a>2019-02-25

### <a name="azure-machine-learning-sdk-for-python-v1017"></a>Azure Machine SDK için Python v1.0.17 Learning

+ **Yeni Özellikler**

  + Azure Machine Learning, popüler DNN çerçevesini bağlayıcı artık birinci sınıf destek sağlar. Kullanarak [ `Chainer` ](https://docs.microsoft.com/en-us/python/api/azureml-train-core/azureml.train.dnn.chainer?view=azure-ml-py) sınıfı kullanıcılar kolayca eğitin ve bağlayıcı Modellerinizi dağıtın.
    + Bilgi nasıl [dağıtılmış eğitimi ChainerMN ile çalıştırın.](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training-with-deep-learning/distributed-chainer/distributed-chainer.ipynb)
    + Bilgi edinmek için nasıl [HyperDrive kullanarak bağlayıcı ile hiper parametre ayarı çalıştırın](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-chainer/train-hyperparameter-tune-deploy-with-chainer.ipynb)
  + Azure Machine Learning işlem hatlarını özelliği tetikleyici işlem hattı çalıştırmasını veri deposu değişikliklerine göre eklendi. İşlem hattı [zamanlama not defteri](https://aka.ms/pl-schedule) bu özelliği göstermek için güncelleştirilir.

+ **Hata düzeltmeleri ve geliştirmeleri**
  + Azure Machine Learning işlem hatlarını destek source_directory_data_store özelliği (örneğin, bir blob depolama) istenen bir veri deposu olarak ayarlanması için üzerinde ekledik [RunConfigurations](https://docs.microsoft.com/en-us/python/api/azureml-core/azureml.core.runconfig.runconfiguration?view=azure-ml-py) için sağlanan [ PythonScriptStep](https://docs.microsoft.com/en-us/python/api/azureml-pipeline-steps/azureml.pipeline.steps.python_script_step.pythonscriptstep?view=azure-ml-py). Varsayılan olarak, adımları çok sayıda eşzamanlı olarak çalıştırıldığında azaltma sorunları içine çalışabilir yedekleme veri deposu olarak Azure dosya depolama adımları kullanın.

### <a name="azure-portal"></a>Azure portal

+ **Yeni Özellikler**
  + Yeni bir Sürükle ve bırak Düzenleyicisi deneyimi raporlar için tablo. Kullanıcılar bir sütun tablonun önizlemesi nerede görüntülenecek tablo alanı sekme grubundan sürükleyebilirsiniz. Sütunları yeniden.
  + Yeni günlük dosyası Görüntüleyici
  + Çalıştırmaları, işlem, modelleri, görüntüleri ve dağıtımları etkinlikleri sekmesinden denemek için bağlantılar

### <a name="azure-machine-learning-data-prep-sdk-v1015"></a>Azure Machine Learning veri hazırlama SDK v1.0.15

+ **Yeni Özellikler**
  + Dosya yazma destekleyen bir veri akışı akışlardan artık veri hazırlama. Ayrıca yeni dosya adları oluşturmak için dosya akışı adları değiştirme olanağı sağlar.
    + Nasıl yapılır kılavuzunda: [Dosya akışları ile çalışmayı not defteri](https://aka.ms/aml-data-prep-file-stream-nb)

+ **Hata düzeltmeleri ve geliştirmeleri**
  + T-özetinin büyük veri kümeleri üzerinde performansı İyileştirildi.
  + Veri hazırlığı, artık bir DataPath okuma verileri destekler.
  + Sık erişimli bir kodlama artık Boole türü and sayısal sütunlarda çalışır.
  + Diğer çeşitli hata düzeltmeleri.

## <a name="2019-02-11"></a>2019-02-11

### <a name="azure-machine-learning-sdk-for-python-v1015"></a>Azure Machine SDK için Python v1.0.15 Learning

+ **Yeni Özellikler**
  + Azure Machine Learning işlem hatlarını AzureBatchStep eklendi ([not defteri](https://aka.ms/pl-azbatch)), HyperDriveStep ([not defteri](https://aka.ms/pl-hyperdrive)) ve zaman tabanlı işlevselliği zamanlama ([not defteri](https://aka.ms/pl-schedule)).
  +  PostgreSQL için Azure veritabanı ve Azure SQL Server ile çalışmak için DataTranferStep güncelleştirilir ([not defteri](https://aka.ms/pl-data-trans)).

+ **Değişiklikleri**
  + Kullanım dışı `PublishedPipeline.get_published_pipeline` sunulmasıyla `PublishedPipeline.get`.
  + Kullanım dışı `Schedule.get_schedule` sunulmasıyla `Schedule.get`.

### <a name="azure-machine-learning-data-prep-sdk-v1012"></a>Azure Machine Learning veri hazırlama SDK v1.0.12

+ **Yeni Özellikler**
  + Veri deposu kullanarak bir Azure SQL veritabanından okumayla veri hazırlığı destekler.
 
+ **Değişiklikleri**
  + Büyük veriler üzerinde belirli işlemlerin bellek performansını önemli ölçüde geliştirildi.
  + `read_pandas_dataframe()` artık ıntune hizmetini kullanabilmeleri `temp_folder` belirtilmelidir.
  + `name` Özelliği `ColumnProfile` olmuştur kullanım dışı - kullanın `column_name` yerine.

## <a name="2019-01-28"></a>2019-01-28

### <a name="azure-machine-learning-sdk-for-python-v1010"></a>Azure Machine SDK için Python v1.0.10 Learning

+ **Değişiklikleri**: 
  + Azure ML SDK'sı, bağımlılık olarak artık azure-cli paketleri vardır. Özellikle, azure CLI core ve azure CLI profili bağımlılıkları azureml çekirdekten kaldırıldı. Kullanıcının etkileyen değişiklikler şunlardır:
    + "Az login" gerçekleştirme ve ardından azureml-SDK'sını kullanarak, SDK'sı bir kez daha tarayıcı veya cihaz kodunu oturum açma yapın. Bunu, "az login" oluşturulan herhangi bir kimlik bilgileri durumu kullanmaz.
    + "Az login" komutunu kullanarak gibi Azure CLI kimlik doğrulaması kullanan _azureml.core.authentication.AzureCliAuthentication_ sınıfı. Azure CLI kimlik doğrulaması için yapmak _pip, azure CLI yükleme_ azureml sdk yüklediğiniz Python ortamında.
    + "Az login Otomasyon için bir hizmet sorumlusunu kullanarak" yapıyorsanız kullanmanızı öneririz _azureml.core.authentication.ServicePrincipalAuthentication_ azureml sdk, azure CLI tarafından oluşturulan kimlik bilgilerini durumu kullanmaz gibi sınıf. 

+ **Hata düzeltmeleri**: Bu sürüm, çoğunlukla küçük hata düzeltmeleri içerir

### <a name="azure-machine-learning-data-prep-sdk-v108"></a>Azure Machine Learning veri hazırlama SDK v1.0.8

+ **Hata düzeltmeleri**
  + Veri profilleri alma performansı önemli ölçüde geliştirildi.
  + Hata Raporlama ile ilgili küçük hata düzeltildi.
  
### <a name="azure-portal-new-features"></a>Azure portalı: yeni özellikler
+ Raporlar için yeni bir Sürükle ve bırak grafik deneyimi. Kullanıcılar bir sütun veya öznitelik sistem bir veri türüne göre kullanıcı için uygun grafik türü otomatik olarak nerede seçip grafik alanı için sekme grubundan sürükleyebilirsiniz. Kullanıcılar, diğer uygulanabilir türleri için grafik türünü değiştirmek veya ek öznitelikler ekleyin.

    Grafik türleri desteklenir:
    - Çizgi Grafiği
    - Histogram
    - Yığılmış çubuk grafik
    - Kutu Çizimi
    - Çizim Dağılım
    - Kabarcık çizimi
+ Portal şimdi raporlar denemeleri için dinamik olarak oluşturur. Bir kullanıcı için bir deneme çalıştırma gönderdiğinde, günlüğe kaydedilen Ölçümler ve grafikler karşılaştırma farklı çalıştırmaları arasında izin verecek şekilde otomatik olarak bir rapor oluşturulur. 

## <a name="2019-01-14"></a>2019-01-14

### <a name="azure-machine-learning-sdk-for-python-v108"></a>Azure Machine SDK için Python v1.0.8 Learning

+ **Hata düzeltmeleri**: Bu sürüm, çoğunlukla küçük hata düzeltmeleri içerir

### <a name="azure-machine-learning-data-prep-sdk-v107"></a>Azure Machine Learning veri hazırlama SDK v1.0.7

+ **Yeni Özellikler**
  + Veri deposu geliştirmeleri (belirtilmiştir [Yardım How-to veri deposu-Kılavuzu](https://aka.ms/aml-data-prep-datastore-nb))
    + Azure dosya paylaşımı ve ADLS veri depoları içinde ölçek büyütme yazma ve okuma özelliği eklendi.
    + Veri depoları kullanırken, veri hazırlığı artık bir hizmet sorumlusu kimlik doğrulaması yerine etkileşimli kimlik doğrulaması kullanarak destekler.
    + Wasb ve wasbs URL'ler için destek eklendi.

## <a name="2019-01-09"></a>2019-01-09

### <a name="azure-machine-learning-data-prep-sdk-v106"></a>Azure Machine Learning veri hazırlama SDK v1.0.6

+ **Hata düzeltmeleri**
  + Spark üzerinde okunabilir ortak Azure Blob kapsayıcıları okumaya ile ilgili hata düzeltildi

## <a name="2018-12-20"></a>2018-12-20 

### <a name="azure-machine-learning-sdk-for-python-v106"></a>Azure Machine SDK için Python v1.0.6 Learning
+ **Hata düzeltmeleri**: Bu sürüm, çoğunlukla küçük hata düzeltmeleri içerir

### <a name="azure-machine-learning-data-prep-sdk-v104"></a>Azure Machine Learning veri hazırlama SDK v1.0.4

+ **Yeni Özellikler**
  + `to_bool` işlevi, hata değerlerini dönüştürülecek eşleşmeyen değerlere artık izin verir. Yeni varsayılan uyuşmazlığı davranışı budur `to_bool` ve `set_column_types`, önceki davranışı False olarak eşleşmeyen değerlerini dönüştürmek için expectedhash.
  + Çağrılırken `to_pandas_dataframe`, sayısal sütunlardaki değerleri null/eksik NaN yorumlamak için yeni bir seçenek yoktur.
  + Bazı ifadelerin türü tutarlılığı sağlamak ve erken başarısızlık için dönüş türü denetlenecek özelliği eklendi.
  + Artık çağırabilirsiniz `parse_json` bir sütundaki değerleri JSON nesnesi olarak ayrıştırılamadı ve bunları birden çok sütuna genişletin.

+ **Hata düzeltmeleri**
  + Kilitlenen bir hata düzeltildi `set_column_types` Python 3.5.2 içinde.
  + Veri deposuna AML görüntüyü kullanarak bağlanırken kilitlenen bir hata düzeltildi.

+ **Güncelleştirmeler**
  * [Örnek Not Defterleri](https://aka.ms/aml-data-prep-notebooks) başlatılan öğreticiler, örnek olay incelemeleri ve nasıl yapılır kılavuzlarından alma.

## <a name="2018-12-04-general-availability"></a>2018-12-04: Genel Erişilebilirlik

Azure Machine Learning hizmeti genel kullanıma sunulmuştur.

### <a name="azure-machine-learning-compute"></a>Azure Machine Learning işlem
Bu sürümle birlikte, yeni bir yönetilen bilgi işlem deneyimi aracılığıyla duyuruyoruz [Azure Machine Learning işlem](how-to-set-up-training-targets.md#amlcompute). Bu işlem hedef Azure Machine Learning için Azure Batch AI işlem değiştirir. 

Bu işlem hedef:
+ Model eğitimi ve batch çıkarım için kullanılır
+ Tek - için çok - node işlem
+ Küme yönetimi yapar ve proje kullanıcı için planlama
+ Varsayılan olarak Daralttığında
+ Hem CPU hem de GPU kaynakları için destek 
+ Düşük öncelikli VM'ler daha az maliyet için kullanılmasına izin verir

Azure Machine Learning işlem, Azure portal veya CLI kullanarak Python'da oluşturulabilir. Çalışma alanınızın bölgesinde oluşturulmalıdır ve herhangi bir çalışma alanına bağlı olamaz. Bu işlem hedef çalıştırmak için bir Docker kapsayıcısı kullanır ve aynı ortamdaki tüm düğümleri arasında çoğaltılmasını bağımlılıklarınızı paketler.

> [!Warning]
> Azure Machine Learning işlem kullanmak için yeni bir çalışma alanı oluşturmanızı öneririz. Kullanıcılar Azure Machine Learning işlem mevcut bir çalışma alanından oluşturulmaya çalışılırken bir hata görebilirsiniz uzak bir fırsat yoktur. Mevcut işlem çalışma alanınızdaki etkilenmeyen çalışmaya devam etmesi gerekir.

### <a name="azure-machine-learning-sdk-for-python-v102"></a>Azure Machine SDK için Python v1.0.2 Learning
+ **Bozucu değişiklikler**
  + Bu sürümle birlikte, Azure Machine Learning ile VM oluşturmaya yönelik destek kaldırıyoruz. Veya uzak bir şirket içi sunucusu hala var olan bir buluta VM ekleyebilirsiniz. 
  + Ayrıca, tüm Azure Machine Learning işlem artık desteklenmelidir BatchAI, desteği kaldırıyoruz.

+ **Yeni**
  + Machine learning işlem hatları için:
    + [EstimatorStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.estimator_step.estimatorstep?view=azure-ml-py)
    + [HyperDriveStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.hyper_drive_step.hyperdrivestep?view=azure-ml-py)
    + [MpiStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.mpi_step.mpistep?view=azure-ml-py)


+ **Güncelleştirildi**
  + Machine learning işlem hatları için:
    + [DatabricksStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.databricks_step.databricksstep?view=azure-ml-py) artık runconfig kabul eder
    + [DataTransferStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.data_transfer_step.datatransferstep?view=azure-ml-py) için ve SQL veri kaynağı artık kopyalar
    + Zamanlama oluşturma ve güncelleştirme zamanlamaları yayımlanan işlem hatları çalıştırmak için SDK'sındaki işlevleri

<!--+ **Bugs fixed**-->

### <a name="azure-machine-learning-data-prep-sdk-v052"></a>Azure Machine Learning veri hazırlama SDK v0.5.2
+ **Bozucu değişiklikler** 
  * `SummaryFunction.N` yeniden adlandırıldı `SummaryFunction.Count`.
  
+ **Hata düzeltmeleri**
  * En son AML çalıştırma okuma ve veri depoları için uzaktan çalıştırmalar yazarken belirteci kullanın. Python'da AML çalıştırma belirteci güncelleştirilir, daha önce veri hazırlığı çalışma zamanı güncelleştirilmiş AML çalıştırma belirteci ile güncelleştirilmez.
  * NET ek hata iletileri
  * to_spark_dataframe() artık kilitlenme Spark kullandığında `Kryo` seri hale getirme
  * Inspector değer sayısı 1000'den fazla benzersiz değerler artık gösterebilirsiniz
  * Özgün veri akışı bir adı yoksa rastgele bir bölünmüş artık başarısız  

+ **Daha fazla bilgi**
  * [Azure Machine Learning Veri Hazırlama SDK'sı](https://aka.ms/data-prep-sdk)

### <a name="docs-and-notebooks"></a>Belgeleri ve Not Defterleri
+ ML işlem hatları
  + İşlem hatları, batch kapsamlar ve stil ile çalışmaya başlama için yeni ve güncelleştirilmiş not defterlerini örnekler aktarın: https://aka.ms/aml-pipeline-notebooks
  + Bilgi edinmek için nasıl [ilk işlem hattınızı oluşturun](how-to-create-your-first-pipeline.md)
  + Bilgi edinmek için nasıl [komut zincirlerini kullanarak batch Öngörüler çalıştırın](how-to-run-batch-predictions.md)
+ Azure Machine Learning işlem hedefi
  + [Örnek Not Defterleri](https://aka.ms/aml-notebooks) artık yeni yönetilen bir işlem kullanacak şekilde güncelleştirildi.
  + [Bu işlem hakkında bilgi edinin](how-to-set-up-training-targets.md#amlcompute)

### <a name="azure-portal-new-features"></a>Azure portalı: yeni özellikler
+ Oluşturma ve yönetme [Azure Machine Learning işlem](how-to-set-up-training-targets.md#amlcompute) portalında türleri.
+ Kota kullanımını izlemek ve [isteği kota](how-to-manage-quotas.md) Azure Machine Learning işlemi için.
+ Azure Machine Learning işlem küme durumunu gerçek zamanlı olarak görüntüleyin.
+ Azure Machine Learning işlem ve Azure Kubernetes hizmeti oluşturmak için sanal ağ desteği eklendi.
+ Yayımlanan hatlarınızı mevcut parametrelerle yeniden çalıştırın.
+ Yeni [machine learning grafikleri otomatik](how-to-track-experiments.md#auto) sınıflandırma modellerini (lift, Kazanç, ayarlama, özellik önem modeli explainability grafikle) ve regresyon modellerini (Kalanlar ve modeli ile özellik önem grafiği explainability). 
+ İşlem hatları Azure portalında görüntülenebilir




## <a name="2018-11-20"></a>2018-11-20

### <a name="azure-machine-learning-sdk-for-python-v0180"></a>Azure Machine SDK için Python v0.1.80 Learning

+ **Bozucu değişiklikler** 
  * *azureml.Train.widgets* ad alanı taşındığı *azureml.widgets*.
  * *azureml.core.compute.AmlCompute* aşağıdaki sınıflar - kullanımdan kaldırıldı *azureml.core.compute.BatchAICompute* ve *azureml.core.compute.DSVMCompute*. İkinci sınıfı sonraki sürümlerde kaldırılacak. AmlCompute sınıfı artık daha kolay bir tanıma sahip yalnızca bir vm_size ve max_nodes gerekir ve bir iş gönderildiğinde kümenize 0 max_nodes otomatik olarak ölçeklendirir. Bizim [örnek not defterleri](https://github.com/Azure/MachineLearningNotebooks/tree/master/training) bu bilgilerle güncelleştirildi ve kullanım örnekleri vermeniz gerekir. Bu basitleştirme ve çok daha heyecan verici özelliklerin bir sonraki sürümde gelir gibi umuyoruz!

### <a name="azure-machine-learning-data-prep-sdk-v051"></a>Azure Machine Learning veri hazırlama SDK v0.5.1 

Veri hazırlığı SDK'sı hakkında daha fazla bilgi edinmek [başvuru belgeleri](https://aka.ms/data-prep-sdk).
+ **Yeni Özellikler**
   * DataPrep paketleri çalıştırmak ve bir veri kümesi veya veri akışı veri profilini görüntülemek için yeni bir DataPrep CLI oluşturdunuz
   * Kullanılabilirliği iyileştirmek için tasarlanan SetColumnType API
   * Yeniden adlandırılan smart_read_file auto_read_file için
   * Şimdi veri profilinde sapması ve basıklık içerir
   * İle stratified örnekleme örnek oluşturabilirsiniz
   * CSV dosyaları içeren zip dosyaları okuyabilir
   * Rastgele bölünmüş row-wise kümeleriyle (örneğin,. test train kümeleri halinde) bölebilirsiniz.
   * Tüm sütun veri türlerini bir veri akışı veri profili çağırarak alabilir veya `.dtypes`
   * Satır sayısı bir veri akışı veri profili çağırarak alabilir veya `.row_count`

+ **Hata düzeltmeleri**
   * Uzun çift dönüştürme sabit 
   * Sabit sütun ekleyin, sonra onaylama 
   * Grupları nereden bazı durumlarda algılaması değil, FuzzyGrouping ile bir sorun düzeltildi
   * Birden çok sütunu sıralamayı cevaben sabit bir sıralama işlevi
   * Sabit ve/veya nasıl benzer ifadeler `pandas` bunları işler
   * Dbfs yolundan sabit okuma
   * Hata iletileri daha anlaşılır hale 
   * AML belirteci kullanarak uzak işlem hedefte okurken şimdi artık başarısız
   * Artık üzerinde Linux DSVM'sini artık başarısız
   * Dize olmayan değerler dize koşullarda şimdi artık çöküyor
   * Artık doğru şekilde veri akışı başarısız olursa, onaylama hatalarını ele alır
   * Artık Azure Databricks'te dbutils bağlı depolama konumları destekler

## <a name="2018-11-05"></a>2018-11-05

### <a name="azure-portal"></a>Azure portal 
Azure Machine Learning hizmeti için Azure portalı aşağıdaki güncelleştirmeler bulunur:
  * Yeni bir **işlem hatları** yayımlanan işlem hatları için sekmesinde.
  * İşlem hedefi olarak var olan bir HDInsight kümesine ekleme desteği eklendi.

### <a name="azure-machine-learning-sdk-for-python-v0174"></a>Azure Machine SDK için Python v0.1.74 Learning

+ **Bozucu değişiklikler** 
  * * Workspace.compute_targets, veri depoları, denemeleri, resimler, modeller ve *webservices'a* yöntemleri yerine özellikler. Örneğin, *Workspace.compute_targets()* ile *Workspace.compute_targets*.
  * *Run.get_context* kullanımdan kaldırıldı *Run.get_submitted_run*. İkinci yöntem, sonraki sürümlerde kaldırılacak.
  * *PipelineData* sınıfı bir parametre yerine olarak datastore_name bir veri deposu nesnesi artık bekliyor. Benzer şekilde, *işlem hattı* default_datastore_name yerine default_datastore kabul eder.

+ **Yeni Özellikler**
  * Azure Machine Learning işlem hatlarını [örnek not defteri](https://github.com/Azure/MachineLearningNotebooks/tree/master/pipeline/pipeline-mpi-batch-prediction.ipynb) artık MPI adımları kullanır.
  * Jupyter not defterleri için RunDetails pencere, işlem hattının görselleştirilmesi göstermek için güncelleştirilir.

### <a name="azure-machine-learning-data-prep-sdk-v040"></a>Azure Machine Learning veri hazırlama SDK v0.4.0 
 
+ **Yeni Özellikler**
  * Veri profili türü sayısı eklendi 
  * Değer sayısı ve Histogram kullanıma sunuldu
  * Daha fazla yüzdebirliklerini veri profilinde
  * ORTANCA özetleme kullanılabilir
  * Python 3.7 artık desteklenmektedir
  * Veri depoları DataPrep paketi içeren bir veri akışı kaydettiğinizde, veri deposu bilgileri DataPrep paketinin bir parçası sürdürülecek
  * Veri deposu yazma artık desteklenmektedir 
        
+ **Hata düzeltildi**
  * 64-bit işaretsiz tamsayı taşma artık düzgün bir şekilde Linux üzerinde işlenir
  * Düz metin dosyalarında smart_read hatalı sabit metin etiketi
  * Dize sütun türü artık ölçümleri görünümünde gösterilir
  * Türü sayısı artık tek tek olanları yerine tek FieldType eşlenen ValueKinds gösterecek şekilde düzeltildi
  * Bir dize olarak yol sağlandığında Write_to_csv artık başarısız
  * Replace kullanırken, "boş bulma" bırakarak artık başarısız olur 

## <a name="2018-10-12"></a>2018-10-12

### <a name="azure-machine-learning-sdk-for-python-v0168"></a>Azure Machine SDK için Python v0.1.68 Learning

+ **Yeni Özellikler**
  * Yeni çalışma alanı oluştururken, çok kiracılı desteği.

+ **Düzeltilen hatalar**
  * Artık web hizmetini dağıtırken pynacl kitaplığı sürüm sabitlemek gerekir.

### <a name="azure-machine-learning-data-prep-sdk-v030"></a>Azure Machine Learning veri hazırlama SDK v0.3.0

+ **Yeni Özellikler**
  * Kullanıcıların yürütmek için bir Python dosyası yolunda geçirilecek yöntemi transform_partition_with_file(script_path) eklendi

## <a name="2018-10-01"></a>2018-10-01

### <a name="azure-machine-learning-sdk-for-python-v0165"></a>Azure Machine SDK için Python v0.1.65 Learning
[Sürüm 0.1.65](https://pypi.org/project/azureml-sdk/0.1.65) yeni özellikler, daha fazla belge, hata düzeltmeleri ve daha fazlasını içeren [örnek not defterleri](https://aka.ms/aml-notebooks).

Bkz: [bilinen sorunların listesi](resource-known-issues.md) bilinen hataların ve geçici çözümleri hakkında bilgi edinmek için.

+ **Bozucu değişiklikler**
  * Workspace.experiments, Workspace.models, Workspace.compute_targets, Workspace.images, Workspace.web_services dönüş sözlük, daha önce listesi döndürdü. Bkz: [azureml.core.Workspace](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace(class)?view=azure-ml-py) API belgeleri.

  * Machine Learning otomatik normalleştirilmiş Ortalama kare hata birincil ölçümleri kaldırıldı.

+ **HyperDrive**
  * Bayes çeşitli HyperDrive hata düzeltmeleri, ölçümleri çağrıları için performans geliştirmeleri alın. 
  * Tensorflow 1.10 1.9 sürümüne yükseltme 
  * Docker görüntü iyileştirmesi hazırlıksız başlatma için. 
  * Bunlar dışında 0 hata kodu ile çıkmak bile işleri artık doğru durum rapor. 
  * Öznitelik doğrulamada SDK RunConfig. 
  * Çalıştırma HyperDrive nesnesi iptal normal çalışmaya benzer destekler: herhangi bir parametre geçirin gerek yoktur. 
  * Dağıtılmış çalıştırır ve HyperDrive çalıştırmalar için aşağı açılan değerleri durumunu korumak için pencere öğesi geliştirmeleri. 
  * Parametre sunucusu için sabit, TensorBoard ve diğer günlük dosyalarını destekler. 
  * Hizmet tarafı Intel(r) MPI desteği. 
  * Hata düzeltmesi, için parametre ayarlama için düzeltme çalışma BatchAI doğrulama sırasında dağıtılmış. 
  * Bağlam Yöneticisi artık birincil tanıtır. 

+ **Azure portal deneyimi**
  * Çalıştırma ayrıntıları log_table() ve log_row() desteklenir. 
  * Otomatik olarak 1, 2 veya 3 sayısal sütunlara ve isteğe bağlı bir kategorik sütun tabloları ve satırları için grafikler oluşturun.

+ **Otomatik makine öğrenimi**
  * Geliştirilmiş hata işleme ve belgeler 
  * Çalıştırma özelliği alımı performans sorunlarını düzelttik. 
  * Sabit çalışma sorunu devam edin. 
  * Ensembling yineleme sorunlar düzeltildi.
  * MAC OS sabit eğitim asılı hata.
  * Aşağı örnekleme makrosu ortalama çekme isteği/ROC eğrisi özel doğrulama senaryosunda.
  * Ek dizin mantıksal kaldırıldı.
  * Filtre get_output API ' kaldırıldı.

+ **İşlem hatları**
  * Bir yöntem bir işlem hattı yürütme ilk çalıştırma gerek kalmadan doğrudan yayımlamak için Pipeline.publish() eklendi.   
  * Yayımlanan bir ardışık düzen tarafından oluşturulan işlem hattı çalıştırmaları getirilecek PipelineRun.get_pipeline_runs() bir yöntem eklendi.

+ **Project Brainwave**
  * Yeni AI modelleri FPGA üzerinde kullanılabilir güncelleştirilmiş destek.

### <a name="azure-machine-learning-data-prep-sdk-v020"></a>Azure Machine Learning veri hazırlama SDK v0.2.0
[Sürüm 0.2.0](https://pypi.org/project/azureml-dataprep/0.2.0/) özellikler ve hata düzeltmeleri içerir:

+ **Yeni Özellikler**
  * Bir sık erişimli kodlaması için destek
  * Quantile dönüştürme için destek
   
+ **Hata düzeltildi:**
  * Herhangi bir hortum sürümü ile çalışır hortum sürümünüzü düşürme gerekmez.
  * Değer için tüm değerleri, yalnızca ilk üç sayar.

## <a name="2018-09-public-preview-refresh"></a>2018-09 (genel Önizleme yenileme)

Yeni bir Azure Machine Learning sürümü yenilendi: Bu sürüm hakkında daha fazlasını okuyun: https://azure.microsoft.com/blog/what-s-new-in-azure-machine-learning-service/


## <a name="next-steps"></a>Sonraki adımlar

Genel bakışı okuyun [Azure Machine Learning hizmeti](../service/overview-what-is-azure-ml.md).
