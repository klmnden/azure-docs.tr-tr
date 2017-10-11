---
title: "Azure Active Directory kimlik koruması Kılavuzu | Microsoft Docs"
description: "Azure AD kimlik koruması nasıl yeteneğini bir saldırgan güvenliği aşılmış kimlik veya aygıt yararlanmaya ve güvenli bir kimlik veya önceden şüpheli veya tehlikeye bilinen bir cihaz için sınırlamak sağladığını öğrenin."
services: active-directory
keywords: "Azure active directory kimlik koruması, cloud app discovery'yi, uygulamalar, güvenlik, risk, risk düzeyi, güvenlik açığı, güvenlik ilkesi yönetme"
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: 60836abf-f0e9-459d-b344-8e06b8341d25
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 2ecd07faed785fa6aa179ac1cca35a70d965e1dc
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2017
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
Bu riski olay türü açan başarıyla anonim Ara sunucu IP adresi olarak tanımlanan bir IP adresinden kullanıcıları tanımlar. Bu proxy'leri kendi aygıtın IP adresi gizlemek istediğiniz ve kötü amaçlı için kullanılabilir kişiler tarafından kullanılır.

**Bir oturum açma anonim bir IP adresinden benzetimini yapmak için aşağıdaki adımları gerçekleştirin**:

