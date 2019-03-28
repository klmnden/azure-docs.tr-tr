---
title: Azure AD kimlik koruması ile karşılaştığında oturum açma | Microsoft Docs
description: Kimlik koruması azaltılabilir veya bir kullanıcı düzeltilen veya çok faktörlü kimlik doğrulaması İlkesi tarafından istendiğinde kullanıcı deneyimini genel bir bakış sağlar.
services: active-directory
keywords: Azure active directory kimlik koruması, bulut uygulaması bulma, yönetme, uygulamaları, güvenlik, risk, risk düzeyi, güvenlik açığı, güvenlik ilkesi
documentationcenter: ''
author: MicrosoftGuyJFlo
manager: daveba
ms.assetid: de5bf637-75a7-4104-b6d8-03686372a319
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
ms.openlocfilehash: 449f808e98c4e0db2972071e160f5335153a88f2
ms.sourcegitcommit: 6da4959d3a1ffcd8a781b709578668471ec6bf1b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2019
ms.locfileid: "58520141"
---
# <a name="sign-in-experiences-with-azure-ad-identity-protection"></a>Azure AD kimlik koruması ile oturum açma deneyimleri
Azure Active Directory kimlik koruması ile yapabilecekleriniz:

* çok faktörlü kimlik doğrulaması için kaydolmalarını iste
* riskli oturum açma işlemleri ve riskli kullanıcılar

Bu sorunlar sistemin yanıta bir kullanıcının oturum açma deneyimini etkilenmesinden doğrudan imzalama kullanıcı adını sağlayarak açma sahip ve bir parola artık mümkün olmayacaktır. Bir kullanıcının güvenli bir şekilde almak için ek adımlar gerekli iş uygulamasına geri.

Bu makalede ortaya çıkabilecek tüm durumlarda bir kullanıcının oturum açma deneyiminin genel bir bakış sağlar.

**Multi-Factor Authentication**

* Çok faktörlü kimlik doğrulaması kaydı

**Riskli oturum açma**

* Riskli oturum açma kurtarma
* Riskli engellenen oturum açma
* Çok faktörlü kimlik doğrulaması kayıt sırasında bir riskli oturum açma

**Kullanıcı risk altında**

* Gizliliği tehlikeye giren hesap kurtarma
* Engellenen gizliliği tehlikeye giren hesap

## <a name="multi-factor-authentication-registration"></a>Çok faktörlü kimlik doğrulaması kaydı
Her ikisi de gizliliği tehlikeye giren hesap kurtarma akış ve riskli oturum açma akışı için en iyi kullanıcı deneyimi, kullanıcının kendi kendine kurtarabilirsiniz andır. Kullanıcılar için multi-Factor authentication kayıtlıysa, zaten güvenlik sorunlarına geçirmek için kullanılan hesaplarıyla ilişkili bir telefon numarası sahiptirler. Hesap bozulmayacak kurtarmak için Yardım masanıza veya yöneticinize katılımı gereklidir. Bu nedenle, çok faktörlü kimlik doğrulamasına kayıtlı kullanıcılarınız almak için tavsiye. 

Yöneticiler, kullanıcıların kendi hesapları için ek güvenlik doğrulaması gerektiren bir ilke ayarlayabilir. Bu ilke, kullanıcıların 14 güne kadar çok faktörlü kimlik doğrulaması kaydolmayı atlamasına izin verir. 14 günlük yetkisiz kullanım süresi yapılandırılabilir değildir.

**Çok faktörlü kimlik doğrulaması kaydı üç adım vardır:**

1. Bu adımda, kullanıcı hesabı çok faktörlü kimlik doğrulaması için ayarlanacak gereksinimi hakkında bir bildirim alır. 
   
    ![Düzeltme](./media/flows/140.png "düzeltme")
2. Çok faktörlü kimlik doğrulamasını'kurmak için sistemin nasıl irtibat kurulmasını istediğinizi bilmeniz gerekir.
   
    ![Düzeltme](./media/flows/141.png "düzeltme")
3. Sistem için bir sınama gönderir ve yanıt vermesi gerekir.
   
    ![Düzeltme](./media/flows/142.png "düzeltme")

