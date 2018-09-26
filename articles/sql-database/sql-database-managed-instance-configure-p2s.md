---
title: P2S - yapılandırma Azure SQL veritabanı yönetilen örneği | Microsoft Docs
description: Azure SQL veritabanı yönetilen bir şirket içi istemci bilgisayarından bir noktadan siteye bağlantısı kullanarak SQL Server Management Studio'yu kullanarak örneğine bağlanın.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: carlrab, bonova, jovanpop
manager: craigg
ms.date: 09/06/2018
ms.openlocfilehash: fc51d7191deb8242075b28fbb42adc2ff9ae2739
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2018
ms.locfileid: "47163010"
---
# <a name="configure-a-point-to-site-connection-to-connect-to-an-azure-sql-database-managed-instance-from-on-premises-computer"></a>Azure SQL veritabanı yönetilen örneği için şirket içi bilgisayarından bağlanmak için noktadan siteye bağlantı yapılandırma

Bu hızlı başlangıçta bir Azure SQL veritabanı yönetilen örneği kullanmaya nasıl bağlanılacağı gösterilmiştir [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/sql-server-management-studio-ssms) (SSMS) bir noktadan siteye bağlantı üzerinden şirket içi istemci bilgisayarından. Noktadan siteye bağlantılar hakkında daha fazla bilgi için bkz. [noktadan siteye VPN hakkında](../vpn-gateway/point-to-site-about.md)

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıç:
- Kendi başlangıç olarak kullanır, bu hızlı başlangıçta oluşturulan kaynakları işaret: [bir yönetilen örnek oluşturma](sql-database-managed-instance-get-started.md).
- PowerShell 5.1 ve Azure PowerShell 5.4.2 gerektirir ya da daha yüksek şirket içi istemci bilgisayarınız.
- En yeni sürümünü gerektirir [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/sql-server-management-studio-ssms) (SSMS) şirket içi istemci bilgisayarınızda

## <a name="attach-a-vpn-gateway-to-your-managed-instance-virtual-network"></a>Yönetilen örnek sanal ağınıza bir VPN ağ geçidi ekleme

1. Şirket içi istemci bilgisayarınızda PowerShell'i açın.
2. Bu PowerShell Betiği kopyalayıp yeniden açın. Bu komut dosyasını oluşturduğunuz yönetilen örnek sanal ağ VPN ağ geçidi ekler [bir yönetilen örnek oluşturma](sql-database-managed-instance-get-started.md) hızlı başlangıç. Bu betik, aşağıdaki üç adımı gerçekleştirir:
  - Oluşturur ve sertifika istemci makineye yükleme
  - Gelecekteki bir VPN ağ geçidi alt ağ IP aralığı hesaplar.
  - GatewaySubnet oluşturur
  - VPN ağ geçidi için VPN alt ağa bağlanan bir Azure Resource Manager şablonu dağıtır

   ```powershell
   $scriptUrlBase = 'https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/manage/azure-sql-db-managed-instance/attach-vpn-gateway'

   $parameters = @{
       subscriptionId = '<subscriptionId>'
       resourceGroupName = '<resourceGroupName>'
       virtualNetworkName = '<virtualNetworkName>'
       certificateNamePrefix  = '<certificateNamePrefix>'
       }

   Invoke-Command -ScriptBlock ([Scriptblock]::Create((iwr ($scriptUrlBase+'/attachVPNGateway.ps1?t='+ [DateTime]::Now.Ticks)).Content)) -ArgumentList $parameters, $scriptUrlBase 
   ```

3. PowerShell betik istenen parametreleri girin. Değerleri `<subscriptionId>`, `<resourceGroup>` ve `<virtualNetworkName>` kullanılan olanlarla eşleşmesi [yönetilen örnek oluşturma](sql-database-managed-instance-get-started.md) hızlı başlangıç. Değeri `<certificateNamePrefix>` tercih ettiğiniz bir dize olabilir.

4. PowerShell betiğini çalıştırın.

## <a name="create-a-vpn-connection-to-your-managed-instance"></a>Yönetilen Örneğiniz için bir VPN bağlantısı oluşturma

