---
title: Azure yönetimine genel bakış
description: Yönetimi için Azure uygulamalarınızın ve kaynaklarınızın Azure Yönetim Araçları içeriği için bağlantılarla birlikte alanlarından genel bakış.
author: DCtheGeek
manager: carmonm
ms.service: governance
ms.topic: article
ms.date: 09/18/2018
ms.author: dacoulte
ms.openlocfilehash: 180b0cb9f52858d9b0f079ea711fd5ccab738ecf
ms.sourcegitcommit: 56d20d444e814800407a955d318a58917e87fe94
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/29/2018
ms.locfileid: "52582325"
---
# <a name="management-in-azure"></a>Azure’da Yönetim

Azure'da idare, Azure Yönetimi'nin bir parçasıdır. Bu makale, kullanmaya başlamanıza yardımcı olabilecek belgeleri içermesinin yanı sıra, Azure’daki uygulamalarınızı ve kaynaklarınızı dağıtıp korumanız için gereken farklı yönetim alanlarını kısaca tanımlar.

Yönetim, iş uygulamalarınızı ve onları destekleyen kaynaklarınızı korumak için gereken görevleri ve süreçleri ifade eder. Azure’da sunulan çeşitli hizmetler ve araçlar, birlikte çalışarak yalnızca Azure’da değil diğer bulutlarda ve şirket içinde çalışan uygulamalarınızı da eksiksiz biçimde yönetmenizi sağlar. Size sunulan farklı araçları ve bu araçların çeşitli yönetim senaryolarında nasıl kullanılabileceğini anlamak, eksiksiz bir yönetim ortamı tasarlamanın ilk adımıdır.

Aşağıdaki diyagramda herhangi bir uygulamayı veya kaynağı korumak için gereken farklı yönetim alanları gösterilmektedir. Bu farklı alanlar, bir kaynağın ömrü boyunca sürekli olarak ve art arda kullanılacakları bir yaşam döngüsü biçiminde düşünülebilir. Bu döngü ilk dağıtımla birlikte başlar, sürekli işlemlerle devam eder ve kullanımdan kaldırıldığında son bulur.

![Yönetim özellikleri](../monitoring/media/management-overview/management-capabilities.png)

Herhangi bir Azure hizmeti, belirli bir yönetim alanının tüm gereksinimlerini tamamen karşılamaz, ancak birlikte çalışan birden çok hizmetlerle bu gereksinimlerin her biri gerçekleştirilebilir. Bazı uygulamalar hedefli işlevsellik sağlar; örneğin Application Insights ile web uygulamalarını izleyebilirsiniz. Diğer hizmetlere yönelik yönetim verilerini depolayarak farklı hizmetler tarafından toplanan farklı türdeki verileri analiz etmenize olanak sağlayan Log Analytics gibi hizmetler ise genel işlevler sunar.

Aşağıdaki bölümlerde farklı yönetim alanları kısaca açıklanır ve ilgili temel Azure hizmetlerindeki ayrıntılı içeriklere yönelik bağlantılar sunulur.

## <a name="monitor"></a>İzleme

İzleme, iş uygulamalarınızın ve bağımlı oldukları kaynakların performansını, sistem durumunu ve kullanılabilirliğini belirlemeye yönelik verileri toplayıp analiz etme eylemidir. Etkili bir izleme stratejisi, uygulamanızdaki farklı bileşenlerin ayrıntılı işlemlerini anlamanıza yardımcı olur ve bazı önemli zorlukları sorun haline gelmeden önce çözmenizi sağlamak amacıyla bunları proaktif bir biçimde size bildirerek çalışma sürenizi artırabilir. İzleme stratejisinde kullanılan farklı hizmetlerin tanımlandığı Azure’da İzlemeye genel bakışı [Azure uygulamalarını ve kaynaklarını izleme](../monitoring/monitoring-overview.md) sayfasından okuyabilirsiniz.

## <a name="configure"></a>Yapılandırma

