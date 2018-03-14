---
title: "Uygulama Proxy Aracısı Bağlayıcısı yükleme sorunu | Microsoft Docs"
description: "Uygulama Proxy Aracısı Bağlayıcısı'nı yüklerken yüz sorunlarını giderme"
services: active-directory
documentationcenter: 
author: ajamess
manager: mtillman
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: 8fbd707b6708661ab0d655afadff2b18694a981e
ms.sourcegitcommit: a0be2dc237d30b7f79914e8adfb85299571374ec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2018
---
# <a name="problem-installing-the-application-proxy-agent-connector"></a>Uygulama Proxy Aracısı Bağlayıcısı yükleme sorunu

Microsoft AAD Application Proxy Connector giden bağlantılar iç etki alanının bulut kullanılabilir uç nokta bağlantısını kurmak için kullandığı bir iç etki alanının bileşendir.

## <a name="general-problem-areas-with-connector-installation"></a>Genel sorun alanlarından bağlayıcısını yükleme

Bir bağlayıcı yüklemesi başarısız olduğunda, kök nedeni genellikle aşağıdaki alanlarda biridir:

1.  **Bağlantı** – kaydetmek ve gelecekteki güven özelliklerini oluşturmak için yeni bağlayıcı gereksinimlerini başarılı bir yüklemeyi tamamlamak için. Bu, AAD uygulama proxy'si bulut hizmetine bağlanarak yapılır.

2.  **Güven oluşturma** – yeni bir bağlayıcı otomatik olarak imzalanan bir sertifika oluşturur ve bulut hizmetine kaydeder.

3.  **Yönetici kimlik doğrulaması** – yükleme sırasında kullanıcı Bağlayıcısı yüklemesini tamamlamak için yönetici kimlik bilgilerini sağlamanız gerekir.

## <a name="verify-connectivity-to-the-cloud-application-proxy-service-and-microsoft-login-page"></a>Bulut uygulama proxy'si hizmeti ve Microsoft Login sayfası bağlanabildiğini doğrulayın

**Hedef:** AAD uygulama proxy'si kayıt uç noktasını yanı sıra Microsoft oturum açma sayfasına bağlayıcı makineye bağlanabildiğinizi doğrulayın.

1.  Bir tarayıcı açın ve aşağıdaki web sayfasına gidin: <https://aadap-portcheck.connectorporttest.msappproxy.net> , 80 ve 443 numaralı bağlantı noktalarını ile orta ABD ve Doğu ABD veri merkezlerine bağlantısının çalışır durumda olduğunu doğrulayın.

2.  Bu bağlantı noktalarının hiçbirinde yoksa başarılı (yeşil bir onay yok), güvenlik duvarı veya arka uç proxy sahip olduğunu doğrulayın \*. msappproxy.net bağlantı noktaları 80 ve 443 doğru tanımlanmadı.

3.  Bir tarayıcı (ayrı sekmesi) açın ve aşağıdaki web sayfasına gidin: <https://login.microsoftonline.com>, bu sayfaya oturum açabileceğiniz emin olun.

## <a name="verify-machine-and-backend-components-support-for-application-proxy-trust-cert"></a>Makine ve arka uç bileşenleri için uygulama proxy'si güven sertifikası desteklediğini doğrulayın

**Hedef:** bağlayıcı makine, arka uç proxy ve güvenlik duvarı gelecekteki güven Bağlayıcısı tarafından oluşturulan sertifikayı destekleyebilir doğrulayın.

>[!NOTE]
>Bağlayıcı TLS1.2 tarafından desteklenen bir SHA512 sertifika oluşturmayı dener. Makine veya arka uç güvenlik duvarı ve proxy desteklemez TLS1.2 yükleme başarısız.
>
>

**Bu sorunu çözmek için:**

1.  Makine TLS1.2 destekleyen – 2012 R2 sonra tüm Windows sürümlerinde TLS 1.2 desteklemelidir doğrulayın. Bağlayıcı makinenizi 2012 R2 sürümü ya da önceki ise, aşağıdaki KB makinede yüklü olduğundan emin olun: <https://support.microsoft.com/help/2973337/sha512-is-disabled-in-windows-when-you-use-tls-1.2>

2.  Ağ yöneticinize başvurun ve arka uç proxy ve güvenlik duvarı SHA512 giden trafiği için engellemez doğrulamak isteyin.

## <a name="verify-admin-is-used-to-install-the-connector"></a>Yönetici Bağlayıcısı'nı yüklemek için kullanılan doğrulayın

**Hedef:** bağlayıcı yüklemeye çalıştığında kullanıcı doğru kimlik bilgileriyle bir yönetici olduğundan emin olun. Şu anda, kullanıcının başarılı olması için yükleme için genel yönetici olması gerekir.

**Kimlik bilgilerinin doğru olduğunu doğrulamak için:**

Bağlanmak <https://login.microsoftonline.com> ve aynı kimlik bilgilerini kullanın. Oturum açma başarılı olduğundan emin olun. Kullanıcı rolü giderek denetleyebilirsiniz **Azure Active Directory**  - &gt; **kullanıcılar ve gruplar**  - &gt; **tüm kullanıcılar**. 

Kullanıcı hesabınız, ardından "dizin rolü" elde edilen menüsünden seçin. Seçili rol "Genel yönetici" olduğunu doğrulayın. Adımları boyunca sayfalar erişemiyor varsa, genel bir yönetici değilsiniz.

## <a name="next-steps"></a>Sonraki adımlar
[Azure AD uygulama proxy'si bağlayıcılar anlama](application-proxy-understand-connectors.md)
