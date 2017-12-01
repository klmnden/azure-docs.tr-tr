---
title: "Python makine öğrenimi betiklerini yürütme | Microsoft Docs"
description: "Anahatları Azure Machine Learning ve temel kullanım senaryoları, özellikleri ve sınırlamaları Python komut dosyaları için destek temel ilkeleri tasarlayın."
keywords: "python makine öğrenimi, pandas, python pandas, python komut dosyaları, python betiği yürütme"
services: machine-learning
documentationcenter: 
author: bradsev
manager: cgronlun
editor: cgronlun
ms.assetid: ee9eb764-0d3e-4104-a797-19fc29345d39
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/29/2017
ms.author: bradsev;garye
ms.openlocfilehash: c25f31ca72417672298657c4585184ad72db6c99
ms.sourcegitcommit: 5a6e943718a8d2bc5babea3cd624c0557ab67bd5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/01/2017
---
# <a name="execute-python-machine-learning-scripts-in-azure-machine-learning-studio"></a>Azure Machine Learning Studio’da Python machine learning betikleri yürütme

Bu konuda, Azure Machine Learning Python komut dosyalarını geçerli desteği için temel alınan tasarım ilkeleri açıklanmaktadır. Sağlanan ana özellikleri de, dahil olmak üzere özetlenmiştir:

- Temel kullanım senaryoları yürütme
- bir web hizmeti bir denemeyi puan
- Varolan kod içeri aktarma desteği
- görselleştirmeleri dışarı aktarma
- denetimli özellik seçimi gerçekleştirin
- bazı sınırlamalar anlama

[Python](https://www.python.org/) birçok veri bilimcilerine, aracı Üstten kapaklı içinde vazgeçilmez bir araçtır. Şunları içerir:

* bir zarif ve kısa sözdizimi 
* platformlar arası desteği 
* çok miktarda güçlü kitaplıkları ve 
* Olgun geliştirme araçları. 

Python makine öğrenimi modelleme genelde kullanılan bir iş akışının tüm aşamaları kullanılıyor:

- veri alma ve işleme 
- özellik oluşturma
- Model eğitimi 
- Model doğrulama
- dağıtım modeli

Azure Machine Learning Studio, bir makine öğrenimi denemesinin ve aynı zamanda sorunsuz bir şekilde Microsoft Azure web Hizmetleri olarak yayımlama çeşitli parçalara katıştırma Python komut dosyalarını destekler.

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]


## <a name="design-principles-of-python-scripts-in-machine-learning"></a>Machine Learning Python betiklerde tasarım ilkeleri

Azure Machine Learning Studio'da Python birincil arabirim durumda [Python betiği yürütme] [ execute-python-script] Şekil 1'de gösterilen modülü.

![image1](./media/execute-python-scripts/execute-machine-learning-python-scripts-module.png)

![image2](./media/execute-python-scripts/embedded-machine-learning-python-script.png)

Şekil 1 '. **Python betiği yürütme** modülü.

[Python betiği yürütme] [ execute-python-script] modül Azure ML Studio'da en çok üç girişleri kabul eder ve onun R analog gibi (aşağıdaki bölümde açıklanmıştır) en fazla iki çıkışları üretir [R betiği yürütün] [ execute-r-script] modülü. Yürütülecek Python kodu olarak özel olarak adlandırılmış parametre kutusuna girilen giriş noktası olarak adlandırılan işlevi `azureml_main`. Bu modül uygulamak için kullanılan temel tasarım ilkeler şunlardır:

