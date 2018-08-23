---
title: Azure Active Directory'de şirket içi koşullu erişimi ayarlama | Microsoft Docs
description: Windows Server 2012 R2'de Active Directory Federasyon Hizmetleri (AD FS) kullanarak şirket içi uygulamalara koşullu erişimi etkinleştirmek için adım adım bir kılavuz.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
ms.component: devices
ms.assetid: 6ae9df8b-31fe-4d72-9181-cf50cfebbf05
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/23/2018
ms.author: markvi
ms.reviewer: jairoc
ms.custom: seohack1
ms.openlocfilehash: 38d024de0fd2490d33f7c06498d3ff8d0d06e503
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2018
ms.locfileid: "42059666"
---
# <a name="setting-up-on-premises-conditional-access-by-using-azure-active-directory-device-registration"></a>Azure Active Directory cihaz kaydı'nı kullanarak şirket içi koşullu erişimi ayarlama
Azure Active Directory (Azure AD) cihaz kayıt hizmeti için kişisel cihazlarını çalışma alanına katılma kullanıcılara ihtiyaç duyduğunuzda, cihazlarını kuruluşunuza bilinen olarak işaretlenebilir. Aşağıda, Windows Server 2012 R2'de Active Directory Federasyon Hizmetleri (AD FS) kullanarak şirket içi uygulamalara koşullu erişimi etkinleştirmek için adım adım yönergeler verilmektedir.

