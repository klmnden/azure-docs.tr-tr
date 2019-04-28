---
title: Azure SQL veri ambarı sorunlarını giderme | Microsoft Docs
description: Azure SQL veri ambarı sorunlarını giderme.
services: sql-data-warehouse
author: mlee3gsd
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: manage
ms.date: 03/27/2019
ms.author: martinle
ms.reviewer: igorstan
ms.openlocfilehash: 5115ffbc3568c87c37bae4a3e65c37f8504f1fb8
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61475357"
---
# <a name="troubleshooting-connectivity-issues"></a>Bağlantı sorunlarını giderme

Bu makalede, SQL veri ambarı'na bağlanma etrafında yaygın sorun giderme teknikleri listelenmektedir.
- [Hizmet kullanılabilirliğini denetle](./sql-data-warehouse-troubleshoot-connectivity.md#check-service-availability)
- [Duraklatılmış veya ölçeklendirme işlemi için denetleyin.](./sql-data-warehouse-troubleshoot-connectivity.md#check-for-paused-or-scaling-operation)
- [Güvenlik Duvarı ayarlarınızı denetleyin](./sql-data-warehouse-troubleshoot-connectivity.md#check-your-firewall-settings)
- [Sanal ağ/Service uç noktasının ayarlarınızı kontrol edin](./sql-data-warehouse-troubleshoot-connectivity.md#check-your-vnetservice-endpoint-settings)
- [En son sürücülere denetle](./sql-data-warehouse-troubleshoot-connectivity.md#check-for-the-latest-drivers)
- [Bağlantı dizenizi denetleyin](./sql-data-warehouse-troubleshoot-connectivity.md#check-your-connection-string)
- [Aralıklı bağlantı sorunları](./sql-data-warehouse-troubleshoot-connectivity.md#intermittent-connection-issues)
- [Genel hata iletileri](./sql-data-warehouse-troubleshoot-connectivity.md#common-error-messages)

## <a name="check-service-availability"></a>Hizmet kullanılabilirliğini denetle

Hizmet kullanılabilir olup olmadığını denetleyin. Azure Portal'da SQL data warehouse'a bağlanmaya çalıştığınız gidin. Soldaki İçindekiler panelinde tıklayarak **Tanıla ve problemleri çözmenize**.

![Kaynak durumu seçin](./media/sql-data-warehouse-troubleshoot-connectivity/diagnostics-link.png)

SQL veri ambarınızın durumunu burada gösterilir. Hizmet olarak görünmüyorsa **kullanılabilir**, ek adımları denetleyin.

![Hizmet kullanılabilir](./media/sql-data-warehouse-troubleshoot-connectivity/resource-health.png)

Kaynak durumu gösteren veri ambarınız duraklatıldığı veya ölçeklendirme, veri Ambarınızı sürdürmek için yönergeleri izleyin.

![Hizmet duraklatıldı](./media/sql-data-warehouse-troubleshoot-connectivity/resource-health-pausing.png) kaynak durumu hakkında daha fazla bilgi şurada bulunabilir.

## <a name="check-for-paused-or-scaling-operation"></a>Duraklatılmış veya ölçeklendirme işlemi için denetleyin.

SQL veri ambarınız duraklatıldığı görmek için portalı veya ölçeklendirme kontrol edin.

![Hizmet duraklatıldı](./media/sql-data-warehouse-troubleshoot-connectivity/overview-paused.png)

Gördüğünüz hizmetinizin duraklatılmadığı veya ölçeklendirme sırasında bakım zamanlamanızı değil görmek için kontrol edin. SQL veri ambarınız için portalda *genel bakış*, seçili bakım zamanlaması görürsünüz.

![Bakım zamanlaması genel bakış](./media/sql-data-warehouse-troubleshoot-connectivity/overview-maintance-schedule.png)

Aksi takdirde, bu bakım için zamanlanmış bir olayı olmadığını doğrulamak için BT yöneticinize danışın. SQL veri ambarı sürdürmek için verilen adımları izleyin. [burada](https://docs.microsoft.com/azure/sql-data-warehouse/pause-and-resume-compute-portal#resume-compute).

## <a name="check-your-firewall-settings"></a>Güvenlik Duvarı ayarlarınızı denetleyin

SQL Veri Ambarı 1433 numaralı bağlantı noktası üzerinden iletişim kurar.   Kurumsal ağ içinden gelen bağlanmaya çalışıyorsanız, ağınızın güvenlik duvarı tarafından 1433 numaralı bağlantı noktası üzerinden giden trafiğe verilmeyebilir. Bu durumda, BT departmanınız 1433 numaralı bağlantı noktasını açmadığı sürece Azure SQL veritabanı sunucunuza bağlanamazsınız. Güvenlik duvarı yapılandırmaları hakkında daha fazla bilgi bulunabilir [burada](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure?toc=%2Fazure%2Fsql-data-warehouse%2Ftoc.json#manage-ip-firewall-rules-using-the-azure-portal).

## <a name="check-your-vnetservice-endpoint-settings"></a>Sanal ağ/Service uç noktasının ayarlarınızı kontrol edin

Hataları 40914 ve 40615 alıyorsunuz olup [hata açıklaması ve burada çözümleme](https://docs.microsoft.com/azure/sql-database/sql-database-vnet-service-endpoint-rule-overview?toc=/azure/sql-data-warehouse/toc.json#errors-40914-and-40615).

## <a name="check-for-the-latest-drivers"></a>En son sürücülere denetle

### <a name="software"></a>Yazılım

SQL veri ambarı'na bağlanmak için en yeni araçları kullandığınızdan emin olmak için kontrol edin:

* SSMS
* Azure Data Studio
* SQL Server veri Araçları (Visual Studio)

### <a name="drivers"></a>Sürücüler

En son sürücü sürümleri kullandığınızdan emin olmak için kontrol edin.  Sürücüleri daha eski bir sürümünü kullanarak, eski sürücüleri yeni özellikleri desteklemiyor olabilir gibi beklenmeyen davranışları neden olabilir.

* [ODBC](https://docs.microsoft.com/sql/connect/odbc/download-odbc-driver-for-sql-server)
* [JDBC](https://docs.microsoft.com/sql/connect/jdbc/download-microsoft-jdbc-driver-for-sql-server)
* [OLE DB](https://docs.microsoft.com/sql/connect/oledb/download-oledb-driver-for-sql-server)
* [PHP](https://docs.microsoft.com/sql/connect/php/download-drivers-php-sql-server)

## <a name="check-your-connection-string"></a>Bağlantı dizenizi denetleyin

Bağlantı Dizelerinizin düzgün ayarlandığından emin olun.  Aşağıda bazı örnekler verilmiştir.  Ek bilgiler bulabilirsiniz [bağlantı dizeleri burada](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-connection-strings).

ADO.NET bağlantı dizesi

```csharp
Server=tcp:{your_server}.database.windows.net,1433;Database={your_database};User ID={your_user_name};Password={your_password_here};Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;
```

ODBC bağlantı dizesi

```csharp
Driver={SQL Server Native Client 11.0};Server=tcp:{your_server}.database.windows.net,1433;Database={your_database};Uid={your_user_name};Pwd={your_password_here};Encrypt=yes;TrustServerCertificate=no;Connection Timeout=30;
```

PHP bağlantı dizesi

```PHP
Server: {your_server}.database.windows.net,1433 \r\nSQL Database: {your_database}\r\nUser Name: {your_user_name}\r\n\r\nPHP Data Objects(PDO) Sample Code:\r\n\r\ntry {\r\n   $conn = new PDO ( \"sqlsrv:server = tcp:{your_server}.database.windows.net,1433; Database = {your_database}\", \"{your_user_name}\", \"{your_password_here}\");\r\n    $conn->setAttribute( PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION );\r\n}\r\ncatch ( PDOException $e ) {\r\n   print( \"Error connecting to SQL Server.\" );\r\n   die(print_r($e));\r\n}\r\n\rSQL Server Extension Sample Code:\r\n\r\n$connectionInfo = array(\"UID\" => \"{your_user_name}\", \"pwd\" => \"{your_password_here}\", \"Database\" => \"{your_database}\", \"LoginTimeout\" => 30, \"Encrypt\" => 1, \"TrustServerCertificate\" => 0);\r\n$serverName = \"tcp:{your_server}.database.windows.net,1433\";\r\n$conn = sqlsrv_connect($serverName, $connectionInfo);
```

JDBC bağlantı dizesi

```Java
jdbc:sqlserver://yourserver.database.windows.net:1433;database=yourdatabase;user={your_user_name};password={your_password_here};encrypt=true;trustServerCertificate=false;hostNameInCertificate=*.database.windows.net;loginTimeout=30;
```

## <a name="intermittent-connection-issues"></a>Aralıklı bağlantı sorunları

Çok sayıda kuyruğa alınan istekler ile sunucuda ağır yük yaşıyorsanız, denetleyin. Ek kaynaklar için veri ambarınızın ölçeğini ölçeklendirmeniz gerekebilir.

## <a name="common-error-messages"></a>Genel hata iletileri

40914 ve 40615, görüyorsunuz [hata açıklaması ve burada çözümleme](https://docs.microsoft.com/azure/sql-database/sql-database-vnet-service-endpoint-rule-overview?toc=/azure/sql-data-warehouse/toc.json#errors-40914-and-40615).

## <a name="still-having-connectivity-issues"></a>Hala bağlantı sorunları mı yaşıyorsunuz?
Oluşturma bir [destek bileti](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-get-started-create-support-ticket) mühendislik ekibine desteklediğiniz şekilde.
