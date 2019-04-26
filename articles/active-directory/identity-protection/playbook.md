---
title: Azure Active Directory kimlik koruması Kılavuzu | Microsoft Docs
description: Azure AD kimlik koruması, güvenliği aşılmış kimlik veya cihaz yararlanma ve güvenli bir kimlik veya daha önce olduğundan şüphelenilen veya tehlikeye bilinen bir cihaz yeteneği bir saldırganın sınırlamak nasıl olanak tanıdığını öğrenin.
services: active-directory
keywords: Azure active directory kimlik koruması, bulut bulma, yönetme, uygulamaları, güvenlik, risk, risk düzeyi, güvenlik açığı, güvenlik ilkesi
documentationcenter: ''
author: MicrosoftGuyJFlo
manager: daveba
ms.assetid: 60836abf-f0e9-459d-b344-8e06b8341d25
ms.service: active-directory
ms.subservice: identity-protection
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2018
ms.author: joflore
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: 697bb8a60861acb120e92d8fd1dda3892a957b57
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60294387"
---
# <a name="azure-active-directory-identity-protection-playbook"></a>Azure Active Directory kimlik koruması Kılavuzu

Playbook'u size yardımcı olur:

* Kimlik koruması ortamında veri benzetimi risk olaylarının ve güvenlik açıklarının tarafından Doldur
* Risk tabanlı koşullu erişim ilkeleri ayarlama ve bu ilkeler etkisini test edin


## <a name="simulating-risk-events"></a>Risk olaylarının benzetimini

Bu bölüm aşağıdaki risk olayı türleri benzetimi için adımları sağlar.

* (Bir kolayca) anonim IP adreslerinden oturum açma
* (Orta) alışılmadık konumlardan oturum açma
* (Güç) alışılmadık konumlara imkansız seyahat

Diğer risk olayları güvenli bir şekilde benzetimi yapılamaz.

### <a name="sign-ins-from-anonymous-ip-addresses"></a>Anonim IP adreslerinden oturum açma işlemleri

