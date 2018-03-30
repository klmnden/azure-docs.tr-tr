---
title: Azure Yönetimi - izleme | Microsoft Docs
description: Azure, birden çok Hizmetleri ve yalnızca Azure aynı zamanda diğer Bulut ve şirket içi çalışan uygulamalarınız için tam yönetim sunmak için birlikte çalışan araçlara sahiptir.  Bu makale, bulut uygulamaları ve kaynakları yönetmek için Azure Araçları yönetimi ve içeriklere farklı alanları üst düzey bir açıklamasını sağlar.
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.service: monitoring
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/27/2018
ms.author: bwren
ms.openlocfilehash: b20283e1189e4f1a3555e2dd8d25972c9a677cd6
ms.sourcegitcommit: c3d53d8901622f93efcd13a31863161019325216
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2018
---
# <a name="azure-management---monitoring"></a>Azure Yönetimi - izleme

Azure'da izleme bir Azure Management yönüdür.  Bu makalede, dağıtmak ve uygulamaları ve Azure kaynaklarında belgelere bağlantıları ile başlamanıza yardımcı olmak için korumak için gereken yönetim işleminin farklı alanları kısaca açıklanmaktadır.

## <a name="management-in-azure"></a>Azure Yönetimi

Yönetim görevleri ve iş uygulamalarınız ve bunları destekleyen kaynaklar sürdürmek için gerekli işlemleri gösterir.  Azure, birden çok Hizmetleri ve yalnızca Azure aynı zamanda diğer Bulut ve şirket içi çalışan uygulamalarınız için tam yönetim sunmak için birlikte çalışan araçlara sahiptir.  Farklı araçlar ve nasıl birlikte çeşitli yönetim senaryoları için kullanılabilmesi için anlama tam yönetim ortamı tasarlamanın ilk adımdır.

Aşağıdaki diyagramda, herhangi bir uygulama veya kaynak sürdürmek için gerekli yönetim işleminin farklı alanları gösterilmektedir.  Farklı alanlara, yaşam döngüsü açısından her bir kaynak kullanım ömrü sürekli art arda burada gereklidir değerlendirilebilir.  Bu, devam eden işlemi aracılığıyla, ilk dağıtım ile başlar ve son olarak ne zaman onu Çekildi.

![Yönetim Özellikleri](media/management-overview/management-capabilities.png)


Tek bir Azure hizmeti tamamen belirli yönetim alanı gereksinimlerini doldurur, ancak bunun yerine her gerçekleşen birlikte çalışan birden fazla hizmet tarafından.  Bazı hizmetler sağlayan izleme web uygulamaları için Application Insights gibi hedeflenen işlevsellik sağlar.  Diğerleri gibi farklı türlerde farklı services tarafından toplanan verileri çözümlemek sağlayarak diğer hizmetler için yönetim verilerini depolayan günlük analizi ortak işlevleri sağlar.  

Aşağıdaki bölümlerde kısaca farklı yönetim alanları tanımlamak ve bunları ele almak ana Azure hizmetleri hakkında ayrıntılı içeriklere yönelik sağlayın.

## <a name="monitor"></a>İzleme
İzleme, toplama ve performans, sistem durumu ve iş uygulamanız ve bağımlı kaynakları kullanılabilirliğini belirlemek için verileri analiz etme işlemidir. Etkili izleme stratejisi, uygulamanızın ve böylece sorun haline gelmeden önce bunları çözümleyebilirsiniz proaktif olarak, kritik sorunlar bildirerek, çalışma süresini artırmak için farklı bileşenlerin ayrıntılı çalışmasını anlamanıza yardımcı olur.  İzleme izleme stratejisi için kullanılan farklı hizmetleri tanımlar Azure genel bir bakış okuyabilirsiniz [izleme Azure uygulamaları ve kaynakları](monitoring-overview.md).


