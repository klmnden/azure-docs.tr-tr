---
title: "Azure Danışmanı yüksek kullanılabilirlik önerileri | Microsoft Docs"
description: "Azure dağıtımlarınızı yüksek kullanılabilirliğini artırmak için Azure Danışmanı'nı kullanın."
services: advisor
documentationcenter: NA
author: KumudD
manager: carmonm
editor: 
ms.assetid: 
ms.service: advisor
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/16/2016
ms.author: kumud
ms.openlocfilehash: 7b63de2453180e562596c211d40cebe85b95bd54
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="advisor-high-availability-recommendations"></a>Advisor yüksek kullanılabilirlik önerileri

Azure Danışmanı emin olun ve iş açısından kritik uygulamalarınızı sürekliliği artırmanıza yardımcı olur. Danışmanına göre yüksek kullanılabilirlik öneriler alabilirsiniz **yüksek kullanılabilirlik** Danışmanı Pano sekmesi.

![Advisor Panoda yüksek kullanılabilirlik düğmesi](./media/advisor-high-availability-recommendations/advisor-high-availability-tab.png)


## <a name="ensure-virtual-machine-fault-tolerance"></a>Sanal makine hataya dayanıklılık sağlamak

Advisor bir kullanılabilirlik kümesi ve bir kullanılabilirlik kümesine taşıma önerir parçası olmayan sanal makineleri tanımlar. Uygulamanıza yedeklilik sağlamak için bir kullanılabilirlik kümesinde iki veya daha fazla sanal makinenin gruplandırılması önerilir. Bu yapılandırma ya da bir planlı veya plansız bir bakım olayı sırasında en az bir sanal makine kullanılabilir ve Azure sanal makinesi SLA karşılayan sağlar. Kullanılabilirlik kümesi için sanal makine oluşturmak için ya da sanal makineyi var olan bir kullanılabilirlik kümesine eklemeyi seçebilirsiniz.

> [!NOTE]
> Bir kullanılabilirlik kümesi oluşturmayı seçerseniz, en az bir daha fazla sanal makine içine eklemeniz gerekir. En az bir makine olduğundan emin olmak için iki veya daha fazla sanal makine bir kullanılabilirlik kümesi bu, Grup kesinti sırasında kullanılabilir öneririz.

![Advisor öneri: sanal makine artıklık için kullanılabilirlik kümelerini kullanın](./media/advisor-high-availability-recommendations/advisor-high-availability-create-availability-set.png)

## <a name="ensure-availability-set-fault-tolerance"></a>Hataya dayanıklılık kullanılabilirlik kümesi emin olun 

Advisor tek bir sanal makine içeren kullanılabilirlik kümeleri tanımlar ve bir veya daha fazla sanal makine eklemeyi önerir. Uygulamanıza yedeklilik sağlamak için bir kullanılabilirlik kümesinde iki veya daha fazla sanal makinenin gruplandırılması önerilir. Bu yapılandırma ya da bir planlı veya plansız bir bakım olayı sırasında en az bir sanal makine kullanılabilir ve Azure sanal makinesi SLA karşılayan sağlar. Ya da bir sanal makine oluşturmak için veya mevcut bir sanal makine kullanmayı ve kullanılabilirlik kümesine eklemek için seçebilirsiniz.  

![Advisor öneri: bir veya daha fazla sanal makine bu kullanılabilirlik kümesine ekleme](./media/advisor-high-availability-recommendations/advisor-high-availability-add-vm-to-availability-set.png)


## <a name="ensure-application-gateway-fault-tolerance"></a>Uygulama ağ geçidi hataya dayanıklılık sağlamak
Uygulama ağ geçidi tarafından desteklenen kritik uygulamaların iş sürekliliğini sağlamak için hataya dayanıklılık için yapılandırılmamış uygulama ağ geçidi örnekleri Danışmanı tanımlar ve yapabileceğiniz düzeltme eylemleri önerir. Orta veya büyük tek örnekli uygulama ağ geçitleri Danışmanı tanımlar ve en az bir daha fazla örneğini eklemede önerir. Ayrıca, tek veya birden çok instance küçük uygulama ağ geçitleri tanımlar ve orta veya büyük SKU'ları için geçiş önerir. Advisor, uygulama ağ geçidi örnekleri bu kaynakların geçerli SLA gereksinimlerini karşılamak için yapılandırıldığından emin olmak için bu eylemleri önerir.

