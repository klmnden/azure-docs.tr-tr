---
title: Azure Active Directory'deki şirket içi koşullu erişimi ayarlama | Microsoft Docs
description: Windows Server 2012 R2'de Active Directory Federasyon Hizmetleri (AD FS) kullanarak şirket içi uygulamalara koşullu erişimin etkinleştirilmesine yönelik adım adım yönergeler.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
ms.assetid: 6ae9df8b-31fe-4d72-9181-cf50cfebbf05
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/14/2017
ms.author: markvi
ms.reviewer: jairoc
ms.custom: seohack1
ms.openlocfilehash: 0ce4497a8bebf9078363509c1f962728ab4189f8
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
ms.locfileid: "33764215"
---
# <a name="setting-up-on-premises-conditional-access-by-using-azure-active-directory-device-registration"></a>Azure Active Directory cihaz kaydı kullanarak şirket içi koşullu erişimi ayarlama
Azure Active Directory (Azure AD) cihaz kayıt hizmeti için kişisel cihazlarını çalışma alanına katılma kullanıcılara ihtiyaç duyduğunuzda, cihazlarını kuruluşunuza bilinen olarak işaretlenebilir. Aşağıda, Windows Server 2012 R2'de Active Directory Federasyon Hizmetleri (AD FS) kullanarak şirket içi uygulamalara koşullu erişimi etkinleştirmek için adım adım yönergeler verilmektedir.

