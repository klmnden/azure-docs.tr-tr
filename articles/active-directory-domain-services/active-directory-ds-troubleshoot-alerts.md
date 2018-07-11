---
title: 'Azure Active Directory Domain Services: sorun giderme uyarıları | Microsoft Docs'
description: Azure AD Domain Services için sorun giderme uyarıları
services: active-directory-ds
documentationcenter: ''
author: eringreenlee
manager: ''
editor: ''
ms.assetid: 54319292-6aa0-4a08-846b-e3c53ecca483
ms.service: active-directory
ms.component: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/28/2018
ms.author: ergreenl
ms.openlocfilehash: 360c6c98227e52f0540b00ef136888d3d143b9fb
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37951083"
---
# <a name="azure-ad-domain-services---troubleshoot-alerts"></a>Azure AD etki alanı Hizmetleri - sorun giderme uyarıları
Bu makalede, yönetilen etki alanınızda karşılaşabileceğiniz herhangi bir uyarı için sorun giderme kılavuzları sağlar.


Karşılık gelen sorun giderme adımlarını kimliği veya ileti uyarıyı seçin.

| **Uyarı Kimliği** | **Uyarı iletisi** | **Çözümleme** |
| --- | --- | :--- |
| AADDS001 | *İnternet üzerinden güvenli LDAP, yönetilen etki alanı için etkinleştirilir. Ancak 636 numaralı bağlantı noktasına erişim bir ağ güvenlik grubu kullanılarak kilitlenmemiş kilitli değil. Bu kullanıcı hesaplarını parola deneme yanılma saldırıları için yönetilen etki alanındaki getirebilir.* | [Yanlış güvenli LDAP yapılandırma](active-directory-ds-troubleshoot-ldaps.md) |
| AADDS100 | *Yönetilen etki alanınızla ilişkili Azure AD dizini silinmiş olabilir. Yönetilen etki alanı artık desteklenen bir yapılandırma değildir. Microsoft olamaz izleme, yönetme, düzeltme eki ve yönetilen Etki Alanınızla eşitleme.* | [Eksik dizin](#aadds100-missing-directory) |
| AADDS101 | *Azure AD etki alanı Hizmetleri, bir Azure AD B2C dizininde etkinleştirilemez.* | [Azure AD B2C, bu dizinde çalışıyor](#aadds101-azure-ad-b2c-is-running-in-this-directory) |
| AADDS102 | *Azure AD dizininizi Azure AD Domain Services düzgün çalışması gerekli bir hizmet sorumlusu silindi. Bu yapılandırma, izleme, yönetme, düzeltme eki, Microsoft'un yeteneğini etkiler ve yönetilen Etki Alanınızla eşitleme.* | [Hizmet sorumlusu eksik](active-directory-ds-troubleshoot-service-principals.md) |
| AADDS103 | *Azure AD Domain Services'i etkinleştirdiğiniz sanal ağı için IP adres aralığı ortak bir IP aralığı ' dir. Azure AD etki alanı Hizmetleri, özel bir IP adresi aralığı ile bir sanal ağda etkinleştirilmelidir. Bu yapılandırma, izleme, yönetme, düzeltme eki için Microsoft'un yeteneğini etkiler ve yönetilen Etki Alanınızla eşitleme.* | [Genel bir IP aralığı adresidir](#aadds103-address-is-in-a-public-ip-range) |
| AADDS104 | *Microsoft bu yönetilen etki alanı için etki alanı denetleyicilerine ulaşamıyoruz. Bu, sanal ağ yönetilen etki alanı erişimi engeller üzerinde yapılandırılan bir ağ güvenlik grubu (NSG) durum meydana. Başka bir olası neden, söz konusu bloklar gelen trafiği internet'ten kullanıcı tanımlı bir yolun olup olmadığını olmasıdır.* | [Ağ hatası](active-directory-ds-troubleshoot-nsg.md) |
| AADDS105 | *Uygulama kimliği ile "d87dcbc6-a371-462e-88e3-28ad15ec4e64" hizmet sorumlusu silindi ve yeniden oluşturulur. Yeniden oluşturarak tutarsız izinleri yönetilen etki alanınıza hizmet için gereken Azure AD etki alanı Hizmetleri kaynaklarında bırakır. Yönetilen etki alanınızda parola eşitlemeyi etkilenebilir.* | [Parola eşitlemesi uygulama güncel değil](active-directory-ds-troubleshoot-service-principals.md#alert-aadds105-password-synchronization-application-is-out-of-date) |
| AADDS500 | *Yönetilen etki alanı [date] üzerinde Azure AD ile son eşitlendiği zaman. Kullanıcılar yönetilen etki alanında oturum açamıyor veya grup üyeliklerini Azure AD ile eşitlenmiş olmayabilir.* | [Eşitleme, bir süredir oldu henüz](#aadds500-synchronization-has-not-completed-in-a-while) |
| AADDS501 | *Yönetilen etki alanı son [date] yedeklendi.* | [Bir yedek bir süredir kalmadığını](#aadds501-a-backup-has-not-been-taken-in-a-while) |
| AADDS502 | *Yönetilen etki alanı için güvenli LDAP sertifikasını [date] dolacak].* | [Güvenli LDAP sertifikasını süresi doluyor](active-directory-ds-troubleshoot-ldaps.md#aadds502-secure-ldap-certificate-expiring) |
| AADDS503 | *Etki alanı ile ilişkili Azure aboneliği etkin olmadığından, yönetilen etki alanı askıya alınır.* | [Devre dışı bırakılmış abonelik nedeniyle askıya alma](#aadds503-suspension-due-to-disabled-subscription) |
| AADDS504 | *Yönetilen etki alanı geçersiz bir yapılandırma nedeniyle askıya alınır. Hizmet, düzeltme eki, yönetme veya yönetilen etki alanınızın etki alanı denetleyicilerinin uzun bir süredir güncelleştirme olmuştur.* | [Geçersiz yapılandırma nedeniyle askıya alma](#aadds504-suspension-due-to-an-invalid-configuration) |


## <a name="aadds100-missing-directory"></a>AADDS100: Dizin eksik
**Uyarı iletisi:**

*Yönetilen etki alanınızla ilişkili Azure AD dizini silinmiş olabilir. Yönetilen etki alanı artık desteklenen bir yapılandırma değildir. Microsoft olamaz izleme, yönetme, düzeltme eki ve yönetilen Etki Alanınızla eşitleme.*

**Çözüm:**

Azure aboneliğiniz için yeni bir Azure yanlış taşıyarak bu hata genellikle kaynaklanır AD dizini ve eski silme hala Azure AD Domain Services ile ilişkili Azure AD dizini.

Bu hata onarılamaz. Gidermek için şunları yapmanız gerekir [mevcut yönetilen etki alanınızı silmek](active-directory-ds-disable-aadds.md) ve yeni dizininizde yeniden oluşturun. Silme konusunda sorun yaşıyorsanız, Azure Active Directory Domain Services ürün ekibiyle [desteği](active-directory-ds-contact-us.md).

## <a name="aadds101-azure-ad-b2c-is-running-in-this-directory"></a>AADDS101: Bu dizinde Azure AD B2C'yi çalıştıran
**Uyarı iletisi:**

*Azure AD etki alanı Hizmetleri, bir Azure AD B2C dizininde etkinleştirilemez.*

**Çözüm:**

>[!NOTE]
>Azure AD Domain Services'ı kullanmaya devam edebilmek için Azure AD Domain Services örneğinizin Azure AD B2C dizini yeniden oluşturmanız gerekir.

Hizmetinizin geri yüklemek için şu adımları izleyin:

1. [Yönetilen etki alanınızı silmek](active-directory-ds-disable-aadds.md) , mevcut Azure AD dizininden.
2. Azure AD B2C dizini değil. yeni bir dizin oluşturun.
3. İzleyin [Başlarken](active-directory-ds-getting-started.md) yönetilen bir etki alanı yeniden oluşturmak için Kılavuzu.

## <a name="aadds103-address-is-in-a-public-ip-range"></a>AADDS103: Genel bir IP aralığı adresidir

**Uyarı iletisi:**

*Azure AD Domain Services'i etkinleştirdiğiniz sanal ağı için IP adres aralığı ortak bir IP aralığı ' dir. Azure AD etki alanı Hizmetleri, özel bir IP adresi aralığı ile bir sanal ağda etkinleştirilmelidir. Bu yapılandırma, izleme, yönetme, düzeltme eki için Microsoft'un yeteneğini etkiler ve yönetilen Etki Alanınızla eşitleme.*

**Çözüm:**

> [!NOTE]
> Bu sorunu gidermek için mevcut bir yönetilen etki alanınızı silin ve bir sanal ağ özel bir IP adresi aralığı ile yeniden oluşturun. Bu işlem, kesintiye uğratan bir durumdur.

Başlamadan önce okumanız **özel IP v4 adresi alanı** konusundaki [bu makalede](https://en.wikipedia.org/wiki/Private_network#Private_IPv4_address_spaces).

Sanal ağ içinde makineler aynı IP adresi aralığı alt ağ için yapılandırılan olarak bulunan Azure kaynaklarını istekleri yapabilir. Ancak, sanal ağ, bu aralık için yapılandırılmış olduğundan, bu istekleri sanal ağ içinde yönlendirilir ve hedeflenen web kaynaklarını ulaşarak. Bu yapılandırma Azure AD Domain Services ile öngörülemeyen hatalara neden olabilir.

**Sanal ağınızda yapılandırılan internet IP adresi aralığında sahipseniz, bu uyarı yoksayılabilir. Ancak, Azure AD Domain Services için işleyemezsiniz [SLA](https://azure.microsoft.com/support/legal/sla/active-directory-ds/v1_0/)] öngörülemeyen hatalarına neden olabileceği bu yapılandırmaya sahip.**


1. [Yönetilen etki alanınızı silmek](active-directory-ds-disable-aadds.md) dizininizden.
2. Alt ağ için IP adresi aralığı Düzelt
  1. Gidin [Azure portalında sanal ağlar sayfasında](https://portal.azure.com/?feature.canmodifystamps=true&Microsoft_AAD_DomainServices=preview#blade/HubsExtension/Resources/resourceType/Microsoft.Network%2FvirtualNetworks).
  2. Azure AD Domain Services için kullanmayı planladığınız sanal ağı seçin.
  3. Tıklayarak **adres alanı** ayarları
  4. Adres aralığı mevcut adres aralığına tıklayarak ve düzenlemeye veya ek bir adres aralığı ekleme güncelleştirin. Yeni bir adres aralığı bir özel IP aralığında olduğundan emin olun. Yaptığınız değişiklikleri kaydedin.
  5. Tıklayarak **alt ağlar** sol taraftaki gezinti.
  6. Tablodaki düzenlemek istediğiniz alt ağa tıklayın.
  7. Adres aralığını güncelleştir ve değişikliklerinizi kaydedin.
3. İzleyin [alma başlatıldı kullanarak Azure AD Domain Services Kılavuzu](active-directory-ds-getting-started.md) yönetilen etki alanınıza yeniden oluşturmak için. Bir sanal ağ ile özel bir IP adresi aralığı çekme emin olun.
4. Yeni etki alanınız için sanal makinelerinizi etki alanına katılma için izleyin [bu kılavuzda](active-directory-ds-admin-guide-join-windows-vm-portal.md).
8. Uyarı çözümlendiğinde emin olmak için iki saat içinde etki alanınızın sistem durumunu denetleyin.

## <a name="aadds500-synchronization-has-not-completed-in-a-while"></a>AADDS500: Eşitleme bir süre içinde tamamlanmadı.

**Uyarı iletisi:**

*Yönetilen etki alanı [date] üzerinde Azure AD ile son eşitlendiği zaman. Kullanıcılar yönetilen etki alanında oturum açamıyor veya grup üyeliklerini Azure AD ile eşitlenmiş olmayabilir.*

**Çözüm:**

[Etki alanınızın sistem durumunu denetleme](active-directory-ds-check-health.md) yönetilen etki alanınızın yapılandırmanızda sorunlarını belirtebilecek herhangi bir uyarı için. Bazı durumlarda, yapılandırma sorunları, Microsoft'un yönetilen Etki Alanınızla eşitleme olanağı engelleyebilirsiniz. Tüm uyarıları çözümlemek bekleyin tamamlayabilirseniz eşitleme tamamlandıysa, görmek için iki saat ve onay yedekleyin.

Neden yönetilen etki alanlarında eşitleme durdurur bazı yaygın nedenler şunlardır:
- Ağ bağlantısı, yönetilen etki alanında engellenir. Nasıl olduğunu okuyun, ağ sorunlarını denetlemesi hakkında daha fazla bilgi için [ağ güvenlik gruplarında sorun giderme](active-directory-ds-troubleshoot-nsg.md) ve [Azure AD Domain Services için ağ gereksinimleri](active-directory-ds-networking.md).
-  Parola Eşitleme hiçbir zaman ayarlayın veya tamamlandı. Parola eşitlemeyi ayarlamak için okuma [bu makalede](active-directory-ds-getting-started-password-sync.md).

## <a name="aadds501-a-backup-has-not-been-taken-in-a-while"></a>AADDS501: Yedekleme bir süredir kullanımda değil

**Uyarı iletisi:**

*Yönetilen etki alanı son [date] yedeklendi.*

**Çözüm:**

[Etki alanınızın sistem durumunu denetleme](active-directory-ds-check-health.md) yönetilen etki alanınızın yapılandırmanızda sorunlarını belirtebilecek herhangi bir uyarı için. Bazı durumlarda, yapılandırma sorunları, Microsoft'un yönetilen Etki Alanınızla eşitleme olanağı engelleyebilirsiniz. Tüm uyarıları çözümlemek bekleyin tamamlayabilirseniz eşitleme tamamlandıysa, görmek için iki saat ve onay yedekleyin.


## <a name="aadds503-suspension-due-to-disabled-subscription"></a>AADDS503: Devre dışı bırakılmış abonelik nedeniyle askıya alma

**Uyarı iletisi:**

*Etki alanı ile ilişkili Azure aboneliği etkin olmadığından, yönetilen etki alanı askıya alınır.*

**Çözüm:**

> [!WARNING]
> Yönetilen etki alanınıza uzun bir süre için askıya alındı, silinmekte olan tehlikesi olur. Askıya alınması mümkün olan en kısa sürede gidermek idealdir. Daha fazla bilgi için ziyaret [bu makalede](active-directory-ds-suspension.md).

Hizmetinizin geri yüklemek için [Azure aboneliğinizi yenileyin](https://docs.microsoft.com/azure/billing/billing-subscription-become-disable) yönetilen Etki Alanınızla ilişkili.

## <a name="aadds504-suspension-due-to-an-invalid-configuration"></a>AADDS504: Geçersiz yapılandırma nedeniyle askıya alma

**Uyarı iletisi:**

*Yönetilen etki alanı geçersiz bir yapılandırma nedeniyle askıya alınır. Hizmet, düzeltme eki, yönetme veya yönetilen etki alanınızın etki alanı denetleyicilerinin uzun bir süredir güncelleştirme olmuştur.*

**Çözüm:**

> [!WARNING]
> Yönetilen etki alanınıza uzun bir süre için askıya alındı, silinmekte olan tehlikesi olur. Askıya alınması mümkün olan en kısa sürede gidermek idealdir. Daha fazla bilgi için ziyaret [bu makalede](active-directory-ds-suspension.md).

[Etki alanınızın sistem durumunu denetleme](active-directory-ds-check-health.md) yönetilen etki alanınızın yapılandırmanızda sorunlarını belirtebilecek herhangi bir uyarı için. Bu uyarı çözümleyebilir, bunu yapın. Sonra aboneliğinizi yeniden etkinleştirmek için desteğe başvurun.


## <a name="contact-us"></a>Bizimle iletişim kurun
Azure Active Directory Domain Services ürün ekibiyle [geri bildirim paylaşma veya destek](active-directory-ds-contact-us.md).
