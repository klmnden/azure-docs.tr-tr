---
title: .NET 2.7 ve .NET 2.7.1 için Azure SDK sürüm notları
description: .NET 2.7 ve .NET 2.7.1 için Azure SDK sürüm notları
services: app-service\web
documentationcenter: .net
author: chrissfanos
editor: ''
ms.assetid: 877d070a-9bd5-49b3-8fac-6bb5f65c3554
ms.service: app-service
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 02/24/2017
ms.author: juliako
ms.openlocfilehash: 4c5a4dfcdde91d1bd0c2728ff9d071d4c2f73f0e
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2018
ms.locfileid: "52969779"
---
# <a name="azure-sdk-for-net-27-and-net-271-release-notes"></a>.NET 2.7 ve .NET 2.7.1 için Azure SDK sürüm notları
## <a name="overview"></a>Genel Bakış
Bu belge, yayın .NET 2.7 için Azure SDK'sı sürüm notlarını içermektedir. 

Belgenin de içermesi sürüm notları için Azure SDK'sı .NET 2.7.1 sürüm için.

Azure SDK 2.7, Visual Studio 2015 ve Visual Studio 2013'te yalnızca desteklenir. [Azure SDK 2.6](https://azure.microsoft.com/downloads/) olan son SDK'sı Visual Studio 2012 için desteklenir.

