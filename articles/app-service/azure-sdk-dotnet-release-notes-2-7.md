---
title: ".NET 2.7 ve 2.7.1 için Azure SDK sürüm notları"
description: ".NET 2.7 ve 2.7.1 için Azure SDK sürüm notları"
services: app-service\web
documentationcenter: .net
author: chrissfanos
editor: 
ms.assetid: 877d070a-9bd5-49b3-8fac-6bb5f65c3554
ms.service: app-service
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 02/24/2017
ms.author: juliako
ms.openlocfilehash: 9a69253129cdedc4f5d7e736d5bd8d6a68f95a1e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="azure-sdk-for-net-27-and-net-271-release-notes"></a>.NET 2.7 ve 2.7.1 için Azure SDK sürüm notları
## <a name="overview"></a>Genel Bakış
Bu belge, .NET 2.7 yayın için Azure SDK'sı için sürüm notlarını içermektedir. 

Belge de içeren sürüm notları için Azure SDK'sı .NET 2.7.1 yayın için.

Azure SDK 2.7 yalnızca Visual Studio 2015 ve Visual Studio 2013 de desteklenir. [Azure SDK 2.6](https://azure.microsoft.com/downloads/) olan son SDK Visual Studio 2012 için desteklenir.

Bu sürüm hakkında ayrıntılı bilgi için bkz: [Azure SDK 2.7 duyuru post](https://azure.microsoft.com/blog/2015/07/20/announcing-the-azure-sdk-2-7-for-net/) ve [Azure SDK'sı 2.7.1 duyuru post](http://go.microsoft.com/fwlink/?LinkId=623850).

## <a name="azure-sdk-for-net-27"></a>.NET 2.7 için Azure SDK’sını
### <a name="sign-in-improvements-for-visual-studio-2015"></a>Oturum açma için Visual Studio 2015 geliştirmeleri
Visual Studio 2015 için Azure SDK 2.7 Visual Studio 2015'te yeni kimlik yönetimi özelliklerini destekler.  Azure rol tabanlı erişim denetimi, bulut çözüm sağlayıcıları, DreamSpark ve diğer hesap ve abonelik türleri üzerinden erişim hesapları için destek içerir.

Azure SDK 2.7 ile dahil olan oturum açma geliştirmeleri, yalnızca Visual Studio 2015'te kullanılabilir. Desteği Visual Studio 2013 için Azure SDK 2.7.1 dahil edilmiştir.

### <a name="mobile-sdk"></a>Mobil SDK'sı
Güncelleştirilmiş **Mobile Apps** yeni yansıtacak şekilde şablonları [NuGet paketi](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server/) ve Kurulum işlemi.

### <a name="service-bus"></a>Service Bus
Genel hata düzeltmeleri ve geliştirmeler yapılmıştır. Güncelleştirmeler ve özellikler hakkında daha fazla ayrıntı için lütfen en son sürüm notları başvurun [Service Bus NuGet](http://www.nuget.org/packages/WindowsAzure.ServiceBus/).

### <a name="hdinsight-tools"></a>Hdınsight araçları
Bu sürümde aşağıdaki güncelleştirmeler yapılmıştır. Bu güncelleştirmeler önizlemede. Daha fazla bilgi için bkz: [bu blog](http://go.microsoft.com/fwlink/?LinkId=619108).

* Tez işlerinde Hive için Hive grafikleri
* Tam yığını DML IntelliSense desteği
* Pig şablon desteği
* Azure Hizmetleri için Storm şablonları

#### <a name="breaking-changes"></a>Yeni değişiklikler
* Eski **Storm** araçları bu sürümünü kullanırken, proje yükseltilmelidir. Daha fazla bilgi için bkz: [bu blog](http://go.microsoft.com/fwlink/?LinkId=619108).
* Visual Studio Web Express artık desteklenmiyor. Daha fazla bilgi için bkz: [bu blog](http://go.microsoft.com/fwlink/?LinkId=619108).

### <a name="azure-app-service-tools"></a>Azure uygulama hizmeti araçları
Bu sürümde Web araç uzantıları için aşağıdaki güncelleştirmeler yapılmıştır. Daha fazla bilgi için bkz: [bu](https://azure.microsoft.com/blog/2015/07/20/announcing-the-azure-sdk-2-7-for-net/) blogu. 

* Eklenen DreamSpark hesaplarına desteği
* Tam Azure Araçları için yeni Azure Resource Management API'leri desteklemek değişiklik
* Azure uygulama hizmeti için destek eklendi [bulut Gezgini](#cloud_explorer)

#### <a name="known-issues"></a>Bilinen sorunlar
Web uygulama dağıtım yuvası düğümleri Sunucu Gezgini yuvaları düğümünde görünmez ve Web uygulaması dağıtım yuvası alt düğümleri altında Cloud Explorer yükleme. Bu sorun çözümlendi ve sonraki SDK sürüm için hazır. 

### <a name="cloud_explorer"></a>Visual Studio 2015 için bulut Gezgini
Azure SDK 2.7 ve bu sayede Azure kaynaklarınızı görüntülemek, özelliklerini inceleyin ve Visual Studio içinde anahtar Geliştirici eylemler gerçekleştirmek Visual Studio 2015 için bulut Gezgini içerir. 

Cloud explorer aşağıdakileri destekler:

* Azure kaynaklarınızı kaynak grubu ve kaynak türü görünümleri 
* Kaynak (kaynak türü görünümünde kullanılabilen) ada göre ara
* Abonelikler ve rol tabanlı erişim denetimi (uygulanan RBAC) sahip kaynakları desteği 
* Geliştirici odaklı eylemleri seçilen kaynaklara belirli gösterir tümleşik eylem panel. Örneğin: oluşturulan sanal makinelerin Kaynak Yöneticisi yığınında kullanarak tanılama verilerini için sanal makineler vb. görüntülemek için uzaktan hata ayıklayıcı ekleyin.
* Yaygın olarak geliştirme ve test sırasında gereken Geliştirici odaklı özellikleri gösteren tümleşik özellikleri paneli 
* (Araç çubuğunda ayarlar komutunu kullanın) kaynakları numaralandırılırken kullanılacak hesap hızlı geçiş yapma 
* (Araç çubuğunda ayarlar komutunu kullanın) kaynakları numaralandırılırken kullanmak için abonelikler filtreleme 
* Azure portalına kaynaklarının yönetimi ve kaynak grupları için ayrıntılı bağlantılar 

### <a name="azure-resource-manager-tools"></a>Azure Kaynak Yöneticisi Araçları
Azure Kaynak Yöneticisi Araçları, rol tabanlı erişim denetimi (RBAC) ve yeni abonelik türleri ile çalışmak için güncelleştirilmiştir.  Bu değişikliklerle dağıtımı sırasında yapılarını depolamak için Klasik depolama yanı sıra yeni depolama hesapları kullanma yeteneğini içerir.  

Bir Azure kaynak grubu projesi SDK'ın önceki bir sürümünden SDK 2.7 ile kullanıyorsanız, yeni bir dağıtım komut dosyası yerine Klasik depolama yeni bir depolama hesabı kullanarak dağıtmak için gereklidir.  Yeni komut dosyası eklemek için projenize değişiklikler yapılmadan önce sizden istenir.  Eski kod yeniden adlandırılacak ve el ile yeni komut dosyası için herhangi bir değişiklik yapmanız gerekecektir.

### <a name="storage-explorer-tools"></a>Depolama Gezgini araçları
* Ek Bloblarını görüntüleme desteği. Daha fazla bilgisini [bu blog gönderisine](http://blogs.msdn.com/b/windowsazurestorage/archive/2015/04/13/introducing-azure-storage-append-blob.aspx). 
* Premium depolama hesapları Sunucu Gezgini üzerinden görüntüleme desteği. Premium depolama hesapları için desteklenen tek tür oldukları gibi Sunucu Gezgini yalnızca premium storage hesapları sayfa bloblarını görüntülenir.

### <a name="azure-data-factory-tools-for-visual-studio"></a>Visual Studio için Azure Data Factory araçları
Giriş **Azure Data Factory Araçları** Visual Studio için. Etkinleştirilmiş özellikler aşağıda verilmiştir. Bkz: [bu blog](http://go.microsoft.com/fwlink/?LinkId=617530) daha fazla bilgi için.

* **Şablona dayalı geliştirme**: seçin kullanım ortası temel şablonları, veri taşıma şablonları veya uçtan uca veri tümleştirme çözümünü dağıtmak ve hızlı bir şekilde Data Factory ile uygulamalı başlamak için veri işleme şablonları. 
* **Geliştirme ve Data Factory varlıklarını dağıtmak için Çözüm Gezgini ile tümleştirme**: oluşturmak ve ardışık düzen ve ilgili varlık Visual Studio projeleri olarak dağıtın. 
* **Geliştirme sırasında visual etkileşim için diyagram görünümü ile tümleştirme**: görsel olarak ardışık düzen ve yardımcı diyagramı görünümünden veri kümeleriyle yazar. 
* **Gözatma ve zaten dağıtılmış varlıklar etkileşim için Sunucu Gezgini ile tümleştirme**: Dağıtılmış Gözat veri fabrikaları ve karşılık gelen varlıklar için Sunucu Gezgini yararlanın. Dağıtılan data factory veya herhangi bir varlığa (ardışık düzen, bağlantılı hizmet, veri kümeleri) projenize içeri aktarın. 
* **JSON şema doğrulama ve zengin IntelliSense ile düzenleme**: verimli bir şekilde yapılandırın ve Data Factory varlıklarını zengin IntelliSense ve şema doğrulama ile JSON belgelerinin Düzenle 
* **Çoklu ortam yayımlama**: yayımlama ardışık düzen geliştirme, test veya üretim ortamı için her ortam için ayrı yapılandırma dosyası oluşturarak yazıldı.
* **Pig, Hive ve .net tabanlı veri işleme Destek**: Pig ve Data Factory projesindeki Hive betikler için destek. .Net yönetmek için C# projesine başvurma desteği etkinlik.

## <a name="azure-sdk-for-net-271"></a>.NET 2.7.1 İçin Azure SDK
Aşağıdaki bölümde .NET 2.7.1 yayın için Azure SDK'sı ile kullanıma sunulan güncelleştirmeleri içerir.

### <a name="hdinsight-tools"></a>Hdınsight araçları
Daha ayrıntılı Hdınsight araçları güncelleştirmeleri hakkında açıklama için bkz: [bu blog](http://go.microsoft.com/fwlink/?LinkId=623831).

* Hive işi işleci görünümü (yeni bir özellik)
  
    Hive sorgunuzu anlamanıza yardımcı olması için daha iyi ve Hive işleci görünümü özelliği eklenmiştir. Bir köşe içindeki tüm işleçleri görmek için iş grafiği köşelerinin çift tıklayın. Belirli bir işlecin daha fazla ayrıntı görüntülemek için işleç gelin.
* Hive hata işaret (yeni bir özellik)
  
    Dilbilgisi hataları görüntülemek etkinleştirmek için hemen Hive hata işaret özelliği eklenmiştir. Ayrıca, hata iletileri Gelişmiş ve ayrıntılı dilbilgisi hataları hemen şimdi görebilirsiniz (Bu sürümde kadar kümeye bir Hive betiği'na gönderin ve hata iletisi hakkında ayrıntıları alınıyor önce bir süre bekleyin gerekiyordu).  
* Storm topolojisini grafik (yeni bir özellik)
  
    Topolojiniz beklendiği gibi çalışıp çalışmadığını görmek istediğinizde, görselleştirme çok önemlidir. Bu sürümde Storm grafiklerde görselleştirme eklendi. Topolojiniz için önemli ölçümleri görselleştirebilirsiniz (örneğin, bir renk hava durumu belirli bir Cıvata "meşgul" olmadığını gösterir). Çift daha fazla ayrıntı görüntülemek için Cıvata/Spout tıklatabilirsiniz.
* Azure Portalı'nda (hata düzeltmesi) oluşturulan Hdınsight kümeleri için destek
  
    Visual Studio artık görüntülemek ve küme burada oluşturulmuş olursa olsun tüm Hdınsight kümeleri için iş göndermek için de kullanabilirsiniz.
* Daha fazla IntelliSense desteği & daha hızlı Hive meta veri yükleme (Geliştirme)
  
    Daha fazla kullanıcı dostu önerileri ekleyerek biz IntelliSense iyileştirilmiştir. Sorgunuzu daha kolay yazmak için örneğin, tablo diğer şimdi de IntelliSense içinde önerilebilir. Ayrıca, biz yalnızca tüm veritabanları, tablolar ve Hive meta depo sütunlarının listelemek için birkaç saniye sürer şekilde yüklenirken kovan meta iyileştirilmiştir.

Daha ayrıntılı Hdınsight araçları güncelleştirmeleri hakkında açıklama için bkz: [bu blog](http://go.microsoft.com/fwlink/?LinkId=623831).

### <a name="improvements-in-visual-studio-2013"></a>Visual Studio 2013'te geliştirmeleri
* Azure SDK'sı 2.7.1 Azure hesaplarına ve aboneliklerine rol tabanlı erişim denetimi, bulut çözüm sağlayıcıları ve Dreamspark aracılığıyla erişmek Visual Studio 2013 sağlar.
* Azure SDK'sı ile 2.7.1, yeni Cloud Explorer araç penceresi de Visual Studio 2013'te kullanıma sunulmuştur.

### <a name="known-issues"></a>Bilinen sorunlar
Visual Studio Community 2013 için Azure SDK 2.6 veya 2.7.1 bir olmayan - İngilizce işletim sisteminde yükleme Visual Studio'nun İngilizce ve İngilizce olmayan kaynakları eşleşmeyen bir uyarı görüntüler. Bu uyarı güvenli bir şekilde kapatıldığında. Yalnızca makine Visual Studio Community 2013 önceki bir yüklemesini içermiyor ve bir harici - İngilizce işletim sisteminde SDK'sını yükleme ortaya çıkar. Uyarı dil paketi için Visual Studio RTM kaynakları uyguladıktan sonra ancak güncelleştirme 4 uygulanmadan önce gösterilir. Uyarı durdurulduğundan, devam etmek ve dil paketi içeriği güncelleştirme 4 sürümünü uygulama tamamlamak dil paketi izin verir.

LightSwitch projeleri bu sürümle uyumlu değildir. Bu sorunu sonraki SDK sürümüyle çözümlenir.

## <a name="also-see"></a>Ayrıca bkz.
[Azure SDK'sı 2.7.1 duyuru post](http://go.microsoft.com/fwlink/?LinkId=623850)

[Azure SDK 2.7 duyuru post](https://azure.microsoft.com/blog/2015/07/20/announcing-the-azure-sdk-2-7-for-net/)

[Destek ve .NET ve API'ler için devre dışı bırakma bilgi Azure SDK'sı](https://msdn.microsoft.com/library/azure/dn479282.aspx/)

