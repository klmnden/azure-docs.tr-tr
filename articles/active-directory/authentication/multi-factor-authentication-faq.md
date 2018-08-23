---
title: Azure çok faktörlü kimlik doğrulaması ile ilgili SSS | Microsoft Docs
description: Sık sorulan sorular ve yanıtlar için Azure multi-Factor Authentication ilgili.
services: multi-factor-authentication
ms.service: active-directory
ms.component: authentication
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: michmcla
ms.openlocfilehash: b6f1185a94f865578d9a6514fb6841f8811b2230
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2018
ms.locfileid: "42057145"
---
# <a name="frequently-asked-questions-about-azure-multi-factor-authentication"></a>Azure multi-Factor Authentication hakkında sık sorulan sorular

Bu SSS, Azure multi-Factor Authentication'ı ve multi-Factor Authentication hizmetini kullanma hakkında sık sorulan sorular yanıtlanmaktadır. Bu hizmet hakkında sorular içine genel olarak, faturalama modelleri, kullanıcı deneyimleri ve sorun giderme ayrılmıştır.

## <a name="general"></a>Genel

**S: nasıl Azure multi-Factor Authentication sunucusu kullanıcı verilerini işliyor?**

Multi-Factor Authentication sunucusu ile kullanıcı verileri yalnızca şirket içi sunucularda depolanır. Kalıcı kullanıcı verileri bulutta depolanmaz. Kullanıcı iki adımlı doğrulama gerçekleştirdiğinde, multi-Factor Authentication sunucusu kimlik doğrulaması için Azure multi-Factor Authentication bulut hizmetine veri gönderir. Multi-Factor Authentication sunucusu ile multi-Factor Authentication bulut hizmeti arasındaki iletişimi, 443 giden bağlantı noktası üzerinden Güvenli Yuva Katmanı (SSL) veya Aktarım Katmanı Güvenliği (TLS) kullanır.

Ne zaman kimlik doğrulama istekleri gönderilir bulut hizmetine kimlik doğrulama ve kullanım için toplanan verileri raporlar. İki aşamalı doğrulama günlüklerine dahil veri alanları aşağıdaki gibidir:

* **Benzersiz kimliği** (ya da kullanıcı adı veya şirket içi multi-Factor Authentication sunucusu kimliği)
* **İlk ve son adı** (isteğe bağlı)
* **E-posta adresi** (isteğe bağlı)
* **Telefon numarası** (sesli arama veya SMS kimlik doğrulaması kullanırken)
* **Cihaz belirteci** (mobil uygulama kimlik doğrulamasını kullanırken)
* **Kimlik doğrulama modu**
* **Kimlik doğrulaması sonucu**
* **Çok faktörlü kimlik doğrulama sunucusu adı**
* **Multi-Factor Authentication sunucusu IP**
* **İstemci IP** (varsa)

İsteğe bağlı alanları, multi-Factor Authentication Sunucusu'nda yapılandırılabilir.

Doğrulama sonucu (başarı veya reddetme) ve reddedildiyse, bu nedenle kimlik doğrulama verileriyle depolanır. Bu veriler, kimlik doğrulama ve kullanım raporlarında kullanılabilir.

**S: hangi SMS kısa kodları Kullanıcılarım için SMS mesajları göndermek için kullanılır?**

Amerika Birleşik Devletleri Microsoft aşağıdaki SMS kısa kodlarını kullanır:

   * 97671
   * 69829
   * 51789
   * 99399

Kanada Microsoft aşağıdaki SMS kısa kodlarını kullanır:

   * 759731 
   * 673801

Microsoft, tutarlı SMS veya sesli tabanlı çok faktörlü kimlik doğrulama istemi teslim aynı sayıda garanti etmez. Kullanıcılarımızın açısından Microsoft ekleyebilir veya SMS teslimat geliştirmek için rota ayarlamalar vermiyoruz kısa kodları dilediğiniz zaman kaldırabilirsiniz. Microsoft, Amerika Birleşik Devletleri ve Kanada yanı sıra ülkeler için kısa kodlarını desteklemiyor

