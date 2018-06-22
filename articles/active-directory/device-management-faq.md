---
title: Azure Active Directory cihaz yönetimi ile ilgili SSS | Microsoft Docs
description: Azure Active Directory cihaz Yönetimi SSS.
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
ms.openlocfilehash: 60b77f5956cb627905eb955995652098337c4dea
ms.sourcegitcommit: 638599eb548e41f341c54e14b29480ab02655db1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/21/2018
ms.locfileid: "36309865"
---
# <a name="azure-active-directory-device-management-faq"></a>Azure Active Directory cihaz yönetimi ile ilgili SSS



**S: macOS cihaz nasıl kayıt mi?**

**Y:** macOS cihazı kaydetmek için:

1.  [Uyumluluk ilkesi oluşturma](https://docs.microsoft.com/intune/compliance-policy-create-mac-os)
2.  [MacOS cihazlar için koşullu erişim ilkesi tanımlayın](active-directory-conditional-access-azure-portal.md) 

**Notlar:**

- Koşullu erişim ilkesinde yer alan kullanıcıların bir [Office sürümü için macOS desteklenen](active-directory-conditional-access-technical-reference.md#client-apps-condition) kaynaklara erişmek için. 

- İlk erişim girişimi sırasında kullanıcılarınızın şirket portalını kullanarak aygıt kaydetmesi istenir.

---

**S: son cihazın kayıtlı. Azure portalında kullanıcı bilgilerimi altında aygıt neden göremiyorum?**

**Y:** karma Azure AD alanına katılmış Windows 10 cihazları gösterme kullanıcı aygıtları altında.
Tüm aygıtlar görünümü Azure portalında kullanmanız gerekir. PowerShell de kullanabilirsiniz [Get-MsolDevice](/powershell/module/msonline/get-msoldevice?view=azureadps-1.0) cmdlet'i.

Yalnızca aşağıdaki cihazları kullanıcı aygıtları altında listelenmiştir:

- Karma Azure AD olmayan tüm kişisel cihazları katıldı. 
- Tüm Windows 10 olmayan / Windows Server 2016 aygıtlar.
- Tüm Windows dışı cihazlar 

--- 

**S: istemci cihaz kayıt durumu nedir nasıl biliyor musunuz?**

**Y:** Azure portalı, tüm aygıtlar için Git ve kullanabileceğiniz cihaz kimliğini kullanarak aygıt için arama Birleştirme türü sütunu altında değerini denetleyin.

Kayıtlı bir cihaza yerel aygıt kaydı durumundan denetlemek istiyorsanız:

- Windows 10 ve Windows Server 2016 veya üzeri cihazlar için dsregcmd.exe/Status çalıştırın.
- Alt düzey işletim sistemi sürümleri için "%programFiles%\Microsoft çalışma alanına Join\autoworkplace.exe" Çalıştır

---

**S: Azure portalında silinmiş veya cihazda Windows PowerShell, ancak yerel durumu kullanma hala kaydedildiğini belirten?**

**Y:** bu tasarım gereğidir. Cihazın kaynaklara bulutta erişimi. 

İşlemi el ile yeniden yeniden kaydetmek istiyorsanız, cihaz üzerinde gerçekleştirilecek olmalıdır. 

Windows 10 ve Windows Server 2016'de, olan birleşim durumundan temizlemek için AD etki alanına katılmış şirket içi:

1.  Komut istemini yönetici olarak açın.

2.  Türü `dsregcmd.exe /debug /leave`

3.  Oturumu kapatın ve yeniden Azure AD ile cihaz kayıtları zamanlanmış görev tetiklemek oturum açın. 

Şirket içi alt düzey Windows işletim sistemi sürümleri için AD etki alanına katılmış:

1.  Komut istemini yönetici olarak açın.
2.  `"%programFiles%\Microsoft Workplace Join\autoworkplace.exe /l"` yazın.
3.  `"%programFiles%\Microsoft Workplace Join\autoworkplace.exe /j"` yazın.

---
** S: Nasıl t bir Azure AD katılmış aygıt yerel cihazda ayrılma?
**A:** 
- Karma Azure AD alanına katılmış cihazlar için zamanlanmış bir görev aygıta yeniden kayıt için otomatik kayıtlarını devre dışı bırakmak üzere emin olun. Ardından, açık komut istemini bir yönetici ve türü `dsregcmd.exe /debug /leave`. Alternatif olarak, bu komutu bir komut dosyası olarak toplu olarak ayrılma birden çok aygıt üzerinden çalıştırılabilir.

- Saf Azure AD katılmış için aygıtlar, tüm Azure AD kullanıcı kimlik bilgileriyle oturum açamazsınız gibi hesap veya bir oluşturma çevrimdışı bir yerel yönetici sağladığınızdan emin olun. Ardından, Git **ayarları** > **hesapları** > **erişim iş veya Okul**. Hesabınızı seçin ve tıklayın **Bağlantıyı Kes**. Komut istemlerini izleyin ve istendiğinde yerel yönetici kimlik bilgilerini sağlayın. Cihaz ayrılma işlemini tamamlamak için yeniden başlatın.

---

**Azure Portalı'nda yinelenen aygıt girişleri neden görüyor musunuz?**

**A:**

-   Windows 10 ve Windows Server 2016 için ayrılma ve aynı aygıt yeniden katılmak için yinelenen denemesi olursa olabilir yinelenen girdi. 

-   Ekleme iş veya Okul hesabınızla kullandıysanız, eklemek iş veya Okul hesabını kullanan her bir windows kullanıcı aynı cihaz adı ile yeni bir cihaz kaydı oluşturun.

-   Şirket içi alt düzey Windows işletim sistemi sürümleri için etki alanına katılmış otomatik kayıt kullanarak AD aynı aygıt adı cihazda oturum her etki alanı kullanıcı için yeni bir cihaz kayıt oluşturur. 

-   Temizlenmeden, yeniden yüklendi ve aynı adla yeniden birleştirilmiş bir Azure AD alanına katılmış makine görünmesini sağlar başka bir kayıtla aynı aygıt adı olarak.

---

**Neden bir kullanıcı Azure portalında devre dışı bırakan bir aygıttan hala kaynaklarına erişebilir mi?**

**Y:** uygulanacak iptal etmek için bir saat sürebilir.

>[!Note] 
>Kayıtlı cihazlar için kullanıcılar kaynaklarına erişemez emin olmak için cihaz silinirken öneririz. Daha fazla ayrıntı için bkz: [cihazları Yönetim için ıntune'a kaydetme](https://docs.microsoft.com/intune/deploy-use/enroll-devices-in-microsoft-intune). 


---

**Kullanıcılarım "Buradan var. alınamıyor" neden görüyor musunuz?**

**Y:** belirli koşullu erişim kuralları belirli cihaz durumu gerektirecek şekilde yapılandırılmış ve cihaz ölçütleri karşılamıyor, kullanıcılar engellenir ve bu ileti görür. Koşullu erişim ilkesi kuralları değerlendirin ve cihaz bu iletinin görünmemesi için belirttiğiniz ölçütlere uyan mümkün olduğundan emin olun.

---


**S: Azure portalında kullanıcı bilgileri altında cihaz kaydı görebilir ve cihazda kayıtlı olarak durumunu görebilirsiniz. Koşullu erişim kullanarak ayarlarım doğru miyim?**

**Y:** DeviceID tarafından yansıtılan aygıt birleşim durumu üzerinde Azure AD ile eşleşen ve koşullu erişim için herhangi bir değerlendirme ölçütleri karşılayan gerekir. Daha fazla ayrıntı için bkz: [Azure Active Directory cihaz kaydı ile çalışmaya başlama](active-directory-device-registration.md).

---

**Yalnızca Azure AD alanına bir aygıt için bir "kullanıcı adı veya parola, yanlış" iletisi neden sağlarım?**

**Y:** bu senaryo için yaygın nedenler şunlardır:

- Kullanıcı kimlik bilgilerinizi artık geçerli değildir.

- Bilgisayarınızı Azure Active Directory ile iletişim kuramıyor. Tüm ağ bağlantısı sorunlarını denetleyin.

- Azure AD katılım ön koşulları karşılanmadı. Lütfen adımları izlediğinizden emin olun [geniletmek bulut özelliklerini Azure Active Directory katılım aracılığıyla Windows 10 cihazlarına](active-directory-azureadjoin-overview.md).  

- Federasyon oturumları bir WS-Trust etkin uç noktası desteklemek için Federasyon sunucusu gerektirir. 

- Kimlik doğrulama geçiş etkinleştirdiyseniz ve kullanıcının oturum açma özelliğini değiştirilmesi gereken geçici bir parola.

---

**S: görmemin nedeni "... Oops bir hata oluştu!" Azure AD yapmak çalıştığınızda iletişim katılma Bilgisayarımda?**

**Y:** bu bir Intune ile Azure Active Directory kaydını ayarlama sonucudur. Azure AD birleştirme yapma girişiminde bulunan kullanıcının doğru Intune lisansı atanmış olduğundan emin olun. Daha fazla ayrıntı için bkz: [Windows cihaz yönetimini ayarlama](https://docs.microsoft.com/intune/deploy-use/set-up-windows-device-management-with-microsoft-intune#azure-active-directory-enrollment).  

---

**Neden hata bilgilerini almadım rağmen bir PC katılmaya my girişimi başarısız oldu?**

**Y:** kullanıcı cihazda yerel yerleşik yönetici hesabı kullanarak oturum, olası bir nedeni olmamasıdır. Lütfen farklı bir yerel hesap Azure Active Directory katılım Kurulumu tamamlamak için kullanmadan önce oluşturun. 

---

**S: otomatik cihaz kaydı için kurulum yönergeleri nereden bulabilirim?**

**Y:** ayrıntılı yönergeler için bkz: [Azure Active Directory ile etki alanına katılmış Windows cihazlarının otomatik kaydını yapılandırma](active-directory-conditional-access-automatic-device-registration-setup.md)

---

**S: sorun giderme nereden bulabilirim otomatik cihaz kaydı hakkında bilgi mi?**

**Y:** sorun giderme bilgileri için bkz:

- [Azure AD ile – Windows 10 ve Windows Server 2016 alanına katılmamış bilgisayarlar etki alanının otomatik kayıt sorunlarını giderme](device-management-troubleshoot-hybrid-join-windows-current.md)

- [Otomatik kaydı etki alanının sorun giderme bilgisayarlar, alt düzey istemciler için Windows Azure AD alanına katılmış](device-management-troubleshoot-hybrid-join-windows-legacy.md)
 
---

