---
title: 'Azure AD Connect: Sürüm yayımlama geçmişi | Microsoft Docs'
description: Bu makalede, Azure AD Connect ve Azure AD eşitleme'nın tüm sürümleri listelenir.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: ef2797d7-d440-4a9a-a648-db32ad137494
ms.service: active-directory
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/18/2018
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7ab0e7ce1891dc3553f89f652c7dac70e66a4df0
ms.sourcegitcommit: f24fdd1ab23927c73595c960d8a26a74e1d12f5d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2019
ms.locfileid: "58499922"
---
# <a name="azure-ad-connect-version-release-history"></a>Azure AD Connect: Sürüm yayınlama geçmişi
Azure Active Directory (Azure AD) ekibi, düzenli olarak yeni özellikler ve işlevler ile Azure AD Connect güncelleştirir. Tüm eklemeleri için tüm kitlelere yönelik uygulanabilir.


Bu makalede, yayımlanan sürümleri izlemenize yardımcı olmak ve en son sürümde değişikliklerin neler olduğunu anlama açısından tasarlanmıştır.

Bu tablo, ilgili konular bir listesi verilmiştir:

Konu |  Ayrıntılar
--------- | --------- |
Azure AD Connect'ten yükseltme adımları | Farklı yöntemlere [en son önceki sürümden yükseltme](how-to-upgrade-previous-version.md) Azure AD Connect sürümü.
Gerekli izinler | Bir güncelleştirmeyi uygulamak için gereken izinler için bkz: [hesapları ve izinleri](reference-connect-accounts-permissions.md#upgrade).

İndir | [Azure AD Connect'i indirme](https://go.microsoft.com/fwlink/?LinkId=615771).

>[!NOTE]
>Tüm sürümleri Azure AD Connect otomatik yükseltme için kullanılabilir hale getirilir. Yayın durumu, bir yayın otomatik yükseltme için veya yalnızca karşıdan yükleme için kullanılabilir hale getirileceğini olup olmadığını gösterir. Ardından Azure AD Connect sunucunuzda otomatik yükseltmeyi etkinleştirilmişse, sunucu otomatik olarak otomatik yükseltme için yayımlanan Azure AD Connect'in en son sürümünü yükseltecek. Tüm Azure AD Connect yapılandırmaları otomatik yükseltme için uygun olduğunu unutmayın. Lütfen hakkında daha fazla bilgi için bu bağlantıyı izleyin [otomatik yükseltme](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-install-automatic-upgrade)

## <a name="13900"></a>1.3.90.0 

### <a name="release-status"></a>Yayın durumu 

3/25/2019: Henüz serbest 
 
### <a name="new-features-and-improvements"></a>Yeni özellikler ve geliştirmeler 



- Etki alanı yenileme için destek eklendi 
- Exchange posta ortak klasörleri özelliğini GA gider 
- Hizmet hataları için işleme Sihirbazı hatası geliştirin 
- Bağlayıcı özellikleri sayfasında eski kullanıcı Arabirimi için uyarı bağlantı eklendi. 
- Birleştirilmiş grup geri yazma özelliğini GA sunulmuştur 
- DC bir LDAP denetimi eksik olduğunda geliştirilmiş SSPR hata iletisi 
- Yükleme sırasında DCOM kayıt hataları için ek tanılama  
- İyileştirilmiş PHS RPC hatalarını izleme 
- Bir alt etki alanından EA güvenilirlikleri izin ver 
- İzin ver (varsayılan ad ADSync) yükleme sırasında girdiğiniz veritabanı adı
- Bir WS-Trust düzeltme için Ping çekmek ve yeni Azure örnekleri için destek eklemek için ADAL 3.19.8 yükseltme 
- Değiştirme grubu eşitleme kurallarını samAccountName DomainNetbios ve buluta - DomainFQDN akış talep için gereken 
- Varsayılan eşitleme kuralı işleme – daha fazla bilgi edinin değiştiren [burada](how-to-connect-fix-default-rules.md).
- Bir windows hizmeti olarak çalışan yeni bir Aracısı eklendi. "Yönetim Aracı" adlı bu aracı, Microsoft destek talebinde bulunun ortaya çıkan sorun giderme Engineers yardımcı olmak için Azure AD Connect sunucusunun uzak daha ayrıntılı tanılama sağlar. Yönetim Aracısı hakkında daha fazla bilgiyi [burada](whatis-aadc-admin-agent.md). 
- Son Kullanıcı Lisans Sözleşmesi (EULA) güncelleştirildi 
- Kullanıcıların oturum açma türü AD FS kullanan dağıtımlar için Otomatik yükseltme desteği eklendi.  Bu ayrıca, yükseltme işleminin bir parçası AD FS'yi Azure AD bağlı olan taraf güveni güncelleştirme gereksinimi kaldırıldı. 
- İki seçenekler sunan bir Azure AD güven yönetim görevi ekledik: güven analiz/güncelleştir ve güvenini sıfırlama. 
- Her zaman - SupportMultipleDomain anahtar kullanır şekilde AD FS'yi Azure AD bağlı olan taraf güveni davranış değiştirilmiştir (güven ve Azure AD etki alanını içeren güncelleştirmeleri). 
- Önceden yüklenmiş bir sertifika kullanma seçeneğini kaldırarak bir .pfx sertifika gerektirir yükleme yeni AD FS grubu davranış değiştirilmiştir.
- Yükleme yeni AD FS grubu iş akışı yalnızca 1 AD FS ve WAP sunucusu 1 dağıtma tanır şekilde güncelleştirildi.  İlk yüklemeden sonra tüm ek sunucular yapılır. 

### <a name="fixed-issues"></a>Düzeltilen sorunlar 


- Düzeltme SQL yeniden ADSync hizmeti için mantığı 
- Boş bir SQL AOA DB kullanarak temiz yükleme izin vermek için düzeltme 
- GWB izinleri iyileştirmek için izinleri PS betiği düzeltin 
- LocalDB ile VSS hataları düzeltin  
- Nesne türü, kapsam içinde değil yanıltıcı bir hata iletisi düzeltildi 
- Azure AD PowerShell yüklemesi bir sunucu üzerinde büyük olasılıkla bir birleştirme çakışması Azure AD Connect ile neden olduğu sorunu düzeltildi. 
- Bağlayıcı kimlik bilgileri eski kullanıcı Arabiriminde güncelleştirildiğinde hazırlama sunucusunda PHS hata düzeltildi. 
- Bazı bellek sızıntılarını düzeltildi 
- Çeşitli Autoupgrade düzeltmeleri 
- Çeşitli düzeltmeleri dışa ve içe aktarma onaylanmamış işleniyor 
- Etki alanı ve OU ters eğik çizgi işleme ile bir hata düzeltildi filtreleme 
- Burada ADSync hizmeti durdurmak için birden fazla 2 dakika sürer ve yükseltme zamanında bir soruna neden olan sorun düzeltildi. 




## <a name="12700"></a>1.2.70.0

### <a name="release-status"></a>Yayın durumu

12/18/2018: İndirme için yayımlanan

### <a name="fixed-issues"></a>Düzeltilen sorunlar

Bu derleme, Azure AD Connect ile birlikte gelen standart bağlayıcılar (örneğin, genel LDAP Bağlayıcısı ve genel SQL Bağlayıcısı) güncelleştirir. Sürümünde 1.1.911.0 uygun bağlayıcılar hakkında daha fazla bilgi için bkz. [bağlayıcı sürümü yayın geçmişi](/microsoft-identity-manager/reference/microsoft-identity-manager-2016-connector-version-history).


## <a name="12690"></a>1.2.69.0

### <a name="release-status"></a>Yayın durumu
12/11/2018: İndirme için yayımlanan

### <a name="fixed-issues"></a>Düzeltilen sorunlar
Bu düzeltme derleme cihaz geri yazmayı etkinleştirdiğinizde RegisteredDevices kapsayıcının belirtilen orman içindeki hedef bir etki alanı seçmesini sağlar.  Yeni cihaz seçenekleri işlevleri içeren önceki sürümlerinde (1.1.819.0 – 1.2.68.0), RegisteredDevices kapsayıcı konumunu orman köküne sınırlıydı ve alt etki alanları izin vermedi.  Yeni dağıtımlarda, kendisi yalnızca bu sınırlama bildirilen: yerinde yükseltme etkilenmez.  

Güncelleştirilmiş cihaz seçenekleri işlevleri içeren herhangi bir yapı yeni bir sunucuya dağıtılan ve cihaz geri yazma etkinleştirildi, bu orman kök dizininde istemiyorsanız, kapsayıcı konumunu el ile belirtmek gerekir.  Bunu yapmak için cihaz geri yazmayı devre dışı bırakabilir ve "Geri yazma ormanı" sayfasında kapsayıcı konumunu belirtmek izin veren yeniden etkinleştirmeniz gerekir.



## <a name="12680"></a>1.2.68.0

### <a name="release-status"></a>Yayın durumu 

11/30/2018:  İndirme için yayımlanan

### <a name="fixed-issues"></a>Düzeltilen sorunlar

Bu düzeltme derleme, burada kimlik doğrulama hatası MSOnline PowerShell Galerisi modüldeki eşitleme sunucusu bağımsız varlığı nedeniyle ortaya çıkabilir çakışma düzeltir.



## <a name="12670"></a>1.2.67.0

### <a name="release-status"></a>Yayın durumu 

11/19/2018:  İndirme için yayımlanan

### <a name="fixed-issues"></a>Düzeltilen sorunlar

Bu düzeltme derleme bir gerileme, parola geri yazma özelliğini bir EKLER etki alanı denetleyicisi Windows Server 2008/R2'de kullanırken nerede başarısız önceki yapı düzeltir.

## <a name="12650"></a>1.2.65.0 

### <a name="release-status"></a>Yayın durumu 

25/10/2018: indirme için yayımlanan

 
### <a name="new-features-and-improvements"></a>Yeni özellikler ve geliştirmeler 


- Öznitelik barındırılan sesli posta beklendiği gibi çalıştığından emin olmak için geri yazma işlevini değiştirildi.  Bazı senaryolarda, Azure AD, bir null değer ile geri yazma sırasında msExchUcVoicemailSettings öznitelik üzerine.  Bulut değeri ayarlanmadığı taktirde azure AD'ye artık şirket içi bu özniteliğin değeri, artık temizler.
- Tanılama, araştırmak ve Azure AD'ye bağlantı sorunlarını belirlemek için Azure AD Connect sihirbazındaki eklendi. Bu aynı Tanılama, Test - AdSyncAzureServiceConnectivity cmdlet'ini kullanarak doğrudan Powershell aracılığıyla da çalıştırılabilir. 
- Tanılama, araştırmak ve AD'ye bağlantı sorunlarını belirlemek için Azure AD Connect sihirbazındaki eklendi. Bu aynı tanılama ADConnectivityTools Powershell modülünde başlangıç ConnectivityValidation işlevi kullanılarak doğrudan Powershell aracılığıyla da çalıştırılabilir.  Daha fazla bilgi için [ADConnectivityTool PowerShell Modülü nedir?](how-to-connect-adconnectivitytools.md)
- Hibrit Azure Active Directory Join ve cihaz geri yazma için bir AD şeması sürüm öncesi denetimi eklendi 
- Duyarlı olmayan bir durumda olmasını dizini uzantısı sayfa özniteliği arama değişti.
-   TLS 1.2 için tam destek eklendi. Bu sürüm, diğer protokolleri devre dışı bırakılmasını ve yalnızca Azure AD Connect, yüklü olduğu makinede etkinleştiriliyor TLS 1.2 destekler.  Daha fazla bilgi için [Azure AD Connect için TLS 1.2 zorlama](reference-connect-tls-enforcement.md)

 

### <a name="fixed-issues"></a>Düzeltilen sorunlar   

- SQL Always On kullanılıyorsa, burada Azure AD Connect yükseltmesi başarısız olur bir hata düzeltildi. 
- Eğik çizgi içeren OU adları doğru şekilde ayrıştırmak için bir hata düzeltildi. 
- Burada geçişli kimlik doğrulaması için hazırlama modunda temiz bir yükleme devre dışı bir sorun düzeltildi. 
- PowerShell modülü, sorun giderme araçları çalıştırırken yüklenmesini engelleyen bir hata düzeltildi 
- Bir ana bilgisayar adının ilk karakteri sayısal değerleri kullanarak müşterilerin engelleyen bir hata düzeltildi. 
- Geçersiz bölüm ve kapsayıcı seçimi Azure AD Connect izin burada bir hata düzeltildi 
- Masaüstü SSO'yu etkinleştirildiğinde "Geçersiz parola" hata iletisi düzeltildi. 
- AD FS güven yönetimi için çeşitli hata düzeltmeleri  
- Cihaz geri yazma - yapılandırma (WS2012 R2'de sunulmuştur) msDs-DeviceContainer nesne sınıfı aramak için şema denetimi düzeltildi

 
## <a name="118820"></a>1.1.882.0  

7/9/2018: yükleme için yayınlanan sürüm için otomatik yükseltmeyi olmaz 

### <a name="fixed-issues"></a>Düzeltilen sorunlar  

SQL her zaman üzerinde kullanılabilirlik ADSync DB için yapılandırıldıysa azure AD Connect yükseltmesi başarısız olur. Bu düzeltme, bu sorunu giderir ve yükseltmesinin başarılı olması için sağlar. 

## <a name="118800"></a>1.1.880.0

### <a name="release-status"></a>Yayın durumu

8/21/2018: İndirme ve Otomatik yükseltme için yayımladı. 

### <a name="new-features-and-improvements"></a>Yeni özellikler ve geliştirmeler

- Azure AD CONNECT'te Ping federasyona tümleştirmesi artık genel kullanılabilirlik için kullanılabilir. [Federasyon Azure AD Federasyonu Ping ile kullanma hakkında daha fazla bilgi edinin](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-user-signin#federation-with-pingfederate)
- Bir güncelleştirme yapıldığında ve gerektiğinde kolayca geri yükleme için ayrı bir dosyada depolar her zaman azure AD Connect artık Azure AD güvenini AD FS'de yedeğini oluşturur. [Azure AD ve yeni işlevler hakkında daha fazla güven Azure AD CONNECT'te yönetim bilgi](https://aka.ms/fedtrustinaadconnect).
- Yeni sorun giderme araçları birincil e-posta adresini değiştirmek ve genel adres listesinde hesabından gizleme gidermenize yardımcı olur.
- Azure AD Connect'in en son SQL Server 2012 Native Client'ı içerecek şekilde güncelleştirildi
- Sorunsuz çoklu oturum açma onay kutusunu, kullanıcı oturum açma için parola karması eşitleme veya doğrudan kimlik doğrulama "Değişiklik kullanıcı oturum açma" görevde geçiş yaptığınızda, varsayılan olarak etkindir.
- Windows Server Essentials 2019 desteği eklendi
- Azure AD Connect Health aracısını en son sürüme 3.1.7.0 güncelleştirildi
- Yükleyici varsayılan eşitleme kuralları için değişiklik algılarsa, yükseltme sırasında yönetim değiştirilmiş kuralları yazmadan bir uyarı istenir. Bu düzeltme girişimlerinde bulunun ve daha sonra devam etmesi olanak tanır. Eski davranışı: Varsa herhangi bir değişiklik out-of-box kural daha sonra el ile yükseltme Bu kurallar kullanıcıya hiçbir uyarı vermeden üzerine ve Eşitleme Zamanlayıcısı kullanıcı bildiren olmadan devre dışı bırakıldı. Yeni davranışı: Kullanıcı, değiştirilen hazır olan eşitleme kuralları yazmadan uyarıyla istenir. Kullanıcı, yükseltme işlemini durdurun ve daha sonra düzeltici eylem aldıktan sonra devam seçeneğine sahip olur.
- Bir daha iyi bir hata iletisi için FIPS uyumlu ortamı ve bu sorun için geçici bir çözüm sağlayan belgeleri bağlantısı MD5 karması oluşturmayı sağlayan bir FIPS uyumluluk sorununu işleme sağlar.
- Kullanıcı Arabirimi artık ayrı bir alt grubu Federasyon altında olan Federasyon görevleri Sihirbazı'nda geliştirmek için güncelleştirin. 
- Tüm Federasyon ek görevler artık, kullanım kolaylığı için tek bir alt menüsü altında gruplandırılır.
- (Bu kısa bir süre içinde kullanım dışı) eski ADSyncPrep.psm1 taşındı yeni AD izinleri işlevleri ile yeni bir ADSyncConfig Posh Modülü (AdSyncConfig.psm1) modernize

### <a name="fixed-issues"></a>Düzeltilen sorunlar 

- . NET'e 4.7.2 yükseltme yaptıktan sonra AAD Connect sunucusu yüksek CPU kullanımına burada gösterebilir bir hata düzeltildi
- Aralıklı olarak bir hata iletisi için bir Otomatik çözümlenen SQL kilitlenme sorunu neden bir hata düzeltildi
- Eşitleme kuralları Düzenleyicisi ve Eşitleme Hizmeti Yöneticisi için çeşitli erişilebilirlik sorunlar düzeltildi  
- Azure AD Connect kayıt defteri ayarı bilgi edinebileceğiniz olmayan bir hata düzeltildi
- Kullanıcı sihirbazda ileri/geri gittiğinde sorunları oluşturulan bir hata düzeltildi
- Bir hatayı sihirbazda gönderdikten yanlış çoklu iş parçacığı nedeniyle oluşmasını önlemek için bir hata düzeltildi
- Eşitleme grubu filtreleme sayfa güvenlik grupları çözülürken LDAP hatayla karşılaştığında, Azure AD Connect artık tam uygunlukta durumla döndürür.  Başvuru özel durumu için kök neden hala bilinmiyor ve farklı bir hatadan ele alınacaktır.
-  Burada STK ve NGC anahtarları (kullanıcı/cihaz nesneleri WHfB için ms-DS-KeyCredentialLink özniteliği) izinlerini düzgün ayarlanmamış bir hata düzeltildi.     
- 'Set-ADSyncRestrictedPermissions' burada doğru şekilde çağrılmadı bir hata düzeltildi
-  Grup geri yazma AADConnect'ın Yükleme Sihirbazı'nda verme izni için destek ekleme
- Oturum açma yöntemi, parola karması eşitleme, AD FS'ye değiştirirken, parola karması eşitleme devre dışı değil.
- AD FS yapılandırmasında IPv6 adresleri için ek doğrulama
- Mevcut bir yapılandırmayı var olduğunu bildiren bir bildirim iletisi güncelleştirildi.
- Güvenilmeyen ormana kapsayıcı algılamak cihaz geri yazma başarısız. Bu, daha iyi hata iletisi ve uygun belgelere bağlantı sağlamak için güncelleştirildi
- Bir OU ve ardından OU genel eşitleme hatası verdiğini karşılık gelen eşitleme/geri yazma seçimini. Bu, daha anlaşılır bir hata iletisi oluşturmak üzere değiştirildi.

## <a name="118190"></a>1.1.819.0

### <a name="release-status"></a>Yayın durumu

5/14/2018: Otomatik yükseltme ve yükleme için yayımladı.

### <a name="new-features-and-improvements"></a>Yeni özellikler ve geliştirmeler

Yeni özellikler ve geliştirmeler

- Bu sürüm, Azure AD Connect'e bağlanan PingFederate tümleştirmesini genel önizlemesini içerir. Bu sürümle birlikte, müşteriler kolayca ve güvenilir bir şekilde, Federasyon sağlayıcısı olarak PingFederate yararlanmak için Azure Active Directory ortamlarının yapılandırın. Bu yeni özelliğin nasıl kullanılacağı hakkında daha fazla bilgi edinmek için lütfen sunduğumuz [çevrimiçi belgeleri](plan-connect-user-signin.md#federation-with-pingfederate). 
- Güncelleştirilen Azure AD Connect Sihirbazı sorun giderme burada bunları artık analiz daha fazla hata senaryonun yardımcı programı, bağlı posta kutuları ve AD dinamik grupları gibi. Sorun giderme yardımcı programı hakkında daha fazla bilgiyi [burada](tshoot-connect-objectsync.md).
- Cihaz geri yazma yapılandırması, artık yalnızca Azure AD Connect Sihirbazı içinde yönetilir.
- Yeni bir PowerShell çağrılan ADSyncTools.psm1 eklediğiniz SQL bağlantı sorunları ve diğer sorun giderme çeşitli yardımcı programları sorun giderme için kullanılan modül. ADSyncTools modülü hakkında daha fazla bilgiyi [burada](tshoot-connect-tshoot-sql-connectivity.md). 
- Yeni bir ek görev "Cihaz seçeneklerini Yapılandır" eklendi. Aşağıdaki iki işlem yapılandırma görevini kullanabilirsiniz: 
  - **Hibrit Azure AD'ye katılma**: Ortamınızda şirket içi AD ayak izi varsa ve Azure Active Directory ile sağlanan özelliklerden yararlanmak istiyorsanız hibrit Azure AD’ye katılmış cihazları uygulayabilirsiniz. Bunlar hem şirket içi Active Directory alanınıza hem de Azure Active Directory alanınıza katılmış cihazlardır.
  - **Cihaz geri yazmayı**: Cihaz geri yazma, AD FS cihazlara dayalı koşullu erişimi etkinleştirmek için kullanılır (2012 R2 veya üzeri) korunan cihazlar

    >[!NOTE] 
    > - Cihaz geri yazma eşitleme seçeneklerini özelleştirme etkinleştirme seçeneği gri görünür. 
    > -  ADPrep için PowerShell modülü, bu sürümle birlikte kullanım dışı bırakılmıştır.



### <a name="fixed-issues"></a>Düzeltilen sorunlar 

- Bu sürüm, SQL Server Express yüklemesi diğerleriyle birlikte, çeşitli güvenlik açıklarını düzeltme sağlayan, SQL Server 2012 SP4 güncelleştirir.  Lütfen [burada](https://support.microsoft.com/help/4018073/sql-server-2012-service-pack-4-release-information) SQL Server 2012 SP4 hakkında daha fazla bilgi.
- Eşitleme kuralının işlenmesi: üst eşitleme kuralı artık geçerli değilse katılmak herhangi bir koşul ile giden birleştirme eşitleme kuralları da uygulanması gerekir
- Eşitleme Hizmeti Yöneticisi UI ve eşitleme kuralları Düzenleyicisi için birçok erişilebilirlik düzeltmeleri uygulandı
- Azure AD Connect Sihirbazı: Azure AD Connect, bir çalışma grubunda olduğunda AD Bağlayıcısı hesabı oluşturulurken hata oluştu
- Azure AD Connect Sihirbazı: AD etki alanları ve Azure AD doğrulandı etki alanlarında herhangi bir uyuşmazlık olduğunda Azure AD oturum açma sayfasında doğrulama onay kutusunu görüntüleyin.
- Otomatik yükseltme durumu doğru bir şekilde çalıştı otomatik yükseltmeden sonra bazı durumlarda ayarlamak için PowerShell düzeltme otomatik yükseltme.
- Azure AD Connect Sihirbazı: Daha önce eksik bilgileri yakalamak için güncelleştirilmiş telemetri
- Azure AD Connect Sihirbazı: Kullanırken aşağıdaki değişiklikler yapılmıştır **değiştirme kullanıcı oturum açma** görev AD FS'den doğrudan kimlik doğrulamaya geçiş yapmak için:
    - Geçişli kimlik doğrulaması Aracısı Azure AD Connect sunucusunda yüklü olduğundan ve biz yönetilen Federasyon oluşturan yönetilenden dönüştürmeden önce geçişli kimlik doğrulaması özelliği etkin.
    - Kullanıcılar artık gelen yönetilen Federasyon oluşturan dönüştürülür. Yalnızca etki alanı dönüştürülür.
- Azure AD Connect Sihirbazı: AD FS çoklu etki alanı Regex doğru değil Kullanıcı UPN sahip olduğunda ' özel karakterler desteklemek için özel karakter Regex güncelleştirme
- Azure AD Connect Sihirbazı: Sahte "Yapılandırma kaynak bağlayıcı özniteliği" iletisi hiçbir değişiklik olduğunda Kaldır 
- Azure AD Connect Sihirbazı: Çift Federasyon senaryo için AD FS desteği
- Azure AD Connect Sihirbazı: AD FS talep eklenen etki alanı için yönetilen etki alanını federasyona dönüştürürken güncelleştirilmez
- Azure AD Connect Sihirbazı: Yüklü paketleri olan algılama sırasında buluyoruz eski Dirsync/Azure AD eşitleme/Azure AD Connect ilgili ürünler. Artık eski ürünler kaldırmayı deneyeceğiz.
- Azure AD Connect Sihirbazı: Geçişli kimlik doğrulaması Aracısı yüklemesi başarısız olduğunda hata iletisi eşleme düzeltin
- Azure AD Connect Sihirbazı: Etki alanı OU filtreleme sayfasından kaldırıldı "Yapılandırma" kapsayıcısı
- Eşitleme altyapısını yükleme: eşitleme altyapısını yükleme MSI zaman zaman başarısız olan gereksiz eski mantığını kaldırırsınız
- Azure AD Connect Sihirbazı: Açılır Yardım metni isteğe bağlı özellikler sayfasında parola karma eşitlemesi için düzeltme
- Eşitleme altyapısı çalışma zamanı: CS nesnesi bir içeri aktarılan sahip olduğu Senaryo silme düzeltme ve eşitleme kuralları nesne yeniden sağlama girişimi.
- Eşitleme altyapısı çalışma zamanı: Bir Yardım bağlantısı için bir içeri aktarma hatası için sorun giderme kılavuzu olay günlüğüne çevrimiçi Bağlantısı Ekle
- Eşitleme altyapısı çalışma zamanı: Eşitleme bağlayıcıları sıralanırken Zamanlayıcı azaltılmış bellek kullanımı
- Azure AD Connect Sihirbazı: Bir sorunu çözme özel bir eşitleme hizmeti hiçbir AD okuma ayrıcalıklarına sahip hesap
- Azure AD Connect Sihirbazı: Etki alanı ve OU filtreleme seçimleri günlüğünü artırmak
- Azure AD Connect Sihirbazı: AD FS ekleme varsayılan MFA senaryo için oluşturulmuş federasyon güveni için talep
- Azure AD Connect Sihirbazı: AD FS WAP dağıtın: Yeni sertifikayı kullanmak sunucu ekleme başarısız
- Azure AD Connect Sihirbazı: Bir etki alanı için onPremCredentials olmayan hazırlarken DSSO özel durumu 
- Tercihen etkin kullanıcı nesneden AD distinguishedName öznitelik akışı.
- Sabit yüzeysel bir hata olan ilk OOB eşitleme kuralının önceliğini 100 yerine 99 ayarlanmıştır



## <a name="117510"></a>1.1.751.0
Durum 4/12/2018: Kullanıma yalnızca indirme

>[!NOTE]
>Bu sürüm için Azure AD Connect düzeltmedir

### <a name="azure-ad-connect-sync"></a>Azure AD Connect Eşitleme
#### <a name="fixed-issues"></a>Düzeltilen sorunlar
Çin kiracılar zaman zaman başarısız için bulma otomatik bir Azure örneği olan bir sorun düzeltildi.  

### <a name="ad-fs-management"></a>AD FS Yönetimi
#### <a name="fixed-issues"></a>Düzeltilen sorunlar

ArgumentException "aynı anahtara sahip bir öğe zaten eklenmiş." belirten neden olan yapılandırmayı yeniden deneme mantığı, bir hata oluştu  Bu, tüm yeniden deneme işlemleri başarısız olmasına neden olur.

## <a name="117500"></a>1.1.750.0
Durum 3/22/2018: Otomatik yükseltme ve yükleme için yayımlamıştır.
>[!NOTE]
>Bu yeni sürüme yükseltme işlemi tamamlandığında, bir tam eşitleme ve Azure AD Bağlayıcısı için tam içeri aktarma ve tam bir eşitleme AD Bağlayıcısı için otomatik olarak gerçekleştirir. Bu Azure AD Connect ortamınızın boyutuna bağlı olarak biraz zaman alabilir, yükseltme bulduğunuz Bunu yapmak için kullanışlı bir süre kadar bekletir ya da bunu desteklemek için gerekli adımları gerçekleştirdiğinizden emin olun.

>[!NOTE]
>"AutoUpgrade işlevi hatalı 1.1.524.0 daha derlemelerin dağıtılmasını bazı Kiracı için devre dışı bırakıldı. Azure AD Connect Örneğiniz için AutoUpgrade hala uygun olduğundan emin olmak için aşağıdaki PowerShell cmdlet'ini çalıştırın: "Set-ADSyncAutoUpgrade - AutoupGradeState etkin"


### <a name="azure-ad-connect"></a>Azure AD Connect
#### <a name="fixed-issues"></a>Düzeltilen sorunlar

* Otomatik yükseltme durumu askıya alındı olarak ayarlanmışsa set-ADSyncAutoUpgrade cmdlet'i önceden Autoupgrade engellenebilir. Gelecekteki yapıların AutoUpgrade engellemez şekilde bu işlevsellik artık değişti.
* Değiştirilen **kullanıcı oturum açma** seçeneği "Parola Karması eşitleme" için "Parola Eşitleme" Sayfa.  Bu ne gerçekten oluştuğunu ile hizalar. Bu nedenle azure AD Connect parola, parola karmaları eşitler.  Daha fazla bilgi için [Azure AD Connect eşitlemesi ile parola karması eşitlemeyi uygulama](how-to-connect-password-hash-synchronization.md)

## <a name="117490"></a>1.1.749.0
Durum: Müşteriler seçmek için yayımlanan

>[!NOTE]
>Bu yeni sürüme yükseltme işlemi tamamlandığında, bir tam eşitleme ve Azure AD Bağlayıcısı için tam içeri aktarma ve tam bir eşitleme AD Bağlayıcısı için otomatik olarak gerçekleştirir. Lütfen bu Azure AD Connect ortamınızın boyutuna bağlı olarak biraz zaman alabilir olduğundan yükseltme bulduğunuz Bunu yapmak için kullanışlı bir süre kadar bekletir ya da bunu desteklemek için gerekli adımları gerçekleştirdiğinizden emin olun.

### <a name="azure-ad-connect"></a>Azure AD Connect
#### <a name="fixed-issues"></a>Düzeltilen sorunlar
* Sonraki sayfaya geçiş yaparken bölüm filtreleme sayfası için arka plan görevleri zamanlama penceresi düzeltmesi.

* Yapılandırmaveritabanı özel eylemi sırasında erişim ihlaline neden bir hata düzeltildi.

* SQL bağlantı zaman aşımı kurtarmak için bir hata düzeltildi.

* Sertifikaları SAN joker karakterler içeren bir önkoşul denetimi başarısız olduğu bir hata düzeltildi.

* Bir Azure AD Bağlayıcısı dışarı aktarma sırasında miiserver.exe kilitlenmesine neden olan hata düzeltildi.

* Hangi hatalı parola denemesi günlüğe DC üzerinde yapılandırmasını değiştirmek için Azure AD Connect Sihirbazı çalıştırırken düzeltildi.


#### <a name="new-features-and-improvements"></a>Yeni özellikler ve geliştirmeler

* Genel veri koruma yönetmeliği (GDPR) için gizlilik ayarları ekleniyor.  Daha fazla bilgi için bkz [burada](reference-connect-user-privacy.md).

[!INCLUDE [Privacy](../../../includes/gdpr-intro-sentence.md)]  

* Uygulama telemetrisini - yönetici bu sınıf veri açık/kapalı dilediğiniz zaman geçiş yapabilirsiniz

* Azure AD sistem durumu verilerini - yönetici durumu ayarlarını denetlemek için sistem durumu portalı ziyaret etmeniz gerekir.
   Hizmet İlkesi değiştirildi sonra aracılar okuyun ve onu zorla.

* Eklenen cihaz geri yazma yapılandırma eylemleri ve sayfa başlatma için bir ilerleme çubuğu

* HTML raporu ve tam veri toplama ZIP metin genel tanılama geliştirilmiş / HTML raporu

* Otomatik yükseltme ve sunucusunun sistem durumunu belirlenebilir emin olmak için eklenen ek telemetri güvenilirliğini geliştirildi

* Kullanılabilir AD Bağlayıcısı hesabındaki ayrıcalıklı hesaplara kısıtlayın

  * Yeni yüklemeler için sihirbazın ayrıcalıklı hesapları izinleri kısıtlar MSOL hesabı oluşturduktan sonra MSOL hesabına sahip.

Değişiklikleri aşağıdakilerden ölçeklendirilmesini:
1. Express yüklemeleri
2. Özel yüklemeler hesabıyla otomatik olarak oluşturma
3. Azure AD Connect temiz yükleme SA ayrıcalığı gerektirmez için yükleyici değiştirildi

* Belirli bir nesnesi için eşitleme sorunlarını gidermek için yeni bir yardımcı eklendi. Azure AD Connect Sihirbazı sorun giderme ek görevi 'Nesne eşitleme sorunlarını giderme' seçeneği altında kullanılabilir. Şu anda, yardımcı program aşağıdakileri denetler:

  * Azure AD kiracısında kullanıcı hesabı ile eşitlenmiş kullanıcı nesnesi arasındaki UserPrincipalName uyuşmazlığı.
  * Nesne filtreleme etki alanı nedeniyle eşitleme filtrelediyseniz
  * Nesne eşitleme filtreleme kuruluş birimi (OU) nedeniyle filtrelediyseniz

* Belirli bir kullanıcı hesabı için şirket içi Active Directory içinde depolanan geçerli parola karması eşitleme için yeni bir yardımcı program eklendi.

Yardımcı programı, parola değişikliği gerektirmez. Azure AD Connect Sihirbazı sorun giderme ek görevi 'Parola Karması eşitleme sorunlarını giderme' seçeneği altında kullanılabilir.






## <a name="116540"></a>1.1.654.0
Durum: 12 Aralık 2017

>[!NOTE]
>Bu sürüm bir güvenlik olan Azure AD Connect için ilgili düzeltme

### <a name="azure-ad-connect"></a>Azure AD Connect
Bir geliştirme önerilen izni altında açıklandığı gibi bölümü değiştirdiğinden emin olmak için Azure AD Connect sürümü 1.1.654.0 (ve sonra) eklendi [AD DS hesabı için erişim kilitleme](#lock) otomatik olarak ne zaman uygulanacağına Azure AD Connect AD DS hesabı oluşturur. 

- Azure AD Connect ayarlarken, yükleme yönetici mevcut bir AD DS hesabı sağlamak veya Azure AD Connect otomatik olarak hesabı oluşturmak istiyorum. Kurulum sırasında oluşturulan Azure AD Connect tarafından AD DS hesabı için izin değişiklikleri otomatik olarak uygulanır. Yüklenirken yönetici tarafından sağlanan mevcut AD DS hesabı için uygulanmaz.
- Azure AD Connect'in eski bir sürümden 1.1.654.0 (veya sonra), izin yükselten müşteriler için değişiklikler geriye dönük olarak yükseltmeden önce oluşturulan mevcut AD DS hesapları için uygulanmaz. Bunlar yalnızca yükseltme işleminden sonra oluşturulan yeni AD DS hesapları için uygulanır. Bu durum, yeni AD ormanlarınızla Azure AD ile eşitlenmeye eklemekte olduğunuz oluşur.

>[!NOTE]
>Bu sürüm, yalnızca yeni yüklemelerine ilişkin hizmet hesabı yükleme işlemi tarafından oluşturulduğu Azure AD Connect güvenlik açığını ortadan kaldırır. Var olan yüklemeler için veya hesap kendiniz sağlarsınız durumlarda, bu güvenlik açığını yok emin olmanız gerekir.

#### <a name="lock"></a> AD DS hesabı için erişim kilitleme
Şirket içi ad'nizde aşağıdaki izin değişiklikleri uygulayarak AD DS hesabı için erişim kilitleme AD:  

*   Belirtilen nesnenin devralma devre dışı bırak
*   Kendi KENDİNE özgü ACE dışında belirli nesne üzerindeki tüm ACE kaldırın. Kendi KENDİNE söz konusu olduğunda, varsayılan izinleri korumak istiyoruz.
*   Bu özel izinler atayın:

Type     | Ad                          | Access               | Şunun İçin Geçerli
---------|-------------------------------|----------------------|--------------|
İzin Ver    | SİSTEM                        | Tam Denetim         | Bu nesne  |
İzin Ver    | Kuruluş Yöneticileri             | Tam Denetim         | Bu nesne  |
İzin Ver    | Etki alanı yöneticileri                 | Tam Denetim         | Bu nesne  |
İzin Ver    | Yöneticiler                | Tam Denetim         | Bu nesne  |
İzin Ver    | Kuruluş etki alanı denetleyicileri | İçeriği Listele        | Bu nesne  |
İzin Ver    | Kuruluş etki alanı denetleyicileri | Tüm özellikleri oku  | Bu nesne  |
İzin Ver    | Kuruluş etki alanı denetleyicileri | Okuma izinleri     | Bu nesne  |
İzin Ver    | Kimliği Doğrulanmış Kullanıcılar           | İçeriği Listele        | Bu nesne  |
İzin Ver    | Kimliği Doğrulanmış Kullanıcılar           | Tüm özellikleri oku  | Bu nesne  |
İzin Ver    | Kimliği Doğrulanmış Kullanıcılar           | Okuma izinleri     | Bu nesne  |

Ayarlar, AD DS hesabı için reddedeceği çalıştırabilirsiniz [bu PowerShell Betiği](https://gallery.technet.microsoft.com/Prepare-Active-Directory-ef20d978). PowerShell Betiği, AD DS hesabı için yukarıda bahsedilen izinlerin atar.

#### <a name="powershell-script-to-tighten-a-pre-existing-service-account"></a>Önceden var olan bir hizmet hesabı reddedeceği PowerShell Betiği

Önceden var olan bir AD DS hesabı için bu ayarları uygulamak için PowerShell Betiği kullanmak için (bir önceki Azure AD Connect yüklemesi tarafından oluşturulan veya kuruluşunuz tarafından sağlanan ether Lütfen betiği indir sağlanan yukarıdaki bağlantıdan.

##### <a name="usage"></a>Kullanım:

```powershell
Set-ADSyncRestrictedPermissions -ObjectDN <$ObjectDN> -Credential <$Credential>
```

Konum 

**$ObjectDN** = Active Directory hesabı izinlerini sıkılaştırıldığını gerekir.

**$Credential** = $ObjectDN hesap izinlerini kısıtlamak için gerekli ayrıcalıklara sahip yönetici kimlik bilgileri. Bu ayrıcalıkların genellikle kuruluş veya etki alanı yöneticisi tarafından tutulur. Hesap arama hatalarını önlemek için yönetici hesabının tam etki alanı adını kullanın. Örnek: contoso.com\admin.

>[!NOTE] 
>$credential. Kullanıcı adı FQDN\username biçiminde olmalıdır. Örnek: contoso.com\admin 

##### <a name="example"></a>Örnek:

```powershell
Set-ADSyncRestrictedPermissions -ObjectDN "CN=TestAccount1,CN=Users,DC=bvtadwbackdc,DC=com" -Credential $credential 
```
### <a name="was-this-vulnerability-used-to-gain-unauthorized-access"></a>Bu güvenlik açığını yetkisiz erişim elde etmek için kullanılan?

Bu güvenlik açığını Azure AD'nize güvenliğini aşmak için kullanılan, görmek için hizmet hesabının Tarih son parola doğrulamalıdır Connect yapılandırması sıfırlayın.  Varsa daha fazla araştırma, zaman damgası beklenmeyen olay parola sıfırlama için olay günlüğü gerçekleştirilen.

Daha fazla bilgi için [Microsoft Güvenlik Danışma 4056318](https://technet.microsoft.com/library/security/4056318)

## <a name="116490"></a>1.1.649.0
Durum: 27 Ekim 2017

>[!NOTE]
>Bu derleme için Azure AD Connect otomatik yükseltme özelliği aracılığıyla müşteriler tarafından kullanılabilir değil.

### <a name="azure-ad-connect"></a>Azure AD Connect
#### <a name="fixed-issue"></a>Sorun düzeltildi
* Azure AD Connect ve Azure AD Connect Health Aracısı (için eşitleme) arasında bir sürüm uyumluluk sorunu düzeltildi. Bu sorunu, Azure AD Connect yerinde yükseltme sürümüne 1.1.647.0 icraatta bulunan müşterileri etkiler, ancak şu anda sistem durumu aracısı sürümü 3.0.127.0 sahiptir. Yükseltmeden sonra sistem durumu aracısı artık Azure AD Connect eşitleme hizmeti ile ilgili sağlık verilerini Azure AD Health hizmetine gönderebilirsiniz. Bu düzeltmeyle, sistem durumu aracısı sürümü 3.0.129.0 Azure AD Connect yerinde yükseltme sırasında yüklenir. Sistem Durumu Aracısı sürümü 3.0.129.0 Azure AD Connect sürümü 1.1.649.0 ile uyumluluk sorunu yok.


## <a name="116470"></a>1.1.647.0
Durum: 19 Ekim 2017

> [!IMPORTANT]
> Bir Azure AD Connect sürümünüzü 1.1.647.0 ve Azure AD Connect Health Aracısı (eşitleme için) sürüm 3.0.127.0 arasında bilinen uyumluluk sorunu yok. Bu sorun, sistem durumu aracısı, Azure AD Health hizmeti için Azure AD Connect eşitleme (Nesne eşitleme hatalarının ve çalıştırma geçmişi verilerini dahil) hizmeti ile ilgili sağlık verilerini göndermesini engeller. El ile Azure AD Connect dağıtımınızı 1.1.647.0 sürüme yükseltmeden önce lütfen Azure AD Connect Health Aracısı'nın geçerli sürümü, Azure AD Connect sunucunuzda yüklü doğrulayın. Giderek bunu *Denetim Masası → Program Ekle/Kaldır* ve uygulamasıyla ilgili daha fazla bilgi için *Microsoft Azure AD Connect Health aracısını eşitleme için*. Sürümü 3.0.127.0 ise, yükseltmeden önce kullanılabilir olması sonraki Azure AD Connect sürümünüzü beklemeniz önerilir. Sistem Durumu Aracısı sürümü 3.0.127.0 yoksa, el ile yerinde yükseltmeye devam etmek uygundur. Bu sorunu swing yükseltme veya müşterileri yeni Azure AD Connect yüklemesi gerçekleştiriyorsanız etkilemez unutmayın.
>
>

### <a name="azure-ad-connect"></a>Azure AD Connect
#### <a name="fixed-issues"></a>Düzeltilen sorunlar
* İle ilgili bir sorun düzeltildi *değiştirme kullanıcı oturum açma* görev Azure AD Connect Sihirbazı'nda:

  * Parola Eşitleme ile mevcut bir Azure AD Connect dağıtımının varsa sorun **etkin**, ve kullanıcı oturum açma yöntemi olarak ayarlamaya çalıştığınız *geçişli kimlik doğrulaması*. Değişikliği uygulamadan önce sihirbaz yanlış gösterir "*parola eşitleme devre dışı*" istemi. Ancak, değişiklik uygulandıktan sonra parola eşitleme etkin durumda kalır. Bu düzeltmeyle, sihirbaz artık istemi gösterir.

  * Kullanıcı oturum açma yöntemini kullanarak güncelleştirdiğinizde tasarım gereği, Sihirbazı Parola Eşitleme devre dışı bırakmaz *değiştirme kullanıcı oturum açma* görev. Doğrudan kimlik doğrulama veya Federasyon oturum açma, birincil kullanıcı yöntemi olarak etkinleştirdiğini halde parola eşitleme, korumak istediğiniz müşteriler kesintiye uğramasını önlemek için budur.
  
  * Kullanıcı oturum açma yöntemini güncelleştirdikten sonra parola eşitleme devre dışı bırakmak isterseniz yürütülmesi gereken *özelleştirme eşitleme yapılandırması* Sihirbazı görev. Ne zaman ulaşmanıza *isteğe bağlı özellikler* sayfasında, onay kutusunu temizleyin *parola eşitleme* seçeneği.
  
  * Sorunsuz çoklu oturum açmayı etkinleştir/devre dışı bırak denerseniz, aynı sorunu da oluştuğunu unutmayın. Özellikle, parola eşitleme etkin var olan bir Azure AD Connect dağıtıma sahip ve kullanıcı oturum açma yöntemi olarak zaten yapılandırılmış *geçişli kimlik doğrulaması*. Kullanarak *değiştirme kullanıcı oturum açma* görev denediğinizde işaretle / *etkinleştirme sorunsuz çoklu oturum açma* kullanıcı oturum açma yöntemini "Geçişli kimlik doğrulaması" yapılandırılmış kalırken seçeneği. Değişikliği uygulamadan önce sihirbaz yanlış gösterir "*parola eşitleme devre dışı*" istemi. Ancak, değişiklik uygulandıktan sonra parola eşitleme etkin durumda kalır. Bu düzeltmeyle, sihirbaz artık istemi gösterir.

* İle ilgili bir sorun düzeltildi *değiştirme kullanıcı oturum açma* görev Azure AD Connect Sihirbazı'nda:

  * Parola Eşitleme ile mevcut bir Azure AD Connect dağıtımının varsa sorun **devre dışı**, ve kullanıcı oturum açma yöntemi olarak ayarlamaya çalıştığınız *geçişli kimlik doğrulaması*. Sihirbaz, değişiklik uygulandığında, geçişli kimlik doğrulaması hem parola eşitleme sağlar. Bu düzeltmeyle, sihirbaz, artık parola eşitleme sağlar.

  * Daha önce parola eşitleme, geçişli kimlik doğrulamasını etkinleştirmek için bir önkoşul oluştu. Kullanıcı oturum açma yöntemi olarak ayarlandığında *geçişli kimlik doğrulaması*, sihirbaz, geçişli kimlik doğrulaması hem parola eşitleme etkinleştirmez. Kısa bir süre önce parola eşitleme, bir önkoşul kaldırıldı. Azure AD Connect sürümü 1.1.557.0 bir parçası olarak, kullanıcı oturum açma yöntemi olarak ayarladığınızda parola eşitlemeyi etkinleştirmek için Azure AD Connect'e bir değişiklik yapılmıştır *geçişli kimlik doğrulaması*. Ancak, değişiklik, yalnızca Azure AD Connect yüklemenize uygulandı. Bu düzeltmeyle, aynı değişikliği de uygulanır *değiştirme kullanıcı oturum açma* görev.
  
  * Sorunsuz çoklu oturum açmayı etkinleştir/devre dışı bırak denerseniz, aynı sorunu da oluştuğunu unutmayın. Özellikle, parola eşitleme devre dışı var olan bir Azure AD Connect dağıtıma sahip ve kullanıcı oturum açma yöntemi olarak zaten yapılandırılmış *geçişli kimlik doğrulaması*. Kullanarak *değiştirme kullanıcı oturum açma* görev denediğinizde işaretle / *etkinleştirme sorunsuz çoklu oturum açma* kullanıcı oturum açma yöntemini "Geçişli kimlik doğrulaması" yapılandırılmış kalırken seçeneği. Sihirbaz, değişiklik uygulandığında, parola eşitleme sağlar. Bu düzeltmeyle, sihirbaz, artık parola eşitleme sağlar. 

* Azure AD Connect yükseltme başarısız olmasına neden olan hata ile bir sorun düzeltildi "*Eşitleme Hizmeti'ni yükseltme kurulamıyor*". Ayrıca, eşitleme hizmeti olay hatasıyla artık başlatabilirsiniz "*hizmet, veritabanı sürümü yüklü ikili dosyaların sürümüyle yeni olduğundan dolayı başlatılamadı*". Yükseltme yapan Yöneticisi, Azure AD Connect tarafından kullanılan SQL Server sysadmin ayrıcalığı yok. Bu sorun oluşur. Bu düzeltmeyle, Azure AD Connect ADSync veritabanı için db_owner ayrıcalık yükseltme sırasında sağlamak yöneticiye yalnızca gerekir.

* Etkilenen etkinleştirdiyseniz müşteriler bir Azure AD Connect Yükseltmesi bir sorun düzeltildi [sorunsuz çoklu oturum açma](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-sso). Azure AD Connect yükseltildikten sonra özelliği etkin ve tam işlevsel kalsa da sorunsuz çoklu oturum açma hatalı Azure AD Connect Sihirbazı'nda, devre dışı bırakılmış şekilde görünür. Bu düzeltmeyle, özelliği artık doğru şekilde sihirbazda olarak etkinleştirilmiş görüntülenir.

* Azure AD Connect Sihirbazı her zaman göster neden olan bir sorun düzeltildi "*kaynak bağlantısını yapılandır*" komut istemi üzerinde *yapılandırmaya hazır* sayfasında bile kaynak bağlantısı için ilgili hiçbir değişiklik yapılmadı.

* Azure AD Connect el ile yerinde yükseltme gerçekleştirirken, müşteri karşılık gelen Azure AD kiracısı genel yönetici kimlik bilgilerini sağlamanız gerekir. Genel yönetici kimlik bilgilerini farklı Azure'a ait olsa bile daha önce yükseltme devam edilemedi AD Kiracı. Yükseltme başarıyla tamamlamak için görüntülenirken, belirli yapılandırmaları doğru yükseltmeye kalıcı değildir. Bu değişiklik, sağlanan kimlik bilgilerinin Azure AD kiracısı eşleşmezse devam ederek, Yükseltme Sihirbazı'nı engeller.

* Azure AD Connect Health hizmetine el ile yükseltme başındaki gereksiz yere yeniden yedekli mantıksal kaldırıldı.


#### <a name="new-features-and-improvements"></a>Yeni özellikler ve geliştirmeler
* Azure AD Connect Microsoft Almanya Bulutu ile ayarlamak için gerekli adımları basitleştirmek için mantıksal eklendi. Daha önce bu makalede açıklandığı gibi Microsoft Almanya Bulutu ile doğru çalışması için Azure AD Connect sunucusunda belirli kayıt defteri anahtarlarını güncelleştirmek için gereklidir. Şimdi, Azure AD Connect otomatik olarak kiracınızda Kurulum sırasında sağlanan genel yönetici kimlik bilgileri Microsoft Almanya bulutunda temel alır, algılayabilir.

### <a name="azure-ad-connect-sync"></a>Azure AD Connect Sync
> [!NOTE]
> Not: Eşitleme hizmeti kendi özel Zamanlayıcı geliştirmenize olanak sağlayan bir WMI arabirimine sahiptir. Bu arabirim artık kullanım dışıdır ve gelecekteki kaldırılır, Azure AD Connect'in sürümlerinde sevk 30 Haziran 2018'den sonra. Eşitleme zamanlaması özelleştirmek isteyen müşteriler kullanması gereken [yerleşik Zamanlayıcı (https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-feature-scheduler).

#### <a name="fixed-issues"></a>Düzeltilen sorunlar
* Azure AD Connect Sihirbazı şirket içi Active Directory'den değişiklikleri eşitlemek için gereken AD Bağlayıcısı hesabı oluşturduğunda, doğru hesabı PublicFolder nesneleri okumak için gerekli izinlere atamaz. Bu sorun, hızlı yükleme hem de özel yükleme etkiler. Bu değişiklik, sorunu giderir.

* Windows Server 2016'dan çalıştıran Yöneticiler için değil işlemek Azure AD Connect Sihirbazı sorun giderme sayfası doğru neden olan bir sorun düzeltildi.

#### <a name="new-features-and-improvements"></a>Yeni özellikler ve geliştirmeler
* Sorun giderme sayfası Azure AD Connect sihirbazını kullanarak parola eşitleme sorunlarını giderirken, sorun giderme sayfası artık etki alanına özel durumu döndürür.

* Daha önce parola karma eşitlemesini etkinleştirme çalıştıysanız, Azure AD Connect doğrulama AD Bağlayıcısı hesabı gelen parola karmalarını eşitleyecek şekilde izinleri gerekli olup şirket içi AD. Şimdi, Azure AD Connect Sihirbazı'nı doğrulayın ve AD Bağlayıcısı hesabın yeterli izinlere sahip değil, sizi uyarır.

### <a name="ad-fs-management"></a>AD FS Yönetimi
#### <a name="fixed-issue"></a>Sorun düzeltildi
* Kullanımı için ilgili bir sorun düzeltildi [kaynak bağlantısı olarak ms-DS-ConsistencyGuid](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-design-concepts#using-ms-ds-consistencyguid-as-sourceanchor) özelliği. Bu sorun, yapılandırmış olduğunuz müşterileri etkiler *AD FS ile Federasyon* kullanıcı oturum açma yöntemi olarak. Yürüttüğünüzde *kaynak bağlantısını yapılandır* sihirbazında, Azure AD CONNECT'te görev anahtarları kullanarak * ms-DS-ConsistencyGuid kaynak İmmutableıd özniteliği olarak. Bu değişikliğin bir parçası olarak, Azure AD Connect Immutableıd AD FS'de talep kurallarını güncelleştirmek çalışır. Ancak, Azure AD Connect AD FS'yi yapılandırmak için gereken yönetici kimlik bilgilerine sahip olmadığından bu adım başarısız oldu. Bu düzeltmeyle, Azure AD Connect artık yürüttüğünüzde AD FS yönetici kimlik bilgilerini girmenizi ister *kaynak bağlantısını yapılandır* görev.



## <a name="116140"></a>1.1.614.0
Durum: 05 Eylül 2017

### <a name="azure-ad-connect"></a>Azure AD Connect

#### <a name="known-issues"></a>Bilinen sorunlar
* Hata ile Azure AD Connect yükseltme başarısız olmasına neden olan bilinen bir sorun "*Eşitleme Hizmeti'ni yükseltme kurulamıyor*". Ayrıca, eşitleme hizmeti olay hatasıyla artık başlatabilirsiniz "*hizmet, veritabanı sürümü yüklü ikili dosyaların sürümüyle yeni olduğundan dolayı başlatılamadı*". Yükseltme yapan Yöneticisi, Azure AD Connect tarafından kullanılan SQL Server sysadmin ayrıcalığı yok. Bu sorun oluşur. Dbo izinleri yeterli değil.

* Azure AD Connect, etkinleştirdiğiniz müşterileri etkilemeden yükseltme ile ilgili bilinen bir sorun var. [sorunsuz çoklu oturum açma](how-to-connect-sso.md). Azure AD Connect yükseltildikten sonra özelliği etkin kalsa da özellik sihirbazda devre dışı olarak görünür. Bu sorun gelecek sağlanan için bir düzeltme bırakın. Bu görünen sorunla ilgili endişeleriniz müşteriler el ile bu sihirbazda sorunsuz çoklu oturum açmayı etkinleştirerek düzeltebilirsiniz.

#### <a name="fixed-issues"></a>Düzeltilen sorunlar
* Azure AD Connect, şirket içi talep kuralları güncelleştirmekten önleyen bir sorun düzeltildi etkinleştirme sırasında AD FS [kaynak bağlantısı olarak ms-DS-ConsistencyGuid](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-design-concepts#using-ms-ds-consistencyguid-as-sourceanchor) özelliği. Sorun, AD FS oturum açma yöntemi olarak yapılandırılmış olan mevcut bir Azure AD Connect dağıtımı için özelliği etkinleştirmek çalışırsanız oluşur. Sihirbaz ADFS kimlik bilgileri için AD FS'de talep kurallarını güncelleştirin denemeden önce istemez sorun oluşur.
* Azure AD Connect yükleme işleminin başarısız olmasına neden olan bir sorun düzeltildi şirket içi AD ormanı olan NTLM devre dışı. Azure AD Connect Sihirbazı tam kimlik bilgileri, Kerberos kimlik doğrulaması için gerekli güvenlik kapsamları oluştururken bulunmaması nedeniyle sorunudur. Bu, Kerberos kimlik doğrulamasının başarısız olmasına ve Azure AD Connect Sihirbazı yeniden NTLM kullanılarak geri dönmesine neden olur.

### <a name="azure-ad-connect-sync"></a>Azure AD Connect Sync
#### <a name="fixed-issues"></a>Düzeltilen sorunlar
* Yeni eşitleme kuralı, etiket özniteliği değil doldurulmuşsa burada oluşturulamıyor bir sorun düzeltildi.
* Bağlanmak Azure AD Connect neden olan bir sorun düzeltildi AD, NTLM kullanarak parola eşitlemesi için Kerberos kullanılabilir olsa bile şirket. Bu sorun, şirket içi AD topolojisine sahip bir yedekten geri yüklenen bir veya daha fazla etki alanı denetleyicileri.
* Tam eşitleme adımları yükseltmeden sonra gereksiz yere oluşmasına neden olan bir sorun düzeltildi. Kullanıma hazır eşitleme kuralları için bir değişiklik varsa genel olarak, tam eşitleme adımları çalıştıran yükseltmeden sonra gereklidir. Yanlış eşitleme kuralı ifadesi yeni satır karakterleri ile karşılaşıldığında bir değişiklik algılandı değişiklik algılama mantığı bir hata nedeniyle bir sorun oluştu. Yeni satır karakterleri okunabilirliği artırmak için eşitleme kuralı ifadesine eklenir.
* Azure AD Connect sunucusu doğru şekilde otomatik yükseltme çalışmamasına neden olabilecek bir sorun düzeltildi. Bu sorun, Azure AD Connect sürümü 1.1.443.0 sunucularıyla etkiler (veya öncesi). Sorun hakkında daha fazla ayrıntı için makalesine bakın [Azure AD Connect otomatik yükseltme sonrasında düzgün çalışmıyor](https://support.microsoft.com/help/4038479/azure-ad-connect-is-not-working-correctly-after-an-automatic-upgrade).
* Her 5 dakikada bir hatayla ne zaman yeniden denenmesi otomatik yükseltme neden olabilecek bir sorun düzeltildi. Hatalarla karşılaştı, düzeltme, Otomatik yükseltme üstel geri alma ile yeniden dener.
* Burada parola eşitleme olay 611 yanlış Windows uygulama olay günlüklerini gösterilen bir sorun düzeltildi **bilgilendirici** yerine **hata**. Parola Eşitleme bir sorunla karşılaştığında olay 611 oluşturulur. 
* Grup geri yazma için gerekli bir OU seçmeden etkinleştirilmesini grup geri yazma özelliği sağlayan Azure AD Connect sihirbazında bir sorun düzeltildi.

#### <a name="new-features-and-improvements"></a>Yeni özellikler ve geliştirmeler
* Azure AD Connect Sihirbazı altında ek görevler için sorun giderme görev eklendi. Parola Eşitleme ile ilgili sorunları giderme ve genel tanılama toplamak için bu görevi, müşteriler yararlanabilir. Gelecekte diğer dizin eşitleme ile ilgili sorunlar dahil etmek için sorun giderme Görev genişletilir.
* Azure AD Connect artık destekliyor adlı yeni bir yükleme modu **varolan veritabanını kullan**. Bu yükleme mod müşterilerin var olan bir ad eşitleme veritabanını belirten Azure AD Connect'i yükleme olanak tanır. Bu özellik hakkında daha fazla bilgi için makalesine bakın [varolan veritabanını kullan](how-to-connect-install-existing-database.md).
* Gelişmiş güvenlik için Azure AD Connect dizin eşitleme için Azure AD'ye bağlamak için TLS1.2 kullanan artık varsayılandır. Daha önce varsayılan TLS1.0 oluştu.
* Azure AD Connect parola eşitleme Aracısı başlatıldığında, Azure AD parola eşitleme için iyi bilinen uç noktasına bağlanmaya çalışır. Başarılı bağlantı kurulduğunda, bölgeye özgü bir uç noktaya yönlendirilir. Daha önce yeniden başlatılana kadar parola eşitleme Aracısı bölgeye özgü uç nokta önbelleğe alır. Şimdi, bölgeye özgü uç noktası ile bağlantı sorunu karşılaşırsa, aracıyı iyi bilinen uç noktası ile yeniden dener ve önbellek temizler. Bu değişiklik, parola eşitleme uygulayabileceğiniz farklı bir bölgeye özgü uç nokta yük devretme önbelleğe alınmış bölgeye özgü uç nokta artık kullanılabilir olduğunda sağlar.
* Şirket içi AD ormanında yapılan değişiklikleri eşitlemek için bir AD DS hesabı kullanmanız gerekir. Ya da (ii) AD oluşturabileceğiniz DS kendiniz hesabı ve Azure AD Connect için kendi kimlik bilgilerini belirtin veya (ii) kurumsal yöneticinin kimlik bilgilerini sağlayın ve Azure AD Connect sizin için AD DS hesabını oluşturmak istiyorum. Daha önce (i) Azure AD Connect Sihirbazı'nda varsayılan seçenektir. Şimdi, (ii), varsayılan seçenektir.

### <a name="azure-ad-connect-health"></a>Azure AD Connect Health

#### <a name="new-features-and-improvements"></a>Yeni özellikler ve geliştirmeler
* Microsoft Azure kamu Bulutu ve Microsoft Cloud Almanya için destek eklendi.

### <a name="ad-fs-management"></a>AD FS Yönetimi
#### <a name="fixed-issues"></a>Düzeltilen sorunlar
* AD hazırlık powershell modülünün Initialize-ADSyncNGCKeysWriteBack cmdlet'inde yanlış ACL'ler için cihaz kaydı kapsayıcısı uygulama ve bu nedenle yalnızca mevcut izinlerini devralır.  Eşitleme hizmeti hesabının doğru izinlere sahip olacak şekilde güncelleştirildi.

#### <a name="new-features-and-improvements"></a>Yeni özellikler ve geliştirmeler
* Microsoft Online ve ADFS yalnızca belirteç alma karşı oturum açma doğrular, AAD Connect doğrulayın ADFS oturum açma görev güncelleştirildi.
* Kullanıcının AD FS ve WAP sunucularını sağlamanız istenir önce artık gerçekleşmesi AAD Connect kullanarak yeni bir AD FS grup ayarlıyorsanız, ADFS kimlik bilgilerinin sorulmasına sayfaya taşındı.  Bu, belirtilen hesabın doğru izinlere sahip olduğunu denetlemek AAD Connect sağlar.
* ADFS güveni AAD güncelleştiremediğinde AAD Connect yükseltmesinde biz artık yükseltme başarısız olur.  Bu durumda, kullanıcı uygun bir uyarı iletisi gösterilir ve AAD Connect ek görevi aracılığıyla güveni sıfırlamak için devam etmelisiniz.

### <a name="seamless-single-sign-on"></a>Sorunsuz Çoklu Oturum Açma
#### <a name="fixed-issues"></a>Düzeltilen sorunlar
* Azure AD Connect Sihirbazı'nı etkinleştirmek denerseniz bir hata döndürmek neden bir sorun düzeltildi [sorunsuz çoklu oturum açma](how-to-connect-sso.md). Hata iletisi *"Yapılandırması Microsoft Azure AD Connect kimlik doğrulaması Aracısı başarısız oldu."* Bu sorun için kimlik doğrulama aracılarının önizleme sürümünü el ile yükselten Mevcut müşterileri etkiler [geçişli kimlik doğrulaması](how-to-connect-sso.md) bu konuda açıklanan adımları göre [makale](how-to-connect-pta-upgrade-preview-authentication-agents.md).


## <a name="115610"></a>1.1.561.0
Durum: 23 Temmuz 2017

### <a name="azure-ad-connect"></a>Azure AD Connect

#### <a name="fixed-issue"></a>Sorun düzeltildi

* "AD - kullanıcı Immutableıd aşımına" kullanıma hazır eşitleme kuralı nedeniyle bir sorun düzeltildi kaldırılacak:

  * Azure AD Connect yükseltilir yaklaştığında veya sorun görev seçeneği *eşitleme yapılandırması güncelleştirmesi* içinde Azure AD Connect Sihirbazı Azure AD Connect eşitleme yapılandırmasını güncelleştirmek için kullanılır.
  
  * Bu eşitleme kuralının etkin müşteriler için geçerlidir [kaynak bağlantısı özellik olarak ms-DS-ConsistencyGuid](plan-connect-design-concepts.md#using-ms-ds-consistencyguid-as-sourceanchor). Bu özellik, sürüm 1.1.524.0 ve sonra sunulmuştur. Azure AD Connect eşitleme kuralı kaldırıldığında, artık şirket içi doldurabilirsiniz AD ms-DS-ConsistencyGuid özniteliği objectGUID özniteliğinin değeri. Yeni kullanıcıların Azure AD ile sağlanan engellemez.
  
  * Özelliğin etkin olduğu sürece eşitleme kuralı artık yükseltme sırasında veya yapılandırma değişikliği sırasında kaldırılacak, düzeltme sağlar. Bu sorunun etkilediği mevcut müşteriler, Azure AD Connect bu sürüme yükseltirken, eşitleme kuralı eklediğiniz düzeltme de sağlar.

* 100'den az öncelik değerine sahip olacak şekilde, kullanıma hazır eşitleme kuralları neden olan bir sorun düzeltildi:

  * Genel olarak, öncelik değerleri 0 - 99 özel eşitleme kuralları için ayrılmıştır. Yükseltme sırasında hazır olan eşitleme kuralları için öncelik değerleri eşitleme kuralı değişiklikleri uyum sağlayacak şekilde güncelleştirilir. Bu sorun nedeniyle, hazır olan eşitleme kuralları 100'den az öncelik değeri atanabilir.
  
  * Düzeltme, yükseltme sırasında oluşmasını sorununu önler. Ancak, bunu öncelik değerleri sorundan etkilenen mevcut müşteriler için geri yüklemez. Ayrı bir düzeltme geri yükleme ile yardımcı olmak için gelecekte sağlanacaktır.

* Bir sorun düzeltildi burada [etki alanı ve OU filtreleme ekranı](how-to-connect-install-custom.md#domain-and-ou-filtering) içinde Azure AD Connect Sihirbazı gösterildiğini *tüm etki alanlarını ve OU'ları Eşitle* seçeneği seçili olarak OU tabanlı filtreleme etkinleştirilmiş olsa bile.

*   Neden bir sorun düzeltildi [dizin bölümlerini Yapılandır ekran](how-to-connect-sync-configure-filtering.md#organizational-unitbased-filtering) bir hata ise döndürülecek Eşitleme Hizmeti Yöneticisi, *Yenile* düğmesine tıklandığında. Hata iletisi *"etki alanları yenilenirken bir hata oluştu: Nesne türü 'türüne System.Collections.ArrayList' yayımlanamıyor ' Microsoft.DirectoryServices.MetadirectoryServices.UI.PropertySheetBase.MaPropertyPages.PartitionObject. "* Yeni AD etki alanı, var olan bir AD ormanına eklendi ve Yenile düğmesini kullanarak Azure AD Connect güncelleştirmeye çalıştığınız hata oluşur.

#### <a name="new-features-and-improvements"></a>Yeni özellikler ve geliştirmeler

* [Otomatik yükseltme özelliğini](how-to-connect-install-automatic-upgrade.md) aşağıdaki yapılandırmalara sahip müşteriler destekleyecek şekilde genişletildi:
  * Cihaz geri yazma özelliğini etkinleştirdiniz.
  * Grup geri yazma özelliğini etkinleştirdiniz.
  * Kurulum, hızlı ayarları veya Dırsync yükseltmesi değil.
  * Meta veri deposunda 100. 000'den fazla nesne var.
  * Birden fazla ormana bağlanırsınız. Hızlı Kurulum yalnızca tek bir ormana bağlanır.
  * AD Bağlayıcısı hesabı varsayılan MSOL_ hesabı artık değil.
  * Sunucu hazırlama moduna ayarlanır.
  * Kullanıcı geri yazma özelliğini etkinleştirdiniz.
  
  >[!NOTE]
  >Müşteriler Azure AD Connect derleme 1.1.105.0 ile ve sonra kapsam genişletme, Otomatik yükseltme özelliğini etkiler. Otomatik olarak yükseltilmesi için Azure AD Connect sunucunuzu istemiyorsanız, Azure AD Connect sunucunuzda cmdlet'i çalıştırmanız gerekir: `Set-ADSyncAutoUpgrade -AutoUpgradeState disabled`. Etkinleştirme/otomatik yükseltme devre dışı bırakma hakkında daha fazla bilgi için makalesine bakın [Azure AD Connect: Otomatik yükseltme](how-to-connect-install-automatic-upgrade.md) makalesine bakın.

## <a name="115580"></a>1.1.558.0
Durum: Serbest bırakılacak değil. Bu yapı değişiklikleri 1.1.561.0 sürümünde dahil edilir.

### <a name="azure-ad-connect"></a>Azure AD Connect

#### <a name="fixed-issue"></a>Sorun düzeltildi

* "AD - kullanıcı Immutableıd aşımına" kullanıma hazır eşitleme kuralı nedeniyle bir sorun düzeltildi OU tabanlı filtreleme yapılandırma güncelleştirildiğinde kaldırılacak. Bu eşitleme kuralı için gerekli olan [kaynak bağlantısı özellik olarak ms-DS-ConsistencyGuid](plan-connect-design-concepts.md#using-ms-ds-consistencyguid-as-sourceanchor).

* Bir sorun düzeltildi burada [etki alanı ve OU filtreleme ekranı](how-to-connect-install-custom.md#domain-and-ou-filtering) içinde Azure AD Connect Sihirbazı gösterildiğini *tüm etki alanlarını ve OU'ları Eşitle* seçeneği seçili olarak OU tabanlı filtreleme etkinleştirilmiş olsa bile.

*   Neden bir sorun düzeltildi [dizin bölümlerini Yapılandır ekran](how-to-connect-sync-configure-filtering.md#organizational-unitbased-filtering) bir hata ise döndürülecek Eşitleme Hizmeti Yöneticisi, *Yenile* düğmesine tıklandığında. Hata iletisi *"etki alanları yenilenirken bir hata oluştu: Nesne türü 'türüne System.Collections.ArrayList' yayımlanamıyor ' Microsoft.DirectoryServices.MetadirectoryServices.UI.PropertySheetBase.MaPropertyPages.PartitionObject. "* Yeni AD etki alanı, var olan bir AD ormanına eklendi ve Yenile düğmesini kullanarak Azure AD Connect güncelleştirmeye çalıştığınız hata oluşur.

#### <a name="new-features-and-improvements"></a>Yeni özellikler ve geliştirmeler

* [Otomatik yükseltme özelliğini](how-to-connect-install-automatic-upgrade.md) aşağıdaki yapılandırmalara sahip müşteriler destekleyecek şekilde genişletildi:
  * Cihaz geri yazma özelliğini etkinleştirdiniz.
  * Grup geri yazma özelliğini etkinleştirdiniz.
  * Kurulum, hızlı ayarları veya Dırsync yükseltmesi değil.
  * Meta veri deposunda 100. 000'den fazla nesne var.
  * Birden fazla ormana bağlanırsınız. Hızlı Kurulum yalnızca tek bir ormana bağlanır.
  * AD Bağlayıcısı hesabı varsayılan MSOL_ hesabı artık değil.
  * Sunucu hazırlama moduna ayarlanır.
  * Kullanıcı geri yazma özelliğini etkinleştirdiniz.
  
  >[!NOTE]
  >Müşteriler Azure AD Connect derleme 1.1.105.0 ile ve sonra kapsam genişletme, Otomatik yükseltme özelliğini etkiler. Otomatik olarak yükseltilmesi için Azure AD Connect sunucunuzu istemiyorsanız, Azure AD Connect sunucunuzda cmdlet'i çalıştırmanız gerekir: `Set-ADSyncAutoUpgrade -AutoUpgradeState disabled`. Etkinleştirme/otomatik yükseltme devre dışı bırakma hakkında daha fazla bilgi için makalesine bakın [Azure AD Connect: Otomatik yükseltme](how-to-connect-install-automatic-upgrade.md) makalesine bakın.

## <a name="115570"></a>1.1.557.0
Durum: Temmuz 2017

>[!NOTE]
>Bu derleme için Azure AD Connect otomatik yükseltme özelliği aracılığıyla müşteriler tarafından kullanılabilir değil.

### <a name="azure-ad-connect"></a>Azure AD Connect

#### <a name="fixed-issue"></a>Sorun düzeltildi
* Doğrulanmış etki alanı hala geçerli bir etki alanı olsa bile değiştirilmesi için var olan hizmet bağlantı noktası nesnesinde yapılandırılmış neden başlatma ADSyncDomainJoinedComputerSync cmdlet'i ile bir sorun düzeltildi. Hizmet bağlantı noktası yapılandırmak için kullanılabilecek birden fazla doğrulanan etki alanlarını Azure AD kiracınıza sahip olduğunda bu sorun oluşur.

#### <a name="new-features-and-improvements"></a>Yeni özellikler ve geliştirmeler
* Parola geri yazma ile Microsoft Azure kamu Bulutu ve Microsoft Cloud Almanya Önizleme için kullanıma sunulmuştur. Farklı hizmet örnekleri için Azure AD Connect desteği hakkında daha fazla bilgi için makalesine bakın [Azure AD Connect: İlgili özel konular örnekleri](reference-connect-instances.md).

* Initialize-ADSyncDomainJoinedComputerSync cmdlet artık AzureADDomain adlı yeni bir isteğe bağlı parametre var. Bu parametre, hizmet bağlantı noktası yapılandırmak için kullanılacak etki alanını doğruladıysanız belirtmenize olanak sağlar.

### <a name="pass-through-authentication"></a>Doğrudan Kimlik Doğrulama

#### <a name="new-features-and-improvements"></a>Yeni özellikler ve geliştirmeler
* Geçişli kimlik doğrulaması değiştirilmiştir için gereken aracı adı *Microsoft Azure AD uygulama ara sunucusu Bağlayıcısı* için *Microsoft Azure AD Connect kimlik doğrulaması Aracısı*.

* Parola Karması eşitleme, geçişli kimlik doğrulaması artık etkinleştirme varsayılan olarak etkinleştirir.


## <a name="115530"></a>1.1.553.0
Durum: Haziran 2017

> [!IMPORTANT]
> Bu derleme sunulan şema ve eşitleme kuralı değişiklikler vardır. Azure AD Connect eşitleme hizmeti, yükseltmeden sonra tam içeri aktarma ve tam eşitleme adımları tetikler. Değişikliklerin ayrıntılarını aşağıda açıklanmıştır. Geçici olarak tam içeri aktarma ve tam eşitleme adımları yükseltmeden sonra ertelemek için makaleyi okuyun [yükseltmeden sonra tam eşitleme erteleneceği nasıl](how-to-upgrade-previous-version.md#how-to-defer-full-synchronization-after-upgrade).
>
>

### <a name="azure-ad-connect-sync"></a>Azure AD Connect Sync

#### <a name="known-issue"></a>Bilinen sorun
* Kullanan müşteriler etkileyen bir sorun [OU tabanlı filtreleme](how-to-connect-sync-configure-filtering.md#organizational-unitbased-filtering) Azure AD Connect eşitlemesi ile. Ne zaman ulaşmanıza [etki alanı ve OU filtreleme sayfası](how-to-connect-install-custom.md#domain-and-ou-filtering) Azure AD Connect Sihirbazı'nda, aşağıdaki davranış beklenir:
  * OU tabanlı filtreleme etkinleştirilirse **seçilen etki alanlarını ve OU'ları Eşitle** seçeneği belirlenir.
  * Aksi takdirde, **tüm etki alanlarını ve OU'ları Eşitle** seçeneği belirlenir.

Ortaya sorunu olan **tüm etki alanlarını ve OU'ları seçeneği Eşitle** Sihirbazı'nı çalıştırdığınızda, her zaman işaretlidir.  OU tabanlı filtrelemeyi daha önce yapılandırılmış olsa bile gerçekleşir. AAD Connect yapılandırma değişiklikleri kaydetmeden önce emin **seçtiğiniz etki alanlarına eşitleyin ve OU'lar seçeneği belirlendiğinde** ve eşitlemek için gereken tüm OU'larda tekrar etkinleştirildiğini doğrulayın. Aksi takdirde, OU tabanlı filtreleme devre dışı bırakılır.

#### <a name="fixed-issues"></a>Düzeltilen sorunlar

* Bir sorun düzeltildi sağlayan bir şirket içi parola sıfırlama için Azure AD yönetici parola geri yazma ile AD ayrıcalıklı kullanıcı hesabı. Azure AD Connect ayrıcalıklı hesabı parola sıfırlama izni verildiğinde sorun oluşur. Sorunu ele yönetici o hesabın sahibi olmadığı sürece bu sürümünde izin vermiyor rastgele şirket içi parolasını sıfırlamak için bir Azure AD Yöneticisi tarafından Azure AD Connect, AD kullanıcı hesabı ayrıcalıklı. Daha fazla bilgi için [güvenlik danışma 4033453](https://technet.microsoft.com/library/security/4033453).

* İlgili bir sorun düzeltildi [kaynak bağlantısı olarak ms-DS-ConsistencyGuid](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-design-concepts#using-ms-ds-consistencyguid-as-sourceanchor) Azure AD Connect için geri yazma değerlendirildiğinde özelliği şirket içi AD ms-DS-ConsistencyGuid özniteliği. Birden çok şirket içi olduğunda sorun AD ormanlarınızla Azure AD Connect'e eklendi ve *kullanıcı kimlikleri arasında birden çok dizin seçeneği mevcut* seçilir. Bu tür yapılandırma kullanıldığında, sonuç eşitleme kuralları meta veri sourceAnchorBinary özniteliğinde doldurmayın. SourceAnchorBinary öznitelik kaynak özniteliği için ms-DS-ConsistencyGuid özniteliği kullanılır. Sonuç olarak ms-DSConsistencyGuid öznitelik geri yazma gerçekleşmez. Bu sorunu düzeltmek için aşağıdaki eşitleme kuralları meta veri sourceAnchorBinary öznitelik her zaman doldurulduğundan emin olmak için güncelleştirilmiştir:
  * İçinde ad - InetOrgPerson AccountEnabled.xml
  * İçinde ad - InetOrgPerson Common.xml
  * İçinde ad - kullanıcı AccountEnabled.xml
  * İçinde ad - kullanıcı Common.xml
  * İçinde AD - kullanıcı katılın SOAInAAD.xml

* Daha önce bile [kaynak bağlantısı olarak ms-DS-ConsistencyGuid](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-design-concepts#using-ms-ds-consistencyguid-as-sourceanchor) özelliği etkin değil, "AD – kullanıcı Immutableıd aşımına" eşitleme kuralı yine de Azure AD Connect'e eklenir. Efekt zararsızdır ve geri yazma ms-DS-ConsistencyGuid özniteliği oluşmasına neden olmaz. Karışıklığı önlemek için mantıksal özelliği etkinleştirilmişse eşitleme kuralı yalnızca eklendiğinden emin olmak için eklendi.

* Parola Karması eşitleme başarısız olmasına neden olan hata olayı 611 ile bir sorun düzeltildi. Bu sorun bir sonraki oluşur veya daha fazla etki alanı denetleyicileri kaldırılmış olan şirket içi AD. Eşitleme tanımlama bilgisi her parola eşitleme döngüsü sonunda verilen şirket içi AD Kaldırılan etki alanı denetleyicileri (güncelleştirme sıra numarası) USN değeri 0 olan çağırma kimliklerini içerir. Parola Eşitleme Yöneticisi eşitleme tanımlama bilgisi içeren USN değeri 0'ın kalıcı hale getirmek belirleyemez ve hata olayı 611 ile başarısız olur. Parola Eşitleme Yöneticisi, sonraki eşitleme döngüsü sırasında 0 USN değeri içermiyor son kalıcı eşitleme tanımlama bilgisi kullanır. Bu, eşitlenmesi aynı parola değişikliğini neden olur. Bu düzeltme ile Parola Eşitleme Yöneticisi doğru eşitleme tanımlama bilgisi kalıcıdır.

* Daha önce Otomatik yükseltme kümesi ADSyncAutoUpgrade cmdlet'i kullanılarak devre dışı bırakılmış olsa bile, işlem devam ediyor olup olmadığını denetlemek için Otomatik yükseltme düzenli aralıklarla yükseltin ve disablement uymanız indirilen yükleyiciyi kullanır. Bu düzeltmeyle, Otomatik yükseltme işlemi artık yükseltme için düzenli aralıklarla kontrol eder. Bu Azure AD Connect sürümü için yükseltme yükleyici kez yürütüldüğünde düzeltme otomatik olarak uygulanır.

#### <a name="new-features-and-improvements"></a>Yeni özellikler ve geliştirmeler

* Daha önce [kaynak bağlantısı olarak ms-DS-ConsistencyGuid](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-design-concepts#using-ms-ds-consistencyguid-as-sourceanchor) özelliği yalnızca yeni dağıtımlar için kullanılabilir. Artık, var olan dağıtımları için kullanılabilir. Daha ayrıntılı belirtmek gerekirse:
  * Bu özelliğe erişmek için Azure AD Connect Sihirbazı'nı başlatın ve seçin *güncelleştirme kaynak bağlantısı* seçeneği.
  * Bu seçenek yalnızca sourceAnchor özniteliği olarak objectGUID kullanıyorsanız var olan dağıtımları görülebilir.
  * Sihirbaz, seçeneği yapılandırırken, şirket içi Active directory'nizde ms-DS-ConsistencyGuid özniteliği durumunu doğrular. Dizine herhangi bir kullanıcı nesnesi öznitelik yapılandırılmazsa sihirbaz ms-DS-ConsistencyGuid sourceAnchor özniteliği olarak kullanır. Öznitelik, bir veya daha fazla kullanıcı, nesneyi directory yapılandırıldıysa, sihirbaz öznitelik diğer uygulamalar tarafından kullanılıyor ve sourceAnchor özniteliği olarak uygun değil ve devam etmek için kaynak bağlantısı değişiklik vermiyor sona eriyor. Öznitelik mevcut uygulamalar tarafından kullanılmadığından eminseniz, hata gösterme hakkında bilgi için desteğe gerekir.

* Özel **userCertificate** özniteliği cihaz nesneleri, artık Azure AD Connect için gerekli sertifikaları değerler için görünür [etki alanına katılmış cihazlar Windows 10'u deneyimi için Azure AD'ye bağlanma](https://docs.microsoft.com/azure/active-directory/active-directory-azureadjoin-devices-group-policy) ve Azure AD ile eşitliyorsanız önce kalan filtreler. Bu davranışı etkinleştirmek için "Out için AAD – cihaz katılın SOAInAD" kutusunu giden eşitleme kuralı güncelleştirildi.

* Azure AD Connect artık desteklediği için geri yazma Exchange Online **cloudPublicDelegates** özniteliğini şirket içi AD **publicDelegates** özniteliği. Bu, burada bir Exchange Online posta kutusu kullanıcılara şirket içi Exchange posta kutusu ile SendOnBehalfTo haklar verilebilir senaryosu sağlar. Bu özellik, yeni bir out-of-box eşitleme kuralı "AD – kullanıcı Exchange karma PublicDelegates geri yazma için çıkış" desteklemek için eklendi. Exchange karma özelliği etkinleştirilmişse bu eşitleme kuralı yalnızca Azure AD Connect'e eklenir.

* Azure AD Connect artık destekliyor eşitleme **altRecipient** Azure ad özniteliği. Bu değişiklik desteklemek için aşağıdaki hazır olan eşitleme kuralları gerekli öznitelik akışı içerecek şekilde güncelleştirildi:
  * İçinde ad – kullanıcı Exchange
  * AAD – kullanıcı ExchangeOnline kullanıma al
  
* **CloudSOAExchMailbox** öznitelik meta veri deposu olarak, belirli bir kullanıcı Exchange Online posta kutusu olup olmadığını gösterir. Tanımı ek Exchange Online RecipientDisplayTypes gibi donanım ve Konferans odası posta kutularını olarak içerecek şekilde güncelleştirildi. Bu değişikliği etkinleştirmek için "İçinde gelen AAD – kullanıcı Exchange karma", kullanıma hazır eşitleme kuralı altında bulunan cloudSOAExchMailbox öznitelik tanımını gelen güncelleştirildi:

```
CBool(IIF(IsNullOrEmpty([cloudMSExchRecipientDisplayType]),NULL,BitAnd([cloudMSExchRecipientDisplayType],&amp;HFF) = 0))
```

… şu şekilde:

```
CBool(
  IIF(IsPresent([cloudMSExchRecipientDisplayType]),(
    IIF([cloudMSExchRecipientDisplayType]=0,True,(
      IIF([cloudMSExchRecipientDisplayType]=2,True,(
        IIF([cloudMSExchRecipientDisplayType]=7,True,(
          IIF([cloudMSExchRecipientDisplayType]=8,True,(
            IIF([cloudMSExchRecipientDisplayType]=10,True,(
              IIF([cloudMSExchRecipientDisplayType]=16,True,(
                IIF([cloudMSExchRecipientDisplayType]=17,True,(
                  IIF([cloudMSExchRecipientDisplayType]=18,True,(
                    IIF([cloudMSExchRecipientDisplayType]=1073741824,True,(
                       IF([cloudMSExchRecipientDisplayType]=1073741840,True,False)))))))))))))))))))),False))

```

* X509Certificate2 uyumlu işlevleri eşitleme userCertificate özniteliğinin yol açtığı sertifika değerleri işlemek için kural ifadeleri oluşturmak için aşağıdaki kümesi eklendi:

    ||||
    | --- | --- | --- |
    |CertSubject|CertIssuer|CertKeyAlgorithm|
    |CertSubjectNameDN|CertIssuerOid|CertNameInfo|
    |CertSubjectNameOid|CertIssuerDN|IsCert|
    |CertFriendlyName|Certthumbprınt|CertExtensionOids|
    |CertFormat|CertNotAfter|CertPublicKeyOid|
    |CertSerialNumber|CertNotBefore|CertPublicKeyParametersOid|
    |CertVersion|CertSignatureAlgorithmOid|Şunu seçin:|
    |CertKeyAlgorithmParams|CertHashString|Konum|
    |||Avantaj ile|

* Aşağıdaki şema değişiklikleri, sAMAccountName, domainNetBios ve Grup nesneleri için domainFQDN yanı sıra, kullanıcı nesnelerinin distinguishedName flow için özel bir eşitleme kuralları oluşturmak kanıtlayabilecekleri sunulmuştur:

  * Aşağıdaki öznitelikler MV şemaya eklenmiştir:
    * Grup: Hesap adı
    * Group: domainNetBios
    * Grup: domainFQDN
    * Kişi: distinguishedName

  * Aşağıdaki öznitelikler Azure AD Bağlayıcısı şemaya eklenmiştir:
    * Grup: OnPremisesSamAccountName
    * Grup: NetBiosName
    * Grup: DnsDomainName
    * Kullanıcı: OnPremisesDistinguishedName

* ADSyncDomainJoinedComputerSync cmdlet betik artık AzureEnvironment adlı yeni bir isteğe bağlı parametre var. Parametresi, ilgili Azure Active Directory kiracısı barındırılan hangi bölgeyi belirtmek için kullanılır. Geçerli değerler şunlardır:
  * AzureCloud (varsayılan)
  * AzureChinaCloud
  * AzureGermanyCloud
  * USGovernment
 
* Güncelleştirilmiş eşitleme kuralı kullanılacak düzenleyici eşitleme kuralı oluşturma sırasında bağlantı türünün varsayılan değer olarak (sağlamak yerine) katılın.

### <a name="ad-fs-management"></a>AD FS Yönetimi

#### <a name="issues-fixed"></a>Düzeltilen sorunlar

* URL'leri kimlik doğrulaması kesintiye karşı dayanıklılığı artırmak için Azure AD tarafından sunulan yeni WS-Federasyon uç noktaları ve şirket içine eklenecek AD FS yanıtlama taraf güven yapılandırması:
  * https://ests.login.microsoftonline.com/login.srf
  * https://stamp2.login.microsoftonline.com/login.srf
  * https://ccs.login.microsoftonline.com/login.srf
  * https://ccs-sdf.login.microsoftonline.com/login.srf
  
* AD FS için İssuerıd yanlış talep değeri oluşturmak neden bir sorun düzeltildi. Azure AD kiracısında birden çok doğrulanmış etki alanı vardır ve İssuerıd talebi oluşturmak için kullanılan userPrincipalName özniteliğinin etki alanı soneki en az ise sorun 3 düzey derin (örneğin, johndoe@us.contoso.com). Talep kuralları tarafından kullanılan normal ifade güncelleştirerek sorun çözülür.

#### <a name="new-features-and-improvements"></a>Yeni özellikler ve geliştirmeler
* Daha önce Azure AD Connect tarafından sağlanan ADFS sertifika yönetimi özelliği ile ADFS grupları Azure AD Connect yönetilen yalnızca kullanılabilir. Şimdi, Azure AD Connect kullanarak yönetilmeyen ile ADFS grupları özelliğini kullanabilirsiniz.

## <a name="115240"></a>1.1.524.0
Yayımlanma tarihi: Mayıs 2017

> [!IMPORTANT]
> Bu derleme sunulan şema ve eşitleme kuralı değişiklikler vardır. Azure AD Connect eşitleme hizmeti, yükseltmeden sonra tam içeri aktarma ve tam eşitleme adımları tetikler. Değişikliklerin ayrıntılarını aşağıda açıklanmıştır.
>
>

**Giderilen sorunlar:**

Azure AD Connect Eşitleme

* Müşteri Set-ADSyncAutoUpgrade cmdlet'ini kullanarak özelliği devre dışı olsa bile Azure AD Connect sunucusu üzerinde gerçekleşmesi otomatik yükseltme neden olan bir sorunu düzelttik. Bu düzeltmeyle sunucusundaki otomatik yükseltme işlemi hala yükseltme için düzenli olarak denetler, ancak otomatik yükseltme yapılandırma indirilen yükleyiciye geliştirir.
* Azure AD Connect, DirSync yerinde yükseltme sırasında Azure AD ile eşitlemek için Azure AD Bağlayıcısı tarafından kullanılacak bir Azure AD hizmet hesabını oluşturur. Hesap oluşturulduktan sonra hesabı kullanarak Azure AD ile Azure AD Connect kimlik doğrulaması yapar. Bazı durumlarda, kimlik doğrulama sırayla DirSync yerinde yükseltme hatası ile başarısız olmasına neden olan geçici bir sorun nedeniyle başarısız *"yürütme yapılandırma AAD eşitleme görevi bir hata oluştu: AADSTS50034: Bu uygulamaya oturum açmak için hesap xxx.onmicrosoft.com dizine eklenmesi gerekir."* Dırsync yükseltmesi esnekliğini geliştirmek için Azure AD Connect artık kimlik doğrulaması adımını yeniden dener.
* Başarılı olması bir DirSync yerinde yükseltme neden olan bir derleme 443 sorun oluştu, ancak dizin eşitlemesi için gereken çalıştırma profilleri oluşturulmaz. Mantıksal iyileştirme Azure AD Connect bu derlemeye dahil edilir. Müşteri için bu derleme yükseltildiğinde, Azure AD Connect çalıştırma profillerini eksik algılar ve bunları oluşturur.
* Parola Eşitleme işlemi, olay kimliği 6900 ve hata ile başarısız olmasına neden olan bir sorun düzeltildi *"aynı anahtara sahip bir öğe zaten eklendi"*. AD yapılandırma bölümü içerecek şekilde OU filtreleme yapılandırmasını güncelleştirirseniz, bu sorun oluşur. Bu sorunu gidermek için parola eşitleme işlemi yalnızca, AD etki alanı bölümleri parola değişikliklerini eşitler. Yapılandırma bölümü gibi etki alanı dışı bölümler atlanır.
* Express yüklemesi sırasında Azure AD Connect, şirket içi oluşturur tarafından AD Bağlayıcısı ile iletişim kurmak için kullanılacak AD DS hesabı şirket içi AD. Daha önce kullanıcı hesabı denetimi özniteliğini ayarlayın PASSWD_NOTREQD bayrağıyla hesabı oluşturulur ve rastgele bir parola hesabı ayarlanır. Şimdi, Azure AD Connect, açıkça hesapta parola ayarlandıktan sonra PASSWD_NOTREQD bayrağını kaldırır.
* Dırsync yükseltmesi başarısız olmasına neden olan hata ile bir sorun düzeltildi *"karşılıklı bir kilitlenme sql Server'da hangi bir uygulama kilidi oluştu"* mailNickname özniteliğinin şirket içinde bulunan zaman AD şema için sınırlanmış değil ancak AD kullanıcısı nesne sınıfı.
* Cihaz geri yazma özelliğini bir yönetici Azure AD Connect Sihirbazı'nı kullanarak Azure AD Connect eşitleme yapılandırmasını güncelleştirirken otomatik olarak devre dışı neden olan bir sorunu düzelttik. Bu sorun, bir önkoşul denetimi mevcut cihaz geri yazma yapılandırması şirket içi AD ve Denetim başarısız gerçekleştirme sihirbaz tarafından neden olur. Cihaz geri yazmayı zaten daha önce etkinleştirilmişse onay atlamak için açıklanmıştır.
* OU filtreleme yapılandırmak için ya da Azure AD Connect Sihirbazı'nı veya Eşitleme Hizmeti Yöneticisi'ni kullanabilirsiniz. OU filtreleme yapılandırmak için Azure AD Connect Sihirbazı'nı kullanırsanız, daha önce oluşturulan yeni OU'lar sonradan dizin eşitlemesi için dahil edilir. Eklenecek yeni OU'ların istemiyorsanız, Eşitleme Hizmeti Yöneticisi'ni kullanarak OU filtreleme yapılandırmanız gerekir. Şimdi, Azure AD Connect Sihirbazı'nı kullanarak aynı davranışı elde edebilirsiniz.
* Azure AD Connect tarafından yüklenen yönetim şemasını altında yerine dbo şemasında altında oluşturulması gerekir, saklı yordamlar neden olan bir sorunu düzelttik.
* AAD Connect sunucusu olay günlüklerine atlanacak Azure AD tarafından döndürülen Trackingıd öznitelik neden olan bir sorunu düzelttik. Azure AD Connect, Azure AD'den bir yeniden yönlendirme iletisi alır ve Azure AD Connect sağlanan uç noktasına bağlantı kuramazsa, sorun oluşur. Trackingıd destek mühendisleri tarafından sorun giderme sırasında hizmet tarafı günlükleriyle ilişkilendirmek için kullanılır.
* Azure AD Connect, Azure AD'den LargeObject hatası aldığında, Azure AD Connect EventID 6941 ve ileti sahip bir olay oluşturur. *"Sağlanan nesne çok büyük. Özniteliği bu nesnedeki değer sayısını azaltın."* Aynı zamanda, Azure AD Connect'i ayrıca EventID 6900 ve ileti yanıltıcı bir olay oluşturur. *"Microsoft.Online.Coexistence.ProvisionRetryException: Windows Azure Active Directory hizmetiyle iletişim kurulamıyor."* Karışıklığı en aza indirmek için Azure AD Connect artık LargeObject hatası alındığında ikinci olay oluşturur.
* Eşitleme Hizmeti Yöneticisi genel LDAP Bağlayıcısı için yapılandırma güncelleştirmesi sırasında yanıt veremez duruma gelmesine neden olan bir sorunu düzelttik.

**Yeni özellikler/iyileştirmeleri:**

Azure AD Connect Eşitleme
* Uygulanan eşitleme kuralı değişiklikler – aşağıdaki eşitleme kuralı:
  * Güncelleştirilmiş varsayılan eşitleme kuralı öznitelikleri vermemeniz kümesine **userCertificate** ve **Usersmımecertificate** öznitelikleri 15'ten fazla değer varsa.
  * AD öznitelikleri **EmployeeID** ve **msExchBypassModerationLink** artık varsayılan eşitleme kuralı kümesine eklenir.
  * AD özniteliği **fotoğraf** varsayılan eşitleme kural kümesinden kaldırılmıştır.
  * Eklenen **preferredDataLocation** meta veri deposu şeması ve AAD bağlayıcı şemasını. Azure AD'de her iki öznitelikleri güncelleştirmek isteyen müşteriler, bunu yapmak için özel eşitleme kuralları uygulayabilirsiniz. 
  * Eklenen **userType** meta veri deposu şeması ve AAD bağlayıcı şemasını. Azure AD'de her iki öznitelikleri güncelleştirmek isteyen müşteriler, bunu yapmak için özel eşitleme kuralları uygulayabilirsiniz.

* Azure AD Connect artık otomatik olarak kullanımını etkinleştirir ConsistencyGuid özniteliği kaynak bağlayıcı özniteliği olarak şirket içi AD nesneleri. Boş ise daha, Azure AD Connect ConsistencyGuid özniteliği objectGUID özniteliği değeri ile doldurur. Bu özellik, yalnızca yeni dağıtımı için geçerlidir. Bu özellik hakkında daha fazla bilgi için makale bölümüne bakın. [Azure AD Connect: Tasarım kavramları - ms-DS-Consistencyguid'i sourceAnchor olarak kullanma](plan-connect-design-concepts.md#using-ms-ds-consistencyguid-as-sourceanchor).
* Yeni cmdlet sorun giderme Invoke-ADSyncDiagnostics parola karması eşitleme tanılanmasına yardımcı olmak için ilgili sorunları eklendi. Cmdlet kullanma hakkında daha fazla bilgi için makalesine bakın [Azure AD Connect eşitlemesi ile parola karması eşitleme sorunlarını giderme](tshoot-connect-password-hash-synchronization.md).
* Azure AD Connect'i eşitleme posta ortak klasör nesneleri destekler artık Azure AD şirket içi. İsteğe bağlı özellikler altında Azure AD Connect sihirbazını kullanarak özelliğini etkinleştirebilirsiniz. Bu özellik hakkında daha fazla bilgi için makalesine bakın. [şirket içi posta etkin ortak klasörleri için destek Office 365 dizin tabanlı uç engelleme](https://blogs.technet.microsoft.com/exchange/2017/05/19/office-365-directory-based-edge-blocking-support-for-on-premises-mail-enabled-public-folders).
* Azure AD Connect gerektiren bir AD DS hesabı eşitleme kaynağı için şirket içi AD. Daha önce Express modu kullanılarak Azure AD Connect'in yüklü değilse, bir kuruluş yöneticisi hesabı ve Azure AD Connect kimlik bilgileri gerekli AD DS hesabı oluşturacak sağlayabilir. Ancak, özel bir yükleme ve ormanlar için mevcut bir dağıtım ekleme için bunun yerine, AD DS hesabı sağlamak gerekirdi. Şimdi, ayrıca özel bir yükleme sırasında bir kuruluş yöneticisi hesabının kimlik bilgilerini sağlayın ve Azure AD Connect'i gerekli AD DS hesabı oluşturma seçeneğiniz vardır.
* Azure AD Connect, artık SQL AOA destekler. Azure AD Connect'i yüklemeden önce SQL AOA etkinleştirmeniz gerekir. Yükleme sırasında Azure AD Connect veya sağlanan SQL örneğinin SQL AOA için etkin olup olmadığını algılar. SQL AOA etkinleştirilirse, daha fazla Azure AD Connect SQL AOA zaman uyumlu veya zaman uyumsuz çoğaltma kullanacak şekilde yapılandırıldı, rakamları. Kullanılabilirlik grubu dinleyicisi Kur ayarlarken RegisterAllProvidersIP özelliğini 0 olarak ayarlayın, önerilir. Bu, Azure AD Connect, SQL Native Client SQL'e bağlanmak için şu anda kullanır ve SQL Native Client MultiSubNetFailover özelliğinin kullanımını desteklemez çünkü önerilir.
* LocalDB için Azure AD Connect sunucusu veritabanı olarak kullanan ve 10 GB boyut sınırına ulaşıldı, eşitleme hizmeti artık başlatır. Daha önce eşitleme hizmeti başlatmak yeterli veritabanı alanını geri kazanarak LocalDB SHRINKDATABASE işlemi gerçekleştirmek gerekir. Sonra daha fazla veritabanı alanını geri kazanarak çalıştırma geçmişini silmek için Eşitleme Hizmeti Yöneticisi'ni kullanabilirsiniz. Şimdi, başlangıç ADSyncPurgeRunHistory cmdlet'i temizleme çalıştırma geçmişini Localdb'den veritabanı alanını geri kazanarak kullanabilirsiniz. Ayrıca, bu cmdlet bir çevrimdışı modu destekler (belirterek - offline parametresini) kullanılabilir eşitleme hizmeti çalışmadığı zaman. Not: Çevrimdışı modda, yalnızca eşitleme hizmeti çalışmıyor ve kullanılan veritabanı LocalDB ise kullanılabilir.
* Gereken depolama alanı Azure AD'ye miktarını azaltmak için Connect artık eşitleme hata ayrıntılarını LocalDB/SQL veritabanlarında depolamadan önce sıkıştırır. Azure AD Connect, daha eski bir Azure AD Connect sürümünden bu sürüme yükseltirken, mevcut eşitleme hata ayrıntıları tek seferlik bir sıkıştırma gerçekleştirir.
* Daha önce OU filtreleme yapılandırmasını güncelleştirdikten sonra el ile var olan nesnelerin düzgün dahil/dizin eşitlemesini dışlanan emin olmak için tam içeri aktarma çalıştırmanız gerekir. Şimdi, Azure AD Connect tam içeri aktarma otomatik olarak tetikleyen sırasında sonraki eşitleme döngüsü. Daha fazla, tam içeri aktarma olduğundan yalnızca uygulanabilir güncelleştirmeden etkilenen AD bağlayıcılar. Not: Bu geliştirme, OU filtreleme yalnızca Azure AD Connect Sihirbazı kullanılarak yapılan güncelleştirmeler için geçerlidir. Eşitleme Hizmeti Yöneticisi kullanılarak yapılan güncelleştirme filtreleme OU için geçerli değildir.
* Daha önce kullanıcıları, grupları, Grup tabanlı filtreleme destekler ve yalnızca ilgili nesneleri. Artık, Grup tabanlı filtreleme de bilgisayar nesnelerini destekler.
* Daha önce Azure AD Connect Eşitleme Zamanlayıcısı'nı devre dışı bırakma, bağlayıcı alanı verileri silebilirsiniz. Şimdi, Zamanlayıcı'nın etkin olduğunu algılarsa, Eşitleme Hizmeti Yöneticisi bağlayıcı alanına veri silinmesini engeller. Daha fazla bağlayıcı alanı verileri silinirse, olası veri kaybı müşterilere bildirmek için bir uyarı döndürdü.
* Daha önce PowerShell döküm düzgün çalışması Azure AD Connect Sihirbazı için devre dışı bırakmalısınız. Bu sorunu kısmen daha çözümlenir. Eşitleme yapılandırması yönetmek için Azure AD Connect Sihirbazı'nı kullanıyorsanız, PowerShell transkripsiyonu etkinleştirebilirsiniz. AD FS yapılandırmasını yönetmek için Azure AD Connect Sihirbazı'nı kullanıyorsanız, PowerShell transkripsiyonu devre dışı bırakmanız gerekir.



## <a name="114860"></a>1.1.486.0
Yayımlanma tarihi: Nisan 2017

**Giderilen sorunlar:**
* Burada Azure AD Connect'i başarıyla yerelleştirilmiş sürümünü Windows Server üzerinde yüklenmez neden olan sorun düzeltildi.

## <a name="114840"></a>1.1.484.0
Yayımlanma tarihi: Nisan 2017

**Bilinen sorunlar:**

* Aşağıdaki koşulların tümü doğruysa, bu Azure AD Connect sürümü başarıyla yüklenmez:
   1. Azure AD Connect ya da DirSync yerinde yükseltme veya temiz yükleme yapıyorsanız.
   2. Windows Server'ın nerede sunucu üzerinde yerleşik yönetici grubunun adı "Yöneticiler" değil yerelleştirilmiş bir sürümünü kullanıyor.
   3. Varsayılan SQL Server 2012 Express tam SQL sağlamak yerine Azure AD Connect ile yüklenen LocalDB kullanıyor.

**Giderilen sorunlar:**

Azure AD Connect Eşitleme
* Burada Eşitleme Zamanlayıcısı tüm eşitleme adımını atlar çalıştırma profili bu eşitleme adımı için bir veya daha fazla bağlayıcı eksikse bir sorun düzeltildi. Örneğin, el ile değişikliği içeri aktarma çalıştırma profili için oluşturmadan Eşitleme Hizmeti Yöneticisi'ni kullanarak bir bağlayıcı eklendi. Bu düzeltme, Eşitleme Zamanlayıcısı değişikliği içeri aktarma için diğer bağlayıcıları çalışmaya devam etmesini sağlar.
* Çalışma adımları bir sorun karşılaştığında olduğunda burada eşitleme hizmetini hemen çalıştırma profili işlemeyi durdurur, bir sorun düzeltildi. Bu düzeltme, eşitleme hizmeti çalıştıran adımı atlar ve kalan işlemeye devam sağlar. Örneğin, değişikliği içeri aktarma (her biri için bir AD etki alanı şirket) ile birden çok çalıştırma adımları çalıştırma profili için AD Bağlayıcınızı var. Bunlardan biri ağ bağlantısı sorunları olsa bile eşitleme hizmeti değişikliği içeri aktarma diğer AD etki alanlarıyla çalıştırın.
* Otomatik yükseltme sırasında atlanacak Azure AD Bağlayıcısı güncelleştirme neden olan bir sorunu düzelttik.
* Hatalı Sunucu Kurulum sırasında bir etki alanı denetleyicisi olup olmadığını belirlemek Azure AD Connect neden olan bir sorunu düzelttik, hangi içinde Aç Dırsync yükseltmesi başarısız olmasına neden olur.
* Her çalıştırma profili için Azure AD Bağlayıcısı oluşturma bir DirSync yerinde yükseltme neden olan bir sorunu düzelttik.
* Burada Eşitleme Hizmeti Yöneticisi kullanıcı arabirimi genel LDAP Bağlayıcısı yapılandırılmaya çalışılırken yanıt vermemeye başlıyor bir sorun düzeltildi.

AD FS Yönetimi
* AD FS birincil düğüm başka bir sunucuya taşınırsa, burada Azure AD Connect Sihirbazı başarısız bir sorun düzeltildi.

Masaüstü SSO
* Bir sorun, burada oturum açma ekranı, oturum açma sırasında yeni bir yükleme seçeneği olarak Password Synchronization'ı seçerseniz, Masaüstü SSO özelliğini etkinleştirmek izin vermez Azure AD Connect Sihirbazı'nda düzeltildi.

**Yeni özellikler/iyileştirmeleri:**

Azure AD Connect Eşitleme
* Azure AD Connect Sync artık sanal hizmet hesabı, yönetilen hizmet hesabı ve Grup yönetilen hizmet hesabı hizmet hesabı olarak kullanımını destekler. Bu, yalnızca Azure AD Connect yeni yükleme için geçerlidir. Azure AD Connect yüklerken:
    * Varsayılan olarak, Azure AD Connect Sihirbazı, bir sanal hizmet hesabı oluşturur ve kendi hizmet hesabı olarak kullanır.
    * Bir etki alanı denetleyicisinde yüklüyorsanız, Azure AD Connect için burada bir etki alanı kullanıcı hesabı oluşturur ve bunun yerine hizmet hesabı olarak kullanan önceki davranış geri döner.
    * Aşağıdakilerden birini sağlayarak varsayılan davranışı geçersiz kılabilirsiniz:
      * Bir grup yönetilen hizmet hesabı
      * Yönetilen hizmet hesabı
      * Bir etki alanı kullanıcı hesabı
      * Yerel bir kullanıcı hesabı
* Daha önce Azure AD Connect içeren yeni bir derleme yükseltirseniz bağlayıcılar güncelleştirebilir veya eşitleme kuralı değişiklikleri, Azure AD Connect, bir tam eşitleme döngüsü tetikler. Şimdi, Azure AD Connect seçmeli olarak tam içeri aktarma adım güncelleştirme ile bağlayıcılar için yalnızca ve yalnızca eşitleme kuralı değişiklikleri Bağlayıcılarla tam eşitleme adımını tetikler.
* Daha önce dışarı aktarma silme eşiği yalnızca eşitleme Zamanlayıcı ile tetiklenen dışarı aktarmaları için geçerlidir. Şimdi, özelliği el ile Eşitleme Hizmeti Yöneticisi'ni kullanarak müşteri tarafından tetiklenen dışarı aktarmaları içerecek şekilde genişletilir.
* Azure AD kiracınızda parola eşitleme özelliğini veya kiracınız için etkinleştirilip etkinleştirilmeyeceğini gösteren bir hizmet yapılandırması yok. Daha önce etkin ve bir hazırlık sunucusu varsa, Azure AD Connect tarafından yanlış yapılandırılması için hizmet yapılandırması kolaydır. Şimdi, Azure AD Connect hizmet yapılandırması, etkin tutarlı tutmak çalışacak yalnızca Azure AD Connect sunucusu.
* Azure AD Connect Sihirbazı artık algılar ve bir uyarı verir AD AD geri dönüşüm kutusu etkin olmayan şirket içi.
* Daha önce toplu işlemdeki nesnelerin birleştirilmiş boyutu belirli bir eşiği aşarsa başarısız olur ve Azure AD zaman aşımına ile dışarı aktarın. Şimdi, eşitleme hizmeti sorunla karşılaşılırsa nesneleri ayrı, daha küçük toplu işler halinde yeniden göndermeyi yeniden deneyecek.
* Eşitleme hizmeti anahtar yönetimi uygulaması, Windows Başlat Menüsü'nden kaldırıldı. Şifreleme anahtarı yönetimi miiskmu.exe kullanarak komut satırı arabirimi aracılığıyla desteklenmeye devam edecektir. Şifreleme anahtarı'nı yönetme hakkında daha fazla bilgi için makalesine bakın [Azure AD Connect Sync şifreleme anahtarını bırakıp](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-change-serviceacct-pass#abandoning-the-azure-ad-connect-sync-encryption-key).
* Azure AD Connect eşitleme hizmeti hesabı parolasını değiştirirseniz, şifreleme anahtarını terk ve Azure AD Connect eşitleme hizmeti hesabı parolasını yeniden kadar daha önce eşitleme hizmeti mümkün başlangıç doğru olmayacak. Şimdi, bu işlem artık gerekli değildir.

Masaüstü SSO

* Azure AD Connect Sihirbazı artık bağlantı noktası 9090'dır, geçişli kimlik doğrulaması ve Masaüstü SSO yapılandırırken ağ üzerinde açılmasını gerektirir. Yalnızca bağlantı noktası 443 gereklidir. 

## <a name="114430"></a>1.1.443.0
Yayımlanma tarihi: Mart 2017

**Giderilen sorunlar:**

Azure AD Connect Eşitleme
* Azure AD Connect Sihirbazı, Azure AD Bağlayıcısı görünen adını Azure AD kiracısına atanan ilk onmicrosoft.com etki alanınız yoksa başarısız olmasına neden olan sorun düzeltildi.
* Azure AD Connect Sihirbazı, eşitleme hizmeti hesabının parolasını kesme işareti, virgül ve boşluk gibi özel karakterler içerdiğinde, SQL veritabanı bağlantısı yaparken başarısız olmasına neden olan sorun düzeltildi.
* "Görüntü görüntüden daha farklı bir yer işaretine sahip" hataya neden olan bir sorun düzeltildi, geçici olarak bir şirket içi dışarıda sonra hazırlama modunda, bir Azure AD Connect sunucusu üzerinde gerçekleşmesi için AD nesne eşitlenmesini ve ardından yeniden eşitleme için dahil.
* "DN bulunan nesne hayali bir" hataya neden olan bir sorun düzeltildi, geçici olarak bir şirket içi dışarıda sonra hazırlama modunda, bir Azure AD Connect sunucusu üzerinde gerçekleşmesi için AD nesne eşitlenmesini ve ardından yeniden eşitleme için dahil.

AD FS Yönetimi
* Burada Azure AD Connect Sihirbazı değil AD FS yapılandırmasını güncelleştirin ve alternatif oturum açma Kimliğini yapılandırıldıktan sonra bağlı olan taraf güvenine sağ taleplere ayarlayın, bir sorun düzeltildi.
* Azure AD Connect Sihirbazı'nı AD FS sunucuları sAMAccountName biçimi yerine userPrincipalName biçimini kullanarak hizmet hesapları yapılandırılmış olan doğru şekilde işleyemedi olduğu bir sorun düzeltildi.

Doğrudan Kimlik Doğrulama
* Azure AD Connect Sihirbazı aracılığıyla başarılı kimlik doğrulaması seçildiğinde ancak kendi bağlayıcı kaydı başarısız olursa başarısız olmasına neden olan sorun düzeltildi.
* Neden olur, Masaüstü SSO özelliği etkinleştirilmişse seçilen oturum açma yöntemi üzerinde doğrulama atlama için Azure AD Connect Sihirbazı'nı denetler bir sorun düzeltildi.

Parola Sıfırlama
* Azure AAD Connect sunucusu bağlantısı bir güvenlik duvarı veya Ara sunucu tarafından sonlandırıldı. yeniden bağlanmak kullanmamanız neden olabilecek bir sorun düzeltildi.

**Yeni özellikler/iyileştirmeleri:**

Azure AD Connect Eşitleme
* Get-ADSyncScheduler cmdlet artık SyncCycleInProgress adlı yeni bir Boolean özelliğini döndürür. Döndürülen değer true ise, devam eden bir zamanlanmış bir eşitleme döngüsü olduğunu gösterir.
* Azure AD Connect yükleme ve Kurulum günlüklerini depolamak için hedef klasör %localappdata%\AADConnect günlük dosyaları için erişilebilirliği iyileştirmek üzere %programdata%\AADConnect için taşındı.

AD FS Yönetimi
* AD FS grubunun SSL sertifikasını güncelleştirmek için destek eklendi.
* AD FS 2016 yönetmek için destek eklendi.
* Şimdi, AD FS yükleme sırasında mevcut gmsa'yı (Grup yönetilen hizmet hesabı) belirtebilirsiniz.
* Şimdi, Azure AD bağlı olan taraf güveni için imza karma algoritma olarak SHA-256'yı da yapılandırabilirsiniz.

Parola Sıfırlama
* İşlev ürüne daha sıkı güvenlik duvarı kuralları içeren ortamlarda izin vermek için sunulan geliştirmeler.
* Azure Service Bus geliştirilmiş bağlantı güvenilirlik.

## <a name="113800"></a>1.1.380.0
Yayımlanma tarihi: Aralık 2016

**Sorun düzeltildi:**

* Active Directory Federasyon Hizmetleri (AD FS) issuerıd talebi kuralı bu derlemede eksik olduğu sorun düzeltildi.

>[!NOTE]
>Bu derleme için Azure AD Connect otomatik yükseltme özelliği aracılığıyla müşteriler tarafından kullanılabilir değil.

## <a name="113710"></a>1.1.371.0
Yayımlanma tarihi: Aralık 2016

**Bilinen sorun:**

* Bu derleme için AD FS issuerıd talebi kuralı eksik. Azure Active Directory (Azure AD) ile birden çok etki alanı birleştirirken issuerıd talebi kuralı gereklidir. Azure AD Connect, şirket içi yönetmek için kullanıyorsanız, AD FS dağıtımı, bu derleme için yükseltme, AD FS'yi yapılandırmanızı mevcut issuerıd talebi kuralı kaldırır. Yükleme/yükseltme sonrasında issuerıd talebi kuralı ekleyerek bu sorunu geçici olarak çalışabilir. Talep kuralı issuerıd ekleme ile ilgili ayrıntılar için bu makaleye göz atın [Azure AD ile Federasyon için çoklu etki alanı desteği](how-to-connect-install-multiple-domains.md).

**Sorun düzeltildi:**

* Giden bağlantı için bağlantı noktası 9090'dır açık değilse Azure AD Connect yüklemesi veya yükseltmesi başarısız olur.

>[!NOTE]
>Bu derleme için Azure AD Connect otomatik yükseltme özelliği aracılığıyla müşteriler tarafından kullanılabilir değil.

## <a name="113700"></a>1.1.370.0
Yayımlanma tarihi: Aralık 2016

**Bilinen sorunlar:**

* Bu derleme için AD FS issuerıd talebi kuralı eksik. Azure AD ile birden çok etki alanı birleştirirken issuerıd talebi kuralı gereklidir. Azure AD Connect, şirket içi yönetmek için kullanıyorsanız, AD FS dağıtımı, bu derleme için yükseltme, AD FS'yi yapılandırmanızı mevcut issuerıd talebi kuralı kaldırır. Yükleme/yükseltme sonrasında issuerıd talebi kuralı ekleyerek bu sorunu geçici olarak çalışabilir. Talep kuralı issuerıd ekleme ile ilgili ayrıntılar için bu makaleye göz atın [Azure AD ile Federasyon için çoklu etki alanı desteği](how-to-connect-install-multiple-domains.md).
* Bağlantı noktası 9090 gerekir olması açın yüklemenin tamamlanabilmesi için giden.

**Yeni özellikler:**

* Geçişli kimlik doğrulaması (Önizleme).

>[!NOTE]
>Bu derleme için Azure AD Connect otomatik yükseltme özelliği aracılığıyla müşteriler tarafından kullanılabilir değil.

## <a name="113430"></a>1.1.343.0
Yayımlanma tarihi: Kasım 2016

**Bilinen sorun:**

* Bu derleme için AD FS issuerıd talebi kuralı eksik. Azure AD ile birden çok etki alanı birleştirirken issuerıd talebi kuralı gereklidir. Azure AD Connect, şirket içi yönetmek için kullanıyorsanız, AD FS dağıtımı, bu derleme için yükseltme, AD FS'yi yapılandırmanızı mevcut issuerıd talebi kuralı kaldırır. Yükleme/yükseltme sonrasında issuerıd talebi kuralı ekleyerek bu sorunu geçici olarak çalışabilir. Talep kuralı issuerıd ekleme ile ilgili ayrıntılar için bu makaleye göz atın [Azure AD ile Federasyon için çoklu etki alanı desteği](how-to-connect-install-multiple-domains.md).

**Giderilen sorunlar:**

* Bazı durumlarda, bir yerel hizmet hesabı parolasını kuruluşunuzun parola ilkesi tarafından belirtilen bir karmaşıklık düzeyi karşıladığını oluşturamadı, çünkü Azure AD Connect yükleme başarısız olur.
* Bir nesne bağlayıcı alanında aynı anda kapsam dışı olduğunda burada birleştirme kuralları yeniden değerlendirilerek değil bir sorun düzeltildi bir kural katılın ve kapsam haline başka bir. Birbirini dışlayan olan birleştirme koşulları iki veya daha fazla birleşim kurallarınız varsa bu durum oluşabilir.
* Bunlar birleştirme kuralları içeren daha düşük önceliğe değerler varsa birleştirme kuralları içermez, gelen eşitleme kurallarını (Azure AD'den), burada işlenmez bir sorun düzeltildi.

**İyileştirmeler:**

* Windows Server 2016 Standard veya üzeri sürümde Azure AD Connect'i yüklemek için destek eklendi.
* SQL Server 2016, Azure AD Connect için Uzak veritabanı olarak kullanma desteği eklendi.

## <a name="112810"></a>1.1.281.0
Yayımlanma tarihi: Ağustos 2016

**Giderilen sorunlar:**

* Sonraki eşitleme döngüsü tamamlandıktan sonra aralığı eşitlemek için değişiklikleri kadar yer almaz.
* Azure AD Connect Sihirbazı, bir Azure AD hesabı kullanıcı adı bir alt çizgiyle başlayan kabul etmez (\_).
* Azure AD Connect sihirbazının hesap parolası çok fazla özel karakterler içeriyorsa Azure AD hesabı kimlik doğrulaması başarısız. "Kimlik bilgileri doğrulanamadı hata iletisi. Beklenmeyen bir hata oluştu." döndürülür.
* Sunucu hazırlama kaldırma, Azure AD kiracısında parola eşitleme devre dışı bırakır ve parola eşitleme ile etkin sunucu başarısız olmasına neden olur.
* Parola Eşitleme, depolanan kullanıcı hiçbir parola karması olduğunda nadir durumlarda başarısız olur.
* Azure AD Connect sunucusu hazırlama modu etkinleştirildiğinde, parola geri yazma geçici olarak devre dışı değil.
* Sunucu hazırlama modunda olduğunda azure AD Connect Sihirbazı gerçek parola eşitleme ve parola geri yazma yapılandırması göstermez. Bu her zaman bunları devre dışı olarak gösterir.
* Parola Eşitleme ve parola geri yazmayı yapılandırma değişikliklerini sunucusu hazırlama modunda olduğunda Azure AD Connect Sihirbazı tarafından kalıcı değildir.

**İyileştirmeler:**

* Başarıyla veya yeni bir eşitleme döngüsü başlatmak mümkün olup olmadığını belirtmek için başlangıç ADSyncSyncCycle cmdlet'i güncelleştirildi.
* Devam eden eşitleme döngüsü ve işlemi sonlandırmak için Stop-ADSyncSyncCycle cmdlet'i eklendi.
* Devam eden eşitleme döngüsü ve işlemi sonlandırmak için Stop-ADSyncScheduler cmdlet'i güncelleştirildi.
* Yapılandırma sırasında [dizin genişletmeleri](how-to-connect-sync-feature-directory-extensions.md) Azure AD Connect Sihirbazı'nda Azure AD öznitelik "Teletex dize" türü artık seçilebilir.

## <a name="111890"></a>1.1.189.0
Yayımlanma tarihi: Haziran 2016

**Sorunlar düzeltildi ve geliştirmeleri:**

* Azure AD Connect artık FIPS ile uyumlu bir sunucuya yüklenebilir.
  * Parola eşitlemesi için bkz: [parola karma eşitlemesi ve FIPS](how-to-connect-password-hash-synchronization.md#password-hash-synchronization-and-fips).
* Burada bir NetBIOS adı için Active Directory Bağlayıcısı FQDN çözümlenemedi bir sorun düzeltildi.

## <a name="111800"></a>1.1.180.0
Yayımlanma tarihi: Mayıs 2016

**Yeni özellikler:**

* Uyarır ve Azure AD Connect çalıştırmadan önce başarmadık etki alanı doğrulama yardımcı olur.
* İçin destek eklendi [Microsoft Cloud Almanya](reference-connect-instances.md#microsoft-cloud-germany).
* En son desteği eklendi [Microsoft Azure kamu bulutundaki](reference-connect-instances.md#microsoft-azure-government-cloud) yeni URL gereksinimlerini altyapısıyla.

**Sorunlar düzeltildi ve geliştirmeleri:**

* Eşitleme kuralları bulmayı kolaylaştırmak için eşitleme kuralı Düzenleyicisi filtreleme eklendi.
* Bağlayıcı alanı silinirken performansı İyileştirildi.
* Aynı nesne hem silindi ve aynı çalıştırmada (çağrılan silme/add) eklenen bir sorun düzeltildi.
* Devre dışı bırakılmış bir eşitleme kuralının artık nesneler dahil yeniden etkinleştirir ve öznitelikleri yükseltme ya da dizin şemasını Yenile.

## <a name="111300"></a>1.1.130.0
Yayımlanma tarihi: Nisan 2016

**Yeni özellikler:**

* Birden çok değerli öznitelikler için destek eklendi [dizin genişletmeleri](how-to-connect-sync-feature-directory-extensions.md).
* Daha fazla yapılandırma çeşitliliğe desteği eklendi [otomatik yükseltme](how-to-connect-install-automatic-upgrade.md) yükseltme için uygun olarak değerlendirilmesi için.
* Bazı cmdlet'ler için eklenen [özel Zamanlayıcı](how-to-connect-sync-feature-scheduler.md#custom-scheduler).

## <a name="111190"></a>1.1.119.0
Yayımlanma tarihi: Mart 2016

**Giderilen sorunlar:**

* Yapılan emin Express yüklemesi Windows Server 2008'de (pre-R2) kullanılamaz çünkü parola eşitleme bu işletim sisteminde desteklenmiyor.
* Özel filtre yapılandırmasıyla dirsync'ten yükseltme, beklendiği gibi çalışmaması.
* Daha yeni bir sürümüne yükseltirken ve yapılandırmasında bir değişiklik yok, tam içeri aktarma/eşitleme değil zamanlanmalıdır.

## <a name="111100"></a>1.1.110.0
Yayımlanma tarihi: 2016 Şubat

**Giderilen sorunlar:**

* Yükleme varsayılan C:\Program Files klasörü içinde değilse, önceki sürümlerinden yükseltme çalışmaz.
* Yükle ve Temizle **eşitleme işlemini başlatmak** yükleme sihirbazının sonunda, Yükleme Sihirbazı'nı ikinci kez çalışan Zamanlayıcı izin vermez.
* Zamanlayıcı, ABD-İngilizcesi tarih/saat biçimi değil kullanıldığı sunucularda beklendiği gibi çalışmaz. Ayrıca engeller `Get-ADSyncScheduler` doğru kez dönün.
* Yükseltme ve oturum açma seçeneği AD FS ile Azure AD Connect'in önceki bir sürümünü yüklediyseniz, Yükleme Sihirbazı'nı yeniden çalıştıramazsınız.

## <a name="111050"></a>1.1.105.0
Yayımlanma tarihi: 2016 Şubat

**Yeni özellikler:**

* [Otomatik yükseltme](how-to-connect-install-automatic-upgrade.md) Express ayarları müşteriler için özellik.
* Yükleme Sihirbazı'nda Azure multi-Factor Authentication ve Privileged Identity Management kullanarak genel yönetici desteği.
  * Ayrıca trafiğe izin vermek bir proxy sunucunuz izin vermeniz gerekir https://secure.aadcdn.microsoftonline-p.com multi-Factor Authentication'ı kullanıyorsanız.
  * Eklemenize gerek https://secure.aadcdn.microsoftonline-p.com Güvenilen siteler listenize multi-Factor Authentication'ın düzgün çalışması için.
* İlk yüklemeden sonra kullanıcının oturum açma yöntemini değiştirme izin verin.
* İzin [etki alanı ve OU filtreleme](how-to-connect-install-custom.md#domain-and-ou-filtering) Yükleme Sihirbazı'nda. Bu, aynı zamanda tüm etki alanları kullanılabilir olduğu ormanlara bağlama sağlar.
* [Zamanlayıcı](how-to-connect-sync-feature-scheduler.md) eşitleme altyapısında yerleşik olarak.

**Genel kullanıma Preview sürümünden yükseltilen özellikler:**

* [Cihaz geri yazmayı](how-to-connect-device-writeback.md).
* [Dizin genişletmeleri](how-to-connect-sync-feature-directory-extensions.md).

**Yeni Önizleme özellikleri:**

* Yeni varsayılan eşitleme döngüsü aralığı 30 dakikadır. Üç saat tüm önceki sürümler için kullanılabilir. Değiştirmek için destek ekler [Zamanlayıcı](how-to-connect-sync-feature-scheduler.md) davranışı.

**Giderilen sorunlar:**

* Doğrulama DNS etki alanları sayfasına her zaman etki alanları tanıyamadık.
* AD FS yapılandırma sırasında etki alanı yönetici kimlik bilgilerini ister.
* Şirket içi AD hesaplarının tanınmıyor Yükleme Sihirbazı tarafından farklı bir DNS ağaç kök etki alanı daha olan bir etki alanında bulunan durumunda.

## <a name="1091310"></a>1.0.9131.0
Yayımlanma tarihi: Aralık 2015

**Giderilen sorunlar:**

* Bir parola belirlediğinizde parolaları Active Directory etki alanı Hizmetleri (AD DS), ancak works değiştirdiğinizde, parola eşitleme çalışmayabilir.
* Bir proxy sunucunuz varsa, yükleme sırasında Azure ad kimlik doğrulaması başarısız olabilir veya yapılandırma sayfasında Yükseltme iptal edildi.
* Bir SQL Server Sistem Yöneticisi (SA) olmayan tam bir SQL Server örneği ile Azure AD Connect'in önceki bir sürümünden güncelleştirme başarısız olur.
* Azure AD Connect ile uzak bir SQL Server'ın önceki bir sürümünden güncelleştirme "Erişilemedi ad eşitleme SQL veritabanı" hata gösterir.

## <a name="1091250"></a>1.0.9125.0
Yayımlanma tarihi: 2015 Kasım

**Yeni özellikler:**

* AD FS için Azure AD güvenini yeniden yapılandırabilirsiniz.
* Active Directory şemasını Yenile ve eşitleme kuralları yeniden kullanabilirsiniz.
* Bir eşitleme kuralı devre dışı bırakabilirsiniz.
* "AuthoritativeNull", bir eşitleme kuralı içinde yeni bir sabit değer olarak tanımlayabilirsiniz.

**Yeni Önizleme özellikleri:**

* [Azure AD Connect Health eşitleme](how-to-connect-health-sync.md).
* Destek [Azure AD Domain Services](../user-help/active-directory-passwords-update-your-own-password.md) parola eşitleme.

**Yeni desteklenen senaryo:**

* Birden çok şirket içi Exchange kuruluşu destekler. Daha fazla bilgi için [birden çok Active Directory ormanı ile karma dağıtımlar](https://technet.microsoft.com/library/jj873754.aspx).

**Giderilen sorunlar:**

* Parola eşitleme sorunlarını:
  * Kapsam için Çıkış'ın kapsamının dışında taşınmış bir nesne parolayla eşitlenen yoktur. Bu, OU ve öznitelik filtreleme içerir.
  * Tam parola eşitleme eşitlenmiş dahil etmek için yeni bir OU seçerek gerektirmez.
  * Devre dışı bir kullanıcı etkinleştirilmişse parola eşitlemez.
  * Parola yeniden deneme kuyruğu sonsuzdur ve önceki sınır 5.000 nesnelerin kullanımdan kaldırılmıştır.
* Windows Server 2016 orman işlev düzeyi Active Directory'ye bağlanmak karşılaştırılamıyor.
* İlk yüklemeden sonra Grup filtreleme için kullanılan grubunu değiştirmek karşılaştırılamıyor.
* Artık Azure AD Connect sunucusunda etkin parola geri yazma ile parola değişikliği yapmadan her kullanıcı için yeni bir kullanıcı profili oluşturur.
* Eşitleme kuralları kapsamları uzun tamsayı değerleri kullanmak mümkün.
* Erişilemeyen etki alanı denetleyicileri varsa "cihaz geri yazmayı" onay kutusunu devre dışı kalır.

## <a name="1086670"></a>1.0.8667.0
Yayımlanma tarihi: Ağustos 2015

**Yeni özellikler:**

* Azure AD Connect Yükleme Sihirbazı'nı şimdi tüm Windows Server diller için yerelleştirilmiş.
* Hesap için destek eklendi Azure AD parola yönetimi kullanırken kilidini açın.

**Giderilen sorunlar:**

* Azure AD Connect Yükleme Sihirbazı, başka bir kullanıcı ilk kez yüklemeyi başlatan kişi yerine yükleme devam ederse kilitleniyor.
* Azure AD Connect eşitleme düzgün bir şekilde kaldırmak, Azure AD Connect'in önceki bir kaldırma başarısız olursa, yeniden yüklemek mümkün değildir.
* Kullanıcı orman kök etki alanında değilse veya Active Directory İngilizce olmayan bir sürümünü kullandıysanız hızlı yükleme kullanarak Azure AD Connect yükleyemezsiniz.
* Active Directory kullanıcı hesabının FQDN'sini çözümlenemezse "şema işleme başarısız" yanıltıcı bir hata iletisi gösterilir.
* Active Directory Bağlayıcısı üzerinde kullanılan hesap Sihirbazı dışında değiştirilirse, sihirbazın sonraki çalıştırmalar başarısız olur.
* Azure AD Connect, bazen bir etki alanı denetleyicisinde yükleme başarısız olur.
* Olamaz, etkinleştirin ve uzantı öznitelikleri eklediyseniz, "Hazırlama modu" devre dışı bırakın.
* Parola geri yazma bazı yapılandırmalarda, Active Directory Bağlayıcısı hatalı parola nedeniyle başarısız olur.
* Öznitelik filtreleme bir ayırt edici ad (DN) kullanılıyorsa, DirSync yükseltilemiyor.
* Parola sıfırlaması kullanırken, aşırı CPU kullanımı.

**Kaldırılan Önizleme özellikleri:**

* Önizleme özelliği [kullanıcı geri yazma](how-to-connect-preview.md#user-writeback) geçici olarak Önizleme müşterilerimizin görüşleri temel alınarak kaldırıldı. Daha sonra tekrar size sağlanan geri bildirim giderdikten sonra eklenecektir.

## <a name="1086410"></a>1.0.8641.0
Yayımlanma tarihi: Haziran 2015

**Azure AD Connect ilk sürümü.**

Azure AD Connect için değiştirilen adı Azure AD eşitleme.

**Yeni özellikler:**

* [Hızlı Ayarlar](how-to-connect-install-express.md) yükleme
* İçin [AD FS yapılandırma](how-to-connect-install-custom.md#configuring-federation-with-ad-fs)
* İçin [dirsync'ten yükseltme](how-to-dirsync-upgrade-get-started.md)
* [Yanlışlıkla silmeleri engelleme](how-to-connect-sync-feature-prevent-accidental-deletes.md)
* Tanıtılan [hazırlama modu](how-to-connect-sync-staging-server.md)

**Yeni Önizleme özellikleri:**

* [Kullanıcı geri yazma](how-to-connect-preview.md#user-writeback)
* [Grup geri yazma](how-to-connect-preview.md#group-writeback)
* [Cihaz geri yazma](how-to-connect-device-writeback.md)
* [Dizin genişletmeleri](how-to-connect-preview.md)

## <a name="104940501"></a>1.0.494.0501
Yayımlanma tarihi: Mayıs 2015

**Yeni gereksinimi:**

* Azure AD Sync artık yüklenmesi için .NET Framework 4.5.1 sürümünü gerektirir.

**Giderilen sorunlar:**

* Azure ad parola geri yazma, bir Azure Service Bus bağlantı hatasıyla başarısız oluyor.

## <a name="104910413"></a>1.0.491.0413
Yayımlanma tarihi: Nisan 2015

**Sorunlar düzeltildi ve geliştirmeleri:**

* Active Directory Bağlayıcısı siler doğru geri dönüşüm kutusu etkinse ve ormandadır birden çok etki alanı işlemez.
* Alma işlemlerinin performansı için Azure Active Directory Bağlayıcısı İyileştirildi.
* Ne zaman bir gruba üyelik sınırını aştı (varsayılan olarak, sınırı 50.000 nesnelere ayarlanır), Azure Active Directory'de Grup silindi. Yeni davranış Grup silinmez, bir hata oluşturulur ve yeni üyelik değişiklikleri aktarılmaz.
* Aynı DN'si hazırlanmış bir silme ve bağlayıcı alanında buna zaten varsa, yeni bir nesne sağlanamıyor.
* Nesne üzerinde aşamalı değişiklik olsa bile bazı nesneler delta eşitlemesi sırasında eşitleme için işaretlenir.
* Parola eşitlemeyi zorlama, tercih edilen DC listesi kaldırır.
* CSExportAnalyzer bazı nesneler durumları ile ilgili sorunlar var.

**Yeni özellikler:**

* Bir birleştirme artık "ANY" nesne türünün MV bağlanabilirsiniz.

## <a name="104850222"></a>1.0.485.0222
Yayımlanma tarihi: Şubat 2015

**İyileştirmeler:**

* Geliştirilmiş alma performans.

**Giderilen sorunlar:**

* Parola Eşitleme, öznitelik filtreleme tarafından kullanılan cloudFiltered özniteliği geliştirir. Filtrelenmiş nesneler artık parola eşitleme kapsamında değildir.
* Burada birçok etki alanı denetleyicileri topoloji b nadir durumlarda, parola eşitleme çalışmıyor.
* "Durduruldu-server" cihaz Yönetimi sonra Azure AD Bağlayıcısı'ndan içeri aktarırken, Azure AD/Intune'da etkinleştirildi.
* Yabancı güvenlik sorumlusu (FSP) birden çok etki aynı ormandaki katılma birleştirme belirsiz hataya neden olur.

## <a name="104751202"></a>1.0.475.1202
Yayımlanma tarihi: Aralık 2014

**Yeni özellikler:**

* Öznitelik tabanlı filtreleme ile parola eşitlemeyi artık desteklenmektedir. Daha fazla bilgi için [filtreleme ile parola eşitlemeyi](how-to-connect-sync-configure-filtering.md).
* Ms-DS-ExternalDirectoryObjectID öznitelik Active Directory'ye geri yazılır. Bu özellik, Office 365 uygulamaları için destek ekler. OAuth2, çevrimiçi ve şirket içi Exchange karma dağıtımı kutularına erişmek için kullanır.

**Yükseltme sorunlar düzeltildi:**

* Sunucuda oturum açma Yardımcısı'nın daha yeni bir sürümü kullanılabilir.
* Özel bir yükleme yolu, Azure AD eşitleme yüklemek için kullanıldı.
* Geçersiz özel birleştirme ölçütü yükseltme engeller.

**Diğer düzeltmeler:**

* Şablon, Office Pro Plus için düzeltildi.
* Sabit yükleme sorunları nedeniyle kısa çizgi ile başlayan kullanıcı adları.
* Yükleme Sihirbazı'nı ikinci kez çalıştırırken sourceAnchor ayarı kaybetme düzeltildi.
* Parola Eşitleme için sabit ETW İzleme.

## <a name="104701023"></a>1.0.470.1023
Yayımlanma tarihi: Ekim 2014

**Yeni özellikler:**

* Birden çok parola eşitleme için Azure AD Active Directory şirket içi.
* Tüm Windows Server diller için yerelleştirilmiş yükleme kullanıcı Arabirimi.

**Aadconnect'in 1.0 GA ' yükseltme**

Azure AD eşitleme'nın yüklü zaten varsa, kullanıma hazır eşitleme kurallardan herhangi birinin değişmesi durumunda yapmanız gereken ek bir adım yoktur. İçin 1.0.470.1023 yükselttikten sonra değiştirilmiş kuralları yinelenen eşitleme bırakın. Her değiştirilen eşitleme kuralı için aşağıdakileri yapın:

1. Eşitleme kuralı değiştirdiniz ve değişiklikleri Not bulun.
1. Eşitleme kuralını silin.
1. Azure AD Sync tarafından oluşturulan yeni bir eşitleme kuralı bulun ve ardından değişiklikleri yeniden uygulayın.

**Active Directory hesabı için izinler**

Active Directory hesabının Active Directory'den parola karmalarının okuyabilir için ek izinler verilmesi gerekir. İzinleri "Dizin Değişikliklerini Çoğaltma" olarak adlandırılır ve "Tüm çoğaltma dizinini değiştirir." Parola karmalarının okumak için her iki izinleri gerekir.

## <a name="104190911"></a>1.0.419.0911
Yayımlanma tarihi: Eylül 2014

**Azure AD eşitleme'nin ilk sürümünden.**

## <a name="next-steps"></a>Sonraki adımlar
[Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md) hakkında daha fazla bilgi edinin.
