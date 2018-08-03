---
title: Güvenli LDAP (LDAPS) kullanarak bir Azure AD Domain Services yönetilen etki bağlama | Microsoft Docs
description: Güvenli LDAP (LDAPS) kullanarak bir Azure AD Domain Services yönetilen etki bağlama
services: active-directory-ds
documentationcenter: ''
author: mahesh-unnikrishnan
manager: mtillman
editor: curtand
ms.assetid: 6871374a-0300-4275-9a45-a39a52c65ae4
ms.service: active-directory
ms.component: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/01/2018
ms.author: maheshu
ms.openlocfilehash: b5ddc44f0011cc90bbfe2a1bdd32e45d44f4b16a
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39460194"
---
# <a name="bind-to-an-azure-ad-domain-services-managed-domain-using-secure-ldap-ldaps"></a>Güvenli LDAP (LDAPS) kullanarak bir Azure AD Domain Services yönetilen etki bağlama

## <a name="before-you-begin"></a>Başlamadan önce
Tam [görev 4 - yönetilen etki alanı internet'ten erişmek için DNS'yi yapılandırma](active-directory-ds-ldaps-configure-dns.md).


## <a name="task-5-bind-to-the-managed-domain-over-ldap-using-ldpexe"></a>5. Görev: yönetilen etki alanına üzerinden Ldp.exe'yi kullanma LDAP bağlama
Bağlama ve üzerinden LDAP arama yapmak için Uzak Sunucu Yönetim Araçları Paketi dahil edilen LDP.exe aracını kullanabilirsiniz.

İlk olarak, LDP'yi açmak ve yönetilen etki alanına bağlanın. Tıklayın **bağlantı** tıklatıp **Bağlan...**  menüsünde. Yönetilen etki alanı DNS etki alanı adını belirtin. Bağlantılar için kullanılacak bağlantı noktasını belirtin. LDAP bağlantıları için bağlantı noktası 389 kullanın. LDAPS bağlantıları için bağlantı noktası 636'ı kullanın. Tıklayın **Tamam** yönetilen etki bağlama düğmesi.

Ardından, yönetilen etki alanına bağlayın. Tıklayın **bağlantı** tıklatıp **Bağla...**  menüsünde. 'AAD DC Administrators' grubuna ait olan bir kullanıcı hesabının kimlik bilgilerini sağlayın.

Seçin **görünümü**ve ardından **ağaç** menüsünde. Temel DN alanı boş bırakın ve Tamam'ı tıklatın. Arama, kapsayıcıya sağ tıklayın ve arama seçmek istediğiniz kapsayıcıya gidin.

> [!TIP]
> - Kullanıcıları ve grupları Azure AD'den eşitlenen depolanır **AADDC Users** kapsayıcı. Bu kapsayıcı için arama yolu benzer ```CN=AADDC\ Users,DC=CONTOSO100,DC=COM```.
> - Yönetilen etki alanına katılmış bilgisayarlar için bilgisayar hesaplarının depolanır **AADDC Computers** kapsayıcı. Bu kapsayıcı için arama yolu benzer ```CN=AADDC\ Computers,DC=CONTOSO100,DC=COM```.
>
>

Daha fazla bilgi - [LDAP sorgu temelleri](https://technet.microsoft.com/library/aa996205.aspx)


## <a name="task-6-lock-down-secure-ldap-access-to-your-managed-domain-over-the-internet"></a>6. Görev: Yönetilen etki alanınıza internet üzerinden güvenli LDAP erişimi kilitleme
> [!NOTE]
> İnternet üzerinden yönetilen etki alanına erişim LDAPS'ı etkinleştirmediyseniz bu yapılandırma görevi atlayın.
>
>

Bu göreve başlamadan önce tam adımlar bölümünde özetlenen [görev 3](active-directory-ds-admin-guide-configure-secure-ldap-enable-ldaps.md).

Yönetilen etki alanınıza internet üzerinden LDAPS erişimi etkinleştirdiğinizde, güvenlik tehdidi oluşturur. Yönetilen etki alanı için güvenli LDAP kullanılan bağlantı noktası internet'ten erişilebilen (diğer bir deyişle, bağlantı noktası 636). Yönetilen etki alanında bilinen belirli IP adreslerine erişimi kısıtlamak seçebilirsiniz. Bir ağ güvenlik grubu (NSG) oluşturun ve Azure AD Domain Services'i etkinleştirdiğiniz alt ağ ile ilişkilendirebilirsiniz.

Örnek NSG aşağıdaki tabloda, internet üzerinden güvenli LDAP erişimini kilitler. NSG kuralları, TCP bağlantı noktası IP adresleri üzerinden 636 yalnızca belirtilen bir kümesinden gelen güvenli LDAP erişimini izin. Varsayılan 'DenyAll' Kural diğer gelen tüm trafiği internet'ten geçerlidir. Belirtilen IP adresleri internet üzerinden LDAPS erişime izin vermek için NSG kuralı DenyAll NSG kuralı daha yüksek bir önceliğe sahiptir.

![Örnek internet üzerinden LDAPS erişimin güvenliğini sağlamak için NSG](./media/active-directory-domain-services-admin-guide/secure-ldap-sample-nsg.png)

**Daha fazla bilgi** - [ağ güvenlik grupları](../virtual-network/security-overview.md).


## <a name="related-content"></a>İlgili içerik
* [Azure AD etki alanı Hizmetleri - başlangıç kılavuzu](active-directory-ds-getting-started.md)
* [Azure AD Domain Services tarafından yönetilen etki alanını yönetme](active-directory-ds-admin-guide-administer-domain.md)
* [LDAP sorgu temelleri](https://technet.microsoft.com/library/aa996205.aspx)
* [Azure AD Domain Services yönetilen etki alanında Grup İlkesi yönetme](active-directory-ds-admin-guide-administer-group-policy.md)
* [Ağ güvenlik grupları](../virtual-network/security-overview.md)
* [Ağ güvenlik grubu oluşturma](../virtual-network/tutorial-filter-network-traffic.md)


## <a name="next-step"></a>Sonraki adım
[Yönetilen bir etki alanı üzerinde güvenli LDAP'yi sorunlarını giderme](active-directory-ds-ldaps-troubleshoot.md)
