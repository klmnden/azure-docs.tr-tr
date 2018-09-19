---
title: Bağlayıcı sürümü yayın geçmişi | Microsoft Docs
description: Forefront Identity Manager (FIM) ve Microsoft Identity Manager (MIM) için bu konuda bağlayıcıları'nın tüm sürümleri listeler
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
editor: ''
ms.assetid: 6a0c66ab-55df-4669-a0c7-1fe1a091a7f9
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/22/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: 95f2ffb1a51184f1194f87a4a5e9a54e682edf80
ms.sourcegitcommit: cf606b01726df2c9c1789d851de326c873f4209a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/19/2018
ms.locfileid: "46312007"
---
# <a name="connector-version-release-history"></a>Bağlayıcı Sürümü Yayınlama Geçmişi
Forefront Identity Manager (FIM) ve Microsoft Identity Manager (MIM) bağlayıcıları sık sık güncelleştirilir.

> [!NOTE]
> Bu konu yalnızca FIM ve MIM yöneliktir. Bu bağlayıcıları için Azure AD Connect yüklenmesi desteklenmez. İçin yükseltme derleme belirtildiğinde yayımlanan bağlayıcılar AADConnect üzerinde önceden yüklenmiş.


Bu konuda, yayımlanmış bağlayıcılarının tüm sürümlerini listeler.

İlgili bağlantılar:

