---
title: Azure Access Control Service (ACS) ad alanları yeniden etkinleştirme devre dışı
description: Bul ve Azure Access Control Service (ACS) ad alanlarınıza etkinleştirmek ve 4 Şubat 2019'e kadar etkin saklamak için bir uzantı isteği hakkında bilgi edinin.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/21/2019
ms.author: celested
ms.reviewer: jlu
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 534c0463cd8aee9fccfa183586b946032dada722
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60299728"
---
# <a name="how-to-reactivate-disabled-access-control-service-namespaces"></a>Nasıl yapılır: Devre dışı bırakılmış Access Control Service ad alanlarını yeniden etkinleştirme

Microsoft Azure Access Control Service (ACS), Azure Active Directory (Azure AD), bir hizmet 7 Kasım 2018'de kullanımdan kaldırıldığında, Kasım 2017'de duyurduk.

Bu yana daha sonra 6 ay, 3 ay, 1 ay, 2 haftalık, 1 hafta, ve 1 gün önce o tarihten 7 Kasım 2018 9 ay içerisinde 12 ay ACS kullanımdan kaldırma hakkında ACS abonelikleri yönetici e-postası için e-posta gönderdik.

3 Ekim 2018'de duyurduk (e-posta aracılığıyla ve [bir blog gönderisi](https://azure.microsoft.com/blog/one-month-retirement-notice-access-control-service/)) bir uzantı teklif müşterilerine 7 Kasım 2018'den önce geçiş işleminizi tamamlayamıyoruz. Duyuruyu uzantısı istemeye ilişkin yönergeler de gerekmektedir.

## <a name="why-your-namespace-is-disabled"></a>Ad alanınız neden devre dışı bırakıldı

Uzantısı için de sabitlemeyi henüz seçtiyseniz 7 Kasım 2018 tarihinden itibaren ACS ad alanları devre dışı bırakmak başlayacağız. Uzantı 4 Şubat 2019 için'zaten istediniz gerekir; Aksi takdirde, PowerShell aracılığıyla ad alanlarını etkinleştirmek mümkün olmayacaktır.

> [!NOTE]
> Bir Hizmet Yöneticisi veya aboneliğin ortak Yöneticisi için PowerShell komutlarını çalıştırın ve bir uzantı isteği olması gerekir.

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

        ```powershell
        Get-Help [Command-Name] -Full
        ```
    
        Burada `[Command-Name]` ACS komut adıdır.
1. ACS kullanarak bağlan **Connect AcsAccount** cmdlet'i. 

    Çalıştırarak yürütme ilkenizi değiştirmeniz gerekebilir **Set-ExecutionPolicy** komutu çalıştırmadan önce.
1. Kullanarak mevcut Azure aboneliklerinizi listesinde **Get-AcsSubscription** cmdlet'i.
1. ACS kullanarak ad alanları listesi **Get-AcsNamespace** cmdlet'i.
1. Ad alanlarını onaylayarak devre dışı olduğunu onaylayın `State` olduğu `Disabled`.

    [![Ad alanlarını devre dışı olduğunu onaylayın](./media/howto-reactivate-disabled-acs-namespaces/confirm-disabled-namespace.png)](./media/howto-reactivate-disabled-acs-namespaces/confirm-disabled-namespace.png#lightbox)

    Ayrıca `nslookup {your-namespace}.accesscontrol.windows.net` etki alanı hala etkin olup olmadığını onaylamak için.

1. ACS kullanarak namespace (s) etkinleştirme **etkinleştir AcsNamespace** cmdlet'i.

    Böylece namespace (s) yeniden Şubat 4 2019 önce devre dışı olmaz, namespace (s) etkinleştirdikten sonra bir uzantı isteyebilir. Bu tarihten sonra ACS tüm istekleri başarısız olur.

## <a name="request-an-extension"></a>Bir uzantı isteği

Yeni Uzantı istekleri 21 Ocak 2019'üzerinde başlatma sürüyor.

4 Şubat 2019 uzantıları isteyen müşteriler için ad alanları devre dışı bırakma başlayacağız. PowerShell ile ad alanlarını yeniden etkinleştirebilirsiniz, ancak ad alanlarını 48 saat sonra tekrar devre dışı bırakılır.

4 Mart 2019'dan sonra müşteriler artık PowerShell üzerinden herhangi bir ad alanları yeniden etkinleştirmek mümkün olacaktır.

Daha fazla uzantıları artık otomatik olarak onaylanır. Geçirmek için ek süreye ihtiyaç duyarsanız, kişi [Azure Destek](https://portal.azure.com/#create/Microsoft.Support) ayrıntılı geçiş zaman çizelgesi sağlamak için.

### <a name="to-request-an-extension"></a>Bir uzantı istemek için

1. Azure portalında oturum açın ve oluşturma bir [yeni destek isteği](https://portal.azure.com/#create/Microsoft.Support).
1. Aşağıdaki örnekte gösterildiği gibi yeni destek isteği formunu doldurun.

    | Destek isteği alanı | Değer |
    |-----------------------|--------------------|
    | **Sorun türü** | `Technical` |
    | **Abonelik** | Aboneliğinizi ayarlama |
    | **Hizmet** | `All services` |
    | **Kaynak** | `General question/Resource not available` |
    | **Sorun türü** | `ACS to SAS Migration` |
    | **Konu** | Sorunu açıklayın |

   ![Yeni teknik destek isteği](./media/howto-reactivate-disabled-acs-namespaces/new-technical-support-request.png)

<!--

1. Navigate to your ACS namespace's management portal by going to `https://{your-namespace}.accesscontrol.windows.net`.
1. Select the **Read Terms** button to read the [updated Terms of Use](https://azure.microsoft.com/support/legal/access-control/), which will direct you to a page with the updated Terms of Use.

    [![Select the Read Terms button](./media/howto-reactivate-disabled-acs-namespaces/read-terms-button-expanded.png)](./media/howto-reactivate-disabled-acs-namespaces/read-terms-button-expanded.png#lightbox)

1. Select **Request Extension** on the banner at the top of the page. The button will only be enabled after you read the [updated Terms of Use](https://azure.microsoft.com/support/legal/access-control/).

    [![Select the Request Extension button](./media/howto-reactivate-disabled-acs-namespaces/request-extension-button-expanded.png)](./media/howto-reactivate-disabled-acs-namespaces/request-extension-button-expanded.png#lightbox)

1. After the extension request is registered, the page will refresh with a new banner at the top of the page.

    [![Updated page with refreshed banner](./media/howto-reactivate-disabled-acs-namespaces/updated-banner-expanded.png)](./media/howto-reactivate-disabled-acs-namespaces/updated-banner-expanded.png#lightbox)
-->

## <a name="help-and-support"></a>Yardım ve destek

- Bu nasıl yapılır yararlandıktan sonra herhangi bir sorunla karşılaşırsanız çalıştırırsanız, kişi [Azure Destek](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview).
- Sorularınız veya ACS devre dışı bırakma hakkında Geri bildiriminiz varsa, adresinden bize başvurun acsfeedback@microsoft.com.

## <a name="next-steps"></a>Sonraki adımlar

- ACS emekli ilgili bilgileri gözden [nasıl yapılır: Azure erişim denetimi Hizmeti'nden geçiş](active-directory-acs-migration.md).
