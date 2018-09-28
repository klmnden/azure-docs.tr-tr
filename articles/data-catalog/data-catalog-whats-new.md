---
title: Azure veri Kataloğu'ndaki Yenilikler
description: Bu makalede, Azure veri Kataloğu'na eklenen yeni özellikler genel bir bakış sağlar.
services: data-catalog
author: markingmyname
ms.author: maghan
ms.assetid: 1201f8d4-6f26-4182-af3f-91e758a12303
ms.service: data-catalog
ms.topic: conceptual
ms.date: 01/18/2018
ms.openlocfilehash: 86c8e8c10811b1478ae2c853f1efef5b6b5caa83
ms.sourcegitcommit: b7e5bbbabc21df9fe93b4c18cc825920a0ab6fab
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/27/2018
ms.locfileid: "47406337"
---
# <a name="whats-new-in-azure-data-catalog"></a>Azure veri Kataloğu'ndaki Yenilikler
Güncelleştirmeleri **Azure veri Kataloğu** düzenli olarak kullanıma sunulur. Her sürüm bazı sürümler arka uç hizmeti özelliklerine odaklanan yeni kullanıcıya yönelik özellikler içerir. Bu sayfa Azure veri Kataloğu hizmetine eklenen yeni kullanıcıya yönelik özellikler vurgulamaktadır.

## <a name="whats-new-for-november-2017"></a>Kasım 2017 için yenilikler nelerdir? 
Kasım 2017'den itibaren aşağıdaki özellikleri, Azure veri Kataloğu'na eklenmiştir:

* Veri Kataloğu Portalı'nda belirli iş sözlüğü terimleri doğrudan bağlamak için destek. Kullanıcılar bağlantıları iş sözlüğü'nden kopyalayın ve bunları belgeleri, e-postaları, raporlara veya başka bir konuma doğrudan sözlüğü terim tanımı bağlantı ekleyin.
* Azure Active Directory Hizmet sorumluları için destek. Veri Kataloğu yöneticilerinin yetkilendirilebilir, istemci uygulama Kataloğu'na erişmek için hizmet sorumlularını kullanma ve bu uygulamalar, yalnızca, kullanıcılar ve güvenlik grupları için izinler verebilirsiniz gibi belirli izinleri verebilir. Daha fazla bilgi için bkz. [uygulaması ve Azure Active Directory'de Hizmet sorumlusu nesneleri](../active-directory/develop/app-objects-and-service-principals.md).
* Veri Kataloğu veri kaynağı kayıt aracını kullanarak Azure SQL veritabanı ve Azure SQL veri ambarı veri kaynaklarına bağlanırken görüntülenen Azure Active Directory kimlik doğrulaması için destek. Daha fazla bilgi için bkz. [kullanımı Azure SQL veritabanı veya SQL veri ambarı ile kimlik doğrulaması için Active Directory kimlik](../sql-database/sql-database-aad-authentication.md).


## <a name="whats-new-for-september-2017"></a>Eylül 2017 için yenilikler nelerdir? 
Eylül 2017'den itibaren aşağıdaki özellikleri, Azure veri Kataloğu'na eklenmiştir:

* Veri kaynağı kayıt aracını kullanarak ilgili tabloları kaydedilirken DB2 veri kaynaklarından ilişkisi meta verileri ayıklamak için destek katılın.
* Veri kaynağı kayıt aracını kullanarak MongoDB sürümü 3.4 veri kaynaklarını kaydederek desteği.
* Veri Kataloğu'ndan kaldırırken bir veritabanı veya başka bir kapsayıcıdaki tüm meta veriler için tek bir işlemde kapsanan nesneleri silmek için destek.
* En fazla 1.000 etiketler, iş sözlüğü terimleri veya diğer arama modelleri, veri Kataloğu Portalı'nda arama geliştirirken görüntüleme desteği.


## <a name="whats-new-for-august-2017"></a>Ağustos 2017 için yenilikler nelerdir? 
Ağustos 2017'den itibaren aşağıdaki özellikleri, Azure veri Kataloğu'na eklenmiştir:

*   Yeni bir geliştirici örnek oluşturma ve veri Kataloğu REST API'si kullanarak ilişkisi meta verileri yönetmek için kullanılabilir. *Veri kataloğuna ilişki bilgilerini içeri aktarma* örnek üzerinde kullanılabilir [veri Kataloğu kod örnekleri sayfasının](https://azure.microsoft.com/resources/samples/?service=data-catalog&sort=0). 
* Veri kaynağı kayıt aracını kullanarak ilgili tabloları kaydedilirken Teradata veri kaynaklarından ilişkisi meta verileri ayıklamak için destek katılın.
* Veri kaynağı kayıt aracını kullanarak SQL Server veri kaynaklarını kaydetme, SQL Server tablo değerli işlev (TVF) nesneleri için destek.
* Birden çok güncelleştirme ve veri Kataloğu Portalı'nın kullanılabilirliği ve performansı artırmak için iyileştirme.

