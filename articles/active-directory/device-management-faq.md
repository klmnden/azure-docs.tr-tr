---
title: "Azure Active Directory cihaz yönetimi ile ilgili SSS | Microsoft Docs"
description: "Azure Active Directory cihaz Yönetimi SSS."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: mtillman
ms.assetid: cdc25576-37f2-4afb-a786-f59ba4c284c2
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/13/2017
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: d44bb23b12e3ccd92d0661175873f6255f238b94
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
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

**Y:** ile otomatik cihaz kaydı etki alanına katılan Windows 10 cihazları gösterme altında kullanıcı bilgileri.
Tüm aygıtları görmek için PowerShell kullanmanız gerekir. 

Yalnızca aşağıdaki aygıtlar, kullanıcı bilgisi altında listelenir:

- Birleştirilmiş Kurumsal olmayan tüm kişisel cihazlar 
- Tüm Windows 10 olmayan / Windows Server 2016 
- Tüm Windows dışı cihazlar 

---

**Neden Azure portalında Azure Active Directory'de kayıtlı tüm cihazları görebilirim değil mi?** 

**Y:** şu anda Azure Portalı'ndaki tüm kayıtlı cihazları görmek için bir yolu yoktur. Tüm aygıtları bulmak için Azure PowerShell'i kullanabilirsiniz. Daha fazla ayrıntı için bkz: [Get-MsolDevice](/powershell/module/msonline/get-msoldevice?view=azureadps-1.0) cmdlet'i.

--- 

**S: istemci cihaz kayıt durumu nedir nasıl biliyor musunuz?**

**Y:** cihaz kayıt durumu bağlıdır:

- Cihaz nedir
- Nasıl kaydedildi 
- Bununla ilgili tüm ayrıntıları. 
 

---

**Bir aygıt neden olduğundan Azure portalında silinmiş ya da Windows PowerShell kullanarak hala listelenen kayıtlı olarak?**

**Y:** bu tasarım gereğidir. Cihazın kaynaklara bulutta erişimi. Aygıtı kaldırın ve yeniden kaydetmek istiyorsanız, el ile bir eylem cihaz üzerinde gerçekleştirilecek olması gerekir. 

Windows 10 ve Windows Server 2016'de, şirket içi AD etki alanına katılmış:

1.  Komut istemini yönetici olarak açın.

2.  Türü`dsregcmd.exe /debug /leave`

3.  Oturumu kapatın ve aygıt yeniden kaydeder zamanlanmış görev tetiklemek oturum açın. 

Şirket içi diğer Windows platformları için AD etki alanına katılmış:

1.  Komut istemini yönetici olarak açın.
2.  `"%programFiles%\Microsoft Workplace Join\autoworkplace.exe /l"` yazın.
3.  `"%programFiles%\Microsoft Workplace Join\autoworkplace.exe /j"` yazın.

---

**Azure Portalı'nda yinelenen aygıt girişleri neden görüyor musunuz?**

**A:**

-   Windows 10 ve Windows Server 2016 için ayrılma ve aynı aygıt yeniden katılmak için yinelenen denemeleri olmaları durumunda olabilir yinelenen girdi. 

-   Ekleme iş veya Okul hesabınızla kullandıysanız, eklemek iş veya Okul hesabını kullanan her bir windows kullanıcı aynı cihaz adı ile yeni bir cihaz kaydı oluşturun.

-   Şirket içi diğer Windows platformları etki alanına katılmış otomatik kayıt kullanarak AD cihazda oturum her etki alanı kullanıcı için aynı aygıt adıyla yeni bir cihaz kayıt oluşturur. 

-   Temizlenmeden, yeniden yüklendi ve aynı adla yeniden birleştirilmiş bir AADJ makine görünmesini sağlar başka bir kayıtla aynı aygıt adı olarak.

---

**Neden bir kullanıcı Azure portalında devre dışı bırakan bir aygıttan hala kaynaklarına erişebilir mi?**

**Y:** uygulanacak iptal etmek için bir saat sürebilir.

