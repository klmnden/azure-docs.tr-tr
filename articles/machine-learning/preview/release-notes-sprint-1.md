---
title: "Sprint 1 Kasım 2017 Azure ML çalışma ekranı sürüm notları"
description: "Bu belgede Azure ML sprint 1 sürümü için güncelleştirmeler ayrıntıları"
services: machine-learning
author: hning86
ms.author: haining
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 11/06/2017
ms.openlocfilehash: a4945c77be5763ffeda328184149f712572937c0
ms.sourcegitcommit: afc78e4fdef08e4ef75e3456fdfe3709d3c3680b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/16/2017
---
# <a name="sprint-1---november-2017"></a>Sprint 1 - Kasım 2017 

**Sürüm numarası: 0.1.1710.31013**

Azure Machine Learning çalışma ekranı ikinci güncelleştirmeye Hoş Geldiniz. Çalışma ekranı uygulama, CLI ve arka uç hizmetlerini katmanı güvenlik, kararlılık ve Bakım çevresinde geliştirmeler yapmak devam ediyoruz. Çok gülümsemeler gönderdiğiniz için teşekkür ederiz ve frowns. Çoğu güncelleştirmeler geri bildirim doğrudan sonuçları olarak hale getirilir. Lütfen geliyor kalmalarını!

## <a name="notable-new-features"></a>Önemli yeni özellikleri
- Azure ML kullanılabilir şimdi iki yeni Azure bölgelerinde: **Batı Avrupa** ve **Güneydoğu Asya**. Önceki bölgeleri katılma **Doğu ABD 2**, **Batı Orta ABD**, ve **Avustralya Doğu**, toplam sayısı getiren dağıtılan bölgeler için beş.
- Biz, Python kodu sözdizimi vurgulama okuma ve Python kaynak kodu düzenleme kolaylaştırmak için çalışma ekranı uygulamada etkinleştirilmiş. 
- Şimdi, sık kullanılan bir IDE tüm proje yerine, doğrudan bir dosyadan da başlatabilirsiniz.  Çalışma ekranı bir dosyayı açıp "Düzenle"'yi tıklatarak (şu anda VS Code ve PyCharm desteklenir), IDE geçerli dosya ve proje başlatır.  Çalışma ekranı metin düzenleyicide dosyayı düzenlemek için Düzenle düğmesini yanındaki oka tıklatabilirsiniz.  Düzenleme, yanlışlıkla yapılan değişiklikleri önleme tıklatıncaya kadar dosyaları salt okunurdur.
- Popüler çizim Kitaplığı `matplotlib` sürüm 2.1.0 çalışma ekranı uygulamayla Şimdi sevk.
- Biz, .NET Core sürüm 2.0 veri hazırlık altyapısı için yükseltilmiştir. Brew yükleme openssl gereksinimini macOS uygulama yüklemesi sırasında kaldırıldı. Ayrıca daha heyecan verici veri yolu yakın gelecekte gelmesini hazırlık özellikleri paves. 
- Daha ilgili sürüm notları almak ve geçerli uygulama sürümlerine göre istemleri güncelleştirmek için size bir sürüme özgü uygulama giriş sayfası etkinleştirdiniz.
- Şimdi, yerel kullanıcı adı bir boşluk varsa, bu uygulama başarıyla yüklenebilir. 

## <a name="detailed-updates"></a>Ayrıntılı güncelleştirmeleri
Azure Machine Learning bu sprint içinde her bileşen bölümünde ayrıntılı güncelleştirmelerinin bir listesini aşağıdadır.

### <a name="installer"></a>Yükleyici
- Uygulama yükleyici şimdi uygulamanın daha eski bir sürüm ile oluşturulan yükleme dizinini siler.
- % 100 oranında macOS yüksek Sierra takılmış yükleyici müşteri adayları hatanın düzeltildiğini.
- Şimdi yükleme başarısız olursa, yükleyici günlüklerini gözden geçirmek kullanıcının yükleyici dizinine doğrudan bağlantı yoktur.
- Şimdi kendi kullanıcı adı alanı olan kullanıcılar için çalışır yükleyin.

### <a name="workbench-authentication"></a>Çalışma ekranı kimlik doğrulaması
- Proxy Yöneticisi'nde kimlik doğrulaması için destek.
- Kullanıcı bir güvenlik duvarının arkasındaysa, artık oturum açmayı başarılı olur. 
- Kullanıcının birden çok Azure bölgelerinde deneme hesapları varsa ve tek bir bölge kullanılamaz olursa, uygulama artık askıda kalır.
- Kimlik doğrulama tamamlanmadı ve kimlik doğrulama iletişim kutusu hala görünür durumda olduğunda, uygulama artık yerel önbellekten çalışma yüklemeye çalışır.

### <a name="workbench-app"></a>Çalışma ekranı uygulama
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

### <a name="data-preparation"></a>Veri hazırlama 
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

