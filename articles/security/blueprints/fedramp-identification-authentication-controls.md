---
title: "FedRAMP Azure şeması Otomasyonu - tanımlama ve kimlik doğrulama"
description: "Web uygulamalarında FedRAMP - tanımlama ve kimlik doğrulama"
services: security
documentationcenter: na
author: jomolesk
manager: mbaldwin
editor: tomsh
ms.assetid: 1033f63f-daf0-4174-a7f6-7b0f569020e2
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/15/2017
ms.author: jomolesk
ms.openlocfilehash: 9fa06a1b7e80a2d50553512a10f4425dc06ef692
ms.sourcegitcommit: 7d107bb9768b7f32ec5d93ae6ede40899cbaa894
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/16/2017
---
> [!NOTE]
> Bu denetimler NIST ve ABD tarafından tanımlanır Ticaret Bakanlığı NIST özel yayını 800-53 düzeltme 4 bir parçası olarak. NIST 800 53 düzeltme 4 yordamları ve yönergeler her denetim için test etme hakkında bilgi için lütfen bakın.
    
    

# <a name="identification-and-authentication-ia"></a>Tanımlama ve kimlik doğrulama (IA)

## <a name="nist-800-53-control-ia-1"></a>NIST 800 53 denetim IA-1

#### <a name="identification-and-authentication-policy-and-procedures"></a>Tanımlama ve kimlik doğrulama ilkesini ve yordamları

**IA-1** kuruluş geliştirir, belgeler ve için disseminates [atama: kuruluş tarafından tanımlanan personel ya da roller] adresleri amacı, kapsam, roller, sorumlulukları, yönetim bir tanımlama ve kimlik doğrulama İlkesi Taahhüt, Kurumsal varlıklar ve uyumluluk arasında koordinasyon; ve tanımlama ve kimlik doğrulama ilkesini ve ilişkili tanımlama ve kimlik doğrulama denetimleri uyarlamasını kolaylaştırmak için yordamlar; gözden geçirir ve geçerli tanımlama ve kimlik doğrulama İlkesi güncelleştirmeleri [atama: kuruluş tarafından tanımlanan sıklığı]; ve tanımlama ve kimlik doğrulama yordamları [atama: kuruluş tarafından tanımlanan sıklığı].

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri'nin kuruluş düzeyinde tanımlama ve kimlik doğrulama ilkesini ve yordamlar bu denetim gidermek yeterli olabilir. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-ia-2"></a>NIST 800 53 denetim IA-2

#### <a name="identification-and-authentication-organizational-users"></a>Tanımlama ve kimlik doğrulama (kuruluş kullanıcılar)

**IA-2** bilgi sistemi benzersiz olarak tanımlayan ve kuruluş kullanıcılar (veya kurumsal kullanıcılar adına hareket işlemleri) kimliğini doğrular.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Bu Azure şeması tarafından oluşturulan hesapların benzersiz tanımlayıcıları vardır. Benzersiz olmayan tanımlayıcılarına sahip yerleşik hesapları devre dışı veya kaldırılır. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-ia-2-1"></a>NIST 800 53 denetim IA-2 (1)

#### <a name="identification-and-authentication-organizational-users--network-access-to-privileged-accounts"></a>Tanımlama ve kimlik doğrulama (kuruluş kullanıcılar) | Ayrıcalıklı hesaplara ağ erişimi

**IA-2 (1)** ayrıcalıklı hesaplara ağ erişimi için çok faktörlü kimlik doğrulama bilgileri sistemi uygular.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri ayrıcalıklı hesaplara ağ erişimi için çok faktörlü kimlik doğrulamasını gerçekleştirmek için sorumlu değildir. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-ia-2-2"></a>NIST 800 53 denetim IA-2 (2)

#### <a name="identification-and-authentication-organizational-users--network-access-to-non-privileged-accounts"></a>Tanımlama ve kimlik doğrulama (kuruluş kullanıcılar) | Ayrıcalıklı olmayan hesaplar için ağ erişimi

**IA-2 (2)** ayrıcalıklı olmayan hesaplar için ağ erişimi için çok faktörlü kimlik doğrulama bilgileri sistemi uygular.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri ayrıcalıklı olmayan hesaplar için ağ erişimi için çok faktörlü kimlik doğrulamasını gerçekleştirmek için sorumlu değildir. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-ia-2-3"></a>NIST 800 53 denetim IA-2 (3)

#### <a name="identification-and-authentication-organizational-users--local-access-to-privileged-accounts"></a>Tanımlama ve kimlik doğrulama (kuruluş kullanıcılar) | Yerel ayrıcalıklı hesaplara erişim izni

**IA-2 (3)** yerel erişim ayrıcalıklı hesaplar için çok faktörlü kimlik doğrulama bilgileri sistemi uygular.

**Sorumlulukları:**`Azure Only`

