---
title: "Azure veri Kataloğu'ndaki Yenilikler | Microsoft Docs"
description: "Bu makalede, Azure veri Kataloğu'na eklenen yeni özelliklere genel bakış sağlar."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: 1201f8d4-6f26-4182-af3f-91e758a12303
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 10/01/2017
ms.author: maroche
ms.openlocfilehash: 2eab8ce96399c7d6da9b4afbccdfd8b836f0c9f3
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/03/2017
---
# <a name="whats-new-in-azure-data-catalog"></a>Azure veri Kataloğu'ndaki Yenilikler
Güncelleştirmeleri **Azure veri Kataloğu** düzenli olarak kullanıma sunulur. Bazı sürümler arka uç hizmeti özellikleri üzerine odaklanan gibi her sürüm yeni kullanıcı yönelik özellikler içerir. Bu sayfa Azure veri Kataloğu hizmetine eklenen yeni kullanıcı dönük özelliklerini vurgular.

## <a name="whats-new-for-september-2017"></a>Eylül 2017 yenilikler nelerdir? 
Eylül 2017'ten itibaren Azure veri kataloğu için aşağıdaki özellikleri eklenmiştir:

* Ayıklama desteği ilişki meta verileri DB2 veri kaynaklarından veri kaynağı kayıt aracını kullanarak ilgili tabloları kaydedilirken katılın.
* Veri kaynağı kayıt aracını kullanarak MongoDB sürüm 3.4 veri kaynaklarını kaydederek desteği.
* Tüm meta veriler tek bir işlemde kapsanan nesneleri için bir veritabanı veya diğer kapsayıcı veri Kataloğu'ndan kaldırırken silme desteği.
* 1. 000'etiketleri, iş terimler veya diğer arama kadar veri Kataloğu portalında bir arama daraltmayı ne zaman modellerini görüntüleme desteği.


## <a name="whats-new-for-august-2017"></a>Ağustos 2017 yenilikler nelerdir? 
Ağustos 2017'ten itibaren Azure veri kataloğu için aşağıdaki özellikleri eklenmiştir:

*   Yeni bir geliştirici örnek oluşturmak ve ilişki meta veri Kataloğu REST API'sini kullanarak yönetmek için kullanılabilir. *Veri kataloğuna ilişki bilgileri alma* örnek edinilebilir [veri Kataloğu kod örnekleri sayfası](https://azure.microsoft.com/resources/samples/?service=data-catalog&sort=0). 
* Ayıklama desteği ilişki meta veri Teradata veri kaynaklarından veri kaynağı kayıt aracını kullanarak ilgili tabloları kaydedilirken katılın.
* Veri kaynağı kayıt aracını kullanarak SQL Server veri kaynaklarını kaydedilirken SQL Server Tablo Değerli Fonksiyon (TVF) nesneleri için destek.
* Birden çok güncelleştirme ve veri Kataloğu portalını kullanılabilirliğini ve performansı artırmak için düzeltmeler.

## <a name="whats-new-for-july-2017"></a>Temmuz 2017 yenilikler nelerdir? 
Temmuz 2017'ten itibaren Azure veri kataloğu için aşağıdaki özellikleri eklenmiştir:
*   İzin verilen meta veri işlemleri dahil olmak üzere üzerinde daha ayrıntılı denetim için destek:
    - Katalog yöneticileri, kullanıcıların etiketleri ve ilgili meta veri Kataloğu'na katkıda yeteneğini katalog salt okunur erişimi etkinleştirme kısıtlayabilirsiniz.
    - Katalog yöneticilerinin yeni veri kaynaklarını kataloğa kaydetmek için kullanıcıların yeteneğini kısıtlayabilirsiniz.
    - Katalog yöneticileri, kullanıcıların katalogdaki veri varlık meta verilerini sahipliğini yeteneğini kısıtlayabilirsiniz.
    - Azure Active Directory güvenlik grupları ve izinleri yönetme kolaylığı için kullanıcılar için izinler.
* Kayıtlı veri varlıklarını veri Kataloğu portalında bulan ilgili verileri varlıklar arasındaki ilişkiler için destek dahil olmak üzere:
    - İlişki meta veri ayıklama SQL Server'ın (Azure SQL veritabanı dahil), Oracle ve MySQL veri kaynakları veri Kataloğu veri kaynağı kayıt aracını kullanırken.
    - Varlık meta veri Kataloğu portalında görüntülerken ilgili veri varlıklarını bulma.
    - Tanımlamak için işlemler bulmak ve veri Kataloğu REST API'sini kullanarak veri varlıklarını arasındaki ilişkileri yönetme.

