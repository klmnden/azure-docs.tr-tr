---
title: Azure sanal ağ TAP'ne genel bakış | Microsoft Docs
description: Sanal ağ TAP hakkında bilgi edinin. Sanal ağ TAP bir paket Toplayıcıya yapılabilen sanal makine ağ trafiğinin derin kopya sağlar.
services: virtual-network
documentationcenter: na
author: karthikananth
manager: ganesr
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/14/2019
ms.author: kaanan
ms.openlocfilehash: ff5c8c4d3f6a0c87afae67404a5a39d4fe3757d9
ms.sourcegitcommit: e89b9a75e3710559a9d2c705801c306c4e3de16c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/15/2019
ms.locfileid: "59571104"
---
# <a name="virtual-network-tap"></a>Sanal ağ TAP

Azure sanal ağ TAP (Terminal erişim noktası) bir ağ paketi Toplayıcı veya Analiz aracı, sanal makine ağ trafiğini sürekli akışı sağlar. Toplayıcı veya Analiz aracı tarafından sağlanan bir [ağ sanal Gereci](https://azure.microsoft.com/solutions/network-appliances/) iş ortağı. Sanal ağ TAP ile çalışacak şekilde doğrulanmış olan iş ortağı çözümlerini bir listesi için bkz. [iş ortağı çözümleri](#virtual-network-tap-partner-solutions).

> [!IMPORTANT]
> Sanal ağ TAP şu an tüm Azure bölgelerinde Önizleme aşamasındadır. Sanal ağ TAP'ı kullanmak için Önizleme'de bir e-posta göndererek kaydedilmelidir <azurevnettap@microsoft.com> abonelik kimliğinizi Aboneliğiniz kaydedildiğinde siz de bir e-posta alırsınız. Bir onay e-posta aldığınız kadar yeteneği kullanmanız mümkün değildir. Bu önizleme sürümü, bir hizmet düzeyi sözleşmesi olmadan sağlanmaktadır ve üretim iş yükleri için kullanılmamalıdır. Belirli özellikler desteklenmiyor olabilir, kısıtlı yeteneklere sahip olabilir veya tüm Azure konumlarında mevcut olmayabilir. Bkz: [ek kullanım koşulları Microsoft Azure önizlemeleri için](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) Ayrıntılar için.

## <a name="virtual-network-tap-partner-solutions"></a>Sanal ağ TAP iş ortağı çözümleri

### <a name="network-packet-brokers"></a>Ağ paket aracıları

- [Yapı izleme büyük büyük anahtarı](https://www.bigswitch.com/products/big-monitoring-fabric/public-cloud/microsoft-azure)
- [Gigamon GigaSECURE](https://blog.gigamon.com/2018/09/13/why-microsofts-new-vtap-service-works-even-better-with-gigasecure-for-azure)
- [Ixia CloudLens](https://www.ixiacom.com/cloudlens/cloudlens-azure)
- [Nubeva Prisms](https://www.nubeva.com/azurevtap)

### <a name="security-analytics-networkapplication-performance-management"></a>Güvenlik analizi, ağ/uygulama performansı Yönetimi

- [Uyanık güvenlik](https://awakesecurity.com/technology-partners/microsoft-azure/)
- [Cisco Stealthwatch Cloud](https://blogs.cisco.com/security/cisco-stealthwatch-cloud-and-microsoft-azure-reliable-cloud-infrastructure-meets-comprehensive-cloud-security)
- [Darktrace](https://www.darktrace.com/en/azure/)
- [ExtraHop Reveal(x)](https://www.extrahop.com/company/tech-partners/microsoft/)
- [Fidelis siber güvenlik](https://www.fidelissecurity.com/technology-partners/microsoft-azure )
- [Flowmon](https://www.flowmon.com/blog/azure-vtap)
- [NetFort LANGuardian](https://www.netfort.com/languardian/solutions/visibility-in-azure-network-tap/)
- [Netscout vSTREAM]( https://www.netscout.com/technology-partners/microsoft/azure-vtap)
- [RSA NetWitness® platformu](https://www.rsa.com/azure)
- [Vectra Cognito](https://vectra.ai/microsoftazure)

DOKUNUN works aşağıdaki resmin gösterdiği nasıl sanal ağ. Bir DOKUNUN yapılandırması ekleyebileceğiniz bir [ağ arabirimi](virtual-network-network-interface.md) sanal ağınızda dağıtılan bir sanal makineye bağlı. İzlenen ağ arabirimi aynı sanal ağdaki bir sanal ağ IP adresi hedef olduğunu veya bir [eşlenmiş sanal](virtual-network-peering-overview.md) ağ. Sanal ağ TAP Toplayıcı çözümü arkasına dağıtılabilir bir [Azure iç yük dengeleyici](../load-balancer/load-balancer-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#concepts) yüksek kullanılabilirlik için. Tek bir çözüm için dağıtım seçeneklerini değerlendirmek için bkz: [iş ortağı çözümleri](#virtual-network-tap-partner-solutions).

![DOKUNUN nasıl sanal ağ çalışır](./media/virtual-network-tap/architecture.png)

## <a name="prerequisites"></a>Önkoşullar

Sanal ağ TAP oluşturmadan önce önizlemede kaydedilen ve bir veya daha fazla sanal makine kullanılarak oluşturulan bir onay posta aldığınız gerekir [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) dağıtım modeli ve bir iş ortağı aynı azure bölgesinde DOKUNUN trafiği toplayarak çözümü. Sanal ağınızdaki bir iş ortağı çözümü yoksa bkz [iş ortağı çözümleri](#virtual-network-tap-partner-solutions) birini dağıtmak için. Aynı sanal ağda birden çok ağ arabirimi aynı veya farklı Aboneliklerdeki trafiğe DOKUNUN kaynak kullanabilirsiniz. İzlenen ağ arabirimleri farklı Aboneliklerde olması halinde, aboneliklerin aynı Azure Active Directory kiracısı ile ilişkilendirilmesi gerekir. Ayrıca, izlenen ağ arabirimleri ve toplama DOKUNUN trafik için hedef uç nokta aynı bölgedeki eşlenmiş sanal ağlarda bulunan olabilir. Bu dağıtım modeli kullanıyorsanız emin [sanal ağ eşlemesi](virtual-network-peering-overview.md) sanal ağ TAP'ı yapılandırmadan önce etkinleştirilir.

## <a name="permissions"></a>İzinler

Ağ arabirimleri üzerinde DOKUNUN yapılandırmasını uygulamak için kullandığınız hesapları atanmalıdır [ağ Katılımcısı](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) rolü veya bir [özel rol](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) gerekli eylemleri aşağıdaki tablodan atanır:

| Eylem | Ad |
|---|---|
| Microsoft.Network/virtualNetworkTaps/* | Oluşturmak için gereken, güncelleştirme, okuma ve bir sanal ağ TAP kaynak silme |
| Microsoft.Network/networkInterfaces/read | DOKUNUN yapılandırılacak ağ arabirimi kaynağı okumak için gerekli |
| Microsoft.Network/tapConfigurations/* | Oluşturmak için gereken, güncelleştirme, okuma ve bir ağ arabiriminde DOKUNUN yapılandırmasını silme |

## <a name="next-steps"></a>Sonraki adımlar

- Bilgi edinmek için nasıl [DOKUNUN sanal ağ oluşturma](tutorial-tap-virtual-network-cli.md).
