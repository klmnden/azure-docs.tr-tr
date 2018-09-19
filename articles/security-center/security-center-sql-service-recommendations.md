---
title: Azure SQL hizmetini ve Azure Güvenlik Merkezi'nde veri koruma | Microsoft Docs
description: Bu belge adresleri yardımcı olacak öneriler Azure Güvenlik Merkezi'nde Azure SQL Hizmeti ve veri koruma ve güvenlik ilkelerine uygun haberdar olun.
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: ''
ms.assetid: bcae6987-05d0-4208-bca8-6a6ce7c9a1e3
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/03/2017
ms.author: terrylan
ms.openlocfilehash: 45f5dc840f015793912e314ab3d47e54a409708e
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2018
ms.locfileid: "46126675"
---
# <a name="protecting-azure-sql-service-and-data-in-azure-security-center"></a>Azure SQL hizmetini ve Azure Güvenlik Merkezi'nde veri koruma
Azure Güvenlik Merkezi, Azure kaynaklarınızın güvenlik durumunu analiz eder. Güvenlik Merkezi olası güvenlik açıklarını belirlediğinde, gerekli denetimlerin yapılandırılması işlemi boyunca size rehberlik öneriler oluşturur.  Öneriler, Azure kaynak türleri için geçerlidir: sanal makineleri (VM'ler), ağ, SQL, veri ve uygulama.

Bu makalede Azure SQL hizmet ve verilere uygulanır önerileri ele alır. Azure SQL sunucuları ve veritabanları, SQL veritabanları için şifreleme ve Azure depolama hesabınızın şifrelemesini etkinleştirmek için denetimi etkinleştirme etrafında önerileri Merkezi.  Aşağıdaki tabloda, hizmet ve veri kullanılabilir SQL önerileri ve uygulamanız durumunda her birinin yaptığı anlamanıza yardımcı olması için bir başvuru olarak kullanın.
### <a name="monitor-data-security"></a>Veri güvenliğini izleme

**Önleme** bölümünde **Veri güvenliği**’ne tıkladığınızda, **Veri Kaynakları** bölümünü SQL ve Depolama’ya yönelik önerilerle birlikte açılır. Ayrıca, veritabanının genel sağlık durumu için [öneriler](security-center-sql-service-recommendations.md) içerir. Depolama şifrelemesi hakkında daha fazla bilgi için [Azure Güvenlik Merkezi'ndeki Azure depolama hesabı için şifrelemeyi etkinleştirme](security-center-enable-encryption-for-storage-account.md) bölümünü okuyun.

![Veri Kaynakları](./media/security-center-monitoring/security-center-monitoring-fig13-newUI-2017.png)

**SQL Önerileri** altında herhangi bir öneriye tıklayabilir ve sorunu çözmek için yapılacak diğer eylemlerle ilgili daha fazla ayrıntı görebilirsiniz. Aşağıdaki örnekte, **SQL veritabanlarında Veritabanı Denetimi ve Tehdit algılama** önerisinin genişletilmiş hali gösterilmiştir.

![SQL önerisi hakkındaki ayrıntılar](./media/security-center-monitoring/security-center-monitoring-fig14-ga-new.png)

**SQL veritabanlarında Denetimi ve Tehdit algılamayı etkinleştirme** bölümünde aşağıdaki bilgiler bulunur:

* SQL veritabanlarının bir listesi
* Bulundukları sunucu
* Bu ayarın sunucudan devralınmış olduğuna veya bu veritabanında benzersiz olduğuna dair bilgiler
* Geçerli durum
* Sorunun önem derecesi

Bu öneriyle ilgilenmek için veritabanına tıkladığınızda, **Denetim ve Tehdit algılama** bölümü aşağıdaki ekran görüntüsünde gösterildiği gibi açılır.

![Denetim ve Tehdit algılama](./media/security-center-monitoring/security-center-monitoring-fig15-ga.png)

Denetimi etkinleştirmek için **Denetim** seçeneğinin altında **AÇIK**'ı seçin.

## <a name="available-sql-service-and-data-recommendations"></a>Kullanılabilir SQL Hizmeti ve veri önerileri
| Öneri | Açıklama |
| --- | --- |
| [SQL sunucularında denetim ve tehdit algılamayı etkinleştirme](security-center-enable-auditing-on-sql-servers.md) |Azure SQL sunucuları (SQL, sanal makineler üzerinde çalışan Azure SQL Hizmeti yalnızca; içermez) için Denetim ve tehdit algılamayı etkinleştirmek önerir. |
| [SQL veritabanlarında denetim ve tehdit algılamayı etkinleştirme](security-center-enable-auditing-on-sql-databases.md) |Azure SQL veritabanları (SQL, sanal makineler üzerinde çalışan Azure SQL Hizmeti yalnızca; içermez) için Denetim ve tehdit algılamayı etkinleştirmek önerir. |
| [SQL veritabanlarında saydam veri şifrelemesini etkinleştirme](security-center-enable-transparent-data-encryption.md) |SQL veritabanları (yalnızca Azure SQL Hizmeti) için şifreleme etkinleştirmenizi önerir. |

## <a name="see-also"></a>Ayrıca bkz.
Diğer Azure kaynak türü için geçerli öneriler hakkında daha fazla bilgi için aşağıdakilere bakın:

* [Azure Güvenlik Merkezi'nde sanal makinelerinizi koruma](security-center-virtual-machine-recommendations.md)
* [Azure Güvenlik Merkezi'nde uygulamalarınızı koruma](security-center-application-recommendations.md)
* [Azure Güvenlik Merkezi'nde ağınızı koruma](security-center-network-recommendations.md)

Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

* [Azure Güvenlik Merkezi'nde güvenlik ilkelerini ayarlama](security-center-policies.md) -- Azure abonelikleriniz ve kaynak gruplarınız için güvenlik ilkelerini yapılandırma hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve yanıtlama](security-center-managing-and-responding-alerts.md) -- Güvenlik uyarılarını yönetme ve yanıtlama hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md) -- Hizmeti kullanımı ile ilgili sık sorulan soruları bulabilirsiniz.
