---
title: Erişim ve Azure Time Series Insights'ı yönetmek için güvenlik yapılandırma | Microsoft Docs
description: Bu makalede güvenlik ve izinler yönetim erişimi yapılandırma ilkeleri ve veri erişim ilkeleri Azure Time Series Insights güvenliğini sağlamak için.
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: anshan
manager: cshankar
ms.reviewer: v-mamcge, jasonh, kfile, anshan
ms.workload: big-data
ms.topic: conceptual
ms.date: 11/26/2018
ms.openlocfilehash: 3694ff0db63d685cb63f586062158b4f8acac5cf
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52847618"
---
# <a name="grant-data-access-to-an-environment"></a>Bir ortam için veri erişim izni verme

Bu makalede, iki tür erişim ilkeleri Azure Time Series Insights (Önizleme) açıklanmaktadır.

## <a name="grant-data-access"></a>Veri erişim izni verme

Bir kullanıcı asıl veri erişimi vermek için aşağıdaki adımları izleyin:

1. [Azure Portal](https://portal.azure.com/)’da oturum açın.
1. Zaman serisi öngörüleri (TSI) ortamınızı bulun. Tür `Time Series` içinde **arama** kutusu. Seçin **zaman serisi ortamı** arama sonuçlarında.
1. TSI ortamınızı listeden seçin.
1. Seçin **veri erişimi ilkeleri**, ardından **+ Ekle**.

    ![Veri erişim bir][1]

1. Seçin **Kullanıcı Seç**. Eklemek istediğiniz kullanıcı bulmak kullanıcı adı veya e-posta adresi arayın. Tıklayın **seçin** Seçimi onaylamak için.

    ![Veri erişim iki][2]

1. Seçin **rol seçme**. Kullanıcı için uygun erişim rolü seçin:

    * Seçin **katkıda bulunan** başvuru verileri ve kaydedilmiş paylaşımı sorguları ve Perspektifleri ortamın diğer kullanıcılarla değiştirmek kullanıcı izin vermek istiyorsanız.

    * Aksi takdirde seçin **okuyucu** ortamında kullanıcı veri izin verme veya kişisel (paylaşılmayan) sorguları ortama kaydetmesine.

    Seçin **Tamam** rolü seçiminizi onaylamak için.

    ![Veri erişim üç][3]

1. Seçin **Tamam** içinde **kullanıcı rolü Seç** sayfası.

    ![Veri erişim dört][4]

1. **Veri erişimi ilkeleri** sayfası, kullanıcıları ve her kullanıcı için rolleri listeler.

    ! [veri erişim beş[5]

## <a name="provide-guest-access-to-a-user-from-another-azure-active-directory-tenant"></a>Konuk erişimi bir kullanıcı başka bir Azure Active Directory kiracısı sağlar.

`Guest` Yönetim rolü değil; Bu, bir kiracıdan diğerine davet bir hesap için kullanılan bir terimdir. Konuk hesabı kiracının dizine davet edildi sonra aynı erişim denetimi gibi başka bir hesap, erişim denetimi (IAM) dikey penceresini kullanarak bir TSI ortamına yönetim erişimi vermek veya verilere erişim uygulanmış olabilir Veri erişimi ilkeleri dikey penceresi aracılığıyla ortam. Azure Active Directory (AAD) Kiracı Konuk erişimi hakkında daha fazla bilgi için okuma [ekleme Azure Active Directory B2B işbirliği kullanıcılarını Azure portalında](https://docs.microsoft.com/azure/active-directory/b2b/add-users-administrator).

TSI ortam bir AAD kullanıcısı için başka bir kiracıdaki Konuk erişimi vermek için aşağıdaki adımları izleyin:

1. [Azure Portal](https://portal.azure.com/) oturum açın.
1. TSI ortamınızı bulun. Tür **zaman serisi** arama kutusuna. Seçin **zaman serisi ortamı** arama sonuçlarında.
1. TSI ortamınızı listeden seçin.
1. Seçin **veri erişimi ilkeleri**, ardından **+ davet**.

    ![Veri erişim altı][6]

1. Davet etmek istediğiniz kullanıcının e-posta sağlayın. Bu, AAD ile ilişkili bir e-posta olması unutmayın. İsteğe bağlı olarak davete kişisel bir ileti içerebilir.

    ![Veri erişim yedi][7]

1. Ekrandaki bir onay Kabarcık görmeniz gerekir.

    ![Veri erişim sekiz][8]

1. Seçin **Kullanıcı Seç**. Eklemek istediğiniz kullanıcı bulmak için yalnızca davet Konuk kullanıcı e-posta adresini arayın. Tıklayın **seçin** Seçimi onaylamak için.

    ![Veri erişim dokuz][9]

1. Seçin **seçin** rol. Konuk kullanıcı için uygun erişim rolü seçin:

    1. Seçin **katkıda bulunan** kullanıcıyı başvuru verileri ve kaydedilmiş paylaşımı sorguları ve Perspektifleri ortamın diğer kullanıcılarla değiştirmeye izin vermek istiyorsanız.
    1. Aksi takdirde seçin **okuyucu** ortamında kullanıcı veri izin verme veya kişisel (paylaşılmayan) sorguları ortama kaydetmesine.
    1. Seçin **Tamam** rolü seçiminizi onaylamak için.

    ![Veri erişim on][10]

1. Seçin **Tamam** içinde **kullanıcı rolü Seç** sayfası.

1. **Veri erişimi ilkeleri** sayfası artık Konuk kullanıcı ve her bir Konuk kullanıcı için rolleri listeler.

    ![Veri-erişim-on][11]

1. Konuk kullanıcı, Azure'da yer alan ortama erişmek için adımlar, yalnızca Kiracı belirli bir işlem yapmanız gerekir artık bunları davet. İlk olarak, yalnızca onlara gönderdiğiniz kendisinden daveti kabul etmesi gerekir. Bu davet, adım 5'te davet e-posta adresine e-posta ile gönderilir. ' A tıklamalıdır **Başlarken**kabul etmek için.

    ![Veri erişim on][12]

1. Ardından, Konuk kullanıcı, yöneticinin kuruluşla ilişkili izinlerini kabul etmenizi gerekir.

    ![Veri-erişim-On üç][13]

1. Sizi davet e-posta adresine Konuk kullanıcı oturum açtığında ve daveti kabul, insights.azure.com için head. Bir kez, bunlar yanındaki kendi e-posta ekranın sağ alt köşesindeki avatar seçeneğine tıklamanız gerekir.

    ![Veri-erişim-on dört][14]

1. Konuk kullanıcı Azure seçip ardından, dizin aşağı açılan menüden Kiracı. Bunları davet Kiracı budur.

    ![Veri erişim beş][15]

1. Son olarak, Konuk kullanıcı kiracınızın seçtiğinde, yalnızca bunlara erişimi sağlanan TSI ortamı görürsünüz. Artık tüm özellikleri adım 8'deki sağlanan rol ile ilişkili olması gerekir.

## <a name="next-steps"></a>Sonraki adımlar

* Bilgi [bir Event Hub olay kaynağı ekleme](./time-series-insights-how-to-add-an-event-source-eventhub.md) Azure TSI ortamınıza.
* Gönderme [olayları için olay kaynağı](./time-series-insights-send-events.md).
* Görünüm [Time Series Insights gezgininin ortamınızda](./time-series-insights-update-explorer.md).

<!-- Images -->
[1]: media/data-access/data-access-one.png
[2]: media/data-access/data-access-two.png
[3]: media/data-access/data-access-three.png
[4]: media/data-access/data-access-four.png
[5]: media/data-access/data-access-five.png
[6]: media/data-access/data-access-six.png
[7]: media/data-access/data-access-seven.png
[8]: media/data-access/data-access-eight.png
[9]: media/data-access/data-access-nine.png
[10]: media/data-access/data-access-ten.png
[11]: media/data-access/data-access-eleven.png
[12]: media/data-access/data-access-twelve.png
[13]: media/data-access/data-access-thirteen.png
[14]: media/data-access/data-access-fourteen.png
[15]: media/data-access/data-access-fifteen.png