## <a name="risky-sign-in-recovery"></a>Riskli oturum açma kurtarma
Yönetici oturum açma risk için bir ilke yapılandırmış, etkilenen kullanıcılar oturum açmak çalıştıklarında bildirilir. 

**Riskli oturum açma akışı iki adımı vardır:** 

1. Olağan dışı bir şey hakkında yeni konum, cihaz veya uygulama oturum açma gibi oturum, algılandığını kullanıcı bilgilendirilir. 
   
    ![Düzeltme](./media/flows/120.png "düzeltme")
2. Kullanıcı, bir güvenlik testi uygulayarak çözme tarafından kimliklerini kanıtlamak için gereklidir. Multi-Factor authentication için kullanıcı kayıtlı değilse yuvarlak bir güvenlik kodu, telefon numaralarına geçirmek için gereken gerekir. Bu bir riskli oturum açma ve güvenliği aşılmış bir hesabı olduğundan, bu akış parolayı değiştirmek kullanıcı olmaz. 
   
    ![Düzeltme](./media/flows/121.png "düzeltme")

## <a name="risky-sign-in-blocked"></a>Riskli engellenen oturum açma
Yöneticiler, risk düzeyine bağlı olarak oturum açma sonrası kullanıcıları engellemek için bir oturum açma riski İlkesi ayarlamak de seçebilirsiniz. Engeli almak için son kullanıcıların bir yönetici veya yardım masasına başvurmanız gerekir ya da bir tanıdık konumu veya CİHAZDAN oturum açarken deneyebilirsiniz. Çok faktörlü kimlik doğrulaması çözme tarafından otomatik kurtarma seçeneği bu durumda değil.

![Düzeltme](./media/flows/200.png "düzeltme")

## <a name="compromised-account-recovery"></a>Gizliliği tehlikeye giren hesap kurtarma
Bir kullanıcı riski İlkesi yapılandırıldığında, kullanıcılar, kullanıcının karşılaması risk düzeyi ilkesinde belirtilen (ve bu nedenle varsayılır tehlikeye) oturum önce kullanıcı güvenliğinin aşılmasına kurtarma akışını gitmeniz gerekir. 

**Kullanıcı güvenlik aşılması kurtarma akışı üç adım vardır:**

1. Kullanıcı hesabı güvenliklerini şüpheli etkinlik nedeniyle risk altında olan veya kimlik bilgilerinin sızdırılması bilgisi verilir.
   
    ![Düzeltme](./media/flows/101.png "düzeltme")
2. Kullanıcı, bir güvenlik testi uygulayarak çözme tarafından kimliklerini kanıtlamak için gereklidir. Multi-Factor authentication için kullanıcı kayıtlı değilse, tehlikeye girmesini Self kurtarabilirsiniz. Round telefon numarasına güvenlik kodunu geçirmek için gereken gerekir. 
   
   ![Düzeltme](./media/flows/110.png "düzeltme")
3. Son olarak, kullanıcı, başka birinin kendi hesaplarına erişim sağlamış parolasını değiştirmek için zorlanır. 
   Bu deneyimi ekran görüntüsü aşağıda verilmiştir.
   
   ![Düzeltme](./media/flows/111.png "düzeltme")

## <a name="compromised-account-blocked"></a>Engellenen gizliliği tehlikeye giren hesap
Kullanıcının engeli kaldırılmış bir kullanıcı riski İlkesi tarafından engellenen bir kullanıcı almak için yöneticinize başvurun veya Yardım Masası gerekir. Çok faktörlü kimlik doğrulaması çözme tarafından otomatik kurtarma seçeneği bu durumda değil.

![Düzeltme](./media/flows/104.png "düzeltme")

## <a name="reset-password"></a>Parola sıfırlama
Riskli kullanıcıların, oturum engellenir varsa bir yönetici için geçici bir parola oluşturun. Kullanıcıların, bir sonraki oturum açma sırasında parolalarını değiştirmesi gerekir.

![Düzeltme](./media/flows/160.png "düzeltme")

## <a name="see-also"></a>Ayrıca bkz.
* [Azure Active Directory kimlik koruması](../active-directory-identityprotection.md) 

