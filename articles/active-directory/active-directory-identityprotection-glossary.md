---
title: "Azure Active Directory kimlik koruması Kılavuzu sözlüğü | Microsoft Docs"
description: "Azure Active Directory kimlik koruması Kılavuzu sözlüğü"
services: active-directory
keywords: "Azure active directory kimlik koruması, cloud app discovery'yi, uygulamalar, güvenlik, risk, risk düzeyi, güvenlik açığı, güvenlik ilkesi, sözlük yönetme"
documentationcenter: 
author: MarkusVi
manager: mtillman
ms.assetid: 833119a5-33d6-4482-adda-fa35218c72c3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2018
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 30cf3911d0f22e2d9351fc606cd6697ef437e452
ms.sourcegitcommit: 384d2ec82214e8af0fc4891f9f840fb7cf89ef59
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/16/2018
---
# <a name="azure-active-directory-identity-protection-glossary"></a>Azure Active Directory kimlik koruması Kılavuzu sözlüğü
### <a name="at-risk-user"></a>Risk (kullanıcı)
Bir veya daha fazla etkin risk olaylarına sahip bir kullanıcı. 

### <a name="atypical-sign-in-location"></a>Oturum açma alışılmadık konumu
Bir oturum açma bir coğrafi konumdan belirli kullanıcı, benzer kullanıcılar veya Kiracı için tipik değildir.

### <a name="azure-ad-identity-protection"></a>Azure AD Kimlik Koruması
Birleştirilmiş görünüme risk olaylarına ve olası güvenlik açıklarını kuruluşun kimlikleri etkileyen sağlayan Azure Active Directory güvenlik modül.

### <a name="conditional-access"></a>Koşullu erişim
Kaynaklara erişim güvenliğini sağlamak için bir ilke. Koşullu erişim kuralları Azure Active Directory'de depolanır ve kaynağa erişim izni vermeden önce Azure AD tarafından değerlendirilir.  Örnek kuralları içeren kullanıcı konuma göre erişimi kısıtlama cihaz sistem durumu veya kullanıcı kimlik doğrulama yöntemi.

### <a name="credentials"></a>Kimlik Bilgileri
Tanımlama ve ağ kaynaklarını ve yerel erişim sağlamak için kullanılan kimlik kanıtı içeren bilgiler. Kimlik bilgileri kullanıcı adları ve parolalar, akıllı kartlar ve sertifikalar gösterilebilir.

### <a name="event"></a>Olay
Azure Active Directory'de bir etkinliği kaydı.

### <a name="false-positive-risk-event"></a>Hatalı pozitif (risk olay)
Risk olay durumu risk olayı araştırılan ve hatalı bir risk olayı olarak bayrak eklenen gösteren bir kimlik koruması kullanıcı tarafından el ile ayarlayın.

### <a name="identity"></a>Kimlik
Bir kişi veya parola veya sertifika gibi ölçütlere göre kimlik doğrulaması yoluyla doğrulanması gereken varlık.

### <a name="identity-risk-event"></a>Kimlik risk olayı
Anormal olarak kimlik koruması tarafından işaretlenen ve bir kimlik tehlikede olduğunu gösterebilecek AAD olay.

### <a name="ignored-risk-event"></a>Göz ardı (risk olay)
Risk olay durumu risk olayı düzeltme eylemi yapılıyor olmadan kapalı olduğunu belirten bir kimlik koruması kullanıcı tarafından el ile ayarlayın.

### <a name="impossible-travel-from-atypical-locations"></a>Alışılmadık konumlardan imkansız seyahat
Aynı kullanıcı için iki oturum açma işleminden en az biri bir alışılmadık oturum açma konumundan olduğu ve oturum açma işlemleri arasındaki zaman fiziksel olarak bu konum arasında seyahat için harcanacak en düşük saat değerinden daha kısa olduğu algılandığında tetiklenen bir risk olayı.  

### <a name="investigation"></a>Araştırma
Etkinlikler, günlükler ve düzeltme veya azaltma adımları gerekli olup olmadığını karar vermek için bir risk olayı ile ilgili diğer ilgili bilgileri gözden geçirme işlemi, varsa ve nasıl kimliğini aşılmış ve anlamak anlamak nasıl kimlik gizliliği kullanıldı.

### <a name="leaked-credentials"></a>Sızdırılan kimlik bilgileri
Geçerli kullanıcı kimlik bilgilerini (kullanıcı adı ve parola) koyu web bizim Araştırmacıları tarafından yayımlandığını bulunduğunda tetiklenen bir risk olayı.

