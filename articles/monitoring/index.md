---
title: Azure Yönetimi ve Operations Management Suite'e (OMS) genel bakış | Microsoft Docs
description: Daha önce Operations Management Suite (OMS) ile birlikte sunulan Azure yönetim araçlarıyla ilgili içeriğe yönlendiren bağlantılar da içeren Azure uygulamalarını ve kaynaklarını yönetme alanlarına genel bakış.
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.service: monitoring
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/09/2018
ms.author: bwren
ms.openlocfilehash: 8598e3528aa0a9fb171853e5f6554346ace937ef
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37059959"
---
# <a name="azure-management---monitoring"></a>Azure Yönetimi - İzleme

Azure’da izleme, Azure Yönetimi’nin bir parçasıdır.  Bu makale, kullanmaya başlamanıza yardımcı olabilecek belgeleri içermesinin yanı sıra, Azure’daki uygulamalarınızı ve kaynaklarınızı dağıtıp korumanız için gereken farklı yönetim alanlarını kısaca tanımlar.

## <a name="management-in-azure"></a>Azure’da Yönetim

Yönetim, iş uygulamalarınızı ve onları destekleyen kaynaklarınızı korumak için gereken görevleri ve süreçleri ifade eder.  Azure’da sunulan çeşitli hizmetler ve araçlar, birlikte çalışarak yalnızca Azure’da değil diğer bulutlarda ve şirket içinde çalışan uygulamalarınızı da eksiksiz biçimde yönetmenizi sağlar.  Size sunulan farklı araçları ve bu araçların çeşitli yönetim senaryolarında nasıl kullanılabileceğini anlamak, eksiksiz bir yönetim ortamı tasarlamanın ilk adımıdır.

Aşağıdaki diyagramda herhangi bir uygulamayı veya kaynağı korumak için gereken farklı yönetim alanları gösterilmektedir.  Bu farklı alanlar, bir kaynağın ömrü boyunca sürekli olarak ve art arda kullanılacakları bir yaşam döngüsü biçiminde düşünülebilir.  Bu döngü ilk dağıtımla birlikte başlar, sürekli işlemlerle devam eder ve kullanımdan kaldırıldığında son bulur.

![Yönetim özellikleri](media/management-overview/management-capabilities.png)


Herhangi bir Azure hizmeti, belirli bir yönetim alanının tüm gereksinimlerini tamamen karşılamaz, ancak birlikte çalışan birden çok hizmetlerle bu gereksinimlerin her biri gerçekleştirilebilir.  Bazı uygulamalar hedefli işlevsellik sağlar; örneğin Application Insights ile web uygulamalarını izleyebilirsiniz.  Diğer hizmetlere yönelik yönetim verilerini depolayarak farklı hizmetler tarafından toplanan farklı türdeki verileri analiz etmenize olanak sağlayan Log Analytics gibi hizmetler ise genel işlevler sunar.  

Aşağıdaki bölümlerde farklı yönetim alanları kısaca açıklanır ve ilgili temel Azure hizmetlerindeki ayrıntılı içeriklere yönelik bağlantılar sunulur.

## <a name="monitor"></a>İzleme
İzleme, iş uygulamalarınızın ve bağımlı oldukları kaynakların performansını, sistem durumunu ve kullanılabilirliğini belirlemeye yönelik verileri toplayıp analiz etme eylemidir. Etkili bir izleme stratejisi, uygulamanızdaki farklı bileşenlerin ayrıntılı işlemlerini anlamanıza yardımcı olur ve bazı önemli zorlukları sorun haline gelmeden önce çözmenizi sağlamak amacıyla bunları proaktif bir biçimde size bildirerek çalışma sürenizi artırabilir.  İzleme stratejisinde kullanılan farklı hizmetlerin tanımlandığı Azure’da İzlemeye genel bakışı [Azure uygulamalarını ve kaynaklarını izleme](monitoring-overview.md) sayfasından okuyabilirsiniz.


## <a name="configure"></a>Yapılandırma
Yapılandırma, uygulama ve kaynakların ilk dağıtımı ile yapılandırmasına ek olarak, yama ve güncelleştirmeler ile sürdürülen bakımlarını ifade eder.  Bu görevlerin betikler ve ilkeler aracılığıyla otomasyonu sayesinde artıklığı ortadan kaldırabilir, doğruluk ve verimliliğinizi artırmak için harcadığınız zamanı ve çabayı en aza indirebilirsiniz.  [Azure Otomasyonu](..\automation\automation-intro.md), yapılandırma görevlerini otomatikleştirmek için toplu hizmetler sunar.  Süreçleri otomatikleştirmeye yönelik runbook’lara ek olarak, ilkeler aracılığıyla yapılandırmayı yönetmenize ve güncelleştirmeleri tanımlayıp dağıtmanıza yardımcı olan yapılandırma ve güncelleştirme yönetimi sağlar.

