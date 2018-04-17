---
title: Azure Güvenlik Merkezi ve Azure SQL veritabanı hizmetinin | Microsoft Docs
description: Bu makalede, Güvenlik Merkezi, Azure SQL veritabanı veritabanlarında güvenli nasıl yardımcı olabileceğini gösterir.
services: sql-database
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: ''
ms.assetid: f109adfd-daed-4257-9692-2042a1399480
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/02/2017
ms.author: terrylan
ms.openlocfilehash: b507a62db9a80866005cb63d2008fb14612b516f
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="azure-security-center-and-azure-sql-database-service"></a>Azure Güvenlik Merkezi ve Azure SQL veritabanı hizmeti
[Azure Güvenlik Merkezi](https://azure.microsoft.com/documentation/services/security-center/), tehditleri önlemenize, algılamanıza ve yanıtlamanıza yardımcı olur. Aboneliklerinizde, tümleşik güvenlik izleme ve ilke yönetimi sağlar; normal koşullarda gözden kaçabilecek tehditleri algılamaya yardımcı olur ve güvenlik çözümlerinin geniş ekosistemiyle çalışır.

Bu makalede, Güvenlik Merkezi, Azure SQL veritabanı veritabanlarında güvenli nasıl yardımcı olabileceğini gösterir.

## <a name="why-use-security-center"></a>Güvenlik Merkezi neden kullanılır?
Güvenlik Merkezi, tüm sunucular ve veritabanları güvenliğini görünürlük sağlayarak SQL veritabanındaki verileri korumaya yardımcı olur. Güvenlik Merkezi ile şunları yapabilirsiniz:

* SQL veritabanı şifreleme ve denetim ilkeleri tanımlar.
* SQL veritabanı kaynaklarınızın güvenliğini tüm aboneliklerinizi izleyin.
* Hızlı bir şekilde tanımlamak ve güvenlik sorunları düzeltin.
* Gelen uyarılar tümleştirmek [Azure SQL veritabanı tehdit algılama](../sql-database/sql-database-threat-detection.md).

SQL veritabanı kaynaklarınızın korunmasına yardımcı olma ek olarak, Güvenlik Merkezi güvenlik izleme ve de Azure sanal makineler, bulut Hizmetleri, uygulama hizmetleri, sanal ağlar ve daha fazla bilgi için yönetim sağlar. Güvenlik Merkezi hakkında daha fazla bilgi [burada](security-center-intro.md).

## <a name="prerequisites"></a>Önkoşullar
Güvenlik Merkezi ile çalışmaya başlamak için Microsoft Azure aboneliğinizin olması gerekir. Ücretsiz katmanı Güvenlik Merkezi, aboneliğiniz ile etkinleştirilir. Güvenlik Merkezi'nin ücretsiz ve standart katmanları hakkında daha fazla bilgi için bkz: [Güvenlik Merkezi fiyatlandırma](https://azure.microsoft.com/pricing/details/security-center/).

Güvenlik Merkezi, rol tabanlı erişim destekler. Azure rol tabanlı erişim denetimi (RBAC) hakkında daha fazla bilgi için bkz: [Azure Active Directory rol tabanlı erişim denetimi](../role-based-access-control/role-assignments-portal.md). Güvenlik Merkezi ile ilgili SSS hakkında bilgi sağlar. [izinleri Güvenlik Merkezi'nde işlenme](security-center-faq.md#permissions).

## <a name="access-security-center"></a>Güvenlik Merkezi'ne erişme
Güvenlik Merkezi'ne [Azure portalından](https://azure.microsoft.com/features/azure-portal/) erişebilirsiniz. [Portalı oturum açma](https://portal.azure.com/) seçip **Güvenlik Merkezi seçeneği**.

![Güvenlik Merkezi seçeneği][1]

**Güvenlik Merkezi** dikey pencere açılır.
![Güvenlik Merkezi dikey penceresi][2]

## <a name="set-security-policy"></a>Güvenlik ilkesi ayarlama
Bir güvenlik ilkesi belirtilen abonelik veya kaynak grubu içindeki kaynaklar için önerilen denetimleri kümesini tanımlar. Güvenlik Merkezi'nde abonelik veya kaynak grupları, şirketinizin güvenlik gereksinimlerini ve uygulamaların türüne ya da her Abonelikteki verilerin duyarlılığına göre ilkeleri tanımlarsınız.

SQL denetimi ve SQL saydam veri şifreleme (TDE) için öneriler göstermek için bir ilke ayarlayabilirsiniz.

* Ne zaman açmanız **SQL denetim ve tehdit algılama**, Güvenlik Merkezi, Azure veritabanına erişim denetimi uyumluluk, Gelişmiş algılama ve araştırma amacıyla etkinleştirilmesini önerir.
* Ne zaman açmanız **SQL saydam veri şifrelemesi**, Güvenlik Merkezi önerir bekleyen şifrelemenin Azure SQL veritabanınızı, ilişkili yedeklemelerinizi ve işlem günlüğü dosyalarını etkin olmalıdır.

Güvenlik ilkesini ayarlamak için seçin **İlkesi** döşeme Güvenlik Merkezi dikey penceresinde. Üzerinde **Güvenlik İlkesi** dikey penceresinde güvenlik ilkesini etkinleştirmek istediğiniz aboneliği seçin. Seçin **önleme İlkesi** ve **üzerinde** bu abonelikte kullanmak istediğiniz güvenlik önerilerini.
![Güvenlik ilkesi][3]

Daha fazla bilgi için bkz: [güvenlik ilkelerini ayarlama](security-center-policies.md).

## <a name="manage-security-recommendation"></a>Güvenlik açısından yönetme
Güvenlik Merkezi düzenli aralıklarla Azure kaynaklarınızın güvenlik durumunu çözümler. Güvenlik Merkezi olası güvenlik açıklarını belirlediğinde öneriler oluşturur. Gerekli denetimlerin yapılandırılması işlemi boyunca öneriler size rehberlik eder.

Bir güvenlik ilkesi ayarladıktan sonra Güvenlik Merkezi olası güvenlik açıklarını tanımlamak amacıyla kaynaklarınızın güvenlik durumunu çözümler. Öneriler her satırın belirli bir önerinin temsil ettiği bir tablo biçiminde görüntülenir. Aşağıdaki tabloda kullanılabilir önerilerini Azure SQL Database ve onu uygularsanız, her bir öneri ne yaptığını anlamanıza yardımcı olması için bir başvuru olarak kullanın. Bir öneri seçilmesi Güvenlik Merkezi'nde öneriyi uygulamayı nasıl açıklayan bir makale için gerçekleştirir.

| Öneri | Açıklama |
| --- | --- |
| [Denetim ve tehdit algılama SQL sunucularında etkinleştir](security-center-enable-auditing-on-sql-servers.md) |SQL veritabanı sunucuları için Denetim ve tehdit algılama Aç önerir. (Yalnızca SQL veritabanı hizmeti. Sanal makinelerde çalışan Microsoft SQL Server içermez.) |
| [SQL veritabanlarında denetim ve tehdit algılama etkinleştir](security-center-enable-auditing-on-sql-databases.md) |SQL veritabanı veritabanları için Denetim ve tehdit algılama Aç önerir. (Yalnızca SQL veritabanı hizmeti. Sanal makinelerde çalışan Microsoft SQL Server içermez.) |
| [Saydam Veri Şifrelemesini Etkinleştirme](security-center-enable-transparent-data-encryption.md) |SQL veritabanları için şifrelemeyi etkinleştirmek önerir. (Yalnızca SQL veritabanı hizmeti.) |

Azure kaynaklarınız için önerilerini görmek için seçin **önerileri** döşeme Güvenlik Merkezi dikey penceresinde. Üzerinde **önerileri** görmek için bir öneri ayrıntıları dikey penceresinde, seçin. Bu örnekte, şimdi seçin **SQL sunucularında denetimi etkinleştirme ve tehdit algılama**.

![Öneriler][4]

Aşağıda gösterildiği gibi Güvenlik Merkezi, burada denetim ve tehdit algılama etkin SQL sunucuları gösterir. Denetim açıldıktan sonra tehdit algılama ayarlarını ve e-posta ayarlarını güvenlik uyarıları almak üzere yapılandırabilirsiniz. Tehdit algılama, veritabanına olası güvenlik tehditlerini gösteren anormal veritabanı etkinliklerini algıladığında sizi uyarır. Uyarılar, Güvenlik Merkezi panosunda görüntülenir.
![Denetim ve tehdit algılama][5]

Adımları [SQL veritabanı tehdit algılama Azure portalında](../sql-database/sql-database-threat-detection-portal.md) etkinleştirmek ve tehdit algılama yapılandırmak ve anormal etkinlikler algılandığında güvenlik uyarı alacak olan e-postaları listesini yapılandırmak için.

Öneriler hakkında daha fazla bilgi için bkz: [güvenlik önerilerini yönetme](security-center-recommendations.md).

## <a name="monitor-security-health"></a>Güvenlik durumunu izleme
Bir aboneliğin kaynakları için [güvenlik ilkelerini](security-center-policies.md) etkinleştirmenizin ardından, Güvenlik Merkezi olası güvenlik açıklarını tanımlamak amacıyla kaynaklarınızın güvenliğini analiz eder.  İçinde kaynaklarınızın güvenlik durumunu görüntüleyebilirsiniz **kaynak güvenlik durumu** döşeme. Tıkladığınızda **veri** içinde **kaynak güvenlik durumu** döşeme, **veri kaynakları** denetim ve saydam veri şifreleme etkinleştirilmemiş olması gibi sorunlar için SQL öneriler dikey pencere açılır. Ayrıca, veritabanının genel sağlık durumu için öneriler içerir.
![Kaynak güvenlik durumu][6]

Daha fazla bilgi için bkz: [güvenlik durumunu izleme](security-center-monitoring.md).

## <a name="manage-and-respond-to-security-alerts"></a>Güvenlik uyarılarını yönetme ve yanıtlama
Güvenlik Merkezi otomatik olarak toplar, çözümler ve tümleştirir günlük verilerini [Azure SQL tehdit algılama](../sql-database/sql-database-threat-detection.md), gerçek tehditleri algılamak ve hatalı pozitif sonuçları azaltmak için diğer Azure kaynaklarını yanı sıra. Öncelikli güvenlik uyarıları listesi, sorunu hızlıca araştırmanız gereken bilgiler ve saldırıyı düzeltme hakkındaki önerilerle birlikte Güvenlik Merkezi'nde gösterilir.

Uyarıları görmek için seçin **güvenlik uyarıları** döşeme Güvenlik Merkezi dikey penceresinde. Üzerinde **güvenlik uyarıları** dikey penceresinde, saldırıyı düzeltmek için atmanız gereken herhangi biri, adımlar varsa uyarı ve hangi tetiklenen olaylar hakkında daha fazla bilgi için bir uyarı seçin. Bu örnekte, şimdi seçin **olası SQL ekleme**.
![Güvenlik uyarıları][7]

Aşağıda gösterildiği gibi Güvenlik Merkezi ne hedef kaynak, uygulanabilirse uyarıyı tetikleyen içine Öngörüler sunar ek ayrıntılar sağlar kaynak IP adresi ve sorunun nasıl hakkında öneriler.
![Olası SQL ekleme][8]

Daha fazla bilgi için bkz: [yönetme ve güvenlik uyarılarını yanıtlama](security-center-managing-and-responding-alerts.md).

## <a name="next-steps"></a>Sonraki adımlar
* [Güvenlik Merkezi ile ilgili SSS](security-center-faq.md) — hizmeti kullanımı ile ilgili sık sorulan soruları bulabilirsiniz.
* [Güvenlik Merkezi planlama ve işlemler Kılavuzu](security-center-planning-and-operations-guide.md) - bir dizi adımları izleyin ve Güvenlik Merkezi kullanımınızı iyileştirmek için görevleri, kuruluşunuzun güvenlik gereksinimlerine ve bulut Yönetimi modeline bağlı.
* [Güvenlik Merkezi veri güvenliği](security-center-data-security.md) – Güvenlik Merkezi nasıl topladığı öğrenin ve yapılandırma bilgileri, meta verileri, olay günlükleri, kilitlenme döküm dosyaları ve daha da dahil olmak üzere Azure kaynaklarınızı hakkındaki verileri işler.
* [Güvenlik olayları işleme](security-center-incident.md) -güvenlik olaylarını işleme yardımcı olmak için Güvenlik Merkezi'nde Güvenlik Uyarısı özelliği kullanmayı öğrenin.

<!--Image references-->
[1]: ./media/security-center-sql-database/security-center.png
[2]: ./media/security-center-sql-database/security-center-blade.png
[3]: ./media/security-center-sql-database/security-policy.png
[4]: ./media/security-center-sql-database/recommendation.png
[5]: ./media/security-center-sql-database/turn-on-auditing.png
[6]: ./media/security-center-sql-database/monitor-health.png
[7]: ./media/security-center-sql-database/alert.png
[8]: ./media/security-center-sql-database/sql-injection.png