### <a name="mitigation"></a>Risk azaltma
Sınırlamak veya saldırgan güvenliği aşılmış kimlik veya cihaz kimliği veya cihaza güvenli bir duruma geri yüklemeden yararlanma yeteneği ortadan kaldırmak için bir eylem. Bir azaltma kimlik veya aygıtla ilişkili önceki risk olaylarını çözümlenmiyor.

### <a name="multi-factor-authentication"></a>Multi-factor authentication
Bir şeyler içerebilir, iki veya daha fazla kimlik doğrulama yöntemleri kullanıcının gerektiren bir kimlik doğrulama yöntemi, böyle bir sertifikası var; bir kullanıcı, kullanıcı adları, parolalar veya parolalar gibi bilir; bir parmak izi gibi fiziksel öznitelikleri; ve kişisel imza gibi kişisel öznitelikleri.

### <a name="offline-detection"></a>Çevrimdışı algılama
Daha fazla bilgi ve oturum açma girişimi gibi bir olay risk değerlendirme zaten gerçekleştirilmedi bir olay için Olgu sonra algılanması.

### <a name="policy-condition"></a>İlke durumu
İlkesine dahil veya ondan dışlanan varlıklar (grupları, kullanıcıları, uygulamalar, cihaz platformları, cihaz durumları, IP aralıkları, istemci türlerinin) tanımlayan bir güvenlik ilkesi parçası.

### <a name="policy-rule"></a>İlke kuralı
İlkeyi ve ilke tetiklendiğinde gerçekleştirilen eylemleri tetikleyecek durumlarda tanımlayan bir güvenlik ilkesi parçası.

### <a name="prevention"></a>Önleme
Bir kimlik veya aygıtın kötüye yoluyla kuruluşun zarar önlemek için bir eylem şüpheli veya tehlikeye bildirin. Önleme eylem aygıt ya da kimlik güvenli değildir ve önceki risk olaylarını çözümlenmiyor.

### <a name="privileged-user"></a>Ayrıcalıklı (kullanıcı)
Bir risk olayı zaman genel bir yönetici gibi Azure Active Directory'de bir veya daha fazla kaynak kalıcı veya geçici yönetim izinlerine sahip bir kullanıcı Faturalama Yöneticisi, Hizmet Yöneticisi, Kullanıcı Yöneticisi ve parola Yöneticisi. 

### <a name="real-time"></a>Gerçek zamanlı
Gerçek zamanlı algılama bakın.

### <a name="real-time-detection"></a>Gerçek zamanlı algılama
Anomalilerin algılanması ve olay önce oturum açma girişimi gibi bir olay risk değerlendirmesine devam etmesine izin verilir.

### <a name="remediated-risk-event"></a>Çözümlendi (risk olay)
Risk olay durumu risk olayı Bu risk olay türü için standart düzeltme eylemini kullanarak düzeltilme olduğunu gösteren kimlik koruması tarafından otomatik olarak ayarlanmış. Örneğin, kullanıcı parolasını sıfırlama, önceki parola aşılmış belirten birçok risk olayları otomatik olarak düzeltilir.

### <a name="remediation"></a>Düzeltme
Bir kimlik veya önceden şüpheli veya tehlikeye bilinen bir cihazı güvenli hale getirmek için bir eylem. Bir düzeltme eylemi kimliği veya cihaza güvenli bir duruma geri yükler ve kimlik veya aygıtla ilişkili önceki risk olaylarını giderir.

### <a name="resolved-risk-event"></a>Çözümlendi (risk olay)
Kullanıcı kimlik koruması dışında bir uygun düzeltme eylemi sürdü ve risk olayı düşünülmesi gereken gösteren el ile bir kimlik koruması kullanıcı tarafından ayarlanan bir risk olay durumu kapalı.

### <a name="risk-event-status"></a>Risk olay durumu
Risk olay özelliği, olayın etkin olup olmadığını ve gösteren kapalı, kapatma nedeni.

### <a name="risk-event-type"></a>Risk olayı türü
Riskli olarak kabul edilmesi olaya neden anomali türünü belirten risk olayı için bir kategori.

### <a name="risk-level-risk-event"></a>Risk düzeyi (risk olay)
Bir gösterge (yüksek, Orta veya düşük) eylemleri öncelik kimlik koruması kullanıcılara yardımcı olmak için risk olay önem derecesi, kuruluşları riskini azaltmak için aldıkları. 

