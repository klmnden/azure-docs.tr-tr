---
title: Yapılandırma genel uç nokta - Azure SQL veritabanı yönetilen örneği | Microsoft Docs
description: Yönetilen örnek için ortak bir uç nokta yapılandırma hakkında bilgi edinin
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: vanto, carlrab
manager: craigg
ms.date: 05/07/2019
ms.openlocfilehash: d3e68a5287e59c576f85491e6e5eba33fac080ca
ms.sourcegitcommit: e6d53649bfb37d01335b6bcfb9de88ac50af23bd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/09/2019
ms.locfileid: "65465143"
---
# <a name="configure-public-endpoint-in-azure-sql-database-managed-instance"></a>Azure SQL veritabanı yönetilen örneği'nde genel uç nokta yapılandırma

Genel uç noktası için bir [yönetilen örnek](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-index) yönetilen Örneğinize dışında veri erişimi sağlayan [sanal ağ](../virtual-network/virtual-networks-overview.md). Power BI, Azure App Service veya şirket içi ağa gibi Azure hizmetlerinden çok kiracılı yönetilen Örneğinize erişmek kullanabilirsiniz. Yönetilen örneğinde genel uç noktasını kullanarak VPN aktarım hızını sorunları önlemeye yardımcı olmak bir VPN kullanmanız gerekmez.

Bu makalede, öğreneceksiniz nasıl yapılır:

> [!div class="checklist"]
> - Azure portalında yönetilen Örneğiniz için genel bir uç nokta etkinleştir
> - PowerShell kullanarak yönetilen Örneğiniz için ortak bir uç noktayı etkinleştirme
> - Yönetilen örnek genel uç nokta trafiğe izin vermek için yönetilen örnek ağ güvenlik grubu yapılandırma
> - Yönetilen örnek genel uç nokta bağlantı dizesini alın

## <a name="permissions"></a>İzinler

Yönetilen bir örneği olan verilerin duyarlılık nedeniyle, yönetilen örnek genel uç nokta yapılandırmasını iki adımlı bir işlem gerektirir. Bu güvenlik önlemi ayrımı (Çim kaplama) için uyar:

