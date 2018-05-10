---
title: Azure Active Directory'deki bir gruba lisans sorunlarını gidermek | Microsoft Docs
description: Tanımlamak ve Azure Active Directory grup tabanlı lisans kullanırken lisans atama sorunları gidermek nasıl
services: active-directory
keywords: Azure AD lisanslama
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: users-groups-roles
ms.topic: article
ms.workload: identity
ms.date: 06/05/2017
ms.author: curtand
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: e91b0a79e7b45ce7e0de1b7cf4aa3123550692af
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="identify-and-resolve-license-assignment-problems-for-a-group-in-azure-active-directory"></a>Belirleyin ve Azure Active Directory'deki bir gruba için lisans atama sorunları çözün

Grup tabanlı Azure Active Directory (Azure AD) lisans kullanıcıların Lisans hata durumuna kavramını sunmaktadır. Bu makalede, kullanıcıların bu durumda neden bitirebilirsiniz nedenleri açıklanmaktadır.

Grup tabanlı lisans kullanmadan doğrudan bireysel kullanıcılara lisans atadığınızda, atama işlemi başarısız olabilir. Örneğin, PowerShell cmdlet'ini çalıştırdığınızda `Set-MsolUserLicense` bir kullanıcı sistemde cmdlet iş mantığına ilgili birçok nedeniyle başarısız olabilir. Örneğin, lisans ya da aynı anda atanamaz iki hizmet planları arasında bir çakışma sayısı yetersiz olabilir. Sorun hemen geri size bildirilir.

Grup tabanlı kullanırken lisanslama, aynı hatalar oluşabilir, ancak Azure AD hizmeti lisans atama sırasında bunlar arka planda gerçekleşir. Bu nedenle, hataları için hemen bildirilmesi olamaz. Bunun yerine, bunlar kullanıcı nesnesindeki kaydedilen ve Yönetim Portalı üzerinden bildirdi. Kullanıcı lisansı için özgün amacı hiçbir zaman kaybolur, ancak gelecekte araştırmak ve çözümleme için bir hata durumu kaydedilir.

## <a name="how-to-find-license-assignment-errors"></a>Lisans atama hataları bulma
**Lisans atama hatalarını bulmak için**

   1. Belirli bir grubunun hata durumunda kullanıcıları bulmak için grubu için bölmesini açın. Altında **lisansları**, bir hata durumunda tüm kullanıcıların varsa bir bildirim görüntülenir.

   ![Grup, hata bildirimi](media/active-directory-licensing-group-problem-resolution-azure-portal/group-error-notification.png)

   2. Tüm etkilenen kullanıcıların bir listesini açmak için bildirimi seçin. Daha fazla ayrıntı için ayrı ayrı her bir kullanıcı seçebilirsiniz.

   ![Hata durumunda kullanıcı grubu listesi](media/active-directory-licensing-group-problem-resolution-azure-portal/list-of-users-with-errors.png)

   3. Üzerinde en az bir hata içeren tüm grupları bulmak için **Azure Active Directory** dikey seçin **lisansları**ve ardından **genel bakış**. Grupları dikkat etmeniz gereken durumlarda bir bilgi kutusu görüntülenir.

   ![Genel bakış, hata durumunda grupları hakkında bilgi](media/active-directory-licensing-group-problem-resolution-azure-portal/group-errors-widget.png)

   4. Hataları olan tüm gruplarının bir listesini görmek için kutusunu seçin. Daha fazla ayrıntı için her bir grup seçebilirsiniz.

   ![Genel bakış, hatalarla gruplarının listesi](media/active-directory-licensing-group-problem-resolution-azure-portal/list-of-groups-with-errors.png)


Aşağıdaki bölümlerde, her olası bir sorunu çözmek için yolu ve bir açıklama girin.

## <a name="not-enough-licenses"></a>Yeterli lisans yok

**Sorun:** grubunda belirtilen ürünlerin biri için kullanılabilir yeterli lisans yok. Ürünü için daha fazla lisans satın ya da diğer kullanıcılara veya gruplara kullanılmayan lisanslarını boşaltmak gerekir.

Kaç tane kullanılabilir olduğunu görmek için şu adrese gidin **Azure Active Directory** > **lisansları** > **tüm ürünleri**.

