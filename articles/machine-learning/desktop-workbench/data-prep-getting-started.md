---
title: Azure Machine Learning veri hazırlama ile çalışmaya başlama | Microsoft Docs
description: AML workbench veri hazırlık bölümünde için başlangıç kılavuzunda budur.
services: machine-learning
author: cforbe
ms.author: cforbe
manager: mwinkle
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.topic: article
ms.date: 09/07/2017
ms.openlocfilehash: ffff3d274e093e9ce4aa32dee7b033bd1ed288bc
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2018
ms.locfileid: "35651176"
---
# <a name="getting-started-with-data-preparation"></a>Veri hazırlama ile çalışmaya başlama

Veri hazırlama Başlarken Kılavuzu'na hoşgeldiniz. 

Veri hazırlama, verimli bir şekilde keşfetmeye, anlama ve sorunlarını giderme veri için araçlar kümesi sağlar. Pek çok biçimde verileri tüketin ve Aşağı Akış kullanımı için daha uygun Temizlenen veri verileri dönüştürün olanak tanır.

Veri hazırlığı, Azure Machine Learning Workbench deneyiminin bir parçası yüklenir.  Veri hazırlama yerel olarak kullanın veya bir hedef kümeye dağıtın ve çalışma zamanı veya yürütme ortamı olarak bulut.

Tasarım zamanı çalışma zamanı, Python için genişletilebilirliği kullanır ve Pandas gibi çeşitli Python kitaplıkları bağlıdır. Azure ML Workbench diğer bileşenlerle olduğundan Python yüklemeniz gerekmez, bunu sizin için yüklenir. Ek kitaplıklar da yüklemeniz gerekir, ancak bu kitaplıklar olmasına gerek olmayan normal Python dizininizi Azure ML Workbench'i Python dizininde yüklü. Paketleri yükleme hakkında daha fazla ayrıntı bulunabilir [burada](data-prep-python-extensibility-overview.md).

Proje oluşturulduktan sonra veri hazırlığı, kullanabilmeniz için önce bir proje veri hazırlayabilirsiniz gereklidir. 

Veri simgesini seçerek proje veri kısmına gidin ![veri kaynağı simgesi](media/data-prep-getting-started/data-source-icon.png) ekranın sol taraftaki.  Tıklayın "+" yeniden **veri kaynağı Ekle**. Veri kaynağı Sihirbazı başlatmalı ve ekler bir **veri kaynağı** Sihirbazı tamamladıktan sonra proje dosyasına (.dsource). Varsayılan olarak, verilerin görünümünü kılavuzdur. Kılavuzun üzerindeki da ölçümler görünümü seçmek mümkündür. Ölçümler Görünümü'nde, Özet istatistikleri gösterilmektedir.  Özet istatistikleri gözden geçirdikten sonra tıklayarak **hazırlama** ekranın üstünde. [Veri kaynağı Sihirbazı hakkında daha fazla bilgi](data-source-wizard.md) 

## <a name="building-blocks-of-data-preparation"></a>Veri hazırlama yapı taşları ##
### <a name="the-package"></a>Paket ###

Bir paket, çalışmanız için birincil kapsayıcıdır. Kaydedilen ve diskten yüklenen bir yapıt paketidir. İstemci içinde çalışırken sürekli otomatik kaydedilmiş arka planda paketidir. 

Python'da veya Jupyter Notebook aracılığıyla bir paketi çıkış/sonuçlarını notebook'ta incelenebilir.

Bir paketi, yerel Python, Spark (Docker dahil) ve HDInsight gibi birden fazla çalışma zamanı arasında çalıştırılabilir.

Bir paket adımları ve verilere uygulanan dönüştürmeler bir veya daha fazla veri akışlarını içerir.

Bir paketi başka bir paket (bir başvuru veri akışı adlandırılır) bir veri kaynağı olarak kullanabilir.

### <a name="the-dataflow"></a>Veri akışı ###
Bir kaynağı bir veri akışı vardır ve isteğe bağlı, bir dizi adımı ve isteğe bağlı hedefleri ile düzenlenmiş dönüştürür. Bir adımı tıklayarak tüm kaynakları ve dönüşümler öncesi dahil seçili adımın yeniden yürütür.  Bu adım aracılığıyla dönüştürülmüş verileri grid içinde gösterilir. Adımlar eklendi, taşınan ve içinde bir veri akışı adım listesi aracılığıyla silindi.

İstemci sağ tarafında adım listesi açılır ve daha fazla ekran alanı sağlamak için kapalı.

Birden çok veri akışı kullanıcı Arabiriminde aynı anda var olabilen, her veri akışı, kullanıcı arabiriminde bir sekme olarak temsil edilir.

### <a name="the-source"></a>Kaynak
Bir, burada verilerin geldiği ve biçim olarak kaynağıdır. Bir veri hazırlık paketi, her zaman başka bir veri Flow(Data Source) kendi veri kaynaklarından. Bu başvuru bilgilerini içeren veri akışı var. Her kaynak yapılandırılması izin vermek için farklı bir kullanıcı deneyimi vardır. Kaynak verilerin "dikdörtgen" / tablolu bir görünümünü oluşturur. Kaynak verileri ilk olarak "düzensiz sağ" varsa, ardından yapısı "dikdörtgen" olarak normalleştirilir [Ek 2 sağlar, desteklenen kaynaklar geçerli listesini](data-prep-appendix2-supported-data-sources.md).

