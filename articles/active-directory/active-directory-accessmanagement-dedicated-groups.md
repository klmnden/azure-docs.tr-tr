---
title: "Azure Active Directory'deki grupları ayrılmış | Microsoft Docs"
description: "Azure Active Directory ve nasıl oluşturulduğunu nasıl ayrılmış gruplar işlerinde genel bakış."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 86158909-083a-41fe-8090-955e96ad1865
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/12/2017
ms.author: curtand
ms.reviewer: kairaz.contractor
ms.custom: it-pro;oldportal
ms.openlocfilehash: 992f4563064d7a292cf4fdd90a9a3c84cdec91c0
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="dedicated-groups-in-azure-active-directory"></a>Azure Active Directory'de ayrılmış gruplar
Azure Active Directory'de (Azure AD), ayrılmış gruplar özelliği otomatik olarak oluşturur ve önceden tanımlanmış Azure AD grupları üyeliğini doldurur. Ayrılmış gruplar üyeleri eklenemez veya Klasik Azure portalı, Windows PowerShell cmdlet'lerini kullanarak kaldırılan veya program aracılığıyla.

> [!NOTE]
> Bir Azure AD Premium lisansı atanmış ayrılmış gruplar gerektirir
>
> * Grupta kuralı yöneten yönetici
> * Kural tarafından grubunun bir üyesi olması için seçilen tüm kullanıcılar
>
>

**Ayrılmış gruplar etkinleştirmek için**

1. İçinde [Azure portal](https://portal.azure.com)seçin **Active Directory**ve ardından, kuruluşunuzun dizininde açın.
2. Seçin **grupları** sekmesini ve ardından düzenlemek istediğiniz grubu açın.
3. Seçin **yapılandırma** sekmesini tıklatın ve ardından **ayrılmış gruplar etkinleştirmek** için **Evet**.

Ayrılmış gruplar etkinleştirme anahtarı ayarlandığında **Evet**, otomatik olarak ayarlayarak tüm kullanıcılar ayrılmış grubu oluşturmak dizin daha fazla etkinleştirebilirsiniz **etkinleştir "Tüm kullanıcılar" grubu** geçiş **Evet**. Sonra da bu adanmış grubunun adını yazarak düzenleyebilirsiniz **"Tüm kullanıcılar" için görünen ad grup** alan.

Tüm kullanıcılar grubu, dizininizdeki tüm kullanıcılara aynı izinleri atamak için kullanılabilir. Örneğin, tüm kullanıcıların SaaS uygulamasına directory Access bu uygulamaya erişim için tüm kullanıcıların ayrılmış Grup atayarak verebilirsiniz.

Ayrılmış tüm kullanıcılar Grup konuklar ve dış kullanıcılar dahil olmak üzere dizinde tüm kullanıcıları içerir. Bir grup ihtiyacınız varsa, dış kullanıcılar hariç, ardından bir öznitelik tabanlı dinamik kuralı ile aşağıdaki gibi bir grup oluşturarak bunu gerçekleştirebilirsiniz:

                (user.userPrincipalName -notContains "#EXT#@")

Tüm konuklar dışlar bir grup için bir kural aşağıdaki gibi kullanın:

                (user.userType -ne "Guest")

Dinamik grup üyeliğine ilişkin *gelişmiş* kuralların (birden çok karşılaştırma içeren kurallar) nasıl oluşturulacağı hakkında bilgi edinmek için bkz. [Gelişmiş kurallar oluşturmak için öznitelikleri kullanma](active-directory-accessmanagement-groups-with-advanced-rules.md).

### <a name="next-steps"></a>Sonraki adımlar
Bu makalelerde Azure Active Directory ile ilgili ek bilgi sağlanmıştır.

* [Azure Active Directory grupları ile kaynaklara erişimi yönetme](active-directory-manage-groups.md)
* [Azure Active Directory'de Uygulama Yönetimi için Makale Dizini](active-directory-apps-index.md)
* [Azure Active Directory nedir?](active-directory-whatis.md)
* [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](active-directory-aadconnect.md)
