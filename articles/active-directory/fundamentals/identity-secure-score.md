---
title: Güvenli kimlik puanı nedir? -Azure Active Directory (genel Önizleme) | Microsoft Docs
description: Kimlik nasıl kullanabileceğiniz hakkında yönergeler, Azure AD kiracınızın güvenlik duruşunu puanı güvenliğini sağlayın.
services: active-directory
keywords: kimlik güvenliği puanı, Azure AD, şirket kaynaklarına güvenli erişim
documentationcenter: ''
author: MarkusVi
manager: daveba
ms.assetid: ''
ms.service: active-directory
ms.subservice: fundamentals
ms.topic: overview
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/19/2018
ms.author: markvi
ms.reviewer: nigu
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8617442c811dc2fe8f961038f35ebcf218a58302
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60247445"
---
# <a name="what-is-the-identity-secure-score-in-azure-active-directory-public-preview"></a>Azure Active Directory'de kimlik güvenli puanı nedir? (Genel Önizleme)

Azure AD kiracınız ne düzeyde güvenli? Bu soruya nasıl yanıt vereceğinizi bilmiyorsanız, kimlik güvenliği puanının kimlik güvenliği duruşunuzu izlemeye ve geliştirmeye nasıl yardımcı olduğunu öğrenmek için bu makaleyi okuyun. 

## <a name="what-is-an-identity-secure-score"></a>Kimlik güvenliği puanı nedir?

Kimlik güvenliği puanı, Microsoft'un güvenlikle ilgili en iyi yöntem önerilerine uyum düzeyinizin göstergesi olarak işlev gören 1 ile 248 arasında bir sayıdır.


![Güvenlik puanı](./media/identity-secure-score/01.png)



Puanın yardımıyla:

- Kimlik güvenliği duruşunuzu nesnel olarak ölçebilirsiniz

- Kimlik güvenliği geliştirmelerini planlayabilirsiniz

- Geliştirmelerinizin başarısını gözden geçirebilirsiniz 


Puana ve ilgili bilgilere kimlik güvenliği puanı panosundan erişebilirsiniz. Bu panoda şunları bulursunuz:

- Puanınız

    ![Güvenlik puanı](./media/identity-secure-score/02.png)

- Karşılaştırma grafiği

    ![Güvenlik puanı](./media/identity-secure-score/03.png)

- Eğilim grafiği

    ![Güvenlik puanı](./media/identity-secure-score/04.png)

- Kimlik güvenliği için en iyi yöntemlerin listesi. 

    ![Güvenlik puanı](./media/identity-secure-score/05.png)


Geliştirme eylemlerini izleyerek:

- Güvenlik duruşunuzu ve puanınızı geliştirebilirsiniz.
 
- Microsoft’un Kimlik özelliklerinden yararlanabilirsiniz. 



## <a name="how-do-i-get-my-secure-score"></a>Güvenlik puanımı nasıl alabilirim?

Kimlik Güvenliği Puanı Azure AD'nin tüm sürümlerinde sağlanır.

Puanınıza erişmek için, [Azure AD Genel Bakış panosuna](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/IdentitySecureScore) gidin.



## <a name="how-does-it-work"></a>Nasıl çalışır?

Her 48 saatte bir Azure güvenlik yapılandırmanıza bakar ve sizin ayarlarınızı önerilen en iyi yöntemlerle karşılaştırır. Bu değerlendirmenin sonucuna dayanarak, kiracınız için yeni bir puan hesaplanır. Başka bir deyişle, yaptığınız yapılandırma değişikliğinin puanınıza yansıması 48 saat kadar sürebilir. 

Her öneri, Azure AD yapılandırmanız temelinde ölçülür. En iyi yöntem önerisini etkinleştirmek için üçüncü taraf ürünler kullanıyorsanız, geliştirme eyleminin ayarlarında bunu belirtebilirsiniz.

![Güvenlik puanı](./media/identity-secure-score/07.png)


Bunun yanı sıra, ortamınıza uygun olmayan önerilerin yoksayılmasını ayarlama seçeneğiniz de vardır. Yoksayılan öneri puanınızın hesaplanmasına katılmaz. 
 
![Güvenlik puanı](./media/identity-secure-score/06.png)



## <a name="how-does-it-help-me"></a>Bana nasıl yardımcı olur?

Güvenlik puanının yardımıyla:

- Kimlik güvenliği duruşunuzu nesnel olarak ölçebilirsiniz

- Kimlik güvenliği geliştirmelerini planlayabilirsiniz

- Geliştirmelerinizin başarısını gözden geçirebilirsiniz



