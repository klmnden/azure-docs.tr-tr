---
title: Azure SQL veritabanı yönetilen mevcut VNET/alt ağ yapılandırma örneği | Microsoft Docs
description: Bu konu, mevcut bir sanal ağ (VNet) yapılandırmak açıklar ve Azure SQL veritabanı yönetilen örneği dağıtabileceğiniz bir alt ağ.
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
ms.openlocfilehash: 53aba5192ddf57598965fcfe0db5f2b18423c7e9
ms.sourcegitcommit: edacc2024b78d9c7450aaf7c50095807acf25fb6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/13/2018
ms.locfileid: "53346608"
---
# <a name="configure-an-existing-vnet-for-azure-sql-database-managed-instance"></a>Azure SQL veritabanı yönetilen örneği için mevcut bir VNet yapılandırma

Azure SQL veritabanı yönetilen örneği, bir Azure dağıtılmalıdır [sanal ağ (VNet)](../virtual-network/virtual-networks-overview.md) ve yalnızca yönetilen örnekler için ayrılmış ağ. Şunlara göre yapılandırdıysanız mevcut bir VNet ve alt ağı kullanabilirsiniz [yönetilen örnek sanal ağ gereksinimleri](sql-database-managed-instance-connectivity-architecture.md#network-requirements). 

Yine de yapılandırılmamış yeni bir alt ağ varsa, emin olmadığınız bir alt ağ ile hizalanır [gereksinimleri](sql-database-managed-instance-connectivity-architecture.md#network-requirements), ya da kontrol etmek istediğiniz alt ağı ile hala uyumlu olup [ağ gereksinimleri](sql-database-managed-instance-connectivity-architecture.md#network-requirements) sonra yaptığınız bazı değişiklikler, doğrulamak ve bu bölümde açıklanan betik kullanarak ağınıza değiştirin. 

  > [!Note]
  > Bu gibi durumlarda, bir yönetilen örneği yalnızca Resource Manager sanal ağları oluşturabilirsiniz. Klasik dağıtım modeli kullanılarak dağıtılan azure sanal ağları vnet'i değildir. Yönergeleri izleyerek alt ağ boyutunu hesaplamak emin [yönetilen örnekler için alt ağ boyutunu belirlemek](#determine-the-size-of-subnet-for-managed-instances) alt ağ içindeki kaynakları dağıtmak sonra boyutlandırılamaz olduğundan bölüm.

## <a name="validate-and-modify-an-existing-virtual-network"></a>Doğrulama ve var olan bir sanal ağı değiştirme 

Var olan bir alt ağ içinde bir yönetilen örnek oluşturmak istiyorsanız, alt hazırlamak için aşağıdaki PowerShell betiğini öneririz:
```powershell
$scriptUrlBase = 'https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/manage/azure-sql-db-managed-instance/prepare-subnet'

$parameters = @{
    subscriptionId = '<subscriptionId>'
    resourceGroupName = '<resourceGroupName>'
    virtualNetworkName = '<virtualNetworkName>'
    subnetName = '<subnetName>'
    }

Invoke-Command -ScriptBlock ([Scriptblock]::Create((iwr ($scriptUrlBase+'/prepareSubnet.ps1?t='+ [DateTime]::Now.Ticks)).Content)) -ArgumentList $parameters
```
Alt ağ hazırlama üç basit adımda gerçekleştirilir:

1. Validate - seçilen sanal ağ ve alt ağ için ağ gereksinimleri örneği yönetilen doğrulanır.
2. Onayla - kullanıcı bir alt ağ yönetilen örnek dağıtımına hazırlanmak için yapılan ve onaylarının sorulan gereken değişiklik kümesini gösterilir.
3. Hazırlama - sanal ağ ve alt düzgün bir şekilde yapılandırılır.

## <a name="next-steps"></a>Sonraki adımlar

- Genel bakış için bkz. [yönetilen örnek nedir](sql-database-managed-instance.md)
- VNet oluşturmak için bir yönetilen örnek oluşturup bir veritabanı bir veritabanı yedeğinden geri gösteren bir öğretici için bkz [Azure SQL veritabanı yönetilen örneği oluşturma](sql-database-managed-instance-get-started.md).
- DNS sorunları için bkz: [özel DNS yapılandırma](sql-database-managed-instance-custom-dns.md).
