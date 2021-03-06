---
title: Azure yönetimine genel bakış - Azure idare
description: Yönetimi için Azure uygulamalarınızın ve kaynaklarınızın Azure Yönetim Araçları içeriği için bağlantılarla birlikte alanlarından genel bakış.
author: DCtheGeek
manager: carmonm
ms.service: governance
ms.topic: article
ms.date: 12/06/2018
ms.author: dacoulte
ms.openlocfilehash: d68e7d9ec3b60b93f0050250d7e7d718db7aa5b8
ms.sourcegitcommit: ac1cfe497341429cf62eb934e87f3b5f3c79948e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/01/2019
ms.locfileid: "67488472"
---
# <a name="overview-of-management-services-in-azure"></a>Azure'daki Yönetim Hizmetleri genel bakış

Azure'da idare, Azure Yönetimi'nin bir parçasıdır. Bu makale, dağıtmak ve Azure kaynaklarınızı korumak için yönetim işleminin farklı alanları kapsar.

Yönetim, iş uygulamalarınızı ve onları destekleyen kaynaklarınızı korumak için gereken görevleri ve süreçleri ifade eder. Azure, birçok Hizmetleri ve tam yönetim sağlamak için birlikte çalışan araçlara sahiptir. Azure, aynı zamanda diğer bulutlarda ve şirket içi kaynaklar için yalnızca bu hizmetler değildir. Farklı araçları ve bunların birlikte nasıl çalıştığını anlamak, eksiksiz bir yönetim ortamı tasarlamanın ilk adımıdır.

Aşağıdaki diyagramda herhangi bir uygulamayı veya kaynağı korumak için gereken farklı yönetim alanları gösterilmektedir. Bu farklı alanlar, bir yaşam döngüsü düşünülebilir. Her alanı, bir kaynağın ömrü sürekli art arda gereklidir. Bu kaynak yaşam döngüsü, devam eden işlemi aracılığıyla ilk dağıtımı ile başlar ve son olarak kullanımdan olduğunda.

![Azure yönetimi, Bilim](../monitoring/media/management-overview/management-capabilities.png)

Herhangi bir Azure hizmeti, belirli yönetim alanı gereksinimlerini tamamen doldurur. Bunun yerine, her birlikte çalışan birkaç hizmet tarafından gerçekleştirilmiş. Application Insights gibi bazı hizmetleri, web uygulamaları için hedeflenmiş izleme işlevselliği sağlar. Diğer Azure izleme günlükleri gibi diğer hizmetler için yönetim verilerini depolar. Bu özellik, farklı hizmetler tarafından toplanan farklı türdeki verileri analiz etmek verir.

Aşağıdaki bölümlerde farklı yönetim alanları kısaca açıklanır ve ilgili temel Azure hizmetlerindeki ayrıntılı içeriklere yönelik bağlantılar sunulur.

## <a name="monitor"></a>İzleme

İzleme, toplama ve performans, sistem durumu ve kaynaklarınızın kullanılabilirliğini denetlemek için verileri analiz etme işlemidir. Etkili bir izleme stratejisi bileşenlerinin ve bildirimleri ile uygulamanızın çalışma süresini artırmak için işlemi anlamanıza yardımcı olur. İzleme genel bir bakış okuma sırasında kullanılan farklı hizmetlerin kapsar [izleme Azure uygulamalarını ve kaynaklarını](../monitoring/monitoring-overview.md).

## <a name="configure"></a>Yapılandırma

Yapılandırma ilk dağıtımı ile yapılandırmasına devam eden bakım ve kaynaklar için ifade eder.
Bu görevlerin Otomasyonu, zaman ve çabayı en aza indirmek ve doğruluk ve verimliliğinizi artırmak artıklığı ortadan kaldırmak sağlar. [Azure Otomasyonu](../automation/automation-intro.md), yapılandırma görevlerini otomatikleştirmek için toplu hizmetler sunar. Süreç otomasyonu runbook'ları işlemek karşın, yapılandırma ve güncelleştirme yönetimi yapılandırma yönetmeye yardımcı.

## <a name="govern"></a>İdare

İdare, Azure’daki uygulama ve kaynaklarınız üzerindeki denetimi sürdürmenize yönelik mekanizmalar ve süreçler sağlar. Girişimlerinizi planlama ve stratejik öncelikleri belirleme de bu kapsama dahildir.
Azure’da İdare, temelde iki hizmet ile uygulanır. [Azure İlkesi](./policy/overview.md) oluşturmanıza olanak tanır atayabilir ve kaynaklarınız için kuralları zorlamak için İlke tanımları yönetebilirsiniz. Bu özellik, bu kaynakları Kurumsal standartlarınız uyumlu tutar. [Cloudyn Azure maliyet Yönetimi](../cost-management/overview.md) , Azure kaynaklarınızın ve diğer bulut sağlayıcıları için bulut kullanımınızı ve harcamalarınızı izlemenize imkan tanır.

## <a name="secure"></a>Güvenlik

Kaynakları ve veri güvenliğini yönetin. Bir güvenlik programı, uygulamalarınızın ve kaynaklarınızın uyumluluk yanı sıra veri toplayıp analiz eden tehditleri değerlendirme içerir. Güvenlik İzleme ve tehdit analizi tarafından sağlanan [Azure Güvenlik Merkezi](../security-center/security-center-intro.md), içeren birleşik güvenlik yönetimi ve Gelişmiş tehdit koruması hibrit bulut iş yüklerinde. Bkz: [Azure güvenliğine giriş](../security/azure-security.md) kapsamlı bilgi ve Azure kaynaklarının güvenliğini sağlama konusunda yönergeler.

## <a name="protect"></a>Koruma

Koruma, uygulamalarınızın ve verilerinizin kullanılabilir, hatta denetiminiz dışında kesintiler tutulması ifade eder. Azure’da Koruma, iki hizmetle sağlanır. [Azure Backup](../backup/backup-introduction-to-azure-backup.md), hem buluttaki hem de şirket içindeki verilerinize yönelik yedekleme ve kurtarma sağlar. [Azure Site Recovery](../site-recovery/site-recovery-overview.md) bir olağanüstü durum sırasında iş sürekliliği ve anında kurtarma sağlar.

## <a name="migrate"></a>Geçiş

Geçiş, şirket içinde çalışan mevcut iş yüklerini Azure bulut ortamına geçirmeyi ifade eder.
[Azure geçişi](../migrate/migrate-overview.md) yardımcı olan bir hizmet, şirket içi sanal makinelerini azure'a geçiş uygunluğunu değerlendirmek olduğu. Azure Site Recovery, sanal makineleri geçirir [şirket içi](../site-recovery/migrate-tutorial-on-premises-azure.md) veya [Amazon Web Hizmetleri'nden](../site-recovery/migrate-tutorial-aws-azure.md). [Azure veritabanı geçişi](../dms/dms-overview.md) geçirme veritabanı kaynakları Azure Data platformlarına yardımcı olur.
