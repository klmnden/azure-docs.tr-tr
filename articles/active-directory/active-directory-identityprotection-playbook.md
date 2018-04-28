---
title: Azure Active Directory kimlik koruması Kılavuzu | Microsoft Docs
description: Azure AD kimlik koruması nasıl yeteneğini bir saldırgan güvenliği aşılmış kimlik veya aygıt yararlanmaya ve güvenli bir kimlik veya önceden şüpheli veya tehlikeye bilinen bir cihaz için sınırlamak sağladığını öğrenin.
services: active-directory
keywords: Azure active directory kimlik koruması, cloud app discovery'yi, uygulamalar, güvenlik, risk, risk düzeyi, güvenlik açığı, güvenlik ilkesi yönetme
documentationcenter: ''
author: MarkusVi
manager: mtillman
ms.assetid: 60836abf-f0e9-459d-b344-8e06b8341d25
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/07/2018
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 210d097f0719725a0ecf145ce536875a383b04e6
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2018
---
# <a name="azure-active-directory-identity-protection-playbook"></a>Azure Active Directory kimlik koruması Kılavuzu

Bu playbook size yardımcı olur:

* Kimlik koruması ortamında verileri benzetirme risk olaylarına ve güvenlik açıkları ile doldurma
* Risk bağlı olarak koşullu erişim ilkeleri Ayarla ve bu ilkeler etkisini test etme


## <a name="simulating-risk-events"></a>Risk olaylarını benzetimini yapma

Bu bölümde aşağıdaki risk olayı türleri benzetimi için adımlara sağlar:

* Oturum açma işlemleri anonim IP adreslerinden (kolay)
* Oturum açma işlemleri tanınmayan konumlardan (Orta)
* (Zor) alışılmadık konumlara imkansız seyahat

Diğer risk olaylarını güvenli bir şekilde benzetimi yapılamaz.

### <a name="sign-ins-from-anonymous-ip-addresses"></a>Anonim IP adreslerinden oturum açma işlemleri

