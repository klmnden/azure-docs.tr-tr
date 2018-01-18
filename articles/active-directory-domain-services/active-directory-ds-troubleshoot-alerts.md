---
title: "Azure Active Directory etki alanı Hizmetleri: sorun giderme uyarıları | Microsoft Docs"
description: "Uyarılar için Azure AD etki alanı Hizmetleri sorunlarını giderme"
services: active-directory-ds
documentationcenter: 
author: eringreenlee
manager: 
editor: 
ms.assetid: 54319292-6aa0-4a08-846b-e3c53ecca483
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/16/2018
ms.author: ergreenl
ms.openlocfilehash: b2e0edf3588f3b1db5f4b6641019be1ded9cb50e
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/18/2018
---
# <a name="azure-ad-domain-services---troubleshoot-alerts"></a>Azure AD etki alanı Hizmetleri - sorun giderme uyarıları
Bu makalede, yönetilen etki alanınızda karşılaşabileceğiniz herhangi bir uyarı için sorun giderme kılavuzları sağlar.


Karşılık gelen sorun giderme adımlarını veya uyarı kimliği veya karşılaştığınız ileti seçin.

| **Uyarı Kimliği** | **Uyarı iletisi** | **Çözümleme** |
| --- | --- | :--- |
| AADDS001 | *Güvenli LDAP internet üzerinden yönetilen etki alanı için etkinleştirilir. Ancak, bağlantı noktası 636 erişimi bir ağ güvenlik grubu kullanılarak aşağı kilitli değil. Bu parola kaba kuvvet saldırıları yönetilen etki alanına kullanıcı hesaplarında getirebilir.* | [Güvenli LDAP yapılandırması hatalı](active-directory-ds-troubleshoot-ldaps.md) |
| AADDS100 | *Yönetilen etki alanı ile ilişkili Azure AD dizini silinmiş olabilir. Yönetilen etki alanı artık desteklenen bir yapılandırma değildir. Microsoft edemez izlemek, yönetmek, düzeltme eki ve yönetilen etki alanınızı eşitlemek.* | [Eksik dizin](#aadds100-missing-directory) |
| AADDS101 | *Azure AD etki alanı Hizmetleri Azure AD B2C dizini etkinleştirilemez.* | [Azure AD B2C bu dizinde çalışıyor](#aadds101-azure-ad-b2c-is-running-in-this-directory) |
| AADDS102 | *Azure AD dizininizi Azure AD etki alanı Hizmetleri düzgün çalışması gerekli bir hizmet sorumlusu silinmiş olabilir. Bu yapılandırma, izleme, yönetme, düzeltme eki, Microsoft'un yeteneğini etkiler ve yönetilen etki alanınızı eşitleyin.* | [Hizmet sorumlusu eksik](active-directory-ds-troubleshoot-service-principals.md) |
| AADDS103 | *Azure AD Etki Alanı Hizmetleri'ni etkinleştirdiğiniz sanal ağı için IP adres aralığı bir ortak IP aralığında değil. Azure AD etki alanı Hizmetleri, bir sanal ağdaki bir özel IP adres aralığı ile etkinleştirilmelidir. Bu yapılandırma, izleme, yönetme, düzeltme eki Microsoft'un yeteneğini etkiler ve yönetilen etki alanınızı eşitleyin.* | [Bir ortak IP aralığında adresidir](#aadds103-address-is-in-a-public-ip-range) |
| AADDS104 | *Microsoft, bu yönetilen etki alanı için etki alanı denetleyicilerinin ulaşmasını alamıyor. Bu sanal ağ blokları erişiminizi yönetilen etki alanı için yapılandırılmış bir ağ güvenlik grubu (NSG) durum. Başka bir olası neden, bu blokları gelen trafiğin internet'ten bir kullanıcı tanımlı yol olup olmadığını olmasıdır.* | [Ağ hatası](active-directory-ds-troubleshoot-nsg.md) |

## <a name="aadds100-missing-directory"></a>AADDS100: Dizin eksik
**Uyarı iletisi:**

*Yönetilen etki alanı ile ilişkili Azure AD dizini silinmiş olabilir. Yönetilen etki alanı artık desteklenen bir yapılandırma değildir. Microsoft edemez izlemek, yönetmek, düzeltme eki ve yönetilen etki alanınızı eşitlemek.*

**Düzeltme:**

Bu hata, genellikle Azure aboneliğiniz için yeni bir Azure yanlış taşıyarak kaynaklanır AD dizini ve eski silme hala Azure AD etki alanı Hizmetleri ile ilişkili Azure AD dizini.

Bu hata kurtarılamaz. Gidermek için şunları yapmanız gerekir [mevcut yönetilen etki alanınızı silme](active-directory-ds-disable-aadds.md) ve yeni dizininizde yeniden oluşturun. Silme konusunda sorun yaşıyorsanız, Azure Active Directory etki alanı Hizmetleri ürün ekibine başvurun [desteği için](active-directory-ds-contact-us.md).

## <a name="aadds101-azure-ad-b2c-is-running-in-this-directory"></a>AADDS101: Bu dizinde Azure AD B2C çalışıyor
**Uyarı iletisi:**

*Azure AD etki alanı Hizmetleri Azure AD B2C dizini etkinleştirilemez.*

**Düzeltme:**

>[!NOTE]
>Azure AD etki alanı Hizmetleri kullanmaya devam edebilmek için Azure AD etki alanı Hizmetleri örneğinizi Azure AD B2C dizini yeniden oluşturmanız gerekir.

Hizmetinizi geri yüklemek için aşağıdaki adımları izleyin:

1. [Yönetilen etki alanınızı silme](active-directory-ds-disable-aadds.md) mevcut Azure AD dizininizden.
2. Azure AD B2C dizini olmayan yeni bir dizin oluşturun.
3. İzleyin [Başlarken](active-directory-ds-getting-started.md) Kılavuzu yönetilen bir etki alanına yeniden oluşturun.

## <a name="aadds103-address-is-in-a-public-ip-range"></a>AADDS103: Bir ortak IP aralığında adresidir

**Uyarı iletisi:**

*Azure AD Etki Alanı Hizmetleri'ni etkinleştirdiğiniz sanal ağı için IP adres aralığı bir ortak IP aralığında değil. Azure AD etki alanı Hizmetleri, bir sanal ağdaki bir özel IP adres aralığı ile etkinleştirilmelidir. Bu yapılandırma, izleme, yönetme, düzeltme eki Microsoft'un yeteneğini etkiler ve yönetilen etki alanınızı eşitleyin.*

**Düzeltme:**

> [!NOTE]
> Bu sorunu gidermek için var olan yönetilen etki alanınızı silin ve bir sanal ağdaki bir özel IP adres aralığı ile yeniden oluşturun. Bu kesintiye uğratan işlemidir.

Başlamadan önce okuyun **özel IP v4 adresi alanı** bölümüne [bu makalede](https://en.wikipedia.org/wiki/Private_network#Private_IPv4_address_spaces).

1. [Yönetilen etki alanınızı silme](active-directory-ds-disable-aadds.md) dizininizden.
2. Alt ağ için IP adresi aralığı Düzelt
  1. Gidin [sanal ağlar Azure portal sayfasında](https://portal.azure.com/?feature.canmodifystamps=true&Microsoft_AAD_DomainServices=preview#blade/HubsExtension/Resources/resourceType/Microsoft.Network%2FvirtualNetworks).
  2. Azure AD etki alanı Hizmetleri için kullanmayı planladığınız sanal ağı seçin.
  3. Tıklayın **adres alanı** ayarlar altında
  4. Adres aralığı var olan adres aralığına tıklatarak ve düzenlemeyi veya ek adres aralığı ekleme güncelleştirin. Yeni adres aralığı bir özel IP aralığında olduğundan emin olun. Yaptığınız değişiklikleri kaydedin.
  5. Tıklayın **alt ağlar** sol gezinti içinde.
  6. Tabloda düzenlemek istediğiniz alt ağ'ı tıklatın.
  7. Adres aralığı güncelleştirin ve değişikliklerinizi kaydedin.
3. İzleyin [alma başlatıldı kullanarak Azure AD etki alanı Hizmetleri Kılavuzu](active-directory-ds-getting-started.md) yönetilen etki alanınızı yeniden oluşturmak için. Bir sanal ağ bir özel IP adres aralığı ile çekme emin olun.
4. Etki alanına katılma için yeni etki alanı, sanal makinelerinizin izleyin [bu kılavuzda](active-directory-ds-admin-guide-join-windows-vm-portal.md).
8. Etki alanınızın sistem durumunu adımlarını doğru tamamladığınızdan emin olmak için iki saat içinde denetleyin.


## <a name="contact-us"></a>Bizimle iletişim kurun
Azure Active Directory etki alanı Hizmetleri ürün ekibine başvurun [paylaşmak geri bildirim veya destek](active-directory-ds-contact-us.md).
