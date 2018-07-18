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
ms.date: 07/16/2018
ms.author: ergreenl
ms.openlocfilehash: 5f350bacdc21ec4e5077c93ecc9d97f2fe6c39a5
ms.sourcegitcommit: e32ea47d9d8158747eaf8fee6ebdd238d3ba01f7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/17/2018
ms.locfileid: "39090918"
---
# <a name="suspended-domains"></a>Askıya alınan etki alanları
Azure AD Domain Services uzun bir süre için yönetilen bir etki alanı hizmeti mümkün olduğunda, yönetilen etki alanı askıya alınmış bir duruma getirilir. Bu makalede, yönetilen etki alanlarını neden askıya alınır, askıya alma uzunluğunu ve sorunun nasıl düzeltileceği askıya alınmış bir etki alanı açıklayacak.


## <a name="states-your-managed-domain-can-be-in"></a>Yönetilen etki alanınıza durumlar olabilir

![Askıya alınmış bir etki alanı zaman çizelgesi](media\active-directory-domain-services-suspension\suspension-timeline.PNG)

Önceki grafiği, bir etki alanı nasıl askıya alınır, ne kadar süreyle askıya ve sonuç olarak, yönetilen etki alanını silme işlemi açıklanmaktadır. Aşağıdaki bölümlerde neden bir etki alanı askıya nedenleri ve yönetilen bir etki alanı erişimi iade etme ayrıntılarını gösterir.

### <a name="running-state"></a>'Çalışıyor' durumunda
Doğru şekilde yapılandırılıp düzenli olarak yönetilen bir etki alanında yer **çalıştıran** durumu.

**Şunların gerçekleşmesini bekleyebilirsiniz:**
* Microsoft, yönetilen etki alanınıza düzenli olarak izlemesi kuramıyor.
* Yönetilen etki alanınız için etki alanı denetleyicileri Yama ve düzenli olarak güncelleştirilir.
* Azure Active Directory'deki değişiklikleri düzenli olarak yönetilen Etki Alanınızla eşitlenir.
* Düzenli yedeklemeler, yönetilen etki alanınıza yönlendirilir.


### <a name="needs-attention-state"></a>'Dikkat etmeniz gereken' durumu
Yönetilen etki alanında yer **dikkat gösterilmesi gerekiyor** durumu bir veya daha fazla sorun, bir yöneticinin işlem yapmasını gerekiyorsa. Yönetilen etki alanınızın sistem durumu sayfasında bir veya daha fazla uyarı Bu durumdayken listeler. Örneğin, kısıtlayıcı bir NSG, sanal ağınız için yapılandırdıysanız, Microsoft update ve yönetilen etki alanınızı izleme olabilir. Oluşturulan bir uyarıyı geçersiz yapılandırmanın sonuçlarını ve yönetilen etki alanınıza 'Dikkat gösterilmesi gerekiyor' durumda konur.

Her uyarı çözüm adımları kümesini sahiptir. Bazı uyarılar geçicidir ve hizmet tarafından otomatik olarak çözülmesi. Bu uyarı için ilgili çözüm adımlarını'ndaki yönergeleri takip ederek, diğer bazı uyarılar çözebilirsiniz. Bazı kritik uyarıları çözümlemek için Microsoft desteğe başvurmanız gerekir.

Daha fazla bilgi için [nasıl sorun giderme uyarıları yönetilen etki alanında](active-directory-ds-troubleshoot-alerts.md).

**Şunların gerçekleşmesini bekleyebilirsiniz:**

Bazı durumlarda (örneğin, bir geçersiz ağ yapılandırması varsa), yönetilen etki alanınız için etki alanı denetleyicilerine erişilemiyor olabilir. Bu nedenle Microsoft, yönetilen etki alanınıza izlenen, düzeltme eki, güncelleştirildi veya bu durumdaki düzenli olarak yedeklenen garanti edemez.

* Yönetilen etki alanınıza kötü bir durumda ve uyarı çözülene kadar devam eden sistem durumu izleme, etkilenmiş olabilir.
* Yönetilen etki alanınız için etki alanı denetleyicileri yama veya güncelleştirildi.
* Azure Active Directory değişikliklerini yönetilen Etki Alanınızla uyumlu olmayabilir.
* Yönetilen etki alanınız için yedeklemeler, mümkünse alınmış olabilir.
* Yönetilen etki alanınıza etkileyen uyarıları çözümlemek, yönetilen etki alanınıza 'Çalışıyor' durumuna geri yüklemeniz mümkün olabilir.
* Microsoft, etki alanı denetleyicilerine ulaşamıyoruz olduğu için yapılandırma sorunları kritik uyarılar tetiklenir. Bu tür uyarılar, 15 gün içinde çözümlenmezse, yönetilen etki alanınıza 'Askıya alındı' durumunda yerleştirilir.


### <a name="suspended-state"></a>'Askıya alındı' durumunda
Yönetilen bir etki alanına yerleştirmenizi **askıya alındı** durum aşağıdaki nedenlerden dolayı:
* Bir veya daha fazla kritik uyarılar 15 günde düzeltilen değil. Bu Azure AD Domain Services tarafından gerekli kaynaklara erişimi engeller yanlış yapılandırma tarafından kaynaklanabilir.
    * Örneğin, yönetilen etki alanı uyarısının [AADDS104: ağ hatası](active-directory-ds-troubleshoot-nsg.md) 15 gün boyunca çözümlenmemiş.
