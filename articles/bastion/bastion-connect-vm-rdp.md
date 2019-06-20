---
title: Azure savunma kullanarak bir Windows VM'ye bağlanma | Microsoft Docs
description: Bu makalede, için bir Azure Windows Azure savunma kullanarak çalıştıran sanal makine bağlanmayı öğreneceksiniz.
services: bastion
author: cherylmc
ms.service: bastion
ms.topic: conceptual
ms.date: 06/03/2019
ms.author: cherylmc
ms.openlocfilehash: c8a4b09a27325f31e548d1b345b2932c6ab6315c
ms.sourcegitcommit: 156b313eec59ad1b5a820fabb4d0f16b602737fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67191895"
---
# <a name="connect-to-a-windows-virtual-machine-using-azure-bastion-preview"></a>Azure savunma (Önizleme) kullanarak bir Windows sanal makinesine bağlanma

Bu makale, nasıl güvenli ve sorunsuz bir şekilde RDP için Windows Vm'lerinizi bir Azure sanal ağ kullanarak Azure savunma. Azure portalından doğrudan bir sanal makineye bağlanabilirsiniz. Azure Bastion’ı kullanırken sanal makineler için bir istemci, aracı veya ek yazılım gerekmez. Azure savunma hakkında daha fazla bilgi için bkz: [genel bakış](bastion-overview.md).

> [!IMPORTANT]
> Bu genel önizleme bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılmamalıdır. Belirli özellikler desteklenmiyor olabilir, kısıtlı yeteneklere sahip olabilir veya tüm Azure konumlarında mevcut olmayabilir. Ayrıntılar için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
>

## <a name="before-you-begin"></a>Başlamadan önce

Sanal Makinenin bulunduğu sanal ağ için bir Azure Burcu ana bilgisayarı'kurmak ayarladığınızdan emin olun. Daha fazla bilgi için [bir Azure Burcu ana bilgisayarı oluşturma](bastion-create-host-portal.md). Savunma hizmet sağlanması ve sanal ağınızda dağıtılan sonra bu sanal ağdaki herhangi bir VM bağlanmak için kullanabilirsiniz. Bu önizleme sürümünde, savunma bağlanmak için bir Windows VM ve SSH, Linux Vm'lere bağlanmak için RDP kullandığınızı varsayar. Bir Linux VM'ye bağlanma hakkında daha fazla bilgi için bkz. [- Linux VM'ye bağlanma](bastion-connect-vm-ssh.md).

Bir bağlantı kurmak için aşağıdaki roller gereklidir:

* Sanal makinede okuyucusu rolü
* NIC sanal makinenin özel IP'si ile okuyucusu rolü
* Okuyucu rolü Azure savunma kaynağı

## <a name="rdp"></a>RDP kullanarak bağlan

1. İçinde [Azure portalında](https://aka.ms/BastionHost) savunma Önizleme, bağlanmak istediğiniz sanal makineye gidin ve ardından **Connect**. VM ile RDP bağlantısı kullanılırken, bir Windows sanal makine olması gerekir.

    ![VM'ye bağlanın](./media/bastion-connect-vm-rdp/connect.png)

1. Bağlan'a tıklayın, sonra üç sekme – RDP ve SSH güvenlikli olan bir kenar çubuğu görünür. Sanal ağ için savunma sağlanması halinde, savunma sekmesi varsayılan olarak etkindir. Sanal ağ için savunma sağlamadıysanız, savunma yapılandırmak için bağlantıya tıklayabilirsiniz. Yapılandırma yönergeleri için bkz. [yapılandırma savunma](bastion-create-host-portal.md). Görmüyorsanız, **savunma** listelenen Önizleme portalı açtığınız değil. Bunu kullanarak portalda açın [Önizleme bağlantı](https://aka.ms/BastionHost).

    ![VM'ye bağlanın](./media/bastion-connect-vm-rdp/bastion.png)

1. Savunma sekmesi, kullanıcı adı ve parola sanal makineniz için ardından **Connect**. Bağlantı noktası 443 ve savunma hizmetini kullanarak doğrudan Azure portalında (HTML5) üzerinden savunma aracılığıyla bu sanal makine ile RDP bağlantısı açılır.

    ![VM'ye bağlanın](./media/bastion-connect-vm-rdp/443rdp.png)
 
## <a name="next-steps"></a>Sonraki adımlar

Okuma [savunma SSS](bastion-faq.md)