* [En son bağlayıcı indirme](http://go.microsoft.com/fwlink/?LinkId=717495)
* [Genel LDAP Bağlayıcısı](https://docs.microsoft.com/microsoft-identity-manager/reference/microsoft-identity-manager-2016-connector-genericldap) başvuru belgeleri
* [Genel SQL Bağlayıcısı](https://docs.microsoft.com/microsoft-identity-manager/reference/microsoft-identity-manager-2016-connector-genericsql) başvuru belgeleri
* [Web Hizmetleri Bağlayıcısı](https://docs.microsoft.com/microsoft-identity-manager/reference/microsoft-identity-manager-2016-ma-ws) başvuru belgeleri
* [PowerShell Bağlayıcısı](https://docs.microsoft.com/microsoft-identity-manager/reference/microsoft-identity-manager-2016-connector-powershell) başvuru belgeleri
* [Lotus Domino Bağlayıcısı](https://docs.microsoft.com/microsoft-identity-manager/reference/microsoft-identity-manager-2016-connector-domino) başvuru belgeleri


## <a name="118300"></a>1.1.830.0

### <a name="fixed-issues"></a>Giderilen sorunlar:
* Çözümlenen ConnectorsLog System.Diagnostics.EventLogInternal.InternalWriteEvent(Message: A device attached to the system is not functioning)
* Bu bağlayıcılar sürümünde 3.3.0.0-4.1.3.0 bağlama yeniden yönlendirme 4.1.4.0 miiserver.exe.config içinde için güncelleştirmeniz gerekir
* Genel Web Hizmetleri:
    * Çözümlenen geçerli bir JSON yanıtı Yapılandırma aracında kaydedilemedi
* Genel SQL:
    * Dışarı aktarma, yalnızca güncelleştirme sorgu silme işlemi için her zaman oluşturur. Delete sorgusu oluşturmak için eklendi
    * 'Delta stratejisi' 'Değişiklik izleme' ise, Delta alma işlemi için nesneleri alır SQL sorgu düzeltildi. Bu uygulamada sınırlama bilinen: Delta içeri aktarma modu 'Değişiklik izleme' birden çok değerli öznitelikler değişiklikleri izleme
    * Öznitelikte son değer silmek gereklidir ve bu satırı silmek gerekli olan değer dışında herhangi bir veri içermez, çalışması için silme sorgusu oluşturmasını olanağı eklendi.
    * İşleme System.ArgumentException çıkış parametreleri SP tarafından uygulanan 
    * Geçersiz sorgu dışarı aktarma işlemi alanına VARBINARY(max) türüne sahip olmak için
    * Sorunu parameterList değişkeni iki kez (işlevleri ExportAttributes ve GetQueryForMultiValue) başlatıldı


## <a name="116490-aadconnect-116490"></a>1.1.649.0 (AADConnect 1.1.649.0)

### <a name="fixed-issues"></a>Giderilen sorunlar:

* Lotus Notes:
  * Özel certifiers seçeneği filtreleme
  * İçeri aktarma ImportOperations sınıfının tanımı hangi işlemleri 'Görünümleri' modu ve hangi 'Ara' modunda çalıştırılabilir düzeltildi.
* Genel LDAP:
  * OpenLDAP Directory DN entryUUI yerine bağlantı olarak kullanır. Yeni bağlantı değiştirilecek veren GLDAP bağlayıcı seçeneği
* Genel SQL:
  * Sabit dışarı aktarma VARBINARY(max) türü olan bir alana.
  * İkili verileri bir veri kaynağından CSEntry nesnesine eklerken, sıfır bayt çevirmelerinde işlevi başarısız oldu. Sabit çevirmelerinde işlevi CSEntryOperationBase sınıf.




### <a name="enhancements"></a>Geliştirmeleri:

* Genel SQL:
  * Yürütme modu yapılandırma becerisi saklı yordam adlandırılmış parametreler veya adlandırılmamış bir yapılandırma penceresinde 'Genel parametreleri' sayfasındaki Genel SQL Yönetim Aracısı'nın eklenir. Sayfada 'Genel parametreleri' var 'modu için yürütme depolanan yordamla sorumlu bir saklı yordamı yürütmek için adlandırılmış parametreleri Kullan' etiketli onay kutusu veya parametreleri adlandırılır.
    * Şu anda yalnızca veritabanları için IBM DB2 ve MSSQL adlandırılmış parametreli saklı yordamı yürütme yeteneği çalışır. Oracle ve MySQL veritabanları için bu yaklaşım çalışmıyor: 
      * MySQL SQL sözdizimi, adlandırılmış parametreler saklı yordamları desteklemez.
      * Oracle için ODBC sürücüsü, saklı yordamlardaki adlandırılmış parametreler için adlandırılmış parametreleri desteklemez)

## <a name="116040-aadconnect-116140"></a>1.1.604.0 (AADConnect 1.1.614.0)


### <a name="fixed-issues"></a>Giderilen sorunlar:

* Genel Web Hizmetleri:
  * İki veya daha fazla uç nokta zamanki oluşturulan bir SOAP proje engelleyen bir sorun düzeltildi.
* Genel SQL:
  * İçeri aktarma işleminde GSQL zaman doğru bağlayıcı alanına kaydedildiğinde dönüştürerek değil. GSQL'ın bağlayıcı alanında varsayılan tarih ve saat Biçim 'yyyy-aa-gg için ssZ' 'yyyy-aa-gg ssZ' değiştirildi.

## <a name="115510-aadconnect-115530"></a>1.1.551.0 (AADConnect 1.1.553.0)

### <a name="fixed-issues"></a>Giderilen sorunlar:

* Genel Web Hizmetleri:
  * Wsconfig Aracı'nı doğru şekilde REST hizmeti yöntemi için "örnek istek" Json dizisi dönüştürmemenizi. Bu, bu Json dizisi REST isteği için serileştirme ile ilgili sorunlar nedeniyle.
  * Web Hizmeti Bağlayıcısı yapılandırma aracını JSON öznitelik adları boşluk sembolleri kullanımını desteklemiyor 
    * Değiştirme deseni el ile WSConfigTool.exe.config dosyasına örn eklenebilir ```<appSettings> <add key=”JSONSpaceNamePattern” value="__" /> </appSettings>```
> [!NOTE]
> Dışarı aktarma için şu hatayı alırsınız gibi JSONSpaceNamePattern anahtarı gereklidir: ileti: boş adı geçerli değil. 

* Lotus Notes:
  * Zaman seçeneği **özel certifiers izin vermek için kuruluş/kuruluş birimi** Bağlayıcısı'nı (güncelleştirme) dışarı aktarma sırasında başarısız olur. tüm öznitelikler Domino ancak dışarı aktarma anında dışarı verme akışı sonra devre dışı bir KeyNotFoundException eşitlemeye izin verilir. 
    * Aşağıdaki özniteliklerden birini değiştirdiğinizde DN (kullanıcı adı özniteliği) değiştirileceğini çalıştığında, yeniden adlandırma işlemi başarısız olduğundan bu gerçekleşir:  
      - LastName
      - FirstName
      - MiddleInitial
      - AltFullName
      - AltFullNameLanguage
      - Kuruluş birimi
      - altcommonname

  * Zaman **özel certifiers izin vermek için kuruluş/kuruluş birimi** seçeneği etkin olduğunda, ancak gerekli certifiers hala boş sonra KeyNotFoundException gerçekleşir.

### <a name="enhancements"></a>Geliştirmeleri:

* Genel SQL:
  * **Senaryo: Yeniden tasarlanmış uygulanır:** "*" özelliği
  * **Çözüm açıklaması:** değiştirilen yaklaşımı [başvurusu birden çok değerli öznitelikler işleme](https://docs.microsoft.com/microsoft-identity-manager/reference/microsoft-identity-manager-2016-connector-genericsql).


### <a name="fixed-issues"></a>Giderilen sorunlar:

* Genel Web Hizmetleri:
  * Web Hizmeti Bağlayıcısı mevcutsa sunucu yapılandırmasını içeri aktarılamıyor
  * Web Hizmeti Bağlayıcısı ile birden çok Web Hizmetleri çalışmıyor

* Genel SQL:
  * Tek değer başvurulan özniteliği için hiçbir nesne türleri listelenir
  * Delta içeri aktarma nesnedeki değer birden çok değerli tablosundan kaldırıldığında değişiklik izleme stratejisi siler
  * OverflowException GSQL Bağlayıcısı ile DB2 AS / 400

Lotus:
  * OU'lar GlobalParameters sayfa açmadan önce arama enable\disable eklenen seçeneği

## <a name="114430"></a>1.1.443.0

Yayımlanma tarihi: Mart 2017

### <a name="enhancements"></a>Geliştirmeler

* Genel SQL:</br>
  **Senaryo Belirtiler:** , bir SQL burada yalnızca bir nesne türü için bir başvuru izin ve gerekli üyelerle çapraz başvuru Bağlayıcısı ile iyi bilinen sınırlamasıdır. </br>
  **Çözüm açıklaması:** işleme adımını başvurular için içinde bulunduğunuz "*" seçeneği belirlenirse, nesne türlerinin tüm bileşimleri eşitleme altyapısında döndürülür.

>[!Important]
- Bu çok sayıda yer tutucu oluşturur
- Adlandırma nesne türlerini benzersiz olduğundan emin olmak için gereklidir.


* Genel LDAP:</br>
 **Senaryo:** belirli bölümünde yalnızca birkaç kapsayıcıları seçildiğinde, ardından arama hala tüm bölümünde yapılır. Özel eşitleme hizmeti, ancak bir performans düşüşüne neden değil MA filtrelenir. </br>

 **Çözüm açıklaması:** değiştirilen GLDAP bağlayıcının kod tüm kapsayıcıları aracılığıyla olası Git yapın ve her biri tüm bölümünde arama yerine nesneleri arayın.


* Lotus Domino:

  **Senaryo:** Domino posta silme desteği bir dışarı aktarma sırasında bir kişi kaldırma. </br>
  **Çözüm:** bir dışarı aktarma sırasında bir kişi kaldırılmak üzere yapılandırılabilir posta silme desteği.

### <a name="fixed-issues"></a>Giderilen sorunlar:
* Genel Web Hizmetleri:
 * Aşağıdaki hata oluşur sonra hizmet URL'si varsayılan değiştirilirken SAP wsconfig WebService yapılandırma aracı ile projeleri: yolun bir parçası bulunamadı

      ``'C:\Users\cstpopovaz\AppData\Local\Temp\2\e2c9d9b0-0d8a-4409-b059-dceeb900a2b3\b9bedcc0-88ac-454c-8c69-7d6ea1c41d17\cfg.config\cloneconfig.xml'. ``

* Genel LDAP:
 * AD LDS tüm öznitelikler GLDAP bağlayıcı görmez
 * LDAP dizini şemadan UPN özniteliklere algılandığında Sihirbazı sonu
 * Delta içeri aktarmalar "objectclass" özniteliği yok seçildiğinde tam içeri aktarma sırasında mevcut bulma hatalarla başarısız
 * Bir "Bölümleri ve hiyerarşileri Yapılandır" yapılandırma sayfası değil Göster herhangi bir nesne türü için genel yeni sunucular için bölüm eşittir  
LDAP MA. Bunlar yalnızca RootDSE bölüm nesnelerden gösterdi.


* Genel SQL:
 * Genel SQL eşiği değişikliği içeri aktarma öznitelikte içeri hata düzeltildi
 * Birden çok değerli öznitelik değerlerini deleted\added aktarırken, bunlar veri kaynağındaki deleted\added değildir.  


* Lotus Notes:
 * Notlar verilirken özniteliğinin değeri Null veya boş. ancak belirli bir alan "Tam adı" meta veri deposunda doğru şekilde gösterilir.
 * Yinelenen Certifier hatası için düzeltme
 * Lotus Domino Bağlayıcısı ile diğer nesneler üzerinde herhangi bir veri olmadan nesne seçildiğinde sonra bulma hatası tam içeri aktarma gerçekleştirilirken aldığımız.
 * Delta içeri aktarma olduğunda Lotus Domino bağlayıcı, bu çalışma sonunda Microsoft.IdentityManagement.MA.LotusDomino.Service.exe çalıştıran hizmet bazen döndürür bir uygulama hatası.
 * Genel grup üyeliği düzgün çalışır ve bir kullanıcıyı üyelikten kaldırma denemek için dışarı aktarma çalıştırırken bir güncelleştirme ile başarılı olarak gösterir ancak kullanıcı gerçekten Lotus Notes üyelikten kaldırıldı değil dışında tutulur.
 * "Ekle öğesi altındaki" GUI, birden çok değerli öznitelikler verme sırasında en altındaki yeni öğeler eklemek için Lotus MA yapılandırması eklendiği gibi dışarı aktarma modu için bir fırsat.
 * Bağlayıcı, posta klasörü ve kimliği kasa dosyayı silmek için gerekli mantık ekleyeceksiniz.
 * Platformlar arası NAB üye çalışmıyor üyelik silin.
 * Değerleri birden çok değerli özniteliği başarıyla silinmesi gerekir

## <a name="111170"></a>1.1.117.0
Yayımlanma tarihi: Mart 2016

**Yeni bir bağlayıcı**  
Başlangıç sürümü [Genel SQL Bağlayıcısı](https://docs.microsoft.com/microsoft-identity-manager/reference/microsoft-identity-manager-2016-connector-genericsql).

**Yeni Özellikler:**

* Genel LDAP Bağlayıcısı:
  * Isode ile değişikliği içeri aktarma için destek eklendi.
* Web Hizmetleri Bağlayıcısı:
  * CsEntryChangeResult etkinliği ve nesne düzeyi hataları eşitleme altyapısı için geri döndürülecek izin vermek için setImportErrorCode etkinliği güncelleştirildi.
  * Yeni nesne düzeyinde hata işlevselliği kullanmak için SAP6 ve SAP6User şablonlarını güncelleştirdik.
* Lotus Domino Bağlayıcısı:
  * Dışarı aktarma için adres defteri başına bir certifier gerekir. Yönetimini kolaylaştırmak için artık tüm certifiers için aynı parolayı kullanabilirsiniz.

**Giderilen sorunlar:**

* Genel LDAP Bağlayıcısı:
  * IBM Tivoli DS için bazı başvuru öznitelikleri düzgün algılanmadı.
  * Delta içeri aktarma sırasında açık LDAP için boşluk dizeleri sonunda ve başındaki kesildi.
  * Örnek olarak Novell ve NetIQ için OU'ları / kapsayıcılar arasında ve aynı zamanda bir nesne taşınan bir dışarı aktarma başarısız nesne yeniden adlandırıldı.
* Web Hizmetleri Bağlayıcısı:
  * Ardından web hizmeti aynı bağlama için birden çok uç noktaya olsaydı, bağlayıcı Bu uç noktalarının doğru bulamadı.
* Lotus Domino Bağlayıcısı:
  * Bir posta veritabanına tam adı özniteliğinin bir dışarı aktarma başarısız oldu.
  * Hem eklendi ve üye bir gruptan kaldırılan bir dışarı aktarma, eklenen üyeler yalnızca verildi.
  * Bir notları adlı belge geçersiz (öznitelik IsValid false olarak ayarlayın) ve ardından bağlayıcı başarısız olması durumunda.

## <a name="older-releases"></a>Eski sürümler
Mart 2016'dan önce bağlayıcıları destek konuları yayımlanmıştır.

**Genel LDAP**

* [KB3078617](https://support.microsoft.com/kb/3078617) -1.0.0597, Eylül 2015
* [KB3044896](https://support.microsoft.com/kb/3044896) -1.0.0549, Mart 2015
* [KB3031009](https://support.microsoft.com/kb/3031009) -1.0.0534, Ocak 2015
* [KB3008177](https://support.microsoft.com/kb/3008177) -1.0.0419, Eylül 2014
* [KB2936070](https://support.microsoft.com/kb/2936070) -4.3.1082, Mart 2014

**Veritabanınızdaki**

* [KB3008178](https://support.microsoft.com/kb/3008178) -1.0.0419, Eylül 2014

**PowerShell**

* [KB3008179](https://support.microsoft.com/kb/3008179) -1.0.0419, Eylül 2014

**Lotus Domino**

* [KB3096533](https://support.microsoft.com/kb/3096533) -1.0.0597, Eylül 2015
* [KB3044895](https://support.microsoft.com/kb/3044895) -1.0.0549, Mart 2015
* [KB2977286](https://support.microsoft.com/kb/2977286) -5.3.0712, Ağustos 2014
* [KB2932635](https://support.microsoft.com/kb/2932635) -5.3.1003, Şubat 2014  
* [KB2899874](https://support.microsoft.com/kb/2899874) -5.3.0721, Ekim 2013
* [KB2875551](https://support.microsoft.com/kb/2875551) -5.3.0534, Ağustos 2013

## <a name="troubleshooting"></a>Sorun giderme 

> [!NOTE]
> Microsoft Identity Manager veya AADConnect ECMA2 bağlayıcılardan herhangi biri ile kullanımı güncelleştirirken. 

Bağlayıcı tanımını eşleşecek şekilde yükseltme sonrasında yenilemeniz gerekir veya kimliği 6947 uyarı bildirmek için uygulama olay günlüğü başlatılırken şu hatayı alırsınız: "derleme sürümü AAD Bağlayıcı yapılandırması ("X.X.XXX. "X") ("X.X.XXX. gerçek sürümden daha eski "X"), "C:\Program Files\Microsoft Azure AD'ye Sync\Extensions\Microsoft.IAM.Connector.GenericLdap.dll".

Tanımını yenilemek için:
* Bağlayıcı örneği için özellikleri açın
* Bağlantıya tıklayarak / bağlanmak için sekmesinde
  * Bağlayıcı hesabı için parolayı girin
* Özellik sekmelerin her birini sırayla tıklayın
  * Bu bağlayıcı türü bir bölüm sekme varsa bir yenileme düğmesi ile bu sekmedeki yenile düğmesine tıklayın.
* Tüm özellik sekmeleri erişilen sonra değişiklikleri kaydetmek için Tamam düğmesine tıklayın.


## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi edinin [Azure AD Connect eşitleme](how-to-connect-sync-whatis.md) yapılandırma.

[Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md) hakkında daha fazla bilgi edinin.
