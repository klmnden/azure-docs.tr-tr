---
title: Azure Active Directory risk olayları | Microsoft Docs
description: Bu artice risk olayları nelerdir ayrıntılı genel bakış sağlar.
services: active-directory
keywords: Azure active directory kimlik koruması, güvenlik, risk, risk düzeyi, güvenlik açığı, güvenlik ilkesi
author: MarkusVi
manager: daveba
ms.assetid: fa2c8b51-d43d-4349-8308-97e87665400b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: report-monitor
ms.date: 11/13/2018
ms.author: markvi
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: 02fc505c8f14f4cc0e486502060a16af47c68bbc
ms.sourcegitcommit: 70550d278cda4355adffe9c66d920919448b0c34
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58439031"
---
# <a name="azure-active-directory-risk-events"></a>Azure Active Directory risk olayları

Güvenlik ihlallerini büyük çoğunluğu göz önüne bir yerde saldırganların bir kullanıcının kimliğini çalarak bir ortama erişimi elde edin. Tehlikeye atılmış kimlik keşfetme hiçbir kolay bir görevdir. Azure Active Directory, kullanıcı hesaplarınızla ilgili kuşkulu eylemleri algılamak için Uyarlamalı makine öğrenimi algoritmaları ve buluşsal yöntemler kullanır. Her kuşkulu eylem adlı bir kayıt depolanır algılanan bir **risk olayı**.

İki yerde bildirilmiş risk olaylarını gözden burada verilmiştir:

 - **Azure AD raporlama** -Risk olayları, Azure AD'nin güvenlik parçası olan raporlar. Daha fazla bilgi için [risk altındaki kullanıcılar güvenlik raporu](concept-user-at-risk.md) ve [riskli oturum açma işlemleri güvenlik raporu](concept-risky-sign-ins.md).

 - **Azure AD kimlik koruması** -Risk olayları parçası olan Raporlama yeteneklerini [Azure Active Directory kimlik koruması](../active-directory-identityprotection.md).

Ayrıca, kullanabileceğiniz [kimlik koruması risk olayları API](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/identityriskevent) Microsoft Graph'ı kullanarak güvenlik algılamaları programlı erişim elde etmek için. Daha fazla bilgi için [Microsoft Graph ve Azure Active Directory kimlik koruması ile çalışmaya başlama](../identity-protection/graph-get-started.md). 

Şu anda, Azure Active Directory risk olayları altı tür algılar:

- [Sızdırılan kimlik bilgilerine sahip kullanıcılar](#leaked-credentials) 
- [Anonim IP adreslerinden oturum açma](#sign-ins-from-anonymous-ip-addresses) 
- [Alışılmadık konumlara imkansız seyahat](#impossible-travel-to-atypical-locations) 
- [Bulaşma olan cihazlardan oturum açma](#sign-ins-from-infected-devices) 
- [Şüpheli etkinliğin olduğu IP adreslerinden oturum açma](#sign-ins-from-ip-addresses-with-suspicious-activity) 
- [Alışılmadık konumlardan oturum açma](#sign-in-from-unfamiliar-locations) 

![Risk olayı](./media/concept-risk-events/91.png)

> [!IMPORTANT]
> Bazı durumlarda, karşılık gelen oturum açma bir giriş olmadan bir risk olayını bulabilirsiniz [oturum açma işlemleri raporu](concept-sign-ins.md). Kimlik koruması her ikisi için risk değerlendirdiğinden budur **etkileşimli** ve **etkileşimli olmayan** oturum açma işlemleri, oysa yalnızca etkileşimli oturum açma oturum açma işlemleri raporu gösterir.

Algılanan risk olayı için alma öngörü için Azure AD aboneliğiniz bağlıdır. 

* İle **Azure AD Premium P2 sürümünü**, temel alınan tüm algılamalar hakkında en ayrıntılı bilgileri alın. 
* İle **Azure AD Premium P1 edition**, lisansınızı tarafından kapsanmaz algılamalar risk olayı görünür **ek risk algılandı ile oturum açma**.

Risk olayları zaten algılanması kimliklerinizi korumanın önemli bir yönüdür temsil ederken, ayrıca el ile bunları adres veya koşullu erişim ilkelerini yapılandırarak otomatik yanıtlar uygulamak seçeneğiniz vardır. Daha fazla bilgi için [Azure Active Directory kimlik koruması](../active-directory-identityprotection.md).

## <a name="risk-event-types"></a>Risk olayı türleri

**Risk olayı türü** özelliği için bir risk olayını kaydı oluşturuldu şüpheli eylem için bir tanımlayıcıdır.

Microsoft'un sürekli yatırım Algılama işlemi neden:

- Mevcut risk olaylarının algılama doğruluğu geliştirmeleri 
- Gelecekte eklenecek yeni risk olayı türleri

### <a name="leaked-credentials"></a>Sızdırılan kimlik bilgileri

Kullanıcıların geçerli parolalarını cybercriminals tehlikeye, bunlar genellikle bu kimlik bilgilerini paylaşır. Bu genellikle, bunları herkese açık şekilde koyu Yapıştır ya da web sitelerinde veya ticari veya kimlik bilgilerini siyah piyasadaki satış yayınlayarak da gerçekleştirilir. Microsoft kimlik bilgilerinin sızdırılması hizmet edinme kullanıcı adı / parola çiftlerini ortak veya koyu web sitelerini izleme ve çalışma tarafından:

- Araştırmacılar
- Yasal makamlar
- Microsoft Güvenlik takımlar
- Diğer güvenilen kaynaklardan 

Zaman hizmeti edinir kullanıcı adı / parola çiftleri bunlar denetlenir karşı AAD kullanıcıların geçerli geçerli kimlik bilgileri. Bir eşleşme bulunduğunda, bir kullanıcının parolasını aşıldığını gösterir ve **kimlik risk olayı sızmasına** oluşturulur.

### <a name="sign-ins-from-anonymous-ip-addresses"></a>Anonim IP adreslerinden oturum açma işlemleri

Bu risk olayı türünü başarıyla anonim proxy IP adresi tanımlanmış bir IP adresinden oturum açmış kullanıcılar tanımlar. Bu proxy'ler cihazlarının IP adresini gizlemek istediğiniz ve için kötü amaçlı kullanılan kişiler tarafından kullanılır.

### <a name="impossible-travel-to-atypical-locations"></a>Alışılmadık konumlara imkansız seyahat

Davranışı verilen burada konumları en az biri de kullanıcı için alışılmadık olabilir, coğrafi olarak uzak konumlardan gerçekleştirilen iki oturum açma Bu risk olayı türünü tanımlar. Diğer çeşitli faktörler arasında bu makine öğrenimi algoritmasının iki oturum açma ve bu kullanıcının ilk konumdan farklı bir kullanıcı aynı kullandığını gösteren ikinci, seyahat alacağı saat arasındaki süre dikkate alır kimlik bilgileri.

Algoritma "VPN'ler ve kuruluştaki diğer kullanıcılar tarafından düzenli olarak kullanılan konumlar gibi mümkün olmayan seyahat koşullar katkıda bulunan belirgin hatalı pozitif sonuçlar" yok sayar. Sistem bir öğrenme dönemi boyunca yeni bir kullanıcının oturum açma davranışı öğrenir 14 gün vardır. 

### <a name="sign-in-from-unfamiliar-locations"></a>Alışılmadık konumlardan oturum açın

Oturum açma konumları Bu risk olayı türünü göz önünde bulundurur (IP, enlem / boylam ve ASN) yeni / alışılmadık konumlara belirlemek için. Sistem, bir kullanıcı tarafından kullanılan önceki konumları hakkında bilgi depolar ve bu "tanıdık" konumlar göz önünde bulundurur. Risk olayı bilinen konumları listesinde olmayan bir konumdan oturum açma meydana geldiğinde tetiklenir. Sistem, bu sırada, yeni konumlarına tanınmayan konumlardan olarak işaretlemez 30 günlük bir öğrenme dönemi sahiptir. Tanıdık cihazlardan ve coğrafi olarak bilinen bir konuma yakın olan konumlardan oturum açma işlemleri de yoksayar. 

Kimlik koruması, ayrıca temel kimlik doğrulaması için alışılmadık konumlardan oturum açma algılar / eski protokoller. Bu protokollerin istemci kimliği gibi modern tanıdık özellikleri olmadığı için hatalı pozitif sonuçları azaltmak için yeterli telemetri yok. Algılanan risk olayları sayısını azaltmak için modern kimlik doğrulaması için taşımanız gerekir.   

### <a name="sign-ins-from-infected-devices"></a>Bulaşma olan cihazlardan oturum açma işlemleri

Etkin bir bot sunucusuyla iletişim kurmak için bilinen kötü amaçlı yazılım, virüs bulaşmış cihazlardan oturum açma Bu risk olayı türünü tanımlar. Bu, IP adreslerini kullanıcı cihazının iletişim kurmayan bir bot sunucusu olan IP adresleri karşı karşılaştırılarak ilişkilendirilmesi yoluyla belirlenir. 

### <a name="sign-ins-from-ip-addresses-with-suspicious-activity"></a>Şüpheli etkinlik gösteren IP adreslerinden gerçekleştirilen oturum açma işlemleri
IP adresleri, çok sayıda başarısız oturum açma denemesi, birden çok kullanıcı hesabında, kısa bir süre içinde karşılaşılan bu risk olayı türünü tanımlar. Bu durum, saldırganlar tarafından kullanılan IP adresleri trafik düzenleriyle eşleşir ve hesapları ya da zaten veya hakkında riske girdiği güçlü bir göstergesi olduğu. Bir machine learning belirgin yanlış pozitifleri, düzenli olarak kuruluştaki diğer kullanıcılar tarafından kullanılan IP adresleri gibi yoksayar algoritması budur.  Burada, yeni kullanıcı ve yeni Kiracı oturum davranışını öğrenir 14 günlük bir öğrenme dönemi sistem var.

## <a name="detection-type"></a>Algılama türü

Algılama type özelliği göstergesidir (**gerçek zamanlı** veya **çevrimdışı**) için bir risk olayının algılama zaman çerçevesi. Şu anda, risk olayı gerçekleştikten sonra çoğu risk olayları çevrimdışı bir işlem sonrası işlemi algılandı.

Aşağıdaki tabloda, ilgili bir raporda görünmesini algılama türü için gereken süre miktarını listeler:

| Algılama türü | Raporlama gecikme süresi |
| --- | --- |
| Gerçek zamanlı | 5-10 dakika |
| Çevrimdışı | 2-4 saat |


Azure Active Directory algılar risk olayı türleri için saptama türleri şunlardır:

| Risk olayı türü | Algılama türü |
| :-- | --- | 
| [Sızdırılan kimlik bilgilerine sahip kullanıcılar](#leaked-credentials) | Çevrimdışı |
| [Anonim IP adreslerinden oturum açma](#sign-ins-from-anonymous-ip-addresses) | Gerçek zamanlı |
| [Alışılmadık konumlara imkansız seyahat](#impossible-travel-to-atypical-locations) | Çevrimdışı |
| [Alışılmadık konumlardan oturum açma](#sign-in-from-unfamiliar-locations) | Gerçek zamanlı |
| [Bulaşma olan cihazlardan oturum açma](#sign-ins-from-infected-devices) | Çevrimdışı |
| [Şüpheli etkinliğin olduğu IP adreslerinden oturum açma](#sign-ins-from-ip-addresses-with-suspicious-activity) | Çevrimdışı|


## <a name="risk-level"></a>Risk düzeyi

Bir risk olayının risk düzeyi özelliği göstergesidir (**yüksek**, **orta**, veya **düşük**) önem ve bir risk olayının güven için. Bu özellik, gerçekleştirmeniz gereken eylemler öncelik vermenize yardımcı olur. 

Risk olayının önem kimliğinin tehlike bir tahmin unsuru sinyal gücünü temsil eder. Güvenle hatalı pozitif sonuçları olasılığını göstergesidir. 

Örneğin, 

* **Yüksek**: Yüksek güvenilirlik ve önem derecesi yüksek risk olayı. Bu, kullanıcının kimliğini açığa çıkardığını ve etkilenen tüm kullanıcı hesaplarını hemen düzeltilmesi güçlü göstergeleri olaylardır.

* **Orta**: Yüksek öneme sahip, ancak daha düşük güven risk olayı ya da tam tersi. Riskli olabilecek bu olaylar ve etkilenen tüm kullanıcı hesaplarını düzeltilmesi.

* **Düşük**: Düşük güvenilirlik ve düşük önem derecesi risk olayı. Bu olay bir Acil eylem gerekli değil, ancak diğer risk olayları ile birleştirildiğinde kimlik tehlikeye girmemesini güçlü bir gösterge sağlayabilir.

![Risk düzeyi](./media/concept-risk-events/01.png)

### <a name="leaked-credentials"></a>Sızdırılan kimlik bilgileri

Risk olayları olarak sınıflandırılan kimlik bilgilerinin sızdırılması bir **yüksek**, kullanıcı adı ve parola için bir saldırgan kullanılabilir olduğunu NET bir belirti sağlarlar.

### <a name="sign-ins-from-anonymous-ip-addresses"></a>Anonim IP adreslerinden oturum açma işlemleri

Bu risk olayı türü için risk düzeyi **orta** anonim bir IP adresi geçerli olmadığından bir hesabı tehlike güçlü bir göstergesi. Anonim IP adresleri kullanmakta olduğunuz olmadığını doğrulamak için kullanıcı hemen başvurmanızı öneririz.


### <a name="impossible-travel-to-atypical-locations"></a>Alışılmadık konumlara imkansız seyahat

Mümkün olmayan seyahat genellikle bir bilgisayar korsanının başarıyla oturum açabilir, iyi bir göstergesidir. Ancak, bir kullanıcı yeni bir cihaz veya genellikle kuruluştaki diğer kullanıcılar tarafından kullanılmayan bir VPN kullanarak dolaşırken yanlış pozitifleri ortaya çıkabilir. Hatalı sunucu IP'ler görünümünü verebilir IP'ler, istemci olarak geçen uygulamaları yanlış pozitifleri başka bir kaynağıdır oturum açma işlemleri alma Burada, uygulama arka uç veri merkezi bir yerde barındırılan (Microsoft veri merkezleri, bunlar genellikle, görünümünü verebilir ait IP adresleri Microsoft gerçekleşen oturum açma işlemleri). Bu yanlış pozitifleri sonucunda bu risk olayı yönelik risk düzeyi olan **orta**.

> [!TIP]
> Yapılandırarak bu risk olayı türü bildirilen yanlış pozitifleri miktarını azaltabilirsiniz [adlandırılmış Konumlar](../active-directory-named-locations.md). 

### <a name="sign-in-from-unfamiliar-locations"></a>Alışılmadık konumlardan oturum açın

Tanınmayan konumlardan saldırgan çalınan kimlik kullanabilmek için güçlü bir gösterge sağlar. Bir kullanıcı seyahatindeki, yeni bir cihaz olduğunu çalışıyor ya da yeni bir VPN kullanarak yanlış pozitifleri ortaya çıkabilir. Bu hatalı pozitif sonuçları sonucu olarak, bu olay türüne yönelik risk düzeyi olan **orta**.

### <a name="sign-ins-from-infected-devices"></a>Bulaşma olan cihazlardan oturum açma işlemleri

Bu risk olayı IP adresleri, kullanıcı cihazları tanımlar. Tek bir IP adresi birden fazla cihazlardır ve yalnızca bazı olan diğer cihazlardan oturum açma işlemleri bir bot ağ my tetikleyicisi bu olay gereksiz yere, bu risk olayı olarak sınıflandırılır neden olduğu denetlediği **düşük**.  

Kullanıcıyla iletişime geçin ve kullanıcının tüm cihazlarına tarama öneririz. Ayrıca bir kullanıcının kişisel cihaz bulaşmış veya başkasının kullanıcı olarak aynı IP adresini bir virüs bulaşmış CİHAZDAN kullanıyordu mümkündür. Etkilenen cihazlar genellikle tarafından virüsten koruma yazılımının henüz belirlenmedi ve aygıtın bulaşmış haline yaşamış olabileceğiniz herhangi bir hatalı kullanıcı alışkanlıkları da gösterebilir kötü amaçlı yazılım tarafından etkilenen.

Adresi kötü amaçlı yazılımdan Etkilenme hakkında daha fazla bilgi için bkz. [kötü amaçlı yazılımdan koruma Merkezi](https://www.microsoft.com/en-us/security/portal/definitions/adl.aspx/).

### <a name="sign-ins-from-ip-addresses-with-suspicious-activity"></a>Şüpheli etkinlik gösteren IP adreslerinden gerçekleştirilen oturum açma işlemleri

Bunlar gerçekten şüpheli olarak işaretlendi bir IP adresinden oturum olmadığını doğrulamak için kullanıcı başvurmanızı öneririz. Bu olay türü için risk düzeyi "**orta**" çeşitli cihazlar aynı IP adresini olabileceğinden, yalnızca kuşkulu etkinlik için sorumlu olabilir çalışırken. 


## <a name="next-steps"></a>Sonraki Adımlar

* [Risk altındaki kullanıcılar güvenlik raporu](concept-user-at-risk.md)
* [Riskli oturum açma işlemleri güvenlik raporu](concept-risky-sign-ins.md)
* [Azure AD kimlik koruması](../active-directory-identityprotection.md).
