---
title: Hibrit Azure Active Directory sorun giderme, alt düzey cihazları katılmış | Microsoft Docs
description: Hibrit Azure Active Directory sorun giderme alt düzey cihazları katıldı.
services: active-directory
documentationcenter: ''
author: MicrosoftGuyJFlo
manager: daveba
ms.assetid: cdc25576-37f2-4afb-a786-f59ba4c284c2
ms.service: active-directory
ms.subservice: devices
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/23/2018
ms.author: joflore
ms.reviewer: jairoc
ms.collection: M365-identity-device-management
ms.openlocfilehash: a68e5a12333e1ee9e920b69599796164534e3c25
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67110523"
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


Bu makalede, sorun giderme rehberi olası sorunların nasıl giderileceğini üzerinde ile sunulmaktadır.  

**Bilmeniz gerekenler:** 

- Hibrit Azure AD'ye katılma için Windows cihazları works biraz daha farklı daha alt düzey Windows 10'da gerçekleştirir. Birçok müşteri AD FS (Federasyon etki alanları için) ihtiyaç duydukları veya sorunsuz çoklu oturum açma (için yönetilen etki alanları) yapılandırılmış başlığımız dikkatinizi çekmiş olabilir değil.

- Hizmet bağlantı noktası (SCP) (örneğin, contoso.onmicrosoft.com, contoso.com yerine), yönetilen etki alanı adına işaret şekilde yapılandırıldıysa, alt düzey Windows cihazlar için Azure AD'ye katılım hibrit Federasyon etki alanlarında olan müşteriler için ardından olur çalışmıyor.

- Şu anda kullanıcı başına cihaz sayısı, alt düzey hibrit Azure AD'ye katılmış cihazlar için de geçerlidir. 

- Aynı fiziksel cihaza ne zaman birden çok etki alanı kullanıcıları alt düzey hibrit Azure AD'ye katılmış cihazların oturum birden çok kez Azure AD'de görünür.  Örneğin, varsa *jdoe* ve *jharnett* oturum açma ayrı bir kayıt (DeviceID) bir cihaza oluşturulur bunlardan her biri için **kullanıcı** bilgileri sekmesi. 

- Ayrıca birden çok girişi bir cihazda kullanıcı bilgileri sekmesi için işletim sistemi veya bir el ile yeniden kayıt yüklenmesinden dolayı alabilirsiniz.

- İlk kayıt / cihazların birleştirme girişimi oturum açma veya kilit gerçekleştirmek / kilidini açmak için yapılandırılır. Zamanlayıcı görevi tarafından tetiklenen 5 dakika gecikme olabilir. 