1. *Python kullanıcılar için kullanılan deyimsel olması gerekir.* Çoğu Python kullanıcılar kendi kod modülleri içinde işlevler olarak öğeli. Bu nedenle executable deyimleri çok üst düzey bir modüle koyma görece olarak daha ender. Sonuç olarak, komut dosyası kutusunu deyimleri yalnızca bir dizi aksine özel olarak adlandırılmış bir Python işlevi alır. İşlevinde sunulan standart Python kitaplığı türleri gibi nesneleridir [Pandas](http://pandas.pydata.org/) veri çerçevelerini ve [NumPy](http://www.numpy.org/) dizileri.
2. *Yüksek kaliteli yerel arasında olmalıdır ve bulut yürütmeleri.* Python kodu yürütmek için kullanılan arka uç dayanır [Anaconda](https://store.continuum.io/cshop/anaconda/), platformlar arası bilimsel Python dağıtım yaygın olarak kullanılır. Bu en yaygın Python paketlerini 200 yakın ile birlikte gelir. Bu nedenle, veri bilimcileri hata ayıklama ve yerel Azure Machine Learning uyumlu Anaconda ortamlarına kendi kodlarına hakkını kullanmaya devam eder. Varolan bir geliştirme ortamı gibi kullandığınız [IPython](http://ipython.org/) dizüstü bilgisayar veya [Visual Studio için Python Araçları](http://aka.ms/ptvs), bir Azure ML deneme bir parçası olarak çalıştırmak için. `azureml_main` Giriş noktasıdır temel alınan bir Python işlevi ve bu nedenle *** Azure ML özgü kodu veya yüklü SDK yazılabilir.
3. *Diğer Azure Machine Learning modüller ile sorunsuz bir şekilde birleştirilebilir olması gerekir.* [Python betiği yürütme] [ execute-python-script] modülü kabul eder, girişleri ve çıkışları, standart Azure Machine Learning veri kümeleri. Temel çerçevesinde saydam ve verimli bir şekilde Azure ML ve Python çalışma zamanları arasında köprü. Bu nedenle Python R ve SQLite çağıran dahil olmak üzere var olan Azure ML iş akışları ile birlikte kullanılabilir. Sonuç veri Bilimcisi iş akışları oluşturma:
   * Python ve Pandas önceden işlenmesi ve temizleme verileri için kullanın
   * formun özellikleri için birden fazla veri kümesi birleştirme bir SQL dönüştürmesi için veri akışı
   * Azure Machine Learning algoritmaları kullanarak modelleri eğitme 
   * değerlendirin ve işlem sonrası r kullanarak sonuçları


## <a name="basic-usage-scenarios-in-ml-for-python-scripts"></a>ML Python komut dosyaları için temel kullanım senaryosu

Bu bölümde, biz bazı temel kullanımlarını anket [Python betiği yürütme] [ execute-python-script] modülü. Python modülü girişleri Pandas veri çerçevelerini sunulur. İşlev Python içinde paketlenmiş tek bir Pandas veri çerçevesi döndürmelidir [dizisi](https://docs.python.org/2/c-api/sequence.html) dizi, liste veya NumPy dizi gibi. Bu dizisinin ilk öğesi sonra ilk çıkış bağlantı noktasına modülün döndürülür. Şekil 2'de bu düzeni gösterilir.

![image3](./media/execute-python-scripts/map-of-python-script-inputs-outputs.png)

Şekil 2. Eşleme parametreleri bağlantı noktalarına giriş ve çıkış bağlantı noktasına dönüş değeri.

Giriş bağlantı noktaları için parametrelerinin nasıl eşlenen, semantiği ayrıntılı `azureml_main` işlevi, Tablo 1'de gösterilir:

![image1T](./media/execute-python-scripts/python-script-inputs-mapped-to-parameters.png)

Tablo 1. İşlev parametreleri için Giriş noktalarının eşleme.

Giriş bağlantı noktaları ve işlev parametreleri arasında eşleme konumsal şöyledir:

- İlk bağlı giriş bağlantı noktası işlevinin ilk parametresi eşlenir. 
- İkinci giriş (bağlı değilse) işlevinin ikinci parametresi eşlenir.

Bkz: *veri analizi için Python* (O'Reilly, 2012) Python Pandas ve nasıl etkili ve verimli şekilde verileri işlemek için kullanılmadan hakkında daha fazla bilgi için Batı McKinney tarafından. 


## <a name="translation-of-input-and-output-types"></a>Çeviri girdi ve çıktı türü 
Azure ML giriş veri kümelerinde veri Pandas çerçevelere dönüştürülür. Çıktı veri çerçevelerini Azure ML veri kümelerine dönüştürülür. Aşağıdaki dönüşümleri gerçekleştirilir:

1. Dize ve sayısal sütunlar olarak dönüştürülür-olduğundan ve bir veri kümesinde eksik değerleri Pandas 'NA' değerlerine dönüştürülür. Aynı dönüştürme geri yolunda olur (Pandas BELİRTİLMEYEN değerler Azure ML eksik değerleri dönüştürülür).
2. Azure ML dizin vektörlerinin Pandas içinde desteklenmiyor. Python işlevi tüm giriş verilerini çerçevelere her zaman bir 64-bit sayısal dizini 0'dan eksi 1 satır sayısına sahip. 
3. Azure ML veri kümeleri, yinelenen sütun adları ve dize olmayan sütun adları olamaz. Bir çıkış veri çerçevesi sayısal olmayan sütunları içeriyorsa, framework çağırması `str` sütun adları. Benzer şekilde, yinelenen sütun adları otomatik olarak adları benzersiz güvence altına almaya karıştırılmış. (2) soneki ilk yinelenen, (3) ikinci yinelenen ve benzeri için eklenir.


## <a name="operationalizing-python-scripts"></a>Faaliyete geçirmeye yönelik Python komut dosyaları

Tüm [Python betiği yürütme] [ execute-python-script] bir Puanlama deneme kullanılan modülleri, bir web hizmeti olarak yayımlandığında çağrılır. Örneğin, Şekil 3'te tek bir Python ifade değerlendirmek için kodu içeren bir Puanlama deneme gösterir. 

![image4](./media/execute-python-scripts/figure3a.png)

![image5](./media/execute-python-scripts/python-script-with-python-pandas.png)

Şekil 3 '. Python ifade değerlendirme için web hizmeti.

Bu deneme oluşturulan web hizmeti:

- Python ifade (bir dize olarak) giriş olarak alır
- Python yorumlayıcı gönderir 
- ifade ve değerlendirilen sonuç içeren bir tablo döndürür.


## <a name="importing-existing-python-script-modules"></a>Varolan Python betik modüllerini içeri aktarma

Birçok veri bilimcilerine için ortak bir kullanım örneği mevcut Python betiklerini Azure ML denemeler birleştirmek için ' dir. Tüm kod birleştirilmiş ve tek bir komut dosyası kutusuna yapıştırılan gerektirmek yerine [Python betiği yürütme] [ execute-python-script] modülü Python modülleri üçüncü giriş bağlantı noktası içeren bir zip dosyası kabul eder. Dosyanın çalışma zamanında yürütme framework tarafından sıkıştırması açılmış ve içeriği Python yorumlayıcı kitaplık yoluna eklenir. `azureml_main` İşlevi aktarabilirsiniz bu modülleri doğrudan giriş noktası.

Örnek olarak, basit bir "Hello, World" işlevini içeren Hello.py dosya göz önünde bulundurun.

![image6](./media/execute-python-scripts/figure4.png)

Şekil 4 '. Kullanıcı tanımlı işlev Hello.py dosyasında.

Ardından, bir dosya Hello.py içeren Hello.zip oluşturun:

![image7](./media/execute-python-scripts/figure5.png)

Şekil 5. Kullanıcı tanımlı Python kodu içeren zip dosyası.

ZIP dosyasının Azure Machine Learning Studio'ya bir veri kümesi karşıya yükleyin. Ardından oluşturma ve Python kodu üçüncü giriş bağlantı noktasına ekleyerek Hello.zip dosyasında kullanan bir denemeyi çalıştırma **Python betiği yürütme** bu şekilde gösterildiği gibi modülü.

![image8](./media/execute-python-scripts/figure6a.png)

![image9](./media/execute-python-scripts/figure6b.png)

Şekil 6. Kullanıcı tanımlı Python kodu ile örnek deneme bir zip dosyası olarak karşıya yüklendi.

Zip dosyası paketlenmemiş taleplere modülü çıktısını gösterir ve işlev `print_hello` çalıştırılmış olabilir.
 
![image10](./media/execute-python-scripts/figure7.png)

Şekil 7. Kullanıcı tanımlı işlev içinde kullanımda [Python betiği yürütme] [ execute-python-script] modülü.


## <a name="working-with-visualizations"></a>Görsel öğeleri ile çalışma

Tarayıcıda canlandırılabilir MatplotLib kullanılarak oluşturulan çizimler tarafından döndürülmesi [Python betiği yürütme][execute-python-script]. Ancak bunlar r kullanırken olduğu çizimleri görüntüleri otomatik olarak yönlendirilmez Azure Machine Learning geri döndürülecek olmaları durumunda bu nedenle kullanıcı açıkça herhangi çizimleri PNG dosyaları için kaydetmeniz gerekir. 

MatplotLib görüntüleri oluşturmak için aşağıdaki yordamı tamamlamanız gerekir:

* arka uç "AGG" varsayılan Qt tabanlı Oluşturucu ' geçiş 
* Yeni bir şekil nesnesi oluşturun 
* Eksen alın ve tüm çizimleri içine oluştur 
* Şekil bir PNG dosyaya kaydet 

Bu işlem içinde Pandas scatter_matrix işlevini kullanarak bir dağılım çizim matris oluşturan aşağıdaki Şekil 8'de gösterilmiştir.

![image1v](./media/execute-python-scripts/figure-v1-8.png)

Şekil 8'de. Görüntüleri MatplotLib rakamları kaydetmek için kod.

Şekil 9 ikinci çıkış bağlantı noktasına geri dönmek için daha önce gösterilen komut dosyası kullanan bir denemeyi çizer gösterir.

![image2v](./media/execute-python-scripts/figure-v2-9a.png) 

![image2v](./media/execute-python-scripts/figure-v2-9b.png) 

Şekil 9. Python koddan oluşturulan çizimleri görselleştirme.

Farklı görüntülere kaydederek birden çok rakamları Geri dönülebilir, Azure Machine Learning çalışma zamanı tüm görüntülerini seçer ve görselleştirme için art arda ekler.


## <a name="advanced-examples"></a>Gelişmiş örnekleri

Azure Machine Learning ile yüklenmiş Anaconda ortamı NumPy, SciPy ve Scikits öğrenin gibi ortak paketleri içerir. Bu paketleri, machine learning ardışık düzen çeşitli veri işleme görevler için etkili bir şekilde kullanılabilir. Örnek olarak, aşağıdaki deneme ve komut dosyası ensemble öğrencileriyle Scikits-özellik önem puanları bir veri kümesi için işlem öğrenin içinde kullanımını gösterir. Puanları, başka bir ML modeline sat önce denetimli özellik seçimi gerçekleştirmek için kullanılabilir.

Önem derecesi puanlarını ve özellikleri puanları temel alarak sırası hesaplamak için kullanılan Python işlevi şöyledir:

![image11](./media/execute-python-scripts/figure8.png)

Şekil 10. Sıra özellikleri tarafından puanları işlev.
 Ardından aşağıdaki deneme hesaplar ve Azure Machine Learning "Pima Hint Diabetes" kümesinde özelliklerinin önem puanları döndürür:

![image12](./media/execute-python-scripts/figure9a.png)
![image13](./media/execute-python-scripts/figure9b.png)    

Şekil 11. Sıralama özelliklerine Pima Hint Diabetes kümesindeki deneyin.

## <a name="limitations"></a>Sınırlamalar
[Python betiği yürütme] [ execute-python-script] şu anda aşağıdaki sınırlamalara sahiptir:

1. *Korumalı yürütme.* Python çalışma zamanı şu anda korumalı ve sonuç olarak, ağ veya yerel dosya sistemine erişim kalıcı bir biçimde izin vermiyor. Yerel olarak kaydedilen tüm dosyaları yalıtılmış ve modül tamamlandıktan sonra silinir. Python kodu geçerli dizinde ve alt dizinlerinde olan özel durum, çalıştıran makinede çoğu dizinlere erişemez.
2. *Gelişmiş geliştirme ve hata ayıklama desteği eksiği.* Python modülü IntelliSense ve hata ayıklama gibi IDE özellikleri şu anda desteklemiyor. Ayrıca, çalışma zamanında modülü başarısız olursa, tam Python yığın izlemesi mevcuttur. Ancak, modül için çıktı günlüğüne görüntülenmelidir. Şu anda, geliştirmek ve Python komut dosyaları IPython gibi bir ortamda hata ayıklama ve ardından kod modülüne içe aktarın öneririz.
3. *Tek bir veri çıkışı çerçevesi.* Python giriş noktası yalnızca bir tek veri çerçevesi çıktı olarak döndürmek için izin verilir. Rasgele Python nesneler doğrudan eğitilmiş modeller gibi Azure Machine Learning çalışma döndürmek şu anda mümkün değildir. Gibi [R betiği yürütün][execute-r-script]aynısına sahip, nesneleri bir bayt dizisine pickle ve, bir veri çerçevesi içinde geri dönmek için birçok durumda mümkündür.
4. *Python yükleme özelleştirmek için bağlanamama*. Şu anda özel Python modülleri eklemek için yalnızca daha önce açıklanan zip dosyası mekanizması yoludur. Bu küçük modülleri için uygun olmakla birlikte, büyük modülleri (özellikle de yerel DLL'leri ile) veya çok sayıda modülleri için sıkıcı. 

## <a name="conclusions"></a>Sonuçları
[Python betiği yürütme] [ execute-python-script] modülü bir veri Bilimcisi varolan Python kodu bulutta barındırılan machine learning iş akışlarında Azure Machine Learning eklemenizi ve sorunsuz bir şekilde web hizmetinin bir parçası olarak faaliyete geçirmenin sağlar. Python komut satırı modülü, doğal olarak Azure Machine Learning diğer modülleri ile birlikte çalışır. Modül, görevleri veri keşfi ön işleme ve özelliği ayıklama ve ardından değerlendirme için bir aralığı ve sonuçları sonrası işlemler için kullanılabilir. Yürütme için kullanılan arka uç çalışma zamanı Anaconda, iyi test edilmiş ve yaygın olarak kullanılan bir Python dağıtımı temel alır. Bu arka uç onu sizin için yerleşik varolan kod varlıklarına bulutunu kolaylaştırır.

Ek işlevsellik sağlamak bekliyoruz [Python betiği yürütme] [ execute-python-script] modülü eğitmek ve Python modellerinde faaliyete geçirmek için ve daha iyi geliştirme ve Azure Machine Learning Studio'da kodda hata ayıklama desteği ekleme yeteneği gibi.

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi için bkz. [Python Geliştirici Merkezi](https://azure.microsoft.com/develop/python/).

<!-- Module References -->
[execute-python-script]: https://msdn.microsoft.com/library/azure/cdb56f95-7f4c-404d-bde7-5bb972e6f232/
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
