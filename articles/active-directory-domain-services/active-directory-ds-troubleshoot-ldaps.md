---
title: "Azure Active Directory etki alanı Hizmetleri: Güvenli LDAP yapılandırma sorunlarını giderme | Microsoft Docs"
description: "Güvenli LDAP için Azure AD Etki Alanı Hizmetleri'nde sorun giderme"
services: active-directory-ds
documentationcenter: 
author: eringreenlee
manager: 
editor: 
ms.assetid: 81208c0b-8d41-4f65-be15-42119b1b5957
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/17/2018
ms.author: ergreenl
ms.openlocfilehash: ad98f3fb1ddb753976be627764d34864e5bf3d50
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/18/2018
---
# <a name="azure-ad-domain-services---troubleshooting-secure-ldap-configuration"></a>Azure AD etki alanı Hizmetleri - sorun giderme güvenli LDAP yapılandırması

Bu makalede çözümler için ortak sağlar ne zaman sorunları [güvenli LDAP yapılandırma](active-directory-ds-admin-guide-configure-secure-ldap.md) Azure AD etki alanı Hizmetleri için.

## <a name="aadds101-secure-ldap-network-security-group-configuration"></a>AADDS101: Güvenli LDAP ağ güvenlik grubu yapılandırma

**Uyarı iletisi:**

*Güvenli LDAP internet üzerinden yönetilen etki alanı için etkinleştirilir. Ancak, bağlantı noktası 636 erişimi bir ağ güvenlik grubu kullanılarak aşağı kilitli değil. Bu parola kaba kuvvet saldırıları yönetilen etki alanına kullanıcı hesaplarında getirebilir.*

### <a name="secure-ldap-port"></a>Güvenli LDAP bağlantı noktası

Güvenli LDAP etkinleştirildiğinde, belirli IP adreslerinden yalnızca gelen LDAPS erişime izin vermek için ek kurallar oluşturmanızı öneririz. Bu kurallar etki alanınızda bir güvenlik tehdidi tehlikeli olabilecek deneme yanılma saldırılarına karşı koruyun. Bağlantı noktası 636, yönetilen etki alanına erişim sağlar. Güvenli LDAP için erişime izin vermek için NSG güncelleştirmek nasıl şöyledir:

1. Gidin [ağ güvenlik grupları sekmesinde](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.Network%2FNetworkSecurityGroups) Azure portalında
2. Tablodan, etki alanı ile ilişkili NSG seçin.
3. Tıklayın **gelen güvenlik kuralları**
4. Bağlantı noktası 636 kuralı oluşturma
   1. Tıklatın **Ekle** üst gezinti çubuğunda.
   2. Seçin **IP adreslerini** kaynağı.
   3. Bu kural için kaynak bağlantı noktası aralığı belirtin.
   4. Hedef bağlantı noktası aralıkları için giriş "636".
   5. Protokol **TCP**.
   6. Kural, uygun bir ad, açıklama ve öncelik verin. Varsa bu kuralın önceliğini "Deny tüm" kuralındaki önceliğe daha yüksek olmalıdır.
   7. **Tamam**’a tıklayın.
5. Kural oluşturulduğunu doğrulayın.
6. Etki alanınızın sistem durumunu adımlarını doğru tamamladığınızdan emin olmak için iki saat içinde denetleyin.


## <a name="contact-us"></a>Bizimle iletişim kurun
Azure Active Directory etki alanı Hizmetleri ürün ekibine başvurun [paylaşmak geri bildirim veya destek](active-directory-ds-contact-us.md).