|||
|---|---|
| **Müşteri** | Müşteri, Azure veri merkezlerinde tüm sistem kaynakları için yerel erişimi yoktur. |
| **Sağlayıcı (Microsoft Azure)** | Microsoft Azure fiziksel erişimi gerekli olmadığı sürece yerel erişim izin vermez. Yerel yönetici erişimi yalnızca burada üye sunucunun ağ sorunları yaşıyor ve etki alanı kimlik doğrulaması çalışmıyor durumlarda sorunları gidermek için kullanılmalıdır. <br /> Azure ortamına fiziksel erişim için gerekli erişim denetimi mekanizmaları aracılığıyla yerel erişimi için çok faktörlü kimlik doğrulamasını uygular. Tüm Azure altyapı sistemleri sistem sınırları içinde içeren Azure veri merkezleri içinde odaları şirket akıllı kart badging erişimi ve biyometrik cihazların gereksinimini de dahil olmak üzere çeşitli fiziksel güvenlik mekanizmaları ile kısıtlanır. Azure veri merkezi colocations Giriş noktasına fiziksel erişim için her iki forms kimlik doğrulaması gereklidir. |


 ### <a name="nist-800-53-control-ia-2-4"></a>NIST 800 53 denetim IA-2 (4)

#### <a name="identification-and-authentication-organizational-users--local-access-to-non-privileged-accounts"></a>Tanımlama ve kimlik doğrulama (kuruluş kullanıcılar) | Ayrıcalıklı olmayan hesaplar için yerel erişim

**IA-2 (4)** yerel erişim ayrıcalıklı olmayan hesaplar için çok faktörlü kimlik doğrulama bilgileri sistemi uygular.

**Sorumlulukları:**`Azure Only`

|||
|---|---|
| **Müşteri** | Müşteri, Azure veri merkezlerinde tüm sistem kaynakları için yerel erişimi yoktur. |
| **Sağlayıcı (Microsoft Azure)** | Microsoft Azure ayrıcalıklı olarak Microsoft Azure kamu personeli tarafından kullanılan tüm Microsoft Azure kamu hesapları göz önünde bulundurur. Çok faktörlü kimlik doğrulama kullanarak akıllı kart ve PIN, tüm Microsoft Azure kamu personel hesapları için yerel erişim içeren uygulanır. |


 ### <a name="nist-800-53-control-ia-2-5"></a>NIST 800 53 denetim IA-2 (5)

#### <a name="identification-and-authentication-organizational-users--group-authentication"></a>Tanımlama ve kimlik doğrulama (kuruluş kullanıcılar) | Grup kimlik doğrulaması

**IA-2 (5)** kuruluşun kişiler bir grup Doğrulayıcısı işe ile tek bir kimlik doğrulayıcı kimliğinin gerektirir.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Hiç paylaşılan grubu hesabı bu Azure şeması tarafından dağıtılan kaynak üzerinde etkindir. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-ia-2-8"></a>NIST 800 53 denetim IA-2 (8)

#### <a name="identification-and-authentication-organizational-users--network-access-to-privileged-accounts---replay-resistant"></a>Tanımlama ve kimlik doğrulama (kuruluş kullanıcılar) | Ağ erişimi ayrıcalıklı hesaplara - dirençli yeniden yürütme

**IA-2 (8)** bilgi sistemi yeniden yürütme dayanıklı kimlik doğrulama mekanizmaları ayrıcalıklı hesaplara ağ erişimi için uygular.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Bu Azure şeması tarafından dağıtılan kaynaklarına erişimi yeniden yürütme saldırılara karşı yerleşik Kerberos işlevselliğini Azure Active Directory, Active Directory ve Windows işletim sistemi tarafından korunur. Kerberos kimlik doğrulaması, istemci tarafından gönderilen Doğrulayıcı şifrelenmiş IP listesi, istemci zaman damgaları ve bilet yaşam süresi gibi ek veriler içerir. Bir paket tekrarlanır, zaman damgası denetlenir. Zaman damgası daha önceki ise veya önceki bir Doğrulayıcı, paket ile aynı reddedilir. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-ia-2-9"></a>NIST 800 53 denetim IA-2 (9)

#### <a name="identification-and-authentication-organizational-users--network-access-to-non-privileged-accounts---replay-resistant"></a>Tanımlama ve kimlik doğrulama (kuruluş kullanıcılar) | Ayrıcalıklı olmayan hesaplar - yeniden yürütme dirençli ağ erişimi

**IA-2 (9)** bilgi sistemi yeniden yürütme dayanıklı kimlik doğrulama mekanizmaları ayrıcalıklı olmayan hesaplar için ağ erişimi için uygular.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Bu Azure şeması tarafından dağıtılan kaynaklarına erişimi yeniden yürütme saldırılara karşı yerleşik Kerberos işlevselliğini Azure Active Directory, Active Directory ve Windows işletim sistemi tarafından korunur. Kerberos kimlik doğrulaması, istemci tarafından gönderilen Doğrulayıcı şifrelenmiş IP listesi, istemci zaman damgaları ve bilet yaşam süresi gibi ek veriler içerir. Bir paket tekrarlanır, zaman damgası denetlenir. Zaman damgası daha önceki ise veya önceki bir Doğrulayıcı, paket ile aynı reddedilir. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-ia-2-11"></a>NIST 800 53 denetim IA-2 (11)