Bu risk olayı hakkında daha fazla bilgi için bkz: [anonim IP adreslerinden oturum açma](../reports-monitoring/concept-risk-events.md#sign-ins-from-anonymous-ip-addresses). 

Aşağıdaki yordamı tamamlayarak kullanmanızı gerektirir:

- [Tor tarayıcı](https://www.torproject.org/projects/torbrowser.html.en) anonim IP adresleri benzetimini yapmak için. Kuruluşunuz kısıtladığında Tor tarayıcınızı kullanarak bir sanal makine kullanmanız gerekebilir.
- Çok faktörlü kimlik doğrulaması için henüz kaydedilmemiş bir test hesabı.

**Bir oturum açma bir anonim IP benzetimini yapmak için aşağıdaki adımları gerçekleştirin.**:

1. Kullanarak [Tor tarayıcı](https://www.torproject.org/projects/torbrowser.html.en), gitmek [ https://myapps.microsoft.com ](https://myapps.microsoft.com).   
2. Görünmesini istediğiniz hesabı kimlik bilgilerini girin **anonim IP adreslerinden oturum açma** rapor.

Oturum açma kimlik koruması panosunda 10-15 dakika içinde gösterilir. 

### <a name="sign-ins-from-unfamiliar-locations"></a>Alışılmadık konumlardan oturum açma işlemleri

Bu risk olayı hakkında daha fazla bilgi için bkz: [alışılmadık konumlardan oturum açma](../reports-monitoring/concept-risk-events.md#sign-in-from-unfamiliar-locations). 

Tanınmayan konumlardan benzetimini yapmak için bir konum ve cihaz test hesabınızı öğesinden önce oturum açmadı oturum açmasına gerek.

Aşağıdaki yordam, yeni oluşturulan kullanır:

- Yeni bir konuma benzetimini yapmak için VPN bağlantısı.

- Yeni bir cihazın benzetimini yapmak için VM.

Aşağıdaki yordamı tamamladıktan sonra olan bir kullanıcı hesabı kullanmanızı gerektirir:

- Oturum açma en az 30 günlük geçmiş.
- Çok faktörlü kimlik doğrulaması etkin.


**Bir oturum açma tanınmayan bir konumdan benzetimini yapmak için aşağıdaki adımları gerçekleştirin.**:

1. Test hesabınızla oturum açtığınızda, MFA testini MFA testini geçmiyor başarısız.
2. Yeni VPN kullanarak [ https://myapps.microsoft.com ](https://myapps.microsoft.com) test hesabınızın kimlik bilgilerini girin.
   

Oturum açma kimlik koruması panosunda 10-15 dakika içinde gösterilir.

### <a name="impossible-travel-to-atypical-location"></a>Alışılmadık konuma imkansız seyahat

Bu risk olayı hakkında daha fazla bilgi için bkz: [mümkün alışılmadık konuma seyahat](../reports-monitoring/concept-risk-events.md#impossible-travel-to-atypical-locations). 

Mümkün olmayan seyahat koşul benzetimi, algoritma mümkün olmayan seyahat tanıdık cihazlardan veya dizinindeki diğer kullanıcılar tarafından kullanılan VPN'ler oturum açma işlemleri gibi yanlış pozitifleri kullanıma ortadan kaldırmak için machine learning kullandığından zordur. Ayrıca, risk olayları oluşturma başlamadan önce algoritması bir oturum açma geçmiş 14 gün ve kullanıcının 10 oturum açma bilgileri gerektirir. Karmaşık makine öğrenimi modellerini nedeniyle ve kurallarına yukarıda, aşağıdaki adımları bir risk olayına neden olmayan bir fırsat yoktur. Bu risk olayı yayımlamak birden çok Azure AD hesapları için bu adımları çoğaltma isteyebilirsiniz.


**Bir alışılmadık konuma imkansız seyahat benzetimini yapmak için aşağıdaki adımları gerçekleştirin.**:

1. Standart tarayıcınızı kullanarak [ https://myapps.microsoft.com ](https://myapps.microsoft.com).  
2. Bir imkansız seyahat risk olayı oluşturmak istediğiniz hesabın kimlik bilgilerini girin.
3. Kullanıcı aracınız değiştirin. Geliştirici Araçları'ndan kullanıcı aracısı Internet Explorer'da değiştirmek ya da Firefox veya Chrome'da bir kullanıcı aracısı değiştirici eklentiyi kullanarak, kullanıcı aracısını değiştirmek.
4. IP adresiniz değiştirin. IP adresiniz bir Tor eklentisi, bir VPN kullanarak veya farklı veri merkezindeki azure'da yeni bir makine dönen değiştirebilirsiniz.
5. Oturum açma için [ https://myapps.microsoft.com ](https://myapps.microsoft.com) olarak önce ve sonra önceki oturum açma birkaç dakika içinde aynı kimlik bilgilerini kullanarak.

Oturum açma kimlik koruması panosunda 2-4 saat içinde gösterilir.

## <a name="simulating-vulnerabilities"></a>Güvenlik açıklarını benzetimi
Bir Azure AD ortamında kötü bir aktör tarafından kötüye kullanılmadan zayıf güvenlik açıklarıdır. Şu anda Azure ad diğer özelliklerden yararlanın Azure AD kimlik koruması 3 tür güvenlik açıklarına takip edilir. Bu özellikleri ayarladıktan sonra bu güvenlik açıklarını otomatik olarak kimlik koruması panosunda görüntülenir.

* Azure AD [çok faktörlü kimlik doğrulaması](../authentication/multi-factor-authentication.md)
* Azure AD [Cloud Discovery](https://docs.microsoft.com/cloud-app-security/).
* Azure AD [Privileged Identity Management](../privileged-identity-management/pim-configure.md). 


## <a name="testing-security-policies"></a>Güvenlik ilkeleri test etme

Bu bölümde kullanıcı riski ve oturum açma riski İlkesi test etmek için adımları sağlar.


### <a name="user-risk-security-policy"></a>Kullanıcı riski İlkesi

Daha fazla bilgi için bkz. [Kullanıcı risk ilkesini yapılandırma](howto-user-risk-policy.md).

![Kullanıcı riski](./media/playbook/02.png "Playbook")


**Bir kullanıcı riski İlkesi test etmek için aşağıdaki adımları gerçekleştirin.**:

1. Oturum açma için [ https://portal.azure.com ](https://portal.azure.com) kiracınız için genel yönetici kimlik bilgilerine sahip.
2. Gidin **kimlik koruması**. 
3. Üzerinde **Azure AD kimlik koruması** sayfasında **kullanıcı riski İlkesi**.
4. İçinde **atamaları** bölümünde, istediğiniz kullanıcıları (ve grup) seçin ve kullanıcı risk düzeyi.

    ![Kullanıcı riski](./media/playbook/03.png "Playbook")

5. İstenen erişim denetimi denetimleri bölümüne seçin (örneğin parola değişikliği iste).
5. Olarak **ilke zorlama**seçin **kapalı**.
6. Bir test hesabının, kullanıcı riski, örneğin, bir risk olaylarının benzetimini birkaç kez Yükselt.
7. Birkaç dakika bekleyin ve ardından, kullanıcı için kullanıcı düzeyi, Orta olduğunu doğrulayın. Aksi takdirde kullanıcı için daha fazla risk olaylarının benzetimini yapma.
8. Olarak **ilke zorlama**seçin **üzerinde**.
9. Ayrıca, bir yükseltilmiş risk düzeyi bir kullanıcı ile oturum tarafından kullanıcı risk tabanlı koşullu erişim artık test edebilirsiniz.
    
    

### <a name="sign-in-risk-security-policy"></a>Oturum açma riski İlkesi

Daha fazla bilgi için bkz. [Oturum açma risk ilkesini yapılandırma](howto-sign-in-risk-policy.md).

![Oturum açma riski](./media/playbook/01.png "Playbook")


**Bir oturum açma riski İlkesi'nde test etmek için aşağıdaki adımları gerçekleştirin:**

1. Oturum açma için [ https://portal.azure.com ](https://portal.azure.com) kiracınız için genel yönetici kimlik bilgilerine sahip.

2. Gidin **Azure AD kimlik koruması**.

3. Ana **Azure AD kimlik koruması** sayfasında **oturum açma riski İlkesi**. 

4. İçinde **atamaları** bölümünde istediğiniz kullanıcıları (ve grup) seçin ve oturum açma risk düzeyini.

    ![Oturum açma riski](./media/playbook/04.png "Playbook")


5. İçinde **denetimleri** bölümünde, istenen erişim denetimi seçin (örneğin, **çok faktörlü kimlik doğrulaması gerektiren**). 

6. Olarak **ilke zorlama**seçin **üzerinde**.

7. **Kaydet**’e tıklayın.

8. Artık oturum açma Risk tabanlı koşullu erişim (örneğin, Tor tarayıcısı kullanılarak) kullanarak bir riskli oturum açarak test edebilirsiniz. 

 




## <a name="see-also"></a>Ayrıca bkz.

- [Azure Active Directory kimlik koruması](../active-directory-identityprotection.md)

