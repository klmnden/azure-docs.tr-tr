---
title: "Bağlayıcısı sürüm yayımlama geçmişi | Microsoft Docs"
description: "Bu konu bağlayıcıları tüm sürümleri, Forefront Identity Manager (FIM) ve Microsoft Identity Manager (MIM) için listeler."
services: active-directory
documentationcenter: 
author: fimguy
manager: mtillman
editor: 
ms.assetid: 6a0c66ab-55df-4669-a0c7-1fe1a091a7f9
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/06/2017
ms.author: fimguy
ms.openlocfilehash: 3fbdc60a21aa16926bc4db00f41ade8ecda415f1
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="connector-version-release-history"></a>Bağlayıcı Sürümü Yayınlama Geçmişi
Forefront Identity Manager (FIM) ve Microsoft Identity Manager (MIM) bağlayıcıları sık sık güncelleştirilir.

> [!NOTE]
> Bu konuda FIM ve MIM yalnızca bilinmiyor. Bu bağlayıcıların üzerinde Azure AD Connect yüklemesi için desteklenmiyor. Yükseltme yapı belirtildiğinde yayımlanan bağlayıcılar AADConnect üzerinde önceden yüklenmiş.


Bu konuda çıkarılan bağlayıcılarının tüm sürümlerini listeler.

İlgili bağlantılar:

* [En son bağlayıcılar indirin](http://go.microsoft.com/fwlink/?LinkId=717495)
* [Genel LDAP Bağlayıcısı](active-directory-aadconnectsync-connector-genericldap.md) başvuru belgelerini
* [Genel SQL bağlayıcı](active-directory-aadconnectsync-connector-genericsql.md) başvuru belgelerini
* [Web Hizmetleri Bağlayıcısı](http://go.microsoft.com/fwlink/?LinkID=226245) başvuru belgelerini
* [PowerShell Bağlayıcısı](active-directory-aadconnectsync-connector-powershell.md) başvuru belgelerini
* [Lotus Domino Bağlayıcısı](active-directory-aadconnectsync-connector-domino.md) başvuru belgelerini

## <a name="116490-aadconnect-116490"></a>1.1.649.0 (AADConnect 1.1.649.0)

### <a name="fixed-issues"></a>Giderilen sorunlar:

* Lotus Notes:
  * Özel certifiers seçeneği filtreleme
  * İçeri aktarma ImportOperations sınıfının hangi işlemleri 'Görünümleri' modu ve hangi 'Arama' modunda çalıştırılabilir tanım sabit.
* Genel LDAP:
  * OpenLDAP Directory DN entryUUI yerine bağlantı olarak kullanır. Bağlantı değiştirilecek veren GLDAP bağlayıcıya yeni seçeneği
* Genel SQL:
  * Sabit verme varbinary(max) türünde alanına.
  * İkili veriler CSEntry nesnesine bir veri kaynağından eklerken, çevirmelerinde işlevi sıfır bayt üzerinde başarısız oldu. CSEntryOperationBase sınıfının sabit çevirmelerinde işlev.




### <a name="enhancements"></a>Geliştirmeleri:

* Genel SQL:
  * Yürütme modunu yapılandırma yeteneğini saklı yordamı adlandırılmış parametreleri ile veya adlandırılmamış 'Genel parametrelerini' sayfasındaki Genel SQL Yönetim Aracısı'nın bir yapılandırma penceresinde eklenir. Sayfanın 'Genel parametrelerini' yok 'execute depolanan yordamla modu sorumlu bir saklı yordamı yürütmek için adlandırılmış parametreleri Kullan' etiketli onay kutusu parametreleri veya adı verilir.
    * Şu anda, adlandırılmış parametreleri içeren saklı yordamı yürütme olanağı yalnızca veritabanları için IBM DB2 ve MSSQL çalışır. Oracle ve MySQL veritabanları için bu yaklaşım çalışmıyor: 
      * MySQL, SQL sözdizimi, adlandırılmış parametreleri depolanmış yordamları desteklemiyor.
      * Oracle için ODBC sürücüsü, saklı yordamlar adlandırılmış parametreleri için adlandırılmış parametreleri desteklemiyor)

## <a name="116040-aadconnect-116140"></a>1.1.604.0 (AADConnect 1.1.614.0)


### <a name="fixed-issues"></a>Giderilen sorunlar:

* Genel Web Hizmetleri:
  * İki veya daha fazla uç noktaları zamanki oluşturulan bir SOAP proje engelleyen bir sorun düzeltilmiştir.
* Genel SQL:
  * Alma işleminde GSQL saati doğru bağlayıcı alanı kaydedildiğinde dönüştürülürken değil. GSQL bağlayıcı alanı için varsayılan tarih ve saat Biçim 'yyyy-aa-gg: ssZ ', 'yyyy-aa-gg için: ssZ' değiştirildi.

## <a name="115510-aadconnect-115530"></a>1.1.551.0 (AADConnect 1.1.553.0)

### <a name="fixed-issues"></a>Giderilen sorunlar:

* Genel Web Hizmetleri:
  * Wsconfig Aracı'nı doğru şekilde isteğinden"örnek" REST hizmeti yöntemi için Json dizisi dönüştürmemenizi. Bu, bu Json dizisi REST isteği için seri hale getirme sorunlara neden oldu.
  * Web Hizmeti Bağlayıcısı Yapılandırması aracını JSON öznitelik adları alanı simgeleri kullanımını desteklemiyor 
    * Değiştirme deseni el ile WSConfigTool.exe.config dosyasına örneğin eklenebilir```<appSettings> <add key=”JSONSpaceNamePattern” value="__" /> </appSettings>```

* Lotus Notes:
  * Zaman seçeneği **kuruluşun/kuruluş birimleri için özel certifiers izin** Bağlayıcısı'nı (güncelleştirme) dışa aktarma sırasında başarısız sonra tüm öznitelikleri Domino ancak dışarı aktarma zamanında dışarı verme akış sonra devre dışı bir KeyNotFoundException eşitlemeye izin verilir. 
    * Aşağıdaki öznitelikler birini değiştirerek DN (kullanıcı adı özniteliği) değiştirmeye çalışırsa yeniden adlandırma işlemi başarısız olduğu için bu gerçekleşir:  
      - Soyadı
      - FirstName
      - MiddleInitial
      - AltFullName
      - AltFullNameLanguage
      - OU
      - altcommonname

  * Zaman **kuruluşun/kuruluş birimleri için özel certifiers izin** seçeneği etkin ancak gerekli certifiers hala boş sonra KeyNotFoundException oluşur.

### <a name="enhancements"></a>Geliştirmeleri:

* Genel SQL:
  * **Senaryo: Gerçekleştirilmedi yeniden tasarlanmıştır:** "*" özelliği
  * **Çözüm açıklaması:** değiştirilmiş bir yaklaşım [birden çok değerli başvuru öznitelikleri işleme](active-directory-aadconnectsync-connector-genericsql.md).


### <a name="fixed-issues"></a>Giderilen sorunlar:

* Genel Web Hizmetleri:
  * Web Hizmeti Bağlayıcısı varsa, sunucu yapılandırmasını içeri aktarılamıyor
  * Web Hizmeti Bağlayıcısı ile birden çok Web Hizmetleri çalışmıyor

* Genel SQL:
  * Tek değer başvurulan özniteliği için hiçbir nesne türleri listelenir
  * Delta içeri aktarma nesnesindeki değeri birden çok değerli tablosundan kaldırıldığında değişiklik izleme stratejisi siler
  * OverflowException GSQL Bağlayıcısı ile DB2 AS / 400

Lotus:
  * OU'lar GlobalParameters sayfa açmadan önce arama enable\disable eklenen seçeneği

## <a name="114430"></a>1.1.443.0

Yayımlanma tarihi: 2017 Mart

### <a name="enhancements"></a>Geliştirmeler

* Genel SQL:</br>
  **Senaryo Belirtiler:** SQL burada biz yalnızca bir nesne türüne bir başvuru izin ve çapraz başvuru üyeleriyle gerektiren Bağlayıcısı ile iyi bilinen bir sınırlama geçerlidir. </br>
  **Çözüm açıklaması:** başvuruları için işlem adımda olan "*" seçeneği seçildiğinde, nesne türlerinin tüm bileşimleri eşitleme altyapısında döndürülür.

>[!Important]
- Bu çok sayıda yer tutucuları oluşturur
- Adlandırma nesne türleri benzersiz olduğundan emin olmak için gereklidir.


* Genel LDAP:</br>
 **Senaryo:** yalnızca birkaç kapsayıcıları belirli bir bölüm seçildiğinde sonra arama hala tüm bölümünde yapılır. Özel eşitleme hizmeti, ancak bir değil, performans düşüşüne neden MA filtrelenir. </br>

 **Çözüm açıklaması:** tüm kapsayıcıları aracılığıyla olası Git yapmak ve her biri tüm bölümünde arama yerine nesneleri aramak için değiştirilmiş GLDAP bağlayıcı'nın kodu.


* Lotus Domino:

  **Senaryo:** Domino posta silme bir dışa aktarma sırasında kişi kaldırma desteği. </br>
  **Çözüm:** bir dışa aktarma sırasında kişi kaldırılmak üzere yapılandırılabilir posta silme desteği.

### <a name="fixed-issues"></a>Giderilen sorunlar:
* Genel Web Hizmetleri:
 * Aşağıdaki hata olur sonra hizmet URL'si varsayılan değiştirirken SAP wsconfig WebService yapılandırma aracı aracılığıyla projeleri: yolunun bir bölümü bulunamadı.

      ``'C:\Users\cstpopovaz\AppData\Local\Temp\2\e2c9d9b0-0d8a-4409-b059-dceeb900a2b3\b9bedcc0-88ac-454c-8c69-7d6ea1c41d17\cfg.config\cloneconfig.xml'. ``

* Genel LDAP:
 * AD LDS tüm öznitelikleri GLDAP bağlayıcı görmez
 * LDAP dizini şemadan UPN özniteliklere algılandığında Sihirbazı sonları
 * Delta içeri aktarmalar başarısız bulma hatalı "objectclass" özniteliği seçilmediğinde tam içeri aktarma sırasında mevcut değil
 * Bir "Bölümleri ve hiyerarşileri Yapılandır" yapılandırma sayfası değil Göster herhangi bir nesne türü için genel yeni sunucuları bölümü eşittir  
LDAP MA. Bunlar yalnızca nesneleri RootDSE bölümünden gösterdi.


* Genel SQL:
 * Delta içeri aktarma öznitelikte içeri hata Genel SQL filigran için düzeltme
 * Birden çok değerli öznitelik deleted\added değerlerini verilirken deleted\added veri kaynağındaki değiller.  


* Lotus Notes:
 * Belirli bir alan "Tam adı" için Notlar verilirken özniteliğinin değeri Null veya boş. ancak meta veri deposunda doğru şekilde gösterilir.
 * Yinelenen Certifier hata düzeltme
 * Lotus Domino Bağlayıcısı diğer nesnelerle üzerinde herhangi bir veri olmadan nesne seçildiğinde sonra bulma hatası tam içeri aktarma işlemi gerçekleştirilirken aldığımız.
 * Delta içeri aktarma olduğunda Lotus Domino Bağlayıcısı, o çalışma sonunda Microsoft.IdentityManagement.MA.LotusDomino.Service.exe çalıştıran hizmet bazen döndürür bir uygulama hatası.
 * Genel grup üyelikleri düzgün çalışır ve bir kullanıcıyı üyelikten kaldırmak denemek için dışa aktarma çalıştırırken içeren bir güncelleştirme başarılı olarak gösterir, ancak kullanıcının gerçekten Lotus Notes üyeliğinin kaldırılmaları değil dışında tutulur.
 * Dışarı aktarma modunu "Append öğesi en altındaki" GUI, birden çok değerli öznitelikler için dışa aktarma sırasında en altındaki yeni öğeler eklemek için Lotus MA yapılandırmasında eklendiği gibi seçmek için bir fırsat.
 * Bağlayıcı posta klasörü ve kimliği kasası dosyayı silmek için gerekli mantığı ekleyeceksiniz.
 * Üyelik için NAB üye çalışmıyor silin.
 * Değerleri başarıyla birden çok değerli özniteliğinden silinmelidir

## <a name="111170"></a>1.1.117.0
Yayımlanma tarihi: 2016 Mart

**Yeni bir bağlayıcı**  
İlk sürümü [Genel SQL bağlayıcı](active-directory-aadconnectsync-connector-genericsql.md).

**Yeni Özellikler:**

* Genel LDAP Bağlayıcısı:
  * Delta içeri aktarma Isode ile desteği eklendi.
* Web Hizmetleri Bağlayıcısı:
  * CsEntryChangeResult ve nesne düzeyi hataları eşitleme altyapısında döndürülecek izin vermek için setImportErrorCode etkinliğiyle güncelleştirildi.
  * Yeni nesne düzeyinde hata işlevselliği kullanmak için SAP6 ve SAP6User şablonları güncelleştirildi.
* Lotus Domino Bağlayıcısı:
  * Dışarı aktarma için adres defteri başına bir certifier gerekir. Artık tüm certifiers için aynı parola yönetimini kolaylaştırmak için de kullanabilirsiniz.

**Giderilen sorunlar:**

* Genel LDAP Bağlayıcısı:
  * IBM Tivoli DS için bazı başvuru öznitelikleri doğru algılanmadı.
  * Delta içeri aktarma sırasında açık LDAP için başında ve dizeleri sonuna boşluk kesildi.
  * Novell ve NetIQ için OU/kapsayıcıları arasında ve aynı zamanda bir nesne taşındı verme başarısız nesne yeniden adlandırıldı.
* Web Hizmetleri Bağlayıcısı:
  * Ardından web hizmeti aynı bağlama için birden çok uç noktalarının olsaydı, bağlayıcı Bu uç noktalarının doğru bulamadı.
* Lotus Domino Bağlayıcısı:
  * Bir verme bir posta veritabanına fullName özniteliğinin çalışmadı.
  * Hem eklendi ve üye bir gruptan kaldırılan bir dışa aktarma yalnızca eklenen üyelerin verildi.
  * Notlar belge geçersiz (öznitelik IsValid false olarak ayarlayın), bağlayıcı başarısız ise.

## <a name="older-releases"></a>Eski sürümleri
Mart 2016 öncesinde bağlayıcıları destek konuları yayımlanmıştır.

**Genel LDAP**

* [KB3078617](https://support.microsoft.com/kb/3078617) -1.0.0597, Eylül 2015
* [KB3044896](https://support.microsoft.com/kb/3044896) -1.0.0549, Mart 2015
* [KB3031009](https://support.microsoft.com/kb/3031009) -1.0.0534, Ocak 2015
* [KB3008177](https://support.microsoft.com/kb/3008177) -1.0.0419, 2014 Eylül
* [KB2936070](https://support.microsoft.com/kb/2936070) -4.3.1082, Mart 2014

**Webservices'a**

* [KB3008178](https://support.microsoft.com/kb/3008178) -1.0.0419, 2014 Eylül

**PowerShell**

* [KB3008179](https://support.microsoft.com/kb/3008179) -1.0.0419, 2014 Eylül

**Lotus Domino**

* [KB3096533](https://support.microsoft.com/kb/3096533) -1.0.0597, Eylül 2015
* [KB3044895](https://support.microsoft.com/kb/3044895) -1.0.0549, Mart 2015
* [KB2977286](https://support.microsoft.com/kb/2977286) -5.3.0712, 2014 Ağustos
* [KB2932635](https://support.microsoft.com/kb/2932635) -5.3.1003, Şubat 2014  
* [KB2899874](https://support.microsoft.com/kb/2899874) -5.3.0721, Ekim 2013
* [KB2875551](https://support.microsoft.com/kb/2875551) -5.3.0534, 2013 Ağustos

## <a name="troubleshooting"></a>Sorun giderme 

> [!NOTE]
> Microsoft Identity Manager veya AADConnect kullanımı ECMA2 bağlayıcılar hiçbirini ile güncelleştirirken. 

Eşleştirilecek yükseltme sırasında bağlayıcı tanımı yenilemeniz gerekir veya kimliği 6947 uyarı rapor için uygulama olay günlüğü Başlat şu hatayla karşılaşırsınız: "derleme sürümünü AAD Bağlayıcı yapılandırması ("X.X.XXX. "X") ("X.X.XXX. gerçek sürümden daha eski "X"), "C:\Program Files\Microsoft Azure AD Sync\Extensions\Microsoft.IAM.Connector.GenericLdap.dll".

Tanımı yenilemek için:
* Bağlayıcı örneği özelliklerini açın
* Bağlantıyı tıklatın / bağlanmak için sekmesi
  * Bağlayıcı hesabı için parolayı girin
* Özellik sekmelerin her birini sırayla tıklatın
  * Bu bağlayıcı türü bir bölüm sekme varsa Yenile düğmesini ile bu sekmedeki Yenile düğmesini tıklatın.
* Tüm özellik sekmeleri eriştikten sonra değişiklikleri kaydetmek için Tamam düğmesini tıklatın.


## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi edinmek [Azure AD Connect eşitleme](active-directory-aadconnectsync-whatis.md) yapılandırma.

[Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](active-directory-aadconnect.md) hakkında daha fazla bilgi edinin.