### <a name="job-execution"></a>İş yürütme
- Bağımsız değişken artık çalıştırma geçmişi kaydında dahil edilmiştir.
- İçinde başlayacağı zamana işleri CLI Şimdi Çalıştır geçmiş işi panelinde otomatik olarak görünür.
- İş paneli şimdi Konuk kullanıcılar AAD kiracısında eklenen tarafından oluşturulan işleri gösterir.
- İş Masası iptal edin ve silme işlemlerinin daha kararlı.
- Yapılandırma dosyalarını hatalı biçimindeyse Çalıştır düğmesine tıklandığında, hata iletisi şimdi tetiklenir.
- Sonlandırma uygulama artık CLI başlayacağı zamana işleriyle karışıklığa neden olabilir.
- İçinde başlayacağı zamana işleri CLI şimdi standart dışı bile bir saat sonra yürütme bölme konumu devam eder.
- Python/PySpark veri hazırlık paketi çalıştırmak başarısız olduğunda daha iyi hata iletileri gösterilir.
- `az ml experiment clean`Şimdi de uzaktan VM Docker görüntülerinde temizler.
- `az ml experiment clean`Şimdi düzgün için yerel hedef macOS üzerinde çalışır.
- Yerel veya uzak Docker hedefleme çalıştığında, hata iletileri yedekleme ve kolay okunur temizlenir.
- Bir yürütme hedef olarak bağlı olduğunda Hdınsight küme baş düğüm adı doğru şekilde biçimlendirilmemiş daha iyi hata iletisi görüntülenir.
- Gizli anahtarı kimlik bilgisi hizmetinde bulunmadığında, daha iyi hata iletisi gösterilir. 
- MMLSpark kitaplığı destek Apache Spark 2.2 yükseltilir.
- MMLSpark artık konu kodlama dönüştürme (kafes kodlama) tıbbi belgeler için içerir.
- `matplotlib`2.1.0 Şimdi sevk edilen çıkış-yepyeni çalışma ekranı ile sürümüdür.

### <a name="jupyter-notebook"></a>Jupyter Notebook
- Dizüstü bilgisayar adı arama şimdi not defterlerini görünümünde düzgün çalışır.
- Not defterlerini görünümünde bir Not Defteri şimdi silebilirsiniz.
- Yeni Sihirli `%upload_artifact` çalıştırma geçmişi veri deposuna not defteri yürütme ortamında üretilen dosyalar için eklenir.
- Çekirdek hataları artık daha kolay hata ayıklama için dizüstü bilgisayar işi durumu çıkmış.
- Kullanıcının uygulama oturumunu açtığında Jupyter sunucu şimdi düzgün kapanır.

### <a name="azure-portal"></a>Azure portalına
- Deneme hesabı ve Model yönetim hesabı artık oluşturulabilir iki yeni Azure bölgelerinde: Batı Avrupa ve Güneydoğu Asya.
- Aboneliği için oluşturulan Birincisi olduğunda model yönetim hesabı DevTest planı yalnızca kullanıma sunulmuştur. 
- Azure portalında yardım bağlantısına doğru belge sayfasına işaret edecek şekilde güncelleştirilir.
- Geçerli olmadığından açıklama alanı Docker görüntü ayrıntıları sayfasından kaldırılır.
- Ayrıntılar Appınsights'dan ve Otomatik ölçek ayarları dahil olmak üzere web hizmeti ayrıntı sayfası eklenir.
- Model Yönetim sayfasını tarayıcıda üçüncü taraf tanımlama bilgilerini devre dışı olsa bile şimdi işler. 

### <a name="operationalization"></a>Operationalization
- Web hizmeti adı "puan" ile artık başarısız olur.
- Kullanıcı, artık yalnızca katkıda bulunan erişimi olan bir Azure kaynak grubu veya abonelik dağıtım ortamı oluşturabilirsiniz. Tüm abonelik sahibi erişimi artık gerekli değildir.
- Şimdi CLI operationalization sekmesinde Otomatik Tamamlama Linux'ta özgürlüğüne.
- Görüntü oluşturma hizmeti artık Azure IOT Hizmetleri/cihazlar için yapı görüntüleri destekler.

### <a name="sample-projects"></a>Örnek Proje
- [_Iris sınıflandırma_ ](./tutorial-classifying-iris-part-1.md) örnek proje:
    - `iris_pyspark.py`yeniden adlandırılır `iris_spark.py`.
    - `iris_score.py`yeniden adlandırılır `score_iris.py`.
    - `iris.dprep`ve `iris.dsource` en son verileri hazırlık Altyapısı güncelleştirmeleri yansıtacak şekilde güncelleştirilir.
    - `iris.ipynb`Hdınsight kümesinde çalışmak için Not Defteri düzeltilir.
    - Çalıştırma geçmişi içinde açık `iris.ipynb` not defteri hücre.
- [_Veri bisiklet paylaşımı verileri kullanarak hazırlığı Gelişmiş_ ](./tutorial-bikeshare-dataprep.md) "İşlemek hata değerini" adım örnek proje sabit.
- [_Yetişkin Census verileri MMLSpark_ ](https://github.com/Azure/MachineLearningSamples-mmlspark) örnek proje `docker.runconfig` JSON öğesinden YAML için güncelleştirilmiş biçimi.
- [_Hyperparameter ayarlama dağıtılmış_ ](./scenario-distributed-tuning-of-hyperparameters.md) örnek proje`docker.runconfig` JSON öğesinden YAML için güncelleştirilmiş biçimi.
- Yeni örnek proje [ _görüntü CNTK kullanarak sınıflandırma_](./scenario-image-classification-using-cntk.md).
