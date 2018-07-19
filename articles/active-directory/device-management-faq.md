---
title: Azure Active Directory cihaz yönetimi hakkında SSS | Microsoft Docs
description: Azure Active Directory cihaz yönetimi hakkında SSS.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
ms.assetid: cdc25576-37f2-4afb-a786-f59ba4c284c2
ms.service: active-directory
ms.component: devices
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2018
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: 10d6b81915432d9f41c0d4751f98cbf380ff2d13
ms.sourcegitcommit: dc646da9fbefcc06c0e11c6a358724b42abb1438
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/18/2018
ms.locfileid: "39136579"
---
# <a name="azure-active-directory-device-management-faq"></a>Azure Active Directory cihaz yönetimi hakkında SSS

**Android veya iOS KCG cihazları kaydedebilirim miyim?**

**Y:** Evet, ancak yalnızca Azure cihaz kayıt hizmeti ile karma müşteriler için. Şirket içi aygıt kaydı hizmetiyle AD FS'de desteklenmez.

**S: nasıl bir macOS cihazı kaydedebilir miyim?**

**Y:** macOS cihazını kaydetmek için:

1.  [Uyumluluk ilkesi oluşturma](https://docs.microsoft.com/intune/compliance-policy-create-mac-os)
2.  [MacOS cihazlar için koşullu erişim ilkesi tanımlama](active-directory-conditional-access-azure-portal.md) 

**Notlar:**

- Koşullu erişim ilkenizi dahil edilen kullanıcıların bir [macOS için Office sürümü desteklenen](active-directory-conditional-access-technical-reference.md#client-apps-condition) kaynaklara erişmek için. 

- İlk erişim denemesi sırasında kullanıcılarınızın şirket portalını kullanarak cihazını kaydetmesi istenir.

---

**S: kısa bir süre önce cihazın kayıtlı. Azure portalındaki kullanıcı Bilgilerim altında cihazın neden göremiyorum?**

**Y:** hibrit Azure AD'ye katılmış olan Windows 10 cihazları görünmez altında kullanıcı cihazları.
Azure portalında tüm cihazları görüntüle kullanmanız gerekir. PowerShell de kullanabilirsiniz [Get-MsolDevice](/powershell/module/msonline/get-msoldevice?view=azureadps-1.0) cmdlet'i.

Yalnızca aşağıdaki cihazlar, kullanıcı cihazları altında listelenir:

- Hibrit Azure AD'ye olmayan tüm kişisel cihazlar katıldı. 
- Tüm Windows 10 olmayan / Windows Server 2016 cihazları.
- Tüm Windows olmayan cihazlar 

--- 

**S: nasıl istemci cihaz kayıt durumu nedir biliyor musunuz?**

**Y:** Azure portalını kullanabilir, tüm cihazlara gidin ve cihaz kimliğini kullanarak cihaz için arama yapın Birleştirme türü sütunu altındaki değerini denetleyin.

Kayıtlı bir cihazı yerel cihaz kayıt durumundan denetlemek istiyorsanız:

- Windows 10 ve Windows Server 2016 veya üzeri cihazlar için dsregcmd.exe/Status çalıştırın.
- Alt düzey işletim sistemi sürümleri için "%programFiles%\Microsoft çalışma alanına Join\autoworkplace.exe" çalıştırın.

---

**S: Azure portalında silinmiş veya hala kayıtlı cihazda Windows PowerShell, ancak yerel durumu kullanma diyor?**

**Y:** bu tasarım gereğidir. Cihaz bulut kaynaklarına erişimi yoktur. 

Yeniden yeniden kaydetmek isterseniz, cihaz üzerinde gerçekleştirilecek el ile gerçekleştirilen bir eylem olmalıdır. 

Windows 10 ve Windows Server 2016, birleşim durumu temizlemek için AD etki alanına katılmış şirket içinde:

1.  Komut istemini yönetici olarak açın.

2.  Türü `dsregcmd.exe /debug /leave`

3.  Oturumu kapatın ve yeniden Azure AD ile cihaz kaydeden zamanlanmış tetikleyici için oturum açın. 

Şirket içi alt düzey Windows işletim sistemi sürümleri için AD etki alanına katılmış:

1.  Komut istemini yönetici olarak açın.
2.  `"%programFiles%\Microsoft Workplace Join\autoworkplace.exe /l"` yazın.
3.  `"%programFiles%\Microsoft Workplace Join\autoworkplace.exe /j"` yazın.

---
**S: ben bir Azure AD katıldı cihaz yerel olarak cihazda ayrılma?**

**Y:** 
- Karma Azure AD alanına katılmış aygıtlar için zamanlanmış görev cihazı yeniden kayıt için otomatik kayıt devre dışı bırakmak üzere emin olun. Ardından, açık komut istemini yönetici olarak çalıştırıp türü `dsregcmd.exe /debug /leave`. Alternatif olarak, bu komutu bir komut dosyası olarak toplu olarak ayrılma için birden çok cihazda çalıştırılabilir.

- Azure AD katıldı saf için cihazlar, tüm Azure AD kullanıcı kimlik bilgileriyle oturum açamayacaksınız gibi hesap veya bir oluşturma çevrimdışı bir yerel yönetici olduğundan emin olun. Ardından, Git **ayarları** > **hesapları** > **işe veya okula erişim**. Hesabınızı seçin ve tıklayın **Bağlantıyı Kes**. Komut istemlerini izleyin ve istendiğinde yerel yönetici kimlik bilgilerini girin. Ayrılma işlemi tamamlamak için cihazı yeniden başlatın.

---

**S: kullanıcılar Azure AD alanına katılmış aygıtlar yazıcıları arama yapamazsınız. Azure AD alanına katılmış aygıtlar yazdırma nasıl etkinleştirebilirim?**

**Y:** yazıcılar için Azure AD alanına katılmış aygıtlar dağıtmak için bkz: [karma bulut yazdırma](https://docs.microsoft.com/en-us/windows-server/administration/hybrid-cloud-print/hybrid-cloud-print-deploy). Hibrit bulut yazdırma dağıtmak için bir şirket içi Windows Server gerekir. Şu anda, bulut tabanlı yazdırma hizmeti kullanılabilir değil. 

---

**S: Uzak bir Azure AD'ye nasıl bağlanırım cihazı alanına? ** 
 **Y:** makaleye bakın https://docs.microsoft.com/en-us/windows/client-management/connect-to-remote-aadj-pc Ayrıntılar için.

---

**S: neden Azure portalında yinelenen cihaz girişi görüyor musunuz?**

**Y:**

-   Windows 10 ve Windows Server 2016 için ayrılma ve aynı cihaza yeniden katılabilir yönelik yinelenen girişimleri varsa olabilir yinelenen girdiler. 

-   Ekleme iş veya Okul hesabı kullandıysanız, ekleme iş veya Okul hesabı kullanan her bir windows kullanıcı yeni bir cihaz kaydı ile aynı cihaz adı oluşturun.

-   Şirket içi alt düzey Windows işletim sistemi sürümleri için AD etki alanına katılmış otomatik kaydı hizmetini kullanarak cihazda oturum açan her etki alanı kullanıcısı için aynı cihaz adı ile yeni bir cihaz kaydı oluşturur. 

-   Temizlenen, yeniden yüklendi ve aynı adla yeniden birleştirilmiş bir Azure AD alanına katılmış makinenizin cihaz aynı ada sahip başka bir kayıt olarak görünecektir.

---

**S: neden bir kullanıcı Azure portalında devre dışı bırakan bir CİHAZDAN hala kaynaklara erişebilir?**

**Y:** uygulanacak bir iptal etme için bir saate kadar sürebilir.

>[!Note] 
>Kayıtlı cihazlar için kullanıcılar kaynaklara erişemez emin olmak için cihaz silinmeden önceki öneririz. Daha fazla ayrıntı için [ıntune'da Yönetim için cihazları kaydetme](https://docs.microsoft.com/intune/deploy-use/enroll-devices-in-microsoft-intune). 


---

**S: Kullanıcılarım "Buradan oraya ulaşamazsınız" neden görüyorum?**

**Y:** kullanıcıları belirli koşullu erişim kuralları, belirli cihaz durumu gerektirecek şekilde yapılandırdıysanız ve cihaz ölçütleri karşılamıyor ise engellenir ve şu mesajı görürsünüz. Lütfen koşullu erişim ilkesi kuralları değerlendirir ve cihazın bu iletiyi engellemek için ölçütleri karşılayan mümkün olduğundan emin olun.

---

**S: neden bazı kullanıcılar değil Azure AD'de MFA istemleri get yapmak katılmış cihazlarda?**

**Y:** kullanıcı katıldığında veya Azure AD ile çok faktörlü kimlik doğrulaması kullanarak bir cihazı kaydeder, cihaz güvenilir ikinci öğe için söz konusu kullanıcıyla olur. Sonuç olarak, aynı kullanıcı oturum açtığında cihaza ve uygulamaya erişen her Azure AD cihaz ikinci öğe olarak göz önünde bulundurur ve ek MFA sorulmadan uygulamalarına sorunsuz bir şekilde erişmek bu kullanıcı sağlar. Bu davranış, cihaz erişen diğer tüm kullanıcılar yine de bir MFA testini ile MFA gerektiren uygulamalar erişmeden önce istendiği şekilde bu cihazda oturum imzalama herhangi bir kullanıcı için geçerli değildir.

---

**S: Azure portalında kullanıcı bilgileri altında cihaz kaydı bakın ve cihazda kayıtlı durumu görebilirsiniz. Koşullu erişim kullanmak için doğru Kurulumu miyim?**

**Y:** DeviceID tarafından yansıtılan cihaz birleşim durumu, Azure AD ile eşleşmesi ve koşullu erişim için herhangi bir değerlendirme ölçütleri karşılayan gerekir. Daha fazla ayrıntı için [Azure Active Directory cihaz kaydı ile çalışmaya başlama](active-directory-device-registration.md).

---

**S: neden Azure AD'ye katılmış yalnızca bir cihaz için bir "kullanıcı adı veya parola hatalı" iletisi alırım?**

**Y:** bu senaryo için yaygın nedenleri şunlardır:

- Kullanıcı kimlik bilgilerinizi artık geçerli değil.

- Bilgisayarınızı Azure Active Directory ile iletişim kuramıyor. Tüm ağ bağlantısı sorunlarını denetleyin.

- Azure AD Join Önkoşullar karşılanmadı. Adımları izlediğinizden emin olun [yaygınlaştırma bulut işlevlerini Windows 10 cihazlarını Azure Active Directory Join ile](active-directory-azureadjoin-overview.md).  

- Federasyon oturum açma bilgileri, WS-Trust ile etkin bir uç nokta desteklemek için Federasyon sunucunuz gerektirir. 

- Kimlik doğrulaması üzerinden etkinleştirdiyseniz ve kullanıcı oturum açma sırasında değiştirilmesi gereken geçici bir parola sahiptir.

---

**S: neden görüyorum "hata... bir hata oluştu!" Azure AD yapmak çalıştığınızda iletişim katılın Bilgisayarımda?**

**Y:** bu bir Azure Active Directory kaydı Intune ile ayarlama sonucudur. Azure AD'ye katılım yapma girişimi kullanıcının doğru Intune lisansı atanmış olduğundan emin olun. Daha fazla ayrıntı için [Windows cihaz yönetimini ayarlama](https://docs.microsoft.com/intune/deploy-use/set-up-windows-device-management-with-microsoft-intune#azure-active-directory-enrollment).  

---

**S: neden hata bilgileri almadım olsa da bir PC katılmaya my girişimi başarısız oldu?**

**Y:** olası bir nedeni, kullanıcı cihazda yerleşik yerel yönetici hesabı kullanarak oturum emin olan. Azure Active Directory Join Kurulumu tamamlamak için kullanmadan önce lütfen farklı bir yerel hesap oluşturun. 

---

**S: otomatik cihaz kaydı için kurulum yönergeleri nereden bulabilirim?**

**Y:** ayrıntılı yönergeler için bkz. [Azure Active Directory ile Windows etki alanına katılmış cihazların otomatik kaydını yapılandırma](active-directory-conditional-access-automatic-device-registration-setup.md)

---

**S: sorun giderme nereden bulabilirim otomatik cihaz kaydı hakkında bilgi?**

**Y:** sorun giderme bilgileri için bkz:

- [Azure AD'ye – Windows 10 ve Windows Server 2016 alanına katılmış bilgisayarları etki alanının otomatik kaydı sorunlarını giderme](device-management-troubleshoot-hybrid-join-windows-current.md)

- [Bilgisayarları Azure AD'ye Windows alt düzey istemciler için katılmış etki alanının otomatik kaydı sorunlarını giderme](device-management-troubleshoot-hybrid-join-windows-legacy.md)
 

---

