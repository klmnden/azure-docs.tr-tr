---
title: Azure SQL veritabanı bağlantı mimarisi | Microsoft Docs
description: Bu belge, Azure SQLDB bağlantı mimariden veya azure'daki açıklar Azure dışında.
services: sql-database
author: DhruvMsft
manager: craigg
ms.service: sql-database
ms.custom: DBs & servers
ms.topic: conceptual
ms.date: 01/24/2018
ms.author: dhruv
ms.openlocfilehash: 8159a9eb8d8829ed01609cebc3ae41713892f6cf
ms.sourcegitcommit: ab9514485569ce511f2a93260ef71c56d7633343
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/15/2018
ms.locfileid: "45630195"
---
# <a name="azure-sql-database-connectivity-architecture"></a>Azure SQL veritabanı bağlantı mimarisi 

Bu makale, Azure SQL veritabanı bağlantı mimarisi açıklamakta ve nasıl Azure SQL veritabanı örneğiniz trafiği yönlendirmek için farklı bileşenleri işlev açıklar. Ağ trafiği için Azure veritabanı Azure dışında bağlanırken istemcilerle içinden Azure bağlanan istemcileri ile yönlendirmek için bu Azure SQL veritabanı bağlantısı bileşenleri işlevi. Bu makalede ayrıca bağlantı nasıl gerçekleştirildiğini değiştirmek için kod örnekleri ve varsayılan bağlantı ayarlarını değiştirmek için ilgili konuları sağlar. 

## <a name="connectivity-architecture"></a>Bağlantı mimarisi

Aşağıdaki diyagram, Azure SQL veritabanı bağlantısı mimarisinin üst düzey bir genel bakış sağlar.

![mimariye genel bakış](./media/sql-database-connectivity-architecture/architecture-overview.png)


Aşağıdaki adımlar, Azure SQL veritabanı yazılım yük dengeleyici (SLB) ve Azure SQL veritabanı ağ geçidi aracılığıyla Azure SQL veritabanı için bir bağlantının nasıl kurulacağını açıklar.

- İstemciler, azure'da veya Azure dışındaki ortak IP adresi ve bağlantı noktası 1433'ü dinler SLB bağlanır.
- SLB, Azure SQL veritabanı ağ geçidi trafiği yönlendirir.
- Ağ geçidi doğru proxy Ara yazılımıyla trafiği yönlendirir.
- Proxy ara yazılım için uygun Azure SQL veritabanı trafiği yönlendirir.

> [!IMPORTANT]
> Bu bileşenlerin her birinin reddi (DDoS) hizmeti koruma ağ ve uygulama katmanı yerleşik dağıtılmış.
>

## <a name="connectivity-from-within-azure"></a>Azure içinde bağlantısı

Azure içinde gelen bağlanıyorsanız, bağlantı İlkesi bağlantılarınızı sahip **yeniden yönlendirme** varsayılan olarak. Bir ilke **yeniden yönlendirme** TCP oturumu, Azure SQL veritabanı için istemci oturum kurulduktan sonra bağlantıları sonra yeniden yönlendirilirse, proxy ara yazılım için hedef sanal IP değişiklik Azure verilerinden anlamına gelir. SQL veritabanı ağ geçidi, proxy ara yazılım. Bundan sonra Azure SQL veritabanı ağ geçidi atlayarak doğrudan proxy ara yazılımı üzerinden, sonraki tüm paketlere akış. Aşağıdaki diyagram Bu trafik akışını gösterir.

![mimariye genel bakış](./media/sql-database-connectivity-architecture/connectivity-from-within-azure.png)

## <a name="connectivity-from-outside-of-azure"></a>Azure dışındaki bağlantısı

Azure dışından bağlanıyorsanız, bağlantılarınızı, bağlantı İlkesi sahip **Proxy** varsayılan olarak. Bir ilke **Proxy** sonraki tüm paketlere akış yoluyla ağ geçidi ve Azure SQL veritabanı ağ geçidi üzerinden TCP oturumun anlamına gelir. Aşağıdaki diyagram Bu trafik akışını gösterir.

![mimariye genel bakış](./media/sql-database-connectivity-architecture/connectivity-from-outside-azure.png)

