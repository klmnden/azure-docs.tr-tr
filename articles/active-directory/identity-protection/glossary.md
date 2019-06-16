---
title: Azure Active Directory kimlik koruması Kılavuzu sözlüğü | Microsoft Docs
description: Azure Active Directory kimlik koruması Kılavuzu sözlüğü
services: active-directory
keywords: Azure active directory kimlik koruması, bulut uygulaması bulma, uygulamaları, güvenlik, risk, risk düzeyi, güvenlik açığı, güvenlik ilkesi, sözlük yönetme
documentationcenter: ''
author: MicrosoftGuyJFlo
manager: daveba
ms.assetid: 833119a5-33d6-4482-adda-fa35218c72c3
ms.service: active-directory
ms.subservice: identity-protection
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2018
ms.author: joflore
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: c371254f344b321969dcc9b3c36212b7536aa95a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67109015"
---
# <a name="azure-active-directory-identity-protection-glossary"></a>Azure Active Directory kimlik koruması Kılavuzu sözlüğü
### <a name="at-risk-user"></a>Risk (kullanıcı)
Bir kullanıcı bir veya daha fazla etkin risk olayları. 

### <a name="atypical-sign-in-location"></a>Oturum açma konumu alışılmadık
Bir oturum açma bir coğrafi konumdan belirli kullanıcı, benzer kullanıcılar veya Kiracı için tipik değildir.

### <a name="azure-ad-identity-protection"></a>Azure AD Kimlik Koruması
Risk olayları ve bir kuruluşunuzun kimliklerini etkileyen olası güvenlik açıklarına ilişkin birleştirilmiş görünüm sağlayan Azure Active Directory güvenlik modül.

### <a name="conditional-access"></a>Koşullu Erişim
Kaynaklara erişimi güvenli hale getirmek için bir ilke. Koşullu erişim kuralları, Azure Active Directory'de depolanır ve kaynağa erişim izni vermeden önce Azure AD tarafından değerlendirilir.  Kullanıcı konumu, cihaz sistem durumu veya kullanıcı kimlik doğrulaması yöntemine dayalı olarak erişimi kısıtlama örnek kurallar içerir.

### <a name="credentials"></a>Kimlik Bilgileri
Kimlik ve ağ kaynakları ve yerel erişim sağlamak için kullanılan kimlik kanıtlama bilgileri. Kullanıcı adları ve parolalar, akıllı kart ve sertifika kimlik bilgileri örnekleridir.

### <a name="event"></a>Olay
Azure Active Directory'de bir etkinlik kaydı.

### <a name="false-positive-risk-event"></a>Hatalı pozitif (risk olayı)
Risk olayı araştırılması ve yanlış bir risk olayı bayrak eklenmiş gösteren bir kimlik koruması kullanıcı tarafından el ile ayarlanan risk olayı durumu.

### <a name="identity"></a>Kimlik
Bir kişi veya kuruluşa kimlik doğrulaması, parola veya sertifika gibi ölçütlere göre yoluyla doğrulanması gerekir.

### <a name="identity-risk-event"></a>Kimlik risk olayı
Anormal kimlik koruması tarafından işaretlenen ve bir kimliğin gizliliğinin bozulduğunu gösteriyor olabilir AAD olay.

### <a name="ignored-risk-event"></a>(Risk olayı) yoksayıldı
Risk olay durumu risk olayı bir düzeltme eylemi yapmadan kapalı olduğunu belirten bir kimlik koruması kullanıcı tarafından el ile olarak ayarlanmış.

### <a name="impossible-travel-from-atypical-locations"></a>Alışılmadık konumlardan mümkün olmayan seyahat
Bir risk olayını iki oturum açma aynı kullanıcı için en az bir tanesi bir alışılmadık oturum açma konumundan olduğu ve oturum açma işlemleri arasındaki zaman fiziksel olarak bu konumlar arasında seyahat harcadığım minimum süre daha kısa olduğu algılandığında tetiklenir.  

### <a name="investigation"></a>Araştırma
Etkinlikler, günlükleri ve düzeltme ya da risk azaltma adımlarını gerekli olup olmadığını karar vermek için bir risk olayını ilgili diğer ilgili bilgileri gözden geçirme işlemi, varsa ve nasıl kimliğinin güvenliğinin aşıldığını ve anlamak anlama nasıl kimliğin gizliliğinin kullanıldı.

### <a name="leaked-credentials"></a>Sızdırılan kimlik bilgileri
Bir risk olayını geçerli kullanıcı kimlik bilgilerini (kullanıcı adı ve parola) koyu web sayfamızı Araştırmacıları tarafından yayımlandığını bulunduğunda tetiklenir.