Yapılandırma, uygulama ve kaynakların ilk dağıtımı ile yapılandırmasına ek olarak, yama ve güncelleştirmeler ile sürdürülen bakımlarını ifade eder. Bu görevlerin betikler ve ilkeler aracılığıyla otomasyonu sayesinde artıklığı ortadan kaldırabilir, doğruluk ve verimliliğinizi artırmak için harcadığınız zamanı ve çabayı en aza indirebilirsiniz. [Azure Otomasyonu](..\automation\automation-intro.md), yapılandırma görevlerini otomatikleştirmek için toplu hizmetler sunar. Süreçleri otomatikleştirmeye yönelik runbook’lara ek olarak, ilkeler aracılığıyla yapılandırmayı yönetmenize ve güncelleştirmeleri tanımlayıp dağıtmanıza yardımcı olan yapılandırma ve güncelleştirme yönetimi sağlar.

## <a name="govern"></a>İdare

İdare, Azure’daki uygulama ve kaynaklarınız üzerindeki denetimi sürdürmenize yönelik mekanizmalar ve süreçler sağlar. Girişimlerinizi planlama ve stratejik öncelikleri belirleme de bu kapsama dahildir.
Azure’da İdare, temelde iki hizmet ile uygulanır. [Azure İlkesi](../azure-policy/azure-policy-introduction.md), kaynaklarınız üzerinden farklı kuralları ve eylemleri uygulatarak kaynaklarınızın kurumsal standartlarınız ve hizmet düzeyi sözleşmelerinizle uyumlu kalmasından emin olmak amacıyla ilke tanımlarını oluşturmanıza, atamanıza ve yönetmenize olanak sağlar. [Cloudyn Tarafından Sağlanan Azure Maliyet Yönetimi](../cost-management/overview.md), Azure kaynaklarınızın yanı sıra AWS ve Google dahil diğer bulut sağlayıcıları için bulut kullanımınızı ve harcamalarınızı izlemenize imkan tanır.

## <a name="secure"></a>Güvenlik

Uygulamalarınızın, kaynakları ve veri güvenliğini yönetme, toplama ve güvenlik verilerini analiz etme ve uygulamalarınızın ve kaynaklarınızın tasarlanmış ve güvenli bir şekilde tasarlanıp yapılandırılmasını sağlayarak kapsamında, değerlendirilen tehditlerin, bir birleşimini içerir. Güvenlik İzleme ve tehdit Analizine hibrit bulut iş yüklerinde Birleşik güvenlik yönetimi ve Gelişmiş tehdit koruması içeren Azure Güvenlik Merkezi tarafından sağlanır. Azure’da güvenlik ve Azure kaynaklarını güvenli bir şekilde yapılandırma hakkında kapsamlı bilgi için [Azure Güvenlik’e Giriş](../security/azure-security.md) sayfasına da başvurabilirsiniz.

## <a name="protect"></a>Koruma

Koruma, kontrolünüzün dışındaki kesintilerde bile uygulamalarınızın ve verilerinizin her zaman kullanılabilir kalmasını sağlamayı ifade eder. Azure’da Koruma, iki hizmetle sağlanır. [Azure yedekleme](../backup/backup-introduction-to-azure-backup.md)yedekleme ve kurtarma, verilerinizin bulutta veya şirket içi ya da sağlar. [Azure Site Recovery](../site-recovery/site-recovery-overview.md), iş sürekliliği ve olağanüstü durum oluşması halinde anında kurtarma olanağı sunarak uygulamanızın yüksek oranda kullanılabilir olmasını sağlar.

## <a name="migrate"></a>Geçiş

Geçiş, şirket içinde çalışan mevcut iş yüklerini Azure bulut ortamına geçirmeyi ifade eder.
[Azure Geçişi](../migrate/migrate-overview.md), performansı temel alan boyutlandırma ve maliyet tahminleri de dahil olmak üzere şirket içi sanal makinelerinizin Azure’a geçiş uygunluğunu değerlendirmenize yardımcı olur. Azure Site Recovery, sanal makinelerin [şirket içinden](../site-recovery/migrate-tutorial-on-premises-azure.md) veya [Amazon Web Services’tan](../site-recovery/migrate-tutorial-aws-azure.md) fiili geçişini gerçekleştirmenize yardımcı olur. [Azure Veritabanı Geçişi](../dms/dms-overview.md), birden çok veritabanı kaynağını Azure Data platformlarına geçirmenizde yardımcı olur.
