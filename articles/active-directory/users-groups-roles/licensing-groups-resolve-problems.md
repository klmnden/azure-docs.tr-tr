---
title: Bir grup - Azure Active Directory için lisans atama sorunlarını gidermek | Microsoft Docs
description: Belirleme ve Azure Active Directory grup tabanlı lisanslama kullanıyorsanız, lisans ataması sorunlarını gidermek
services: active-directory
keywords: Azure AD lisanslama
documentationcenter: ''
author: curtand
manager: mtillman
ms.service: active-directory
ms.subservice: users-groups-roles
ms.topic: article
ms.workload: identity
ms.date: 03/18/2019
ms.author: curtand
ms.reviewer: sumitp
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: c92969015910cc5bd72e2d9339d5c15c1f7af48b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60470286"
---
# <a name="identify-and-resolve-license-assignment-problems-for-a-group-in-azure-active-directory"></a>Azure Active Directory'de bir grup için lisans atama sorunlarını tanımlama ve çözme

Grup tabanlı lisanslama Azure Active Directory (Azure AD) bir lisans hata durumundaki kullanıcıların kavramını sunar. Bu makalede, biz kullanıcıların bu duruma neden çıkabilir nedenleri açıklanmaktadır.

Grup tabanlı lisanslama kullanmadan doğrudan bireysel kullanıcılar için lisans atadığınızda, atama işlemi başarısız olabilir. Örneğin, PowerShell cmdlet'ini çalıştırdığınızda `Set-MsolUserLicense` kullanıcı sistemde cmdlet'i iş mantığı için ilgili birçok nedenden dolayı başarısız olabilir. Örneğin, aynı anda atanamaz iki hizmet planları arasında çakışma veya lisans sayısı yetersiz olabilir. Sorun hemen geri size bildirilir.

Lisanslama, Grup tabanlı kullanırken aynı hatalar oluşabilir ancak Azure AD hizmeti, lisans atama sırasında bunlar arka planda gerçekleşir. Bu nedenle, hataları, hemen bildirilmesi olamaz. Bunun yerine, kullanıcı nesnesindeki kaydedilen ve sonra Yönetici portalı üzerinden bildirilen. Kullanıcı lisansı için özgün amacı hiçbir zaman kaybolduğu ancak gelecekte araştırmak ve çözüm için bir hata durumu kaydedilir.

## <a name="how-to-find-license-assignment-errors"></a>Lisans atama hataları bulma
**Lisans atama hataları bulmak için**

1. Hata durumunda belirli bir gruptaki kullanıcıları bulmak için Grup için bölmesini açın. Altında **lisansları**, hata durumunda herhangi bir kullanıcı varsa bir bildirim görüntülenir.

   ![Grup ve hata bildirimi iletisi](./media/licensing-groups-resolve-problems/group-error-notification.png)

2. Etkilenen tüm kullanıcıların listesini açmak için bildirimi seçin. Daha fazla ayrıntı için ayrı ayrı her bir kullanıcı seçebilirsiniz.

   ![hata durumu lisans grubundaki kullanıcıların listesi](./media/licensing-groups-resolve-problems/list-of-users-with-errors.png)

3. Üzerinde en az bir hata içeren tüm grupları bulmak için **Azure Active Directory** dikey penceresini seçin **lisansları**ve ardından **genel bakış**. Grupları dikkat etmeniz gereken bir bilgi kutusu görüntülenir.

   ![Genel bakış ve hata durumunda grupları hakkında bilgi](./media/licensing-groups-resolve-problems/group-errors-widget.png)

4. Hata içeren tüm grupların bir listesini görmek için kutusunu seçin. Daha fazla ayrıntı için her bir grup seçebilirsiniz.

   ![Genel bakış ve hatalarla gruplarının listesi](./media/licensing-groups-resolve-problems/list-of-groups-with-errors.png)


