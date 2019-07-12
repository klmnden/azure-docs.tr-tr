---
title: SQL FQDN ile Azure güvenlik duvarı uygulama kurallarını yapılandırma
description: Bu makalede, Azure güvenlik duvarı uygulama kurallarında SQL FQDN yapılandırma öğrenin.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: article
ms.date: 7/11/2019
ms.author: victorh
ms.openlocfilehash: e188a5dda8f936ad369aa2b9222bc726bb0d6a5e
ms.sourcegitcommit: 1572b615c8f863be4986c23ea2ff7642b02bc605
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67786573"
---
# <a name="configure-azure-firewall-application-rules-with-sql-fqdns"></a>SQL FQDN ile Azure güvenlik duvarı uygulama kurallarını yapılandırma

> [!IMPORTANT]
> Azure güvenlik duvarı uygulama kuralları SQL FQDN ile şu anda genel önizlemede.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Artık SQL FQDN ile Azure güvenlik duvarı uygulama kuralları yapılandırabilirsiniz. Bu, yalnızca belirtilen SQL server örneklerinin sanal ağlarınızdan erişimi sınırlamak sağlar.

SQL FQDN ile trafiğini filtreleyebilirsiniz:

- Bir Azure SQL veritabanı veya bir Azure SQL veri ambarı sanal ağlarınızdan. Örneğin: Yalnızca erişim izni *sql server1.database.windows.net*.
- Şirket içinden Azure SQL yönetilen örneğine veya SQL Iaas, sanal ağlarda çalışan.
- Uç-için-uç Azure SQL yönetilen örneğine veya SQL Iaas, sanal ağlarda çalışan.

Genel Önizleme sırasında SQL filtreleme FQDN desteklenen [proxy modunu](https://docs.microsoft.com/azure/sql-database/sql-database-connectivity-architecture#connection-policy) yalnızca (bağlantı noktası 1433). Varsayılan yönlendirme modunda SQL kullanırsanız, bir parçası olarak SQL hizmet etiketini kullanarak erişim filtreleyebilirsiniz [ağ kuralları](overview.md#network-traffic-filtering-rules).
SQL Iaas trafik için varsayılan olmayan bağlantı noktalarını kullanırsanız, bu bağlantı noktalarını güvenlik duvarı uygulama kuralları yapılandırabilirsiniz.

> [!NOTE]
> Uygulama kuralları SQL FQDN ile şu anda Azure CLI, REST ve Şablonlar aracılığıyla tüm bölgelerde. Portal kullanıcı arabirimini bölgelere artımlı olarak eklendiğinden ve dağıtımı tamamlandığında, tüm bölgelerde kullanıma sunulacaktır.

## <a name="configure-using-azure-cli"></a>Azure CLI kullanarak yapılandırma

1. Dağıtım bir [Azure Azure CLI kullanarak güvenlik duvarı](deploy-cli.md).
2. Azure SQL veritabanı, SQL veri ambarı veya SQL yönetilen örneği için trafik filtreleme, SQL bağlantı modunu ayarlandığından emin olun **Proxy**. SQL bağlantı moduna geçmek öğrenmek için bkz: [Azure SQL bağlantı mimarisi](https://docs.microsoft.com/azure/sql-database/sql-database-connectivity-architecture#change-azure-sql-database-connection-policy). 

   > [!NOTE]
   > SQL *proxy* modu ile karşılaştırıldığında daha fazla gecikme sonuçlanabilir *yeniden yönlendirme*. Azure içinde bağlanan istemciler için varsayılan olan yeniden yönlendirme modu kullanmaya devam etmek isterseniz SQL kullanarak erişimi filtreleyebilirsiniz [hizmet etiketi](service-tags.md) Güvenlik Duvarı'nda [ağ kuralları](tutorial-firewall-deploy-portal.md#configure-a-network-rule).

3. Bir SQL sunucusuna erişmesine izin vermek için SQL FQDN'si ile bir uygulama kuralı yapılandırın:

   ```azurecli
   az network firewall application-rule create \
   -g FWRG \
   -f azfirewall \
   -c FWAppRules \
   -n srule \
   --protocols mssql=1433 \
   --source-addresses 10.0.0.0/24 \
   --target-fqdns sql-serv1.database.windows.net
   ```

## <a name="configure-using-the-azure-portal"></a>Azure portalını kullanarak yapılandırma
1. Dağıtım bir [Azure Azure CLI kullanarak güvenlik duvarı](deploy-cli.md).
2. Azure SQL veritabanı, SQL veri ambarı veya SQL yönetilen örneği için trafik filtreleme, SQL bağlantı modunu ayarlandığından emin olun **Proxy**. SQL bağlantı moduna geçmek öğrenmek için bkz: [Azure SQL bağlantı mimarisi](../sql-database/sql-database-connectivity-architecture.md#change-azure-sql-database-connection-policy). 

   > [!NOTE]
   > SQL *proxy* modu ile karşılaştırıldığında daha fazla gecikme sonuçlanabilir *yeniden yönlendirme*. Azure içinde bağlanan istemciler için varsayılan olan yeniden yönlendirme modu kullanmaya devam etmek isterseniz SQL kullanarak erişimi filtreleyebilirsiniz [hizmet etiketi](service-tags.md) Güvenlik Duvarı'nda [ağ kuralları](tutorial-firewall-deploy-portal.md#configure-a-network-rule).
3. Uygun bir protokol, bağlantı noktası ve SQL FQDN'si ile uygulama kuralı ekleyin ve ardından **Kaydet**.
   ![SQL FQDN'si ile uygulama kuralı](media/sql-fqdn-filtering/application-rule-sql.png)
4. Güvenlik Duvarı üzerinden trafiği filtreleyen bir sanal ağ içindeki bir sanal makineden SQL erişim. 
5. Doğrulamak [Azure güvenlik duvarı günlükleri](log-analytics-samples.md) Göster trafiğe izin verilir.

## <a name="next-steps"></a>Sonraki adımlar

SQL proxy hakkında bilgi edinin ve yeniden yönlendirme modları için bkz [Azure SQL veritabanı bağlantı mimarisi](../sql-database/sql-database-connectivity-architecture.md).