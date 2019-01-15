---
title: Azure Danışmanı yüksek kullanılabilirlik önerisi | Microsoft Docs
description: Azure Danışmanı, Azure dağıtımlarınızı yüksek kullanılabilirliğini artırmak için kullanın.
services: advisor
documentationcenter: NA
author: kasparks
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: advisor
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/16/2016
ms.author: kasparks
ms.openlocfilehash: 928fb5421297fedbffabc45db35a89a74026477e
ms.sourcegitcommit: 70471c4febc7835e643207420e515b6436235d29
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/15/2019
ms.locfileid: "54305080"
---
# <a name="advisor-high-availability-recommendations"></a>Advisor yüksek kullanılabilirlik önerisi

Azure Danışmanı, sağlamak ve iş açısından kritik uygulamalarınızın sürekliliğini yardımcı olur. Yüksek kullanılabilirlik önerisi Danışmandan tarafından alabilirsiniz **yüksek kullanılabilirlik** Danışman Panosu sekmesi.

## <a name="ensure-virtual-machine-fault-tolerance"></a>Sanal makine hataya dayanıklılık sağlamak

Uygulamanıza yedeklilik sağlamak için bir kullanılabilirlik kümesinde iki veya daha fazla sanal makinenin gruplandırılması önerilir. Danışman, bir kullanılabilirlik kümesi ve bir kullanılabilirlik kümesine taşımak önerir parçası olmayan sanal makineleri tanımlar. Bu yapılandırma ya da bir planlı veya Plansız bakım olayı sırasında en az bir sanal makinenin kullanılabilir ve Azure sanal makine SLA'sına sağlar. Bir kullanılabilirlik kümesi için sanal makine veya sanal makineyi var olan bir kullanılabilirlik kümesine eklemek için seçebilirsiniz.

> [!NOTE]
> Bir kullanılabilirlik kümesi oluşturmayı seçerseniz, içine en az bir sanal makine daha eklemeniz gerekir. Bu en az bir makine emin olmak için iki veya daha fazla sanal makineyi bir kullanılabilirlik kümesi bu, Grup bir kesinti sırasında kullanılabilir öneririz.

## <a name="ensure-availability-set-fault-tolerance"></a>Kullanılabilirlik kümesi hataya dayanıklılık sağlamak 

Uygulamanıza yedeklilik sağlamak için bir kullanılabilirlik kümesinde iki veya daha fazla sanal makinenin gruplandırılması önerilir. Danışman, tek bir sanal makine içeren kullanılabilirlik kümeleri tanımlar ve bir veya daha fazla sanal makine eklemeyi önerir. Bu yapılandırma ya da bir planlı veya Plansız bakım olayı sırasında en az bir sanal makinenin kullanılabilir ve Azure sanal makine SLA'sına sağlar. Bir sanal makine oluşturun veya mevcut bir sanal makine kullanılabilirlik kümesine eklemek için seçebilirsiniz.  

## <a name="use-managed-disks-to-improve-data-reliability"></a>Veri güvenilirliğini geliştirmek için Yönetilen Diskler kullanın
Bir kullanılabilirlik kümesinde depolama hesaplarını veya depolama ölçek birimlerini paylaşan diskler ile olan sanal makineler kesintiler sırasında tek bir depolama ölçek birimi hatalarına karşı dayanıklı değildir. Danışman bu kullanılabilirlik kümeleri tanımlar ve Azure yönetilen diskler için geçiş yapmanızı öneririz. Bu işlem, kullanılabilirlik kümesindeki farklı sanal makinelerin diskleri bir tek hata noktasını önlemek için ayrı tutulmasını garanti eder. 

## <a name="ensure-application-gateway-fault-tolerance"></a>Uygulama ağ geçidi hataya dayanıklılık sağlamak

Uygulama ağ geçidi tarafından desteklenen görev açısından kritik uygulamaların iş sürekliliği sağlamak için hataya dayanıklılık için yapılandırılmamış uygulama ağ geçidi örnekleri Advisor tanımlar ve, uygulayabileceğiniz düzeltme eylemi önerir. Orta ölçekli veya büyük tek örnekli uygulama ağ geçitleri Advisor tanımlar ve en az bir daha fazla örnek ekleyerek önerir. Ayrıca, tek veya çok instance kısa uygulama ağ geçitleri tanımlar ve orta ölçekli veya büyük SKU'lara geçiş önerir. Danışman, uygulama ağ geçidi örnekleri bu kaynaklar için geçerli SLA gereksinimlerini karşılamak için yapılandırıldığından emin olmak için bu eylemler önerir.

## <a name="protect-your-virtual-machine-data-from-accidental-deletion"></a>Sanal makine verilerinizi yanlışlıkla silinmeye karşı koru

Sanal makine yedekleme ayarı, iş açısından kritik verilerin kullanılabilirliğini sağlar ve kazayla silinme veya bozulmaya karşı koruma sağlar. Advisor burada yedekleme etkin değildir ve bu yedekleme etkinleştirilmesini öneriyor sanal makineleri tanımlar. 

## <a name="ensure-you-have-access-to-azure-cloud-experts-when-you-need-it"></a>İhtiyacınız olduğunda Azure bulut uzmanlara erişim olduğundan emin olun

Bir iş açısından kritik iş yükü, gerektiğinde teknik destek erişiminiz olması önemlidir. Danışman, teknik destek destek planlarına sahip olmaması potansiyel iş açısından önemli abonelik tanımlar ve teknik destek içeren bir seçenek yükseltme önerir.

