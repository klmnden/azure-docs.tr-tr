---
title: Birleştirilmiş alt düzey cihazlar Azure Active Directory karma sorunlarını giderme | Microsoft Docs
description: Karma Azure Active Directory sorun giderme alt düzey aygıtları katıldı.
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
ms.date: 04/23/2018
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: d41e83c11f33b0bcbe4ea632332f2cd8bb12313f
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34714121"
---
# <a name="troubleshooting-hybrid-azure-active-directory-joined-down-level-devices"></a>Alt düzey aygıtları birleştirilmiş karma Azure Active Directory sorun giderme 

Bu makalede, yalnızca aşağıdaki cihazlar için geçerlidir: 

- Windows 7 
- Windows 8.1 
- Windows Server 2008 R2 
- Windows Server 2012 
- Windows Server 2012 R2 
 

Windows 10 veya Windows Server 2016 için bkz: [sorun giderme karma Azure Active Directory'ye katılmış Windows 10 ve Windows Server 2016 cihazları](device-management-troubleshoot-hybrid-join-windows-current.md).

Bu makalede, sahibi olduğunuzu varsayar [yapılandırılmış karma Azure Active Directory'ye katılmış cihazları](device-management-hybrid-azuread-joined-devices-setup.md) aşağıdaki senaryoları desteklemek için:

- Cihaz temelli koşullu erişim

- [Kurumsal Dolaşım ayarları](active-directory-windows-enterprise-state-roaming-overview.md)

- [İş İçin Windows Hello](active-directory-azureadjoin-passport-deployment.md) 





Bu makalede, sorun giderme ile ilgili olası sorunları gidermek nasıl yönergelerini sağlar.  

**Bilmeniz gerekenler:** 

- En fazla kullanıcı başına aygıtları Aygıt odaklı sayısıdır. Örneğin, varsa *jdoe* ve *jharnett* bir cihaza, ayrı bir kayıt (DeviceID) oturum açma bunların her biri için oluşturulduğunda **kullanıcı** bilgileri sekmesi.  

- İlk kaydı / aygıtların birleştirme denemesi oturum açma veya kilidi gerçekleştirmek / kilidini açmak için yapılandırılır. Bir Görev Zamanlayıcı görevi tarafından tetiklenen 5 dakikalık gecikme olabilir. 

- İşletim sistemi veya el ile yeniden kayıt yüklenmesinin Azure portalında kullanıcı bilgileri sekmesi altında birden çok giriş sonuçlanır Azure AD içinde yeni bir kayıt oluşturabilir. 

## <a name="step-1-retrieve-the-registration-status"></a>1. adım: kayıt durumunu alma 

**Kayıt durumunu doğrulamak için:**  

1. Karma Azure AD birleştirme yürüttü kullanıcı hesabı ile oturum açma.

2. Komut istemini yönetici olarak açın 

3. Türü `"%programFiles%\Microsoft Workplace Join\autoworkplace.exe" /i`

Bu komut, birleşim durumu hakkında daha fazla ayrıntı sağlayan bir iletişim kutusu görüntüler.

![Windows için çalışma alanına katılma](./media/active-directory-device-registration-troubleshoot-windows-legacy/01.png)


## <a name="step-2-evaluate-the-hybrid-azure-ad-join-status"></a>2. adım: Azure AD birleştirme durum karma değerlendirme 

Karma Azure AD birleştirme başarılı olmadıysa iletişim kutusu oluştu sorun hakkında ayrıntılar sağlar.

**En yaygın sorunları şunlardır:**

- Yanlış yapılandırılmış AD FS veya Azure AD

    ![Windows için çalışma alanına katılma](./media/active-directory-device-registration-troubleshoot-windows-legacy/02.png)

- Bir etki alanı kullanıcısı olarak imzalı değil

    ![Windows için çalışma alanına katılma](./media/active-directory-device-registration-troubleshoot-windows-legacy/03.png)
    
    Neden bu durum ortaya çıkabilir birkaç farklı nedeni vardır:
    
    - Oturum açmış olan kullanıcının etki alanı kullanıcı (örneğin, yerel bir kullanıcı) değil. Alt düzey cihazlarda karma Azure AD birleştirme yalnızca etki alanı kullanıcıları için desteklenir.
    
    - Autoworkplace.exe sessizce Azure AD veya AD FS kimlik doğrulaması alamıyor. Bu, bir Azure AD URL'lere dışarı bağlı ağ bağlantı sorunlarından kaynaklanıyor olabilir. Ayrıca, kullanıcı için etkin/yapılandırılan çok faktörlü kimlik doğrulaması (MFA) ve WIAORMUTLIAUTHN federasyon sunucusunda yapılandırılmamış olabilir. Bu giriş bölgesi bulma (HRD) sayfasını engelleyen kullanıcı etkileşimi için bekleyen başka bir olasılık olabilir **autoworkplace.exe** sessizce bir belirteç edinme gelen.
    
    - Azure AD sorunsuz çoklu oturum açma, kuruluşunuzun kullandığı `https://autologon.microsoftazuread-sso.com` veya `https://aadg.windows.net.nsatc.net` cihazın IE intranet ayarlarını, mevcut olmayan ve **izin durum çubuğunda komut dosyası aracılığıyla güncelleştirmeleri** Intranet bölgesi için etkin değil.

- Bir kotasına ulaşıldı

    ![Windows için çalışma alanına katılma](./media/active-directory-device-registration-troubleshoot-windows-legacy/04.png)

- Hizmeti yanıt vermiyor 

    ![Windows için çalışma alanına katılma](./media/active-directory-device-registration-troubleshoot-windows-legacy/05.png)

Olay günlüğünde altında durum bilgisi bulabilirsiniz: **uygulamaları ve Hizmetleri Log\Microsoft-çalışma alanına katılma**
  
**Başarısız karma Azure AD katılım için en yaygın nedenler şunlardır:** 

- Bilgisayarınız, kuruluşunuzun iç ağa veya bir VPN bağlantısı olan bir şirket içi bağlı olmadığını AD etki alanı denetleyicisi.

- Bilgisayarınıza yerel bilgisayar hesabı ile oturum. 

- Hizmet yapılandırma sorunları: 

  - Federasyon sunucusunu desteklemek üzere yapılandırılmış **WIAORMULTIAUTHN**. 

  - Bilgisayarınızın orman sahip Azure AD'de doğrulanmış etki alanı adına işaret eden hizmet bağlantı noktası nesnesi yok 

  - Bir kullanıcı cihaz sınırına ulaştı. 

## <a name="next-steps"></a>Sonraki adımlar

Soruları için bkz: [aygıt yönetimi hakkında SSS](device-management-faq.md)  