## <a name="configure"></a>Yapılandırma
Yapılandırma ilk dağıtım ve uygulamaları ve kaynakları ve bunların düzeltme eklerinin ve Güncelleştirmeler ile devam eden bakım yapılandırmasını ifade eder.  Komut dosyası ve ilke aracılığıyla bu görevlerin otomasyonunu zaman ve çaba en aza indirerek ve doğruluk ve verimliliği artırma artıklık ortadan kaldırmak sağlar.  [Azure Otomasyonu](..\automation\automation-intro.md) yapılandırma görevlerini otomatikleştirmek için Hizmetleri toplu sağlar.  İşlemleri otomatikleştirmek için runbook'lar yanı sıra, yapılandırma İlkesi aracılığıyla ve tanımlama ve güncelleştirmeleri dağıtmak yönetmeye yardımcı yapılandırmasını ve güncelleştirme yönetimi sağlar.

## <a name="govern"></a>Yöneten
İdare mekanizmalar ve işlemler, uygulamalarınızı ve Azure kaynaklarında üzerinde denetim kurmanızı sağlar.  Girişimleri planlama ve stratejik önceliklerini ayarlama içerir.  Azure'da idare temelde iki Hizmetleri ile uygulanır.  [Azure ilke](../azure-policy/azure-policy-introduction.md) oluşturmanıza olanak tanıyan atayabilir ve, bu kaynakları Kurumsal standartlarla uyumlu kalmak ve hizmet düzeyi sözleşmelerine böylece farklı kurallar ve Eylemler kaynaklarınızı zorla ilke tanımları yönetebilirsiniz. [Azure maliyeti Yönetimi Cloudyn tarafından](../cost-management/overview.md) bulut kullanımı ve Azure kaynaklarınızı ve AWS ve Google gibi diğer bulut sağlayıcıları harcamalarını izlemenize olanak tanır.

## <a name="secure"></a>Güvenlik
Uygulamalar, kaynakları ve verilerinizin güvenliğini yönetme değerlendirme tehditler, toplama ve güvenlik verilerini çözümleme ve uygulamaları ve kaynakları tasarlanmış ve güvenli bir şekilde yapılandırılmış bir birleşimini içerir.  Tarafından sağlanan güvenlik izleme ve tehdit analizi [Azure Güvenlik Merkezi](../security-center/security-center-intro.md) içeren güvenlik yönetimi ve Gelişmiş tehdit koruması birleşik karma bulut iş yüklerinde.  Ayrıca görmelisiniz [Azure güvenlik giriş](../security/azure-security.md) Azure ve güvenli bir şekilde Azure kaynaklarını yapılandırma konusunda yönergeler güvenliği hakkında kapsamlı bilgi için.


## <a name="protect"></a>Koruma
Koruma uygulamaları ve verileri her zaman kullanılabilir denetiminizi ötesinde kesintiler durumunda bile emin olma konusunda başvuruyor.  Azure koruması iki Hizmetleri tarafından sağlanır.  [Azure yedekleme](../backup/backup-introduction-to-azure-backup.md) yedekleme ve kurtarma, Bulut veya şirket içi verileriniz sağlar.    [Azure Site Recovery](../site-recovery/site-recovery-overview.md) iş sürekliliği ve olağanüstü durum oluşması durumunda hemen kurtarma sağlayarak uygulamanızın yüksek kullanılabilirlik sağlar.

## <a name="migrate"></a>Geçiş 
Şirket içi Azure bulut şu anda çalışan iş yüklerini geçiş için geçiş başvuruyor.  [Azure geçirme](../migrate/migrate-overview.md) performans tabanlı boyutlandırma dahil olmak üzere geçiş uygunluğu değerlendirmek ve Azure sanal makinelerine tahminleri, şirket içi maliyet yardımcı olan bir hizmettir.  Azure Site Recovery, sanal makinelerin gerçek geçiş gerçekleştirmenize yardımcı olabilir [şirket içi](../site-recovery/migrate-tutorial-on-premises-azure.md) veya [Amazon Web hizmetlerinden](../site-recovery/migrate-tutorial-aws-azure.md).  [Azure veritabanı geçiş](../dms/dms-overview.md) birden çok veritabanı kaynağı Azure veri platformlara geçiş de yardımcı olur.