## <a name="billing"></a>Faturalandırma

Çoğu faturalama soruları için başvurarak yanıtlanması gereken [multi-Factor Authentication Fiyatlandırma sayfasında](https://azure.microsoft.com/pricing/details/multi-factor-authentication/) veya ilgili belgelere [Azure multi-Factor Authentication'ı alma](concept-mfa-licensing.md).

**S: Kuruluşum telefon aramaları ve kimlik doğrulaması için kullanılan metin iletileri göndermek için ücretlendirilir mi?**

Hayır, yapılan bireysel telefon görüşmeleri veya kısa mesaj ücretlendirilmez kullanıcılara Azure multi-Factor Authentication aracılığıyla gönderilen iletiler. Kimlik doğrulaması başına MFA sağlayıcısı kullanıyorsanız, her kimlik doğrulaması için ancak kullanılan yöntem için faturalandırılırsınız.

Kullanıcılarınız için telefon aramalarını veya kısa mesaj, kendi kişisel telefon servis göre aldıkları ücret.

**S: kullanıcı başına faturalandırma modeli bana tüm etkin kullanıcılar ya da iki aşamalı doğrulama gerçekleştirilen olanlar için ücretli mu?**

Kullanıcı multi-Factor Authentication, iki aşamalı doğrulama söz konusu ay gerçekleştirilip olmadığını bağımsız olarak kullanmak üzere yapılandırılmış göre faturalandırılır.

**S: multi-Factor Authentication Faturalaması nasıl çalışır?**

Bir kullanıcı başına veya kimlik doğrulaması başına MFA sağlayıcısı oluştururken, kuruluşunuzun Azure aboneliği aylık kullanımınıza göre faturalandırılır. Bu faturalama modeli, sanal makineler ve Web siteleri kullanımı için nasıl Azure faturaları benzerdir.

Azure multi-Factor Authentication için bir abonelik satın aldığınızda, kuruluşunuz için her bir kullanıcı yalnızca yıllık lisans ücreti öder. MFA lisans ve Office 365, Azure AD Premium veya Enterprise Mobility + Security paketleri bu şekilde faturalandırılır. 

Kullanabileceğiniz seçenekler hakkında daha fazla bilgi [Azure multi-Factor Authentication'ı alma](concept-mfa-licensing.md).

**S: ücretsiz bir Azure multi-Factor Authentication sürümü var mı?**

Bazı durumlarda, Evet.

Azure yöneticileri için multi-Factor Authentication, Azure ve Office 365 Yönetici portalı dahil olmak üzere Microsoft çevrimiçi hizmetlerine erişim için hiçbir ücret ödemeden Azure mfa'yı özelliklerinin bir alt kümesi sunar. Bu teklif, yalnızca genel Yöneticiler, Azure MFA'ın tam sürümünü MFA lisans, bir paket veya bir tek başına kullanım tabanlı sağlayıcısı aracılığıyla sahip değilseniz Azure Active Directory örnekleri için geçerlidir. Ardından yöneticilerinize ücretsiz sürümü kullanan ve Azure MFA tam sürümünü satın almanız, tüm genel Yöneticiler Ücretli bir sürüme otomatik olarak yükseltilir.

Office 365 kullanıcıları için multi-Factor Authentication, Exchange Online ve SharePoint Online gibi Office 365 hizmetlerine erişim için ücret olmadan Azure mfa'yı özelliklerinin bir alt kümesi sunar. Bu teklif, ilgili Azure Active Directory örneğini Azure MFA'ın tam sürümünü MFA lisans, bir paket veya bir tek başına kullanım tabanlı sağlayıcısı aracılığıyla sahip olmadığında, atanan bir Office 365 lisansına sahip kullanıcılar için geçerlidir.

**Kuruluşum, herhangi bir zamanda, tüketim faturalandırma modelleri kullanıcı başına ve kimlik doğrulaması başına arasında geçiş miyim?**

Kuruluşunuz, kullanım tabanlı faturalandırma ile tek başına bir hizmet olarak MFA satın alıyorsa, MFA sağlayıcısı oluştururken faturalama modelini seçin. MFA sağlayıcısı oluşturulduktan sonra faturalandırma modeli değiştiremezsiniz. Ancak, MFA sağlayıcısını Sil ve ardından farklı bir faturalandırma modeliyle oluşturun.

MFA sağlayıcısı oluştururken bir Azure Active Directory (diğer adıyla "Azure AD kiracısı") bağlanabilir. Geçerli MFA sağlayıcısı bir Azure AD kiracısına bağlı ise, güvenli bir şekilde MFA sağlayıcısı silebilir ve aynı Azure AD kiracısına bağlı bir tane oluşturun. Alternatif olarak, MFA için etkinleştirilen tüm kullanıcıları kapsayacak sayıda MFA, Azure AD Premium veya Enterprise Mobility + Security (EMS) lisansı satın aldıysanız, MFA sağlayıcısını tamamen silebilirsiniz.

MFA sağlayıcınızı ise *değil* bir Azure AD kiracısına bağlı veya yeni MFA sağlayıcısına bağlamak için farklı bir Azure AD Kiracı, kullanıcı ayarlarını ve yapılandırma seçenekleri aktarılmaz. Ayrıca, yeni MFA Sağlayıcısı ile oluşturulan etkinleştirme kimlik bilgileri kullanılarak mevcut Azure MFA Sunucularının yeniden etkinleştirilmesi gerekir. MFA Sunucularını yeni MFA Sağlayıcısına bağlamak için yeniden etkinleştirmek, telefon çağrısı ve kısa mesaj kimlik doğrulamasını etkilemez, ancak mobil uygulama etkinleştirilinceye kadar tüm kullanıcılar için mobil uygulama bildirimleri çalışmaz.

MFA sağlayıcıları hakkında daha fazla bilgi [Azure multi-Factor Auth sağlayıcısını kullanmaya başlama](concept-mfa-authprovider.md).

**Kuruluşum kullanım tabanlı faturalandırma ve abonelik (tabanlı lisans modeli) arasında dilediğiniz zaman geçiş miyim?**

Bazı durumlarda, Evet.

Dizininiz varsa bir *kullanıcı başına* Azure multi-Factor Authentication sağlayıcısı MFA lisans ekleyebilirsiniz. Kullanıcılara lisanslarına sahip olmayan sayılması kullanıcı başına kullanım tabanlı faturalandırma. Lisansı olmayan kullanıcılar için mfa'yı MFA sağlayıcısı aracılığıyla yine de etkinleştirilebilir. Satın alma ve multi-Factor Authentication kullanmak üzere yapılandırılmış tüm kullanıcılarınız için lisans atama, Azure multi-Factor Authentication sağlayıcısı silebilirsiniz. Gelecekte daha fazla kullanıcı lisansı sayısından varsa, her zaman başka bir kullanıcı başına MFA sağlayıcısı oluşturabilirsiniz.

Dizininiz varsa bir *kimlik doğrulaması başına* Azure multi-Factor Authentication sağlayıcısı, MFA sağlayıcısını aboneliğinize bağlı olduğu sürece her zaman her kimlik doğrulama için faturalandırılırsınız. MFA lisansları kullanıcılara atayabilir, ancak bir MFA lisans atanmış birinden gelen olup olmadığını, yine de her iki aşamalı doğrulama isteği için faturalandırılırsınız.

**S: Kuruluşum kullanın ve Azure multi-Factor Authentication'ı kullanmak için kimlikleri eşitlemek sahip mu?**

Kuruluşunuz, kullanım tabanlı faturalandırma modeli kullanıyorsa, Azure Active Directory isteğe bağlı, ancak gerekli değildir. MFA sağlayıcınız bir Azure AD kiracısına bağlı değilse, yalnızca Azure multi-Factor Authentication sunucusu şirket içi dağıtabilirsiniz.

Azure Active Directory lisansları satın alıp dizininde yer alan kullanıcılar atamanız olduğunda Azure AD kiracısına eklendiğinden, lisans modeli için gereklidir.

## <a name="manage-and-support-user-accounts"></a>Yönetme ve kullanıcı hesaplarını destekler

**S: Bunlar telefonundan yanıt almaz veya bunlarla telefon numaraları yoksa yapmanız Kullanıcılarım ne söylemeliyim?**

Umarım tüm kullanıcılarınız birden fazla doğrulama yöntemi olarak yapılandırılmış. Kullanıcılarınıza oturum açma sayfasında farklı bir doğrulama yöntemini seçerek yeniden oturum açmayı denemesini söyleyin.

Uygulamanızı kullanıcılarınızın kullanımına işaret edebilir [son kullanıcı sorun giderme kılavuzu](../user-help/multi-factor-authentication-end-user-troubleshoot.md).

**S: kullanıcılarımdan biri, kullanıcının hesabına erişemiyorsanız ne yapmalıyım?**

Yeniden kayıt sürecinden Git hale getirerek, kullanıcının hesabı sıfırlayabilirsiniz. Daha fazla bilgi edinin [bulutta Azure multi Factor Authentication ile kullanıcı ve cihaz ayarlarını yönetme](howto-mfa-userdevicesettings.md).

**Q: kullanıcılarımdan biri, uygulama parolaları kullanarak bir telefon kaybederse ne yapmalıyım?**

Yetkisiz erişimi önlemek için kullanıcının tüm uygulama parolalarını Sil. Kullanıcı yeni bir cihaza sahip olduktan sonra parolaları yeniden oluşturabilirsiniz. Daha fazla bilgi edinin [bulutta Azure multi Factor Authentication ile kullanıcı ve cihaz ayarlarını yönetme](howto-mfa-userdevicesettings.md).

**S: bir kullanıcı için tarayıcı olmayan uygulamalara oturum açamıyorsunuz?**

Kuruluşunuz eski istemcileri ve yine de kullanıyorsa [uygulama parolaları kullanılmasına izin](howto-mfa-mfasettings.md#app-passwords), sonra da kullanıcılarınızın bu eski istemcilere kullanıcı adı ve parola ile oturum açamazsınız. Bunun yerine, için ihtiyaç duydukları [uygulama parolaları ayarlamanız](../user-help/multi-factor-authentication-end-user-app-passwords.md). Kullanıcılarınızın (Sil) temizlemeniz gerekir oturum açma bilgilerini, uygulamayı yeniden başlatın ve sonra kendi kullanıcı adıyla oturum ve *uygulama parolası* normal parolalarını yerine.

Kuruluşunuz eski istemciler yoksa, kullanıcılarınızın uygulama parolaları oluşturmasına izin.

> [!NOTE]
> Office 2013 istemcilerindeki için modern kimlik doğrulaması
>
> Uygulama parolaları yalnızca modern kimlik doğrulamayı desteklemeyen uygulamalar için gereklidir. Office 2013 istemcilerindeki modern kimlik doğrulama protokolleri destekler, ancak yapılandırılması gerekir. Yeni Office istemcileri otomatik olarak, modern kimlik doğrulama protokolleri de destekler. Daha fazla bilgi için [Office 2013 modern kimlik doğrulaması genel Önizleme Duyurusu](https://blogs.office.com/2015/03/23/office-2013-modern-authentication-public-preview-announced/).

**S: kullanıcılarımın bazen kısa mesajı almadığınız ya da bunlar için iki yönlü kısa mesaj yanıtlama ancak doğrulama zaman aşımına varsayalım.**

Metin iletileri teslim ve iki yönlü SMS yanıtlara giriş garanti edilmez, hizmet güvenilirliğini etkileyebilecek denetlenemeyen Etkenler olduğundan. Bu etkenler, hedef ülke, cep telefonu operatörü ve sinyal gücü içerir.

Kullanıcılarınız genellikle güvenilir bir şekilde kısa mesaj alma sorunları varsa, mobil uygulama ya da telefon aramasına yöntemi kullanmayı söyleyin. Mobil uygulama bildirimleri hem hücresel ve Wi-Fi bağlantıları üzerinden alabilir. Ayrıca, cihaz hiçbir sinyal hiç olsa bile mobil uygulama doğrulama kodları oluşturur. Microsoft Authenticator uygulamasını kullanılabilir [Android](http://go.microsoft.com/fwlink/?Linkid=825072), [IOS](http://go.microsoft.com/fwlink/?Linkid=825073), ve [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071).

Kısa mesaj kullanmanız gerekiyorsa, iki yönlü SMS mümkün olduğunda yerine tek yönlü SMS kullanmanızı öneririz. Tek yönlü SMS daha güvenilirdir ve kullanıcıların başka bir ülkeden gönderildiği bir mesaj yanıtlama genel SMS ücretleri doğurmasını engeller.

**Kullanıcılarımın sistem zaman aşımına uğramadan önce kısa mesaj doğrulama kodu girmeniz gerekir süre miktarını değiştirebilirim miyim?**

Bazı durumlarda, Evet. 

Tek yönlü SMS için Azure MFA sunucusu v7.0 veya daha üzeri sürümleri, zaman aşımı ayarını bir kayıt defteri anahtarı yapılandırabilirsiniz. SMS mesajı MFA bulut hizmetine gönderdikten sonra MFA sunucusuyla doğrulama kodu (veya bir kerelik geçiş kodu) döndürülür. MFA sunucusu varsayılan olarak 300 saniye bellekte kodunu depolar. 300 saniye geçtikten önce kullanıcı kodu girmezse kendi kimlik doğrulaması reddedildi. Varsayılan zaman aşımı ayarını değiştirmek için aşağıdaki adımları kullanın:

1. İçin HKLM\Software\Wow6432Node\Positive Networks\PhoneFactor gidin.
2. Adlı bir DWORD kayıt defteri anahtarı oluşturma **pfsvc_pendingSmsTimeoutSeconds** ve saniye cinsinden bir kerelik geçiş kodlarını depolamak için Azure MFA sunucusu istediğiniz zamanı ayarlayın.

>[!TIP] 
>Birden çok MFA sunucunuz varsa, özgün kimlik doğrulama isteği işleyen bir kullanıcıya gönderilen doğrulama kodu bilir. Bunu doğrulamak için kimlik doğrulama isteği, kullanıcı kodu girdiğinde, aynı sunucuya gönderilmelidir. Farklı bir sunucuya gönderilen kodu doğrulama, kimlik doğrulaması reddedilir. 

Azure MFA sunucusu ile iki yönlü SMS için MFA Yönetim Portalı'nda zaman aşımı ayarını yapılandırabilirsiniz. Kullanıcılar için SMS tanımlanan zaman aşımı süresi içinde yanıt yoksa, kendi kimlik doğrulaması reddedildi. 

(AD FS bağdaştırıcısı ve ağ ilkesi sunucusu uzantısı dahil) bulutta Azure MFA ile tek yönlü SMS için zaman aşımı ayarını yapılandıramazsınız. Azure AD, 180 saniye doğrulama kodunu depolar. 

**Donanım belirteçleri ile Azure multi-Factor Authentication Sunucusu'nu kullanabilir miyim?**

Azure multi-Factor Authentication sunucusu kullanıyorsanız, üçüncü taraf açık kimlik doğrulaması (OATH) zamana bağlı, bir kerelik parola (TOTP) belirteçleri içeri aktarabilir ve sonra da bunları iki aşamalı doğrulama için kullanın.

Azure multi-Factor Authentication sunucusu için bir CSV dosyasında gizli anahtar yerleştirin ve aktarırsanız TOTP OATH belirteçleri ActiveIdentity belirteçleri kullanabilirsiniz. İstemci sistem, kullanıcı girişi kabul sürece Active Directory Federasyon Hizmetleri (ADFS), Internet Information Server (IIS) Form tabanlı kimlik doğrulaması ve uzak kimlik denetimi içeri arama kullanıcı hizmeti (RADIUS) OATH belirteçleri kullanabilirsiniz.

Üçüncü taraf OATH TOTP belirteçleri ile şu biçimlerden içeri aktarabilirsiniz:  

- Taşınabilir simetrik anahtar kapsayıcısı (PSKC)  
- CSV dosyası seri numarası, gizli bir anahtar temel-32 biçiminde ve bir zaman aralığı içeriyorsa  

**S: Azure multi-Factor Authentication sunucusu Terminal hizmetlerinin güvenliğini sağlamak için kullanabilir miyim?**

Windows Server 2012 R2 kullanıyorsanız veya daha sonra yalnızca Terminal Hizmetleri Uzak Masaüstü Ağ Geçidi (RD Ağ Geçidi) kullanarak güvenliğini sağlayabilirsiniz ancak Evet.

Windows Server 2012 R2'deki güvenlik değişiklikleri, Azure multi-Factor Authentication sunucusu yerel güvenlik yetkilisi (LSA) güvenlik paketi Windows Server 2012 ve önceki sürümlerinde nasıl bağlanır değiştirildi. Terminal Hizmetleri Windows Server 2012 veya önceki sürümleri için aşağıdakileri yapabilirsiniz [güvenli bir uygulaması Windows kimlik doğrulaması ile](howto-mfaserver-windows.md#to-secure-an-application-with-windows-authentication-use-the-following-procedure). Windows Server 2012 R2 kullanıyorsanız, RD Ağ Geçidi gerekir.

**S: arayan kimliği MFA sunucusu yapılandırılmış, ancak kullanıcılar multi-Factor Authentication aramalarını anonim bir çağrıyı yapandan almaya devam.**

Bazen genel telefon ağ üzerinden multi-Factor Authentication aramalarını yerleştirildiğinde, bunlar arayan kimliği desteği olmayan bir taşıyıcı yönlendirilir Çok faktörlü kimlik doğrulama sistemi her zaman gönderdiği olsa bile bu nedenle, arayan kimliği, garanti edilmez.

**S: neden kullanıcılarımın güvenlik bilgilerini kaydetmek için uyarılmasını?**
Kullanıcıların güvenlik bilgilerini kaydetmek için görüntülenebilir birkaç nedeni vardır:

- Kullanıcı için mfa'yı Azure AD'de yönetici tarafından etkinleştirildi, ancak güvenlik bilgileri için hesabı henüz kayıtlı gerekli değildir.
- Kullanıcı Self Servis parola sıfırlama Azure AD'de için etkinleştirildi. Güvenlik bilgilerini bunları bunlar hiç olmadığı kadar unutursanız gelecekte kullanıcının parolasını sıfırlamasını yardımcı olur.
- Kullanıcı MFA gerektirmek için koşullu erişim ilkesi varsa ve daha önce MFA için kaydolduğunu edilmemiş bir uygulama erişilir.
- Kullanıcı (Azure AD Join dahil), Azure AD ile bir cihazın kaydolduğunu ve kuruluşunuzun cihaz kaydı için MFA gerekir, ancak kullanıcının daha önce MFA için kayıtlı değil.
- Kullanıcı Windows Hello iş için Windows 10 ' (MFA gerektiren) oluşturuyor ve daha önce MFA için kaydolduğunu edilmemiş.
- Kuruluş, oluşturulur ve kullanıcıya uygulanan bir MFA kayıt ilkesi etkin.
- Kullanıcı daha önce MFA için kaydolduğunu ancak yönetici beri devre dışı bir doğrulama yöntemi seçin. Kullanıcı MFA kaydından tekrar yeni bir varsayılan doğrulama yöntemi seçin. Bu nedenle gitmeniz gerekir.

## <a name="errors"></a>Hatalar

**S: kullanıcıların, mobil uygulama bildirimleri kullanırken "kimlik doğrulama isteği etkinleştirilmiş bir hesap için değil" hata iletisi görürseniz ne yapması gerekir?**

Mobil uygulama hesabını kaldırmak için bu yordamı izlemek için söyleyin ve sonra tekrar ekleyin:

1. Git [Azure portal profilinizi](https://account.activedirectory.windowsazure.com/profile/) ve kuruluş hesabınızla oturum açın.
2. Seçin **ek güvenlik doğrulaması**.
3. Mobil uygulamadaki mevcut hesabı kaldırın.
4. Tıklayın **yapılandırma**ve ardından mobil uygulamayı yapılandırmak için yönergeleri izleyin.

**S: kullanıcıların, bir tarayıcı içi uygulaması için oturum açarken 0x800434D4L hata iletisini görürseniz ne yapması gerekir?**

İki aşamalı doğrulama gerektiren bir hesap ile çalışmayan bir yerel bilgisayarda yüklü bir tarayıcı içi uygulaması oturum açmaya çalıştığınızda 0x800434D4L hata oluşur.

Bu hata ayrı kullanıcı için geçici bir çözüm için yönetici ile ilgili hesapları ve yönetici olmayan işlemler. Daha sonra yönetici olmayan bir hesap kullanarak Outlook'a oturum açabilirsiniz, böylece yönetici hesabı ve yönetici olmayan hesapta arasında posta kutularını bağlayabilirsiniz. Bu çözüm hakkında daha fazla ayrıntı için bilgi nasıl [bir yönetici açın ve bir kullanıcının posta kutusuna içeriğini görüntüleme olanağı sağlayacak](http://help.outlook.com/141/gg709759.aspx?sl=1).

## <a name="next-steps"></a>Sonraki adımlar

Lütfen Sorunuzu buraya yanıtlanmadıysa sayfanın alt kısmındaki açıklamalarda bırakın. Veya Yardım almak için bazı ek seçenekler şunlardır:

* Arama [Microsoft destek Bilgi Bankası](https://www.microsoft.com/Search/result.aspx?form=mssupport&q=phonefactor&form=mssupport) teknik sık karşılaşılan sorunlara çözümler için.
* İçin arama yapın ve teknik sorular ve cevaplar topluluğundan göz atın veya kendi soru [Azure Active Directory forumları](https://social.msdn.microsoft.com/Forums/azure/newthread?category=windowsazureplatform&forum=WindowsAzureAD&prof=required).
* Eski PhoneFactor müşteri iseniz ve sorularınız varsa veya kullanımı bir parola sıfırlama yardıma gereksinim [parola sıfırlama](mailto:phonefactorsupport@microsoft.com) bir destek olayı açmaya yönelik bağlantı.
* Üzerinden bir destek uzmanına başvurun [Azure multi-Factor Authentication sunucusu (PhoneFactor) Destek](https://support.microsoft.com/oas/default.aspx?prid=14947). Mümkün olduğunca sorununuzla ilgili kadar bilgi dahil ederseniz bizimle iletişime geçtiğiniz yararlı olur. Hata, söz konusu hata kodunu, belirli bir oturum kimliği ve hata gördüğünüz kullanıcı Kimliğini nerede gördüğünüzü sayfası, sağladığınız bilgiler içerir.
