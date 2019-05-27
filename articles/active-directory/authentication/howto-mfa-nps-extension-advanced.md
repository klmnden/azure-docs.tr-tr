---
title: -Azure Active Directory'de Azure MFA NPS uzantısı yapılandırma
description: NPS uzantısını yükledikten sonra IP beyaz listesi ve UPN değiştirme gibi gelişmiş yapılandırma için aşağıdaki adımları kullanın.
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: b8ac0497b13dad6795e8dc7ffaf761fe887a9953
ms.sourcegitcommit: cfbc8db6a3e3744062a533803e664ccee19f6d63
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65988619"
---
# <a name="advanced-configuration-options-for-the-nps-extension-for-multi-factor-authentication"></a>Multi-Factor Authentication için NPS uzantısı için Gelişmiş yapılandırma seçenekleri

Ağ İlkesi Sunucusu (NPS) uzantısı, şirket içi altyapınızı, bulut tabanlı Azure multi-Factor Authentication özellikleriyle genişletir. Yüklü uzantı zaten yüklü ve artık gereksinimleri uzantısı özelleştirmek nasıl öğrenmek istiyorsanız bu makalede varsayılır. 

## <a name="alternate-login-id"></a>Alternatif oturum açma kimliği

NPS uzantısı, şirket içi ve bulut için bağlandığında bu yana dizinleri, şirket içi kullanıcı asıl adlarından (UPN) bulutta adları eşleşmiyor nerede sorun çalıştırdığınızca. Bu sorunu çözmek için alternatif bir oturum açma kimlikleri kullanın. 

NPS uzantısı, Azure multi-Factor Authentication için UPN yerine kullanılacak Active Directory öznitelik belirleyebilirsiniz. Bu, şirket içi UPN değiştirmeden iki aşamalı doğrulama kullanarak şirket içi kaynaklarınızı korumanıza olanak tanır. 

Alternatif oturum açma kimliklerini yapılandırmak için Git `HKLM\SOFTWARE\Microsoft\AzureMfa` ve aşağıdaki kayıt defteri değerlerini düzenleyin:

| Ad | Tür | Varsayılan değer | Açıklama |
| ---- | ---- | ------------- | ----------- |
| LDAP_ALTERNATE_LOGINID_ATTRIBUTE | string | Boş | UPN yerine kullanmak istediğiniz Active Directory öznitelik adı belirleyin. Bu öznitelik AlternateLoginId özniteliği olarak kullanılır. Bu kayıt defteri değeri ayarlanırsa bir [geçerli Active Directory öznitelik](https://msdn.microsoft.com/library/ms675090.aspx) (örneğin, e-posta ya da displayName için), ardından özniteliğin değeri yerine kullanıcının UPN kimlik doğrulaması için kullanılır. Bu kayıt defteri değerini veya boşsa, yapılandırılmış, ardından AlternateLoginId devre dışı bırakıldı ve kullanıcının UPN kimlik doğrulaması için kullanılır. |
| LDAP_FORCE_GLOBAL_CATALOG | boole | False | Genel katalog LDAP aramaları için kullanımını AlternateLoginId aranırken zorlamak için bu bayrağı kullanın. Bir etki alanı denetleyicisi genel katalog olarak yapılandırın, genel kataloğa AlternateLoginId özniteliği ekleyin ve bu bayrağı etkinleştirin. <br><br> LDAP_LOOKUP_FORESTS (boş değilse), yapılandırılmışsa **bu bayrağı true zorlanır**kayıt defteri ayarının değeri ne olursa olsun. Bu durumda, her orman için AlternateLoginId özniteliği ile yapılandırılması genel katalog NPS uzantısı gerektirir. |
| LDAP_LOOKUP_FORESTS | string | Boş | Aranacak ormanları noktalı virgülle ayrılmış listesini sağlayın. Örneğin, *contoso.com;foobar.com*. Bu kayıt defteri değeri yapılandırıldıysa, NPS uzantısı tüm ormanlardaki sırası, bunlar listelenen ve ilk başarılı AlternateLoginId değeri döndürür yinelemeli olarak arar. Bu kayıt defteri değerini yapılandırmazsanız, geçerli etki alanına AlternateLoginId arama sınırlıdır.|

Alternatif oturum açma kimlikleri ile ilgili sorunları gidermek için önerilen adımları kullanır [alternatif oturum açma kimliği hataları](howto-mfa-nps-extension-errors.md#alternate-login-id-errors).

## <a name="ip-exceptions"></a>IP özel durumları

Sunucu kullanılabilirliği, yük Dengeleyiciler hangi sunucuların iş yükleri, göndermeden önce çalıştığını doğrulayın, gibi izlemeniz gerekiyorsa bu denetimler, doğrulama isteklerinin tarafından engellenmesi istemezsiniz. Bunun yerine, bildiğiniz hizmet hesapları tarafından kullanılan IP adreslerinden oluşan bir liste oluşturun ve söz konusu liste için çok faktörlü kimlik doğrulaması gereksinimleri devre dışı bırakın.

İzin verilen listesi IP yapılandırmak için Git `HKLM\SOFTWARE\Microsoft\AzureMfa` ve aşağıdaki kayıt defteri değeri yapılandırın:

| Ad | Tür | Varsayılan değer | Açıklama |
| ---- | ---- | ------------- | ----------- |
| IP_WHITELIST | string | Boş | IP adresleri noktalı virgülle ayrılmış listesini sağlayın. Burada hizmet istekleri, NAS/VPN sunucusu gibi kaynaklanan makinelerin IP adreslerini içerir. IP aralıkları ve alt ağlar desteklenmiyor. <br><br> Örneğin, *10.0.0.1;10.0.0.2;10.0.0.3*.

Ne zaman bir istek geldiğinde, var olan bir IP adresinden `IP_WHITELIST`, iki aşamalı doğrulama atlandı. IP listesi, sağlanan IP adresi karşılaştırılan *ratNASIPAddress* RADIUS isteğini özniteliği. Bir RADIUS isteği ratNASIPAddress özniteliği olmadan geliyorsa, aşağıdaki uyarı kaydedilir: "Kaynak IP, RADIUS isteğini NasIpAddress özniteliği eksik olduğundan P_WHITE_LIST_WARNING::IP beyaz liste yok sayılıyor."

## <a name="next-steps"></a>Sonraki adımlar

[Azure Multi-Factor Authentication için NPS uzantısından alınan hata iletilerini çözme](howto-mfa-nps-extension-errors.md)
