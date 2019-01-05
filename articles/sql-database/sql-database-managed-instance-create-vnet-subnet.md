---
title: Azure SQL veritabanı yönetilen örneği, sanal ağ oluşturma | Microsoft Docs
description: Bu konuda, Azure SQL veritabanı yönetilen örneği dağıtabileceğiniz bir sanal ağ (VNet) oluşturmayı açıklar.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: howto
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: bonova, carlrab
manager: craigg
ms.date: 09/20/2018
ms.openlocfilehash: a8000fb26ce5496a9c62ba475b862f8f80adf6b7
ms.sourcegitcommit: 8330a262abaddaafd4acb04016b68486fba5835b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/04/2019
ms.locfileid: "54041773"
---
# <a name="configure-a-vnet-for-azure-sql-database-managed-instance"></a>Azure SQL veritabanı yönetilen örneği için bir sanal ağ yapılandırma

Bu konu, geçerli bir sanal ağ ve Azure SQL veritabanı yönetilen örnekleri dağıtabileceğiniz bir alt ağ oluşturma işlemini açıklar.

Azure SQL veritabanı yönetilen örneği, bir Azure dağıtılmalıdır [sanal ağ (VNet)](../virtual-network/virtual-networks-overview.md). Bu dağıtım aşağıdaki senaryolara olanak tanır:

- Özel IP adresini güvenli hale getirin.
- Bir şirket içi ağdan doğrudan bir yönetilen örneğe bağlanma
- Bağlantılı bir sunucu veya başka bir yönetilen örneğe bağlanma veri deposu şirket
- Azure kaynakları için yönetilen örneğe bağlanma  

  > [!Note]
  > Yapmanız gerekenler [yönetilen örneği için alt ağ boyutunu belirlemek](sql-database-managed-instance-determine-size-vnet-subnet.md) kaynakları içine yerleştirdiğiniz sonra sunet boyutlandırılamaz çünkü ilk örneği dağıtmadan önce.
  > Mevcut bir sanal ağı kullanmayı planlıyorsanız, yönetilen Örneğinize uyum sağlamak için bu ağ yapılandırmasını değiştirmeniz gerekir. Daha fazla bilgi için [yönetilen örneği için var olan sanal ağı değiştirme](sql-database-managed-instance-configure-vnet-subnet.md).

## <a name="create-a-new-virtual-network"></a>Yeni sanal ağ oluştur

Oluşturma ve sanal ağ yapılandırma en kolay yolu, Azure Resource Manager dağıtım şablonu kullanmaktır.

1. Azure Portal’da oturum açın.

2. Kullanım **azure'a Dağıt** düğmesi sanal ağı Azure bulutunda dağıtmak için:

   <a target="_blank" href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-sql-managed-instance-azure-environment%2Fazuredeploy.json" rel="noopener" data-linktype="external"> <img src="http://azuredeploy.net/deploybutton.png" data-linktype="external"> </a>

   Bu düğme, yönetilen örneği dağıtabileceğiniz ağ ortamını yapılandırmak için kullanabileceğiniz bir form açılır.

   > [!Note]
   > Bu Azure Resource Manager şablonu, sanal ağı iki alt ağ ile dağıtır. Adlı bir alt ağ **ManagedInstances** yönetilen örnekler için ayrılmıştır ve diğer alt ağı adlı sırada yol tablosu, önceden yapılandırılmış **varsayılan** yönetilen erişmeli diğer kaynaklar için kullanılır Örneği (örneğin, Azure sanal makineler). Kaldırabilirsiniz **varsayılan** ihtiyacınız yoksa alt ağ.

3. Ağ ortamı yapılandırın. Aşağıdaki formda ağ ortamınızın parametreleri yapılandırabilirsiniz:

![Azure ağı yapılandırma](./media/sql-database-managed-instance-vnet-configuration/create-mi-network-arm.png)

VNet ve alt ağlar adlarını değiştirme ve ağ kaynaklarınıza ilişkili IP aralıklarını ayarlama. "Satın Al" düğmesine basın, sonra bu form oluşturma ve ortamınızı yapılandırın. İki alt ağa gerekmiyorsa, varsayılan silebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- Genel bakış için bkz. [yönetilen örnek nedir](sql-database-managed-instance.md).
- Hakkında bilgi edinin [bağlantı mimarisi yönetilen örneğinde](sql-database-managed-instance-connectivity-architecture.md).
- Temizle nasıl [yönetilen örneği için var olan sanal ağı değiştirme](sql-database-managed-instance-configure-vnet-subnet.md)
- VNet oluşturmak için bir yönetilen örnek oluşturup bir veritabanı bir veritabanı yedeğinden geri gösteren bir öğretici için bkz [Azure SQL veritabanı yönetilen örneği oluşturma](sql-database-managed-instance-get-started.md).
- DNS sorunları için bkz: [özel DNS yapılandırma](sql-database-managed-instance-custom-dns.md)