Aşağıdaki bölümlerde, her olası bir sorunu çözmek için yol ve bir açıklama girin.

## <a name="not-enough-licenses"></a>Yeterli lisans yok

**Sorun:** Bir grup içinde belirtilen ürün için yeterli sayıda kullanılabilir lisans yok. Ürün için daha fazla lisans satın almanıza veya diğer kullanıcıları veya grupları kullanılmayan lisanslarını boşaltmak gerekir.

Kaç tane lisansın kullanılabilir olduğunu görmek için Git **Azure Active Directory** > **lisansları** > **tüm ürünleri**.

Hangi kullanıcıların ve grupların lisanslarını kullanan görmek için bir ürün seçin. Altında **lisanslı kullanıcılar**, doğrudan veya bir veya daha fazla grubu aracılığıyla atanan lisansları materyaline erişebilen tüm kullanıcıların bir listesini görürsünüz. Altında **lisanslı gruplar**, söz konusu ürünlerin atanan tüm grupları görürsünüz.

**PowerShell:** PowerShell cmdlet'leri bu hata olarak rapor _CountViolation_.

## <a name="conflicting-service-plans"></a>Çakışan hizmet planları var

**Sorun:** Grubunda belirtilen ürünlerinden biri için farklı bir ürünü aracılığıyla kullanıcı zaten atanmış başka bir hizmet planı ile çakışan bir hizmet planı içerir. Bazı hizmet planları, bunlar ilgili başka bir hizmet planı ile aynı kullanıcıya atanamaz bir şekilde yapılandırılır.

Aşağıdaki örneği inceleyin. Bir kullanıcının Office 365 kuruluş için bir lisans *E1* doğrudan etkin tüm planlarla atanmış. Kullanıcının Office 365 Kurumsal olan bir gruba eklendi *E3* kendisine atanmış bir ürün. E3 ürün grubu lisans ataması "Çakışan hizmet planları" hatasıyla başarısız oluyor, böylece E1 dahil edilen planları ile çakışamaz hizmet planları içerir. Bu örnekte, çakışan hizmet planları şunlardır:

-   SharePoint Online (Plan 1) ile SharePoint Online (Plan 2) çakışıyor.
-   Exchange Online (Plan 1) ile Exchange Online (Plan 2) çakışıyor.

Bu çakışmayı çözmek için iki planları devre dışı bırakmak gerekir. Doğrudan kullanıcıya atanmış E1 lisans devre dışı bırakabilirsiniz. Veya, tüm Grup lisans atamasını değiştirmek ve E3 lisans planları devre dışı bırakmak gerekir. Alternatif olarak, E3 lisansı bağlamında yedek varsa, kullanıcıdan E1 lisansını kaldırmak isteyebilirsiniz.

Çakışan ürün lisansları her zaman çözme hakkında karar yöneticiye aittir. Azure AD'ye otomatik olarak lisans çakışmaları değil.

**PowerShell:** PowerShell cmdlet'leri bu hata olarak rapor _MutuallyExclusiveViolation_.

## <a name="other-products-depend-on-this-license"></a>Bu lisansa bağımlı olan başka ürünler var

**Sorun:** Bir grup içinde belirtilen ürün işlevi için başka bir ürün içinde başka bir hizmet planı için etkinleştirilmesi gereken bir hizmet planına içeriyor. Azure AD temel hizmet planı kaldırmaya çalıştığında bu hata oluşur. Örneğin, kullanıcı gruptan kaldırdığınızda bu durum oluşabilir.

Bu sorunu çözmek için başka bir yöntem kullanıcılara, gerekli planı yine de atanır veya bağımlı hizmetleri söz konusu kullanıcılar için devre dışı bırakıldığında emin olmanız gerekir. Böylece bu kullanıcıların Grup lisansı düzgün bir şekilde kaldırabilirsiniz.

**PowerShell:** PowerShell cmdlet'leri bu hata olarak rapor _DependencyViolation_.

