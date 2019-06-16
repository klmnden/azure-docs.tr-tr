---
title: Yönetilen örnek genel uç noktalar - güvenli Azure SQL veritabanı yönetilen örneği | Microsoft Docs
description: Ortak Uç noktalara bir yönetilen örneği ile Azure'da güvenli bir şekilde kullanın
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: vanto, carlrab
manager: craigg
ms.date: 05/08/2019
ms.openlocfilehash: f06677b66c8f6586fec8cc5dfe97b1515b741e9c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65470297"
---
# <a name="use-an-azure-sql-database-managed-instance-securely-with-public-endpoints"></a>Bir Azure SQL veritabanı yönetilen örnek genel uç noktaları ile güvenli bir şekilde kullanın.

Azure SQL veritabanı yönetilen örnekleri üzerinde kullanıcı bağlantısı sağlayabilir [ortak Uç noktalara](../virtual-network/virtual-network-service-endpoints-overview.md). Bu makalede, bu yapılandırma daha güvenli hale getirmek açıklanmaktadır.

## <a name="scenarios"></a>Senaryolar

SQL veritabanı yönetilen örneği bağlantısı, sanal ağ içindeki izin vermek için özel bir uç nokta sağlar. En fazla yalıtım sağlamak için varsayılan seçenektir. Ancak, bir ortak uç nokta bağlantı sağlamak için gerek duyduğunuz senaryo vardır:

- Yönetilen örnek çok-tenant-yalnızca hizmet olarak platform (PaaS) teklifleri ile tümleştirmeniz gerekir.
- Veri değişimi VPN kullanırken mümkün olandan daha yüksek iş hacmi ihtiyacınız vardır.
- Şirket ilkelerine, şirket ağlarına PaaS yasaktır.

## <a name="deploy-a-managed-instance-for-public-endpoint-access"></a>Yönetilen örnek genel uç nokta erişim dağıtma

Zorunlu olsa da, bir yönetilen örnek genel uç noktaya erişim için ortak dağıtım modeli adanmış bir yalıtılmış sanal ağında örneği oluşturmaktır. Bu yapılandırmada, sanal ağ yalıtımı için yalnızca sanal küme kullanılır. Yönetilen örneğin IP adres alanını kurumsal bir ağın IP adres alanıyla örtüşüyor olup olmaması önemli değildir.

## <a name="secure-data-in-motion"></a>Hareket halindeki verilerin güvenliğini sağlama

İstemci Sürücü Şifreleme destekliyorsa, yönetilen örnek veri trafiği her zaman şifrelenir. Yönetilen örnek ve diğer Azure sanal makineleri veya Azure hizmetleri arasında gönderilen veriler, Azure'un omurga hiçbir zaman ayrılmaz. Yönetilen örnek ve bir şirket içi ağ arasında bir bağlantı varsa, Microsoft eşlemesi ile Azure ExpressRoute kullanmanızı öneririz. ExpressRoute genel internet üzerinden veri taşıma önlemenize yardımcı olur. Yönetilen örnek için özel bağlantı, yalnızca özel eşleme kullanılabilir.

## <a name="lock-down-inbound-and-outbound-connectivity"></a>Gelen ve giden bağlantı kesilmesi Kilitle

Önerilen güvenlik yapılandırmalarını Aşağıdaki diyagramda gösterilmiştir:

![Güvenlik yapılandırmalarını gelen ve giden bağlantı kesilmesi kilitlemek için](media/sql-database-managed-instance-public-endpoint-securely/managed-instance-vnet.png)

Yönetilen örnek sahip bir [genel uç nokta adresi ayrılmış](sql-database-managed-instance-find-management-endpoint-ip-address.md). İstemci tarafı giden güvenlik duvarı ve ağ güvenlik grubu kuralları, giden bağlantı sınırlamak için bu genel uç noktanın IP adresine ayarlayın.

Yönetilen örnek için trafiği güvenilir bir kaynaktan geliyor emin olmak için iyi bilinen IP adresleriyle kaynaklardan bağlanma öneririz. Yönetilen örnek genel uç nokta bağlantı noktası 3342 erişimi sınırlamak için bir ağ güvenlik grubu kullanın.

İstemcileri, şirket içi ağ üzerinden bir bağlantı başlatmak gerektiğinde kaynak adresi bilinen bir IP adresleri kümesini çevrilir emin olun. Bu nedenle (örneğin, tipik bir senaryo olan bir mobil iş gücü), kullandığınız olan öneririz, yapamazsanız [noktadan siteye VPN bağlantıları ve özel bir uç nokta](sql-database-managed-instance-configure-p2s.md).

Azure'dan bağlantıları başladıysa, trafiği iyi bilinen bir atanan geldiğini öneririz [sanal IP adresi](../virtual-network/virtual-networks-reserved-public-ip.md) (örneğin, bir sanal makine). Sanal IP (VIP) adresleri daha kolay yönetme yapmak için kullanmak isteyebilirsiniz [genel IP adresi ön eklerini](../virtual-network/public-ip-address-prefix.md).

## <a name="next-steps"></a>Sonraki adımlar

- Genel bir uç nokta yönetme örnekleri için yapılandırmayı öğrenin: [Genel uç noktasını yapılandırma](sql-database-managed-instance-public-endpoint-configure.md)
