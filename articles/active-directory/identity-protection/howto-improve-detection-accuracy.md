---
title: Azure Active Directory kimlik koruması (yenilenmiş) içinde algılama doğruluğunu artırmak nasıl | Microsoft Docs
description: Azure Active Directory kimlik koruması (yenilenmiş) içinde algılama doğruluğunu artırmak nasıl.
services: active-directory
keywords: Azure active directory kimlik koruması, bulut uygulaması bulma, yönetme, uygulamaları, güvenlik, risk, risk düzeyi, güvenlik açığı, güvenlik ilkesi
documentationcenter: ''
author: MicrosoftGuyJFlo
manager: mtillman
ms.assetid: e7434eeb-4e98-4b6b-a895-b5598a6cccf1
ms.service: active-directory
ms.subservice: identity-protection
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/31/2019
ms.author: joflore
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7724d69a9294b420ca061d5ad26ad64826372203
ms.sourcegitcommit: 6da4959d3a1ffcd8a781b709578668471ec6bf1b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2019
ms.locfileid: "58517683"
---
# <a name="how-to-improve-the-detection-accuracy"></a>Nasıl Yapılır: Algılama doğruluğunu artırmak 

Kimlik koruması, Azure AD'ye ortamınızdaki risk algılama hakkında geri bildirim sağlamak için mekanizmaları sağlar. Geri bildirim sağlamak için algılanan riskli kullanıcı veya oturum açma olayı durumunu doğrulayabilirsiniz. Microsoft kullanıcılar üzerinde geçerli risk algılamalar eylemi gerçekleştirin ve gelecekteki algılamalar doğruluğunu artırmak için bu geri bildirim. 


## <a name="what-is-detection"></a>Algılama nedir?

Algılama, kullanıcı hesapları ile birlikte şüpheli etkinlikleri tanımlamak işlemidir. Azure AD algılayabilir şüpheli etkinlikler denir [risk olayı](../reports-monitoring/concept-risk-events.md). Algılama işlemini Uyarlamalı makine öğrenimi algoritmaları ve kullanıcılara yönelik risk olaylarını algılamak için buluşsal yöntemler temel alır.

Algılama sonuçları, kullanıcılar ve oturum açma risk altında olup olmadığını belirlemek için kullanılır. 


## <a name="how-can-i-improve-the-detection-accuracy"></a>Algılama doğruluğu nasıl geliştirebilirim?

Algılama, otomatik bir işlem olduğu için Azure AD hatalı pozitif sonuçları raporlar mümkündür. Azure AD'ye algılama sonuçlarıyla ilgili geri bildirim sağlayarak algılama doğruluğunu geliştirebilir.

Algılama doğruluğunu artırmak için üç yolu vardır: riskli oturum açma comfirm güvenli oturum açma onaylayın ve kullanıcı riski yok sayın. Aşağıdaki raporlardan bunu yapabilirsiniz:

- **Riskli oturum açma işlemleri raporu -** riskli oturum açma işlemleri raporu içinde oturum açma güvenli veya riskli olduğunu doğrulayabilirsiniz

- **Riskli kullanıcılar raporu -** riskli kullanıcılar raporda kullanıcı riski Kapat 

Geri bildiriminiz algılama sonuçları doğruluğunu artırmak için Azure AD tarafından işlenir. Genellikle, bir kullanıcı risk veya oturum açma riski araştırma bir parçası olarak geri bildirim sağlayın. Daha fazla bilgi için [riskli kullanıcılar ve oturum açma araştırma](howto-investigate-risky-users-signins.md).


## <a name="confirm-compromised"></a>Güvenliğin tehlikeye girdiğini onaylayın

Tehlikeye gibi bir oturum açma olay onaylama için Azure AD oturum açma kimlik sahibinin yetkilendirdiği değildi, bildirir. "Tehlikeye onaylayın" seçtiğinizde Azure AD olacaktır.

- Etkilenen kullanıcı yüksek kullanıcı riskini artırır.

- Makine öğrenimi, risk olayları algılar iyileştirilmesine yardımcı
 
- Kuruluşunuzun daha iyi korumak için ek ölçüler gerçekleştirin