## <a name="whats-new-for-july-2017"></a>Temmuz 2017 için yenilikler nelerdir? 
Temmuz 2017'den itibaren aşağıdaki özellikleri, Azure veri Kataloğu'na eklenmiştir:
*   Yasalar meta veri işlemleri de dahil olmak üzere üzerinde daha ayrıntılı denetim için destek:
    - Katalog yöneticileri, kullanıcıların etiketleri ve ilgili meta veri Kataloğu'na katkıda yeteneğini Kataloğu salt okunur erişimi etkinleştirme kısıtlayabilirsiniz.
    - Katalog yöneticileri, kullanıcıların yeni veri kaynaklarını kataloğa kaydetme yeteneğini kısıtlayabilirsiniz.
    - Katalog yöneticileri, kullanıcıların katalogdaki veri varlık meta verilerini sahipliğini yeteneğini kısıtlayabilirsiniz.
    - Azure Active Directory güvenlik grupları ve izinleri yönetme kolaylığı için kullanıcılara izinler verilebilir.
* Kayıtlı veri varlıklarını bulma ilgili veri varlıklarını veri Kataloğu portalında arasındaki ilişkiler için destek dahil olmak üzere:
    - SQL Server'ın (Azure SQL veritabanı dahil), Oracle ve MySQL ilişkisi meta veri ayıklama veri kaynakları veri Kataloğu veri kaynağı kayıt aracını kullanırken.
    - Varlık meta verileri veri Kataloğu Portalı'nda görüntülerken ilgili veri varlıklarını bulma.
    - İşlemleri tanımlamak için keşfetmek ve veri Kataloğu REST API'sini kullanarak veri varlıkları arasındaki ilişkileri Yönet.

Veri Kataloğu'nda izinleri yönetme ile ilgili ek ayrıntılar için bkz. [veri kataloğu ve veri varlıklarına erişim güvenliğinin nasıl sağlanacağını](data-catalog-how-to-secure-catalog.md).
Veri Kataloğu'nda ilişkileri hakkında ek ayrıntılar için bkz. [Azure veri Kataloğu'nda ilgili veri varlıklarını görüntüleme](data-catalog-how-to-view-related-data-assets.md).

## <a name="whats-new-for-june-2017"></a>Haziran 2017 için yenilikler nelerdir? 
Haziran 2017'den itibaren aşağıdaki özellikleri, Azure veri Kataloğu'na eklenmiştir:
*   Sybase, Apache Cassandra ve MongoDB için veri kaynaklarını destekler. Kullanıcılar artık kaydedebilir ve Cassandra ve MongoDB veritabanları ve tablolar ve Sybase veritabanı tabloları ve görünümleri keşfedin.

> [!NOTE]
> Kayıtları ve diziler gibi karmaşık veri türlerinde sütunlar içeren MongoDB ve Cassandra tabloları kaydedilirken bu sütunların veri Kataloğu'na eklenen meta verilerinde dahil edilmez.

