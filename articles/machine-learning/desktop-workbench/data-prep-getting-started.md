---
title: Azure Machine Learning için verileri hazırlama ile çalışmaya başlama | Microsoft Docs
description: Başlangıç kılavuzuna AML ekranının veri hazırlık bölümü için budur
services: machine-learning
author: cforbe
ms.author: cforbe
manager: mwinkle
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 09/07/2017
ms.openlocfilehash: b0fbb0af433cfad6693b022d7a00373dc39533aa
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/19/2018
---
# <a name="getting-started-with-data-preparation"></a>Veri hazırlama ile çalışmaya başlama

Veri hazırlama Başlarken Kılavuzu'na Hoş. 

Veri hazırlama verimli bir şekilde keşfetme, anlama ve verileri sorunlarını giderme için araçlar sağlar. Birçok formlarda verileri kullanmak ve daha iyi Aşağı Akış kullanımı için uygun cleansed veri verileri dönüştürmek sağlar.

Veri hazırlama Azure Machine Learning çalışma ekranı deneyimi bir parçası olarak yüklenir.  Veri hazırlama yerel olarak kullanın veya bir hedef küme dağıtın ve çalışma zamanı veya yürütme ortamı olarak bulut.

Tasarım zamanı çalışma zamanı genişletilebilirlik için Python kullanır ve Pandas gibi çeşitli Python kitaplıkları bağlıdır. Azure ML ekranının diğer bileşenlerle olduğundan Python yüklemek gerekmez, sizin için yüklenir. Ek kitaplıklara yüklemeniz gerekiyorsa, ancak bu kitaplıklar olmasına gerek olmayan normal Python dizininizi Azure ML çalışma ekranı Python dizinde yüklü. Paketleri yükleme hakkında daha fazla ayrıntı bulunabilir [burada](data-prep-python-extensibility-overview.md).

Proje oluşturulduktan sonra veri hazırlığı kullanabilmeniz için önce bir proje veri hazırlayabilirsiniz gereklidir. 

Veri simgesini seçerek proje veri bölümüne gidin ![veri kaynağı simgesi](media/data-prep-getting-started/data-source-icon.png) ekranın sol.  Tıklayın "+" yeniden **bir veri kaynağı ekleme**. Veri kaynağı Sihirbazı'nı başlatma ve ekler bir **veri kaynağı** Sihirbazı tamamlandıktan sonra proje dosyasına (.dsource). Varsayılan olarak, verilerin görünümünü kılavuz ' dir. Kılavuz, ayrıca ölçümleri görünüm seçmek mümkündür. Ölçümleri görünümünde Özet istatistikleri gösterilir.  Özet istatistikleri gözden geçirdikten sonra tıklayın **Prepare** ekranın üstünde. [Veri kaynağı Sihirbazı hakkında daha fazla bilgi](data-source-wizard.md) 

## <a name="building-blocks-of-data-preparation"></a>Veri hazırlama yapı taşları ##
### <a name="the-package"></a>Paket ###

Bir paketi, iş için birincil kapsayıcıdır. Kaydedilen ve diskten yüklenen yapı paketidir. İstemci içindeki çalışırken sürekli otomatik kaydedilmiş arka planda paketidir. 

Bir paket çıkış/sonuçlarını Python veya Jupyter not defteri yoluyla keşfedilen.

Bir paketi yerel Python, Spark (Docker dahil) ve Hdınsight dahil olmak üzere birden çok çalışma zamanları arasında çalıştırılabilir.

Bir paket adımları ve verilere uygulanan Dönüşümlerin bir veya daha fazla veri akışları içerir.

Bir paketi başka bir paketi (bir başvuru veri akış adlandırılır) bir veri kaynağı olarak kullanabilir.

### <a name="the-dataflow"></a>Veri akışı ###
Bir kaynağı bir veri akışı vardır ve isteğe bağlı olan bir dizi adım ve isteğe bağlı hedefleri düzenlenmiş dönüştürür. Bir adımı tıklatarak tüm kaynakları ve dönüşümler önceki seçili adımı dahil yeniden yürütür.  Bu adım dönüştürülmüş verilerine ızgara içinde gösterilir. Adımları eklenebilir, taşınır ve adım listesi kullanılarak bir veri akışı içinde silinir.

İstemci sağ tarafındaki adım listesi açılır ve daha fazla ekran alanı sağlamak üzere kapalı.

Birden çok veri akışları kullanıcı Arabiriminde aynı anda var olabilen, her veri akışı kullanıcı arabiriminde bir sekme olarak temsil edilir.

### <a name="the-source"></a>Kaynak
Bir, burada verilerin geldiği ve biçimi olarak kaynağıdır. Bir veri hazırlığı paketi her zaman başka bir veri Flow(Data Source) kendi verilerden kaynaklanır. Bu başvuru bilgilerini içeren veri akışını değil. Her kaynak yapılandırılacak izin vermek için bir başka bir kullanıcı deneyimi olur. Kaynak verileri "dikdörtgen" / tablolu bir görünümünü oluşturur. Kaynak veri başlangıçta "düzensiz sağ" varsa, ardından yapısı "dikdörtgen" olmasını normalleştirilmiş [Ek 2 sağlar desteklenen kaynaklarının geçerli listesini](data-prep-appendix2-supported-data-sources.md).