## <a name="create-azure-service-health-alerts-to-be-notified-when-azure-issues-affect-you"></a>Azure sorunlardan etkilendiğiniz durumlar bildirim almak için Azure hizmet durumu uyarıları oluşturma

Azure hizmet durumu uyarıları ayarlama Azure hizmeti sorunlardan etkilendiğiniz durumlar bildirilmesini öneririz. [Azure hizmet durumu](https://azure.microsoft.com/features/service-health/) tarafından bir Azure hizmet sorunu etkilendiğinde, durumlar için kişiselleştirilmiş rehberlik ve destek sağlayan ücretsiz bir hizmettir. Danışman, yapılandırılan uyarı yok abonelikleri tanımlar ve bir oluşturulmasını önerir.

## <a name="configure-traffic-manager-endpoints-for-resiliency"></a>Dayanıklılık için traffic Manager uç noktalarını yapılandırma

Verilen herhangi bir uç noktası başarısız olursa yüksek kullanılabilirlik ile birden fazla uç nokta traffic Manager profillerini karşılaşırsınız. Daha fazla farklı bölgelerde uç noktaları yerleştirme, hizmet güvenilirliğini artırır. Danışman, tek bir uç nokta olduğu ve başka bir bölgede en az bir daha fazla uç nokta ekleme önerir Traffic Manager profilleri tanımlar.

Bir Traffic Manager profilinde yakınlık yönlendirme için yapılandırılan tüm uç noktalar aynı bölgedeyse kullanıcılar diğer bölgelerdeki bağlantı gecikmeleri karşılaşabilir. Ekleme veya başka bir bölgeye bir uç nokta taşıma genel performansını ve tüm uç noktaları bir bölgede başarısız olursa, daha iyi kullanılabilirlik sağlar. Danışman, Traffic Manager profillerini tüm uç noktalar aynı bölgede nerede yakınlık yönlendirme için yapılandırılan tanımlar ve ekleme veya başka bir Azure bölgesine bir uç nokta taşıma önerir.

Traffic Manager profili devre dışı coğrafi yönlendirme için yapılandırılmışsa, trafiği tanımlanmış bölgelerine bağlı Uç noktalara yönlendirilir. Bir bölgede başarısız olursa, önceden tanımlanmış hiçbir yük devretme yoktur. Bölgesel gruplandırma "Tüm (dünya)" yapılandırıldığı bir uç noktaya sahip bırakılan trafik önlemek ve hizmet kullanılabilirliğini artırın. Danışman, Traffic Manager profillerini uç nokta "Tüm (World)" bölgesel gruplandırma olacak şekilde yapılandırılmış olduğu ve bu yapılandırma değişikliği yapmadan önerir coğrafi yönlendirme için yapılandırılan tanımlar.

## <a name="use-soft-delete-on-your-azure-storage-account-to-save-and-recover-data-in-the-event-of-accidental-overwrite-or-deletion"></a>Yazılım kullanım kaydetmek ve verileri yanlışlıkla üzerine yaz veya silinmesi durumunda kurtarma için Azure depolama hesabı Sil

Etkinleştirme [geçici silme](https://docs.microsoft.com/azure/storage/blobs/storage-blob-soft-delete) silinen blobları geçiş kalıcı olan yerine geçici silinen durumuna, depolama hesabınız silindi. Verilerin üzerine, verilerin üzerine yazılması durumunu kaydetmek için geçici silinen bir anlık görüntü oluşturulur. Bu, yanlışlıkla silinmesi durumunda kurtarmanıza olanak tanır veya üzerine yazar. Danışman, geçici silme etkinleştirilebilir yoksa bir Azure depolama hesapları tanımlar ve bu etkinleştirmenizi önerir.

## <a name="configure-your-vpn-gateway-to-active-active-for-connection-resiliency"></a>Etkin-etkin VPN ağ geçidinizi yapılandırma bağlantı dayanıklılığı

Etkin-etkin yapılandırmasında S2S VPN tünelinde şirket içi VPN cihazınız için hem bir VPN ağ geçidi örneklerini oluşturacaktır. Bir ağ geçidi örneğinde planlı bir bakım olayı veya planlanmamış bir olay gerçekleştiğinde, trafiğin diğer etkin IPSec tünel için otomatik olarak geçirilecek. Azure Danışmanı etkin-etkin olarak yapılandırılmamış bir VPN ağ geçitleri tanımlar ve bunları yüksek kullanılabilirlik için yapılandırma önerin.

## <a name="how-to-access-high-availability-recommendations-in-advisor"></a>Yüksek kullanılabilirlik önerileri Danışman erişme

1. Oturum [Azure portalında](https://portal.azure.com)ve ardından açın [Advisor](https://aka.ms/azureadvisordashboard).

2.  Advisor panosunda **yüksek kullanılabilirlik** sekmesi.

## <a name="next-steps"></a>Sonraki adımlar

Danışman önerileri hakkında daha fazla bilgi için bkz:
* [Azure Danışmanı giriş](advisor-overview.md)
* [Danışman’ı kullanmaya başlama](advisor-get-started.md)
* [Advisor maliyet önerileri](advisor-cost-recommendations.md)
* [Danışmanı performans önerileri](advisor-performance-recommendations.md)
* [Advisor güvenlik önerileri](advisor-security-recommendations.md)

