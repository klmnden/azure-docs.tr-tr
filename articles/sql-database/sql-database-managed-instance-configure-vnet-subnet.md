---
title: Azure SQL veritabanı yönetilen örneği için mevcut bir sanal ağ yapılandırma | Microsoft Docs
description: Bu makalede, bir sanal ağınız ve Azure SQL veritabanı yönetilen örneği dağıtabileceğiniz alt nasıl yapılandırılacağı açıklanır.
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
ms.date: 01/15/2019
ms.openlocfilehash: c4ff12f0c9adcb9943a6e2426eaf2740ba171e39
ms.sourcegitcommit: 43b85f28abcacf30c59ae64725eecaa3b7eb561a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/09/2019
ms.locfileid: "59358837"
---
# <a name="configure-an-existing-virtual-network-for-azure-sql-database-managed-instance"></a>Azure SQL veritabanı yönetilen örneği için mevcut bir sanal ağ yapılandırma

Azure SQL veritabanı yönetilen örneği, bir Azure dağıtılmalıdır [sanal ağ](../virtual-network/virtual-networks-overview.md) ve yalnızca yönetilen örnekler için ayrılmış ağ. Şunlara göre yapılandırdıysanız mevcut bir sanal ağ ve alt kullanabilirsiniz [yönetilen örnek sanal ağ gereksinimleri](sql-database-managed-instance-connectivity-architecture.md#network-requirements).

Aşağıdaki durumlarda birini sizin için geçerliyse, doğrulamak ve bu makalede açıklanan komut dosyası kullanarak ağınıza değiştirin:

- Yine de yapılandırılmamış yeni bir alt ağ var.
- Alt ağ ile hizalanır emin değilseniz [gereksinimleri](sql-database-managed-instance-connectivity-architecture.md#network-requirements).
- Alt ağ hala uyduğundan emin kontrol etmek istediğiniz [ağ gereksinimleri](sql-database-managed-instance-connectivity-architecture.md#network-requirements) sonra değişiklikler yaptınız.

> [!Note]
> Yönetilen bir örneği yalnızca Azure Resource Manager dağıtım modeliyle oluşturulan sanal ağlar oluşturabilirsiniz. Klasik dağıtım modeliyle oluşturulan azure sanal ağlar desteklenmez. Yönergeleri izleyerek alt ağ boyutunu hesaplamak [yönetilen örnekler için alt ağ boyutunu belirlemek](sql-database-managed-instance-determine-size-vnet-subnet.md) makalesi. Alt ağ içindeki kaynakları dağıttıktan sonra yeniden boyutlandıramazsınız.

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

Komut dosyası üç adımda alt hazırlar:

1. Doğrulama: Bu, seçilen sanal ağ ve alt ağ için ağ gereksinimleri yönetilen örneği doğrular.
2. Onaylayın: Bu kullanıcı bir alt ağ yönetilen örnek dağıtımına hazırlanmak için yapılması gereken değişiklik kümesini gösterir. Ayrıca, onay ister.
3. Hazırlayın: Sanal ağ ve alt düzgün şekilde yapılandırır.

## <a name="next-steps"></a>Sonraki adımlar

- Genel bakış için bkz. [yönetilen örnek nedir?](sql-database-managed-instance.md).
- Sanal ağ oluşturma için bir yönetilen örnek oluşturup bir veritabanı bir veritabanı yedeğinden geri gösteren bir öğretici için bkz. [bir Azure SQL veritabanı yönetilen örneği oluşturma](sql-database-managed-instance-get-started.md).
- DNS sorunları için bkz: [özel DNS yapılandırma](sql-database-managed-instance-custom-dns.md).
