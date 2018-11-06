---
title: Azure Access Control Service (ACS) ad alanları yeniden etkinleştirme devre dışı
description: Bul ve Azure Access Control Service (ACS) ad alanlarınıza etkinleştirmek ve 4 Şubat 2019'e kadar etkin saklamak için bir uzantı isteği hakkında bilgi edinin.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 11/07/2018
ms.author: celested
ms.reviewer: jlu
ms.custom: aaddev
ms.openlocfilehash: 298afda7549690a9ea0314bff63a714be50a33b9
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/05/2018
ms.locfileid: "51019355"
---
# <a name="how-to-reactivate-disabled-access-control-service-namespaces"></a>Nasıl yapılır: yeniden etkinleştirme devre dışı, Access Control Service ad alanları

Microsoft Azure Access Control Service (ACS), Azure Active Directory (Azure AD), bir hizmet 7 Kasım 2018'de kullanımdan kaldırıldığında, Kasım 2017'de duyurduk.

Bu yana daha sonra 6 ay, 3 ay, 1 ay, 2 haftalık, 1 hafta, ve 1 gün önce o tarihten 7 Kasım 2018 9 ay içerisinde 12 ay ACS kullanımdan kaldırma hakkında ACS abonelikleri yönetici e-postası için birden çok e-posta gönderdik.

3 Ekim 2018'de duyurduk (e-posta aracılığıyla ve [bir blog gönderisi](https://azure.microsoft.com/blog/one-month-retirement-notice-access-control-service/)) bir uzantı teklif müşterilerine 7 Kasım 2018'den önce geçiş işleminizi tamamlayamıyoruz. Duyuruyu uzantısı istemeye ilişkin yönergeler de içeriyor.

## <a name="why-your-namespace-is-disabled"></a>Ad alanınız neden devre dışı bırakıldı

Uzantısı için de sabitlemeyi henüz seçtiyseniz 7 Kasım 2018 tarihinden itibaren ACS ad alanları devre dışı bırakmak başlayacağız. İletişim eksik ve 4 Şubat 2019 uzantısı için katılım hala istiyorsanız aşağıdaki bölümlerdeki yönergeleri izleyin.

> [!NOTE]
> PowerShell komutlarını çalıştırın ve bir uzantı isteği aboneliğinin Yöneticisi olmalıdır.

## <a name="find-and-enable-your-acs-namespaces"></a>Bulma ve ACS ad alanlarınıza etkinleştir

Tüm ACS ad listesi ve devre dışı bırakıldı, yeniden etkinleştirmek için ACS PowerShell kullanabilirsiniz.

1. ACS PowerShell'i indirip yükleyin:
    1. Git PowerShell Galerisi'nden ve indirme [Acs.Namespaces](https://www.powershellgallery.com/packages/Acs.Namespaces/1.0.2).
    1. Modülünü yükleyin:

        ```powershell
        Install-Module -Name Acs.Namespaces
        ```

    1. Tüm olası komutların bir listesini alın:

        ```powershell
        Get-Command -Module Acs.Namespaces
        ```

        Belirli bir komutla ilgili Yardım almak için şunu çalıştırın:

        ```
        Get-Help [Command-Name] -Full
        ```
    
        Burada `[Command-Name]` ACS komut adıdır.
1. Kullanarak mevcut Azure aboneliklerinizi listesinde **Get-AcsSubscription** cmdlet'i.
1. ACS kullanarak ad alanları listesi **Get-AcsNamespace** cmdlet'i.
1. Ad alanlarını onaylayarak devre dışı olduğunu onaylayın `State` olduğu `Disabled`.

    [![Ad alanlarını devre dışı olduğunu onaylayın](./media/howto-reactivate-disabled-acs-namespaces/confirm-disabled-namespace.png)](./media/howto-reactivate-disabled-acs-namespaces/confirm-disabled-namespace.png#lightbox)

    Ayrıca `nslookup {your-namespace}.accesscontrol.windows.net` etki alanı hala etkin olup olmadığını onaylamak için.

1. ACS kullanarak namespace (s) etkinleştirme **etkinleştir AcsNamespace** cmdlet'i.

    Böylece namespace (s) yeniden Şubat 4 2019 önce devre dışı olmaz, namespace (s) etkinleştirdikten sonra bir uzantı isteyebilir. Bu tarihten sonra ACS tüm istekleri başarısız olur.

## <a name="request-an-extension"></a>Bir uzantı isteği

1. ACS ad alanının Yönetim Portalı'na giderek gidin `https://{your-namespace}.accesscontrol.windows.net`.
1. Seçin **okuma koşulları** okumak için düğme [kullanım güncelleştirilmiş](https://azure.microsoft.com/support/legal/access-control/), hangi doğrudan, güncelleştirilmiş kullanım koşullarını içeren bir sayfa için.

    [![Okuma koşulları düğmeyi seçin](./media/howto-reactivate-disabled-acs-namespaces/read-terms-button-expanded.png)](./media/howto-reactivate-disabled-acs-namespaces/read-terms-button-expanded.png#lightbox)

1. Seçin **uzantı isteği** sayfanın üst kısmındaki başlık çubuğunda. Düğme yalnızca, okuduktan sonra etkinleştirilecek [kullanım güncelleştirilmiş](https://azure.microsoft.com/support/legal/access-control/).

    [![Uzantı isteği düğmeyi seçin](./media/howto-reactivate-disabled-acs-namespaces/request-extension-button-expanded.png)](./media/howto-reactivate-disabled-acs-namespaces/request-extension-button-expanded.png#lightbox)

1. Uzantı isteği kaydedildikten sonra sayfa yeni bir başlığı sayfanın üstündeki ile yenilenir.

    [![Yenilenmiş başlığı ile güncelleştirilmiş sayfası](./media/howto-reactivate-disabled-acs-namespaces/updated-banner-expanded.png)](./media/howto-reactivate-disabled-acs-namespaces/updated-banner-expanded.png#lightbox)

## <a name="help-and-support"></a>Yardım ve destek

- Bu nasıl yapılır yararlandıktan sonra herhangi bir sorunla karşılaşırsanız çalıştırırsanız, kişi [Azure Destek](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview).
- Sorularınız veya ACS devre dışı bırakma hakkında Geri bildiriminiz varsa, adresinden bize başvurun acsfeedback@microsoft.com.

## <a name="next-steps"></a>Sonraki adımlar

- ACS emekli ilgili bilgileri gözden [nasıl yapılır: Azure erişim denetimi Hizmeti'nden geçiş](active-directory-acs-migration.md).
