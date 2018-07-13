---
title: Azure Machine Learning 2017 önizleme hakkında SSS | Microsoft Docs
description: Bu makalede, sık sorulan sorular ve yanıtlar için Azure Machine Learning Önizleme özellikleri
services: machine-learning
author: serinakaye
ms.author: serinak
manager: mwinkle
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.topic: article
ms.date: 08/30/2017
ms.openlocfilehash: 94a1f3bbba83e8e71cf9440b5ded0784f4616c99
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38674164"
---
# <a name="azure-machine-learning-frequently-asked-questions"></a>Azure Machine Learning hakkında sık sorulan sorular

Azure Machine Learning oluşturun, test etmek, yönetmek ve makine öğrenimi ve yapay ZEKA modelleri dağıtın olanak tanıyan bir tam olarak yönetilen Azure hizmetidir. Sunduğumuz Hizmetleri ve indirilebilir uygulama bulut, şirket içi ve eğitimi sağlayın, dağıtma, yönetme, edge yararlanan kod öncelikli bir yaklaşım sunar ve modelleri power, hız ve esneklik ile izleyin. Alternatif olarak, Azure Machine Learning Studio kodlama gerekli olduğu bir tarayıcı tabanlı, görsel sürükle ve bırak geliştirme ortamı sunar. 

## <a name="general-product-questions"></a>Ürün genel sorular

**Diğer Azure Hizmetleri gereklidir?**

