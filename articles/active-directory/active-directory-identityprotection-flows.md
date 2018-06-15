---
title: Azure AD kimlik koruması ile karşılaştığında oturum açma | Microsoft Docs
description: Kimlik koruması azaltıldığından veya bir kullanıcı düzeltilen veya çok faktörlü kimlik doğrulama İlkesi tarafından istendiğinde kullanıcı deneyimini genel bir bakış sağlar.
services: active-directory
keywords: Azure active directory kimlik koruması, cloud app discovery'yi, uygulamalar, güvenlik, risk, risk düzeyi, güvenlik açığı, güvenlik ilkesi yönetme
documentationcenter: ''
author: MarkusVi
manager: mtillman
ms.assetid: de5bf637-75a7-4104-b6d8-03686372a319
ms.service: active-directory
ms.component: protection
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2018
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 424ea1eca637653c5e7cfed9dc4b970d5afd91ad
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34713417"
---
# <a name="sign-in-experiences-with-azure-ad-identity-protection"></a>Oturum açma deneyimlerini Azure AD kimlik koruması
Azure Active Directory kimlik koruması ile şunları yapabilirsiniz:

* çok faktörlü kimlik doğrulaması için kullanıcıların
* riskli oturum açma işlemleri ve güvenliği aşılan kullanıcılar işleme

Bu sorunları sistem yanıta sahip bir kullanıcının oturum açma deneyimi üzerinde bir etkisi doğrudan imzalama kullanıcı adını sağlayarak bileşenini olduğundan ve bir parola artık mümkün olmayacaktır. Bir kullanıcı güvenli bir şekilde almak için gereken ek adımlar iş uygulamasına geri.

Bu makalede, oluşabilecek tüm durumlarda bir kullanıcının oturum açma deneyimini genel bir bakış sağlar.

**Multi-Factor Authentication**

* Çok faktörlü kimlik doğrulaması kayıt

**Risk altında oturum aç**

* Oturum açma riskli kurtarma
* Riskli oturum engellenen açma
* Çok faktörlü kimlik doğrulaması kayıt sırasında bir riskli oturum açma

**Risk kullanıcı**

* Gizliliği tehlikeye giren hesap kurtarma
* Engellenen gizliliği tehlikeye giren hesap

## <a name="multi-factor-authentication-registration"></a>Çok faktörlü kimlik doğrulaması kayıt
En iyi kullanıcı deneyimi, gizliliği tehlikeye giren hesap kurtarma akışı hem hem de riskli oturum açma akışını, kullanıcının kendi kendine kurtarabilirsiniz durumdur. Kullanıcılar için multi-Factor authentication kaydettiyseniz, zaten güvenlik tehditlerine geçirmek için kullanılan kendi hesabıyla ilişkili bir telefon numarası sahiptirler. Yardım masasına veya yöneticinize katılımı hesap bozulmayacak kurtarmak için gereklidir. Bu nedenle, çok faktörlü kimlik doğrulaması için kayıtlı kullanıcılarınızın almak için tavsiye. 

Yöneticiler, kullanıcılar hesaplarını ek güvenlik doğrulaması ayarlamak gerektiren bir ilke ayarlayabilir. Bu ilke kullanıcıların 14 gün boyunca çok faktörlü kimlik doğrulama seçeneğini atlayın olanak tanır. 14 günlük yetkisiz kullanım süresi yapılandırılabilir değildir.

**Çok faktörlü kimlik doğrulaması kayıt üç adım vardır:**

1. İlk adımda, kullanıcı hesabı çok faktörlü kimlik doğrulama kaydınızı ayarlamak için gereksinimi hakkında bir bildirim alır. 
   
    ![Düzeltme](./media/active-directory-identityprotection-flows/140.png "düzeltme")
2. Çok faktörlü kimlik doğrulamasını kurmak için nasıl kurulmasını istediğinizi bilmeniz sistem izin gerekir.
   
    ![Düzeltme](./media/active-directory-identityprotection-flows/141.png "düzeltme")
3. Sistem için bir sınama gönderir ve yanıt vermesi gerekir.
   
    ![Düzeltme](./media/active-directory-identityprotection-flows/142.png "düzeltme")

## <a name="risky-sign-in-recovery"></a>Oturum açma riskli kurtarma
Yönetici oturum açma riskler için bir ilke yapılandırmış, oturum açmaya çalıştığınızda etkilenen kullanıcılara bildirilir. 