### <a name="the-transform"></a>Dönüştürme ###
Dönüşümler verileri belirli bir biçimde kullanmak, (örneğin, veri türünü değiştirme) verileri başka bir işlem gerçekleştirmek ve verileri yeni biçiminde üretir. Her dönüştürme kendi kullanıcı Arabirimi ve behavior(s) vardır. Veri akışı adımlarda aracılığıyla birlikte birkaç dönüşümler zincirleme veri hazırlığı işlevlerin çekirdeği olur. [Ek 3 sağlar desteklenen dönüşümler geçerli listesini](data-prep-appendix3-supported-transforms.md).

### <a name="the-inspector"></a>Denetleyici ###

Denetçiler veri görselleştirmeleri ve veri anlayış geliştirmek kullanılabilir.  Veri ve veri anlama kalite hangi eylemleri (dönüşümler) alınıp alınmayacağını karar vermenize yardımcı verir. Bazı denetçiler dönüşümler oluşturacak eylemlerden destekler. Örneğin, değer sayısı denetçisi bir değer seçin ve ardından bu değeri eklemek için veya bu değere dışlamak için bir filtre uygulamak sağlar. Denetçiler dönüşümler için ayrıca içerik sağlayabilir. Örneğin, bir veya daha fazla sütun seçilmesi uygulanabilir olası dönüşümler değiştirir.

Bir sütun birden çok denetçiler herhangi bir noktada saati (örneğin, sütun istatistikleri ve bir Histogram) olabilir. Ayrıca olabilir Inspector örnekleri arasında birden çok sütun. Örneğin, tüm sayısal sütunlar Histogram aynı anda sahip olabilir.

Denetçiler profil da ekranın altındaki görünür.  Ana içerik alanı içinde daha büyük bir görebileceği denetçiler ekranı kaplamasını sağlayın. Veri Kılavuzu varsayılan denetçi olarak düşünün. Ana içerik alanı herhangi denetçisi genişletilebilir. Ana içerik alanı içinde denetçiler profil de için en aza indirin. Bir denetleyici denetçisi içinde kalem simgesine tıklayarak özelleştirin. Kullanılarak içinde yeniden sıralama denetçiler sürükleyin ve bırakın.

Bazı denetçiler "Hale" modunu destekler. En son dönüştürme uygulanmadan önce bu mod değer veya durumu gösterir. Eski değer ön planda geçerli değeriyle gri renkte görüntülenir ve bir dönüşüm etkisini gösterir. [Ek 4 sağlar desteklenen denetçiler geçerli listesini](data-prep-appendix4-supported-inspectors.md).

### <a name="the-destination"></a>Hedef
 Bir hedefi olduğu, yazma/verileri veri akışı hazırladıktan sonra dışarı aktarma değil. Belirli bir veri akışı, birden çok hedefe sahip olabilir. Her hedef yapılandırılacak izin vermek için bir başka bir kullanıcı deneyimi olur. Hedef, "dikdörtgen" / tablo biçiminde veri tüketir ve belirli bir biçimde bazı konuma yazar. [Ek 5 sağlar desteklenen hedefleri geçerli listesini](data-prep-appendix5-supported-destinations.md).

### <a name="using-data-preparation"></a>Veri hazırlama kullanma ###
Veri hazırlama bir temel beş adımı Metodoloji/yaklaşımı veri hazırlığı varsayar.

#### <a name="step-1-ingestion"></a>1. adım: alımı ####
Veri içeri aktarma verileri hazırlığı kullanarak **veri kaynağı Ekle** proje görünümü içinde seçeneği.  Tüm ilk alım verilerin veri kaynağı Sihirbazı gerçekleştirilir.

#### <a name="step-2-understandprofile-the-data"></a>2. adım: Anlamak / veri profili ####

İlk olarak, her bir sütunun üst veri kalitesi çubuğu bakın. Yeşil değerleri olan satırları temsil eder. Gri eksik bir değeri, null, vb. satırlarla temsil eder. Kırmızı hata değerlerini gösterir. Her üç demet satır tam sayı olan bir araç ipucu almak için çubuğu üzerine gelerek. Veri Kalitesi çubuğu kullandığı Logaritmik ölçek bu nedenle her zaman eksik veri birimi için bir kaba fikir almak için gerçek sayılar denetleyin.

![sütunları](media/data-prep-getting-started/columns.png)

Ardından, veri özelliklerini daha iyi anlamak için diğer denetçiler artı kılavuz bileşimini kullanın.  Varsayımlar hakkında daha fazla çözümleme için gerekli veri hazırlık formulating başlatın. Çoğu denetçiler tek bir sütun veya sütunlar az sayıda üzerinde çalışır.  