## <a name="usage-location-isnt-allowed"></a>Kullanım konumu izin verilmiyor

**Sorun:** Bazı Microsoft Hizmetleri yerel kanunlarınız ve düzenlemelerinizle nedeniyle tüm konumlarda mevcut değildir. Bir kullanıcıya bir lisans atamak için önce belirtmelisiniz **kullanım konumu** kullanıcı özelliği. Konum altında belirtebilirsiniz **kullanıcı** > **profili** > **ayarları** bölümünde Azure portalında.

Azure AD grubu lisans, kullanım konumu desteklenmiyor kullanıcıya atamak denediğinde başarısız olur ve kullanıcının bir hata kaydeder.

Bu sorunu çözmek için kullanıcıların lisanslı Grup ekranla konumlardan kaldırın. Geçerli kullanım konumu değerleri gerçek kullanıcı konumunu temsil eden yoksa, (yeni konuma destekleniyorsa) lisansların sonraki doğru şekilde atandığını gösteren için alternatif olarak, bunları değiştirebilirsiniz.

**PowerShell:** PowerShell cmdlet'leri bu hata olarak rapor _ProhibitedInUsageLocationViolation_.

> [!NOTE]
> Azure AD Grup lisansları atarken, belirtilen kullanım konumu olmadan herhangi bir kullanıcı dizin konumunu alır. Yöneticiler doğru kullanım konumu değerlerinin kullanıcıları grup tabanlı lisanslama yerel yasalara ve düzenlemelere uygun kullanmadan önce ayarlamanızı öneririz.

## <a name="duplicate-proxy-addresses"></a>Yinelenen proxy adresleri

Exchange Online kullanıyorsanız, kiracınızdaki bazı kullanıcılar yanlışlıkla aynı proxy adres değeriyle yapılandırılmış olabilir. Grup tabanlı lisanslama böyle bir kullanıcıya bir lisans atamak denediğinde başarısız olur ve "Proxy adresi zaten kullanılıyor" gösterir.

