---
title: Azure AD eşitleme hizmeti Yöneticisi arabiriminde bağlayıcılar | Microsoft Docs
description: Bağlayıcılar sekmesi Eşitleme Hizmeti Yöneticisi'nde, Azure AD Connect için anlayın.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: 60f1d979-8e6d-4460-aaab-747fffedfc1e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/13/2017
ms.subservice: hybrid
ms.author: billmath
ms.custom: H1Hack27Feb2017
ms.collection: M365-identity-device-management
ms.openlocfilehash: ae932191c7b76590ea217386dfd729add5566f87
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60384196"
---
# <a name="using-connectors-with-the-azure-ad-connect-sync-service-manager"></a>Azure AD Connect eşitleme hizmeti yöneticisiyle bağlayıcıları kullanma

![Eşitleme Hizmeti Yöneticisi](./media/how-to-connect-sync-service-manager-ui-connectors/connectors.png)

Bağlayıcılar sekmesi eşitleme altyapısının bağlandığı tüm sistemleri yönetmek için kullanılır.

## <a name="connector-actions"></a>Bağlayıcı eylemi
| Eylem | Yorum |
| --- | --- |
| Create |Kullanmayın. İçin ek AD ormanına bağlanmak için Yükleme Sihirbazı'nı kullanın. |
| Özellikler |Etki alanı ve OU filtreleme için kullanılır. |
| [Silme](#delete) |Ya da bağlayıcı alanında verileri silmek için veya bir orman için bağlantıyı silmek için kullanılır. |
| [Çalıştırma profillerini Yapılandır](#configure-run-profiles) |Filtreleme, etki alanı dışında bir şey burada yapılandırın. Bu eylem, önceden yapılandırılmış çalıştırma profillerini görmek için kullanabilirsiniz. |
| Çalıştırın |Tek seferlik bir çalıştırma profili başlatmak için kullanılır. |
| Durdur |Çalışmakta olan bir profili bir bağlayıcı durdurur. |
| Bağlayıcı dışarı aktarma |Kullanmayın. |
| Bağlayıcı alma |Kullanmayın. |
| Bağlayıcıyı güncelleştir |Kullanmayın. |
| Şemasını Yenile |Önbelleğe alınan şemasını yeniler. Bu yana, aynı zamanda güncelleştirmeleri kuralları eşitleme, Yükleme Sihirbazı'nda, bunun yerine, bu seçeneği kullanmak için tercih edilir. |
| [Bağlayıcı alanı arama](#search-connector-space) |Nesneleri bulmak ve bir nesne ve verileri sistem aracılığıyla izlemek için kullanılır. |

### <a name="delete"></a>Sil
Silme eylemi, iki farklı işlemler için kullanılır.  
![Eşitleme Hizmeti Yöneticisi](./media/how-to-connect-sync-service-manager-ui-connectors/connectordelete.png)

Seçenek **silme yalnızca bağlayıcı alanına** tüm verileri kaldırır, ancak yapılandırmayı tutun.

Seçenek **Bağlayıcısı silin ve bağlayıcı alanı** verileri ve yapılandırma kaldırır. Artık bir ormana bağlanmasını istemiyorsanız bu seçeneği kullanılır.

Her iki seçenek, tüm nesneleri eşitlemek ve meta veri deposu nesne güncelleştirin. Bu işlem uzun süren bir işlemdir.

### <a name="configure-run-profiles"></a>Çalıştırma profillerini Yapılandır
Bu seçenek, yapılandırılmış bir bağlayıcı için çalıştırma profillerini görmenize olanak tanır.

![Eşitleme Hizmeti Yöneticisi](./media/how-to-connect-sync-service-manager-ui-connectors/configurerunprofiles.png)

### <a name="search-connector-space"></a>Bağlayıcı alanı arama
Arama bağlayıcı alanı eylem nesneleri bulmak ve veri sorunları gidermek kullanışlıdır.

![Eşitleme Hizmeti Yöneticisi](./media/how-to-connect-sync-service-manager-ui-connectors/cssearch.png)

Başlangıç olarak bir **kapsam**. (RDN, DN, bağlantı, alt ağacı), verilere dayalı olarak arama yapabilirsiniz veya durum nesnesi (tüm diğer seçenekleri için).  
![Eşitleme Hizmeti Yöneticisi](./media/how-to-connect-sync-service-manager-ui-connectors/cssearchscope.png)  
Örneğin bir alt ağacı arama yaparsanız, tüm nesneler bir OU'da alın.  
![Eşitleme Hizmeti Yöneticisi](./media/how-to-connect-sync-service-manager-ui-connectors/cssearchsubtree.png)  
Bu kılavuzdan nesneyi seçin, seçin **özellikleri**, ve [celbi](tshoot-connect-object-not-syncing.md) kaynak Bağlayıcısı alanından, hedef bağlayıcı alanı ve meta veri deposu aracılığıyla.

### <a name="changing-the-ad-ds-account-password"></a>AD DS hesap parolasını değiştirme
Hesap parolasını değiştirirseniz, eşitleme hizmeti şirket içi değişiklikleri içeri/dışarı aktarma mümkün olmayacak AD.   Şu durumlarla karşılaşabilirsiniz:

- AD Bağlayıcısı için içeri/dışarı aktarma adım "no-start-credentials" hatasıyla başarısız oluyor.
- "Kimlik bilgileri geçersiz olduğundan çalıştırmak"contoso.com"yönetim aracı başarısız oldu." altındaki Windows Olay Görüntüleyicisi, uygulama olay günlüğüne olay kimliği 6000 ve iletiyi bir hata içeriyor

Sorunu çözmek için aşağıdakini kullanarak AD DS kullanıcı hesabı güncelleştirin:


1. Eşitleme Hizmeti Yöneticisi'ni (Başlangıç → eşitleme hizmeti) başlatın.
</br>![Eşitleme Hizmeti Yöneticisi](./media/how-to-connect-sync-service-manager-ui-connectors/startmenu.png)
2. Git **Bağlayıcılar** sekmesi.
3. AD DS hesabını kullanmak üzere yapılandırılmış AD bağlayıcısını seçin.
4. Eylemler altında seçin **özellikleri**.
5. Açılan iletişim kutusunda, Active Directory ormanına Bağlan'ı seçin:
6. Orman adı, şirket içi AD karşılık gelen gösterir.
7. Eşitleme için kullanılan AD DS hesabı kullanıcı adını belirtir.
8. Parola metin kutusuna yeni AD DS hesap parolasını girin ![Azure AD Connect eşitleme şifreleme anahtarı yardımcı programı](./media/how-to-connect-sync-service-manager-ui-connectors/key6.png)
9. Eski parola bellek önbelleğinden kaldırmak için eşitleme hizmetini yeniden başlatın ve yeni parolayı kaydetmek için Tamam'a tıklayın.



## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi edinin [Azure AD Connect eşitleme](how-to-connect-sync-whatis.md) yapılandırma.

[Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md) hakkında daha fazla bilgi edinin.
