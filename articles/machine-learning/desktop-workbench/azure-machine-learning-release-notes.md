---
title: Azure Machine Learning'deki yenilikler
description: Bu belge Azure Machine Learning güncelleştirmeleri ayrıntıları.
services: machine-learning
author: hning86
ms.author: haining
manager: mwinkle
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.topic: reference
ms.date: 03/28/2018
ms.openlocfilehash: 9c135c1db557393a26e6e071f92b61fcbf3ae477
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2018
ms.locfileid: "35647822"
---
# <a name="whats-new-in-azure-machine-learning"></a>Azure Machine Learning’deki Yenilikler

Bu makalede, yeni sürümleri hakkında bilgi edinin [Azure Machine Learning](../service/overview-what-is-azure-ml.md). 


## <a name="2018-05-sprint-5"></a>2018-05 (sprint 5)

Azure Machine Learning bu sürümle birlikte, şunları yapabilirsiniz:
+ Özellik kazandırın görüntüleri ResNet 50 quantized bir sürümü ile bu özellikler, temel bir sınıflandırıcı eğitmek ve [modelin bir FPGA azure'da dağıtmak](../service/how-to-deploy-fpga-web-service.md) Ultra düşük gecikme süresi çıkarım için.

+ Hızla oluşturmak ve yüksek doğruluk oranına makine öğrenimi ve derin öğrenme modelleri kullanarak dağıtmak [özel Azure Machine Learning paketleri](../service/reference-python-package-overview.md) aşağıdaki etki alanları için:
  + [Görüntü işleme](../service/how-to-build-deploy-image-classification-models.md)
  + [Metin analizi](../service/how-to-build-deploy-text-classification-models.md)
  + [Tahmin etme](../service/how-to-build-deploy-forecast-models.md)

## <a name="2018-03-sprint-4"></a>2018-03 (sprint 4)
**Sürüm numarası**: 0.1.1801.24353 &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;([sürümünüzü bulmak](../service/known-issues-and-troubleshooting-guide.md#find-the-workbench-build-number))


Aşağıdaki güncelleştirmelerin çoğu, doğrudan geri bildirim sonuçlarını yapılır. Lütfen bunları devam ettirmeye kararlıyız!

**Önemli yeni özellikler ve değişiklikleri**

- Temelli yürütme komut dosyalarınızı uzak Ubuntu vm'lerinde uzak docker yanı sıra kendi ortamınızı üzerinde yerel olarak çalıştırma desteği.
- Yeni ortam deneyimi Workbench uygulamasında işlem hedeflerini oluşturmak ve CLI tabanlı deneyimimizi ek yapılandırmaları Çalıştır sağlar.
![Ortamlar sekmesi](media/azure-machine-learning-release-notes/environment-page.png)
- Özelleştirilebilir çalıştırma geçmişi raporlarını ![yeni geçmiş raporları çalıştırma görüntüsü](media/azure-machine-learning-release-notes/new-run-history-reports.png)

**Ayrıntılı güncelleştirmeleri**

Her bileşen bölümünde Azure Machine Learning'de bu sprint ayrıntılı güncelleştirmeleri listesi aşağıda verilmiştir.

### <a name="workbench-ui"></a>Workbench kullanıcı Arabirimi
- Özelleştirilebilir çalıştırma geçmişi raporları
  - Çalıştırma geçmişi raporlar için geliştirilmiş grafik yapılandırması
    - Kullanılan giriş noktaları değiştirilebilir
    - Üst düzey filtre eklenebilir ve değiştiren ![filtreleri ekleyin](media/azure-machine-learning-release-notes/add-filters.jpg)
    - Grafikler ve istatistikleri eklenecek veya değiştirilecek (ve düzenlenmeyecek sürükle ve bırak).
    ![Yeni grafik oluşturma](media/azure-machine-learning-release-notes/configure-charts.png)

  - CRUD çalıştırma geçmişi raporları
  - İşlem hatları gibi hareket eden yapılandırmaları sunucu tarafı raporlara, seçilen giriş noktalarını çalıştıran var olan tüm çalıştırma geçmişi liste görünümü taşındı.

- Ortamlar sekmesi
  - Kolayca yeni işlem hedefini ekleme ve yapılandırma dosyaları projenize ![yeni işlem hedefi](media/azure-machine-learning-release-notes/add-new-environments.png)
  - Yönetme ve yapılandırma dosyalarınızın basit, form tabanlı bir UX kullanarak güncelleştirme
  - Yeni düğme yürütme için ortamınızı hazırlama

- Kenar çubuğunda dosyalarının listesi için performans geliştirmeleri

### <a name="data-preparation"></a>Veri hazırlama 
- Azure Machine Learning Workbench, bilinen bir sütunun adını kullanarak bir sütun için arama yapabilmesi olanak sağlıyor.


### <a name="experimentation"></a>Deneme
- Azure Machine Learning Workbench, betiklerinizi yerel olarak kendi python veya pyspark'tan ortamında çalışan artık desteklemektedir. Bu özellik için kullanıcı oluşturur ve kendi uzak sanal ortamda yönetir ve bu hedefte betiklerini yürütmek için Azure Machine Learning Workbench'i kullanma. Lütfen bkz [Azure Machine Learning deneme hizmeti yapılandırma](experimentation-service-configuration.md) 

### <a name="model-management"></a>Model Yönetimi
- Dağıtılmış kapsayıcıları özelleştirmek için destek: apt-get- vb. kullanarak dış kitaplıkları yüklenmesini sağlayarak kapsayıcı görüntüsünü özelleştirme sağlar. Artık, kolayca yüklenebilir kitaplıklara sınırlı değildir. Bkz: [belgeleri](model-management-custom-container.md) daha fazla bilgi için.
  - Kullanım `--docker-file myDockerStepsFilename` bildirimi, görüntü veya hizmet oluşturma komutları bayrağı ve dosya adı.
  - Temel görüntü Ubuntu ve değiştirilemez olduğunu unutmayın.
  - Örnek komut: 
  
      ```shell
      $ az ml image create -n myimage -m mymodel.pkl -f score.py --docker-file mydockerstepsfile
      ```



## <a name="2018-01-sprint-3"></a>2018-01 (sprint 3) 
**Sürüm numarası**: 0.1.1712.18263 &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;([sürümünüzü bulmak](../service/known-issues-and-troubleshooting-guide.md#find-the-workbench-build-number))

Bu sprint'te geliştirmeleri ve güncelleştirmeler şunlardır: Bu güncelleştirmelerin çoğu kullanıcı geri bildirim doğrudan sonucu yapılır. 


Her bileşen bölümünde Azure Machine Learning'de bu sprint ayrıntılı güncelleştirmeleri listesi aşağıda verilmiştir.

- Güncelleştirmeleri kimlik doğrulaması yığınına zorlar başlangıçta oturum açma ve hesap seçimi

### <a name="workbench"></a>Workbench
- Program Ekle/Kaldır'ndan uygulamanın yükleme/kaldırma yeteneği
- Güncelleştirmeleri kimlik doğrulaması yığınına zorlar başlangıç oturum açma ve hesap seçimi
- Windows üzerinde geliştirilmiş çoklu oturum açma (SSO) deneyimi
- Farklı kimlik bilgileri ile birden çok kiracıya ait olan kullanıcıların artık Workbench oturum açabilir olacaktır

### <a name="ui"></a>KULLANICI ARABİRİMİ
- Genel iyileştirmeler ve hata düzeltmeleri

### <a name="notebooks"></a>Not Defterleri
- Genel iyileştirmeler ve hata düzeltmeleri

### <a name="data-preparation"></a>Veri hazırlama 
- Örnek tarafından dönüştürmeler gerçekleştirilirken geliştirilmiş otomatik-önerileri
- Gelişmiş algoritma için desen sıklığı denetçisi
- Örnek tarafından dönüştürmeler gerçekleştirilirken örnek veriler ve geri bildirim gönderme özelliğine ![türetilen sütun dönüştürme hakkında geri bildirim bağlantısı Gönder görüntüsü](media/azure-machine-learning-release-notes/SendFeedbackFromDeriveColumn.png)
- Spark çalışma zamanı geliştirmeleri
- Scala Pyspark değiştirilmiştir
- Zaman serisi denetleyici için veri uygulanamaz kapatmak için sabit bağlantı kurma sorunu 
- Sabit veri hazırlığı HDI yürütülmek için yanıt vermemesine saat

### <a name="model-management-cli-updates"></a>Model Yönetimi CLI güncelleştirmeleri 
  - Aboneliğin sahipliğini, artık kaynak sağlamak için gereklidir. Katkıda bulunan erişimi kaynak grubuna dağıtım ortamı ayarlamak yeterli olacaktır.
  - Etkin yerel ortamı Kurulumu ücretsiz abonelikler 

## <a name="2017-12-sprint-2-qfe"></a>2017-12 (sprint 2 QFE) 
**Sürüm numarası**: 0.1.1711.15323 &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;([sürümünüzü bulmak](../service/known-issues-and-troubleshooting-guide.md#find-the-workbench-build-number))

Alt sürüm QFE (hızlı düzeltme Mühendisliği) yayın budur. Birkaç telemetri sorunları ele alır ve ürün ekibine ürün nasıl kullanılmakta olduğunu daha iyi anlamak için yardımcı olur. Bilgi Bankası ürün deneyimini geliştirmek, gelecekteki çalışmalarını uygulamasına gidebilirsiniz. 

Ayrıca, iki önemli güncelleştirme vardır:

- Zaman serisi denetçisi veri hazırlama paketinde görüntülenmesini engelleyen veri hazırlığı, bir hata düzeltildi.
- Komut satırı aracı artık Machine Learning işlem ACS kümeleri sağlamak için bir Azure aboneliğine sahip olmanız gerekir. 

## <a name="2017-12-sprint-2"></a>2017-12 (sprint 2)
**Sürüm numarası**: 0.1.1711.15263 &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;([sürümünüzü bulmak](../service/known-issues-and-troubleshooting-guide.md#find-the-workbench-build-number))

Azure Machine Learning üçüncü güncelleştirmeye Hoş Geldiniz. Bu güncelleştirme, workbench uygulaması, komut satırı arabirimi (CLI) ve arka uç Hizmetleri geliştirmeleri içerir. Gülümsemeleri göndermek için çok teşekkür ederiz ve frowns. Aşağıdaki güncelleştirmelerin çoğu, doğrudan geri bildirim sonuçlarını yapılır. 

**Önemli yeni özellikler**
- [SQL Server ve Azure SQL DB veri kaynağı olarak desteği](data-prep-appendix2-supported-data-sources.md#types) 
- [MMLSpark kullanma GPU desteğiyle Spark üzerinde derin öğrenme](https://github.com/Azure/mmlspark/blob/master/docs/gpu-setup.md)
- [Tüm AML kapsayıcıları (ek bir adım gerekli değildir) dağıtılan Azure IOT Edge cihazları ile uyumludur](http://aka.ms/aml-iot-edge-blog)
- Kayıtlı modeli listesi ve ayrıntılı görünümlerini kullanılabilir Azure portalı
- Kullanıcı adı/parola tabanlı erişim yanı sıra SSH anahtar tabanlı kimlik doğrulaması'nı kullanarak işlem hedeflerini erişme. 
- Yeni desen sıklığı denetçi'deki verileri deneyimi hazırlayın. 

**Güncelleştirmeleri ayrıntılı** her bileşen bölümünde Azure Machine Learning'de bu sprint ayrıntılı güncelleştirmeleri listesi aşağıda verilmiştir.

### <a name="installer"></a>Yükleyici
- Yükleyici, kendi kendini güncelleştirme, hata düzeltmeleri ve yeni özellikleri, kullanıcı yeniden yüklemek zorunda desteklenebilir olabilir

### <a name="workbench-authentication"></a>Workbench kimlik doğrulaması
- Birden çok kimlik doğrulama sistemi düzeltir. Lütfen oturum açma sorunları yaşamaya varsa bize bildirin.
- Daha kolay hale getirmek UI değişiklikleri Proxy Manager ayarları bulunamıyor.

### <a name="workbench"></a>Workbench
- Salt okunur dosya görünümü artık açık mavi arka plana sahip
- Taşınan Düzenle düğmesine sağ daha bulunabilir olmasını sağlayın.
- ham metin biçiminde, "dsource", "dprep" ve "ipynb" dosya biçimleri artık işlenebilecek
- Workbench, betikleri düzenlemek ve yalnızca (örneğin, dizüstü bilgisayarlar, veri kaynakları, veri hazırlık paketlerini) bir zengin düzenleme deneyimi olan dosya türlerini düzenlemek için Workbench'i kullanabilmeniz için dış IDE kullanarak doğru kullanıcılara yol gösteren yeni bir düzenleme deneyimi artık sahiptir.
- Çalışma alanları ve kullanıcının erişimi olan projeler listesi yüklenirken artık önemli ölçüde daha hızlıdır

### <a name="data-preparation"></a>Veri hazırlama 
- Bir desen sıklığı sütundaki dize desenlerini görüntülemek için denetçisi. Ayrıca, bu desenleri kullanarak verilerinize de filtreleyebilirsiniz. Bu, bir görünüm benzer değer sayıları Inspector'ı gösterir. Desen sıklığını benzersiz veri sayısı yerine verilerin benzersiz desenleri sayılarını gösterir farktır. Ayrıca, içinde veya belirli bir düzene sığıp tüm satırları filtreleyebilirsiniz.

![Ürün sayısı deseni sıklığı denetçisinin görüntüsü](media/azure-machine-learning-release-notes/pattern-inspector-product-number.png)

- 'Sütunu örneğe göre türet' dönüşümünde gözden geçirmek için istisnai durumlara öneren sırasında performans geliştirmeleri

- [SQL Server ve Azure SQL DB veri kaynağı olarak desteği](data-prep-appendix2-supported-data-sources.md#types) 

![Yeni bir SQL server veri kaynağı oluşturma görüntüsü](media/azure-machine-learning-release-notes/sql-server-data-source.png)

- "Bir bakışta" satır ve sütun sayılarını görünümünü etkin

![Bir yetkisiz kullanım sırasında satır sütun sayısı görüntüsü](media/azure-machine-learning-release-notes/row-col-count.png)

- Veri hazırlığı işlem kısayolun her bağlamda etkin
- SQL Server veritabanı veri kaynakları tüm işlem bağlamlarda etkin
- Kılavuz sütunları veri türüne göre filtrelenebilir veri hazırlama
- Birden çok sütun için tarih dönüştürülmesiyle sorun düzeltildi
- Gelişmiş mod çıktı sütunu adına kullanıcı değiştirdiyseniz, sorun düzeltildi kullanıcı çıktı sütunu bir sütun örneği tarafından türetilen kaynağı olarak seçebilirsiniz.

### <a name="job-execution"></a>İş yürütme
Şimdi, oluşturma ve SSH anahtar tabanlı kimlik doğrulaması aşağıdaki adımları kullanarak remotedocker veya küme türü işlem hedefi erişebilirsiniz:
- CLI aşağıdaki komutu kullanarak bir işlem hedefine ekleyin

    ```azure-cli
    $ az ml computetarget attach remotedocker --name "remotevm" --address "remotevm_IP_address" --username "sshuser" --use-azureml-ssh-key
    ```
>[!NOTE]
>-k (veya--kullanın-azureml-ssh-key) seçeneğinde komut oluşturma ve SSH anahtarı kullanmak için belirtir.

- Azure ML Workbench, ortak anahtar oluşturma ve Konsolunuzda, çıktı. İşlem hedef aynı kullanıcı adını kullanarak oturum açın ve bu ortak anahtarla ~/.ssh/authorized_keys dosyası ekleyin.

- Bu işlem hedefi hazırla ve yürütme için kullanın ve Azure ML Workbench, kimlik doğrulaması için bu anahtarı kullanır.  

İşlem hedefleri oluşturma hakkında daha fazla bilgi için bkz. [Azure Machine Learning deneme hizmeti yapılandırma](experimentation-service-configuration.md)

### <a name="visual-studio-tools-for-ai"></a>AI için Visual Studio Araçları
- İçin destek eklendi [yapay ZEKA için Visual Studio Araçları](https://marketplace.visualstudio.com/items?itemName=ms-toolsai.vstoolsai-vs2017). 

### <a name="command-line-interface-cli"></a>Komut Satırı Arabirimi (CLI)
- Eklenen `az ml datasource create` komut, komut satırından bir veri kaynağı dosyası oluşturmaya olanak sağlar

### <a name="model-management-and-operationalization"></a>Model yönetimi ve kullanıma hazır hale getirme
- [Tüm AML kapsayıcıları (ek bir adım gerekli değildir) kullanıma hazır hale getirdiniz Azure IOT Edge cihazlarında ile uyumludur](http://aka.ms/aml-iot-edge-blog) 
- Hata iletilerinin o16n CLI geliştirmeleri
- Model yönetim portalındaki UX hata düzeltmeleri  
- Model Yönetimi öznitelikler ayrıntı sayfası için tutarlı harfi büyük/küçük harf
- Gerçek zamanlı Puanlama çağrıları zaman aşımını 60 saniye olarak belirlendi
- Kayıtlı model listesi ve ayrıntılı görünümlerini Azure portalında kullanılabilir

![portalında model ayrıntıları](media/azure-machine-learning-release-notes/model-list.jpg)

![portalında modeline genel bakış](media/azure-machine-learning-release-notes/model-overview-portal.jpg)

### <a name="mmlspark"></a>MMLSpark
- Derin öğrenme ile spark'ta [GPU desteği](https://github.com/Azure/mmlspark/blob/master/docs/gpu-setup.md)
- Resource Manager şablonlarını kolay kaynak dağıtımının desteği
- SparklyR ekosistemi için destek
- [AZTK tümleştirme](https://github.com/Azure/aztk/wiki/Spark-on-Azure-for-Python-Users#optional-set-up-mmlspark)

### <a name="sample-projects"></a>Örnek Proje
- [Iris](https://github.com/Azure/MachineLearningSamples-Iris) ve [MMLSpark](https://github.com/Azure/mmlspark) örnekleri yeni Azure ML SDK'sı sürüm ile güncelleştirildi

### <a name="breaking-changes"></a>Hataya neden olan değişiklikler
- Yükseltilen `--type` anahtarının `az ml computetarget attach` için bir alt. 

    - `az ml computetarget attach --type remotedocker` Şimdi `az ml computetarget attach remotedocker`
    - `az ml computetarget attach --type cluster` Şimdi `az ml computetarget attach cluster`

## <a name="2017-11-sprint-1"></a>2017-11 (sprint 1) 
**Sürüm numarası**: 0.1.1710.31013 &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;([sürümünüzü bulmak](../service/known-issues-and-troubleshooting-guide.md#find-the-workbench-build-number))

Bu sürümde, güvenlik, kararlılık ve sürdürülebilirliği workbench uygulaması, CLI ve arka uç Hizmetleri katmanı geliştirmeler yaptık. Ve frowns çok gülümsemeler gönderdiğiniz için teşekkür ederiz. Çoğu güncelleştirmeleri doğrudan geri bildirim sonuçlarını yapılır. Bunları devam ettirmeye kararlıyız!

### <a name="notable-new-features"></a>Önemli yeni özellikler
- Azure ML, artık iki yeni Azure bölgelerinde: **Batı Avrupa** ve **Güneydoğu Asya**. Önceki bölgeleri katılmaları **Doğu ABD 2**, **Batı Orta ABD**, ve **Avustralya Doğu**, toplam sayısı getirme dağıtılan bölgeleri için beş.
- Okuma ve düzenleme Python kaynak kodu daha kolay hale getirmek için Workbench uygulamasında Python kodu, sözdizimi vurgulamalı etkinleştirdik. 
- Şimdi, doğrudan bir dosyadan yerine tüm projeye en sevdiğiniz IDE de başlatabilirsiniz.  Workbench'te bir dosyanın açılmasını ve "Düzenle"'yi tıklatarak IDE'nizi (şu anda VS Code ve PyCharm desteklenir) geçerli dosya ve proje başlatır.  Ayrıca Workbench metin düzenleyicide dosyayı düzenlemek için Düzenle düğmesini yanındaki oka tıklayabilirsiniz.  Düzenleme, yanlışlıkla yapılan değişiklikleri engelleyerek tıklayana kadar dosyaları salt okunurdur.
- Popüler çizim kitaplığını `matplotlib` sürüm 2.1.0 artık Workbench uygulaması ile birlikte gönderilir.
- Biz, .NET Core sürümünün 2.0 veri hazırlığı altyapısı için yükseltildi. Bu yükleme brew openssl gereksinimini macOS üzerinde uygulama yüklemesi sırasında kaldırıldı. Bu ayrıca daha heyecan verici veriler için hazırlık özellikleri yakın gelecekte gelen niteliğindedir. 
- Daha fazla ilgili sürüm notlarını edinin ve güncelleştirme istemleri, geçerli uygulama sürümü temel bir sürüme özgü uygulama giriş etkinleştirdik.
- Şimdi yerel kullanıcı adında boşluk varsa, bu uygulamayı başarılı bir şekilde yüklenebilir. 

### <a name="detailed-updates"></a>Ayrıntılı güncelleştirmeleri
Her bileşen bölümünde Azure Machine Learning'de bu sprint ayrıntılı güncelleştirmeleri listesi aşağıdadır.

#### <a name="installer"></a>Yükleyici
- Uygulama yükleyici artık uygulama eski bir sürümü tarafından oluşturulmuş yükleme dizini temizler.
- MacOS High Sierra % 100 takılı yükleyici müşteri adaylarını bir hatayı düzelttik.
- Artık kullanıcı yüklemesi başarısız olursa, yükleyici günlüklerini gözden geçirmek için yükleyici dizin doğrudan bağlantı yoktur.
- Artık kullanıcı adında boşluk olan kullanıcılar için çalışır yükleyin.

#### <a name="workbench-authentication"></a>Workbench kimlik doğrulaması
- Ara Sunucu Yöneticisi'nde kimlik doğrulaması için destek.
- Kullanıcı bir güvenlik duvarının arkasındaysa, artık oturum açma başarılı olur. 
- Kullanıcının deneme hesapları birden çok Azure bölgesinde olup olmadığını ve tek bir bölge kullanılamaz durumda uygulamaya artık askıda kalır.
- Kimlik doğrulaması tamamlanmadı ve kimlik doğrulama iletişim kutusuna hala görünür olduğunda, uygulama artık yerel önbellekten çalışma yüklemeye çalışır.

#### <a name="workbench-app"></a>Workbench uygulaması
- Python kodu söz dizimi vurgulama, metin düzenleyici ile etkinleştirilir.
- Düzenle düğmesi metin düzenleyicisinde, dosyayı düzenlemek için ya da bir IDE sağlar (VS Code ve PyCharm desteklenir) veya yerleşik bir metin düzenleyicisinde.
- Metin düzenleyici, varsayılan olarak salt okunur modda olduğundan. 
- Geçerli dosya kaydedilmiş ve bu nedenle artık geçersiz olduktan sonra düğme görsel durum şimdi değişiklikleri devre dışı olarak kaydedin.
- Workbench kaydeder _tüm_ bir Farklı Çalıştır'ı başlattığınızda dosyaları kaydedilmemiş.
- Workbench otomatik olarak açılır son çalışma yerel makinede kullanılan hatırlar.
- Workbench yalnızca tek bir örneği çalıştırmak için artık verilir. Daha önce birden çok örneği aynı proje üzerinde çalışırken sorunları neden başlatılamıyor.
- Yeniden adlandırılan Dosya menüsü "Açık proje..." "Ekle varolan klasör olarak proje..." 
- Sekme geçişi artık çok daha hızlıdır.
- Yardım bağlantıları IDE yapılandırma iletişim kutusuna eklenir.
- Geri bildirim formu artık son kez girdiğiniz e-posta adresi hatırlar.
- Bize daha fazla geri bildirim gönderebilmek için gülümsemeler ve frowns form metin alanı artık büyük,! 
- `--owner` Geçiş Yardım metni `az ml workspace create` düzeltilir.
- Kolayca görüntüleyin ve uygulamanın sürüm numarasını kopyalamayı kullanıcının yardımcı olmak için bir "Hakkında" iletişim kutusu ekledik.
- Yardım menüsüne "özellik önerin" menü öğesi eklendi.
- Deneme hesabı adı uygulama başlık çubuğu, uygulama adı "Azure Machine Learning Workbench" önceki görünür.
- Algılanan uygulama sürümünde artık temel bir sürüme özgü uygulama giriş sayfası görüntülenir.

#### <a name="data-preparation"></a>Veri hazırlama 
- Dış web sitesi artık olası güvenlik sorunlarını önlemek için harita denetçisini ' yüklenebilir.
- Histogram ve değer sayısı denetçiler Logaritmik ölçek grafını Göster seçeneği sunuyor.
- Veri Kalitesi çubuğu ne zaman bir hesaplama devam ediyor, şimdi "hesaplama" durumunu göstermek için farklı bir renkle gösterilir.
- Sütun ölçümlerini kategorik değer sütunlar için istatistik gösterilir.
- Veri kaynağı adı son karakter artık kesilir.
- Veri hazırlama paketi artık gözle görülür bir performans kazancı elde edilen sekmeler arasında geçiş yaparken açık kalır.
- Veri kaynağındaki veri görünümü ve ölçüm görünümü arasında geçiş yaparken, artık artık sütunların sırasını değiştirir.
- Geçersiz bir açma `.dprep` veya `.dsource` dosya artık Workbench çökmesine neden.
- Veri hazırlama paketi artık kullanabilir göreli yol çıkış için _CSV'ye yazma_ dönüştürün.
- _Sütunu tut_ dönüştürme düzenlendiğinde ek sütunlar eklemek kullanıcının artık izin verir.
- _Bunu değiştirin_ menüsü artık gerçekten başlatır _değiştirme değeri_ iletişim kutusu.
- _Değeri Değiştir_ hata oluşturma yerine beklendiği gibi işlevleri artık dönüştürün.
- Veri hazırlama paketi, artık veri dosyalarına veri dosyasına paketin mutlak yolu ile yerel bağlamda çalıştırmanızı edinerek proje klasörünün dışında başvururken mutlak yolu kullanır.
- _Tam dosya_ örnekleme stratejisi artık Azure kullanırken desteklenmediğinden blob veri kaynağı olarak.
- Python koddan (veri hazırlama paketi) oluşturulan CR hem Windows kolay hale LF, artık taşır.
- _Ölçümleri Seç_ açılan veri görünümüne geçiş yaparken özelliği artık gizler.
- Python çalışma zamanında bile kullanırken, işlem parquet dosyalarını artık workbench geçirebilirsiniz. Daha önce yalnızca Spark parquet dosyalarını işlenirken kullanılabilir. 
- İle bir sütundaki değerleri filtreleme _tarih_ veri türü artık veri hazırlık altyapısı kilitlenmesine neden olur.
- Ölçümü görünümünü örnekleme stratejisi güncelleştirmeleri artık dikkate alır.
- Uzak işler artık örnekleme düzgün çalışır.

#### <a name="job-execution"></a>İş yürütme
- Bağımsız değişken artık çalıştırma geçmişi kaydına dahil edilir.
- İşleri de başlatıldı CLI artık çalıştırma geçmişi iş panelinde otomatik olarak gösterilir.
- Proje paneli, artık Azure AD kiracısına eklenen Konuk kullanıcılar tarafından oluşturulan işleri gösterir.
- İş panelini iptal edin ve Sil eylemlerinin daha kararlı.
- Yapılandırma dosyalarını hatalı biçimindeyse Çalıştır düğmesine tıklandığında, hata iletisi artık tetiklenir.
- Sondaki uygulama artık CLI'daki başlatıldı işleriyle uğratır.
- İçinde başlatıldı işleri CLI artık standart çıkış bölme konumu bile bir saat sonra yürütme devam eder.
- Python/PySpark veri hazırlama paketini çalıştırma başarısız olduğunda daha iyi hata iletisi gösterilir.
- `az ml experiment clean` Uzak sanal Docker görüntüleri artık temizler.
- `az ml experiment clean` artık düzgün şekilde için yerel hedef macOS üzerinde çalışır.
- Yerel veya uzak bir Docker'ı hedefleyen çalıştığında hata iletileri yedekleme ve okunması kolay temizlenir.
- HDInsight küme baş düğümü adı bir yürütme hedefi bağlanıldığında düzgün şekilde biçimlendirilmemiş daha iyi hata iletisi görüntülenir.
- Gizli dizi kimlik bilgisi hizmetinde bulunmadığında, daha iyi hata iletisi gösterilir. 
- MMLSpark kitaplığı desteklemek için Apache Spark 2.2 yükseltilir.
- MMLSpark artık konu kodlama dönüştürme (kodlama kafes) tıbbi belgeleri içerir.
- `matplotlib` sürüm 2.1.0 Şimdi sevk edilen çıkış-hazır Workbench ile ' dir.

#### <a name="jupyter-notebook"></a>Jupyter Notebook
- Not defteri adı arama artık not defterlerini Görünümü'nde düzgün çalışır.
- Şimdi bir not defteri not defterlerini görüntüleme silebilirsiniz.
- Yeni Sihirli `%upload_artifact` çalıştırma geçmişi veri deposuna not defteri yürütme ortamında üretilen dosyaları karşıya yükleme için eklenir.
- Çekirdek hataları artık daha kolay hata ayıklama için Not Defteri iş durumu içinde takip edilir.
- Kullanıcı uygulama oturumunu açtığında Jupyter sunucusu artık düzgün bir şekilde kapanıyorsa öyle kapanır.

#### <a name="azure-portal"></a>Azure portal
- Deneme hesabı ve Model Yönetimi hesabı artık oluşturulabilir iki yeni Azure bölgelerinde: Batı Avrupa ve Güneydoğu Asya.
- Aboneliği için oluşturulan ilk hesaptır, model Yönetimi hesabı geliştirme ve test planı yalnızca kullanıma sunuldu. 
- Azure portalında Yardım bağlantısı için doğru belgeler sayfası işaret edecek şekilde güncelleştirilir.
- Geçerli olmadığından açıklama alanı Docker görüntü ayrıntıları sayfasından kaldırılır.
- Appınsights ve otomatik ölçeklendirme ayarları dahil olmak üzere ayrıntılarını, web hizmeti ayrıntıları sayfasına eklenir.
- Model Yönetimi sayfası, tarayıcıda üçüncü taraf tanımlama bilgilerini devre dışı bırakılsa dahi artık işler. 

#### <a name="operationalization"></a>Kullanıma hazır hale getirme
- Adında "puan" Web hizmetiyle artık başarısız olur.
- Kullanıcı, artık yalnızca bir Azure kaynak grubu veya abonelik katkıda bulunan erişimi olan bir dağıtım ortamı oluşturabilirsiniz. Aboneliğin tümü sahip erişimi artık gerekli değildir.
- Kullanıma hazır hale getirme artık CLI sekmesinde Otomatik Tamamlama Linux'ta ölçeklenebilme.
- Görüntü oluşturma hizmeti, artık Azure IOT Hizmetleri/cihazlar için yapı görüntüleri destekler.

#### <a name="sample-projects"></a>Örnek Proje
- [_Süsen sınıflandırması_ ](./tutorial-classifying-iris-part-1.md) örnek proje:
    - `iris_pyspark.py` olarak yeniden adlandırıldı `iris_spark.py`.
    - `iris_score.py` olarak yeniden adlandırıldı `score_iris.py`.
    - `iris.dprep` ve `iris.dsource` veri hazırlık Altyapısı güncelleştirmeleri yansıtacak şekilde güncelleştirilir.
    - `iris.ipynb` Not Defteri, HDInsight kümesinde çalışacak şekilde düzeltilir.
    - Çalıştırma geçmişi içinde açık `iris.ipynb` not defteri hücre.
- [_Bisiklet paylaşımı verileri kullanarak veri hazırlığı Gelişmiş_ ](./tutorial-bikeshare-dataprep.md) "İşleme hata değerini" adım örnek proje düzeltildi.
- [_MMLSpark yetişkinlere yönelik Görselleştirmenizdeki verilerin üzerinde_ ](https://github.com/Azure/MachineLearningSamples-mmlspark) kodunuzla `docker.runconfig` biçimi için YAML JSON'dan güncelleştirildi.
- [_Hiper parametre ayarı dağıtılmış_ ](./scenario-distributed-tuning-of-hyperparameters.md) kodunuzla`docker.runconfig` biçimi için YAML JSON'dan güncelleştirildi.
- Yeni örnek proje [ _CNTK kullanarak görüntü sınıflandırması_](./scenario-image-classification-using-cntk.md).


## <a name="2017-10-sprint-0"></a>2017-10 (sprint 0) 
**Sürüm numarası**: 0.1.1710.31013 &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;([sürümünüzü bulmak](../service/known-issues-and-troubleshooting-guide.md#find-the-workbench-build-number))

İlk bizim genel Önizleme'de Microsoft Ignite 2017 konferansına izleyerek Azure Machine Learning Workbench, ilk güncelleştirme Hoş Geldiniz. Ana güncelleştirmeler bu sürümde, güvenilirlik ve sabitleme giderir.  Biz giderilen kritik sorunlar bazıları şunlardır:

### <a name="new-features"></a>Yeni Özellikler
- macOS High Sierra artık desteklenmektedir

### <a name="bug-fixes"></a>Hata düzeltmeleri
#### <a name="workbench-experience"></a>Workbench deneyimi
- Sürükle ve bırak Workbench dosyasına Workbench çökmesine neden olur.
- Terminal penceresinde bir IDE Workbench tanımıyor için yapılandırılan VS code'da _az ml_ komutları.

#### <a name="workbench-authentication"></a>Workbench kimlik doğrulaması
Bir rapor çeşitli oturum açma ve kimlik doğrulama sorunları geliştirmek için güncelleştirme sayısı yaptık.
- Özellikle Internet bağlantısı kararlı olmadığında pencerelerinin yukarı, kimlik doğrulama penceresi tutar.
- Kimlik doğrulama belirteci süre sonu etrafında sorunları güvenilirlik düzeyi artırıldı.
- Bazı durumlarda, iki kez kimlik doğrulaması penceresi görüntülenir.
- Workbench ana pencere yine de "kimlik doğrulaması" ileti kimlik doğrulama işlemi tamamlandığında ve zaten kapatıldı açılır iletişim kutusu görüntüler.
- Internet bağlantısı yoksa bu kimlik doğrulaması iletişim kutusu ile boş bir ekran açılır.

#### <a name="data-preparation"></a>Veri hazırlama 
- Belirli bir değere filtrelenir, hataları ve eksik değerleri de filtreleniyor.
- Bir örnekleme stratejisi değiştirilmesi, sonraki mevcut birleştirme işlemleri kaldırır.
- Eksik değerini değiştirerek dönüştürme NaN dikkate almaz.
- Null değer karşılaştığında tarih tür çıkarımı, özel durum oluşturur.

#### <a name="job-execution"></a>İş yürütme
- Boyut sınırını aştığı için proje klasörü karşıya yüklemek iş yürütme başarısız olduğunda hiçbir hata iletisini Temizle yoktur.
- Kullanıcının Python betiğini çalışma dizini değişirse, çıktı klasörüne yazılmış olan dosyalar izlenmez. 
- Geçerli projeye ait olandan farklı bir etkin Azure aboneliği ise, iş gönderme bir 403 hatası oluşur.
- Docker mevcut olmadığında, kullanıcı bir yürütme hedefi Docker kullanmaya çalışırsa Temizle hata iletisi döndürülür.
- .runconfig dosyası kaydedilmiyor otomatik olarak kullanıcı tıkladığında _çalıştırma_ düğmesi.

#### <a name="jupyter-notebook"></a>Jupyter Notebook
- Kullanıcı belirli bir oturum açma türleri ile kullanıyorsa, Not Defteri sunucusu başlatılamıyor.
- Not Defteri sunucusu hata iletilerini kullanıcıya görünür günlüklerinde yüzey değil.

#### <a name="azure-portal"></a>Azure portal
- Azure portal'ın koyu tema seçilmesi, bir siyah kutu olarak görüntülemek, Model Yönetimi dikey penceresinde neden olur.

#### <a name="operationalization"></a>Kullanıma hazır hale getirme
- Bir web hizmetini güncelleştirmek için bir bildirim yeniden rastgele bir ad ile oluşturulan yeni bir Docker görüntüsü neden olur.
- Web hizmeti günlükleri Kubernetes küme alınamıyor.
- Yanıltıcı bir hata iletisi, kullanıcı bir Model Yönetimi hesabı veya bir ML işlem hesabı oluşturma girişiminde bulunduğunda yazdırılır ve izin sorunları karşılaşır.

## <a name="next-steps"></a>Sonraki adımlar

[Azure Machine Learning](../service/overview-what-is-azure-ml.md)’e genel bakışı okuyun.