1. Karşıdan [Tor tarayıcı](https://www.torproject.org/projects/torbrowser.html.en).
2. Tor tarayıcı kullanarak gidin [https://myapps.microsoft.com](https://myapps.microsoft.com).   
3. Görünmesini istediğiniz hesap kimlik bilgilerini girin **anonim IP adreslerinden gerçekleştirilen oturum açma işlemleri** rapor.

Oturum açma kimlik koruması Panoda 5 dakika içinde görünecektir. 

### <a name="sign-ins-from-unfamiliar-locations"></a>Alışılmadık konumlardan oturum açma işlemleri
Oturum açma konumları göz önünde bulundurur bir oturum açma gerçek zamanlı değerlendirme mekanizması tanınmayan konumlardan riskidir (IP, enlem / boylam ve ASN) yeni / tanınmayan konumlarını belirlemek için. Sistem önceki IP'leri, enlem depolar / boylam ve bir kullanıcının Asn'ler ve tanıdık konumlarının olması için bu göz önünde bulundurur. Oturum açma konumu herhangi bir varolan tanıdık konumda eşleşmiyorsa bir oturum açma konumu tanınmayan olarak kabul edilir.

Azure Active Directory kimlik koruması:  

* 14 gün boyunca, tüm yeni konumlar tanınmayan konumları olarak işaretlemez bir ilk öğrenme süre sahiptir.
* oturum açma işlemleri hakkında bilgi sahibi aygıtları ve coğrafi olarak yakın varolan bir bilinen konum olan konumları yok sayar.

Tanınmayan konumlardan benzetimini yapmak için bir konum ve hesap öğesinden önce oturum açtığı olmayan bir aygıttan oturum açmak zorunda. 

**Bir oturum açma tanınmayan bir konumdan benzetimini yapmak için aşağıdaki adımları gerçekleştirin**:

1. Oturum açma en az bir 14 gün geçmiş olan bir hesap seçin. 
2. Ya da yapın:
   
   a. Bir VPN kullanırken gidin [https://myapps.microsoft.com](https://myapps.microsoft.com) ve risk olayı için benzetimini yapmak istediğiniz hesabın kimlik bilgilerini girin.
   
   b. Bir ilişkilendirme (önerilmez) hesabın kimlik bilgilerini kullanarak oturum farklı bir konumda isteyin.

Oturum açma kimlik koruması Panoda 5 dakika içinde görünecektir.

### <a name="impossible-travel-to-atypical-location"></a>Alışılmadık konumu imkansız seyahat
Makine yanlış pozitifler tanıdık aygıtlardan mümkün olmayan seyahat veya oturum açmalardan dizindeki diğer kullanıcılar tarafından kullanılan VPN'ler gibi çıkışı ortadan kaldırmak öğrenme algoritmasını kullandığı için mümkün olmayan seyahat koşul benzetimi zor olabilir. Ayrıca, risk olaylarını oluşturma işlemi başlamadan önce kullanıcı için bir oturum açma geçmişine 3-14 gün algoritma gerektirir.

**Bir mümkün olmayan seyahat alışılmadık konuma benzetimini yapmak için aşağıdaki adımları gerçekleştirin**:

1. Standart, tarayıcı kullanarak gidin [https://myapps.microsoft.com](https://myapps.microsoft.com).  
2. İçin bir mümkün olmayan seyahat risk olay oluşturmak için kullanmak istediğiniz hesabın kimlik bilgilerini girin.
3. Kullanıcı Aracısı değiştirin. Geliştirici Araçları'ndan kullanıcı aracısı Internet Explorer'da değiştirmek veya kullanıcı aracınız Firefox veya kullanıcı aracısı değiştirici Eklentisi'ni kullanarak Chrome değiştirin.
4. IP adresini değiştirin. VPN, Tor eklentisi kullanılarak veya farklı bir veri merkezinde Azure içinde yeni bir makine dönmesini IP adresiniz değiştirebilirsiniz.
5. Oturum açma için [https://myapps.microsoft.com](https://myapps.microsoft.com) olarak önce ve sonra önceki oturum açma birkaç dakika içinde aynı kimlik bilgilerini kullanarak.

Oturum açma kimlik koruması panosunda 2-4 saat içinde görünecektir.<br>
Modelleri ilgili karmaşık machine learning nedeniyle, bu toplanma değil bir fırsat yok.<br> Birden çok Azure AD hesapları için bu adımları çoğaltmak istediğiniz.

## <a name="simulating-vulnerabilities"></a>Güvenlik açıkları benzetimini yapma
Güvenlik açıkları tarafından hatalı aktör yararlanan bir Azure AD ortamda zayıf giderilmiştir. Şu anda açık 3 türlerinin diğer özelliklerden Azure ad içinde Azure AD Identity Protection çıkmış. Bu özellikler ayarlandıktan sonra bu güvenlik açıklarından otomatik olarak kimlik koruması panosunda görüntülenir.

* Azure AD [çok faktörlü kimlik doğrulaması?](../multi-factor-authentication/multi-factor-authentication.md)
* Azure AD [Cloud App Discovery](active-directory-cloudappdiscovery-whatis.md).
* Azure AD [Privileged Identity Management](active-directory-privileged-identity-management-configure.md). 

## <a name="user-compromise-risk"></a>Kullanıcı güvenlik aşılması riski
**Kullanıcı güvenlik aşılması risk sınamak için aşağıdaki adımları gerçekleştirin**:

1. Oturum açma için [https://portal.azure.com](https://portal.azure.com) kiracınız için genel yönetici kimlik bilgilerine sahip.
2. Gidin **kimlik koruması**. 
3. Ana **Azure AD Identity Protection** dikey penceresinde tıklatın **ayarları**. 
4. Üzerinde **Portalı Ayarları** dikey altında **güvenlik kuralları**, tıklatın **kullanıcı güvenliğinin aşılmasına riski**. 
5. Üzerinde **Risk oturum** dikey penceresinde Aç **etkinleştir kural** kapalı ve ardından **kaydetmek** ayarlar.
6. Belirtilen kullanıcı hesabı için bir tanınmayan benzetimini konumlar veya anonim IP risk olayı. Bu, kullanıcı için kullanıcı risk düzeyini Yükselt **orta**.
7. Birkaç dakika bekleyin ve kullanıcı için bu kullanıcı düzeyini doğrulayın **orta**.
8. Git **Portalı Ayarları** dikey.
9. Üzerinde **kullanıcı güvenliğinin aşılmasına Risk** dikey altında **etkinleştir kural**seçin **üzerinde** . 
10. Aşağıdaki seçeneklerden birini seçin:
    
    a. Bloğuna, select **orta** altında **blok oturum**.
    
    b. Güvenli parola değişikliğini uygulamak için seçin **orta** altında **çok faktörlü kimlik doğrulaması gerektiren**.
11. **Kaydet** düğmesine tıklayın.
12. Bir kullanıcı bir yükseltilmiş risk düzeyi ile kullanarak oturum açma tarafından risk bağlı olarak koşullu erişim artık test edebilirsiniz. Kullanıcı risk Orta ise ya da engellenecek ilkeniz yapılandırmasına bağlı olarak, oturum açma işleminiz olduğundan veya parolanızı değiştirmek için zorlanır. 
    <br><br>
    ![Playbook](./media/active-directory-identityprotection-playbook/201.png "Playbook")
    <br>

## <a name="sign-in-risk"></a>Oturum açma riski
**Bir oturum risk test etmek için aşağıdaki adımları gerçekleştirin:**

1. Oturum açma için [https://portal.azure.com ](https://portal.azure.com) kiracınız için genel yönetici kimlik bilgilerine sahip.
2. Gidin **kimlik koruması**.
3. Ana **Azure AD Identity Protection** dikey penceresinde tıklatın **ayarları**. 
4. Üzerinde **Portalı Ayarları** dikey altında **güvenlik kuralları**, tıklatın **risk oturum**.
5. Üzerinde ** Risk oturum ** dikey penceresinde, select **üzerinde** altında **etkinleştir kural**. 
6. Aşağıdaki seçeneklerden birini seçin:
   
   a. Engellemek için seçin **orta** altında **blok oturum açın**
   
   b. Güvenli parola değişikliğini uygulamak için seçin **orta** altında **çok faktörlü kimlik doğrulaması gerektiren**.
7. Engellemek için blok oturum açma altında Orta seçin.
8. Çok faktörlü kimlik doğrulamasını zorunlu kılmak için seçin **orta** altında **çok faktörlü kimlik doğrulaması gerektiren**.
9. **Kaydet**'e tıklayın.
10. Tanınmayan konumlardan taklit ederek risk bağlı olarak koşullu erişim artık sınayabilirsiniz veya her ikisi de olduklarından anonim IP risk olayları **orta** risk olayları.


![Playbook](./media/active-directory-identityprotection-playbook/200.png "Playbook")


## <a name="see-also"></a>Ayrıca bkz.
* [Azure Active Directory kimlik koruması](active-directory-identityprotection.md)