- Emin [KB4284842](https://support.microsoft.com/help/4284842) Windows 7 SP1 veya Windows Server 2008 R2 SP1 durumunda yüklenir. Bu güncelleştirme, parolayı değiştirdikten sonra korumalı anahtarları gelecekteki kimlik doğrulama hataları nedeniyle müşterinin erişim kaybını engeller.

## <a name="step-1-retrieve-the-registration-status"></a>1\. adım: Kayıt durumu alma 

**Kayıt durumunu doğrulamak için:**  

1. Hibrit Azure AD'ye katılım gerçekleştirilen bir kullanıcı hesabıyla oturum açın.

2. Komut istemini açın 

3. Türü `"%programFiles%\Microsoft Workplace Join\autoworkplace.exe" /i`

Bu komut, birleşim durumu hakkında ayrıntılar sağlayan bir iletişim kutusu görüntüler.

![Windows için çalışma alanına katılma](./media/troubleshoot-hybrid-join-windows-legacy/01.png)


## <a name="step-2-evaluate-the-hybrid-azure-ad-join-status"></a>2\. adım: Hibrit Azure AD'ye katılma durumu değerlendirin 

Cihaz hibrit Azure AD'ye katılmış başarısız olduysa, "Birleştirme" düğmesine tıklayarak hibrit Azure AD'ye katılma yapmak deneyebilirsiniz. Hibrit Azure AD'ye katılım yapmak için deneme başarısız olursa, hata hakkındaki ayrıntılar gösterilir.


**En yaygın sorunlar şunlardır:**

- AD FS veya Azure AD yanlış yapılandırılmış veya ağ sorunları

    ![Windows için çalışma alanına katılma](./media/troubleshoot-hybrid-join-windows-legacy/02.png)
    
  - Azure AD veya AD FS ile sessizce kimlik doğrulaması Autoworkplace.exe silemiyor. Bu, eksik neden olmuş olabilir veya AD FS (için Federasyon etki alanları) veya eksik veya yanlış yapılandırılmış Azure AD sorunsuz çoklu oturum açma (için yönetilen etki alanları) veya ağ sorunları yanlış yapılandırılmış. 
    
    - Çok faktörlü kimlik doğrulaması (MFA) kullanıcı için etkin/yapılandırılan ve AD FS sunucusunda WIAORMULTIAUTHN yapılandırılmamış olabilir. 
     
    - Bu giriş bölgesi bulma (HRD) sayfasını engelleyen kullanıcı etkileşimi için bekleyen başka bir olasılıktır **autoworkplace.exe** sessiz bir belirteç isteyen öğesinden.
     
    - AD FS ve Azure AD URL'leri IE'nin Intranet istemci üzerinde eksik olduğundan emin olabilir.
     
    - Ağ bağlantı sorunları engelliyor **autoworkplace.exe** AD FS veya Azure AD URL'leri ulaşmasını. 
     
    - **Autoworkplace.exe** gerektirir istemcinin doğrudan görebilmesi istemciden kuruluşun şirket içi AD etki alanı denetleyicisi, hibrit Azure AD'ye katılma başarılı yalnızca kuruluşunuzun intranet istemci bağlandığında anlamına gelir.
     
    - Azure AD sorunsuz çoklu oturum açma, kuruluşunuzun kullandığı `https://autologon.microsoftazuread-sso.com` veya `https://aadg.windows.net.nsatc.net` cihazın IE intranet ayarlarını, mevcut olmayan ve **izin vermek için durum çubuğu komut dosyası aracılığıyla güncelleştirmeleri** Intranet bölgesi için etkin değil.

- Bir etki alanı kullanıcısı olarak imzalı değil

    ![Windows için çalışma alanına katılma](./media/troubleshoot-hybrid-join-windows-legacy/03.png)
    
    Neden bu durum ortaya çıkabilir birkaç farklı nedeni vardır:
    
    - Oturum açmış olan kullanıcının etki alanı kullanıcısı (örneğin, yerel bir kullanıcı) değil. Alt düzey cihazlar karma Azure AD join, yalnızca etki alanı kullanıcıları için desteklenir.
    
    - İstemci, bir etki alanı denetleyicisine bağlanmak kuramıyor.    

- Bir kotasına ulaşıldı

    ![Windows için çalışma alanına katılma](./media/troubleshoot-hybrid-join-windows-legacy/04.png)

- Hizmeti yanıt vermiyor 

    ![Windows için çalışma alanına katılma](./media/troubleshoot-hybrid-join-windows-legacy/05.png)

Ayrıca, altında olay günlüğüne durum bilgileri de bulabilirsiniz: **Uygulamaları ve Hizmetleri Log\Microsoft-çalışma alanına katılma**
  
**Başarısız hibrit Azure AD'ye katılma en yaygın nedenleri şunlardır:** 

- Bilgisayarınız, kuruluşunuzun iç ağ veya VPN ile şirket içi bağlantı bağlanmadı AD etki alanı denetleyicisi.

- Bilgisayarınızın yerel bilgisayar hesabı ile günlüğe kaydedilir. 

- Hizmet yapılandırma sorunları: 

  - AD FS sunucusuna desteklemek üzere yapılandırılmamış **WIAORMULTIAUTHN**. 

  - Doğrulanmış etki alanı adınızı Azure AD'ye işaret eden hizmet bağlantı noktası nesne bilgisayarınızın ormanda vardır 
  
  - Veya etki alanınızda yönetilen ardından sorunsuz çoklu oturum açma yapılandırılmamış veya çalışma.

  - Bir kullanıcı cihaz sınırına ulaştı. 

## <a name="next-steps"></a>Sonraki adımlar

Sorularınız varsa bkz [cihaz yönetimi hakkında SSS](faq.md)  
