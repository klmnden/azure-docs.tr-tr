---
title: Azure savunma | Microsoft Docs
description: Azure savunma hakkında bilgi edinin
services: bastion
author: cherylmc
ms.service: bastion
ms.topic: overview
ms.date: 06/17/2019
ms.author: cherylmc
ms.openlocfilehash: cfd68bbacf4cf8171efdba7878ec8c06055a4997
ms.sourcegitcommit: 156b313eec59ad1b5a820fabb4d0f16b602737fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67191171"
---
# <a name="what-is-azure-bastion-preview"></a>Azure savunma nedir? (Önizleme)

Sanal ağınız içinde sağladığınız yeni bir tam platformu olarak yönetilen PaaS hizmeti Azure savunma hizmetidir. Bu, SSL üzerinden doğrudan Azure portalında sanal makinelerinize güvenli ve sorunsuz RDP/SSH bağlantı sağlar. Azure Bastion aracılığıyla bağlandığınızda, sanal makinelerinizin bir genel IP adresi olması gerekmez.

 Savunma, sağlanan sanal ağ içindeki tüm sanal makineleri güvenli RDP ve SSH bağlantısı sağlar. Azure savunma kullanarak sanal makinelerinizi hala RDP/SSH kullanarak güvenli erişim sağlarken dış dünya RDP/SSH bağlantı noktalarını gösterme korur. Azure savunma ile doğrudan Azure portalından sanal makineye bağlanın. Bir ek istemci, aracı veya yazılım gerekmez.

> [!IMPORTANT]
> Bu genel önizleme bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılmamalıdır. Belirli özellikler desteklenmiyor olabilir, kısıtlı yeteneklere sahip olabilir veya tüm Azure konumlarında mevcut olmayabilir. Ayrıntılar için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
>

## <a name="architecture"></a>Mimari

Azure savunma sanal ağınızda dağıtılan ve dağıtıldıktan sonra sanal ağınızdaki tüm sanal makineler için güvenli RDP/SSH deneyimi sağlar. Sanal ağınızda Azure savunma hizmet sağladıktan sonra RDP/SSH deneyimi, aynı sanal ağdaki tüm Vm'leriniz için kullanılabilir. Abonelik/hesap ya da sanal makine başına sanal ağ başına dağıtımıdır.

RDP ve SSH Azure'da çalışan bir iş yükünüz için bağlanabileceğiniz temel anlamına gelir bazılarıdır. RDP/SSH bağlantı noktası Internet üzerinden kullanıma sunan gerekli değildir ve önemli tehdit yüzeyi olarak görülür. Bu genellikle protokol güvenlik açıkları nedeniyle olur. Bu tehdit yüzeyini içeren çevre ağınızda genel tarafında savunma ana bilgisayar (diğer adıyla atlama-sunucuları) dağıtabilirsiniz. Savunma ana bilgisayarı sunucularını tasarlanmış ve saldırıları dayanacak şekilde yapılandırılmış. Savunma sunucuları de savunma oturan iş yükleri için RDP ve SSH bağlantısı sağlamak, hem de ağ içinde daha fazla.

![architecture](./media/bastion-overview/architecture.png)

Bu şekilde bir Azure savunma dağıtım mimarisi gösterilmektedir. Bu diyagramda:

* Kale ana bilgisayarı sanal ağ içinde dağıtılır.
* Kullanıcı, Azure portalında herhangi bir HTML5 tarayıcı kullanarak bağlanır.
* Kullanıcı bağlanmak için sanal makine seçer.
* Tek bir tıklamayla RDP/SSH oturumundan tarayıcıda açılır.
* Azure sanal makinesinde hiçbir genel IP gereklidir.

## <a name="key-features"></a>Önemli özellikler

Genel Önizleme sırasında denemek aşağıdaki özellikler mevcuttur:

* **RDP ve SSH doğrudan Azure Portalı'nda:** Tek tıklamayla sorunsuz bir deneyim kullanarak doğrudan Azure portalının RDP ve SSH oturumunda doğrudan alabilirsiniz.
* **Uzak oturum üzerinden SSL ve güvenlik duvarı geçişi RDP/SSH için:** SSL üzerinden güvenlik duvarları güvenli bir şekilde çapraz geçiş sağlayarak 443 numaralı bağlantı noktası üzerinde RDP/SSH oturumunuzun eklentisinden azure savunma yerel cihazınız otomatik olarak akışa bir HTML5 tabanlı web istemcisi kullanır.
* **Hiçbir genel IP, Azure sanal makinesinde gerekli:** Azure savunma sanal Makinenize özel IP kullanan, Azure sanal makinesi için RDP/SSH bağlantısı açılır. Sanal makinenizde bir genel IP gerekmez.
* **Nsg'ler yönetme sorunsuz:** Azure savunma, RDP/SSH bağlantısı güvenliğini sağlamak için dahili olarak sıkı Azure PaaS hizmeti tam olarak yönetilen bir platformdur. Azure savunma alt ağda Nsg uygulamak gerek yoktur. Azure savunma, sanal makinelerinizi, özel IP bağlandığından, Nsg'lerinizi yalnızca Azure savunma RDP/SSH izin verecek şekilde yapılandırabilirsiniz. Bu, Nsg'ler sanal makinelerinize güvenli bir şekilde bağlanmak için her zaman yönetmek için ortadan kaldırır.
* **Korumayı yeniden bağlantı noktası tarama:** Sanal makinelerinizi genel İnternet'e kullanıma gerekmez çünkü Vm'lerinizi bağlantı noktası tarama dolandırıcı ve sanal ağınızın dışında bulunan kötü amaçlı kullanıcılara karşı korunur.
* **Sıfır gün açıklarından karşı koruyun. Yalnızca tek bir yerde sağlamlaştırma:** Azure savunma bir tam platformu olarak yönetilen PaaS hizmetidir. Sanal ağ çevre bulunur çünkü sanal ağınızdaki sanal makinelerin her biri sağlamlaştırma hakkında endişelenmeniz gerekmez. Azure platformu, Azure savunma sağlamlaştırılmış ve her zaman güncel sizin için otomatik olarak kilitlenmesini sağlayarak sıfır gün açıklarından karşı korur.

## <a name="faq"></a>SSS

[!INCLUDE [Bastion FAQ](../../includes/bastion-faq-include.md)]

## <a name="next-steps"></a>Sonraki adımlar

* [Bir Azure savunma ana kaynak oluşturma](bastion-create-host-portal.md).
* Azure'un diğer önemli [ağ özelliklerinden](../networking/networking-overview.md) bazıları hakkında bilgi edinin.