Azure Blob Depolama ve Azure Container Registry, Azure Machine Learning tarafından kullanılır. Ayrıca, bir veri bilimi sanal makinesi veya HDInsight kümesi gibi bilgi işlem kaynakları sağlamak gerekir. İşlem ve barındırma da gerekli, web Hizmetleri gibi dağıtım yaparken [Azure Container Service](https://docs.microsoft.com/azure/aks).

**SQL Server 2017'de Microsoft Machine Learning Hizmetleri için nasıl Azure Machine Learning ilişkilidir?**   

SQL Server 2017 Machine Learning Hizmetleri makine öğrenimi görevlerini veritabanı iş akışlarınızla tümleştirmek için genişletilebilir, ölçeklenebilir bir platformdur. Bu, şirket içi bir çözüm, örnek veri taşıma pahalı veya untenable olduğu için gerekli olduğu senaryolar için mükemmel olur. Buna karşılık, Bulut veya hibrit iş yükleri için yeni Azure hizmetlerimizi harika uygun olan. 

**Hem Python hem de R destekliyorsunuz? Diğer C++ gibi programlama dilleri hakkında**

Python şu anda yalnızca destekler. Biz, R tümleştirmesi üzerinde çalışıyoruz ve kullanılabilir olan en kısa sürede sahip olmayı beklediğiniz. 

**Azure Machine Learning'i nasıl Spark için Microsoft Machine Learning ile ilişkilidir?**

Derin öğrenme MMLSpark sağlar ve üretkenlik, indirimlere ile Apache Spark için veri bilimi araçlarını deneme ve durum resim algoritmaları kolaylaştırır. MMLSpark Spark Machine Learning işlem hatlarını OpenCV ve Microsoft Bilişsel araç seti ile tümleştirme sağlar. Güçlü, yüksek oranda ölçeklenebilir Tahmine dayalı ve analitik modeller resim ve metin veri oluşturabilirsiniz. MMLSpark, bir açık kaynak lisansı altında sunulur ve AML Workbench'te tüketim modelleri ve algoritmaları kümesi olarak dahil edilir. MMLSpark hakkında daha fazla bilgi için belgelerimize ürün kaydedilir. 

**Yeni araç ve Hizmetleri tarafından Spark'ın hangi sürümleri destekleniyor?**

Workbench, şu anda içerir ve Apache Spark 2.1 ile uyumlu sürümü 0.8 MMLSpark destekler. Linux sanal makinelerinde MMLSpark 0.8 GPU özellikli bir Docker görüntüsü kullanmak için bir seçeneğiniz de vardır.

## <a name="experimentation-service"></a>Deneme hizmeti

**Azure Machine Learning deneme hizmeti nedir?**

Deneme hizmeti, machine learning denemesi bir sonraki düzeye alan yönetilen bir Azure hizmetidir. Yerel olarak veya bulutta denemeleri oluşturulabilir. Hızlı bir şekilde prototip masaüstünde, sonra ölçeği sanal makine ya da Spark kümeleri. En son GPU teknolojisi ile Azure Vm'leri hızla ve etkili bir şekilde derin öğrenme de gerçekleştirmesine imkan sağlar. İzleme kodu, yapılandırma ve işbirliği için mevcut iş akışları halinde kolayca takılabilir için Git ile kapsamlı tümleştirme de ekledik. 

**Deneme hizmeti için nasıl ücretlendirilirim?**

Azure Machine Learning deneme hizmeti ile ilişkili ilk iki kullanıcı ücretsizdir. Ek kullanıcılar, aylık 50 $ genel Önizleme fiyatı üzerinden ücretlendirilirsiniz. Fiyatlandırma ve faturalandırma hakkında daha fazla bilgi için fiyatlandırma sayfamızı ziyaret edin.

**Kaç tane denemeleri çalıştırabilir göre ücretlendirilirim?**

Hayır, deneme hizmeti dilediğiniz sayıda denemeye gereksinimini ve yalnızca kullanıcı sayısına göre ücret sağlar. Deneme işlem kaynakları ayrı olarak ücretlendirilir. Çözümünüz için model sığdırma en iyi bulabilmesi için birden çok deneme yapmaya öneririz.   

**Hangi tür işlem ve depolama kaynakları kullanabilirim?**

Deneme hizmeti, yerel makine üzerinde (doğrudan veya Docker tabanlı), denemelerinizi yürütebilirsiniz [Azure sanal makineler](https://azure.microsoft.com/services/virtual-machines/), ve [HDInsight](https://azure.microsoft.com/services/hdinsight/). Ayrıca hizmete erişen bir [Azure depolama](https://azure.microsoft.com/services/storage/) yararlanabilir ve hesap yürütme çıktılarının depolanması için bir [Visual Studio Team Service](https://azure.microsoft.com/services/visual-studio-team-services/) sürüm denetimi ve Git depolama hesabı. Bağımsız olarak tüketilen tüm işlem ve depolama kaynaklarını tek tek kendi fiyatı temel alınarak faturalandırılırsınız olduğunu unutmayın.  


## <a name="model-management"></a>Model Yönetimi

**Azure Machine Learning Model yönetimi nedir?**

Azure Machine Learning Model Yönetimi Tahmine dayalı modelleri çok çeşitli ortamlar içinde güvenilir bir şekilde dağıtmak veri bilimcilerine ve geliştirme ekipleri sağlayan yönetilen bir Azure hizmetidir. Git depoları ve Docker kapsayıcılarını izlenebilirlik ve yinelenebilirliği sağlamak. Bulut, şirket içinde veya uç modelleri güvenilir bir şekilde dağıtılabilir. Üretim ortamında, model performansını yönetebilirsiniz. sonra performansı düşürür, ardından proaktif olarak yeniden eğitme. Yerel makinede modelleri için dağıtabileceğiniz [Azure Vm'leri](https://azure.microsoft.com/services/virtual-machines/), üzerinde Spark [HDInsight](https://azure.microsoft.com/services/hdinsight/) veya Kubernetes düzenlenen [Azure Container Service](https://azure.microsoft.com/services/container-service/) kümeleri.  

**"Modeli" nedir?**

Model yönetimi için çalışan bir deneme çıktısını yükseltildi bir modeldir. Barındırma hesabında kayıtlı bir modeli yeniden eğitme veya sürüm yineleme güncelleştirilmiş modelleri de dahil olmak üzere, planınıza göre sayılır.

**"Yönetilen model?" nedir**

Model terimi, bir eğitim işleminin çıktısıdır ve eğitim verilerine bir makine öğrenimi algoritmasının uygulanmasını ifade eder. Model yönetimi, Modellerinizi web Hizmetleri olarak dağıtmanıza, modellerin çeşitli sürümlerini yönetmek ve performans ve ölçümleri izleme sağlar. "Yönetilen" modeller bir Azure Machine Learning Model Yönetimi hesabı ile kaydedilmiş. Örneğin, satışları tahmin etmeye çalıştığınız bir senaryoyu ele alalım. Deneme aşaması boyunca farklı veri kümeleri veya algoritmalar kullanarak birçok modelleri oluşturun. Dört model oluşturduğunuz doğruluk ancak en yüksek doğruluğa sahip modeli kaydetmeyi tercih. Kayıtlı modeli ilk yönetilen modeliniz olur.
 
**Bir "dağıtım?" nedir**

Model Yönetimi modelleri azure'da paketlenmiş web hizmeti kapsayıcıları olarak dağıtmanıza olanak tanır. Bu web Hizmetleri, REST API'lerini kullanarak çağrılabilir. Her web hizmeti tek bir dağıtım olarak kabul edilir ve etkin dağıtımların toplam sayısı planınızdan sayılır. Örnek, en iyi performansa sahip model dağıttığınızda tahmini satış kullanarak, planınızı bir dağıtım tarafından artırılır. Daha sonra yeniden eğitme ve başka bir sürümü dağıtırsanız iki dağıtımınız. Karar verirseniz yeni modelin daha iyi ve delete özgün, dağıtım sayınız bir azaltılır.  

**Hangi belirli işlem kaynakları my dağıtımları için kullanılabilir mi?** 

Model yönetimi, dağıtımlarınızı çalıştırabilir, Docker kapsayıcıları için kayıtlı [Azure Container Service](https://azure.microsoft.com/services/container-service/), olarak [Azure sanal makineler](https://azure.microsoft.com/services/virtual-machines/), veya yerel makine üzerinde. Ek dağıtım hedefleri kısa bir süre sonra eklenecektir. Sizin için bireysel kullanıcıların fiyatı temel alınarak tüketilen tüm işlem kaynakları, faturalandırılacağınızı olduğunu unutmayın.     

**Deneme hizmeti dışındaki araçlarla oluşturulmuş modelleri dağıtabilir Azure Machine Learning Model Yönetimi kullanabilir miyim?**

Deneme hizmeti kullanılarak oluşturulmuş modelleri dağıttığınızda en iyi deneyimi alın. Ancak, diğer çerçeveleri ve araçları kullanılarak oluşturulmuş modelleri dağıtabilirsiniz. Modelleri MMLSpark, TensorFlow, Microsoft Bilişsel araç seti, scikit dahil olmak üzere çeşitli destekliyoruz-learn ve Keras, vs. 

**Kendi Azure kaynaklarını kullanabilir miyim?**

Evet, deneme hizmeti ve Model Yönetimi birden çok Azure veri deposu ile birlikte çalışma, işlem iş yükleri ve diğer hizmetleri. Gerekli Azure hizmetleri hakkında daha ayrıntılı bilgi için teknik belgelerimize bakın.

**Hem şirket içi ve bulut dağıtım senaryolarına?**

Evet. Biz şirket içi ve bulut dağıtım senaryolarına Docker kapsayıcıları aracılığıyla. Yerel yürütme hedefler içerir: tek düğümlü Docker dağıtımları [Microsoft SQL Server ML Hizmetleri ile](https://docs.microsoft.com/sql/advanced-analytics/r/r-services), Hadoop ve Spark. Bulut da destekliyoruz Docker'ı aracılığıyla dağıtımları da dahil olmak üzere: Azure Container Service ve Kubernetes, HDInsight ve Spark kümeleri kümelenmiş dağıtımları. Edge senaryolar, Docker kapsayıcıları ve Azure IOT Edge ile desteklenir. 

**Başka bir konakta Azure Machine Learning CLI kullanılarak oluşturulmuş bir Docker görüntüsü çalıştırabilir miyim?**

Evet. Docker görüntüsünü barındırmak için yeterli işlem kaynakları konak sahip olduğu sürece herhangi bir docker konağı üzerinde bir web hizmeti olarak görüntü kullanabilirsiniz.

**Dağıtılan modelleri yeniden eğitme destekliyorsunuz?**

Evet, birden çok sürümünü aynı modelin dağıtabilirsiniz. Model Yönetimi tüm güncelleştirilmiş modelleri ve görüntüleri için hizmet güncelleştirmeleri destekler.

## <a name="workbench"></a>Workbench

**Azure Machine Learning Workbench nedir?**

Azure Machine Learning Workbench, Uzman veri bilimcilerinin için yerleşik yardımcı bir uygulamadır. Windows ve Mac için kullanılabilir, Machine Learning Workbench genel bakış, yönetim ve denetimi için makine öğrenimi çözümleri sağlar. Machine Learning Workbench modern AI çerçeveleri için hem Microsoft hem de açık kaynak topluluğundan erişimi içerir. TensorFlow, Microsoft Cognitive Toolkit, Spark ML ve scikit dahil olmak üzere en popüler veri bilimi araç Setleri ekledik-öğrenin ve daha fazlası. Jupyter Notebook belgeleri, PyCharm ve Visual Studio Code gibi popüler veri bilimi IDE'ler ile tümleştirme de etkinleştirdik. Machine Learning Workbench, hızlı bir şekilde örnek, anlamak ve yapılandırılmış veya yapılandırılmamış verileri hazırlamak için yerleşik veri hazırlama özellikleri vardır. Adlı sunduğumuz yeni veri Hazırlama aracı [PROSE](https://microsoft.github.io/prose/), Microsoft Research tarafından hazırlanan son teknoloji ürünü teknolojisi üzerine kurulmuştur.  

**Workbench, bir IDE mi?**

Hayır. Machine Learning Workbench, Jupyter Notebook belgeleri, Visual Studio Code ve PyCharm gibi popüler Ide'leri için eşlikçi olarak tasarlanmıştır ancak tam olarak işlevsel bir IDE değil. Machine Learning Workbench, bazı düzenleme özellikleri, ancak hata ayıklama temel metin, IntelliSense ve diğer yaygın olarak kullanılan IDE özellikleri desteklenmez sunar. En sevdiğiniz IDE için kod geliştirme, düzenleme ve hata ayıklama kullanmanızı öneririz. Ayrıca denemeyi düşünebileceğiz [yapay ZEKA için Visual Studio Code Araçları](https://www.visualstudio.com/downloads/ai-tools-vscode).

**Azure Machine Learning Workbench'i kullanarak ücreti var mıdır?**

Hayır. Azure Machine Learning Workbench ücretsiz bir uygulamadır. Bu uygulamayı dilediğiniz sayıda makinede ve dilediğiniz sayıda kullanıcı için indirebilirsiniz. Azure Machine Learning Workbench’i kullanabilmeniz için bir Deneme hesabınız olmalıdır. .  

**Komut satırı özelliklerini destekler mi?**

Evet, Azure Machine Learning, tam bir CLI arabirimi sunar. Machine Learning CLI, Azure Machine Learning Workbench ile varsayılan olarak yüklenir. Azure üzerinde Linux veri bilimi sanal makinesi bir parçası olarak sağlanır ve içine tümleşik [Azure CLI](https://docs.microsoft.com/cli/azure?view=azure-cli-latest)


**Jupyter not defterleri Workbench ile kullanabilir miyim?**

Evet! Yeni bir tarayıcı istemci olarak kullandığınız gibi Jupyter not defterleri Workbench'te Workbench istemci barındırma uygulaması olarak çalıştırabilirsiniz. 

**Hangi Jupyter not defterleri destekleniyor mu?**

Workbench'te dahil Jupyter'ın geçerli sürümü Python 3 Çekirdek ve aml_config klasörünüzde her "runconfig" dosyası için ek bir çekirdeği başlatır. Desteklenen yapılandırmalar şunlardır:
- Yerel Python
- Yerel veya uzak bir Docker Python

## <a name="data-formats-and-capabilities"></a>Veri biçimleri ve özellikleri

**Hangi dosya biçimleri, workbench'teki veri alımı için destekleniyor mu?**

Workbench uygulamasında veri hazırlama araçları şu anda aşağıdaki biçimlerde alma destekler: 
- CSV, TSV, vb. gibi ayrılmış dosyalar.  
- Sabit genişlikli dosyalar
- Düz metin dosyaları
- Excel (.xls/xlsx)
- JSON dosyaları
- Parquet dosyalarını 
- Özel dosyaları (ek kaynaklardan veri alımı, çözümünüzün gerektiriyorsa, komut dosyaları), Python kodu için kullanılabilir... 

**Hangi veri depolama konumları, şu anda destekleniyor mu?**

Genel Önizleme için veri alma Workbench destekler: 
- Yerel sabit diske veya eşlenmiş ağ depolama konumu
- Azure BLOB veya Azure depolama (Azure aboneliği gerektirir)
- Azure SQL Server
- Microsoft SQL Server


**Ne tür veri denetimi, hazırlama ve dönüşümleri var mı?**

Genel Önizleme için "Sütun örneği tarafından türetilir", "Örneği tarafından bölünen sütun", "Metin Kümeleme", "Eksik değerleri işlemek" ve diğer birçok Workbench destekler.  Workbench, veri türü dönüşümü, veri toplama (sayısı, ortalama, FARKI, vb.) ve karmaşık veri birleşimleri de destekler. Desteklenen yeteneklerin tam listesi için ürün belgeler sayfamızı ziyaret edin. 

**Azure Machine Learning Workbench, deneme ve Model Yönetimi tarafından zorlanan herhangi bir veri boyutu sınırlar var mıdır?**

Hayır, yeni hizmetleri herhangi bir veri kısıtlamaları dayatır değil. Ancak, veri hazırlama, model eğitiminin, deneme ya da dağıtımı yapıyorsanız ortamı tarafından sunulan sınırlamaları vardır. Örneğin, eğitim için yerel bir ortamı hedefliyorsanız, sabit sürücünüzde kullanılabilir alanı tarafından sınırlandırılmıştır. Alternatif olarak, HDInsight'ı hedefliyorsanız, ilişkili tüm boyutuyla sınırlıdır veya kısıtlamaları işlem. 

## <a name="algorithms-and-libraries"></a>Algoritmalar ve kitaplıkları

**Azure Machine Learning Workbench'te hangi algoritmalar destekleniyor?**

Önizleme Ürünlerimiz ve hizmetlerimiz, açık kaynak topluluğundan en iyi şekilde içerir. Çok çeşitli algoritmalar ve kitaplıkları TensorFlow, scikit dahil olmak üzere destekliyoruz-learn ve Apache Spark ve Microsoft Bilişsel Araç Seti. Ayrıca Azure Machine Learning Workbench paketleri [Microsoft revoscalepy](https://docs.microsoft.com/sql/advanced-analytics/python/what-is-revoscalepy) paket.

**Azure Machine Learning Microsoft Bilişsel araç seti nasıl ilişkilidir?**

[Microsoft Bilişsel Araç Seti](https://www.microsoft.com/en-us/cognitive-toolkit/) sunduğumuz yeni araçları ve Hizmetleri tarafından desteklenen çok sayıda çerçeveleri biridir. Bilişsel Araç Seti kullanmak ve popüler makine öğrenimi modellerini akış iletme derin sinir ağları, karmaşık ağ dahil olmak üzere birleştirmek sağlayan bir birleşik derin öğrenme araç seti olan Sequence Sequence ve yinelenen ağlar. Microsoft Bilişsel araç seti ile ilgili daha fazla bilgi için müşterilerimize [ürün belgelerine](https://docs.microsoft.com/cognitive-toolkit/). 