### <a name="the-transform"></a>Dönüştürme ###
Dönüşümler verileri belirli bir biçimde, bazı verileri (örneğin, veri türünü değiştirme) üzerinde işlemi ve ardından yeni biçimindeki verileri. Her dönüştürme, kendi kullanıcı Arabirimi ve behavior(s) vardır. Birden çok dönüşüm veri akışı'ndaki adımları aracılığıyla birbirine zincirleme veri hazırlama işlevlerin çekirdeği olur. [Ek 3 sağlar geçerli desteklenen dönüşümler listesi](data-prep-appendix3-supported-transforms.md).

### <a name="the-inspector"></a>Denetçisi ###

Denetçiler veri görselleştirmeleri ve verileri bir anlayış geliştirmek kullanılabilir.  Veri ve veri anlama (dönüşümler) hangi eylemlerin yapılması gerektiğini karar vermenize yardımcı olur kalite sorunları. Bazı denetçiler dönüşümler oluşturacak eylemlerden destekler. Örneğin, değer sayısı Denetçide bir değer seçin ve ardından bu değeri eklemek veya bu değeri hariç tutmak için bir filtre uygulamak sağlar. Denetçiler, dönüşümler için bağlamı da sağlayabilir. Örneğin, bir veya daha fazla sütun seçmek uygulanabilir olası dönüşümleri değiştirir.

Bir sütun birden çok denetçiler (örneğin, sütun istatistikleri ve bir çubuk grafik) zaman içinde herhangi bir noktada olabilir. Ayrıca olabilir bir denetçi örneklerini birden çok sütunda. Örneğin, tüm sayısal sütunları histogramlar aynı anda sahip olabilir.

Denetçiler profil oluşturma da ekranın alt kısmındaki görünür.  Ana içerik alanı daha büyük bir bunları görmek için denetçiler en üst düzeye çıkarın. Veri Kılavuzu varsayılan denetçi olarak düşünün. Ana içerik alanı herhangi denetçisi genişletilebilir. Ana içerik alanı içinde denetçiler profil oluşturma düzgün en aza indirin. Bir denetçi, denetleyici içinde kalem simgesine tıklayarak özelleştirin. Kullanılarak içinde yeniden sıralama denetçiler sürükleyip bırakın.

Bazı denetçiler "Halo" modu destekler. En son dönüştürme uygulanmadan önce bu mod değer veya durumu gösterir. Eski değeri gri renkle ön planda geçerli değeri görüntülenir ve dönüşümü etkisini gösterir. [Ek 4, desteklenen denetçiler geçerli listesini sağlar](data-prep-appendix4-supported-inspectors.md).

### <a name="the-destination"></a>Hedef
 Bir hedef olduğu, yazma/verileri bir veri akışı hazırladıktan sonra dışarı aktarma ' dir. Belirli bir veri akışı birden çok hedefe sahip olabilir. Her hedef yapılandırılması izin vermek için farklı bir kullanıcı deneyimi vardır. Hedef, "dikdörtgen" / tablo biçimindeki verileri kullanır ve belirli bir biçimdeki bir konuma yazıyor. [Ek 5 desteklenen hedefleri geçerli listesini sağlayan](data-prep-appendix5-supported-destinations.md).

### <a name="using-data-preparation"></a>Veri hazırlama kullanma ###
Veri hazırlama temel beş adımı Metodoloji/yönelik bir yaklaşım veri hazırlama varsayar.

#### <a name="step-1-ingestion"></a>1. adım: alma ####
Verileri içeri aktarma için veri hazırlama kullanarak **veri kaynağı Ekle** Proje Görünümü ' seçeneği.  Verilerin tüm ilk alımı, veri kaynağı Sihirbazı işlenir.

#### <a name="step-2-understandprofile-the-data"></a>2. adım: Anlamak / veri profili ####

İlk olarak, her bir sütunun üst veri kalitesi çubuğu bakın. Yeşil değerler içeren satırları temsil eder. Gri, eksik bir değeri, null, vb. ile satırları temsil eder. Kırmızı hata değerlerini gösterir. Tam sayı satır içeren bir araç ipucu her üç kadar demeti almak için çubuğu üzerine gelin. Veri Kalitesi çubuğu kullandığı Logaritmik ölçek bu nedenle, her zaman eksik veri hacmi için kaba bir genel görünüm almak için gerçek sayıları denetleyin.

![Sütunları](media/data-prep-getting-started/columns.png)

Ardından, veri özelliklerini daha iyi anlamak için diğer Inspector ayrıca kılavuzun bir birleşimini kullanın.  Daha fazla analiz için gerekli veri hazırlama hakkında varsayımlar formulating başlatın. Çoğu denetçiler, tek bir sütun veya sütunlar az sayıda çalışır.  

