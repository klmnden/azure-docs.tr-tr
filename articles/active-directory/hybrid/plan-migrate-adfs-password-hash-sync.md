---
title: "Azure AD Connect: Federasyon seçeneğinden PHS için Azure AD'ye geçirme | Microsoft Docs"
description: Bu makalede, karma kimlik ortamınızı Federasyon seçeneğinden parola karması eşitleme için taşıma hakkında bilgi bulunur.
services: active-directory
author: billmath
manager: daveba
ms.reviewer: martincoetzer
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 05/31/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9ce9c0c6d4f9002b061afd2ad09f02266d452979
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67109256"
---
# <a name="migrate-from-federation-to-password-hash-synchronization-for-azure-active-directory"></a>Federasyon seçeneğinden parola karması eşitleme için Azure Active Directory geçirme

Bu makalede, parola karması eşitleme için Active Directory Federasyon Hizmetleri (AD FS) kuruluş etki alanlarınızı taşımak açıklar.

Yapabilecekleriniz [bu makalede indirme](https://aka.ms/ADFSTOPHSDPDownload).

## <a name="prerequisites-for-migrating-to-password-hash-synchronization"></a>Parola Karması eşitleme için geçiş için Önkoşullar

Parola Karması eşitleme kullanarak AD FS geçiş için aşağıdaki önkoşullar gereklidir.

### <a name="update-azure-ad-connect"></a>Güncelleştirme Azure AD'ye bağlanma

Başarıyla parola karması eşitleme için geçiş adımlarını gerçekleştirmek için minimum olarak olmalıdır [Azure AD connect](https://www.microsoft.com/download/details.aspx?id=47594) 1.1.819.0. Bu sürümü, oturum açma dönüştürme yapılır ve dakika potansiyel olarak saat bulut kimlik doğrulaması Federasyon seçeneğinden geçirmek için toplam süreyi azaltır şekilde önemli değişiklikler içerir.


> [!IMPORTANT]
> Eski belgelere, araçları ve etki alanları için yönetilen kimlik Federasyon kimlik dönüştürdüğünüzde, kullanıcı dönüştürme gerekli olduğunu blogları okuyabilir. *Kullanıcıların dönüştürme* artık gerekli değildir. Microsoft, belgeleri ve araçları, bu değişikliği yansıtacak şekilde güncelleştirmek için çalışmaktadır.

Azure AD Connect güncelleştirmek için adımları tamamlamanız [Azure AD Connect: En son sürüme yükseltme](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-upgrade-previous-version).

### <a name="password-hash-synchronization-required-permissions"></a>Parola Karması eşitleme gerekli izinler

Hızlı ayarları veya özel bir yükleme kullanarak Azure AD Connect yapılandırabilirsiniz. Özel yükleme seçeneğini kullandıysanız [gerekli izinler](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-accounts-permissions) için parola karması eşitleme, bir yerde olmayabilir.

Azure AD Connect Active Directory etki alanı Hizmetleri (AD DS) hizmet hesabı, parola karmalarını eşitleyecek şekilde aşağıdaki izinleri gerektirir:

* Dizin Değişikliklerini Çoğaltma
* Çoğaltma Directory yapılan tüm değişiklikler

Şimdi bu izinleri ormandaki tüm etki alanları için yerinde olduğunu doğrulamak için iyi bir zamandır.

### <a name="plan-the-migration-method"></a>Geçiş yöntemi planlama

Federal Kimlik Yönetimi'nden parola karma eşitlemesi ve sorunsuz çoklu oturum açma (SSO) geçirmek için iki yöntemleri seçebilirsiniz. Kullandığınız yöntem, nasıl AD FS örneğinizin özgün yapılandırmasında üzerinde bağlıdır.

* **Azure AD Connect**. Azure AD Connect kullanarak özgün AD FS yapılandırdıysanız, *gerekir* parola karması eşitleme için Azure AD Connect Sihirbazı'nı kullanarak değiştirin.

   Azure AD Connect otomatik olarak çalıştırır **kümesi MsolDomainAuthentication** kullanıcı oturum açma yöntemini değiştirdiğinizde cmdlet'i. Azure AD Connect, tüm doğrulanmış Federasyon etki alanlarını Azure AD kiracınızda otomatik olarak unfederates.

   > [!NOTE]
   > Şu anda, ilk olarak AD FS'yi yapılandırmak için Azure AD Connect kullandıysanız, kullanıcı oturum açma için parola karması eşitleme değiştirdiğinizde, kiracınızdaki tüm etki alanları unfederating kaçınamazsınız. ‎
* **PowerShell ile Azure AD Connect**. Yalnızca, ilk olarak AD FS'yi Azure AD Connect kullanarak yapılandırmadıysanız, bu yöntemi kullanabilirsiniz. Bu seçenek için Azure AD Connect Sihirbazı aracılığıyla kullanıcı oturum açma yöntemini yine de değiştirmeniz gerekir. Çekirdek bu seçenekle Sihirbazı otomatik olarak çalışmıyor fark **kümesi MsolDomainAuthentication** cmdlet'i. Bu seçenek belirtilmişse, hangi etki alanlarının dönüştürülür ve hangi sırada tam denetim sahibi.

Hangi yöntemi kullanmanız gerektiğini anlamak için aşağıdaki bölümlerde yer alan adımları tamamlayın.

#### <a name="verify-current-user-sign-in-settings"></a>Geçerli kullanıcı oturum açma ayarlarını doğrulayın

Geçerli kullanıcı oturum açma ayarlarınızı doğrulamak için:

1. Oturum [Azure AD portalında](https://aad.portal.azure.com/) bir genel yönetici hesabını kullanarak.
2. İçinde **kullanıcı oturum açma** bölümünde, aşağıdaki ayarları doğrulayın:
   * **Federasyon** ayarlanır **etkin**.
   * **Sorunsuz çoklu oturum açma** ayarlanır **devre dışı bırakılmış**.
   * **Geçişli kimlik doğrulaması** ayarlanır **devre dışı bırakılmış**.

   ![Oturum açma Azure AD Connect kullanıcı bölümündeki ayarları görüntüsü](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image1.png)

#### <a name="verify-the-azure-ad-connect-configuration"></a>Azure AD Connect yapılandırmasını doğrula

1. Azure AD Connect, Azure AD Connect sunucunuzda açın. Seçin **yapılandırma**.
2. Üzerinde **ek görevler** sayfasında **geçerli yapılandırmayı görüntüleme**ve ardından **sonraki**.<br />

   ![Ek Görevler sayfasında seçtiğiniz görünümü geçerli yapılandırma seçeneğinin ekran görüntüsü](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image2.png)<br />
3. Üzerinde **çözümünüzü İnceleme** sayfa, Not **parola karması eşitleme** durumu.<br /> 

   * Varsa **parola karması eşitleme** ayarlanır **devre dışı bırakılmış**, etkinleştirmek için bu makaledeki adımları tamamlayın.
   * Varsa **parola karması eşitleme** ayarlanır **etkin**, bölümü atlayabilirsiniz **1. adım: Parola karma eşitlemesini etkinleştirme** bu makaledeki.
4. Üzerinde **çözümünüzü İnceleme** sayfasında, kaydırma **Active Directory Federasyon Hizmetleri (AD FS)** .<br />

   * AD FS yapılandırması bu bölümünde görünüyorsa, güvenli bir şekilde Azure AD Connect kullanarak AD FS başlangıçta yapılandırıldığını kabul edilebilir. Azure AD Connect kullanarak yönetilen kimlik için Federasyon kimlik etki alanlarınızı dönüştürebilirsiniz **değiştirme kullanıcı oturum açma** seçeneği. İşlem bölümünde ayrıntılı olarak verilmiştir **seçenek A: Geçiş Federasyon seçeneğinden parola karması eşitleme için Azure AD Connect kullanarak**.
   * AD FS geçerli ayarları listede yoksa, el ile etki alanlarınızı Federal Kimlik için yönetilen kimlik PowerShell kullanarak dönüştürmeniz gerekir. Bu işlem hakkında daha fazla bilgi için bkz **seçenek B: Geçiş Federasyon seçeneğinden parola karması eşitleme için Azure AD Connect ve PowerShell kullanarak**.

### <a name="document-current-federation-settings"></a>Belge geçerli federasyon ayarları

Geçerli federasyon ayarlarını bulmak için çalıştırın **Get-msoldomainfederationsettings komutunu** cmdlet:

``` PowerShell
Get-MsolDomainFederationSettings -DomainName YourDomain.extention | fl *
```

Örnek:

``` PowerShell
Get-MsolDomainFederationSettings -DomainName Contoso.com | fl *
```

Federasyon tasarım ve dağıtım belgeleri için özelleştirilmiş herhangi bir ayarı doğrulayın. Özelleştirmeler için özellikle şunlara dikkat **PreferredAuthenticationProtocol**, **SupportsMfa**, ve **PromptLoginBehavior**.

Daha fazla bilgi için şu makalelere bakın:

* [AD FS istemi = oturum açma parametresi desteği](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/ad-fs-prompt-login)
* [Set-MsolDomainAuthentication](https://docs.microsoft.com/powershell/module/msonline/set-msoldomainauthentication?view=azureadps-1.0)

> [!NOTE]
> Varsa **SupportsMfa** ayarlanır **True**, kullanıcı kimlik doğrulaması akışı bir ikinci faktör challenge eklenmek üzere bir şirket içi multi-Factor authentication çözümünü kullanıyorsanız. Kimlik doğrulaması bu etki alanından dönüştürmeden sonra Azure AD kimlik doğrulama senaryoları için çalışır Federasyon oluşturan bu ayar artık yönetilen. Federasyon devre dışı bıraktıktan sonra ilişki için şirket içi Federasyon sunucusu ve bu, şirket içi MFA bağdaştırıcıları içerir. 
>
> Bunun yerine, bulut tabanlı Azure multi-Factor Authentication hizmeti ile aynı işlevi gerçekleştirmek için kullanın. Devam etmeden önce multi-Factor authentication gereksinimlerini dikkatlice değerlendirin. Etki alanlarınızı dönüştürmeden önce Azure multi-Factor Authentication, lisans etkileri ve kullanıcı kayıt işleminin nasıl kullanılacağını anlamak emin olun.

#### <a name="back-up-federation-settings"></a>Federasyon ayarlarını yedekle

Bu makalede açıklanan işlemleri sırasında AD FS grubunuzdaki diğer bağlı taraflar için hiçbir değişiklik yapılmadı olsa da, geçerli bir geçerli yedek geri yükleyebilirsiniz, AD FS grubunun sahip olmasını öneririz. Ücretsiz Microsoft kullanarak geçerli bir geçerli yedek oluşturabilir [AD FS hızlı geri yükleme aracı](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/ad-fs-rapid-restore-tool). AD FS, yedekleme ve var olan bir grubu geri yüklemek veya yeni bir grup oluşturmak için Aracı'nı kullanabilirsiniz.

AD FS hızlı geri yükleme aracı en az kullanmayı tercih ederseniz, Microsoft Office 365 kimlik platformu taraf güveni ve eklediğiniz tüm ilişkili özel talep kuralları dışarı aktarmanız gerekir. Aşağıdaki PowerShell örneğini kullanarak ilişkili talep kuralları ve bağlı olan taraf güveni dışarı aktarabilirsiniz:

``` PowerShell
(Get-AdfsRelyingPartyTrust -Name "Microsoft Office 365 Identity Platform") | Export-CliXML "C:\temp\O365-RelyingPartyTrust.xml"
```

## <a name="deployment-considerations-and-using-ad-fs"></a>Dağıtım hakkında önemli noktalar ve AD FS kullanma

Bu bölümde, AD FS kullanımı ile ilgili dikkat edilecekler ve Ayrıntılar açıklanmaktadır.

### <a name="current-ad-fs-use"></a>Geçerli AD FS kullanma

Yönetilen kimliğe federe kimlik dönüştürmeden önce yakından şu anda Azure AD, Office 365 için AD FS'yi kullanın ve diğer uygulamalar (bağlı olan taraf güvenleri) bakın. Özellikle, aşağıdaki tabloda açıklanan senaryoları göz önünde bulundurun:

| Eğer | Ardından |
|-|-|
| AD FS ile diğer uygulamaları kullanmaya devam etmek plan (Azure AD dışındaki ve Office 365). | Etki alanlarınızı dönüştürdükten sonra hem AD FS hem de Azure AD'den kullanacaksınız. Kullanıcı deneyimini düşünün. Bazı senaryolarda, kullanıcıların iki kez kimlik doğrulaması için gerekli olabilir: bir kez (burada bir kullanıcı Office 365 gibi diğer uygulamalara SSO erişimi alır) Azure AD'ye ve yeniden yine de AD FS bağlı olan taraf güveni olarak bağlanmış olan tüm uygulamalar için. |
| AD FS örneğinizi yoğun olarak özelleştirilmiş ve onload.js dosyasındaki belirli özelleştirme ayarları kullanır (örneğin, kullanıcılar yalnızca kullanın, böylece oturum açma deneyimini değiştirdiyseniz, bir **SamAccountName** kullanıcı adı biçimi yerine bir kullanıcı asıl adı (UPN) veya Kurumunuz yoğun olarak oturum açma deneyimini markalı). Azure AD'de onload.js dosya yinelenemez. | Devam etmeden önce Azure AD geçerli özelleştirme gereksinimlerinizi karşıladığını doğrulamanız gerekir. Daha fazla bilgi ve yönergeler için AD FS markası ve AD FS özelleştirmesi bölümlere bakın.|
| AD FS kimlik doğrulama istemcilerini önceki sürümlerini engellemek için kullanın.| Bir bileşimini kullanarak kimlik doğrulama istemcilerini önceki sürümlerini engelle AD FS denetimleri değiştirmeyi göz önüne alın [koşullu erişim denetimleri](https://docs.microsoft.com/azure/active-directory/conditional-access/conditions) ve [Exchange Online istemci erişim kuralları](https://aka.ms/EXOCAR). |
| Kullanıcıları AD FS'ye kimlik doğrulaması sırasında bir şirket içi multi-Factor authentication server çözümünü – çok faktörlü kimlik doğrulaması yapmalarını gerektirir.| Bir yönetilen kimlik etki alanında bir çok faktörlü kimlik doğrulaması sınaması aracılığıyla şirket içi multi-Factor authentication çözümünü kimlik doğrulaması akışı eklenemiyor. Ancak, etki alanı dönüştürüldükten sonra Azure multi-Factor Authentication hizmeti çok faktörlü kimlik doğrulaması için kullanabilirsiniz.<br /><br /> Kullanıcılarınızın şu anda Azure çok faktörlü kimlik doğrulaması kullanmıyorsanız, tek seferlik kullanıcı kayıt adımı gereklidir. Hazırlama ve kullanıcılarınız için planlanan kayıt iletişim gerekir. |
| Şu anda Office 365 erişimi denetlemek için erişim denetim ilkeleri (AuthZ kuralları) AD FS'de kullanın.| İlkeleri eşdeğer Azure AD ile değiştirmeyi göz önüne alın [koşullu erişim ilkeleri](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal) ve [Exchange Online istemci erişim kuralları](https://aka.ms/EXOCAR).|

### <a name="common-ad-fs-customizations"></a>Ortak AD FS özelleştirmelerini

Bu bölümde, ortak AD FS özelleştirmelerini açıklanmaktadır.

#### <a name="insidecorporatenetwork-claim"></a>InsideCorporateNetwork talep

AD FS sorunları **InsideCorporateNetwork** kimlik doğrulaması kullanıcı şirket ağı içinde olup olmadığını talep. Bu talep ardından Azure AD'ye geçirilebilir. Talep, kullanıcının ağ konumuna göre çok faktörlü kimlik doğrulamasını atlamak için kullanılır. Bu işlevsellik şu anda AD FS'de etkin olup olmadığını belirlemek öğrenmek için bkz. [güvenilen IP'leri Federasyon kullanıcıları için](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-get-started-adfs-cloud).

**InsideCorporateNetwork** talep, parola karma eşitlemesini alanlarınızı dönüştürüldükten sonra kullanılamaz. Kullanabileceğiniz [Azure AD'de adlandırılmış Konumlar](https://docs.microsoft.com/azure/active-directory/active-directory-named-locations) bu işlevselliği değiştirilecek.

Adlandırılmış konumlar yapılandırdıktan sonra dahil etmek veya hariç ağ için yapılandırılmış olan tüm koşullu erişim ilkeleri güncelleştirmelisiniz **tüm Güvenilen Konumlar** veya **MFA güvenilen IP'ler** değerler Yeni adlandırılmış konumlar yansıtır.

Hakkında daha fazla bilgi için **konumu** koşullu erişim koşulu için bkz: [Active Directory koşullu erişimi konumlarındaki](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-locations).

#### <a name="hybrid-azure-ad-joined-devices"></a>Hibrit Azure AD'ye katılmış cihazları

Bir cihazı Azure AD'ye Ekle, cihazları, güvenlik ve uyumluluğa yönelik standartlarınızı erişim karşıladığını zorlayan koşullu erişim kuralları oluşturabilirsiniz. Ayrıca, kullanıcılar bir cihaza bir kurumsal iş veya Okul hesabı yerine kişisel bir hesap kullanarak oturum açabilir. Kullandığınızda hibrit Azure AD'ye katılmış cihazlarda, Active Directory etki alanına katılmış cihazlarınızı Azure AD'ye Katıl için. Ortamınız federe bu özelliği kullanmak için ayarlanan.

Bu karma birleştirme, etki alanları için Windows 10 istemcileri için parola karması eşitlemeyi dönüştürüldükten sonra etki alanına katılmış cihazlar için çalışmaya devam edebilmesini sağlamak üzere Active Directory bilgisayar hesapları için Azure AD eşitleme için Azure AD Connect kullanmanız gerekir. 

Windows 8 ve Windows 7 bilgisayar hesapları için karma birleştirme bilgisayar Azure AD'ye kaydetme sorunsuz çoklu oturum açma kullanır. Eşitleme Windows 8 ve Windows 10 cihazları için yaptığınız gibi Windows 7 bilgisayar hesaplarını gerekmez. Ancak, bunlar kendilerini sorunsuz çoklu oturum açma kullanarak kaydedebilirsiniz, böylece bir güncelleştirilmiş workplacejoin.exe dosyası (.msi dosyası) aracılığıyla Windows 8 ve Windows 7 istemcileri için dağıtmanız gerekir. [.Msi dosyasını indirme](https://www.microsoft.com/download/details.aspx?id=53554).

Daha fazla bilgi için [yapılandırma hibrit Azure AD'ye katılmış cihazları](https://docs.microsoft.com/azure/active-directory/device-management-hybrid-azuread-joined-devices-setup).

#### <a name="branding"></a>Markalama

Varsa, kuruluşunuzun [, AD FS oturum sayfalarının özelleştirilmiş](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/ad-fs-user-sign-in-customization) kuruluşa daha ilgili bilgileri görüntülemek için benzer yapmayı düşünün [Azure AD oturum açma sayfası özelleştirmeleri](https://docs.microsoft.com/azure/active-directory/customize-branding).

Benzer özelleştirmeleri kullanılabilir olsa da, dönüştürme işleminden sonra oturum açma sayfalarındaki görsel bazı değişiklikler beklenmelidir. Kullanıcılara, iletişimin beklenen değişiklikleri hakkında bilgi sağlamak isteyebilirsiniz.

> [!NOTE]
> Kuruluşunuzun markasını yalnızca Azure Active Directory Premium veya temel lisansı satın aldıysanız veya bir Office 365 lisansınız varsa kullanılabilir.

## <a name="plan-deployment-and-support"></a>Dağıtım ve Destek planı

Dağıtım ve destek için planlama yapmanıza yardımcı olması için bu bölümdeki açıklanan görevleri tamamlayın.

### <a name="plan-the-maintenance-window"></a>Bakım penceresi planlama

Etki alanı dönüştürme işlemi görece hızlı olsa da, Azure AD etki alanı dönüştürme tamamlandıktan sonra en fazla dört saat için AD FS sunucularınızın bazı kimlik doğrulama istekleri göndermeye devam edebilir. Bu dört saatlik penceresi sırasında ve çeşitli hizmet tarafı önbellekler bağlı olarak, Azure AD kimlik doğrulamaları bu kabul etmeyebilir. Kullanıcılar, bir hata alabilirsiniz. Kullanıcı yine de başarılı olabilir karşı AD FS kimlik doğrulaması, ancak Azure AD'ye artık kabul kullanıcının verilen belirteç çünkü bu federasyon güveni artık kaldırılır.

Hizmet tarafı önbelleği temizlenmeden önce bu dönüştürme sonrası penceresi sırasında hizmetler bir web tarayıcısı üzerinden erişim yalnızca kullanıcılar etkilenmez. Eski istemcileri (Exchange ActiveSync, Outlook 2010/2013) Exchange Online kimlik bilgilerini önbelleğini belirli bir süre için tuttuğu etkilenen beklenen değildir. Önbellek sessizce kullanıcının yeniden kimlik doğrulamaya zorlayabilir kullanılır. Kullanıcının AD FS için döndürülecek yok. Bu istemciler için cihazda depolanan kimlik bilgileri, bu önbelleğe temizlendikten sonra kendilerini sessizce sağlamalarını için kullanılır. Kullanıcıların etki alanı dönüştürme işlemi sonucunda herhangi bir parola istemi almak için beklenen değildir. 

Modern kimlik doğrulaması istemcileri (Office 2016 ve Office 2013, iOS ve Android uygulamaları) erişmeye devam etmek için AD FS döndürmek yerine kaynakları için yeni erişim belirteçleri almak için geçerli bir yenileme belirteci kullanın. Bu istemciler etki alanı dönüştürme işleminden kaynaklanan herhangi bir parola istemi için ayrıcalıklı. İstemcilerin, ek yapılandırma çalışmaya devam eder.

> [!IMPORTANT]
> AD FS, ortamı kapatın veya Bulut kimlik doğrulaması kullanarak, tüm kullanıcılar başarıyla doğrulanabilir doğrulayana kadar Office 365 bağlı olan taraf güveni kaldırma kullanmayın.

### <a name="plan-for-rollback"></a>Geri alma planlama

Hızlı bir şekilde çözümlenemiyor önemli bir sorun yaşarsanız, çözümü için Federasyon geri karar verebilirsiniz. Dağıtımınızın beklendiği gibi değil atarsanız yapmanız gerekenler planlamanız önemlidir. Dağıtım sırasında etki alanı veya kullanıcı dönüştürme başarısız olursa veya Federasyon hizmetine geri almak gerekiyorsa, herhangi bir kesinti azaltmak ve kullanıcılarınızın üzerindeki etkisini azaltmak nasıl anlamanız gerekir.

#### <a name="to-roll-back"></a>Geri almak için

Geri alma için planlamak için belirli dağıtım ayrıntıları Federasyon tasarım ve dağıtım belgelerini denetleyin. İşlem, bu görevleri içermelidir:

* Kullanarak yönetilen etki alanlarını Federasyon etki alanları için dönüştürme **dönüştürme MSOLDomainToFederated** cmdlet'i.
* Gerekirse, ek yapılandırma kuralları talepleri.

### <a name="plan-communications"></a>İletişimi planlama

Dağıtım ve Destek planlamanın önemli bir bölümü kullanıcılarınızın proaktif olarak yaklaşan değişiklikler hakkında bilgi sahibi olduğundan emin olmaktır. Kullanıcılar, bunları gerekli nedir ve ne bunlar karşılaşabilirsiniz önceden bilmeniz gerekir. 

Parola Karması eşitleme hem sorunsuz çoklu oturum açma dağıtıldıktan sonra kullanıcı oturum açma deneyimi Azure AD değişiklikleri Office 365 ve kimlik doğrulamalarının diğer kaynaklara erişmek için. Kullanıcılar ağ dışındayken yalnızca Azure AD oturum açma sayfasına bakın. Bu kullanıcılar dönük web uygulaması Ara sunucusu tarafından sunulan form tabanlı sayfasına yeniden yönlendirilen değildir.

İletişim stratejinizi aşağıdaki öğeleri içerir:

* Kullanarak yaklaşan ve yayımlanan işlevselliği konusunda kullanıcıları bilgilendirin:
   * E-posta ve diğer şirket içi iletişim kanallarını.
   * Görsel ve posterler gibi.
   * Yönetici, live veya diğer iletişimler.
* Kullanan iletişimleri özelleştireceğim belirlemek ve iletişim gönderecek kimin ve ne zaman.

## <a name="implement-your-solution"></a>Çözümünüzü uygulama

Çözümünüzü planladığınız. Şimdi, bunu şimdi uygulayabilirsiniz. Uygulama aşağıdaki bileşenleri içerir:

* Parola karma eşitlemesini etkinleştirme.
* Sorunsuz çoklu oturum açma için hazırlık yapılıyor.
* Oturum açma yöntemi, parola karma eşitlemesi ve sorunsuz çoklu oturum açmayı etkinleştirme değiştiriliyor.

### <a name="step-1-enable-password-hash-synchronization"></a>1\. adım: Parola karma eşitlemesini etkinleştirme

Bu çözümü uygulamak için ilk adım, Azure AD Connect Sihirbazı'nı kullanarak parola karması eşitlemeyi etkinleştirmektir. Parola Karması eşitleme Federasyon kullanan ortamlarda olanak veren isteğe bağlı bir özelliktir. Kimlik doğrulaması akışı üzerinde hiçbir etkisi yoktur. Bu durumda, Azure AD Connect parola karmalarının Federasyon kullanarak oturum açın kullanıcıları etkilemeden eşitleme başlar.

Bu nedenle, etki alanınızın oturum açma yöntemi değiştirmeden önce hazırlama görevi iyi bu adımı tamamlamanızı öneririz. Ardından, parola karması eşitleme düzgün çalıştığını doğrulamak için bol zaman sahip olacaksınız.

Parola Karması eşitlemeyi etkinleştirmek için:

1. Azure AD Connect sunucusunda Azure AD Connect Sihirbazı'nı açın ve ardından **yapılandırma**.
2. Seçin **eşitleme seçeneklerini özelleştirme**ve ardından **sonraki**.
3. Üzerinde **Azure ad Connect** sayfasında, kullanıcı adı ve bir genel yönetici hesabının parolasını girin.
4. Üzerinde **dizinlerinizi bağlama** sayfasında **sonraki**.
5. Üzerinde **etki alanı ve OU filtreleme** sayfasında **sonraki**.
6. Üzerinde **isteğe bağlı özellikler** sayfasında **parola eşitleme**ve ardından **sonraki**.
 
   ![İsteğe bağlı özellikler sayfasında seçtiğiniz parola eşitleme seçeneğinin ekran görüntüsü](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image6.png)<br />
7. Seçin **sonraki** kalan sayfalarında. Son sayfasında, seçin **yapılandırma**.
8. Azure AD Connect parola karmalarının sonraki eşitlemede eşitleme başlar.

Parola Karması eşitleme etkinleştirildikten sonra Azure AD Connect eşitleme kapsamı yeniden karma haline getirilen ve Azure AD'ye yazılan tüm kullanıcılar için parola karma hale getirir. Kullanıcıların sayısına bağlı olarak, bu işlem, dakika veya birkaç saat sürebilir.

Planlama amacıyla, 20.000 kullanıcı yaklaşık 1 saat içindeki işlenen tahmin etmelidir.

Parola Karması eşitleme düzgün çalıştığını doğrulamak için tamamlayın **sorun giderme** görev Azure AD Connect Sihirbazı'nda:

1. Yönetici seçeneği Çalıştır'ı kullanarak Azure AD Connect sunucunuzda yeni bir Windows PowerShell oturumu açın.
2. Çalıştırma `Set-ExecutionPolicy RemoteSigned` veya `Set-ExecutionPolicy Unrestricted`.
3. Azure AD Connect Sihirbazı'nı başlatın.
4. Git **ek görevler** sayfasında **sorun giderme**ve ardından **sonraki**.
5. Üzerinde **sorun giderme** sayfasında **başlatma** PowerShell'de sorun giderme menü başlatmak için.
6. Ana menüden **parola karması eşitleme sorunlarını giderme**.
7. Alt menüden seçin **parola karması eşitleme hiç çalışmıyor**.

Sorunlarını gidermek için bkz: [Azure AD Connect eşitlemesi ile parola karması eşitleme sorunlarını giderme](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-troubleshoot-password-hash-synchronization).

### <a name="step-2-prepare-for-seamless-sso"></a>2\. adım: Sorunsuz çoklu oturum açma için hazırlama

Cihazlarınızı sorunsuz çoklu oturum açmayı kullanmak, Active Directory'de Grup İlkesi'ni kullanarak bir Azure AD URL'si kullanıcıların intranet bölgesi ayarlarına eklemeniz gerekir.

Varsayılan olarak, web tarayıcıları otomatik olarak doğru bölgeyi hesaplamak internet veya intranet, bir URL'den. Örneğin, **http:\/\/contoso /** eşler intranet bölgesine ve **http:\/\/intranet.contoso.com** (çünkü Internet bölgesine eşler URL bir dönem bulunur). Yalnızca açıkça URL için tarayıcının intranet bölgesine eklerseniz tarayıcılar Kerberos biletleri Azure AD URL'yi gibi bir bulut uç noktasına gönderin.

Adımlarını tamamlamanız [dağıtmadan](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-sso-quick-start) cihazlarınıza gerekli değişiklikleri.

> [!IMPORTANT]
> Bu değişiklik, kullanıcıların Azure AD'de oturum biçimini değiştirmez. Ancak, devam etmeden önce bu yapılandırma tüm cihazlarınıza uygulama önemlidir. Bu yapılandırmayı almadığını cihazlarda oturum kullanıcılar yalnızca bir kullanıcı adı ve Azure AD'de oturum açmak için parola girmesini gerekir.

### <a name="step-3-change-the-sign-in-method-to-password-hash-synchronization-and-enable-seamless-sso"></a>3\. adım: Oturum açma yöntemi, parola karma eşitlemesini değiştirmek ve sorunsuz çoklu oturum açmayı etkinleştir

Parola Karması eşitleme için oturum açma yöntemini değiştirme ve sorunsuz çoklu oturum açmayı etkinleştirmek için iki seçeneğiniz vardır.

#### <a name="option-a-switch-from-federation-to-password-hash-synchronization-by-using-azure-ad-connect"></a>Seçenek A: Federasyon seçeneğinden parola karması eşitleme için Azure AD Connect kullanarak geçiş

Başlangıçta Azure AD Connect kullanarak AD FS ortamınızı yapılandırılmıştır, bu yöntemi kullanın. Bu yöntemi kullanamazsınız, *yaramadı* ilk olarak Azure AD Connect kullanarak AD FS ortamınızı yapılandırın.

İlk olarak, oturum açma yöntemini değiştirin:

1. Azure AD Connect sunucusunda Azure AD Connect Sihirbazı'nı açın.
2. Seçin **değiştirme kullanıcı oturum açma**ve ardından **sonraki**. 

   ![Ekran görüntüsü değişiklik kullanıcı oturum açma sayfasında seçenek ek görevler](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image7.png)<br />
3. Üzerinde **Azure ad Connect** sayfasında, kullanıcı adı ve bir genel yönetici hesabının parolasını girin.
4. Üzerinde **kullanıcı oturum açma** sayfasında **parola karması eşitleme düğmesini**. Seçtiğinizden emin olun **kullanıcı hesaplarını dönüştürme** onay kutusu. Seçeneği kullanım dışı bırakılmıştır. Seçin **çoklu oturum açmayı etkinleştirme**ve ardından **sonraki**.

   ![Oturum açmayı etkinleştir tek sayfasının ekran görüntüsü](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image8.png)<br />

   > [!NOTE]
   > Azure AD Connect sürümü 1.1.880.0, başlangıç **sorunsuz çoklu oturum açma** onay kutusu varsayılan olarak seçilidir.

   > [!IMPORTANT]
   > Bu kullanıcı dönüştürme belirtmek uyarılar güvenle yok sayabilirsiniz ve tam parola karması eşitleme Federasyon seçeneğinden bulut kimlik doğrulaması için dönüştürmek için gerekli adımlar verilmiştir. Bu adımları artık gerekli olmadığını unutmayın. Azure AD Connect ve, en son sürümünü çalıştırdığınızdan emin olun, bu uyarılar görmeye devam ediyorsanız bu kılavuzun en son sürümünü kullanıyorsanız. Daha fazla bilgi için konudaki [güncelleştirme Azure AD Connect](#update-azure-ad-connect).

5. Üzerinde **çoklu oturum açmayı etkinleştirme** sayfa, etki alanı yöneticisi hesabının kimlik bilgilerini girin ve ardından **sonraki**.

   ![Oturum açmayı etkinleştir tek sayfasının ekran görüntüsü](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image9.png)<br />

   > [!NOTE]
   > Etki alanı yönetici hesabı kimlik bilgilerini sorunsuz çoklu oturum açmayı etkinleştirmek için gereklidir. İşlemini bu yükseltilmiş izinler gerektiren aşağıdaki eylemleri gerçekleştirir. Etki alanı yönetici hesabı kimlik bilgileri, Azure AD CONNECT'te veya Azure AD'de depolanmaz. Etki alanı yönetici hesabı kimlik bilgileri yalnızca özelliği etkinleştirmek için kullanılır. İşlem başarıyla tamamlandığında, kimlik bilgileri atılır.
   >
   > 1. (Azure AD temsil eden) AZUREADSSOACC adlı bir bilgisayar hesabı, şirket içi Active Directory Örneğinizde oluşturulur.
   > 2. Bilgisayar hesabının Kerberos şifre çözme anahtarı güvenli bir şekilde Azure AD ile paylaşılır.
   > 3. Azure AD oturum açma sırasında kullanılan iki URL temsil etmek için iki Kerberos hizmet asıl adı (SPN) oluşturulur.

6. Üzerinde **yapılandırma için hazır** sayfasında, aşağıdakileri sağlayın **Yapılandırma tamamlandığında eşitleme işlemini başlatmak** onay kutusu seçilidir. Ardından, **yapılandırma**.

      ![Ekran görüntüsü sayfasını yapılandırmak için hazır](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image10.png)<br />

   > [!IMPORTANT]
   > Bu noktada, tüm Federasyon etki alanları, yönetilen kimlik doğrulamasına değişecektir. Parola Karması eşitleme yeni kimlik doğrulama yöntemidir.

7. Azure AD portalında **Azure Active Directory** > **Azure AD Connect**.
8. Bu ayarları doğrulayın:
   * **Federasyon** ayarlanır **devre dışı bırakılmış**.
   * **Sorunsuz çoklu oturum açma** ayarlanır **etkin**.
   * **Parola Eşitleme** ayarlanır **etkin**.<br /> 

   ![Oturum açma kullanıcı bölümünde ayarları gösteren ekran görüntüsü](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image11.png)<br />

Atlamak [test ve sonraki adımları](#testing-and-next-steps).

   > [!IMPORTANT]
   > Bu bölümü atlayın **seçenek B: Geçiş Federasyon seçeneğinden parola karması eşitleme için Azure AD Connect ve PowerShell kullanarak**. Oturum açma yöntemi, parola karma eşitlemesini değiştirmek ve sorunsuz çoklu oturum açmayı etkinleştirmek için bir seçeneği seçerseniz, bu bölümdeki adımları geçerli değildir.

#### <a name="option-b-switch-from-federation-to-password-hash-synchronization-using-azure-ad-connect-and-powershell"></a>Seçenek B: Federasyon seçeneğinden parola karması eşitleme için Azure AD Connect ve PowerShell kullanarak geçiş

Başlangıçta, Azure AD Connect kullanarak Federasyon etki alanlarınızı yapılandırmadıysanız, bu seçeneği kullanın. Bu işlem sırasında sorunsuz SSO ve, etki alanlarından yönetilen Federasyon oluşturan bir anahtar sağlar.

1. Azure AD Connect sunucusunda Azure AD Connect Sihirbazı'nı açın.
2. Seçin **değiştirme kullanıcı oturum açma**ve ardından **sonraki**.
3. Üzerinde **Azure ad Connect** sayfasında, bir genel yönetici hesabı için kullanıcı adı ve parola girin.
4. Üzerinde **kullanıcı oturum açma** sayfasında **parola karması eşitleme** düğmesi. Seçin **çoklu oturum açmayı etkinleştirme**ve ardından **sonraki**.

   Parola Karması eşitlemeyi etkinleştirmeden önce: ![Do gösteren ekran görüntüsü kullanıcı oturum açma sayfasında seçenek yapılandırmazsanız](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image12.png)<br />

   Parola Karması eşitlemeyi etkinleştirdikten sonra: ![Kullanıcı oturum açma sayfasında yeni seçenekleri gösteren ekran görüntüsü](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image13.png)<br />
   
   > [!NOTE]
   > Azure AD Connect sürümü 1.1.880.0, başlangıç **sorunsuz çoklu oturum açma** onay kutusu varsayılan olarak seçilidir.

5. Üzerinde **çoklu oturum açmayı etkinleştirme** sayfasında bir etki alanı yönetici hesabı için kimlik bilgilerini girin ve ardından **sonraki**.

   > [!NOTE]
   > Etki alanı yönetici hesabı kimlik bilgilerini sorunsuz çoklu oturum açmayı etkinleştirmek için gereklidir. İşlemini bu yükseltilmiş izinler gerektiren aşağıdaki eylemleri gerçekleştirir. Etki alanı yönetici hesabı kimlik bilgileri, Azure AD CONNECT'te veya Azure AD'de depolanmaz. Etki alanı yönetici hesabı kimlik bilgileri yalnızca özelliği etkinleştirmek için kullanılır. İşlem başarıyla tamamlandığında, kimlik bilgileri atılır.
   >
   > 1. (Azure AD temsil eden) AZUREADSSOACC adlı bir bilgisayar hesabı, şirket içi Active Directory Örneğinizde oluşturulur.
   > 2. Bilgisayar hesabının Kerberos şifre çözme anahtarı güvenli bir şekilde Azure AD ile paylaşılır.
   > 3. Azure AD oturum açma sırasında kullanılan iki URL temsil etmek için iki Kerberos hizmet asıl adı (SPN) oluşturulur.

6. Üzerinde **yapılandırma için hazır** sayfasında, aşağıdakileri sağlayın **Yapılandırma tamamlandığında eşitleme işlemini başlatmak** onay kutusu seçilidir. Ardından, **yapılandırma**.

   ![Hazır sayfasını yapılandırmak için Yapılandır düğmesini gösteren ekran görüntüsü](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image15.png)<br />
   Seçtiğinizde, **yapılandırma** düğme, sorunsuz çoklu oturum açma önceki adımda belirtildiği şekilde yapılandırılır. Daha önce etkin olduğundan, parola karması eşitleme yapılandırması değiştirilmez.

   > [!IMPORTANT]
   > Yolu için hiçbir değişiklik yapılmaz kullanıcılar oturum şu an.

7. Bu ayarlar Azure AD Portalı'nda doğrulayın:
   * **Federasyon** ayarlanır **etkin**.
   * **Sorunsuz çoklu oturum açma** ayarlanır **etkin**.
   * **Parola Eşitleme** ayarlanır **etkin**.

   ![Oturum açma kullanıcı bölümünde ayarları gösteren ekran görüntüsü](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image16.png)

#### <a name="convert-domains-from-federated-to-managed"></a>Yönetilen Federasyon oluşturan etki alanlarından Dönüştür

Bu noktada, Federasyon hala etkinse ve operasyonel etki. Dağıtıma devam etmek için her etki alanı dönüştürülecek kullanıcı kimlik doğrulaması aracılığıyla parola karma eşitlemesini zorlamak için yönetilen Federasyon oluşturan gerekir.

> [!IMPORTANT]
> Tüm etki alanları aynı anda dönüştürmeniz gerekmez. Test üretim kiracınızın etki alanı ile başlayamaz veya en düşük kullanıcı sayısı olan, etki alanı ile başlamak isteyebilirsiniz.

Dönüştürme, Azure AD PowerShell modülünü kullanarak tamamlayın:

1. PowerShell'de genel yönetici hesabını kullanarak Azure AD'de oturum açın.
2. İlk etki alanı dönüştürmek için aşağıdaki komutu çalıştırın:

   ``` PowerShell
   Set-MsolDomainAuthentication -Authentication Managed -DomainName <domain name>
   ```

3. Azure AD portalında **Azure Active Directory** > **Azure AD Connect**.
4. Etki alanı için aşağıdaki komutu çalıştırarak yönetilen dönüştürülmüş olduğunu doğrulayın:

   ``` PowerShell
   Get-MsolDomain -DomainName <domain name>
   ```

## <a name="testing-and-next-steps"></a>Test ve sonraki adımlar

Parola Karması eşitleme doğrulamak ve dönüştürme işlemini tamamlamak için aşağıdaki görevleri tamamlayın.

### <a name="test-authentication-by-using-password-hash-synchronization"></a>Parola Karması eşitleme kullanarak kimlik doğrulamasını Sına 

Kiracınızın Federasyon kimliği kullanıldığında, kullanıcıların AD FS ortamınız için Azure AD oturum açma sayfasından yönlendirildiniz. Kiracı parola karması eşitleme yerine şirket dışı kimlik doğrulaması kullanmak üzere yapılandırılmış, kullanıcıları AD FS'ye yönlendirilen değildir. Kullanıcıların doğrudan Azure AD oturum açma sayfasında bunun yerine, oturum açın.

Parola Karması eşitleme test etmek için:

1. Sorunsuz çoklu oturum açma oturumunuzu otomatik olarak değil, Internet Explorer InPrivate modunda açın.
2. Office 365 oturum açma sayfasına gidin ([https://portal.office.com](https://portal.office.com/)).
3. Bir kullanıcının UPN girin ve ardından **sonraki**. Kim, şirket içi Active Directory örneğinden eşitlendi ve daha önce şirket dışı kimlik doğrulaması kullanan bir karma kullanıcının UPN'sini girdiğinizden emin olun. Kullanıcı adı ve parola, girdiğiniz bir sayfa görüntülenir:

   ![Oturum açma sayfasında, bir kullanıcı adı girmeniz gösteren ekran görüntüsü](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image18.png)

   ![Bir parola girmenizi oturum açma sayfasını gösteren ekran görüntüsü](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image19.png)

4. Parolayı girin ve seçin sonra **oturum**, Office 365 portalına yönlendirilirsiniz.

   ![Office 365 portalını gösteren ekran görüntüsü](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image20.png)


### <a name="test-seamless-sso"></a>Sorunsuz çoklu oturum açma testi

1. Şirket ağına bağlı bir etki alanına katılmış bir makinede oturum açın.
2. Internet Explorer veya Chrome, aşağıdaki URL'ler (Değiştir "contoso" etki alanınız ile) birine gidin:

   * https:\/\/myapps.microsoft.com/contoso.com
   * https:\/\/myapps.microsoft.com/contoso.onmicrosoft.com

   Kullanıcı kısa ", oturum açmaya çalışırken." iletisini gösteren Azure AD oturum açma sayfasına yönlendirilir Kullanıcı, bir kullanıcı adı veya parola için istemde değil.<br />

   ![Azure AD oturum açma sayfası ve iletiyi gösteren ekran görüntüsü](media/plan-migrate-adfs-password-hash-sync/migrating-adfs-to-phs_image21.png)<br />
3. Kullanıcı yönlendirilir ve erişim paneli için başarıyla oturum açıldı:

   > [!NOTE]
   > Sorunsuz çoklu oturum açma etki alanı İpucu (örneğin, myapps.microsoft.com/contoso.com) destekleyen Office 365 hizmetleri üzerinde çalışır. Şu anda, Office 365 portalında (portal.office.com) etki alanı ipuçlarını desteklemez. Kullanıcıların UPN girmeniz gerekir. Sorunsuz çoklu oturum açma, bir UPN girildikten sonra kullanıcı adına bir Kerberos anahtarı alır. Kullanıcıyı parola girmeye gerek kalmadan açmıştır.

   > [!TIP]
   > Dağıtmayı göz önünde bulundurun [hibrit Azure AD'ye Katıl Windows 10'da](https://docs.microsoft.com/azure/active-directory/device-management-introduction) geliştirilmiş SSO bir deneyim için.

### <a name="remove-the-relying-party-trust"></a>Bağlı olan taraf güveni kaldırma

Tüm kullanıcılar ve istemcilerin başarıyla aracılığıyla Azure AD kimlik doğrulama yapıyorsunuz olduğunu doğruladıktan sonra Office 365 bağlı olan taraf güveni kaldırmak güvenlidir.

Diğer amaçlar için AD FS kullanmıyorsanız (diğer bir deyişle, diğer bağlı olan taraf güveni için), AD FS bu noktada yetkisini almak güvenlidir.

### <a name="rollback"></a>Geri alma

Önemli bir sorun keşfedin ve hızlı bir şekilde çözümlenemiyor çözümü için Federasyon geri seçebilirsiniz.

Federasyon tasarım ve dağıtım için belirli dağıtım ayrıntıları belgelerine bakın. İşlem, bu görevleri içermelidir:

* Yönetilen etki alanlarını Federasyon kimlik doğrulaması kullanarak dönüştürme **dönüştürme MSOLDomainToFederated** cmdlet'i.
* Gerekirse, ek talep kurallarını yapılandırın.

### <a name="sync-userprincipalname-updates"></a>Eşitleme userPrincipalName güncelleştirmeleri

Tarihsel olarak, güncelleştirmeleri **UserPrincipalName** bu koşulların her ikisinin de doğru olduğu sürece şirket içi ortamdan eşitleme hizmeti kullanan bir öznitelik engellenir:

* Kullanıcı, bir yönetilen kimlik (Federasyon olmayan) etki alanında değil.
* Kullanıcının bir lisansı atanmamış.

Doğrulamak veya bu özelliği etkinleştirmek öğrenmek için bkz: [userPrincipalName güncelleştirmeleri eşitleme](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsyncservice-features).

### <a name="troubleshooting"></a>Sorun giderme

Sırasında veya değişiklikten sonra yönetilen için Federasyon seçeneğinden kaynaklanan kimlik doğrulama sorunları gidermek nasıl destek ekibinize anlamanız gerekir. Kendilerini tanıyın genel sorun giderme adımları ve yalıtmak ve sorunu çözmek için yardımcı olabilecek uygun eylemleri ile destek ekibinize yardımcı olması için aşağıdaki sorun giderme belgelerini kullanın.

[Azure Active Directory parola karması eşitleme sorunlarını giderme](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-troubleshoot-password-hash-synchronization)

[Azure Active Directory sorunsuz çoklu oturum açma sorunlarını giderme](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-troubleshoot-sso)

## <a name="roll-over-the-seamless-sso-kerberos-decryption-key"></a>Sorunsuz SSO Kerberos şifre çözme anahtarını başa döndürmek

Sık Kerberos şifre çözme anahtarı (Azure AD temsil eden) AZUREADSSOACC bilgisayar hesabının geri almak önemlidir. Şirket içi Active Directory ormanınızda AZUREADSSOACC bilgisayar hesabı oluşturulur. Active Directory etki alanı üyeleri parola değişiklikleri gönderir biçimindeki hizalamak için en azından her 30 gün Kerberos şifre çözme anahtarı alma önemle öneririz. El ile geçiş yapmanız gerekir böylece AZUREADSSOACC bilgisayar hesabı nesnesine ekli ilişkili cihaz yoktur.

Azure AD Connect'i çalıştıran şirket içi sunucuda sorunsuz SSO Kerberos şifre çözme anahtarı geçişi başlatın.

Daha fazla bilgi için [nasıl miyim geri AZUREADSSOACC bilgisayar hesabının Kerberos şifre çözme anahtarı?](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-sso-faq).

## <a name="next-steps"></a>Sonraki adımlar

* Hakkında bilgi edinin [Azure AD Connect tasarım kavramları](plan-connect-design-concepts.md).
* Seçin [doğru kimlik doğrulaması](https://docs.microsoft.com/azure/security/azure-ad-choose-authn).
* Hakkında bilgi edinin [desteklenen topolojiler](plan-connect-design-concepts.md).