## <a name="what-you-should-know"></a>Bilmeniz gerekenler

### <a name="who-can-use-the-identity-secure-score"></a>Kimlik güvenliği puanını kimler kullanabilir?

Kimlik güvenliği puanı aşağıdaki roller tarafından kullanılabilir:

- Genel yönetici
- Güvenlik yöneticisi 
- Güvenlik okuyucuları 

### <a name="what-does-not-scored-mean"></a>[Puanlanmadı] ne anlama geliyor?

[Puanlanmadı] olarak etiketlenen eylemler, kuruluşunuzda gerçekleştirebileceğiniz ama araca bağlanmadığından (henüz!) puanlanmayacak olan eylemlerdir. Dolayısıyla, yine de güvenliğinizi geliştirebilirsiniz ama şu anda bu eylemler için puan almazsınız.

### <a name="how-often-is-my-score-updated"></a>Puanım ne sıklıkta güncelleştirilir?

Puan günde bir kez hesaplanır (yaklaşık 24:00 PST). Ölçülen eylemde değişiklik yaparsanız, puan ertesi gün otomatik olarak güncelleştirilir. Değişikliğin puanınıza yansıması 48 saat kadar sürebilir.


### <a name="my-score-changed-how-do-i-figure-out-why"></a>Puanım değişti. Nedenini nasıl anlayabilirim?

[Güvenlik puanı portalının](https://securescore.microsoft.com/#!/score) puan analizi sayfasında, belirli bir günün bir veri noktasına tıklayın, sonra neyin değiştiğini bulmak için sayfayı kaydırarak o günün tamamlanmış ve tamamlanmamış eylemlerine bakın.

### <a name="does-the-secure-score-measure-my-risk-of-getting-breached"></a>Güvenlik Puanı güvenlik ihlali riskimi ölçüyor mu?

Kısacası, hayır. Güvenlik Puanı güvenlik ihlaliyle karşılaşma olasılığınız için mutlak bir ölçü ortaya koymaz. Güvenlik ihlaline uğrama riskini azaltabilecek özellikleri ne düzeyde benimsediğinizi ortaya koyar. Hiçbir hizmet güvenlik ihlaline uğramayacağınızı garanti edemez ve Güvenlik Puanı herhangi bir şekilde garanti olarak yorumlanmamalıdır.

### <a name="how-should-i-interpret-my-score"></a>Puanımı nasıl yorumlayabilirim?

Önerilen güvenlik özelliklerini yapılandırmanızdan veya güvenlikle ilgili görevleri (raporları okuma gibi) gerçekleştirmenizden dolayı size puan verilir. Kullanıcılarınız için Multi-Factor Authentication'ı (MFA) etkinleştirmeniz gibi bazı eylemler, kısmen tamamlanmış olarak puanlanır. Güvenlik Puanınız, kullandığınız Microsoft güvenlik hizmetlerinin doğrudan göstergesidir. Güvenliğin her zaman kullanılabilirlikle dengelenmesi gerektiğini unutmayın. Tüm güvenlik denetimlerinin bir de kullanıcı etkisi bileşeni vardır. Düşük kullanıcı etkisine sahip denetimlerin, kullanıcılarınızın gündelik işlemleri üzerinde çok az etkisi olması veya hiç olmaması gerekir.

Puan geçmişinizi görmek için, [güvenlik puanı portalında](https://securescore.microsoft.com/#!/score) puan analizi sayfasına gidin. Belirli bir tarih seçerek o gün hangi denetimlerin etkinleştirildiğini ve her birinden kaç puan kazandığınızı görün.


### <a name="how-does-the-identity-secure-score-relate-to-the-office-365-secure-score"></a>Kimlik güvenliği puanı ile Office 365 güvenlik puanı arasında nasıl bir ilişki vardır? 

[Office 365 güvenlik puanı](https://docs.microsoft.com/office365/securitycompliance/office-365-secure-score), yakında beş farklı puanın toplamına geçirilecektir:

- Kimlik

- Veriler

- Cihazlar

- Altyapı

- Uygulamalar

Kimlik güvenliği puanı, Office 365 güvenlik puanının kimlik bölümünü temsil eder. Başka bir deyişle, kimlik güvenliği puanı ve Office 365'teki kimlik puanı için aynı önerileri alırsınız. 


## <a name="next-steps"></a>Sonraki adımlar

Office 365 güvenlik puanıyla ilgili bir video izlemek isterseniz, [buraya](https://www.youtube.com/watch?v=jzfpDJ9Kg-A) tıklayın.
 
