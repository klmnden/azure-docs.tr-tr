---
title: "Kimlik doğrulama - Microsoft tehdit modelleme aracı - Azure | Microsoft Docs"
description: "Azaltıcı Etkenler tehdit modelleme Aracı kullanıma sunulan tehditleri"
services: security
documentationcenter: na
author: RodSan
manager: RodSan
editor: RodSan
ms.assetid: na
ms.service: security
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: rodsan
ms.openlocfilehash: e547469dc61eddd1d772571ab0919532ac91f128
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2017
---
# <a name="security-frame-authentication--mitigations"></a>Güvenlik çerçevesi: Kimlik doğrulaması | Azaltıcı Etkenler 
| Ürün/hizmet | Makale |
| --------------- | ------- |
| **Web uygulaması**    | <ul><li>[Web uygulaması için kimlik doğrulaması için bir standart kimlik doğrulama mekanizması kullanmayı düşünün](#standard-authn-web-app)</li><li>[Uygulamalar başarısız kimlik doğrulama senaryoları güvenli bir şekilde işlemesi gerekir](#handle-failed-authn)</li><li>[Etkinleştirme adım veya uyarlamalı kimlik doğrulaması](#step-up-adaptive-authn)</li><li>[Yönetim arabirimleri uygun şekilde kilitlendiğini emin olun](#admin-interface-lockdown)</li><li>[Uygulama parolası işlevler güvenli bir şekilde unuttum](#forgot-pword-fxn)</li><li>[Parola ve hesap ilkesi uygulanır emin olun](#pword-account-policy)</li><li>[Kullanıcı adı numaralandırması önlemek için denetimleri uygulayın](#controls-username-enum)</li></ul> |
| **Veritabanı** | <ul><li>[Mümkün olduğunda, SQL Server'a bağlanmak için Windows kimlik doğrulaması kullanın](#win-authn-sql)</li><li>[Mümkün olduğunda SQL veritabanına bağlanma için Azure Active Directory kimlik doğrulaması kullanın](#aad-authn-sql)</li><li>[SQL kimlik doğrulama modu kullanıldığında, hesap ve parola ilke SQL Server'da zorunlu emin olun](#authn-account-pword)</li><li>[SQL kimlik doğrulaması kapsanan veritabanlarında kullanmayın](#autn-contained-db)</li></ul> |
| **Azure Event hub'ı** | <ul><li>[SaS belirteci kullanarak cihaz kimlik doğrulaması kimlik bilgilerini kullanın](#authn-sas-tokens)</li></ul> |
| **Azure güven sınırı** | <ul><li>[Azure yöneticileri Azure çok faktörlü kimlik doğrulamasını etkinleştir](#multi-factor-azure-admin)</li></ul> |
| **Service Fabric güven sınırı** | <ul><li>[Service Fabric kümesi için anonim erişimi kısıtlama](#anon-access-cluster)</li><li>[Service Fabric istemcisi düğümü sertifikanın düğümü düğümü sertifikadan farklı olduğundan emin olun](#fabric-cn-nn)</li><li>[Service fabric kümeleri istemcilerin kimliğini doğrulamak için AAD kullanın](#aad-client-fabric)</li><li>[Service fabric sertifikaları bir onaylanmış sertifika yetkilisinden (CA) aldığınız emin olun](#fabric-cert-ca)</li></ul> |
| **Kimlik sunucusu** | <ul><li>[Kimlik sunucusu tarafından desteklenen standart kimlik doğrulama senaryoları kullanın](#standard-authn-id)</li><li>[Varsayılan kimlik sunucusunu belirteç önbelleği ile ölçeklenebilir bir alternatif geçersiz kıl](#override-token)</li></ul> |
| **Makine güven sınırı** | <ul><li>[Dağıtılan uygulamanın ikili dosyaları dijital olarak imzalandığından emin olun](#binaries-signed)</li></ul> |
| **WCF** | <ul><li>[WCF MSMQ sıraları bağlanırken kimlik doğrulamasını etkinleştirin](#msmq-queues)</li><li>[WCF ileti clientCredentialType none olarak ayarlı değil](#message-none)</li><li>[WCF aktarım clientCredentialType none olarak ayarlı değil](#transport-none)</li></ul> |
| **Web API** | <ul><li>[Bu standart kimlik doğrulama teknikleri Web API güvenliğini sağlamak için kullanılan emin olun](#authn-secure-api)</li></ul> |
| **Azure AD** | <ul><li>[Azure Active Directory tarafından desteklenen standart kimlik doğrulama senaryoları kullanın](#authn-aad)</li><li>[Varsayılan ADAL belirteç önbelleği ile ölçeklenebilir bir alternatif geçersiz kıl](#adal-scalable)</li><li>[TokenReplayCache ADAL kimlik doğrulaması belirteçlerinin yeniden yürütme önlemek için kullanıldığından emin olun](#tokenreplaycache-adal)</li><li>[AAD'ye OAuth2 istemcilerden belirteç isteklerini yönetmek için ADAL kitaplıklarını kullanın (veya şirket içi AD)](#adal-oauth2)</li></ul> |
| **IOT alan ağ geçidi** | <ul><li>[Alan ağ geçidi için bağlanan cihazları kimlik doğrulaması](#authn-devices-field)</li></ul> |
| **IOT bulut ağ geçidi** | <ul><li>[Bulut ağ geçidi bağlanan cihazları doğrulanır emin olun](#authn-devices-cloud)</li><li>[Cihaz başına kimlik doğrulama kimlik bilgilerini kullan](#authn-cred)</li></ul> |
| **Azure Depolama** | <ul><li>[Yalnızca gerekli kapsayıcılar ve bloblar anonim okuma erişimini verildiğinden emin olun](#req-containers-anon)</li><li>[SAS veya SAP kullanarak Azure depolama alanında nesnelere sınırlı erişim](#limited-access-sas)</li></ul> |

## <a id="standard-authn-web-app"></a>Web uygulaması için kimlik doğrulaması için bir standart kimlik doğrulama mekanizması kullanmayı düşünün

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | Yok  |
| Ayrıntılar | <p>Kimlik doğrulama, burada bir varlık genellikle bir kullanıcı adı ve parola gibi kimlik bilgilerini aracılığıyla kendi kimliklerini kanıtlayan işlemidir. Düşünülebilecek birden çok kimlik doğrulama protokolleri kullanılabilir vardır. Bunlardan bazıları aşağıda listelenmiştir:</p><ul><li>İstemci sertifikaları</li><li>Windows tabanlı</li><li>Formlara</li><li>Federasyon - ADFS</li><li>Federasyon - Azure AD</li><li>Federasyon - kimlik sunucusu</li></ul><p>Kaynak işlemi tanımlamak için bir standart kimlik doğrulama mekanizması kullanmayı düşünün</p>|

## <a id="handle-failed-authn"></a>Uygulamalar başarısız kimlik doğrulama senaryoları güvenli bir şekilde işlemesi gerekir

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | Yok  |
| Ayrıntılar | <p>Açıkça kullanıcıların kimliğini doğrulayan uygulamalar başarısız kimlik doğrulama senaryoları güvenli bir şekilde işlemelidir. Kimlik doğrulama mekanizması gerekir:</p><ul><li>Kimlik doğrulama başarısız olduğunda ayrıcalıklı kaynaklara erişimi reddetme</li><li>Genel hata iletisini başarısız kimlik doğrulamasından sonra görüntüler ve erişim reddedildi gerçekleşir</li></ul><p>İçin test edin:</p><ul><li>Başarısız oturum açmalar sonra ayrıcalıklı kaynakların koruma</li><li>Başarısız kimlik doğrulaması genel bir hata iletisi görüntülenir ve olay erişim reddedildi</li><li>Hesapları aşırı sayıda başarısız girişimden sonra devre dışı bırakılır</li><ul>|

## <a id="step-up-adaptive-authn"></a>Etkinleştirme adım veya uyarlamalı kimlik doğrulaması

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | Yok  |
| Ayrıntılar | <p>Uygulaması ek yetkilendirme (adım yukarı gibi ya da OTP SMS, e-posta vb. veya yeniden kimlik doğrulaması için isteyen gönderme gibi çok faktörlü kimlik doğrulaması aracılığıyla Uyarlamalı kimlik doğrulaması) olduğunu doğrulayın kullanıcı için erişim verilmeden önce kimlik doğrulaması için hassas bilgileri. Bu kural için bir hesap veya eylem önemli değişiklikler yapmak için de geçerlidir</p><p>Bu da kimlik doğrulama uyarlama uygulama doğru yoluyla yetkisiz işleme örnekte izin verme amacıyla bağlama duyarlı yetkilendirme zorlar şekilde uygulanması sahip olduğu anlamına gelir parametre oynama</p>|

## <a id="admin-interface-lockdown"></a>Yönetim arabirimleri uygun şekilde kilitlendiğini emin olun

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | Yok  |
| Ayrıntılar | İlk Yönetim arabirimine yalnızca belirli kaynak IP arasında bir aralık erişimi çözümüdür. Bu çözüm her zaman daha mümkün olmayacaktır Yönetim arabirimine oturum açma için geçmeksizin veya uyarlamalı bir kimlik doğrulaması zorlamak için önerilen |

## <a id="forgot-pword-fxn"></a>Uygulama parolası işlevler güvenli bir şekilde unuttum

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | Yok  |
| Ayrıntılar | <p>İlk şey doğrulamak için parolanızı mı unuttunuz ve diğer kurtarma yolları parola yerine bir zaman sınırlı etkinleştirme belirteci dahil olmak üzere bağlantısı Gönder ' dir. Bağlantı üzerinden gönderilmeden önce ek kimlik doğrulama soft-belirteçleri (örn. SMS belirteci, yerel mobil uygulamalar, vb.) göre de gerekli olabilir. Yeni bir parola alma işlemi devam ediyor adımında ikinci olarak, kullanıcı hesabını kilitlemek değil.</p><p>Bu durum, otomatik bir saldırı kullanıcılarla çıkışı bilerek kilitlemek bir saldırgan karar her bir hizmet reddi saldırısına için neden olabilir. Yeni parola isteği ediyor kümesi olduğunda, üçüncü görüntü iletisi kullanıcıadı numaralandırması önlemek için genelleştirilmiş. Dördüncü, her zaman eski parolaların kullanımına izin verme ve güçlü bir parola ilkesi uygulayın.</p> |

## <a id="pword-account-policy"></a>Parola ve hesap ilkesi uygulanır emin olun

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | Yok  |
| Ayrıntılar | <p>Parola ve hesap ilkesiyle uyumlu kuruluş ilkesi ve en iyi yöntemler uygulanmalıdır.</p><p>Yanılma ve temel sözlük tahmin karşı korumak için: güçlü bir parola ilkesi uygulanan, kullanıcıların karmaşık parola (örn., 12 karakter en az uzunluk, alfasayısal ve özel karakterler) oluşturma emin olmak için.</p><p>Hesap kilitleme ilkeleri aşağıdaki şekilde uygulanabilir:</p><ul><li>**Yazılım kilidi genişletme:** bu kullanıcılarınızın deneme yanılma saldırılarına karşı korumak için iyi bir seçenek olabilir. Yanlış bir parola kullanıcının girdiği her Örneğin, üç kez uygulama hesap bir dakika için aşağı aşağı ve bu da saldırganın devam etmek için daha az karlı yapmadan parolasını zorlama kaba işlemi yavaş için kilitlenemedi. Bu örnek elde etmek için Sabit kilitleme önlemler uygulamak için olsaydı "kalıcı olarak hesapları kilitleme tarafından Dos" a. Alternatif olarak, uygulama bir OTP (bir saat parola) oluşturma ve bant dışı Gönder (e-posta, aracılığıyla sms vb.) kullanıcı. Başka bir yaklaşım, başarısız girişim sayısı eşiği sayısını ulaşıldıktan sonra CAPTCHA uygulamak için olabilir.</li><li>**Sabit kilitleme:** uygulamanızı saldırmak kullanıcı algılamak ve onu bir yanıt takım kendi hukuk yapmak için vaktim kadar kalıcı olarak kendi hesabını kilitlemek yoluyla sayaç kilitleme bu tür uygulanmalıdır. Kullanıcıya vermek için karar verebilirsiniz bu işlem hesabının geri veya daha fazla kendisine karşı yasal eylemleri gerçekleştirin. Bu tür bir yaklaşım, daha fazla uygulama ve altyapı penetrating gelen saldırganın engeller.</li></ul><p>Varsayılan ve tahmin edilebilir hesapları saldırılarına karşı korumak için tüm anahtarları ve parolaları değiştirilebilir ve olan oluşturulan veya değiştirilen sonra yükleme süresini doğrulayın.</p><p>Uygulama parolaları otomatik olarak oluşturmak varsa, üretilen parola rastgele ve yüksek entropi içerdiğinden emin olun.</p>|

## <a id="controls-username-enum"></a>Kullanıcı adı numaralandırması önlemek için denetimleri uygulayın

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web Uygulaması | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | Kullanıcı adı numaralandırması önlemek için tüm hata iletilerinin genelleştirilmiş. Ayrıca bazen bir kayıt sayfasına gibi işlevler sızmasını bilgi kaçının olamaz. Burada bir saldırgan tarafından otomatik bir saldırı önlemek için hız sınırlaması yöntemler CAPTCHA gibi kullanmanız gerekir. |

## <a id="win-authn-sql"></a>Mümkün olduğunda, SQL Server'a bağlanmak için Windows kimlik doğrulaması kullanın

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Database | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | OnPrem |
| **Öznitelikleri**              | SQL sürümü - tüm |
| **Başvuruları**              | [SQL Server - bir kimlik doğrulama modu seçin](https://msdn.microsoft.com/library/ms144284.aspx) |
| **Adımları** | Windows kimlik doğrulaması Kerberos güvenlik protokolünü kullanır, güçlü parolalar karmaşıklık doğrulaması açısından parola ilkesi zorunluluğu sağlar, hesap kilitleme için destek sağlar ve parola süre aşımını destekler.|

## <a id="aad-authn-sql"></a>Mümkün olduğunda SQL veritabanına bağlanma için Azure Active Directory kimlik doğrulaması kullanın

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Database | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | SQL Azure |
| **Öznitelikleri**              | SQL sürümü - V12 |
| **Başvuruları**              | [Azure Active Directory kimlik doğrulamasını kullanarak SQL veritabanına bağlanma](https://azure.microsoft.com/documentation/articles/sql-database-aad-authentication/) |
| **Adımları** | **En düşük sürüm:** Azure SQL Database V12 gereken Microsoft Directory AAD kimlik doğrulamasını kullanmak Azure SQL veritabanı izin vermek için |

## <a id="authn-account-pword"></a>SQL kimlik doğrulama modu kullanıldığında, hesap ve parola ilke SQL Server'da zorunlu emin olun

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Database | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | [SQL Server Parola İlkesi](https://technet.microsoft.com/library/ms161959(v=sql.110).aspx) |
| **Adımları** | SQL Server kimlik doğrulaması kullanırken, Windows kullanıcı hesaplarına dayalı olmayan SQL Server oturumları oluşturulur. Kullanıcı adı ve parola SQL Server kullanarak oluşturulur ve SQL Server içinde depolanır. SQL Server, Windows parola ilkesi mekanizmaları kullanabilirsiniz. Aynı karmaşıklığını ve SQL Server içinde kullanılan parolalar için Windows'ta kullanılan süre sonu ilkelerini uygulayabilirsiniz. |

## <a id="autn-contained-db"></a>SQL kimlik doğrulaması kapsanan veritabanlarında kullanmayın

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Database | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | OnPrem, SQL Azure |
| **Öznitelikleri**              | SQL sürüm - MSSQL2012, SQL sürümü - V12 |
| **Başvuruları**              | [Kapsanan veritabanları ile en iyi güvenlik uygulamaları](http://msdn.microsoft.com/library/ff929055.aspx) |
| **Adımları** | Zorlanan parola ilkesi yokluğu kapsanan bir veritabanında kuruldu zayıf bir kimlik bilgisi olasılığını artırabilir. Windows kimlik doğrulaması yararlanın. |

## <a id="authn-sas-tokens"></a>SaS belirteci kullanarak cihaz kimlik doğrulaması kimlik bilgilerini kullanın

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Azure Event hub'ı | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | [Olay hub'ları kimlik doğrulaması ve güvenlik modeline genel bakış](https://azure.microsoft.com/documentation/articles/event-hubs-authentication-and-security-model-overview/) |
| **Adımları** | <p>Olay hub'ları güvenlik modelini birleşimi paylaşılan erişim imzası (SAS) belirteçleri ve olay yayımcıları üzerinde temel alır. Yayımcı adı, belirteç alan DeviceID temsil eder. Bu, ilgili aygıtlarla oluşturulan belirteçleri ilişkilendirmek yardımcı olacaktır.</p><p>Tüm iletileri gönderen hizmet tarafında algılama denemeleri yanıltma yükü kaynak izin verme ile etiketlenir. Cihaz kimlik doğrulamasını yaparken, Oluştur bir benzersiz bir yayımcı kapsamlı aygıt başına SaS belirteci.</p>|

## <a id="multi-factor-azure-admin"></a>Azure yöneticileri Azure çok faktörlü kimlik doğrulamasını etkinleştir

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Azure güven sınırı | 
| **SDL aşaması**               | Dağıtım |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | [Azure Multi-Factor Authentication nedir?](https://azure.microsoft.com/documentation/articles/multi-factor-authentication/) |
| **Adımları** | <p>Çok faktörlü kimlik doğrulaması (MFA), birden fazla doğrulama yöntemi gerektiren ve kullanıcı oturum açmalarına ve işlemlerine önemli bir ikinci güvenlik katmanı ekleyen kimlik doğrulama yöntemidir. Her iki veya daha fazla aşağıdaki doğrulama yöntemlerini isteyerek çalışır:</p><ul><li>(Genellikle parola) bildiğiniz bir şey</li><li>(Kolayca, bir telefon gibi yineleniyor değil güvenilir bir cihaz) sahip olduğunuz şey</li><li>(Biyometri) olan bir şey</li><ul>|

## <a id="anon-access-cluster"></a>Service Fabric kümesi için anonim erişimi kısıtlama

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Service Fabric güven sınırı | 
| **SDL aşaması**               | Dağıtım |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Ortam - Azure  |
| **Başvuruları**              | [Service Fabric kümesi güvenlik senaryoları](https://azure.microsoft.com/documentation/articles/service-fabric-cluster-security) |
| **Adımları** | <p>Yetkisiz kullanıcılar özellikle üretim iş yükleri üzerinde çalıştırılan sahip olduğunda, kümeniz için bağlanmasını önlemek için kümeleri her zaman güvenli hale getirilmelidir.</p><p>Service fabric kümesi oluşturulurken kullanılan güvenlik modunu "secure" ve gerekli X.509 sunucu sertifikası yapılandırmak için ayarlandığından emin olun. "Güvenli olmayan" bir küme oluşturma genel internet yönetim uç noktalarını kullanıma sunar, bağlanmak herhangi bir anonim kullanıcı izin verir.</p>|

## <a id="fabric-cn-nn"></a>Service Fabric istemcisi düğümü sertifikanın düğümü düğümü sertifikadan farklı olduğundan emin olun

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Service Fabric güven sınırı | 
| **SDL aşaması**               | Dağıtım |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Ortam - Azure, ortam - tek başına |
| **Başvuruları**              | [Service Fabric istemcisi düğümü sertifika güvenliği](https://azure.microsoft.com/documentation/articles/service-fabric-cluster-security/#_client-to-node-certificate-security), [istemci sertifikası ile güvenli bir kümeye Bağlan](https://azure.microsoft.com/documentation/articles/service-fabric-connect-to-secure-cluster/) |
| **Adımları** | <p>İstemci düğüm sertifika güvenliği Resource Manager şablonları ya da bir yönetici istemci sertifikası ve/veya bir kullanıcı istemci sertifikası belirterek tek başına JSON şablon Azure Portalı aracılığıyla ya da küme oluşturulurken yapılandırılır.</p><p>Belirttiğiniz yönetici istemci ve kullanıcı istemci sertifikası düğümü düğümü güvenlik için belirttiğiniz birincil ve ikincil sertifikaları farklı olmalıdır.</p>|

## <a id="aad-client-fabric"></a>Service fabric kümeleri istemcilerin kimliğini doğrulamak için AAD kullanın

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Service Fabric güven sınırı | 
| **SDL aşaması**               | Dağıtım |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Ortam - Azure |
| **Başvuruları**              | [Kümesi güvenlik senaryolarını - güvenlik önerileri](https://azure.microsoft.com/documentation/articles/service-fabric-cluster-security/#security-recommendations) |
| **Adımları** | Azure üzerinde çalışan kümelerle de Azure Active Directory (AAD) dışında istemci sertifikalarını kullanan yönetim uç noktalarına erişime güvenliğini sağlayabilirsiniz. Azure kümeler için istemciler ve sertifikaları düğümü düğümü güvenlik kimlik doğrulaması için AAD güvenlik kullanmanız önerilir.|

## <a id="fabric-cert-ca"></a>Service fabric sertifikaları bir onaylanmış sertifika yetkilisinden (CA) aldığınız emin olun

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Service Fabric güven sınırı | 
| **SDL aşaması**               | Dağıtım |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Ortam - Azure |
| **Başvuruları**              | [X.509 sertifikaları ve Service Fabric](https://azure.microsoft.com/documentation/articles/service-fabric-cluster-security/#x509-certificates-and-service-fabric) |
| **Adımları** | <p>Service Fabric, kimlik doğrulama, düğümlerin ve istemciler için X.509 sertifikaları kullanır.</p><p>Hizmet fabrics'e sertifikalar kullanırken dikkate alınması gereken bazı önemli noktalar:</p><ul><li>Üretim iş yükleri çalıştıran kümelerde kullanılan sertifikaları doğru yapılandırılmış bir Windows Server sertifika hizmeti kullanılarak oluşturulan veya bir onaylanmış sertifika yetkilisinden (CA) edinilen. CA'ın onaylı bir dış CA veya bir düzgün yönetilen iç ortak anahtar altyapısı (PKI) olabilir</li><li>Hiçbir zaman hiçbir geçici kullanın veya MakeCert.exe gibi araçları ile oluşturulmuş üretim sertifikaları test et</li><li>Kendinden imzalı bir sertifika kullanabilirsiniz, ancak yalnızca test kümeleri için alan ve üretim bunu</li></ul>|

## <a id="standard-authn-id"></a>Kimlik sunucusu tarafından desteklenen standart kimlik doğrulama senaryoları kullanın

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Kimlik sunucusu | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | [IdentityServer3 - büyük resmi](https://identityserver.github.io/Documentation/docsv2/overview/bigPicture.html) |
| **Adımları** | <p>Aşağıda kimlik sunucusu tarafından desteklenen tipik etkileşimler şunlardır:</p><ul><li>Tarayıcılar web uygulamaları ile iletişim</li><li>Web uygulamalarını web API'leri (bazen üzerinde kendi, bazen bir kullanıcı adına) ile iletişim</li><li>Tarayıcı tabanlı uygulamalar, web API'leri ile iletişim</li><li>Yerel uygulamalar, web API ile iletişim</li><li>Sunucu tabanlı uygulamalar, web API'leri ile iletişim</li><li>Web API web API'leri (bazen üzerinde kendi, bazen bir kullanıcı adına) ile iletişim</li></ul>|

## <a id="override-token"></a>Varsayılan kimlik sunucusunu belirteç önbelleği ile ölçeklenebilir bir alternatif geçersiz kıl

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Kimlik sunucusu | 
| **SDL aşaması**               | Dağıtım |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | [Kimlik sunucusu dağıtım - önbelleğe alma](https://identityserver.github.io/Documentation/docsv2/advanced/deployment.html) |
| **Adımları** | <p>IdentityServer basit, yerleşik bir bellek içi önbellek bulunur. Bu küçük ölçekli yerel uygulamalar için iyi olmakla birlikte, aşağıdaki nedenlerle orta katman ve arka uç uygulamalar için ölçeklenmez:</p><ul><li>Bu uygulamaları aynı anda birden çok kullanıcı tarafından erişilir. Aynı depoda tüm erişim belirteçleri kaydetme yalıtım sorunlar oluşturur ve ölçekte çalışırken zorluklar sunar: her uygulama erişen onların adına, kaynak olarak sayıda belirteçleri ile çok sayıda kullanıcı, çok büyük sayılar ve çok pahalı arama işlemleri anlamına gelebilir</li><li>Bu uygulamalar genellikle burada birden çok düğümün aynı önbellek erişiminiz olmalıdır dağıtılmış topolojileri dağıtılan</li><li>Önbelleğe alınan belirteçleri, işlem dönüştürür ve deactivations varlığını sürdürmesini gerekir</li><li>Tüm yukarıdaki nedeniyle, web uygulamaları, uygulama çalışırken varsayılan kimlik sunucunun belirteç önbelleği ile Azure Redis önbelleği gibi ölçeklenebilir bir alternatif geçersiz kılmak için önerilir.</li></ul>|

## <a id="binaries-signed"></a>Dağıtılan uygulamanın ikili dosyaları dijital olarak imzalandığından emin olun

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Makine güven sınırı | 
| **SDL aşaması**               | Dağıtım |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | Böylece ikili dosyaların bütünlüğünü doğrulanabilir dağıtılan uygulamanın ikili dosyaları dijital olarak imzalandığından emin olun|

## <a id="msmq-queues"></a>WCF MSMQ sıraları bağlanırken kimlik doğrulamasını etkinleştirin

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | WCF | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel, NET Framework 3 |
| **Öznitelikleri**              | Yok |
| **Başvuruları**              | [MSDN](https://msdn.microsoft.com/library/ff648500.aspx) |
| **Adımları** | MSMQ sıralara bağlanırken kimlik doğrulamasını etkinleştirmek program başarısız olursa, bir saldırganın anonim olarak iletilerini işlemek için kuyruğa gönderebilirsiniz. Kimlik doğrulama ileti başka bir programa teslim etmek için kullanılan bir MSMQ sırası bağlanmak için kullanılan değil, bir saldırgan kötü amaçlı bir anonim iletisi gönder.|

### <a name="example"></a>Örnek
`<netMsmqBinding/>` WCF yapılandırma dosyasının aşağıdaki öğesinin kimlik doğrulama ileti teslimi bir MSMQ sırası bağlanırken devre dışı bırakmak için WCF bildirir.
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
MSMQ gelen veya Giden iletileriniz için her zaman Windows etki alanı veya sertifika kimlik doğrulaması gerektirecek şekilde yapılandırın.

### <a name="example"></a>Örnek
`<netMsmqBinding/>` Öğesi aşağıdaki WCF yapılandırma dosyasının bir MSMQ kuyruğuna bağlanırken sertifika kimlik doğrulamasını etkinleştirmek için WCF bildirir. İstemci, X.509 sertifikaları kullanılarak doğrulanır. İstemci sertifikası sunucu sertifika deposunda mevcut olması gerekir.
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

## <a id="message-none"></a>WCF ileti clientCredentialType none olarak ayarlı değil

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | WCF | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | .NET framework 3 |
| **Öznitelikleri**              | İstemci kimlik bilgisi türü - yok |
| **Başvuruları**              | [MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [Fortify](https://vulncat.fortify.com/en/vulncat/index.html) |
| **Adımları** | Kimlik doğrulama yokluğu herkes bu hizmete erişebilmesi için anlamına gelir. İstemcilerinden kimlik doğrulama kullanmaz bir hizmetin tüm kullanıcılara erişim izni verir. İstemci kimlik bilgilerine karşı kimlik doğrulaması için uygulamayı yapılandırma. Bu, Windows ya da sertifika ileti clientCredentialType ayarlayarak yapılabilir. |

### <a name="example"></a>Örnek
```
<message clientCredentialType=""Certificate""/>
```

## <a id="transport-none"></a>WCF aktarım clientCredentialType none olarak ayarlı değil

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | WCF | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel, .NET Framework 3 |
| **Öznitelikleri**              | İstemci kimlik bilgisi türü - yok |
| **Başvuruları**              | [MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [Fortify](https://vulncat.fortify.com/en/vulncat/index.html) |
| **Adımları** | Kimlik doğrulama yokluğu herkes bu hizmete erişebilmesi için anlamına gelir. İstemcilerinden kimlik doğrulama kullanmaz bir hizmetin tüm kullanıcılara işlevselliğini erişim sağlar. İstemci kimlik bilgilerine karşı kimlik doğrulaması için uygulamayı yapılandırma. Bu, Windows ya da sertifika aktarım clientCredentialType ayarlayarak yapılabilir. |

### <a name="example"></a>Örnek
```
<transport clientCredentialType=""Certificate""/>
```

## <a id="authn-secure-api"></a>Bu standart kimlik doğrulama teknikleri Web API güvenliğini sağlamak için kullanılan emin olun

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Web API | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | [Kimlik doğrulama ve yetkilendirme ASP.NET Web API'de](http://www.asp.net/web-api/overview/security/authentication-and-authorization-in-aspnet-web-api), [ASP.NET Web API ile dış kimlik doğrulama hizmeti (C#)](http://www.asp.net/web-api/overview/security/external-authentication-services) |
| **Adımları** | <p>Kimlik doğrulama, burada bir varlık genellikle bir kullanıcı adı ve parola gibi kimlik bilgilerini aracılığıyla kendi kimliklerini kanıtlayan işlemidir. Düşünülebilecek birden çok kimlik doğrulama protokolleri kullanılabilir vardır. Bunlardan bazıları aşağıda listelenmiştir:</p><ul><li>İstemci sertifikaları</li><li>Windows tabanlı</li><li>Formlara</li><li>Federasyon - ADFS</li><li>Federasyon - Azure AD</li><li>Federasyon - kimlik sunucusu</li></ul><p>Başvurular bölümündeki bağlantıları nasıl her kimlik doğrulama şemasını bir Web API güvenliğini sağlamak için uygulanabilir üzerinde alt düzey ayrıntıları sağlayın.</p>|

## <a id="authn-aad"></a>Azure Active Directory tarafından desteklenen standart kimlik doğrulama senaryoları kullanın

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Azure AD | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | [Azure AD için kimlik doğrulama senaryoları](https://azure.microsoft.com/documentation/articles/active-directory-authentication-scenarios/), [Azure Active Directory kod örnekleri](https://azure.microsoft.com/documentation/articles/active-directory-code-samples/), [Azure Active Directory Geliştirici Kılavuzu](https://azure.microsoft.com/documentation/articles/active-directory-developers-guide/) |
| **Adımları** | <p>Azure Active Directory (Azure AD), OAuth 2.0 ve Openıd Connect gibi endüstri standardı protokoller desteği olan bir hizmet olarak kimlik sağlayarak geliştiriciler için kimlik doğrulama basitleştirir. Aşağıda Azure AD tarafından desteklenen beş birincil uygulama senaryolar şunlardır:</p><ul><li>Web tarayıcısı Web uygulamasına: bir kullanıcı Azure AD tarafından güvenliği sağlanan bir web uygulaması oturum açması gerekiyor</li><li>Tek sayfa uygulama (SPA): Bir kullanıcı Azure AD tarafından güvenliği sağlanan bir tek sayfa uygulaması oturum açması gerekiyor</li><li>Web API yerel uygulamaya: Web'den Azure AD tarafından güvenli API kaynakları almak için bir kullanıcının kimliğini doğrulamak telefon, tablet veya PC çalıştıran yerel bir uygulama gerekiyor</li><li>Web uygulaması Web API'si: kaynakları bir Web API Azure AD tarafından güvenlik altına almak bir web uygulaması gerekiyor</li><li>Arka plan programı veya Web API için sunucu uygulaması: arka plan programı uygulama veya bir sunucu uygulaması web kullanıcı arabirimi ile kaynakları web API'si Azure AD tarafından güvenlik altına almanız gerekir</li></ul><p>Alt düzey uygulama ayrıntıları için başvurular bölümündeki bağlantılara bakın</p>|

## <a id="adal-scalable"></a>Varsayılan ADAL belirteç önbelleği ile ölçeklenebilir bir alternatif geçersiz kıl

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Azure AD | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | [Web uygulamaları için Azure Active Directory ile modern kimlik doğrulaması](https://blogs.msdn.microsoft.com/microsoft_press/2016/01/04/new-book-modern-authentication-with-azure-active-directory-for-web-applications/), [kullanarak Redis ADAL belirteç önbelleği olarak](https://blogs.msdn.microsoft.com/mrochon/2016/09/19/using-redis-as-adal-token-cache/)  |
| **Adımları** | <p>ADAL (Active Directory Authentication Library) kullanımdır işlem genelinde kullanılabilir bir statik depolamaya dayanır bir bellek içi önbellek varsayılan önbellek. Bu yerel uygulamalar için çalışırken, aşağıdaki nedenlerle orta katman ve arka uç uygulamalar için ölçeklenmez:</p><ul><li>Bu uygulamaları aynı anda birden çok kullanıcı tarafından erişilir. Aynı depoda tüm erişim belirteçleri kaydetme yalıtım sorunlar oluşturur ve ölçekte çalışırken zorluklar sunar: her uygulama erişen onların adına, kaynak olarak sayıda belirteçleri ile çok sayıda kullanıcı, çok büyük sayılar ve çok pahalı arama işlemleri anlamına gelebilir</li><li>Bu uygulamalar genellikle burada birden çok düğümün aynı önbellek erişiminiz olmalıdır dağıtılmış topolojileri dağıtılan</li><li>Önbelleğe alınan belirteçleri, işlem dönüştürür ve deactivations varlığını sürdürmesini gerekir</li></ul><p>Tüm yukarıdaki nedeniyle, web uygulamaları, uygulama yüklenirken varsayılan ADAL belirteç önbelleği ile Azure Redis önbelleği gibi ölçeklenebilir bir alternatif geçersiz kılmak için önerilir.</p>|

## <a id="tokenreplaycache-adal"></a>TokenReplayCache ADAL kimlik doğrulaması belirteçlerinin yeniden yürütme önlemek için kullanıldığından emin olun

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Azure AD | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | [Web uygulamaları için Azure Active Directory ile modern kimlik doğrulaması](https://blogs.msdn.microsoft.com/microsoft_press/2016/01/04/new-book-modern-authentication-with-azure-active-directory-for-web-applications/) |
| **Adımları** | <p>Hiçbir belirteci birden çok kez kullanılabilir doğrulama amacıyla belirteçleri kaydetmek için kullanılan bir mağaza bir belirteç yeniden yürütme önbellek tanımlamak geliştiricilerin TokenReplayCache özelliği sağlar.</p><p>Bu bir ölçüdür aptly çağrılan belirteç yeniden yürütme saldırı ortak bir saldırılara karşı: oturum açma sırasında gönderilen belirteç kesintiye uğratan bir saldırganın uygulamaya yeniden göndermeyi deneyin ("yeni bir oturum oluşturmak için anlayamazsanız"). Örneğin, kullanıcı başarıyla kimlik doğrulamasından sonra içinde OIDC kodu verme akışı, bir istek "/ signin-oidc" "id_token" ile "code" ve "Durum" parametreleri bağlı olan taraf uç nokta yapılan.</p><p>Bağlı olan taraf bu isteği doğrular ve yeni bir oturum oluşturur. Bir saldırganın bu isteği yakalar ve bunu başlayarak yeniden oynatılır, klasöründe başarılı bir oturum oluşturun ve kullanıcının aldatma. Openıd Connect içinde nonce varlığını sınırlamak ancak tam olarak hangi saldırı başarıyla kamulaştırılmış koşullar ortadan kaldırmak. Uygulamalarını korumak için geliştiriciler bir ITokenReplayCache belirtin ve bir örnek için TokenReplayCache atayın.</p>|

### <a name="example"></a>Örnek
```C#
// ITokenReplayCache defined in ADAL
public interface ITokenReplayCache
{
bool TryAdd(string securityToken, DateTime expiresOn);
bool TryFind(string securityToken);
}
```

### <a name="example"></a>Örnek
Burada, örnek uygulama ITokenReplayCache arabiriminin verilmiştir. (Lütfen özelleştirmek ve projeye özgü önbelleğe alma framework'ünüzün uygulama)
```C#
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
"Tokenvalidationparameters değerini" özelliği aracılığıyla OIDC seçeneklerinde gibi başvurulacak uygulanan bir önbelleğe sahiptir.
```C#
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

Lütfen bu yapılandırma, oturum açma uygulamanıza yerel OIDC korumalı verimliliğini test ve isteği yakalamak için unutmayın `"/signin-oidc"` fiddler'da uç noktası. Koruma yerinde olmadığı durumlarda, bu istek fiddler'da yeniden oynatmak yeni bir oturum tanımlama ayarlayın. TokenReplayCache koruma eklendikten sonra isteği tekrarlanır, uygulama gibi bir özel durum oluşturur:`SecurityTokenReplayDetectedException: IDX10228: The securityToken has previously been validated, securityToken: 'eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1uQ19WWmNBVGZNNXBPWWlKSE1iYTlnb0VLWSIsImtpZCI6Ik1uQ1......`

## <a id="adal-oauth2"></a>AAD'ye OAuth2 istemcilerden belirteç isteklerini yönetmek için ADAL kitaplıklarını kullanın (veya şirket içi AD)

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Azure AD | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | [ADAL](https://azure.microsoft.com/documentation/articles/active-directory-authentication-libraries/) |
| **Adımları** | <p>Azure AD authentication Library (ADAL) kolayca bulut için kullanıcıların kimliğini doğrulamak istemci uygulaması geliştiricileri sağlar veya şirket içi Active Directory (AD) ve API çağrıları güvenliğini sağlamak için erişim belirteci alın.</p><p>ADAL sahip birçok özellik yapma kimlik doğrulamanın daha kolay zaman uyumsuz desteği, erişim belirteçleri ve yenileme belirteçleri, bir erişim belirtecinin süresi ve yenileme belirteci kullanılabilir olduğunda otomatik belirteci yenileme depolar yapılandırılabilir bir belirteç önbelleği gibi geliştiriciler için ve Daha fazla.</p><p>ADAL, çoğu karmaşıklık işleyerek kendi iş mantığı Geliştirici odaklanmanıza yardımcı ve kolayca kaynakları güvenlik uzmanı olmadan güvenli hale getirme. Ayrı kitaplıklar, .NET, JavaScript (istemci ve Node.js), iOS, Android ve Java için kullanılabilir.</p>|

## <a id="authn-devices-field"></a>Alan ağ geçidi için bağlanan cihazları kimlik doğrulaması

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | IOT alan ağ geçidi | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok  |
| **Başvuruları**              | Yok  |
| **Adımları** | Her cihaz bunları verileri kabul etmek ve bulut ağ geçidi Yukarı Akış iletişimlerinizde kolaylaştırmanın önce alan ağ geçidi tarafından doğrulandıktan emin olun. Ayrıca, aygıtlar ile bağlanmasını sağlamak bir aygıt başına tek bir cihazı benzersiz olarak tanımlanabilir böylece kimlik bilgisi.|

## <a id="authn-devices-cloud"></a>Bulut ağ geçidi bağlanan cihazları doğrulanır emin olun

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | IOT bulut ağ geçidi | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel, C#, Node.JS,  |
| **Öznitelikleri**              | Yok, ağ geçidi seçim - Azure IOT Hub |
| **Başvuruları**              | Yok, [.NET ile Azure IOT hub](https://azure.microsoft.com/documentation/articles/iot-hub-csharp-csharp-getstarted/), [olan IOT hub'ı kullanmaya başlama ve düğüm JS](https://azure.microsoft.com/documentation/articles/iot-hub-node-node-getstarted), [SAS ve sertifikaları ile güvenli hale getirme IOT](https://azure.microsoft.com/documentation/articles/iot-hub-sas-tokens/), [Git deposu](https://github.com/Azure/azure-iot-sdks/tree/master/node) |
| **Adımları** | <ul><li>**Genel:** aygıtı Aktarım Katmanı Güvenliği (TLS) veya IPSec kullanarak kimlik doğrulaması. Altyapı önceden paylaşılan anahtar (PSK) kullanarak tam asimetrik şifreleme işleyemiyor bu cihazlarda desteklemelidir. Azure AD, Oauth yararlanın.</li><li>**C# ' ta:** DeviceClient örneği oluşturulurken varsayılan olarak, Create yöntemi IOT Hub ile iletişim kurmak için AMQP protokolünü kullanan bir DeviceClient örneği oluşturur. HTTPS protokolünü kullanmak için Oluştur yönteminin Protokolü belirtmenize olanak tanıyan geçersiz kılma kullanın. HTTPS protokolünü kullanıyorsanız, aynı zamanda eklemelisiniz `Microsoft.AspNet.WebApi.Client` NuGet paketini projenize eklemek için `System.Net.Http.Formatting` ad alanı.</li></ul>|

### <a name="example"></a>Örnek
```C#
static DeviceClient deviceClient;

static string deviceKey = "{device key}";
static string iotHubUri = "{iot hub hostname}";

var messageString = "{message in string format}";
var message = new Message(Encoding.ASCII.GetBytes(messageString));

deviceClient = DeviceClient.Create(iotHubUri, new DeviceAuthenticationWithRegistrySymmetricKey("myFirstDevice", deviceKey));

await deviceClient.SendEventAsync(message);
```

### <a name="example"></a>Örnek
**Node.JS: kimlik doğrulaması**
#### <a name="symmetric-key"></a>Simetrik anahtar
* Azure üzerinde bir IOT hub oluşturma
* Cihaz kimlik kayıt defterinde bir giriş oluşturun
    ```javascript
    var device = new iothub.Device(null);
    device.deviceId = <DeviceId >
    registry.create(device, function(err, deviceInfo, res) {})
    ```
* Sanal bir cihaz oluşturma
    ```javascript
    var clientFromConnectionString = require('azure-iot-device-amqp').clientFromConnectionString;
    var Message = require('azure-iot-device').Message;
    var connectionString = 'HostName=<HostName>DeviceId=<DeviceId>SharedAccessKey=<SharedAccessKey>';
    var client = clientFromConnectionString(connectionString);
    ```
#### <a name="sas-token"></a>SAS belirteci
* Kullanırken, simetrik anahtar ancak dahili olarak oluşturulan oluşturabilir ve açıkça de kullanın
* Bir protokol tanımlayın:`var Http = require('azure-iot-device-http').Http;`
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
* Kendi imzalı X509 oluşturmak sertifika ve anahtar sırasıyla depolamak için bir .cert ve .key dosyaları oluşturmak için OpenSSL gibi herhangi bir aracı kullanarak sertifika
* Sertifikaları kullanarak güvenli bir bağlantı kabul eden bir cihaz hazırlayın.
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
* Bir sertifika kullanarak bir aygıtı bağlayın
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

## <a id="authn-cred"></a>Cihaz başına kimlik doğrulama kimlik bilgilerini kullan

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | IOT bulut ağ geçidi  | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Ağ geçidi seçim - Azure IOT Hub |
| **Başvuruları**              | [Azure IOT Hub güvenlik belirteçleri](https://azure.microsoft.com/documentation/articles/iot-hub-sas-tokens/) |
| **Adımları** | SaS belirteci kullanarak cihaz kimlik doğrulaması kimlik bilgilerini başına kullanım aygıt anahtarı veya istemci sertifikası göre IOT Hub düzeyi yerine paylaşılan erişim ilkeleri. Bu bir başkası tarafından bir aygıt veya alan ağ geçidi'nin kimlik doğrulama belirteçleri kullanılmasını engeller |

## <a id="req-containers-anon"></a>Yalnızca gerekli kapsayıcılar ve bloblar anonim okuma erişimini verildiğinden emin olun

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Azure Storage | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | StorageType - Blob |
| **Başvuruları**              | [Kapsayıcılar ve bloblar için anonim okuma erişimini yönetme](https://azure.microsoft.com/documentation/articles/storage-manage-access-to-resources/), [paylaşılan erişim imzaları, 1. parça: SAS modelini anlama](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/) |
| **Adımları** | <p>Varsayılan olarak, bir kapsayıcı ve içindeki tüm BLOB'ları yalnızca depolama hesabı sahibi tarafından erişilebilir. Anonim kullanıcılar bir kapsayıcı ve bloblarını için Okuma izinleri vermek için bir genel erişime izin vermek için kapsayıcı izinlerini ayarlayabilirsiniz. Anonim kullanıcılar istek doğrulanmadan genel olarak erişilebilir bir kapsayıcıdaki blobları okuyabilir.</p><p>Kapsayıcılar kapsayıcı erişimi yönetmek için aşağıdaki seçenekleri sağlar:</p><ul><li>Tam herkese okuma erişimi: kapsayıcı ve blob verilerini anonim istek okunabilir. İstemcilerin anonim istek aracılığıyla kapsayıcıdaki blobları sıralayabilirsiniz, ancak depolama hesabı kapsayıcılara numaralandırılamıyor.</li><li>Ortak okuma erişiminin yalnızca BLOB'lar için: Bu kapsayıcı içindeki Blob verilerini anonim istek okunabilir ancak kapsayıcı verileri kullanılabilir değil. İstemcilerin anonim istek aracılığıyla kapsayıcıdaki blobları numaralandırılamıyor</li><li>Hiçbir public okuma erişimi: kapsayıcı ve blob verileri yalnızca hesap sahibi tarafından okunabilir</li></ul><p>Anonim erişim burada belirli BLOB'lar her zaman için anonim okuma erişimini kullanılabilir olmalıdır senaryoları için en iyisidir. Geçirmiş denetim için bir farklı izinleri kullanarak Kısıtlanmış temsilci erişimi ve belirli bir zaman aralığı içinde sağlayan bir paylaşılan erişim imzası oluşturabilirsiniz. Kapsayıcılar ve olası hassas verileri içerebilir, bloblar anonim erişim yanlışlıkla verilmez emin olun</p>|

## <a id="limited-access-sas"></a>SAS veya SAP kullanarak Azure depolama alanında nesnelere sınırlı erişim

| Başlık                   | Ayrıntılar      |
| ----------------------- | ------------ |
| **Bileşen**               | Azure Storage | 
| **SDL aşaması**               | Oluşturma |  
| **İlgili teknolojiler** | Genel |
| **Öznitelikleri**              | Yok |
| **Başvuruları**              | [Paylaşılan erişim imzalar, 1. parça: SAS modelini anlama](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/), [paylaşılan erişim imzaları, bölüm 2: oluşturma ve bir SAS Blob storage'ı kullanma](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-2/), [paylaşılan kullanarak hesabınızda nesnelere erişimi nasıl Erişim imzalar ve saklı erişim ilkeleri](https://azure.microsoft.com/documentation/articles/storage-security-guide/#_how-to-delegate-access-to-objects-in-your-account-using-shared-access-signatures-and-stored-access-policies) |
| **Adımları** | <p>Paylaşılan erişim imzası (SAS) hesap erişim tuşu kullanıma gerek kalmadan diğer istemcilere bir depolama hesabındaki nesnelere sınırlı erişim vermek için güçlü bir yöntemdir kullanmaktır. Kendi sorgu parametrelerini kapsayan bir URI SAS olduğu depolama kaynağa erişim için gereken bilgilerin tümünü kimlik doğrulaması. SAS ile depolama kaynaklarına erişmek için istemcinin yalnızca uygun Oluşturucusu veya yöntem SAS geçirmek gerekir.</p><p>Hesap anahtarı ile güvenilir olmayan bir istemci için depolama hesabınızdaki kaynaklara erişimi sağlamak istediğinizde bir SAS kullanabilirsiniz. Depolama hesabı anahtarlarını hem her ikisi de hesabınızı ve tüm kaynaklar için yönetici erişimi vermek birincil ve ikincil bir anahtar içerir. Hesap anahtarlarınızı birini gösterme kötü amaçlı veya ihmalkar kullanma olasılığını hesabınıza açar. Paylaşılan erişim imzaları okuma, yazma ve izinler göre ve hesap anahtarı için gerek kalmadan depolama hesabınızdaki veri silmek diğer istemcilerin güvenli bir alternatif sunar.</p><p>Bir mantıksal parametrelerinin her zaman benzer varsa, depolanan erişim ilkesi (SAP) kullanarak daha iyi bir fikirdir. Depolanan bir erişim ilkesi tarafından türetilmiş SAS kullanarak bu SAS hemen iptal etme olanağını verdiğinden, bu her zaman mümkün olduğunda depolanan erişim ilkelerini kullanmak için önerilen en iyi uygulamadır.</p>|