- Yönetilen örnek genel uç noktada etkinleştirme yönetilen örnek Yöneticisi tarafından yapılmalıdır Yönetilen örnek Yöneticisi bulunabilir **genel bakış** sayfasında, SQL yönetilen örneği kaynağı.
- Bir ağ yöneticisi tarafından yapılması gereken bir ağ güvenlik grubu kullanarak trafiğe izin vermek Daha fazla bilgi için [ağ güvenlik grubu izinlerini](../virtual-network/manage-network-security-group.md#permissions).

## <a name="enabling-public-endpoint-for-a-managed-instance-in-the-azure-portal"></a>Azure portalında bir yönetilen örnek için genel bir uç nokta etkinleştirme

1. Adresinden Azure portalında başlatın <https://portal.azure.com/.>
1. Yönetilen örnek ile kaynak grubunu açın ve seçin **SQL yönetilen örneği** üzerinde genel uç noktası yapılandırmak istediğiniz.
1. Üzerinde **güvenlik** ayarları, select **sanal ağ** sekmesi.
1. Sanal ağ yapılandırma sayfasında seçin **etkinleştirme** ve ardından **Kaydet** yapılandırmasını güncelleştirmek için simge.

![mı vnet config.png](media/sql-database-managed-instance-public-endpoint-configure/mi-vnet-config.png)

## <a name="enabling-public-endpoint-for-a-managed-instance-using-powershell"></a>PowerShell kullanarak bir yönetilen örnek için genel bir uç nokta etkinleştirme

### <a name="enable-public-endpoint"></a>Genel bir uç noktayı etkinleştirme

Aşağıdaki PowerShell komutlarını çalıştırın. Değiştirin **subscrıptıon-ID** abonelik kimliğinizi Ayrıca değiştirin **rg adı** kaynak grubuyla değiştirin ve yönetilen örnek için **mı adı** yönetilen örneğinizin adı.

```powershell
Install-Module -Name Az

Import-Module Az.Accounts
Import-Module Az.Sql

Connect-AzAccount

# Use your subscription ID in place of subscription-id below

Select-AzSubscription -SubscriptionId {subscription-id}

# Replace rg-name with the resource group for your managed instance, and replace mi-name with the name of your managed instance

$mi = Get-AzSqlInstance -ResourceGroupName {rg-name} -Name {mi-name}

$mi = $mi | Set-AzSqlInstance -PublicDataEndpointEnabled $true -force
```

### <a name="disable-public-endpoint"></a>Genel bir uç noktayı devre dışı

PowerShell kullanarak genel uç noktası devre dışı bırakmak için aşağıdaki komutu yürüterek (ve NSG gelen bağlantı noktası 3342 için yapılandırdıysanız, kapatmak de unutmayın):

```powershell
Set-AzSqlInstance -PublicDataEndpointEnabled $false -force
```

## <a name="allow-public-endpoint-traffic-on-the-network-security-group"></a>Genel bir uç nokta trafiği üzerinde ağ güvenlik grubu izin ver

1. Yönetilen örnek hala açık yapılandırma sayfasında varsa gidin **genel bakış** sekmesi. Aksi takdirde, geri dönüp, **SQL yönetilen örneği** kaynak. Seçin **sanal ağ/alt ağ** bağlantı, sanal ağ yapılandırma sayfasına götürür.

    ![mi-overview.png](media/sql-database-managed-instance-public-endpoint-configure/mi-overview.png)

1. Seçin **alt ağlar** sanal ağınızın yapılandırma sol bölmede sekme ve Not **güvenlik grubu** yönetilen Örneğiniz için.

    ![mi-vnet-subnet.png](media/sql-database-managed-instance-public-endpoint-configure/mi-vnet-subnet.png)

1. Yönetilen örneğinizi içeren, kaynak grubuna geri dönün. Görmelisiniz **ağ güvenlik grubu** adı belirtildiği üstünde. Ağ güvenlik grubu yapılandırma sayfasına gitmek için bir ad seçin.

1. Seçin **gelen güvenlik kuralları** sekmesinde ve **Ekle** daha yüksek önceliğe sahip kural **deny_all_inbound** kuralını aşağıdaki ayarlarla: </br> </br>

    |Ayar  |Önerilen değer  |Açıklama  |
    |---------|---------|---------|
    |**Kaynak**     |Herhangi bir IP adresi veya hizmet etiketi         |<ul><li>Power BI gibi Azure Hizmetleri için Azure bulut hizmeti etiketi seçin</li> <li>Bilgisayar veya Azure VM için NAT IP adresini kullanın</li></ul> |
    |**Kaynak bağlantı noktası aralıkları**     |*         |Bu şekilde bırakın * (herhangi bir) olarak kaynak bağlantı noktaları genellikle dinamik olarak ayrılan ve gibi bu tür, beklenmedik |
    |**Hedef**     |Herhangi         |Bırakma hedefi olarak herhangi bir yönetilen örnek alt ağa trafiğe izin vermek için |
    |**Hedef bağlantı noktası aralıkları**     |3342         |Yönetilen örnek genel TDS uç noktası olan kapsam hedef bağlantı noktasına 3342, |
    |**Protokolü**     |TCP         |Örneğinin kullandığı TCP protokolü için TDS yönetilen |
    |**Eylem**     |İzin ver         |Yönetilen örnek genel uç noktası üzerinden gelen trafiğe izin vermek |
    |**Öncelik**     |1300         |Bu kural, daha yüksek bir öncelik olduğundan emin olun **deny_all_inbound** kuralı |

    ![mi-nsg-rules.png](media/sql-database-managed-instance-public-endpoint-configure/mi-nsg-rules.png)

    > [!NOTE]
    > Bağlantı noktası 3342 bağlantı, yönetilen örnek ve bu noktada değiştirilemez genel uç noktası için kullanılır.

## <a name="obtaining-the-managed-instance-public-endpoint-connection-string"></a>Yönetilen örnek genel uç nokta bağlantı dizesini alma

1. Genel uç noktası için etkin SQL yönetilen örnek yapılandırma sayfasına gidin. Seçin **bağlantı dizeleri** sekmesinde altında **ayarları** yapılandırma.
1. Genel bir uç nokta ana bilgisayar adı biçiminde < mi_name > geldiğini unutmayın. **genel**. < dns_zone >. database.windows.net ve bağlantı için kullanılan bağlantı noktası 3342 olduğunu.

    ![mi-public-endpoint-conn-string.png](media/sql-database-managed-instance-public-endpoint-configure/mi-public-endpoint-conn-string.png)

## <a name="next-steps"></a>Sonraki adımlar

- Hakkında bilgi edinin [kullanarak Azure SQL veritabanı yönetilen örnek genel uç noktası ile güvenli bir şekilde](sql-database-managed-instance-public-endpoint-securely.md).