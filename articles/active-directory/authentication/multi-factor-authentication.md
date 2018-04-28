---
title: İki aşamalı doğrulama Azure MFA hakkında bilgi edinin | Microsoft Docs
description: 'Azure multi-Factor Authentication nedir, neden MFA, çok faktörlü kimlik doğrulaması istemci farklı yöntemler ve kullanılabilir sürümlerini hakkında daha fazla bilgi kullanın. '
keywords: mfa nedir MFA, mfa genel bakış, giriş
services: multi-factor-authentication
documentationcenter: ''
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: richagi
ms.assetid: c40d7a34-1274-4496-96b0-784850c06e9b
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/03/2017
ms.author: joflore
ms.openlocfilehash: 74441b5053a9c7c25d94406293828fe92f210bee
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2018
---
# <a name="what-is-azure-multi-factor-authentication"></a>Azure Multi-Factor Authentication nedir?
İki aşamalı doğrulama, birden fazla doğrulama yöntemi gerektiren ve kullanıcı oturum açmalarına ve işlemlerine önemli bir ikinci güvenlik katmanı ekleyen kimlik doğrulama yöntemidir. Her iki veya daha fazla aşağıdaki doğrulama yöntemlerini isteyerek çalışır:

* (Genellikle parola) bildiğiniz bir şey
* (Kolayca, bir telefon gibi yineleniyor değil güvenilir bir cihaz) sahip olduğunuz şey
* (Biyometri) olan bir şey

<center>![Kullanıcı adı ve parola](./media/multi-factor-authentication/pword.png) &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; ![sertifikaları](./media/multi-factor-authentication/phone.png) &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; ![Akıllı Telefon](./media/multi-factor-authentication/hware.png) &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; ![akıllı kart](./media/multi-factor-authentication/smart.png) &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; ![Sanal akıllı kart](./media/multi-factor-authentication/vsmart.png) &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; ![kullanıcı adı ve parola](./media/multi-factor-authentication/cert.png)</center>

Azure Multi-Factor Authentication (MFA) Microsoft'un iki adımlı doğrulama çözümüdür. Azure MFA, kullanıcıların oturum açmaya yönelik basit işlem taleplerini karşılarken, verilere ve uygulamalara erişimi korumaya da yardımcı olur. Bir dizi doğrulama yöntemi (ör. telefon çağrısı, metin mesajı veya mobil uygulama doğrulaması) aracılığıyla güçlü kimlik doğrulaması sunar.

## <a name="why-use-azure-multi-factor-authentication"></a>Azure Multi-Factor Authentication neden kullanılmalıdır?
Bugün, birden çok, kişilerin giderek bağlanır. Akıllı telefonlar, tabletler, dizüstü bilgisayarlar ve Bilgisayarları ile kişiler kendi hesaplarının ve uygulamaların dilediğiniz yerde erişmek ve herhangi bir zamanda bağlı kalmak için birden fazla seçeneği vardır.

Azure çok faktörlü kimlik doğrulaması, kullanıcılarınızın korumak için kimlik doğrulamasının ikinci bir yöntem sağlayan bir kullanımı kolay, ölçeklenebilir ve güvenilir çözümüdür.

| ![Kullanımı Kolay](./media/multi-factor-authentication/simple.png) | ![Ölçeklenebilir](./media/multi-factor-authentication/scalable.png) | ![Her zaman korunması](./media/multi-factor-authentication/protected.png) | ![Güvenilir](./media/multi-factor-authentication/reliable.png) |
|:---:|:---:|:---:|:---:|
| **Kullanımı kolaydır** |**Ölçeklenebilir** |**Her zaman korunması** |**Güvenilir** |

* **Kullanımı kolay** -ayarlamak ve kullanmak Azure multi-Factor Authentication basittir. Azure multi-Factor Authentication ile birlikte gelen fazladan koruma kullanıcıların kendi cihazlarını yönetmesine olanak tanır. Birçok durumlarda tüm en iyi, yalnızca birkaç basit tıklama ile ayarlanabilir.
* **Ölçeklenebilir** -Azure multi-Factor Authentication bulut gücünü kullanır ve şirket içi ile tümleşir AD ve özel uygulamalar. Bu koruma, yüksek hacimli, kritik senaryolarınız için bile genişletilir.
* **Her zaman korumalı** -Azure çok faktörlü kimlik doğrulaması en yüksek endüstri standartları kullanarak güçlü kimlik doğrulaması sağlar.
* **Güvenilir** -Microsoft Azure multi-Factor Authentication % 99,9 kullanılabilirliğini garanti eder. Alan veya iki aşamalı doğrulama için doğrulama isteklerini işlemek erişemediği zaman hizmeti kullanılamaz olarak kabul edilir.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Windows-Azure-Multi-Factor-Authentication/player]


## <a name="next-steps"></a>Sonraki adımlar

- Hakkında bilgi edinin [Azure multi-Factor Authentication nasıl çalışır](concept-mfa-howitworks.md)

- Farklı hakkında okuyun [sürümleri ve Azure çok faktörlü kimlik doğrulaması için tüketim yöntemi](concept-mfa-licensing.md)
