---
title: Azure SQL veritabanı yönetilen örneği için bir sanal ağ oluşturma | Microsoft Docs
description: Bu makalede, Azure SQL veritabanı yönetilen örneği dağıtabileceğiniz bir sanal ağ oluşturmayı açıklar.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: howto
author: WenJason
ms.author: v-jay
ms.reviewer: bonova, carlrab
manager: craigg
origin.date: 01/15/2019
ms.date: 02/25/2019
ms.openlocfilehash: 5e8b385d018482d281153f1cf80f9953cb8c7f06
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60700499"
---
# <a name="create-a-virtual-network-for-azure-sql-database-managed-instance"></a>Azure SQL veritabanı yönetilen örneği için bir sanal ağ oluşturma

Bu makalede, geçerli sanal ağ ve alt ağ Azure SQL veritabanı yönetilen örneği dağıtabileceğiniz nasıl oluşturulacağı açıklanmaktadır.

Azure SQL veritabanı yönetilen örneği, bir Azure dağıtılmalıdır [sanal ağ](../virtual-network/virtual-networks-overview.md). Bu dağıtım aşağıdaki senaryolara olanak tanır:

- Güvenli özel IP adresi
- Bir şirket içi ağdan doğrudan bir yönetilen örneğe bağlanma
- Bağlantılı bir sunucu veya başka bir yönetilen örneğe bağlanma veri deposu şirket
- Azure kaynakları için yönetilen örneğe bağlanma  

> [!Note]
> Yapmanız gerekenler [yönetilen örneği için alt ağ boyutunu belirlemek](sql-database-managed-instance-determine-size-vnet-subnet.md) ilk örneği dağıtmadan önce. Kaynakları içine yerleştirdiğiniz sonra alt yeniden boyutlandıramazsınız.
>
> Mevcut bir sanal ağı kullanmayı planlıyorsanız, yönetilen Örneğinize uyum sağlamak için bu ağ yapılandırmasını değiştirmeniz gerekir. Daha fazla bilgi için [yönetilen örneği için bir sanal ağınız değiştirme](sql-database-managed-instance-configure-vnet-subnet.md).

## <a name="create-a-virtual-network"></a>Sanal ağ oluşturma

Oluşturma ve bir sanal ağ yapılandırma en kolay yolu, bir Azure Resource Manager dağıtım şablonu kullanmaktır.

1. Azure Portal’da oturum açın.

2. Seçin **azure'a Dağıt** düğmesi:

   <a target="_blank" href="https://portal.azure.cn/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-sql-managed-instance-azure-environment%2Fazuredeploy.json" rel="noopener" data-linktype="external"> <img src="http://azuredeploy.net/deploybutton.png" data-linktype="external"> </a>

   Bu düğme, yönetilen örneği dağıtabileceğiniz ağ ortamını yapılandırmak için kullanabileceğiniz bir formu açılır.

   > [!Note]
   > Bu Azure Resource Manager şablonu bir sanal ağda iki alt ağ dağıtırsınız. Adlı bir alt ağ **ManagedInstances**, yönetilen örneği için ayrılmıştır ve bir önceden yapılandırılmış bir yönlendirme tablosu vardır. Diğer alt, **varsayılan**, yönetilen örneği (örneğin, Azure sanal makineler) erişmeli diğer kaynaklar için kullanılır.

3. Ağ ortamını yapılandırın. Aşağıdaki formda ağ ortamınızın parametreleri yapılandırabilirsiniz:

   ![Azure ağı yapılandırmak için resource Manager şablonu](./media/sql-database-managed-instance-vnet-configuration/create-mi-network-arm.png)

   Sanal ağ ve alt ağlar adlarını değiştirmek ve, ağ kaynakları ile ilişkili IP aralıklarını ayarlama. Seçtikten sonra **satın alma** düğmesi, bu form oluşturma ve ortamınızı yapılandırın. İki alt ağa gerekmiyorsa, varsayılan silebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- Genel bakış için bkz. [yönetilen örnek nedir?](sql-database-managed-instance.md).
- Hakkında bilgi edinin [bağlantı mimarisi yönetilen örneğinde](sql-database-managed-instance-connectivity-architecture.md).
- Bilgi nasıl [yönetilen örneği için bir sanal ağınız değiştirme](sql-database-managed-instance-configure-vnet-subnet.md).
- Sanal ağ oluşturma için bir yönetilen örnek oluşturup bir veritabanı bir veritabanı yedeğinden geri gösteren bir öğretici için bkz. [bir Azure SQL veritabanı yönetilen örneği oluşturma](sql-database-managed-instance-get-started.md).
- DNS sorunları için bkz: [özel DNS yapılandırma](sql-database-managed-instance-custom-dns.md).
