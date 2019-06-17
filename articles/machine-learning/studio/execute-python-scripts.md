---
title: Python machine learning betikleri yürütme
titleSuffix: Azure Machine Learning Studio
description: Azure Machine Learning Studio'da Python'ı kullanmayı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: xiaoharper
ms.author: amlstudiodocs
ms.custom: previous-author=heatherbshapiro, previous-ms.author=hshapiro
ms.date: 03/12/2019
ms.openlocfilehash: 4b4f3877b56752756050de0af226571ac2a93293
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60750863"
---
# <a name="execute-python-machine-learning-scripts-in-azure-machine-learning-studio"></a>Azure Machine Learning Studio’da Python machine learning betikleri yürütme

Python aracını Üstten kapaklı birçok veri bilimcileri, bir değerli araçtır. Bu, tipik machine learning iş akışı veri keşfi, özellik ayıklama, model eğitiminin ve doğrulama ve dağıtımı da dahil olmak üzere her aşamasında kullanılır.

Bu makalede, Azure Machine Learning Studio denemeleri ve web hizmetlerinizdeki Python kodu kullanmak için Python betiği yürütme modülünü nasıl kullanabileceğinizi açıklar.

## <a name="using-the-execute-python-script-module"></a>Python betiği yürütme modülünü kullanma

Studio'da Python için birincil arabirim aracılığıyladır [Python betiği yürütme] [ execute-python-script] modülü. En fazla üç girişleri kabul eder ve benzer şekilde en fazla iki çıktılar üretir [R betiği yürütme] [ execute-r-script] modülü. Python kodu aracılığıyla çağrılan özel olarak adlandırılmış giriş noktası işlev parametresi kutusuna girilen `azureml_main`.

![Python betik modülü Yürüt](./media/execute-python-scripts/execute-machine-learning-python-scripts-module.png)

![Örnek python kodu modülü parametresi kutusunda](./media/execute-python-scripts/embedded-machine-learning-python-script.png)

### <a name="input-parameters"></a>Giriş parametreleri

Python modülü için giriş Pandas DataFrames sunulur. `azureml_main` İşlevi parametre olarak isteğe bağlı iki Pandas veri çerçevelerini kabul eder.

İşlev parametreleri giriş bağlantı noktaları arasındaki eşleme, konumsal bir:

- İlk bağlı giriş bağlantı noktasını işlevinin ilk parametresi eşlenir.
- İkinci giriş (bağlı değilse) işlevinin ikinci parametresi eşlenir.
- Üçüncü giriş için kullanılan [ek Python modüllerini içeri aktarma](#import-modules).

Giriş bağlantı noktaları için parametreleri nasıl eşlenir, semantiği ayrıntılı `azureml_main` işlevi aşağıda gösterilmektedir.

![Giriş bağlantı noktası yapılandırmaları ve sonuçta elde edilen Python imza tablosu](./media/execute-python-scripts/python-script-inputs-mapped-to-parameters.png)

### <a name="output-return-values"></a>Çıkış dönüş değerleri

`azureml_main` İşlevi, bir Python paketlenmiş tek bir Pandas DataFrame döndürmelidir [dizisi](https://docs.python.org/2/c-api/sequence.html) gibi bir dizi, liste veya NumPy dizisi. Bu dizinin ilk öğesi modülünün ilk çıkış bağlantı noktasına döndürülür. İkinci modülün çıkış bağlantı için kullanılan [görselleştirmeler](#visualizations) ve dönüş değeri gerektirmez. Bu düzen, aşağıda gösterilmiştir.

![Eşleme parametreleri için bağlantı noktalarını giriş ve çıkış bağlantı noktasına dönüş değeri](./media/execute-python-scripts/map-of-python-script-inputs-outputs.png)

## <a name="translation-of-input-and-output-data-types"></a>Giriş ve çıkış veri türlerinin çeviri

Studio veri kümeleri Panda DataFrames aynı değildir. Sonuç olarak, giriş veri kümeleri Studio'da Pandas DataFrame için dönüştürülür ve DataFrames çıkış Studio veri kümeleri dönüştürülür. Bu dönüştürme işlemi sırasında aşağıdaki çevirileri da gerçekleştirilir:

 **Python veri türü** | **Studio çeviri yordamı** |
| --- | --- |
| Dizeler ve sayısal türler| Olarak çevrilir. |
| Pandas 'NA' | 'Eksik değer' çevrilmiş |
| Dizin vektörleri | Desteklenmeyen * |
| Dize olmayan sütun adları | Çağrı `str` sütun adları |
| Yinelenen sütun adları | Sayısal bir sonek olarak ekleyin: (1), (2), (3) ve benzeri.

**Python işlevindeki tüm giriş veri çerçevelerini her zaman bir 64-bit sayısal dizin 0'dan eksi 1 satır numarasına sahip*

## <a id="import-modules"></a>Mevcut Python betik modüllerini içeri aktarma

Python yürütmek için kullanılan arka uç dayanır [Anaconda](https://store.continuum.io/cshop/anaconda/), bilimsel Python dağıtım yaygın olarak kullanılan. Bu, veri merkezli iş yükleri içinde kullanılan en yaygın Python paketlerini 200 yakın birlikte gelir. Studio yüklemek ve dış kitaplıkları yönetmek için Pip ya da Conda gibi paket Yönetimi sistemlerine kullanımı şu anda desteklemiyor.  Ek kitaplıklar eklemenize gerek fark ederseniz, aşağıdaki senaryoyu bir kılavuz olarak kullanın.

Studio denemeleri mevcut Python betiklerini birleştirmek için yaygın bir kullanım örneği var. [Python betiği yürütme] [ execute-python-script] modülü Python modüllerini üçüncü Giriş noktasındaki içeren zip dosyasını kabul eder. Dosyanın çalışma zamanında yürütme framework tarafından sıkıştırması açılan ve içeriği Python yorumlayıcısı kitaplık yolunu eklenir. `azureml_main` İşlevi daha sonra içeri aktarabilir bu modülleri doğrudan giriş noktası. 

Örneğin, dosyayı içeren basit bir "Hello, World" işlevi Hello.py göz önünde bulundurun.

![Kullanıcı tanımlı işlev Hello.py dosyasında](./media/execute-python-scripts/figure4.png)

Ardından, Hello.zip Hello.py içeren bir dosya oluşturun:

![Kullanıcı tanımlı Python kodu içeren zip dosyası](./media/execute-python-scripts/figure5.png)

Zip dosyası Studio uygulamasına bir veri kümesi olarak karşıya yükleyin. Ardından, oluşturun ve üçüncü giriş bağlantı noktasına ekleyerek Hello.zip dosyasında Python kodu kullanan bir deneme çalıştırma **Python betiği yürütme** aşağıdaki görüntüde gösterildiği gibi modülü.

![Örnek deneme bir Python betiği yürütme modülü için giriş olarak Hello.zip ile](./media/execute-python-scripts/figure6a.png)

![Kullanıcı tanımlı Python kodu zip dosyası olarak karşıya yüklendi](./media/execute-python-scripts/figure6b.png)

Modül çıktı zip dosyası paketlenmemiş olduğunu gösterir ve işlev `print_hello` çalıştırıldıktan.

![Kullanıcı tanımlı işlev gösteren modülü çıkışı](./media/execute-python-scripts/figure7.png)

## <a name="accessing-azure-storage-blobs"></a>Azure depolama Blobları erişme

Aşağıdaki adımları kullanarak bir Azure Blob Depolama hesabında depolanan verilere erişebilirsiniz:

1. İndirme [Python için Azure Blob Depolama paketi](https://azuremlpackagesupport.blob.core.windows.net/python/azure.zip) yerel olarak.
1. Zip dosyası Studio çalışma alanınıza bir veri kümesi olarak karşıya yükleyin.
1. BlobService nesnenizle oluşturma `protocol='http'`

```
from azure.storage.blob import BlockBlobService

# Create the BlockBlockService that is used to call the Blob service for the storage account
block_blob_service = BlockBlobService(account_name='account_name', account_key='account_key', protocol='http')
```

1. Devre dışı **güvenli aktarım gerekli** depolama alanınızda **yapılandırma** ayarı sekmesi

![Güvenli aktarım gerekli Azure portalında devre dışı bırak](./media/execute-python-scripts/disable-secure-transfer-required.png)

## <a name="operationalizing-python-scripts"></a>Python betiklerini faaliyete geçirmeye yönelik

Tüm [Python betiği yürütme] [ execute-python-script] bir web hizmeti olarak yayımlandığında bir Puanlama denemesine kullanılan modülleri çağrılır. Örneğin, aşağıdaki görüntüde, tek bir Python ifade değerlendirilemiyor kodu içeren bir Puanlama deneme gösterilmektedir.

![Studio çalışma alanı için bir web hizmeti](./media/execute-python-scripts/figure3a.png)

![Python Pandas ifadesi](./media/execute-python-scripts/python-script-with-python-pandas.png)

Bu bir deneyden oluşturulan bir web hizmeti, aşağıdaki eylemleri gerçekleştirmeniz:

1. Python ifadesini almak (bir dize olarak) giriş
1. Python yorumlayıcısı Python ifade Gönder
1. ifade hem değerlendirilen sonucunu içeren bir tablo döndürür.

## <a id="visualizations"></a>Görselleştirmeleri ile çalışma

MatplotLib kullanılarak oluşturulan çizimler döndürülen [Python betiği yürütme][execute-python-script]. R kullanırken olduğu gibi ancak çizimleri otomatik olarak görüntüleri yeniden yönlendirilen değil Bu nedenle kullanıcı PNG dosyaları açıkça tüm çizimleri kaydetmelisiniz.

MatplotLib görüntüleri oluşturmak için aşağıdaki adımları izlemelisiniz:

1. Arka uç "AGG için" varsayılan Qt tabanlı işleyicisi bağlantısı geçin.
1. Yeni bir şekil nesnesi oluşturun.
1. Eksen alın ve tüm çizimleri içine oluşturun.
1. Şekil bir PNG dosyasına kaydedin.

Bu işlem, aşağıdaki resimlerde içinde Pandas scatter_matrix işlevini kullanarak bir dağılım çizim matrisi oluşturmak gösterilmiştir.

![Görüntüleri MatplotLib rakamları kaydetmek için kod](./media/execute-python-scripts/figure-v1-8.png)

![Click rakamları görüntülemek için bir Python betiği yürütme modülü görselleştirin](./media/execute-python-scripts/figure-v2-9a.png)

![Python kodu kullanarak bir örnek deneme için çizimleri Görselleştirme](./media/execute-python-scripts/figure-v2-9b.png)

Farklı görüntülere kaydederek birden çok şekil dönmek mümkündür. Studio çalışma zamanı, tüm görüntülerini seçer ve görselleştirme için art arda ekler.

## <a name="advanced-examples"></a>Gelişmiş örnekleri

Studio'da yüklenmiş Anaconda ortam NumPy, SciPy ve Scikits öğrenin gibi yaygın paketleri içerir. Bu paketleri, bir makine öğrenimi işlem hattı ile veri işleme için etkili bir şekilde kullanılabilir.

Örneğin, aşağıdaki denemenin ve betik topluluğu öğrencileriyle Scikits-bir veri kümesi için özellik önem puanlarını hesaplama öğrenin, kullanımını gösterir. Puanları, önce başka bir modele beslenir denetimli özellik seçimi gerçekleştirmek için kullanılabilir.

Önem derecesi puanları ve özellikleri puanları temel alarak sırası hesaplamak için kullanılan Python işlevi şu şekildedir:

![Sıra özellikleri tarafından puanları işlevi](./media/execute-python-scripts/figure8.png)

Aşağıdaki denemenin hesaplar ve Azure Machine Learning Studio'da "Pima Hint Ailelere" veri kümesindeki özelliklerinin önem puanlarını döndürür:

![Python kullanarak Pima Hint Ailelere kümesindeki derece özellikleri deneme](./media/execute-python-scripts/figure9a.png)

![Python betiği yürütme modülü çıkışı Görselleştirme](./media/execute-python-scripts/figure9b.png)

## <a name="limitations"></a>Sınırlamalar

[Python betiği yürütme] [ execute-python-script] modülü şu anda aşağıdaki sınırlamalara sahiptir:

### <a name="sandboxed-execution"></a>Korumalı yürütme

Python çalışma zamanını şu anda korumalı ve kalıcı bir biçimde ağ veya yerel dosya sistemine erişim izin vermez. Yerel olarak kaydedilmiş tüm dosyalar yalıtılmış ve modülü tamamladıktan sonra silinir. Python kodu geçerli dizin ve alt dizinlerinde olan özel durum üzerinde çalıştığı makinede çoğu dizinlere erişemez.

### <a name="lack-of-sophisticated-development-and-debugging-support"></a>Gelişmiş geliştirme ve hata ayıklama desteği

Python modülü, IntelliSense ve hata ayıklama gibi IDE özellikleri şu anda desteklemiyor. Ayrıca, çalışma zamanında modülü başarısız olursa, tam Python yığın izlemesi kullanılabilir. Ancak, modül için çıktı günlüğüne görüntülenmelidir. Şu anda, geliştirme ve Ipython gibi bir ortamda Python betiklerinde hata ayıklamak ve ardından kod modülüne içe aktarın öneririz.

### <a name="single-data-frame-output"></a>Tek bir veri çerçevesi çıkış

Python giriş noktası, yalnızca bir tek veri çerçevesine çıktı olarak verilecek izin verilir. Geri Studio çalışma zamanı rastgele Python nesneleri doğrudan eğitilen modelleri gibi döndürmek şu anda mümkün değildir. Gibi [R betiği yürütme][execute-r-script]aynısına sahip, çoğu durumda, nesneleri bir bayt dizisine okunamayacak pickle ve ardından, bir veri çerçevesi içinde dönmek mümkündür.

### <a name="inability-to-customize-python-installation"></a>Şubelerde Python yüklemeyi özelleştirmek için

Şu anda, özel bir Python modüllerini eklemek için yalnızca daha önce açıklanan zip dosyası mekanizması yoludur. Küçük modüller için uygun olsa da büyük modülleri (özellikle yerel DLL'leri modülleriyle) veya modülleri çok sayıda yavaşlatan bir yöntemdir.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için bkz. [Python Geliştirici Merkezi](https://azure.microsoft.com/develop/python/).

<!-- Module References -->
[execute-python-script]: https://docs.microsoft.com/azure/machine-learning/studio-module-reference/execute-python-script
[execute-r-script]: https://docs.microsoft.com/azure/machine-learning/studio-module-reference/execute-r-script