## <a name="govern"></a>İdare
İdare, Azure’daki uygulama ve kaynaklarınız üzerindeki denetimi sürdürmenize yönelik mekanizmalar ve süreçler sağlar.  Girişimlerinizi planlama ve stratejik öncelikleri belirleme de bu kapsama dahildir.  Azure’da İdare, temelde iki hizmet ile uygulanır.  [Azure İlkesi](../azure-policy/azure-policy-introduction.md), kaynaklarınız üzerinden farklı kuralları ve eylemleri uygulatarak kaynaklarınızın kurumsal standartlarınız ve hizmet düzeyi sözleşmelerinizle uyumlu kalmasından emin olmak amacıyla ilke tanımlarını oluşturmanıza, atamanıza ve yönetmenize olanak sağlar. [Cloudyn Tarafından Sağlanan Azure Maliyet Yönetimi](../cost-management/overview.md), Azure kaynaklarınızın yanı sıra AWS ve Google dahil diğer bulut sağlayıcıları için bulut kullanımınızı ve harcamalarınızı izlemenize imkan tanır.

## <a name="secure"></a>Güvenlik
Uygulamalarınızın, kaynaklarınızın ve verilerinizin yönetimi kapsamında, değerlendirilen tehditlerin, toplanıp çözümlenen güvenlik verilerinin ve uygulama ile kaynaklarınızın güvenli bir şekilde tasarlanıp yapılandırılmasını sağlamanın bir birleşimi bulunur.  [Azure Güvenlik Merkezi](../security-center/security-center-intro.md)’nin sağladığı güvenlik izleme ve tehdit analizine hibrit bulut iş yükleri arasında birleşik güvenlik yönetimi ve gelişmiş tehdit koruması dahildir.  Azure’da güvenlik ve Azure kaynaklarını güvenli bir şekilde yapılandırma hakkında kapsamlı bilgi için [Azure Güvenlik’e Giriş](../security/azure-security.md) sayfasına da başvurabilirsiniz.


## <a name="protect"></a>Koruma
Koruma, kontrolünüzün dışındaki kesintilerde bile uygulamalarınızın ve verilerinizin her zaman kullanılabilir kalmasını sağlamayı ifade eder.  Azure’da Koruma, iki hizmetle sağlanır.  [Azure Backup](../backup/backup-introduction-to-azure-backup.md), hem buluttaki hem de şirket içindeki verilerinize yönelik yedekleme ve kurtarma sağlar.    [Azure Site Recovery](../site-recovery/site-recovery-overview.md), iş sürekliliği ve olağanüstü durum oluşması halinde anında kurtarma olanağı sunarak uygulamanızın yüksek oranda kullanılabilir olmasını sağlar.

## <a name="migrate"></a>Geçiş 
Geçiş, şirket içinde çalışan mevcut iş yüklerini Azure bulut ortamına geçirmeyi ifade eder.  [Azure Geçişi](../migrate/migrate-overview.md), performansı temel alan boyutlandırma ve maliyet tahminleri de dahil olmak üzere şirket içi sanal makinelerinizin Azure’a geçiş uygunluğunu değerlendirmenize yardımcı olur.  Azure Site Recovery, sanal makinelerin [şirket içinden](../site-recovery/migrate-tutorial-on-premises-azure.md) veya [Amazon Web Services’tan](../site-recovery/migrate-tutorial-aws-azure.md) fiili geçişini gerçekleştirmenize yardımcı olur.  [Azure Veritabanı Geçişi](../dms/dms-overview.md), birden çok veritabanı kaynağını Azure Data platformlarına geçirmenizde yardımcı olur.


## <a name="operations-management-suite"></a>Operations Management Suite
Azure yönetimine ilişkin önceki teknik belgeler arasında, aşağıdaki Azure yönetim hizmetlerinin bir arada toplandığı bir paket olan Operations Management Suite (OMS) de bulunuyordu:

- Azure Otomasyonu
- Azure Backup
- Log Analytics
- Site Recovery

Azure’da eksiksiz yönetim diğer hizmetleri de kapsayacak şekilde genişletildiğinden, artık teknik belgelerimizde bu paketi açıklamayacağız. OMS’ye dahil olan hizmetlerde herhangi bir değişiklik yapılmamıştır ve bunlar, Azure uygulamalarınızın ve kaynaklarınızın yönetiminde kritik bir rol oynamaya devam eder. Gerçekleştirmeniz gereken yönetim görevlerine ve her bir görev için birlikte çalışan farklı Azure hizmetlerine odaklanmalısınız.