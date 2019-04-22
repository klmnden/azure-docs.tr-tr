---
title: Azure SQL veritabanı yönetilen örneği, VNet/alt ağ boyutunu belirlemek | Microsoft Docs
description: Bu konu, Azure SQL veritabanı yönetilen örnekleri dağıtılacağı alt ağ boyutunu hesaplamak açıklar.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: sstein, bonova, carlrab
manager: craigg
ms.date: 02/22/2019
ms.openlocfilehash: 2a10876bc3c9558de29caf9fee2ae0b06ee87f28
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59783858"
---
# <a name="determine-vnet-subnet-size-for-azure-sql-database-managed-instance"></a>Sanal ağ alt ağı boyutunu belirlemek için Azure SQL veritabanı yönetilen örneği

Azure SQL veritabanı yönetilen örneği, bir Azure dağıtılmalıdır [sanal ağ (VNet)](../virtual-network/virtual-networks-overview.md).

Sanal ağ alt ağ içinde dağıtılabilir yönetilen örnek sayısını (alt ağ aralığı) alt ağı boyutuna bağlıdır.

Yönetilen bir örneği oluşturduğunuzda, Azure sanal makineler sağlama sırasında seçtiğiniz katmana bağlı olarak bir dizi ayırır. Bu sanal makineler, alt ağ ile ilişkili olduğundan, bunlar IP adresi gerektirir. Normal işlemler ve hizmet bakım sırasında yüksek kullanılabilirlik sağlamak için Azure ek sanal makineler ayırabilir. Sonuç olarak, alt ağ gerekli IP adresi sayısı bu alt ağdaki yönetilen örnekler sayısından büyüktür.

Tasarıma göre yönetilen örneğe en az 16 IP adresleri bir alt ağda olmalıdır ve en fazla 256 IP adresi kullanıyor olabilirsiniz. Sonuç olarak, alt ağ IP aralıkları tanımlarken/28'i /24 arasındaki bir alt ağ maskelerini kullanabilirsiniz. Ağ maskesi biraz 28 (14 ana ağ başına), bir tek bir genel amaçlı veya iş açısından kritik dağıtımı iyi bir boyut içindir. Maske biraz/27 (30 ana ağ başına), aynı sanal ağda birden fazla bir yönetilen örnek dağıtım için idealdir. Maske bit ayarlarını /26 (62 ana) /24 (254 ana bilgisayardan) ve daha fazla ek yönetilen örnekler desteklemek için sanal ağ dışında ölçeklendirme sağlar.

> [!IMPORTANT]
> Bir alt ağ boyutu 16 IP adresleriyle ile başka yönetilen örneğe ölçek genişletme için sınırlı olası tam düşük düzeyde grup üyeliğidir. Seçme alt ağ ön eki en az/27 veya altında önerilir.

## <a name="determine-subnet-size"></a>Alt ağ boyutunu belirler

Birden çok yönetilen örnek alt ağ içinde dağıtın ve alt ağı boyutuna göre en iyi duruma getirmeyi planlıyorsanız, bir hesaplama oluşturmak için şu parametreleri kullan:

- Azure alt ağdaki beş IP adreslerini, kendi gereksinimleriniz için kullanır.
- Genel amaçlı örneği her iki adres olması gerekir
- Her bir iş açısından kritik örneği dört adres olması gerekir

**Örnek**: Üç genel amaçlı ve iki iş açısından kritik yönetilen örneği planlayın. 5 + 3 * 2 + 2 * 4 = 19 ihtiyacınız anlamına gelir IP adreslerini. IP aralıklarını 2'in gücünü tanımlanan 32 IP aralığı gerekir (2 ^ 5) IP adresi. Bu nedenle, / 27 alt ağ maskesine sahip bir alt ağı ayırmanız gerekir.

> [!IMPORTANT]
> Yukarıda gösterilen hesaplama geliştirmelerle daha eski hale gelir.

## <a name="next-steps"></a>Sonraki adımlar

- Genel bakış için bkz. [yönetilen örnek nedir](sql-database-managed-instance.md).
- Daha fazla bilgi edinin [bağlantı mimarisi yönetilen örneği için](sql-database-managed-instance-connectivity-architecture.md).
- Bkz. nasıl [yönetilen örnekler dağıtacağınız VNet oluşturma](sql-database-managed-instance-create-vnet-subnet.md)
- DNS sorunları için bkz: [özel DNS yapılandırma](sql-database-managed-instance-custom-dns.md)
