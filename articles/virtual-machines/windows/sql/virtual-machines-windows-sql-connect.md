---
title: "Bir SQL Server sanal makinesine (Resource Manager) bağlanma | Microsoft Docs"
description: "Azure'da bir sanal makinede çalışan SQL Server bağlayacağınızı öğrenin. Bu konu Klasik dağıtım modelini kullanır. Senaryoları ağ yapılandırmasını ve istemcinin konumu bağlı olarak farklılık gösterir."
services: virtual-machines-windows
documentationcenter: na
author: rothja
manager: jhubbard
tags: azure-resource-manager
ms.assetid: aa5bf144-37a3-4781-892d-e0e300913d03
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 08/14/2017
ms.author: jroth
ms.openlocfilehash: 67ba43f9456bbeffbf602067586143c4c68af672
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="connect-to-a-sql-server-virtual-machine-on-azure-resource-manager"></a>Azure’daki bir SQL Server Sanal Makinesi’ne Bağlanma (Resource Manager)
> [!div class="op_single_selector"]
> * [Resource Manager](virtual-machines-windows-sql-connect.md)
> * [Klasik](../classic/sql-connect.md)
> 
> 

## <a name="overview"></a>Genel Bakış

Bu konuda, bir Azure sanal makinede çalışan SQL Server örneğine bağlanmak açıklar. Bazı kapsayan [genel bağlantı senaryoları](#connection-scenarios) ve ardından sağlar [ayrıntılı bir Azure VM'de SQL Server bağlantısı yapılandırma adımları](#steps-for-manually-configuring-sql-server-connectivity-in-an-azure-vm).

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

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

Özel bağlantı ile birlikte kullanılan genellikle [sanal ağ](../../../virtual-network/virtual-networks-overview.md), birkaç senaryo sağlar. Bu sanal makineleri farklı bir kaynak grubu mevcut olsa bile aynı sanal ağda VM'ler bağlanabilir. İle bir [siteden siteye VPN](../../../vpn-gateway/vpn-gateway-site-to-site-create.md), şirket içi ağlar ve makineler VM'ler bağlayan bir karma mimarisi oluşturabilirsiniz.

Sanal ağlar Azure Vm'leriniz için bir etki alanına katılmak sağlar. Bu, SQL Server için Windows kimlik doğrulamasını kullanmak için tek yoludur. Diğer bağlantı senaryolar, kullanıcı adlarını ve Parolaları SQL kimlik doğrulaması gerektirir.

Sanal ağınıza DNS yapılandırmış olduğunuz varsayılarak, bağlantı dizesinde SQL Server VM bilgisayar adını belirterek SQL Server örneğinizi bağlanabilir. Aşağıdaki örnek, ayrıca Windows kimlik doğrulaması da yapılandırılmış olduğunu ve kullanıcının SQL Server örneğine erişimi verildiğini varsayar.

```
Server=mysqlvm;Integrated Security=true
```

## <a id="change"></a>SQL bağlantı ayarlarını değiştirme

SQL Server sanal makinenizi Azure portalında için bağlantı ayarlarını değiştirebilirsiniz.

1. Azure portalında seçin **sanal makineleri**.

2. SQL Server VM'yi seçin.

3. Altında **ayarları**, tıklatın **SQL Server yapılandırma**.

4. Değişiklik **SQL bağlantı düzeyi** gerekli ayarınız için. İsteğe bağlı olarak, SQL Server bağlantı noktası veya SQL kimlik doğrulaması ayarlarını değiştirmek için bu alanı kullanabilirsiniz.

   ![SQL Bağlantısı değiştirme](./media/virtual-machines-windows-sql-connect/sql-vm-portal-connectivity-change.png)

5. Güncelleştirmenin tamamlanması birkaç dakika bekleyin.

   ![SQL VM güncelleştirme bildirimi](./media/virtual-machines-windows-sql-connect/sql-vm-updating-notification.png)

## <a id="manualtcp"></a>Geliştirici ve Express sürümleri için TCP/IP'yi etkinleştirin

SQL Server bağlantı ayarları değiştirirken, Azure otomatik olarak TCP/IP Protokolü SQL Server Developer ve Express sürümleri için izin vermez. Aşağıdaki adımlarda, uzaktan IP adresiyle bağlanabilmeniz için TCP/IP’yi el ile nasıl etkinleştirebileceğiniz açıklanmıştır.

İlk olarak, Uzak Masaüstü ile SQL Server makinesinde bağlayın.

> [!INCLUDE [Connect to SQL Server VM with remote desktop](../../../../includes/virtual-machines-sql-server-remote-desktop-connect.md)]

Ardından, TCP/IP protokolü ile etkinleştirmek **SQL Server Configuration Manager**.

> [!INCLUDE [Connect to SQL Server VM with remote desktop](../../../../includes/virtual-machines-sql-server-connection-tcp-protocol.md)]

## <a name="connect-with-ssms"></a>SSMS ile bağlanma

Aşağıdaki adımlar, Azure VM için isteğe bağlı bir DNS etiketi oluşturmak ve ardından SQL Server Management Studio (SSMS) ile bağlanmak nasıl gösterir.

[!INCLUDE [Connect to SQL Server in a VM Resource Manager](../../../../includes/virtual-machines-sql-server-connection-steps-resource-manager.md)]

## <a name="next-steps"></a>Sonraki Adımlar

Bağlantı adımları birlikte yönergeleri sağlama görmek için bkz: [Azure üzerinde bir SQL Server sanal makine sağlama](virtual-machines-windows-portal-sql-server-provision.md).

Azure Vm'lerde SQL Server çalıştırma ile ilgili diğer konular için bkz: [Azure Virtual Machines'de SQL Server](virtual-machines-windows-sql-server-iaas-overview.md).