## <a name="whats-new-for-may-2017"></a>Mayıs 2017 için yenilikler nelerdir? 
Mayıs 2017'den itibaren aşağıdaki özellikleri, Azure veri Kataloğu'na eklenmiştir:
*   Yeni bir geliştirici örnek, toplu iş sözlüğü terimleri içeri aktarma için kullanılabilir. Sözlük toplu olarak içeri aktarma örnek kullanılabilir [veri Kataloğu geliştirici örnekleri sayfası](https://docs.microsoft.com/azure/data-catalog/data-catalog-samples). 
*   Azure veri Kataloğu portalında ODBC bağlantı bilgilerini düzenlemek için destek. Veri varlığı sahipleri ve yöneticileri veri Kataloğu, veri kaynakları yeniden kaydetmeye gerek kalmadan kayıtlı ODBC veri kaynakları için bağlantı bilgilerini artık düzenleyebilirsiniz.
*   İş sözlüğü tanımları ve açıklamaları tıklanabilir URL'ler için destek. URL'ler için iş sözlüğü terimleri açıklama ve tanımı özelliklerinde eklendiğinde URL'leri, veri Kataloğu portalının köprü olarak görüntülenir.
*   Uzman e-posta adreslerini yanı sıra Uzman adlarını görüntüleme desteği. Özellikleri bir veri varlığı için uzmanlar veri Kataloğu Portalı'nda görüntülediğinizde, araç ipucunda Uzmanın Azure Active Directory'den tam adı görüntülenir.

## <a name="whats-new-for-april-2017"></a>Nisan 2017 için yenilikler nelerdir? 
Nisan 2017'den itibaren aşağıdaki özellikleri, Azure veri Kataloğu'na eklenmiştir:
*   ODBC veri kaynakları için destek. Kullanıcılar artık kaydedebilir ve ODBC veritabanları, tabloları ve görünümleri veri Kataloğu veri kaynağı kayıt aracını kullanarak keşfedin.

## <a name="whats-new-for-march-2017"></a>Mart 2017 için yenilikler nelerdir? 
Mart 2017'den itibaren aşağıdaki özellikleri, Azure veri Kataloğu'na eklenmiştir:
*   Veri Kataloğu yöneticileri için AAD güvenlik gruplarını kullanma desteği.
*   Azure veri Kataloğu, artık yeni bir Azure bölgesinde kullanılabilir. Müşteriler Azure veri Kataloğu bölgede Batı Orta ABD, Doğu ABD, Batı ABD, Batı Avrupa ve Avustralya Doğu, Kuzey Avrupa ve Güneydoğu Asya yanı sıra sağlayabilirsiniz. Daha fazla bilgi için [Azure bölgeleri](https://azure.microsoft.com/regions/).

## <a name="whats-new-for-february-2017"></a>Şubat 2017 yenilikleri 
Şubat 2017'den itibaren aşağıdaki özellikleri, Azure veri Kataloğu'na eklenmiştir:
*   Veri Kataloğu veri kaynağı kayıt aracını Gelişmiş ayarları için destek. Kullanıcılar, büyük veri kaynaklarını kaydederek olduğunda komut zaman aşımı değerlerini belirtebilirsiniz.
*   FTP ve PostgreSQL için veri kaynaklarını destekler. Kullanıcılar artık kaydedebilir ve FTP dosyaları ve dizinleri ve PostgreSQL veritabanları, tabloları ve görünümleri keşfedin.

## <a name="whats-new-for-january-2017"></a>Ocak 2017 için yenilikler nelerdir? 
Ocak 2017'den itibaren aşağıdaki özellikleri, Azure veri Kataloğu'na eklenmiştir:
*   Azure veri Kataloğu, artık [CSA STAR](https://www.microsoft.com/en-us/trustcenter/compliance/csa-star-certification) uyumlu.
*   İle tümleştirme [Al ve Dönüştür Excel 2016 ve Excel için Power Query](https://support.office.com/article/Introduction-to-Microsoft-Power-Query-for-Excel-6E92E2F4-2079-4E1F-BAD5-89F6269CD605). Excel kullanıcılarına sorguları paylaşın ve Azure veri Kataloğu'ndan kullanarak sorguları bulmak Excel içinde. Bu işlev, Power BI Pro lisansı olan kullanıcılar tarafından kullanılabilir.

## <a name="whats-new-for-december-2016"></a>Aralık 2016 için yenilikler nelerdir?
Aralık 2016'den itibaren aşağıdaki özellikleri, Azure veri Kataloğu'na eklenmiştir:
*   Azure veri Kataloğu, artık [HIPAA](https://www.microsoft.com/en-us/TrustCenter/Compliance/hipaa) ve [AB Model sözleşme maddeleri](https://www.microsoft.com/en-us/TrustCenter/Compliance/EU-Model-Clauses) uyumlu.
*   Veri kaynağı bağlantı bilgilerini düzenlemek için destek. Veri varlığı sahipleri ve yöneticileri veri Kataloğu, veri kaynakları yeniden kaydetmeye gerek kalmadan kayıtlı veri kaynakları için bağlantı bilgilerini artık düzenleyebilirsiniz.
*   Salesforce.com veri kaynakları için destek. Kullanıcılar artık kaydedebilir ve Salesforce nesneleri keşfedin.


## <a name="whats-new-for-november-2016"></a>Kasım 2016 için Yenilikler
Kasım 2016'den itibaren aşağıdaki özellikleri, Azure veri Kataloğu'na eklenmiştir:
*   Azure veri Kataloğu, artık [ISO/IEC 27001](https://www.microsoft.com/en-us/trustcenter/compliance/iso-iec-27001) ve [ISO/IEC 27018'i](https://www.microsoft.com/en-us/TrustCenter/Compliance/iso-iec-27018) uyumlu.
*   ODBC veri kaynakları veri Kataloğu portalını ve REST API'sini kullanarak el ile kayıt için destek.

## <a name="whats-new-for-september-2016"></a>Eylül 2016 için Yenilikler
Eylül 2016'den itibaren aşağıdaki özellikleri, Azure veri Kataloğu'na eklenmiştir:

* IBM DB2 veri kaynakları için destek. Kullanıcılar artık kaydedebilir ve DB2 veritabanları, tabloları ve görünümleri keşfedin.
* Azure Cosmos DB veri kaynakları için destek. Kullanıcılar artık kaydedebilir ve Cosmos DB veritabanları ve koleksiyonlarına keşfedin.
* Veri Kataloğu portalındaki katalog adı özelleştirme desteği. Katalog yöneticileri artık portalı başlığında, kuruluşunuzun adı gibi görüntülenen metni sağlar.

## <a name="whats-new-for-august-2016"></a>Ağustos 2016 için Yenilikler
Ağustos 2016'den itibaren aşağıdaki özellikleri, Azure veri Kataloğu'na eklenmiştir:

* SQL Server Master Data Services (MDS) veri kaynakları için kayıt iyileştirmeler. Kullanıcıları, artık veri Kataloğu veri kaynağı kayıt aracını kullanarak MDS varlıklar kaydedilirken önizlemeler ve veri profilleri içerebilir.
* Yönetici tarafından tanımlanan kuruluş kayıtlı aramalar için destek. Bir arama veri Kataloğu Portalı'nda kaydederken, veri Kataloğu yöneticileri artık aramaları tüm katalog kullanıcıları veya kişisel kullanım için kaydetmeyi seçebilirsiniz. Kuruluş kayıtlı aramalar, tüm Kataloğu kullanıcılarla paylaşılır ve standartlaştırılmış bir başlangıç noktası için veri kaynağı bulma sağlayabilir.
* Veri Kataloğu portalında özellikler görünümüne güncelleştirmeler. Tüm veri varlığı özelliklerini artık görüntülenir ve bir daha tutarlı sağlamak için tek, yeniden boyutlandırılabilir bölmesi ve bulunabilirlik deneyimi yönetilen.

## <a name="whats-new-for-july-2016"></a>Temmuz 2016 için Yenilikler
Temmuz 2016 itibariyle, aşağıdaki özellikleri, Azure veri Kataloğu'na eklenmiştir:

* SQL Server Master Data Services (MDS) için veri kaynaklarını destekler. Kullanıcılar artık kaydedebilir ve MDS modelleri ve varlıkları keşfedin.
* SQL Server saklı yordamları için destek. Kullanıcılar artık kaydedebilir ve SQL Server veri kaynaklarında saklı yordam nesnelerini bulur.
* Azure veri Kataloğu portalında ve 18 desteklenen diller toplam veri kaynağı kayıt aracını ek dil desteği. Azure veri Kataloğu kullanıcı deneyimini, Windows veya web tarayıcısında ayarlamak dil tercihlerine göre yerelleştirilir.
* Güncelleştirmeler ve veri Kataloğu portalı ana performans geliştirmeleri ve kolaylaştırılmış kullanıcı deneyimi de dahil olmak üzere sayfa, iyileştirme.

## <a name="whats-new-for-june-2016"></a>Haziran 2016 yenilikleri
Haziran 2016'den itibaren aşağıdaki özellikleri, Azure veri Kataloğu'na eklenmiştir:

* Veri varlıklarını veri Kataloğu portalında keşfederken liste görünümünde sütunları yeniden boyutlandırma için destek. Kullanıcılar artık daha kolay etiketleri ve açıklamaları gibi uzun varlık meta verileri okumak için ayrı sütunları yeniden boyutlandırabilirsiniz.
* Excel için Power Query veri Kataloğu Portalı'nda "Aç" menüsü eklendi. Kullanıcılar artık açabilir desteklenen veri kaynakları Excel 2016 veya Excel 2010 ve Excel 2013 ile [Excel için Power Query](https://support.office.com/article/Introduction-to-Microsoft-Power-Query-for-Excel-6E92E2F4-2079-4E1F-BAD5-89F6269CD605) eklentisi yüklendi.
* Azure tablo depolama veri kaynakları için destek. Kullanıcılar artık kaydedebilir ve Azure depolama veri kaynaklarında tablo nesnelerini bulur.

## <a name="whats-new-for-may-2016"></a>Mayıs 2016 için Yenilikler
Mayıs 2016 itibariyle, aşağıdaki özellikleri, Azure veri Kataloğu'na eklenmiştir:

* Katalog yöneticileri, iş terimlerini ve ortak bir iş sözlüğü oluşturmak için hiyerarşileri tanımlamanızı sağlayan bir iş sözlüğü. Kullanıcılar, kayıtlı veri varlıklarını bulmak ve kataloğun içeriği anlamak daha kolay hale getirmek için Sözlük terimlerini etiketleyebilir. Daha fazla bilgi için bkz. [İş Sözlüğünü Yönetilen Etiketleme için ayarlama](data-catalog-how-to-business-glossary.md)  
* Tek bir işlemde birden çok sözlük terimleri güncelleştirmek kullanıcılara veri Kataloğu iş sözlüğünü geliştirmeler. Kullanıcılar, aşağıdaki alanları düzenlemek için birden çok kullanım koşulları seçebilirsiniz:
  * Üst terim: Yeni bir üst terim kullanıcı seçebilir ve seçili olan tüm koşulları, seçili üst ermeden alt öğeleri olarak güncelleştirilir. Seçili tüm koşulları, aynı üst öğeye sahip, sonra da, üst metin kutusunda, aksi takdirde alanı üst terim gösterilir boş.   
  * Etiketleri ve proje katılımcıları: kullanıcılar ekleyebilir ve etiketleri ve proje katılımcıları için birden fazla veri varlığına etiketleme olarak aynı deneyimi kullanarak birden fazla sözlük terimleri kaldırabilirsiniz.

> [!NOTE]
> İş sözlüğü, yalnızca, Azure veri Kataloğu standart sürümü içinde kullanılabilir. Ücretsiz sürüm için bir iş sözlüğünü yönetilen etiketleme veya özellikleri sağlamaz.

## <a name="whats-new-for-march-2016"></a>Mart 2016 için Yenilikler
Mart 2016 itibariyle, aşağıdaki özellikleri, Azure veri Kataloğu: g eklenmiştir:

* Azure veri Kataloğu hizmet Kataloğu varlık yönetimi özellikleri ve arama özellikleri program aracılığıyla erişmek için bir birleştirilmiş REST API uç noktası. Bu arama API uç noktası ve kataloğu API'si uç noktası kullanım dışı ve 21 Mart 2016 tarihinde kullanımdan kaldırıldı. Herhangi bir değişiklik API semantiği vardır. Yalnızca uç noktası URI'si değiştirildi. Ek bilgi için bkz: [Azure veri Kataloğu REST API Başvurusu](https://msdn.microsoft.com/library/azure/mt267595.aspx). API örnekleri için bkz. [Azure veri Kataloğu geliştirici örnekleri](data-catalog-samples.md).

## <a name="whats-new-for-february-2016"></a>Şubat 2016 için Yenilikler
Şubat 2016'den itibaren aşağıdaki özellikleri, Azure veri Kataloğu'na eklenmiştir:

* Yeni baştan tasarlanan veri kaynağı seçimi Azure veri Kataloğu veri kaynağı kayıt aracını deneyin. Veri kaynağı kayıt aracını bulup veri kaynaklarından seçmek için Azure veri Kataloğu tarafından desteklenen kolaylaştırmak için güncelleştirildi.
* Azure veri Kataloğu portalında ve veri kaynağı kayıt aracını 10 ek dil için destek. İngilizce ek olarak, Azure veri Kataloğu deneyimi Almanca, İspanyolca, Fransızca, İtalyanca, Japonca, Korece, Portekizce (Brezilya), Rusça, Basitleştirilmiş Çince ve Geleneksel Çince kullanıma sunulmuştur. Azure veri Kataloğu kullanıcı deneyimini, Windows veya kullanıcının web tarayıcısında ayarlamak dil tercihlerine göre yerelleştirilir.
* Coğrafi çoğaltma, iş sürekliliği ve olağanüstü durum kurtarma için Azure veri Kataloğu veri desteği. Veri kaynağı meta verilerini ve kitle kaynaklı ek açıklamaları, dahil olmak üzere tüm Azure veri Kataloğu içeriği artık müşterilerine hiçbir ek ücret ödemeden iki Azure bölgeleri arasında çoğaltılır. Azure bölgeleri, en az 500 mil uzaklıkta önceden eşlenmiş ve eşleştirme açıklandığı gibi izleyin [iş sürekliliği ve olağanüstü durum kurtarma (BCDR): eşleştirilmiş Azure bölgeleri](../best-practices-availability-paired-regions.md).
* Azure veri Kataloğu tarafından kullanılan Azure aboneliğinin değiştirmek için destek. Azure veri Kataloğu yöneticileri, faturalama amacıyla farklı bir Azure aboneliği seçmek için Azure veri Kataloğu Portalı'nda ayarlar sayfasını kullanabilirsiniz.

## <a name="whats-new-for-january-2016"></a>Ocak 2016 için Yenilikler
Ocak 2016 itibariyle, aşağıdaki özellikleri, Azure veri Kataloğu'na eklenmiştir:

* El ile ek veri kaynaklarını kaydederek desteği. Şimdi, Azure veri Kataloğu Portalı'nda "El ile giriş oluştur" kullanabilir veya aşağıdaki veri kaynaklarını kaydetmek için Azure veri Kataloğu REST API'si kullanın:
  * OData - işlevi ve varlık kümesinin varlık kapsayıcısı
  * HTTP - dosya, uç nokta, rapor ve Site
  * Dosya sistemi - dosya
  * SharePoint - liste
  * FTP - dosya ve dizin
  * Salesforce.com - nesne
  * DB2 - tablo, Görünüm ve veritabanı
  * PostgreSQL - tablo, Görünüm ve veritabanı
* SQL Server'ın (Azure SQL veritabanı ve Azure SQL veri ambarı gibi) veri kaynakları için "Açık olarak SQL Server veri Araçları" için destek.  
* SAP HANA görünümleri ve paketleri bulma ve kaydetme desteği. Azure veri Kataloğu veri kaynağı kayıt aracı, açıklama ve Azure veri Kataloğu portalını kullanarak kayıtlı SAP HANA veri kaynaklarını bulma kullanarak SAP HANA veri kaynaklarını kaydedebilirsiniz.
* Sabitleme ve Azure veri Kataloğu Portalı'nda veri varlıklarını sabitleme olanağı. Yeniden Bul ve yeniden daha kolay hale getirmek için veri varlıklarını sabitleme seçebilirsiniz.
* Azure veri Kataloğu Portalı'nda yeni baştan tasarlanan bir giriş sayfası. Yeni giriş sayfası, bir bütün olarak kataloğunda son yayımlanan varlıkları, Sabitlenen varlıklar ve kayıtlı aramalar - ve etkinlik öngörü dahil olmak üzere geçerli kullanıcılar etkinliği - öngörü içerir.
* Azure veri Kataloğu Portalı'nda kalıcı kullanıcı ayarları desteği. Kullanıcı deneyimi ayarlarını - kılavuz veya kutucuk görünümü, sayfa ve isabet açıp vurgulama başına sonuç sayısı dahil olmak üzere - kullanıcı oturumları arasında kalıcıdır.
* Azure veri Kataloğu, artık iki yeni Azure bölgelerinde kullanılabilir. Müşteriler, Kuzey Avrupa ve Güneydoğu Asya bölgelerde, Doğu ABD, Batı ABD, Batı Avrupa ve Avustralya Doğu yanı sıra Azure veri Kataloğu hazırlayabilirsiniz. Daha fazla bilgi için [Azure bölgeleri](https://azure.microsoft.com/regions/).

> [!NOTE]
> "SQL Server veri araçları Aç" Visual Studio 2013 güncelleştirme 4 ve SQL Server araç yüklü olmasını gerektirir. SQL Server veri Araçları'nın en son sürümünü yüklemek için şurayı ziyaret edin [indirme SQL Server veri Araçları'nı (SSDT)](https://msdn.microsoft.com/library/mt204009.aspx).


## <a name="whats-new-for-december-2015"></a>Aralık 2015 için yenilikler nelerdir?
Aralık 2015'ten itibaren aşağıdaki özellikleri, Azure veri Kataloğu'na eklenmiştir:

* Azure SQL veri ambarı veri kaynakları için veri profilleri için destek. Azure SQL veri ambarı tabloları ve görünümleri kaydolurken kullanıcılar veri kaynağından ayıklanan meta verileri ile veri profili ölçümleri içerecek şekilde seçebilirsiniz.
* Kaydetme ve MySQL nesneleri ve veritabanlarını bulmak için destek. Kullanıcılar, Azure veri Kataloğu veri kaynağı kayıt aracı, açıklama ve Azure veri Kataloğu portalını kullanarak kayıtlı MySQL veri kaynaklarını bulma kullanarak MySQL veri kaynaklarını kaydedebilirsiniz.
* Teradata veri kaynakları için SPNEGO ve Windows kimlik doğrulaması için destek. Teradata tabloları ve görünümleri kaydolurken kullanıcılar SPNEGO ve Windows ve LDAP ve TD2 kimlik doğrulaması kullanarak Teradata için bağlanmayı seçebilirsiniz.
* Azure Data Lake Store veri kaynakları için destek. Kullanıcılar artık kaydedebilir ve Azure veri Kataloğu'nu kullanarak Azure Data Lake Store veri kaynakları keşfedin.
* Ağ proxy ayarları Azure veri Kataloğu veri kaynağı kayıt aracını el ile belirtmek için destek. Kullanıcılar Aracı'nın Karşılama sayfasından "Proxy ayarları değiştir" seçebilirsiniz ve araç tarafından kullanılacak bağlantı noktasını ve proxy adresi belirtebilirsiniz.

## <a name="whats-new-for-november-2015"></a>Kasım 2015 için yenilikler nelerdir?
Kasım 2015'ten itibaren aşağıdaki özellikleri, Azure veri Kataloğu'na eklenmiştir:

* Görüntüleme ve SQL Server'ın (Azure SQL veritabanı dahil) ve Oracle veri kaynakları için Azure veri Kataloğu portalındaki bağlantı dizeleri kopyalama olanağı. Kullanıcılar, SQL Server veya Oracle tablosu, görünümü veya veri kaynağına bağlanmak için kullanılan bağlantı dizeleri görmek için veritabanı için bağlantı bilgilerini "Bağlantı dizelerini görüntüle" bağlantısına tıklayabilirsiniz. ADO.NET, ODBC, OLEDB ve JDBC bağlantı dizeleri, SQL Server veri kaynakları için sağlanır. ODBC ve OLEDB bağlantı dizelerini Oracle veri kaynakları için sağlanır.
* Teradata tabloları ve görünümleri kaydederken veri profilleri de dahil olmak üzere desteği.
* "Açık olarak Power BI Desktop için" (Azure SQL DB ile Azure SQL veri ambarı dahil) SQL Server için SQL Server Analysis Services, Azure depolama ve HDFS kaynakları destekler.  
* Teradata veri kaynakları için LDAP kimlik doğrulaması için destek. Teradata tabloları ve görünümleri kaydolurken kullanıcılar Teradata için bağlanmak, LDAP kullanarak seçebilir ve TD2 kimlik doğrulaması.
* Teradata veri kaynakları için "Excel açık olarak" için destek.
* Azure veri Kataloğu Portalı'nda yeni bir arama terimi için destek. Portalda ararken, kullanıcıların bulma deneyimini hızlandırmak için son kullanılan arama terimlerden seçebilirsiniz.
* Teradata veri kaynakları için önizlemesi desteği. Teradata tabloları ve görünümleri kaydederken, kullanıcılar veri kaynağından ayıklanan meta verileri ile anlık görüntü kayıt eklemeyi seçebilirsiniz.
* Azure SQL veri ambarı veri kaynakları için "Excel açık olarak" için destek.
* Tanımlama ve el ile kayıtlı veri varlıklarını için sütun düzeyinde şemalarını düzenleme desteği. Kullanıcıları, el ile Azure veri Kataloğu portalını kullanarak bir veri varlığı oluşturduktan sonra veri varlığı özelliklerini sütun tanımları ekleyebilirsiniz.
* Belirli meta veriler sahip kayıtlı veri varlıklarının bulunmasını etkinleştirmek için Azure veri kataloğu arama yaparken "sahip" sorgu desteği. Azure veri Kataloğu sorgu söz dizimi artık içerir:

| Sorgu söz dizimi | Amaç |
| --- | --- |
| `has:previews` |Bir önizlemesini içeren veri varlıklarını bulur |
| `has:documentation` |Veri varlıklarını belgeleri sağlanan bulur |
| `has:tableDataProfiles` |Tablo düzeyi veri profili bilgileri ile veri varlıklarını bulur |
| `has:columnsDataProfiles` |Sütun düzeyinde veri profili bilgileri ile veri varlıklarını bulur |


> [!NOTE]
> "Power BI Desktop'ta Aç" geçerli bir sürüm Power BI Desktop uygulamasının yüklü olmasını gerektirir. Sorunları veya bu özelliği kullanarak hataları karşılaşırsanız, Power BI Desktop'tan en son sürümüne sahip olduğunuzdan emin olun [PowerBI.com](https://powerbi.com).


## <a name="whats-new-for-october-2015"></a>Ekim 2015 için yenilikler nelerdir?
Ekim 2015'ten itibaren aşağıdaki özellikleri, Azure veri Kataloğu'na eklenmiştir:

* Bekleyen veri şifreleme için kayıtlı veri kaynakları için Önizleme ve veri profillerini destekler. Azure veri Kataloğu herhangi bir önizleme kayıt şeffaf bir şekilde şifreler ve veri Kataloğu yöneticileri anahtar yönetimine gerek kalmadan hizmet kayıtlı veri kaynaklarını profiller.
* Teradata veri kaynakları için destek. Kullanıcılar artık kaydedebilir ve Teradata tabloları ve görünümleri keşfedin.
* Şirket içi Hive veri kaynakları için destek. Kullanıcılar artık kaydedebilir ve Apache Hive Hadoop şirket içi veri kaynakları için Hive tablolarını keşfedin.
* Azure veri Kataloğu Portalı'nda kayıtlı aramalar için destek. Kullanıcılar, arama terimleri kaydedin ve filtre seçimleri kolayca önceki aramaları yineleyin ve kataloğun içeriği yararlı görünümlerini tanımlamak için. Kullanıcı, kendi varsayılan arama kayıtlı bir aramayı de işaretleyebilirsiniz. Bir kullanıcı Azure veri Kataloğu portalı giriş sayfasından veya "Başlarken" sayfasındaki "Büyüteç" arama simgesine tıkladığında, kullanıcının doğrudan varsayılan bayrak eklenmiş kayıtlı arama için alınır.
* Zengin metin belgeleri için kayıtlı veri varlıklarını ve Azure veri Kataloğu portalında kapsayıcılar için destek. Kullanıcılar artık, tablolar, görünümler ve raporlar gibi veri varlıklarına ve veritabanları ve modeller, etiketleri ve açıklamaları yeterli olmadığı senaryoları gibi kapsayıcılar için belgeleri sağlayabilir.
* El ile bilinen bir veri kaynağı türlerini kaydetme desteği. Kullanıcılar, Azure veri Kataloğu tarafından desteklenen tüm veri kaynağı türleri için Azure veri Kataloğu portalı kullanarak veri kaynağı bilgilerini el ile girebilirsiniz.
* Azure Active Directory güvenlik grupları yetkilendirmek için destek. Katalog yöneticileri güvenlik grupları, kullanıcı hesapları, Azure veri Kataloğu'na erişimi yönetmek kolaylaştırarak katalog erişimi etkinleştirebilir.
* Hive veri kaynakları, Azure veri Kataloğu Portalı'ndan Excel'de açma desteği.

> [!NOTE]
> Geçerli sürüme yönelik kimlik doğrulaması Teradata TD2 desteklenir. Ek kimlik doğrulama mekanizmaları gelecekte desteklenir serbest bırakır.

> [!NOTE]
> Hive veri kaynakları için "Açık Excel'de Çözümle" özelliğini kullanmak için kullanıcılar için Hive ODBC sürücüsü olarak yüklemiş olmalısınız.

## <a name="whats-new-for-september-2015"></a>Eylül 2015 için yenilikler nelerdir?
Eylül 2015'ten itibaren aşağıdaki özellikleri, Azure veri Kataloğu'na eklenmiştir:

* Hive veri kaynaklarını kaydederek veri profilleri gibi desteği.
* Programlama yoluyla Azure veri Kataloğu ile tümleştirmek için uygulamaların kolaylaştırmaya Kataloğu API'sini keşfetmek için destek.
* "Başlarken" yeni veri kaynağı Azure veri Kataloğu Portalı'nda bulma deneyimini. Kullanıcılar, bir arama terimi girerek olmadan Azure veri Kataloğu Portalı'nın "bulmak" sayfasında girdiğinde, bunlar en sık kullanılan etiketleri, uzmanlar, veri kaynağı türleri ve nesne türleri dahil olmak üzere katalog içeriği ile bir genel bakış sunulur.
* Kaydetme ve Azure SQL veri ambarı nesneleri ve veritabanlarını bulmak için destek. Azure SQL veri ambarı hakkında ek bilgi için bkz. [SQL veri ambarı](https://azure.microsoft.com/services/sql-data-warehouse/).
* Kaydetme ve SQL Server Analysis Services modellerine ve SQL Server Reporting Services sunucu kapsayıcıları olarak keşfetmek için destek. Azure veri Kataloğu, SSAS ve SSRS nesneler kaydedilirken SSAS modeline ve SSRS sunucusuna ve raporlar ve diğer nesneler için bir giriş oluşturur. Kapsayıcıları, bulunan ve Azure veri Kataloğu portalını kullanarak ek açıklama. Kullanıcılar ayrıca arama ve filtre bir model veya aramayı ve filtrelemeyi katalog içeriğini yanı sıra sunucu içeriği.
* Kaydetme ve HTTP/HTTPS üzerinden SQL Server Analysis Services nesneleri bulmak için destek. Kullanıcılar artık bir URL kullanarak SSAS sunucularına bağlanma (gibi https://servername/olap/msmdpump.dll) yerine bir sunucu adı ve temel kimlik doğrulaması ve Windows kimlik doğrulaması yanı sıra anonim bağlantılar kullanabilirsiniz. SSAS HTTP/HTTPS bağlantıları hakkında ek bilgi için bkz. [Analysis Services'e yönelik HTTP erişimi Yapılandır](https://msdn.microsoft.com/library/gg492140.aspx).
* HDInsight üzerinde Hive veri kaynakları için destek. Kullanıcılar artık kaydedebilir ve veri kaynaklarında HDInsight Hadoop, Apache Hive için Hive tablolarını keşfedin. HDInsight üzerindeki Hive'a hakkında ek bilgi için bkz. [HDInsight Belge Merkezi'ne](../hdinsight/hadoop/hdinsight-use-hive.md).
* Kaydetme ve Oracle veritabanları ve kapsayıcıları olarak HDFS kümeleri bulma desteği. Azure veri Kataloğu, Oracle tabloları ve görünümleri veya HDFS kaydederken, veritabanı, tablolar ve görünümler için bir giriş oluşturur. Veritabanı bağlantısı bulunabilir ve Azure veri Kataloğu portalını kullanarak ek açıklama. Kullanıcılar ayrıca arama ve bir veritabanı veya aramayı ve filtrelemeyi katalog içeriğini yanı sıra küme içeriğini filtre.
* Bilinmeyen veri kaynağı türleri el ile kaydetmek için destek. Kullanıcılar, böylece açıkça veri kaynağı kayıt aracı tarafından desteklenen veri kaynakları açıklama ve bulunan Azure veri Kataloğu portalı kullanarak veri kaynağı bilgilerini el ile girebilirsiniz.
* Kaydetme ve SQL Server veritabanları kapsayıcıları olarak keşfetmek için destek. Azure veri Kataloğu, SQL Server tabloları ve görünümleri kaydederken, veritabanı, tablolar ve görünümler için bir giriş oluşturur. Veritabanı bağlantısı bulunabilir ve Azure veri Kataloğu portalını kullanarak ek açıklama. Kullanıcılar, aynı zamanda arama ve aramayı ve filtrelemeyi katalog içeriğini yanı sıra bir veritabanının içeriğini filtreleyin.

> [!NOTE]
> Kayıtlı önceki 18 Eylül sürümüne yükseltilmiş olan SSAS ve SSRS nesne modeli veya sunucu giriş kataloğa eklenmeden önce veri kaynağı kayıt aracını kullanarak yeniden kaydedilmesi gerekir. Bir veri kaynağı'nı yeniden kaydederek Azure veri Kataloğu portalında kullanıcılar tarafından eklenmiş olan tüm ek açıklamaları etkilemez.

> [!NOTE]
> Oracle tabloları ve görünümleri ve HDFS dosya ve dizinleri kayıtlı önceki 11 Eylül sürümüne yükseltilmiş olan veritabanı veya küme girişi kataloğa eklenmeden önce veri kaynağı kayıt aracını kullanarak yeniden kaydedilmesi gerekir. Bir veri kaynağı'nı yeniden kaydederek Azure veri Kataloğu portalında kullanıcılar tarafından eklenmiş olan tüm ek açıklamaları etkilemez.

> [!NOTE]
> SQL Server tabloları ve kayıtlı önceki Eylül 4 sürümüne yükseltilmiş olan görünümleri veritabanı girişi kataloğa eklenmeden önce veri kaynağı kayıt aracını kullanarak yeniden kaydedilmesi gerekir. Bir veri kaynağı'nı yeniden kaydederek Azure veri Kataloğu portalında kullanıcılar tarafından eklenmiş olan tüm ek açıklamaları etkilemez.

## <a name="whats-new-for-august-2015"></a>Ağustos 2015 için yenilikler nelerdir?
Ağustos 2015'ten itibaren aşağıdaki özellikleri, Azure veri Kataloğu'na eklenmiştir:

* SQL Server ve Oracle veri kaynaklarından veri profil oluşturma desteği. SQL Server ve Oracle tabloları ve görünümleri kaydederken, kullanıcıların kayıtlı nesneler için veri profil bilgilerini içerecek şekilde seçebilirsiniz. Veri profili, nesne düzeyinde ve sütun düzeyindeki istatistikleri içerir.
* Hadoop HDFS veri kaynakları için destek. Kullanıcılar artık kaydedebilir ve HDFS dosya ve dizinleri keşfedin.
* Kayıtlı veri kaynakları için erişim isteği bilgilerini sağlamak için destek. Herhangi bir kayıtlı veri varlığı için kullanıcılar artık var olan araçlarınız ve süreçlerinizle kolayca tümleştirmek amacıyla e-posta bağlantılarını veya URL'ler gibi erişim isteme yönergeleri sağlar.
* Etiketleri ve hangi kullanıcıların hangi meta verileri için kayıtlı veri varlıklarını sağlamış bulmak daha kolay hale getirmek için uzmanları için araç ipuçları.
* Bizim için üst gezinti çubuğundaki yeni "Kullanıcı" düğmesi ve menüsü ekledik. Azure veri Kataloğu'na oturum açmak için kullanılan hesap kullanıcı bu menüyü ve varsa oturumu kapatmak için istenen. Bu menü, ayrıca Azure veri Kataloğu REST API'si kullanan geliştiriciler için değerlidir katalog adı görüntüler.
* Yalnızca standart sürüm: sahipleri veri varlıklarına eklerken, Azure veri Kataloğu artık hem kullanıcı hesaplarına hem de güvenlik grupları sahip olarak destekler. Seçili veri varlıkları için bir sahip olarak bir güvenlik grubu eklemek için varsa, grubun görünen adını veya grubun UPN e-posta adresi, girebilirsiniz.
* Azure Blob Depolama veri kaynakları için destek. Kullanıcılar artık kaydedebilir ve Azure depolama blobları ve dizinleri keşfedin.