#### <a name="identification-and-authentication-organizational-users--remote-access----separate-device"></a>Tanımlama ve kimlik doğrulama (kuruluş kullanıcılar) | Uzaktan erişim - birbirinden ayrı cihaz

**IA-2 (11)** ayrıcalıklı için uzaktan erişim için çok faktörlü kimlik doğrulama bilgileri sistem uygular ve ayrıcalıklı olmayan hesapları etkenlerden biri erişimini sisteminden ayrı bir aygıt tarafından sağlanır ve cihaz karşılayan [şekilde Atama: mekanizması gereksinimleri kuruluşunuz tarafından tanımlanan gücünü].

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri, müşteri tarafından dağıtılan kaynaklara uzaktan erişmek için çok faktörlü kimlik doğrulaması uygulamak üzere sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-ia-2-12"></a>NIST 800 53 denetim IA-2 (12)

#### <a name="identification-and-authentication-organizational-users--acceptance-of-piv-credentials"></a>Tanımlama ve kimlik doğrulama (kuruluş kullanıcılar) | PIV kimlik kabulü

**IA-2 (12)** bilgi sistemi kabul eder ve elektronik olarak kişisel kimlik doğrulama (PIV) kimlik bilgilerini doğrular.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri, kabul etme ve kişisel kimlik doğrulama (PIV) kimlik doğrulama sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-ia-3"></a>NIST 800 53 denetim IA-3

#### <a name="device-identification-and-authentication"></a>Aygıt tanımlama ve kimlik doğrulama

**IA-3** bilgi sistemi benzersiz olarak tanımlar ve kimlik doğrulaması [atama: kuruluş tarafından tanımlanan özel ve/veya cihaz türlerini] kurmadan önce bir [seçimi (bir veya daha fazla): yerel; uzaktan; ağ] bağlantısı.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri, cihaz kimliğini ve kimlik doğrulaması uygulamak üzere sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-ia-4a"></a>NIST 800 53 denetim IA-4.a

#### <a name="identifier-management"></a>Tanımlayıcı Yönetimi

**IA-4.a** onayı alma tarafından kuruluş bilgileri sistem tanımlayıcılarını yönetir [atama: kuruluş tarafından tanımlanan personel ya da roller] bir kişi, Grup, rol ya da cihaz tanımlayıcı atamak için.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri, müşteri kaynaklar için tanımlayıcıları (yani, kişiler, gruplar, roller ve aygıtlar) yönetilmesinden sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-ia-4b"></a>NIST 800 53 denetim IA-4.b

#### <a name="identifier-management"></a>Tanımlayıcı Yönetimi

**IA-4.b** bir kişinin tanımlayan bir tanımlayıcı seçerek bilgileri sistem tanımlayıcılarını kuruluş yönetir grup, rol ya da cihaz.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Bireysel hesaplar için müşteri tarafından belirtilen tanımlayıcıları dağıtımı sırasında bu Azure şeması ister.  |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-ia-4c"></a>NIST 800 53 denetim IA-4.c

#### <a name="identifier-management"></a>Tanımlayıcı Yönetimi

**IA-4.c** hedeflenen kişi, Grup, rol veya aygıt tanımlayıcısı atayarak kuruluş bilgileri sistem tanımlayıcılarını yönetir.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri, müşteri kaynaklar için tanımlayıcıları (yani, kişiler, gruplar, roller ve aygıtlar) yönetilmesinden sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-ia-4d"></a>NIST 800 53 denetim IA-4.d

#### <a name="identifier-management"></a>Tanımlayıcı Yönetimi

**IA-4.d** tanımlayıcıları kullanılmasını engelleyerek kuruluş bilgileri sistem tanımlayıcılarını yönetir [atama: kuruluş tanımlı süre].

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Active Directory ve yerel Windows işletim sistemi hesapları benzersiz güvenlik tanımlayıcısı (SID) atanır. Azure Active Directory hesaplarını genel olarak benzersiz bir nesne kimliği atanır. Bu benzersiz kimliklerinin yeniden tabi değildir. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-ia-4e"></a>NIST 800 53 denetim IA-4.e

#### <a name="identifier-management"></a>Tanımlayıcı Yönetimi

**IA-4.e** Tanımlayıcısının sonunda devre dışı bırakarak kuruluş bilgileri sistem tanımlayıcılarını yönetir [atama: kuruluş tanımlı süre etkinlik dışı kalma].

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Bu Azure şeması Active Directory hesapları kaldıktan 35 gün sonra otomatik olarak devre dışı bırakmak için zamanlanmış bir görev uygular. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-ia-4-4"></a>NIST 800 53 denetim IA-4 (4)

