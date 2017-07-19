---
title: Azure AD Connect Health'i AD DS ile Kullanma| Microsoft Belgeleri
description: "Bu Azure AD Connect Health sayfasında AD DS’nin nasıl izleneceği ele alınmaktadır."
services: active-directory
documentationcenter: 
author: arluca
manager: femila
editor: curtand
ms.assetid: 19e3cf15-f150-46a3-a10c-2990702cd700
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/18/2017
ms.author: billmath
ms.translationtype: Human Translation
ms.sourcegitcommit: 9d7640577eedcc9221f6346b650aed85f1d31a65
ms.openlocfilehash: 26ebdbc6f568dd3d9bbcaa3250aae70d80af2d35
ms.contentlocale: tr-tr
ms.lasthandoff: 01/18/2017

---
# <a name="using-azure-ad-connect-health-with-ad-ds"></a>Azure AD Connect Health'i AD DS ile Kullanma
Aşağıdaki belgeler Azure AD Connect Health ile Active Directory Etki Alanı Hizmetleri’nin izlenmesine özgüdür. AD DS'nin desteklenen sürümleri şunlardır: Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2 ve Windows Server 2016.

Azure Connect Health ile AD FS'yi izleme hakkında daha fazla bilgi almak için bkz. [Azure AD Connect Health'i AD FS ile kullanma](active-directory-aadconnect-health-adfs.md). Ek olarak, Azure AD Connect’i (Eşitleme) Azure AD Connect Health ile izleme hakkında bilgi için bkz. [Eşitleme için Azure AD Connect Health Kullanma](active-directory-aadconnect-health-sync.md).

![AD DS için Azure AD Connect Health](./media/active-directory-aadconnect-health/aadconnect-health-adds-entry.png)

## <a name="alerts-for-azure-ad-connect-health-for-ad-ds"></a>AD DS için Azure AD Connect Health uyarıları
AD DS için Azure AD Connect Health içindeki Uyarılar bölümünde, etki alanı denetleyicileriniz ile ilgili olarak etkin ve çözümlenmiş uyarıların bir listesi verilir. Etkin veya çözümlenmiş bir uyarının seçilmesi, çözüm adımlarıyla birlikte ek bilgileri ve destekleyici belgelerin bağlantılarını içeren yeni bir dikey pencere açar. Her uyarı türünün, ilgili uyarıdan etkilenen etki alanı denetleyicilerinin her birine karşılık gelen bir veya daha fazla örneği olabilir. Uyarı dikey penceresinin alt kısmında etkilenen bir etki alanı denetleyicisine çift tıklayarak, uyarı örneğine ilişkin daha fazla ayrıntı içeren bir ek dikey pencere açabilirsiniz.

Bu dikey pencerede uyarılar için e-posta bildirimlerini etkinleştirebilir ve görüntülenen zaman aralığını değiştirebilirsiniz. Zaman aralığının genişletilmesi daha önce çözümlenen uyarıları görmenize imkan tanır.

![Azure AD Connect eşitleme hatası](./media/active-directory-aadconnect-health/aadconnect-health-adds-alerts.png)

## <a name="domain-controllers-dashboard"></a>Etki Alanı Denetleyicileri Panosu
Bu pano teme çalışma ölçümleri ve izlenen etki alanlarınızın her birinin sistem durumu ile birlikte ortamınızın topolojik bir görünümünü sağlar. Sunulan ölçümler daha fazla araştırma gerektirebilecek herhangi bir etki alanı denetleyicisini hızlıca tanımlamanıza yardımcı olur. Varsayılan olarak, yalnızca bir sütun alt kümesi görüntülenir. Ancak, sütun komutuna çift tıklayarak kullanılabilir sütunların tamamını bulabilirsiniz. En fazla önemsediğiniz sütunların seçilmesi bu panoyu AD DS ortamınızın durumunu görüntülemek için tek ve kolay bir yer haline getirir.

![Etki Alanı Denetleyicileri](./media/active-directory-aadconnect-health/aadconnect-health-adds-domainsandsites-dashboard.png)

Etki alanı denetleyicileri ilgili etki alanlarına ya da sitelerine göre gruplandırılabilir; bunun yapılması ortam topolojisinin anlaşılması için yararlıdır. Son olarak, dikey pencere üst bilgisine çift tıklarsanız pano mevcut ekrandan yararlanmak üzere genişletilir. Daha geniş olan bu görünüm birden fazla sütun gösterilirken yararlıdır.

## <a name="replication-status-dashboard"></a>Çoğaltma Durumu Panosu
Bu pano, izlenen etki alanı denetleyicilerinizin çoğaltma durumuna ve çoğaltma topolojisine ilişkin görünümü sağlar. En son çoğaltma girişiminin durumu, bulunan herhangi bir hataya ilişkin yararlı belgelerle birlikte listelenir. Hata içeren bir etki alanı denetleyicisine çift tıklayarak şu bilgileri içeren yeni bir dikey pencere açabilirsiniz: hata ayrıntıları, önerilen çözüm adımları ve sorun giderme belgelerinin bağlantıları.

![Çoğaltma Durumu](./media/active-directory-aadconnect-health/aadconnect-health-adds-replication.png)

## <a name="monitoring"></a>İzleme
Bu özellik, izlenen her bir etki alanından sürekli olarak toplanan farklı performans sayaçlarının grafik eğilimlerini verir. Bir etki alanı denetleyicisinin performansı, ormanınızdaki diğer tüm izlenen etki alanı denetleyicileri arasında kolayca karşılaştırılabilir. Ayrıca, ortamınızdaki sorunları gidermede yardımcı olmak üzere çeşitli performans sayaçlarını yan yana görebilirsiniz.

![İzleme](./media/active-directory-aadconnect-health/aadconnect-health-adds-monitoring.png)

Varsayılan olarak, dört performans sayacı önceden seçilmiştir; ancak, filtre komutuna tıklayarak ve istediğiniz performans sayaçlarını seçerek ya da seçimini kaldırarak başkalarını ekleyebilirsiniz. Ayrıca, bir performans sayacı grafiğine çift tıklayarak, izlenen etki alanı denetleyicilerinin her birine ait veri noktalarını içeren yeni bir dikey pencere açabilirsiniz.

## <a name="related-links"></a>İlgili bağlantılar
* [Azure AD Connect Health](active-directory-aadconnect-health.md)
* [Azure AD Connect Health Aracısı Yüklemesi](active-directory-aadconnect-health-agent-install.md)
* [Azure AD Connect Health İşlemleri](active-directory-aadconnect-health-operations.md)
* [Azure AD Connect Health'i AD FS ile Kullanma](active-directory-aadconnect-health-adfs.md)
* [Eşitleme için Azure AD Connect Health'i kullanma](active-directory-aadconnect-health-sync.md)
* [Azure AD Connect Health ile ilgili SSS](active-directory-aadconnect-health-faq.md)
* [Azure AD Connect Health Sürüm Geçmişi](active-directory-aadconnect-health-version-history.md)


