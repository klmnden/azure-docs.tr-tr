---
title: Yönetilen örnek genel uç noktalar - güvenli Azure SQL veritabanı yönetilen örneği | Microsoft Docs
description: Ortak Uç noktalara yönetilen örneği ile Azure'da güvenli bir şekilde kullanma
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: vanto, carlrab
manager: craigg
ms.date: 04/16/2019
ms.openlocfilehash: 9d5a3d18e8a7d3c5a6cb08e16e74dd4fbda9ca31
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "60014532"
---
# <a name="using-azure-sql-database-managed-instance-securely-with-public-endpoint"></a>Azure SQL veritabanı yönetilen örnek genel uç noktası ile güvenli bir şekilde

Azure SQL veritabanı yönetilen örneği üzerinden kullanıcı bağlantısı sağlamak için etkinleştirilmesi [genel uç nokta](../virtual-network/virtual-network-service-endpoints-overview.md). Bu makalede, bu yapılandırma daha güvenli hale getirme yönergeleri sağlanır.

## <a name="scenarios"></a>Senaryolar

Yönetilen örnek, sanal ağ içindeki bağlantısı etkinleştirmek için özel uç nokta sağlar. En fazla yalıtım sağlamak için varsayılan seçenektir. Ancak, bir ortak uç nokta bağlantı gerekmesi halinde senaryo vardır:

- Çok kiracılı yalnızca PaaS teklifleri ile tümleştirme.
- VPN kullanarak daha yüksek aktarım hızı veri değişimi.
- Şirket ilkelerine, şirket ağlarına PaaS yasaktır.

## <a name="deploying-managed-instance-for-public-endpoint-access"></a>Yönetilen örnek genel uç nokta erişim dağıtma

Zorunlu olsa da, bir yönetilen örnek genel uç noktaya erişim için ortak dağıtım modeli adanmış bir yalıtılmış sanal ağında örneği oluşturmaktır. Bu yapılandırmada, sanal ağ yalıtımı için yalnızca sanal küme kullanılır. Yönetilen örnek IP adres alanı bir şirket ağındaki IP adres alanıyla çakışıyor ilgili değildir.

## <a name="securing-data-in-motion"></a>Hareket halindeki verilerin güvenliğini sağlama

İstemci Sürücü Şifreleme destekliyorsa, yönetilen örnek veri trafiği her zaman şifrelenir. Yönetilen örnek ve diğer Azure sanal makineler veya Azure Hizmetleri arasındaki veri, Azure'un omurga hiçbir zaman ayrılmaz. Yönetilen örnek için bir şirket içi ağ bağlantısı varsa, Express Route Microsoft eşlemesi ile kullanmak için önerilir. Expressroute genel Internet üzerinden veri taşımaktan kaçının yardımcı olur (yönetilen örnek için özel bağlantı, yalnızca özel eşleme kullanılabilir).

## <a name="locking-down-inbound-and-outbound-connectivity"></a>Gelen ve giden bağlantı kesilmesi kilitleme

Aşağıdaki diyagramda, önerilen güvenlik yapılandırmalarını gösterir.

![managed-instance-vnet.png](media/sql-database-managed-instance-public-endpoint-securely/managed-instance-vnet.png)

Yönetilen örnek sahip bir [genel uç nokta adresi ayrılmış](sql-database-managed-instance-find-management-endpoint-ip-address.md). Bu IP adresi, istemci tarafı giden güvenlik duvarı ve ağ güvenlik grubu kurallarını giden bağlantı sınırlamak için ayarlanmalıdır.

Yönetilen örnek için trafiği güvenilir bir kaynaktan geliyor emin olmak için bu iyi bilinen IP adresleriyle kaynaklardan bağlanmak için önerilir. Bağlantı noktası 3342 yönetilen örnek genel uç noktaya erişimi sınırlayan bir ağ güvenlik grubu kullanarak.

İstemcileri, şirket içi ağ üzerinden bir bağlantı başlatmak gerektiğinde kaynak adresi IP'ler iyi bilinen bir dizi için çevrilir emin olun. (Örneğin, tipik bir senaryo olan mobil iş gücü) karşılanamıyorsa kullanmak için önerilir [noktadan siteye VPN bağlantıları ve özel bir uç nokta](sql-database-managed-instance-configure-p2s.md).

Azure'dan bağlantıları başladıysa, trafiği gelen bilinen atanan geldiğini önerilir [VIP](../virtual-network/virtual-networks-reserved-public-ip.md) (örneğin, sanal makineler). VIP adresleri yönetme kolaylığı için müşterilerin kullanmayı düşünebilirsiniz [genel IP adresi ön eki](../virtual-network/public-ip-address-prefix.md).