![Advisor öneri: iki veya daha fazla Orta veya büyük boyutlu uygulama ağ geçidi örnekleri dağıtma](./media/advisor-high-availability-recommendations/advisor-high-availability-application-gateway.png)

## <a name="improve-the-performance-and-reliability-of-virtual-machine-disks"></a>Sanal makine disklerini güvenilirliğini ve performansını geliştirmek

Advisor standart diskler ile sanal makineleri tanımlar ve premium disklere yükseltme önerir.
 
Azure Premium depolama g/Ç kullanımı yoğun iş yükleri çalıştıran sanal makineler için yüksek performanslı, düşük gecikmeli disk desteği sunar. Premium depolama hesapları kullanan sanal makine diskler katı hal sürücüleri (SSD) verileri depolar. Uygulamanız için en iyi performans için premium depolama alanına yüksek IOPS gerektiren herhangi bir sanal makine disklerini geçirmek öneririz. 

Disklerinizi yüksek IOPS ihtiyacınız yoksa, standart depolama tutarak maliyetleri sınırlayabilirsiniz. Standart depolama SSD yerine sabit disk sürücülerinin (HDD'ler) üzerinde sanal makine disk verilerini depolar. Sanal makine disklerinizi premium diskleri geçirmek seçebilirsiniz. Premium diskleri çoğu sanal makine SKU'ları üzerinde desteklenir. Premium diskleri kullanmak istiyorsanız, ancak bazı durumlarda, sanal makinenize SKU'ları da yükseltmeniz gerekebilir.

![Advisor öneri: premium diskleri yükselterek, sanal makine diskleriniz güvenilirliğini geliştirmeye](./media/advisor-high-availability-recommendations/advisor-high-availability-upgrade-to-premium-disks.png)

## <a name="protect-your-virtual-machine-data-from-accidental-deletion"></a>Sanal makine verilerinizi yanlışlıkla silinmeye karşı koru
Advisor burada yedekleme etkin değil ve yedekleme etkinleştirme önerir sanal makineleri tanımlar. Sanal makine yedekleme ayarı, iş açısından kritik verilerin kullanılabilirliğini sağlar ve yanlışlıkla silme veya bozulmasına karşı koruma sağlar.

![Advisor öneri: Görev açısından kritik verilerinizi korumak için sanal makine yedeklemeyi yapılandırma](./media/advisor-high-availability-recommendations/advisor-high-availability-virtual-machine-backup.png)

## <a name="access-high-availability-recommendations-in-advisor"></a>Advisor erişim yüksek kullanılabilirlik önerileri

1. [Azure Portal](https://portal.azure.com) oturum açın.

2. Sol bölmede **daha fazla hizmet**.

3. Hizmet menü bölmesinde altında **izleme ve Yönetim**, tıklatın **Azure Danışmanı**.  
 Advisor Panosu görüntülenir.

4. Advisor Panoda tıklatın **yüksek kullanılabilirlik** sekmesini tıklatın ve ardından önerileri almak istediğiniz aboneliği seçin.

> [!NOTE]
> Advisor önerileri erişmek için öncelikle *aboneliğinizi kaydetmek* Danışmanı ile. Bir abonelik kayıtlı olduğunda bir *abonelik sahibi* Danışmanı Pano başlatır ve tıkladığında **alma önerileri** düğmesi. Bu bir *tek seferlik işlem*. Abonelik kaydedildikten sonra Advisor önerileri olarak erişebilir *sahibi*, *katkıda bulunan*, veya *okuyucu* bir abonelik, bir kaynak grubu ya da belirli bir kaynak için.

## <a name="next-steps"></a>Sonraki adımlar

Advisor önerileri hakkında daha fazla bilgi için bkz:
* [Azure Danışmanı giriş](advisor-overview.md)
* [Danışman’ı kullanmaya başlama](advisor-get-started.md)
* [Advisor maliyet önerileri](advisor-performance-recommendations.md)
* [Advisor performans önerileri](advisor-performance-recommendations.md)
* [Advisor güvenlik önerileri](advisor-security-recommendations.md)

