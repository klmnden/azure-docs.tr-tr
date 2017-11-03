---
title: "Azure çok faktörlü kimlik doğrulaması ile ilgili SSS | Microsoft Docs"
description: "Sık sorulan sorular ve yanıtlar Azure çok faktörlü kimlik doğrulaması ile ilgili. Çok faktörlü kimlik doğrulaması birden çok kullanıcı adı ve parola gerektiren bir kullanıcının kimlik doğrulama yöntemidir. Ek bir kullanıcı oturum açma ve işlemler için güvenlik katmanı sağlar."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: 50bb8ac3-5559-4d8b-a96a-799a74978b14
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: kgremban
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 023dee623ca6ec35ab77578c97e5bf197b4bfe75
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="frequently-asked-questions-about-azure-multi-factor-authentication"></a>Azure çok faktörlü kimlik doğrulaması hakkında sık sorulan sorular
Bu SSS, Azure multi-Factor Authentication ve çok faktörlü kimlik doğrulama hizmeti kullanma hakkında sık sorulan soruları yanıtlar. Bu hizmet hakkında sorular içine genel olarak, modelleri, kullanıcı deneyimleri, faturalama ve sorun giderme ayrılmıştır.

## <a name="general"></a>Genel
**S: Azure multi-Factor Authentication sunucusu kullanıcı verileri nasıl işler?**

Multi-Factor Authentication sunucusu ile kullanıcı verileri yalnızca şirket içi sunucularda depolanır. Kalıcı kullanıcı verileri bulutta depolanmaz. Kullanıcı iki aşamalı doğrulama gerçekleştirdiğinde, multi-Factor Authentication sunucusu kimlik doğrulaması için Azure multi-Factor Authentication bulut hizmeti verileri gönderir. Çok faktörlü kimlik doğrulama sunucusu ve çok faktörlü kimlik doğrulama bulut hizmeti arasındaki iletişim 443 giden bağlantı noktası üzerinden Güvenli Yuva Katmanı (SSL) veya Aktarım Katmanı Güvenliği (TLS) kullanır.

Ne kimlik doğrulama isteklerini gönderileceğini bulut hizmeti için kimlik doğrulama ve kullanımı için toplanan veriler bildirir. İki aşamalı doğrulama günlüklerinde dahil veri alanları aşağıdaki gibidir:

* **Benzersiz kimliği** (ya da kullanıcı adı veya şirket içi çok faktörlü kimlik doğrulama sunucusu kimliği)
* **İlk ve son adı** (isteğe bağlı)
* **E-posta adresi** (isteğe bağlı)
* **Telefon numarası** (sesli arama veya SMS kimlik doğrulaması kullanırken)
* **Cihaz belirteci** (mobil uygulama kimlik doğrulaması kullanırken)
* **Kimlik doğrulama modu**
* **Kimlik doğrulaması sonucu**
* **Çok faktörlü kimlik doğrulama sunucu adı**
* **Çok faktörlü kimlik doğrulama sunucusu IP**
* **İstemci IP** (varsa)

İsteğe bağlı alanları multi-Factor Authentication Sunucusu'nda yapılandırılabilir.

Kimlik doğrulama verileriyle doğrulama sonucu (başarılı veya engelleme) ve reddedildiyse, nedenini depolanır. Bu veriler, kimlik doğrulama ve kullanım raporlarında kullanılabilir.