Hangi kullanıcıların ve grupların lisansları kullanıyor görmek için bir ürün seçin. Altında **lisanslı kullanıcıların**, doğrudan veya bir veya daha fazla grupları üzerinden atanan lisansları beklendiğinden tüm kullanıcıların listesini görürsünüz. Altında **lisans grupları**, bu ürünler atanmış olan tüm grupları bakın.

**PowerShell:** PowerShell cmdlet'leri bu hata olarak rapor _CountViolation_.

## <a name="conflicting-service-plans"></a>Çakışan hizmet planları var

**Sorun:** zaten farklı bir ürün aracılığıyla kullanıcıya atanmış başka bir hizmet planı olan çakışan bir hizmet planı grubunda belirtilen ürünleri içerir. Bazı hizmet planları, bunlar aynı kullanıcıya ilgili başka bir hizmet planı olarak atanamaz şekilde yapılandırılır.

Aşağıdaki örnek göz önünde bulundurun. Bir kullanıcı için Office 365 Enterprise lisansının *E1* doğrudan sahip tüm planlar etkin atanmış. Kullanıcı Office 365 Kurumsal olan bir gruba eklenen *E3* ürün atanmış. E3 ürün çakışamaz hizmet planları Grup lisans atamasını "Çakışan hizmet planları" hatasıyla başarısız olur. böylece E1 içinde bulunan planına sahip içerir. Bu örnekte, çakışan hizmet planları şunlardır:

-   SharePoint Online (planlama 1) ile SharePoint Online (2 planlama) çakışıyor.
-   Exchange Online (planlama 1) ile Exchange Online (2 planlama) çakışıyor.

Bu çakışmayı çözmek için iki planları devre dışı bırakmanız gerekir. Doğrudan kullanıcıya atanan E1 lisans devre dışı bırakabilirsiniz. Veya, tüm Grup lisans atamasını değiştirmek ve E3 lisans planları devre dışı bırakmak gerekir. Alternatif olarak, E1 lisans E3 lisans bağlamında yedek varsa kullanıcıyı kaldırmak isteyebilirsiniz.

Çakışan ürün lisansları her zaman çözümü hakkındaki kararınızı yöneticiye aittir. Azure AD otomatik olarak lisans çakışmaları değil.

**PowerShell:** PowerShell cmdlet'leri bu hata olarak rapor _MutuallyExclusiveViolation_.

## <a name="other-products-depend-on-this-license"></a>Bu lisansa bağımlı olan başka ürünler var

**Sorun:** işlevi için başka bir ürün içinde başka bir hizmet planı için etkinleştirilmesi gereken bir hizmet planına grubunda belirtilen ürünleri içerir. Azure AD temel alınan hizmet planını kaldırmanız çalıştığında bu hata oluşur. Örneğin, kullanıcıyı gruptan kaldırdığınızda bu durum oluşabilir.

Bu sorunu çözmek için başka bir yöntem kullanıcılara, gerekli planı hala atanır veya bağımlı hizmetler bu kullanıcılar için devre dışı bırakılır emin olmanız gerekir. Yaptıktan sonra bu kullanıcılardan Grup lisans düzgün kaldırabilirsiniz.

**PowerShell:** PowerShell cmdlet'leri bu hata olarak rapor _DependencyViolation_.

## <a name="usage-location-isnt-allowed"></a>Kullanım konumu izin verilmiyor

**Sorun:** bazı Microsoft Hizmetleri tüm konumlarda yerel yasalarına ve düzenlemelerine nedeniyle kullanılamaz. Bir kullanıcıya bir lisans atamak için önce belirtmelisiniz **kullanım konumu** kullanıcı için özellik. Konum altında belirtebilirsiniz **kullanıcı** > **profil** > **ayarları** Azure portalı bölümünde.

Azure AD, kullanım konumu desteklenmiyor bir kullanıcı grubu lisans atamak çalıştığında başarısız olur ve kullanıcı bir hata kaydeder.

Bu sorunu çözmek için kullanıcılar ekranla konumları lisanslı grubundan kaldırın. Geçerli kullanım konumu değerlerinin gerçek kullanıcı konumu temsil eden yok, lisansları (yeni konumu destekleniyorsa) doğru sonraki kalmayacak şekilde alternatif olarak, bunları değiştirebilirsiniz.