> [!NOTE]
> Azure Active Directory cihaz kaydı hizmeti koşullu erişim ilkeleri kayıtlı cihazlar kullanılırken bir Office 365 lisansı veya Azure AD Premium lisansı gereklidir. Bunlar, şirket içi kaynaklara'de AD FS tarafından zorlanan ilkeler içerir.
> 
> Şirket içi kaynaklar için koşullu erişim senaryoları hakkında daha fazla bilgi için bkz: [çalışma alanına herhangi bir CİHAZDAN SSO ve sorunsuz ikinci faktör kimlik doğrulaması için şirket uygulamaları arasında katılma](https://technet.microsoft.com/library/dn280945.aspx).

Bu özellikler bir Azure Active Directory Premium lisansı satın almış olan müşteriler için kullanılabilir.

## <a name="supported-devices"></a>Desteklenen cihazlar
* Windows 7 etki alanına katılmış cihazlar
* Windows 8.1 kişisel ve etki alanına katılmış cihazlar
* iOS 6 ve üzeri için Safari tarayıcısı
* Android 4.0 ve üzeri, Samsung GS3 veya üzeri telefonlar, Samsung Galaxy Not 2 veya üzeri tabletler

## <a name="scenario-prerequisites"></a>Senaryo önkoşulları
* Office 365 aboneliği veya Azure Active Directory Premium
* Azure Active Directory kiracısı
* Windows Server Active Directory (Windows Server 2008 veya üzeri)
* Windows Server 2012 R2'de güncelleştirilen şeması
* Azure Active Directory Premium lisansı
* Windows Server 2012 R2 Federasyon Azure ad SSO için yapılandırılmış hizmetleri,
* Windows Server 2012 R2 Web uygulaması Ara sunucusu 
* Microsoft Azure Active Directory Connect (Azure AD Connect) [(Azure AD Connect'i indirme)](http://www.microsoft.com/download/details.aspx?id=47594)
* Doğrulanmış etki alanı

## <a name="known-issues-in-this-release"></a>Bu sürümdeki bilinen sorunlar
* Cihaz tabanlı koşullu erişim ilkeleri, Azure Active Directory'den Active Directory'ye cihaz nesnesi geri yazma gerektirir. Bu cihaz nesneleri, Active Directory'ye geri yazılması üç saate kadar sürebilir.
* iOS 7 cihazları kullanıcı istemci sertifikası kimlik doğrulaması sırasında bir sertifika seçmek için her zaman sor.
* İOS 8 iOS 8.3 çalışmıyor önce bazı sürümleri.

## <a name="scenario-assumptions"></a>Senaryo varsayımlar
Bu senaryo, Azure AD kiracısı ve bir şirket içi Active Directory oluşan karma bir ortamda sahibi olduğunuzu varsayar. Bu Kiracı Azure AD Connect, doğrulanmış bir etki alanı ve AD FS için SSO bağlanması gerekir. Gereksinimlerine göre ortamınızı yapılandırmanıza yardımcı olması için aşağıdaki denetim listesini kullanın.

## <a name="checklist-prerequisites-for-conditional-access-scenario"></a>Denetim listesi: Koşullu erişim senaryosu için Önkoşullar
Azure AD kiracınız ile şirket içi Active Directory örneğinizi bağlayın.

## <a name="configure-azure-active-directory-device-registration-service"></a>Azure Active Directory cihaz kayıt hizmeti yapılandırma
Kuruluşunuz için Azure Active Directory cihaz Kayıt Hizmeti'ni yapılandırmak ve dağıtmak için bu kılavuzu kullanın.

Bu kılavuzda, Windows Server Active Directory yapılandırdıktan ve Microsoft Azure Active Directory'ye abone olduğunuz varsayılmaktadır. Daha önce açıklanan önkoşullara bakın.

Azure Active Directory kiracınız ile Azure Active Directory cihaz kayıt hizmeti dağıtmak için aşağıdaki denetim sırada görevleri tamamlayın. Ne zaman bir referans bağlantı sizi bir kavramsal konuya götürür, daha sonra kalan görevlere devam edebilmeniz adına bu denetim listesine geri dönün. Bazı görevler adım başarıyla tamamlanıp tamamlanmadığını doğrulayın yardımcı olabilecek bir senaryo doğrulama adımı içerir.

## <a name="part-1-enable-azure-active-directory-device-registration"></a>1. Bölüm: Enable Azure Active Directory cihaz kaydı
Etkinleştirmek ve Azure Active Directory cihaz Kayıt Hizmeti'ni yapılandırmak için denetim listesindeki adımları izleyin.

| Görev | Başvuru | 
| --- | --- |
| Azure Active Directory kiracınızdaki cihazların çalışma alanına katılma izin vermek için cihaz kaydını etkinleştirin. Varsayılan olarak, Azure multi-Factor Authentication hizmeti için etkin değil. Ancak, bir cihaz kaydettiğinizde, çok faktörlü kimlik doğrulaması kullanmanızı öneririz. Active Directory kayıt hizmetinde çok faktörlü kimlik doğrulamasını etkinleştirmeden önce AD FS için multi-Factor Authentication sağlayıcısı yapılandırıldığından emin olun. |[Azure Active Directory cihaz kaydı etkinleştirme](active-directory-device-registration-get-started.md)| 
|Cihazları Azure Active Directory cihaz kaydı hizmetiniz, iyi bilinen DNS kayıtlarına bakarak keşfedin. Cihazlar, Azure Active Directory cihaz kayıt hizmeti keşfedebilmesi için şirket DNS'nizi yapılandırın. |[Azure Active Directory cihaz kaydı keşfini yapılandırma](active-directory-device-registration-get-started.md)| 


## <a name="part-2-deploy-and-configure-windows-server-2012-r2-active-directory-federation-services-and-set-up-a-federation-relationship-with-azure-ad"></a>2. Bölüm: Dağıtın ve Windows Server 2012 R2 Active Directory Federasyon Hizmetleri Yapılandırma ve Azure AD ile bir Federasyon ilişkisi ayarlayın

| Görev | Başvuru |
| --- | --- |
| Active Directory etki alanı Hizmetleri ile Windows Server 2012 R2 şema uzantılarını dağıtın. Herhangi bir etki alanı denetleyicileriniz Windows Server 2012 R2 için yükseltme gerekmez. Şema yükseltme tek gereksinim olmasıdır. |[Active Directory Domain Services şemanızı yükseltme](#upgrade-your-active-directory-domain-services-schema) |
| Cihazları Azure Active Directory cihaz kaydı hizmetiniz, iyi bilinen DNS kayıtlarına bakarak keşfedin. Cihazlar, Azure Active Directory cihaz kayıt hizmeti keşfedebilmesi için şirket DNS'nizi yapılandırın. |[Active Directory desteği cihazlarınızı hazırlama](#prepare-your-active-directory-to-support-devices) |

## <a name="part-3-enable-device-writeback-in-azure-ad"></a>3. Bölüm: Azure AD'de etkin cihaz geri yazma
| Görev | Başvuru |
| --- | --- |
| "Etkinleştirme cihaz geri yazma özelliğini Azure AD CONNECT'te." iki bölümü tamamlayın Tamamladığınızda, bu kılavuza döndürür. |[Azure AD Connect’te cihaz geri yazma özelliğini etkinleştirme](./connect/active-directory-aadconnect-feature-device-writeback.md) |

## <a name="optional-part-4-enable-multi-factor-authentication"></a>[İsteğe bağlı] 4. Bölüm: Enable çok faktörlü kimlik doğrulaması
Multi-Factor Authentication'a ilişkin birkaç seçenekten birini yapılandırmanız önerilir. Çok faktörlü kimlik doğrulaması gerektiren istiyorsanız, bkz. [sizin için multi-Factor Authentication güvenlik çözümünüzü seçin](authentication/concept-mfa-whichversion.md). Bu, her çözüm ve çözüm, tercih ettiğiniz yapılandırmanıza yardımcı olması için bağlantılar açıklamasını içerir.

## <a name="part-5-verification"></a>5. Bölüm: doğrulama
Dağıtım tamamlandı, ve bazı senaryolarını deneyebilirsiniz. Hizmet ile denemeler yapın ve özellikleri ile aşina olmak için aşağıdaki bağlantıları kullanın.

| Görev | Başvuru |
| --- | --- |
| Bazı cihazlar, Azure Active Directory cihaz kayıt hizmeti kullanarak, çalışma alanına katılma. İOS, Windows ve Android cihazlarda katılabilirsiniz. |[Cihazları Azure Active Directory cihaz kayıt hizmeti kullanarak, çalışma alanına katılma](#join-devices-to-your-workplace-using-azure-active-directory-device-registration) |
| Görüntüleme ve etkinleştirme veya Yönetici portalını kullanarak kayıtlı cihazları devre dışı bırakın. Bu görevde Yönetici portalını kullanarak bazı kayıtlı cihazları görüntüleyin. |[Azure Active Directory cihaz kayıt hizmetine genel bakış](active-directory-device-registration-get-started.md) |
| Windows Server Active Directory için Azure Active Directory'den, cihaz nesneleri geri yazılır doğrulayın. |[Kayıtlı cihazlar için Active Directory geri yazılır doğrulayın](#verify-registered-devices-are-written-back-to-active-directory) |
| Kullanıcılar cihazlarını kaydedebilir, uygulama oluşturabileceğiniz erişim ilkeleri yalnızca kayıtlı cihazlara izin ver'de AD FS. Bu görevde, bir uygulama erişim kuralı ve bir özel erişim reddedildi iletisi oluşturun. |[Bir uygulama erişim ilkesi ve özel erişim reddedildi iletisi oluştur](#create-an-application-access-policy-and-custom-access-denied-message) |

## <a name="integrate-azure-active-directory-with-on-premises-active-directory"></a>Azure Active Directory, şirket içi Active Directory ile tümleştirme

**Bkz:**

- [Şirket içi dizinlerinizi Azure Active Directory ile tümleştirme](./connect/active-directory-aadconnect.md) - kavramsal bilgileri gözden geçirin.

- [Azure AD Connect özel yüklemesi](./connect/active-directory-aadconnect-get-started-custom.md) - yükleme yönergeleri.


## <a name="upgrade-your-active-directory-domain-services-schema"></a>Active Directory Domain Services şemanızı yükseltme
> [!NOTE]
> Active Directory şemanızı yükselttikten sonra işlem geri alınamaz. Bir test ortamında bir yükseltme almanızı öneririz.
> 

1. Etki alanı denetleyicisine hem kuruluş yöneticisi ve şema yönetici haklarına sahip bir hesap ile oturum açın.
2. Kopyalama **[media] \support\adprep** directory ve Active Directory etki alanı denetleyicilerinizden biri alt dizinlere (burada **[media]** yolu Windows Server 2012 R2 yükleme medyasına).
4. Bir komut isteminden Git **adprep** dizin ve çalışma **adprep.exe/forestprep**. Şema yükseltmeyi tamamlamak için ekrandaki yönergeleri izleyin.

## <a name="prepare-your-active-directory-to-support-devices"></a>Active Directory'nizde cihazları destekleyecek şekilde hazırlama
> [!NOTE]
> Bu, Active Directory ormanınızın aygıtları destekleyecek şekilde hazırlamak için çalıştırılması gereken tek seferlik bir işlemdir. Bu yordamı tamamlamak için Kurumsal Yönetici izinleri ile oturum açmış ve Active Directory ormanınızda Windows Server 2012 R2 şema olması gerekir.
> 


### <a name="prepare-your-active-directory-forest"></a>Active Directory ormanınızda hazırlama
1. Federasyon sunucunuzda, Windows PowerShell komut penceresi açın ve ardından yazın **başlatma ADDeviceRegistration**. 
2. İstendiğinde **ServiceAccountName**, seçtiğiniz hizmet hesabı olarak AD FS için hizmet hesabı adını girin. GMSA hesabı ise, hesap girin **domain\accountname$** biçimi. Bir etki alanı hesabı için biçimini kullanın **domain\accountname**.

### <a name="enable-device-authentication-in-ad-fs"></a>AD FS'de cihaz kimlik doğrulamasını etkinleştir
1. Federasyon sunucunuzda AD FS yönetim konsolunu açın ve gidin **AD FS** > **kimlik doğrulama ilkeleri**.
2. Üzerinde **eylemleri** bölmesinde **genel birincil kimlik doğrulamasını Düzenle**.
3. Denetleme **cihaz kimlik doğrulamasını etkinleştir**ve ardından **Tamam**.
4. Varsayılan olarak, AD FS, kullanılmayan cihazları düzenli olarak Active Directory'den kaldırır. Böylece Azure'da cihazlar yönetilebilir Azure Active Directory cihaz Kayıt Hizmeti'ni kullanıyorsanız, bu görevi devre dışı bırakın.

### <a name="disable-unused-device-cleanup"></a>Kullanılmayan cihaz temizleme devre dışı bırak
Federasyon sunucunuzda, Windows PowerShell komut penceresi açın ve ardından yazın **AdfsDeviceRegistration Set - MaximumInactiveDays 0**.

### <a name="prepare-azure-ad-connect-for-device-writeback"></a>Cihaz geri yazma için Azure AD Connect'i hazırlama
1. Bölüm tamamlayın: Azure AD Connect'i hazırlama.

## <a name="join-devices-to-your-workplace-by-using-azure-active-directory-device-registration-service"></a>Azure Active Directory cihaz kayıt hizmeti kullanarak cihazları çalışma alanınıza katılma

### <a name="join-an-ios-device-by-using-azure-active-directory-device-registration"></a>Azure Active Directory cihaz kaydı'nı kullanarak bir iOS cihazı katılın
Azure Active Directory cihaz kaydı iOS cihazları için Over-the-Air profili kayıt işlemi kullanır. Kullanıcı profili kayıt URL'si Safari ile bağlandığında bu işlemi başlar. URL biçimi aşağıdaki gibidir:

    https://enterpriseregistration.windows.net/enrollmentserver/otaprofile/"yourdomainname"

Bu durumda, `yourdomainname` Azure Active Directory ile yapılandırdığınız etki alanı adıdır. Etki alanı adınız contoso.com ise, örneğin, URL şu şekildedir:

    https://enterpriseregistration.windows.net/enrollmentserver/otaprofile/contoso.com

Bu URL'yi, kullanıcılarınıza iletmenin birçok farklı yolu vardır. Örneğin, bir yöntem öneririz, bu URL'yi AD FS'de özel uygulama erişimi reddedildi iletisinde yayımlıyor. Bu bilgiler gelecek bölümde ele alınmıştır [bir uygulama erişim ilkesi oluşturup özel erişim reddedildi iletisi](#create-an-application-access-policy-and-custom-access-denied-message).

### <a name="join-a-windows-81-device-by-using-azure-active-directory-device-registration"></a>Azure Active Directory cihaz kaydı'nı kullanarak Windows 8.1 cihaz ekleme
1. Windows 8.1 Cihazınızı seçin **PC Ayarları** > **ağ** > **çalışma alanına**.
2. Kullanıcı adı UPN biçiminde girin; Örneğin, **dan@contoso.com**.
3. Seçin **katılın**.
4. İstendiğinde, kimlik bilgilerinizle oturum açın. Cihaz artık katıldı.

### <a name="join-a-windows-7-device-by-using-azure-active-directory-device-registration"></a>Azure Active Directory cihaz kaydı'nı kullanarak Windows 7 cihaz ekleme
Windows 7 etki alanına katılmış cihazları kaydetmek için dağıtmak için ihtiyacınız [cihaz kayıt yazılım paketini](https://www.microsoft.com/download/details.aspx?id=53554).

Paketin nasıl kullanılacağı hakkında yönergeler için bkz: [Windows 10 bilgisayarları için Windows Installer paketleri](devices/hybrid-azuread-join-control.md#control-windows-down-level-devices).

## <a name="verify-that-registered-devices-are-written-back-to-active-directory"></a>Kayıtlı cihazlar için Active Directory geri yazılır doğrulayın
Görüntüleyebilir ve cihazı nesnelerinizin geri Active Directory'nize LDP.exe veya ADSI Edit kullanarak yazılmış olduğunu doğrulayın. Her ikisi de, Active Directory Yöneticisi Araçları ile kullanılabilir.

Varsayılan olarak, Azure Active Directory'den geri yazılır cihaz nesneleri, AD FS grubunuzun aynı etki alanında yerleştirilir.

    CN=RegisteredDevices,defaultNamingContext

## <a name="create-an-application-access-policy-and-custom-access-denied-message"></a>Bir uygulama erişim ilkesi ve özel erişim reddedildi iletisi oluştur
Aşağıdaki senaryoları düşünün: bağlı taraf güveni AD FS'de uygulama oluşturma ve yalnızca kayıtlı cihazlara izin veren bir verme yetkilendirme kuralı yapılandırma. Artık yalnızca kayıtlı cihazlar, uygulamaya erişmesine izin verilir. 

Kullanıcılarınızın uygulamaya erişmeniz kolaylaştırmak için cihazını katılmaya ilişkin yönergeleri içeren bir özel erişim reddedildi iletisi yapılandırın. Artık, kullanıcıların uygulamaya erişebilmek için cihazlarını kaydetmek için sorunsuz bir yöntem var.

Aşağıdaki adımlar bu senaryonun nasıl uygulanacağını gösterir.

> [!NOTE]
> Bu bölümde, zaten bir bağlı olan taraf güveni AD FS'de uygulamanız için yapılandırmış olduğunuz varsayılır.
> 

1. AD FS MMC Aracı'nı açın ve ardından **AD FS** > **güven ilişkileri** > **bağlı olan taraf güvenleri**.
2. Bu yeni bir erişim kuralı uygulandığı uygulamayı bulun. Uygulamaya sağ tıklayın ve ardından **talep kurallarını Düzenle**.
3. Seçin **verme yetkilendirme kuralları** sekmesine tıklayın ve ardından **Kuralı Ekle**.
4. Gelen **talep kuralı** şablon aşağı açılan listesinden **kullanıcılara izin ver veya Reddet üzerinde bir gelen talep**. Sonra **İleri**’yi seçin.
5. İçinde **talep kuralı adı** alanına **kayıtlı cihazlardan erişim izin**.
6. Gelen **gelen talep türü** aşağı açılan listesinden **kayıtlı kullanıcı**.
7. İçinde **gelen talep değeri** alanına **true**.
8. Seçin **bu gelen talep ile kullanıcılara erişim izin** radyo düğmesi.
9. Seçin **son**ve ardından **Uygula**.
10. Oluşturduğunuz kural daha esnek olan tüm kuralları kaldırın. Örneğin, varsayılan kuralı kaldırmak **tüm kullanıcılara erişim izin**.

Uygulamanız artık yalnızca kullanıcının kayıtlı ve çalışma alanına katılmış bir CİHAZDAN geliyorsa, erişime izin verecek şekilde yapılandırılır. Daha gelişmiş erişim ilkeleri için bkz: [duyarlı uygulamalar için ek multi Factor Authentication ile Risk Yönetimi](https://technet.microsoft.com/library/dn280949.aspx).

Ardından, uygulamanız için bir özel hata iletisi yapılandırın. Hata iletisi, kullanıcıların uygulamaya erişebilmesi için önce cihazlarını çalışma alanına katılmaları gerekir olduğunu bilmek olanak sağlar. Özel HTML ve PowerShell kullanarak bir özel uygulama erişim reddedildi iletisi oluşturabilirsiniz.

Federasyon sunucunuzda bir PowerShell komut penceresi açın ve ardından aşağıdaki komutu yazın. Komut bölümlerini sisteminize özel öğeleri değiştirin:

    Set-AdfsRelyingPartyWebContent -Name "relying party trust name" -ErrorPageAuthorizationErrorMessage
Bu uygulamaya erişebilmeniz için önce Cihazınızı kaydetmeniz gerekir.

**Bir iOS cihazı kullanıyorsanız, Cihazınızı katılmak için bu bağlantıyı seçin**:

    a href='https://enterpriseregistration.windows.net/enrollmentserver/otaprofile/yourdomain.com

Bu iOS cihaz, çalışma alanına katılma.

Windows 8.1 cihaz kullanıyorsanız cihazınızın seçerek katılabilir **PC Ayarları**> **ağ** > **çalışma alanına**.

Önceki komutlarda **bağlı olan taraf güveni adı** AD FS'de bağlı olan taraf güveni nesnesi, uygulamanızın adıdır.
Ve **yourdomain.com** Azure Active Directory (örneğin, contoso.com) ile yapılandırdığınız etki alanı adıdır.
Tüm satır sonlarını (varsa) öğesine geçirdiğiniz HTML içeriği kaldırmak mutlaka **kümesi AdfsRelyingPartyWebContent** cmdlet'i.

Artık kullanıcılar, uygulamanızın Azure Active Directory cihaz kaydı hizmeti ile kaydedilmemiş bir CİHAZDAN eriştiklerinde bir hata görürsünüz.

