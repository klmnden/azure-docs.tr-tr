---
title: İki aşamalı doğrulamayı Azure MFA hakkında bilgi edinin | Microsoft Docs
description: Azure multi-Factor Authentication nedir, neden MFA'yı farklı yöntemler ve kullanılabilir bir sürümü kullanın.
services: multi-factor-authentication
ms.service: active-directory
ms.component: authentication
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: michmcla
ms.openlocfilehash: d58d81d85dac7e5cd520b8d8a3fb5d91650e0cbe
ms.sourcegitcommit: 1478591671a0d5f73e75aa3fb1143e59f4b04e6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/19/2018
ms.locfileid: "39161614"
---
# <a name="what-is-azure-multi-factor-authentication"></a>Azure Multi-Factor Authentication nedir?

İki aşamalı doğrulama, birden fazla doğrulama yöntemi gerektiren ve kullanıcı oturum açmalarına ve işlemlerine önemli bir ikinci güvenlik katmanı ekler kimlik doğrulama için kullanılan bir yöntemdir. Aşağıdaki doğrulama yöntemlerinden herhangi ikisini veya isteyerek çalışır:

* Bir şey (genellikle parola) bildirin
* (Kolayca, bir telefon gibi kopyalaması değil güvenilir bir cihaz) sahip olduğunuz şey
* Bir şey (Biyometri) olan

<center>![Kullanıcı adı ve parola](./media/multi-factor-authentication/pword.png) &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; ![sertifikaları](./media/multi-factor-authentication/phone.png) &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; ![Akıllı Telefon](./media/multi-factor-authentication/hware.png) &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; ![akıllı kart](./media/multi-factor-authentication/smart.png) &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; ![Sanal akıllı kart](./media/multi-factor-authentication/vsmart.png) &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; ![kullanıcı adı ve parola](./media/multi-factor-authentication/cert.png)</center>

Azure Multi-Factor Authentication (MFA) Microsoft'un iki adımlı doğrulama çözümüdür. Azure MFA, kullanıcıların oturum açmaya yönelik basit işlem taleplerini karşılarken, verilere ve uygulamalara erişimi korumaya da yardımcı olur. Bir dizi doğrulama yöntemi (ör. telefon çağrısı, metin mesajı veya mobil uygulama doğrulaması) aracılığıyla güçlü kimlik doğrulaması sunar.

## <a name="why-use-azure-multi-factor-authentication"></a>Azure Multi-Factor Authentication neden kullanılmalıdır?
Bugün, birden fazla şimdiye kadar kişilerin giderek bağlanır. Akıllı telefonlar, tabletler, dizüstü bilgisayarlar ve Bilgisayarları ile kişiler, hesaplar ve uygulamaları dilediğiniz yerde erişmek ve herhangi bir zamanda sürdürün için birden çok seçeneğiniz vardır.

Azure multi-Factor Authentication ikinci bir yöntem kullanıcılarınızı korumak için kimlik doğrulama sağlayan bir kullanımı kolay, ölçeklenebilir ve güvenilir çözümüdür.

| ![Kullanımı Kolay](./media/multi-factor-authentication/simple.png) | ![Ölçeklenebilir](./media/multi-factor-authentication/scalable.png) | ![Her zaman korunması](./media/multi-factor-authentication/protected.png) | ![Güvenilir](./media/multi-factor-authentication/reliable.png) |
|:---:|:---:|:---:|:---:|
| **Kullanımı kolay** |**Ölçeklenebilir** |**Her zaman korunması** |**Güvenilir** |

* **Kullanımı kolay** -ayarlama ve kullanma Azure multi-Factor Authentication basittir. Azure multi-Factor Authentication ile birlikte gelen ek koruma, kullanıcıların kendi cihazlarını yönetmesine olanak tanır. Birçok durumlarda tüm en iyi, yalnızca birkaç basit tıklama ile ayarlanabilir.
* **Ölçeklenebilir** -Azure multi-Factor Authentication bulutun gücünü kullanır ve şirket içi ile tümleştirilir AD ve özel uygulamalar. Bu koruma, yüksek hacimli, görev açısından kritik senaryolar için bile genişletilir.
* **Her zaman korumalı** -Azure multi-Factor Authentication, en yüksek endüstri standartları kullanarak güçlü kimlik doğrulaması sağlar.
* **Güvenilir** -Microsoft Azure multi-Factor Authentication'ın % 99,9 oranında kullanılabilirliği garanti eder. Bu iki adımlı doğrulama için doğrulama istekleri alınamadığında veya işlenemediğinde hizmet kullanılamaz olarak kabul edilir.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Windows-Azure-Multi-Factor-Authentication/player]


## <a name="next-steps"></a>Sonraki adımlar

- Hakkında bilgi edinin [Azure multi-Factor Authentication nasıl çalışır?](concept-mfa-howitworks.md)

- Farklı hakkında okuyun [sürümleri ve Azure multi-Factor Authentication için tüketim yöntemi](concept-mfa-licensing.md)
