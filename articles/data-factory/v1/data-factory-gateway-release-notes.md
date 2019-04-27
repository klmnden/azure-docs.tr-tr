---
title: Veri Yönetimi ağ geçidi için sürüm notları | Microsoft Docs
description: Veri Yönetimi ağ geçidi yazı sürüm notları
services: data-factory
author: nabhishek
manager: craigg
ms.assetid: 14762e82-76d9-41c4-ba9f-14a54da29c36
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: abnarain
robots: noindex
ms.openlocfilehash: 865bfdae199bca7ebee888be527db239d34511d1
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60486473"
---
# <a name="release-notes-for-data-management-gateway"></a>Veri Yönetimi Ağ Geçidi için sürüm notları
> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Data Factory hizmetinin geçerli sürümünü kullanıyorsanız bkz [barındırılan tümleştirme çalışma zamanını V2'de](../create-self-hosted-integration-runtime.md).

Modern veri tümleştirme için sorunlarından biri, verileri şirket içinden buluta taşıma sağlamaktır. Veri fabrikası, veri yönetimi, karma veri taşıma olanağı şirket yükleyebileceğiniz bir aracı olan ağ geçidi ile tümleştirme sağlar.

Veri Yönetimi ağ geçidi ve nasıl kullanılacağı hakkında ayrıntılı bilgi için aşağıdaki makalelere bakın:

*  [Veri Yönetimi Ağ Geçidi](data-factory-data-management-gateway.md)
*  [Azure Data Factory kullanarak Bulut ve şirket içi arasında veri taşıma](data-factory-move-data-between-onprem-and-cloud.md)


## <a name="current-version"></a>GEÇERLİ SÜRÜM 
Daha fazla sürüm notlarını buradan saklarız. En son sürüm notlarını edinin [burada](https://go.microsoft.com/fwlink/?linkid=853077)




## <a name="earlier-versions"></a>Önceki sürümleri
## <a name="21063477"></a>2.10.6347.7
### <a name="enhancements-"></a>Geliştirmeleri-
- DNS girişlerini beyaz listeye ekleme yerine beyaz liste hizmet veri yolu için tüm Azure IP adresleri, Güvenlik Duvarı (gerekirse) ekleyebilirsiniz. Azure portalında ilgili DNS girişi bulabilirsiniz (Data Factory -> "Geliştir ve Dağıt" -> 'Ağ geçidi' -> (JSON içinde) "serviceUrls"
- HDFS bağlayıcı otomatik olarak imzalanan ortak sertifika SSL doğrulamasını Atla vererek olarak destekler.
- Düzeltildi: (Saat eğriltme) nedeniyle güncelleştirme sırasında çevrimdışı ağ geçidi ile sorun


## <a name="2963132"></a>2.9.6313.2
### <a name="enhancements-"></a>Geliştirmeleri-
-   DNS girişlerini beyaz listeye beyaz listeye ekleme yerine Service Bus tüm Azure IP adresleri, Güvenlik Duvarı (gerekirse) ekleyebilirsiniz. Daha ayrıntılı bilgiyi burada.
-   4,75 blok blobu desteklenen en büyük boyutu olan TB'a kadar kopyalama veri gönderip buralardan veri tek bir blok blob artık. (önceki sınır 195 GB idi).
-   Düzeltildi: Kopyalama etkinliği sırasında birkaç küçük dosyaları sıkıştırmayı açma sırasında bellek sorunu dışında.
-   Düzeltildi: Eşkuvvetlilik özelliği ile bir şirket içi SQL Server belge DB'den kopyalanırken aralığı sorunu dışında dizin.
-   Düzeltildi: SQL temizleme betiği Kopyalama Sihirbazı'ndan şirket içi SQL Server ile çalışmaz.
-   Düzeltildi: Kopyalama etkinliği, sonunda boşluk olan sütun adı çalışmaz.

## <a name="28662833"></a>2.8.66283.3
### <a name="enhancements-"></a>Geliştirmeleri-
- Düzeltildi: Ağ geçidi makinenin yeniden başlatılmasını kimlik bilgileri eksik olan sorunu.
- Düzeltildi: Kayıt sırasında ağ geçidi ile ilgili sorun, yedekleme dosyasını kullanarak geri yükleme.


## <a name="2762401"></a>2.7.6240.1
### <a name="enhancements-"></a>Geliştirmeleri-
- Düzeltildi: Yanlış okuma kaynağı olarak Oracle ondalık null değer.

## <a name="2661922"></a>2.6.6192.2
### <a name="whats-new"></a>Yenilikler
- Müşteri Deneyimi kaydetme ağ geçidinde geri bildirim sağlayabilirsiniz.
- Yeni bir sıkıştırma biçimini destekler: ZIP (Deflate)

### <a name="enhancements-"></a>Geliştirmeleri-
- Oracle havuz, HDFS kaynak için performans geliştirmesi.
- Ağ geçidi işleme kapasitesi paralel hata düzeltmesi için ağ geçidi otomatik güncelleştirme.


## <a name="2561641"></a>2.5.6164.1
### <a name="enhancements"></a>Geliştirmeler
- Daha güçlü ve Gelişmiş ağ geçidi kayıt deneyimi artık kayıt deneyimini daha hızlı hale getirir ağ geçidi kayıt işlemi sırasında ilerleme durumunu izleyebilirsiniz-.
- Bu güncelleştirme ile ağ geçidi yedekleme dosyası yoksa bile gelişme ağ geçidi geri işlem, ağ geçidi kurtarmaya devam edebilirsiniz. Bu Portalı'nda bağlı hizmet kimlik bilgilerini sıfırlamanız gerekir.
- Hata düzeltmesi.

## <a name="2461511"></a>2.4.6151.1

### <a name="whats-new"></a>Yenilikler

- Şimdi veri kaynağı kimlik bilgileri yerel olarak depolayabilirsiniz. Kimlik bilgileri şifrelenir. Veri kaynağı kimlik bilgileri, kurtarılabilir ve mevcut ağ geçidini, tüm şirket içi aktarılabilir yedekleme dosyasını kullanarak geri.

### <a name="enhancements-"></a>Geliştirmeleri-

- Ağ geçidi kayıt deneyimi geliştirildi ve daha güçlü.
- Kopyalama Sihirbazı'nı metin biçiminde otomatik algılama QuoteChar yapılandırma desteği ve genel biçimini algılama doğruluğunu artırmak.

## <a name="2361002"></a>2.3.6100.2

- FirstRowAsHeader ve SkipLineCount otomatik algılama için metin dosyaları kopyalama Sihirbazı'nı şirket içi dosya sistemi ve HDFS destekler.
- Service Bus ağ geçidi arasında ağ bağlantısı kararlılığını geliştirmek
- Bazı hata düzeltmeleri


## <a name="2260721"></a>2.2.6072.1

*  Ağ geçidi Yapılandırma Yöneticisi'ni kullanarak ağ geçidi için HTTP proxy ayarı destekler. Yapılandırılmışsa, Azure Blob, Azure tablo, Azure Data Lake ve Document DB, HTTP proxy üzerinden erişilir.
*  / İçin Azure Blob, Azure Data Lake Store, veri kopyalama sırasında TextFormat için işleme destekler üst bilgi şirket içinde dosya sistemi ve şirket içinde HDFS.
*  Ekleme Blob ve sayfa blobu zaten desteklenen blok blobu yanı sıra veri kopyalamayı destekler.
*  Yeni bir ağ geçidi durumu tanıtır **çevrimiçi (sınırlı)**, ağ geçidi ana işlevlerini Kopyalama Sihirbazı'nı etkileşimli işlem desteği dışında çalıştığını gösterir.
*  Kayıt anahtarını kullanarak ağ geçidi kaydı sağlamlığını geliştirir.

## <a name="216040"></a>2.1.6040.

*  DB2 sürücüsü artık ağ geçidi yükleme paketine dahildir. Ayrıca yüklemeniz gerekmez.
*  DB2 sürücüsü z/OS ve DB2 için artık desteklemektedir miyim (AS / 400) birlikte zaten desteklenen platformlar (Linux, Unix ve Windows).
*  Şirket içi veri depoları için bir kaynak veya hedef Azure Cosmos DB kullanmayı destekler
*  Veri kopyalama, BLOB'dan/soğuk/seyrek destekler birlikte zaten desteklenen genel amaçlı depolama hesabı depolama blob.
*  Uzak oturumu ayrıcalıkları ile ağ geçidi üzerinden şirket içi SQL Server bağlanmanıza olanak sağlar.  

## <a name="2060131"></a>2.0.6013.1

*  El ile yükleme sırasında bir ağ geçidi tarafından kullanılacak dili/kültürü seçebilirsiniz.

*  Ağ geçidi beklendiği gibi çalışmaz, ağ geçidi günlükleri son yedi gün sorun giderme işlemlerini kolaylaştırmak için Microsoft'a göndermek seçebilirsiniz. Ağ geçidi bulut hizmetine bağlı değilse, kaydetme ve ağ geçidi günlükleri arşiv seçebilirsiniz.  

*  Configuration manager ağ geçidi için kullanıcı arabirimi iyileştirmeleri:

    *  Ağ geçidi durumu daha görünür giriş sekmesinde yapın.

    *  Yeniden düzenlenen ve Basitleştirilmiş denetler.

    *  Verileri kullanarak bir depolama kopyalayabilirsiniz [kod gerektirmeyen kopyalama aracı](data-factory-copy-data-wizard-tutorial.md). Bkz: [hazırlanmış kopya](data-factory-copy-activity-performance.md#staged-copy) bu hakkındaki ayrıntılar için özellik genel.
*  Azure Machine Learning içine veri yönetimi ağ geçidi giriş verileri için bir şirket içi SQL Server veritabanından doğrudan kullanabilirsiniz.

*  Performans iyileştirmeleri

    * Kod gerektirmeyen kopyalama Aracı'nda SQL Server Önizlemedeki şema/görüntüleme performansı.

## <a name="11259531"></a>1.12.5953.1

*  Hata düzeltmeleri

## <a name="11159181"></a>1.11.5918.1

*  Ağ geçidi olay günlüğünün en büyük boyutu 1 MB'tan 40 MB olarak artırıldı.

*  Yeniden başlatma sırasında ağ geçidi otomatik güncelleştirme gerekmesi halinde bir uyarı iletişim kutusu görüntülenir. Hemen sonra veya daha sonra yeniden başlatmayı seçebilirsiniz.

*  Otomatik güncelleştirme başarısız olursa, ağ geçidi yükleyicisinin en fazla üç kez sırasında otomatik olarak güncelleştiriliyor yeniden dener.

*  Performans iyileştirmeleri

    * Şirket içi sunucudan kod gerektirmeyen kopyalama senaryosu büyük tablolar yükleme performansı.

*  Hata düzeltmeleri

## <a name="11058921"></a>1.10.5892.1

*  Performans iyileştirmeleri

*  Hata düzeltmeleri

## <a name="1958652"></a>1.9.5865.2

*  Sıfır dokunma otomatik güncelleştirme özelliği
*  Ağ geçidi durumu göstergeleri ile yeni Tepsi simgesi
*  "Şimdi güncelleştir" özelliği istemci
*  Güncelleştirme zamanlama süresi ayarlama olanağı
*  Otomatik güncelleştirme açık/kapalı durumu değiştirildiğinde yönelik PowerShell Betiği
*  JSON biçimi için destek  
*  Performans iyileştirmeleri
*  Hata düzeltmeleri

## <a name="1858221"></a>1.8.5822.1

*  Sorun giderme deneyimini geliştirmeye
*  Performans iyileştirmeleri
*  Hata düzeltmeleri

### <a name="1757951"></a>1.7.5795.1

*  Performans iyileştirmeleri
*  Hata düzeltmeleri

### <a name="1757641"></a>1.7.5764.1

*  Performans iyileştirmeleri
*  Hata düzeltmeleri

### <a name="1657351"></a>1.6.5735.1

*  Şirket içi HDFS kaynak/havuz desteği
*  Performans iyileştirmeleri
*  Hata düzeltmeleri

### <a name="1656961"></a>1.6.5696.1

*  Performans iyileştirmeleri
*  Hata düzeltmeleri

### <a name="1656761"></a>1.6.5676.1

*  Configuration Manager'ı tanılama araçları desteği
*  Azure Data Factory için tablo biçimindeki veri kaynakları için destek tablo sütunları
*  SQL DW, Azure Data Factory için destek
*  Azure Data Factory için BlobSource ve dosya kaynağına yazılamıyor Reclusive desteği
*  Azure Data Factory için BlobSink ve ikili kopya FileSink CopyBehavior – MergeFiles PreserveHierarchy ve FlattenHierarchy desteği
*  Kopyalama etkinliği Azure Data Factory için ilerleme raporlaması desteği
*  Azure Data Factory için destek veri kaynağı bağlantısı doğrulama
*  Hata düzeltmeleri

### <a name="1656721"></a>1.6.5672.1

*  Azure Data Factory için ODBC veri kaynağı için destek tablo adı
*  Performans iyileştirmeleri
*  Hata düzeltmeleri

### <a name="1656581"></a>1.6.5658.1

*  Azure Data Factory için destek dosyasını havuz
*  Hiyerarşi için Azure Data Factory ikili kopya koruma desteği
*  Azure Data Factory için kopyalama etkinliği Teklik desteği
*  Hata düzeltmeleri

### <a name="1656401"></a>1.6.5640.1

*  3 daha fazla veri kaynağına (ODBC, OData, HDFS) Azure Data Factory için destek
*  Azure Data Factory için csv ayrıştırıcısı çift tırnak karakteri desteği
*  Sıkıştırma desteğine (Bzıp2)
*  Hata düzeltmeleri

### <a name="1556121"></a>1.5.5612.1

*  (MySQL, PostgreSQL, DB2, Teradata ve Sybase) Azure Data Factory için beş ilişkisel veritabanı desteği
*  (GZIP ve Deflate) sıkıştırma desteği
*  Performans iyileştirmeleri
*  Hata düzeltmeleri

### <a name="1455491"></a>1.4.5549.1

*  Azure Data Factory için Oracle veri kaynağı desteği eklendi
*  Performans iyileştirmeleri
*  Hata düzeltmeleri

### <a name="1454921"></a>1.4.5492.1

*  Microsoft Azure Data Factory hem de Office 365 Power BI hizmetleri destekleyen birleşik ikili
*  Yapılandırma kullanıcı Arabirimi ve kayıt sürecini iyileştirin
*  Azure Data Factory – Azure giriş ve çıkış desteklemek için SQL Server veri kaynağı

### <a name="1253031"></a>1.2.5303.1

*  Daha fazla zaman alan bir veri kaynağı bağlantıları desteklemek için zaman aşımı sorunu düzeltildi.

### <a name="1155268"></a>1.1.5526.8

*  .NET Framework 4.5.1, Kurulum sırasında önkoşul olarak gerektirir.

### <a name="1051442"></a>1.0.5144.2

*  Azure Data Factory senaryoları etkileyen değişiklik yok.
