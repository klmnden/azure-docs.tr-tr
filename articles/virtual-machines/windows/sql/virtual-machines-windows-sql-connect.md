---
title: Bir SQL Server sanal makinesine (Resource Manager) bağlanma | Microsoft Docs
description: Azure'da bir sanal makinede çalışan SQL Server'a bağlanma hakkında bilgi edinin. Bu konuda, Klasik dağıtım modeli kullanır. Senaryo ağ yapılandırmasını ve istemcinin konumuna bağlı olarak farklılık gösterir.
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
manager: craigg
tags: azure-resource-manager
ms.assetid: aa5bf144-37a3-4781-892d-e0e300913d03
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 12/12/2017
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: a33525e44b2e294b7ce85c7081864dbef0856588
ms.sourcegitcommit: 22ad896b84d2eef878f95963f6dc0910ee098913
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58650862"
---
# <a name="connect-to-a-sql-server-virtual-machine-on-azure"></a>Azure'da bir SQL Server sanal makinesine bağlanma

## <a name="overview"></a>Genel Bakış

Bu konu, bir Azure sanal makinesinde çalışan SQL Server Örneğinize bağlanmak açıklar. Bazılarını içermektedir [genel bağlantı senaryoları](#connection-scenarios) ve daha sonra sağlar [portal bağlantı ayarları değiştirmeye ilişkin adımları](#change). Gerekirse sorun giderme veya portal dışında bağlantı yapılandırmak için bkz [el ile yapılandırma](#manual) bu konunun sonunda. 

İncelemenin tamamını sağlama ve bağlantı olurdu olup [azure'da bir SQL Server sanal makinesi sağlama](virtual-machines-windows-portal-sql-server-provision.md).

## <a name="connection-scenarios"></a>Bağlantı senaryoları

SQL Server'ın bir sanal makine üzerinde çalışan bir istemcinin bağlandığı şekilde konumunu istemci ve ağ yapılandırmasına bağlı olarak farklılık gösterir.

Azure portalında bir SQL Server VM'si sağlama türünü belirtme seçeneğiniz varsa **SQL Bağlantısı**.

![Sağlama sırasında genel SQL bağlantı seçeneği](./media/virtual-machines-windows-sql-connect/sql-vm-portal-connectivity.png)

Bağlantı seçenekleri şunlardır:

| Seçenek | Açıklama |
|---|---|
| **Genel** | İnternet üzerinden SQL Server'a bağlanma |
| **Özel** | Aynı sanal ağda SQL Server'a bağlanma |
| **Yerel** | SQL Server'a aynı sanal makinede yerel olarak bağlanma | 

Aşağıdaki bölümlerde açıklanmaktadır **genel** ve **özel** daha ayrıntılı seçenekleri.

## <a name="connect-to-sql-server-over-the-internet"></a>Internet üzerinden SQL Server'a bağlanma

SQL Server veritabanı altyapısına Internet'ten bağlanmak istiyorsanız seçin **genel** için **SQL Bağlantısı** sağlama sırasında portalında türü. Portal otomatik olarak aşağıdaki adımları gerçekleştirir:

* TCP/IP Protokolü SQL Server için etkinleştirir.
* SQL Server TCP bağlantı noktasını (varsayılan 1433) açmak için bir güvenlik duvarı kuralı yapılandırır.
* SQL Server genel erişim için gerekli kimlik doğrulamasını etkinleştirir.
* VM'de SQL Server bağlantı tüm TCP trafiği için ağ güvenlik grubu yapılandırır.

> [!IMPORTANT]
> Sanal makine görüntüleri SQL Server Developer ve Express sürümleri için TCP/IP protokolünü otomatik olarak etkinleştirmez. Developer ve Express sürümleri için SQL Server Configuration Manager için kullanmalısınız [TCP/IP protokolünü el ile etkinleştirmek](#manualtcp) VM oluşturduktan sonra.

İnternet erişimi olan herhangi bir istemci, sanal makinenin genel IP adresini veya IP adresi için atanan herhangi bir DNS etiketi belirterek SQL Server örneğine bağlanabilirsiniz. SQL Server bağlantı noktası 1433 ise, bağlantı dizesini belirtmek gerekmez. Aşağıdaki bağlantı dizesi, bir DNS etiketi ile bir SQL VM bağlandığı `sqlvmlabel.eastus.cloudapp.azure.com` SQL (Ayrıca kullanabileceğiniz ortak IP adresi) kimlik doğrulaması kullanarak.

```
Server=sqlvmlabel.eastus.cloudapp.azure.com;Integrated Security=false;User ID=<login_name>;Password=<your_password>
```

Bu bağlantı için istemcileri internet üzerinden sağlar, ancak bu herkes SQL Server'ınıza bağlanmak göstermez. Dışında doğru kullanıcı adını ve parolayı istemciler vardır. Ancak, ek güvenlik için iyi bilinen bağlantı noktası 1433 önleyebilirsiniz. Örneğin, SQL Server'ın bağlantı noktası 1500 ve yerleşik uygun güvenlik duvarı ve ağ güvenlik grubu kuralları dinleyecek şekilde yapılandırdıysanız, sunucu adı için bağlantı noktası numarasını ekleyerek bağlanamadı. Aşağıdaki örnek bir özel bağlantı noktası numarası ekleyerek Öncekine değiştirir **1500**, sunucu adı:

```
Server=sqlvmlabel.eastus.cloudapp.azure.com,1500;Integrated Security=false;User ID=<login_name>;Password=<your_password>"
```

> [!NOTE]
> İnternet üzerinden bir sanal makinede SQL Server sorguladığınızda, Azure veri merkezinden tüm giden veri normal tabi olan [üzerinde giden veri aktarımları fiyatlandırma](https://azure.microsoft.com/pricing/details/data-transfers/).

## <a name="connect-to-sql-server-within-a-virtual-network"></a>Bir sanal ağ içinde SQL Server'a bağlanma

Seçeneğini belirlediğinizde **özel** için **SQL Bağlantısı** türü, Azure portalında aynı ayarların çoğu yapılandırır **genel**. SQL Server bağlantı noktası (varsayılan 1433) dışında trafiğe izin verecek şekilde hiçbir ağ güvenlik grubu kuralı yok farktır.

> [!IMPORTANT]
> Sanal makine görüntüleri SQL Server Developer ve Express sürümleri için TCP/IP protokolünü otomatik olarak etkinleştirmez. Developer ve Express sürümleri için SQL Server Configuration Manager için kullanmalısınız [TCP/IP protokolünü el ile etkinleştirmek](#manualtcp) VM oluşturduktan sonra.

Özel bir bağlantı ile birlikte kullanılan genellikle [sanal ağ](../../../virtual-network/virtual-networks-overview.md), çeşitli senaryolar sağlar. Bu sanal makineler farklı kaynak gruplarında bulunan mevcut olsa bile, Vm'leri aynı sanal ağda bağlanabilirsiniz. İle bir [siteden siteye VPN](../../../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md), makineleri şirket içi ağlar ile Vm'leri bağlanan karma mimarisi oluşturabilirsiniz.

Sanal ağları Azure Vm'lerinizi bir etki alanına sağlar. SQL Server için Windows kimlik doğrulamasını kullanmak için tek yolu budur. Bir bağlantı senaryoları, kullanıcı adları ve parolaları ile SQL kimlik doğrulaması gerektirir.

Sanal ağınızdaki DNS yapılandırmış olduğunuz varsayılarak, SQL Server örneğinizi SQL Server VM bilgisayar adı bağlantı dizesinde belirterek bağlanabilirsiniz. Aşağıdaki örnek, Windows kimlik doğrulaması de yapılandırılmış olmasını ve kullanıcı SQL Server örneği için erişim verildi da varsayılır.

```
Server=mysqlvm;Integrated Security=true
```

## <a id="change"></a> SQL bağlantı ayarlarını değiştir

Azure portalında SQL Server sanal makineniz için bağlantı ayarları değiştirebilirsiniz.

1. Azure portalında **sanal makineler**.

2. VM, SQL Server'ı seçin.

3. Altında **ayarları**, tıklayın **SQL Server Yapılandırması**.

4. Değişiklik **SQL bağlantı düzeyi** gerekli ayarınız için. İsteğe bağlı olarak, SQL Server bağlantı noktası veya SQL kimlik doğrulaması ayarlarını değiştirmek için bu alanı kullanabilirsiniz.

   ![Değişiklik SQL Bağlantısı](./media/virtual-machines-windows-sql-connect/sql-vm-portal-connectivity-change.png)

5. Güncelleştirmenin tamamlanması birkaç dakika bekleyin.

   ![SQL VM güncelleştirme bildirimi](./media/virtual-machines-windows-sql-connect/sql-vm-updating-notification.png)

## <a id="manualtcp"></a> Developer ve Express sürümleri için TCP/IP'yi etkinleştirin

SQL Server bağlantı ayarlarını değiştirirken, Azure otomatik olarak TCP/IP Protokolü SQL Server Developer ve Express sürümleri için izin vermez. Aşağıdaki adımlarda, uzaktan IP adresiyle bağlanabilmeniz için TCP/IP’yi el ile nasıl etkinleştirebileceğiniz açıklanmıştır.

İlk olarak, SQL Server makinesine Uzak Masaüstü ile bağlanın.

[!INCLUDE [Connect to SQL Server VM with remote desktop](../../../../includes/virtual-machines-sql-server-remote-desktop-connect.md)]

Ardından, TCP/IP protokolü ile etkinleştirmek **SQL Server Configuration Manager**.

[!INCLUDE [Connect to SQL Server VM with remote desktop](../../../../includes/virtual-machines-sql-server-connection-tcp-protocol.md)]

## <a name="connect-with-ssms"></a>SSMS ile bağlanma

Aşağıdaki adımlarda, Azure VM'niz için isteğe bağlı bir DNS etiketi oluşturmak ve sonra SQL Server Management Studio (SSMS) bağlanma gösterilmektedir.

[!INCLUDE [Connect to SQL Server in a VM Resource Manager](../../../../includes/virtual-machines-sql-server-connection-steps-resource-manager.md)]

## <a id="manual"></a> El ile yapılandırma ve sorun giderme

Portal otomatik olarak bağlantı yapılandırma seçenekleri sağlasa da, bağlantıyı el ile yapılandırma bilmek yararlıdır. Gereksinimleri anlama da sorun giderme konusunda yardımcı olabilir.

Aşağıdaki tabloda, bir Azure sanal Makinesinde çalışan SQL Server'a bağlanmak için gereken listelenmektedir.

| Gereksinim | Açıklama |
|---|---|
| [SQL Server kimlik doğrulaması modunu etkinleştirin](https://docs.microsoft.com/sql/database-engine/configure-windows/change-server-authentication-mode#SSMSProcedure) | SQL Server kimlik doğrulaması, bir sanal ağda Active Directory yapılandırmadığınız sürece VM'ye uzaktan bağlanmak için gereklidir. |
| [Bir SQL oturum açma oluşturma](https://docs.microsoft.com/sql/relational-databases/security/authentication-access/create-a-login) | SQL kimlik doğrulaması kullanıyorsanız, bir SQL oturum açma kullanıcı adı ve hedef veritabanınıza izinlere de sahip bir parola gerekir. |
| [TCP/IP protokolünü etkinleştirin](#manualtcp) | SQL Server TCP üzerinden bağlantılara izin vermesi gerekir. |
| [SQL Server bağlantı noktası için güvenlik duvarı kuralını etkinleştirme](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access) | VM Güvenlik duvarını SQL Server bağlantı noktası (varsayılan 1433) gelen trafiğe izin vermeniz gerekir. |
| [TCP 1433 için bir ağ güvenlik grubu kuralı oluşturma](../../../virtual-network/manage-network-security-group.md#create-a-security-rule) | VM, internet üzerinden bağlanmak istiyorsanız (varsayılan 1433) SQL Server bağlantı noktası üzerinde trafiği almak izin vermeniz gerekir. Yerel ve sanal ağ-yalnızca bağlantıları bu gerektirmez. Azure portalında gereken tek adım budur. |

> [!TIP]
> Portalda bağlantısını yapılandırırken yukarıdaki tabloda adımlarda sizin için gerçekleştirilir. Yapılandırmanızı doğrulamak veya el ile bağlantı kurmak için bu adımları yalnızca kullanın SQL Server için.

## <a name="next-steps"></a>Sonraki Adımlar

Bağlantı adımları birlikte yönergeleri sağlama görmek için bkz. [azure'da bir SQL Server sanal makinesi sağlama](virtual-machines-windows-portal-sql-server-provision.md).

Azure Vm'lerinde SQL Server çalıştırma ile ilgili diğer konular için bkz [Azure Virtual Machines'de SQL Server](virtual-machines-windows-sql-server-iaas-overview.md).