Birçok sütun boyunca birkaç denetçiler için verileri anlamak için gerekli olan olasıdır. Çeşitli Inspector iyi profil aracılığıyla gezinebilirsiniz. Kutusu içinde de Denetçiler listenin başında hemen görüntülenebilir alanında görmeniz için taşıyabilirsiniz.

![Denetçiler](media/data-prep-getting-started/inspectors.PNG)

Farklı denetçiler sürekli vs kategorik değişkenleri/sütunlar için sağlanır. Inspector menüsünü etkinleştirir ve değişkenleri/sütunları elinizde türüne bağlı olarak seçenekleri devre dışı bırakır.

Çok sayıda sütuna sahip geniş veri kümeleri ile çalışırken, alt kümeleri ile çalışma kolay bir yaklaşım önerilir. Bu yaklaşım, az sayıda sütun (örneğin, 5-10) üzerinde odaklanan, bunları hazırlama ve kalan sütunlarda çalışma içerir. Kılavuz denetçisi "aralarında sayfasında" yapmanız sonra 300'den fazla sütun varsa dikey sütunları ve bu nedenle bölümleme destekler.
 

#### <a name="step-3-transform-the-data"></a>3. adım: veri dönüştürme ####
Dönüşümler verileri değiştirmek ve geçerli çalışma varsayımınızın desteklemek için veri yürütülmesini sağlar. Dönüşümler sağ taraftaki adımları adım listesinde görünür. Adım listesindeki herhangi bir rastgele noktasını tıklayarak adım listesi kullanılarak "zaman seyahat" için mümkündür.

Verilen adım solundaki yeşil bir simge çalıştıktan ve veri dönüştürme yürütülmesi yansıtır gösterir. Adım solundaki dikey çubuk denetçiler verilerinde geçerli durumunu gösterir.

![adımlar](media/data-prep-getting-started/steps.PNG)

Küçük sık değişiklikler yapın ve varsayımını geliştikçe her bir değişiklikten sonra (adım 4) doğrulamak için deneyin.

#### <a name="step-4-verify-the-impact-of-the-transformation"></a>4. adım: dönüştürme etkisini doğrulayın. 
Varsayımını doğru olup olmadığını belirleyin. Doğruysa, sonraki varsayımınızın geliştirin ve için yeni bir 2-3 arasındaki adımları yineleyin. Yanlış durumunda son dönüştürme geri almak ve yeni bir varsayım geliştirmek ve 2-3 arasındaki adımları yineleyin.

Dönüştürme sağ etkisine sahip belirlemek için birincil yolu denetçiler kullanmaktır. Varolan kullanın. Kullanım denetçiler etkisi etkin ya da veri görüntülemek için birden çok denetçiler Başlat hale ile verilen sürede noktalarının.

![hale denetçisi](media/data-prep-getting-started/halo1.PNG) ![hale denetçisi](media/data-prep-getting-started/halo2.PNG)

Bir dönüşüm geri almak için adımlar listesi UI sağ tarafta gidin. (Adımları listesi paneli geri çıkışı Sil'i gerekebilir. Çift Köşeli Çift Ayraca işaret eden açmak için tıklatın sol). Masası'nda, geri almak istediğiniz yürütüldü dönüştürme seçin. UI blok sağ tarafında açılan seçin. Şunlardan birini seçin **Düzenle** değişiklik yapma veya **silmek** dönüştürme adımları listesi ve veri akışı kaldırmak için.

#### <a name="step-5-output"></a>5. adım: çıktı 
İle veri hazırlığı tamamlandığında, bir çıktı veri akışı yazabilirsiniz. Bir veri akışı birçok çıkışları olabilir. Dönüşümler menüsünden hangi olarak yazılacak dataset istediğiniz çıktı seçebilirsiniz. Çıkış hedef de seçebilirsiniz. 

## <a name="list-of-appendices"></a>Ekler listesi 
[Ek 2 - desteklenen veri kaynakları](data-prep-appendix2-supported-data-sources.md)  
[Ek 3 - desteklenen dönüştürür](data-prep-appendix3-supported-transforms.md)  
[Ek 4 - desteklenen denetçiler](data-prep-appendix4-supported-inspectors.md)  
[Ek 5 - desteklenen hedefleri](data-prep-appendix5-supported-destinations.md)  
[Ek 6 - Python örnek filtre ifadelerinde](data-prep-appendix6-sample-filter-expressions-python.md)  
[Ek 7 - örnek Python veri akışı ifadelerinde dönüştürme](data-prep-appendix7-sample-transform-data-flow-python.md)  
[Ek 8 - Python örnek veri kaynaklarında](data-prep-appendix8-sample-source-connections-python.md)  
[Ek 9 - Python örnek hedef bağlantıları](data-prep-appendix9-sample-destination-connections-python.md)  
[Ek 10 - örnek sütun Python içinde dönüştürür.](data-prep-appendix10-sample-custom-column-transforms-python.md)  

## <a name="see-also"></a>Ayrıca Bkz.

[Gelişmiş veri hazırlık Öğreticisi](tutorial-bikeshare-dataprep.md)