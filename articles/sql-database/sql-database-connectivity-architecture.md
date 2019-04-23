---
title: Azure SQL veritabanı ve SQL veri ambarı için Azure trafiği yönlendiren | Microsoft Docs
description: Bu belge, veritabanı bağlantıları veya azure'daki Azcure SQL onnectivity mimarisini açıklar. Azure dışında.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: carlrab
manager: craigg
ms.date: 04/03/2019
ms.openlocfilehash: 4ff6cc0ba18074f353eb5b99af7052edd658a80e
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59799278"
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

Şirket içi kaynaklardan bir Azure SQL veritabanına bağlanmak için Azure SQL veritabanı ağ geçidi, Azure bölgesi için giden ağ trafiğine izin vermeniz gerekir. Bağlantılarınızı yalnızca içinde bağlanırken bir ağ geçidi üzerinden Git `Proxy` bağlanırken şirket içi kaynaklara bağlandığınızda varsayılan modu.

Aşağıdaki tablo, Azure SQL veritabanı ağ geçidi tüm veri bölgeleri için birincil ve ikincil IP'ler listeler. Bazı bölgeler için iki IP adresi vardır. Bu bölgede, birincil IP adresi geçerli bir ağ geçidi IP adresini ve ikinci IP adresini bir yük devretme IP adresidir. Yük devretme adresi size yüksek hizmet kullanılabilirliğini korumak için sunucunuzun taşıyabilirsiniz adresidir. Bu bölgeler için her iki IP adreslerine giden izin öneririz. İkinci IP adresi Microsoft'a ait ve bağlantıları kabul etmek üzere Azure SQL veritabanı tarafından etkinleştirilinceye kadar tüm hizmetleri dinlemez.

| Bölge Adı | Birincil IP adresi | İkincil IP adresi |
| --- | --- |--- |
| Avustralya Doğu | 13.75.149.87 | 40.79.161.1 |
| Avustralya Güneydoğu | 191.239.192.109 | 13.73.109.251 |
| Güney Brezilya | 104.41.11.5 | |
| Orta Kanada | 40.85.224.249 | |
| Doğu Kanada | 40.86.226.166 | |
| Orta ABD | 23.99.160.139 | 13.67.215.62 |
| Çin Doğu 1 | 139.219.130.35 | |
| Çin Doğu 2 | 40.73.82.1 | |
| Çin Kuzey 1 | 139.219.15.17 | |
| Çin Kuzey 2 | 40.73.50.0 | |
| Doğu Asya | 191.234.2.139 | 52.175.33.150 |
| Doğu ABD 1 | 191.238.6.43 | 40.121.158.30 |
| Doğu ABD 2 | 191.239.224.107 | 40.79.84.180 * |
| Fransa Orta | 40.79.137.0 | 40.79.129.1 |
| Almanya Orta | 51.4.144.100 | |
| Almanya Kuzey Doğu | 51.5.144.179 | |
| Hindistan Orta | 104.211.96.159 | |
| Hindistan Güney | 104.211.224.146 | |
| Hindistan Batı | 104.211.160.80 | |
| Japonya Doğu | 191.237.240.43 | 13.78.61.196 |
| Japonya Batı | 191.238.68.11 | 104.214.148.156 |
| Kore Orta | 52.231.32.42 | |
| Kore Güney | 52.231.200.86 |  |
| Orta Kuzey ABD | 23.98.55.75 | 23.96.178.199 |
| Kuzey Avrupa | 191.235.193.75 | 40.113.93.91 |
| Orta Güney ABD | 23.98.162.75 | 13.66.62.124 |
| Güneydoğu Asya | 23.100.117.95 | 104.43.15.0 |
| Birleşik Krallık Güney | 51.140.184.11 | |
| Birleşik Krallık Batı | 51.141.8.11| |
| Batı Orta ABD | 13.78.145.25 | |
| Batı Avrupa | 191.237.232.75 | 40.68.37.158 |
| Batı ABD 1 | 23.99.34.75 | 104.42.238.205 |
| Batı ABD 2 | 13.66.226.202 | |
||||

\* **NOT:** *Doğu ABD 2* üçüncül bir IP adresi de sahip `52.167.104.0`.

## <a name="change-azure-sql-database-connection-policy"></a>Azure SQL veritabanı bağlantı ilkesini değiştirme

Azure SQL veritabanı sunucusu için Azure SQL veritabanı bağlantı İlkesi değiştirmek için kullanın [conn ilke](https://docs.microsoft.com/cli/azure/sql/server/conn-policy) komutu.

- Bağlantı ilkelerinizi ayarlanırsa `Proxy`, tüm Azure SQL veritabanı ağ geçidi üzerinden paket akışı ağ. Bu ayar için yalnızca Azure SQL veritabanı ağ geçidi IP giden izin vermeniz gerekir. Ayarı kullanarak `Proxy` ayarı üzerinde daha fazla gecikme sahip `Redirect`.
- Bağlantı ilkelerinizi ayarlıyorsanız `Redirect`, tüm veritabanı kümeye doğrudan paket akışı ağ. Bu ayar için birden çok IP'yi giden izin vermeniz gerekir.

## <a name="script-to-change-connection-settings-via-powershell"></a>PowerShell aracılığıyla bağlantı ayarlarını değiştirmek için komut dosyası

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
> [!IMPORTANT]
> Azure Resource Manager PowerShell modülü, Azure SQL veritabanı tarafından hala desteklenmektedir, ancak tüm gelecekteki geliştirme için Az.Sql modüldür. Bu cmdlet'ler için bkz. [Azurerm.SQL'e](https://docs.microsoft.com/powershell/module/AzureRM.Sql/). Az modül ve AzureRm modülleri komutları için bağımsız değişkenler büyük ölçüde aynıdır.

> [!IMPORTANT]
> Bu betik [Azure PowerShell Modülü](/powershell/azure/install-az-ps).

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

Aşağıdaki CLI betiği, bağlantı İlkesi değiştirme gösterir.

```azurecli-interactive
# Get SQL Server ID
sqlserverid=$(az sql server show -n sql-server-name -g sql-server-group --query 'id' -o tsv)

# Set URI
id="$sqlserverid/connectionPolicies/Default"

# Get current connection policy
az resource show --ids $id

# Update connection policy
az resource update --ids $id --set properties.connectionType=Proxy
```

## <a name="next-steps"></a>Sonraki adımlar

- Azure SQL veritabanı sunucusu için Azure SQL veritabanı bağlantı İlkesi değiştirme hakkında daha fazla bilgi için bkz: [conn ilke](https://docs.microsoft.com/cli/azure/sql/server/conn-policy).
- ADO.NET 4.5 veya sonraki bir sürümünü kullanan istemciler için Azure SQL veritabanı bağlantı davranışları hakkında ek bilgi için bkz. [ADO.NET 4.5 için 1433 dışındaki bağlantı noktaları](sql-database-develop-direct-route-ports-adonet-v12.md).
- Genel uygulama geliştirme genel bakış bilgileri için bkz. [SQL veritabanı uygulaması geliştirmeye genel bakış](sql-database-develop-overview.md).
