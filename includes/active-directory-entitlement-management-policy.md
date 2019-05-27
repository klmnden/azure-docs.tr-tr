---
title: include dosyası
description: include dosyası
services: active-directory
author: rolyon
ms.service: active-directory
ms.topic: include
ms.date: 05/16/2019
ms.author: rolyon
ms.custom: include file
ms.openlocfilehash: 6711506c1e489dcbd50aedd36241affc3bbed80b
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66113401"
---
### <a name="policy-for-users-in-your-directory"></a>İlke: Dizininizdeki kullanıcılar için

İlkeniz, kullanıcıların ve grupların dizininizdeki bu erişim paketini talep edebilir olmasını istiyorsanız şu adımları izleyin.

1. İçinde **erişim isteğinde bulunabileceği kullanıcılar** bölümünden **dizininizdeki kullanıcılar için**.

1. İçinde **kullanıcıları ve grupları seçin** bölümünde **kullanıcılar ve gruplar ekleme**.

1. Belirli kullanıcılar ve grupları bölmesi, kullanıcıları ve eklemek istediğiniz grupları seçin.

    ![Erişim package - ilke seçme kullanıcılar ve gruplar](./media/active-directory-entitlement-management-policy/policy-select-users-groups.png)

1. Tıklayın **seçin** kullanıcılar ve gruplar eklemek için.