Veri Kataloğu'ndaki izinleri yönetme ile ilgili ek ayrıntılar için bkz: [veri kataloğu ve veri varlıkları güvenli şekilde nasıl](data-catalog-how-to-secure-catalog.md).
Veri Kataloğu ilişkilerde hakkında daha fazla ayrıntı için bkz: [Azure veri Kataloğu'nda ilgili veri varlıklarını görüntülemek nasıl](data-catalog-how-to-view-related-data-assets.md).

## <a name="whats-new-for-june-2017"></a>Haziran 2017 yenilikler nelerdir? 
Haziran 2017'ten itibaren Azure veri kataloğu için aşağıdaki özellikleri eklenmiştir:
*   Sybase, Apache Cassandra ve MongoDB veri kaynakları için destek. Kullanıcılar artık kaydedebilir ve Cassandra ve MongoDB veritabanları ve tablolar ve Sybase veritabanları, tabloları ve görünümleri keşfedin.

> [!NOTE]
> Kayıtlar ve dizi gibi karmaşık veri türleriyle sütunlarını içerecek MongoDB ve Cassandra tabloları kaydedilirken bu sütunların veri Kataloğu'na eklenen meta veriler dahil edilmez.

## <a name="whats-new-for-may-2017"></a>Mayıs 2017 yenilikler nelerdir? 
Mayıs 2017'dan sonra aşağıdaki özellikleri Azure veri Kataloğu'na eklenmiştir:
*   Yeni bir geliştirici örnek toplu iş terimler içeri aktarmak için kullanılabilir. Sözlük toplu içeri örnek edinilebilir [veri Kataloğu geliştirici örnekleri sayfasına](https://docs.microsoft.com/azure/data-catalog/data-catalog-samples). 
*   Azure veri Kataloğu portalında ODBC bağlantı bilgilerini düzenlemek için destek. Veri varlık sahipleri ve veri Kataloğu yöneticileri, veri kaynaklarını yeniden kaydetmeye gerek kalmadan kayıtlı ODBC veri kaynakları için bağlantı bilgilerini şimdi düzenleyebilirsiniz.
*   İş sözlüğü tanımları ve açıklamaları tıklanabilir URL'ler için destek. URL'ler için iş terimler açıklama ve tanım özelliklerinde eklendiğinde URL'leri veri Kataloğu portalını köprü olarak görüntülenir.
*   Uzman e-posta adresleri yanı sıra Uzman adlarını görüntülemek için destek. Bir veri varlığına özelliklerinde uzmanlar veri Kataloğu portalında görüntülerken, Azure Active Directory'den Uzman kullanıcının tam adını ipucunda görüntülenir.

## <a name="whats-new-for-april-2017"></a>Nisan 2017 yenilikler nelerdir? 
Nisan 2017'ten itibaren Azure veri kataloğu için aşağıdaki özellikleri eklenmiştir:
*   ODBC veri kaynakları için destek. Kullanıcılar artık kaydedebilir ve ODBC veritabanları, tabloları ve görünümleri veri Kataloğu veri kaynağı kayıt aracını kullanarak keşfedin.

## <a name="whats-new-for-march-2017"></a>Mart 2017 yenilikler nelerdir? 
Mart 2017'ten itibaren Azure veri kataloğu için aşağıdaki özellikleri eklenmiştir:
*   Veri Kataloğu Yöneticiler için AAD güvenlik gruplarını kullanma desteği.
*   Azure veri Kataloğu, artık yeni bir Azure bölgesinde kullanılabilir. Müşteriler bölgede Batı Orta ABD, Doğu ABD, Batı ABD, Batı Avrupa ve Doğu Avustralya, Kuzey Avrupa ve Güneydoğu Asya yanı sıra Azure veri Kataloğu hazırlayabilirsiniz. Daha fazla bilgi için bkz: [Azure bölgeleri](https://azure.microsoft.com/regions/).

## <a name="whats-new-for-february-2017"></a>Şubat 2017 yenilikler nelerdir? 
Şubat 2017'ten itibaren Azure veri kataloğu için aşağıdaki özellikleri eklenmiştir:
*   Veri Kataloğu veri kaynağı kayıt aracını Gelişmiş ayarlarda desteği. Kullanıcıların büyük veri kaynaklarını kaydederek komutu zaman aşımı değerlerini belirtebilirsiniz.
*   FTP ve PostgreSQL veri kaynakları için destek. Kullanıcılar artık kaydedebilir ve FTP dosyaları ve dizinleri ve PostgreSQL veritabanları, tabloları ve görünümleri keşfedin.

## <a name="whats-new-for-january-2017"></a>Ocak 2017 yenilikler nelerdir? 
Ocak 2017'ten itibaren Azure veri kataloğu için aşağıdaki özellikleri eklenmiştir:
*   Azure veri Kataloğu, şimdi [CSA yıldız](https://www.microsoft.com/trustcenter/compliance/csa-star-certification) uyumlu.
*   İle tümleştirme [Al & Excel 2016 ve Excel için Power Query dönüştürme](https://support.office.com/article/Introduction-to-Microsoft-Power-Query-for-Excel-6E92E2F4-2079-4E1F-BAD5-89F6269CD605). Excel kullanıcıları paylaşmak sorgular ve Azure veri Kataloğu'ndan kullanarak sorguları Bul Excel içinde. Bu işlevsellik, Power BI Pro lisansına sahip olan kullanıcılar için kullanılabilir.

## <a name="whats-new-for-december-2016"></a>Aralık 2016 için Yenilikler
Aralık 2016 itibariyle, aşağıdaki özellikleri, Azure veri Kataloğu'na eklenmiştir:
*   Azure veri Kataloğu, şimdi [HIPAA](https://www.microsoft.com/trustcenter/Compliance/HIPAA) ve [AB Model maddeleri](https://www.microsoft.com/TrustCenter/Compliance/EU-Model-Clauses) uyumlu.
*   Veri kaynağı bağlantı bilgilerini düzenlemek için destek. Veri varlık sahipleri ve veri Kataloğu yöneticileri, veri kaynaklarını yeniden kaydetmeye gerek kalmadan kayıtlı veri kaynakları için bağlantı bilgilerini şimdi düzenleyebilirsiniz.
*   Salesforce.com veri kaynakları için destek. Kullanıcılar artık kaydedebilir ve Salesforce nesnelerini bulur.


## <a name="whats-new-for-november-2016"></a>Kasım 2016 için Yenilikler
Kasım 2016 itibariyle, aşağıdaki özellikleri, Azure veri Kataloğu'na eklenmiştir:
*   Azure veri Kataloğu, şimdi [ISO/IEC 27001](https://www.microsoft.com/trustcenter/compliance/iso-iec-27001) ve [ISO/IEC 27018](https://www.microsoft.com/TrustCenter/Compliance/iso-iec-27018) uyumlu.
*   ODBC veri kaynakları veri Kataloğu portalını ve REST API kullanarak el ile kaydını desteği.

## <a name="whats-new-for-september-2016"></a>Eylül 2016 için Yenilikler
Eylül 2016 itibariyle, aşağıdaki özellikleri, Azure veri Kataloğu'na eklenmiştir:

* IBM DB2 veri kaynakları için destek. Kullanıcılar artık kaydedebilir ve DB2 veritabanları, tabloları ve görünümleri keşfedin.
* Azure Cosmos DB veri kaynakları için destek. Kullanıcılar artık kaydedebilir ve Cosmos DB veritabanları ve koleksiyonlarına keşfedin.
* Veri Kataloğu portalındaki katalog adı özelleştirme desteği. Katalog yöneticileri artık kuruluş adı gibi portal başlık görüntülenen metin sağlayabilir.

## <a name="whats-new-for-august-2016"></a>Ağustos 2016 için Yenilikler
Ağustos 2016 itibariyle, aşağıdaki özellikleri, Azure veri Kataloğu'na eklenmiştir:

* SQL Server ana veri Hizmetleri (MDS) veri kaynaklarının geliştirmeleri kaydı için. Kullanıcılar, artık veri Kataloğu veri kaynağı kayıt aracını kullanarak MDS varlıklar kaydedilirken önizlemeleri ve veri profili bulunabilir.
* Yönetici tarafından tanımlanan kuruluş Kaydedilmiş aramaları için destek. Bir arama veri Kataloğu portalında kaydederken, veri Kataloğu Yöneticiler artık aramaları tüm katalog kullanıcıları veya kişisel kullanım için kaydetmeyi seçebilirsiniz. Kuruluş Kaydedilmiş aramaları tüm katalog kullanıcılarla paylaşılan ve standart başlangıç noktası için veri kaynağı bulma sağlayabilir.
* Veri Kataloğu portalında özelliklerini görüntülemek için güncelleştirmeler. Tüm veri varlık özellikleri artık görüntülenir ve bir daha tutarlı sağlamak için tek, yeniden boyutlandırılabilir bölmesi ve bulunabilirlik deneyimi içinde yönetilir.

## <a name="whats-new-for-july-2016"></a>Temmuz 2016 için Yenilikler
Temmuz 2016 itibariyle, aşağıdaki özellikleri, Azure veri Kataloğu'na eklenmiştir:

* SQL Server ana veri Hizmetleri (MDS) veri kaynakları için destek. Kullanıcılar artık kaydedebilir ve MDS modelleri ve varlıkları keşfedin.
* SQL Server saklı yordamlar için destek. Kullanıcılar artık kaydedebilir ve SQL Server veri kaynaklarını saklı yordam nesneleri keşfedin.
* Azure veri Kataloğu portalını ve veri kaynağı kayıt aracını toplam 18 desteklenen diller için ek dilleri için destek. Azure veri Kataloğu kullanıcı deneyimi, Windows veya web tarayıcısı dil tercihlerini göre yerelleştirilmiştir.
* Güncelleştirmeleri ve veri Kataloğu portalı giriş performans iyileştirmeleri ve geliştirilmiş kullanıcı deneyimi de dahil olmak üzere sayfa için iyileştirme.

## <a name="whats-new-for-june-2016"></a>Haziran 2016 yenilikleri
Haziran 2016 itibariyle, aşağıdaki özellikleri, Azure veri Kataloğu'na eklenmiştir:

* Veri varlıklarını veri Kataloğu portalında keşfederken liste görünümünde sütunları yeniden boyutlandırma için destek. Kullanıcılar artık uzun varlık meta etiketleri ve açıklamaları gibi daha kolay okumak için ayrı ayrı sütunları yeniden boyutlandırabilirsiniz.
* Excel için Power Query veri Kataloğu portalında "Aç" menüsüne eklendi. Kullanıcılar artık açabilir desteklenen veri kaynakları Excel 2016 veya Excel 2010 ve Microsoft Excel 2013 ile [Excel için Power Query](https://support.office.com/article/Introduction-to-Microsoft-Power-Query-for-Excel-6E92E2F4-2079-4E1F-BAD5-89F6269CD605) yüklü eklentisi.
* Azure Table depolama veri kaynakları için destek. Kullanıcılar artık kaydedebilir ve Azure Storage veri kaynaklarında tablo nesnelerini bulur.

## <a name="whats-new-for-may-2016"></a>Mayıs 2016 için Yenilikler
Mayıs 2016 itibariyle, aşağıdaki özellikleri, Azure veri Kataloğu'na eklenmiştir:

* Katalog yöneticilerin iş hüküm ve ortak bir iş sözlüğü oluşturmak için hiyerarşileri tanımlamasına izin veren bir iş sözlüğü. Kullanıcıları, sözlük terimlerini kayıtlı veri varlıklarını bulmak ve Katalog içeriği anlamak daha kolay hale getirmek için etiketleyebilirsiniz. Daha fazla bilgi için bkz. [İş Sözlüğünü Yönetilen Etiketleme için ayarlama](data-catalog-how-to-business-glossary.md)  
* Tek bir işlemde birden çok terimler güncelleştirmek kullanıcılara veri Kataloğu iş sözlüğünü geliştirmeler. Kullanıcıların aşağıdaki alanları düzenlemek için birden çok koşulları seçebilirsiniz:
  * Üst terim: Kullanıcı yeni bir üst terim seçebilir ve tüm seçili koşulları alt öğelerinin seçili üst terim olacak şekilde güncelleştirilir. Seçili tüm koşulları, aynı üst öğeye sahip ve ardından o üst metin kutusuna alan ayarlanmış üst terim aksi gösterilen boş.   
  * Etiketleri ve Paydaşlar: kullanıcılar ekleyebilir ve birden fazla veri varlığına etiketleme olarak aynı deneyimi kullanarak birden çok terimler için etiketleri ve Paydaşlar kaldırabilirsiniz.

> [!NOTE]
> İş sözlüğünü yalnızca, Azure veri Kataloğu standart sürümü mevcuttur. Ücretsiz sürüm özelliklerini iş sözlüğünü yönetilen etiketleme veya sağlamaz.

## <a name="whats-new-for-march-2016"></a>Mart 2016 için Yenilikler
Mart 2016 itibariyle, Azure veri Kataloğu: g için aşağıdaki özellikleri eklenmiştir:

* Azure veri Kataloğu hizmet Kataloğu varlık yönetim özelliklerini ve arama yetenekleri programlı olarak erişmek için bir birleştirilmiş REST API uç noktası. Bu arama API uç noktası ve Katalog API uç noktası kullanım ve 21 Mart 2016 tarihinde devam etmez. API semantiği herhangi bir değişiklik yoktur. Yalnızca uç noktası URI değiştirildi. Ek bilgi için bkz: [Azure veri Kataloğu REST API Başvurusu](https://msdn.microsoft.com/library/azure/mt267595.aspx). API örnekleri için bkz: [Azure veri Kataloğu geliştirici örnekleri](data-catalog-samples.md).

## <a name="whats-new-for-february-2016"></a>Şubat 2016 için Yenilikler
Şubat 2016 itibariyle, aşağıdaki özellikleri, Azure veri Kataloğu'na eklenmiştir:

* Yeniden tasarlanan yeni veri kaynağı seçimi Azure veri Kataloğu veri kaynağı kayıt aracında karşılaşırsınız. Veri kaynağı kayıt aracını bulup veri kaynaklarından seçmek için Azure veri Kataloğu tarafından desteklenen kolaylaştırmak için güncelleştirilmiştir.
* Azure veri Kataloğu portalını ve veri kaynağı kayıt aracını 10 ek dil için destek. İngilizce yanı sıra, Azure veri Kataloğu deneyimi Almanca, İspanyolca, Fransızca, İtalyanca, Japonca, Korece, Portekizce (Brezilya), Rusça, Basitleştirilmiş Çince ve Geleneksel Çince kullanıma sunulmuştur. Azure veri Kataloğu kullanıcı deneyimi, Windows veya kullanıcının web tarayıcısı dil tercihlerini göre yerelleştirilmiştir.
* Azure veri Kataloğu veri iş devamlılığı ve olağanüstü durum kurtarma için coğrafi çoğaltma için destek. Veri kaynağı meta verileri ve nun açıklamalar, dahil olmak üzere tüm Azure veri Kataloğu içeriği artık müşteriler için hiçbir ek ücret ödemeden iki Azure bölgeler arasında çoğaltılır. Azure bölgeleri, en az 500 mil birbirinden, önceden eşleştirilmelidir ve eşleme açıklandığı gibi uygulayın [iş devamlılığı ve olağanüstü durum kurtarma (BCDR): Azure eşleştirilmiş bölgeleri](../best-practices-availability-paired-regions.md).
* Azure veri Kataloğu tarafından kullanılan Azure aboneliğini değiştirmek için destek. Azure veri Kataloğu Yöneticiler faturalandırma için farklı bir Azure aboneliği seçmek için Azure veri Kataloğu portalında ayarlar sayfasını kullanabilirsiniz.

## <a name="whats-new-for-january-2016"></a>Ocak 2016 için Yenilikler
Ocak 2016 itibariyle, aşağıdaki özellikleri, Azure veri Kataloğu'na eklenmiştir:

* El ile ek veri kaynaklarını kaydederek desteği. Şimdi, Azure veri Kataloğu Portalı'nda "El ile girişi oluşturma" kullanın veya aşağıdaki veri kaynaklarını kaydetmek için Azure veri Kataloğu REST API'sini kullanın:
  * OData - işlevi, varlık kümesi veya varlık kapsayıcısı
  * HTTP - dosya, uç nokta, rapor ve Site
  * Dosya sistemi - dosya
  * SharePoint - liste
  * FTP - dosya ve dizin
  * Salesforce.com - nesnesi
  * DB2 - tablo, Görünüm ve veritabanı
  * PostgreSQL - tablo, Görünüm ve veritabanı
* SQL Server'ın (Azure SQL DB ve Azure SQL Data Warehouse dahil) veri kaynakları için "Aç içinde SQL Server veri Araçları" için destek.  
* Kaydetme ve SAP HANA görünümleri ve paketleri keşfetmek için destek. Veri kaynağı kayıt aracını ve açıklamalar ekleyebilir ve bunları Azure veri Kataloğu Portalı'nı kullanarak kayıtlı SAP HANA veri kaynaklarını bulmasına Azure veri kataloğu kullanarak SAP HANA veri kaynaklarını kaydedebilirsiniz.
* Sabitleme ve veri varlıklarını Azure veri Kataloğu portalında çözme yeteneği. Veri varlıklarını yeniden Bul ve yeniden daha kolay hale getirmek için PIN seçebilirsiniz.
* Azure veri Kataloğu portalında yeniden tasarlanan yeni bir giriş sayfası. Yeni giriş sayfası bir bütün olarak kataloğunda son yayımlanan varlıklar, sabitlenmiş varlıklar ve kaydedilmiş aramaları - ve etkinlik bir anlayış dahil olmak üzere geçerli kullanıcıların etkinlik - bir anlayış içerir.
* Azure veri Kataloğu portalındaki kalıcı kullanıcı ayarları desteği. Kullanıcı deneyimi ayarlarını - kılavuz veya döşeme görünümü, sonuçlar sayfası ve vurgulamayı açmak veya kapatmak isabet sayısı dahil olmak üzere - kullanıcı oturumları arasında kalıcı niteliktedir.
* Azure veri Kataloğu, artık iki yeni Azure bölgelerde kullanılabilir. Müşteriler bölgelerdeki Kuzey Avrupa ve Güneydoğu Asya, Doğu ABD, Batı ABD, Batı Avrupa ve Doğu Avustralya yanı sıra Azure veri Kataloğu hazırlayabilirsiniz. Daha fazla bilgi için bkz: [Azure bölgeleri](https://azure.microsoft.com/regions/).

> [!NOTE]
> "SQL Server veri araçları Aç" Visual Studio 2013 güncelleştirme 4 ve SQL Server araçları yüklü olmasını gerektirir. SQL Server veri Araçları'nın en son sürümünü yüklemek için ziyaret edin [karşıdan SQL Server veri Araçları'nı (SSDT)](https://msdn.microsoft.com/library/mt204009.aspx).


## <a name="whats-new-for-december-2015"></a>Aralık 2015 için Yenilikler
Aralık 2015'ten itibaren Azure veri kataloğu için aşağıdaki özellikleri eklenmiştir:

* Azure SQL Data Warehouse veri kaynakları için veri profilleri için destek. Azure SQL Data Warehouse tabloları ve görünümleri kaydederken, kullanıcıların veri kaynağından ayıklanan meta verilerle veri profili ölçümleri içerecek şekilde seçebilirsiniz.
* Kaydetme ve MySQL nesneleri ve veritabanları keşfetmek için destek. Kullanıcılar veri kaynağı kayıt aracını ve açıklamalar ekleyebilir ve bunları Azure veri Kataloğu Portalı'nı kullanarak kayıtlı MySQL veri kaynaklarını bulmasına Azure veri kataloğu kullanarak MySQL veri kaynaklarını kaydedebilirsiniz.
* Teradata veri kaynakları için SPNEGO ve Windows kimlik doğrulaması için destek. Teradata tabloları ve görünümleri kaydederken, kullanıcıların SPNEGO ve Windows ve LDAP ve TD2 kimlik doğrulaması kullanarak Teradata için bağlanmayı seçebilirsiniz.
* Azure Data Lake Store veri kaynakları için destek. Kullanıcılar artık kaydedebilir ve Azure veri Kataloğu'nu kullanarak Azure Data Lake Store veri kaynaklarını bulmasına.
* Azure veri Kataloğu veri kaynağı kayıt aracını el ile ağ proxy ayarlarını belirtmek için destek. Kullanıcılar "Proxy ayarlarını değiştir" aracın Karşılama sayfasından seçebilirsiniz ve aracı tarafından kullanılacak bağlantı noktasını ve proxy adresi belirtebilirsiniz.

## <a name="whats-new-for-november-2015"></a>Kasım 2015 için Yenilikler
Kasım 2015'ten itibaren Azure veri kataloğu için aşağıdaki özellikleri eklenmiştir:

* Görüntüleme ve SQL Server'ın (Azure SQL veritabanı dahil olmak üzere) ve Oracle veri kaynakları için bağlantı dizeleri Azure veri Kataloğu portalındaki kopyalama olanağı. Kullanıcılar için bağlantı bilgilerini bir SQL Server veya Oracle tablosu, görünümü veya veri kaynağına bağlanmak için kullanılan bağlantı dizeleri görmek için veritabanı, "Bağlantı dizelerini görüntüle" bağlantısına tıklayabilirsiniz. ADO.NET, ODBC, OLEDB ve JDBC bağlantı dizeleri için SQL Server veri kaynaklarını sağlanır. ODBC ve OLEDB bağlantı dizeleri Oracle veri kaynakları için sağlanır.
* Teradata tabloları ve görünümleri kaydederken veri profilleri de dahil olmak üzere desteği.
* "Açık olarak Power BI Desktop için" (Azure SQL DB ve Azure SQL Data Warehouse dahil) SQL Server için SQL Server Analysis Services, Azure Storage ve HDFS kaynakları destekler.  
* Teradata veri kaynakları için LDAP kimlik doğrulaması için destek. Teradata tabloları ve görünümleri kaydederken, kullanıcılar için Teradata bağlanmak LDAP, kullanarak seçebilir ve TD2 kimlik doğrulaması.
* Teradata veri kaynakları için "Aç Excel'de" için destek.
* Azure veri Kataloğu portalını son arama terimlerini desteği. Portalda arama yaparken, kullanıcıların bulma deneyimini hızlandırmak için son kullanılan arama koşullarına seçebilirsiniz.
* Teradata veri kaynakları için Önizleme için destek. Teradata tabloları ve görünümleri kaydederken, kullanıcıların veri kaynağından ayıklanan meta verilerle anlık görüntü kayıtları eklemek seçebilirsiniz.
* Azure SQL Data Warehouse veri kaynakları için "Aç Excel'de" için destek.
* Tanımlama ve el ile kayıtlı veri varlıklarını sütun düzeyi şemalarda düzenlemek için destek. El ile Azure veri Kataloğu Portalı'nı kullanarak bir veri varlığına oluşturduktan sonra kullanıcılar veri varlık özelliklerinde sütun tanımları ekleyebilirsiniz.
* Belirli meta verilerine sahip kayıtlı veri varlıklarını bulunmasını etkinleştirmek için Azure veri kataloğu arama yaparken "sahip" sorgular için destek. Azure veri Kataloğu sorgu sözdizimi artık içerir:

| Sorgu sözdizimi | Amaç |
| --- | --- |
| `has:previews` |Önizleme ekleme veri varlıklarını bulur |
| `has:documentation` |Veri varlıklarını belgelerine sağlanan bulur |
| `has:tableDataProfiles` |Tablo düzeyi veri profili bilgileri ile veri varlıklarını bulur |
| `has:columnsDataProfiles` |Sütun düzeyinde veri profili bilgileri ile veri varlıklarını bulur |


> [!NOTE]
> "Power BI Desktop'ta Aç" Power BI Desktop uygulamanın yüklenmesi için güncel bir sürümünü gerektirir. Sorunları veya bu özelliği kullanarak hatalarla karşılaşırsanız, Power BI Desktop'en son sürümüne sahip olduğunuzdan emin olun [PowerBI.com](https://powerbi.com).


## <a name="whats-new-for-october-2015"></a>Ekim 2015 için Yenilikler
Ekim 2015'ten itibaren Azure veri kataloğu için aşağıdaki özellikleri eklenmiştir:

* Kayıtlı veri kaynaklarının önizlemeleri ve veri profilleri deposunda kalan veri şifrelemesi desteği. Azure veri Kataloğu saydam herhangi Önizleme kayıtları şifreler ve anahtar yönetimi katalog yöneticileri tarafından gerek kalmadan hizmeti ile kayıtlı veri kaynaklarına veri profilleri.
* Teradata veri kaynakları için destek. Kullanıcılar artık kaydedebilir ve Teradata tabloları ve görünümleri keşfedin.
* Şirket içi Hive veri kaynakları için destek. Kullanıcılar artık kaydedebilir ve Apache Hive Hadoop şirket içi veri kaynakları için Hive tablolarını keşfedin.
* Azure veri Kataloğu portalında Kaydedilmiş aramaları için destek. Kullanıcılar, arama terimleri kaydedebilir ve filtre seçimleri kolayca önceki aramaları yineleyin ve Katalog içeriği yararlı görünümlerini tanımlamak için. Kullanıcı, kendi varsayılan arama olarak kaydedilmiş bir aramayı de işaretleyebilirsiniz. Bir kullanıcı Azure veri Kataloğu portalı giriş sayfasından veya "Başlarken" Sayfa "Büyüteç" arama simgesine tıkladığında, kullanıcının doğrudan varsayılan olarak işaretlenen kaydedilen arama alınır.
* Kayıtlı veri varlıklarını ve Azure veri Kataloğu portalında kapsayıcıları için zengin metin belgeleri için destek. Kullanıcılar, artık veritabanları ve burada etiketleri ve açıklamaları yeterli olmadığı senaryoları için modelleri gibi kapsayıcıları ve tablolar, görünümler ve raporlar gibi veri varlıklarını için belgeleri sağlayabilir.
* Bilinen bir veri kaynağı türlerini el ile kaydetme desteği. Kullanıcılar, Azure veri Kataloğu tarafından desteklenen tüm veri kaynağı türleri için Azure veri Kataloğu Portalı'nı kullanarak veri kaynağı bilgilerini el ile girebilirsiniz.
* Azure Active Directory güvenlik gruplarını yetkilendirmek için destek. Katalog yöneticileri güvenlik grupları, kullanıcı hesapları, Azure veri Kataloğu erişimi yönetmek daha kolay katalog erişimi etkinleştirebilirsiniz.
* Azure veri Kataloğu Portalı'ndan Excel'de yığın veri kaynakları açmak için destek.

> [!NOTE]
> Geçerli sürüm için yalnızca Teradata TD2 kimlik doğrulaması desteklenir. Ek kimlik doğrulama mekanizmaları gelecekte desteklenir serbest bırakır.

> [!NOTE]
> Hive veri kaynakları için "Aç Excel'de" özelliğini kullanmak için kullanıcılar için Hive ODBC sürücüsü olarak yüklemiş olmalısınız.

## <a name="whats-new-for-september-2015"></a>Eylül 2015 için Yenilikler
Eylül 2015'ten itibaren Azure veri kataloğu için aşağıdaki özellikleri eklenmiştir:

* Veri profilleri Hive veri kaynaklarını kaydederek zaman dahil olmak üzere desteği.
* Program aracılığıyla Azure veri Kataloğu ile tümleştirmek üzere uygulamalar için daha kolay Kataloğu API'si bulmak için destek.
* Yeni bir "Başlarken" veri kaynağı bulma deneyimini Azure veri Kataloğu portalında. Kullanıcılar, bir arama terimi girerek olmadan Azure veri Kataloğu portalı "Bul" sayfasına girdiğinde, bunlar en sık kullanılan etiketleri, uzmanlar, veri kaynağı türü ve nesne türleri dahil olmak üzere katalog içeriği bir bakış sunulmuştur.
* Kaydetme ve Azure SQL veri ambarı nesneleri ve veritabanları keşfetmek için destek. Azure SQL Data Warehouse hakkında ek bilgi için bkz: [SQL Data Warehouse](https://azure.microsoft.com/services/sql-data-warehouse/).
* Kaydetme ve SQL Server Analysis Services modelleri ve SQL Server Reporting Services sunucuları kapsayıcılar olarak keşfetmek için destek. SSAS ve SSRS nesneleri kaydederken, Azure veri Kataloğu SSAS modeli ve SSRS sunucusuna ve raporlar ve diğer nesneleri için bir giriş oluşturur. Kapsayıcılar keşfedilmesini ve Azure veri Kataloğu Portalı'nı kullanarak açıklama. Kullanıcılar ayrıca arama ve filtre bir model veya arama ve filtreleme katalog içeriğinin yanı sıra sunucu içeriği.
* Kaydetme ve HTTP/HTTPS üzerinden SQL Server Analysis Services nesneleri bulmak için destek. Kullanıcılar artık SSAS sunucuları bir sunucu adı yerine bir URL (örneğin, https://servername/olap/msmdpump.dll) kullanarak bağlanabilir ve temel kimlik doğrulaması ve anonim bağlantılara ek olarak Windows kimlik doğrulaması kullanabilirsiniz. SSAS HTTP/HTTPS bağlantıları hakkında ek bilgi için bkz: [Analysis Services HTTP erişimi Yapılandır](https://msdn.microsoft.com/library/gg492140.aspx).
* Hdınsight'ta Hive veri kaynakları için destek. Kullanıcılar artık kaydedebilir ve Apache Hive hadoop'ta Hdınsight veri kaynaklarında için Hive tablolarını keşfedin. Hdınsight'ta Hive hakkında ek bilgi için bkz: [Hdınsight Belge Merkezi](../hdinsight/hadoop/hdinsight-use-hive.md).
* Kaydetme ve Oracle veritabanları ve kapsayıcı olarak HDFS kümeleri keşfetmek için destek. Oracle tabloları ve görünümleri veya HDFS kaydederken, Azure veri Kataloğu veritabanı, tabloları ve görünümleri için bir giriş oluşturur. Veritabanı keşfedilmesini ve Azure veri Kataloğu Portalı'nı kullanarak açıklama. Kullanıcılar ayrıca arama ve filtre bir veritabanı veya arama ve filtreleme katalog içeriğinin yanı sıra küme içeriği.
* Bilinmeyen veri kaynağı türlerini el ile kaydetme desteği. Kullanıcılar, böylece açıkça veri kaynağı kayıt aracını tarafından desteklenen veri kaynakları açıklama ve bulunan Azure veri Kataloğu Portalı'nı kullanarak veri kaynağı bilgilerini el ile girebilirsiniz.
* Kaydetme ve SQL Server veritabanlarını kapsayıcı olarak keşfetmek için destek. SQL Server tabloları ve görünümleri kaydederken, Azure veri Kataloğu veritabanı, tabloları ve görünümleri için bir giriş oluşturur. Veritabanı keşfedilmesini ve Azure veri Kataloğu Portalı'nı kullanarak açıklama. Kullanıcılar ayrıca arayın ve arama ve filtreleme katalog içeriğinin yanı sıra bir veritabanının içeriğini filtre.

> [!NOTE]
> Eylül 18 yayın kayıtlı öncesinde silinmiş SSAS ve SSRS nesneleri modeli veya sunucu girişi kataloğa eklenmeden önce veri kaynağı kayıt aracını kullanarak yeniden kaydedilmesi gerekir. Bir veri kaynağı yeniden kaydetme Azure veri Kataloğu portalında kullanıcılar tarafından eklenen tüm ek açıklamaları etkilemez.

> [!NOTE]
> Oracle tabloları ve görünümleri ve HDFS dosyaları ve 11 Eylül yayın kayıtlı öncesinde silinmiş dizinleri veritabanı veya küme girişi kataloğa eklenmeden önce veri kaynağı kayıt aracını kullanarak yeniden kaydedilmesi gerekir. Bir veri kaynağı yeniden kaydetme Azure veri Kataloğu portalında kullanıcılar tarafından eklenen tüm ek açıklamaları etkilemez.

> [!NOTE]
> SQL Server tablolarını ve Eylül 4 yayın kayıtlı öncesinde silinmiş görünümleri veritabanı girişi kataloğa eklenmeden önce veri kaynağı kayıt aracını kullanarak yeniden kaydedilmesi gerekir. Bir veri kaynağı yeniden kaydetme Azure veri Kataloğu portalında kullanıcılar tarafından eklenen tüm ek açıklamaları etkilemez.

## <a name="whats-new-for-august-2015"></a>Ağustos 2015 için Yenilikler
Ağustos 2015'ten itibaren Azure veri kataloğu için aşağıdaki özellikleri eklenmiştir:

* SQL Server ve Oracle veri kaynaklarından veri profil desteği. SQL Server ve Oracle tabloları ve görünümleri kaydederken, kullanıcıların Kaydedilmekte nesneler için veri profili bilgileri dahil etmeyi seçebilirsiniz. Veri profili nesne ve sütun düzeyi istatistikleri içerir.
* Hadoop HDFS veri kaynakları için destek. Kullanıcılar artık kaydedebilir ve HDFS dosyaları ve dizinleri bulur.
* Kayıtlı veri kaynakları için erişim isteği bilgilerini sağlamak için destek. Herhangi bir kayıtlı veri varlığını için kullanıcılara şimdi kolayca varolan araçları ve süreçleri ile tümleştirme hakkında yönergeler için e-posta bağlantılar veya URL'ler dahil olmak üzere, istekte bulunan erişimi sağlayabilir.
* Etiketleri ve hangi kullanıcıların hangi meta verileri için kayıtlı veri varlıklarını sağlamış bulmasını kolaylaştırmak için uzmanları için araç ipuçları.
* Bizim üst gezinti çubuğu yeni "Kullanıcı" düğmesi ve menüsü ekledik. İstenen varsa oturumu için ve bu menü Azure veri Kataloğu'na oturum açmak için kullanılan hesabı bakın kullanıcı olanak sağlar. Bu menü ayrıca Azure veri Kataloğu REST API'sini kullanarak geliştiricileri için değerlidir katalog adı görüntüler.
* Yalnızca Standard Edition: sahipleri veri varlıklarına eklerken, Azure veri Kataloğu artık kullanıcı hesapları ve güvenlik grupları sahipleri olarak destekler. Seçilen veri varlıklarının sahibi olarak bir güvenlik grubu eklemek için varsa, grubun görünen adı veya grubun UPN e-posta adresi girebilirsiniz.
* Azure Blob Depolama veri kaynakları için destek. Kullanıcılar artık kaydedebilir ve Azure Storage blobları ve dizinleri bulur.

