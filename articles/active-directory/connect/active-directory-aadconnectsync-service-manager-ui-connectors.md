---
title: "Azure AD eşitleme hizmeti Yöneticisi arabiriminde bağlayıcılar | Microsoft Docs"
description: "Eşitleme Hizmeti Yöneticisi'nde bağlayıcılar sekmesi için Azure AD Connect anlayın."
services: active-directory
documentationcenter: 
author: billmath
manager: mtillman
editor: 
ms.assetid: 60f1d979-8e6d-4460-aaab-747fffedfc1e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: billmath
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 3bbbe5d0d7a7ed7065133b4bc6e5fc2dba39bf7d
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/18/2018
---
# <a name="using-connectors-with-the-azure-ad-connect-sync-service-manager"></a>Azure AD Connect Eşitleme Hizmeti Yöneticisi ile bağlayıcıları kullanma

![Eşitleme Hizmeti Yöneticisi](./media/active-directory-aadconnectsync-service-manager-ui/connectors.png)

Bağlayıcılar sekmesi, eşitleme altyapısı bağlı tüm sistemleri yönetmek için kullanılır.

## <a name="connector-actions"></a>Bağlayıcı Eylemler
| Eylem | Yorum |
| --- | --- |
| Oluştur |Kullanmayın. Ek AD ormanına bağlanmak için Yükleme Sihirbazı'nı kullanın. |
| Özellikler |Etki alanı ve OU filtreleme için kullanılır. |
| [Silme](#delete) |Ya da bağlayıcı alanı verileri silmek veya bir orman bağlantıyı silmek için kullanılır. |
| [Çalıştırma profillerini Yapılandır](#configure-run-profiles) |Filtreleme, etki alanı dışındaki Burada yapılandırdığınız bir şey yok. Önceden yapılandırılmış çalıştırma profillerini görmek için bu eylemi kullanın. |
| Çalıştırın |Bir kerelik bir çalıştırma profili başlatmak için kullanılır. |
| Durdur |Çalışmakta olan bir profili bir bağlayıcı durdurur. |
| Bağlayıcı dışarı aktarma |Kullanmayın. |
| İçeri aktarma Bağlayıcısı |Kullanmayın. |
| Güncelleştirme Bağlayıcısı |Kullanmayın. |
| Şemayı Yenile |Önbelleğe alınan şema yeniler. Bu yana, aynı zamanda güncelleştirmeleri kuralları eşitleme, bunun yerine, Yükleme Sihirbazı'nda bu seçeneği kullanmak için tercih edilir. |
| [Bağlayıcı alanı arama](#search-connector-space) |Nesneleri bulmak için kullanılan ve [bir nesne ve verileri sistemi aracılığıyla izleyin](#follow-an-object-and-its-data-through-the-system). |

### <a name="delete"></a>Sil
Silme eylemi iki farklı işlemler için kullanılır.  
![Eşitleme Hizmeti Yöneticisi](./media/active-directory-aadconnectsync-service-manager-ui/connectordelete.png)

Seçenek **silme yalnızca bağlayıcı alanı** tüm verileri kaldırır, ancak yapılandırmayı tutun.

Seçenek **bağlayıcı silin ve bağlayıcı alanı** verileri ve yapılandırma kaldırır. Bir ormana artık bağlanmak istemediğinizde bu seçenek kullanılır.

Her iki seçenek, tüm nesneleri eşitlemek ve meta veri deposu nesneleri güncelleştirin. Bu işlem uzun süren bir işlemdir.

### <a name="configure-run-profiles"></a>Çalıştırma profillerini Yapılandır
Bu seçenek, yapılandırılmış bir bağlayıcı için çalıştırma profillerini görmenize olanak tanır.

![Eşitleme Hizmeti Yöneticisi](./media/active-directory-aadconnectsync-service-manager-ui/configurerunprofiles.png)

### <a name="search-connector-space"></a>Bağlayıcı alanı arama
Arama bağlayıcı alanı eylem nesneleri bulmak ve veri sorunlarını gidermek kullanışlıdır.

![Eşitleme Hizmeti Yöneticisi](./media/active-directory-aadconnectsync-service-manager-ui/cssearch.png)

Başlangıç seçerek bir **kapsam**. Bağlı veri (RDN, DN, bağlayıcı, alt ağacı), arama yapabilirsiniz veya durum nesnesi (tüm diğer seçenekleri için).  
![Eşitleme Hizmeti Yöneticisi](./media/active-directory-aadconnectsync-service-manager-ui/cssearchscope.png)  
Örneğin bir alt ağacı arama yaparsanız, bir OU'daki tüm nesneleri alın.  
![Eşitleme Hizmeti Yöneticisi](./media/active-directory-aadconnectsync-service-manager-ui/cssearchsubtree.png)  
Bu kılavuzdan nesneyi seçin, seçin **özellikleri**, ve [izleyerek](active-directory-aadconnectsync-troubleshoot-object-not-syncing.md) kaynak bağlayıcı alandan üzerinden meta veri deposu ve hedef bağlayıcı alanı.

### <a name="changing-the-ad-ds-account-password"></a>AD DS hesap parolasını değiştirme
Hesap parolası değiştirirseniz, eşitleme hizmeti artık şirket içi değişiklikler içeri/dışarı aktarma kuramaz AD.   Aşağıdakileri görebilirsiniz:

- İçeri/dışarı aktarma adım AD Bağlayıcısı için "-start-kimlik bilgileri yok" hatası ile başarısız olur.
- Windows Olay Görüntüleyicisi'ni altında uygulama olay günlüğüne olay kimliği 6000 ve ileti hatayla "Yönetim Aracısı"contoso.com"kimlik bilgileri geçersiz olduğundan çalıştırılamadı." içerir

Sorunu çözmek için aşağıdakileri kullanarak AD DS kullanıcı hesabı güncelleştirin:


1. Eşitleme Hizmeti Yöneticisi'ni (Başlangıç → eşitleme hizmeti) başlatın.
</br>![Eşitleme Hizmeti Yöneticisi](./media/active-directory-aadconnectsync-service-manager-ui/startmenu.png)
2. Git **Bağlayıcılar** sekmesi.
3. AD DS hesabı kullanacak şekilde yapılandırılmış AD Bağlayıcısı'nı seçin.
4. Eylemler altında seçin **özellikleri**.
5. Açılan iletişim kutusunda, Active Directory ormanına Bağlan seçin:
6. Orman adı karşılık gelen içi gösterir AD.
7. Eşitleme için kullanılan AD DS hesap kullanıcı adını belirtir.
8. Parola metin kutusuna yeni AD DS hesabının parolasını girin ![Azure AD Connect eşitleme şifreleme anahtarı yardımcı programı](media/active-directory-aadconnectsync-encryption-key/key6.png)
9. Yeni parola kaydetmek ve eski parola bellek önbelleğinden kaldırılacak eşitleme hizmetini yeniden başlatmak için Tamam'ı tıklatın.



## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi edinmek [Azure AD Connect eşitleme](active-directory-aadconnectsync-whatis.md) yapılandırma.

[Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](active-directory-aadconnect.md) hakkında daha fazla bilgi edinin.
