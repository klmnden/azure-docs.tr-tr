---
title: Güvenli LDAP (LDAPS) kullanarak bir Azure AD Domain Services yönetilen etki bağlama | Microsoft Docs
description: Güvenli LDAP (LDAPS) kullanarak bir Azure AD Domain Services yönetilen etki bağlama
services: active-directory-ds
documentationcenter: ''
author: iainfoulds
manager: daveba
editor: curtand
ms.assetid: 6871374a-0300-4275-9a45-a39a52c65ae4
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/28/2019
ms.author: iainfou
ms.openlocfilehash: df0b3d27eec478280a33be831a2431eccdf05a74
ms.sourcegitcommit: 9b80d1e560b02f74d2237489fa1c6eb7eca5ee10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/01/2019
ms.locfileid: "67483379"
---
# <a name="bind-to-an-azure-ad-domain-services-managed-domain-using-secure-ldap-ldaps"></a>Güvenli LDAP (LDAPS) kullanarak bir Azure AD Domain Services yönetilen etki bağlama

## <a name="before-you-begin"></a>Başlamadan önce
Tam [görev 4 - yönetilen etki alanı internet'ten erişmek için DNS'yi yapılandırma](active-directory-ds-ldaps-configure-dns.md).


## <a name="task-5-bind-to-the-managed-domain-over-ldap-using-ldpexe"></a>5\. Görev: Yönetilen etki bağlama üzerinden LDAP Ldp.exe'yi kullanma
Bağlama ve üzerinden LDAP arama yapmak için Uzak Sunucu Yönetim Araçları Paketi dahil edilen LDP.exe aracını kullanabilirsiniz.

İlk olarak, LDP'yi açmak ve yönetilen etki alanına bağlanın. Tıklayın **bağlantı** tıklatıp **Bağlan...**  menüsünde. Yönetilen etki alanı DNS etki alanı adını belirtin. Bağlantılar için kullanılacak bağlantı noktasını belirtin. LDAP bağlantıları için bağlantı noktası 389 kullanın. LDAPS bağlantıları için bağlantı noktası 636'ı kullanın. Tıklayın **Tamam** yönetilen etki bağlama düğmesi.

Ardından, yönetilen etki alanına bağlayın. Tıklayın **bağlantı** tıklatıp **Bağla...**  menüsünde. 'AAD DC Administrators' grubuna ait olan bir kullanıcı hesabının kimlik bilgilerini sağlayın.

> [!IMPORTANT]
> Azure AD Domain Services örneğinizin NTLM parola karması eşitleme devre dışı, kullanıcıların (ve hizmet hesapları) LDAP basit bağlamaları gerçekleştirilemiyor.  NTLM parola karması eşitleme devre dışı bırakma hakkında daha fazla bilgi için okuma [Azure AD DOmain Services yönetilen etki alanınıza güvenli](secure-your-domain.md).
>
>

Seçin **görünümü**ve ardından **ağaç** menüsünde. Temel DN alanı boş bırakın ve Tamam'ı tıklatın. Arama, kapsayıcıya sağ tıklayın ve arama seçmek istediğiniz kapsayıcıya gidin.

> [!TIP]
> - Kullanıcıları ve grupları Azure AD'den eşitlenen depolanır **AADDC Users** kuruluş birimi. Bu kuruluş birimi için arama yolu benzer ```OU=AADDC Users,DC=CONTOSO100,DC=COM```.
> - Yönetilen etki alanına katılmış bilgisayarlar için bilgisayar hesaplarının depolanır **AADDC Computers** kuruluş birimi. Bu kuruluş birimi için arama yolu benzer ```OU=AADDC Computers,DC=CONTOSO100,DC=COM```.
>
>

Daha fazla bilgi - [LDAP sorgu temelleri](https://docs.microsoft.com/windows/desktop/ad/creating-a-query-filter)


## <a name="task-6-lock-down-secure-ldap-access-to-your-managed-domain-over-the-internet"></a>6\. Görev: İnternet üzerinden yönetilen etki alanınıza güvenli LDAP erişimi kilitleme
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
* [Azure AD etki alanı Hizmetleri - başlangıç kılavuzu](create-instance.md)
* [Bir Azure AD Domain Services etki alanını yönetin](manage-domain.md)
* [LDAP sorgu temelleri](https://docs.microsoft.com/windows/desktop/ad/creating-a-query-filter)
* [Azure AD etki alanı Hizmetleri için Grup İlkesi yönetme](manage-group-policy.md)
* [Ağ güvenlik grupları](../virtual-network/security-overview.md)
* [Ağ güvenlik grubu oluşturma](../virtual-network/tutorial-filter-network-traffic.md)


## <a name="next-step"></a>Sonraki adım
[Yönetilen bir etki alanı üzerinde güvenli LDAP'yi sorunlarını giderme](tshoot-ldaps.md)