### <a name="mitigation"></a>Risk azaltma
Sınırlama veya saldırgan güvenliği aşılmış kimlik veya cihaz kimliği veya cihaz güvenli bir duruma geri yüklemeden yararlanma olanağı ortadan kaldırmak için bir eylem. Bir risk azaltma kimlik veya cihaz ile ilişkili önceki risk olayları çözmez.

### <a name="multi-factor-authentication"></a>Multi-factor authentication
Kullanıcının bir şey içerebilir, iki veya daha fazla kimlik doğrulama yöntemleri gerektiren bir kimlik doğrulama yöntemi, bu tür bir sertifika vardır; bir kullanıcının, kullanıcı adları, parolalar veya parolalar gibi bildiği; parmak izi gibi fiziksel öznitelikleri, ve kişisel imzası gibi kişisel öznitelikleri.

### <a name="offline-detection"></a>Çevrimdışı olan algılama
Anomalileri ve riskli oturum açma girişimi gibi bir olayın bir değerlendirme zaten gerçekleşen olay çözümledikten sonra algılanması.

### <a name="policy-condition"></a>İlke koşulu
(Grupları, kullanıcılar, uygulamalar, cihaz platformları, cihaz durumları, IP aralıkları, istemci türlerinin) varlıklar tanımlayan bir Güvenlik İlkesi ' nin bir parçası, ilkeye dahil veya ondan hariç.

### <a name="policy-rule"></a>İlke kuralı
İlkeyi ve ilke tetiklendiğinde gerçekleştirilecek eylemleri tetikleyecek olan koşullar açıklayan bir güvenlik ilkesinin parçası.

### <a name="prevention"></a>Önleme
Bir kimlik veya cihazın kötüye kullanımı ile kuruluşun zarar önlemek için bir eylem olduğundan şüphelenilen veya tehlikeye biliyor. Bir engelleme eylemi, kimlik ve cihaz güvenliğini sağlamaz ve önceki risk olayları çözmez.

### <a name="privileged-user"></a>Ayrıcalıklı (kullanıcı)
Bir genel yönetici gibi Azure Active Directory'de bir veya daha fazla kaynak için geçici veya kalıcı yönetici izinlerine sahip olan bir risk olayının zaman kullanıcı Faturalama Yöneticisi, Hizmet Yöneticisi, yönetici kullanıcı ve parola Yöneticisi. 

### <a name="real-time"></a>Gerçek zamanlı
Gerçek zamanlı algılama bakın.

### <a name="real-time-detection"></a>Gerçek zamanlı algılama
Anomali algılama ve değerlendirme riskli oturum açma denemesi olayından önce gibi bir olayın devam etmesine izin verilir.

### <a name="remediated-risk-event"></a>Çözümlendi (risk olayı)
Kimlik koruması, risk olayının standart düzeltme eylemini kullanarak bu risk olayı türü için düzeltilen olduğunu belirten tarafından otomatik olarak ayarlanan bir risk olayı durumu. Örneğin, kullanıcı parolasını sıfırlama, önceki parola güvenliğinin aşıldığını belirten birçok risk olayları otomatik olarak düzeltilir.

### <a name="remediation"></a>Düzeltme
Bir kimlik veya daha önce olduğundan şüphelenilen veya tehlikeye bilinen bir cihazı güvenli hale getirmek için bir eylem. Bir düzeltme eylemi kimliği veya cihaz güvenli bir duruma geri yükler ve kimlik ya da cihaz ile ilişkili önceki risk olayları giderir.

### <a name="resolved-risk-event"></a>Çözümlendi (risk olayı)
Kullanıcı uygun düzeltme eylemi dışında kimlik koruması sürdü ve risk olayının değerlendirilmesi gerektiğini gösteren el ile bir kimlik koruması kullanıcılar tarafından ayarlanan bir risk olayı durumu kapalı.

### <a name="risk-event-status"></a>Risk olayı durumu
Bir özellik bir risk olayının olay etkin olup olmadığını ve gösteren kapalı, kapatma nedeni.

### <a name="risk-event-type"></a>Risk olayı türü
Riskli olarak kabul edilmesi olaya neden anomali türünü belirten risk olayı bir kategori.

### <a name="risk-level-risk-event"></a>Risk düzeyi (risk olayı)
Önem derecesini eylemleri öncelik kimlik koruması kullanıcılara yardımcı olmak için risk olayı bir göstergesi (yüksek, Orta veya düşük) kuruluşuna riskini azaltmak için bunlar yararlanın. 