#### <a name="identifier-management--identify-user-status"></a>Tanımlayıcı Yönetimi | Kullanıcı durumu tanımlayın

**IA-4 (4)** her olarak tanıtan tarafından bireysel tanımlayıcıları kuruluş yönetir [atama: kuruluş tarafından tanımlanan karakteristiğini tek tek durum tanımlama].

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Azure Active Directory ve Active Directory desteği Yükleniciler, satıcılar ve bunların tanımlayıcıları uygulanan adlandırma kurallarını kullanarak diğer kullanıcı türleri belirten. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-ia-5a"></a>NIST 800 53 denetim IA-5.a

#### <a name="authenticator-management"></a>Doğrulayıcı Yönetimi

**IA-5.a** , ilk kimlik doğrulayıcı dağıtım, tek tek kimlik, Grup, rol veya doğrulayıcı alma aygıt bir parçası olarak doğrulayarak kuruluş bilgileri sistem Doğrulayıcı yönetir.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri, Doğrulayıcı yönetilmesinden sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-ia-5b"></a>NIST 800 53 denetim IA-5.b

#### <a name="authenticator-management"></a>Doğrulayıcı Yönetimi

**IA-5.b** kuruluşunuz tarafından tanımlanan doğrulayıcı için ilk kimlik doğrulayıcı içerik oluşturarak kuruluş bilgileri sistem Doğrulayıcı yönetir.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Bu Azure şeması tarafından oluşturulan hesaplar için tüm ilk kimlik doğrulayıcı içeriği IA-müşteri tarafından dağıtımı sırasında belirtildiğinde doğrulandı 5 (1) belirtilen gereksinimleri karşılar.  |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-ia-5c"></a>NIST 800 53 denetim IA-5.c

#### <a name="authenticator-management"></a>Doğrulayıcı Yönetimi

**IA-5.c** Doğrulayıcı mekanizması kullanım için yeterince güçlü sağlayarak kuruluş bilgileri sistem Doğrulayıcı yönetir.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Doğrulayıcı gücü FedRAMP gerektirdiği için bu Azure şeması karşılayan gereksinimleri tarafından kullanılır. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-ia-5d"></a>NIST 800 53 denetim IA-5.d

#### <a name="authenticator-management"></a>Doğrulayıcı Yönetimi

**IA-5.d** kuruluş bilgileri sistem Doğrulayıcı oluşturma ve ilk kimlik doğrulayıcı dağıtım, bozuk veya kayıp/riske Doğrulayıcı ve iptal etmek için yönetim yordamları uygulayarak yönetir. Doğrulayıcı.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri, Doğrulayıcı yönetilmesinden sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-ia-5e"></a>NIST 800 53 denetim IA-5.e

#### <a name="authenticator-management"></a>Doğrulayıcı Yönetimi

**IA-5.e** Kuruluş bilgilerini sistem yüklemeden önce Doğrulayıcı varsayılan içeriğini değiştirerek bilgileri sistem Doğrulayıcı yönetir.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Bu Azure şeması bileşenleri için tüm Doğrulayıcı varsayılanlarına değiştirildi. Doğrulayıcı, bu çözüm dağıtımı sırasında müşteri tanımlı. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-ia-5f"></a>NIST 800 53 denetim IA-5.f

#### <a name="authenticator-management"></a>Doğrulayıcı Yönetimi

**IA-5.f** minimum ve maksimum ömrü kısıtlamaları ve Doğrulayıcı yeniden kullanım koşulları oluşturarak kuruluş bilgileri sistem Doğrulayıcı yönetir.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri, Doğrulayıcı yönetilmesinden sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-ia-5g"></a>NIST 800 53 denetim IA-5.g

#### <a name="authenticator-management"></a>Doğrulayıcı Yönetimi

**IA-5.g** değiştirme/Doğrulayıcı yenileyerek kuruluş bilgileri sistem Doğrulayıcı yönetir [atama: kuruluş-süre Doğrulayıcı türü tarafından tanımlanan].

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Bu Azure şeması için tüm dağıtılan sanal makinelerin katıldığı bir etki alanı denetleyicisi dağıtır. Bir Grup İlkesi kurulmuş ve parola ömrü kısıtlamaları (60 gün) uygulamak için yapılandırılmış. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-ia-5h"></a>NIST 800 53 denetim IA-5.h

#### <a name="authenticator-management"></a>Doğrulayıcı Yönetimi

**IA-5.h** yetkisiz olarak ifşa ve değişiklik Doğrulayıcı içerik koruyarak kuruluş bilgileri sistem Doğrulayıcı yönetir.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Bu Azure şeması yetkisiz olarak ifşa ve değişikliği Doğrulayıcı içeriği korumak için anahtar kasası uygular. Aşağıdaki kimlik doğrulayan anahtar kasasında depolanan: Azure parolasını dağıtma hesabı, sanal makine yönetici parolası, SQL Server hizmet hesabı parolası. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-ia-5i"></a>NIST 800 53 denetim IA-5.i

