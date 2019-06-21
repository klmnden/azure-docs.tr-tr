---
title: Erişim ve Azure zaman serisi öngörüleri önizlemesi yönetmek için güvenlik yapılandırma | Microsoft Docs
description: Bu makalede güvenlik ve izinler yönetim erişimi yapılandırma ilkeleri ve veri erişim ilkeleri Azure zaman serisi öngörüleri Preview korumak için.
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: dpalled
manager: cshankar
ms.reviewer: v-mamcge, jasonh, kfile
ms.workload: big-data
ms.topic: conceptual
ms.date: 05/01/2019
ms.custom: seodec18
ms.openlocfilehash: 69180e17714b7d7004e63dce0de82a50e1f0b3af
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/17/2019
ms.locfileid: "67164634"
---
# <a name="grant-data-access-to-an-environment"></a>Bir ortam için veri erişim izni verme

Bu makalede, iki tür erişim ilkeleri Azure zaman serisi öngörüleri önizlemesi açıklanmaktadır.

## <a name="sign-in-to-time-series-insights"></a>Time Series Insights için oturum açın

1. [Azure Portal](https://portal.azure.com/) oturum açın.
1. Time Series Insights ortamınızı bulun. Girin `Time Series` içinde **arama** kutusu. Seçin **zaman serisi ortamı** arama sonuçlarında.
1. Listeden Zaman Serisi Görüşleri ortamınızı seçin.

## <a name="grant-data-access"></a>Veri erişim izni verme

Bir kullanıcı asıl veri erişimi vermek için aşağıdaki adımları izleyin.

1. Seçin **veri erişimi ilkeleri**ve ardından **+ Ekle**.

    [![Veri erişim bir](media/data-access/data-access-one.png)](media/data-access/data-access-one.png#lightbox)

1. Seçin **Kullanıcı Seç**. Eklemek istediğiniz kullanıcı bulmak kullanıcı adı veya e-posta adresi arayın. Seçin **seçin** Seçimi onaylamak için.

    [![Veri erişim iki](media/data-access/data-access-two.png)](media/data-access/data-access-two.png#lightbox)

1. Seçin **rol seçme**. Kullanıcı için uygun erişim rolü seçin:

    * Seçin **katkıda bulunan** kullanıcıyı başvuru verileri ve kaydedilmiş paylaşımı sorguları ve Perspektifleri ortamın diğer kullanıcılarla değiştirmeye izin vermek istiyorsanız.

    * Aksi takdirde seçin **okuyucu** ortamda veri verin ve kişisel, paylaşılan, sorguları ortama kaydetmesine.

   Seçin **Tamam** rolü seçiminizi onaylamak için.

    [![Veri erişim üç](media/data-access/data-access-three.png)](media/data-access/data-access-three.png#lightbox)

1. Seçin **Tamam** üzerinde **kullanıcı rolü Seç** sayfası.

    [![Veri erişim dört](media/data-access/data-access-four.png)](media/data-access/data-access-four.png#lightbox)

1. Onaylayın **veri erişimi ilkeleri** sayfası, kullanıcıları ve her kullanıcı için rolleri listeler.

    [![Veri erişim beş](media/data-access/data-access-five.png)](media/data-access/data-access-five.png#lightbox)

## <a name="provide-guest-access-from-another-aad-tenant"></a>Başka bir AAD kiracısı Konuk erişimi sağlar

`Guest` Yönetim rolü değil. Bu, başka bir kiracıdaki davet bir hesap için kullanılan bir terimdir. Konuk hesabı kiracının dizine davet sonra aynı erişim denetimi gibi başka bir hesap uygulanmış sahip olabilir. Erişim denetimi (IAM) dikey penceresini kullanarak bir zaman serisi görüşleri ortamına yönetim erişim verebilirsiniz. Ya da veri erişimi ilkeleri dikey penceresi aracılığıyla ortam içindeki verilere erişim izni verebilirsiniz. Azure Active Directory (Azure AD) kiracısı Konuk erişimi hakkında daha fazla bilgi için okuma [ekleme Azure Active Directory B2B işbirliği kullanıcılarını Azure portalında](https://docs.microsoft.com/azure/active-directory/b2b/add-users-administrator).

Başka bir kiracıdaki Azure AD kullanıcısı için bir zaman serisi görüşleri ortamına Konuk erişimi vermek için aşağıdaki adımları izleyin.

1. Seçin **veri erişimi ilkeleri**ve ardından **+ davet**.

    [![Data-access-six](media/data-access/data-access-six.png)](media/data-access/data-access-six.png#lightbox)

1. Davet etmek istediğiniz kullanıcı için e-posta adresi girin. Bu e-posta adresi, Azure AD ile ilişkilendirilmelidir. İsteğe bağlı olarak davete kişisel bir ileti içerebilir.

    [![Veri erişim yedi](media/data-access/data-access-seven.png)](media/data-access/data-access-seven.png#lightbox)

1. Ekranda görünen onay Kabarcık arayın.

    [![Veri erişim sekiz](media/data-access/data-access-eight.png)](media/data-access/data-access-eight.png#lightbox)

1. Seçin **Kullanıcı Seç**. Konuk kullanıcı eklemek istediğiniz kullanıcı bulmak için davet e-posta adresini arayın. Ardından, **seçin** Seçimi onaylamak için.

    [![Veri erişim dokuz](media/data-access/data-access-nine.png)](media/data-access/data-access-nine.png#lightbox)

1. Seçin **rol seçme**. Konuk kullanıcı için uygun erişim rolü seçin:

    * Seçin **katkıda bulunan** kullanıcıyı başvuru verileri ve kaydedilmiş paylaşımı sorguları ve Perspektifleri ortamın diğer kullanıcılarla değiştirmeye izin vermek istiyorsanız.

    * Aksi takdirde seçin **okuyucu** ortamda veri verin ve kişisel, paylaşılan, sorguları ortama kaydetmesine.

   Seçin **Tamam** rolü seçiminizi onaylamak için.

    [![Veri erişim on](media/data-access/data-access-ten.png)](media/data-access/data-access-ten.png#lightbox)

1. Seçin **Tamam** üzerinde **kullanıcı rolü Seç** sayfası.

1. Onaylayın **veri erişimi ilkeleri** sayfasında, Konuk kullanıcı ve her bir Konuk kullanıcı için rolleri listeler.

    [![Veri-erişim-on](media/data-access/data-access-eleven.png)](media/data-access/data-access-eleven.png#lightbox)

1. Artık Konuk kullanıcı için bunları davet Azure kiracısında bulunan bir ortama erişmek için adımları izlemeniz gerekir. İlk olarak, bunlar onlara gönderdiğiniz daveti kabul edin. Bu davet, 5. adımda kullandığınız e-posta adresine e-posta ile gönderilir. Seçmeleri **Başlarken** kabul etmek için.

    [![Veri erişim on](media/data-access/data-access-twelve.png)](media/data-access/data-access-twelve.png#lightbox)

1. Ardından, Konuk kullanıcı, yöneticinin kuruluşla ilişkili izinleri kabul eder.

    [![Veri-erişim-On üç](media/data-access/data-access-thirteen.png)](media/data-access/data-access-thirteen.png#lightbox)

1. Konuk kullanıcı oturum açtıktan sonra onları davet etmek için kullanılan e-posta adresine ve daveti kabul, bunlar için insights.azure.com gidin. Sonra e-posta adresi yanındaki avatar ekranın sağ üst köşedeki seçerler.

    [![Veri-erişim-on dört](media/data-access/data-access-fourteen.png)](media/data-access/data-access-fourteen.png#lightbox)

1. Yanında, Azure directory aşağı açılan menüden Kiracı Konuk kullanıcı seçer. Bu Kiracı için bunları davet olur.

    [![Veri erişim beş](media/data-access/data-access-fifteen.png)](media/data-access/data-access-fifteen.png#lightbox)

Kiracınıza Konuk kullanıcı seçtikten sonra bunları erişim sağladığınız zaman serisi görüşleri ortamına görürler. Artık bunları ile sağlanan rol ile ilişkili olan tüm özellikleri sahip oldukları **5. adım**.

## <a name="next-steps"></a>Sonraki adımlar

* Bilgi [Azure Event Hubs olay kaynağı ekleme](./time-series-insights-how-to-add-an-event-source-eventhub.md) zaman serisi görüşleri ortamınıza.

* Gönderme [olayları için olay kaynağı](./time-series-insights-send-events.md).

* Görünüm [zaman serisi öngörüleri Önizleme Gezgini ortamınızda](./time-series-insights-update-explorer.md).
