---
title: 'Azure AD Connect: Yükleme sırasında kaynak bağlantı sorunlarını giderme | Microsoft Docs'
description: Bu konu, yükleme sırasında kaynak bağlantısı ile ilgili sorunları gidermek için adımları sağlar.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: article
ms.date: 04/19/2019
ms.subservice: hybrid
ms.author: billmath
ms.openlocfilehash: fac33a01afc2efc1ab06c4783c11f7a089bb6208
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "60014752"
---
# <a name="troubleshooting-source-anchor-issues-during-installation"></a>Yükleme sırasında kaynak bağlantı sorunlarını giderme
Bu makalede farklı açıklanmaktadır kaynak bağlantısı ilgili bu sorunları çözmek için yükleme ve teklifler yolları sırasında meydana gelebilecek sorunları.

## <a name="invalid-source-anchor-in-azure-active-directory"></a>Azure Active Directory'de geçersiz kaynak bağlantısı

### <a name="custom-installation"></a>Özel Yükleme

Özel yükleme sırasında Azure AD Connect, Azure Active Directory'den kaynak bağlantı İlkesi okur. Azure Active Directory'de bir ilke varsa Azure AD Connect müşteri tarafından geçersiz kılınmadığı sürece aynı ilke geçerlidir. Sihirbaz hangi özniteliğin okuma bildirir. Ayrıca, sihirbaz, kaynak bağlantı ilkesi geçersiz kılmak deneyin sizi uyarır.

Bu okuma işlemi sırasında Azure Active Directory'de kaynak bağlantı İlkesi beklenmiyor mümkündür. Bu durumda, Azure AD Connect, hangi kaynak bağlantısı kullanmayı bilmeyen ve el ile geçersiz kılma gerekiyor.</br>
![beklenmeyen](media/tshoot-connect-source-anchor/source1.png)

Bu sorunu çözmek için el ile kaynak bağlantısını belirli bir öznitelik seçerek geçersiz kılabilirsiniz. Bu seçenekle belirli ve yalnızca, hangi özniteliğinin seçin. Emin değilseniz, kişi [Microsoft Destek](https://support.microsoft.com/contactus/) Kılavuzu. Kaynak bağlantı ilkeyi değiştirirseniz, şirket içi kullanıcılarınıza ve bunların ilişkili Azure kaynakları arasında ilişki bozabilir.</br>
![beklenmeyen](media/tshoot-connect-source-anchor/source2.png)

### <a name="express-installation"></a>Hızlı yükleme
Hızlı yükleme sırasında Azure AD Connect, Azure Active Directory'den kaynak bağlantı İlkesi okur. Azure Active Directory'de bir ilke varsa Azure AD Connect aynı ilke geçerlidir. El ile geçersiz kılma yapmak için seçeneği yoktur.

Bu okuma işlemi sırasında Azure Active Directory'de kaynak bağlantı İlkesi beklenmiyor mümkündür. Bu durumda, Azure AD Connect kaynak bağlantısı olması gereken bilmez.</br>
![beklenmeyen](media/tshoot-connect-source-anchor/source3.png)

Bu sorunu çözmek için özel bir modunu kullanarak yeniden yükleyin ve el ile kaynak bağlantısı, belirli bir öznitelik seçerek yok saymak gerekir. Bu seçenekle belirli ve yalnızca, hangi özniteliğinin seçin. Emin değilseniz, kişi [Microsoft Destek](https://support.microsoft.com/contactus/) Kılavuzu. Kaynak bağlantı ilkeyi değiştirirseniz, şirket içi kullanıcılarınıza ve bunların ilişkili Azure kaynakları arasında ilişki bozabilir.

### <a name="invalid-source-anchor-in-sync-engine"></a>Eşitleme altyapısındaki geçersiz kaynak bağlantısı
İsteğe bağlı olarak yükleme sırasında olası bir Azure AD, bir geçersiz kaynak bağlantısı kullanılarak eşitleme altyapısını yapılandırma girişimleri bağlanın. Bu işlem, büyük olasılıkla bir ürün sorunu olur ve Azure AD Connect yüklemesi başarısız olur. İlgili kişi [Microsoft Destek](https://support.microsoft.com/contactus/) bu soruna çalıştırırsanız.</br>
![beklenmeyen](media/tshoot-connect-source-anchor/source4.png)


## <a name="next-steps"></a>Sonraki adımlar
[Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md) hakkında daha fazla bilgi edinin.