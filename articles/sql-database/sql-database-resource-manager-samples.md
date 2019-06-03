---
title: SQL veritabanı için Azure Resource Manager şablonları | Microsoft Docs
description: Azure Resource Manager şablonları oluşturmak ve Azure SQL veritabanı'nı yapılandırmak için kullanın.
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.custom: overview-samples
ms.devlang: ''
ms.topic: sample
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: sstein
manager: craigg
ms.date: 02/04/2019
ms.openlocfilehash: b967dc872529ec8b045df81542eec4c555b17a6c
ms.sourcegitcommit: c05618a257787af6f9a2751c549c9a3634832c90
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66418443"
---
# <a name="azure-resource-manager-templates-for-azure-sql-database"></a>Azure SQL veritabanı için Azure Resource Manager şablonları

Azure Resource Manager şablonları, kod olarak altyapınızı tanımlamak ve çözümlerinizi Azure bulutuna dağıtmak etkinleştirin.

## <a name="single-database--elastic-pool"></a>Tek veritabanı ve elastik havuz

Aşağıdaki tabloda, Azure SQL veritabanı için Azure Resource Manager şablonlarının bağlantılarını içerir.

| |  |
|---|---|
| [Tek veritabanı](https://github.com/Azure/azure-quickstart-templates/tree/master/201-sql-database-transparent-encryption-create) | Bu Azure Resource Manager şablonu, tek bir Azure SQL veritabanı mantıksal sunucusu oluşturur ve güvenlik duvarı kuralları yapılandırır. |
| [Mantıksal sunucu](https://github.com/Azure/azure-quickstart-templates/tree/master/101-sql-logical-server) | Bu Azure Resource Manager şablonu, Azure SQL veritabanı için bir mantıksal sunucu oluşturur. |
| [Elastik havuz](https://github.com/Azure/azure-quickstart-templates/tree/master/101-sql-elastic-pool-create) | Bu şablon, yeni bir elastik havuz yeni ilişkili SQL Server ve SQL veritabanlarını yeni atamak dağıtmanıza olanak sağlar. |
| [Yük devretme grupları](https://github.com/Azure/azure-quickstart-templates/tree/master/101-sql-with-failover-group) | Bu şablon, iki Azure SQL mantıksal sunucuları ve SQL veritabanı yük devretme grubu oluşturur.|
| [Tehdit algılama](https://github.com/Azure/azure-quickstart-templates/tree/master/201-sql-threat-detection-db-policy-multiple-databases) | Bu şablon, bir Azure SQL mantıksal sunucusu ve bir Azure SQL veritabanı tehdit algılama, her veritabanı için uyarılar için e-posta adresiyle etkin ile dağıtmanıza olanak sağlar. Tehdit algılama, SQL Gelişmiş tehdit Koruması (ATP) sunan bir parçasıdır ve bir SQL sunucuları ve veritabanları üzerinde olası tehditleri yanıt veren bir güvenlik katmanı sağlar.|
| [Azure Blob depolama alanına denetleme](https://github.com/Azure/azure-quickstart-templates/tree/master/201-sql-auditing-server-policy-to-blob-storage) | Bu şablon, bir blob depolama alanı için Denetim günlükleri yazmak için etkin bir denetim ile bir Azure SQL mantıksal sunucusu dağıtmanızı sağlar. Azure SQL veritabanı denetimi veritabanı olaylarını izler ve Azure depolama hesabı, OMS çalışma alanına veya olay hub'ları yerleştirilebilir bir denetim günlüğüne yazar.|
| [Azure olay Hub'ına denetleme](https://github.com/Azure/azure-quickstart-templates/tree/master/201-sql-auditing-server-policy-to-eventhub) | Bu şablon, mevcut bir olay Hub'ına denetim günlükleri yazmak için etkin bir denetim ile bir Azure SQL sunucusu dağıtmanızı sağlar. Denetim olayları olay Hub'ına göndermek için ayarlamanız denetim ayarları ile `Enabled` `State` ayarlayıp `IsAzureMonitorTargetEnabled` olarak `true`. Ayrıca, tanılama ayarlarla yapılandırın `SQLSecurityAuditEvents` tanılama günlükleri kategorisine göre `master` veritabanı (hizmet düzeyi denetimi için). Azure SQL veritabanı ve SQL veri ambarı için denetimi veritabanı olaylarını izler ve Azure depolama hesabı, OMS çalışma alanına veya olay hub'ları yerleştirilebilir bir denetim günlüğüne yazar.|
| [SQL veritabanı ile Azure Web uygulaması](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-sql-database) | Bu örnek, "Temel" hizmet düzeyinde ücretsiz bir Azure Web uygulaması ve SQL veritabanı oluşturur.|
| [Azure Web uygulaması ve SQL veritabanı ile Redis önbelleği](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-redis-cache-sql-database) | Bu şablon aynı kaynak grubunda bir Web uygulaması, Redis Cache ve SQL veritabanı oluşturur ve iki bağlantı dizesi SQL veritabanı ve Redis Cache için Web uygulaması oluşturur.|
| [ADF V2 kullanarak blob Depolama'dan veri aktarın](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-v2-blob-to-sql-copy) | Bu Azure Resource Manager şablonu, Azure Blob depolama alanından SQL veritabanına veri kopyalayan Azure Data Factory V2 oluşturur.|
| [SQL veritabanı ile HDInsight kümesi](https://github.com/Azure/azure-quickstart-templates/tree/master/101-hdinsight-linux-with-sql-database) | Bu şablon, bir HDInsight kümesi, bir SQL veritabanı sunucusu, SQL veritabanı ve iki tablo oluşturmanıza olanak sağlar. Bu şablon tarafından kullanılan [makalede HDInsight Hadoop ile Sqoop kullanma](https://docs.microsoft.com/azure/hdinsight/hadoop/hdinsight-use-sqoop) |
| [SQL saklı yordamı bir zamanlamaya göre çalışan Azure mantıksal uygulama](https://github.com/Azure/azure-quickstart-templates/tree/master/101-logic-app-sql-proc) | Bu şablon bir SQL saklı yordamı zamanlamaya göre çalıştırılacak bir mantıksal uygulama oluşturmanıza olanak sağlar. Yordam için herhangi bir bağımsız değişken şablonun gövdesini bölüme yerleştirebilirsiniz.|

## <a name="managed-instance"></a>Yönetilen Örnek

Aşağıdaki tabloda, Azure SQL veritabanı - yönetilen örnek için Azure Resource Manager şablonlarının bağlantılarını içerir.

| |  |
|---|---|
| [Yönetilen örnek yeni bir sanal ağ](https://github.com/Azure/azure-quickstart-templates/tree/master/101-sqlmi-new-vnet) | Bu Azure Resource Manager şablonu, sanal ağda yapılandırılmış yeni Azure VNet ve yönetilen örneği oluşturur. |
| [Yönetilen örnek için ağ ortamı](https://github.com/Azure/azure-quickstart-templates/tree/master/101-sql-managed-instance-azure-environment) | Bu dağıtım, iki alt ağa sahip - yönetilen örneklerinizin adanmış bir ve diğer kaynaklara (örneğin VM, App Service ortamları, vs.) yere yerleştirebilirsiniz başka bir yapılandırılmış bir Azure sanal ağ oluşturur. Bu şablon, yönetilen örnekler dağıtabileceğiniz düzgün bir şekilde yapılandırılmış bir ağ ortamı oluşturur. |
| [P2S bağlantısı ile yönetilen örnek](https://github.com/Azure/azure-quickstart-templates/tree/master/201-sqlmi-new-vnet-w-point-to-site-vpn) | Bu dağıtım, bir Azure sanal ağı iki alt ağa sahip oluşturacak `ManagedInstance` ve `GatewaySubnet`. Yönetilen örnek alt ağında ManagedInstance dağıtılır. Sanal ağ geçidi oluşturulacak `GatewaySubnet` alt ağ ve noktadan siteye VPN bağlantısı için yapılandırılmış. |
| [Sanal makine ile yönetilen örnek](https://github.com/Azure/azure-quickstart-templates/tree/master/201-sqlmi-new-vnet-w-jumpbox) | Bu dağıtım, bir Azure sanal ağı iki alt ağa sahip oluşturacak `ManagedInstance` ve `Management`. Yönetilen örnek dağıtılacağı `ManagedInstance` alt ağ. Sanal makine SQL Server Management Studio (SSMS) en son sürümle dağıtılacağı `Management` alt ağ. |
