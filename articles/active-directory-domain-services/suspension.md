---
title: 'Azure Active Directory etki alanı Hizmetleri: Etki alanı askıya | Microsoft Docs'
description: Yönetilen etki alanı askıya alma ve silme
services: active-directory-ds
documentationcenter: ''
author: MikeStephens-MS
manager: daveba
editor: curtand
ms.assetid: 95e1d8da-60c7-4fc1-987d-f48fde56a8cb
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/20/2019
ms.author: mstephen
ms.openlocfilehash: a4e43518ce70c040b8243034c52e4b215548a47c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66246727"
---
# <a name="suspended-domains"></a>Askıya alınan etki alanları
Azure Active Directory etki alanı Hizmetleri (Azure AD DS) uzun bir süre için yönetilen bir etki alanı hizmeti mümkün olduğunda, yönetilen etki alanı askıya alınmış bir duruma koyar. Bu makalede, yönetilen etki alanlarını neden askıya alınır ve sorunun nasıl düzeltileceği askıya alınmış bir etki alanı açıklanmaktadır.


## <a name="states-your-managed-domain-can-be-in"></a>Yönetilen etki alanınıza durumlar olabilir

![Askıya alınmış bir etki alanı zaman çizelgesi](media/active-directory-domain-services-suspension/suspension-timeline.PNG)

Önceki grafiği, bir Azure AD DS yönetilen etki alanı içinde yer alabileceği olası durumlar açıklanmaktadır.

### <a name="running-state"></a>"Çalışıyor" durumunda
Doğru şekilde yapılandırılıp düzenli olarak yönetilen bir etki alanında yer **çalıştıran** durumu.

**Beklenecekler**
* Microsoft, yönetilen etki alanınıza durumunu düzenli olarak izleyebilirsiniz.
* Yönetilen etki alanınız için etki alanı denetleyicileri Yama ve düzenli olarak güncelleştirilir.
* Azure Active Directory'deki değişiklikleri düzenli olarak yönetilen Etki Alanınızla eşitlenir.
* Düzenli yedeklemeler, yönetilen etki alanınıza yönlendirilir.


### <a name="needs-attention-state"></a>"İlgilenilmesi gerekiyor" durumu
Yönetilen etki alanında yer **dikkat gösterilmesi gerekiyor** bir veya daha fazla sorun, bir yöneticinin işlem yapmasını gerektirip gerektirmediğini belirtin. Yönetilen etki alanınızın sistem durumu sayfasında bir veya daha fazla uyarı Bu durumdayken listeler.

Örneğin, kısıtlayıcı bir NSG, sanal ağınız için yapılandırdıysanız, Microsoft update ve yönetilen etki alanınıza izlemek mümkün olmayabilir. Geçersiz bu yapılandırma, yönetilen etki alanınıza yerleştirir "Dikkat gösterilmesi gerekiyor" durumuna bir uyarı tetikler.

Her uyarı çözüm adımları kümesini sahiptir. Bazı uyarılar geçicidir ve hizmet tarafından otomatik olarak çözülebilir. Bu uyarı için ilgili çözüm adımlarını'ndaki yönergeleri takip ederek, diğer bazı uyarılar çözebilirsiniz. Bazı kritik uyarılar için bir çözüm için Microsoft desteğine başvurun gerekir.

Daha fazla bilgi için [nasıl sorun giderme uyarıları yönetilen etki alanında](troubleshoot-alerts.md).

**Beklenecekler**

Bazı durumlarda (örneğin, bir geçersiz ağ yapılandırması varsa), yönetilen etki alanınız için etki alanı denetleyicileri erişilemez durumda olabilir. Yönetilen etki alanınız "Dikkat gösterilmesi gerekiyor" durumda olduğunda, Microsoft görüntülenecektir izlenen, düzeltme eki, güncelleştirildi veya yedeklenen düzenli olarak olduğunu garanti edemez.

* Yönetilen etki alanınıza kötü bir durumda ve uyarı çözülene kadar devam eden sistem durumu izleme durabilir.
* Yönetilen etki alanınız için etki alanı denetleyicileri yama veya güncelleştirilemez.
* Azure Active Directory değişikliklerini, yönetilen Etki Alanınızla eşitlenemeyebilir.
* Yönetilen etki alanınız için yedeklemeler, mümkünse duruma getirilebilir.
* Yönetilen etki alanınıza etkileyen uyarıları çözümlemek, "Çalışır" duruma geri yüklemeniz mümkün olabilir.
* Microsoft, etki alanı denetleyicilerine ulaşamıyoruz olduğu için yapılandırma sorunları kritik uyarılar tetiklenir. Bu tür uyarılar 15 gün içinde çözümlenen değil, yönetilen etki alanınız "Askıya alındı" durumunda konur.


