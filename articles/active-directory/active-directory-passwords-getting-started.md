<properties
    pageTitle="Başlarken: Azure AD Parola Yönetimi | Microsoft Azure"
    description="Kullanıcıların kendi parolalarını sıfırlamasına, parola sıfırlama önkoşullarını öğrenmesine ve Active Directory'deki şirket içi parolaları yönetmek için Parola Geri Yazma özelliğini etkinleştirmesine olanak tanıyın."
    services="active-directory"
    keywords="Active directory parola yönetimi, parola yönetimi, Azure AD parolasını sıfırlama"
    documentationCenter=""
    authors="asteen"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="07/12/2016"
    ms.author="asteen"/>

# Parola Yönetimine Başlarken

> [AZURE.IMPORTANT] **Oturum açmada sorun yaşadığınız için mi buradasınız?** Sorun yaşıyorsanız bkz. [kendi parolanızı değiştirme ve sıfırlama](active-directory-passwords-update-your-own-password.md).

Kullanıcıların kendi bulut Azure Active Directory veya şirket içi Active Directory parolalarını yönetmesine olanak tanımak için birkaç basit adımı tamamlamak yeterlidir. Birkaç basit önkoşulu yerine getirdiğinizi doğrulamanızın ardından, kuruluşunuzun tamamı için parola değiştirme ve sıfırlama işlemleri çok kısa süre içinde etkinleştirilir. Bu makalede aşağıdaki kavramlarla ilgili olarak size yol gösterilecektir:

