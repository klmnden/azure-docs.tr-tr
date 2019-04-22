---
title: Uygulama proxy'si aracı Bağlayıcısı'nı yüklerken sorunla | Microsoft Docs
description: Uygulama proxy'si aracı Bağlayıcısı'nı yükleme sırasında karşılaşacağınız sorunlarını giderme
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/21/2018
ms.author: celested
ms.reviewer: japere
ms.collection: M365-identity-device-management
ms.openlocfilehash: 87d828e596c42b786d1c54a7158e1151f374c560
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "58793048"
---
# <a name="problem-installing-the-application-proxy-agent-connector"></a>Uygulama Ara Sunucusu Aracı Bağlayıcısı’nı yüklerken sorun oluşuyor

Microsoft AAD Application Proxy Connector giden bağlantılar iç etki alanı bulut kullanılabilir uç noktasından bağlantısını kurmak için kullandığı bir iç etki alanı bileşendir.

## <a name="general-problem-areas-with-connector-installation"></a>Genel sorun alanlarından bağlayıcısını yükleme

Bir bağlayıcı yüklemesi başarısız olduğunda, kök nedeni genellikle aşağıdaki alanları biridir:

1.  **Bağlantı** – kaydetmek ve gelecekteki güven özelliklerini oluşturmak için yeni bağlayıcı gereksinimlerini yüklemenin tamamlanması. Bu, AAD uygulama proxy'sini bulut hizmetine bağlanarak yapılır.

2.  **Güven oluşturma** – yeni bir bağlayıcı otomatik olarak imzalanan bir sertifika oluşturur ve bulut hizmetine kaydeder.

3.  **Yönetici kimlik doğrulama** – yükleme sırasında kullanıcı Bağlayıcısı yüklemesini tamamlamak için yönetici kimlik bilgileri sağlamanız gerekir.

## <a name="verify-connectivity-to-the-cloud-application-proxy-service-and-microsoft-login-page"></a>Bulut uygulaması Ara Sunucusu hizmeti ve Microsoft Login sayfasına bağlantı doğrulayın

**Hedef:** Bağlayıcı makinesinde AAD uygulama proxy'sini kayıt uç noktası yanı sıra Microsoft oturum açma sayfasına bağlanabildiğini doğrulayın.

1.  Bir tarayıcı açın ve aşağıdaki web sayfasına gidin: <https://aadap-portcheck.connectorporttest.msappproxy.net> , Orta ABD ve Doğu ABD veri merkezleri ile bağlantı noktaları 80 ve 443 bağlantı çalıştığını doğrulayın.

2.  Bağlantı noktalarından birini değilse, başarılı (yeşil bir onay işareti yok), güvenlik duvarı veya arka uç proxy sahip olduğunu doğrulayın \*. msappproxy.net bağlantı noktaları 80 ve 443 doğru şekilde tanımlanmış.

3.  Bir tarayıcı (ayrı sekmesi) açın ve aşağıdaki web sayfasına gidin: <https://login.microsoftonline.com>, söz konusu sayfaya oturum açabileceğiniz emin olun.

## <a name="verify-machine-and-backend-components-support-for-application-proxy-trust-cert"></a>Uygulama Proxy güven sertifikası için makine ve arka uç bileşenlerine desteklediğini doğrulayın

**Hedef:** Bağlayıcı makinesinde, arka uç proxy ve Güvenlik Duvarı için gelecekteki güven bağlayıcı tarafından oluşturulan sertifikayı destekleyebilir doğrulayın.

>[!NOTE]
>Bağlayıcı, TLS1.2 tarafından desteklenen bir SHA512 sertifika oluşturmayı dener. Makine veya arka uç güvenlik duvarı ve proxy desteklemez, TLS1.2 yükleme başarısız.
>
>

**Bu sorunu çözmek için:**

1.  Makine TLS1.2 destekler – 2012 R2 sonra tüm Windows sürümlerinde TLS 1.2 desteklemelidir doğrulayın. Bağlayıcı makinenizi bir sürüm 2012 R2 veya önceki ise, aşağıdaki KB'leri makinede yüklü olduğundan emin olun: <https://support.microsoft.com/help/2973337/sha512-is-disabled-in-windows-when-you-use-tls-1.2>

2.  Ağ yöneticinizle iletişime geçerek arka uç proxy ve güvenlik duvarı SHA512 giden trafiğini engellemediğinden doğrulamak etmesini isteyin.

## <a name="verify-admin-is-used-to-install-the-connector"></a>Yönetici Bağlayıcısı'nı yüklemek için kullanılan doğrulayın

**Hedef:** Bağlayıcı yüklemeye çalıştığında kullanıcı doğru kimlik bilgilerine sahip bir yönetici olduğundan emin olun. Şu anda, kullanıcının en az bir uygulama Yöneticisi yükleme başarılı olması gerekir.

**Kimlik bilgilerinin doğru olup olmadığını doğrulamak için:**

Bağlanma <https://login.microsoftonline.com> ve aynı kimlik bilgilerini kullanın. Oturum açma başarılı olduğundan emin olun. Kullanıcı rolü giderek denetleyebilirsiniz **Azure Active Directory**  - &gt; **kullanıcılar ve gruplar**  - &gt; **tüm kullanıcılar**. 

Kullanıcı hesabınızın, ardından "dizin rolü" elde edilen menüsünde seçin. Seçili rolü "Uygulama Yöneticisi" olduğunu doğrulayın. Herhangi bir sayfasında bu adımları boyunca erişim bulamıyorsanız, gerekli rol yok.

## <a name="next-steps"></a>Sonraki adımlar
[Azure AD uygulama ara sunucusu bağlayıcıları anlama](application-proxy-connectors.md)