### <a name="risk-level-sign-in"></a>Risk düzeyi (oturum açma)
Bir göstergesi (yüksek, Orta veya düşük) bir özel oturum açma için başka birinin kullanıcının kimliğini kullanmaya çalışıyor olduğunu olasılığı.

### <a name="risk-level-user-compromise"></a>Risk düzeyi (kullanıcı güvenliğinin aşılması)
Bir göstergesi (yüksek, Orta veya düşük) bir kimlik aşılmış olasılığı.

### <a name="risk-level-vulnerability"></a>Risk düzeyi (güvenlik açığı)
Bir gösterge (yüksek, Orta veya düşük) eylemleri öncelik kimlik koruması kullanıcılara yardımcı olmak için güvenlik açığının önem derecesi, kuruluşları riskini azaltmak için aldıkları.

### <a name="secure-identity"></a>(Kimlik) güvenli
Parola değiştirme veya riskli bir kimlik güvenliği aşılmamış bir duruma geri yüklemek için yeniden görüntüsünü oluşturuyor makine gibi düzeltme eylemi gerçekleştirin.

### <a name="security-policy"></a>Güvenlik ilkesi
İlke kuralları ve koşul koleksiyonu. Kullanıcılar, gruplar, uygulamalar, cihazları, cihaz platformları, cihaz durumları, IP aralıkları ve Auth2.0 istemci türleri gibi varlıklar için bir ilke uygulanabilir. Bir ilke etkinleştirildiğinde, bir kaynak için bir belirteç ilkesine dahil bir varlık verilen her değerlendirilir.

### <a name="sign-in-v"></a>(V'de) oturum açın
Azure Active Directory'de bir kimlik doğrulamaya.

### <a name="sign-in-n"></a>Oturum açma (n)
Azure Active Directory ve bu işlem yakalar olay bir kimlik doğrulama eylemi veya işlemi.

### <a name="sign-in-from-anonymous-ip-address"></a>Anonim IP adresinden oturum açın
Bir başarılı oturum açma anonim Ara sunucu IP adresi olarak tanımlanan IP adresinden sonra bir risk olayı tetiklenir.

### <a name="sign-in-from-infected-device"></a>Etkilenen aygıttan oturum aç
Bir oturum açma etkin bir şekilde bir bot sunucusu ile iletişim kurmak için çalıştığınız bir veya daha fazla güvenliği aşılmış cihazlara tarafından kullanılmak üzere bilinen bir IP adresi kaynaklanan harekete bir risk olayı.

### <a name="sign-in-from-ip-address-with-suspicious-activity"></a>Oturum IP adresinden kuşkulu etkinliği ile açma
Birden çok kullanıcı hesapları arasında kısa bir süre boyunca çok sayıda başarısız oturum açma denemesi bir başarılı oturum açma gelen bir IP adresi sonra risk olay tetiklenir.

### <a name="sign-in-from-unfamiliar-location"></a>Tanınmayan bir konumdan oturum aç
Kullanıcı başarıyla yeni bir konumdan (IP, enlem/boylam ve ASN) oturum açtığında tetiklenen bir risk olayı.

### <a name="sign-in-risk"></a>Oturum açma riski
Risk bkz düzeyi (oturum açma)

### <a name="sign-in-risk-policy"></a>Oturum açma riski ilkesi
Bir özel oturum açma riski değerlendirir ve önceden tanımlanmış koşullara ve kurallarına göre Azaltıcı Etkenler geçerli koşullu erişim ilkesi.

### <a name="user-compromise-risk"></a>Kullanıcı güvenlik aşılması riski
Risk bakın (kullanıcı güvenliğinin aşılması) düzeyi

### <a name="user-risk"></a>Kullanıcı riski
Risk bakın (kullanıcı güvenliğinin aşılması) düzeyi.

### <a name="user-risk-policy"></a>Kullanıcı riski ilkesi
Oturum açma göz önünde bulundurur ve önceden tanımlanmış koşullara ve kurallarına göre Azaltıcı geçerlidir koşullu erişim ilkesi.

### <a name="users-flagged-for-risk"></a>Riskli oldukları belirlenen kullanıcılar
Etkin ya da düzeltilen risk olaylarına sahip kullanıcılar

### <a name="vulnerability"></a>Güvenlik açığı
Bir yapılandırma veya Azure Active Directory'de dizin açıklarına maruz kalabilir kılan koşul veya tehditleri.

## <a name="see-also"></a>Ayrıca bkz.
* [Azure Active Directory kimlik koruması](active-directory-identityprotection.md)

