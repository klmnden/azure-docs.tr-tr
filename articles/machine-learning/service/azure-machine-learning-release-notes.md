---
title: Sürümde yenilikler nelerdir?
titleSuffix: Azure Machine Learning service
description: Azure Machine Learning hizmeti ve makine öğrenimi için en son güncelleştirmeleri öğrenin ve Python SDK'ları veri hazırlama.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: reference
author: hning86
ms.author: haining
ms.reviewer: j-martens
ms.date: 12/20/2018
ms.custom: seodec18
ms.openlocfilehash: a7a15e4cd8670e71e1000bc6b1827a4b9292302b
ms.sourcegitcommit: d4f728095cf52b109b3117be9059809c12b69e32
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/10/2019
ms.locfileid: "54200624"
---
# <a name="azure-machine-learning-service-release-notes"></a>Azure Machine Learning hizmeti sürüm notları

Bu makalede, Azure Machine Learning hizmet sürümleri hakkında bilgi edinin. 

## <a name="2019-01-09"></a>2019-01-09

### <a name="azure-machine-learning-data-prep-sdk-v106"></a>Azure Machine Learning veri hazırlama SDK v1.0.6

+ **SDK başvuru belgeleri**: https://aka.ms/data-prep-sdk

+ **Hata düzeltmeleri**
  + Spark üzerinde okunabilir ortak Azure Blob kapsayıcıları okumaya ile ilgili hata düzeltildi

## <a name="2018-12-20"></a>2018-12-20 

### <a name="azure-machine-learning-sdk-for-python-v106"></a>Azure Machine SDK için Python v1.0.6 Learning

+ **SDK başvuru belgeleri**: https://aka.ms/aml-sdk

+ **Hata düzeltmeleri**: Bu sürüm, çoğunlukla küçük hata düzeltmeleri içerir

### <a name="azure-machine-learning-data-prep-sdk-v104"></a>Azure Machine Learning veri hazırlama SDK v1.0.4

+ **SDK başvuru belgeleri**: https://aka.ms/data-prep-sdk

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
  + [Örnek not defterleri] (https://aka.ms/aml-notebooks) artık yeni yönetilen bir işlem kullanacak şekilde güncelleştirildi.
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
  * *azureml.core.compute.AmlCompute* aşağıdaki sınıflar - kullanımdan kaldırıldı *azureml.core.compute.BatchAICompute* ve *azureml.core.compute.DSVMCompute*. İkinci sınıfı sonraki sürümlerde kaldırılacak. AmlCompute sınıfı artık daha kolay bir tanıma sahip yalnızca bir vm_size ve max_nodes gerekir ve bir iş gönderildiğinde kümenize 0 max_nodes otomatik olarak ölçeklendirir. Bizim [örnek not defterleri] (https://github.com/Azure/MachineLearningNotebooks/tree/master/training) bu bilgilerle güncelleştirildi ve kullanım örnekleri vermeniz gerekir. Bu basitleştirme ve çok daha heyecan verici özelliklerin bir sonraki sürümde gelir gibi umuyoruz!

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
