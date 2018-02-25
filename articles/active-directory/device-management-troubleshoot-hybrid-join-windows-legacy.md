---
title: "Birleştirilmiş alt düzey cihazlar Azure Active Directory karma sorunlarını giderme | Microsoft Docs"
description: "Karma Azure Active Directory sorun giderme alt düzey aygıtları katıldı."
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
ms.date: 11/08/2017
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: ecf77a614922ef58cdfb2b2c8174f66e01ea9b46
ms.sourcegitcommit: fbba5027fa76674b64294f47baef85b669de04b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/24/2018
---
# <a name="troubleshooting-hybrid-azure-active-directory-joined-down-level-devices"></a>Alt düzey aygıtları birleştirilmiş karma Azure Active Directory sorun giderme 

Bu konuda, yalnızca aşağıdaki cihazlar için geçerlidir: 

- Windows 7 
- Windows 8.1 
- Windows Server 2008 R2 
- Windows Server 2012 
- Windows Server 2012 R2 
 

Windows 10 veya Windows Server 2016 için bkz: [sorun giderme karma Azure Active Directory'ye katılmış Windows 10 ve Windows Server 2016 cihazları](device-management-troubleshoot-hybrid-join-windows-current.md).

Bu konu, sahibi olduğunuzu varsayar [yapılandırılmış karma Azure Active Directory'ye katılmış cihazları](device-management-hybrid-azuread-joined-devices-setup.md) aşağıdaki senaryoları desteklemek için:

- Cihaz temelli koşullu erişim

- [Kurumsal Dolaşım ayarları](active-directory-windows-enterprise-state-roaming-overview.md)

- [İş İçin Windows Hello](active-directory-azureadjoin-passport-deployment.md) 





Bu konu, sorun giderme ile ilgili olası sorunları gidermek nasıl yönergelerini sağlar.  

**Bilmeniz gerekenler:** 

- En fazla kullanıcı başına aygıtları Aygıt odaklı sayısıdır. Örneğin, varsa *jdoe* ve *jharnett* oturum açma bir cihaza, ayrı bir kayıt (DeviceID) için oluşturulduğunda bunların her birini **kullanıcı** bilgileri sekmesi.  

- İlk kaydı / aygıtların birleştirme denemesi oturum açma veya kilidi gerçekleştirmek / kilidini açmak için yapılandırılır. Bir Görev Zamanlayıcı görevi tarafından tetiklenen 5 dakikalık gecikme olabilir. 

- İşletim sistemi veya el ile yeniden unregister kaldırıp yeniden kaydettirin oluşturabilir yeni bir kayıt üzerinde Azure AD ve Azure portalında kullanıcı bilgileri sekmesi altında birden çok giriş sonuçlanır. 

## <a name="step-1-retrieve-the-registration-status"></a>1. adım: kayıt durumunu alma 

**Kayıt durumunu doğrulamak için:**  

1. Komut istemini yönetici olarak açın 

2. Türü `"%programFiles%\Microsoft Workplace Join\autoworkplace.exe /i"`

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
    
    1. Kullanıcının oturum açtığı etki alanı kullanıcı (örneğin, yerel bir kullanıcı) değildir. Alt düzey cihazlarda karma Azure AD birleştirme yalnızca etki alanı kullanıcıları için desteklenir.
    
    2. Herhangi bir nedenle Autoworkplace.exe sessizce Azure AD veya AD FS kimlik doğrulaması yapamıyorsa. Birkaç olası nedenleri (Önkoşul denetimi) Azure AD URL'lere dışarı bağlı ağ bağlantısı sorunları olabilir veya kullanıcı için etkin/yapılandırılan MFA, ancak WIAORMUTLIAUTHN (onay yapılandırma adımlarını) federasyon sunucusunda yapılandırılmamış olabilir. Başka bir olasılık, bu giriş bölgesi bulma (HRD) sayfasını Autoworkplace.exe sessizce bir belirteç elde etmesini engelleyen bir kullanıcı etkileşimi bekliyor olabilir.
    
    3. Kuruluş Azure AD kullanıyorsanız sorunsuz çoklu oturum açma, aşağıdaki URL'ler cihazın IE intranet ayarlarını yok:
    
       - https://autologon.microsoftazuread-sso.com
       - https://aadg.windows.net.nsatc.net
    
       ve Intranet bölgesi için "Durum çubuğunda komut dosyası aracılığıyla izin güncelleştirmeler" ayarının etkinleştirilmesi gerekir.

- Bir kotasına ulaşıldı

    ![Windows için çalışma alanına katılma](./media/active-directory-device-registration-troubleshoot-windows-legacy/04.png)

- Hizmeti yanıt vermiyor 

    ![Windows için çalışma alanına katılma](./media/active-directory-device-registration-troubleshoot-windows-legacy/05.png)

Olay günlüğünde altında durum bilgisi bulabilirsiniz **uygulamaları ve Hizmetleri Log\Microsoft-çalışma alanına katılma**.
  
**Başarısız karma Azure AD katılım için en yaygın nedenler şunlardır:** 

- Bilgisayarınız bir şirket içi bağlantısı olmadan VPN'yi veya kuruluşunuzun iç ağ üzerinde değil AD etki alanı denetleyicisi.

- Bilgisayarınıza yerel bilgisayar hesabı ile oturum. 

- Hizmet yapılandırma sorunları: 

  - Federasyon sunucusunu desteklemek üzere yapılandırılmış **WIAORMULTIAUTHN**. 

  - Bilgisayar için ait olduğu AD ormanında Azure AD'de doğrulanmış etki alanı adınızı işaret eden bir hizmet bağlantı noktası nesne yok.

  - Bir kullanıcı cihaz sınırına ulaştı. 

## <a name="next-steps"></a>Sonraki adımlar

Soruları için bkz: [aygıt yönetimi hakkında SSS](device-management-faq.md)  
