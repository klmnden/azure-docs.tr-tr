---
title: Azure AD Connect sistem durumu ve kullanıcı gizliliğini | Microsoft Docs
description: Bu belgede Azure AD Connect Health ile kullanıcı gizliliği açıklanmaktadır.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2018
ms.author: billmath
ms.openlocfilehash: bf26e91308cfec0dc8ede20e683919b5764a4868
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="user-privacy-and-azure-ad-connect-health"></a>Kullanıcı gizliliği ve Azure AD Connect Health 

[!INCLUDE [Privacy](../../../includes/gdpr-intro-sentence.md)]

>[!NOTE] 
>Bu makalede, Azure AD Connect Health ve kullanıcı gizlilik ile ilgilidir.  Azure AD Connect ve kullanıcı gizlilik hakkında bilgi için bkz: [burada](../../active-directory/connect/active-directory-aadconnect-gdpr.md).

## <a name="user-privacy-classification"></a>Kullanıcı gizlilik sınıflandırma
Azure AD Connect Health döner içine **veri işlemcisi** GDPR sınıflandırma kategorisi. Veri işlemcisi ardışık hizmeti anahtar iş ortakları ve son tüketicileri veri işleme hizmetleri sağlar. Azure AD Connect Health kullanıcı verilerini oluşturmaz ve kişisel verileri toplanır ve nasıl kullanıldığı bağımsız denetimi yoktur. Veri alma, toplama, çözümleme ve Azure AD Connect Health raporlama mevcut şirket içi verilere dayanır. 

## <a name="data-retention-policy"></a>Veri saklama ilkesi
Azure AD Connect Health değil raporları oluşturun, analiz gerçekleştirmek veya 30 gün ötesinde Öngörüler sağlayın. Bu nedenle, Azure AD Connect Health depolamaz, işlem veya 30 gün ötesinde herhangi bir veriyi korur. Bu tasarım, GDPR düzenlemeler, Microsoft gizlilik uyumluluk düzenlemeleri ve Azure AD veri bekletme ilkeleri ile uyumludur. 

Etkin sunucularıyla **sistem sağlığı hizmeti verileri güncel değil** **hata** hiçbir veri bu zaman aralığı sırasında Connect Health ulaştı 30 güne önermek için uyarılar. Bu sunucular devre dışı ve Connect Health Portalı'nda gösterilmez. Sunucuları yeniden etkinleştirmek için öncelikle kaldırmanız ve [sistem durumu aracısı yeniden](active-directory-aadconnect-health-agent-install.md). Lütfen bu için uygulanmaz Not **uyarıları** ile aynı uyarı türü. Uyarılar, kısmi veri için uyarılırsınız sunucusundan eksik olduğunu gösterir. 
 
## <a name="disable-data-collection-and-monitoring-in-azure-ad-connect-health"></a>Veri toplama ve Azure AD Connect Health izleme devre dışı bırak
Azure AD Connect Health, izlenen bir hizmet örneği veya ayrı ayrı her izlenen sunucu için veri toplamayı durdurmak sağlar. Örneğin, Azure AD Connect Health kullanılarak izlenen tek tek ADFS (Active Directory Federasyon Hizmetleri) sunucuları için veri toplama durdurabilirsiniz. Ayrıca, Azure AD Connect Health ile izlenmekte olan tüm ADFS örneği için veri toplama durdurabilirsiniz. Bunu yapmak seçtiğinizde, bunlara karşılık gelen sunucuları Azure AD Connect Health Portalı'ndan veri toplamayı durdurduktan sonra silinir. 

>[!IMPORTANT]
> Azure AD genel yönetici ayrıcalıklarıyla veya RBAC katılımcı rolü silme izlenen sunuculara Azure AD Connect Health ihtiyacınız var.
>
> Bir sunucu veya hizmeti örneğini Azure AD Connect Health kaldırma ters çevrilebilir bir eylem değil. 

### <a name="what-to-expect"></a>Neler beklenir?
Veri toplama ve tek bir izlenen sunucu veya izlenen bir hizmet örneği için izleme durdurursanız, aşağıdakilere dikkat edin:

- İzlenen bir hizmet örneği sildiğinizde, örnek Portalı'nda Azure AD Connect Health izleme hizmeti listesinden kaldırılır. 
- İzlenen bir sunucuya veya izlenen bir hizmet örneği sildiğinizde, sistem durumu aracısı değil kaldırıldı veya sunucularınızdan kaldırılamaz. Sistem Durumu Aracısı, verileri Azure AD Connect Health için göndermek için yapılandırılır. Sistem Durumu Aracısı daha önce izlenen sunuculara el ile kaldırmanız gerekir.
- Sistem Durumu Aracısı, bu adımı uygulamadan önce kaldırmadıysanız, sistem durumu aracısı ile ilgili hata olayları görebilirsiniz.
- Microsoft Azure veri bekletme ilkesi uyarınca izlenen hizmet örneğine ait tüm veriler silinir.

### <a name="disable-data-collection-and-monitoring-for-a-monitored-server"></a>Veri toplama ve izlenen bir sunucu için izlemeyi devre dışı bırak
Bkz: [Azure AD Connect Health bir sunucuyu kaldırmak nasıl](active-directory-aadconnect-health-operations.md#delete-a-server-from-the-azure-ad-connect-health-service).

### <a name="disable-data-collection-and-monitoring-for-an-instance-of-a-monitored-service"></a>Veri toplama ve izlenen bir hizmet örneği için izleme devre dışı bırak
Bkz: [Azure AD Connect Health bir hizmet örneği kaldırma](active-directory-aadconnect-health-operations.md#delete-a-service-instance-from-azure-ad-connect-health-service).


## <a name="re-enable-data-collection-and-monitoring-in-azure-ad-connect-health"></a>Veri toplama ve Azure AD Connect Health izleme yeniden etkinleştirin
Azure AD Connect Health daha önce silinmiş izlenen hizmete ilişkin izleme yeniden etkinleştirmek için öncelikle kaldırmanız ve [sistem durumu aracısı yeniden](active-directory-aadconnect-health-agent-install.md) tüm sunuculara.


## <a name="next-steps"></a>Sonraki adımlar
* [Güven Merkezi Microsoft Privacy ilkeyi gözden geçirin](https://www.microsoft.com/trustcenter)
* [Azure AD Connect ve kullanıcı gizliliği](../../active-directory/connect/active-directory-aadconnect-gdpr.md)