> [!TIP]
> Yinelenen proxy adresi olup olmadığını görmek için Exchange Online karşı aşağıdaki PowerShell cmdlet'ini yürütün:
> ```
> Run Get-Recipient | where {$_.EmailAddresses -match "user@contoso.onmicrosoft.com"} | fL Name, RecipientType,emailaddresses
> ```
> Bu sorun hakkında daha fazla bilgi için bkz. [Exchange Online'da "Proxy adresi zaten kullanılıyor" hata iletisini](https://support.microsoft.com/help/3042584/-proxy-address-address-is-already-being-used-error-message-in-exchange-online). Makale hakkında bilgileri de içerir. [Exchange Online'a uzak PowerShell kullanarak bağlanma konusunda](https://technet.microsoft.com/library/jj984289.aspx). Daha fazla bilgi için bu makaleye bakın [proxyAddresses özniteliği Azure AD'de nasıl doldurulur üzerinde](https://support.microsoft.com/help/3190357/how-the-proxyaddresses-attribute-is-populated-in-azure-ad).

Etkilenen kullanıcılar için herhangi bir proxy adresi sorunu çözdükten sonra lisans artık uygulanabilir emin olmak için grupta lisans işleme zorlamak emin olun.

## <a name="what-happens-when-theres-more-than-one-product-license-on-a-group"></a>Bir grup üzerinde birden fazla ürün lisans olmadığında ne olur?

Bir grup için birden fazla ürün lisansı atayabilirsiniz. Örneğin, bir gruba dahil edilen tüm Hizmetleri kullanıcıları kolayca etkinleştirmek için Office 365 Kurumsal E3 ve Enterprise Mobility + Security atayabilirsiniz.

Azure AD grubunda her kullanıcı için belirtilen tüm lisansları atamak çalışır. Azure AD ürünlerinden biri nedeniyle iş mantığı sorunlarını atayamadığı durumlarda bu gruptaki diğer lisanslar ya da atanmaz. Tümü için yeterli sayıda lisans yoksa veya kullanıcıya etkin diğer hizmetlerle çakışma varsa örneğidir.

Atanır ve bu sorundan etkilenen hangi ürünler için başarısız oldu. kullanıcıları görebilirsiniz.

## <a name="what-happens-when-a-group-with-licenses-assigned-is-deleted"></a>Atanmış lisansları olan bir grup olduğunda ne silindi mi?

Grup silebilmeniz için önce bir gruba atanmış tüm lisansları kaldırmanız gerekir. Ancak, gruptaki tüm kullanıcılardan lisansları kaldırma zaman alabilir. Bir gruptan lisans ataması kaldırılırken olabilir hataları kullanıcının atanmış bağımlı bir lisans olup olmadığını veya lisans kaldırma önleyen bir proxy adresi çakışması sorunu ise. Bir kullanıcı bir lisans grubu silme işlemi nedeniyle kaldırılan bağımlı olduğu lisans varsa, kullanıcıya lisans ataması yönlendirmek için devralınan dönüştürülür.

Örneğin, Office 365 E3/atanmış E5 ile birlikte bir Skype kurumsal iş hizmet planı etkin bir grup göz önünde bulundurun. Ayrıca grubun bazı üyeleri ses konferans lisansları doğrudan atanmış olduğunu hayal edin. Grup silindiğinde, Grup tabanlı lisanslama tüm kullanıcıların Office 365 E3/E5 kaldırmayı deneyecek. Ses konferans Skype Kurumsal'a bağımlı olduğundan, ses konferans sahip tüm kullanıcılar için atanan, Grup tabanlı lisanslama doğrudan lisans ataması için Office 365 E3/E5 lisansı dönüştürür.

## <a name="how-do-you-manage-licenses-for-products-with-prerequisites"></a>Önkoşullar ürünleriyle lisanslarını nasıl yönetir?

Bazı Microsoft Online ürünleri olduğunuz *eklentileri*. Eklentiler, bir kullanıcı veya grup için bir lisans atanabilmesi için önce etkinleştirilmesi için bir önkoşul hizmet planı gerektirir. Grup tabanlı lisanslama ile sistem önkoşul ve eklenti hizmet planlarını aynı grubun var olmasını gerektirir. Bu gruba eklenen tüm kullanıcılar, tam olarak bir çalışma ürünü aldığından emin olmak için gerçekleştirilir. Aşağıdaki örnek düşünelim:

Microsoft Workplace Analytics bir eklenti ürünüdür. Bu, aynı ada sahip bir tek hizmet planı içerir. Aşağıdaki Önkoşullar biri de atandığında, biz bu hizmet planı yalnızca bir kullanıcı veya grup için atayabilirsiniz:
- Exchange Online (Plan 1) 
- Exchange Online (Plan 2)

Biz bu ürün kendi kendine bir gruba atamak çalışırsanız, portal bir hata döndürür. Hata bildirimine seçme, aşağıdaki ayrıntıları gösterir:

![Grup, önkoşul eksik](./media/licensing-groups-resolve-problems/group-prerequisite-required.png)

Ayrıntıları seçtiğinizde, aşağıdaki hata iletisini gösterir:

>Lisans işlemi başarısız oldu. Grup ekleme veya bağımlı hizmetin kaldırmadan önce gerekli hizmetleri olduğundan emin olun. **Microsoft Workplace Analytics hizmeti, Exchange Online (Plan de etkinleştirilmesini 2) gerektirir.**

Bu eklenti lisansı bir gruba atamak için biz grubu da önkoşul hizmet planı içeren emin olmanız gerekir. Örneğin, biz tam Office 365 E3 ürün zaten var. Varolan bir grubu güncelleştirin ve eklenti ürün ekleyin.

İş eklenti yalnızca en düşük gerekli ürünlerin içeren bir tek başına grubunu oluşturmak mümkündür. Yalnızca seçilen kullanıcılar eklenti ürün için lisans için kullanılabilir. Bu örnekte biz aşağıdaki ürünler aynı gruba atadığınız:
- Office 365 Kurumsal E3 ile yalnızca Exchange Online (Plan 2) hizmet planı etkin
- Microsoft Workplace Analytics

![Dahil edilen Grup, önkoşul](./media/licensing-groups-resolve-problems/group-addon-with-prerequisite.png)

Artık bu gruba eklenen herhangi bir kullanıcı bir lisans E3 ürünün ve Workplace Analytics ürünün bir lisans kullanır. Aynı anda kullanıcılarla tam E3 ürün sağlayan başka bir grubun üyesi olabilir ve yine de yalnızca bir lisans bu ürün için kullandıkları.

> [!TIP]
> Her bir önkoşul hizmet planı için birden çok gruplar oluşturabilirsiniz. Örneğin, kullanıcılarınız için hem Office 365 Kurumsal E1 hem de Office 365 Kurumsal E3 kullanırsanız, iki gruba lisans Microsoft Workplace Analytics oluşturabilirsiniz: E1 önkoşul ve E3 kullanan diğer kullanan bir. Bu, eklenti E1 ve E3 kullanıcılara ek lisans kullanmasa bile Dağıt sağlar.

## <a name="how-do-you-force-license-processing-in-a-group-to-resolve-errors"></a>Hataları gidermek için bir gruptaki lisans işleme nasıl zorlama?

Hataları gidermek için zamandaki bağlı olarak hangi adımları el ile kullanıcı durumunu güncelleştirmek için bir grup işlenmesini tetiklemek gerekli olabilir.

Örneğin, kullanıcıların doğrudan lisans atamaları kaldırarak bazı lisanslarını ücretsiz, tam olarak tüm kullanıcı üyeleri lisans daha önce başarısız olan grupları işlenmesini tetiklemek gerekir. Bir grubu yeniden işlemek için Grup bölmesine, açık Git **lisansları**ve ardından **suretiyle** araç çubuğunda.

## <a name="how-do-you-force-license-processing-on-a-user-to-resolve-errors"></a>Hataları gidermek için bir kullanıcı lisansı işleme nasıl zorlama?

Hataları gidermek için zamandaki bağlı olarak hangi adımları kullanıcı durumu güncelleştirmek için bir kullanıcı işlenmesini el ile tetiklemek gerekli olabilir.

Örneğin, etkilenen bir kullanıcı için yinelenen bir proxy adresi sorunu çözdükten sonra kullanıcının işlemi tetiklemek gerekir. Bir kullanıcı yeniden işlemek için kullanıcı bölmesine, açık Git **lisansları**ve ardından **suretiyle** araç çubuğunda.

## <a name="next-steps"></a>Sonraki adımlar

Grupları aracılığıyla lisans yönetimi için diğer senaryolar hakkında daha fazla bilgi için şunlara bakın:

* [Grup tabanlı Azure Active Directory lisansı nedir?](../fundamentals/active-directory-licensing-whatis-azure-portal.md)
* [Azure Active Directory'de gruba lisans atama](licensing-groups-assign.md)
* [Azure Active Directory'de tek tek lisanslı kullanıcıları grup tabanlı lisanslamaya geçirme](licensing-groups-migrate-users.md)
* [Kullanıcılar Azure Active Directory'de Grup tabanlı lisanslama kullanarak ürün lisansları arasında geçirme](licensing-groups-change-licenses.md)
* [Azure Active Directory grup tabanlı lisanslamayla ilgili ek senaryolar](licensing-group-advanced.md)
* [Azure Active Directory'de Grup tabanlı lisanslama için PowerShell örnekleri](licensing-ps-examples.md)