**Riskli oturum açma akışını iki adımı vardır:** 

1. Olağan dışı bir şey hakkında kendi oturum yeni konumu, cihaz veya uygulama oturum açma gibi algılandı kullanıcı bilgilendirilir. 
   
    ![Düzeltme](./media/active-directory-identityprotection-flows/120.png "düzeltme")
2. Kullanıcı güvenlik sınaması çözme tarafından kimliğini kanıtlamak için gereklidir. Kullanıcı çok faktörlü kimlik doğrulaması için kaydedilmişse yuvarlayın bunların telefon numarasına bir güvenlik kodu trip ihtiyaç duyar. Bu yalnızca riskli bir oturum açma ve güvenliği aşılmış bir hesabı olduğundan, kullanıcı bu akış parolayı değiştirmek zorunda kalmazsınız. 
   
    ![Düzeltme](./media/active-directory-identityprotection-flows/121.png "düzeltme")

## <a name="risky-sign-in-blocked"></a>Riskli oturum engellenen açma
Yöneticiler, blok kullanıcılar oturum açma risk düzeyine bağlı olarak bağlı bir oturum açma riski ilkesini ayarlamak de seçebilirsiniz. Engeli almak için son kullanıcıların bir yönetici veya yardım masasına başvurmanız gerekir veya tanıdık konumu ya da cihaz açmayı deneyin. Çok faktörlü kimlik doğrulaması çözme tarafından otomatik olarak kurtarma seçeneği bu durumda değil.

![Düzeltme](./media/active-directory-identityprotection-flows/200.png "düzeltme")

## <a name="compromised-account-recovery"></a>Gizliliği tehlikeye giren hesap kurtarma
Bir kullanıcı risk Güvenlik İlkesi yapılandırıldığında kullanıcı karşılayan kullanıcılar risk düzeyi İlkesi'nde belirtilen (ve bu nedenle varsayılır tehlikeye) bunlar oturum açabilmeniz için önce kullanıcı güvenliğinin aşılmasına kurtarma aktığı gitmeniz gerekir. 

**Kullanıcı güvenlik aşılması kurtarma akışı üç adım vardır:**

1. Kullanıcı, kendi hesabı güvenlik riski nedeniyle şüpheli etkinlik olduğu veya kimlik bilgilerini sızmasını bilgilendirilir.
   
    ![Düzeltme](./media/active-directory-identityprotection-flows/101.png "düzeltme")
2. Kullanıcı güvenlik sınaması çözme tarafından kimliğini kanıtlamak için gereklidir. Kullanıcı çok faktörlü kimlik doğrulaması için kayıtlı değilse, güvenliğinin bozulması riskini Self kurtarabilirsiniz. Round telefon numarasına bir güvenlik kodu trip gerekecektir. 
   
   ![Düzeltme](./media/active-directory-identityprotection-flows/110.png "düzeltme")
3. Son olarak, kullanıcı, başka birinin kendi hesaplarına erişim sağlamış olma ihtimaline parolalarını değiştirmek için zorlanır. 
   Aşağıda bu deneyim ekran görüntüleri verilmiştir.
   
   ![Düzeltme](./media/active-directory-identityprotection-flows/111.png "düzeltme")

## <a name="compromised-account-blocked"></a>Engellenen gizliliği tehlikeye giren hesap
Engeli kaldırılmış bir kullanıcı risk güvenlik ilkesi tarafından engellenen bir kullanıcı almak için kullanıcı bir yöneticiye başvurun veya Yardım Masası gerekir. Çok faktörlü kimlik doğrulaması çözme tarafından otomatik olarak kurtarma seçeneği bu durumda değil.

![Düzeltme](./media/active-directory-identityprotection-flows/104.png "düzeltme")

## <a name="reset-password"></a>Parola sıfırlama
Güvenliği aşılmış kullanıcıların açmasını engellenirse yönetici kendileri için geçici bir parola oluşturun. Kullanıcıların, bir sonraki oturum açma sırasında parolalarını değiştirmek gerekir.

![Düzeltme](./media/active-directory-identityprotection-flows/160.png "düzeltme")

## <a name="see-also"></a>Ayrıca bkz.
* [Azure Active Directory kimlik koruması](active-directory-identityprotection.md) 

