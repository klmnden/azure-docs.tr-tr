---
title: Azure Machine Learning Studio'da Python machine learning betikleri - yürütme | Microsoft Docs
description: Anahatları Python betikleri Azure Machine Learning Studio, temel kullanım senaryoları, özellikler ve sınırlamalar için destek temel ilkeleri tasarlayın.
keywords: python machine Learning, pandas python pandas, python betikleri python betiklerini yürütme
services: machine-learning
documentationcenter: ''
author: ericlicoding
ms.custom: (previous ms.author=hshapiro, author=heatherbshapiro)
ms.author: amlstudiodocs
manager: hjerez
editor: cgronlun
ms.assetid: ee9eb764-0d3e-4104-a797-19fc29345d39
ms.service: machine-learning
ms.component: studio
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/29/2017
ms.openlocfilehash: 958dd91277a81a9082a4149d2b0026fc11bf882a
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/26/2018
ms.locfileid: "52317594"
---
# <a name="execute-python-machine-learning-scripts-in-azure-machine-learning-studio"></a>Azure Machine Learning Studio’da Python machine learning betikleri yürütme

Bu konuda, Python betikleri Azure Machine learning'de geçerli desteğini temel tasarım ilkeleri açıklanır. Sağlanan ana özellikleri de, dahil olmak üzere özetlenmiştir:

- Temel kullanım senaryoları yürütün
- bir web hizmetinde deneme Puanlama
- Varolan kodu alma desteği
- dışarı aktarma görselleştirmeler
- denetimli özellik seçimi gerçekleştirin
- bazı sınırlamaları anlama

[Python](https://www.python.org/) birçok veri bilimcileri, aracı Üstten kapaklı içinde vazgeçilmez bir araçtır. Şunları içerir:

* Zarif ve kısa sözdizimi 
* platformlar arası destek 
* çok miktarda güçlü kitaplıkları ve 
* Olgun bir geliştirme aracı. 

Python machine learning modelleme genellikle kullanılan bir iş akışının tüm aşamalardaki kullanılmaktadır:

- veri alma ve işleme 
- özellik oluşturma
- Modeli eğitimi 
- Model doğrulama
- Dağıtım modelleri

Azure Machine Learning Studio, makine öğrenimi denemesi ve aynı zamanda sorunsuz bir şekilde Microsoft Azure web Hizmetleri olarak yayımlama çeşitli parçalara katıştırma Python betiklerini destekler.

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]


## <a name="design-principles-of-python-scripts-in-machine-learning"></a>Machine Learning Python betiklerini tasarım ilkeleri

Azure Machine Learning Studio'da Python için birincil arabirim aracılığıyla olduğu [Python betiği yürütme] [ execute-python-script] Şekil 1'de gösterilen modülü.

![image1](./media/execute-python-scripts/execute-machine-learning-python-scripts-module.png)

![image2](./media/execute-python-scripts/embedded-machine-learning-python-script.png)

Şekil 1. **Python betiği yürütme** modülü.

[Python betiği yürütme] [ execute-python-script] modülü Azure ML Studio'daki en fazla üç girişleri kabul eder ve kendi R analog gibi (aşağıdaki bölümde açıklanmıştır) en fazla iki çıktılar üretir [R yürütün Betik] [ execute-r-script] modülü. Yürütülecek Python kodu olarak adlandırılmış bir özel parametre kutusuna girilen çağrılan işlev giriş noktası `azureml_main`. Bu modül uygulamak için kullanılan temel tasarım ilkeleri şunlardır:

1. *Python kullanıcılar için kullanılan deyimsel olmalıdır.* Python kullanıcıların çoğu kodlarını modülleri içinde işlevler olarak faktörü. Bu nedenle, yürütülebilir deyimlerin çok en üst düzey bir modülde koyarak nispeten nadir olarak rastlanıyor. Sonuç olarak, komut dosyası kutusunu da deyimleri yalnızca bir dizi aksine özel olarak adlandırılmış bir Python işlevi alır. İşlevde kullanıma sunulan standart Python kitaplık türleri gibi nesnelerdir [Pandas](http://pandas.pydata.org/) veri çerçevelerini ve [NumPy](http://www.numpy.org/) dizileri.
2. *Yüksek kaliteli yerel arasında olmalıdır ve bulut yürütme.* Python kodu yürütmek için kullanılan arka uç dayanır [Anaconda](https://store.continuum.io/cshop/anaconda/), platformlar arası bilimsel Python dağıtım yaygın olarak kullanılan. Bu, en yaygın Python paketlerini 200 yakın birlikte gelir. Bu nedenle, veri bilimcileri hata ayıklama ve Azure Machine Learning ile uyumlu Anaconda ortamlarının kendi kodlarına uygun. Ardından var olan bir geliştirme ortamı gibi kullanın [Ipython](http://ipython.org/) Not Defteri veya [Visual Studio için Python Araçları](https://aka.ms/ptvs), bir Azure ML deneme bir parçası olarak çalıştırmak için. `azureml_main` Giriş noktasıdır ve bu nedenle temel alınan bir Python işlevi *** Azure ML özgü kod veya yüklü SDK yazılabilir.
3. *Diğer Azure Machine Learning modüller ile birleştirilebilir sorunsuz olmalıdır.* [Python betiği yürütme] [ execute-python-script] modülü kabul eder, girdi ve çıktı, olarak standart Azure Machine Learning veri kümeleri. Temel çerçevesinde şeffaf ve verimli bir şekilde Azure ML ve Python çalışma zamanları arasında köprü. Bu nedenle Python, R ve SQLite çağıran de dahil olmak üzere mevcut Azure ML iş akışları ile birlikte kullanılabilir. Sonuç veri Bilimcisi iş akışları oluşturma:
   * ön işleme ve temizleme verileri için Python ve Pandas kullanma
   * birden fazla veri kümesi için form özelliklerini birleştirme, bir SQL dönüştürme için veri akışı
   * Azure Machine Learning'de algoritmalar kullanarak modeller eğitin 
   * değerlendirmek ve r kullanarak sonuçları işlem sonrası


## <a name="basic-usage-scenarios-in-ml-for-python-scripts"></a>Python betiklerini için ML temel kullanım senaryoları

Bu bölümde, biz bazı temel kullanımlarını anket [Python betiği yürütme] [ execute-python-script] modülü. Python modülü için giriş Pandas veri çerçevesi sunulur. İşlev içinde bir Python paketlenmiş tek bir Pandas veri çerçevesine döndürmelidir [dizisi](https://docs.python.org/2/c-api/sequence.html) gibi bir dizi, liste veya NumPy dizisi. Bu dizinin ilk öğesi, ardından ilk çıkış bağlantı noktasını modülünün döndürülür. Bu düzen, Şekil 2'de gösterilir.

![image3](./media/execute-python-scripts/map-of-python-script-inputs-outputs.png)

Şekil 2. Eşleme parametreleri için bağlantı noktalarını giriş ve çıkış bağlantı noktasına değerini döndürür.

Giriş bağlantı noktaları için parametreleri nasıl eşlenir, semantiği ayrıntılı `azureml_main` işlevi, Tablo 1'de gösterilmiştir:

![image1T](./media/execute-python-scripts/python-script-inputs-mapped-to-parameters.png)

Tablo 1. İşlev parametreleri için giriş bağlantı noktası eşlemesi.

İşlev parametreleri giriş bağlantı noktaları arasındaki eşleme, konumsal bir:

- İlk bağlı giriş bağlantı noktasını işlevinin ilk parametresi eşlenir. 
- İkinci giriş (bağlı değilse) işlevinin ikinci parametresi eşlenir.

Bkz: *veri analizi için Python* (O'Reilly, 2012) tarafından Batı McKinney Python Pandas ve nasıl etkili ve verimli şekilde verileri işlemek için kullanılmadan hakkında daha fazla bilgi için. 


## <a name="translation-of-input-and-output-types"></a>Çeviri girdi ve çıktı türleri 
Azure ML giriş veri kümelerinde veri çerçeveleri Pandas dönüştürülür. Azure ML veri kümelerine çıkış veri çerçevelerini dönüştürülür. Aşağıdaki dönüştürmeler gerçekleştirilir:

1. Dize ve sayısal bir sütun olarak dönüştürülür-olduğu ve bir veri kümesindeki eksik değerleri Pandas 'NA' değerlerine dönüştürülür. Aynı dönüştürme yolu geri olur (Pandas NA değerleri eksik değerleri Azure ML dönüştürülür).
2. Azure ML üzerinde dizin vektörleri Pandas içinde desteklenmez. Python işlevindeki tüm giriş veri çerçevelerini her zaman bir 64-bit sayısal dizin 0'dan eksi 1 satır sayısına sahip. 
3. Azure ML veri kümeleri, yinelenen sütun adları ve dize olmayan sütun adları olamaz. Bir çıkış veri çerçevesine sayısal olmayan sütunlar içeriyorsa, framework çağırır `str` sütun adları. Benzer şekilde, yinelenen sütun adları otomatik olarak adlarının benzersiz olduğundan sağlamak üzere karışmış. ' % S'soneki (2), ikinci yinelenen ve benzeri için ilk yinelenen (3) eklenir.


## <a name="operationalizing-python-scripts"></a>Python betiklerini faaliyete geçirmeye yönelik

Tüm [Python betiği yürütme] [ execute-python-script] bir web hizmeti olarak yayımlandığında bir Puanlama denemesine kullanılan modülleri çağrılır. Örneğin, Şekil 3 tek bir Python ifade değerlendirilemiyor kodu içeren bir Puanlama deneme gösterir. 

![image4](./media/execute-python-scripts/figure3a.png)

![image5](./media/execute-python-scripts/python-script-with-python-pandas.png)

Şekil 3. Python ifadenin değerlendirilmesi için web hizmeti.

Bir web hizmeti bu denemeden oluşturuldu:

- bir Python ifadesi (bir dize olarak) giriş olarak alır
- Python yorumlayıcısı gönderir 
- ifade hem değerlendirilen sonucunu içeren bir tablo döndürür.


## <a name="importing-existing-python-script-modules"></a>Mevcut Python betik modüllerini içeri aktarma

Birçok veri bilimcilerine yönelik yaygın bir kullanım örneği, mevcut Python betikleri Azure ML denemeleri dahil edilip derecelendirilir. sağlamaktır. Tüm kod olarak ve birleştirilmiş tek bir betik kutusuna yapıştırılan olduğunu göstermek zorunda kalmadan [Python betiği yürütme] [ execute-python-script] modülü Python modüllerini üçüncü giriş bağlantı noktası içeren bir zip dosyası kabul eder. Dosyanın çalışma zamanında yürütme framework tarafından sıkıştırması açılan ve içeriği Python yorumlayıcısı kitaplık yolunu eklenir. `azureml_main` İşlevi daha sonra içeri aktarabilir bu modülleri doğrudan giriş noktası.

Örneğin, dosyayı içeren basit bir "Hello, World" işlevi Hello.py göz önünde bulundurun.

![image6](./media/execute-python-scripts/figure4.png)

Şekil 4. Kullanıcı tanımlı işlev Hello.py dosyasında.

Ardından, Hello.zip Hello.py içeren bir dosya oluşturun:

![image7](./media/execute-python-scripts/figure5.png)

Şekil 5. Kullanıcı tanımlı Python kodu içeren zip dosyası.

Zip dosyası, Azure Machine Learning Studio'ya veri kümesi olarak karşıya yükleyin. Ardından, oluşturun ve üçüncü giriş bağlantı noktasına ekleyerek Hello.zip dosyasında Python kodu kullanan bir deneme çalıştırma **Python betiği yürütme** modülü, bu şekilde gösterildiği gibi.

![image8](./media/execute-python-scripts/figure6a.png)

![image9](./media/execute-python-scripts/figure6b.png)

Şekil 6. Kullanıcı tanımlı Python kodu ile örnek deneme bir zip dosyası olarak karşıya yüklendi.

Modül çıktı zip dosyası paketlenmemiş olduğunu gösterir ve işlev `print_hello` çalıştırıldıktan.
 
![image10](./media/execute-python-scripts/figure7.png)

Şekil 7. Kullanıcı tanımlı işlev içinde Kullanımdaki [Python betiği yürütme] [ execute-python-script] modülü.


## <a name="working-with-visualizations"></a>Görselleştirmeleri ile çalışma

Tarayıcıda görselleştirilebilir MatplotLib kullanılarak oluşturulan çizimler döndürülen [Python betiği yürütme][execute-python-script]. Ancak r kullanırken olduğu gibi çizimleri görüntüleri otomatik olarak yönlendirilmez Azure Machine Learning geri döndürülecek olmaları durumunda bu nedenle kullanıcı açıkça tüm çizimleri PNG dosyaları kaydetmeniz gerekir. 

MatplotLib görüntüleri oluşturmak için aşağıdaki yordamı tamamlamanız gerekir:

* arka uç "AGG için" varsayılan Qt tabanlı işleyicisi bağlantısı geçin. 
* Yeni bir şekil nesnesi oluşturma 
* Eksen alın ve tüm çizimleri içine oluştur 
* Şekil bir PNG dosyaya kaydet 

Bu işlem içinde Pandas scatter_matrix işlevini kullanarak bir dağılım çizim matris oluşturan aşağıdaki Şekil 8'de gösterilmiştir.

![image1v](./media/execute-python-scripts/figure-v1-8.png)

Şekil 8. Görüntüleri MatplotLib rakamları kaydetmek için kod.

Şekil 9, ikinci çıkış bağlantı noktasına döndürmek için daha önce gösterilen komut dosyası kullanan bir denemeyi çizer gösterir.

![image2v](./media/execute-python-scripts/figure-v2-9a.png) 

![image2v](./media/execute-python-scripts/figure-v2-9b.png) 

Şekil 9. Python koddan oluşturulan çizimleri görselleştirme.

Farklı görüntülere kaydederek birden çok şekil dönmek mümkündür, Azure Machine Learning çalışma zamanı tüm görüntülerini seçer ve görselleştirme için art arda ekler.


## <a name="advanced-examples"></a>Gelişmiş örnekleri

Azure Machine Learning'de yüklenmiş Anaconda ortam NumPy, SciPy ve Scikits öğrenin gibi yaygın paketleri içerir. Bu paketleri, bir makine öğrenimi işlem hattı içindeki çeşitli veri işleme görevleri için etkili bir şekilde kullanılabilir. Örnek olarak, betik ve aşağıdaki denemenin topluluğu öğrencileriyle Scikits-bir veri kümesi için özellik önem puanlarını hesaplama öğrenin, kullanımını gösterir. Puanları, önce başka bir ML modeline iletilir ve denetimli özellik seçimi gerçekleştirmek için kullanılabilir.

Önem derecesi puanları ve özellikleri puanları temel alarak sırası hesaplamak için kullanılan Python işlevi şu şekildedir:

![image11](./media/execute-python-scripts/figure8.png)

Şekil 10. Sıra özellikleri tarafından puanları işlev.
 
Aşağıdaki denemenin hesaplar ve Azure Machine learning'de "Pima Hint Ailelere" veri kümesindeki özelliklerinin önem puanlarını döndürür:

![image12](./media/execute-python-scripts/figure9a.png)
![image13](./media/execute-python-scripts/figure9b.png)    

Şekil 11. Sıralama özelliklerine Pima Hint Ailelere kümesindeki denemeler yapın.

## <a name="limitations"></a>Sınırlamalar
[Python betiği yürütme] [ execute-python-script] şu anda aşağıdaki sınırlamalara sahiptir:

1. *Korumalı yürütme.* Python çalışma zamanını, şu anda korumalı ve sonuç olarak, ağ veya yerel dosya sistemi erişimini kalıcı bir biçimde izin vermiyor. Yerel olarak kaydedilmiş tüm dosyalar yalıtılmış ve modülü tamamladıktan sonra silinir. Python kodu geçerli dizin ve alt dizinlerinde olan özel durum üzerinde çalıştığı makinede çoğu dizinlere erişemez.
2. *Gelişmiş geliştirme ve hata ayıklama desteği eksikliği.* Python modülü, IntelliSense ve hata ayıklama gibi IDE özellikleri şu anda desteklemiyor. Ayrıca, çalışma zamanında modülü başarısız olursa, tam Python yığın izlemesi kullanılabilir. Ancak, modül için çıktı günlüğüne görüntülenmelidir. Şu anda, geliştirme ve Ipython gibi bir ortamda Python betiklerinde hata ayıklamak ve ardından kod modülüne içe aktarın öneririz.
3. *Tek bir veri çerçevesi çıktı.* Python giriş noktası, yalnızca bir tek veri çerçevesine çıktı olarak verilecek izin verilir. Geri Azure Machine Learning çalışma zamanı rastgele Python nesneleri doğrudan eğitilen modelleri gibi döndürmek şu anda mümkün değildir. Gibi [R betiği yürütme][execute-r-script]aynısına sahip, çoğu durumda, nesneleri bir bayt dizisine okunamayacak pickle ve ardından, bir veri çerçevesi içinde dönmek mümkündür.
4. *Python yüklemeyi özelleştirmek için bağlanamama*. Şu anda, özel bir Python modüllerini eklemek için yalnızca daha önce açıklanan zip dosyası mekanizması yoludur. Küçük modüller için uygun olsa da büyük modülleri (özellikle de yerel DLL'leri ile) ya da çok sayıda modüller için yavaşlatan bir yöntemdir. 

## <a name="conclusions"></a>Sonuçları
[Python betiği yürütme] [ execute-python-script] modül mevcut Python kodu Azure Machine Learning ve için bulutta barındırılan machine learning iş akışlarınızla sorunsuzca birleştirmek bir veri Bilimcisi sağlar bir web hizmetinin bir parçası olarak çalışır hale getirme. Python betik modülü, doğal olarak Azure Machine learning'de diğer modüllerle birlikte çalışır. Modül görevlerden veri keşfi önceden işleme ve ayıklama özellik ve daha sonra değerlendirmesi için bir dizi ve sonuçların sonrası işleme için kullanılabilir. Yürütme için kullanılan arka uç çalışma zamanı Anaconda, iyi test edilmiş ve yaygın olarak kullanılan bir Python dağıtım temel alır. Bu arka uç, var olan yerleşik kod varlıklarına buluta kolaylaştırır.

İçin ek işlevler sağlamasına olanak bekliyoruz [Python betiği yürütme] [ execute-python-script] eğitmek ve Python modelleri kullanıma hazır hale getirmek için ve geliştirme için daha iyi destek ekleme olanağı gibi modülü ve Azure Machine Learning Studio'da kodda hata ayıklama.

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi için bkz. [Python Geliştirici Merkezi](https://azure.microsoft.com/develop/python/).

<!-- Module References -->
[execute-python-script]: https://msdn.microsoft.com/library/azure/cdb56f95-7f4c-404d-bde7-5bb972e6f232/
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
