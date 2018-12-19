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
ms.openlocfilehash: a0cfd65aa2444956336e5363d20acab61a404c68
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/12/2018
ms.locfileid: "53309187"
---
# <a name="azure-active-directory-device-management-faq"></a>Azure Active Directory cihaz yönetimi hakkında SSS

**S: Ben kısa bir süre önce cihazın kayıtlı. Azure portalındaki kullanıcı Bilgilerim altında cihazın neden göremiyorum? Veya neden yok hibrit Azure AD'ye katılmış cihazlar için cihaz sahibi işaretlenmiş?**
 **Y:** Hibrit Azure AD'ye katılmış olan Windows 10 cihazları altında kullanıcı cihazları gösterilmez.
Azure portalında tüm cihazları görüntüle kullanmanız gerekir. PowerShell de kullanabilirsiniz [Get-MsolDevice](/powershell/module/msonline/get-msoldevice?view=azureadps-1.0) cmdlet'i.

Yalnızca aşağıdaki cihazlar, kullanıcı cihazları altında listelenir:

- Hibrit Azure AD'ye olmayan tüm kişisel cihazlar katıldı. 
- Tüm Windows 10 olmayan / Windows Server 2016 cihazları.
- Tüm Windows olmayan cihazlar 

--- 

**S: İstemci cihaz kayıt durumu nedir olduğunu nasıl öğrenebilirim?**

**Y:** Azure portalını kullanabilir, tüm cihazlara gidin ve cihaz kimliğini kullanarak cihaz için arama yapın Birleştirme türü sütunu altındaki değerini denetleyin. Bazı durumlarda, cihaz sıfırlama görüntüsü yeniden oluşturulabildiği veya silinmiş. Bu nedenle, aynı zamanda çok cihazın aygıt kayıt durumunu denetlemek için önemlidir:

- Windows 10 ve Windows Server 2016 veya üzeri cihazlar için dsregcmd.exe/Status çalıştırın.
- Alt düzey işletim sistemi sürümleri için "%programFiles%\Microsoft çalışma alanına Join\autoworkplace.exe" çalıştırın.

---

**S: Azure portalındaki kullanıcı bilgileri altında cihaz kaydı görüntülemek ve cihazda kayıtlı durumu görebilirsiniz bildirimi. Doğru kullanarak koşullu erişim için ayarladığım?**

**Y:** DeviceID tarafından yansıtılan cihaz birleşim durumu, Azure AD ile eşleşen ve koşullu erişim için herhangi bir değerlendirme ölçütleri karşılayan gerekir. Daha fazla bilgi için [gerektiren yönetilen cihazlar için koşullu erişim ile bulut uygulaması erişimi](../conditional-access/require-managed-devices.md).

---

**S: Azure portalında silinmiş veya hala kayıtlı cihazda Windows PowerShell, ancak yerel durumu kullanma diyor?**

**Y:** Bu tasarım gereğidir. Cihaz bulut kaynaklarına erişimi yoktur. 

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

**S: Yinelenen cihaz girişlerini Azure portalında neden görüyorum?**

**Y:**

-   Windows 10 ve Windows Server 2016 için aynı cihazı alanına katın ve ayrılma yönelik yinelenen girişimleri varsa olabilir yinelenen girdiler. 

-   Ekleme iş veya Okul hesabı kullandıysanız, ekleme iş veya Okul hesabı kullanan her bir windows kullanıcı yeni bir cihaz kaydı ile aynı cihaz adı oluşturun.

-   Şirket içi alt düzey Windows işletim sistemi sürümleri için AD etki alanına katılmış otomatik kaydı hizmetini kullanarak cihazda oturum açan her etki alanı kullanıcısı için aynı cihaz adı ile yeni bir cihaz kaydı oluşturur. 

-   Temizlenen, yeniden, ve aynı adla yeniden katılınması bir Azure AD alanına katılmış makine başka bir kayıtla aynı cihaz adı olarak görünür.

---

**S: Neden bir kullanıcı Azure portalında devre dışı bırakan bir CİHAZDAN hala kaynaklara erişebilir?**

**Y:** Bu bir iptal etme uygulanacak bir saate kadar sürebilir.