1. Aşağı atla [İlkesi: İstek](#policy-request) bölümü.

### <a name="policy-for-users-not-in-your-directory"></a>İlke: Kullanıcılar için dizin içinde değil

İlkeniz, dizininizdeki bu erişim paket isteyebileceği olmayan kullanıcılar için olmasını istiyorsanız şu adımları izleyin. Dizinleri izin verilmesi için yapılandırılmalıdır **kuruluş ilişkileri işbirliği kısıtlamaları** ayarları.

> [!NOTE]
> Henüz dizininizin, istek onaylanamıyor veya otomatik olarak onaylanan'de bir kullanıcı için bir Konuk kullanıcı hesabı oluşturulur. Konuk davet edilir, ancak bir davet e-posta almayacaksınız. Bunun yerine, kendi erişim paket atamasını gönderildiğinde bir e-posta alırsınız. Son kullanıcıların atama süresi doldu veya iptal edilmiş olduğundan varsayılan olarak, Konuk kullanıcının, artık sonraki herhangi bir erişim paket atamaları Konuk kullanıcı hesabı oturum açma engellendi ve silindi, vardır. Hiçbir erişim paket atamalarını olsa bile Konuk kullanıcılar dizininizde önbelleğinde kalıcı olarak kalması olmasını istiyorsanız, hak yönetimi yapılandırmanız için ayarları değiştirebilirsiniz.

1. İçinde **erişim isteğinde bulunabileceği kullanıcılar** bölümünden **sizin dizininizdeki kullanıcılar için**.

1. İçinde **seçin dış Azure AD directory** bölümünde **ekleme dizinleri**.

1. Bir etki alanı adını ve dış Ara girin Azure AD dizini.

1. Sağlanan dizin adı ve ilk etki alanı tarafından doğru dizinde olduğunu doğrulayın.

    > [!NOTE]
    > Dizindeki tüm kullanıcılar bu erişim paket isteği mümkün olacaktır. Bu dizin, yalnızca Search'te kullanılan etki alanı ile ilişkili tüm alt etki alanlarını kullanıcılardan içerir.

    ![Erişim package - ilke seçme dizinleri](./media/active-directory-entitlement-management-policy/policy-select-directories.png)

1. Tıklayın **Ekle** dizin eklemek için.

1. Daha fazla herhangi bir dizin eklemek için bu adımı yineleyin.

1. Tüm dizinler ekledikten sonra istediğiniz ilkeye dahil **seçin**.

1. Aşağı atla [İlkesi: İstek](#policy-request) bölümü.

### <a name="policy-none-administrator-direct-assignments-only"></a>İlke: Hiçbiri (doğrudan atama Yöneticisi yalnızca)

İlkenizin erişim isteklerini atlayabilir ve doğrudan erişim paketi belirli kullanıcılara atamanız yöneticilerin izin vermek istiyorsanız şu adımları izleyin. Kullanıcılar, erişim paket isteği gerekmez. Sona erme tarihi hala ayarlanmış, ancak hiçbir istek ayarları vardır.

1. İçinde **erişim isteğinde bulunabileceği kullanıcılar** bölümünden **yok (yalnızca yönetici doğrudan atamaları**.

    Erişim paket oluşturduğunuzda, doğrudan erişim paketi belirli iç ve dış kullanıcılara atayabilirsiniz. Bir dış kullanıcının belirtirseniz, dizininizde bir Konuk kullanıcı hesabı oluşturulur.

1. Aşağı atla [İlkesi: Sona erme](#policy-expiration) bölümü.

### <a name="policy-request"></a>İlke: İste

Kullanıcıların erişim paket istediğinde isteği bölümünde onay ayarlarını belirtin.

1. Seçilen kullanıcılardan gelen istekleri onay gerektirmek için ayarlanmış **onayı iste** geç **Evet**. Otomatik olarak onaylanan istekler için getirin **Hayır**.

1. İçinde onay gerekiyorsa **onaylayanları seçin** bölümünde **onaylayan Ekle**.

1. Select onaylayanlar bölmesinde, bir veya daha fazla kullanıcı ve/veya grupları onaylayan olarak seçin.

    Bir isteği onaylamak seçili onaylayanlar yalnızca biri gerekir. Tüm onaylayanlara onay gerekli değildir. Onay kararını hangi onaylayan isteği önce gözden geçirmeleri üzerinde temel alır.

    ![Erişim package - ilke seçme onaylayanlar](./media/active-directory-entitlement-management-policy/policy-select-approvers.png)

1. Tıklayın **seçin** onaylayanlardan eklemek için.

1. Tıklayın **Gelişmiş istek ayarlarını göster** ek ayarlar gösterilecek.

    ![Erişim package - ilke seçme dizinleri](./media/active-directory-entitlement-management-policy/policy-advanced-request.png)

1. Kullanıcıların erişim paket isteği için bir kullanıcının bir gerekçe gerektirecek şekilde **gerekçelendirme iste** için **Evet**.

1. Erişim paket için bir isteği onaylamak için gerekçe göstermesi onaylayan gerektirecek şekilde **onaylayan gerekçelendirme iste** için **Evet**.

1. İçinde **onay isteği zaman aşımı (gün)** kutusunda, onaylayanlar sahip bir isteği gözden geçirmek için süreyi belirtin. Bu gün sayısı içinde hiçbir onaylayan gözden geçirin, isteğin süresi dolar ve kullanıcının erişim paket için başka bir istek göndermeniz gerekir.

### <a name="policy-expiration"></a>İlke: Süre sonu

Sona erme bölümünde, bir kullanıcının erişim paket atamaya süresinin sona erdiği belirtin.

1. İçinde **sona erme** bölümünde, **erişim paket süresi** için **tarihinde**, **gün sayısı**, veya **hiçbir zaman**.

    İçin **tarihinde**, gelecekte bir sona erme tarihi seçin.

    İçin **gün sayısı**, 0 ile 3660 gün arasında bir sayı belirtin.

    Yaptığınız seçime göre bir kullanıcının erişim paket atamaya belirli bir tarihte, belirli sayıda gün onaylandıktan sonra süresi dolar veya hiçbir zaman.

1. Tıklayın **Gelişmiş sona erme ayarları göster** ek ayarlar gösterilecek.

1. Kullanıcı atamalarını genişletmek izin verecek şekilde ayarlanmış **erişim genişletmek kullanıcıların** için **Evet**.

    Uzantıları ilkede izin veriliyorsa, kullanıcı 14 gün e-posta alır ve ayrıca kendi erişim paket atamasını süresi dolacak şekilde ayarlanmadan önce 1 gün bunları atamasını genişletme isteyen.

    ![Erişim package - ilkeyi - sona erme tarihi](./media/active-directory-entitlement-management-policy/policy-expiration.png)

### <a name="policy-enable-policy"></a>İlke: İlkeyi etkinleştir

1. Erişim paket ilke ile kullanıcılara için hemen kullanılabilir olmasını isterseniz **Evet** ilkesini etkinleştirmek üzere.

    Erişim paket oluşturma işlemini tamamladıktan sonra her zaman gelecekteki etkinleştirebilirsiniz.

    ![Erişim package - ilke etkinleştirmek ilke ayarı](./media/active-directory-entitlement-management-policy/policy-enable.png)

1. Tıklayın **sonraki** veya **oluşturma**.