> [!NOTE]
> Bir Office 365 lisansı veya Azure AD Premium lisansı Azure Active Directory cihaz kayıt hizmeti koşullu erişim ilkelerini kayıtlı cihazlar kullanılırken gereklidir. Bunlar şirket içi kaynakları'de AD FS tarafından uygulanan ilkeler içerir.
> 
> Şirket içi kaynaklara koşullu erişim senaryoları hakkında daha fazla bilgi için bkz: [çalışma alanına herhangi bir aygıttan SSO ve sorunsuz ikinci faktör kimlik doğrulaması için şirket uygulamaları arasında katılma](https://technet.microsoft.com/library/dn280945.aspx).

Bu özellikler bir Azure Active Directory Premium lisansı satın müşteriler için kullanılabilir.

## <a name="supported-devices"></a>Desteklenen cihazlar
* Windows 7 etki alanına katılmış cihazlar
* Windows 8.1 kişisel ve etki alanına katılmış aygıtlar
* iOS 6 ve üzeri Safari tarayıcısı
* Android 4.0 ve üzeri, Samsung GS3 veya üzeri telefonları, Samsung Galaxy Not 2 veya sonraki tabletler

## <a name="scenario-prerequisites"></a>Senaryo önkoşulları
* Office 365 aboneliği veya Azure Active Directory Premium
* Azure Active Directory kiracısı
* Windows Server Active Directory (Windows Server 2008 veya üzeri)
* Windows Server 2012 R2'de güncelleştirilmiş şeması
* Azure Active Directory Premium lisansı
* Windows Server 2012 R2 Federasyon SSO Azure ad için yapılandırılmış hizmetleri,
* Windows Server 2012 R2 Web uygulaması proxy'si 
* Microsoft Azure Active Directory Connect (Azure AD Connect) [(Azure AD Connect'i indirme)](http://www.microsoft.com/en-us/download/details.aspx?id=47594)
* Doğrulanmış etki alanı

## <a name="known-issues-in-this-release"></a>Bu sürümdeki bilinen sorunlar
* Cihaz temelli koşullu erişim ilkeleri Azure Active Directory'den Active Directory'ye cihaz nesnesi geri yazma gerektirir. Cihaz nesneleri, Active Directory'ye geri yazılması üç saat sürebilir.
* iOS 7 aygıtları kullanıcı istemci sertifikası kimlik doğrulaması sırasında bir sertifika seçmek için her zaman sor.
* İOS 8 iOS 8.3 çalışmıyor önce bazı sürümleri.

## <a name="scenario-assumptions"></a>Senaryo varsayımlar
Bu senaryo, Azure AD kiracısı ve bir şirket içi Active Directory oluşan karma bir ortamınız sahip olduğunuzu varsayar. Bu kiracılar Azure AD Connect, doğrulanmış bir etki alanı ve AD FS için SSO bağlı olması gerekir. Ortamınızın gereksinimlerine göre yapılandırmanıza yardımcı olması için aşağıdaki denetim listesini kullanın.

## <a name="checklist-prerequisites-for-conditional-access-scenario"></a>Denetim listesi: Koşullu erişim senaryo Önkoşullar
Azure AD kiracınıza şirket içi Active Directory örneğinizle bağlayın.

## <a name="configure-azure-active-directory-device-registration-service"></a>Azure Active Directory cihaz kayıt hizmeti yapılandırma
Dağıtmak ve kuruluşunuz için Azure Active Directory cihaz Kayıt Hizmeti'ni yapılandırmak için bu kılavuzu kullanın.

Bu kılavuz, Windows Server Active Directory yapılandırdıysanız ve Microsoft Azure Active Directory'ye abone olduğunuz varsayılmaktadır. Daha önce açıklanan önkoşullara bakın.

Azure Active Directory kiracınızın Azure Active Directory cihaz kaydı hizmetiyle dağıtmak için aşağıdaki denetim sırada görevleri tamamlayın. Bir referans bağlantı sizi bir kavramsal konuya götürdüğünde ile kalan görevlere devam edebilirsiniz, bu denetim için daha sonra döndür. Bazı görevler, adım başarıyla tamamlandı doğrulamanıza yardımcı olabilir bir senaryo doğrulama adımı içerir.

## <a name="part-1-enable-azure-active-directory-device-registration"></a>1. Kısım: Etkinleştir Azure Active Directory cihaz kaydı
Denetim etkinleştirmek ve Azure Active Directory cihaz Kayıt Hizmeti'ni yapılandırmak için adımları izleyin.

| Görev | Başvuru | 
| --- | --- |
| Azure Active Directory kiracınızda aygıtların çalışma alanına katılma olanak tanımak için cihaz kaydını etkinleştirin. Varsayılan olarak, Azure multi-Factor Authentication hizmeti için etkin değil. Ancak, bir cihaz kaydettiğinizde, çok faktörlü kimlik doğrulaması kullanmanızı öneririz. Active Directory kayıt hizmetinde çok faktörlü kimlik doğrulamasını etkinleştirmeden önce AD FS çok faktörlü kimlik doğrulama sağlayıcısı için yapılandırıldığından emin olun. |[Azure Active Directory cihaz kaydı etkinleştirme](active-directory-device-registration-get-started.md)| 
|Cihazlar, iyi bilinen DNS kayıtlarına bakarak, Azure Active Directory cihaz kayıt hizmeti bulur. Böylece, Azure Active Directory cihaz kayıt hizmeti aygıtları bulabilir, şirketinizin DNS yapılandırın. |[Azure Active Directory cihaz kaydı keşfini yapılandırma](active-directory-device-registration-get-started.md)| 


## <a name="part-2-deploy-and-configure-windows-server-2012-r2-active-directory-federation-services-and-set-up-a-federation-relationship-with-azure-ad"></a>2. Kısım: Dağıtın ve Windows Server 2012 R2 Active Directory Federasyon Hizmetleri Yapılandırma ve Azure AD ile bir Federasyon ilişkisi ayarlayın

| Görev | Başvuru |
| --- | --- |
| Active Directory etki alanı Hizmetleri, Windows Server 2012 R2 şema uzantılarıyla dağıtın. Herhangi bir etki alanı denetleyicileriniz Windows Server 2012 R2 için yükseltme gerekmez. Şema yükseltmesi yalnızca bir gereksinimdir. |[Active Directory etki alanı Hizmetleri şeması yükseltme](#upgrade-your-active-directory-domain-services-schema) |
| Cihazlar, iyi bilinen DNS kayıtlarına bakarak, Azure Active Directory cihaz kayıt hizmeti bulur. Böylece, Azure Active Directory cihaz kayıt hizmeti aygıtları bulabilir, şirketinizin DNS yapılandırın. |[Active Directory destek aygıtlarınızı hazırlama](#prepare-your-active-directory-to-support-devices) |

## <a name="part-3-enable-device-writeback-in-azure-ad"></a>3. Kısım: Etkinleştir cihaz geri yazma özelliğini Azure AD
| Görev | Başvuru |
| --- | --- |
| "Etkinleştirme cihaz geri yazma özelliğini Azure AD Connect." iki bölümü tamamlayın Bitirdiğinizde, bu Kılavuzu'na dönün. |[Azure AD Connect’te cihaz geri yazma özelliğini etkinleştirme](./connect/active-directory-aadconnect-feature-device-writeback.md) |

## <a name="optional-part-4-enable-multi-factor-authentication"></a>[İsteğe bağlı] 4. Kısım: Etkinleştir çok faktörlü kimlik doğrulaması
Çok faktörlü kimlik doğrulaması için çeşitli seçenekler birini yapılandırmanız önerilir. Çok faktörlü kimlik doğrulaması gerektiren istiyorsanız, bkz: [çok faktörlü kimlik doğrulaması güvenliği çözümü seçtiğiniz](authentication/concept-mfa-whichversion.md). Her çözüm ve tercih ettiğiniz çözüm yapılandırmanıza yardımcı olması için bağlantıları açıklamasını içerir.

## <a name="part-5-verification"></a>Bölüm 5: doğrulama
Dağıtımı tamamlanmıştır ve bazı senaryolar deneyebilirsiniz. Hizmet ile denemek ve özellikleri ile tanımak için aşağıdaki bağlantıları kullanın.

| Görev | Başvuru |
| --- | --- |
| Bazı aygıtlar, Azure Active Directory cihaz kayıt hizmeti kullanarak çalışma alanınıza ekleyin. İOS, Windows ve Android cihazlarını birleştirebilirsiniz. |[Cihazların Azure Active Directory cihaz Kayıt Hizmeti'ni kullanarak, çalışma alanına katılma](#join-devices-to-your-workplace-using-azure-active-directory-device-registration) |
| Görüntülemek ve etkinleştirme veya Yönetici portalı'nı kullanarak kayıtlı cihazları devre dışı bırakın. Bu görevde, Yönetici portalı'nı kullanarak bazı kayıtlı cihazları görüntüleyin. |[Azure Active Directory cihaz kayıt hizmetine genel bakış](active-directory-device-registration-get-started.md) |
| Windows Server Active Directory için Azure Active Directory'den, aygıt nesneleri geri yazılır doğrulayın. |[Kayıtlı cihazlar için Active Directory geri yazılır doğrulayın](#verify-registered-devices-are-written-back-to-active-directory) |
| Kullanıcılar, cihazlarını kaydedebilir, uygulama oluşturabileceğiniz erişim ilkeleri yalnızca kayıtlı cihazlara izin AD FS'de. Bu görevde, bir uygulama erişim kuralı ve özel bir erişim reddedildi iletisi oluşturun. |[Bir uygulama erişim ilkesi ve özel erişim reddedildi iletisi oluştur](#create-an-application-access-policy-and-custom-access-denied-message) |

## <a name="integrate-azure-active-directory-with-on-premises-active-directory"></a>Azure Active Directory şirket içi Active Directory ile tümleştirme

**Bkz.:**

- [Şirket içi dizinlerinizi Azure Active Directory ile tümleştirme](./connect/active-directory-aadconnect.md) - kavramsal bilgileri gözden geçirin.

- [Azure AD Connect özel yüklemesi](./connect/active-directory-aadconnect-get-started-custom.md) - yükleme yönergeleri için.


## <a name="upgrade-your-active-directory-domain-services-schema"></a>Active Directory etki alanı Hizmetleri şeması yükseltme
> [!NOTE]
> Active Directory şemanızı yükselttikten sonra işlem tersine çevrilemez. Bir test ortamında bir yükseltme gerçekleştirmenizi öneririz.
> 

1. Etki alanı denetleyicinizi kuruluş yöneticisi ve şema yönetici haklarına sahip bir hesapla oturum açın.
2. Kopya **[media] \support\adprep** directory ve Active Directory etki alanı denetleyicilerinden biri alt dizinlere (burada **[media]** Windows Server 2012 R2 yükleme medyasını yolu).
4. Bir komut isteminden Git **adprep** dizin ve çalışma **adprep.exe/forestprep**. Şema yükseltmeyi tamamlamak için ekrandaki yönergeleri izleyin.

## <a name="prepare-your-active-directory-to-support-devices"></a>Cihazları desteklemek için Active Directory hazırlama
> [!NOTE]
> Bu, Active Directory ormanınızın aygıtları destekleyecek şekilde hazırlamak için çalıştırmanız gereken tek seferlik bir işlemdir. Bu yordamı tamamlamak için kuruluş yönetici izinlerine oturum imzalanır ve Active Directory ormanınızın Windows Server 2012 R2 şema olması gerekir.
> 


### <a name="prepare-your-active-directory-forest"></a>Active Directory ormanı hazırlama
1. Federasyon sunucunuzda, Windows PowerShell komut penceresi açın ve ardından yazın **Initialize-ADDeviceRegistration**. 
2. İstendiğinde **ServiceAccountName**, AD FS için hizmet hesabı olarak seçilen hizmet hesabının adını girin. GMSA hesabı ise, hesap girin **domain\accountname$** biçimi. Bir etki alanı hesabı için biçimini kullanın **domain\accountname**.

### <a name="enable-device-authentication-in-ad-fs"></a>AD FS'de cihaz kimlik doğrulamasını etkinleştir
1. Federasyon sunucunuzda, AD FS Yönetimi konsolunu açın ve gidin **AD FS** > **kimlik doğrulama ilkeleri**.
2. Üzerinde **Eylemler** bölmesinde, **Düzenle Genel birincil kimlik doğrulama**.
3. Denetleme **cihaz kimlik doğrulamasını etkinleştir**ve ardından **Tamam**.
4. Varsayılan olarak, AD FS, kullanılmayan aygıtlarını düzenli olarak Active Directory'den kaldırır. Böylece cihazlar Azure'da yönetilebilir Azure Active Directory cihaz kayıt hizmeti kullanırken bu görevi devre dışı bırakın.

### <a name="disable-unused-device-cleanup"></a>Kullanılmayan cihaz temizleme devre dışı bırak
Federasyon sunucunuzda, Windows PowerShell komut penceresi açın ve ardından yazın **kümesi AdfsDeviceRegistration - MaximumInactiveDays 0**.

### <a name="prepare-azure-ad-connect-for-device-writeback"></a>Cihaz geri yazma için Azure AD Connect'i hazırlama
Bölüm 1 tamamlamak: Azure AD Connect hazırlayın.

## <a name="join-devices-to-your-workplace-by-using-azure-active-directory-device-registration-service"></a>Azure Active Directory cihaz kayıt hizmeti kullanarak cihazları çalışma alanına katılma

### <a name="join-an-ios-device-by-using-azure-active-directory-device-registration"></a>Azure Active Directory cihaz kaydı kullanarak bir iOS aygıtı birleştirme
Azure Active Directory cihaz kaydı iOS cihazları için Over-the-Air profili kayıt işlemi kullanır. Kullanıcı profili kayıt URL'si Safari ile bağlandığında bu işlemi başlar. URL biçimi aşağıdaki gibidir:

    https://enterpriseregistration.windows.net/enrollmentserver/otaprofile/"yourdomainname"

Bu durumda, `yourdomainname` Azure Active Directory ile yapılandırılmış bir etki alanı adıdır. Etki alanı adınız contoso.com ise, örneğin, URL şu şekildedir:

    https://enterpriseregistration.windows.net/enrollmentserver/otaprofile/contoso.com

Bu URL'yi, kullanıcılarınıza iletmenin birçok farklı yolu vardır. Örneğin, bir yöntem öneririz bu URL'yi AD FS'de bir özel uygulama erişim reddedildi iletisi yayımlama. Bu bilgiler ilerideki bölümde ele [bir uygulama erişim ilkesi ve özel erişim reddedildi iletisi Oluştur](#create-an-application-access-policy-and-custom-access-denied-message).

### <a name="join-a-windows-81-device-by-using-azure-active-directory-device-registration"></a>Azure Active Directory cihaz kaydı kullanarak bir Windows 8.1 cihaz birleştirme
1. Windows 8.1 Cihazınızı seçin **PC Ayarları** > **ağ** > **çalışma alanına**.
2. Kullanıcı adınızı UPN biçiminde girin; Örneğin, **dan@contoso.com**.
3. Seçin **katılma**.
4. İstendiğinde, kimlik bilgilerinizle oturum açın. Cihaz artık birleştirilir.

### <a name="join-a-windows-7-device-by-using-azure-active-directory-device-registration"></a>Azure Active Directory cihaz kaydı kullanarak bir Windows 7 aygıtı birleştirme
Windows 7 etki alanına katılmış cihazları kaydetmek için dağıtmak gereken [cihaz kayıt yazılım paketini](https://www.microsoft.com/download/details.aspx?id=53554).

Paketin nasıl kullanılacağı hakkında yönergeler için bkz: [Windows 10 bilgisayarları için Windows Installer paketleri](device-management-hybrid-azuread-joined-devices-setup.md#windows-installer-packages-for-non-windows-10-computers).

## <a name="verify-that-registered-devices-are-written-back-to-active-directory"></a>Kayıtlı cihazlar için Active Directory geri yazılır doğrulayın
Görüntüleyebilir ve aygıt nesneleri geri Active Directory'ye LDP.exe veya ADSI Düzenleyicisi kullanılarak yazılan olduğunu doğrulayın. Her ikisi de, Active Directory Yöneticisi Araçları ile kullanılabilir.

Varsayılan olarak, Azure Active Directory'den geri yazılır aygıt nesneleri AD FS grubunuzun aynı etki alanında yerleştirilir.

    CN=RegisteredDevices,defaultNamingContext

## <a name="create-an-application-access-policy-and-custom-access-denied-message"></a>Bir uygulama erişim ilkesi ve özel erişim reddedildi iletisi oluştur
Aşağıdaki senaryoyu düşünün: AD FS'de bağlı taraf güveni uygulama oluşturma ve yalnızca kayıtlı cihazlara izin veren bir verme yetkilendirme kuralı yapılandırın. Şimdi, kayıtlı cihazların uygulamaya erişmesine izin verilir. 

Uygulamaya erişmek, kullanıcılarınızın kolaylaştırmak için cihazlarını katılmaya hakkında yönergeler içeren bir özel erişim reddedildi iletisi yapılandırın. Şimdi, kullanıcılarınızın bir uygulama erişebilmek için cihazlarını kaydetmek için sorunsuz bir yolu yoktur.

Aşağıdaki adımlar bu senaryonun nasıl uygulanacağını gösterir.

> [!NOTE]
> Bu bölümde, zaten bir bağlı olan taraf güveni için AD FS uygulamanızda yapılandırmış olduğunuz varsayılır.
> 

1. AD FS MMC Aracı'nı açın ve ardından **AD FS** > **güven ilişkileri** > **bağlı olan taraf güvenleri**.
2. Bu yeni erişim kuralının uygulanacağı uygulamasını bulun. Uygulamayı sağ tıklatın ve ardından **talep kurallarını Düzenle**.
3. Seçin **verme yetkilendirme kuralları** sekmesini tıklatın ve ardından **Kuralı Ekle**.
4. Gelen **talep kuralı** şablonu aşağı açılan listesinden, **göre izin ver veya Reddet kullanıcılar bir gelen talep**. Sonra **İleri**’yi seçin.
5. İçinde **talep kuralı adı** alanında, yazın **kayıtlı cihazlardan erişim izin**.
6. Gelen **gelen talep türü** aşağı açılan listesinden, **kayıtlı kullanıcı**.
7. İçinde **gelen talep değeri** alanında, yazın **doğru**.
8. Seçin **bu gelen talep ile kullanıcılara erişim izin** radyo düğmesi.
9. Seçin **son**ve ardından **Uygula**.
10. Oluşturduğunuz kural daha fazla izin veren herhangi bir kuralın kaldırın. Örneğin, varsayılan kuralı kaldırmak **tüm kullanıcılara izin erişimi**.

Uygulamanız artık yalnızca, kayıtlı ve çalışma alanına katılmış bir cihazda kullanıcı geldiği zaman erişime izin verecek şekilde yapılandırılmıştır. Daha gelişmiş erişim ilkeleri için bkz: [duyarlı uygulamalar için ek multi Factor Authentication ile Risk Yönetimi](https://technet.microsoft.com/library/dn280949.aspx).

Ardından, uygulamanız için bir özel hata iletisi yapılandırın. Hata iletisi uygulamaya erişebilmeniz bunlar cihazlarını çalışma alanına katılmalı bilmenize olanak sağlar. Özel HTML ve PowerShell kullanarak bir özel uygulama erişim reddedildi iletisi oluşturabilirsiniz.

Federasyon sunucunuzda bir PowerShell komut penceresi açın ve aşağıdaki komutu yazın. Komut bölümlerini sisteme özgü öğeleri ile değiştirin:

    Set-AdfsRelyingPartyWebContent -Name "relying party trust name" -ErrorPageAuthorizationErrorMessage
Bu uygulamaya erişebilmeniz için Cihazınızı kaydetmeniz gerekir.

**İOS cihazı kullanıyorsanız, cihazınız katılmak için bu bağlantıyı seçin**:

    a href='https://enterpriseregistration.windows.net/enrollmentserver/otaprofile/yourdomain.com

Bu iOS cihazı çalışma alanınıza ekleyin.

Windows 8.1 cihaz kullanıyorsanız, seçerek Cihazınızı katılabilirsiniz **PC Ayarları**> **ağ** > **çalışma alanına**.

Yukarıdaki komutlarda **bağlı olan taraf güven adı** AD FS'de, uygulamanızın bağlı olan taraf güveni nesnesinin adı.
Ve **etkialaniniz.com** Azure Active Directory (örneğin, contoso.com) ile yapılandırılmış bir etki alanı adıdır.
Satır sonları (varsa) için geçirdiğiniz HTML içeriğini kaldırmak mutlaka **kümesi AdfsRelyingPartyWebContent** cmdlet'i.

Artık kullanıcılar uygulamanızı Azure Active Directory cihaz kaydı hizmetiyle kaydedilmemiş bir CİHAZDAN eriştiklerinde hataya bakın.

