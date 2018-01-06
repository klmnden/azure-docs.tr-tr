---
title: "Azure Active Directory risk olaylarını | Microsoft Docs"
description: "Bu konu, risk olaylarını nelerdir ayrıntılı genel bakış sağlar."
services: active-directory
keywords: "Azure active directory kimlik koruması, güvenlik, risk, risk düzeyi, güvenlik açığı, güvenlik ilkesi"
author: MarkusVi
manager: mtillman
ms.assetid: fa2c8b51-d43d-4349-8308-97e87665400b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/07/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 59c8932f7676a5388413baf2edb5d9e259769f93
ms.sourcegitcommit: 0e1c4b925c778de4924c4985504a1791b8330c71
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/06/2018
---
# <a name="azure-active-directory-risk-events"></a>Azure Active Directory risk olayları

Güvenlik ihlallerini çoğunluğu saldırganlar bir ortamda bir kullanıcının kimliğini çalarak erişmek zaman yer ayırın. Güvenliği aşılmış kimlikleri keşfetme hiçbir kolay bir görevdir. Azure Active Directory kullanıcı hesaplarınızı ilgili kuşkulu eylemleri algılamak için Uyarlamalı machine learning algoritmaları ve buluşsal yöntemler kullanır. Her kuşkulu eylem olarak adlandırılan bir kayıtta depolanan algılanan *risk olayı*.

Şu anda, Azure Active Directory altı tür risk olaylarını algılar:

- [Sızan kimlik bilgilerine sahip kullanıcılar](#leaked-credentials) 
- [Anonim IP adreslerinden gerçekleştirilen oturum açma işlemleri](#sign-ins-from-anonymous-ip-addresses) 
- [Alışılmadık konumlara imkansız seyahat](#impossible-travel-to-atypical-locations) 
- [Virüs bulaşmış cihazlardan gerçekleştirilen oturum açma işlemleri](#sign-ins-from-infected-devices) 
- [Şüpheli etkinlik gösteren IP adreslerinden gerçekleştirilen oturum açma işlemleri](#sign-ins-from-ip-addresses-with-suspicious-activity) 
- [Fazla tanınmayan konumlardan gerçekleştirilen oturum açma işlemleri](#sign-in-from-unfamiliar-locations) 


![Risk olayı](./media/active-directory-reporting-risk-events/91.png)

Algılanan risk olayı için alma Insight Azure AD aboneliğinizi bağlıdır. Azure AD Premium P2 Edition'la tüm temel algılamaların hakkında en ayrıntılı bilgi alın. Azure AD Premium P1 Edition'la lisansınız tarafından kapsanmayan algılamaların risk olayı olarak görünür **algılanan ek risk ile oturum açma**.


Bu konuda, hangi risk olaylarını ayrıntılı bir genel bakış olduğunuz ve Azure AD kimliklerinizi korumak için bunları nasıl kullanabileceğiniz sağlar.


## <a name="risk-event-types"></a>Risk olayı türleri

Risk olay türü şüpheli eylem risk olay kaydı için tanımlayıcı için oluşturulan bir özelliktir.  
Microsoft'un sürekli Yatırımlar algılama işlemine neden:

- Varolan risk olayların algılama doğruluğu geliştirmeleri 
- Gelecekte eklenecek yeni risk olayı türleri

### <a name="leaked-credentials"></a>Sızan kimlik bilgileri

Normal kullanıcıların geçerli parolalarını cybercriminals tehlikeye zaman suçlular genellikle bu kimlik bilgilerini paylaşır. Bu genellikle, bunları ortak olarak koyu web ya da Yapıştır sitelerde veya ticaret veya kimlik bilgilerini siyah pazara satış göndererek da gerçekleştirilir. Microsoft kimlik bilgilerini sızmasını hizmet edinir kullanıcı adı / parola çiftleri ortak açık ve koyu renkli web siteleri izleme ve çalışma:

- Araştırmacılar
- Yasa uygulama
- Microsoft Güvenlik ekiplerden
- Güvenilir diğer kaynaklar 

Ne zaman hizmeti edinir kullanıcı adı / parola çiftleri, bunlar denetlenir karşı AAD kullanıcıların geçerli geçerli kimlik bilgileri. Bir eşleşme bulunduğunda, bir kullanıcının parolasını aşıldığını gelir ve bir *kimlik bilgileri riskini olay sızmasını* oluşturulur.

### <a name="sign-ins-from-anonymous-ip-addresses"></a>Anonim IP adreslerinden oturum açma işlemleri

Bu riski olay türü açan başarıyla anonim Ara sunucu IP adresi olarak tanımlanan bir IP adresinden kullanıcıları tanımlar. Bu proxy'leri kendi aygıtın IP adresi gizlemek istediğiniz ve kötü amaçlı için kullanılabilir kişiler tarafından kullanılır.


### <a name="impossible-travel-to-atypical-locations"></a>Alışılmadık konumlara imkansız seyahat

Bu riski olay türü burada konumları en az biri de kullanıcı için alışılmadık olabilir, coğrafi olarak birbirinden uzak konumlardan davranış davranışlarına iki oturum açma işlemleri tanımlar. Diğer çeşitli etkenler arasında dikkate bu makine öğrenme algoritmasını, iki oturum açma işlemleri ve bunu kullanıcının ilk konumdan farklı bir kullanıcı aynı kullandığını gösteren ikinci, seyahat alacağı saat arasındaki zaman alır. kimlik bilgileri.

Algoritma "VPN'ler ve kuruluşunuzdaki diğer kullanıcılar tarafından düzenli olarak kullanılan konumlar gibi mümkün olmayan seyahat koşullar katkıda bulunan belirgin yalancı pozitifler" yoksayar. Sistem bir ilk öğrenme süre 14 gün boyunca yeni kullanıcının oturum açma davranışı öğrenir sahip. 

### <a name="sign-in-from-unfamiliar-locations"></a>Tanınmayan konumlardan oturum aç

Bu riski olay türü oturum açma konumları göz önünde bulundurur (IP, enlem / boylam ve ASN) yeni / tanınmayan konumlarını belirlemek için. Sistem, bir kullanıcı tarafından kullanılan önceki konumları hakkında bilgi depolar ve bu "bilinen" konumları göz önünde bulundurur. Tanıdık konumları listesinde olmayan bir konumdan oturum açma ortaya çıktığında risk olay tetiklenir. Sistem bir ilk öğrenme süre boyunca, tüm yeni konumlar tanınmayan konumları olarak işaretlemez 30 gün sahip. Sistem, ayrıca oturum açma işlemleri hakkında bilgi sahibi aygıtları ve coğrafi olarak yakın tanıdık bir konum olan konumları yoksayar. 

### <a name="sign-ins-from-infected-devices"></a>Bulaşma olan cihazlardan oturum açma işlemleri

Bu riski olay türü etkin bir şekilde bir bot sunucusu ile iletişim kurmak için bilinen kötü amaçlı yazılım, virüs bulaşmış cihazlardan gerçekleştirilen oturum açma işlemleri tanımlar. Bu, kullanıcının aygıtına bir bot sunucusu ile iletişim kurmuş olan IP adresleri karşı IP adresleri ile ilişkilendirilmesi yoluyla belirlenir. 

### <a name="sign-ins-from-ip-addresses-with-suspicious-activity"></a>Şüpheli etkinlik gösteren IP adreslerinden gerçekleştirilen oturum açma işlemleri
Bu riski olay türü, IP adresleri, çok sayıda başarısız oturum açma denemeleri, birden çok kullanıcı hesapları arasında kısa bir süre boyunca karşılaşılan tanımlar. Bu trafik düzenlerini saldırganlar tarafından kullanılan IP adreslerinin eşleşir ve hesapları ya da zaten olan veya hakkında tehlikeye olan güçlü bir gösterge olur. Belirgin yoksayar makine öğrenme algoritmasını budur "*yanlış pozitifler*", IP adresleri gibi düzenli olarak kuruluştaki diğer kullanıcılar tarafından kullanılır.  Sistem, burada yeni bir kullanıcı ve yeni Kiracı oturum davranışını öğrenir bir ilk öğrenme süre 14 gün sahip.


## <a name="detection-type"></a>Algılama türü

Algılama tür özelliği bir göstergesidir (gerçek zamanlı veya çevrimdışı) risk olay algılama zaman dilimi için.  
Şu anda, risk olay gerçekleştikten sonra çoğu risk olaylarını çevrimdışı bir işlem sonrası işlemi algılandı.

Aşağıdaki tabloda ilgili bir raporda gösterilmeye algılama türü için geçen süreyi listelenmektedir:

| Algılama türü | Raporlama gecikme süresi |
| --- | --- |
| Gerçek zamanlı | 5-10 dakika |
| Çevrimdışı | 2-4 saat |


Azure Active Directory algılanan risk olayı türleri için algılama türleri şunlardır:

| Risk olay türü | Algılama türü |
| :-- | --- | 
| [Sızan kimlik bilgilerine sahip kullanıcılar](#leaked-credentials) | Çevrimdışı |
| [Anonim IP adreslerinden gerçekleştirilen oturum açma işlemleri](#sign-ins-from-anonymous-ip-addresses) | Gerçek zamanlı |
| [Alışılmadık konumlara imkansız seyahat](#impossible-travel-to-atypical-locations) | Çevrimdışı |
| [Fazla tanınmayan konumlardan gerçekleştirilen oturum açma işlemleri](#sign-in-from-unfamiliar-locations) | Gerçek zamanlı |
| [Virüs bulaşmış cihazlardan gerçekleştirilen oturum açma işlemleri](#sign-ins-from-infected-devices) | Çevrimdışı |
| [Şüpheli etkinlik gösteren IP adreslerinden gerçekleştirilen oturum açma işlemleri](#sign-ins-from-ip-addresses-with-suspicious-activity) | Çevrimdışı|


## <a name="risk-level"></a>Risk düzeyi

Risk düzeyi risk olay önem ve risk olayı güven için bir gösterge (yüksek, Orta veya düşük) özelliğidir. Bu özellik, gerçekleştirmeniz gereken eylemleri öncelik vermeniz için yardımcı olur. 

Risk olay önem bir göstergesi olduğu kimlik güvenliğinin aşılmasına sinyal gücünü temsil eder.  
GÜVENİRLİK hatalı pozitif sonuç olasılığını için bir göstergesidir. 

Örneğin, 

* **Yüksek**: yüksek güvenilirlik ve yüksek önem derecesi risk olay. Bu olaylar, kullanıcının kimliğini tehlikeye girdiğini ve etkilenen herhangi bir kullanıcı hesabı hemen düzeltilmesi güçlü göstergesidir.

* **Orta**: yüksek öneme sahip, ancak daha düşük güvenilirlik risk olayı (veya tersi). Bu olaylar riskli ve etkilenen herhangi bir kullanıcı hesabı düzeltilmesi.

* **Düşük**: Düşük güvenilirlik ve düşük önem risk olay. Bu olay bir Acil eylem gerekli değil, ancak diğer risk olayları ile birleştirildiğinde kimliğini aşılıp güçlü bir gösterge sağlayabilir.

![Risk düzeyi](./media/active-directory-reporting-risk-events/01.png)

### <a name="leaked-credentials"></a>Sızan kimlik bilgileri

Kimlik bilgileri Risk olaylarını olarak sınıflandırılan sızmasını bir **yüksek**, kullanıcı adı ve parola bir saldırgan kullanılabilir olduğunu NET bir belirti sağlarlar.

### <a name="sign-ins-from-anonymous-ip-addresses"></a>Anonim IP adreslerinden oturum açma işlemleri

Bu riski olay türü için risk düzeyi **orta** anonim bir IP adresi bir hesap güvenliğin güçlü bir gösterge olmadığından.  
Anonim IP adreslerini kullanmakta oldukları olmadığını doğrulamak için kullanıcı hemen başvurmanızı öneririz.


### <a name="impossible-travel-to-atypical-locations"></a>Alışılmadık konumlara imkansız seyahat

Mümkün olmayan seyahat genellikle, bir bilgisayar korsanının başarıyla oturum açma için iyi bir göstergesidir. Ancak, bir kullanıcı yeni bir cihaz veya genellikle kuruluşunuzdaki diğer kullanıcılar tarafından kullanılmayan bir VPN kullanarak dolaşırken yanlış pozitif sonuç ortaya çıkabilir. Yanlış sunucusu IP'leri görünümü verebilir IP'leri istemci olarak geçirmek uygulamaları yanlış pozitifler başka bir kaynağıdır oturum açma işlemleri burada bu uygulamayı arka uç veri merkezi alma yerden barındırılan (Microsoft veri merkezleri, bunlar genellikle, Görünüm verebilir oturum açma işlemleri Microsoft'tan gerçekleşmesini ait IP adresleri). Bu riski olay risk düzeyi bu yanlış pozitifler sonucunda olan **orta**.

> [!TIP]
> Yapılandırarak bu risk olay türü için bildirilen yanlış pozitif sonuç miktarını azaltabilirsiniz [konumları adlı](active-directory-named-locations.md). 

### <a name="sign-in-from-unfamiliar-locations"></a>Tanınmayan konumlardan oturum aç

Tanınmayan konumlardan saldırgan çalınan kimlik kullanabilmek için güçlü bir gösterge sağlar. Bir kullanıcı seyahat ederken, yeni bir cihaz çalışıyor ya da yeni bir VPN kullanarak yanlış pozitif sonuç ortaya çıkabilir. Bu hatalı pozitif sonuç sonucu olarak, bu olay türü için risk düzeyi olan **orta**.

### <a name="sign-ins-from-infected-devices"></a>Bulaşma olan cihazlardan oturum açma işlemleri

Bu riski olay IP adresleri, kullanıcı aygıtları tanımlar. Tek bir IP adresi birkaç aygıtlardır ve bazı öğeler yalnızca oturum açma işlemleri diğer aygıtlardan bir bot ağ my tetikleyici bu olay gereksiz yere, bu risk olay sınıflandırma neden olduğu denetlediği olarak **düşük**.  

Kullanıcıyla iletişime geçin ve kullanıcının tüm cihazlarına tarama öneririz. Kullanıcının kişisel cihaz bulaşmış veya daha önce belirtildiği gibi kullanıcı olarak aynı IP adresini etkilenen bir aygıttan, başka biri tarafından kullanılan mümkündür. Etkilenen cihazlar genellikle virüsten koruma yazılımı tarafından henüz tanımlanmamış ve cihaz etkilenmesine neden olmuş olabilir hatalı kullanıcı alışkanlıklarınıza da gösterebilir kötü amaçlı yazılımdan etkilenmiş durumdadır.

Adres kötü amaçlı yazılımların yayılmasını kullanma hakkında daha fazla bilgi için bkz: [kötü amaçlı yazılımdan koruma Merkezi](http://go.microsoft.com/fwlink/?linkid=335773&clcid=0x409).


### <a name="sign-ins-from-ip-addresses-with-suspicious-activity"></a>Şüpheli etkinlik gösteren IP adreslerinden gerçekleştirilen oturum açma işlemleri

Bunlar aslında şüpheli olarak işaretlendi bir IP adresinden oturum olmadığını doğrulamak için kullanıcı başvurmanızı öneririz. Bu olay türü için risk düzeyi "**orta**" birkaç cihaz aynı IP adresi olabilir çünkü bazı şüpheli etkinlik için sorumlu olabilir yalnızca while. 


 
## <a name="next-steps"></a>Sonraki adımlar

Risk olayları, Azure AD kimlik koruması temelidir. Azure AD, şu anda altı risk olaylarını tespit edebilirsiniz: 


| Risk olay türü | Risk düzeyi | Algılama türü |
| :-- | --- | --- |
| [Sızan kimlik bilgilerine sahip kullanıcılar](#leaked-credentials) | Yüksek | Çevrimdışı |
| [Anonim IP adreslerinden gerçekleştirilen oturum açma işlemleri](#sign-ins-from-anonymous-ip-addresses) | Orta | Gerçek zamanlı |
| [Alışılmadık konumlara imkansız seyahat](#impossible-travel-to-atypical-locations) | Orta | Çevrimdışı |
| [Fazla tanınmayan konumlardan gerçekleştirilen oturum açma işlemleri](#sign-in-from-unfamiliar-locations) | Orta | Gerçek zamanlı |
| [Virüs bulaşmış cihazlardan gerçekleştirilen oturum açma işlemleri](#sign-ins-from-infected-devices) | Düşük | Çevrimdışı |
| [Şüpheli etkinlik gösteren IP adreslerinden gerçekleştirilen oturum açma işlemleri](#sign-ins-from-ip-addresses-with-suspicious-activity) | Orta | Çevrimdışı|

Ortamınızda algılanan risk olayı bulabileceğiniz?
Burada bildirilen risk olayları gözden iki yerde vardır:

 - **Azure AD raporlama** -Risk olaylarını Azure AD güvenlik parçası olan raporlar. Daha fazla ayrıntı için bkz: [risk güvenlik raporu kullanıcılar](active-directory-reporting-security-user-at-risk.md) ve [riskli oturum açma işlemleri güvenlik raporu](active-directory-reporting-security-risky-sign-ins.md).

 - **Azure AD Identity Protection** -Risk olaylardır ayrıca parçası [Azure Active Directory Identity Protection'ın](active-directory-identityprotection.md) raporlama özellikleri.
    

Risk olaylarını zaten algılanması kimliklerinizi korumanın önemli bir özelliği temsil ederken Ayrıca bunları el ile adres veya koşullu erişim ilkeleri yapılandırarak otomatik yanıtlar bile uygulama seçeneğiniz vardır. Daha fazla ayrıntı için bkz: [Azure Active Directory Identity Protection'ın](active-directory-identityprotection.md).
 