* [**Kullanıcılara bulut Azure Active Directory parolalarını sıfırlama olanağı tanıma**](#enable-users-to-reset-their-azure-ad-passwords)
 - [Self servis parola sıfırlama önkoşulları](#prerequisites)
 - [1. Adım: Parola sıfırlama ilkesini yapılandırma](#step-1-configure-password-reset-policy)
 - [2. Adım: Test kullanıcınız için iletişim verileri ekleme](#step-2-add-contact-data-for-your-test-user)
 - [3. Adım: Kullanıcı olarak parolanızı sıfırlama](#step-3-reset-your-azure-ad-password-as-a-user)
* [**Kullanıcılara şirket içi Azure Active Directory parolalarını sıfırlama veya değiştirme olanağı tanıma**](#enable-users-to-reset-or-change-their-ad-passwords)
 - [Parola Geri Yazma önkoşulları](#writeback-prerequisites)
 - [1. Adım: Azure AD Connect'in en son sürümünü indirme](#step-1-download-the-latest-version-of-azure-ad-connect)
 - [2. Adım: Kullanıcı arabirimi veya PowerShell ile Azure AD Connect'te Parola Geri Yazma'yı etkinleştirme ve doğrulama](#step-2-enable-password-writeback-in-azure-ad-connect)
 - [3. Adım: Güvenlik duvarınızı yapılandırma](#step-3-configure-your-firewall)
 - [4. Adım: İlgili izinleri ayarlama](#step-4-set-up-the-appropriate-active-directory-permissions)
 - [5. Adım: Kullanıcı olarak AD parolanızı sıfırlama ve doğrulama](#step-5-reset-your-ad-password-as-a-user)

## Kullanıcıların Azure AD parolalarını sıfırlamalarına olanak tanıma
Bu bölümde, AAD bulut dizininiz için self servis parola sıfırlamayı etkinleştirme, kullanıcıları self servis parola sıfırlama özelliğine kaydetme ve ardından son olarak, test amaçlı self servis parola sıfırlama işlemini kullanıcı olarak gerçekleştirme işlemlerinde size yol gösterilir.

- [Self servis parola sıfırlama önkoşulları](#prerequisites)
- [1. Adım: Parola sıfırlama ilkesini yapılandırma](#step-1-configure-password-reset-policy)
- [2. Adım: Test kullanıcınız için iletişim verileri ekleme](#step-2-add-contact-data-for-your-test-user)
- [3. Adım: Kullanıcı olarak parolanızı sıfırlama](#step-3-reset-your-azure-ad-password-as-a-user)


###  Önkoşullar
Self servis parola sıfırlamayı etkinleştirebilmek ve kullanabilmek için ilk olarak aşağıdaki önkoşulları yerine getirmeniz gerekir:

- Bir AAD kiracısı oluşturun. Daha fazla bilgi için bkz. [Azure AD ile Çalışmaya Başlama](https://azure.microsoft.com/trial/get-started-active-directory/)
- Bir Azure aboneliği edinin. Daha fazla bilgi için bkz. [Azure AD kiracısı nedir?](active-directory-administer.md#what-is-an-azure-ad-tenant).
- AAD kiracınızı Azure aboneliğinizle ilişkilendirin. Daha fazla bilgi için bkz. [Azure aboneliklerinin Azure AD ile ilişkisi](https://msdn.microsoft.com/library/azure/dn629581.aspx).
- Azure AD Premium veya Basic sürümüne yükseltin ya da ücretli O365 lisansı kullanın. Daha fazla bilgi için bkz. [Azure Active Directory Sürümleri](https://azure.microsoft.com/pricing/details/active-directory/).

  >[AZURE.NOTE] Bulut kullanıcıları için self servis parola sıfırlama özelliğini etkinleştirmek için Azure AD Premium, Azure AD Basic veya O365 ücretli lisansına yükseltmeniz gerekir.  Şirket içi kullanıcılarınız için self servis parola sıfırlama özelliğini etkinleştirmek için Azure AD Premium sürümüne yükseltmeniz gerekir. Daha fazla bilgi için bkz. [Azure Active Directory Sürümleri](https://azure.microsoft.com/pricing/details/active-directory/). Bu bilgiler, Azure AD Premium veya Basic sürümüne kaydolmaya, lisans planınızı ve Azure AD erişiminizi etkinleştirmeye, yönetici ve kullanıcı hesaplarına erişim atamaya yönelik ayrıntılı yönergeler içerir.

- AAD dizininizde en az bir yönetici hesabı ve bir kullanıcı hesabı oluşturun.
- Oluşturduğunuz yönetici ve kullanıcı hesabına bir AAD Premium, Basic veya O365 ücretli lisansı atayın.

### 1. Adım: Parola sıfırlama ilkesini yapılandırma
Kullanıcı parolası sıfırlama ilkesini yapılandırmak için aşağıdaki adımları tamamlayın:

1.  Tercih ettiğiniz tarayıcıyı açın ve [Azure Yönetim Portalı](https://manage.windowsazure.com)'na gidin.
2.  [Azure Yönetim Portalı](https://manage.windowsazure.com)'nda, sol taraftaki gezinti çubuğunda **Active Directory uzantısını** bulun.

    ![Azure AD'de Parola Yönetimi][001]

3. **Directory (Dizin)** sekmesinde, içinde kullanıcı parolası sıfırlama ilkesini yapılandırmak istediğiniz dizine (ör. Wingtip Toys) tıklayın.

    ![][002]

4.  **Configure (Yapılandır)** sekmesine tıklayın.

    ![][003]

5.  **Configure (Yapılandır)** sekmesinde, aşağı kaydırarak **user password reset policy (kullanıcı parolası sıfırlama ilkesi)** bölümüne gidin.  Burada, belirli bir dizin için kullanıcı parolası sıfırlama ilkesinin tüm özelliklerini yapılandırabilirsiniz.  

    >[AZURE.NOTE] Bu **ilke yöneticiler için değil, yalnızca kuruluşunuzdaki son kullanıcılar için geçerlidir**. Güvenlikle ilgili nedenlerle, yöneticilere yönelik parola sıfırlama ilkesini Microsoft denetler. Bu bölümü görmüyorsanız Azure Active Directory Premium veya Basic sürümüne kaydolduğunuzdan ve bu özelliği yapılandıran yönetici hesabına **lisans atadığınızdan** emin olun.

    ![][004]

6.  Kullanıcı parolası sıfırlama ilkesini yapılandırmak üzere **users enabled for password reset (kullanıcılar parola sıfırlayabilir)** seçeneğini **yes (evet)** ayarına kaydırın.  Bu işlem, bu özelliğin dizininizde çalışma biçimini yapılandırmanızı sağlayan birkaç ek denetimi görünür hale getirir.  Parola sıfırlama özelliğini uygun gördüğünüz şekilde özelleştirebilirsiniz.  Parola sıfırlama ilkesi denetimlerinden her birinin işlevi hakkında daha fazla bilgi edinmek istiyorsanız lütfen bkz. [Özelleştirme: Azure AD Parola Yönetimi](active-directory-passwords-customize.md).

    ![][005]

7.  Kiracınız için kullanıcı parolası sıfırlama ilkesini istediğiniz gibi yapılandırdıktan sonra, ekranın alt kısmındaki **Save (Kaydet)** düğmesine tıklayın.

  >[AZURE.NOTE] İşlevin en karmaşık durumda nasıl çalıştığını görebilmeniz için iki aşamalı kullanıcı parolası sıfırlama ilkesi önerilir.

  ![][006]

### 2. Adım: Test kullanıcınız için iletişim verileri ekleme
Kuruluşunuzdaki kullanıcılar için parola sıfırlamaya yönelik olarak kullanılacak verileri belirtmek üzere kullanabileceğiniz birkaç seçenek mevcuttur.

-   [Azure Yönetim Portalı](https://manage.windowsazure.com)'nda veya [Office 365 Yönetici Portalı](https://portal.microsoftonline.com)'nda kullanıcıları düzenleme
-   Şirket içi Active Directory etki alanındaki kullanıcı özelliklerini Azure AD ile eşitlemek için AAD Connect'i kullanma
-   Kullanıcı özelliklerini düzenlemek için Windows PowerShell'i kullanma
-   Kullanıcıları [http://aka.ms/ssprsetup](http://aka.ms/ssprsetup) adresindeki kayıt portalına yönlendirerek kullanıcıların kendi verilerini kaydetmelerine olanak tanıma
-   **Require users to register when signing in to the access panel (Kullanıcıların erişim panelinde oturum açarken kaydolmasını gerekli kıl)** SSPR yapılandırma seçeneğini **Yes (Evet)** olarak ayarlayarak, kullanıcıların [http://myapps.microsoft.com](http://myapps.microsoft.com) adresinden Erişim Paneli'nde oturum açtığında parola sıfırlama özelliğine kaydolmasını gerekli kılma.

Parola sıfırlama için hangi verilerin kullanıldığı ve bu verilere yönelik biçimlendirme gereksinimleri ile ilgili daha fazla bilgi edinmek istiyorsanız lütfen bkz. [Parola sıfırlama hangi verileri kullanır?](active-directory-passwords-learn-more.md#what-data-is-used-by-password-reset).

#### Kullanıcı Kayıt Portalı üzerinden kullanıcı kişi verilerini eklemek için
1.  Parola sıfırlama kayıt portalının kullanılması için kuruluşunuzdaki kullanıcılara bu sayfanın bağlantısını sağlamanız ([http://aka.ms/ssprsetup](http://aka.ms/ssprsetup)) veya kullanıcıların otomatik olarak kaydolmasını gerektiren seçeneği açmanız gerekir.  Kullanıcılar bu bağlantıya tıkladığında kuruluş hesaplarıyla oturum açmaları istenir.  Bunu yaptıktan sonra kullanıcılar şu sayfayı görür:

    ![][007]

2.  Burada, kullanıcılar cep telefonu numaralarını, alternatif e-posta adreslerini veya güvenlik sorularını sağlayabilir ve doğrulayabilir.  Cep telefonu numarası doğrulama işlemi aşağıdaki gibidir.

    ![][008]

3.  Bir kullanıcı bu bilgileri belirttikten sonra sayfa, bilgilerin geçerli olduğunu belirtecek şekilde güncelleştirilir (aşağıda karartılan kısımda olduğu gibi).  Kullanıcı, **Son** veya **İptal** düğmesine tıkladığında Erişim Paneli'ne yönlendirilir.

    ![][009]

4.  Kullanıcı bu bilgilerin her ikisini de doğruladıktan sonra profili, sağladığı verilerle güncelleştirilir.  Bu örnekte, **Ofis Telefonu** numarası el ile belirtilmiştir; bu nedenle kullanıcı, parolasını sıfırlamak için iletişim yöntemi olarak bunu kullanabilir.

    ![][010]

### 3. Adım: Kullanıcı olarak Azure AD parolanızı sıfırlama
Artık bir kullanıcı sıfırlama ilkesi yapılandırdığınıza ve kullanıcınız için kişi ayrıntılarını belirttiğinize göre bu kullanıcı, self servis parola sıfırlama işlemi gerçekleştirebilir.

#### Self servis parola sıfırlama işlemini gerçekleştirmek için
1.  [**portal.microsoftonline.com**](http://portal.microsoftonline.com) gibi bir siteye giderseniz aşağıdakine benzer bir oturum açma ekranı görürsünüz.  Parola sıfırlama kullanıcı arabirimini test etmek için **Hesabınıza erişemiyor musunuz?** bağlantısına tıklayın.

    ![][011]

2.  **Hesabınıza erişemiyor musunuz?** bağlantısına tıkladıktan sonra, parolasını sıfırlamak istediğiniz **kullanıcı kimliğini** isteyen yeni bir sayfaya yönlendirilirsiniz.  Test **kullanıcı kimliğinizi** buraya girin, captcha doğrulamasından geçin ve **sonraki** seçeneğine tıklayın.

    ![][012]

3.  Kullanıcı bu örnekte bir **ofis telefonu**, **cep telefonu** ve **alternatif e-posta** belirttiğinden, kullanıcıya ilk testten geçmesi için bu seçeneklerin tümünün sunulduğunu göreceksiniz.

    ![][013]

4.  Bu durumda, ilk olarak **ofis telefonunu** **aramayı** tercih edin.  Telefon tabanlı bir yöntem seçildiğinde kullanıcılardan parolalarını sıfırlayabilmeleri için ilk olarak **telefon numaralarını doğrulamaları** istenir.  Bunun amacı, kötü amaçlı kişilerin kuruluşunuzdaki kullanıcıların telefon numaralarını istenmeyen şekilde kullanmasını engellemektir.

    ![][014]

5.  Kullanıcı, telefon numarasını onayladıktan sonra, arama seçeneğine tıklanması bir değer değiştiricinin görünmesine ve kullanıcının telefonunun çalmasına neden olur.  Kullanıcı, aramanızı açtığında hesabını doğrulamak için **kullanıcının "#" tuşuna basması gerektiğini** belirten bir ileti oynatılır.  Bu tuşa basılması kullanıcının birinci testi geçtiğini otomatik olarak doğrular ve kullanıcı arabirimini ikinci doğrulama adımına geçirir.

    ![][015]

6.  İlk testi başarıyla geçmenizin ardından, kullanıcı arabirimi bunu kullanıcının sahip olduğu seçeneklerin listesinden kaldıracak şekilde otomatik olarak güncelleştirilir.  Bu durumda, ilk olarak **Ofis Telefonu** seçeneğinizi kullanmanız nedeniyle, yalnızca **Cep Telefonu** ve **Alternatif E-posta** seçenekleri ikinci doğrulama adımı için geriye geçerli seçenekler olarak kalır.  **Alternatif e-posta adresime gönder** seçeneğine tıklayın.  Bu işlemin ardından, e-posta seçeneğine basılması kayıtlı alternatif e-posta adresine e-posta gönderir.

    ![][016]

7.  Aşağıda, kullanıcıların göreceği e-postaya örnek verilmiştir. Kiracı markasına dikkat edin:

    ![][017]

8.  E-posta geldikten sonra sayfa güncelleştirilir ve e-postanın içerdiği doğrulama kodunu aşağıda gösterilen giriş kutusuna girmeniz mümkün olur.  İlgili kod girildikten sonra, sonraki düğmesi yanar ve ikinci doğrulama adımından geçebilirsiniz.

    ![][018]

9.  Kuruluş ilkesinin gereksinimlerini karşılamanızın ardından yeni bir parola seçmenize izin verilir.  Parola, AAD "güçlü" parola gereksinimlerini karşılaması temelinde doğrulanır (bkz. [Azure AD'de parola ilkesi](https://msdn.microsoft.com/library/azure/jj943764.aspx)) ve girilen parolanın bu ilkeyi karşılayıp karşılamadığının kullanıcı için belirtilmesine yönelik bir güç doğrulayıcı görünür.

    ![][019]

10. Kuruluş ilkesine uygun eşleşen parolalar sağladığınızda, parolanız sıfırlanır ve yeni parolanızla hemen oturum açabilirsiniz.

    ![][020]


## Kullanıcıların AD parolalarını sıfırlamasına veya değiştirmesine olanak tanıma

Bu bölümde, parolaların şirket içi Active Directory'ye geri yazılması amacıyla parola sıfırlama özelliğini yapılandırma konusunda size yol gösterilmektedir.

- [Parola Geri Yazma önkoşulları](#writeback-prerequisites)
- [1. Adım: Azure AD Connect'in en son sürümünü indirme](#step-1-download-the-latest-version-of-azure-ad-connect)
- [2. Adım: Kullanıcı arabirimi veya PowerShell ile Azure AD Connect'te Parola Geri Yazma'yı etkinleştirme ve doğrulama](#step-2-enable-password-writeback-in-azure-ad-connect)
- [3. Adım: Güvenlik duvarınızı yapılandırma](#step-3-configure-your-firewall)
- [4. Adım: İlgili izinleri ayarlama](#step-4-set-up-the-appropriate-active-directory-permissions)
- [5. Adım: Kullanıcı olarak AD parolanızı sıfırlama ve doğrulama](#step-5-reset-your-ad-password-as-a-user)


### Geri Yazma önkoşulları
Parola Geri Yazma özelliğini etkinleştirebilmek ve kullanabilmek için ilk olarak aşağıdaki önkoşulları yerine getirmeniz gerekir:

- Azure AD Premium'un etkin olduğu bir Azure AD kiracısına sahip olmanız gerekir.  Daha fazla bilgi için bkz. [Azure Active Directory Sürümleri](active-directory-editions.md).
- Parola sıfırlama, kiracınızda yapılandırılmış ve etkinleştirilmiştir.  Daha fazla bilgi için bkz. [Kullanıcıların Azure AD parolalarını sıfırlamasına olanak tanıma](#enable-users-to-reset-their-azure-ad-passwords)
- Bu özelliği test etmek için kullanabileceğiniz, Azure AD Premium lisansına sahip olan en az bir yönetici hesabına ve bir test kullanıcı hesabına sahip olmanız gerekir.  Daha fazla bilgi için bkz. [Azure Active Directory Sürümleri](active-directory-editions.md).

  > [AZURE.NOTE] Parola Geri Yazma özelliğini etkinleştirmek için kullandığınız yönetici hesabının bir bulut yönetici hesabı (Azure AD'de oluşturulmuş) olduğundan ve birleştirilmiş bir hesap (şirket içi AD'de oluşturulmuş ve Azure AD ile eşitlenmiş) olmadığından emin olun.

- En son hizmet paketleri yüklü şekilde Windows Server 2008, Windows Server 2008 R2, Windows Server 2012 veya Windows Server 2012 R2'yi çalıştıran tek veya çok ormanlı AD şirket içi dağıtımına sahip olmanız gerekir.

  > [AZURE.NOTE] Windows Server 2008 veya 2008 R2'nin daha eski bir sürümünü çalıştırıyorsanız bu özelliği kullanmaya devam edebilirsiniz ancak bulutta yerel AD parola ilkenizi zorunlu kılabilmeniz için ilk olarak [KB 2386717'yi indirip yüklemeniz](https://support.microsoft.com/kb/2386717) gerekir.

- Azure AD Connect aracı yüklü olmalı ve AD ortamınızı bulut ile eşitlemeye yönelik olarak hazırlamış olmanız gerekir.  Daha fazla bilgi için bkz. [Şirket içi kimlik altyapınızı bulutta kullanma](active-directory-aadconnect.md).

  > [AZURE.NOTE] Parola geri yazma özelliğini test etmeden önce ilk olarak Azure AD Connect'te hem AD'den hem de Azure AD'den tam içeri aktarma işlemini tamamladığınızdan emin olun.

- Azure AD Eşitleme veya Azure AD Connect kullanıyorsanız giden **TCP 443** (ve bazı durumlarda **TCP 9350-9354**) açık olmalıdır.  Daha fazla bilgi için bkz. [3. Adım: Güvenlik duvarınızı yapılandırma](#step-3-configure-your-firewall). Bu senaryo için DirSync kullanımı artık desteklenmemektedir.  DirSync kullanmaya devam ediyorsanız parola geri yazma özelliğini dağıtmadan önce lütfen Azure AD Connect'in en son sürümüne yükseltme yapın.

  > [AZURE.NOTE] Mümkün olan en iyi deneyimin ve en yeni özelliklerin sağlanması için, Azure AD Eşitleme veya DirSync araçlarını kullanan tüm kişilerin Azure AD Connect'in en son sürümüne yükseltme yapmasını öneririz.


### 1. Adım: Azure AD Connect'in en son sürümünü indirme
Parola Geri Yazma özelliği Azure AD Connect'in sürümlerinde veya sürüm numarası **1.0.0419.0911** veya üzeri olan Azure AD Eşitleme aracında kullanılabilir.  Otomatik hesap kilidi açma özelliğine sahip Parola Geri Yazma, Azure AD Connect'in sürümlerinde veya sürüm numarası **1.0.0485.0222** ya da üzeri olan Azure AD Eşitleme aracında kullanılabilir. Eski bir sürümü çalıştırıyorsanız lütfen devam etmeden önce en azından bu sürüme yükseltme yapın. [Azure AD Connect'in en son sürümünü indirmek için buraya tıklayın](active-directory-aadconnect.md#install-azure-ad-connect).

#### Azure AD Eşitleme'nin sürümünü denetleme
1.  **%ProgramFiles%\Azure Active Directory Sync\** konumuna gidin.
2.  **ConfigWizard.exe** yürütülebilir dosyasını bulun.
3.  Yürütülebilir dosyaya sağ tıklayıp bağlam menüsünden **Özellikler** seçeneğini belirleyin.
4.  **Ayrıntılar** sekmesine tıklayın.
5.  **Dosya sürümü** alanını bulun.

    ![][021]

Bu sürüm numarası **1.0.0419.0911** değerinden büyük veya bu değere eşitse ya da Azure AD Connect'i yüklüyorsanız [2. Adım: Kullanıcı arabirimi veya powershell ile Azure AD Connect'te Parola Geri Yazma'yı etkinleştirme ve doğrulama](#step-2-enable-password-writeback-in-azure-ad-connect) adımına geçebilirsiniz.

 > [AZURE.NOTE] Azure AD Connect aracını ilk kez yüklüyorsanız ortamınızı dizin eşitleme için hazırlamak üzere birkaç en iyi uygulamayı izlemeniz önerilir.  Azure AD Connect aracını yüklemeden önce [Office 365 Yönetici Portalı](https://portal.microsoftonline.com)'nda veya [Azure Yönetim Portalı](https://manage.windowsazure.com)'nda dizin eşitlemesini etkinleştirmeniz gerekir.  Daha fazla bilgi için bkz. [Azure AD Connect'i Yönetme](active-directory-aadconnect-whats-next.md).


### 2. Adım: Azure AD Connect'te Parola Geri Yazma'yı etkinleştirme
Azure AD Connect aracını indirdiğinize göre, artık Parola Geri Yazma özelliğini etkinleştirmek için hazırsınız.  Bunu iki yoldan birini kullanarak yapabilirsiniz.  Parola Geri Yazma özelliğini Azure AD Connect kurulum sihirbazının isteğe bağlı özellikler ekranında veya Windows PowerShell üzerinden etkinleştirebilirsiniz.

#### Yapılandırma sihirbazında Parola Geri Yazma özelliğini etkinleştirmek için
1.  **Dizin Eşitlemesi bilgisayarınızda**, **Azure AD Connect** yapılandırma sihirbazını açın.
2.  **İsteğe bağlı özellikler** yapılandırma ekranına erişene kadar adımlara tıklayarak geçin.
3.  **Parola geri yazma** seçeneğini işaretleyin.

    ![][022]

4.  Sihirbazı tamamlayın; son sayfada değişiklikler özetlenir ve Parola Geri Yazma yapılandırma değişikliği bulunur.

> [AZURE.NOTE] Bu sihirbazı yeniden çalıştırıp özelliğin seçimini kaldırarak veya [Azure Yönetim Portalı](https://manage.windowsazure.com)'nda dizininizin **Configure (Yapılandır)** sekmesinin **User Password Reset Policy (Kullanıcı Parolası Sıfırlama İlkesi)** bölümünde **Write Passwords Back to On-Premises Directory (Şirket İçi Dizine Parolaları Geri Yaz)** ayarını **No (Hayır)** olarak belirleyerek Parola Geri Yazma özelliğini dilediğiniz zaman devre dışı bırakabilirsiniz.  Parola sıfırlama deneyiminizi özelleştirme hakkında daha fazla bilgi için bkz. [Özelleştirin: Azure AD Parola Yönetimi](active-directory-passwords-customize.md).

#### Parola Geri Yazma özelliğini Windows PowerShell kullanarak etkinleştirmek için
1.  **Dizin Eşitlemesi bilgisayarınızda** yeni bir **yükseltilmiş Windows PowerShell penceresi** açın.
2.  Modül zaten yüklü değilse Azure AD Connect cmdlet'lerini geçerli oturumunuza yüklemek için `Import-Module ADSync` komutunu yazın.
3.  `Get-ADSyncConnector` cmdlet'ini çalıştırarak ve sonuçları şurada depolayarak AAD Bağlayıcıları'nın listesini alın: `$aadConnectorName`
4.  Aşağıdaki cmdlet'i çalıştırarak geçerli bağlayıcı için geri yazmanın geçerli durumunu almak üzere: `Get-ADSyncAADPasswordResetConfiguration –Connector $aadConnectorName`
5.  Parola Geri Yazma'yı cmdlet'i çalıştırarak etkinleştirme: `Set-ADSyncAADPasswordResetConfiguration –Connector $aadConnectorName –Enable $true`

> [AZURE.NOTE] Kimlik bilgilerinin istenmesi halinde, AzureADCredential için belirttiğiniz kullandığınız yönetici hesabının bir **bulut yönetici hesabı (Azure AD'de oluşturulmuş)** olduğundan ve birleştirilmiş bir hesap (şirket içi AD'de oluşturulmuş ve Azure AD ile eşitlenmiş) olmadığından emin olun.
> [AZURE.NOTE] Adım içinde `$false` kısmını geçecek şekilde yukarıdaki yönergeleri tekrarlayarak veya [Azure Yönetim Portalı](https://manage.windowsazure.com)'nda dizininizin **Configure (Yapılandır)** sekmesinin **User Password Reset Policy (Kullanıcı Parolası Sıfırlama İlkesi)** bölümünde **Write Passwords Back to On-Premises Directory (Şirket İçi Dizine Parolaları Geri Yaz)** ayarını **No (Hayır)** olarak belirleyerek PowerShell ile Parola Geri Yazma özelliğini devre dışı bırakabilirsiniz.

#### Yapılandırmanın başarılı olduğunu doğrulama
Yapılandırma başarılı olduğunda, Windows PowerShell penceresinde Parola sıfırlama geri yazma işlemi etkinleştirildi iletisini görür veya yapılandırma kullanıcı arabiriminde bir başarı iletisi görürsünüz.

Ayrıca Olay Görüntüleyicisi'ni açarak, uygulama olay günlüğüne giderek ve **PasswordResetService** kaynağına ait **31005 - OnboardingEventSuccess** olayını arayarak hizmetin doğru şekilde yüklendiğini doğrulayabilirsiniz.

  ![][023]

### 3. Adım: Güvenlik duvarınızı yapılandırma
Azure AD Connect aracında Parola Geri Yazma'yı etkinleştirdikten sonra, hizmetin buluta bağlanabildiğinden emin olmanız gerekir.

1.  Yükleme tamamlandığında, ortamınızda bilinmeyen giden bağlantıları engelliyorsanız güvenlik duvarınıza aşağıdaki kuralları da eklemeniz gerekir. Bu değişiklikleri yaptıktan sonra AAD Connect makinenizi yeniden başlattığınızdan emin olun:
   - Bağlantı noktası 443 TCP üzerinden giden bağlantılara izin verin
   - https://ssprsbprodncu-sb.accesscontrol.windows.net/ hedefine giden bağlantılara izin verin
   - Ara sunucu kullanıldığında veya genel bağlantı sorunları söz konusu olduğunda, bağlantı noktası 9350-9354 ve bağlantı noktası 5671 TCP üzerinden giden bağlantılara izin verin

### 4. Adım: İlgili Active Directory izinlerini ayarlama
Parolaları sıfırlanacak olan kullanıcıları içeren her bir orman için, yapılandırma sihirbazında söz konusu orman için belirtilen hesap X ise X hesabına `lockoutTime` üzerinde **Parola Sıfırlama**, **Parola Değiştirme**, **Yazma İzinleri**, `pwdLastSet` üzerinde **Yazma İzinleri** ve bu ormandaki her bir etki alanının kök nesnesi üzerinde genişletilmiş haklar verilmelidir. Hak, tüm kullanıcı nesneleri tarafından devralınmış olarak işaretlenmelidir.  

Yukarıdakilerin hangi hesaba başvuruda bulunduğundan emin değilseniz Azure Active Directory Connect yapılandırma kullanıcı arabirimini açın ve **Çözümünüzü İnceleme** seçeneğine tıklayın.  Aşağıdaki ekran görüntüsünde, izin eklemeniz gereken hesabın kırmızıyla altı çizilmiştir.

**<font color="red">Aksi halde parola geri yazma düzgün şekilde çalışmayacağından, sisteminizdeki her ormanda her etki alanı için bu izni ayarladığınızdan emin olun.</font>**

  ![][032]

  Bu izinlerin ayarlanması, her bir ormana yönelik MA hizmet hesabının bu orman içindeki kullanıcı hesapları adına parolaları yönetmesine olanak tanır. Bu izinleri atamazsanız geri yazma doğru yapılandırılmış gibi görünse dahi kullanıcılar buluttan şirket içi parolalarını yönetme girişiminde bulunduğunda hatalarla karşılaşır. Bunun **Active Directory Kullanıcıları ve Bilgisayarları** yönetim eklentisi kullanılarak nasıl yapılacağına dair ayrıntılı adımlar burada verilmiştir:

>[AZURE.NOTE] Bu izinlerin dizininizdeki tüm nesneler için çoğaltılması bir saate kadar sürebilir.

#### Geri yazmanın gerçekleşmesini sağlamak üzere doğru izinleri ayarlamak için

1.  İlgili etki alanı yöneticisi izinlerine sahip bir hesapla **Active Directory Kullanıcıları ve Bilgisayarları**'nı açın.
2.  **Görünüm Menüsü** seçeneğinde **Gelişmiş Özellikler**'in açık olduğundan emin olun.
3.  Sol panelde, etki alanının kökünü temsil eden nesneye sağ tıklayın.
4.  **Güvenlik** sekmesine tıklayın.
5.  Ardından **Gelişmiş**'e tıklayın.

    ![][024]

6.  **İzinler** sekmesinde **Ekle**'ye tıklayın.

    ![][025]

7.  İzin vermek istediğiniz hesabı seçin (bu, orman için eşitleme ayarlanırken belirtilen hesapla aynıdır).
8.  Üstteki açılan menüde **Descendent User objects (Alt Kullanıcı nesneleri)** seçeneğini belirleyin.
9.  Görünen **İzin Girdisi** iletişim kutusunda `lockoutTime` üzerinde **Parola Sıfırlama**, **Parola Değiştirme**, **Yazma İzinleri** ve `pwdLastSet` üzerinde **Yazma İzinleri** kutusunu işaretleyin.

    ![][026]
    ![][027]
    ![][028]

10. Ardından tüm açık iletişim kutularında **Uygula/Tamam** seçeneğine tıklayın.

### 5. Adım: Kullanıcı olarak AD parolanızı sıfırlama
Parola Geri Yazma etkinleştirildikten sonra, hesabı bulut kiracınızın hesabına eşitlenmiş bir kullanıcının parolasını sıfırlayarak bu özelliğin çalışıp çalışmadığını test edebilirsiniz.

#### Parola Geri Yazma'nın doğru şekilde çalıştığını doğrulamak için
1.  [https://passwordreset.microsoftonline.com](https://passwordreset.microsoftonline.com) adresine veya herhangi bir kuruluş kimliği oturum açma ekranına gidin ve **Hesabınıza erişemiyor musunuz?** bağlantısına tıklayın.

    ![][029]

2.  Bu durumda, parolasını sıfırlamak istediğiniz kullanıcı kimliğini soran yeni bir sayfa görmeniz gerekir. Test kullanıcı kimliğinizi girin ve parola sıfırlama işlem akışında ilerleyin.
3.  Parolanızı sıfırladıktan sonra aşağıdakine benzer bir ekran görürsünüz. Bu, şirket içi ve/veya bulut dizinlerinizde parolanızı başarıyla sıfırladığınız anlamına gelir.

    ![][030]

4.  İşlemin başarılı olduğunu doğrulamak veya mevcut hataları tanılamak için **Dizin Eşitlemesi bilgisayarınıza** gidin, **Olay Görüntüleyicisi**'ni açın, **uygulama olay günlüğüne** gidin ve test kullanıcınız için **PasswordResetService** kaynağına ait **31002 - PasswordResetSuccess** olayını arayın.

    ![][031]


<br/>
<br/>
<br/>

## Parola sıfırlama belgelerinin bağlantıları
Aşağıda, tüm Azure AD Parola Sıfırlama belge sayfalarının bağlantıları verilmiştir:

* **Oturum açmada sorun yaşadığınız için mi buradasınız?** Sorun yaşıyorsanız bkz. [kendi parolanızı değiştirme ve sıfırlama](active-directory-passwords-update-your-own-password.md).
* [**Nasıl çalışır?**](active-directory-passwords-how-it-works.md) - Hizmetin altı farklı bileşeni ve işlevleri hakkında bilgi edinin
* [**Özelleştirin**](active-directory-passwords-customize.md) - Hizmetin genel görünümünü ve hareketlerini kuruluşunuzun ihtiyaçlarına göre nasıl özelleştireceğinizi öğrenin
* [**En iyi uygulamalar**](active-directory-passwords-best-practices.md) - Kuruluşunuzdaki parolaları nasıl hızlı bir şekilde dağıtacağınızı ve etkili bir şekilde yöneteceğinizi öğrenin
* [**Görüş alın**](active-directory-passwords-get-insights.md) - Tümleşik raporlama özelliklerimiz hakkında bilgi edinin
* [**SSS**](active-directory-passwords-faq.md) - Sık sorulan soruların yanıtlarını alın
* [**Sorunları giderin**](active-directory-passwords-troubleshoot.md) - Hizmetle ilgili sorunların hızlı bir şekilde nasıl giderileceğini öğrenin
* [**Daha fazla bilgi edinin**](active-directory-passwords-learn-more.md) - Hizmetin çalışma biçimine ilişkin teknik bilgileri ayrıntılı olarak inceleyin



[001]: ./media/active-directory-passwords-getting-started/001.jpg "Image_001.jpg"
[002]: ./media/active-directory-passwords-getting-started/002.jpg "Image_002.jpg"
[003]: ./media/active-directory-passwords-getting-started/003.jpg "Image_003.jpg"
[004]: ./media/active-directory-passwords-getting-started/004.jpg "Image_004.jpg"
[005]: ./media/active-directory-passwords-getting-started/005.jpg "Image_005.jpg"
[006]: ./media/active-directory-passwords-getting-started/006.jpg "Image_006.jpg"
[007]: ./media/active-directory-passwords-getting-started/007.jpg "Image_007.jpg"
[008]: ./media/active-directory-passwords-getting-started/008.jpg "Image_008.jpg"
[009]: ./media/active-directory-passwords-getting-started/009.jpg "Image_009.jpg"
[010]: ./media/active-directory-passwords-getting-started/010.jpg "Image_010.jpg"
[011]: ./media/active-directory-passwords-getting-started/011.jpg "Image_011.jpg"
[012]: ./media/active-directory-passwords-getting-started/012.jpg "Image_012.jpg"
[013]: ./media/active-directory-passwords-getting-started/013.jpg "Image_013.jpg"
[014]: ./media/active-directory-passwords-getting-started/014.jpg "Image_014.jpg"
[015]: ./media/active-directory-passwords-getting-started/015.jpg "Image_015.jpg"
[016]: ./media/active-directory-passwords-getting-started/016.jpg "Image_016.jpg"
[017]: ./media/active-directory-passwords-getting-started/017.jpg "Image_017.jpg"
[018]: ./media/active-directory-passwords-getting-started/018.jpg "Image_018.jpg"
[019]: ./media/active-directory-passwords-getting-started/019.jpg "Image_019.jpg"
[020]: ./media/active-directory-passwords-getting-started/020.jpg "Image_020.jpg"
[021]: ./media/active-directory-passwords-getting-started/021.jpg "Image_021.jpg"
[022]: ./media/active-directory-passwords-getting-started/022.jpg "Image_022.jpg"
[023]: ./media/active-directory-passwords-getting-started/023.jpg "Image_023.jpg"
[024]: ./media/active-directory-passwords-getting-started/024.jpg "Image_024.jpg"
[025]: ./media/active-directory-passwords-getting-started/025.jpg "Image_025.jpg"
[026]: ./media/active-directory-passwords-getting-started/026.jpg "Image_026.jpg"
[027]: ./media/active-directory-passwords-getting-started/027.jpg "Image_027.jpg"
[028]: ./media/active-directory-passwords-getting-started/028.jpg "Image_028.jpg"
[029]: ./media/active-directory-passwords-getting-started/029.jpg "Image_029.jpg"
[030]: ./media/active-directory-passwords-getting-started/030.jpg "Image_030.jpg"
[031]: ./media/active-directory-passwords-getting-started/031.jpg "Image_031.jpg"
[032]: ./media/active-directory-passwords-getting-started/032.jpg "Image_032.jpg"



<!--HONumber=Aug16_HO1-->


