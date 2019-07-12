---
title: Azure Active Directory kimlik koruması nedir (yenilenebilecek)? | Microsoft Docs
description: Azure Active Directory kimlik koruması nedir (yenilenebilecek)?
services: active-directory
keywords: Azure active directory kimlik koruması, bulut uygulaması bulma, yönetme, uygulamaları, güvenlik, risk, risk düzeyi, güvenlik açığı, güvenlik ilkesi
documentationcenter: ''
author: MicrosoftGuyJFlo
manager: mtillman
ms.assetid: e7434eeb-4e98-4b6b-a895-b5598a6cccf1
ms.service: active-directory
ms.subservice: identity-protection
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/03/2018
ms.author: joflore
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0f6c2f36e1061243851b37da47659aaf7a18e8d6
ms.sourcegitcommit: 2e4b99023ecaf2ea3d6d3604da068d04682a8c2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67673018"
---
# <a name="what-is-azure-active-directory-identity-protection-refreshed"></a>Azure Active Directory kimlik koruması nedir (yenilenebilecek)?

Kimlik koruması deneyimi daha iyi, kuruluşunuzun kimliklerini korumanıza yenilendi. Bu yenilenmiş deneyim sağlar:

- You¬ kullanıcı riski ve oturum açma riski en uygun risk etrafında döner yeniden tasarlanan yönetici deneyimi

- Güçlü araştırmalar filtreleme, sıralama ve akıllı yüklemeleri için destek deneyimini

- Kullanıcı risk hesaplama tehlikeye olasılığı en yüksek olan kullanıcıları doğrultusunda çabalarına öncelik yardımcı olması için geliştirildi

- Yeni API desteği risk verilerini için programlı erişim etkinleştirmek için

- Hemen kullanıcılarınızın korunmasına olanak tanıyan Basitleştirilmiş Yönetim geri bildirim süreci

- Yeni denetimli makine öğrenimi risk değerlendirmeleri doğruluğunu artırmak için



Güvenlik, kuruluşlar için güvenliğin çok önemli bugün sağlıyor. Güvenlik ihlallerinin ele çoğunu, saldırganların bir kullanıcının kimliğini çalarak bir ortama erişimi elde etmek zaman yerleştirin. Yıllar içinde saldırganlar kullanarak gelişmiş kimlik avı saldırıları ve üçüncü taraf ihlallerini yararlanarak, giderek daha etkili hale gelmiştir. Saldırganlar düşük ayrıcalıklı kullanıcı hesaplarına bile erişim hemen sonra bunları yanal hareket önemli şirket kaynaklarına erişim kazanmak oldukça kolay bir işlemdir. 

Bu tehditlere yanıt vermek için Azure AD kimlik koruması sağlar: 

- Proaktif olarak güvenliği aşılmış kimlik kötüye gelen engelle 

- Şüpheli etkinlik algılandığında otomatik olarak riski azaltın 

- Riskli kullanıcılar ve oturum açma adresi olası güvenlik açıkları için inceleyin  

- Bir kullanıcının risk belirtilen bir eşiğe ulaştığında uyarı 

 

Azure AD kimlik koruması, Azure Active Directory Premium, bir kullanıcının kimlik güvenliği aşıldığında otomatik olarak yanıt verecek şekilde ilkelerini yapılandırmanıza olanak sağlayan P2 özelliği veya birisi dışında olduğunda hesap sahibi kullanarak oturum çalışılıyor, kimlik. Azure AD tarafından sağlanan diğer koşullu erişim denetimleri ek olarak bu ilkeleri ya da otomatik olarak erişim ya da parola sıfırlama veya çok faktörlü kimlik doğrulaması zorlama gibi başlatma risk azaltma eylemleri engelleyebilirsiniz. Ayrıca, kimlik koruması, risk ve olası güvenlik ihlalleri, kuruluşunuzda daha derin bir anlayış kazanmak için izleme ve raporlama yetenekleri sağlar. 