>[!Note] 
>Kayıtlı cihazlar için kullanıcılar kaynaklara erişemez emin olmak için cihaz silinmeden önceki öneririz. Daha fazla bilgi için [ıntune'da Yönetim için cihazları kaydetme](https://docs.microsoft.com/intune/deploy-use/enroll-devices-in-microsoft-intune). 

---

## <a name="azure-ad-join-faq"></a>Azure AD katılımı ile ilgili SSS

**S: Nasıl ben bir Azure AD katıldı cihaz yerel olarak cihazda ayrılma?**

**Y:** 
- Karma Azure AD alanına katılmış aygıtlar için zamanlanmış görev cihazı yeniden kayıt için otomatik kayıt devre dışı bırakmak üzere emin olun. Ardından, açık komut istemini yönetici olarak çalıştırıp türü `dsregcmd.exe /debug /leave`. Alternatif olarak, bu komutu bir komut dosyası olarak toplu olarak ayrılma için birden çok cihazda çalıştırılabilir.

- Azure AD katıldı saf için cihazlar, tüm Azure AD kullanıcı kimlik bilgileriyle oturum açamayacaksınız gibi hesap veya bir oluşturma çevrimdışı bir yerel yönetici olduğundan emin olun. Ardından, Git **ayarları** > **hesapları** > **işe veya okula erişim**. Hesabınızı seçin ve tıklayın **Bağlantıyı Kes**. Komut istemlerini izleyin ve istendiğinde yerel yönetici kimlik bilgilerini girin. Ayrılma işlemi tamamlamak için cihazı yeniden başlatın.

---

**S: Kullanıcılarım silinmiş veya Azure AD'de devre dışı Azure AD'ye katılmış cihazlar oturum açarak?**
 **Y:** Evet. Windows, kullanıcılar Masaüstü bile ağ bağlantısı hızlıca erişmek için önceden günlüğe yazılan izin vermek için oturum açma özelliği önbelleğe almıştır. Cihazın silindi veya Azure AD'de devre dışı olduğunda Windows cihaza bilinmiyor. Bu nedenle, daha önce oturum kullanıcılar Masaüstü önbelleğe alınmış oturum açma ile erişmeye devam edebilir. Ancak, cihazın silinmesi veya devre dışı olarak Kullanıcılar Cihaz tabanlı koşullu erişim tarafından korunan herhangi bir kaynağa erişemez. 

Önbelleğe alınmış oturum açma için etkin olarak zaten oturum açmadıysanız kullanıcıların cihaz erişemez. 

---

**S: Devre dışı bırakılan veya silinen kullanıcılar Azure AD'ye katılmış cihazlar için oturum açabilir?**
 **Y:** Evet, ancak yalnızca sınırlı bir süreliğine. Bir kullanıcı silindi veya Azure AD'de devre dışı olduğunda Windows cihaza hemen bilinmiyor. Bu nedenle, daha önce oturum Masaüstü önbelleğe alınmış oturum açma ile kullanıcıların erişim sağlayabilir. Cihaz (genellikle kısa 4 saat) kullanıcı durumu uyumlu hale geldikten sonra Windows kullanıcılarla Masaüstü erişmesini engeller. Kullanıcı silindi veya Azure AD'de devre dışı olarak bunlar herhangi bir kaynağa erişilemiyor. Bu nedenle tüm belirteçlerin, iptal edilir. 

Önbelleğe alınmış oturum açma için bunları etkin olduğundan, daha önce oturum açmadıysanız silinmiş veya devre dışı bırakılmış kullanıcılar bir cihaz erişemez. 

---

**S: Kullanıcılar, Azure AD alanına katılmış aygıtlar yazıcıları arama yapamazsınız. Azure AD alanına katılmış aygıtlar yazdırma nasıl etkinleştirebilirim?**

**Y:** Azure AD alanına katılmış aygıtlar için yazıcıları dağıtmak için bkz: [karma bulut yazdırma](https://docs.microsoft.com/windows-server/administration/hybrid-cloud-print/hybrid-cloud-print-deploy). Hibrit bulut yazdırma dağıtmak için bir şirket içi Windows Server gerekir. Şu anda, bulut tabanlı yazdırma hizmeti kullanılabilir değil. 

---

**S: Nasıl bir uzak Azure AD'ye bağlanabilirim katılmış?**
 **Y:** Makalesine bakın https://docs.microsoft.com/windows/client-management/connect-to-remote-aadj-pc Ayrıntılar için.

---

**S: Kullanıcılarım "Buradan oraya ulaşamazsınız" neden görüyorum?**

**Y:** Bazı koşullu erişim kuralları, belirli cihaz durumu gerektirecek şekilde yapılandırdıysanız ve cihaz ölçütleri karşılamıyor ise kullanıcılar engellenir ve şu mesajı görürsünüz. Koşullu erişim ilkesi kuralları değerlendirir ve cihazın bu iletiyi engellemek için ölçütleri karşılayan mümkün olduğundan emin olun.

---

**S: Katılmış cihazlarda neden benim kullanıcıların Azure AD MFA ister get bazıları musunuz?**

**Y:** Kullanıcı katıldığında veya Azure AD ile çok faktörlü kimlik doğrulaması kullanarak bir cihazı kaydeder, cihaz güvenilir ikinci öğe için söz konusu kullanıcıyla olur. Sonuç olarak, aynı kullanıcı oturum açtığında cihaza ve uygulamaya erişen her Azure AD cihaz ikinci öğe olarak göz önünde bulundurur ve ek MFA sorulmadan uygulamalarına sorunsuz bir şekilde erişmek bu kullanıcı sağlar. Bu davranış, cihaz erişen diğer tüm kullanıcılar yine de bir MFA testini ile MFA gerektiren uygulamalar erişmeden önce istendiği şekilde bu cihazda oturum imzalama herhangi bir kullanıcı için geçerli değildir.

---

**S: Neden Azure AD'ye katılmış yalnızca bir cihaz için bir "kullanıcı adı veya parola hatalı" iletisi alıyorum?**

**Y:** Bu senaryo için yaygın nedenleri şunlardır:

- Kullanıcı kimlik bilgilerinizi artık geçerli değil.

- Bilgisayarınızı Azure Active Directory ile iletişim kuramıyor. Tüm ağ bağlantısı sorunlarını denetleyin.

- Federasyon oturum açma bilgileri, WS-Trust uç noktaları etkinleştirilmiş ve erişilebilir desteklemek için Federasyon sunucunuz gerektirir. 

- Kimlik doğrulaması üzerinden etkinleştirdiyseniz ve kullanıcı oturum açma sırasında değiştirilmesi gereken geçici bir parola sahiptir.

---

**S: Neden görüyorum "hata... bir hata oluştu!" Azure AD yapmak çalıştığınızda iletişim katılın Bilgisayarımda?**

**Y:** Bu, Intune ile Azure Active Directory kaydını ayarlama sonucudur. Azure AD'ye katılım yapma girişimi kullanıcı doğru Intune lisansı atanmış olduğundan emin olun. Daha fazla bilgi için [Windows cihaz yönetimini ayarlama](https://docs.microsoft.com/intune/deploy-use/set-up-windows-device-management-with-microsoft-intune#azure-active-directory-enrollment).  

---

**S: Neden hata bilgileri almadım olsa da Azure AD Alanım girişimi PC başarısız katılın?**

**Y:** Olası bir nedeni, kullanıcı cihazda yerleşik yerel yönetici hesabı kullanarak oturum emin olan. Azure Active Directory Join Kurulumu tamamlamak için kullanmadan önce farklı bir yerel hesap oluşturun. 

---

## <a name="hybrid-azure-ad-join-faq"></a>Hibrit Azure AD'ye katılım SSS

**S: Sorun giderme nereden bulabilirim hibrit Azure AD'ye katılma hatalarını bilgi?**

**Y:** Sorun giderme bilgileri için bkz:

- [Azure AD'ye – Windows 10 ve Windows Server 2016 alanına katılmış bilgisayarları etki alanının otomatik kayıt sorunlarını giderme](troubleshoot-hybrid-join-windows-current.md)

- [Bilgisayarları Azure AD'ye Windows alt düzey istemciler için katılmış etki alanının otomatik kayıt sorunlarını giderme](troubleshoot-hybrid-join-windows-legacy.md)
 

---

## <a name="azure-ad-register-faq"></a>Azure AD kayıt SSS

**S: Android veya iOS KCG cihazları kaydedebilir miyim?**

**Y:** Evet, ancak yalnızca Azure cihaz kayıt hizmeti ve karma müşteriler için. Şirket içi aygıt kaydı hizmetiyle AD FS'de desteklenmez.

**S: Bir macOS cihazı nasıl kaydedebilir miyim?**

**Y:** MacOS cihazını kaydetmek için:

1.  [Uyumluluk ilkesi oluşturma](https://docs.microsoft.com/intune/compliance-policy-create-mac-os)
2.  [MacOS cihazlar için koşullu erişim ilkesi tanımlama](../active-directory-conditional-access-azure-portal.md) 

**Notlar:**

- Koşullu erişim ilkenizi dahil edilen kullanıcıların bir [macOS için Office sürümü desteklenen](../conditional-access/technical-reference.md#client-apps-condition) kaynaklara erişmek için. 

- İlk erişim denemesi sırasında kullanıcılarınızın şirket portalını kullanarak cihazını kaydetmesi istenir.

---
