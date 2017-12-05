---
title: "Sprint 2 aralık 2017 Azure ML çalışma ekranı sürüm notları"
description: "Bu belgede Azure ML sprint 2 sürümü için güncelleştirmeler ayrıntıları"
services: machine-learning
author: raymondlaghaeian
ms.author: raymondl
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 12/04/2017
ms.openlocfilehash: 3209ad7c9b2afd9ff06d685c41b1775800a62a53
ms.sourcegitcommit: 7136d06474dd20bb8ef6a821c8d7e31edf3a2820
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2017
---
# <a name="sprint-2---december-2017"></a>2 - aralık 2017 sprint 

#### <a name="version-number-01171115263"></a>Sürüm numarası: 0.1.1711.15263

>İşte nasıl [sürüm numarasını bulun](https://docs.microsoft.com/en-us/azure/machine-learning/preview/known-issues-and-troubleshooting-guide).

Azure Machine Learning çalışma ekranı üçüncü güncelleştirmeye Hoş Geldiniz. Bu güncelleştirme, çalışma ekranı uygulamayı, komut satırı arabirimi (CLI) ve arka uç hizmetlerini geliştirmeleri içerir. Gülümsemeleri göndermek için çok teşekkür ederiz ve frowns. Aşağıdaki güncelleştirmeler çoğunu görüşlerinizi doğrudan sonuçları olarak yapılır. 

## <a name="notable-new-features"></a>Önemli yeni özellikleri
- [SQL Server ve Azure SQL DB veri kaynağı olarak desteği](https://docs.microsoft.com/en-us/azure/machine-learning/preview/data-prep-appendix2-supported-data-sources#types) 
- [GPU desteğiyle MMLSpark kullanarak Spark üzerinde derin öğrenme](https://github.com/Azure/mmlspark/blob/master/docs/gpu-setup.md)
- [Tüm AML kapsayıcıları (ek bir adım gerekli değildir) dağıtılan Azure IOT sınır cihazları ile uyumludur](http://aka.ms/aml-iot-edge-blog)
- Kayıtlı model listesi ve ayrıntılı görünümler kullanılabilir Azure portalı
- SSH anahtar tabanlı kimlik doğrulaması kullanıcı adı/parola tabanlı erişim yanı sıra kullanarak işlem hedefleri erişme. 
- Yeni düzeni sıklığı denetçisi veri prep deneyimi. 

## <a name="detailed-updates"></a>Ayrıntılı güncelleştirmeleri
Azure Machine Learning bu sprint içinde her bileşen bölümünde ayrıntılı güncelleştirmelerin bir listesi aşağıda verilmiştir.

### <a name="installer"></a>Yükleyici
- Yükleyici kendi kendini güncelleştirme düzeltmeleri hataları ve yeni özellikler yeniden yüklemenize gerek kalmadan kullanıcı desteklenebilir olabilir

### <a name="workbench-authentication"></a>Çalışma ekranı kimlik doğrulaması
- Birden çok kimlik doğrulama sistemi giderir. Lütfen oturum açma sorunları yaşamaya durumunda bize bildirin.
- Daha kolay hale UI değişiklikleri Proxy Manager ayarları bulunamıyor.

### <a name="workbench"></a>Workbench
- Salt okunur dosya görünümü açık mavi arka planı artık sahiptir.
- Daha fazla bulunabilir yapma hakkı taşınan Düzenle düğmesine.
- "dsource", "dprep" ve "ipynb" dosya biçimleri şimdi ham metin biçiminde işlenip
- Çalışma ekranı artık betikleri düzenlemek ve yalnızca (örneğin, dizüstü bilgisayarlar, veri kaynakları, veri hazırlık paketleri) bir zengin düzenleme deneyimi olan dosya türlerini düzenlemek için çalışma ekranı kullanmak için dış IDE'nin kullanarak doğrultusunda kullanıcılara yol gösteren yeni bir düzenleme deneyimi sahiptir
- Çalışma alanları ve kullanıcının erişimi projeleri listesi yükleniyor şimdi önemli ölçüde daha hızlıdır

### <a name="data-preparation"></a>Veri hazırlama 
- Bir desen sıklığı sütundaki dize desenlerini görüntülemek için denetleyici. Ayrıca bu desenleri kullanarak verilerinizi filtre uygulayabilirsiniz. Bu, bir görünüm değeri sayar denetçisine benzer gösterir. Desen sıklığı benzersiz veri sayısı yerine benzersiz desenleri verilerin sayılarını gösterir farktır. Ayrıca veya belirli bir desene uyan tüm satırları uzaklaştırma filtre uygulayabilirsiniz.

![Görüntü düzeni sıklığı denetçisinin ürün sayısı](media/release-notes-sprint-2/pattern-inspector-product-number.png)

- 'Sütun örneği tarafından türetilen' dönüşümünde gözden geçirmek için sınır durumlarda öneren sırasında performans iyileştirmeleri

- [SQL Server ve Azure SQL DB veri kaynağı olarak desteği](https://docs.microsoft.com/en-us/azure/machine-learning/preview/data-prep-appendix2-supported-data-sources#types) 

![Yeni bir SQL server veri kaynağı oluşturma resmi](media/release-notes-sprint-2/sql-server-data-source.png)

- "Bir bakışta" satır ve sütun sayılarını görünümünü etkin

![Satır sütun sayısı bir yetkisiz kullanım adresindeki görüntüsü](media/release-notes-sprint-2/row-col-count.png)

- Veri hazırlığı tüm işlem bağlamlarda etkin
- Bir SQL Server veritabanı kullanan veri kaynaklarına tüm işlem bağlamlarda etkinleştirilir
- Veri sütunları veri türüne göre filtrelenebilir kılavuz hazırla
- Birden çok sütun tarihine dönüştürme sabit sorun
- Kullanıcı Gelişmiş modda çıktı sütun adı değişirse sorunu sabit kullanıcı çıkış sütununun sütun örnekte tarafından türetilen bir kaynak olarak seçebilirsiniz.

### <a name="job-execution"></a>İş yürütme
Şimdi oluşturmak ve SSH anahtar tabanlı kimlik doğrulaması aşağıdaki adımları kullanarak remotedocker veya küme türü işlem hedef erişebilirsiniz:
- Aşağıdaki komut CLI kullanarak bir işlem hedef ekleme
```
az ml computetarget attach remotedocker -a <fqdn or IP address> -n <name for your compute target> -u <username to be used to access the compute target> –k
```
[!NOTE] oluşturmak ve SSH anahtarı kullanmak için komutu -k seçeneğinde belirtir.

- Azure ML çalışma ekranı ortak bir anahtar oluşturmak ve Konsolunuzda çıktı. Aynı kullanıcı adı kullanarak işlem hedef oturum ve bu ortak anahtarla ~/.ssh/authorized_keys dosya ekleyin.

- Bu işlem hedef hazırlamak ve yürütme için kullanın ve Azure ML çalışma ekranı kimlik doğrulaması için bu anahtarı kullanırsınız.  

İşlem hedefleri oluşturma hakkında daha fazla bilgi için bkz: [Azure Machine Learning deneme hizmeti yapılandırma](https://docs.microsoft.com/en-us/azure/machine-learning/preview/experimentation-service-configuration)

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

![portal model ayrıntıları](media/release-notes-sprint-2/model-list.jpg)

![portalında modeline genel bakış](media/release-notes-sprint-2/model-overview-portal.jpg)

### <a name="mmlspark"></a>MMLSpark
- İle Spark öğrenme derin [GPU desteği](https://github.com/Azure/mmlspark/blob/master/docs/gpu-setup.md)
- Resource Manager şablonları kolay kaynak dağıtımı için desteği
- SparklyR ekosistemi desteği
- [AZTK tümleştirme](https://github.com/Azure/aztk/wiki/Spark-on-Azure-for-Python-Users#optional-set-up-mmlspark)

### <a name="sample-projects"></a>Örnek Proje
- Yeni Azure ML SDK sürümüyle güncelleştirilmiş Iris ve SparkMML örnekleri

## <a name="breaking-changes"></a>YENİ DEĞİŞİKLİKLER
- Yükseltilen `--type` anahtarının `az ml computetarget attach` alt komutu. 

- `az ml computetarget attach --type remotedocker`artık`az ml computetarget attach remotedocker`

- `az ml computetarget attach --type cluster`artık`az ml computetarget attach cluster`
