---
title: Azure MFA NPS uzantısını yapılandırmak | Microsoft Docs
description: NPS uzantısı yüklendikten sonra IP uygulamaları güvenilir listeye almayı ve UPN değiştirme gibi gelişmiş yapılandırma için aşağıdaki adımları kullanın.
services: multi-factor-authentication
ms.service: active-directory
ms.component: authentication
ms.topic: article
ms.date: 07/14/2017
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: richagi
ms.openlocfilehash: 1aa474424823a180a16206ac509053a93c7bfa18
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2018
---
# <a name="advanced-configuration-options-for-the-nps-extension-for-multi-factor-authentication"></a>Çok faktörlü kimlik doğrulaması için NPS uzantısı Gelişmiş yapılandırma seçenekleri

Ağ İlkesi Sunucusu (NPS) uzantısı, şirket içi altyapınızı, bulut tabanlı Azure çok faktörlü kimlik doğrulama özelliklerini genişletir. Bu makale, zaten yüklü uzantısına sahiptir ve şimdi gereksinimlerini uzantısı özelleştirmek nasıl öğrenmek istiyorsanız varsayar. 

## <a name="alternate-login-id"></a>Alternatif oturum açma kimliği

NPS uzantısı, şirket içi ve bulut için bağlayan bu yana dizinleri, burada, şirket içi kullanıcı asıl adları (UPN'ler) eşleşmiyor bulut adlarında sorunu karşılaşabileceğiniz. Bu sorunu çözmek için alternatif oturum açma kimliklerini kullanın. 

NPS uzantısı içinde UPN yerine Azure çok faktörlü kimlik doğrulaması için kullanılmak üzere Active Directory öznitelik belirleyebilirsiniz. Bu, şirket içi UPN değiştirmeden iki aşamalı doğrulama ile şirket içi kaynaklarınızı korumanıza olanak sağlar. 

Alternatif oturum açma kimliklerini yapılandırmak için şu adrese gidin `HKLM\SOFTWARE\Microsoft\AzureMfa` ve aşağıdaki kayıt defteri değerlerini düzenleyin:

| Ad | Tür | Varsayılan değer | Açıklama |
| ---- | ---- | ------------- | ----------- |
| LDAP_ALTERNATE_LOGINID_ATTRIBUTE | string | Boş | UPN yerine kullanmak istediğiniz Active Directory öznitelik adı belirtin. Bu öznitelik AlternateLoginId özniteliği olarak kullanılır. Bu kayıt defteri değeri ayarlanırsa bir [geçerli Active Directory özniteliğini](https://msdn.microsoft.com/library/ms675090.aspx) (örneğin, posta ya da displayName için), ardından özniteliğinin değeri yerine kullanıcının UPN kimlik doğrulaması için kullanılır. Bu kayıt defteri değerini veya boşsa, yapılandırıldıktan sonra AlternateLoginId devre dışıdır ve kullanıcının UPN kimlik doğrulaması için kullanılır. |
| LDAP_FORCE_GLOBAL_CATALOG | boole | False | Genel katalog LDAP aramaları için kullanımı AlternateLoginId bakarken zorlamak için bu bayrağı kullanın. Bir etki alanı denetleyicisi genel katalog olarak yapılandırın, genel kataloğa AlternateLoginId özniteliği ekleyin ve ardından bu bayrağı etkinleştirin. <br><br> LDAP_LOOKUP_FORESTS (boş değilse), yapılandırılmışsa, **bu bayrağı true zorlanır**ne olursa olsun, kayıt defteri ayarının değeri. Bu durumda, her orman için AlternateLoginId özniteliği ile yapılandırılması genel katalog NPS uzantısı gerekir. |
| LDAP_LOOKUP_FORESTS | string | Boş | Aranacak ormanlar noktalı virgülle ayrılmış listesini sağlayın. Örneğin, *contoso.com;foobar.com*. Bu kayıt defteri değeri yapılandırıldıysa, NPS uzantısı tüm ormanlardaki sipariş, bunlar listelenen ve ilk başarılı AlternateLoginId değeri döndürür tekrarlayarak arar. Bu kayıt defteri değeri yapılandırılmamışsa, AlternateLoginId arama için geçerli etki alanı sınırlıysa.|

Alternatif oturum açma kimliklerini ile ilgili sorunları gidermek için önerilen adımları kullanın [alternatif oturum açma kimliği hataları](howto-mfa-nps-extension-errors.md#alternate-login-id-errors).

## <a name="ip-exceptions"></a>IP özel durumlar

Sunucu kullanılabilirliği, yük dengeleyici iş yükleri, göndermeden önce hangi sunucuların çalıştıran doğrulayın, gibi izlemeniz gerekiyorsa bu denetimleri doğrulama isteklerini tarafından engellenmesi istemezsiniz. Bunun yerine, hizmet hesapları tarafından kullanılan bildiğiniz IP adresleri listesi oluşturun ve bu liste çok faktörlü kimlik doğrulama gereksinimleri devre dışı. 

Bir IP beyaz listesi yapılandırmak için şu adrese gidin `HKLM\SOFTWARE\Microsoft\AzureMfa` ve aşağıdaki kayıt defteri değerini yapılandırın: 

| Ad | Tür | Varsayılan değer | Açıklama |
| ---- | ---- | ------------- | ----------- |
| IP_WHITELIST | string | Boş | IP adreslerinin noktalı virgülle ayrılmış listesini sağlayın. Burada hizmet istekleri, NAS/VPN sunucusu gibi kaynaklanan makinelerin IP adreslerini içerir. IP aralıkları, alt ağlar desteklenmez:. <br><br> Örneğin, *10.0.0.1;10.0.0.2;10.0.0.3*.

Bir istek beyaz mevcut bir IP adresinden geldiğinde, iki aşamalı doğrulama atlandı. IP beyaz listesi içinde sağlanan IP adresi karşılaştırılır *ratNASIPAddress* RADIUS isteğini özniteliğidir. Bir RADIUS isteğini ratNASIPAddress özniteliği olmadan geliyorsa, aşağıdaki uyarı kaydedilir: "P_WHITE_LIST_WARNING::IP beyaz liste yoksayılır kaynak IP RADIUS isteğini NasIpAddress öznitelik yok."

## <a name="next-steps"></a>Sonraki adımlar

[Azure Multi-Factor Authentication için NPS uzantısından alınan hata iletilerini çözme](howto-mfa-nps-extension-errors.md)