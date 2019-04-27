---
title: Kimlik doğrulama - Microsoft tehdit modelleme aracı - Azure | Microsoft Docs
description: Tehdit modelleme Aracı kullanıma sunulan tehdit azaltma
services: security
documentationcenter: na
author: jegeib
manager: jegeib
editor: jegeib
ms.assetid: na
ms.service: security
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/07/2017
ms.author: jegeib
ms.openlocfilehash: 56620dc1d3e315caa3e259715ed84a539b91356d
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60610873"
---
# <a name="security-frame-authentication--mitigations"></a>Güvenlik çerçevesi: Kimlik doğrulaması | Risk azaltma işlemleri 

| Ürün/hizmet | Makale |
| --------------- | ------- |
| **Web uygulaması**    | <ul><li>[Web uygulaması için kimlik doğrulaması için bir standart kimlik doğrulama mekanizması kullanmayı düşünün](#standard-authn-web-app)</li><li>[Uygulamalar başarısız kimlik doğrulama senaryoları güvenli bir şekilde işlemesi gerekir](#handle-failed-authn)</li><li>[Etkinleştirme adım veya uyarlamalı kimlik doğrulaması](#step-up-adaptive-authn)</li><li>[Yönetim arabirimleri uygun şekilde kilitlendiğini emin olun.](#admin-interface-lockdown)</li><li>[Uygulama parolası işlevlerini güvenli bir şekilde unuttum](#forgot-pword-fxn)</li><li>[Parola ve hesap ilkesi uygulandığından emin olun.](#pword-account-policy)</li><li>[Kullanıcı adı numaralandırma önlemek için denetimleri uygulayın](#controls-username-enum)</li></ul> |
| **Veritabanı** | <ul><li>[Mümkün olduğunda, SQL Server'a bağlanmak için Windows kimlik doğrulaması kullanın](#win-authn-sql)</li><li>[Mümkün olduğunda, Azure Active Directory kimlik doğrulaması için SQL veritabanına bağlanma kullanın](#aad-authn-sql)</li><li>[SQL kimlik doğrulama modu kullanılırken, hesap ve parola ilkesi SQL Server'da zorlandığında emin olun](#authn-account-pword)</li><li>[Kapsanan veritabanlarında SQL kimlik doğrulaması kullanmayın](#autn-contained-db)</li></ul> |
| **Azure Event Hub** | <ul><li>[SaS belirteçleri kullanarak cihaz kimlik doğrulama bilgilerini kullanın](#authn-sas-tokens)</li></ul> |
| **Azure güven sınırı** | <ul><li>[Azure yöneticileri için Azure multi-Factor Authentication'ı etkinleştir](#multi-factor-azure-admin)</li></ul> |
| **Service Fabric güven sınırı** | <ul><li>[Service Fabric kümesi için anonim erişimi kısıtlama](#anon-access-cluster)</li><li>[Service Fabric düğümü için istemci sertifikası düğümden düğüme sertifikasından farklı olduğundan emin olun](#fabric-cn-nn)</li><li>[Service fabric kümeleri istemcilerin kimliğini doğrulamak için AAD kullanın](#aad-client-fabric)</li><li>[Service fabric sertifikaları bir onaylanan sertifika yetkilisi (CA) aldığınız emin olun.](#fabric-cert-ca)</li></ul> |
| **Kimlik sunucusu** | <ul><li>[Kimlik sunucusu tarafından desteklenen standart kimlik doğrulama senaryoları kullanın](#standard-authn-id)</li><li>[Varsayılan kimlik sunucusu belirteç önbelleği ile ölçeklenebilir alternatif bir geçersiz kılma](#override-token)</li></ul> |
| **Makine güven sınırı** | <ul><li>[Dağıtılan uygulamanın ikili dosyaları dijital olarak imzalandığından emin olun](#binaries-signed)</li></ul> |
| **WCF** | <ul><li>[Wcf'de kuyruklar MSMQ bağlanırken kimlik doğrulamasını etkinleştirme](#msmq-queues)</li><li>[WCF iş ileti clientCredentialType hiçbiri ayarlanmadı](#message-none)</li><li>[WCF iş aktarım clientCredentialType hiçbiri ayarlanmadı](#transport-none)</li></ul> |
| **Web API** | <ul><li>[Web API'leri güvenli hale getirmek için kullanılan teknikleri standart söz konusu kimlik doğrulamasını emin olun](#authn-secure-api)</li></ul> |
| **Azure AD** | <ul><li>[Azure Active Directory tarafından desteklenen standart kimlik doğrulama senaryoları kullanın](#authn-aad)</li><li>[Varsayılan ADAL belirteç önbelleğini ölçeklenebilir bir alternatif ile geçersiz kıl](#adal-scalable)</li><li>[TokenReplayCache yeniden yürütme ADAL kimlik doğrulaması belirteçlerinin önlemek için kullanıldığından emin olun](#tokenreplaycache-adal)</li><li>[Belirteç isteklerini AAD'ye OAuth2 istemcileri yönetmek için ADAL kitaplıklarını kullanın (veya şirket içi AD)](#adal-oauth2)</li></ul> |
| **IOT alan ağ geçidi** | <ul><li>[Alan ağ geçidi için bağlanan cihazların kimliğini doğrulama](#authn-devices-field)</li></ul> |
| **IOT bulut ağ geçidi** | <ul><li>[Bulut ağ geçidine bağlı cihazların kimlik doğrulaması emin olun.](#authn-devices-cloud)</li><li>[Cihaz başına kimlik doğrulaması kimlik bilgilerini kullan](#authn-cred)</li></ul> |
| **Azure Depolama** | <ul><li>[Yalnızca gerekli kapsayıcılara ve blob'lara anonim okuma erişimini verildiğinden emin olun](#req-containers-anon)</li><li>[SAS veya SAP kullanarak Azure Depolama'daki nesnelere sınırlı erişim](#limited-access-sas)</li></ul> |

## <a id="standard-authn-web-app"></a>Web uygulaması için kimlik doğrulaması için bir standart kimlik doğrulama mekanizması kullanmayı düşünün

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | Yok  |
| Ayrıntılar | <p>Kimlik doğrulaması nerede varlığın kimliğini, genellikle bir kullanıcı adı ve parola gibi kimlik bilgileri aracılığıyla kanıtlar işlemidir. Düşünülebilecek birden çok kimlik doğrulama protokolleri kullanılabilir vardır. Bunlardan bazıları aşağıda listelenmiştir:</p><ul><li>İstemci sertifikaları</li><li>Windows tabanlı</li><li>Form tabanlı</li><li>Federasyon - ADFS</li><li>Federasyon - Azure AD</li><li>Federasyon - kimlik sunucusu</li></ul><p>Kaynak işlemi tanımlamak için bir standart kimlik doğrulama mekanizması kullanmayı düşünün</p>|

## <a id="handle-failed-authn"></a>Uygulamalar başarısız kimlik doğrulama senaryoları güvenli bir şekilde işlemesi gerekir

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | Yok  |
| Ayrıntılar | <p>Açıkça kullanıcıların kimlik doğrulaması yapan uygulamalar başarısız kimlik doğrulama senaryoları güvenli bir şekilde işlemesi gerekir. Kimlik doğrulama mekanizması gerekir:</p><ul><li>Kimlik doğrulama başarısız olduğunda ayrıcalıklı kaynaklara erişimi reddetme</li><li>Başarısız kimlik doğrulamasından sonra genel bir hata iletisi görüntüler ve erişim reddedildi gerçekleşir</li></ul><p>Test için:</p><ul><li>Başarısız oturum açma denemelerinden sonra ayrıcalıklı kaynakları koruma</li><li>Başarısız kimlik doğrulaması genel bir hata iletisi görüntülenir ve olay erişim reddedildi</li><li>Hesapları, aşırı sayıda başarısız girişimden sonra devre dışı bırakılır</li><ul>|

## <a id="step-up-adaptive-authn"></a>Etkinleştirme adım veya uyarlamalı kimlik doğrulaması

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | Yok  |
| Ayrıntılar | <p>Uygulamanın sahip ek yetkilendirme (adım yukarı gibi ya da OTP SMS, e-posta vb. veya yeniden kimlik doğrulaması için isteyen gönderme gibi çok faktörlü kimlik doğrulaması aracılığıyla Uyarlamalı kimlik doğrulama) doğrulayın kullanıcı erişimi verilmeden önce kimlik doğrulaması için hassas bilgileri. Bu kural için bir hesap veya eylem önemli değişiklikler yapmak için de geçerlidir</p><p>Bu da kimlik doğrulama uyarlama uygulama örneğinde, yetkisiz işleme yoluyla izin vermemek için bağlama duyarlı yetkilendirme doğru zorlar şekilde uygulanması gerektiği anlamına gelir parametresi bozma</p>|

## <a id="admin-interface-lockdown"></a>Yönetim arabirimleri uygun şekilde kilitlendiğini emin olun.

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | Yok  |
| Ayrıntılar | İlk çözümü, bir yönetim arabirimi yalnızca bir belirli kaynak IP aralığından erişim sağlamaktır. Bu çözüm, her zaman daha mümkün olmayacak, uygulamak Yönetim arabirimine oturum açmak için doğrulamayı yükseltme veya uyumlu bir kimlik doğrulaması için önerilen |

## <a id="forgot-pword-fxn"></a>Uygulama parolası işlevlerini güvenli bir şekilde unuttum

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | Yok  |
| Ayrıntılar | <p>İlk şey, doğrulamak için parolanızı mı unuttunuz ve diğer kurtarma yolları parola yerine bir süre sınırlı etkinleştirme belirteci dahil olmak üzere bağlantısı Gönder ' dir. Bağlantı üzerinden gönderilmeden önce ek kimlik doğrulama geçici belirteçleri üzerinde (örn. SMS belirteci, yerel mobil uygulamalar, vb.) göre de gerekli olabilir. Yeni bir parola alma işlemi devam ediyor artırabileceksiniz ikinci, kullanıcı hesabının kilitleneceği değil.</p><p>Bu durum, otomatik bir saldırı kullanıcılarla kullanıma kasıtlı olarak kilitlemek bir saldırgan karar her bir hizmet reddi saldırısı için neden olabilir. Yeni parola isteği devam eden ayarlanmış olduğunda, üçüncü görüntü ileti kullanıcıadı numaralandırma engellemek için genelleştirilerek. Dördüncü, her zaman eski parolaların kullanımına izin verme ve güçlü bir parola ilkesi uygulayın.</p> |

## <a id="pword-account-policy"></a>Parola ve hesap ilkesi uygulandığından emin olun.

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | Yok  |
| Ayrıntılar | <p>Parola ve hesap ilkeyle uyumlu kuruluş ilkesi ve en iyi uygulamaları uygulanmalıdır.</p><p>Deneme yanılma ve temel sözlük tahmin karşı korumak için: Kullanıcıların karmaşık bir parola (örneğin, en az 12 karakter uzunluğu, alfasayısal ve özel karakterler) oluşturduğunuzdan emin olmak için güçlü bir parola ilkesi uygulanmalıdır.</p><p>Hesap kilitleme ilkeleri şu şekilde uygulanabilir:</p><ul><li>**Yazılım kilidi çıkışı:** Bu, kullanıcılarınızın deneme yanılma saldırılarına karşı korumaya yönelik iyi bir seçenek olabilir. Yanlış bir parola kullanıcının girdiği her Örneğin, üç kez uygulamanın hesabı için bir dakika kaba devam etmek saldırganın az karlı yaparak kendi parola zorlama işlemi yavaşlatmaz için kilitleme. elde bu örneğin Sabit kilitleme karşı önlemler uygulamak için olsaydı "kalıcı olarak hesapları kilitleme tarafından Dos" a. Alternatif olarak, uygulama bir OTP (bir saat parola) oluştur ve bant dışı Gönder (e-posta, sms vb.) kullanıcı. Başarısız girişim eşiği sayısına ulaşıldıktan sonra CAPTCHA uygulamak için başka bir yaklaşım olabilir.</li><li>**Sabit kilit çıkışı:** Her bir kullanıcı uygulama ve sayaç ona yanıtı ekibi, adli araştırma yapmak için bir süre beklendiğinden kadar kalıcı olarak kendi hesabını kilitlemek yoluyla saldırmak algılamak, bu tür bir kilitleme uygulanmalıdır. Bu işleminden sonra kullanıcıya vermek karar verebilirsiniz hesabını yedekleme veya daha fazla kendisine karşı yasal eylemleri gerçekleştirin. Bu tür bir yaklaşım, uygulamanızın ve altyapınızın daha fazla penetrating gelen saldırgan engeller.</li></ul><p>Varsayılan ve tahmin edilebilir hesapları saldırılarına karşı korumak için tüm anahtarları ve parolaları değiştirilebilir ve olan oluşturulan veya yükleme süre sonra değiştirilmesi doğrulayın.</p><p>Uygulama parolaları otomatik olarak oluşturmak varsa, oluşturulan parolalar rastgele ve yüksek entropi sahip olun.</p>|

## <a id="controls-username-enum"></a>Kullanıcı adı numaralandırma önlemek için denetimleri uygulayın

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | Kullanıcı adı numaralandırma önlemek için tüm hata iletilerini genelleştirilmiş. Ayrıca bazen bir kayıt sayfasına gibi işlevler, sızıntı bilgilerden kaçınmak olamaz. Burada bir saldırgan tarafından otomatik bir saldırıyı engellemek için CAPTCHA gibi hız sınırlaması yöntemleri kullanmanız gerekir. |

## <a id="win-authn-sql"></a>Mümkün olduğunda, SQL Server'a bağlanmak için Windows kimlik doğrulaması kullanın

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Database | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Şirket içi |
| **Öznitelikler**              | SQL sürümü - tüm |
| **Başvuruları**              | [SQL Server - kimlik doğrulama modu seçme](https://msdn.microsoft.com/library/ms144284.aspx) |
| **Adımları** | Windows kimlik doğrulaması Kerberos güvenlik protokolünü kullanır, parola ilkesi zorlama için güçlü parolalar karmaşıklık doğrulaması onaylamaz sağlar, hesap kilitleme desteği sağlar ve parola süre aşımını destekler.|

## <a id="aad-authn-sql"></a>Mümkün olduğunda, Azure Active Directory kimlik doğrulaması için SQL veritabanına bağlanma kullanın

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Database | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | SQL Azure |
| **Öznitelikler**              | SQL sürümü - V12 |
| **Başvuruları**              | [Azure Active Directory kimlik doğrulamasını kullanarak SQL veritabanına bağlanma](https://azure.microsoft.com/documentation/articles/sql-database-aad-authentication/) |
| **Adımları** | **En düşük sürümü:** Azure SQL veritabanı V12 Azure SQL veritabanı, Microsoft Directory AAD kimlik doğrulamasını kullanmak izin vermek için gerekli |

## <a id="authn-account-pword"></a>SQL kimlik doğrulama modu kullanılırken, hesap ve parola ilkesi SQL Server'da zorlandığında emin olun

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Database | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | [SQL Server Parola İlkesi](https://technet.microsoft.com/library/ms161959(v=sql.110).aspx) |
| **Adımları** | SQL Server kimlik doğrulamasını kullanırken oturum açma bilgileri Windows kullanıcı hesaplarına bağlı olmayan bir SQL Server'da oluşturulur. Hem kullanıcı adı ve parola SQL Server'ı kullanarak oluşturulur ve SQL Server'da depolanan. SQL Server Windows parola ilkesi mekanizmalar kullanabilirsiniz. Aynı karmaşıklığı ve SQL Server içinde kullanılan parolaları Windows içinde kullanılan süre sonu ilkeleri uygulayabilirsiniz. |

## <a id="autn-contained-db"></a>Kapsanan veritabanlarında SQL kimlik doğrulaması kullanmayın

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Database | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Şirket içi, SQL Azure |
| **Öznitelikler**              | SQL - MSSQL2012, SQL sürüm - sürüm V12 |
| **Başvuruları**              | [Kapsanan veritabanları ile en iyi güvenlik uygulamaları](https://msdn.microsoft.com/library/ff929055.aspx) |
| **Adımları** | Bir uygulanan parola ilkesine olmaması kapsanan bir veritabanında kurulmasını zayıf bir kimlik bilgisi olasılığını artırabilir. Windows kimlik doğrulaması yararlanın. |

## <a id="authn-sas-tokens"></a>SaS belirteçleri kullanarak cihaz kimlik doğrulama bilgilerini kullanın

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Azure Olay Hub'ı | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | [Event Hubs kimlik doğrulama ve güvenlik modeline genel bakış](https://azure.microsoft.com/documentation/articles/event-hubs-authentication-and-security-model-overview/) |
| **Adımları** | <p>Event Hubs güvenlik modeli, olay yayımcıları ve paylaşılan erişim imzası (SAS) belirteç üzerindeki bir birleşimini temel alır. Yayımcı adı belirteci alan DeviceID temsil eder. Bu, ilgili cihazları ile oluşturulan belirteçleri ilişkilendirmek yardımcı olacaktır.</p><p>Tüm iletileri gönderen algılama denemeleri yanıltma yükü kaynak sağlayan hizmet tarafındaki ile etiketlenir. Cihaz kimlik doğrulamasını yaparken, oluşturun bir başına benzersiz bir yayımcı için kapsamlı cihaz SaS belirteci.</p>|

## <a id="multi-factor-azure-admin"></a>Azure yöneticileri için Azure multi-Factor Authentication'ı etkinleştir

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Azure güven sınırı | 
| **SDL aşaması**               | Dağıtım |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | [Azure Multi-Factor Authentication nedir?](https://azure.microsoft.com/documentation/articles/multi-factor-authentication/) |
| **Adımları** | <p>Çok faktörlü kimlik doğrulaması (MFA), birden fazla doğrulama yöntemi gerektiren ve kullanıcı oturum açmalarına ve işlemlerine önemli bir ikinci güvenlik katmanı ekler kimlik doğrulama yöntemidir. Aşağıdaki doğrulama yöntemlerinden herhangi ikisini veya isteyerek çalışır:</p><ul><li>Bir şey (genellikle parola) bildirin</li><li>(Kolayca, bir telefon gibi kopyalaması değil güvenilir bir cihaz) sahip olduğunuz şey</li><li>Bir şey (Biyometri) olan</li><ul>|

## <a id="anon-access-cluster"></a>Service Fabric kümesi için anonim erişimi kısıtlama

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Service Fabric güven sınırı | 
| **SDL aşaması**               | Dağıtım |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Ortam - Azure  |
| **Başvuruları**              | [Service Fabric kümesi güvenlik senaryoları](https://azure.microsoft.com/documentation/articles/service-fabric-cluster-security) |
| **Adımları** | <p>Özellikle de üretim iş yükleri üzerinde çalıştırılan olduğunda, kümeye bağlanma yetkisiz kullanıcıların önlemek için kümeleri her zaman güvenli hale getirilmelidir.</p><p>Service fabric kümesi oluştururken, güvenlik modu "güvenli" ve gerekli X.509 sertifikası yapılandırmak için ayarlandığından emin olun. "Güvenli" bir küme oluşturma, genel İnternet'e yönetim uç noktalarını sunarsa bağlanmak herhangi bir anonim kullanıcı izin verir.</p>|

## <a id="fabric-cn-nn"></a>Service Fabric düğümü için istemci sertifikası düğümden düğüme sertifikasından farklı olduğundan emin olun

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Service Fabric güven sınırı | 
| **SDL aşaması**               | Dağıtım |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Ortam - Azure, ortam - tek başına |
| **Başvuruları**              | [Service Fabric düğümü için istemci sertifikası güvenliği](https://azure.microsoft.com/documentation/articles/service-fabric-cluster-security/#_client-to-node-certificate-security), [istemci sertifikası ile güvenli bir kümeye bağlanma](https://azure.microsoft.com/documentation/articles/service-fabric-connect-to-secure-cluster/) |
| **Adımları** | <p>İstemci düğümü sertifika güvenliği, kümeyi oluştururken ya da Resource Manager şablonları ya da bir yönetici istemci sertifikası ve/veya bir kullanıcı istemci sertifikası belirterek tek başına JSON şablonu Azure Portalı aracılığıyla yapılandırılır.</p><p>Belirttiğiniz yönetici istemci ve kullanıcı istemci sertifikaları, düğümden düğüme güvenlik için belirttiğiniz birincil ve ikincil sertifikaları farklı olmalıdır.</p>|

## <a id="aad-client-fabric"></a>Service fabric kümeleri istemcilerin kimliğini doğrulamak için AAD kullanın

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Service Fabric güven sınırı | 
| **SDL aşaması**               | Dağıtım |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Ortam - Azure |
| **Başvuruları**              | [Küme güvenliği senaryoları - güvenlik önerileri](https://azure.microsoft.com/documentation/articles/service-fabric-cluster-security/#security-recommendations) |
| **Adımları** | Azure üzerinde çalışan kümelerle ayrıca Azure Active Directory (AAD), istemci sertifikaları dışında kullanarak yönetim uç noktalarına erişimi güvenliğini sağlayabilirsiniz. Azure kümeleri için istemciler ve düğümden düğüme güvenliği için sertifika kimlik doğrulaması için AAD güvenlik kullanmanız önerilir.|

## <a id="fabric-cert-ca"></a>Service fabric sertifikaları bir onaylanan sertifika yetkilisi (CA) aldığınız emin olun.

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Service Fabric güven sınırı | 
| **SDL aşaması**               | Dağıtım |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Ortam - Azure |
| **Başvuruları**              | [X.509 sertifikaları ve Service Fabric](https://azure.microsoft.com/documentation/articles/service-fabric-cluster-security/#x509-certificates-and-service-fabric) |
| **Adımları** | <p>Service Fabric, kimlik doğrulama, düğümlerin ve istemciler için X.509 sertifikaları kullanır.</p><p>Hizmet dokularda sertifikaları kullanırken dikkate alınması gereken bazı önemli noktalar:</p><ul><li>Üretim iş yükleri çalıştıran kümelerde kullanılan sertifikaları doğru şekilde yapılandırılmış bir Windows Server sertifika hizmeti kullanılarak oluşturulan veya edinilen bir onaylanan sertifika yetkilisi (CA). CA, onaylanan bir dış CA veya bir düzgün bir şekilde yönetilen iç ortak anahtar altyapısı (PKI) olabilir</li><li>Hiçbir zaman herhangi bir geçici kullanın veya sertifikaları MakeCert.exe gibi araçlarla oluşturulan üretimde test</li><li>Kendinden imzalı bir sertifika kullanabilirsiniz, ancak yalnızca test kümeleri için alan ve üretim bunu yapmasını</li></ul>|

## <a id="standard-authn-id"></a>Kimlik sunucusu tarafından desteklenen standart kimlik doğrulama senaryoları kullanın

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Kimlik sunucusu | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | [IdentityServer3 - büyük resmi](https://identityserver.github.io/Documentation/docsv2/overview/bigPicture.html) |
| **Adımları** | <p>Kimlik sunucusu tarafından desteklenen tipik etkileşimler aşağıdadır:</p><ul><li>Tarayıcıları ile web uygulamaları ile iletişim kurar.</li><li>Web uygulamaları, web API'leri (bazen kendi, bazen bir kullanıcı adına) ile iletişim</li><li>Tarayıcı tabanlı uygulamalar, web API'leri ile iletişim kurma</li><li>Yerel uygulamalar, web API'leri ile iletişim</li><li>Sunucu tabanlı uygulamalar, web API'leri ile iletişim kurar</li><li>Web API'leri, web API'leri (bazen kendi, bazen bir kullanıcı adına) ile iletişim</li></ul>|

## <a id="override-token"></a>Varsayılan kimlik sunucusu belirteç önbelleği ile ölçeklenebilir alternatif bir geçersiz kılma

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Kimlik sunucusu | 
| **SDL aşaması**               | Dağıtım |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | [Kimlik sunucusu dağıtımı - önbelleğe alma](https://identityserver.github.io/Documentation/docsv2/advanced/deployment.html) |
| **Adımları** | <p>Basit, yerleşik bir bellek içi önbelleği IdentityServer sahiptir. Bu küçük ölçekli yerel uygulamalar için iyi olsa da, aşağıdaki nedenlerden dolayı orta katman ve arka uç uygulamaları için ölçeklenmez:</p><ul><li>Bu uygulamalar, aynı anda birden çok kullanıcı tarafından erişilir. Aynı depodaki tüm erişim belirteçlerini kaydetme yalıtım sorunları oluşturur ve uygun ölçekte çalışırken zorluklar teşkil etmektedir: her uygulamanın kendi adınıza eriştiği kaynaklar gibi çok belirteç içeren fazla kullanıcı, çok büyük sayılar ve çok pahalı arama işlemleri gelebilir.</li><li>Bu uygulamalar genellikle birden çok düğümün aynı önbelleğe erişimi burada olmalıdır, dağıtılmış topolojileri dağıtılmaktadır</li><li>Önbelleğe alınan belirteçleri, işlem geri dönüştürülmeden ve deactivations varlığını sürdürmesini gerekir</li><li>Tüm yukarıdaki nedeniyle, web uygulamaları, uygulama çalışırken, Redis için varsayılan kimlik sunucunun belirteç önbelleği gibi Azure Cache ölçeklenebilir bir alternatif ile geçersiz kılmak için önerilir</li></ul>|

## <a id="binaries-signed"></a>Dağıtılan uygulamanın ikili dosyaları dijital olarak imzalandığından emin olun

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Makine güven sınırı | 
| **SDL aşaması**               | Dağıtım |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | İkili dosyaların bütünlüğünü doğrulayabilirsiniz, dağıtılan uygulamanın ikili dosyaları dijital olarak imzalandığından emin olun|

## <a id="msmq-queues"></a>Wcf'de kuyruklar MSMQ bağlanırken kimlik doğrulamasını etkinleştirme

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | WCF | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel, NET Framework 3 |
| **Öznitelikler**              | Yok |
| **Başvuruları**              | [MSDN](https://msdn.microsoft.com/library/ff648500.aspx) |
| **Adımları** | MSMQ sıralara bağlanırken kimlik doğrulamasını etkinleştirmek program başarısız olursa, bir saldırgan, anonim olarak işleme için kuyruğa gönderebilirsiniz. Başka bir programa bir iletiyi teslim etmek için kullanılan bir MSMQ sırası bağlanmak için kimlik doğrulaması kullanılmıyorsa, bir saldırgan kötü amaçlı anonim bir ileti göndermek.|

### <a name="example"></a>Örnek
`<netMsmqBinding/>` WCF yapılandırma dosyasına aşağıdaki öğesinin ileti teslimi bir MSMQ sırası bağlanırken kimlik doğrulama devre dışı bırakmak için WCF bildirir.
```
<bindings>
    <netMsmqBinding>
        <binding>
            <security>
                <transport msmqAuthenticationMode=""None"" />
            </security>
        </binding>
    </netMsmqBinding>
</bindings>
```
MSMQ her zaman Windows etki alanı veya sertifika kimlik doğrulaması için gelen veya giden iletileri gerektirecek şekilde yapılandırın.

### <a name="example"></a>Örnek
`<netMsmqBinding/>` WCF yapılandırma dosyasına aşağıdaki öğesi için bir MSMQ sırası bağlanırken sertifika kimlik doğrulamasını etkinleştirmek için WCF bildirir. İstemci, X.509 sertifikaları kullanarak kimlik doğrulaması yapılır. İstemci sertifikası sertifika deposunda bulunması gerekir.
```
<bindings>
    <netMsmqBinding>
        <binding>
            <security>
                <transport msmqAuthenticationMode=""Certificate"" />
            </security>
        </binding>
    </netMsmqBinding>
</bindings>
```

## <a id="message-none"></a>WCF iş ileti clientCredentialType hiçbiri ayarlanmadı

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | WCF | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | .NET Framework 3 |
| **Öznitelikler**              | İstemci kimlik bilgisi türü - yok |
| **Başvuruları**              | [MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [Fortify](https://vulncat.fortify.com/en/detail?id=desc.semantic.dotnet.wcf_misconfiguration_anonymous_message_client) |
| **Adımları** | Kimlik doğrulaması olmaması, herkesin bu hizmete erişebilmesi için olduğu anlamına gelir. İstemcilerinden kimlik doğrulamasını yapmaz bir hizmet, tüm kullanıcılara erişim izni verir. İstemci kimlik bilgilerini doğrulamak için uygulamayı yapılandırma. Bu, Windows ya da sertifika ileti clientCredentialType ayarlayarak yapılabilir. |

### <a name="example"></a>Örnek
```
<message clientCredentialType=""Certificate""/>
```

## <a id="transport-none"></a>WCF iş aktarım clientCredentialType hiçbiri ayarlanmadı

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | WCF | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Generic, .NET Framework 3 |
| **Öznitelikler**              | İstemci kimlik bilgisi türü - yok |
| **Başvuruları**              | [MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [Fortify](https://vulncat.fortify.com/en/detail?id=desc.semantic.dotnet.wcf_misconfiguration_anonymous_transport_client) |
| **Adımları** | Kimlik doğrulaması olmaması, herkesin bu hizmete erişebilmesi için olduğu anlamına gelir. İstemcilerinden kimlik doğrulamasını yapmaz bir hizmetin, tüm kullanıcıların işlevselliğini erişmesine izin verir. İstemci kimlik bilgilerini doğrulamak için uygulamayı yapılandırma. Bu, Windows ya da sertifika aktarım clientCredentialType ayarlayarak yapılabilir. |

### <a name="example"></a>Örnek
```
<transport clientCredentialType=""Certificate""/>
```

## <a id="authn-secure-api"></a>Web API'leri güvenli hale getirmek için kullanılan teknikleri standart söz konusu kimlik doğrulamasını emin olun

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web API | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | [Kimlik doğrulama ve yetkilendirme ASP.NET Web API'de](https://www.asp.net/web-api/overview/security/authentication-and-authorization-in-aspnet-web-api), [ASP.NET Web API ile dış kimlik doğrulama hizmeti (C#)](https://www.asp.net/web-api/overview/security/external-authentication-services) |
| **Adımları** | <p>Kimlik doğrulaması nerede varlığın kimliğini, genellikle bir kullanıcı adı ve parola gibi kimlik bilgileri aracılığıyla kanıtlar işlemidir. Düşünülebilecek birden çok kimlik doğrulama protokolleri kullanılabilir vardır. Bunlardan bazıları aşağıda listelenmiştir:</p><ul><li>İstemci sertifikaları</li><li>Windows tabanlı</li><li>Form tabanlı</li><li>Federasyon - ADFS</li><li>Federasyon - Azure AD</li><li>Federasyon - kimlik sunucusu</li></ul><p>Başvuruları bölümündeki bağlantıları nasıl her kimlik doğrulama düzeni, bir Web API'SİNİN güvenliğini sağlamak için uygulanabilir üzerinde alt düzey ayrıntıları sağlayın.</p>|

## <a id="authn-aad"></a>Azure Active Directory tarafından desteklenen standart kimlik doğrulama senaryoları kullanın

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Azure AD | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | [Azure AD için kimlik doğrulama senaryoları](https://azure.microsoft.com/documentation/articles/active-directory-authentication-scenarios/), [Azure Active Directory kod örnekleri](https://azure.microsoft.com/documentation/articles/active-directory-code-samples/), [Azure Active Directory Geliştirici Kılavuzu](https://azure.microsoft.com/documentation/articles/active-directory-developers-guide/) |
| **Adımları** | <p>Azure Active Directory (Azure AD), OAuth 2.0 ve Openıd Connect gibi endüstri standardı protokoller için desteği olan bir hizmet olarak kimlik sağlayarak geliştiriciler için kimlik doğrulaması kolaylaştırır. Aşağıda Azure AD tarafından desteklenen beş birincil uygulama senaryoları şunlardır:</p><ul><li>Web uygulamasına Web tarayıcısı: Bir kullanıcı Azure AD tarafından güvenliği sağlanan bir web uygulamasına oturum açması gerekiyor</li><li>Tek sayfalı uygulama (SPA): Bir kullanıcı Azure AD tarafından güvenliği sağlanan bir tek sayfalı bir uygulamada oturum açması gerekiyor</li><li>Web API'si yerel uygulamaya: Telefon, tablet veya PC çalıştıran yerel bir uygulama bir Web API'si Azure AD tarafından güvenli hale getirilen kaynakları almak için bir kullanıcının kimliğini doğrulaması gerektiğinde</li><li>Web uygulaması Web API'si için: Bir Web API'si Azure AD tarafından güvenliği sağlanan kaynakları almak bir web uygulaması gerekiyor</li><li>Daemon veya sunucu uygulaması Web API'si için: Azure AD tarafından güvenliği sağlanan bir web API'den kaynakları almak bir arka plan programı uygulaması veya web kullanıcı arabirimi ile bir sunucu uygulaması gerekiyor</li></ul><p>Alt düzey uygulama ayrıntıları için başvurular bölümündeki bağlantılara başvurun</p>|

## <a id="adal-scalable"></a>Varsayılan ADAL belirteç önbelleğini ölçeklenebilir bir alternatif ile geçersiz kıl

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Azure AD | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | [Web uygulamaları için Azure Active Directory ile modern kimlik doğrulaması](https://blogs.msdn.microsoft.com/microsoft_press/2016/01/04/new-book-modern-authentication-with-azure-active-directory-for-web-applications/), [ADAL belirteç önbelleği olarak Redis kullanma](https://blogs.msdn.microsoft.com/mrochon/2016/09/19/using-redis-as-adal-token-cache/)  |
| **Adımları** | <p>ADAL (Active Directory Authentication Library) kullanan mevcut işlem genelinde bir statik depolama kullanan bir bellek içi önbellek varsayılan önbellek. Bu yerel uygulamalar için çalışırken, aşağıdaki nedenlerden dolayı orta katman ve arka uç uygulamaları için ölçeklenmez:</p><ul><li>Bu uygulamalar, aynı anda birden çok kullanıcı tarafından erişilir. Aynı depodaki tüm erişim belirteçlerini kaydetme yalıtım sorunları oluşturur ve uygun ölçekte çalışırken zorluklar teşkil etmektedir: her uygulamanın kendi adınıza eriştiği kaynaklar gibi çok belirteç içeren fazla kullanıcı, çok büyük sayılar ve çok pahalı arama işlemleri gelebilir.</li><li>Bu uygulamalar genellikle birden çok düğümün aynı önbelleğe erişimi burada olmalıdır, dağıtılmış topolojileri dağıtılmaktadır</li><li>Önbelleğe alınan belirteçleri, işlem geri dönüştürülmeden ve deactivations varlığını sürdürmesini gerekir</li></ul><p>Tüm yukarıdaki nedeniyle, web uygulamaları, uygulama çalışırken, Redis için varsayılan ADAL belirteç önbelleğini Azure önbellek gibi ölçeklenebilir bir alternatif ile geçersiz kılmak için önerilir.</p>|

## <a id="tokenreplaycache-adal"></a>TokenReplayCache yeniden yürütme ADAL kimlik doğrulaması belirteçlerinin önlemek için kullanıldığından emin olun

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Azure AD | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | [Web uygulamaları için Azure Active Directory ile modern kimlik doğrulaması](https://blogs.msdn.microsoft.com/microsoft_press/2016/01/04/new-book-modern-authentication-with-azure-active-directory-for-web-applications/) |
| **Adımları** | <p>TokenReplayCache özelliği, geliştiricilerin belirteçleri, belirteci olmadan birden çok kez kullanılabileceğini doğrulama amacıyla kaydetmek için kullanılan bir depo bir belirteç yeniden yürütme önbellek tanımlamak olanak tanır.</p><p>Bu yaygın bir saldırı, aptly çağrılan belirteç yeniden yürütme saldırıya karşı ölçümüdür: uygulamaya yeniden göndermek oturum açma işleminde gönderilen belirteç kesintiye neden olan bir saldırganın deneyebilirsiniz ("yeni bir oturum oluşturmak için anlayamazsanız"). Örneğin, kullanıcı başarıyla kimlik doğrulamasından sonra OIDC kodu verme akışı, bir istek "/ signin-oıdc" bağlı olan taraf uç noktası ile "id_token", "code" ve "eyalet" parametreleri yapılır.</p><p>Bağlı olan taraf, bu isteği doğrular ve yeni bir oturum oluşturur. Bir saldırganın bu isteği yakalar ve bunu yeniden yürütür, derse başarılı bir oturumu ve kullanıcı sızmasını. Openıd Connect, nonce varlığını, sınırlamak ancak, saldırı başarıyla yapılandırmalara uygun koşullarda çalışıldığından koşullar tamamen ortadan kaldırın. Geliştiriciler uygulamalarını korumak için ITokenReplayCache bir uygulamasını sağlamak ve bir örneği için TokenReplayCache atayın.</p>|

### <a name="example"></a>Örnek
```csharp
// ITokenReplayCache defined in ADAL
public interface ITokenReplayCache
{
bool TryAdd(string securityToken, DateTime expiresOn);
bool TryFind(string securityToken);
}
```

### <a name="example"></a>Örnek
Örnek uygulama ITokenReplayCache arabiriminin aşağıda verilmiştir. (Lütfen özelleştirme ve önbelleğe alma, projeye özgü bir çerçeve)
```csharp
public class TokenReplayCache : ITokenReplayCache
{
    private readonly ICacheProvider cache; // Your project-specific cache provider
    public TokenReplayCache(ICacheProvider cache)
    {
        this.cache = cache;
    }
    public bool TryAdd(string securityToken, DateTime expiresOn)
    {
        if (this.cache.Get<string>(securityToken) == null)
        {
            this.cache.Set(securityToken, securityToken);
            return true;
        }
        return false;
    }
    public bool TryFind(string securityToken)
    {
        return this.cache.Get<string>(securityToken) != null;
    }
}
```
OIDC seçeneklerinde "Tokenvalidationparameters değerini" özelliği aracılığıyla gibi başvurulabilmesi uygulanan önbellek vardır.
```csharp
OpenIdConnectOptions openIdConnectOptions = new OpenIdConnectOptions
{
    AutomaticAuthenticate = true,
    ... // other configuration properties follow..
    TokenValidationParameters = new TokenValidationParameters
    {
        TokenReplayCache = new TokenReplayCache(/*Inject your cache provider*/);
    }
}
```

Unutmayın, bu yapılandırma, oturum açma uygulamanıza yerel OIDC korumalı etkisini test edin ve isteği yakalamak için `"/signin-oidc"` fiddler'da uç noktası. Koruma yerinde olmadığında, fiddler'ı bu istekte yeniden yürüterek yeni oturum tanımlama bilgisi ayarlar. TokenReplayCache koruma eklendikten sonra isteği yeniden yürütülmesi, uygulama gibi bir özel durum oluşturur: `SecurityTokenReplayDetectedException: IDX10228: The securityToken has previously been validated, securityToken: 'eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNBVGZNNXBPWWlKSE1iYTlnb0VLWSIsImtpZCI6Ik1uQ1......`

## <a id="adal-oauth2"></a>Belirteç isteklerini AAD'ye OAuth2 istemcileri yönetmek için ADAL kitaplıklarını kullanın (veya şirket içi AD)

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Azure AD | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | [ADAL](https://azure.microsoft.com/documentation/articles/active-directory-authentication-libraries/) |
| **Adımları** | <p>Azure AD authentication Library (ADAL) kolayca buluta kullanıcıların kimliğini doğrulamak istemci uygulaması geliştiricileri sağlar veya şirket içi Active Directory (AD) ve ardından API çağrıları güvenliğini sağlamaya yönelik erişim belirteçleri edinin.</p><p>ADAL sahip birçok özellik yapma kimlik doğrulamanın daha kolay erişim belirteci ve yenileme belirteçleri, bir erişim belirtecinin süresi ve yenileme belirteci kullanılabilir olduğunda otomatik belirteç yenileme depolar yapılandırılabilir bir belirteç önbelleği zaman uyumsuz desteği gibi geliştiriciler için ve Daha fazla.</p><p>ADAL karmaşıklığın çoğunu işleyerek, kendi iş mantığı üzerinde bir geliştirici odaklanmasına yardımcı olmak ve güvenlik uzmanı olmadan kaynakları kolayca güvenli. Ayrı kitaplıkları, .NET, JavaScript (istemci ve Node.js), iOS, Android ve Java için kullanılabilir.</p>|

## <a id="authn-devices-field"></a>Alan ağ geçidi için bağlanan cihazların kimliğini doğrulama

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | IOT alan ağ geçidi | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | Her cihaz verileri kabul eden ve bulut ağ geçidi ile Yukarı Akış iletişimleri etkinleştirme önce alan ağ geçidi tarafından doğrulandıktan emin olun. Ayrıca, cihazlar ile bağlanma olun bir cihaz başına tek cihazlara benzersiz şekilde tanımlanabilir böylece kimlik bilgisi.|

## <a id="authn-devices-cloud"></a>Bulut ağ geçidine bağlı cihazların kimlik doğrulaması emin olun.

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | IOT bulut ağ geçidi | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Generic, C#, Node.JS,  |
| **Öznitelikler**              | Yok, ağ geçidi seçim - Azure IOT hub'ı |
| **Başvuruları**              | Yok, [.NET ile Azure IOT hub](https://azure.microsoft.com/documentation/articles/iot-hub-csharp-csharp-getstarted/), [IOT hub ve düğüm JS ile çalışmaya başlama](https://azure.microsoft.com/documentation/articles/iot-hub-node-node-getstarted), [SAS ve sertifikaları güvenli hale getirme IOT](https://azure.microsoft.com/documentation/articles/iot-hub-sas-tokens/), [Git deposu](https://github.com/Azure/azure-iot-sdks/tree/master/node) |
| **Adımları** | <ul><li>**Genel:** Aktarım Katmanı Güvenliği (TLS) veya IPSec kullanarak cihazın kimliği. Altyapı önceden paylaşılan anahtar (PSK) kullanarak tam asimetrik şifreleme işleyemiyor bu cihazlarda desteklemelidir. Azure AD, Oauth özelliğinden yararlanabilirsiniz.</li><li>**C#:** Oluşturma yöntemi, bir DeviceClient örneği oluşturulurken varsayılan olarak, IOT Hub ile iletişim kurmak için AMQP protokolünü kullanan bir DeviceClient örneği oluşturur. HTTPS protokolünü kullanmak için oluşturma yönteminin Protokolü belirtmenize olanak tanıyan geçersiz kılmasını kullanın. De eklemeniz gerekir, HTTPS protokolünü kullanıyorsanız `Microsoft.AspNet.WebApi.Client` NuGet paketini projenize eklenecek `System.Net.Http.Formatting` ad alanı.</li></ul>|

### <a name="example"></a>Örnek
```csharp
static DeviceClient deviceClient;

static string deviceKey = "{device key}";
static string iotHubUri = "{iot hub hostname}";

var messageString = "{message in string format}";
var message = new Message(Encoding.ASCII.GetBytes(messageString));

deviceClient = DeviceClient.Create(iotHubUri, new DeviceAuthenticationWithRegistrySymmetricKey("myFirstDevice", deviceKey));

await deviceClient.SendEventAsync(message);
```

### <a name="example"></a>Örnek
**Node.JS: Kimlik doğrulaması**
#### <a name="symmetric-key"></a>Simetrik anahtar
* Azure IOT hub oluşturma
* Cihaz kimlik kayıt defterinde girişi oluşturma
    ```javascript
    var device = new iothub.Device(null);
    device.deviceId = <DeviceId >
    registry.create(device, function(err, deviceInfo, res) {})
    ```
* Sanal cihaz oluşturma
    ```javascript
    var clientFromConnectionString = require('azure-iot-device-amqp').clientFromConnectionString;
    var Message = require('azure-iot-device').Message;
    var connectionString = 'HostName=<HostName>DeviceId=<DeviceId>SharedAccessKey=<SharedAccessKey>';
    var client = clientFromConnectionString(connectionString);
    ```
  #### <a name="sas-token"></a>SAS Belirteci
* Simetrik anahtar ancak kullanırken, dahili olarak oluşturulan oluşturabilir ve aynı zamanda açıkça kullanın
* Bir protokol tanımlar: `var Http = require('azure-iot-device-http').Http;`
* Bir sas belirteci oluşturun:
    ```javascript
    resourceUri = encodeURIComponent(resourceUri.toLowerCase()).toLowerCase();
    var deviceName = "<deviceName >";
    var expires = (Date.now() / 1000) + expiresInMins * 60;
    var toSign = resourceUri + '\n' + expires;
    // using crypto
    var decodedPassword = new Buffer(signingKey, 'base64').toString('binary');
    const hmac = crypto.createHmac('sha256', decodedPassword);
    hmac.update(toSign);
    var base64signature = hmac.digest('base64');
    var base64UriEncoded = encodeURIComponent(base64signature);
    // construct authorization string
    var token = "SharedAccessSignature sr=" + resourceUri + "%2fdevices%2f"+deviceName+"&sig="
  + base64UriEncoded + "&se=" + expires;
    if (policyName) token += "&skn="+policyName;
    return token;
    ```
* SAS belirteci kullanarak bağlan: 
    ```javascript
    Client.fromSharedAccessSignature(sas, Http); 
    ```
  #### <a name="certificates"></a>Sertifikalar
* Kendinden imzalı bir X509 oluşturmak sertifika ve anahtar sırasıyla depolamak için bir .cert ve .key dosyaları oluşturmak için OpenSSL gibi herhangi bir aracı kullanarak sertifika
* Sertifikaları kullanarak güvenli bağlantı kabul eden bir cihaz hazırlayın.
    ```javascript
    var connectionString = '<connectionString>';
    var registry = iothub.Registry.fromConnectionString(connectionString);
    var deviceJSON = {deviceId:"<deviceId>",
    authentication: {
        x509Thumbprint: {
        primaryThumbprint: "<PrimaryThumbprint>",
        secondaryThumbprint: "<SecondaryThumbprint>"
        }
    }}
    var device = deviceJSON;
    registry.create(device, function (err) {});
    ```
* Bir sertifika kullanarak bir cihazı bağlayın
    ```javascript
    var Protocol = require('azure-iot-device-http').Http;
    var Client = require('azure-iot-device').Client;
    var connectionString = 'HostName=<HostName>DeviceId=<DeviceId>x509=true';
    var client = Client.fromConnectionString(connectionString, Protocol);
    var options = {
        key: fs.readFileSync('./key.pem', 'utf8'),
        cert: fs.readFileSync('./server.crt', 'utf8')
    }; 
    // Calling setOptions with the x509 certificate and key (and optionally, passphrase) will configure the client //transport to use x509 when connecting to IoT Hub
    client.setOptions(options);
    //call fn to execute after the connection is set up
    client.open(fn);
    ```

## <a id="authn-cred"></a>Cihaz başına kimlik doğrulaması kimlik bilgilerini kullan

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | IOT bulut ağ geçidi  | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Ağ geçidi seçim - Azure IOT hub'ı |
| **Başvuruları**              | [Azure IOT Hub güvenlik belirteçleri](https://azure.microsoft.com/documentation/articles/iot-hub-sas-tokens/) |
| **Adımları** | Kullanımı cihaz kimlik doğrulaması kimlik bilgilerini kullanarak SaS belirteçlerini başına cihaz anahtarı veya istemci sertifikasını göre IOT hub'ı düzeyinde yerine paylaşılan erişim ilkeleri. Bu bir başkası tarafından bir cihaz veya alan ağ geçidi kimlik doğrulama belirteçlerinizi kullanılmasını engeller |

## <a id="req-containers-anon"></a>Yalnızca gerekli kapsayıcılara ve blob'lara anonim okuma erişimini verildiğinden emin olun

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Azure Storage | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | StorageType - Blob |
| **Başvuruları**              | [Kapsayıcılar ve bloblar için anonim okuma erişimini yönetme](https://azure.microsoft.com/documentation/articles/storage-manage-access-to-resources/), [paylaşılan erişim imzaları, 1. Bölüm: SAS modelini anlama](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/) |
| **Adımları** | <p>Varsayılan olarak, bir kapsayıcı ve içerdiği tüm blobları yalnızca depolama hesabı sahibi tarafından erişilebilir. Anonim kullanıcılar için bir kapsayıcı ve bloblarını Okuma izinleri vermek için genel erişime izin vermek için kapsayıcı izinlerini ayarlayabilirsiniz. Anonim kullanıcılar, istek kimliği doğrulanmadan ortak olarak erişilebilen bir kapsayıcı içindeki blobları okuyabilir.</p><p>Kapsayıcılar, kapsayıcı erişimini yönetmek için aşağıdaki seçenekleri sağlar:</p><ul><li>Tam genel okuma erişimi: Kapsayıcı ve blob verilerini anonim istek okunabilir. İstemcilerin anonim isteğiyle kapsayıcı içindeki blobları listeleme, ancak depolama hesabında kapsayıcıları numaralandırılamıyor.</li><li>Yalnızca BLOB'lar için genel okuma erişimi: Anonim İstek bu kapsayıcıdaki BLOB verilerini okuyabilir, ancak kapsayıcı verileri mevcut değil. İstemcilerin anonim isteğiyle kapsayıcı içindeki blobları numaralandırılamıyor</li><li>Genel okuma erişimi yok: Kapsayıcı ve blob verileri yalnızca hesap sahibi tarafından okunabilir</li></ul><p>Anonim erişim burada belirli bloblar her zaman için anonim okuma erişimi kullanılabilir olmalıdır senaryolar için idealdir. Daha ayrıntılı denetim için farklı izinlerle Kısıtlanmış temsilci erişimi ve belirtilen zaman aralığında sağlayan bir paylaşılan erişim imzası oluşturmayı seçebilirsiniz. Kapsayıcılar ve bloblar, potansiyel olarak hassas veriler içerebilir, anonim erişim yanlışlıkla verilmez emin olun.</p>|

## <a id="limited-access-sas"></a>SAS veya SAP kullanarak Azure Depolama'daki nesnelere sınırlı erişim

| Unvan                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Azure Storage | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikler**              | Yok |
| **Başvuruları**              | [Paylaşılan erişim imzaları, 1. Bölüm: SAS modelini anlama](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/), [paylaşılan erişim imzaları, bölüm 2: Oluşturma ve bir SAS ile Blob Depolama kullanma](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-2/), [paylaşılan erişim imzalarını ve depolanan erişim ilkelerini kullanarak hesabınızdaki nesnelere erişim devretmek nasıl](https://azure.microsoft.com/documentation/articles/storage-security-guide/#_how-to-delegate-access-to-objects-in-your-account-using-shared-access-signatures-and-stored-access-policies) |
| **Adımları** | <p>Paylaşılan erişim imzası (SAS) hesabı erişim anahtarı göstermek zorunda kalmadan diğer istemciler için bir depolama hesabındaki nesnelere sınırlı erişim izni vermek için güçlü bir yol kullanmaktır. SAS sorgu parametrelerini kapsayan bir URI'dir. depolama kaynağı erişimi için gereken bilgilerin tümünü kimlik doğrulaması. SAS ile depolama kaynaklarına erişmek için istemci SAS uygun oluşturucu veya yönteme geçirmek yeterlidir.</p><p>Bir SAS ile hesap anahtarı güvenilir olmayan bir istemci, depolama hesabınızdaki kaynaklara erişimi sağlamak istediğinizde kullanabilirsiniz. Depolama hesabı anahtarlarınızı hem ikisi için de hesabınızda ve tüm kaynakları da yönetici erişim izni birincil ve ikincil bir anahtar içerir. Hesap anahtarlarınızı birini gösterme, kötü amaçlı veya hatalı kullanım olasılığını hesabınıza açılır. Paylaşılan erişim imzaları, okuma, yazma ve depolama hesabınızda göre verilen izinleri ve hesap anahtarı için gerek kalmadan verileri silmek diğer istemcilerin güvenli bir yöntem sağlar.</p><p>Her zaman benzer parametreler mantıksal bir dizi varsa, depolanmış erişim ilkesini (SAP) kullanarak daha iyi bir fikirdir. Depolanan bir erişim ilkesi tarafından türetilmiş bir SAS kullanarak bu SAS hemen iptal etme olanağı sağlar, her zaman mümkün olduğunda depolanan erişim ilkelerini kullanmak için önerilen en iyi uygulama olmasıdır.</p>|