## <a name="billing"></a>Faturalandırma
Ya da başvurarak çoğu fatura soruların yanıtlanması [çok faktörlü kimlik doğrulaması Fiyatlandırma sayfasında](https://azure.microsoft.com/pricing/details/multi-factor-authentication/) veya ilgili belgelere [Azure çok faktörlü kimlik doğrulama alma](multi-factor-authentication-versions-plans.md).

**S: telefon aramaları yapmak ve kimlik doğrulaması için kullanılan metin iletileri göndermek için sizden ücret Kuruluşum mi?**

Hayır, size yerleştirilen telefon çağrıları tek tek veya metin için sizden ücret istenmese Azure çok faktörlü kimlik doğrulaması aracılığıyla kullanıcılara gönderilen iletiler. Bir kimlik doğrulaması başına MFA sağlayıcısı kullanırsanız, her kimlik doğrulama ancak kullanılan yöntem değil faturalandırılır.

Kullanıcılarınızın telefon aramaları veya, kendi kişisel telefon hizmetine göre aldıkları metin iletileri için sizden ücret.

**S: kullanıcı başına fatura modelini bana tüm etkin kullanıcıları veya yalnızca iki aşamalı doğrulamayı gerçekleştirilen olanlar için ücretli mi?**

Faturalama iki aşamalı doğrulamayı o ay gerçekleştirilip olmadığını bağımsız olarak çok faktörlü kimlik doğrulaması kullanmak üzere yapılandırılmış kullanıcı sayısını temel alır.

**S: çok faktörlü kimlik doğrulaması faturalama nasıl çalışır?**

Kullanıcı başına veya kimlik doğrulaması başına MFA sağlayıcısı oluşturduğunuzda, kuruluşunuzun Azure aboneliği aylık kullanıma dayalı faturalandırılır. Bu fatura modelini, sanal makinelerin ve Web sitelerinde kullanım için nasıl Azure faturaları benzerdir.

Azure çok faktörlü kimlik doğrulaması için bir abonelik satın aldığınızda, kuruluşunuzun her kullanıcı için yalnızca yıllık lisans ücreti ödenen. MFA lisansları ve Office 365, Azure AD Premium veya Enterprise Mobility + güvenlik paketleri bu şekilde faturalandırılır. 

Seçenekleriniz hakkında daha fazla bilgi [Azure çok faktörlü kimlik doğrulama alma](multi-factor-authentication-versions-plans.md).

**S: Azure multi-Factor Authentication ücretsiz sürümü var mı?**

Bazı durumlarda, Evet.

Azure yöneticileri için çok faktörlü kimlik doğrulama erişim Azure ve Office 365 Yönetici portalı dahil olmak üzere Microsoft Çevrimiçi Hizmetler için herhangi bir ücret ödemeden Azure MFA özelliklerinin bir alt kümesi sunar. Bu teklif yalnızca genel Yöneticiler MFA lisans, bir paket veya tek başına tüketim tabanlı sağlayıcı aracılığıyla Azure MFA'ın tam sürümünü sahip değilseniz Azure Active Directory örnekleri için geçerlidir. Yöneticilerinizin ücretsiz sürümünü kullanıyorsanız ve Azure MFA tam sürümünü satın almak, ardından tüm genel Yöneticiler Ücretli sürüme otomatik olarak yükseltilir.

Office 365 kullanıcıları için multi-Factor Authentication Exchange Online ve SharePoint Online gibi Office 365 hizmetlerine erişim için herhangi bir ücret ödemeden Azure MFA özelliklerinin bir alt kümesi sunar. Bu teklif, Azure Active Directory karşılık gelen örneği MFA lisans, bir paket veya tek başına tüketim tabanlı sağlayıcı aracılığıyla Azure MFA'ın tam sürümünü sahip olmadığında, atanan bir Office 365 lisansına sahip kullanıcılar için geçerlidir.

**S: Kuruluşum, kullanıcı başına ve kimlik doğrulaması başına tüketim faturalama modelleri arasında herhangi bir zamanda geçiş yapabilirim?**

Tüketim tabanlı faturalama ile tek başına bir hizmet olarak MFA, kuruluşunuzun satın aldıysa, MFA sağlayıcısı oluşturduğunuzda faturalama modelini seçin. MFA sağlayıcısı oluşturulduktan sonra Fatura modelini değiştiremezsiniz. Ancak, MFA sağlayıcısı silin ve farklı bir fatura model biriyle oluşturun.

MFA sağlayıcısı oluşturulduğunda, bir Azure Active Directory (diğer adıyla "Azure AD kiracısı") bağlanabilir. Geçerli MFA sağlayıcısı için Azure AD kiracısı bağlıysa, güvenli bir şekilde MFA sağlayıcısını silmek ve aynı Azure AD kiracısı bağlantılı bir tane oluşturun. Alternatif olarak, MFA için etkinleştirilen tüm kullanıcıları kapsayacak sayıda MFA, Azure AD Premium veya Enterprise Mobility + Security (EMS) lisansı satın aldıysanız, MFA sağlayıcısını tamamen silebilirsiniz.

MFA sağlayıcınızı ise *değil* Azure AD kiracısı için bağlı olan veya yeni MFA sağlayıcısı bağlamak için farklı bir Azure AD Kiracı, kullanıcı ayarlarını ve yapılandırma seçenekleri aktarılmaz. Ayrıca, yeni MFA Sağlayıcısı ile oluşturulan etkinleştirme kimlik bilgileri kullanılarak mevcut Azure MFA Sunucularının yeniden etkinleştirilmesi gerekir. MFA Sunucularını yeni MFA Sağlayıcısına bağlamak için yeniden etkinleştirmek, telefon çağrısı ve kısa mesaj kimlik doğrulamasını etkilemez, ancak mobil uygulama etkinleştirilinceye kadar tüm kullanıcılar için mobil uygulama bildirimleri çalışmaz.

MFA sağlayıcıları hakkında daha fazla bilgi [Azure multi-Factor Auth sağlayıcısını kullanmaya başlama](multi-factor-authentication-get-started-auth-provider.md).

**S: Kuruluşum, herhangi bir zamanda tüketim tabanlı faturalama ve abonelik (lisans tabanlı modeli) arasında geçiş yapabilirim?**

Bazı durumlarda, Evet.

Dizininizde varsa bir *kullanıcı başına* Azure çok faktörlü kimlik doğrulama sağlayıcısı olarak MFA lisanslar ekleyebilirsiniz. Lisansına sahip olan kullanıcılar olmayan sayılır kullanıcı başına tüketim tabanlı faturalama içinde. Lisanssız kullanıcılar MFA sağlayıcısı üzerinden hala MFA için etkinleştirilebilir. Satın alma ve çok faktörlü kimlik doğrulaması kullanmak üzere yapılandırılmış tüm kullanıcıları için lisans atama Azure çok faktörlü kimlik doğrulama sağlayıcısı silebilirsiniz. Daha fazla kullanıcı lisansı sayısından gelecekte varsa, her zaman başka bir kullanıcı başına MFA sağlayıcısını oluşturabilirsiniz.

Dizininizde varsa bir *kimlik doğrulaması başına* Azure çok faktörlü kimlik doğrulama sağlayıcısı olarak MFA sağlayıcısı aboneliğinize bağlı olduğu sürece her zaman her kimlik doğrulama için faturalandırılır. Kullanıcılara MFA lisansları atayabilirsiniz, ancak bunu birinden bir MFA lisansı veya atanmış gelmediğini, hala her iki aşamalı doğrulama isteği için faturalandırılırsınız.

**S: kuruluşumun kullanın ve Azure çok faktörlü kimlik doğrulaması kullanacak şekilde kimlikleri eşitlemek sahip?**

Kuruluşunuz tüketim tabanlı faturalama modeli kullanıyorsa, Azure Active Directory isteğe bağlıdır, ancak gerekli değildir. Azure AD kiracısı için MFA sağlayıcınızı bağlı değilse, yalnızca Azure multi-Factor Authentication sunucusu veya Azure multi-Factor Authentication SDK'sı şirket içi dağıtabilirsiniz.

Lisansları satın almak ve bunları dizininde kullanıcılara atamak için Azure AD kiracısı eklendiğinden azure Active Directory için lisans modeli gereklidir.

## <a name="manage-and-support-user-accounts"></a>Yönetmek ve kullanıcı hesaplarını destekler

**S: Kullanıcılarım telefonlarını yanıtta almadığınız veya onlarla telefon yok mu yoksa ne söylemeliyim?**

Neyse tüm kullanıcılarınız birden fazla doğrulama yöntemi yapılandırılmış. Kullanıcılarınıza oturum açma sayfasında farklı bir doğrulama yöntemini seçerek yeniden oturum açmayı denemesini söyleyin.

Kullanıcılarınıza gösterebilir [son kullanıcı sorun giderme kılavuzu](./end-user/multi-factor-authentication-end-user-troubleshoot.md).


**S: Kullanıcılarım birini hesaplarında alamazsa ne yapmalıyım?**

Kullanıcılara kayıt işleminde size yeniden gitmesini yaparak kullanıcı hesabının sıfırlayabilirsiniz. Daha fazla bilgi edinmek [bulutta Azure multi Factor Authentication ile kullanıcı ve cihaz ayarlarını yönetme](multi-factor-authentication-manage-users-and-devices.md).

**S: Kullanıcılarım birini uygulama parolaları kullanarak bir telefon kaybederse ne yapmalıyım?**

Yetkisiz erişimi önlemek için kullanıcının tüm uygulama parolalarını Sil. Kullanıcı değiştirme aygıt sahip olduktan sonra parolaları yeniden oluşturabilirsiniz. Daha fazla bilgi edinmek [bulutta Azure multi Factor Authentication ile kullanıcı ve cihaz ayarlarını yönetme](multi-factor-authentication-manage-users-and-devices.md).

**S: Peki bir kullanıcı için tarayıcı olmayan uygulamalara oturum açamıyorsunuz?**

Kuruluşunuz eski istemcileri ve hala kullanıyorsa [uygulama parolaları kullanılmasına izin](multi-factor-authentication-whats-next.md#app-passwords), sonra da kullanıcılarınızın bu eski istemciler kullanıcı adı ve parola ile oturum açılamıyor. Bunun yerine, için gereksinim duydukları [uygulama parolaları ayarlamanız](./end-user/multi-factor-authentication-end-user-app-passwords.md). Kullanıcılarınızın (silme) temizlemeniz gerekir, oturum açma bilgilerini uygulamayı yeniden başlatın ve kendi kullanıcı adıyla oturum ve *uygulama parolası* normal parolalarını yerine.

Kuruluşunuz eski istemciler yoksa, kullanıcılarınızın uygulama parolaları oluşturmasına izin.

> [!NOTE]
> Office 2013 istemcilerin için modern kimlik doğrulaması
>
> Uygulama parolaları yalnızca modern kimlik doğrulaması desteklemeyen uygulamalar için gereklidir. Office 2013 istemcileri modern kimlik doğrulama protokollerini destekler, ancak yapılandırılması gerekir. Yeni Office istemcileri otomatik olarak modern kimlik doğrulama protokollerini destekler. Daha fazla bilgi için bkz: [Office 2013 modern kimlik doğrulaması genel önizlemesi duyuru](https://blogs.office.com/2015/03/23/office-2013-modern-authentication-public-preview-announced/).

**S: kullanıcılar bazen metin iletisi almadığınız veya çift yönlü metin iletileri Yanıtla ancak doğrulama zaman aşımına uğruyor söyleyin.**

Metin iletileri teslimini ve iki yönlü SMS yanıtlar alınmasını garanti edilmez çünkü hizmet güvenilirliğini etkileyebilecek denetlenemeyen Etkenler vardır. Bu etkenler hedef ülke, cep telefonu taşıyıcı ve sinyal gücü içerir.

Kullanıcılarınızın sık güvenilir bir şekilde metin iletileri alma sorunları varsa, mobil uygulama veya telefon görüşmesi yöntemi kullanmayı söyleyin. Mobil uygulama hem cep ve Wi-Fi bağlantıları üzerinden bildirimleri alabilirsiniz. Ayrıca, cihaz sinyali yok hiç olsa bile mobil uygulama doğrulama kodları oluşturabilirsiniz. Microsoft Authenticator uygulaması [Android](http://go.microsoft.com/fwlink/?Linkid=825072), [IOS](http://go.microsoft.com/fwlink/?Linkid=825073), ve [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071).

Metin iletileri kullanmanız gerekiyorsa, iki yönlü SMS mümkün olduğunda yerine tek yönlü SMS kullanmanızı öneririz. Tek yönlü SMS daha güvenilirdir ve kullanıcıların başka bir ülkeden gönderildiği bir kısa mesaj yanıtlarken genel SMS ücretleri doğurmasını engeller.

**S: Kullanıcılarım sistem zaman aşımına uğramadan önce bir metin iletisi doğrulama kodunu girmek zorunda süre miktarını değiştirmek?**

Bazı durumlarda, Evet. 

Tek yönlü SMS için Azure MFA sunucusu v7.0 veya sonrası, zaman aşımı bir kayıt defteri anahtarı ayarını yapılandırabilirsiniz. SMS mesajı MFA bulut hizmeti gönderdikten sonra MFA sunucusu doğrulama kodu (veya bir kerelik geçiş kodu) döndürülür. MFA sunucusu varsayılan olarak 300 saniye bellekte kodunu depolar. 300 saniye geçtikten önce kullanıcı kodu girmezse kendi kimlik doğrulaması reddedildi. Varsayılan zaman aşımı ayarını değiştirmek için aşağıdaki adımları kullanın:

1. HKLM\Software\Wow6432Node\Positive Networks\PhoneFactor gidin.
2. Adlı bir DWORD kayıt defteri anahtarı oluşturun **pfsvc_pendingSmsTimeoutSeconds** ve tek seferlik parolaları depolamak için Azure MFA sunucusu istediğiniz saniye cinsinden süreyi ayarlayın.

>[!TIP] 
>Birden çok MFA sunucusu varsa, özgün kimlik doğrulama isteği işleyen bir kullanıcıya gönderilen doğrulama kodunu bilir. Kullanıcı kodu girdiğinde, doğrulamak için kimlik doğrulama isteği aynı sunucusuna gönderilmesi gerekir. Kod doğrulama farklı bir sunucuya gönderilir, kimlik doğrulaması reddedilir. 

Azure MFA sunucusu ile iki yönlü SMS için MFA Yönetim Portalı'nda zaman aşımı ayarını yapılandırabilirsiniz. Kullanıcılar için SMS tanımlı zaman aşımı süresi içinde yanıt vermezseniz kendi kimlik doğrulaması reddedildi. 

(AD FS bağdaştırıcısı ve ağ ilkesi sunucusu uzantısı dahil) bulutta Azure MFA ile tek yönlü SMS için zaman aşımı ayarını yapılandıramaz. Azure AD 180 saniye doğrulama kodunu depolar. 

**S: Azure multi-Factor Authentication sunucusu ile donanım belirteçleri kullanın?**

Azure multi-Factor Authentication sunucusu kullanıyorsanız, üçüncü taraf açık kimlik doğrulama (OATH) zamana dayalı, bir kerelik parola (TOTP) belirteçleri almak ve bunları iki aşamalı doğrulama için kullanın.

Bir CSV dosyasında gizli anahtarı yerleştirin ve Azure multi-Factor Authentication Sunucusu'na alın, OATH TOTP belirteçleri ActiveIdentity belirteçler kullanabilirsiniz. İstemci sistem, kullanıcı girişi kabul sürece OATH belirteçleri Active Directory Federasyon Hizmetleri (ADFS), Internet Information Server (IIS) Form tabanlı kimlik doğrulama ve Uzaktan Kimlik Doğrulama Çevirmeli Kullanıcı Hizmeti (RADIUS) kullanabilirsiniz.

Üçüncü taraf OATH TOTP belirteçleri aşağıdaki biçimlerde içe aktarabilirsiniz:  

- Taşınabilir simetrik anahtar kapsayıcısı (PSKC)  
- Bir seri numarası, gizli bir anahtar temel 32 biçiminde ve bir zaman aralığı dosya içeriyorsa, CSV  

**S: Azure multi-Factor Authentication sunucusu Terminal Hizmetleri güvenli hale getirmek için kullanabilir miyim?**

Windows Server 2012 R2 kullanıyorsanız veya daha sonra yalnızca Terminal Hizmetleri Uzak Masaüstü Ağ Geçidi (RD Ağ Geçidi) kullanarak güvenliğini sağlayabilirsiniz ancak Evet.

Windows Server 2012 R2'deki güvenlik değişiklikleri nasıl Azure çok faktörlü kimlik doğrulama sunucusu yerel güvenlik yetkilisi (LSA) güvenlik paketi, Windows Server 2012 ve önceki sürümlerinde bağlandığı değiştirildi. Terminal Hizmetleri Windows Server 2012 veya önceki sürümleri için şunları yapabilirsiniz [uygulama Windows kimlik doğrulaması ile güvenli](multi-factor-authentication-get-started-server-windows.md#to-secure-an-application-with-windows-authentication-use-the-following-procedure). Windows Server 2012 R2 kullanıyorsanız, RD Ağ Geçidi gerekir.

**S: arayan kimliği MFA sunucusunda yapılandırılmış, ancak kullanıcılar multi-Factor Authentication aramalarını anonim bir çağrıyı yapandan almaya devam.**

Bazen multi-Factor Authentication aramalarını ortak telefon ağına yerleştirildiğinde, bunlar arayan kimliği desteği olmayan bir taşıyıcı yönlendirilir Çok faktörlü kimlik doğrulama sistemi her zaman, gönderdiği olsa bile bu nedenle, arayan kimliği, garanti edilmez.

**S: neden Kullanıcılarım güvenlik bilgilerini kaydetmek için istenmesini?**
Kullanıcılar kendi güvenlik bilgilerini kaydetmek için sorulması birkaç nedeni vardır:

- Kullanıcı kendi yönetici tarafından Azure AD'de MFA için etkinleştirildi, ancak kendi hesabı için henüz kayıtlı güvenlik bilgileri yok.
- Kullanıcı, Self Servis parola sıfırlama Azure AD'de etkinleştirildi. Güvenlik bilgileri bunları bunlar herhangi bir zamanda unutursanız parolasını gelecekte sıfırlamayı yardımcı olur.
- Kullanıcı MFA gerektirecek şekilde bir koşullu erişim ilkesi vardır ve MFA için daha önce kaydedilmiş kurmadı uygulamanın erişilir.
- Kullanıcı (Azure AD katılım dahil), Azure AD ile bir cihaz kaydetme ve kuruluşunuzun cihaz kaydı için MFA gerekir, ancak kullanıcı daha önce MFA'ya kayıtlı değil.
- Kullanıcının iş için Windows Hello Windows 10'da (MFA gerektiren) oluşturuyor ve MFA için daha önce kaydedilen kurmadı.
- Kuruluş, oluşturulan ve kullanıcıya uygulanan bir MFA kayıt ilkesi etkinleştirilmiş.
- Kullanıcı daha önce MFA'ya kayıtlı ancak yönetici beri devre dışı olan bir doğrulama yöntemi seçin. Kullanıcı, bu nedenle yeniden yeni bir varsayılan doğrulama yöntemi seçmek için MFA kaydından gitmeniz gerekir.


## <a name="errors"></a>Hatalar
**S: Bunlar mobil uygulamasının bildirimleri kullanırken, "kimlik doğrulama isteği etkinleştirilmiş bir hesap için değil" hata iletisi görürseniz kullanıcılar ne?**

Mobil uygulama hesabını kaldırmak için bu yordamı izlemeden söyleyin ve ardından tekrar ekleyin:

1. Git [Azure portal profilinizi](https://account.activedirectory.windowsazure.com/profile/) ve kuruluş hesabınızla oturum açın.
2. Seçin **ek güvenlik doğrulaması**.
3. Mobil uygulamadaki mevcut hesabı kaldırın.
4. Tıklatın **yapılandırma**ve ardından mobil uygulamayı yapılandırmak için yönergeleri izleyin.

**S: Bunlar bir tarayıcı olmayan uygulamaya oturum açarken 0x800434D4L hata iletisi görürseniz kullanıcılar ne?**

İki aşamalı doğrulama gerektiren bir hesap ile işe yaramazsa yerel bir bilgisayarda, yüklenmiş bir tarayıcı olmayan uygulamanız oturum açmaya çalıştığınızda 0x800434D4L hata oluşur.

Bu hata ayrı kullanıcınız için geçici bir çözüm için yönetici ile ilgili hesapları ve yönetici olmayan işlemler. Daha sonra böylece yönetici olmayan hesabınızı kullanarak Outlook'a oturum yönetici hesabı ve yönetici olmayan hesapta arasında posta kutularını bağlayabilirsiniz. Bu çözüm hakkında daha fazla ayrıntı için bilgi nasıl [bir yönetici açın ve bir kullanıcının posta içeriğini görüntüleme olanağı verir](http://help.outlook.com/141/gg709759.aspx?sl=1).

## <a name="next-steps"></a>Sonraki adımlar
Lütfen sorunuzu burada cevaplanıp değil, sayfanın sonundaki açıklamalarında bırakın. Ya da Yardım almak için bazı ek seçenekleri şunlardır:

* Arama [Microsoft destek bilgi bankasına](https://www.microsoft.com/en-us/Search/result.aspx?form=mssupport&q=phonefactor&form=mssupport) ortak teknik sorunların çözümleri.
* Arama ve teknik sorular ve yanıtlar topluluktan göz atın veya kendi soru [Azure Active Directory forumları](https://social.msdn.microsoft.com/Forums/azure/newthread?category=windowsazureplatform&forum=WindowsAzureAD&prof=required).
* Eski PhoneFactor müşteri iseniz ve sorularınız varsa ya da kullanım bir parola sıfırlama yardıma gereksinim [parola sıfırlama](mailto:phonefactorsupport@microsoft.com) bir destek servis talebi açmaya bağlantı.
* Aracılığıyla destek uzmanına başvurun [Azure çok faktörlü kimlik doğrulama sunucusu (PhoneFactor) Destek](https://support.microsoft.com/oas/default.aspx?prid=14947). Sorununuzu mümkün olduğunca hakkında kadar bilgi dahil ederseniz bize kurulurken yardımcı olur. Hata, özel hata kodu, belirli bir oturum kimliği ve hata gördüğünüz kullanıcının Kimliğini gördüğünüz yerin sayfası, sağladığınız bilgileri içerir.
