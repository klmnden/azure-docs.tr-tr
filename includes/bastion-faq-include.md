---
title: include dosyası
description: include dosyası
services: bastion
author: cherylmc
ms.service: bastion
ms.topic: include
ms.date: 06/17/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 29ab9b3c33aae6005510c34b207c7f87714149e5
ms.sourcegitcommit: ccb9a7b7da48473362266f20950af190ae88c09b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2019
ms.locfileid: "67608398"
---
### <a name="preview"></a>Genel önizlemeye nasıl katılmak?

Ekleme genel önizlemeye katılmak için ihtiyacınız vardır. İçindeki adımları kullanın [bu makalede](../articles/bastion/bastion-create-host-portal.md) yeni Azure savunma kaynak oluşturmak için. Şu anda erişme ve bu hizmeti kullanırken kullanmalısınız [Azure portalı - preview](https://aka.ms/BastionHost) normal Azure portalı yerine.

### <a name="regions"></a>Önizleme sırasında hangi bölgeler mevcuttur?

Dağıtmak ve bunlardan birine kaynağı Önizleme bölgeleri aracılığıyla savunma kullanın [Azure portalı - Önizleme bağlantı](https://aka.ms/BastionHost).

[!INCLUDE [region](bastion-regions-include.md)]

### <a name="portal"></a>Azure portalında savunma kaynak bulamıyorum. Ne yapmalıyım?

Kullandığınız emin [Azure portalı - Önizleme bağlantı](https://aka.ms/BastionHost), normal Azure portalını değil.

### <a name="publicip"></a>Sanal Makinem bir genel IP gerekiyor mu?

Bir genel IP üzerinde Azure Azure savunma hizmetiyle bağlandığınız VM gerekmez. Savunma hizmetini RDP/SSH'yi açar, sanal makinenizin sanal makinenizin sanal ağınızdaki özel IP üzerinden oturum/bağlantı.

### <a name="rdpssh"></a>Bir RDP veya SSH istemcisi ihtiyacım var?

Azure portalınızda Azure sanal makinenize RDP/SSH erişimi sağlamak için RDP veya SSH istemcisine ihtiyacınız yoktur. Kullanım [Azure portalı - Önizleme bağlantı](https://aka.ms/BastionHost) Portal Önizleme uçuş erişmek için. Bu, doğrudan tarayıcıda sanal makinenize RDP/SSH erişimi elde etmenizi sağlar.

### <a name="agent"></a>Azure sanal makinesinde çalışan bir aracının ihtiyacım var?

Tarayıcınıza veya Azure sanal makinenize bir aracı veya herhangi bir yazılım yüklemeniz gerekmez. Bastion hizmeti aracısızdır ve RDP/SSH için ek bir yazılım gerektirmez.

### <a name="browsers"></a>Hangi tarayıcılar desteklenmektedir?

Genel Önizleme sırasında Windows üzerinde Microsoft Edge tarayıcı veya Google Chrome kullanın. Apple Mac için Google Chrome tarayıcıyı kullanın. Microsoft Edge Chromium sırasıyla Windows ve Mac’te de desteklenir.

### <a name="roles"></a>Bir sanal makineye erişmek için gerekli herhangi bir rolü misiniz?

Bir bağlantı kurmak için aşağıdaki roller gereklidir:

* Sanal makinede okuyucusu rolü
* NIC sanal makinenin özel IP'si ile okuyucusu rolü
* Okuyucu rolü Azure savunma kaynağı

### <a name="previewbill"></a>Fiyatlandırma - Önizleme'de katılmak için faturalandırılırım?

Yalnızca kısmen genel Önizleme sırasında faturalandırılırsınız. Ancak, dağıtımınıza bağlı SLA yoktur. Daha fazla bilgi edinmek için bkz. [fiyatlandırma sayfası](https://aka.ms/BastionHostPricing).

### <a name="previewbill"></a>Neden "oturumunuzun süresi doldu" hatası alıyorum savunma oturumu başlamadan önce ileti?

Yalnızca Azure Portalı'ndan bir oturumu yeniden başlatılması gerekir. Azure portalında oturum açın ve oturumunuz yeniden başlayın. Doğrudan başka bir tarayıcı oturumu veya sekme URL'sine gidin, bu hata beklenir. Oturumunuz daha güvenli olduğunu ve oturum yalnızca Azure portalı üzerinden erişilebildiğinden emin olun yardımcı olur.