Çeşitli sütunları arasında birden fazla denetçiler için verileri anlamak için gerekli olan olasıdır. Çeşitli Inspector iyi profil oluşturma ile kaydırma yapabilirsiniz. Kutusu içinde ayrıca denetçiler listenin başındaki hemen görüntülenebilir alanında görmeniz için taşıyabilirsiniz.

![Denetçiler](media/data-prep-getting-started/inspectors.PNG)

Farklı denetçiler sürekli vs kategorik değişkenleri/sütunlar için sağlanır. Inspector menüsünü etkinleştirir ve türüne bağlı olarak sahip olduğunuz değişkenler/sütun seçenekleri devre dışı bırakır.

Fazla sayıda sütun içeren geniş veri kümeleriyle çalışırken, alt kümeleri ile çalışma Pragmatik bir yaklaşım önerilir. Bu yaklaşım, az sayıda sütun (örneğin, 5-10) üzerinde odaklanarak, bunları hazırlama ve ardından kalan sütunlar ile çalışma içerir. Kılavuz denetçisi 300'den fazla sütun "aracılığıyla sayfa" ihtiyacınız varsa dikey sütun ve bunu bölümlemeyi destekler.
 

#### <a name="step-3-transform-the-data"></a>3. adım: verileri dönüştürme ####
Dönüştürmeler verileri değiştirin ve geçerli çalışma varsayım desteklemek üzere verileri yürütmeye izin verilir. Dönüşümler adım listesindeki adımları sağ tarafında görünür. Bu adım listesindeki herhangi bir rastgele noktasını tıklayarak "zaman seyahat" adım listesinde için mümkündür.

Belirli bir adımın solundaki yeşil bir simge çalıştıktan ve veri dönüştürme yürütülmesini yansıtır gösterir. Dikey çubuk adımın solundaki denetçiler verilerin geçerli durumunu gösterir.

![adımlar](media/data-prep-getting-started/steps.PNG)

Verilere küçük sık sık değişiklik yapıyorsanız ve varsayım geliştikçe her değişiklikten sonra (adım 4) doğrulamak için deneyin.

#### <a name="step-4-verify-the-impact-of-the-transformation"></a>4. adım: dönüştürme etkisini doğrulayın. 
Hipotezin doğru olup olmadığını belirleyin. Doğruysa, sonraki varsayım geliştirme ve yeni bir tane için 2-3 arasındaki adımları yineleyin. Yanlış son dönüştürmeyi geri ve yeni bir varsayım geliştirme ve 2-3 arasındaki adımları yineleyin.

Sahip olduğu doğru etkisi dönüşümü belirlemek için birincil denetçiler kullanmaktır. Var olanı kullan. Halo etkisi etkin ya da veri görüntülemek için birden çok denetçiler Başlat ile kullanım denetçiler noktaları süre verilir.

![Halo denetçisi](media/data-prep-getting-started/halo1.PNG) ![Halo denetçisi](media/data-prep-getting-started/halo2.PNG)

Dönüştürme geri almak için adımlar listesi UI sağ tarafta gidin. (Adımları liste bölmesi çıkar POP gerekebilir. İşaret eden çift köşeli çift Ayraca tıklayın açmak için sol). Panelinde geri almak istediğiniz yürütülmesi dönüştürme seçin. UI bloğunun sağ taraftaki açılan listeyi seçin. Şunlardan birini seçin **Düzenle** değişiklikleri yapmak veya **Sil** adımları listesi ve veri akışı dönüştürme kaldırmak için.

#### <a name="step-5-output"></a>5. adım: çıktı 
İle veri hazırlama tamamlandığında, veri akışı için bir çıktı yazabilirsiniz. Bir veri akışı birçok çıkışları olabilir. Dönüşümler menüsünden hangi çıktı veri kümesi olarak yazılacak istediğiniz seçebilirsiniz. Çıkışın hedef de seçebilirsiniz. 

## <a name="list-of-appendices"></a>Eklerin listesi 
[Ek 2 - desteklenen veri kaynakları](data-prep-appendix2-supported-data-sources.md)  
[Ek 3 - desteklenen dönüşümler](data-prep-appendix3-supported-transforms.md)  
[Ek 4 - desteklenen denetçiler](data-prep-appendix4-supported-inspectors.md)  
[Ek 5 - desteklenen hedefleri](data-prep-appendix5-supported-destinations.md)  
[Ek 6 - python'da filtre ifadeleri örneği](data-prep-appendix6-sample-filter-expressions-python.md)  
[Ek 7 - örnek Python ifadelerinde veri akışı dönüştürme](data-prep-appendix7-sample-transform-data-flow-python.md)  
[Ek 8 - Python örnek veri kaynakları](data-prep-appendix8-sample-source-connections-python.md)  
[Ek 9 - python'da hedef bağlantıları örneği](data-prep-appendix9-sample-destination-connections-python.md)  
[10 - ek örnek sütun Python'da dönüştürür.](data-prep-appendix10-sample-custom-column-transforms-python.md)  

## <a name="see-also"></a>Ayrıca Bkz.

[Gelişmiş veri hazırlama Öğreticisi](tutorial-bikeshare-dataprep.md)