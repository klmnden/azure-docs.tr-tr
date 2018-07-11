---
title: 'Azure Active Directory etki alanı Hizmetleri: Etki alanı askıya | Microsoft Docs'
description: Yönetilen etki alanı askıya alma ve silme
services: active-directory-ds
documentationcenter: ''
author: eringreenlee
manager: mtillman
editor: curtand
ms.assetid: 95e1d8da-60c7-4fc1-987d-f48fde56a8cb
ms.service: active-directory
ms.component: domains
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/09/2018
ms.author: ergreenl
ms.openlocfilehash: 37524fbdf3cd521414a507f6fb67421708343ccd
ms.sourcegitcommit: aa988666476c05787afc84db94cfa50bc6852520
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37934251"
---
# <a name="suspended-domains"></a>Askıya alınan etki alanları
Azure AD Domain Services uzun bir süre için yönetilen bir etki alanı hizmeti mümkün olduğunda, yönetilen etki alanı askıya alınmış bir duruma getirilir. Bu makalede, yönetilen etki alanlarını neden askıya alınır, askıya alma uzunluğunu ve sorunun nasıl düzeltileceği askıya alınmış bir etki alanı açıklayacak.


## <a name="overview-of-suspended-domains"></a>Askıya alınmış etki alanlarına genel bakış

![Askıya alınmış bir etki alanı zaman çizelgesi](media\active-directory-domain-services-suspension\suspension-timeline.PNG)

Önceki grafiği, bir etki alanı nasıl askıya alınır, ne kadar süreyle askıya ve sonuç olarak, yönetilen etki alanını silme işlemi açıklanmaktadır. Aşağıdaki bölümlerde neden bir etki alanı askıya nedenleri ve yönetilen bir etki alanı erişimi iade etme ayrıntılarını gösterir.


## <a name="why-are-managed-domains-suspended"></a>Neden yönetilen bir etki alanı askıya alınır?

Yönetilen etki alanlarını Azure AD Domain Services etki alanını yönetme mümkün olduğu bir durumda olduklarında askıya alınır. Bu Azure AD Domain Services veya etkin olmayan bir Azure aboneliği için gerekli kaynaklara erişimi engeller yanlış yapılandırma tarafından kaynaklanabilir. Yönetilen bir etki alanı hizmeti erişememe 15 gün sonra Azure AD Domain Services etki alanı askıya alırız.

Etki alanınızı neden askıya alınamadan tam nedenlerini aşağıda listelenmiştir:
* Bir veya daha fazla mevcut aşağıdaki uyarılar için 15 gün arka arkaya kullanılmaması etki alanınızda sahip:
   * [AADDS104: Ağ hatası](active-directory-ds-troubleshoot-nsg.md).
* Azure aboneliğinizin süresi dolmuş veya devre dışı


## <a name="what-happens-when-a-domain-is-suspended"></a>Bir etki alanı askıya ne olur?

Bir etki alanı askıya alındığında, Azure AD Domain Services yönetilen etki alanınıza hizmet sanal makineleri durdurur. Bu yedeklemeler artık alınır, kullanıcılar, etki alanında oturum açamıyor ve Azure AD ile eşitleme artık gerçekleştirilir anlamına gelir.

Etki alanı, yalnızca bir en fazla 15 gün için askıya alınma durumunda kalır. Zamanında kurtarma sağlamak üzere askıya alınması olabildiğince çabuk adresi önerilir.

## <a name="how-do-i-know-if-my-domain-is-suspended"></a>Etki alanım askıya alınmış olduğunu nasıl öğrenebilirim?
Yönetilen etki alanı alacak bir [uyarı](active-directory-ds-troubleshoot-alerts.md) askıya etki alanı bildirir Azure portalının Azure AD Domain Services durumu sayfasında. Ayrıca, etki alanı durumu "Askıya alındı" olarak etiketlenen.


## <a name="unsuspending-and-restoring-domains"></a>Unsuspending ve geri yüklenen etki alanları

Bir etki alanı uygulamasına yönelik erişimlerini iade için aşağıdaki adımlar gereklidir:

1. Gidin [Azure AD Domain Services sayfası](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.AAD%2FdomainServices) Azure portalında
2. Erişimini iade etmek istediğiniz etki alanını tıklatın
3. Sol gezinti bölmesinden **sistem durumu**
4. (Uyarı Kimliği AADDS503 ya da AADDS504, askıya alınma nedenini bağlı olacaktır) askıya alma uyarıya tıklayın.
5. Uyarıda belirtilen çözünürlük bağlantısına tıklayın ve, askıya alınma durumu çözümlemek için adımları izleyin.

Etki alanınıza yalnızca son yedeklemeden tarihinden geri yüklenebilir. Son yedekleme tarihini, yönetilen etki alanınızın sistem durumu sayfasında görüntülenir. Son yedeklemeden bu yana herhangi bir değişiklik unsuspension geri yüklenmez. Ayrıca, Azure AD Domain Services yedeklemeleri yalnızca 30 güne kadar depolayabilirsiniz. En son yedeklemesini 30 günden daha eski ise, yedekleme silinmesi gerekir ve Azure AD Domain Services bir yedekten geri yüklemeniz mümkün olmayacaktır.

## <a name="deleting-domains"></a>Etki alanı siliniyor

15 günden fazla bir süre için etki alanı askıya alınırsa, Azure AD Domain Services yönetilen etki alanı nedeniyle etkinlik ve etki alanı hizmeti yeteneğinin siler. Artık Azure AD Domain Services için faturalandırılırsınız. Yönetilen etki alanınıza geri yüklemek için yeniden oluşturmanız gerekir.


## <a name="next-steps"></a>Sonraki adımlar
- [Yönetilen etki alanınızda uyarıları çözümleyin](active-directory-ds-troubleshoot-alerts.md)
- [Azure AD Domain Services hakkında daha fazla bilgi](active-directory-ds-overview.md)
- [Ürün ekibine başvurun](active-directory-ds-contact-us.md)

## <a name="contact-us"></a>Bizimle İletişim Kurun

Azure Active Directory Domain Services ürün ekibiyle [geri bildirim paylaşma veya destek](active-directory-ds-contact-us.md).