**PowerShell:** PowerShell cmdlet'leri bu hata olarak rapor _ProhibitedInUsageLocationViolation_.

> [!NOTE]
> Azure AD Grup lisansları atarken, belirtilen kullanım konumu olmayan tüm kullanıcılar dizininin konumunu devralır. Yöneticiler doğru kullanım konumu değerlerinin kullanıcıları grup tabanlı lisans yerel yasa ve yönetmeliklere uymak için kullanmadan önce ayarlamanızı öneririz.

## <a name="what-happens-when-theres-more-than-one-product-license-on-a-group"></a>Bir gruba birden fazla ürün lisans olmadığında ne olur?

Bir gruba birden fazla ürün lisans atayabilirsiniz. Örneğin, tüm dahil Hizmetleri kullanıcıları kolayca etkinleştirmek için bir grubu için Office 365 Kurumsal E3 ve Enterprise Mobility + Security atayabilirsiniz.

Azure AD grubundaki her kullanıcı için belirtilen tüm lisansları atama dener. Azure AD ürünlerden birinin iş mantığı sorunları nedeniyle atayamazsınız, bu da grubundaki diğer lisansları atama olmaz. Tümü için yeterli lisans yoksa ya da kullanıcıya etkin diğer hizmetlerle çakışma varsa örneğidir.

Atanır ve bu sorundan etkilenen hangi ürünler denetlemek için başarısız olan kullanıcılara görebilirsiniz.

## <a name="how-do-you-manage-licenses-for-products-with-prerequisites"></a>Önkoşul ile ürünler için lisans nasıl yönettiğiniz?

Kendi bazı Microsoft Online ürünleri *eklentileri*. Eklentilerin lisans atanmış bir kullanıcı veya grup için etkinleştirilebilmesi için önce bir önkoşul hizmet planı gerektirir. Grup tabanlı lisanslama ile sistem önkoşul ve eklenti hizmet planları aynı grupta mevcut olmalıdır. Bu gruba eklenen kullanıcılar tam olarak çalışma ürün aldığından emin olmak için gerçekleştirilir. Şimdi aşağıdaki örneği göz önünde bulundurun:

Microsoft iş yeri analizi eklentisi bir üründür. Aynı ada sahip bir tek hizmet planı içerir. Aşağıdaki Önkoşullar biri de atandığında, biz bu hizmet planı yalnızca bir kullanıcı veya grup için atayabilirsiniz:
- Exchange Online (Plan 1) 
- Exchange Online (Plan 2)

Biz bu ürünü kendi başına bir gruba atamak çalışırsanız, portalı bir hata döndürür. Hata bildirim seçilmesiyle, aşağıdaki ayrıntıları gösterir:

![Grup, önkoşul eksik](media/active-directory-licensing-group-problem-resolution-azure-portal/group-prerequisite-required.png)

Biz ayrıntıları seçerseniz, aşağıdaki hata iletisini gösterir:

>Lisans işlemi başarısız oldu. Grup ekleme veya bağımlı bir hizmet kaldırmadan önce gerekli hizmetler olduğundan emin olun. **Microsoft çalışma alanı Analytics hizmeti, Exchange Online (2 de etkinleştirilmesi için planlama) gerektirir.**

Bu eklenti lisansı bir gruba atamak için biz grubu da önkoşul hizmet planı içeren emin olmalısınız. Örneğin, biz tam Office 365 E3 ürün zaten var. Varolan bir grubu güncelleştirin ve eklenti ürünü ekleyin.

İş eklenti yapmak için yalnızca en az gerekli ürün içeren bir tek başına grup oluşturmak mümkündür. Yalnızca seçilen kullanıcılar eklenti ürünü için lisans için kullanılabilir. Bu örnekte, biz aşağıdaki ürünleri aynı grubuna atadığınız:
- Office 365 Kurumsal E3 yalnızca Exchange Online (planı 2) hizmet planı ile etkin
- Microsoft iş yeri analizi

![Grup, önkoşul dahil](media/active-directory-licensing-group-problem-resolution-azure-portal/group-addon-with-prerequisite.png)

