---
title: VMware çözümüyle CloudSimple - Azure ağ bağlantıları
description: Azure sanal ağınız CloudSimple bölge ağınıza bağlama hakkında bilgi edinin
author: sharaths-cs
ms.author: dikamath
ms.date: 04/10/2019
ms.topic: article
ms.service: vmware
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: f2ab82b6c1b4b373c186019eaf96f9864861b9d9
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66497608"
---
# <a name="azure-network-connections-overview"></a>Azure ağ bağlantılara genel bakış

Bir bölgede bir CloudSimple hizmet oluşturduğunuzda bunu:

* Bir Azure ExpressRoute bağlantı hattı oluşturur ve bu bölgede hizmeti ekler
* Azure sanal ağınız veya şirket içi ağınızı Azure ExpressRoute kullanarak CloudSimple bölge arasında bağlantı sağlar
* Azure aboneliğiniz ya da şirket içi ağınızdan, özel bulut ortamınızda çalışan erişim hizmetleri sağlar

Bağlantı verilmiştir:

* Güvenlik
* Özel
* Yüksek bant genişliği
* Düşük gecikme süresi

## <a name="benefits"></a>Avantajlar

Azure ağ bağlantısı sağlar:

* Azure sanal makineleri, özel bulut için bir yedekleme hedefi olarak kullanın.
* KMS sunucuları, özel bulut vSAN veri deposunu şifrelemek için Azure aboneliğinize dağıtın.
* Burada, uygulamanın web katmanı genel bulutta sırasında uygulama çalışır ve veritabanı katmanları, özel bulutta çalışan karma uygulamalar'ı kullanın.

## <a name="azure-virtual-network-connection"></a>Azure sanal ağ bağlantısı

ExpressRoute kullanarak Azure kaynaklarınızı özel Bulutları bağlanabilir.  Bu bağlantı, Azure aboneliğinizde özel Buluttan çalıştırılan farklı kaynaklara erişmek için kullanabilirsiniz.  Bu bağlantı, Azure sanal ağınız için özel bulut ağ genişletmenizi sağlar.

![Sanal ağ için Azure ExpressRoute bağlantısı](media/cloudsimple-azure-network-connection.png)

## <a name="expressroute-connection-to-on-premises-network"></a>Şirket içi bir ağı ExpressRoute bağlantısı

Var olan Azure ExpressRoute devreniz CloudSimple bölgenizi bağlanabilirsiniz. ExpressRoute Global erişim özelliği, iki bağlantı hatlarının birbirleri ile bağlanmak için kullanılır.  CloudSimple ExpressRoute bağlantı hatları ve şirket içi arasında bağlantı kurulur.  Bu bağlantı, özel bulut ağ, şirket içi ağların kapsamını genişletmek olanak tanır.

![Şirket içi ExpressRoute bağlantısı - Global erişim](media/cloudsimple-global-reach-connection.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure sanal ağına CloudSimple bağlantı için eşleme bilgi alın](https://docs.azure.cloudsimple.com/virtual-network-connection)
* [ExpressRoute kullanarak CloudSimple için şirket içi bağlanma](https://docs.azure.cloudsimple.com/on-premises-connection)