#### <a name="authenticator-management"></a>Doğrulayıcı Yönetimi

**IA-5.i** yapılacak kişiler gerektirerek kuruluş bilgileri sistem Doğrulayıcı yönetir ve uygulama cihazlara sahip, belirli güvenlik Doğrulayıcı korumak için koruma sağlar.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Bu Azure şeması yetkisiz olarak ifşa ve değişikliği Doğrulayıcı içeriği korumak için anahtar kasası uygular. Aşağıdaki kimlik doğrulayan anahtar kasasında depolanan: Azure parolasını dağıtma hesabı, sanal makine yönetici parolası, SQL Server hizmet hesabı parolası. Anahtar kasası anahtarları ve gizli anahtarları (örneğin, kimlik doğrulaması anahtarları, depolama hesabı anahtarları, veri şifreleme anahtarları ve parolaları), donanım güvenlik modülleri (HSM'ler) tarafından korunan anahtarları kullanarak şifreler. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-ia-5j"></a>NIST 800 53 denetim IA-5.j

#### <a name="authenticator-management"></a>Doğrulayıcı Yönetimi

**IA-5.j** Doğrulayıcısı Grup/rol hesapları için bu üyelik değişiklikleri hesaplarını değiştirerek kuruluş bilgileri sistem Doğrulayıcı yönetir.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Hiç paylaşılan grubu hesabı bu Azure şeması tarafından dağıtılan kaynak üzerinde etkindir. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-ia-5-1a"></a>NIST 800 53 denetim IA-5 (1) bir

#### <a name="authenticator-management--password-based-authentication"></a>Doğrulayıcı Yönetimi | Parola tabanlı kimlik doğrulama

**IA-5 (1) bir** bilgi sistemi parola tabanlı kimlik doğrulama için en az parola karmaşıklığını zorlar [atama: büyük küçük harfe duyarlılığın, karakter numarası kuruluşunuz tarafından tanımlanan gereksinimleri karışık büyük harf, küçük harf harf, rakam ve özel karakterler, her tür için en düşük gereksinimleri dahil olmak üzere].

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Bu Azure şeması için tüm dağıtılan sanal makinelerin katıldığı bir etki alanı denetleyicisi dağıtır. Bir Grup İlkesi oluşturulmuş ve sanal makine yerel hesaplar ve AD hesaplar için parola karmaşıklığı gereksinimlerini zorunlu kılmak üzere yapılandırılmış.  |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-ia-5-1b"></a>NIST 800 53 denetim IA-5 (1) .b

#### <a name="authenticator-management--password-based-authentication"></a>Doğrulayıcı Yönetimi | Parola tabanlı kimlik doğrulama

**IA-5 (1) .b** parola tabanlı kimlik doğrulaması için bilgi sistemi zorlar en az şu sayıda değiştirilmiş karakter yeni parolalar oluşturulduğunda: [atama: kuruluş tanımlı sayı].

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri, müşteri tarafından dağıtılan kaynaklar içinde parola tabanlı kimlik doğrulaması kullanan sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-ia-5-1c"></a>NIST 800 53 denetim IA-5 (1) .c

#### <a name="authenticator-management--password-based-authentication"></a>Doğrulayıcı Yönetimi | Parola tabanlı kimlik doğrulama

**IA-5 (1) .c** parola tabanlı kimlik doğrulaması için bilgi sistemi depolar ve parolaları yalnızca şifreleme açısından korumalı iletir.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Azure Directory tüm parolaları depolanan ve aktarılan sırasında şifreleme açısından korunduğundan emin olmak için kullanılır. Active Directory tarafından ve dağıtılan Windows sanal makinelerde yerel olarak depolanan parolaları yerleşik güvenlik işlevselliğinin bir parçası otomatik olarak karma hale getirilir. Uzak Masaüstü kimlik doğrulama oturumları Doğrulayıcı aktarırken korunduklarından emin olmak için TLS kullanılarak güvenli hale getirilir. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-ia-5-1d"></a>NIST 800 53 denetim IA-5 (1) .d

#### <a name="authenticator-management--password-based-authentication"></a>Doğrulayıcı Yönetimi | Parola tabanlı kimlik doğrulama

**IA-5 (1) .d** parola tabanlı kimlik doğrulaması için bilgi sistemi zorlar parola minimum ve maksimum ömrü kısıtlamaları [atama: kuruluş tarafından tanımlanan numaraları etkin kalma süresi en az, etkin kalma süresi en fazla].

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Bu Azure şeması için tüm dağıtılan sanal makinelerin katıldığı bir etki alanı denetleyicisi dağıtır. Bir Grup İlkesi oluşturulur ve en küçük (1 gün) ve (60 gün) en fazla ömrü zorunlu parolaları kısıtlamaları zorunlu kılmak üzere yapılandırılmış ve yerel AD hesapları için kısıtlamalar. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-ia-5-1e"></a>NIST 800 53 denetim IA-5 (1) .e

#### <a name="authenticator-management--password-based-authentication"></a>Doğrulayıcı Yönetimi | Parola tabanlı kimlik doğrulama

**IA-5 (1) .e** Parola tabanlı kimlik doğrulaması için bilgi sistemi yasaklar için parolayı yeniden [atama: kuruluş tanımlı sayı] nesli.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Bu Azure şeması için tüm dağıtılan sanal makinelerin katıldığı bir etki alanı denetleyicisi dağıtır. Bir Grup İlkesi kurulmuş ve yeniden koşullarını (24 parolalar) yerel ve AD hesaplarının kısıtlamaları zorunlu kılmak üzere yapılandırılmış. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-ia-5-1f"></a>NIST 800 53 denetim IA-5 (1) .f

#### <a name="authenticator-management--password-based-authentication"></a>Doğrulayıcı Yönetimi | Parola tabanlı kimlik doğrulama

**IA-5 (1) .f** bilgi sistemi, parola tabanlı kimlik doğrulaması için geçici bir parola kullanmak için sistem oturum kalıcı bir parola için hemen bir değişiklikle sağlar.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Azure Active Directory bilgileri sistem denetim erişimi yönetmek için kullanılır. Her bir hesap başlangıçta oluşturulur veya geçici bir parola oluşturulan Azure Active Directory kullanıcı parolayı sonraki oturum açma sırasında değiştirmek gerektirecek şekilde kullanılmaz. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-ia-5-2a"></a>NIST 800 53 denetim IA-5 (2) bir

#### <a name="authenticator-management--pki-based-authentication"></a>Doğrulayıcı Yönetimi | PKI tabanlı kimlik doğrulaması

**IA-5 (2) bir** bilgi sistemi PKI tabanlı kimlik doğrulaması için sertifikaları oluşturma ve sertifika durum bilgilerinin denetimi dahil olmak üzere bir kabul edilen güven bağlayıcı için bir sertifika yolu doğrulama tarafından doğrular.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri, müşteri tarafından dağıtılan kaynaklar içinde PKI tabanlı kimlik doğrulaması kullanan sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-ia-5-2b"></a>NIST 800 53 denetim IA-5 (2) .b

#### <a name="authenticator-management--pki-based-authentication"></a>Doğrulayıcı Yönetimi | PKI tabanlı kimlik doğrulaması

**IA-5 (2) .b** bilgi sistemi PKI tabanlı kimlik doğrulamayı uygulayan için yetkili karşılık gelen özel anahtar erişimi.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri, müşteri tarafından dağıtılan kaynaklar içinde PKI tabanlı kimlik doğrulaması kullanan sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-ia-5-2c"></a>NIST 800 53 denetim IA-5 (2) .c

#### <a name="authenticator-management--pki-based-authentication"></a>Doğrulayıcı Yönetimi | PKI tabanlı kimlik doğrulaması

**IA-5 (2) .c** bilgi sistemi PKI tabanlı kimlik doğrulaması için tek tek hesabı veya grup için kimlik doğrulaması eşler.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri, müşteri tarafından dağıtılan kaynaklar içinde PKI tabanlı kimlik doğrulaması kullanan sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-ia-5-2d"></a>NIST 800 53 denetim IA-5 (2) .d

#### <a name="authenticator-management--pki-based-authentication"></a>Doğrulayıcı Yönetimi | PKI tabanlı kimlik doğrulaması

**IA-5 (2) .d** PKI tabanlı kimlik doğrulama için bilgi sistemi, yerel bir yol keşfi ve ağ üzerinden iptal bilgilerini erişememe durumu durumunda doğrulama desteklemek üzere iptal veri önbelleği uygular.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri, müşteri tarafından dağıtılan kaynaklar içinde PKI tabanlı kimlik doğrulaması kullanan sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-ia-5-3"></a>NIST 800 53 denetim IA-5 (3)

#### <a name="authenticator-management--in-person-or-trusted-third-party-registration"></a>Doğrulayıcı Yönetimi | yüz yüze veya güvenilen bir üçüncü taraf kayıt

**IA-5 (3)** kayıt almak için işlem kuruluşunuza [atama: kuruluş tarafından tanımlanan türlerini ve/veya belirli kimlik doğrulayan] gerçekleştirilen [Seçimi: Kişisel; güvenilen bir üçüncü taraf tarafından] önce [atama: Kuruluşunuz tarafından tanımlanan kayıt yetkilisi] göre yetkilendirme ile [atama: kuruluş tarafından tanımlanan personel ya da roller].

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri, Doğrulayıcı kayıt için sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-ia-5-4"></a>NIST 800 53 denetim IA-5 (4)

#### <a name="authenticator-management--automated-support--for-password-strength-determination"></a>Doğrulayıcı Yönetimi | Otomatik parola gücünü belirleme desteği

**IA-5 (4)** kuruluş Parola doğrulayıcı karşılamak için yeterince güçlü olup olmadığını belirlemek için otomatik araçları kullanır [atama: kuruluş tarafından tanımlanan gereksinimleri].

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Kullanıcı hesaplarını bu Azure şeması ile dağıtılan AD ekleyin ve yerel kullanıcı hesapları. Bunların her ikisi de uyumluluk ilk bir parola oluşturmak için oluşturulan parola gereksinimleri ve parola değişiklikleri sırasında zorla mekanizmaları sağlar. Azure Active Directory Parola doğrulayıcı parola uzunluğu, karmaşıklık, döndürme ve yaşam süresi kısıtlamaları IA-5'te (1) oluşturulmuş karşılamak için yeterince güçlü olup olmadığını belirlemek için işe otomatik bir araçtır. Azure Active Directory parola kimlik doğrulayıcı gücünü oluşturulurken bu standartları karşılar sağlar. Bu çözümü dağıtmak için kullanılan müşteri belirtilen parolalar, parola gücü gereksinimlerini karşılamak için denetlenir. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-ia-5-6"></a>NIST 800 53 denetim IA-5 (6)

#### <a name="authenticator-management--protection-of-authenticators"></a>Doğrulayıcı Yönetimi | Doğrulayıcı koruma

**IA-5 (6)** kuruluş Doğrulayıcı Doğrulayıcı kullanımını verir erişim bilgileri güvenlik kategorisiyle orantılı korur.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri, Doğrulayıcı korumak için sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-ia-5-7"></a>NIST 800 53 denetim IA-5 (7)

#### <a name="authenticator-management--no-embedded-unencrypted-static-authenticators"></a>Doğrulayıcı Yönetimi | Katıştırılmış şifrelenmemiş statik Doğrulayıcı

**IA-5 (7)** kuruluş şifrelenmemiş statik Doğrulayıcı olmayan uygulamaları veya erişim betikleri katıştırılmış veya işlev tuşlarını üzerinde depolanan olduğunu sağlar.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Şifrelenmemiş statik Doğrulayıcı hiçbir kullanımını yoktur uygulamalarda katıştırılmış, erişim komut dosyaları veya işlev bu Azure şeması tarafından dağıtılan anahtarları. Herhangi bir komut dosyası veya bir kimlik doğrulayıcı kullanan uygulamayı bir Azure anahtar kasası kapsayıcısına her kullanım önce bir çağrı yapar. Azure anahtar kasası kapsayıcı karşılık gelen çağrıyı olmadan bir sisteme erişmek için bir hizmet hesabı kullanılıyorsa bu Yasak ihlalleri algılanması sağlayan Azure anahtar kasası kapsayıcıları erişimi denetlenir. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-ia-5-8"></a>NIST 800 53 denetim IA-5 (8)

#### <a name="authenticator-management--multiple-information-system-accounts"></a>Doğrulayıcı Yönetimi | Birden çok bilgi sistem hesabı

**IA-5 (8)** kuruluş Implements [atama: kuruluş tanımlanan güvenlik önlemlerinin] birden çok bilgi systems hesaplarına sahip kişiler nedeniyle güvenliğinin aşılmasına riskini yönetmek için.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri hesapları birden çok sistem üzerinde sahip bireyler ile ilişkili riskleri yönetmek için kurumsal düzeyde güvenlik önlemleri bağlı. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-ia-5-11"></a>NIST 800 53 denetim IA-5 (11)

#### <a name="authenticator-management--hardware-token-based-authentication"></a>Doğrulayıcı Yönetimi | Donanım belirteç tabanlı kimlik doğrulaması

**IA-5 (11)** donanım belirteç tabanlı kimlik doğrulaması, bilgi sistemi karşılamak mekanizmaları kullanan [atama: kuruluş tarafından tanımlanan belirteci kalitesi gereksinimleri].

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri için kullanan mekanizmaları donanım belirteç tabanlı kimlik doğrulaması kalite gereksinimlerini karşılamak sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-ia-5-13"></a>NIST 800 53 denetim IA-5 (13)

#### <a name="authenticator-management--expiration-of-cached-authenticators"></a>Doğrulayıcı Yönetimi | Önbelleğe alınmış kimlik doğrulayan sona erme tarihi

**IA-5 (13)** bilgi sistemi sonra önbelleğe alınmış kimlik doğrulayan kullanımını yasaklar [atama: kuruluş tanımlı süre].

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Bu Azure şeması tarafından dağıtılan kaynak yok önbelleğe alınmış kimlik doğrulayan kullanılmasına izin verecek şekilde yapılandırılmıştır. Bir kimlik doğrulayıcı kimlik doğrulama aynı anda girilir dağıtılan sanal makineler için kimlik doğrulaması gerektirir. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-ia-6"></a>NIST 800 53 denetim IA-6

#### <a name="authenticator-feedback"></a>Doğrulayıcı geri bildirim

**IA-6** bilgi sistemi geri bildirim kullanımdan olası kötüye kullanımlara/yetkisiz kişiler tarafından bilgileri korumak için kimlik doğrulama işlemi sırasında kimlik bilgilerinin gizler.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Bu Azure şeması tarafından dağıtılan kaynaklara erişimi Uzak Masaüstü ve Windows kimlik doğrulaması kullanır. Windows kimlik doğrulama oturumları varsayılan davranışını ne zaman bir kimlik doğrulama oturumu sırasında giriş parolaları maskeleri.  |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-ia-7"></a>NIST 800 53 denetim IA-7

#### <a name="cryptographic-module-authentication"></a>Şifreleme modülü kimlik doğrulama

**IA-7** bilgi sistemi şifreleme modülü için kimlik doğrulaması için geçerli federal yasalar, Executive siparişler, yönergeleri, ilkeleri, düzenlemeleri, standartlar ve gibi yönelik yönergeler gereksinimlerini karşılamak mekanizmaları uygular kimlik doğrulaması.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Windows kimlik doğrulaması, Uzak Masaüstü'nü ve BitLocker tarafından bu Azure şeması görevli olduğu. Bu bileşenler, FIPS 140 doğrulanmış şifreleme modüllerinde yararlanmayı yapılandırılabilir. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ## <a name="nist-800-53-control-ia-8"></a>NIST 800 53 denetim IA-8

#### <a name="identification-and-authentication-non-organizational-users"></a>Tanımlama ve kimlik doğrulama (kuruluş dışı kullanıcılar)

**IA-8** bilgi sistemi benzersiz olarak tanımlayan ve kuruluş geneli kullanıcılar (veya Kurumsal olmayan kullanıcılar adına hareket işlemleri) kimliğini doğrular.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri tanımlama ve müşteri tarafından dağıtılan kaynaklara erişme kuruluş olmayan kullanıcıların kimlik doğrulaması sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-ia-8-1"></a>NIST 800 53 denetim IA-8 (1)

#### <a name="identification-and-authentication-non-organizational-users--acceptance-of-piv-credentials-from-other-agencies"></a>Tanımlama ve kimlik doğrulama (kuruluş dışı kullanıcılar) | Diğer kurumları PIV kimlik bilgilerini kabulü

**IA-8 (1)** bilgi sistemi kabul eder ve elektronik olarak federal diğer kurumları kişisel kimlik doğrulama (PIV) kimlik bilgilerini doğrular.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri, kabul etme ve federal diğer kurumları tarafından verilmiş kişisel kimlik doğrulama (PIV) kimlik doğrulama sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-ia-8-2"></a>NIST 800 53 denetim IA-8 (2)

#### <a name="identification-and-authentication-non-organizational-users--acceptance-of-third-party-credentials"></a>Tanımlama ve kimlik doğrulama (kuruluş dışı kullanıcılar) | Üçüncü taraf kimlik kabulü

**IA-8 (2)** bilgi sistemi yalnızca FICAM onaylı üçüncü taraf kimlik bilgileri kabul eder.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri, yalnızca Federal Kimlik, kimlik bilgisi ve erişim yönetimi (FICAM) güven Framework çözümleri Initiative tarafından onaylanan üçüncü taraf kimlik bilgileri kabul etmek için sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-ia-8-3"></a>NIST 800 53 denetim IA-8 (3)

#### <a name="identification-and-authentication-non-organizational-users--use-of-ficam-approved-products"></a>Tanımlama ve kimlik doğrulama (kuruluş dışı kullanıcılar) | Ficam onaylı ürünlerin kullanın

**IA-8 (3)** kuruluş yalnızca FICAM onaylı bilgileri sistem bileşenlerinde kullanır [atama: kuruluş tarafından tanımlanan bilgileri sistemleri] üçüncü taraf kimlik bilgileri kabul etmek için.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri yalnızca Federal Kimlik, kimlik bilgileri, kullanan sorumludur ve üçüncü taraf kimlik bilgileri kabul etmek için kaynaklara erişim yönetimi (FICAM) güven Framework çözümleri Initiative onaylanmalıdır. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |


 ### <a name="nist-800-53-control-ia-8-4"></a>NIST 800 53 denetim IA-8 (4)

#### <a name="identification-and-authentication-non-organizational-users--use-of-ficam-issued-profiles"></a>Tanımlama ve kimlik doğrulama (kuruluş dışı kullanıcılar) | Ficam verilen profilleri kullanımı

**IA-8 (4)** FICAM verilen profillere bilgi sistemi uyumlu.

**Sorumlulukları:**`Customer Only`

|||
|---|---|
| **Müşteri** | Müşteri, Federal Kimlik, kimlik bilgisi ve erişim yönetimi (FICAM) güven Framework çözümleri Initiative tarafından verilen profilleri uyumludur için sorumludur. |
| **Sağlayıcı (Microsoft Azure)** | Uygulanamaz |
