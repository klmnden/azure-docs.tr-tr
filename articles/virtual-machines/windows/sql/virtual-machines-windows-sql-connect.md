---
title: Bir SQL Server sanal makinesine (Resource Manager) bağlanma | Microsoft Docs
description: Azure'da bir sanal makinede çalışan SQL Server bağlayacağınızı öğrenin. Bu konu Klasik dağıtım modelini kullanır. Senaryoları ağ yapılandırmasını ve istemcinin konumu bağlı olarak farklılık gösterir.
services: virtual-machines-windows
documentationcenter: na
author: rothja
manager: craigg
tags: azure-resource-manager
ms.assetid: aa5bf144-37a3-4781-892d-e0e300913d03
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 12/12/2017
ms.author: jroth
ms.openlocfilehash: 7285cf47c3a5ec731cd9cfe311053e9d19886f1d
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
ms.locfileid: "29400248"
---
# <a name="connect-to-a-sql-server-virtual-machine-on-azure"></a>Azure SQL Server sanal makineye bağlanma

## <a name="overview"></a>Genel Bakış

Bu konuda, bir Azure sanal makinede çalışan SQL Server örneğine bağlanmak açıklar. Bazı kapsayan [genel bağlantı senaryoları](#connection-scenarios) ve ardından sağlar [bağlantı ayarlarını değiştirmek için Portalı'ndaki adımları](#change). Gerekirse sorun giderme veya portal dışında bağlantısı yapılandırmak bkz: [el ile yapılandırma](#manual) bu konunun sonundaki. 

Hem sağlama hem de bağlantı tam bir gözden geçirme olurdu olup [Azure üzerinde bir SQL Server sanal makine sağlama](virtual-machines-windows-portal-sql-server-provision.md).

## <a name="connection-scenarios"></a>Bağlantı senaryoları

Bir sanal makinede çalışan SQL Server bir istemcinin bağlandığı yöntemi konumunu istemci ve ağ yapılandırmasına bağlı olarak değişir.

Azure portalında bir SQL Server VM sağlarsanız, türünü belirtme seçeneğiniz **SQL Bağlantısı**.

![Sağlama sırasında ortak SQL bağlantı seçeneği](./media/virtual-machines-windows-sql-connect/sql-vm-portal-connectivity.png)

Bağlantı için seçenekleriniz şunlardır:

| Seçenek | Açıklama |
|---|---|
| **Ortak** | Internet üzerinden SQL Server'a bağlanma |
| **Özel** | Aynı sanal ağ içinde SQL Server'a bağlanma |
| **Yerel** | SQL Server yerel olarak aynı sanal makineye Bağlan | 

Aşağıdaki bölümlerde açıklanmıştır **ortak** ve **özel** daha ayrıntılı seçenekleri.

## <a name="connect-to-sql-server-over-the-internet"></a>Internet üzerinden SQL Server'a bağlanma

SQL Server veritabanı altyapısına Internet'ten bağlanmak isterseniz seçin **ortak** için **SQL Bağlantısı** sağlama sırasında portalında türü. Portal otomatik olarak aşağıdaki adımları gerçekleştirir:

* TCP/IP Protokolü SQL Server için etkinleştirir.
* SQL Server TCP bağlantı noktasını (varsayılan 1433) açmak için bir güvenlik duvarı kuralı yapılandırır.
* SQL Server ortak erişim için gerekli kimlik doğrulamasını etkinleştirir.
* Ağ güvenlik grubu VM SQL Server bağlantı noktasındaki tüm TCP trafiğine yapılandırır.

> [!IMPORTANT]
> Express sürümleri ve SQL Server Geliştirici için sanal makine görüntüleri otomatik olarak TCP/IP protokol etkinleştirmeyin. Geliştirici ve Express sürümleri için SQL Server Configuration Manager için kullanmanız gerekir [el ile TCP/IP protokolünü etkinleştirin](#manualtcp) VM oluşturduktan sonra.

İnternet erişimi olan herhangi bir istemci ya da sanal makineyi genel IP adresi, IP adresi atanmış herhangi bir DNS etiketi belirterek SQL Server örneğine bağlanabilir. SQL Server bağlantı noktası 1433 ise, bağlantı dizesinde belirtmek gerekmez. Bir SQL VM DNS etiketine sahip bir bağlantı dizesi olarak şunu bağlandığı `sqlvmlabel.eastus.cloudapp.azure.com` SQL kimlik doğrulaması (Ayrıca kullanabileceğiniz ortak IP adresi) kullanarak.

```
Server=sqlvmlabel.eastus.cloudapp.azure.com;Integrated Security=false;User ID=<login_name>;Password=<your_password>
```

Bu bağlantı istemciler için internet üzerinden sağlar, ancak bu herkes SQL sunucunuza bağlanmak göstermez. Dışında doğru kullanıcı adını ve parolayı istemciler vardır. Ancak, ek güvenlik için iyi bilinen bağlantı noktası 1433 önleyebilirsiniz. Örneğin, SQL Server'ın dinleme bağlantı noktası 1500 ve yerleşik uygun güvenlik duvarı ve ağ güvenlik grubu kural yapılandırdıysanız, sunucu adı için bağlantı noktası numarasını ekleyerek bağlanamadı. Aşağıdaki örnekte bir özel bağlantı noktası numarası ekleyerek öncekinin değiştirir **1500**, sunucu adı:

```
Server=sqlvmlabel.eastus.cloudapp.azure.com,1500;Integrated Security=false;User ID=<login_name>;Password=<your_password>"
```

> [!NOTE]
> İnternet üzerinden bir VM'de SQL Server sorguladığınızda, tüm giden Azure veri merkezi verilerdir normal tabi [giden veri aktarımları fiyatlandırma](https://azure.microsoft.com/pricing/details/data-transfers/).

## <a name="connect-to-sql-server-within-a-virtual-network"></a>Bir sanal ağ içinde SQL Server'a bağlanma

Seçtiğinizde **özel** için **SQL Bağlantısı** türünü Azure Portalı'nda aynı ayarların çoğu yapılandırır **ortak**. (Varsayılan 1433) SQL Server bağlantı noktasındaki dış trafiğe izin vermek için ağ güvenlik grubu kural olduğunu farktır.

> [!IMPORTANT]
> Express sürümleri ve SQL Server Geliştirici için sanal makine görüntüleri otomatik olarak TCP/IP protokol etkinleştirmeyin. Geliştirici ve Express sürümleri için SQL Server Configuration Manager için kullanmanız gerekir [el ile TCP/IP protokolünü etkinleştirin](#manualtcp) VM oluşturduktan sonra.

Özel bağlantı ile birlikte kullanılan genellikle [sanal ağ](../../../virtual-network/virtual-networks-overview.md), birkaç senaryo sağlar. Bu sanal makineleri farklı bir kaynak grubu mevcut olsa bile aynı sanal ağda VM'ler bağlanabilir. İle bir [siteden siteye VPN](../../../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md), şirket içi ağlar ve makineler VM'ler bağlayan bir karma mimarisi oluşturabilirsiniz.

Sanal ağlar Azure Vm'leriniz için bir etki alanına katılmak sağlar. Bu, SQL Server için Windows kimlik doğrulamasını kullanmak için tek yoludur. Diğer bağlantı senaryolar, kullanıcı adlarını ve Parolaları SQL kimlik doğrulaması gerektirir.

Sanal ağınıza DNS yapılandırmış olduğunuz varsayılarak, bağlantı dizesinde SQL Server VM bilgisayar adını belirterek SQL Server örneğinizi bağlanabilir. Aşağıdaki örnek, ayrıca Windows kimlik doğrulaması da yapılandırılmış olduğunu ve kullanıcının SQL Server örneğine erişimi verildiğini varsayar.

```
Server=mysqlvm;Integrated Security=true
```

## <a id="change"></a> SQL bağlantı ayarlarını değiştirme

SQL Server sanal makinenizi Azure portalında için bağlantı ayarlarını değiştirebilirsiniz.

1. Azure portalında seçin **sanal makineleri**.

2. SQL Server VM'yi seçin.

3. Altında **ayarları**, tıklatın **SQL Server yapılandırma**.

4. Değişiklik **SQL bağlantı düzeyi** gerekli ayarınız için. İsteğe bağlı olarak, SQL Server bağlantı noktası veya SQL kimlik doğrulaması ayarlarını değiştirmek için bu alanı kullanabilirsiniz.

   ![SQL Bağlantısı değiştirme](./media/virtual-machines-windows-sql-connect/sql-vm-portal-connectivity-change.png)

5. Güncelleştirmenin tamamlanması birkaç dakika bekleyin.

   ![SQL VM güncelleştirme bildirimi](./media/virtual-machines-windows-sql-connect/sql-vm-updating-notification.png)

## <a id="manualtcp"></a> Geliştirici ve Express sürümleri için TCP/IP'yi etkinleştirin

SQL Server bağlantı ayarları değiştirirken, Azure otomatik olarak TCP/IP Protokolü SQL Server Developer ve Express sürümleri için izin vermez. Aşağıdaki adımlarda, uzaktan IP adresiyle bağlanabilmeniz için TCP/IP’yi el ile nasıl etkinleştirebileceğiniz açıklanmıştır.

İlk olarak, Uzak Masaüstü ile SQL Server makinesinde bağlayın.

[!INCLUDE [Connect to SQL Server VM with remote desktop](../../../../includes/virtual-machines-sql-server-remote-desktop-connect.md)]

Ardından, TCP/IP protokolü ile etkinleştirmek **SQL Server Configuration Manager**.

[!INCLUDE [Connect to SQL Server VM with remote desktop](../../../../includes/virtual-machines-sql-server-connection-tcp-protocol.md)]

## <a name="connect-with-ssms"></a>SSMS ile bağlanma

Aşağıdaki adımlar, Azure VM için isteğe bağlı bir DNS etiketi oluşturmak ve ardından SQL Server Management Studio (SSMS) ile bağlanmak nasıl gösterir.

[!INCLUDE [Connect to SQL Server in a VM Resource Manager](../../../../includes/virtual-machines-sql-server-connection-steps-resource-manager.md)]

## <a id="manual"></a> El ile yapılandırma ve sorun giderme

Portal bağlantı otomatik olarak yapılandırmak için seçenekleri sağlasa da, el ile bağlantı nasıl yapılandırılacağını öğrenmek yararlıdır. Gereksinimleri anlama de sorun giderme yardımcı olabilir.

Aşağıdaki tabloda bir Azure VM içinde çalışan SQL Server için bağlantı kurmaya yönelik gereksinimler listelenmektedir.

| Gereksinim | Açıklama |
|---|---|
| [SQL Server kimlik doğrulaması modunu etkinleştir](https://docs.microsoft.com/sql/database-engine/configure-windows/change-server-authentication-mode#SSMSProcedure) | SQL Server kimlik doğrulaması, bir sanal ağ üzerinde Active Directory yapılandırmadıysanız VM uzaktan bağlanmak için gereklidir. |
| [Bir SQL oturum açma oluşturun](https://docs.microsoft.com/sql/relational-databases/security/authentication-access/create-a-login) | SQL kimlik doğrulaması kullanıyorsanız, bir SQL oturum açma kullanıcı adı ve ayrıca, hedef veritabanı için izinlere sahip parola gerekir. |
| [TCP/IP protokolünü etkinleştirin](#manualTCP) | SQL Server TCP üzerinden bağlantılara izin vermesi gerekir. |
| [SQL Server bağlantı noktası için güvenlik duvarı kuralını etkinleştir](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access) | Güvenlik Duvarı'nı VM SQL Server bağlantı noktası (varsayılan 1433) gelen trafiğe izin vermelidir. |
| [TCP 1433 için ağ güvenlik grubu kural oluşturma](../../../virtual-network/virtual-networks-create-nsg-arm-pportal.md#create-rules-in-an-existing-nsg) | VM internet üzerinden bağlanmak isterseniz SQL Server bağlantı noktası (varsayılan 1433) trafiği almaya izin vermeniz gerekir. Yerel ve sanal ağ-yalnızca bağlantıları bu gerektirmez. Azure portalında gereken tek adım budur. |

> [!TIP]
> Portalda bağlantısını yapılandırırken yukarıdaki tabloda adımlarda sizin için yapılır. Yalnızca yapılandırmanızı onaylamak için veya el ile bağlantı kurmak için aşağıdaki adımları kullanın SQL Server için.

## <a name="next-steps"></a>Sonraki Adımlar

Bağlantı adımları birlikte yönergeleri sağlama görmek için bkz: [Azure üzerinde bir SQL Server sanal makine sağlama](virtual-machines-windows-portal-sql-server-provision.md).

Azure Vm'lerde SQL Server çalıştırma ile ilgili diğer konular için bkz: [Azure Virtual Machines'de SQL Server](virtual-machines-windows-sql-server-iaas-overview.md).