> [!IMPORTANT]
> Hizmet uç noktaları Azure SQL veritabanı ile ilkenizi kullanıldığında **Proxy** varsayılan olarak. Bağlantısı, sanal ağ içinde etkinleştirmek için aşağıdaki listede belirtilen Azure SQL veritabanı ağ geçidi IP adreslerine giden bağlantılara izin verin. Hizmet uç noktaları kullanırken bağlantı ilkelerinizi değiştirme öneririz **yeniden yönlendirme** daha iyi performans sağlamak. Bağlantı ilkelerinizi değiştirirseniz **yeniden yönlendirme** olmayacaktır NSG IP'ler, aşağıda listelenen Azure SQLDB ağ geçidi üzerinde giden izin vermek için yeterli, giden tüm Azure SQLDB IP'lere izin vermelidir. Bu NSG (ağ güvenlik grupları) hizmet etiketleri yardımıyla gerçekleştirilebilir. Daha fazla bilgi için [hizmet etiketleri](https://docs.microsoft.com/azure/virtual-network/security-overview#service-tags).

## <a name="azure-sql-database-gateway-ip-addresses"></a>Azure SQL veritabanı ağ geçidi IP adresleri

Şirket içi kaynaklardan bir Azure SQL veritabanına bağlanmak için Azure SQL veritabanı ağ geçidi, Azure bölgesi için giden ağ trafiğine izin vermeniz gerekir. Bağlantılarınızı yalnızca ağ geçidi üzerinden şirket içi kaynaklardan bağlanırken varsayılan değer olan ara sunucu modunda bağlanırken gidin.

Aşağıdaki tablo, Azure SQL veritabanı ağ geçidi tüm veri bölgeleri için birincil ve ikincil IP'ler listeler. Bazı bölgeler için iki IP adresi vardır. Bu bölgede, birincil IP adresi geçerli bir ağ geçidi IP adresini ve ikinci IP adresini bir yük devretme IP adresidir. Yük devretme adresi size yüksek hizmet kullanılabilirliğini korumak için sunucunuzun taşıyabilirsiniz adresidir. Bu bölgeler için her iki IP adreslerine giden izin öneririz. İkinci IP adresi Microsoft'a ait ve bağlantıları kabul etmek üzere Azure SQL veritabanı tarafından etkinleştirilinceye kadar tüm hizmetleri dinlemez.

| Bölge Adı | Birincil IP adresi | İkincil IP adresi |
| --- | --- |--- |
| Avustralya Doğu | 191.238.66.109 | 13.75.149.87 |
| Avustralya Güneydoğu | 191.239.192.109 | 13.73.109.251 |
| Güney Brezilya | 104.41.11.5 | |
| Orta Kanada | 40.85.224.249 | |
| Doğu Kanada | 40.86.226.166 | |
| Orta ABD | 23.99.160.139 | 13.67.215.62 |
| Doğu Asya | 191.234.2.139 | 52.175.33.150 |
| Doğu ABD 1 | 191.238.6.43 | 40.121.158.30 |
| Doğu ABD 2 | 191.239.224.107 | 40.79.84.180 * |
| Hindistan Orta | 104.211.96.159  | |
| Hindistan Güney | 104.211.224.146  | |
| Hindistan Batı | 104.211.160.80 | |
| Japonya Doğu | 191.237.240.43 | 13.78.61.196 |
| Japonya Batı | 191.238.68.11 | 104.214.148.156 |
| Kore Orta | 52.231.32.42 | |
| Kore Güney | 52.231.200.86 |  |
| Orta Kuzey ABD | 23.98.55.75 | 23.96.178.199 |
| Kuzey Avrupa | 191.235.193.75 | 40.113.93.91 |
| Orta Güney ABD | 23.98.162.75 | 13.66.62.124 |
| Güneydoğu Asya | 23.100.117.95 | 104.43.15.0 |
| UK Kuzey | 13.87.97.210 | |
| UK Güney 1 | 51.140.184.11 | |
| UK Güney 2 | 13.87.34.7 | |
| Birleşik Krallık Batı | 51.141.8.11  | |
| Batı Orta ABD | 13.78.145.25 | |
| Batı Avrupa | 191.237.232.75 | 40.68.37.158 |
| Batı ABD 1 | 23.99.34.75 | 104.42.238.205 |
| Batı ABD 2 | 13.66.226.202  | |
||||

\* **Not:** *Doğu ABD 2* üçüncül bir IP adresi de sahip `52.167.104.0`.

## <a name="change-azure-sql-database-connection-policy"></a>Azure SQL veritabanı bağlantı ilkesini değiştirme

Azure SQL veritabanı sunucusu için Azure SQL veritabanı bağlantı İlkesi değiştirmek için kullanın [conn ilke](https://docs.microsoft.com/cli/azure/sql/server/conn-policy) komutu.

- Bağlantı ilkelerinizi ayarlanırsa **Proxy**, tüm Azure SQL veritabanı ağ geçidi üzerinden paket akışı ağ. Bu ayar için yalnızca Azure SQL veritabanı ağ geçidi IP giden izin vermeniz gerekir. Ayarı kullanarak **Proxy** ayarı üzerinde daha fazla gecikme sahip **yeniden yönlendirme**.
- Bağlantı ilkelerinizi ayarlıyorsanız **yeniden yönlendirme**, tüm ağ ara yazılım proxy doğrudan paket akışı. Bu ayar için birden çok IP'yi giden izin vermeniz gerekir.

## <a name="script-to-change-connection-settings-via-powershell"></a>PowerShell aracılığıyla bağlantı ayarlarını değiştirmek için komut dosyası

> [!IMPORTANT]
> Bu betik [Azure PowerShell Modülü](/powershell/azure/install-azurerm-ps).
>

Aşağıdaki PowerShell betiğini bağlantı ilkesini değiştirme işlemi gösterilmektedir.

```powershell
Connect-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName <Subscription Name>

# Azure Active Directory ID
$tenantId = "<Azure Active Directory GUID>"
$authUrl = "https://login.microsoftonline.com/$tenantId"

# Subscription ID
$subscriptionId = "<Subscription GUID>"

# Create an App Registration in Azure Active Directory.  Ensure the application type is set to NATIVE
# Under Required Permissions, add the API:  Windows Azure Service Management API

# Specify the redirect URL for the app registration
$uri = "<NATIVE APP - REDIRECT URI>"

# Specify the application id for the app registration
$clientId = "<NATIVE APP - APPLICATION ID>"

# Logical SQL Server Name
$serverName = "<LOGICAL DATABASE SERVER - NAME>"

# Resource Group where the SQL Server is located
$resourceGroupName= "<LOGICAL DATABASE SERVER - RESOURCE GROUP NAME>"


# Login and acquire a bearer token
$AuthContext = [Microsoft.IdentityModel.Clients.ActiveDirectory.AuthenticationContext]$authUrl
$result = $AuthContext.AcquireToken(
"https://management.core.windows.net/",
$clientId,
[Uri]$uri,
[Microsoft.IdentityModel.Clients.ActiveDirectory.PromptBehavior]::Auto
)

$authHeader = @{
'Content-Type'='application\json; '
'Authorization'=$result.CreateAuthorizationHeader()
}

#Get current connection Policy
Invoke-RestMethod -Uri "https://management.azure.com/subscriptions/$subscriptionId/resourceGroups/$resourceGroupName/providers/Microsoft.Sql/servers/$serverName/connectionPolicies/Default?api-version=2014-04-01-preview" -Method GET -Headers $authHeader

#Set connection policy to Proxy
$connectionType="Proxy" <#Redirect / Default are other options#>
$body = @{properties=@{connectionType=$connectionType}} | ConvertTo-Json

# Apply Changes
Invoke-RestMethod -Uri "https://management.azure.com/subscriptions/$subscriptionId/resourceGroups/$resourceGroupName/providers/Microsoft.Sql/servers/$serverName/connectionPolicies/Default?api-version=2014-04-01-preview" -Method PUT -Headers $authHeader -Body $body -ContentType "application/json"
```

## <a name="script-to-change-connection-settings-via-azure-cli-20"></a>Azure CLI 2.0 aracılığıyla bağlantı ayarlarını değiştirmek için komut dosyası

> [!IMPORTANT]
> Bu betik [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).
>

Aşağıdaki CLI betiği, bağlantı İlkesi değiştirme gösterir.

<pre>
# Get SQL Server ID
sqlserverid=$(az sql server show -n <b>sql-server-name</b> -g <b>sql-server-group</b> --query 'id' -o tsv)

# Set URI
id="$sqlserverid/connectionPolicies/Default"

# Get current connection policy 
az resource show --ids $id

# Update connection policy 
az resource update --ids $id --set properties.connectionType=Proxy

</pre>

## <a name="next-steps"></a>Sonraki adımlar

- Azure SQL veritabanı sunucusu için Azure SQL veritabanı bağlantı İlkesi değiştirme hakkında daha fazla bilgi için bkz: [conn ilke](https://docs.microsoft.com/cli/azure/sql/server/conn-policy).
- ADO.NET 4.5 veya sonraki bir sürümünü kullanan istemciler için Azure SQL veritabanı bağlantı davranışları hakkında ek bilgi için bkz. [ADO.NET 4.5 için 1433 dışındaki bağlantı noktaları](sql-database-develop-direct-route-ports-adonet-v12.md).
- Genel uygulama geliştirme genel bakış bilgileri için bkz. [SQL veritabanı uygulaması geliştirmeye genel bakış](sql-database-develop-overview.md).
