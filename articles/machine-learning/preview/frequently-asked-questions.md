---
title: "Azure Machine Learning 2017 Önizleme SSS | Microsoft Docs"
description: "Bu makalede Azure Machine Learning Önizleme özellikleri için yanıtlar ve sık sorulan soruların içerir"
services: machine-learning
author: serinakaye
ms.author: serinak
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 08/30/2017
ms.openlocfilehash: 94c6576f92b5a004e0fd509c1dc6e091c294658a
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2017
---
# <a name="azure-machine-learning-frequently-asked-questions"></a>Azure Machine Learning sık sorulan sorular

## <a name="general-product-questions"></a>Genel ürün soruları

**Azure Machine Learning nedir?**

Azure Machine Learning oluşturmak, sınamak, yönetmek ve makine öğrenme ve AI modelleri dağıtmak izin veren bir tam olarak yönetilen Azure hizmetidir. Bizim yeni hizmetler ve indirilebilir uygulama bulut, şirket içi ve eğitimi sağlamak için dağıtmak, yönetmek kenar yararlanır ilk kod yaklaşımı sunar ve güç, hız ve esneklik modelleriyle izleyebilirsiniz. Alternatif olarak, Azure Machine Learning Studio hiçbir kodlama gerekli olduğu bir tarayıcı tabanlı, visual sürükle ve bırak yazma ortamı sağlar. 

**Önizleme ile çalışmaya nasıl?**

Başlamak için kolay Azure deneme hizmete kaydolmak için yoludur. Portalı'ndan ücretsiz çalışma ekranı uygulamamız karşıdan yükleyin veya bir Model yönetim hesabı için kaydolun. Ayrıca, nasıl yapılır makaleleri, videoları, tanıtım kodu ve daha fazlası için bizim belge sitesini ziyaret edebilirsiniz. 

**Bir Azure aboneliğiniz yok. Yeni hizmetler deneyebilir miyim?**

Microsoft Azure Portföy bir parçası olarak, yeni hizmetlerimizle Azure aboneliği gerektirir. Ayrıca, kaynak grupları, sanal makineler, vb. gibi varlıklar oluşturmak için yeterli izinleri olmalıdır. 

**Bölgeler yeni hizmetlerin kullanılabilir olduğu?**