Bir riskli oturum açma doğrulamak için:

- **Riskli oturum açma işlemleri raporu** -bu seçenek bir riskli oturum açma için bir veya daha fazla oturum açma olaylarını doğrulamanızı sağlar.

    ![Kullanıcı riskini kapat](./media/howto-improve-detection-accuracy/07.png)

- **Riskli oturum açma işlemleri raporu Ayrıntılar görünümünü** -bu seçenek, seçilen oturum açma olayı riskli oturum açma işlemleri raporu için güvenliği aşılmış bir hesabı onaylamak sağlar. 

    ![Kullanıcı riskini kapat](./media/howto-improve-detection-accuracy/04.png)


 
## <a name="confirm-safe"></a>Güvenli olduğunu onayla


Azure AD'ye güvenli sinyalleri olarak oturum açma olay onaylama, oturum açma **olan** ilgili kimlik sahibinin yetkilendirdiği. "Güvenli onaylayın" seçtiğinizde Azure AD olacaktır:

- Seçilen oturum açma kullanıcı risk katkısını geri döndür

- Temel risk olayları kapatın

- Makine öğrenimi, risk olayları algılar iyileştirilmesine yardımcı

- Kuruluşunuzun daha iyi korumak için ek ölçüler gerçekleştirin
 

Bir güvenli oturum açma, doğrulamak için:

- **Riskli oturum açma işlemleri raporu** -bu seçenek, bir güvenli oturum açmak için bir veya daha fazla oturum açma olaylarını onaylamak sağlar.

    ![Kullanıcı riskini kapat](./media/howto-improve-detection-accuracy/08.png)

- **Riskli oturum açma işlemleri raporu Ayrıntılar görünümünü** -bu seçenek, bir güvenli oturum açma için seçilen oturum açma olayı riskli oturum açma işlemleri raporu onaylamak sağlar. 

    ![Kullanıcı riskini kapat](./media/howto-improve-detection-accuracy/05.png)




## <a name="dismiss-user-risk"></a>Kullanıcı riskini kapat

Düzeltme eylemleri bir risk kullanıcı için zaten gerçekleştirdik ya da bunlar yanlışlıkla riskli olarak işaretlenen düşünüyorsanız, bir kullanıcının risk sayabilirsiniz. Bir kullanıcının risk kapatılıyor kullanıcı riskli olmayan bir duruma geri yükler. Tüm riskli oturum açma işlemleri ve risk, seçilen kullanıcı için olaylar kapatıldı.


İçinde bildirilen kullanıcı riski yok sayın:

- **Riskli kullanıcılar raporu** - bu seçenek, bir son kullanıcı riski verilecek sağlar veya daha fazla seçilen kullanıcılar.

    ![Kullanıcı riskini kapat](./media/howto-improve-detection-accuracy/02.png)

- **Ayrıntılar görünümü** -kullanıcı risk rapor seçilen kullanıcı için kullanıcı risk kapatmak bu seçeneği sağlar. 

    ![Kullanıcı riskini kapat](./media/howto-improve-detection-accuracy/01.png)


**Bilmeniz gerekenler:**

- Bu eylem geri alınamaz.

- Bu, bu eylemin tamamlanması isteğinizi yeniden gönderdikten değil neden olan birkaç dakika sürebilir.

- AD, kullanıcının kimlik bilgilerini yönetiliyorsa, yalnızca bu eylemi gerçekleştirebilir. 



## <a name="best-practices"></a>En iyi uygulamalar

Bir kullanıcının risk kapatılıyor kullanıcı riski İlkesi tarafından engellendi ve kendi kendine parola sıfırlama ve/veya MFA etkin olmaması nedeniyle düzeltme olamaz engelini kaldırmak için bir yoludur. Bu durumda, tüm gelecek risk olayları kendi kendine düzeltme olanağınız olduğundan kullanıcı için parola sıfırlama ve MFA kaydeder emin olun ve en iyisidir.


## <a name="next-steps"></a>Sonraki adımlar

Azure AD kimlik koruması genel bakış için bkz: [Azure AD kimlik koruması genel bakış](overview-v2.md).


