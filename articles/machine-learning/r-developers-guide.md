---
title: R Geliştirici Kılavuzu-Azure - R programlama diline | Microsoft Docs
description: Bu makalede, programlama dili azure'da veri bilimcileri, mevcut becerilerini R ile yararlanabileceğiniz çeşitli yollarına genel bakış sağlar. Azure, R geliştiricilerin veri bilimi iş yüklerini buluta genişletmek için yararlanabileceğiniz birçok hizmet sunar.
services: machine-learning
documentationcenter: ''
author: AnalyticJeremy
manager: cgronlun
editor: cgronlun
ms.assetid: ''
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: R
ms.topic: article
ms.date: 09/12/2018
ms.author: jepeach
ms.openlocfilehash: 70fc78fb515c56f0b3102bb006eb6491a664babd
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/18/2019
ms.locfileid: "57886697"
---
# <a name="r-developers-guide-to-azure"></a>Azure için R Geliştirici Kılavuzu
<img src="media/r-developers-guide/logo_r.svg" alt="R logo" align="right" width="200" />

Sürekli artan ile ilgili birçok veri uzmanları veri hacimleri için kendi analizleri bulut gücünden yollarını arıyor.  Bu makale, mevcut becerilerini ile veri bilimcileri yararlanan çeşitli yollarına genel bakış sağlar [R programlama dili](https://www.r-project.org) azure'da.

Microsoft tam olan R programlama dili birinci sınıf bir araç veri uzmanları için benimsemekteydi.  Kodlarını Azure'da çalıştırmak R geliştiricileri için birçok farklı seçenekler sunarak, şirket veri Bilim büyük ölçekli projeler bağlayabileceğiniz veri bilimi iş yüklerini buluta doğru genişletmenize imkan verir.

Çeşitli seçenekler ve her biri için en ilgi çekici senaryoları inceleyelim.

## <a name="azure-services-with-r-language-support"></a>Azure Hizmetleri ile R dil desteği
Bu makalede, R dil desteği aşağıdaki Azure Hizmetleri yer almaktadır:

|Hizmet                                                          |Açıklama                                                                       |
|-----------------------------------------------------------------|----------------------------------------------------------------------------------|
|[Veri bilimi sanal makinesi](#data-science-virtual-machine)    |bir veri bilimi iş istasyonu veya özel işlem hedefi olarak kullanmak için özelleştirilmiş bir VM|
|[HDInsight üzerinde ML Hizmetleri](#ml-services-on-hdinsight)            |R analizleri büyük veri kümelerinde çok sayıda düğümleri arasında çalışan küme tabanlı sistem   |
|[Azure Databricks](#azure-databricks)                            |R ve diğer dilleri destekleyen işbirliğine dayalı bir Spark ortam               |
|[Azure Machine Learning Studio](#azure-machine-learning-studio)  |Azure'nın makine öğrenimi denemelerini özel R betikleri çalıştırma                      |
|[Azure Batch](#azure-batch)                                      |çeşitli ekonomik bir kümede çok düğüm arasında R kodunu çalıştırma seçenekleri sunar.|
|[Azure Notebooks](#azure-notebooks)                              |bulut tabanlı bir ücretsiz sürüm, Jupyter Not Defterleri                  |
|[Azure SQL Veritabanı](#azure-sql-database)                        |SQL Server veritabanı altyapısı içinde R betikleri çalıştırma                            |

## <a name="data-science-virtual-machine"></a>Veri Bilimi Sanal Makinesi
[Veri bilimi sanal makinesi](https://docs.microsoft.com/azure/machine-learning/data-science-virtual-machine/overview) (DSVM) olan Microsoft Azure bulut platformunda yerleşik özellikle veri bilimi için özelleştirilmiş bir VM görüntüsü. Dahil olmak üzere birçok popüler veri bilimi araçlarını içerir:
* [Microsoft R Open](https://mran.microsoft.com/open/)
* [Microsoft Machine Learning Server](https://docs.microsoft.com/machine-learning-server/what-is-machine-learning-server)
* [RStudio Desktop](https://www.rstudio.com/products/rstudio/#Desktop)
* [RStudio Server](https://www.rstudio.com/products/rstudio/#Server)

DSVM'nin Windows veya Linux işletim sistemi olarak sağlanabilir.  DSVM iki farklı şekilde kullanabilirsiniz: etkileşimli bir iş istasyonu veya özel bir küme için bir bilgi işlem platformu olarak.

### <a name="as-a-workstation"></a>Bir iş istasyonu olarak
R bulutta hızlı ve kolay bir şekilde kullanmaya başlamak istiyorsanız, en iyi sonucu budur.  Ortam R ile yerel iş istasyonunda çalışmıştır herkese tanıdık gelecektir.  Ancak, yerel kaynakları kullanmak yerine, R bulutta bir VM'de bir ortamdır.  Verilerinizi Azure'da kaydedildiyse, bu verme R betiklerinizi "verileri daha yakın." çalıştırmak eklenen avantajına sahiptir. Verileri Internet üzerinden aktarmak yerine, verileri daha hızlı bir şekilde erişim zamanları sağlayan Azure'nın iç ağ üzerinden erişilebilir.

DSVM R geliştiriciler oluşan küçük ekipler için özellikle yararlı olabilir.  Her geliştirici, güçlü iş istasyonlarında bulunan her geliştirici için yatırım ve takım üyelerinin hangi sürümlerine ilişkin çeşitli yazılım paketlerini kullanacakları eşitleme gerektiren yerine gerektiğinde DSVM örneği yavaşlatabilir.

### <a name="as-a-compute-platform"></a>Bir bilgi işlem platformu
Bir iş istasyonu olarak kullanılan yanı sıra DSVM de bir esnek bir şekilde ölçeklenebilir bilgi işlem platformu R projeleri için kullanılır.  Kullanarak <code>[AzureDSVM](https://github.com/Azure/AzureDSVM)</code> R paketi oluşturma ve silme DSVM örneklerinin programlı olarak denetleyebilirsiniz.  Bir küme örneği oluşturmak ve bulutta gerçekleştirilecek bir dağıtılmış analiz dağıtın.  Bu işlemi, yerel iş istasyonunuzda çalışan R kodu tarafından denetlenebilir.

DSVM hakkında daha fazla bilgi için bakın ["Giriş Azure veri bilimi sanal makinesi için Linux ve Windows."](https://docs.microsoft.com/azure/machine-learning/data-science-virtual-machine/overview)

## <a name="ml-services-on-hdinsight"></a>HDInsight üzerindeki ML Services
[Microsoft ML Hizmetleri](https://docs.microsoft.com/azure/hdinsight/r-server/r-server-overview) HDInsight üzerinde analitik yöntemlerine Dağıtılmış veri uzmanlarının, istatistikçilerin ve R programcılarının ölçeklenebilir, isteğe bağlı erişim sağlar.  Bu çözüm, Azure Blob veya Data Lake depolama birimine yüklenen, neredeyse her boyuttaki veri kümelerinde R tabanlı analiz için en son özellikleri sağlar.

Bu bir küme genelinde R kodunuzu ölçeklendirme olanak tanıyan bir kurumsal düzeyde çözümdür.  Microsoft'un işlevlerde yararlanarak
<code>[RevoScaleR](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/revoscaler)</code> Paket, HDInsight üzerinde R betikleriniz veri işleme işlevleri paralel olarak kümedeki birçok düğümde çalıştırabilirsiniz.  Bu R işlemden geçirin veriler tek iş parçacıklı R ile mümkün olandan çok daha büyük bir ölçek üzerinde çalışan bir iş istasyonunda sağlar.

Ölçeklendirme olanağı ML Hizmetleri HDInsight üzerinde R geliştiriciler ile büyük hacimli veri kümeleri için harika bir seçenek yapar.  R betiklerinizi bulutta çalıştırmak için esnek ve ölçeklenebilir bir platform sağlar.

ML Hizmetleri kümesi oluşturma talimatları için bkz. ["Azure HDInsight üzerinde ML hizmetleri kullanmaya başlama"](https://docs.microsoft.com/azure/hdinsight/r-server/r-server-get-started) makalesi.

## <a name="azure-databricks"></a>Azure Databricks
[Azure Databricks](https://azure.microsoft.com/services/databricks/), Microsoft Azure bulut hizmetleri platformu için iyileştirilen Apache Spark tabanlı bir analiz platformudur.  Apache Spark’ın kurucuları ile birlikte tasarlanan Databricks, tek tıklama ile kurulum olanağı ve kolaylaştırılmış iş akışlarının yanı sıra veri uzmanları, veri mühendisleri ve iş analistleri arasında işbirliği sağlayan etkileşimli bir çalışma alanı sunmak amacıyla Azure ile tümleştirilmiştir.

Databricks'te işbirliği platformun dizüstü bilgisayar sistemi tarafından etkinleştirilir.  Kullanıcılar oluşturabilir, paylaşma ve Not Defterleri sistemleri diğer kullanıcılarla düzenleyin.  Bu not defterlerini Spark kümeleri Databricks ortamında yönetilen karşı yürütülen kodu yazmak kullanıcıların izin verin.  Bu not defterlerini tam olarak R desteği ve kullanıcılar için Spark hem erişmesini `SparkR` ve `sparklyr` paketleri.

Databricks Spark üzerinde kurulmuştur ve üzerinde işbirliği güçlü odaklı olduğundan, platform genellikle büyük veri kümelerinde karmaşık analizlerini üzerinde birlikte çalışarak veri bilimcileri, takımlar tarafından kullanılır.  Databricks not defterlerinde R yanı sıra diğer diller desteklediğinden, burada analistleri farklı diller için birincil iş kullanan ekipler için özellikle yararlıdır.

Makaleyi ["Azure Databricks nedir?"](https://docs.microsoft.com/azure/azure-databricks/what-is-azure-databricks)
başlamanıza yardımcı olur ve platformu hakkında daha fazla ayrıntı sağlar.

## <a name="azure-machine-learning-studio"></a>Azure Machine Learning Studio
[Azure Machine Learning studio](https://azure.microsoft.com/services/machine-learning-studio/) derleme, test etme ve bulutta Tahmine dayalı analiz çözümlerini dağıtmak için kullanabileceğiniz bir işbirliğine dayalı sürükle ve bırak aracıdır.  Bu, oluşturmak ve makine öğrenimi modelleri kadar kod yazmak zorunda kalmadan dağıtmak ortaya çıkan veri bilimcileri sağlar.

Azure Machine Learning studio, hem R ve Python destekler.  Azure Machine Learning studio iki yolla ile R kullanabilirsiniz.

### <a name="custom-r-scripts-in-your-experiments"></a>Denemelerinizi özel R betiklerini
İlk olarak, veri işleme ve makine öğrenimi özellikleri ML Studio özel R betikleri yazarak genişletebilirsiniz.
ML Studio çok çeşitli hazırlama ve veri çözümlemesi için modüller içerir ancak olgun bir dil R'dir gibi yeteneklerini eşleşemez  Bu nedenle, hizmet, kendi özel R betikleri burada belirtilen modülleri karşılamıyor durumlarda tanıtmak için gereksinimlerinizi olanak sağlamak için tasarlanmıştır.

Bu özellikten yararlanmak için sürükleyip denemenize "R betiği yürütme" modülü bırakın.  Ardından Kod Düzenleyicisi "Özellikler" bölmesinde yeni bir R betik yazma veya mevcut bir komut dosyasının yapıştırmak için kullanın.  Komut dosyası içinde dış R paketleri başvuruda bulunabilir.  Betik verileri işlemek için ya da standart Azure Machine Learning studio model kitaplığının parçası olmayan karmaşık ML modelleri eğitmek için kullanabilirsiniz.

ML Studio denemeleri içinde R kullanarak kapsamlı giriş için kullanıma ["Olan R programlama dili için Azure Machine Learning studio için hızlı başlangıç Öğreticisi."](https://docs.microsoft.com/azure/machine-learning/studio/r-quickstart)

### <a name="create-manage-and-deploy-experiments-from-your-local-r-environment"></a>Oluşturma, yönetme ve yerel R ortamınızda denemelerle dağıtma
Azure Machine Learning studio ile R kullanabileceğiniz başka bir şekilde kullanmaktır
<code>[AzureML](https://cran.r-project.org/web/packages/AzureML/vignettes/getting_started.html)</code> R programlama ortamı ile deneme işlemini denetlemek ve izlemek için paket.  Microsoft tarafından korunur, bu paket karşıya yükleme ve indirme veri kümelerine izin verir ve Azure Machine Learning hizmetinden R yayımlama denemeleri, aracıyı sorgulamak için studio, web Hizmetleri olarak çalışır ve R çalıştırmak için mevcut aracılığıyla veri web hizmetleri ve almak çıktı.

Bu paketi Azure Machine Learning studio için R kodunuzu ölçeklenebilir dağıtım platformu olarak kullanmak çok daha kolay hale getirir.  ' I tıklatarak ve sürükleyerek kullanıcı Arabiriminde yerine zaten bildiğiniz araçları kullanarak tüm dağıtım işlemini otomatik hale getirebilirsiniz.

## <a name="azure-batch"></a>Azure Batch
Büyük ölçekli R işleri için kullanabileceğiniz [Azure Batch](https://azure.microsoft.com/services/batch/).  Bu hizmet bulut ölçeğinde iş zamanlaması ve işlem yönetimi sağlar. böylece, R iş yükünüz onlarca, yüzlerce veya binlerce sanal makine arasında ölçeklendirebilirsiniz.  Yok, genelleştirilmiş bir bilgi işlem platformu olduğundan, birkaç seçenek Azure Batch'te R işlerini çalıştırmak için.

Bir seçenek, Microsoft'un kullanmaktır <code>[doAzureParallel](https://github.com/Azure/doAzureParallel)</code> paket.  Bu bir R paketi için bir paralel arka uç olan `foreach` paket.  Her bir yinelemesini sağlayan `foreach` Azure Batch kümedeki bir düğümde paralel olarak çalıştırmak için döngü.  Paket için bir giriş için lütfen okuyun ["doAzureParallel: Azure'un esnek işlem doğrudan R oturumunuzda yararlanmak"](https://azure.microsoft.com/blog/doazureparallel/) blog gönderisi.

Azure Batch hizmetinde bir R betiğini çalıştırmak için başka bir seçenek, Azure portalında bir Batch uygulaması olarak "RScript.exe" kodunuzla paket oluşturmaktır.  Ayrıntılı bir Rehber için başvurun ["R iş yüklerini Azure Batch."](https://azure.microsoft.com/blog/r-workloads-on-azure-batch/)

Üçüncü bir seçenek kullanmaktır [Azure Dağıtılmış veri Mühendisliği Araç Seti](https://github.com/Azure/aztk) (AZTK) olanak tanıyan, isteğe bağlı Spark kümeleri Azure Batch'te Docker kapsayıcılarını kullanarak sağlama.  Bu, Spark işlerini Azure'da çalıştırmak için ekonomik bir yol sağlar.  Kullanarak [AZTK ile SparklyR](https://github.com/Azure/aztk/wiki/SparklyR-on-Azure-with-AZTK), R betiklerinizi bulutta kolayca ve ekonomik bir şekilde genişletilebilir.

## <a name="azure-notebooks"></a>Azure Notebooks

[Azure not defterleri](https://notebooks.azure.com) için Not Defterleri ile çalışmayı tercih ettiğiniz R geliştiricileri kendi kodlarını Azure'a taşımalarına için düşük maliyetli, düşük uyuşmazlıkları yöntemidir.  Herkesin geliştirin ve kendi tarayıcı kullanarak kodu çalıştırmak için ücretsiz bir hizmet olduğundan [Jupyter](https://jupyter.org/), markdown prose tarayan ve yürütülebilir kod grafik tek bir tuvale sağlayan bir açık kaynak projesi olan.

4 GB bellek ve 1 GB veri kümelerinin her not defterinin işleme sınırları gibi Azure not defterleri ücretsiz hizmet katmanına küçük ölçekli projeler için uygun bir seçenektir. Ancak, işlem ve veri güç bu sınırlamaların ötesine gerekiyorsa, bir veri bilimi sanal makinesi örneğinde not defterlerini çalıştırabilirsiniz. Daha fazla bilgi için [yönetme ve Azure not defterleri projeleri - bilgi işlem katmanı yapılandırma](/azure/notebooks/configure-manage-azure-notebooks-projects#compute-tier).

## <a name="azure-sql-database"></a>Azure SQL Database
[Azure SQL veritabanı](https://azure.microsoft.com/services/sql-database/) Microsoft'un akıllı, tam olarak yönetilen bir ilişkisel bulut veritabanı hizmetidir.  SQL Server'ın gücünden altyapı kurulumu bir çaba harcamalarına gerek kalmadan kullanmanıza olanak sağlar.  Bu içerir [Machine Learning Hizmetleri](https://docs.microsoft.com/sql/advanced-analytics/what-is-sql-server-machine-learning?view=sql-server-2017), SQL Hizmeti daha yeni eklemeler birinde.

Bu özellik, bir SQL Server veritabanı içinde R kod saklı yordamlar, R deyimleri içeren T-SQL betikleri veya T-SQL içeren R kod olarak yürütülen bir katıştırılmış, Tahmine dayalı analiz ve veri bilimi altyapısı sunar.  Veritabanından verileri ayıklayarak ve R ortamına yükleniyor, yerine R kodunuzu doğrudan veritabanına yükleyin ve sağa verileri çalışmasına olanak tanır.

Machine Learning Services 2016'dan sonra şirket içi SQL Server'ın bir parçası olmuştur, ancak Azure SQL veritabanı'na nispeten yeni bir özelliktir.  Şu anda [sınırlı Önizleme](https://docs.microsoft.com/sql/advanced-analytics/what-s-new-in-sql-server-machine-learning-services?view=sql-server-2017#azure-sql-database-roadmap) ancak gelişmeye devam eder.


### <a name="next-steps"></a>Sonraki adımlar
* [R kodunuzu mrsdeploy ile Azure üzerinde çalışan](https://blog.revolutionanalytics.com/2017/03/running-your-r-code-azure.html)
* [Machine Learning Server bulutta](https://docs.microsoft.com/machine-learning-server/install/machine-learning-server-in-the-cloud)
* [Machine Learning sunucusu ve Microsoft R için ek kaynaklar](https://docs.microsoft.com/machine-learning-server/resources-more)
* [Azure üzerinde R](https://github.com/yueguoguo/r-on-azure) - sadece paketler, Araçlar ve Azure ile R kullanmak için örnek olay incelemeleri

---

<sub>R logosudur &copy; 2016 R Foundation ve koşullarına göre kullanılan [Creative Commons atıf-Lisansdevam 4.0 Uluslararası lisansı](https://creativecommons.org/licenses/by-sa/4.0/).</sub>