Bu sürüm hakkında ayrıntılı bilgi için bkz. [Azure SDK 2.7 Duyurusu post](https://azure.microsoft.com/blog/2015/07/20/announcing-the-azure-sdk-2-7-for-net/) ve [Azure SDK 2.7.1 Duyurusu post](https://go.microsoft.com/fwlink/?LinkId=623850).

## <a name="azure-sdk-for-net-27"></a>.NET 2.7 için Azure SDK’sını
### <a name="sign-in-improvements-for-visual-studio-2015"></a>Visual Studio 2015 için geliştirmeler oturum açma
Visual Studio 2015 için Azure SDK 2.7, Visual Studio 2015'te yeni kimlik yönetimi özellikleri destekler.  Bu, Azure ile rol tabanlı erişim denetimi, bulut çözümü sağlayıcıları, DreamSpark ve diğer hesap ve abonelik türleriyle erişen hesaplar için destek içerir.

Azure SDK 2.7 ile eklenen oturum geliştirmeler yalnızca Visual Studio 2015'te kullanılabilir. Visual Studio 2013 desteği, Azure SDK 2.7.1 dahildir.

### <a name="mobile-sdk"></a>Mobil SDK'sı
Güncelleştirilmiş **Mobile Apps** en yeni yansıtacak şekilde şablonları [NuGet paketini](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server/) ve Kurulum işlemi.

### <a name="service-bus"></a>Service Bus
Genel hata düzeltmeleri ve geliştirmeleri. Güncelleştirmeleri ve özellikleri hakkında daha fazla ayrıntı için lütfen en son sürüm notları için başvuru [Service Bus NuGet](https://www.nuget.org/packages/WindowsAzure.ServiceBus/).

### <a name="hdinsight-tools"></a>HDInsight araçları
Bu sürümde aşağıdaki güncelleştirmeler yapılmıştır. Bu güncelleştirmeler için Önizleme aşamasındadır. Daha fazla bilgi için [bu blog](https://go.microsoft.com/fwlink/?LinkId=619108).

* Tez işlerinde Hive için Hive grafikleri
* Tüm Hive DML IntelliSense desteğimizi
* Pig şablonu desteği
* Azure Hizmetleri için Storm şablonları

#### <a name="breaking-changes"></a>Yeni değişiklikler
* Eski **Storm** Araçları'nın bu sürümünü kullanırken, proje yükseltilmelidir. Daha fazla bilgi için [bu blog](https://go.microsoft.com/fwlink/?LinkId=619108).
* Visual Studio Web Express'in artık desteklenmiyor. Daha fazla bilgi için [bu blog](https://go.microsoft.com/fwlink/?LinkId=619108).

### <a name="azure-app-service-tools"></a>Azure uygulama hizmeti araçları
Bu sürümde, Web Araçları uzantıları için aşağıdaki güncelleştirmeler yapılmıştır. Daha fazla bilgi için [bu](https://azure.microsoft.com/blog/2015/07/20/announcing-the-azure-sdk-2-7-for-net/) blogu. 

* Eklenen DreamSpark hesaplar için destek
* Yeni Azure Resource Management API'leri desteklemek Azure Araçları için tam değişiklik yapıldığında
* Azure App Service için destek eklendi [Cloud Explorer](#cloud_explorer)

#### <a name="known-issues"></a>Bilinen sorunlar
Web uygulaması dağıtım yuvası düğümleri altında Sunucu Gezgininde yuvaları görünmez ve bu Web uygulaması dağıtım yuvası alt düğümleri altında Cloud Explorer yükleme. Bu sorun Çözüldü ve sonraki SDK sürüm için hazır. 

### <a name="cloud_explorer"></a>Visual Studio 2015 için cloud Explorer
Azure SDK 2.7, bu sayede, Azure kaynaklarınızı görüntüleyin, özelliklerini inceleme ve Visual Studio temel Geliştirici eylemler gerçekleştirmek Visual Studio 2015 için Cloud Explorer içerir. 

Cloud explorer aşağıdakileri destekler:

* Azure kaynaklarınızı kaynak grubu ve kaynak görünümlerini 
* Kaynak (kaynak türü Görünümü'nde kullanılabilen) adına göre ara
* Abonelikler ve rol tabanlı erişim denetimi (uygulanan RBAC) olan kaynaklar için destek 
* Geliştirici odaklı eylemleri seçili kaynaklara belirli gösterir tümleşik eylem bölmesi. Örneğin: oluşturulan sanal makineler Resource Manager yığını'nı kullanarak tanılama verilerini için sanal makineler vb. görüntülemek için uzaktan hata ayıklayıcı ekleyin.
* Geliştirme ve test sırasında yaygın olarak gereken Geliştirici odaklı özellikleri gösterir özellikler panelini tümleşik 
* (Araç çubuğunda ayarlar komutunu kullanın) kaynaklarını numaralandırma kullanılacak hesabı, hızlı geçiş 
* (Araç çubuğunda ayarlar komutunu kullanın) kaynaklarını numaralandırma sırasında kullanılacak aboneliklerini filtreleme 
* Azure Portal'da kaynaklarını ve kaynak gruplarını, yönetimi için ayrıntılı bağlantılar 

### <a name="azure-resource-manager-tools"></a>Azure Resource Manager araçları
Azure Resource Manager araçları, rol tabanlı erişim denetimi (RBAC) ve yeni abonelik türleriyle çalışmak için güncelleştirilmiştir.  Bu değişikliklerle, dağıtım sırasında yapıtları depolamak için Klasik depolamaya ek olarak, yeni depolama hesapları kullanma yeteneğini içerir.  

SDK 2.7 ile bir Azure kaynak grubu projesi SDK'ın önceki bir sürümden kullanıyorsanız, yeni bir dağıtım betiği Klasik depolama yerine yeni bir depolama hesabı kullanarak dağıtmak için gereklidir.  Yeni komut dosyası eklemek için yapılan değişiklikleri projenize önce sizden istenir.  Eski kodun yeniden adlandırılacak ve el ile yeni betik herhangi bir değişiklik yapmanız gerekecektir.

### <a name="storage-explorer-tools"></a>Depolama Gezgini araçları
* Ekleme Blobları görüntüleme desteği. Daha fazla bilgi [bu blog gönderisini](https://blogs.msdn.com/b/windowsazurestorage/archive/2015/04/13/introducing-azure-storage-append-blob.aspx). 
* Premium depolama hesapları Sunucu Gezgini aracılığıyla görüntüleme desteği. Premium depolama hesapları için desteklenen tek tür oldukları gibi Sunucu Gezgini yalnızca sayfa blobları için premium depolama hesapları görüntülenir.

### <a name="azure-data-factory-tools-for-visual-studio"></a>Visual Studio için Azure Data Factory araçları
Karşınızda **Azure Data Factory Araçları** Visual Studio için. Etkin özellikler aşağıda verilmiştir. Bkz: [bu blog](https://go.microsoft.com/fwlink/?LinkId=617530) daha fazla bilgi için.

* **Şablon tabanlı yazma**: Select kullanmak büyük küçük harfleri tabanlı şablonları, veri taşıma şablonları veya veri işleme şablonlarını kullanarak uçtan uca veri tümleştirme çözümünü dağıtmak ve uygulamalı hızla Data Factory ile çalışmaya başlama. 
* **Geliştirme ve Data Factory varlıklarını dağıtmak için Çözüm Gezgini ile tümleştirme**: işlem hatları ve ilgili varlıklar olarak Visual Studio projeleri dağıtma & Oluştur. 
* **Geliştirme sırasında visual etkileşimi için diyagram görünümü ile tümleştirme**: görsel olarak işlem hatları ve ürettiği diyagramı görünümünden veri kümeleriyle yazar. 
* **Göz atma ve zaten dağıtılmış varlıklarıyla etkileşimi için Sunucu Gezgini ile tümleştirme**: göz atma zaten Dağıtılmış veri fabrikaları ve karşılık gelen varlıklar için sunucu Gezgini'ni yararlanın. Dağıtılan veri fabrikası veya herhangi bir varlık (işlem hattı, bağlı hizmet, veri kümeleri) projenize içeri aktarın. 
* **JSON şema doğrulama ve zengin IntelliSense ile düzenleme**: verimli bir şekilde yapılandırın ve zengin IntelliSense ve şema doğrulama Data Factory varlıkları JSON belgelerinin Düzenle 
* **Çok ortamlı yayımlama**: yayımlama geliştirme, test veya üretim ortamı için işlem hatları her ortam için ayrı yapılandırma dosyalarına oluşturarak yazıldı.
* **Pig, Hive ve .net tabanlı veri işleme desteği**: Pig ve Hive betiklerini Data Factory projesindeki desteği. Başvuru için destek C# .Net yönetmek için proje etkinliği.

## <a name="azure-sdk-for-net-271"></a>.NET 2.7.1 İçin Azure SDK
Aşağıdaki bölümde, yayın .NET 2.7.1 için Azure SDK'sı ile sunulan güncelleştirmeler içerir.

### <a name="hdinsight-tools"></a>HDInsight araçları
Daha ayrıntılı bir HDInsight araçları güncelleştirmeleri hakkında daha fazla açıklama için bkz: [bu blog](https://go.microsoft.com/fwlink/?LinkId=623831).

* Hive işi işleci görünümü (yeni bir özellik)
  
    Hive sorgunuzu anlamanıza yardımcı olması için daha iyi Hive işleci görünümü özelliği eklenmiştir. Köşe içindeki tüm işleçleri görmek için iş grafiğinin köşeleri çift tıklayın. Belirli bir işleç, işleç üzerine geldiğinizde daha fazla ayrıntı görüntülemek için.
* Hive hata işaret (yeni bir özellik)
  
    Dil bilgisi hataları görüntülemek sağlamak için anında, Hive hata işaret özelliği eklenmiştir. Ayrıca, hata iletileri Gelişmiş ve ayrıntılı gramer hataları anında artık bkz (Bu sürüme kadar kümeye bir Hive betiği gönderin ve bir süre önce hata iletisiyle ilgili ayrıntıları alınırken bekleyin gerekiyordu).  
* Storm topoloji graf (yeni bir özellik)
  
    Topolojiniz beklendiği gibi çalışıp çalışmadığını görmek istediğinizde, görselleştirme çok önemlidir. Bu sürümde Storm grafik için görselleştirme ekledik. Topolojiniz için önemli olan ölçümleri görselleştirebilir miyim (örneğin, bir renk veya belirli bir Bolt hava durumu "meşgul" gösterir). Ayrıca çift daha fazla ayrıntı görüntülemek için Bolt/Spout tıklayabilirsiniz.
* Azure Portalı'nda (bir hata düzeltmesi) oluşturulmuş olan HDInsight kümeleri için destek
  
    Visual Studio artık görüntülemek ve küme oluşturulduğundan nerede olursa olsun tüm HDInsight kümeleri için iş göndermek için de kullanabilirsiniz.
* Daha fazla IntelliSense desteği ve daha hızlı Hive meta veri yüklenmesini (Geliştirme)
  
    Daha fazla kullanıcı dostu öneri ekleyerek IntelliSense geliştirildi. Sorgunuzu daha kolay yazabilirsiniz için örneğin, tablo diğer ad artık ayrıca IntelliSense içinde önerilebilir. Ayrıca, yalnızca tüm veritabanları, tablolar ve sütunlar, Hive meta veri deposu listelemek için birkaç saniye sürer şekilde yükleniyor Hive meta veri geliştirildi.

Daha ayrıntılı bir HDInsight araçları güncelleştirmeleri hakkında daha fazla açıklama için bkz: [bu blog](https://go.microsoft.com/fwlink/?LinkId=623831).

### <a name="improvements-in-visual-studio-2013"></a>Visual Studio 2013'teki geliştirmeler
* Visual Studio 2013 Azure hesaplar ve abonelikler rol tabanlı erişim denetimi, bulut çözüm sağlayıcıları ve Dreamspark aracılığıyla erişmek Azure SDK 2.7.1 sağlar.
* Azure SDK 2.7.1 ile yeni bulut Gezgini araç penceresi de Visual Studio 2013'te kullanıma sunulmuştur.

### <a name="known-issues"></a>Bilinen sorunlar
Azure SDK 2.6 veya 2.7.1 için Visual Studio Community 2013 bir İngilizce işletim sistemi olmayan üzerinde yüklenmesi Visual Studio'nun İngilizce ve İngilizce olmayan kaynakları eşleşmiyor olabilir, bir uyarı görüntüler. Bu uyarı güvenli bir şekilde kapatıldı. Ancak, makinede Visual Studio Community 2013'ün önceki bir yükleme içermiyor ve bir İngilizce işletim sistemi olmayan üzerinde SDK'sı yüklüyorsanız gerçekleşir. Uyarı dil paketi, Visual Studio RTM kaynakları uyguladıktan sonra ancak güncelleştirme 4 uygulanmadan önce gösterilir. Uyarı kapatılıyor, devam etmek ve dil paketi içeriği güncelleştirme 4 sürümünü uygulama tamamlamak dil paketi izin verir.

LightSwitch projeleri, bu sürümle uyumlu değildir. Bu sorun sonraki SDK sürümüyle çözülmüş olacaktır.

## <a name="also-see"></a>Ayrıca bkz:
[Azure SDK 2.7.1 Duyurusu sonrası](https://go.microsoft.com/fwlink/?LinkId=623850)

[Azure SDK 2.7 Duyurusu sonrası](https://azure.microsoft.com/blog/2015/07/20/announcing-the-azure-sdk-2-7-for-net/)

[Destek ve kullanımdan kaldırma Azure SDK'sı ve bilgi için .NET API'leri](https://msdn.microsoft.com/library/azure/dn479282.aspx/)

