---
title: 'Kopyalayıp için ve bir sanal makineden yapıştırın: Azure savunma | Microsoft Docs'
description: Bu makalede, bilgi nasıl kopyalayıp savunma kullanarak bir Azure VM'ye gelen ve giden.
services: bastion
author: cherylmc
ms.service: bastion
ms.topic: conceptual
ms.date: 06/03/2019
ms.author: cherylmc
ms.openlocfilehash: 9d69d1a9fae258c9546a8c794fc0b0c68b952049
ms.sourcegitcommit: 156b313eec59ad1b5a820fabb4d0f16b602737fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67191817"
---
# <a name="copy-and-paste-to-a-virtual-machine-azure-bastion-preview"></a>Sanal bir makineyi kopyalayıp yeniden oluştur: Azure savunma (Önizleme)

Bu makalede, Azure savunma kullanırken sanal makinelere gelen ve giden metni yapıştırın yardımcı olur. Bir VM ile çalışmadan önce adımlarına emin olun [Burcu ana bilgisayarı oluşturma](bastion-create-host-portal.md). Ardından, ya da kullanarak birlikte çalışmak istediğiniz VM bağlanmak [RDP](bastion-connect-vm-rdp.md) veya [SSH](bastion-connect-vm-ssh.md).

> [!IMPORTANT]
> Bu genel önizleme bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılmamalıdır. Belirli özellikler desteklenmiyor olabilir, kısıtlı yeteneklere sahip olabilir veya tüm Azure konumlarında mevcut olmayabilir. Ayrıntılar için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
>

Gelişmiş Pano API erişimi destekleyen tarayıcılar için kopyalayın ve metni kopyalayın ve yapıştırın, yerel cihazda uygulamalar arasında aynı şekilde cihazınızın yerel ve uzak oturumu arasında yapıştırın. Diğer tarayıcılar için savunma Pano erişim aracı palet kullanabilirsiniz.

  ![Pano izin ver](./media/bastion-vm-manage/allow.png)

Yalnızca metin kopyala/yapıştır desteklenir. Savunma oturum başlatıldığında doğrudan kopyalama ve yapıştırma için tarayıcınızı için Pano erişimini isteyebilir. **İzin** panoya erişmek için web sayfası.

## <a name="to"></a>Uzak oturumu kopyalayın

Sanal makineyi kullanarak bağlandıktan sonra [Azure portalında](https://aka.ms/BastionHost) savunma önizlemesi için:

1. Metin/içeriği yerel cihazdaki yerel panoya kopyalayın.
1. Uzak oturumu sırasında savunma Pano erişim aracı palet iki ok seçerek başlatın. Oklar, oturumun sol Merkezi'nde yer alır.

    ![aracı paleti](./media/bastion-vm-manage/left.png)

    ![Pano](./media/bastion-vm-manage/clipboard.png)

1. Genellikle, kopyalanan metni otomatik olarak savunma kopyası Yapıştır paletinde gösterir. Metninizi yoksa, metni Palette metin alanına yapıştırın.
1. Metni metin alanına olduktan sonra uzak oturumu yapıştırabilirsiniz.

    ![Yapıştır](./media/bastion-vm-manage/local.png)

## <a name="from"></a>Uzak oturumu kopyalayın

Sanal makineyi kullanarak bağlandıktan sonra [Azure portalında](https://aka.ms/BastionHost) savunma önizlemesi için:

1. Metin/içeriği uzak oturumu (Ctrl-C kullanarak) uzaktan panoya kopyalayın.

    ![aracı paleti](./media/bastion-vm-manage/remote.png)

1. Uzak oturumu sırasında savunma Pano erişim aracı palet iki ok seçerek başlatın. Oklar, oturumun sol Merkezi'nde yer alır.

    ![Pano](./media/bastion-vm-manage/clipboard2.png)

1. Genellikle, kopyalanan metni otomatik olarak savunma kopyası Yapıştır paletinde gösterir. Metninizi yoksa, metni Palette metin alanına yapıştırın.
1. Metin metin alanında olduğunda, yerel cihaza yapıştırabilirsiniz.

    ![Yapıştır](./media/bastion-vm-manage/local2.png)
 
## <a name="next-steps"></a>Sonraki adımlar

Okuma [savunma SSS](bastion-faq.md).