1. [Azure Portal](https://portal.azure.com/) oturum açın.
2. Sanal ağ geçidi oluşturduğunuz kaynak grubunu açın ve ardından sanal ağ geçidi kaynağı açın.

    ![sanal ağ geçidi kaynağı](./media/sql-database-managed-instance-configure-p2s/vnet-gateway.png)  

3. Tıklayın **noktadan siteye yapılandırma** ve ardından **VPN istemcisini indir**.

    ![VPN istemcisini indir](./media/sql-database-managed-instance-configure-p2s/download-vpn-client.png)  
4. Zip dosyasından dosyaları ayıklayın ve ardından ayıklanan klasörü açın. 
5. Havigate WindowsAmd64 klasörüne ve açık **VpnClientSetupAmd64.exe** dosya.
6. Alırsanız bir **Windows bilgisayarınıza korumalı** ye ileti **daha fazla bilgi** ve ardından **yine de Çalıştır**.

    ![VPN istemcisini yükleme](./media/sql-database-managed-instance-configure-p2s/vpn-client-defender.png)\
7. Tıklayın **Evet** devam etmek için kullanıcı hesabı denetimi iletişim kutusunda.
8. MyNewVNet iletişim kutusunda **Evet** MyNewVNet için bir Vpn istemcisi yüklemek için.

## <a name="connect-to-the-vpn-connection"></a>VPN bağlantısı bağlanın.

1. İstemci bilgisayarınızda VPN bağlantılarına gidin ve tıklayın **MyNewVNet** bu sanal ağ ile bağlantı kurmak için.

    ![VPN bağlantısı](./media/sql-database-managed-instance-configure-p2s/vpn-connection.png)  
2. **Bağlan**'a tıklayın.
3. MyNewVNet iletişim kutusunda **Connect**.

    ![VPN bağlantısı](./media/sql-database-managed-instance-configure-p2s/vpn-connection2.png)  
4. Yol tablonuz güncelleştirmek için Bağlantı Yöneticisi gereksinimlerini yükseltilmiş ayrıcalığa sorulduğunda **devam**.
5. Tıklayın **Evet** devam etmek için kullanıcı hesabı denetimi iletişim kutusunda.

    ![VPN bağlantısı](./media/sql-database-managed-instance-configure-p2s/vpn-connection-succeeded.png)  

   Yönetilen örnek sanal ağınıza bir VPN bağlantısı kurduğunuz.

## <a name="use-ssms-to-connect-to-the-managed-instance"></a>SSMS, yönetilen örneği'ne bağlanın

1. Şirket içi istemci bilgisayarda, SQL Server Management Studio (SSMS) açın.
 
2. İçinde **sunucuya Bağlan** iletişim kutusunda, tam girin **ana bilgisayar adı** yönetilen örneğinizin **sunucu adı** kutusunda **SQL Server Kimlik doğrulaması**, kullanıcı adı ve parola sağlayın ve ardından **Connect**.

    ![ssms bağlanma](./media/sql-database-managed-instance-configure-vm/ssms-connect.png)  

Bağlandıktan sonra Veritabanları düğümündeki sistem ve kullanıcı veritabanlarınızı ve Güvenlik, Sunucu Nesneleri, Çoğaltma, Yönetim, SQL Server Agent ile XEvent Profil Oluşturucu düğümlerindeki çeşitli nesneleri görüntüleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- Azure sanal makinesinden bağlanma gösteren Hızlı Başlangıç için bkz: [noktadan siteye bağlantı yapılandırma](sql-database-managed-instance-configure-p2s.md)
- Bağlantı seçenekleri uygulamalar için genel bakış için bkz: [, uygulamaların yönetilen örneğe bağlanma](sql-database-managed-instance-connect-app.md).
- Mevcut SQL Server veritabanını şirket içi, yönetilen örneğe geri yüklemek için kullanabileceğiniz [geçiş için Azure veritabanı geçiş hizmeti (DMS)](../dms/tutorial-sql-server-to-managed-instance.md) bir veritabanı yedekleme dosyasından geri yüklemek veya [T-SQL RESTORE komut](sql-database-managed-instance-get-started-restore.md) bir veritabanı yedekleme dosyasından geri yüklemek için.