Azure Machine Learning altında deney ve model yönetim hizmeti için desteklenen Azure bölgeleri arayın [bölgeye göre Azure ürünleri](https://azure.microsoft.com/regions/services/) sayfası.

Biz ürünü geliştirmek daha fazla bölgeler eklenecektir.  İçin üzerinde dağıtmak için hangi bölgeleri belirlememize yardımcı bizim [Azure Machine Learning geribildirim](https://feedback.azure.com/forums/257792-machine-learning) site. 

**Diğer Azure hizmetleriyle gereklidir?**

Tam Azure Machine Learning tüm özelliklerini deneyimi için Azure makine Learnign Visual Studio VSTS hesabı, Azure Blob Storage, Azure kapsayıcı kayıt, veri bilimi VM veya Hdınsight işlem ve Azure kapsayıcı hizmeti kullanır.

**Azure Machine Learning Microsoft Machine Learning SQL Server 2017 Hizmetleri'nde ne şekilde ilişkilidir?**   

SQL Server 2017 makine öğrenme Hizmetleri veritabanı iş akışlarıyla birleştirilerek makine öğrenimi görevlerini tümleştirmek için genişletilebilir, ölçeklenebilir bir platformdur. Mükemmel bir şirket içi çözüm, veri taşıma pahalı veya untenable olduğu örnek için gerekli olduğu senaryolar için olur. Buna karşılık, Bulut veya karma iş yükleri yeni Azure hizmetlerimizle için harika bir Sığdırma ' dir. 

**Python ve R destekliyor musunuz? C++ gibi programlama diğer diller hakkında**

Python şu anda yalnızca destekler. Biz R tümleştirme üzerinde çalışıyorsunuz ve kullanılabilen en kısa sürede beklediğiniz. 

**Azure Machine Learning nasıl için Spark Microsoft Machine Learning ile ilişkilidir?**

A: derin öğrenme MMLSpark sağlar ve veri bilimi araçlarıyla Apache Spark için üretkenliği indirimlere deneme ve durum resim algoritmaları kolaylaştırır. MMLSpark Spark Machine Learning ardışık düzen OpenCV ve Microsoft Bilişsel araç seti ile tümleştirilmesi sunar. Güçlü, ölçeklendirilebilir Tahmine dayalı ve analitik modeller resim ve metin verileri için oluşturabilirsiniz. MMLSpark bir açık kaynak lisansı altında kullanılabilir ve AML çalışma ekranı bir dizi tüketilebilir modelleri ve algoritmaları eklenir. MMLSpark hakkında daha fazla bilgi için ürün Belgelerimizi ziyaret edin. 

**Spark'ın hangi sürümleri, yeni araçlar ve hizmetler tarafından destekleniyor mu? Üst kısmında**

Çalışma ekranı şu anda içerir ve MMLSpark sürüm 0,8, Apache Spark 2.1 ile uyumlu olduğu destekler. Linux sanal makineleri MMLSpark 0,8 GPU etkin Docker görüntüsünü kullanmak için bir seçeneğiniz de vardır.

## <a name="experimentation-service"></a>Deneme hizmeti

**Azure Machine Learning deneme hizmeti nedir?**

Deneme hizmeti bir sonraki düzeye machine learning deneme alır yönetilen bir Azure hizmetidir. Denemeler yerel olarak veya bulutta oluşturulabilir. Hızlı bir şekilde prototip bir masaüstünde, sonra ölçek sanal makine ya da Spark kümeleri. Azure VM son GPU teknolojisi ile birlikte hızlı ve etkili şekilde derin öğrenme içinde gerçekleştirmesine olanak sağlar. İzleme kodu, yapılandırma ve işbirliği için var olan iş akışlarını içine kolayca ekleyebilirsiniz böylece biz Git ile derin tümleştirme de dahil ettiğiniz. 

**Nasıl ı deneme hizmeti için sizden ücret alınır?**

Azure Machine Learning deneme hizmetiniz ile ilişkili ilk iki kullanıcılar ücretsizdir. Ek kullanıcılar $50/ay genel Önizleme hızında ücretlendirilir. Fiyatlandırma ve faturalama hakkında daha fazla bilgi için fiyatlandırma sayfamızı ziyaret edin.

**I çalıştırabilirim kaç denemeler göre ücretlendirilir?**

Hayır, Deneme hizmet gereksinimi ve yalnızca kullanıcı sayısına göre ücretleri aynı sayıda denemeler sağlar. Deneme işlem kaynakları ayrı olarak ücretlendirilir. Çözümünüz için model sığdırma en iyi bulabilmek için birden çok denemeler gerçekleştirmenizi öneririz.   

**İşlem ve depolama kaynaklarını belirli ne tür kullanabilir miyim?**

Deneme hizmeti yerel makine üzerinde (doğrudan veya Docker tabanlı), denemelerinizi yürütebilir [Azure sanal makineleri](https://azure.microsoft.com/services/virtual-machines/), ve [Hdınsight](https://azure.microsoft.com/services/hdinsight/). Ayrıca hizmete erişen bir [Azure Storage](https://azure.microsoft.com/services/storage/) hesap yürütme çıkışları depolamak için ve yararlanabilirsiniz bir [Visual Studio Team hizmeti](https://azure.microsoft.com/services/visual-studio-team-services/) sürüm denetimi ve Git depolama hesabı. Bağımsız olarak tüm tüketilen hesaplama ve depolama kaynakları, tek tek veritabanlarının fiyatlandırma üzerine dayalı için Fatura edilecek olduğunu unutmayın.  


## <a name="model-management"></a>Model Yönetimi

**Azure Machine Learning modeli Management nedir?**

Azure Machine Learning modeli yönetim Tahmine dayalı modelleri çok çeşitli ortamlar güvenilir bir şekilde dağıtmak veri bilimcilerine ve geliştirme ops ekipleri sağlayan yönetilen bir Azure hizmetidir. Git depoları ve Docker kapsayıcıları izlenebilirlik ve Yinelenebilirlik sağlar. Modeller, güvenilir bulut, şirket içi veya edge dağıtılabilir. Üretimde modeli performans yönetebilirsiniz sonra performans düşerse sonra önceden çağırma. Yerel makinede modelleri için dağıtabilirsiniz [Azure Vm'leri](https://azure.microsoft.com/services/virtual-machines/), üzerinde Spark [Hdınsight](https://azure.microsoft.com/services/hdinsight/) veya Kubernetes bağımsızlıklar [Azure kapsayıcı hizmeti](https://azure.microsoft.com/services/container-service/) kümeleri.  

**"Modeli" nedir?**

Çalıştıran bir deneme çıktısı model yönetimi için yükseltilmiş bir modeldir. Barındırma hesabında kayıtlı bir modeli yeniden eğitme veya sürüm yineleme güncelleştirilmiş modelleri de dahil olmak üzere planınıza göre hesaplanır.

**Bir "yönetilen modeli?" nedir**

Model terimi, bir eğitim işleminin çıktısıdır ve eğitim verilerine bir makine öğrenimi algoritmasının uygulanmasını ifade eder. Model yönetim modelleri web Hizmetleri olarak dağıtmak, Modellerinizi çeşitli sürümlerini yönetmek ve kendi performansını ve ölçümleri izleme sağlar. "Yönetilen" modelleri bir Azure Machine Learning modeli yönetim hesabıyla kaydettiniz. Örneğin, satışları tahmin etmeye çalıştığınız bir senaryoyu ele alalım. Deneme aşamasında, farklı veri kümeleri veya algoritmaları kullanarak birçok modelleri oluşturun. Değişen accuracies dört modelleriyle oluşturulmasını ancak yalnızca model yüksek doğruluk ile kaydetmek seçin. Kayıtlı modeli ilk yönetilen modelinizi olur.
 
**Bir "dağıtım?" nedir**

Model yönetim modelleri paketlenmiş web hizmeti Azure kapsayıcılarında olarak dağıtmanızı sağlar. Bu web hizmetleri REST API'lerini kullanarak çağrılabilir. Her web hizmeti tek bir dağıtım kabul edilir ve etkin dağıtım toplam sayısı planınıza doğru sayılır. Örnek, model gerçekleştirme en iyi dağıttığınızda tahmin satış kullanarak, bir dağıtım tarafından planınız artırılır. Daha sonra yeniden eğitme ve başka bir sürümü dağıtırsanız, iki dağıtım sahip. Karar verirseniz daha yeni model iyidir ve delete özgün dağıtım sayısı tarafından düşülür.  

**Hangi belirli işlem kaynakları my dağıtımlar için kullanılabilir mi?** 

Model yönetimi için Docker kapsayıcıları kayıtlı olarak dağıtımlarınızı çalıştırabilirsiniz [Azure kapsayıcı hizmeti](https://azure.microsoft.com/services/container-service/), olarak [Azure sanal makineleri](https://azure.microsoft.com/services/virtual-machines/), veya yerel makine üzerinde. Ek dağıtım hedefleri kısa süre içinde eklenir. Bağımsız olarak tek tek veritabanlarının fiyatlandırma üzerine dayalı herhangi tüketilen işlem kaynakları faturalandırılır olduğunu unutmayın.     

**Deneme hizmeti dışındaki araçlar kullanılarak oluşturulan modelleri dağıtmak için Azure Machine Learning modeli Management kullanabilir miyim?**

Deneme hizmeti kullanılarak oluşturulan modelleri dağıtırken en iyi deneyimi alın. Ancak, diğer çerçeveler ve araçlar kullanılarak oluşturulan modelleri dağıtabilirsiniz. Modelleri MMLSpark, TensorFlow, Microsoft Bilişsel araç seti, scikit dahil olmak üzere çeşitli destekliyoruz-öğrenme, Keras, vs. 

**Azure Kaynaklarım kullanabilir miyim?**

Evet, deneme hizmeti ve Model yönetim birden çok Azure veri depoları ile birlikte çalışma, işlem iş yükleri ve diğer hizmetler. Gerekli Azure hizmetleri hakkında daha fazla ayrıntı için teknik belgelerimize bakın.

**Hem şirket içi desteklemek ve dağıtım senaryoları bulut?**

Evet. Biz şirket içi desteklemek ve dağıtım senaryoları Docker kapsayıcıları aracılığıyla bulut. Yerel yürütme hedefleri şunlardır: tek düğümlü Docker dağıtımları [ML Hizmetleri ile Microsoft SQL Server](https://docs.microsoft.com/sql/advanced-analytics/r/r-services), Hadoop veya Spark. Ayrıca bulut destekliyoruz Docker, aracılığıyla dağıtımlar dahil olmak üzere: Kümelenmiş dağıtımları Azure kapsayıcı hizmeti ve Kubernetes, Hdınsight veya Spark kümeleri. Edge senaryoları Docker kapsayıcıları ve Azure IOT kenar aracılığıyla desteklenir. 

**Azure Machine Learning CLI başka bir ana bilgisayarda kullanılarak oluşturulmuş bir Docker görüntüsü çalıştırabilir miyim?**

Evet. Ana bilgisayar docker görüntü barındırmak için yeterli işlem kaynakları olduğu sürece herhangi bir docker ana bilgisayar üzerinde bir web hizmeti olarak görüntüsü kullanabilirsiniz.

**Dağıtılan modelleri yeniden eğitme destekliyor musunuz?**

Evet, birden çok sürümü aynı modelin dağıtabilirsiniz. Model yönetim tüm güncelleştirilmiş modelleri ve görüntüleri için hizmet güncelleştirmeleri destekler.

## <a name="workbench"></a>Workbench

**Azure Machine Learning çalışma ekranı nedir?**

Azure Machine Learning çalışma ekranı, profesyonel veri bilimcilerine için yerleşik yardımcı bir uygulamadır. Windows ve Mac için kullanılabilir, Machine Learning çalışma ekranı genel bakış, yönetim ve denetimi için makine öğrenimi çözümleri sağlar. Machine Learning çalışma ekranı erişim modern AI çerçeveleri hem Microsoft hem de açık kaynak topluluğu içerir. TensorFlow, Microsoft Bilişsel araç seti, Spark ML, scikit dahil olmak üzere en popüler veri bilimi araç takımları dahil ettiğiniz-öğrenin ve daha fazlası. Biz de Jupyter not defterleri, PyCharm ve Visual Studio Code gibi popüler veri bilimi IDE ile tümleştirme etkinleştirdikten. Machine Learning çalışma ekranı hızlı bir şekilde örnek, anlamak ve yapılandırılmış veya yapılandırılmamış verileri, hazırlamak için yerleşik veri hazırlık özellikleri vardır. Adlı bizim yeni verileri Hazırlama aracı [bir yazı olabilir](https://microsoft.github.io/prose/), Microsoft Research modern teknolojisi üzerine kurulmuştur.  

**Çalışma ekranı bir IDE mi?**

Hayır. Machine Learning çalışma ekranı Jupyter not defterleri, Visual Studio Code ve PyCharm gibi popüler IDE için özel olarak tasarlanmış ancak tam olarak işlevsel bir IDE değil. Machine Learning çalışma ekranı bazı temel metin düzenleme özellikleri, ancak hata ayıklama, IntelliSense ve diğer yaygın olarak kullanılan IDE özellikleri desteklenmez sunar. Sık kullanılan IDE'yi kod geliştirme için düzenleme ve hata ayıklama kullanmanızı öneririz. 

**Azure Machine Learning çalışma ekranı kullanmak için bir ücret var mı?**

Hayır. Azure Machine Learning çalışma ekranı ücretsiz bir uygulamasıdır. Bu uygulamayı dilediğiniz sayıda makinede ve dilediğiniz sayıda kullanıcı için indirebilirsiniz. Azure Machine Learning Workbench’i kullanabilmeniz için bir Deneme hesabınız olmalıdır. .  

**Komut satırı özelliklerini destekler mi?**

A: Evet Azure Machine Learning tam CLI sunar. Machine Learning CLI, Azure Machine Learning çalışma ekranı varsayılan olarak yüklenir. Ayrıca Azure Linux veri bilimi sanal makinede bir parçası olarak sağlanır ve içine tümleştirilmiştir [Azure CLI](https://docs.microsoft.com/cli/azure/overview?view=azure-cli-latest)


**Jupyter not defterleri ile çalışma ekranı kullanabilir miyim?**

Evet! Yalnızca bir tarayıcı istemci olarak kullanacağınız gibi istemci barındırma uygulaması gibi çalışma ekranı ile çalışma ekranındaki Jupyter not defterleri çalıştırabilirsiniz. 

**Hangi Jupyter not defterlerinde çekirdekler destekleniyor mu?**

Y: ile çalışma ekranı dahil Jupyter geçerli sürümü Python 3 Çekirdek ve aml_config klasördeki her "runconfig" dosyası için ek bir çekirdek başlatır. Desteklenen yapılandırmalar şunlardır:
- Yerel Python
- Yerel veya uzak Docker Python'da

## <a name="data-formats-and-capabilities"></a>Veri biçimleri ve özellikleri

**Hangi dosya biçimleri, çalışma ekranı veri alımında için destekleniyor mu?**

Çalışma ekranı veri hazırlık araçları şu anda aşağıdaki biçimlerden alım destekler: 
- CSV, vb. TSV sınırlandırılmış dosyalar.  
- Sabit genişlikte dosyaları
- Düz metin dosyaları
- Excel (.xls/xlsx)
- JSON dosyaları
- Parquet dosyaları 
- Özel dosyaları (ek kaynaklardan veri alımı çözümünüzü gerektiriyorsa, komut dosyaları), Python kodu için kullanılabilir... 

**Hangi veri depolama konumları şu anda destekleniyor mu?**

Genel Önizleme için çalışma ekranı gelen veri alımı destekler: 
- Yerel sabit diske veya eşlenen ağ depolama konumu
- Azure BLOB veya Azure Storage (bir Azure aboneliği gerektirir)
- Azure SQL Server
- Microsoft SQL Server


**Hangi türde veri wrangling, hazırlık ve dönüşümleri var mı?**

Genel Önizleme için çalışma ekranı "Sütun örneği tarafından türetilen", "Örnek tarafından bölünmüş sütun", "Metin Kümeleme", "Eksik değerleri işlemek" ve diğer birçok destekler.  Çalışma ekranı, veri türü dönüşümü, veri toplama (sayısı, ortalama, fark, vb.) ve karmaşık veri birleştirmeler da destekler. Desteklenen yeteneklerin tam listesi için ürün Belgelerimizi ziyaret edin. 

**Azure Machine Learning çalışma ekranı, deneme veya Model Yönetimi tarafından zorlanan veri boyut sınırlarını var mı?**

A: Hayır yeni hizmetler veri sınırlamalar zorunlu tuttukları değil. Ancak, veri hazırlığı, model eğitim, deneme veya dağıtım gerçekleştiriyorsanız ortamı tarafından sunulan sınırlamaları vardır. Örneğin, eğitim için yerel bir ortam hedefliyorsanız, kullanılabilir alanı tarafından sabit diskinize sınırlıdır. Alternatif olarak, Hdınsight hedefliyorsanız, ilişkili tüm boyutuyla sınırlı veya ayarlarıdır işlem. 

## <a name="algorithms-and-libraries"></a>Algoritmaları ve kitaplıklar

**Hangi algoritmaları Azure Machine Learning çalışma destekleniyor mu?**

Önizleme ürünlerimizi ve hizmetlerimizi açık kaynak Topluluğu'nun en iyi içerir. Çok çeşitli algoritmalar ve TensorFlow, scikit dahil olmak üzere kitaplıkları destekliyoruz-öğrenme, Apache Spark ve Microsoft Bilişsel Araç Seti. Azure Machine Learning çalışma ekranı ayrıca paketleri [Microsoft revoscalepy](https://docs.microsoft.com/sql/advanced-analytics/python/what-is-revoscalepy) paket.

**Azure Machine Learning Microsoft Bilişsel araç seti nasıl ilişkilidir?**

[Microsoft Bilişsel Araç Seti](https://www.microsoft.com/cognitive-toolkit/) bizim yeni araçlar ve hizmetler tarafından desteklenen birçok çerçeveleri biridir. Bilişsel Araç Seti kullanabilir ve popüler machine learning modellerini akış İleri derin sinir ağları, Convolutional ağ dahil olmak üzere birleştirme olanak sağlayan bir birleşik derin öğrenme araç takımıdır sırası dizisi ve yinelenen ağlar. Microsoft Bilişsel araç seti ile ilgili daha fazla bilgi için ziyaret bizim [ürün belgelerine](https://docs.microsoft.com/cognitive-toolkit/). 

## <a name="pricing-and-billing"></a>Fiyatlandırma ve Faturalama

**Önizleme sırasında Azure Machine Learning için şarj?** 

Y: Azure Machine Learning çalışma ekranı uygulama kullanılabilir ücretsiz Azure aboneleri için. Ücretli katmanlar, genel Önizleme sırasında indirimle avialable yanı sıra boş katmanları, deneme hizmeti ve Model yönetim sunar.

**I çalıştırabilirim kaç denemeler göre ücretlendirilir?**

Hayır, Deneme Hizmeti dilediğiniz sayıda denemeye izin verir ve ücretler yalnızca kullanıcı sayısına bağlıdır. Deneme işlem kaynakları ayrı olarak ücretlendirilir.  Çözümünüz için en iyi modeli gelmesi birden çok denemeler gerçekleştirmenizi öneririz. 

**I tabanlı web Hizmetlerim kaç kez ücret çağrıldığında?**

Hayır. Web Hizmetleri, gerektiği tüm Model yönetim fatura etkileri olmadan sık olarak çağrılabilir. Dağıtımlarınızı uygulamalarınızın gereksinimlerini karşılayacak şekilde ölçeklendirme konusunda tam denetime sahip olursunuz.

**Azure Machine Learning modeli Yönetimi'nde satın aldığınız birim sayısını nasıl ölçeklendirme yapabilir?**

Azure portalı veya CLI kullanarak, birim sayısını yukarı veya aşağı değiştirebilirsiniz. 

**Ne my fatura görünüm ister?**

Fatura günlük oluşturulur. Faturalandırma işlemlerinde, gün başlangıcı gece yarısı (UTC) olarak kabul edilir. Faturalar aylık olarak oluşturulur. Ayrı ücretleri, Azure Machine Learning ile birlikte kullanılan herhangi bir Azure hizmeti için ücrete. Ücretler içerebilir, ancak bunlarla sınırlı değildir: 
- İşlem ücretlerini
- HDInsight
- Azure Container Service
- Azure Container Kayıt Defteri 
- Azure Blob Depolama
- Application Insights
- Azure Key Vault
- Visual Studio Team Services
- Azure Event hub'ı
- Azure Stream Analytics için Ayrıntılar daha ayrıntılı olarak veya bir örnek fatura görüntülemek için fiyatlandırma sayfamızı ziyaret edin. 

## <a name="support-and-training"></a>Destek ve eğitim

**Azure Machine Learning için nereden eğitim alabilirim?**

[Azure Machine Learning Belge Merkezi](./overview-what-is-azure-ml.md), video öğreticilerinin yanı sıra nasıl yapılır kılavuzlarını da barındırır. Bu adım adım kılavuzlar Hizmetleri getirir ve veri bilimi yaşam döngüsü açıklanmaktadır. Machine Learning Merkezi'ne düzenli olarak yeni materyaller ekleriz. Kullanıcı geri bildirim Forumunda adresindeki ek öğrenme materyalleri için istek gönderebilirsiniz.

**Azure Machine Learning için nasıl destek alabilirim?**

Teknik destek için Azure desteği için gidin ve Machine Learning seçin. Azure Machine Learning de sahip etkin bir topluluk [StackOverflow](https://stackoverflow.com/questions/tagged/azure-machine-learning) "Azure-makine ekibi tarafından izlenen öğrenme", ile etiketlenir.  Öneriler ve özellik istekleri gönderilebilir adresindeki [Azure Machine Learning geribildirim](https://feedback.azure.com/forums/257792-machine-learning) site. 