### <a name="risk-level-sign-in"></a>Risk düzeyi (oturum açma)
Bir göstergesi (yüksek, Orta veya düşük) bir özel oturum açmak için başkasının kullanıcı kimliğini kullanın bağlanmaya çalıştığı olasılığını.

### <a name="risk-level-user-compromise"></a>Risk düzeyine (kullanıcı güvenliğinin aşılması)
Bir göstergesi (yüksek, Orta veya düşük) bir kimliğin gizliliğinin bozulduğunu olasılığı azaltır.

### <a name="risk-level-vulnerability"></a>Risk düzeyi (güvenlik)
Önem derecesini eylemleri öncelik kimlik koruması kullanıcılara yardımcı olmak için güvenlik açığı bir göstergesi (yüksek, Orta veya düşük) kuruluşuna riskini azaltmak için bunlar yararlanın.

### <a name="secure-identity"></a>(Kimlik) güvenli hale getirme
Parola değiştirme veya potansiyel olarak güvenliği aşılmış kimlik güvenliği aşılmamış bir duruma geri yüklemek için yeniden görüntü makine gibi düzeltme eylemi yararlanın.

### <a name="security-policy"></a>Güvenlik ilkesi
İlke kuralları ve koşul koleksiyonu. Kullanıcılar, gruplar, uygulamalar, cihazlar, cihaz platformları, cihaz durumları, IP aralıkları ve Auth2.0 istemci türleri gibi varlıkları için bir ilke uygulanabilir. Bir ilke etkinleştirildiğinde, ilkeye dahil bir varlık, bir kaynak için bir belirteç verilir olduğunda değerlendirilir.

### <a name="sign-in-v"></a>(V) oturum açın
Azure Active Directory'de kimlik doğrulamak için.

### <a name="sign-in-n"></a>Oturum açma (n)
Bu işlem yakalar olay yanı sıra Azure Active Directory kimlik doğrulama eylemi veya işlemi.

### <a name="sign-in-from-anonymous-ip-address"></a>Anonim IP adresinden oturum açın
Risk olayı bir başarılı oturum açma IP adresinden anonim proxy IP adresi belirlendikten sonra tetiklenir.

### <a name="sign-in-from-infected-device"></a>Virüs bulaşmış CİHAZDAN oturum aç
Bir risk olayı bir bot sunucusuyla iletişim kurmak için etkin bir şekilde çalıştığınız bir veya daha fazla güvenliği aşılmış cihazlar tarafından kullanılmak üzere bilinen bir IP adresinden kaynaklanan bir oturum açma tetiklenir.

### <a name="sign-in-from-ip-address-with-suspicious-activity"></a>Şüpheli etkinliğin olduğu IP adresinden oturum açın
Birden çok kullanıcı hesabında kısa bir süre içinde çok sayıda başarısız oturum açma girişimlerini bir oturum açma başarılı bir IP adresi sonra bir risk olayı tetiklenir.

### <a name="sign-in-from-unfamiliar-location"></a>Bilinmeyen konumdan oturum açın
Kullanıcı başarılı bir şekilde yeni bir konumdan (IP, enlem/boylam ve ASN) oturum açtığında tetiklenen bir risk olayı.

### <a name="sign-in-risk"></a>Oturum açma riski
Risk bakın (oturum açma) düzeyi

### <a name="sign-in-risk-policy"></a>Oturum açma riski İlkesi
Bir özel oturum açma riskini değerlendirir ve önceden tanımlanmış koşullar ve kurallara dayanan bir risk azaltma işlemleri geçerli bir koşullu erişim ilkesi.

### <a name="user-compromise-risk"></a>Kullanıcı güvenlik ihlali riski
Risk bakın (kullanıcı güvenliğinin aşılması) düzeyi

### <a name="user-risk"></a>Kullanıcı riski
Bkz risk düzeyine (kullanıcı güvenliğinin aşılması).

### <a name="user-risk-policy"></a>Kullanıcı riski İlkesi
Oturum açma göz önünde bulundurur ve önceden tanımlanmış koşullar ve kurallara dayanan bir risk azaltma işlemleri geçerli bir koşullu erişim ilkesi.

### <a name="users-flagged-for-risk"></a>Riskli oldukları belirlenen kullanıcılar
Etkin ya da düzeltilen risk olayları, kullanıcılar

### <a name="vulnerability"></a>Güvenlik Açığı
Bir yapılandırma veya Azure Active Directory'de koşul getiren dizin açıkları veya tehditlere maruz kalabilir.

## <a name="see-also"></a>Ayrıca bkz.
* [Azure Active Directory kimlik koruması](../active-directory-identityprotection.md)