>[!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RWsS6Q]


## <a name="risk-events"></a>Risk olayları

Azure AD kimlik koruması, aşağıdaki risk olaylarını algılar: 

 

| Risk olayı türü | Açıklama | Algılama türü |
| ---             | ---         | ---            |
| Alışılmadık seyahat | Kullanıcının son oturum açma işlemleri üzerinde temel alışılmadık bir konumdan oturum açın. | Çevrimdışı |
| Anonim IP adresi | Anonim bir IP adresinden oturum açın (örneğin: Tor tarayıcısı, anonymizer VPN'ler). | Gerçek zamanlı |
| Alışılmadık oturum açma özellikleri | Oturum özellikleri değil gördük son belirtilen kullanıcı için oturum açın. | Gerçek zamanlı |
| Kötü amaçlı yazılım bağlı IP adresi | Bir bağlı kötü amaçlı IP adresinden oturum açın | Çevrimdışı |
| Kimlik bilgilerinin sızdırılması | Bu risk olayı kullanıcının geçerli kimlik bilgilerinin sızdırılması gösterir. | Çevrimdışı |





## <a name="types-of-risk"></a>Risk türü 

Kimlik koruması, risk iki türlerine dayanır:

- Oturum açma riski

- Kullanıcı riski

### <a name="sign-in-risk"></a>Oturum açma riski

Oturum açma riski bir belirli kimlik doğrulama isteği kimlik sahibinin yetkilendirdiği değil olasılık temsil eder.

İki değerlendirmeleri, oturum açma riski vardır: 

- **Oturum açma riski (gerçek zamanlı)** -oturum açma riski (gerçek zamanlı) oturum açma işlemi sırasında tetikleyen tüm gerçek zamanlı algılama temel alır.  

- **Oturum açma riski (toplama)** -oturum açma riski (toplam) bir oturum açma, toplam riskidir. Bir machine learning göz önünde bulundurur modeli tarafından hesaplanır:

    - Gerçek zamanlı algılama (yukarıda açıklanmıştır)
    
    - (Oturum açma gerçekleştikten sonra harekete) çevrimdışı algılamalar 
    
    - Oturum açmanın diğer tüm özellikleri


### <a name="user-risk"></a>Kullanıcı riski

Kullanıcı riski, belirli bir kimlik güvenliği ihlal olasılığını temsil eder. 

Kullanıcı riski kullanıcıyla ilişkili tüm riskleri göz önünde bulundurularak hesaplanır:

- Tüm riskli oturum açma işlemleri
- Bir oturum açma için bağlı olmayan tüm risk olayları
- Geçerli kullanıcı riski
- Tüm risk düzeltme veya işten çıkarma kullanıcı tarih kasa üzerinde gerçekleştirilen eylemler



## <a name="how-identity-protection-detects-risk"></a>Kimlik koruması, risk nasıl algılar?  

Azure AD, anormallikleri ve şüpheli etkinlikleri algılamak için makine öğrenimini kullanıyor, algılandı. her iki sinyalleri kullanarak gerçek zamanlı sırasında oturum açma işlemleri de gerçek olmayan süresi sinyalleri gibi kullanıcı ve oturum açma etkinlikleriyle ilgili. Bu verileri kullanarak, kimlik koruması, her kullanıcı için genel bir kullanıcı risk düzeyi belirleyen yanı sıra bir kullanıcının kimlik her zaman gerçek zamanlı bir oturum açma riski hesaplar. Kimlik koruması otomatik olarak bu risk algılamalar üzerinde yapılandırma kimlik koruması kullanıcı riski ve oturum açma riski ilkeleri tarafından harekete geçmenizi sağlar.  

 

Kimlik Koruması'nın risk nasıl algıladığı anlamak için iki önemli kavram vardır: kullanıcı riski ve oturum açma riski. Oturum açma riski bir belirli kimlik doğrulama isteği kimlik sahibinin yetkilendirdiği değil olasılık yansıtır. Oturum açma risk iki tür vardır: Gerçek zamanlı ve toplam. Belirtilen oturum açma denemesinin (örneğin, anonim IP adreslerinden oturum açma işlemleri) zaman gerçek zamanlı riskli oturum açma algılandı. Toplam oturum açma riski olduğu algılanan gerçek zamanlı oturum toplamını risklerini de kullanıcı ile ilişkili herhangi bir sonraki gerçek zamanlı risk olayları oturum açmalar (mümkün olmayan seyahat gibi). Kullanıcı riski kötü bir aktör belirli bir kimlik güvenliği ihlal genel olasılığını yansıtır. Kullanıcı riski içeren belirli bir kullanıcı için tüm risk etkinlikler dahil olmak üzere:

- Gerçek zamanlı oturum açma riski
- Sonraki oturum açma riski
- Riskli kullanıcı algılamalar.   

 

 
 ![Akış](./media/overview-v2/01.png)
 

 

Kimlik koruması, risk algılama için temel akış ve yanıt herhangi belirli oturum açma için yukarıdaki grafikte özetlenir.  

 

 

 

## <a name="common-scenarios"></a>Genel senaryolar 

Bir Contoso çalışanı örneğe göz atalım. 

1. Exchange Online'a Tor tarayıcıdan oturum açmak bir çalışan çalışır. Oturum açma zaman Azure AD, gerçek zamanlı risk olaylarını algılar. 

2. Azure AD, çalışan bir anonim IP adresinden orta düzeyde riskli oturum açma düzeyini tetikleme oturum açmak için olduğunu algılar. 

3. Contoso'nun BT Yöneticisi oturum açma kimlik Koruması riski koşullu erişim ilkesi yapılandırıldığı için çalışan bir MFA istemi tarafından sınandı. İlkeyi bir oturum açma riskini Orta büyüklükte ya da daha yüksek için mfa'yı gerektirir. 

4. Çalışan MFA istemi geçirir ve Exchange Online erişir ve kendi kullanıcı risk düzeyi değiştirilmez. 

Arka planda ne oldu? Tor tarayıcıdan oturum açma denemesi gerçek zamanlı bir oturum açma riski anonim IP adresi için Azure AD'de tetiklenir. Azure AD isteği işlendi olarak, kimlik koruması çalışanın oturum açma riski düzeyi (Orta) eşiğine çünkü yapılandırılmış oturum açma riski ilkesi uygulanır. Çalışan, daha önce MFA için kaydolduğunu olduğundan, yanıt ve MFA testini geçirin. MFA testini başarıyla geçirmek için kendi yeteneği, büyük olasılıkla meşru kimlik sahibi oldukları ve kendi kullanıcı risk düzeyi artmıyor Azure AD'ye sinyal. 


Ancak bir oturum açmaya çalışan ne edilmedi? 

1. Kendi IP adresini gizlemek çalışıyor olduğundan, Exchange Online hesabında Tor tarayıcıdan oturum açmak kötü amaçlı bir aktör çalışanın kimlik bilgileriyle çalışır. 

2. Azure AD oturum açma denemesi'nın anonim bir IP adresinden, gerçek zamanlı bir oturum açma riski tetikleme algılar. 

3. Kötü amaçlı aktör, Contoso'nun BT Yöneticisi oturum açma riski Orta büyüklükte ya da daha yüksek olduğunda MFA gerektirecek şekilde kimlik koruma oturum açma riski koşullu erişim ilkesinin yapılandırıldığı bir MFA istemi tarafından sınandı. 

4. Kötü amaçlı aktör MFA testini başarısız olur ve Exchange Online hesabı çalışanın erişemez. 

5. Başarısız MFA istemi gelecekteki oturum açma işlemleri için kendi kullanıcı riski yükseltme kaydedilmesi için risk olayı tetiklendi. 

Kötü amaçlı bir aktör Sarah'ın hesabı erişmeyi denedi, çalışan oturum açmak için çalıştığında ne olacağını görelim. 

1. Outlook'tan Exchange Online'da oturum açmak çalışan çalışır. Oturum açma zaman Azure AD, herhangi bir önceki kullanıcı risk yanı sıra gerçek zamanlı risk olaylarını algılar. 

2. Azure AD, gerçek zamanlı bir oturum açma riski algılamıyor, ancak önceki senaryolarda yüksek kullanıcı riski nedeniyle son riskli etkinlik algılar.  

3. Çalışan, çünkü bir parola sıfırlama istemi tarafından sınandı Contoso BT yöneticisinin kimlik koruması kullanıcı riski ilkesi yüksek riskli bir kullanıcıyla oturum açtığında parola değişikliği gerektirecek şekilde yapılandırılmış. 

4. SSPR ve MFA için çalışan kayıtlı olduğundan, bunlar başarıyla sıfırlar parolalarını. 

5. Tarafından parola sıfırlama, çalışanın kimlik bilgileri artık tehlikede olur ve güvenli bir duruma kimliklerini döndürür. 

6. Çalışanın önceki risk olaylarını daha çözümlenir ve kendi kullanıcı risk düzeyi kimlik bilgilerinin tehlikeye atılması Azaltıcı yanıt olarak otomatik olarak sıfırlanır. 

## <a name="how-do-i-configure-identity-protection"></a>Kimlik koruması nasıl yapılandırılır? 

Kimlik koruması ile çalışmaya başlamak için öncelikle bir kullanıcı riski İlkesi ve bir oturum açma riski ilkesi yapılandırın. Bu ilkeleri yapılandırıldığında ve bir test grubuna uygulanan sonra kimlik koruması, ortamınızda nasıl yanıt vereceğini anlamak için risk olaylarının benzetimini yapabilirsiniz. Hızlı Başlangıç kılavuzları yukarıda sözü edilen ilkeleri ayarlayın ve ortamınızda test etmek nasıl bir kılavuz sağlar. 

 

Kimlik koruması, dağıtımınızı etrafında yönetimi etkinlikleri dengelemek için Azure AD'de 3 rollerini destekler: 

| Role | Yapabilirsiniz | Bunu yapamazsınız |
| --- | --- | --- |
| Genel yönetici | Kimlik koruması, yerleşik kimlik koruması tam erişim | |
| Güvenlik yöneticisi | Kimlik koruması tam erişim | Yerleşik kimlik koruması, kullanıcı parolalarını sıfırlama |
| Güvenlik okuyucusu | Kimlik koruması salt okunur erişim | Yerleşik kimlik koruması, kullanıcıları düzeltme, ilkelerini yapılandırma, parolaları sıfırlama| 

Daha fazla ayrıntı için [Azure Active Directory'de yönetici rolleri atama](../users-groups-roles/directory-assign-admin-roles.md)

 
## <a name="licensing"></a>Lisanslama

>[!NOTE]
> Kimlik koruması (yenilenmiş) genel Önizleme sırasında yalnızca Azure AD Premium P2 müşterileri riskli kullanıcılar raporu ve riskli oturum açma işlemleri raporu erişebilir.



| Özellik | Azure AD Premium P2 | Azure AD Premium P1 | Azure AD Basic/ücretsiz |
| --- | --- | --- | --- |
| Kullanıcı riski İlkesi | Evet | Hayır | Hayır |
| Oturum açma riski İlkesi | Evet | Hayır | Hayır |
| Riskli kullanıcılar raporu | Tam erişim | Sınırlı bilgiler | Sınırlı bilgiler |
| Riskli oturum açma işlemleri raporu | Tam erişim | Sınırlı bilgiler | Sınırlı bilgiler |
| MFA kayıt ilkesi | Evet | Hayır | Hayır |







## <a name="next-steps"></a>Sonraki adımlar 

Kimlik koruması ile çalışmaya başlamak için bkz. [yapılandırma oturum açma riski İlkesi](quickstart-sign-in-risk-policy.md). 






