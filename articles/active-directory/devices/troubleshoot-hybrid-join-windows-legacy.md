---
title: Hibrit Azure Active Directory sorun giderme, alt düzey cihazları katılmış | Microsoft Docs
description: Hibrit Azure Active Directory sorun giderme alt düzey cihazları katıldı.
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
ms.openlocfilehash: aad2b4c3edcdc488257940062e8861613ece25e8
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2018
ms.locfileid: "42062126"
---
# <a name="troubleshooting-hybrid-azure-active-directory-joined-down-level-devices"></a>Alt düzey cihazları katılmış karma Azure Active Directory sorun giderme 

Bu makalede, yalnızca aşağıdaki cihazlar için geçerlidir: 

- Windows 7 
- Windows 8.1 
- Windows Server 2008 R2 
- Windows Server 2012 
- Windows Server 2012 R2 
 

Windows 10 veya Windows Server 2016 için bkz: [sorun giderme hibrit Azure Active Directory'ye katılmış Windows 10 ve Windows Server 2016 cihazları](troubleshoot-hybrid-join-windows-current.md).

Bu makalede, sahibi olduğunuzu varsayar [yapılandırılmış hibrit Azure Active Directory alanına katılmış cihazlar](hybrid-azuread-join-plan.md) aşağıdaki senaryoları desteklemek için:

- Cihaz tabanlı koşullu erişim

- [Kurumsal Dolaşım ayarları](../active-directory-windows-enterprise-state-roaming-overview.md)

- [İş İçin Windows Hello](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-identity-verification) 





Bu makalede, sorun giderme rehberi olası sorunların nasıl giderileceğini üzerinde ile sunulmaktadır.  

**Bilmeniz gerekenler:** 

- Kullanıcı başına cihaz sayısı, aygıt odaklı. Örneğin, varsa *jdoe* ve *jharnett* oturum açma ayrı bir kayıt (DeviceID) bir cihaza oluşturulur bunlardan her biri için **kullanıcı** bilgileri sekmesi.  

- İlk kayıt / cihazların birleştirme girişimi oturum açma veya kilit gerçekleştirmek / kilidini açmak için yapılandırılır. Zamanlayıcı görevi tarafından tetiklenen 5 dakika gecikme olabilir. 

- Bir işletim sistemi veya bir el ile yeniden kayıt yüklenmesinden dolayı bir cihazda kullanıcı bilgileri sekmesi için birden çok girişi elde edebilirsiniz. 

- Emin [KB4284842](https://support.microsoft.com/help/4284842) Windows 7 SP1 veya Windows Server 2008 R2 SP1 durumunda yüklenir. Bu güncelleştirme, parolayı değiştirdikten sonra korumalı anahtarları gelecekteki kimlik doğrulama hataları nedeniyle müşterinin erişim kaybını engeller.

## <a name="step-1-retrieve-the-registration-status"></a>1. adım: kayıt durumunu alma 

**Kayıt durumunu doğrulamak için:**  

1. Hibrit Azure AD'ye katılım gerçekleştirilen bir kullanıcı hesabıyla oturum açın.

2. Komut istemini yönetici olarak açın. 

3. Türü `"%programFiles%\Microsoft Workplace Join\autoworkplace.exe" /i`

Bu komut, birleşim durumu hakkında daha fazla ayrıntı sağlayan bir iletişim kutusu görüntüler.

![Windows için çalışma alanına katılma](./media/troubleshoot-hybrid-join-windows-legacy/01.png)


## <a name="step-2-evaluate-the-hybrid-azure-ad-join-status"></a>2. adım: ' % s'hibrit Azure AD'ye katılma durumu değerlendirme 

Hibrit Azure AD'ye katılma başarılı olmadıysa, iletişim kutusu, oluşan sorunla ilgili ayrıntıları sağlar.

**En yaygın sorunlar şunlardır:**

- Yanlış yapılandırılmış AD FS veya Azure AD

    ![Windows için çalışma alanına katılma](./media/troubleshoot-hybrid-join-windows-legacy/02.png)

- Bir etki alanı kullanıcısı olarak imzalı değil

    ![Windows için çalışma alanına katılma](./media/troubleshoot-hybrid-join-windows-legacy/03.png)
    
    Neden bu durum ortaya çıkabilir birkaç farklı nedeni vardır:
    
    - Oturum açmış olan kullanıcının etki alanı kullanıcısı (örneğin, yerel bir kullanıcı) değil. Alt düzey cihazlar karma Azure AD join, yalnızca etki alanı kullanıcıları için desteklenir.
    
    - Azure AD veya AD FS ile sessizce kimlik doğrulaması Autoworkplace.exe silemiyor. Bu bir dışarı ilişkili ağ bağlantısıyla ilgili sorunlar için Azure AD URL'leri kaynaklanabilir. Ayrıca, çok faktörlü kimlik doğrulaması (MFA) kullanıcı için etkin/yapılandırılan ve WIAORMUTLIAUTHN federasyon sunucusunda yapılandırılmamış olabilir. Bu giriş bölgesi bulma (HRD) sayfasını engelleyen kullanıcı etkileşimi için bekleyen başka bir olasılıktır **autoworkplace.exe** sessiz bir belirteç isteyen öğesinden.
    
    - Azure AD sorunsuz çoklu oturum açma, kuruluşunuzun kullandığı `https://autologon.microsoftazuread-sso.com` veya `https://aadg.windows.net.nsatc.net` cihazın IE intranet ayarlarını, mevcut olmayan ve **izin vermek için durum çubuğu komut dosyası aracılığıyla güncelleştirmeleri** Intranet bölgesi için etkin değil.

- Bir kotasına ulaşıldı

    ![Windows için çalışma alanına katılma](./media/troubleshoot-hybrid-join-windows-legacy/04.png)

- Hizmeti yanıt vermiyor 

    ![Windows için çalışma alanına katılma](./media/troubleshoot-hybrid-join-windows-legacy/05.png)

Altında olay günlüğüne durum bilgileri bulabilirsiniz: **uygulamaları ve Hizmetleri Log\Microsoft çalışma alanına katılma**
  
**Başarısız hibrit Azure AD'ye katılma en yaygın nedenleri şunlardır:** 

- Bilgisayarınız, kuruluşunuzun iç ağ veya VPN ile şirket içi bağlantı bağlanmadı AD etki alanı denetleyicisi.

- Bilgisayarınızın yerel bilgisayar hesabı ile günlüğe kaydedilir. 

- Hizmet yapılandırma sorunları: 

  - Federasyon sunucusunu desteklemek üzere yapılandırılmış **WIAORMULTIAUTHN**. 

  - Doğrulanmış etki alanı adınızı Azure AD'ye işaret eden hizmet bağlantı noktası nesne bilgisayarınızın ormanda vardır 

  - Bir kullanıcı cihaz sınırına ulaştı. 

## <a name="next-steps"></a>Sonraki adımlar

Sorularınız varsa bkz [cihaz yönetimi hakkında SSS](faq.md)  
