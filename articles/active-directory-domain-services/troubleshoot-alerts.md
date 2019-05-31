---
title: 'Azure Active Directory etki alanı Hizmetleri: Sorun giderme uyarıları | Microsoft Docs'
description: Azure AD Domain Services için sorun giderme uyarıları
services: active-directory-ds
documentationcenter: ''
author: MikeStephens-MS
manager: ''
editor: ''
ms.assetid: 54319292-6aa0-4a08-846b-e3c53ecca483
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/22/2019
ms.author: mstephen
ms.openlocfilehash: ed0ff8cde39f2660c2149e43399d937184620773
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66246712"
---
# <a name="azure-ad-domain-services---troubleshoot-alerts"></a>Azure AD etki alanı Hizmetleri - sorun giderme uyarıları
Bu makalede, yönetilen etki alanınızda karşılaşabileceğiniz herhangi bir uyarı için sorun giderme kılavuzları sağlar.


Karşılık gelen sorun giderme adımlarını kimliği veya ileti uyarıyı seçin.

| **Uyarı Kimliği** | **Uyarı iletisi** | **Çözümleme** |
| --- | --- | :--- |
| AADDS001 | *İnternet üzerinden güvenli LDAP, yönetilen etki alanı için etkinleştirilir. Ancak 636 numaralı bağlantı noktasına erişim bir ağ güvenlik grubu kullanılarak kilitlenmemiş kilitli değil. Bu kullanıcı hesaplarını parola deneme yanılma saldırıları için yönetilen etki alanındaki getirebilir.* | [Yanlış güvenli LDAP yapılandırma](alert-ldaps.md) |
| AADDS100 | *Yönetilen etki alanınızla ilişkili Azure AD dizini silinmiş olabilir. Yönetilen etki alanı artık desteklenen bir yapılandırma değildir. Microsoft olamaz izleme, yönetme, düzeltme eki ve yönetilen Etki Alanınızla eşitleme.* | [Eksik dizin](#aadds100-missing-directory) |
| AADDS101 | *Azure AD etki alanı Hizmetleri, bir Azure AD B2C dizininde etkinleştirilemez.* | [Azure AD B2C, bu dizinde çalışıyor](#aadds101-azure-ad-b2c-is-running-in-this-directory) |
| AADDS102 | *Azure AD dizininizi Azure AD Domain Services düzgün çalışması gerekli bir hizmet sorumlusu silindi. Bu yapılandırma, izleme, yönetme, düzeltme eki, Microsoft'un yeteneğini etkiler ve yönetilen Etki Alanınızla eşitleme.* | [Hizmet sorumlusu eksik](alert-service-principal.md) |
| AADDS103 | *Azure AD Domain Services'i etkinleştirdiğiniz sanal ağı için IP adres aralığı ortak bir IP aralığı ' dir. Azure AD etki alanı Hizmetleri, özel bir IP adresi aralığı ile bir sanal ağda etkinleştirilmelidir. Bu yapılandırma, izleme, yönetme, düzeltme eki için Microsoft'un yeteneğini etkiler ve yönetilen Etki Alanınızla eşitleme.* | [Genel bir IP aralığı adresidir](#aadds103-address-is-in-a-public-ip-range) |
| AADDS104 | *Microsoft bu yönetilen etki alanı için etki alanı denetleyicilerine ulaşamıyoruz. Bu, sanal ağ yönetilen etki alanı erişimi engeller üzerinde yapılandırılan bir ağ güvenlik grubu (NSG) durum meydana. Başka bir olası neden, söz konusu bloklar gelen trafiği internet'ten kullanıcı tanımlı bir yolun olup olmadığını olmasıdır.* | [Ağ hatası](alert-nsg.md) |
| AADDS105 | *Uygulama kimliği ile "d87dcbc6-a371-462e-88e3-28ad15ec4e64" hizmet sorumlusu silindi ve yeniden oluşturulur. Yeniden oluşturarak tutarsız izinleri yönetilen etki alanınıza hizmet için gereken Azure AD etki alanı Hizmetleri kaynaklarında bırakır. Yönetilen etki alanınızda parola eşitlemeyi etkilenebilir.* | [Parola eşitlemesi uygulama güncel değil](alert-service-principal.md#alert-aadds105-password-synchronization-application-is-out-of-date) |
| AADDS106 | *Yönetilen etki alanınızla ilişkili Azure aboneliğiniz silindi.  Azure AD Domain Services'in düzgün çalışmaya devam edebilmesi için etkin bir aboneliği gerektirir.* | [Azure aboneliği bulunamadı](#aadds106-your-azure-subscription-is-not-found) |
| AADDS107 | *Yönetilen etki alanınızla ilişkili Azure aboneliğinizi etkin değil.  Azure AD Domain Services'in düzgün çalışmaya devam edebilmesi için etkin bir aboneliği gerektirir.* | [Azure aboneliğiniz devre dışı](#aadds107-your-azure-subscription-is-disabled) |
| AADDS108 | *Azure AD Domain Services tarafından kullanılan abonelik başka bir dizinine taşındı. Azure AD Domain Services'in düzgün çalışması için aynı dizinde etkin bir aboneliğiniz olmalıdır.* | [Abonelik, taşınan dizinleri](#aadds108-subscription-moved-directories) |
| AADDS109 | *Yönetilen etki alanınız için kullanılan bir kaynağı sildi. Bu kaynak için Azure AD Domain Services düzgün çalışması gereklidir.* | [Bir kaynağı sildi](#aadds109-resources-for-your-managed-domain-cannot-be-found) |
| AADDS110 | *Azure AD Domain Services dağıtımı için seçilen alt ağda dolu ve oluşturulması gereken ek bir etki alanı denetleyicisi alan yok.* | [Alt ağ dolu](#aadds110-the-subnet-associated-with-your-managed-domain-is-full) |
| AADDS111 | *Azure AD Domain Services'ı kullanan hizmet etki alanınız için bir hizmet sorumlusu Azure aboneliğinde kaynakları yönetmek için yetkili değil. Yönetilen etki alanınıza hizmet izni kazanmak hizmet sorumlusu gerekir.* | [Hizmet sorumlusu yetkisiz](#aadds111-service-principal-unauthorized) |
| AADDS112 | *Bu etki alanındaki sanal ağın alt ağında yeterli IP adresi olmayabilir olduğunu belirledik. Azure AD Domain Services, en az iki kullanılabilir IP adresi, etkin alt ağ içinde olmalıdır. Sahip en az 3-5 yedek IP adresi alt ağ içinde öneririz. Böylece kullanılabilir IP adresleri veya alt ağdaki kullanılabilir IP adresi sayısı bir kısıtlama olup olmadığını tüketme alt ağda dağıtılan diğer sanal makineler varsa bu oluşmuş olabilir.* | [Yeterli IP adresleri](#aadds112-not-enough-ip-address-in-the-managed-domain) |
| AADDS113 | *Azure AD Domain Services tarafından kullanılan kaynakları beklenmeyen bir durumda algılandı ve geri alınamaz.* | [Kaynakları kurtarılamaz olarak kabul edilir](#aadds113-resources-are-unrecoverable) |
| AADDS114 | *Azure AD Domain Services dağıtımı için seçilen alt ağda geçerli değil ve kullanılamaz.* | [Geçersiz alt ağ](#aadds114-subnet-invalid) |
| AADDS115 | *Hedef kapsamı kilitli bir veya daha fazla yönetilen etki alanı tarafından kullanılan ağ kaynakları üzerinde çalışılamıyor.* | [Kaynak kilitli](#aadds115-resources-are-locked) |
| AADDS116 | *Bir veya daha fazla yönetilen etki alanı tarafından kullanılan ağ kaynakları üzerinde nedeniyle ilke restriction(s) çalışılamıyor.* | [Kaynak kullanılamıyor](#aadds116-resources-are-unusable) |
| AADDS500 | *Yönetilen etki alanı [date] üzerinde Azure AD ile son eşitlendiği zaman. Kullanıcılar yönetilen etki alanında oturum açamıyor veya grup üyeliklerini Azure AD ile eşitlenmiş olmayabilir.* | [Eşitleme, bir süredir oldu henüz](#aadds500-synchronization-has-not-completed-in-a-while) |
| AADDS501 | *Yönetilen etki alanı son [date] yedeklendi.* | [Bir yedek bir süredir kalmadığını](#aadds501-a-backup-has-not-been-taken-in-a-while) |
| AADDS502 | *Yönetilen etki alanı için güvenli LDAP sertifikasını [date] sona erecek.* | [Güvenli LDAP sertifikasını süresi doluyor](alert-ldaps.md#aadds502-secure-ldap-certificate-expiring) |
| AADDS503 | *Etki alanı ile ilişkili Azure aboneliği etkin olmadığından, yönetilen etki alanı askıya alınır.* | [Devre dışı bırakılmış abonelik nedeniyle askıya alma](#aadds503-suspension-due-to-disabled-subscription) |
| AADDS504 | *Yönetilen etki alanı geçersiz bir yapılandırma nedeniyle askıya alınır. Hizmet, düzeltme eki, yönetme veya yönetilen etki alanınızın etki alanı denetleyicilerinin uzun bir süredir güncelleştirme olmuştur.* | [Geçersiz yapılandırma nedeniyle askıya alma](#aadds504-suspension-due-to-an-invalid-configuration) |



## <a name="aadds100-missing-directory"></a>AADDS100: Eksik dizin
**Uyarı iletisi:**

*Yönetilen etki alanınızla ilişkili Azure AD dizini silinmiş olabilir. Yönetilen etki alanı artık desteklenen bir yapılandırma değildir. Microsoft olamaz izleme, yönetme, düzeltme eki ve yönetilen Etki Alanınızla eşitleme.*

**Çözüm:**

Azure aboneliğiniz için yeni bir Azure yanlış taşıyarak bu hata genellikle kaynaklanır AD dizini ve eski silme hala Azure AD Domain Services ile ilişkili Azure AD dizini.

Bu hata onarılamaz. Gidermek için şunları yapmanız gerekir [mevcut yönetilen etki alanınızı silmek](delete-aadds.md) ve yeni dizininizde yeniden oluşturun. Silme konusunda sorun yaşıyorsanız, Azure Active Directory Domain Services ürün ekibiyle [desteği](contact-us.md).

## <a name="aadds101-azure-ad-b2c-is-running-in-this-directory"></a>AADDS101: Azure AD B2C, bu dizinde çalışıyor
**Uyarı iletisi:**

*Azure AD etki alanı Hizmetleri, bir Azure AD B2C dizininde etkinleştirilemez.*

**Çözüm:**

>[!NOTE]
>Azure AD Domain Services'ı kullanmaya devam edebilmek için Azure AD Domain Services örneğinizin Azure AD B2C dizini yeniden oluşturmanız gerekir.

Hizmetinizin geri yüklemek için şu adımları izleyin:

1. [Yönetilen etki alanınızı silmek](delete-aadds.md) , mevcut Azure AD dizininden.
2. Azure AD B2C dizini değil. yeni bir dizin oluşturun.
3. İzleyin [Başlarken](create-instance.md) yönetilen bir etki alanı yeniden oluşturmak için Kılavuzu.

## <a name="aadds103-address-is-in-a-public-ip-range"></a>AADDS103: Genel bir IP aralığı adresidir

**Uyarı iletisi:**

*Azure AD Domain Services'i etkinleştirdiğiniz sanal ağı için IP adres aralığı ortak bir IP aralığı ' dir. Azure AD etki alanı Hizmetleri, özel bir IP adresi aralığı ile bir sanal ağda etkinleştirilmelidir. Bu yapılandırma, izleme, yönetme, düzeltme eki için Microsoft'un yeteneğini etkiler ve yönetilen Etki Alanınızla eşitleme.*

**Çözüm:**

> [!NOTE]
> Bu sorunu gidermek için mevcut bir yönetilen etki alanınızı silin ve bir sanal ağ özel bir IP adresi aralığı ile yeniden oluşturun. Bu işlem, kesintiye uğratan bir durumdur.

Başlamadan önce okumanız **özel IP v4 adresi alanı** konusundaki [bu makalede](https://en.wikipedia.org/wiki/Private_network#Private_IPv4_address_spaces).

Sanal ağ içinde makineler aynı IP adresi aralığı alt ağ için yapılandırılan olarak bulunan Azure kaynaklarını istekleri yapabilir. Ancak, sanal ağ, bu aralık için yapılandırılmış olduğundan, bu istekleri sanal ağ içinde yönlendirilir ve hedeflenen web kaynaklarını ulaşarak. Bu yapılandırma Azure AD Domain Services ile öngörülemeyen hatalara neden olabilir.

**Sanal ağınızda yapılandırılan internet IP adresi aralığında sahipseniz, bu uyarı yoksayılabilir. Ancak, Azure AD Domain Services için işleyemezsiniz [SLA](https://azure.microsoft.com/support/legal/sla/active-directory-ds/v1_0/)] öngörülemeyen hatalarına neden olabileceği bu yapılandırmaya sahip.**


1. [Yönetilen etki alanınızı silmek](delete-aadds.md) dizininizden.
2. Alt ağ için IP adresi aralığı Düzelt
   1. Gidin [Azure portalında sanal ağlar sayfasında](https://portal.azure.com/?feature.canmodifystamps=true&Microsoft_AAD_DomainServices=preview#blade/HubsExtension/Resources/resourceType/Microsoft.Network%2FvirtualNetworks).
   2. Azure AD Domain Services için kullanmayı planladığınız sanal ağı seçin.
   3. Tıklayarak **adres alanı** ayarları
   4. Adres aralığı mevcut adres aralığına tıklayarak ve düzenlemeye veya ek bir adres aralığı ekleme güncelleştirin. Yeni bir adres aralığı bir özel IP aralığında olduğundan emin olun. Yaptığınız değişiklikleri kaydedin.
   5. Tıklayarak **alt ağlar** sol taraftaki gezinti.
   6. Tablodaki düzenlemek istediğiniz alt ağa tıklayın.
   7. Adres aralığını güncelleştir ve değişikliklerinizi kaydedin.
3. İzleyin [alma başlatıldı kullanarak Azure AD Domain Services Kılavuzu](create-instance.md) yönetilen etki alanınıza yeniden oluşturmak için. Bir sanal ağ ile özel bir IP adresi aralığı çekme emin olun.
4. Yeni etki alanınız için sanal makinelerinizi etki alanına katılma için izleyin [bu kılavuzda](join-windows-vm.md).
8. Uyarı çözümlendiğinde emin olmak için iki saat içinde etki alanınızın sistem durumunu denetleyin.

## <a name="aadds106-your-azure-subscription-is-not-found"></a>AADDS106: Azure aboneliğinizi bulunamadı

**Uyarı iletisi:**

*Yönetilen etki alanınızla ilişkili Azure aboneliğiniz silindi.  Azure AD Domain Services'in düzgün çalışmaya devam edebilmesi için etkin bir aboneliği gerektirir.*

**Çözüm:**

Azure AD Domain Services işlevi aboneliği gerektirir ve başka bir aboneliğe taşınamaz. Yönetilen etki alanınızla ilişkili Azure aboneliği silindiği için bir Azure aboneliği ve Azure AD Domain Services'ı yeniden oluşturmanız gerekir.

1. Bir Azure aboneliği oluşturun
2. [Yönetilen etki alanınızı silmek](delete-aadds.md) , mevcut Azure AD dizininden.
3. İzleyin [Başlarken](create-instance.md) yönetilen bir etki alanı yeniden oluşturmak için Kılavuzu.

## <a name="aadds107-your-azure-subscription-is-disabled"></a>AADDS107: Azure aboneliğiniz devre dışı

**Uyarı iletisi:**

*Yönetilen etki alanınızla ilişkili Azure aboneliğinizi etkin değil.  Azure AD Domain Services'in düzgün çalışmaya devam edebilmesi için etkin bir aboneliği gerektirir.*

**Çözüm:**


1. [Azure aboneliğinizi yenileyin](https://docs.microsoft.com/azure/billing/billing-subscription-become-disable).
2. Abonelik yenilendikten sonra Azure AD Domain Services yönetilen etki alanınıza yeniden etkinleştirmek için Azure'nın sunduğu bir bildirim alırsınız.

## <a name="aadds108-subscription-moved-directories"></a>AADDS108: Abonelik, taşınan dizinleri

**Uyarı iletisi:**

*Azure AD Domain Services tarafından kullanılan abonelik başka bir dizinine taşındı. Azure AD Domain Services'in düzgün çalışması için aynı dizinde etkin bir aboneliğiniz olmalıdır.*

**Çözüm:**

Önceki dizinine Azure AD Domain Services ile ilişkili aboneliği ya da taşıyabilir veya ihtiyacınız [yönetilen etki alanınızı silmek](delete-aadds.md) mevcut dizinden ve seçtiğiniz dizinde oluşturun (ile birlikte bir Yeni Abonelik veya değişiklik dizini Azure AD Domain Services örneğinizin bulunduğu).

## <a name="aadds109-resources-for-your-managed-domain-cannot-be-found"></a>AADDS109: Yönetilen etki alanınız için kaynaklar bulunamıyor

**Uyarı iletisi:**

*Yönetilen etki alanınız için kullanılan bir kaynağı sildi. Bu kaynak için Azure AD Domain Services düzgün çalışması gereklidir.*

**Çözüm:**

Azure AD etki alanı Hizmetleri oluşturur belirli kaynaklar düzgün çalışabilmesi için dağıtırken genel IP adresleri, NIC ve bir yük dengeleyici gibi. Herhangi bir adlandırılmış silindiğinde, bu desteklenmeyen bir durumda olması için yönetilen etki alanınızı neden olur ve yönetilmekte olan etki alanınıza engeller. Bu uyarı ne zaman Azure AD Domain Services kaynaklarını bulabildiği birisi gerekli kaynak siler bulunur. Aşağıdaki adımlar, yönetilen etki alanınıza geri yükleme özetlemektedir.

1. Azure AD Domain Services durumu sayfasına gidin
   1.    İçin seyahat [Azure AD Domain Services sayfası](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.AAD%2FdomainServices) Azure portalında.
   2.    Sol gezinti bölmesinde tıklayın **sistem durumu**
2. Uyarı 4 saatten eski olup olmadığını denetleyin
   1.    Kimlikli uyarı sistem durumu sayfasında, tıklayın **AADDS109**
   2.    Uyarı olduğunda, ilk bulunan için bir zaman damgasına sahip olur. Zaman damgası 4 saatten kısa bir süre önce Azure AD Domain Services silinen kaynak yeniden oluşturabileceğinizi şansı yoktur.
3. Uyarı 4 saatten eski ise, yönetilen etki alanı kurtarılamaz bir durumda değil. Azure AD Domain Services silip yeniden açmanız gerekir.


## <a name="aadds110-the-subnet-associated-with-your-managed-domain-is-full"></a>AADDS110: Yönetilen etki alanınızla ilişkili alt ağ dolu

**Uyarı iletisi:**

*Azure AD Domain Services dağıtımı için seçilen alt ağda dolu ve oluşturulması gereken ek bir etki alanı denetleyicisi alan yok.*

**Çözüm:**

Bu hata onarılamaz. Gidermek için şunları yapmanız gerekir [mevcut yönetilen etki alanınızı silmek](delete-aadds.md) ve [yönetilen etki alanınıza yeniden oluşturun](create-instance.md)

## <a name="aadds111-service-principal-unauthorized"></a>AADDS111: Hizmet sorumlusu yetkisiz

**Uyarı iletisi:**

*Azure AD Domain Services'ı kullanan hizmet etki alanınız için bir hizmet sorumlusu Azure aboneliğinde kaynakları yönetmek için yetkili değil. Yönetilen etki alanınıza hizmet izni kazanmak hizmet sorumlusu gerekir.*

**Çözüm:**

Bizim hizmet sorumluları, yönetilen etki alanınızda kaynakları oluşturmak ve yönetmek için erişim gerekir. Birisi, hizmet sorumlusu erişimi reddedildi ve artık kaynakları yönetmek yüklenemiyor. Hizmet sorumlusu erişimi vermek için adımları izleyin.

1. Hakkında bilgi edinin [RBAC denetimi ve uygulamalara Azure portalında erişim izni verme hakkında](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal)
2. Erişim gözden geçirin, hizmet sorumlusu kimliği ile ```abba844e-bc0e-44b0-947a-dc74e5d09022``` ve daha önceki bir tarihte reddedildi erişim.


## <a name="aadds112-not-enough-ip-address-in-the-managed-domain"></a>AADDS112: Yönetilen etki alanında yeterli IP adresi

**Uyarı iletisi:**

*Bu etki alanındaki sanal ağın alt ağında yeterli IP adresi olmayabilir olduğunu belirledik. Azure AD Domain Services, en az iki kullanılabilir IP adresi, etkin alt ağ içinde olmalıdır. Sahip en az 3-5 yedek IP adresi alt ağ içinde öneririz. Böylece kullanılabilir IP adresleri veya alt ağdaki kullanılabilir IP adresi sayısı bir kısıtlama olup olmadığını tüketme alt ağda dağıtılan diğer sanal makineler varsa bu oluşmuş olabilir.*

**Çözüm:**

1. Yönetilen etki alanınıza kiracınızdan silin.
2. Alt ağ için IP adresi aralığı Düzelt
   1. Gidin [Azure portalında sanal ağlar sayfasında](https://portal.azure.com/?feature.canmodifystamps=true&Microsoft_AAD_DomainServices=preview#blade/HubsExtension/Resources/resourceType/Microsoft.Network%2FvirtualNetworks).
   2. Azure AD Domain Services için kullanmayı planladığınız sanal ağı seçin.
   3. Tıklayarak **adres alanı** ayarları
   4. Adres aralığı mevcut adres aralığına tıklayarak ve düzenlemeye veya ek bir adres aralığı ekleme güncelleştirin. Yaptığınız değişiklikleri kaydedin.
   5. Tıklayarak **alt ağlar** sol taraftaki gezinti.
   6. Tablodaki düzenlemek istediğiniz alt ağa tıklayın.
   7. Adres aralığını güncelleştir ve değişikliklerinizi kaydedin.
3. İzleyin [alma başlatıldı kullanarak Azure AD Domain Services Kılavuzu](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-getting-started) yönetilen etki alanınıza yeniden oluşturmak için. Bir sanal ağ ile özel bir IP adresi aralığı çekme emin olun.
4. Yeni etki alanınız için sanal makinelerinizi etki alanına katılma için izleyin [bu kılavuzda](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-admin-guide-join-windows-vm-portal).
5. Adımları doğru şekilde tamamladığınızdan emin olmak için iki saat içinde etki alanınızın sistem durumunu denetleyin.

## <a name="aadds113-resources-are-unrecoverable"></a>AADDS113: Kaynakları kurtarılamaz olarak kabul edilir

**Uyarı iletisi:**

*Azure AD Domain Services tarafından kullanılan kaynakları beklenmeyen bir durumda algılandı ve geri alınamaz.*

**Çözüm:**

Bu hata onarılamaz. Gidermek için şunları yapmanız gerekir [mevcut yönetilen etki alanınızı silmek](delete-aadds.md) ve [yönetilen etki alanınıza yeniden](create-instance.md).

## <a name="aadds114-subnet-invalid"></a>AADDS114: Geçersiz alt ağ

**Uyarı iletisi:**

*Azure AD Domain Services dağıtımı için seçilen alt ağda geçerli değil ve kullanılamaz.*

**Çözüm:**

Bu hata onarılamaz. Gidermek için şunları yapmanız gerekir [mevcut yönetilen etki alanınızı silmek](delete-aadds.md) ve [yönetilen etki alanınıza yeniden](create-instance.md).

## <a name="aadds115-resources-are-locked"></a>AADDS115: Kaynak kilitli

**Uyarı iletisi:**

*Hedef kapsamı kilitli bir veya daha fazla yönetilen etki alanı tarafından kullanılan ağ kaynakları üzerinde çalışılamıyor.*

**Çözüm:**

1.  Gözden geçirme Resource Manager işlem günlükleri ağ kaynaklarına (Bu bilgileri size değişiklik hangi kilidi engelliyor).
2.  Azure AD Domain Services hizmet sorumlusu, bunlar üzerinde çalışabilir, böylece kilitler kaynakları kaldırın.

## <a name="aadds116-resources-are-unusable"></a>AADDS116: Kaynak kullanılamıyor

**Uyarı iletisi:**

*Bir veya daha fazla yönetilen etki alanı tarafından kullanılan ağ kaynakları üzerinde nedeniyle ilke restriction(s) çalışılamıyor.*

**Çözüm:**

1.  Kaynak Yöneticisi gözden geçirme işlemi yönetilen etki alanınız için ağ kaynaklarına oturum açar.
2.  AAD-DS hizmet sorumlusu, bunlar üzerinde çalışabilir, böylece kaynaklar üzerindeki ilke kısıtlamaları zayıflatabilir.



## <a name="aadds500-synchronization-has-not-completed-in-a-while"></a>AADDS500: Eşitleme bir süre içinde tamamlanmadı.

**Uyarı iletisi:**

*Yönetilen etki alanı [date] üzerinde Azure AD ile son eşitlendiği zaman. Kullanıcılar yönetilen etki alanında oturum açamıyor veya grup üyeliklerini Azure AD ile eşitlenmiş olmayabilir.*

**Çözüm:**

[Etki alanınızın sistem durumunu denetleme](check-health.md) yönetilen etki alanınızın yapılandırmanızda sorunlarını belirtebilecek herhangi bir uyarı için. Bazı durumlarda, yapılandırma sorunları, Microsoft'un yönetilen Etki Alanınızla eşitleme olanağı engelleyebilirsiniz. Tüm uyarıları çözümlemek bekleyin tamamlayabilirseniz eşitleme tamamlandıysa, görmek için iki saat ve onay yedekleyin.

Neden yönetilen etki alanlarında eşitleme durdurur bazı yaygın nedenler şunlardır:
- Ağ bağlantısı, yönetilen etki alanında engellenir. Nasıl olduğunu okuyun, ağ sorunlarını denetlemesi hakkında daha fazla bilgi için [ağ güvenlik gruplarında sorun giderme](alert-nsg.md) ve [Azure AD Domain Services için ağ gereksinimleri](network-considerations.md).
-  Parola Eşitleme hiçbir zaman ayarlayın veya tamamlandı. Parola eşitlemeyi ayarlamak için okuma [bu makalede](active-directory-ds-getting-started-password-sync.md).

## <a name="aadds501-a-backup-has-not-been-taken-in-a-while"></a>AADDS501: Bir yedek bir süredir kullanımda değil

**Uyarı iletisi:**

*Yönetilen etki alanı son [date] yedeklendi.*

**Çözüm:**

[Etki alanınızın sistem durumunu denetleme](check-health.md) yönetilen etki alanınızın yapılandırmanızda sorunlarını belirtebilecek herhangi bir uyarı için. Bazı durumlarda, yapılandırma sorunları, Microsoft'un yönetilen etki alanınıza Yedekleme olanağı engelleyebilirsiniz. Tüm uyarıları çözümlemek bekleyin tamamlayabilirseniz yedekleme tamamlandıysa, görmek için iki saat ve onay yedekleyin.


## <a name="aadds503-suspension-due-to-disabled-subscription"></a>AADDS503: Devre dışı bırakılmış abonelik nedeniyle askıya alma

**Uyarı iletisi:**

*Etki alanı ile ilişkili Azure aboneliği etkin olmadığından, yönetilen etki alanı askıya alınır.*

**Çözüm:**

> [!WARNING]
> Yönetilen etki alanınıza uzun bir süre için askıya alındı, silinmekte olan tehlikesi olur. Askıya alınması mümkün olan en kısa sürede gidermek idealdir. Daha fazla bilgi için ziyaret [bu makalede](suspension.md).

Hizmetinizin geri yüklemek için [Azure aboneliğinizi yenileyin](https://docs.microsoft.com/azure/billing/billing-subscription-become-disable) yönetilen Etki Alanınızla ilişkili.

## <a name="aadds504-suspension-due-to-an-invalid-configuration"></a>AADDS504: Geçersiz yapılandırma nedeniyle askıya alma

**Uyarı iletisi:**

*Yönetilen etki alanı geçersiz bir yapılandırma nedeniyle askıya alınır. Hizmet, düzeltme eki, yönetme veya yönetilen etki alanınızın etki alanı denetleyicilerinin uzun bir süredir güncelleştirme olmuştur.*

**Çözüm:**

> [!WARNING]
> Yönetilen etki alanınıza uzun bir süre için askıya alındı, silinmekte olan tehlikesi olur. Askıya alınması mümkün olan en kısa sürede gidermek idealdir. Daha fazla bilgi için ziyaret [bu makalede](suspension.md).

[Etki alanınızın sistem durumunu denetleme](check-health.md) yönetilen etki alanınızın yapılandırmanızda sorunlarını belirtebilecek herhangi bir uyarı için. Bu uyarı çözümleyebilir, bunu yapın. Sonra aboneliğinizi yeniden etkinleştirmek için desteğe başvurun.


## <a name="contact-us"></a>Bizimle iletişim kurun
Azure Active Directory Domain Services ürün ekibiyle [geri bildirim paylaşma veya destek](contact-us.md).
