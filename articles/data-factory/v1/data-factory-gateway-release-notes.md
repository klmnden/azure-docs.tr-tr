---
title: "Veri Yönetimi ağ geçidi için sürüm notları | Microsoft Docs"
description: "Veri Yönetimi ağ geçidi yazı sürüm notları"
services: data-factory
author: nabhishek
manager: jhubbard
editor: monicar
ms.assetid: 14762e82-76d9-41c4-ba9f-14a54da29c36
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/15/2017
ms.author: abnarain
robots: noindex
ms.openlocfilehash: 3cc96b22b45e5c741991b11e1bbee758a569bed9
ms.sourcegitcommit: d41d9049625a7c9fc186ef721b8df4feeb28215f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/02/2017
---
# <a name="release-notes-for-data-management-gateway"></a>Veri Yönetimi Ağ Geçidi için sürüm notları
> [!NOTE]
> Bu makale, Data Factory’nin genel kullanıma açık olan (GA) 1. sürümü için geçerlidir. Önizlemede değil, Data Factory hizmetinin 2 sürümünü kullanıyorsanız bkz [V2 tümleştirmesi çalışma zamanı'kendi kendini barındıran](../create-self-hosted-integration-runtime.md).

Modern veri tümleştirme sorunları için ve bulut şirket içi veri taşımak için biridir. Data Factory veri yönetimi karma veri taşıma etkinleştirmek için şirket içi yükleyebilmek için bir aracı olan ağ geçidi ile tümleştirme sağlar.

Veri Yönetimi ağ geçidi ve nasıl kullanılacağını hakkında ayrıntılı bilgi için aşağıdaki makalelere bakın:

*  [Veri Yönetimi Ağ Geçidi](data-factory-data-management-gateway.md)
*  [Şirket içi arasında veri taşıyabilir ve Azure Data Factory kullanarak bulut](data-factory-move-data-between-onprem-and-cloud.md)


## <a name="current-version"></a>GEÇERLİ SÜRÜM 
Biz artık burada sürüm notları tutun. En son sürüm notları almak [burada](https://go.microsoft.com/fwlink/?linkid=853077)




## <a name="earlier-versions"></a>Önceki sürümleri
## <a name="21063477"></a>2.10.6347.7
### <a name="enhancements-"></a>Geliştirmeler-
- DNS girişlerini uygulamaları güvenilir listeye almayı yerine beyaz liste hizmet veri yolu için tüm Azure IP adresleri, Güvenlik Duvarı (gerekirse) ekleyebilirsiniz. Azure Portal'da ilgili DNS girişi bulabilirsiniz (Data Factory -> 'Geliştir ve Dağıt' -> 'Ağ geçidi' -> (JSON içinde) "serviceUrls"
- HDFS bağlayıcı kendinden imzalı bir ortak sertifika SSL Doğrulamayı atla izin vererek olarak destekler.
- Sabit: Ağ geçidi çevrimdışı (nedeniyle saat eğriltme) güncelleştirme sorun


## <a name="2963132"></a>2.9.6313.2
### <a name="enhancements-"></a>Geliştirmeler-
-   DNS girişlerini uygulamaları güvenilir listeye almayı yerine hizmet veri yolu beyaz liste ile tüm Azure IP adresleri, Güvenlik Duvarı (gerekirse) ekleyebilirsiniz. Daha fazla ayrıntı burada.
-   Tek bir blok/gruptan veri Kopyala 4.75 blok blobu, desteklenen en büyük boyut olan TB'ye kadar blob artık kullanabilirsiniz. (önceki sınırı 195 GB'den okunurdu).
-   Sabit: Kopyalama etkinliği sırasında birkaç küçük dosyalar unzipping çalışırken bellek sorununu dışı.
-   Sabit: Dizini benzersizlik özelliği ile bir şirket içi SQL sunucusuna belge DB'den kopyalama sırasında aralığı sorunu dışında.
-   Sabit: SQL temizleme betiğini Kopyalama Sihirbazı'ndan şirket içi SQL Server ile çalışmaz.
-   Sabit: Sütun adı alanı sonunda kopyalama etkinliği çalışmaz.

## <a name="28662833"></a>2.8.66283.3
### <a name="enhancements-"></a>Geliştirmeler-
- Sabit: ağ geçidi makinenin yeniden başlatılmasını kimlik bilgileri eksik olan sorun.
- Sabit: Kayıt sırasında ağ geçidi ile ilgili sorun geri yedekleme dosyası kullanarak.


## <a name="2762401"></a>2.7.6240.1
### <a name="enhancements-"></a>Geliştirmeler-
- Sabit: Kaynak olarak Oracle arasında ondalık null değer yanlış okuyun.

## <a name="2661922"></a>2.6.6192.2
### <a name="whats-new"></a>Yenilikler
- Müşteri geri bildirim deneyimi kaydetme ağ geçidinde sağlayabilir.
- Yeni bir sıkıştırma biçimi destekler: ZIP (Deflate)

### <a name="enhancements-"></a>Geliştirmeler-
- Oracle havuz, HDFS kaynak için performans geliştirmesi.
- Ağ geçidi işleme kapasitesi paralel hata düzeltmesi için ağ geçidi otomatik güncelleştirme.


## <a name="2561641"></a>2.5.6164.1
### <a name="enhancements"></a>Geliştirmeleri
- Geliştirilmiş ve daha sağlam ağ geçidi kayıt deneyimi kaydı yapar ağ geçidi kayıt işlemi sırasında ilerleme durumunu izleyebilirsiniz artık-deneyimi daha iyi yanıt.
- Bu güncelleştirme ile ağ geçidi yedekleme dosyası yoksa bile gelişme ağ geçidi geri yükleme işlem, ağ geçidi kurtarmaya devam edebilirsiniz. Bu bağlantılı hizmet kimlik bilgilerini Portalı'nda sıfırlamanız gerekir.
- Hata düzeltmesi.

## <a name="2461511"></a>2.4.6151.1

### <a name="whats-new"></a>Yenilikler

- Şimdi veri kaynağı kimlik bilgileri yerel olarak depolayabilirsiniz. Kimlik bilgileri şifrelenir. Veri kaynağı kimlik bilgileri kurtarılabilir ve varolan ağ geçidi'nden, tüm şirket içi aktarılabilir yedekleme dosyasını kullanarak geri.

### <a name="enhancements-"></a>Geliştirmeler-

- Geliştirilmiş ve daha sağlam ağ geçidi kayıt deneyimi.
- Kopyalama Sihirbazı'nı metin biçiminde quotechar özelliğine yapılandırmasının otomatik algılama desteği ve genel biçimi algılama doğruluğunu artırmak.

## <a name="2361002"></a>2.3.6100.2

- Metin dosyaları kopyalama Sihirbazı'nda firstRowAsHeader ve SkipLineCount otomatik algılama şirket içi dosya sistemi ve HDFS'i destekler.
- Ağ geçidi ile Service Bus arasındaki ağ bağlantısının kararlılığını geliştirmek
- Birkaç hata düzeltmeleri


## <a name="2260721"></a>2.2.6072.1

*  Ağ geçidi Yapılandırma Yöneticisi'ni kullanarak ağ geçidi için HTTP proxy ayarını destekler. Yapılandırılmışsa, Azure Blob, Azure Table, Azure Data Lake ve belge DB HTTP proxy üzerinden erişilir.
*  TextFormat için başlangıç/bitiş Azure Blob, Azure Data Lake Store, veri kopyalama işlemi sırasında işleme destekler üstbilgisi şirket içi dosya sistemi ve şirket içi HDFS.
*  Veri ekleme Blob ve sayfa Blob zaten desteklenen blok blobu yanı sıra kopyalamayı destekler.
*  Yeni bir ağ geçidi durumu tanıtır **çevrimiçi (sınırlı)**, ağ geçidi ana işlevselliğini Kopyalama Sihirbazı'nı etkileşimli işlem desteği dışında çalıştığını gösterir.
*  Kayıt anahtarı kullanarak ağ geçidi kaydı sağlamlık geliştirir.

## <a name="216040"></a>2.1.6040.

*  DB2 sürücüsü ağ geçidi yükleme paketini artık dahil edilmiştir. Ayrı olarak yüklemeniz gerekmez.
*  DB2 sürücüsü artık destekliyor z/OS ve DB2 için t (AS / 400) zaten desteklenen platformlar (Linux, Unix ve Windows) ile birlikte.
*  Azure Cosmos DB şirket içi veri depoları için bir kaynak veya hedef olarak kullanılmasını destekler
*  Blob depolama zaten desteklenen genel amaçlı depolama hesabı yanı sıra veri kopyalama/soğuk ve hot için destekler.
*  Şirket içi SQL Server'a uzaktan oturum açma ayrıcalıklarına sahip ağ geçidi üzerinden bağlanmanıza olanak sağlar.  

## <a name="2060131"></a>2.0.6013.1

*  El ile yükleme sırasında bir ağ geçidi tarafından kullanılacak dil/kültür seçebilirsiniz.

*  Ağ geçidi beklendiği gibi çalışmaz, son yedi gün ağ geçidi günlüklerini sorun giderme kolaylaştırmak üzere Microsoft'a göndermeyi seçebilirsiniz. Ağ geçidi bulut hizmetine bağlı değilse, kaydetme ve ağ geçidi günlüklerini arşivlemek seçebilirsiniz.  

*  Ağ geçidi Yapılandırma Yöneticisi için kullanıcı arabirimi iyileştirmeleri:

    *  Ağ geçidi durumu daha görünür giriş sekmesinde yapın.

    *  Yeniden düzenlenen ve Basitleştirilmiş denetler.

    *  Bir depolama kullanarak veri kopyalayabilirsiniz [Kodsuz kopya önizleme aracını](data-factory-copy-data-wizard-tutorial.md). Bkz: [hazırlanan kopyalama](data-factory-copy-activity-performance.md#staged-copy) bu hakkındaki ayrıntılar için özelliklere genel.
*  Veri Yönetimi ağ geçidi giriş verileri için bir şirket içi SQL Server veritabanından doğrudan Azure Machine Learning kullanabilirsiniz.

*  Performans iyileştirmeleri

    * Şema/Önizleme SQL sunucusuna karşı Kodsuz kopyalama Önizleme aracında görüntüleme performansı.

## <a name="11259531"></a>1.12.5953.1

*  Hata düzeltmeleri

## <a name="11159181"></a>1.11.5918.1

*  Ağ geçidi olay günlüğünün en büyük boyutu 1 MB'tan 40 MB çıkarılmıştır.

*  Ağ geçidi otomatik güncelleştirme sırasında yeniden başlatma gerektiğinde bir uyarı iletişim kutusu görüntülenir. Hemen sonra veya daha sonra yeniden başlatmayı seçebilirsiniz.

*  Otomatik güncelleştirme başarısız olursa, otomatik güncelleştirme en fazla üç kez konumundaki ağ geçidi yükleyicisi yeniden dener.

*  Performans iyileştirmeleri

    * Şirket içi sunucudan Kodsuz kopyalama senaryosu büyük tabloları yüklenmesi için performansı.

*  Hata düzeltmeleri

## <a name="11058921"></a>1.10.5892.1

*  Performans iyileştirmeleri

*  Hata düzeltmeleri

## <a name="1958652"></a>1.9.5865.2

*  Sıfır dokunma otomatik güncelleştirme özelliği
*  Ağ geçidi Durum göstergeleri ile yeni Tepsi simgesi
*  "Şimdi güncelleştirme" özelliği istemci
*  Güncelleştirme zamanlaması saat ayarlayabilme
*  Otomatik güncelleştirmeyi Aç/Kapat geçiş için PowerShell Betiği
*  JSON biçimi desteği  
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

*  Configuration Manager Destek Tanılama araçları
*  Tablo sütunları tablo veri kaynakları için Azure Data Factory için destek.
*  Azure Data Factory SQL DW desteği
*  Azure veri fabrikası için BlobSource ve FileSource Reclusive desteği
*  Destek CopyBehavior – MergeFiles, PreserveHierarchy ve BlobSink ve Azure veri fabrikası için ikili kopyayla FileSink FlattenHierarchy
*  Kopya etkinliği Azure Data Factory için ilerleme durumu raporlama desteği
*  Azure veri fabrikası için destek veri kaynağı bağlantısı doğrulama
*  Hata düzeltmeleri

### <a name="1656721"></a>1.6.5672.1

*  ODBC veri kaynağı için tablo adı için Azure Data Factory destek
*  Performans iyileştirmeleri
*  Hata düzeltmeleri

### <a name="1656581"></a>1.6.5658.1

*  Destek dosyası havuz için Azure veri fabrikası
*  Hiyerarşi için Azure Data Factory ikili kopyasında koruma desteği
*  Azure veri fabrikası için kopyalama etkinliği benzersizlik desteği
*  Hata düzeltmeleri

### <a name="1656401"></a>1.6.5640.1

*  3 daha fazla veri kaynakları (ODBC, OData, HDFS) Azure Data Factory için destek
*  Azure Data Factory için csv ayrıştırıcısını tırnak işareti karakteri desteği
*  Sıkıştırma desteği (Bzıp2)
*  Hata düzeltmeleri

### <a name="1556121"></a>1.5.5612.1

*  Azure Data Factory (MySQL, PostgreSQL, DB2, Teradata ve Sybase) için beş ilişkisel veritabanlarını destekler
*  Sıkıştırma desteği (Gzip ve Deflate)
*  Performans iyileştirmeleri
*  Hata düzeltmeleri

### <a name="1455491"></a>1.4.5549.1

*  Azure Data Factory için Oracle veri kaynağı destek ekleme
*  Performans iyileştirmeleri
*  Hata düzeltmeleri

### <a name="1454921"></a>1.4.5492.1

*  Microsoft Azure veri fabrikası ve Office 365 Power BI hizmetlerini destekleyen birleşik ikili
*  Yapılandırma kullanıcı Arabirimi ve kayıt işlemini daraltın
*  Azure Data Factory – Azure giriş ve çıkış desteklemek için SQL Server veri kaynağı

### <a name="1253031"></a>1.2.5303.1

*  Daha fazla uzun süren veri kaynağı bağlantılarını desteklemek için zaman aşımı sorunu düzeltin.

### <a name="1155268"></a>1.1.5526.8

*  .NET Framework 4.5.1 Kurulum sırasında bir önkoşul olarak gereklidir.

### <a name="1051442"></a>1.0.5144.2

*  Azure Data Factory senaryoları etkileyen değişiklik yok.
