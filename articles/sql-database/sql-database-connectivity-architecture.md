---
title: Azure SQL veritabanı ve SQL veri ambarı bağlantı mimarisi | Microsoft Docs
description: Bu belge, Azure SQL bağlantı mimarisi için veritabanı bağlantıları veya azure'daki açıklar Azure dışında.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: rohitnayakmsft
ms.author: rohitna
ms.reviewer: carlrab, vanto
manager: craigg
ms.date: 07/02/2019
ms.openlocfilehash: 8441e64981b7157e91a56124a08c0aa02a9b1db0
ms.sourcegitcommit: 084630bb22ae4cf037794923a1ef602d84831c57
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2019
ms.locfileid: "67537937"
---
# <a name="azure-sql-connectivity-architecture"></a>Azure SQL bağlantı mimarisi

Bu makalede, Azure SQL Örneğiniz için trafiği farklı bileşenleri işlevi nasıl Azure SQL veritabanı ve SQL veri ambarı bağlantı mimarisi de açıklanmaktadır. İçinden Azure bağlanan istemcileri ve Azure dışında bağlanırken istemcileri ile Azure SQL veritabanı veya SQL veri ambarı ağ trafiğini yönlendirmek için bu bağlantı bileşenleri işlevi. Bu makalede ayrıca bağlantı nasıl gerçekleştirildiğini değiştirmek için kod örnekleri ve varsayılan bağlantı ayarlarını değiştirmek için ilgili konuları sağlar.

## <a name="connectivity-architecture"></a>Bağlantı mimarisi

Aşağıdaki diyagram, Azure SQL veritabanı bağlantısı mimarisinin üst düzey bir genel bakış sağlar.

![mimariye genel bakış](./media/sql-database-connectivity-architecture/connectivity-overview.png)

Aşağıdaki adımlar, bir bağlantının bir Azure SQL veritabanına nasıl kurulacağını açıklar:

- İstemciler, 1433 numaralı bağlantı noktasını dinler ve bir genel IP adresine sahip ağ geçidi bağlanır.
- Etkin bağlantı İlkesi, yeniden yönlendirmeleri veya proxy'ler doğru veritabanı küme trafiğine bağlı olarak ağ geçidi.
- Veritabanı içinde küme trafiği için uygun Azure SQL veritabanı iletilir.

## <a name="connection-policy"></a>Bağlantı İlkesi

Azure SQL veritabanı, SQL veritabanı sunucusu bağlantı İlkesi ayarı için aşağıdaki üç seçenekten destekler:

- **Yeniden yönlendirme (önerilen):** İstemciler veritabanını barındıran düğüme doğrudan bağlantı kurar. Bağlantıyı etkinleştirmek için istemcileri ile ağ güvenlik grupları (NSG) kullanarak bölgedeki tüm Azure IP adreslerine giden güvenlik duvarı kuralları izin [hizmet etiketleri](../virtual-network/security-overview.md#service-tags)) bağlantı noktaları 11000 11999, yalnızca Azure SQL veritabanı ağ geçidi IP 1433 numaralı bağlantı noktasında adresleri. Paketler, doğrudan veritabanına gidin olduğundan, gecikme süresi ve aktarım hızı performansı geliştirdik.
- **Proxy:** Bu modda, tüm bağlantılar, Azure SQL veritabanı ağ geçitleri taşınır. Bağlantıyı etkinleştirmek için istemci yalnızca Azure SQL veritabanı ağ geçidi IP adresleri (genellikle iki IP adresi bölge başına) izin giden güvenlik duvarı kurallarınız olmalıdır. Bu modu seçme, daha yüksek gecikme süresi ve iş yükü doğasına bağlı olarak daha düşük aktarım hızı, sonuçlanabilir. Öneririz `Redirect` bağlantı ilkesi üzerine `Proxy` düşük gecikme süresi ve yüksek aktarım hızı için bağlantı ilkesi.
- **Varsayılan:** Açıkça ya da bağlantı İlkesi yapmadığınız sürece bu bağlantı geçerli tüm sunucularda oluşturulduktan sonra ilkedir `Proxy` veya `Redirect`. Etkin ilke olup bağlantıları Azure içinde kaynaklanan üzerinde bağlıdır (`Redirect`) veya Azure dışında (`Proxy`).

## <a name="connectivity-from-within-azure"></a>Azure içinde bağlantısı

Bağlantılarınızı da Azure içinde bağlantı, bağlantı ilkesi varsa `Redirect` varsayılan olarak. Bir ilke `Redirect` TCP oturumu, Azure SQL veritabanına kurulduktan sonra istemci oturumundan sonra doğru veritabanı kümeye hedef sanal IP değişiklik için Azure SQL veritabanı ağ geçidi verilerinden yönlendirildiğini anlamına gelir. Küme. Bundan sonra sonraki tüm paketlere, Azure SQL veritabanı ağ geçidi atlayarak doğrudan kümeye akış. Aşağıdaki diyagram Bu trafik akışını gösterir.

![mimariye genel bakış](./media/sql-database-connectivity-architecture/connectivity-azure.png)

## <a name="connectivity-from-outside-of-azure"></a>Azure dışındaki bağlantısı

Azure dışından bağlanıyorsanız, bağlantılarınızı, bağlantı İlkesi sahip `Proxy` varsayılan olarak. Bir ilke `Proxy` sonraki tüm paketlere akış yoluyla ağ geçidi ve Azure SQL veritabanı ağ geçidi üzerinden TCP oturumun anlamına gelir. Aşağıdaki diyagram Bu trafik akışını gösterir.

![mimariye genel bakış](./media/sql-database-connectivity-architecture/connectivity-onprem.png)

## <a name="azure-sql-database-gateway-ip-addresses"></a>Azure SQL veritabanı ağ geçidi IP adresleri

Aşağıdaki tabloda, IP adresleri, ağ geçitleri bölgeye göre listeler. & Gelen ağ trafiğine izin vermek istediğiniz bir Azure SQL veritabanı'na bağlanmak için **tüm** ağ geçitleri için bölge.

Bundan sonra size her bölgede daha fazla ağ geçitleri ekleyecek ve aşağıdaki tabloda kullanımdan ağ geçidi IP adresi sütununda ağ geçitleri devre dışı bırakma. İşlem aşağıdaki makalede verilen yetki alma hakkında daha fazla ayrıntı için: [Yeni ağ geçitleri için Azure SQL veritabanı trafiği geçişi](sql-database-gateway-migration.md)


| Bölge Adı          | Ağ geçidi IP adresi | Yetkisi alınan bir ağ geçidi </br> IP adresi| İlgili notlar yetkisini alma | 
| --- | --- | --- | --- |
| Avustralya Doğu       | 13.75.149.87, 40.79.161.1 | | |
| Avustralya Güneydoğu | 191.239.192.109, 13.73.109.251 | | |
| Güney Brezilya         | 104.41.11.5        |                 | |
| Orta Kanada       | 40.85.224.249      |                 | |
| Doğu Kanada          | 40.86.226.166      |                 | |
| Orta ABD           | 13.67.215.62, 52.182.137.15 | 23.99.160.139 | 1 Eylül 2019 sonra bağlantı yok |
| Çin Doğu 1         | 139.219.130.35     |                 | |
| Çin Doğu 2         | 40.73.82.1         |                 | |
| Çin Kuzey 1        | 139.219.15.17      |                 | |
| Çin Kuzey 2        | 40.73.50.0         |                 | |
| Doğu Asya            | 191.234.2.139, 52.175.33.150 |       | |
| Doğu ABD 1            | 40.121.158.30, 40.79.153.12 | 191.238.6.43 | 1 Eylül 2019 sonra bağlantı yok |
| Doğu ABD 2            | 40.79.84.180, 52.177.185.181, 52.167.104.0 | 191.239.224.107    | 1 Eylül 2019 sonra bağlantı yok |
| Fransa Orta       | 40.79.137.0, 40.79.129.1 |           | |
| Almanya Orta      | 51.4.144.100       |                 | |
| Almanya Kuzey Doğu   | 51.5.144.179       |                 | |
| Hindistan Orta        | 104.211.96.159     |                 | |
| Hindistan Güney          | 104.211.224.146    |                 | |
| Hindistan Batı           | 104.211.160.80     |                 | |
| Japonya Doğu           | 13.78.61.196, 40.79.184.8, 13.78.106.224 | 191.237.240.43 | 1 Eylül 2019 sonra bağlantı yok |
| Japonya Batı           | 104.214.148.156, 40.74.100.192 | 191.238.68.11 | 1 Eylül 2019 sonra bağlantı yok |
| Kore Orta        | 52.231.32.42       |                 | |
| Kore Güney          | 52.231.200.86      |                 | |
| Orta Kuzey ABD     | 23.96.178.199      | 23.98.55.75     | 1 Eylül 2019 sonra bağlantı yok |
| Kuzey Avrupa         | 40.113.93.91       | 191.235.193.75  | 1 Eylül 2019 sonra bağlantı yok |
| Orta Güney ABD     | 13.66.62.124       | 23.98.162.75    | 1 Eylül 2019 sonra bağlantı yok |
| Güneydoğu Asya      | 104.43.15.0        | 23.100.117.95   | 1 Eylül 2019 sonra bağlantı yok |
| Birleşik Krallık Güney             | 51.140.184.11      |                 | |
| Birleşik Krallık Batı              | 51.141.8.11        |                 | |
| Batı Orta ABD      | 13.78.145.25       |                 | |
| Batı Avrupa          | 191.237.232.75, 40.68.37.158 |       | |
| Batı ABD 1            | 23.99.34.75, 104.42.238.205 |        | |
| Batı ABD 2            | 13.66.226.202      |                 | |
|                      |                    |                 | |

## <a name="change-azure-sql-database-connection-policy"></a>Azure SQL veritabanı bağlantı ilkesini değiştirme

Azure SQL veritabanı sunucusu için Azure SQL veritabanı bağlantı İlkesi değiştirmek için kullanın [conn ilke](https://docs.microsoft.com/cli/azure/sql/server/conn-policy) komutu.

- Bağlantı ilkelerinizi ayarlanırsa `Proxy`, tüm Azure SQL veritabanı ağ geçidi üzerinden paket akışı ağ. Bu ayar için yalnızca Azure SQL veritabanı ağ geçidi IP giden izin vermeniz gerekir. Ayarı kullanarak `Proxy` ayarı üzerinde daha fazla gecikme sahip `Redirect`.
- Bağlantı ilkelerinizi ayarlıyorsanız `Redirect`, tüm veritabanı kümeye doğrudan paket akışı ağ. Bu ayar için birden çok IP'yi giden izin vermeniz gerekir.

## <a name="script-to-change-connection-settings-via-powershell"></a>PowerShell aracılığıyla bağlantı ayarlarını değiştirmek için komut dosyası

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
> [!IMPORTANT]
> Azure Resource Manager PowerShell modülü, Azure SQL veritabanı tarafından hala desteklenmektedir, ancak tüm gelecekteki geliştirme için Az.Sql modüldür. Bu cmdlet'ler için bkz. [Azurerm.SQL'e](https://docs.microsoft.com/powershell/module/AzureRM.Sql/). Az modül ve AzureRm modülleri komutları için bağımsız değişkenler büyük ölçüde aynıdır. Aşağıdaki komut dosyası gerektirir [Azure PowerShell Modülü](/powershell/azure/install-az-ps).

Aşağıdaki PowerShell betiğini bağlantı ilkesini değiştirme işlemi gösterilmektedir.

```powershell
# Get SQL Server ID
$sqlserverid=(Get-AzSqlServer -ServerName sql-server-name -ResourceGroupName sql-server-group).ResourceId

# Set URI
$id="$sqlserverid/connectionPolicies/Default"

# Get current connection policy
(Get-AzResource -ResourceId $id).Properties.connectionType

# Update connection policy
Set-AzResource -ResourceId $id -Properties @{"connectionType" = "Proxy"} -f
```

## <a name="script-to-change-connection-settings-via-azure-cli"></a>Azure CLI aracılığıyla bağlantı ayarlarını değiştirmek için komut dosyası

> [!IMPORTANT]
> Bu betik [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli).

### <a name="azure-cli-in-a-bash-shell"></a>Bir bash Kabuğu'nda Azure CLI

> [!IMPORTANT]
> Bu betik [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli).

Aşağıdaki CLI betiği, bir bash kabuğunda bağlantı İlkesi değiştirme gösterir.

```azurecli-interactive
# Get SQL Server ID
sqlserverid=$(az sql server show -n sql-server-name -g sql-server-group --query 'id' -o tsv)

# Set URI
ids="$sqlserverid/connectionPolicies/Default"

# Get current connection policy
az resource show --ids $ids

# Update connection policy
az resource update --ids $ids --set properties.connectionType=Proxy
```

### <a name="azure-cli-from-a-windows-command-prompt"></a>Azure CLI'yı bir Windows komut isteminden

> [!IMPORTANT]
> Bu betik [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli).

Aşağıdaki CLI betiği, nasıl bağlantı İlkesi, bir Windows Komut İstemi'nden (Azure yüklü CLI ile) değiştirileceğini gösterir.

```azurecli
# Get SQL Server ID and set URI
FOR /F "tokens=*" %g IN ('az sql server show --resource-group myResourceGroup-571418053 --name server-538465606 --query "id" -o tsv') do (SET sqlserverid=%g/connectionPolicies/Default)

# Get current connection policy
az resource show --ids %sqlserverid%

# Update connection policy
az resource update --ids %sqlserverid% --set properties.connectionType=Proxy
```

## <a name="next-steps"></a>Sonraki adımlar

- Azure SQL veritabanı sunucusu için Azure SQL veritabanı bağlantı İlkesi değiştirme hakkında daha fazla bilgi için bkz: [conn ilke](https://docs.microsoft.com/cli/azure/sql/server/conn-policy).
- ADO.NET 4.5 veya sonraki bir sürümünü kullanan istemciler için Azure SQL veritabanı bağlantı davranışları hakkında ek bilgi için bkz. [ADO.NET 4.5 için 1433 dışındaki bağlantı noktaları](sql-database-develop-direct-route-ports-adonet-v12.md).
- Genel uygulama geliştirme genel bakış bilgileri için bkz. [SQL veritabanı uygulaması geliştirmeye genel bakış](sql-database-develop-overview.md).