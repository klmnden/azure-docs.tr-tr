---
title: Azure Active Directory kimlik koruması etkin risk olayları kapatma | Microsoft Docs
description: Seçenekler hakkında bilgi edinin Kapat etkin risk olayları sahip.
services: active-directory
keywords: Azure active directory kimlik koruması, bulut uygulaması bulma, yönetme, uygulamaları, güvenlik, risk, risk düzeyi, güvenlik açığı, güvenlik ilkesi
documentationcenter: ''
author: MicrosoftGuyJFlo
manager: daveba
ms.assetid: e7434eeb-4e98-4b6b-a895-b5598a6cccf1
ms.service: active-directory
ms.subservice: identity-protection
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/24/2018
ms.author: joflore
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2e003aec8fa5aeab587fa07acdae3a13b370a535
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60453502"
---
# <a name="how-to-close-active-risk-events"></a>Nasıl Yapılır: Etkin risk olaylarını kapatma

İle [risk olayları](../reports-monitoring/concept-risk-events.md), Azure Active Directory, riskli olabilecek kullanıcı hesaplarının göstergeleri algılar. Bir yönetici olarak etkilenen kullanıcılar artık risk altında olacak şekilde kapatıldığında tüm risk olayları almak istiyorsanız.

Bu makalede, etkin risk olayı kapatmak için sahip ek seçeneklerine genel bakış sağlar.

## <a name="options-to-close-risk-events"></a>Risk olayı kapatmak için seçenekleri 

Risk olayı durumu bozulmuş **etkin** veya **kapalı**. Tüm etkin risk olaylarını kullanıcı risk düzeyi adı verilen bir değer hesaplamaya katkıda bulunur. Kullanıcı risk düzeyinde (düşük, Orta, yüksek) bir hesap tehlikede olasılık göstergesidir. 

Etkin risk olayı kapatmak için aşağıdaki seçenekleriniz vardır:

- Parola sıfırlama kullanıcı riski İlkesi ile iste

- El ile parola sıfırlama
 
- Tüm risk olayları kapatılamadı 

- Tek tek risk olaylarını elle kapatın



## <a name="require-password-reset-with-a-user-risk-policy"></a>Parola sıfırlama kullanıcı riski İlkesi ile iste

[Kullanıcı riski koşullu erişim ilkesini](howto-user-risk-policy.md) yapılandırarak belirli bir kullanıcı riski seviyesinin otomatik olarak algılanması durumunda parola değişikliği yapılmasını sağlayabilirsiniz. 

![Parola sıfırla](./media/howto-close-active-risk-events/13.png)

Bir parola sıfırlama ilgili kullanıcının tüm etkin risk olayları kapatır ve kimlik güvenli bir duruma geri getirir. Kullanıcı riski İlkesi kullanarak, çünkü bu yöntem otomatik active risk olayı kapatmak için tercih edilen yöntemdir. Etkilenen kullanıcı ve Yardım Masası veya bir yönetici arasında gerekli bir etkileşim yoktur.

Ancak, bir kullanıcı riski İlkesi kullanarak her zaman geçerli değildir. Bu, örneğin, için geçerlidir:

- Bu kullanıcı için multi-Factor authentication (MFA) kayıtlı değil.
- Silinmiş active risk olayları sahip kullanıcılar.
- Bildirilen risk olayı yasal kullanıcı tarafından gerçekleştirilen olduğunu gösteren bir araştırma.


## <a name="manual-password-reset"></a>El ile parola sıfırlama

Kullanıcı riski İlkesi kullanarak bir parola sıfırlama gerektiren bir seçenek değilse, el ile parola sıfırlama ile kapalı bir kullanıcı için tüm risk olayları alabilirsiniz.

![Parola sıfırla](./media/howto-close-active-risk-events/04.png)


İlgili iletişim kutusu, bir parola sıfırlama için iki farklı yöntem sunar:

![Parola sıfırla](./media/howto-close-active-risk-events/05.png)