>[!Note] 
>Kaybolan cihazlarda, Kullanıcılar Cihaz erişemiyor emin olmak için cihaz silinirken öneririz. Daha fazla ayrıntı için bkz: [cihazları Yönetim için ıntune'a kaydetme](https://docs.microsoft.com/intune/deploy-use/enroll-devices-in-microsoft-intune). 


---

**Kullanıcılarım "Buradan var. alınamıyor" neden görüyor musunuz?**

**Y:** belirli koşullu erişim kuralları belirli cihaz durumu gerektirecek şekilde yapılandırılmış ve cihaz ölçütleri karşılamıyor, kullanıcılar engellenir ve bu ileti görür. Lütfen kuralları değerlendirin ve cihaz bu iletinin görünmemesi için belirttiğiniz ölçütlere uyan mümkün olduğundan emin olun.

---


**S: Azure portalında kullanıcı bilgileri altında cihaz kaydı görebilir ve istemcide kayıtlı olarak durumunu görebilirsiniz. Koşullu erişim kullanarak ayarlarım doğru miyim?**

**Y:** aygıt kaydı (DeviceID) ve Azure Portal'da durumu gerekir istemci eşleşen ve koşullu erişim için herhangi bir değerlendirme ölçütleri karşılayan. Daha fazla ayrıntı için bkz: [Azure Active Directory cihaz kaydı ile çalışmaya başlama](active-directory-device-registration.md).

---

**Yalnızca Azure AD alanına bir aygıt için bir "kullanıcı adı veya parola, yanlış" iletisi neden sağlarım?**

**Y:** bu senaryo için yaygın nedenler şunlardır:

- Kullanıcı kimlik bilgilerinizi artık geçerli değildir.

- Bilgisayarınızı Azure Active Directory ile iletişim kuramıyor. Tüm ağ bağlantısı sorunlarını denetleyin.

- Azure AD katılım ön koşulları karşılanmadı. Lütfen adımları izlediğinizden emin olun [geniletmek bulut özelliklerini Azure Active Directory katılım aracılığıyla Windows 10 cihazlarına](active-directory-azureadjoin-overview.md).  

- Federasyon oturumları bir WS-Trust etkin uç noktası desteklemek için Federasyon sunucusu gerektirir. 

---

**S: görmemin nedeni "... Oops bir hata oluştu!" çalıştığımda iletişim Bilgisayarımda katılma?**

**Y:** bu bir Intune ile Azure Active Directory kaydını ayarlama sonucudur. Daha fazla ayrıntı için bkz: [Windows cihaz yönetimini ayarlama](https://docs.microsoft.com/intune/deploy-use/set-up-windows-device-management-with-microsoft-intune#azure-active-directory-enrollment).  

---

**Neden hata bilgilerini almadım rağmen bir PC katılmaya my girişimi başarısız oldu?**

**Y:** kullanıcının aygıtına yerleşik yönetici hesabı kullanarak açanlar bir nedeni olması. Lütfen farklı bir yerel hesap Azure Active Directory katılım Kurulumu tamamlamak için kullanmadan önce oluşturun. 

---

**S: otomatik cihaz kaydı için kurulum yönergeleri nereden bulabilirim?**

**Y:** ayrıntılı yönergeler için bkz: [Azure Active Directory ile etki alanına katılmış Windows cihazlarının otomatik kaydını yapılandırma](active-directory-conditional-access-automatic-device-registration-setup.md)

---

**S: sorun giderme nereden bulabilirim otomatik cihaz kaydı hakkında bilgi mi?**

**Y:** sorun giderme bilgileri için bkz:

- [Azure AD ile – Windows 10 ve Windows Server 2016 alanına katılmamış bilgisayarlar etki alanının otomatik kayıt sorunlarını giderme](device-management-troubleshoot-hybrid-join-windows-current.md)

- [Otomatik kaydı etki alanının sorun giderme bilgisayarlar, alt düzey istemciler için Windows Azure AD alanına katılmış](device-management-troubleshoot-hybrid-join-windows-legacy.md)
 
---

