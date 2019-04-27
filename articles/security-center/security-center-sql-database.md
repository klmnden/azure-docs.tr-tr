---
title: Azure Güvenlik Merkezi ve Azure SQL veritabanı hizmeti | Microsoft Docs
description: Bu makalede, Güvenlik Merkezi, Azure SQL veritabanı'nda veritabanlarınızın güvenliğini sağlamanıza nasıl yardımcı olabileceğini gösterir.
services: sql-database
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: f109adfd-daed-4257-9692-2042a1399480
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/02/2017
ms.author: rkarlin
ms.openlocfilehash: 0a889de79b6a5921007614dac8d610c1be0222d2
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60704620"
---
# <a name="azure-security-center-and-azure-sql-database-service"></a>Azure Güvenlik Merkezi ve Azure SQL veritabanı hizmeti
[Azure Güvenlik Merkezi](https://azure.microsoft.com/documentation/services/security-center/), tehditleri önlemenize, algılamanıza ve yanıtlamanıza yardımcı olur. Aboneliklerinizde, tümleşik güvenlik izleme ve ilke yönetimi sağlar; normal koşullarda gözden kaçabilecek tehditleri algılamaya yardımcı olur ve güvenlik çözümlerinin geniş ekosistemiyle çalışır.

Bu makalede, Güvenlik Merkezi, Azure SQL veritabanı'nda veritabanlarınızın güvenliğini sağlamanıza nasıl yardımcı olabileceğini gösterir.

## <a name="why-use-security-center"></a>Güvenlik Merkezi neden kullanılır?
Güvenlik Merkezi, sunucularınıza ve veritabanlarınıza güvenliğini görünürlük sağlayarak SQL veritabanındaki verileri korumanıza yardımcı olur. Güvenlik Merkezi ile şunları yapabilirsiniz:

* SQL veritabanı şifreleme ve denetim için ilkeler tanımlarsınız.
* SQL veritabanı kaynaklarının tüm aboneliklerinizde izleyin.
* Hızlı bir şekilde belirleyin ve güvenlik sorunlarını düzeltmesine.
* Uyarılardan tümleştirme [Azure SQL veritabanı tehdit algılama](../sql-database/sql-database-threat-detection.md).

SQL veritabanı kaynaklarınızın korunmasına yardımcı olmanın yanı sıra, Güvenlik Merkezi ayrıca güvenlik izleme ve yönetim Azure sanal makineleri, bulut Hizmetleri, uygulama hizmetleri, sanal ağlar ve daha fazlasını sağlar. Güvenlik Merkezi hakkında daha fazla bilgi [burada](security-center-intro.md).

## <a name="prerequisites"></a>Önkoşullar
Güvenlik Merkezi ile çalışmaya başlamak için Microsoft Azure aboneliğinizin olması gerekir. Güvenlik Merkezi'nin ücretsiz katmanı, aboneliğiniz ile etkinleştirilir. Güvenlik Merkezi'nin ücretsiz ve standart katmanları hakkında daha fazla bilgi için bkz. [Güvenlik Merkezi fiyatlandırma](https://azure.microsoft.com/pricing/details/security-center/).

Güvenlik Merkezi, rol tabanlı erişimi destekler. Azure rol tabanlı erişim denetimi (RBAC) hakkında daha fazla bilgi için bkz: [Azure Active Directory rol tabanlı erişim denetimi](../role-based-access-control/role-assignments-portal.md). Güvenlik Merkezi ile ilgili SSS hakkında bilgi sağlar. [izinleri, Güvenlik Merkezi'nde nasıl işlenir](security-center-faq.md#permissions).

## <a name="access-security-center"></a>Güvenlik Merkezi'ne erişme
Güvenlik Merkezi'ne [Azure portalından](https://azure.microsoft.com/features/azure-portal/) erişebilirsiniz. [Portalda oturum açın](https://portal.azure.com/) seçip **Güvenlik Merkezi seçeneği**.

![Güvenlik Merkezi seçeneği][1]

**Güvenlik Merkezi** dikey penceresi açılır.
![Güvenlik Merkezi dikey penceresi][2]

## <a name="set-security-policy"></a>Güvenlik ilkesi ayarlama
Güvenlik İlkesi belirtilen abonelik veya kaynak grubundaki kaynaklar için önerilen denetim kümesini tanımlar. Güvenlik Merkezi'nde, abonelik veya kaynak grupları, şirketinizin güvenlik gereksinimlerine veya uygulamaların türüne ya da her Abonelikteki verilerin duyarlılığına göre ilkeleri tanımlarsınız.

SQL denetimi ve SQL saydam veri şifrelemesi (TDE) için öneriler göstermek için bir ilke ayarlayabilirsiniz.

* Ne zaman açmanız **SQL denetim ve tehdit algılamayı**, Güvenlik Merkezi için Azure veritabanı'na erişim denetiminin uyumluluk, Gelişmiş algılama ve araştırma amacıyla etkinleştirilmesini önerir.
* Ne zaman açmanız **SQL saydam veri şifrelemesi**, Güvenlik Merkezi, bekleyen şifrelemenin önerir, Azure SQL veritabanı, ilişkili yedeklemeler ve işlem günlük dosyaları için etkin olmalıdır.

Güvenlik İlkesi ayarlamak için seçin **ilke** Güvenlik Merkezi Dikey Döşe. Üzerinde **Güvenlik İlkesi** dikey penceresinde, güvenlik ilkesini etkinleştirmek istediğiniz aboneliği seçin. Seçin **önleme İlkesi** ve **üzerinde** bu abonelikte kullanmak istediğiniz güvenlik önerilerini.
![Güvenlik ilkesi][3]

Daha fazla bilgi için bkz. [güvenlik ilkelerini ayarlama](tutorial-security-policy.md).

## <a name="manage-security-recommendation"></a>Güvenlik önerisi yönetme
Güvenlik Merkezi düzenli aralıklarla Azure kaynaklarınızın güvenlik durumunu çözümler. Güvenlik Merkezi olası güvenlik açıklarını belirlediğinde öneriler oluşturur. Gerekli denetimlerin yapılandırılması işlemi boyunca öneriler size rehberlik eder.

Güvenlik İlkesi ayarladıktan sonra Güvenlik Merkezi olası güvenlik açıklarını tanımlamak amacıyla kaynaklarınızın güvenlik durumunu analiz eder. Öneriler her satırın belirli bir öneriyi temsil ettiği bir tablo biçiminde görüntülenir. Aşağıdaki tabloda kullanılabilir önerilerini Azure SQL veritabanı ve uygulamanız durumunda her öneri ne yaptığını anlamanıza yardımcı olması için bir başvuru olarak kullanın. Bir öneriyi seçtiğinizde, öneriyi Güvenlik Merkezi'nde uygulama açıklayan bir makale için açılır.

| Öneri | Açıklama |
| --- | --- |
| [SQL sunucularında denetim ve tehdit algılamayı etkinleştirme](security-center-enable-auditing-on-sql-servers.md) |SQL veritabanı sunucuları için Denetim ve tehdit algılamayı etkinleştirmek önerir. (Yalnızca SQL veritabanı hizmeti. Microsoft SQL Server'ın, sanal makineler üzerinde çalışan içermez.) |
| [SQL veritabanlarında denetim ve tehdit algılamayı etkinleştirme](security-center-enable-auditing-on-sql-databases.md) |SQL veritabanı veritabanları için Denetim ve tehdit algılamayı etkinleştirmek önerir. (Yalnızca SQL veritabanı hizmeti. Microsoft SQL Server'ın, sanal makineler üzerinde çalışan içermez.) |
| [Saydam Veri Şifrelemesini Etkinleştirme](security-center-enable-transparent-data-encryption.md) |SQL veritabanları için şifreleme etkinleştirmenizi önerir. (Yalnızca SQL veritabanı hizmeti.) |

Azure kaynaklarınız için önerileri görmek için seçin **önerileri** Güvenlik Merkezi Dikey Döşe. Üzerinde **önerileri** görmek için bir öneri ayrıntıları dikey penceresinde seçin. Bu örnekte, seçelim **SQL sunucularında denetimi etkinleştirme ve tehdit algılama**.

![Öneriler][4]

Aşağıda gösterildiği gibi Güvenlik Merkezi, burada denetim ve tehdit algılama etkin değil SQL sunucuları gösterir. Denetim açıldıktan sonra güvenlik uyarıları almak için tehdit algılama ayarları ve e-posta ayarlarını yapılandırabilirsiniz. Tehdit algılama, veritabanına olası güvenlik tehditlerine işaret eden anormal veritabanı etkinliklerini algıladığında sizi uyarır. Uyarılar, Güvenlik Merkezi panosunda görüntülenir.
![Denetim ve tehdit algılama][5]

Bağlantısındaki [Azure portalında SQL veritabanı tehdit algılama](../sql-database/sql-database-threat-detection-portal.md) açabilir ve tehdit algılamayı yapılandırma ve anormal etkinliklerinin algılanması üzerine güvenlik uyarıları alacak e-postaların listesini yapılandırın.

Öneriler hakkında daha fazla bilgi edinmek için [güvenlik önerilerini yönetme](security-center-recommendations.md).

## <a name="monitor-security-health"></a>Güvenlik durumunu izleme
Bir aboneliğin kaynakları için [güvenlik ilkelerini](tutorial-security-policy.md) etkinleştirmenizin ardından, Güvenlik Merkezi olası güvenlik açıklarını tanımlamak amacıyla kaynaklarınızın güvenliğini analiz eder.  İçinde kaynaklarınızın güvenlik durumunu görüntüleyebileceğiniz **kaynak güvenlik durumu** Döşe. Tıkladığınızda **veri** içinde **kaynak güvenlik durumu** kutucuğunda **veri kaynakları** dikey penceresi denetim ve saydam veri gibi sorunlar için SQL önerilerle birlikte açılır şifrelemesinin etkinleştirilmemiş olması. Ayrıca, veritabanının genel sağlık durumu için öneriler içerir.
![Kaynak güvenlik durumu][6]

Daha fazla bilgi için bkz. [güvenlik durumunu izleme](security-center-monitoring.md).

## <a name="manage-and-respond-to-security-alerts"></a>Güvenlik uyarılarını yönetme ve yanıtlama
Güvenlik Merkezi otomatik olarak toplar, çözümler ve tümleştirir günlük verilerini [Azure SQL tehdit algılama](../sql-database/sql-database-threat-detection.md), gerçek tehditleri algılamak ve hatalı pozitif sonuçları azaltmak için diğer Azure kaynaklarını yanı sıra. Öncelikli güvenlik uyarıları listesi, sorunu hızlıca araştırmanız gereken bilgiler ve saldırıyı düzeltme hakkındaki önerilerle birlikte Güvenlik Merkezi'nde gösterilir.

Uyarıları görmek için seçin **güvenlik uyarıları** Güvenlik Merkezi Dikey Döşe. Üzerinde **güvenlik uyarıları** dikey penceresinde bir uyarı, adımları, uyarı ve ne, tetiklenen olayları hakkında daha fazla bilgi için gereken bir saldırıyı düzeltmek için adımlarla seçin. Bu örnekte, seçelim **olası SQL ekleme**.
![Güvenlik uyarıları][7]

Aşağıda gösterildiği gibi Güvenlik Merkezi, hedef kaynak, uygulanabilirse uyarıyı neyin tetiklediği Öngörüler sunan ek ayrıntılar sağlar. kaynak IP adresi ve sorunun nasıl düzeltileceği hakkında öneriler.
![Olası SQL eklemesi][8]

Daha fazla bilgi için bkz. [yönetme ve güvenlik uyarılarını yanıtlama](security-center-managing-and-responding-alerts.md).

## <a name="next-steps"></a>Sonraki adımlar
* [Güvenlik Merkezi SSS](security-center-faq.md) — hizmet kullanımı ile ilgili sık sorulan soruları bulabilirsiniz.
* [Güvenlik Merkezi planlama ve işlemler Kılavuzu](security-center-planning-and-operations-guide.md) - adımları izleyin ve kuruluşunuzun güvenlik gereksinimlerine ve bulut Yönetimi modeline tabanlı Güvenlik Merkezi kullanımınızı iyileştirmek için görevler.
* [Güvenlik Merkezi veri güvenliği](security-center-data-security.md) : Güvenlik Merkezi'nin nasıl topladığı öğrenin ve yapılandırma bilgileri, meta verileri, olay günlükleri, kilitlenme dökümü dosyaları ve daha da dahil olmak üzere Azure kaynaklarınızı ilgili verileri işler.
* [Güvenlik olaylarını işlemenize](security-center-incident.md) -güvenlik uyarısı özelliğinin Güvenlik Merkezi'nde güvenlik olaylarını işleme bölümünde yardımcı olmak için nasıl kullanılacağını öğrenin.

<!--Image references-->
[1]: ./media/security-center-sql-database/security-center.png
[2]: ./media/security-center-sql-database/security-center-blade.png
[3]: ./media/security-center-sql-database/security-policy.png
[4]: ./media/security-center-sql-database/recommendation.png
[5]: ./media/security-center-sql-database/turn-on-auditing.png
[6]: ./media/security-center-sql-database/monitor-health.png
[7]: ./media/security-center-sql-database/alert.png
[8]: ./media/security-center-sql-database/sql-injection.png