**Geçici bir parola oluştur** -geçici bir parola oluşturarak, hemen bir kimlik geri güvenli bir duruma getirebilirsiniz. Bu yöntem, geçici parolayı ne olduğunu bilmeniz gerekir çünkü etkilenen kullanıcılarla etkileşim gerektirir. Örneğin, yeni bir geçici parola kullanıcı için bir alternatif e-posta adresi veya Kullanıcı Yöneticisi göndermek olabilir. Geçici bir parola olduğundan, kullanıcının sonraki oturum açma sırasında parola değiştirme istenir.


**Kullanıcının parola sıfırlama** -kullanıcıların parolalarını sıfırlamasına gerek sağlayan Self-Service recovery Yardım Masası veya bir yönetici ile iletişim kurmadan. Gibi bir kullanıcı riski İlkesi söz konusu olduğunda, bu yöntem yalnızca MFA için kaydolduğunu kullanıcılara uygulanır. MFA için henüz kaydedilmemiş kullanıcılar için bu seçenek kullanılamaz.


## <a name="dismiss-all-risk-events"></a>Tüm risk olayları kapatılamadı

Bir parola, sıfırlama sizin için bir seçenek değil, tüm risk olayları kapatılamadı. 

![Parola sıfırla](./media/howto-close-active-risk-events/03.png)

Tıkladığınızda **tüm olayları kapatılamadı**, tüm olayları kapatılır ve etkilenen kullanıcı artık risk altındadır. Bu yöntem mevcut parolayı bir etkiye sahip olmadığından, ancak bunu ilgili kimlik geri güvenli bir duruma getirmek değil. Bu yöntem için tercih edilen kullanım örneği, etkin risk olayları ile silinen bir kullanıcıdır. 


## <a name="close-individual-risk-events-manually"></a>Tek tek risk olaylarını elle kapatın

Tek tek risk olayları el ile kapatabilirsiniz. Risk olaylarını elle kapatma kullanıcı risk düzeyi düşürebilirsiniz. Genellikle, risk olayları, el ile ilgili bir araştırma yanıt kapatılır. Örneğin, konuşma, bir kullanıcı bir etkin risk olayı artık gerekli olmadığını ortaya çıkarır. 
 
Risk olaylarını elle kapatma zaman, risk olayı durumunu değiştirmek için aşağıdaki eylemlerden herhangi birini tercih edebilirsiniz:

![Eylemler](./media/howto-close-active-risk-events/06.png)

- **Çözmek** - bir risk olayını araştırdıktan sonra uygun düzeltme eylemi dışında kimlik koruması sürdü ve risk olayı kapatıldı, değerlendirilmesi gerektiğini düşünüyorsanız olayı Çözüldü olarak işaretleyin. Olayları risk olayın durumu kapalı olarak ayarlanır ve risk olayının artık kullanıcı riski katkıda bulunan çözüldü.

- **Hatalı pozitif olarak işaretleme** -bazı durumlarda, bir risk olayını araştırmak ve olabilirsiniz, yanlış riskli işaretlenmiş olduğunu keşfedin. Risk olayı hatalı pozitif sonuç olarak işaretleyerek böyle oluşum sayısını azaltmaya yardımcı olabilir. Bu, makine öğrenimi algoritmaları sınıflandırma benzer olayların gelecekte artırmak için yardımcı olur. Yanlış pozitif sonuç veren olayların durumunu kapalı olduğunu ve kullanıcı riski artık katkıda bulunur.

- **Yoksay** - herhangi bir düzeltme eylemi olmamıştır, ancak etkin listesinden kaldırılması için risk olayı istiyorsanız, bir risk olayını yoksay işaretleyebilirsiniz ve olay durumu kapatılır. Kullanıcı riski yok sayılan olayları katkıda bulunmuyor. Bu seçenek yalnızca olağan dışı durumlarda kullanılmalıdır.

- **Yeniden** -(çözümleme, hatalı pozitif ya da yoksay seçerek) el ile kapatılmış Risk olayları yeniden etkinleştirilmediğinden, olay durumu etkin olarak ayarlama. Yeniden etkinleştirilen risk olayları için kullanıcı risk düzeyi hesaplama katkıda bulunur. (Güvenli parola sıfırlama gibi) düzeltme aracılığıyla kapatılan risk olayları yeniden etkinleştirilemez.
  

## <a name="next-steps"></a>Sonraki adımlar

Azure AD kimlik koruması genel bakış için bkz: [Azure AD kimlik koruması genel bakış](overview.md).