Artık bu gruba eklenen herhangi bir kullanıcının bir lisans E3 ürünün ve çalışma alanına Analytics ürününün bir lisans tüketir. Aynı zamanda bu kullanıcıların tam E3 ürün veren başka bir grubun üyesi olabilir ve bunlar hala bu ürün için yalnızca bir lisans kullanmak.

> [!TIP]
> Her önkoşul hizmet plan için birden çok gruplar oluşturabilirsiniz. Örneğin, kullanıcılarınız için Office 365 Kurumsal E1 ve Office 365 Kurumsal E3 kullanırsanız, lisans Microsoft çalışma alanına Analytics için iki grup oluşturabilirsiniz: E1 önkoşul ve E3 kullanan diğer kullanan. Bu, kullanıcıların E1 ve E3 eklentiye Ek lisanslar kullanmasa bile Dağıt sağlar.

## <a name="license-assignment-fails-silently-for-a-user-due-to-duplicate-proxy-addresses-in-exchange-online"></a>Lisans atamasını sessizce yinelenen proxy adreslerini nedeniyle bir kullanıcı için Exchange Online başarısız

Exchange Online kullanıyorsanız, bazı kullanıcılar kiracınızda aynı proxy adresi değerle yanlış yapılandırılmış olabilir. Grup tabanlı lisans böyle bir kullanıcıya bir lisans atamak çalıştığında başarısız olur ve bir hata kaydetmez. Bu örnekte hata kaydetmek için bu özellik Önizleme sürümünde bir sınırlama hatadır ve biz önce ele alınacaktır *genel kullanılabilirlik*.

> [!TIP]
> İlk olarak, bazı kullanıcıların bir lisansı almamış olabilir ve bu kullanıcılar için kaydedilen herhangi bir hata olduğunu fark ederseniz, yinelenen proxy adresi olup olmadığını denetleyin.
> Yinelenen proxy adresi olup olmadığını görmek için Exchange Online karşı aşağıdaki PowerShell cmdlet'ini yürütün:
```
Run Get-Recipient | where {$_.EmailAddresses -match "user@contoso.onmicrosoft.com"} | fL Name, RecipientType,emailaddresses
```
> Bu sorun hakkında daha fazla bilgi için bkz: ["Proxy adresi zaten kullanılıyor" hata iletisi:'te Exchange Online](https://support.microsoft.com/help/3042584/-proxy-address-address-is-already-being-used-error-message-in-exchange-online). Makale ayrıca hakkında bilgiler içerir [Exchange Online'a uzak PowerShell kullanarak nasıl bağlanacağınızı](https://technet.microsoft.com/library/jj984289.aspx).

Etkilenen kullanıcılar için proxy adresi sorunları çözdükten sonra lisans işleme lisansları şimdi uygulanabilir emin olmak için grubunda zorlamak emin olun.

## <a name="how-do-you-force-license-processing-in-a-group-to-resolve-errors"></a>Lisans işleme hataları gidermek için bir grup içinde nasıl zorla?

Hataları gidermek için ayırdıktan hangi adımları bağlı olarak, el ile kullanıcı durumunu güncelleştirmek için bir grup işlenmesini tetiklemek gerekli olabilir.

Örneğin, kullanıcıların doğrudan lisans anlaşmalarını kaldırarak bazı lisansları ücretsiz, tam olarak tüm kullanıcı üyeler lisans için daha önce başarısız grupları işlenmesini tetiklemek gerekir. Bir grubu yeniden işlemek için Grup bölmesine, açık Git **lisansları**ve ardından **yeniden işleyin** araç çubuğunda.

## <a name="next-steps"></a>Sonraki adımlar

Lisans Yönetimi grupları üzerinden diğer senaryolar hakkında daha fazla bilgi için aşağıdakilere bakın:

* [Azure Active Directory'deki bir gruba lisans atama](active-directory-licensing-group-assignment-azure-portal.md)
* [Grup tabanlı Azure Active Directory lisanslaması nedir?](active-directory-licensing-whatis-azure-portal.md)
* [Azure Active Directory'de Grup tabanlı lisans için tek tek lisanslı kullanıcıları geçirme](active-directory-licensing-group-migration-azure-portal.md)
* [Azure Active Directory grup tabanlı ilave senaryolar lisanslama](active-directory-licensing-group-advanced.md)