* Azure aboneliğiniz fatura bir sorun ya da Azure aboneliğinin süresi doldu.

Yönetilen etki alanlarını Microsoft yönetmek, izlemek, düzeltme eki veya etki alanı düzenli olarak yedekle erişemediği zaman askıya alınır.

**Şunların gerçekleşmesini bekleyebilirsiniz:**
* Yönetilen etki alanınız için etki alanı denetleyicileri XML'deki sağlanır ve sanal ağ içinde ulaşılamıyor.
* Yönetilen etki alanında (etkinse) internet üzerinden güvenli LDAP erişimini çalışmayı durduruyor.
* LDAP/LDAPS vb. bağlanan sanal makineler, yönetilen etki alanında etki alanı için oturum açma, kimlik doğrulama hataları katılmış görürsünüz.
* Yönetilen etki alanınız için yedeklemeler artık alınır.
* 'Askıya alındı' durumunda olmasını ve Destek ile iletişime geçin için yönetilen etki alanınızı neden uyarıyı çözmek gerekir.
* 30 günden eski olan varolan bir yedeği varsa destek yönetilen etki alanınıza geri yüklenmesi mümkün olabilir.


### <a name="deleted-state"></a>'Silindi' durumunda
'Askıya alındı' durumunda 15 gün boyunca kalacak yönetilen bir etki alanı **silinmiş**.

**Şunların gerçekleşmesini bekleyebilirsiniz:**
* Tüm kaynaklar ve yedeklemeler için yönetilen etki alanı silinir.
* Yönetilen etki alanı geri yüklenemiyor ve Azure AD Domain Services'ı kullanmak için yeni yönetilen etki alanında oluşturmanız gerekir.
* Yönetilen etki alanı için faturalandırılmazsınız.


## <a name="what-happens-when-a-managed-domain-is-suspended"></a>Yönetilen bir etki alanı askıya alındığında ne olur?
Bir etki alanı askıya alındığında, Azure AD Domain Services durdurur ve yönetilen etki alanınızın etki alanı denetleyicilerinin geri sağlamaları sildiğinde. Sonuç olarak, yedeklemeler artık alınır, kullanıcılar, etki alanında oturum açamıyor ve Azure AD ile eşitleme işlemini durdurur.

Yönetilen etki alanı, yalnızca en fazla 15 gün için askıya alınmış durumda kalır. Zamanında kurtarma sağlamak üzere askıya alınması olabildiğince çabuk adresi önerilir.


## <a name="how-do-i-know-if-my-managed-domain-is-suspended"></a>Yönetilen etki alanım askıya alınmış olduğunu nasıl öğrenebilirim?
Göreceğiniz bir [uyarı](active-directory-ds-troubleshoot-alerts.md) askıya etki alanı bildirir Azure portalının Azure AD Domain Services durumu sayfasında. Ayrıca, etki alanı durumu "Askıya alındı" gösterir.


## <a name="how-do-i-restore-a-suspended-domain"></a>Askıya alınmış bir etki alanı nasıl geri yüklerim?
Bir etki alanı 'Askıya alındı' durumunda geri yüklemek için aşağıdaki adımları tamamlayın:

1. Gidin [Azure AD Domain Services sayfası](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.AAD%2FdomainServices) Azure portalında
2. Erişimini iade etmek istediğiniz etki alanını tıklatın
3. Sol gezinti bölmesinden **sistem durumu**
4. Uyarıya tıklayın. Uyarı Kimliği AADDS503 ya da AADDS504, askıya alınma nedenini bağlı olacaktır.
5. Uyarıda belirtilen çözünürlük bağlantısına tıklayın ve uyarıyı çözmek için adımları izleyin.

Etki alanınıza yalnızca son yedekleme tarihini için geri yüklenebilir. Son yedekleme tarihini, yönetilen etki alanınızın sistem durumu sayfasında görüntülenir. Son yedeklemeden geri yüklenmez sonra gerçekleşen değişiklikler. Yedekleri yönetilen bir etki alanı için 30 gün boyunca saklanır. 30 günden daha eski yedeklemelerin silinmesi.


## <a name="deleting-domains"></a>Etki alanı siliniyor
15 günden fazla bir süre için etki alanı askıya alınırsa, Azure AD Domain Services yönetilen etki alanı nedeniyle etkinlik ve etki alanı hizmeti yeteneğinin siler. Artık Azure AD Domain Services için faturalandırılırsınız. Bu noktada, yönetilen etki alanınıza geri yükle ve yeniden oluşturmanız gerekir.


## <a name="next-steps"></a>Sonraki adımlar
- [Yönetilen etki alanınızda uyarıları çözümleyin](active-directory-ds-troubleshoot-alerts.md)
- [Azure AD Domain Services hakkında daha fazla bilgi](active-directory-ds-overview.md)
- [Ürün ekibine başvurun](active-directory-ds-contact-us.md)

## <a name="contact-us"></a>Bizimle İletişim Kurun
Azure Active Directory Domain Services ürün ekibiyle [geri bildirim paylaşma veya destek](active-directory-ds-contact-us.md).