### <a name="the-suspended-state"></a>"Askıya alındı" durumunda
Yönetilen bir etki alanına yerleştirmenizi **askıya alındı** durum aşağıdaki nedenlerden dolayı:

* Bir veya daha fazla kritik uyarılar 15 gün içinde çözümlenen henüz. Kritik uyarılar tarafından yanlış yapılandırma, Azure AD DS tarafından gerekli kaynaklara erişimi engeller neden olabilir.
    * Örneğin, uyarıyı [AADDS104: Ağ hatası](alert-nsg.md) 15 günden fazla bir süre için yönetilen etki alanında çözümlenmemiş.
* Azure aboneliğiniz fatura bir sorun veya Azure aboneliğinizin süresi doldu.

Yönetilen etki alanlarını Microsoft yönetmek, izlemek, düzeltme eki veya sürekli olarak etki alanı geri erişemediği zaman askıya alınır.

**Beklenecekler**
* Yönetilen etki alanınız için etki alanı denetleyicileri XML'deki sağlanır ve sanal ağ içinde erişilebilir değil.
* Yönetilen etki alanında (etkinse) internet üzerinden güvenli LDAP erişimini çalışmayı durduruyor.
* Yönetilen etki alanına kimlik doğrulama, etki alanına katılmış sanal makineleri için oturum açma veya LDAP/LDAPS bağlanma hataları dikkat edin.
* Yönetilen etki alanınız için yedeklemeler artık alınır.
* Azure AD ile eşitleme işlemini durdurur.

Uyarı çözdükten sonra yönetilen etki alanınız "Askıya alındı" durumuna geçtiğinde. Ardından desteğe başvurmanız gerekir.
Destek, yönetilen etki alanınıza geri yükleyebilir, ancak yalnızca 30 günden eski olan bir yedek yok.

Yönetilen etki alanı, yalnızca 15 gün boyunca askıya alınmış durumda kalır. Yönetilen etki alanınıza kurtarmak için Microsoft, kritik uyarılar hemen çözmek önerir.


### <a name="deleted-state"></a>"Silindi" durumu
15 gün boyunca "Askıya alındı" durumunda kalır yönetilen bir etki alanı **silinmiş**.

**Beklenecekler**
* Tüm kaynaklar ve yedeklemeler için yönetilen etki alanı silinir.
* Yönetilen etki alanı geri yükleme ve Azure AD DS kullanmak için yeni yönetilen etki alanında oluşturmanız gerekir.
* Silindikten sonra yönetilen etki alanı için fatura değildir.


## <a name="how-do-you-know-if-your-managed-domain-is-suspended"></a>Yönetilen etki alanınıza askıya alınmış nasıl biliyor musunuz?
Gördüğünüz bir [uyarı](troubleshoot-alerts.md) etki alanı askıya alındığından bildirir Azure portalının Azure AD DS durumu sayfasında. Etki alanı durumu "Askıya alındı" de gösterir.


## <a name="restore-a-suspended-domain"></a>Askıya alınmış bir etki alanı geri yükleme
"Askıya alındı" durumunda bir etki alanı geri yüklemek için aşağıdaki adımları uygulayın:

1. Git [Azure Active Directory Domain Services sayfası](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.AAD%2FdomainServices) Azure portalında.
2. Yönetilen etki alanını seçin.
3. Sol bölmede bulunan seçin **sistem durumu**.
4. Uyarıyı seçin. Uyarı Kimliği AADDS503 ya da AADDS504, askıya alınma nedenini bağlı olacaktır.
5. Sağlanan çözümleme bağlantıyı uyarıyı seçin. Ardından, uyarıyı çözmek için adımları izleyin.

Yönetilen etki alanınıza yalnızca son yedekleme tarihini için geri yüklenebilir. Son yedekleme tarihini, yönetilen etki alanınızın sistem durumu sayfasında görüntülenir. Son yedeklemeden geri yüklenemeyecek sonra gerçekleşen değişiklikler. Yedekleri yönetilen bir etki alanı için 30 gün boyunca saklanır. 30 günden daha eski yedeklemelerin silinmesi.


## <a name="next-steps"></a>Sonraki adımlar
- [Yönetilen etki alanınız için uyarıları çözümleyin](troubleshoot-alerts.md)
- [Azure Active Directory Domain Services hakkında daha fazla bilgi](overview.md)
- [Ürün ekibine başvurun](contact-us.md)

## <a name="contact-us"></a>Bizimle iletişim kurun
Azure Active Directory Domain Services ürün ekibiyle [geri bildirim paylaşma veya destek](contact-us.md).
