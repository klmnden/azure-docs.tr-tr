---
title: Güvenli LDAP (LDAPS) Azure AD Etki Alanı Hizmetleri'nde sorun giderme | Microsoft Docs
description: Bir Azure AD Domain Services yönetilen etki alanı için güvenli LDAP (LDAPS) sorunlarını giderme
services: active-directory-ds
documentationcenter: ''
author: eringreenlee
manager: daveba
editor: curtand
ms.assetid: 445c60da-e115-447b-841d-96739975bdf6
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 08/01/2018
ms.author: ergreenl
ms.openlocfilehash: 97a2ee23435d0b29565bc3bf1dcb426e9e83fd31
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60416764"
---
# <a name="troubleshoot-secure-ldap-ldaps-for-an-azure-ad-domain-services-managed-domain"></a>Bir Azure AD Domain Services yönetilen etki alanı için güvenli LDAP (LDAPS) sorunlarını giderme

## <a name="connection-issues"></a>Bağlantı sorunları
Güvenli LDAP kullanarak yönetilen etki alanına bağlanırken sorun varsa:

* Güvenli LDAP sertifikasını veren zincirini istemcide güvenilir olması gerekir. Kök sertifika yetkilisi güven oluşturmak için istemcide güvenilen kök sertifika deposuna eklemek tercih edebilirsiniz.
* LDAP istemcisi (örneğin, ldp.exe) bir DNS adı veya IP adresini kullanarak güvenli LDAP uç noktasına bağlandığını doğrulayın.
* LDAP istemcisi bağlandığı DNS adını denetleyin. Yönetilen etki alanı üzerinde güvenli LDAP'yi için genel IP adresine çözümlenmelidir.
* Yönetilen etki alanınıza güvenli LDAP sertifikası konu veya konu diğer adları özniteliği bir DNS adı olduğunu doğrulayın.
* Sanal ağ için NSG ayarları, internet'ten bağlantı noktası 636 trafiğine izin vermelidir. Yalnızca internet üzerinden güvenli LDAP erişimi etkinleştirdiyseniz bu adımı geçerlidir.


## <a name="need-help"></a>Yardıma mı ihtiyacınız var?
Güvenli LDAP kullanarak yönetilen etki alanına bağlanırken sorun yaşamaya devam ediyorsanız [ürün ekibiyle](active-directory-ds-contact-us.md) Yardım. Daha iyi sorunun tanılanmasına yardımcı olmak için aşağıdaki bilgileri ekleyin:
* Bağlantı oluşturma ve başarısız Ldp.exe'yi görüntüsü.
* Azure AD Kiracı Kimliğinizi ve yönetilen etki alanınızın DNS etki alanı adı.
* Bağlama olarak çalıştığınız tam kullanıcı adı.


## <a name="related-content"></a>İlgili içerik
* [Azure AD etki alanı Hizmetleri - başlangıç kılavuzu](active-directory-ds-getting-started.md)
* [Azure AD Domain Services tarafından yönetilen etki alanını yönetme](active-directory-ds-admin-guide-administer-domain.md)
* [LDAP sorgu temelleri](https://technet.microsoft.com/library/aa996205.aspx)
* [Azure AD Domain Services yönetilen etki alanında Grup İlkesi yönetme](active-directory-ds-admin-guide-administer-group-policy.md)
* [Ağ güvenlik grupları](../virtual-network/security-overview.md)
* [Ağ güvenlik grubu oluşturma](../virtual-network/tutorial-filter-network-traffic.md)
