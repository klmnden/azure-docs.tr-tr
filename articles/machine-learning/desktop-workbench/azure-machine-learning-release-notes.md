---
title: Azure Machine Learning yenilikler nelerdir?
description: Bu belge Azure Machine Learning güncelleştirmeleri ayrıntılarını verir.
services: machine-learning
author: hning86
ms.author: haining
manager: mwinkle
ms.service: machine-learning
ms.workload: data-services
ms.topic: reference
ms.date: 03/28/2018
ms.openlocfilehash: e30943426ad68171e1464f828a9c8672b06c975a
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/19/2018
---
# <a name="whats-new-in-azure-machine-learning"></a>Azure Machine Learning’deki Yenilikler

Bu makalede, yeni sürümleri hakkında bilgi edinin [Azure Machine Learning Hizmetleri](../service/overview-what-is-azure-ml.md). 

## <a name="2018-03-sprint-4"></a>2018-03 (sprint 4)
**Sürüm numarası**: 0.1.1801.24353 &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;([sürümüne bulun](../service/known-issues-and-troubleshooting-guide.md#find-the-workbench-build-number))


Azure Machine Learning çalışma ekranı beşinci güncelleştirmeye Hoş Geldiniz. Aşağıdaki güncelleştirmeler çoğunu görüşlerinizi doğrudan sonuçları olarak yapılır. Lütfen geliyor kalmalarını!

**Önemli yeni özellikleri ve değişiklikleri**

- Temelli yürütme komut dosyalarınızı uzak Ubuntu vm'lerde uzaktan docker yanı sıra ortamınızdaki üzerinde yerel olarak çalıştırma desteği.
- Yeni ortam deneyimi çalışma ekranı uygulamasında işlem hedefleri oluşturmak ve yapılandırmaları CLI tabanlı deneyimi bizim ek olarak çalıştırmak sağlar.
![Ortamlar sekmesi](media/azure-machine-learning-release-notes/environment-page.png)
- Özelleştirilebilir çalıştırma geçmişi raporları ![yeni geçmiş raporları çalıştırmak görüntüsü](media/azure-machine-learning-release-notes/new-run-history-reports.png)

**Ayrıntılı güncelleştirmeleri**

Azure Machine Learning bu sprint içinde her bileşen bölümünde ayrıntılı güncelleştirmelerin bir listesi aşağıda verilmiştir.

### <a name="workbench-ui"></a>Çalışma ekranı kullanıcı Arabirimi
- Özelleştirilebilir çalıştırma geçmişi raporları
  - Çalıştırma geçmişi raporları için geliştirilmiş grafik yapılandırma
    - Kullanılan giriş noktaları değiştirilebilir.
    - Üst düzey filtre eklenebilir ve değişiklik ![filtreler Ekle](media/azure-machine-learning-release-notes/add-filters.jpg)
    - Grafikler ve istatistikleri eklenen değiştirilebilir veya (ve sürükle ve bırak düzenlenmeyecek).
    ![Yeni grafikler oluşturma](media/azure-machine-learning-release-notes/configure-charts.png)

  - CRUD çalıştırma geçmişi raporları
  - Ardışık Düzen üzerinde gibi davranan yapılandırmalar sunucu tarafı raporlara, seçilen giriş noktalarından çalıştıran var olan tüm çalıştırma geçmişi liste görünümü taşındı.

- Ortamlar sekmesi
  - Kolayca yeni işlem hedef ekleme ve yapılandırma dosyalarını projenize çalıştırın ![yeni işlem hedefi](media/azure-machine-learning-release-notes/add-new-environments.png)
  - Yönetme ve yapılandırma dosyalarınızın basit, form tabanlı UX kullanarak güncelleştirme
  - Yürütme için ortamınızı hazırlama yeni düğmesi

- Kenar dosyaların listesi için performans iyileştirmeleri

### <a name="data-preparation"></a>Veri hazırlama 
- Azure Machine Learning çalışma ekranı artık bir sütun için bilinen bir sütunun adını kullanarak arayabilirsiniz izin verir.


### <a name="experimentation"></a>Deneme
- Azure Machine Learning çalışma ekranı şimdi komut dosyalarınızı yerel olarak kendi python veya pyspark ortamda çalıştırılmasını destekler. Bu özelliği, kullanıcı oluşturur ve uzak VM kendi ortamda yönetir ve Azure Machine Learning çalışma ekranı hedefleyen üzerinde betikleri çalıştırmak için kullanın. Lütfen bakın [Azure Machine Learning deneme hizmeti yapılandırma](experimentation-service-configuration.md) 

### <a name="model-management"></a>Model Yönetimi
- Dağıtılan kapsayıcıları özelleştirmek için destek: get apt vb. kullanarak dış kitaplıkları yüklenmesini sağlayarak kapsayıcı görüntü özelleştirme sağlar. Artık, PIP yüklenebilir kitaplıklara sınırlı değildir. Bkz: [belgelerine](model-management-custom-container.md) daha fazla bilgi için.
  - Kullanım `--docker-file myDockerStepsFilename` bildirimi, görüntü veya hizmeti oluşturma komutlarını bayrağı ve dosya adı.
  - Temel görüntü Ubuntu ve değiştirilemez olduğunu unutmayın.
  - Örnek komut: 
  
      ```shell
      $ az ml image create -n myimage -m mymodel.pkl -f score.py --docker-file mydockerstepsfile
      ```



## <a name="2018-01-sprint-3"></a>2018-01 (sprint 3) 
**Sürüm numarası**: 0.1.1712.18263 &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;([sürümüne bulun](../service/known-issues-and-troubleshooting-guide.md#find-the-workbench-build-number))

Güncelleştirmeleri ve bu sprint yenilikleri şunlardır: Bu güncelleştirmeler çoğunu doğrudan kullanıcı geri bildirim sonucu olarak yapılır. 


Azure Machine Learning bu sprint içinde her bileşen bölümünde ayrıntılı güncelleştirmelerin bir listesi aşağıda verilmiştir.

- Kimlik doğrulama yığınını güncelleştirmeleri zorlar başlangıçta oturum açma ve hesap seçimi

### <a name="workbench"></a>Workbench
- Program Ekle/Kaldır'ndan uygulamanın yükleme/kaldırma yeteneği
- Kimlik doğrulama yığınını güncelleştirmeleri zorlar başlangıç oturum açma ve hesap seçimi
- Windows geliştirilmiş çoklu oturum açma (SSO) deneyimi
- Farklı kimlik bilgileriyle birden çok kiracıya ait olan kullanıcıların artık çalışma ekranına aktardığınız oturum mümkün olacaktır

### <a name="ui"></a>KULLANICI ARABİRİMİ
- Genel geliştirmeler ve hata düzeltmeleri

### <a name="notebooks"></a>Dizüstü bilgisayarlar
- Genel geliştirmeler ve hata düzeltmeleri

### <a name="data-preparation"></a>Veri hazırlama 
- Örnekle dönüşümleri gerçekleştirirken geliştirilmiş otomatik-önerileri
- Desen sıklığı denetçisi için geliştirilmiş algoritması
- Örnekle dönüşümleri gerçekleştirilirken örnek veriler ve geri bildirim gönderme olanağı ![gönderme geri bildirim bağlantısı üzerinde görüntüsü türetilen sütun dönüştürme](media/azure-machine-learning-release-notes/SendFeedbackFromDeriveColumn.png)
- Spark çalışma zamanı geliştirmeleri
- Scala Pyspark değiştirilmiştir
- Zaman serisi denetleyici için veri uygulanamaz kapatmaya sabit kullanamama 
- Sabit veri hazırlığı yürütme HDI için yanıt vermemesine saat

### <a name="model-management-cli-updates"></a>Model yönetim CLI güncelleştirir 
  - Aboneliğin sahipliğini, artık kaynakların sağlamak için gereklidir. Katkıda bulunan erişim kaynak grubuna dağıtım ortamı ayarlamak yeterli olacaktır.
  - Etkin yerel ortamı Kurulumu ücretsiz abonelikler 

## <a name="2017-12-sprint-2-qfe"></a>(2 QFE sprint) 2017-12 
**Sürüm numarası**: 0.1.1711.15323 &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;([sürümüne bulun](../service/known-issues-and-troubleshooting-guide.md#find-the-workbench-build-number))

QFE (hızlı düzeltme Mühendisliği) sürüm, küçük bir sürüm olmasıdır. Birkaç telemetri sorunları giderir ve ürünü nasıl kullanıldığını daha iyi anlamak için ürün ekibi yardımcı olur. Bilgi Bankası, ürün deneyimini geliştirmek için gelecekteki çaba içine gidebilirsiniz. 

Ayrıca, iki önemli güncelleştirmeleri vardır:

- Veri hazırlama paketlerinde görüntülenmesini zaman serisi denetçisi engelleyen veri hazırlığı hata sabit.
- Komut satırı aracı, artık Machine Learning işlem ACS kümeleri sağlamak için bir Azure aboneliğine sahip olmanız gerekir. 

## <a name="2017-12-sprint-2"></a>2017-12 (sprint 2)
**Sürüm numarası**: 0.1.1711.15263 &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;([sürümüne bulun](../service/known-issues-and-troubleshooting-guide.md#find-the-workbench-build-number))

Azure Machine Learning üçüncü güncelleştirmeye Hoş Geldiniz. Bu güncelleştirme, çalışma ekranı uygulamayı, komut satırı arabirimi (CLI) ve arka uç hizmetlerini geliştirmeleri içerir. Gülümsemeleri göndermek için çok teşekkür ederiz ve frowns. Aşağıdaki güncelleştirmeler çoğunu görüşlerinizi doğrudan sonuçları olarak yapılır. 

**Önemli yeni özellikleri**
- [SQL Server ve Azure SQL DB veri kaynağı olarak desteği](data-prep-appendix2-supported-data-sources.md#types) 
- [GPU desteğiyle MMLSpark kullanarak Spark üzerinde derin öğrenme](https://github.com/Azure/mmlspark/blob/master/docs/gpu-setup.md)
- [Tüm AML kapsayıcıları (ek bir adım gerekli değildir) dağıtılan Azure IOT sınır cihazları ile uyumludur](http://aka.ms/aml-iot-edge-blog)
- Kayıtlı model listesi ve ayrıntılı görünümler kullanılabilir Azure portalı
- SSH anahtar tabanlı kimlik doğrulaması kullanıcı adı/parola tabanlı erişim yanı sıra kullanarak işlem hedefleri erişme. 
- Yeni düzeni sıklığı denetçisi veri prep deneyimi. 

**Güncelleştirmeleri ayrıntılı** aşağıdaki Azure Machine Learning bu sprint içinde her bileşen bölümünde ayrıntılı güncelleştirmeleri listesi verilmiştir.

### <a name="installer"></a>Yükleyici
- Yükleyici kendi kendini güncelleştirme düzeltmeleri hataları ve yeni özellikler yeniden yüklemenize gerek kalmadan kullanıcı desteklenebilir olabilir

### <a name="workbench-authentication"></a>Çalışma ekranı kimlik doğrulaması
- Birden çok kimlik doğrulama sistemi giderir. Lütfen oturum açma sorunları yaşamaya durumunda bize bildirin.
- Daha kolay hale UI değişiklikleri Proxy Manager ayarları bulunamıyor.

### <a name="workbench"></a>Workbench
- Salt okunur dosya görünümü açık mavi arka planı artık sahiptir.
- Daha fazla bulunabilir yapma hakkı taşınan Düzenle düğmesine.
- "dsource", "dprep" ve "ipynb" dosya biçimleri şimdi ham metin biçiminde işlenip
- Çalışma ekranı artık betikleri düzenlemek ve yalnızca (örneğin, dizüstü bilgisayarlar, veri kaynakları, veri hazırlık paketleri) bir zengin düzenleme deneyimi olan dosya türlerini düzenlemek için çalışma ekranı kullanmak için dış IDE kullanarak doğrultusunda kullanıcılara yol gösteren yeni bir düzenleme deneyimi sahiptir
- Çalışma alanları ve kullanıcının erişimi projeleri listesi yükleniyor şimdi önemli ölçüde daha hızlıdır

### <a name="data-preparation"></a>Veri hazırlama 
- Bir desen sıklığı sütundaki dize desenlerini görüntülemek için denetleyici. Ayrıca bu desenleri kullanarak verilerinizi filtre uygulayabilirsiniz. Bu, bir görünüm değeri sayar denetçisine benzer gösterir. Desen sıklığı benzersiz veri sayısı yerine benzersiz desenleri verilerin sayılarını gösterir farktır. Ayrıca veya belirli bir desene uyan tüm satırları uzaklaştırma filtre uygulayabilirsiniz.

![Görüntü düzeni sıklığı denetçisinin ürün sayısı](media/azure-machine-learning-release-notes/pattern-inspector-product-number.png)

- 'Sütun örneği tarafından türetilen' dönüşümünde gözden geçirmek için sınır durumlarda öneren sırasında performans iyileştirmeleri

- [SQL Server ve Azure SQL DB veri kaynağı olarak desteği](data-prep-appendix2-supported-data-sources.md#types) 

![Yeni bir SQL server veri kaynağı oluşturma resmi](media/azure-machine-learning-release-notes/sql-server-data-source.png)

- "Bir bakışta" satır ve sütun sayılarını görünümünü etkin

![Satır sütun sayısı bir yetkisiz kullanım adresindeki görüntüsü](media/azure-machine-learning-release-notes/row-col-count.png)

- Veri hazırlığı tüm işlem bağlamlarda etkin
- Bir SQL Server veritabanı kullanan veri kaynaklarına tüm işlem bağlamlarda etkinleştirilir
- Veri sütunları veri türüne göre filtrelenebilir kılavuz hazırla
- Birden çok sütun tarihine dönüştürme sabit sorun
- Kullanıcı Gelişmiş modda çıktı sütun adı değişirse sorunu sabit kullanıcı çıkış sütununun sütun örnekte tarafından türetilen bir kaynak olarak seçebilirsiniz.

### <a name="job-execution"></a>İş yürütme
Şimdi oluşturmak ve SSH anahtar tabanlı kimlik doğrulaması aşağıdaki adımları kullanarak remotedocker veya küme türü işlem hedef erişebilirsiniz:
- Aşağıdaki komut CLI kullanarak bir işlem hedef ekleme

    ```azure-cli
    $ az ml computetarget attach remotedocker --name "remotevm" --address "remotevm_IP_address" --username "sshuser" --use-azureml-ssh-key
    ```
>[!NOTE]
>oluşturmak ve SSH anahtarı kullanmak için komutu -k (veya---azureml-ssh-anahtar kullan) seçeneğinde belirtir.

- Azure ML çalışma ekranı ortak bir anahtar oluşturmak ve Konsolunuzda çıktı. Aynı kullanıcı adı kullanarak işlem hedef oturum ve bu ortak anahtarla ~/.ssh/authorized_keys dosya ekleyin.

- Bu işlem hedef hazırlamak ve yürütme için kullanın ve Azure ML çalışma ekranı kimlik doğrulaması için bu anahtarı kullanırsınız.  

İşlem hedefleri oluşturma hakkında daha fazla bilgi için bkz: [Azure Machine Learning deneme hizmeti yapılandırma](experimentation-service-configuration.md)

### <a name="visual-studio-tools-for-ai"></a>AI için Visual Studio Araçları
- Desteği eklendi [AI için Visual Studio Araçları](https://marketplace.visualstudio.com/items?itemName=ms-toolsai.vstoolsai-vs2017). 

### <a name="command-line-interface-cli"></a>Komut Satırı Arabirimi (CLI)
- Eklenen `az ml datasource create` komutu sağlayan bir veri kaynağı dosyası komut satırı ile oluşturmak için

### <a name="model-management-and-operationalization"></a>Model yönetimi ve Operationalization
- [Tüm AML kapsayıcıları (ek bir adım gerekli değildir) operationalized zaman Azure IOT sınır cihazları ile uyumludur](http://aka.ms/aml-iot-edge-blog) 
- O16n CLI hata iletilerinde geliştirmeleri
- Model yönetim portalındaki UX hata düzeltmeleri  
- Ayrıntı Sayfası model yönetim öznitelikler için tutarlı harfi büyük/küçük harf
- Gerçek zamanlı çağrıları zaman aşımı Puanlama 60 saniye olarak ayarlayın
- Kayıtlı model listesi ve ayrıntılı görünümlerini Azure portalında kullanılabilir

![portal model ayrıntıları](media/azure-machine-learning-release-notes/model-list.jpg)

![portalında modeline genel bakış](media/azure-machine-learning-release-notes/model-overview-portal.jpg)

### <a name="mmlspark"></a>MMLSpark
- İle Spark öğrenme derin [GPU desteği](https://github.com/Azure/mmlspark/blob/master/docs/gpu-setup.md)
- Resource Manager şablonları kolay kaynak dağıtımı için desteği
- SparklyR ekosistemi desteği
- [AZTK tümleştirme](https://github.com/Azure/aztk/wiki/Spark-on-Azure-for-Python-Users#optional-set-up-mmlspark)

### <a name="sample-projects"></a>Örnek Proje
- [Iris](https://github.com/Azure/MachineLearningSamples-Iris) ve [MMLSpark](https://github.com/Azure/mmlspark) yeni Azure ML SDK sürümüyle güncelleştirilmiş örnekleri

### <a name="breaking-changes"></a>Yeni değişiklikler
- Yükseltilen `--type` anahtarının `az ml computetarget attach` bir alt için. 

    - `az ml computetarget attach --type remotedocker` artık `az ml computetarget attach remotedocker`
    - `az ml computetarget attach --type cluster` artık `az ml computetarget attach cluster`

## <a name="2017-11-sprint-1"></a>2017-11 (sprint 1) 
**Sürüm numarası**: 0.1.1710.31013 &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;([sürümüne bulun](../service/known-issues-and-troubleshooting-guide.md#find-the-workbench-build-number))

Bu sürümde, güvenlik, kararlılık ve çalışma ekranı uygulama, CLI ve arka uç hizmetlerini katman Bakımı çevresinde geliştirmeler yaptık. Çok gülümsemeler gönderdiğiniz için teşekkür ederiz ve frowns. Çoğu güncelleştirmeler geri bildirim doğrudan sonuçları olarak hale getirilir. Gelen kalmalarını!

### <a name="notable-new-features"></a>Önemli yeni özellikleri
- Azure ML kullanılabilir şimdi iki yeni Azure bölgelerinde: **Batı Avrupa** ve **Güneydoğu Asya**. Önceki bölgeleri katılma **Doğu ABD 2**, **Batı Orta ABD**, ve **Avustralya Doğu**, toplam sayısı getiren dağıtılan bölgeler için beş.
- Biz, Python kodu sözdizimi vurgulama okuma ve Python kaynak kodu düzenleme kolaylaştırmak için çalışma ekranı uygulamada etkinleştirilmiş. 
- Şimdi, sık kullanılan bir IDE tüm proje yerine, doğrudan bir dosyadan da başlatabilirsiniz.  Çalışma ekranı bir dosyayı açıp "Düzenle"'yi tıklatarak (şu anda VS Code ve PyCharm desteklenir), IDE geçerli dosya ve proje başlatır.  Çalışma ekranı metin düzenleyicide dosyayı düzenlemek için Düzenle düğmesini yanındaki oka tıklatabilirsiniz.  Düzenleme, yanlışlıkla yapılan değişiklikleri önleme tıklatıncaya kadar dosyaları salt okunurdur.
- Popüler çizim Kitaplığı `matplotlib` sürüm 2.1.0 çalışma ekranı uygulamayla Şimdi sevk.
- Biz, .NET Core sürüm 2.0 veri hazırlık altyapısı için yükseltilmiştir. Brew yükleme openssl gereksinimini macOS uygulama yüklemesi sırasında kaldırıldı. Ayrıca daha heyecan verici veri yolu yakın gelecekte gelmesini hazırlık özellikleri paves. 
- Daha ilgili sürüm notları almak ve geçerli uygulama sürümlerine göre istemleri güncelleştirmek için size bir sürüme özgü uygulama giriş sayfası etkinleştirdiniz.
- Şimdi, yerel kullanıcı adı bir boşluk varsa, bu uygulama başarıyla yüklenebilir. 

### <a name="detailed-updates"></a>Ayrıntılı güncelleştirmeleri
Azure Machine Learning bu sprint içinde her bileşen bölümünde ayrıntılı güncelleştirmelerinin bir listesini aşağıdadır.

#### <a name="installer"></a>Yükleyici
- Uygulama yükleyici şimdi uygulamanın daha eski bir sürüm ile oluşturulan yükleme dizinini siler.
- % 100 oranında macOS yüksek Sierra takılmış yükleyici müşteri adayları hatanın düzeltildiğini.
- Şimdi yükleme başarısız olursa, yükleyici günlüklerini gözden geçirmek kullanıcının yükleyici dizinine doğrudan bağlantı yoktur.
- Şimdi kendi kullanıcı adı alanı olan kullanıcılar için çalışır yükleyin.

#### <a name="workbench-authentication"></a>Çalışma ekranı kimlik doğrulaması
- Proxy Yöneticisi'nde kimlik doğrulaması için destek.
- Kullanıcı bir güvenlik duvarının arkasındaysa, artık oturum açmayı başarılı olur. 
- Kullanıcının birden çok Azure bölgelerinde deneme hesapları varsa ve tek bir bölge kullanılamaz olursa, uygulama artık askıda kalır.
- Kimlik doğrulama tamamlanmadı ve kimlik doğrulama iletişim kutusu hala görünür durumda olduğunda, uygulama artık yerel önbellekten çalışma yüklemeye çalışır.

#### <a name="workbench-app"></a>Çalışma ekranı uygulama
- Python kodu sözdizimi vurgulama, Metin Düzenleyicisi'nde etkinleştirilir.
- Düzenle düğmesi metin düzenleyicide, dosyayı düzenlemek için ya da bir IDE sağlar (VS Code ve PyCharm desteklenir) veya yerleşik bir metin düzenleyicisinde.
- Metin düzenleyici, salt okunur modda varsayılan olarak kullanılıyor. 
- Geçerli dosya kaydedilmiş ve bu nedenle artık kirli sonra düğmesi visual durumu şimdi değişiklikleri devre dışı olarak kaydedin.
- Çalışma ekranı kaydeder _tüm_ Çalıştır başlattığınızda dosyaları kaydedilmemiş.
- Çalışma ekranı otomatik olarak açılacak şekilde son çalışma alanı yerel makinede kullanılan hatırlıyor.
- Çalışma ekranı tek bir örneğini Şimdi Çalıştır izin verilir. Daha önce birden çok örneği aynı proje üzerinde çalışırken sorunları neden başlatılması.
- Yeniden adlandırılmış Dosya menüsü "Açık proje..." için "Ekle varolan klasör olarak proje..." 
- Sekme değiştirme çok daha hızlı sunulmuştur.
- Yardım bağlantıları yapılandırma IDE iletişim kutusuna eklenir.
- Geri bildirim formu şimdi son zamanı girdiğiniz e-posta adresi hatırlıyor.
- Bize daha fazla geri bildirim gönderebilmek için gülümsemeler ve frowns form metin alanı şimdi daha büyük,! 
- `--owner` Geçiş Yardım metni `az ml workspace create` düzeltilir.
- "Kolayca görüntüleme ve uygulamanın sürüm numarasını kopyalama kullanıcının yardımcı olmak için bir hakkında" iletişim kutusunu eklediğimiz.
- "Bir özellik önerme" menü öğesi için Yardım menüsünden eklenir.
- Deneme hesabı adı şimdi uygulama başlık çubuğu, uygulama adı "Azure Machine Learning çalışma ekranı" önceki görünür olur.
- Şimdi algılanan uygulama sürümlerine göre bir sürüme özgü uygulama giriş sayfası görüntülenir.

#### <a name="data-preparation"></a>Veri hazırlama 
- Dış web sitesi artık olası güvenlik sorunlarını önlemek için harita Denetçisi ' yüklenebilir.
- Histogram ve değer sayısı denetçiler artık grafik Logaritmik ölçek göstermek için seçeneği vardır.
- Bir hesaplama devam eden olduğunda, veri kalitesi çubuğu şimdi "hesaplama" durumunu göstermek için farklı bir renk gösterir.
- Sütun ölçümleri şimdi kategorik değer sütunlar için istatistik gösterir.
- Veri kaynağı adı son karakter artık kesilir.
- Veri hazırlık paketi artık belirgin performans artışı kaynaklanan sekmeler arasında geçiş yaparken açık kalır.
- Veri kaynağındaki veri görünümü ve ölçüm görünümü arasında geçiş yaparken, artık şimdi sütunların sırasını değiştirir.
- Geçersiz bir açma `.dprep` veya `.dsource` dosyasının çökmesine artık çalışma ekranı neden olur.
- Veri hazırlık paketi artık kullanabilir göreli yol çıktısında için _CSV'ye yazma_ dönüştürün.
- _Sütun tutmak_ dönüştürme düzenlendiğinde ek sütunlar eklemek kullanıcı artık izin verir.
- _Bunun yerine_ menü şimdi gerçekten başlatır _değiştirme değeri_ iletişim kutusu.
- _Değeri değiştirme_ hata oluşturma yerine beklendiği gibi şimdi işlevlerini dönüştürün.
- Veri hazırlık paketi artık veri dosyasına mutlak bir yol ile yerel bağlamda paketini çalıştırmak olası hale getirerek proje klasörünü dışında veri dosyalarını başvururken mutlak bir yol kullanır.
- _Tam dosya_ örnekleme stratejisi artık Azure kullanırken desteklendiğinden blob veri kaynağı olarak.
- Python koddan (veri hazırlık paketi) oluşturulan şimdi CR ve Windows'da kolay kolaylaştırarak LF taşır.
- _Ölçümleri Seç_ açılır veri görünümüne geçiş yaparken özelliği şimdi gizler.
- Python çalışma zamanı bile kullanırken, işlem parquet dosyaları artık çalışma ekranı açabilir. Daha önce yalnızca Spark parquet dosyaları işlenirken kullanılabilir. 
- İle bir sütundaki değerlerin çıkışı filtreleme _tarih_ veri türü artık veri hazırlık altyapısı çökmesine neden olur.
- Ölçümü görünümünü örnekleme stratejisi güncelleştirmeleri şimdi dikkate alır.
- İşlerini şimdi örnekleme uzaktan düzgün çalışır.

#### <a name="job-execution"></a>İş yürütme
- Bağımsız değişken artık çalıştırma geçmişi kaydında dahil edilmiştir.
- İçinde başlayacağı zamana işleri CLI Şimdi Çalıştır geçmiş işi panelinde otomatik olarak görünür.
- İş paneli artık Azure AD kiracısı eklenen Konuk kullanıcılar tarafından oluşturulan işleri gösterir.
- İş Masası iptal edin ve silme işlemlerinin daha kararlı.
- Yapılandırma dosyalarını hatalı biçimindeyse Çalıştır düğmesine tıklandığında, hata iletisi şimdi tetiklenir.
- Sonlandırma uygulama artık CLI başlayacağı zamana işleriyle karışıklığa neden olabilir.
- İçinde başlayacağı zamana işleri CLI şimdi standart dışı bile bir saat sonra yürütme bölme konumu devam eder.
- Python/PySpark veri hazırlık paketi çalıştırmak başarısız olduğunda daha iyi hata iletileri gösterilir.
- `az ml experiment clean` Şimdi de uzaktan VM Docker görüntülerinde temizler.
- `az ml experiment clean` Şimdi düzgün için yerel hedef macOS üzerinde çalışır.
- Yerel veya uzak Docker hedefleme çalıştığında, hata iletileri yedekleme ve kolay okunur temizlenir.
- Bir yürütme hedef olarak bağlı olduğunda Hdınsight küme baş düğüm adı doğru şekilde biçimlendirilmemiş daha iyi hata iletisi görüntülenir.
- Gizli anahtarı kimlik bilgisi hizmetinde bulunmadığında, daha iyi hata iletisi gösterilir. 
- MMLSpark kitaplığı destek Apache Spark 2.2 yükseltilir.
- MMLSpark artık konu kodlama dönüştürme (kafes kodlama) tıbbi belgeler için içerir.
- `matplotlib` 2.1.0 Şimdi sevk edilen çıkış-yepyeni çalışma ekranı ile sürümüdür.

#### <a name="jupyter-notebook"></a>Jupyter Notebook
- Dizüstü bilgisayar adı arama şimdi not defterlerini görünümünde düzgün çalışır.
- Not defterlerini görünümünde bir Not Defteri şimdi silebilirsiniz.
- Yeni Sihirli `%upload_artifact` çalıştırma geçmişi veri deposuna not defteri yürütme ortamında üretilen dosyalar için eklenir.
- Çekirdek hataları artık daha kolay hata ayıklama için dizüstü bilgisayar işi durumu çıkmış.
- Kullanıcının uygulama oturumunu açtığında Jupyter sunucu şimdi düzgün kapanır.

#### <a name="azure-portal"></a>Azure portalına
- Deneme hesabı ve Model yönetim hesabı artık oluşturulabilir iki yeni Azure bölgelerinde: Batı Avrupa ve Güneydoğu Asya.
- Aboneliği için oluşturulan Birincisi olduğunda model yönetim hesabı DevTest planı yalnızca kullanıma sunulmuştur. 
- Azure portalında yardım bağlantısına doğru belge sayfasına işaret edecek şekilde güncelleştirilir.
- Geçerli olmadığından açıklama alanı Docker görüntü ayrıntıları sayfasından kaldırılır.
- Ayrıntılar Appınsights'dan ve Otomatik ölçek ayarları dahil olmak üzere web hizmeti ayrıntı sayfası eklenir.
- Model Yönetim sayfasını tarayıcıda üçüncü taraf tanımlama bilgilerini devre dışı olsa bile şimdi işler. 

#### <a name="operationalization"></a>Operationalization
- Web hizmeti adı "puan" ile artık başarısız olur.
- Kullanıcı, artık yalnızca katkıda bulunan erişimi olan bir Azure kaynak grubu veya abonelik dağıtım ortamı oluşturabilirsiniz. Tüm abonelik sahibi erişimi artık gerekli değildir.
- Şimdi CLI operationalization sekmesinde Otomatik Tamamlama Linux'ta özgürlüğüne.
- Görüntü oluşturma hizmeti artık Azure IOT Hizmetleri/cihazlar için yapı görüntüleri destekler.

#### <a name="sample-projects"></a>Örnek Proje
- [_Iris sınıflandırma_ ](./tutorial-classifying-iris-part-1.md) örnek proje:
    - `iris_pyspark.py` yeniden adlandırılır `iris_spark.py`.
    - `iris_score.py` yeniden adlandırılır `score_iris.py`.
    - `iris.dprep` ve `iris.dsource` en son verileri hazırlık Altyapısı güncelleştirmeleri yansıtacak şekilde güncelleştirilir.
    - `iris.ipynb` Hdınsight kümesinde çalışmak için Not Defteri düzeltilir.
    - Çalıştırma geçmişi içinde açık `iris.ipynb` not defteri hücre.
- [_Veri bisiklet paylaşımı verileri kullanarak hazırlığı Gelişmiş_ ](./tutorial-bikeshare-dataprep.md) "İşlemek hata değerini" adım örnek proje sabit.
- [_Yetişkin Census verileri MMLSpark_ ](https://github.com/Azure/MachineLearningSamples-mmlspark) örnek proje `docker.runconfig` JSON öğesinden YAML için güncelleştirilmiş biçimi.
- [_Hyperparameter ayarlama dağıtılmış_ ](./scenario-distributed-tuning-of-hyperparameters.md) örnek proje`docker.runconfig` JSON öğesinden YAML için güncelleştirilmiş biçimi.
- Yeni örnek proje [ _görüntü CNTK kullanarak sınıflandırma_](./scenario-image-classification-using-cntk.md).


## <a name="2017-10-sprint-0"></a>2017-10 (sprint 0) 
**Sürüm numarası**: 0.1.1710.31013 &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;([sürümüne bulun](../service/known-issues-and-troubleshooting-guide.md#find-the-workbench-build-number))

Bizim ilk genel Önizleme Microsoft Ignite 2017 konferansında aşağıdaki Azure Machine Learning ekranının ilk güncelleştirme Hoş Geldiniz. Ana bu sürümde güvenilirlik ve sabitlemeyi güncelleştirmelerin giderir.  Biz ele kritik sorunlardan bazıları şunlardır:

### <a name="new-features"></a>Yeni Özellikler
- macOS yüksek Sierra artık desteklenmiyor

### <a name="bug-fixes"></a>Hata düzeltmeleri
#### <a name="workbench-experience"></a>Çalışma ekranı deneyimi
- Sürükle ve bırak çalışma ekranı dosyasına çalışma ekranı çökmesine neden olur.
- VS code'da çalışma ekranı tanımaz için bir IDE yapılandırılan terminal penceresi _az ml_ komutları.

#### <a name="workbench-authentication"></a>Çalışma ekranı kimlik doğrulaması
Biz yapılan bildirilen çeşitli oturum açma ve kimlik doğrulama sorunları artırmak için güncelleştirmeleri sayısı.
- Özellikle Internet bağlantısı kararlı olmadığında kimlik doğrulama penceresi pencerelerinin-yukarı, tutar.
- Geliştirilmiş güvenilirlik sorunlarını çözmek kimlik doğrulama belirteci süre sonu.
- Bazı durumlarda, iki kez kimlik doğrulaması penceresi görüntülenir.
- Çalışma ekranı ana penceresi hala zaten kapatıldığında açılır iletişim kutusunu ve kimlik doğrulama işlemi bittiğinde "kimlik doğrulaması" iletisi görüntüler.
- Internet bağlantısı yoksa bu kimlik doğrulama iletişim boş bir ekran ile açılır.

#### <a name="data-preparation"></a>Veri hazırlama 
- Belirli bir değere filtre uygulandığında, hataları ve eksik değerleri de filtrelenir.
- Bir örnekleme stratejiyi değiştirmeden sonraki varolan birleştirme işlemleri kaldırır.
- Eksik değerini değiştirerek dönüştürme NaN dikkate almaz.
- Null değer karşılaştığında tarih tür çıkarımı özel durum oluşturur.

#### <a name="job-execution"></a>İş yürütme
- Boyut sınırını aştığı için proje klasörünü karşıya yüklemek iş yürütme başarısız olduğunda Temizle hata iletisi yok.
- Kullanıcının Python komut çalışma dizini değişirse çıktıları klasörlere yazılmış dosyaları izlenmez. 
- Etkin Azure aboneliği geçerli projeye ait olandan farklı ise, iş gönderme 403 hatası oluşur.
- Docker mevcut olmadığında, kullanıcı bir yürütme hedef olarak Docker kullanmaya çalışırsa Temizle hata iletisi döndürülür.
- .runconfig dosyası kaydedilmedi otomatik olarak kullanıcı tıklattığında _çalıştırmak_ düğmesi.

#### <a name="jupyter-notebook"></a>Jupyter Notebook
- Belirli bir oturum açma türleri ile kullanıcı kullanıyorsa, Not Defteri sunucusu başlatılamıyor.
- Not Defteri sunucu hata iletileri, kullanıcıya görünür günlüklerindeki yüzey değil.

#### <a name="azure-portal"></a>Azure portalına
- Azure portal'ın koyu tema seçilmesi siyah bir kutu görüntülemek Model yönetim dikey neden olur.

#### <a name="operationalization"></a>Operationalization
- Bir web hizmeti güncelleştirmek için bir bildirim yeniden rastgele bir ad ile oluşturulmuş yeni bir Docker görüntüsü neden olur.
- Web hizmeti günlükleri Kubernetes kümeden alınamıyor.
- Yanıltıcı hata iletisi kullanıcı bir Model yönetim hesabı veya bir ML işlem hesabı oluşturma girişiminde bulunduğunda yazdırılabilir ve izinleri sorunları karşılaşır.

## <a name="next-steps"></a>Sonraki adımlar

[Azure Machine Learning](../service/overview-what-is-azure-ml.md)’e genel bakışı okuyun.