Bu riski olay hakkında daha fazla bilgi için bkz: [anonim IP adreslerinden gerçekleştirilen oturum açma işlemleri](active-directory-reporting-risk-events.md#sign-ins-from-anonymous-ip-addresses). 

Aşağıdaki yordamı tamamlayarak kullanmanızı gerektirir:

- [Tor tarayıcı](https://www.torproject.org/projects/torbrowser.html.en) anonim IP adreslerini benzetimini yapmak için. Kuruluşunuz Tor tarayıcı kullanarak kısıtladığında bir sanal makine kullanmanız gerekebilir.
- Çok faktörlü kimlik doğrulaması için henüz kaydedilmemiş bir test hesabı.

**Bir oturum açma anonim bir IP adresinden benzetimini yapmak için aşağıdaki adımları gerçekleştirin**:

1. Kullanarak [Tor tarayıcı](https://www.torproject.org/projects/torbrowser.html.en), gitmek [ https://myapps.microsoft.com ](https://myapps.microsoft.com).   
2. Görünmesini istediğiniz hesap kimlik bilgilerini girin **anonim IP adreslerinden gerçekleştirilen oturum açma işlemleri** rapor.

Oturum açma kimlik koruması Panoda 10-15 dakika içinde görünür. 

### <a name="sign-ins-from-unfamiliar-locations"></a>Alışılmadık konumlardan oturum açma işlemleri

Bu riski olay hakkında daha fazla bilgi için bkz: [tanınmayan konumlardan gerçekleştirilen oturum açma işlemleri](active-directory-reporting-risk-events.md#sign-in-from-unfamiliar-locations). 

Tanınmayan konumlardan benzetimini yapmak için bir konum ve test hesabınız öğesinden önce oturum açtığı değil aygıttan oturum açmak zorunda.

Aşağıdaki yordam, yeni oluşturulan kullanır:

- VPN bağlantısı, yeni konum benzetimini yapmak için.

- Sanal yeni bir cihazın benzetimini yapmak için makine.

Aşağıdaki yordamı tamamlayarak olan bir kullanıcı hesabı kullanmanızı gerektirir:

- Oturum açma en az 30 günlük geçmişi.
- Çok faktörlü kimlik doğrulaması etkin.


**Bir oturum açma tanınmayan bir konumdan benzetimini yapmak için aşağıdaki adımları gerçekleştirin**:

1. Test hesabınızla oturum açarken MFA testini MFA testini geçirerek değil başarısız.
2. Yeni, VPN kullanarak gidin [ https://myapps.microsoft.com ](https://myapps.microsoft.com) ve test hesabınız kimlik bilgilerini girin.
   

Oturum açma kimlik koruması Panoda 10-15 dakika içinde görünür.

### <a name="impossible-travel-to-atypical-location"></a>Alışılmadık konumu imkansız seyahat

Bu riski olay hakkında daha fazla bilgi için bkz: [Impossible alışılmadık konuma seyahat](active-directory-reporting-risk-events.md#impossible-travel-to-atypical-locations). 

Makine yanlış pozitifler tanıdık aygıtlardan mümkün olmayan seyahat veya oturum açmalardan dizindeki diğer kullanıcılar tarafından kullanılan VPN'ler gibi çıkışı ortadan kaldırmak öğrenme algoritmasını kullandığı için mümkün olmayan seyahat koşul benzetimi zor olabilir. Ayrıca, risk olaylarını oluşturma işlemi başlamadan önce algoritma 14 gün oturum açma geçmişini ve kullanıcının 10 oturumları gerektirir. Karmaşık makine öğrenimi modellerini nedeniyle ve kurallar üzerinde aşağıdaki adımları risk olaya neden olmayan bir fırsat yoktur. Bu risk olayı yayımlamak birden çok Azure AD hesapları için bu adımları çoğaltmak istediğiniz.


**Bir mümkün olmayan seyahat alışılmadık konuma benzetimini yapmak için aşağıdaki adımları gerçekleştirin**:

1. Standart, tarayıcı kullanarak gidin [ https://myapps.microsoft.com ](https://myapps.microsoft.com).  
2. İçin bir mümkün olmayan seyahat risk olay oluşturmak için kullanmak istediğiniz hesabın kimlik bilgilerini girin.
3. Kullanıcı Aracısı değiştirin. Geliştirici Araçları'ndan kullanıcı aracısı Internet Explorer'da değiştirmek veya kullanıcı aracınız Firefox veya kullanıcı aracısı değiştirici Eklentisi'ni kullanarak Chrome değiştirin.
4. IP adresini değiştirin. VPN, Tor eklentisi kullanılarak veya farklı bir veri merkezinde Azure içinde yeni bir makine dönmesini IP adresiniz değiştirebilirsiniz.
5. Oturum açma için [ https://myapps.microsoft.com ](https://myapps.microsoft.com) olarak önce ve sonra önceki oturum açma birkaç dakika içinde aynı kimlik bilgilerini kullanarak.

Oturum açma kimlik koruması panosunda 2-4 saat içinde görüntülenir.

## <a name="simulating-vulnerabilities"></a>Güvenlik açıkları benzetimini yapma
Güvenlik açıkları tarafından hatalı aktör yararlanan bir Azure AD ortamda zayıf giderilmiştir. Şu anda açık 3 türlerinin diğer özelliklerden Azure ad içinde Azure AD Identity Protection çıkmış. Bu özellikler ayarlandıktan sonra bu güvenlik açıklarından otomatik olarak kimlik koruması panosunda görüntülenir.

* Azure AD [çok faktörlü kimlik doğrulaması](authentication/multi-factor-authentication.md)
* Azure AD [Cloud App Discovery](active-directory-cloudappdiscovery-whatis.md).
* Azure AD [Privileged Identity Management](active-directory-privileged-identity-management-configure.md). 


## <a name="testing-security-policies"></a>Güvenlik ilkelerini sınama

Bu bölümde kullanıcı riski ve oturum açma riski güvenlik ilkesini test etmek için adımları sağlar.


### <a name="user-risk-security-policy"></a>Kullanıcı risk güvenlik ilkesi

Daha fazla bilgi için bkz: [kullanıcı risk Güvenlik İlkesi](active-directory-identityprotection.md#user-risk-security-policy).

![Kullanıcı riski](./media/active-directory-identityprotection-playbook/02.png "Playbook")


**Bir kullanıcı risk güvenlik ilkesini test etmek için aşağıdaki adımları gerçekleştirin**:

1. Oturum açma için [ https://portal.azure.com ](https://portal.azure.com) kiracınız için genel yönetici kimlik bilgilerine sahip.
2. Gidin **kimlik koruması**. 
3. Üzerinde **Azure AD Identity Protection** sayfasında, **kullanıcı risk ilkesine**.
4. İçinde **atamaları** bölümünde, istenen kullanıcıların (ve gruplar) seçin ve kullanıcı risk düzeyi.

    ![Kullanıcı riski](./media/active-directory-identityprotection-playbook/03.png "Playbook")

5. İstenen erişim denetimi denetimleri bölümünde seçin (örneğin parola değişikliği gerekir).
5. Olarak **zorunlu İlkesi**seçin **devre dışı**.
6. Örneğin, test hesaba göre kullanıcı riskini risk olaylardan biri birkaç kez benzetimini yapma Yükselt.
7. Birkaç dakika bekleyin ve ardından, kullanıcı için kullanıcı düzeyinde Orta olduğunu doğrulayın. Aksi durumda, kullanıcı için daha fazla risk olaylarını benzetimi.
8. Olarak **zorunlu İlkesi**seçin **üzerinde**.
9. Bir kullanıcı bir yükseltilmiş risk düzeyi ile kullanarak oturum açma tarafından kullanıcı risk göre koşullu erişim artık test edebilirsiniz.
    
    

### <a name="sign-in-risk-security-policy"></a>Oturum açma riski güvenlik ilkesi

Daha fazla bilgi için bkz: [kullanıcı risk Güvenlik İlkesi](active-directory-identityprotection.md#user-risk-security-policy).

![Oturum açma riski](./media/active-directory-identityprotection-playbook/01.png "Playbook")


**Bir oturum risk ilkesinde test etmek için aşağıdaki adımları gerçekleştirin:**

1. Oturum açma için [ https://portal.azure.com ](https://portal.azure.com) kiracınız için genel yönetici kimlik bilgilerine sahip.

2. Gidin **Azure AD kimlik koruması**.

3. Ana **Azure AD Identity Protection** sayfasında, **oturum açma risk ilkesine**. 

4. İçinde **atamaları** bölümünde, istenen kullanıcıların (ve gruplar) seçin ve oturum açma risk düzeyi.

    ![Oturum açma riski](./media/active-directory-identityprotection-playbook/04.png "Playbook")


5. İçinde **denetimleri** bölümünde, istenen erişim denetimi seçin (örneğin, **çok faktörlü kimlik doğrulaması gerektiren**). 

6. Olarak **zorunlu İlkesi**seçin **üzerinde**.

7. **Kaydet**’e tıklayın.

8. Şimdi oturum açma riski bağlı olarak koşullu erişim riskli oturumu (örneğin, Tor tarayıcı kullanarak) kullanarak oturum açmayı tarafından test edebilirsiniz. 

 




## <a name="see-also"></a>Ayrıca bkz.

- [Azure Active Directory kimlik koruması](active-directory-identityprotection.md)

