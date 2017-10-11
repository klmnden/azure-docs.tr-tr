---
title: "Azure (Klasik) SQL Server sanal makineye bağlanma | Microsoft Docs"
description: "Azure'da bir sanal makinede çalışan SQL Server bağlayacağınızı öğrenin. Bu konu Klasik dağıtım modelini kullanır. Senaryoları ağ yapılandırmasını ve istemcinin konumu bağlı olarak farklılık gösterir."
services: virtual-machines-windows
documentationcenter: na
author: rothja
manager: jhubbard
tags: azure-service-management
ms.assetid: 416948af-454f-4cfe-8fd2-7cf971cbd3e9
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 01/31/2017
ms.author: jroth
experimental_id: d51f3cc6-753b-4e
ms.openlocfilehash: 4218b6d274abbeda542c1507aec998ba56f5c145
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="connect-to-a-sql-server-virtual-machine-on-azure-classic-deployment"></a>Azure’daki bir SQL Server Sanal Makinesi’ne Bağlanma (Klasik Dağıtım)
> [!div class="op_single_selector"]
> * [Resource Manager](../sql/virtual-machines-windows-sql-connect.md)
> * [Klasik](../classic/sql-connect.md)
> 
> 

## <a name="overview"></a>Genel Bakış
Bu konuda, bir Azure sanal makinede çalışan SQL Server örneğine bağlanmak açıklar. Bazı kapsayan [genel bağlantı senaryoları](#connection-scenarios) ve ardından sağlar [ayrıntılı bir Azure VM'de SQL Server bağlantısı yapılandırma adımları](#steps-for-configuring-sql-server-connectivity-in-an-azure-vm).

> [!IMPORTANT] 
> Azure oluşturmak ve kaynaklarla çalışmak için iki farklı dağıtım modeli vardır: [Resource Manager ve klasik](../../../azure-resource-manager/resource-manager-deployment-model.md). Bu makalede, Klasik dağıtım modeli kullanarak yer almaktadır. Microsoft, yeni dağıtımların çoğunun Resource Manager modelini kullanmasını önerir. Resource Manager VM kullanıyorsanız, bkz: [bir SQL Server sanal makinesine bağlanma Kaynak Yöneticisi'ni kullanarak azure'da](../sql/virtual-machines-windows-sql-connect.md).

## <a name="connection-scenarios"></a>Bağlantı senaryoları
Bir sanal makinede çalışan SQL Server bir istemcinin bağlandığı yöntemi istemci ve makine/ağ yapılandırması konumunu bağlı olarak değişir. Bu senaryolar şunlardır:

* [SQL Server'a aynı bulut hizmetinde bağlanın](#connect-to-sql-server-in-the-same-cloud-service)
* [Internet üzerinden SQL Server'a bağlanma](#connect-to-sql-server-over-the-internet)
* [Aynı sanal ağ içinde SQL Server'a bağlanma](#connect-to-sql-server-in-the-same-virtual-network)

> [!NOTE]
> Bu yöntemlerin herhangi biriyle ile bağlamadan önce izlemeniz gereken [bağlantısını yapılandırmak için bu makaledeki adımları](#steps-for-configuring-sql-server-connectivity-in-an-azure-vm).
> 
> 

### <a name="connect-to-sql-server-in-the-same-cloud-service"></a>SQL Server'a aynı bulut hizmetinde bağlanın
Birden fazla sanal makineyi aynı bulut hizmetinde oluşturulabilir. Bu sanal makineleri senaryo anlamak için bkz: [bir sanal ağ veya Bulut hizmetiyle sanal makinelere bağlanmak nasıl](../classic/connect-vms.md#connect-vms-in-a-standalone-cloud-service). Bir istemci bir sanal makineye aynı bulut hizmetinde başka bir sanal makinede çalışan SQL Server bağlanmaya çalıştığında bu senaryodur.

Bu senaryoda, VM kullanarak bağlanabilir **adı** (Ayrıca gösterilen **bilgisayar adı** veya **ana bilgisayar adı** Portalı'nda). Bu VM için oluşturma sırasında sağlanan adıdır. Örneğin, SQL VM'nizi adlı **mysqlvm**, aynı bulut hizmetindeki bir istemci VM bağlanmak için bağlantı dizesi olarak şunu kullanabilirsiniz:

    "Server=mysqlvm;Integrated Security=false;User ID=<login_name>;Password=<your_password>"

### <a name="connect-to-sql-server-over-the-internet"></a>Internet üzerinden SQL Server'a bağlanma
SQL Server veritabanı altyapısına Internet'ten bağlanmak istiyorsanız, bir sanal makine uç noktası gelen TCP iletişimi için oluşturmanız gerekir. Bu Azure yapılandırma adımı, gelen TCP bağlantı noktası trafiğini sanal makinenin erişebildiği bir TCP bağlantı noktasına yönlendirir.

Internet üzerinden bağlanmak için sanal makinenin DNS adı ve VM uç nokta bağlantı noktası numarasını (Bu makalenin sonraki bölümlerinde yapılandırılmış) kullanmanız gerekir. DNS adını bulmak için Azure Portalı'na gidin ve seçin **sanal makineleri (Klasik)**. Ardından, sanal makineyi seçin. **DNS adı** gösterilen **genel bakış** bölümü.

Örneğin, klasik bir sanal makine adlı göz önünde bulundurun **mysqlvm** bir DNS adı ile **mysqlvm7777.cloudapp.net** VM bitiş noktası **57500**. Düzgün yapılandırılmış bağlantı varsayıldığında, bağlantı dizesi olarak şunu sanal makineyi dilediğiniz yerde erişmek için kullanılabilecek Internet'te:

    "Server=mycloudservice.cloudapp.net,57500;Integrated Security=false;User ID=<login_name>;Password=<your_password>"

Bu bağlantı istemciler için internet üzerinden sağlar, ancak bu herkes SQL sunucunuza bağlanmak göstermez. Dışında doğru kullanıcı adını ve parolayı istemciler vardır. Ek güvenlik için iyi bilinen bağlantı noktası 1433 genel sanal makine uç noktası için kullanmayın. Ve mümkünse, bir ACL eklemeyi düşünün yalnızca istemcilere trafiği kısıtlamak için uç noktada izin verir. ACL'ler uç ile kullanma ile ilgili yönergeler için bkz: [bir uç nokta ACL'sini Yönet](../classic/setup-endpoints.md#manage-the-acl-on-an-endpoint).

> [!NOTE]
> SQL Server ile iletişim kurmak için bu yöntemi kullandığınızda, Azure veri merkezi tüm giden veriler olduğunu tabi normal dikkate almak önemlidir [giden veri aktarımları fiyatlandırma](https://azure.microsoft.com/pricing/details/data-transfers/).
> 
> 

### <a name="connect-to-sql-server-in-the-same-virtual-network"></a>Aynı sanal ağ içinde SQL Server'a bağlanma
[Sanal ağ](../../../virtual-network/virtual-networks-overview.md) ek senaryolara olanak sağlar. Bu sanal makineleri farklı bir bulut Hizmetleri'nde bulunsa bile sanal makineleri aynı sanal ağdaki bağlanabilir. İle bir [siteden siteye VPN](../../../vpn-gateway/vpn-gateway-site-to-site-create.md), şirket içi ağlar ve makineler VM'ler bağlayan bir karma mimarisi oluşturabilirsiniz.

Sanal ağlar da Azure sanal makineleri bir etki alanına katılma olanak sağlar. Bu, SQL Server için Windows kimlik doğrulamasını kullanmak için tek yoludur. Diğer bağlantı senaryolar, kullanıcı adlarını ve Parolaları SQL kimlik doğrulaması gerektirir.

Bir etki alanı ortamı ve Windows kimlik doğrulamasını yapılandırmak için kullanacaksanız, bu makaledeki adımları genel bir uç nokta veya SQL kimlik doğrulaması ve oturum açma bilgileri yapılandırmak gerekmez. Bu senaryoda, SQL Server örneğinizi SQL Server VM adı bağlantı dizesinde belirterek bağlanabilir. Aşağıdaki örnek, Windows kimlik doğrulaması da yapılandırılmış olduğunu ve kullanıcının SQL Server örneğine erişimi verildiğini varsayar.

    "Server=mysqlvm;Integrated Security=true"

## <a name="steps-for-configuring-sql-server-connectivity-in-an-azure-vm"></a>Bir Azure VM'de SQL Server bağlantısı yapılandırma adımları
Aşağıdaki adımlarda, SQL Server Management Studio (SSMS) kullanarak Internet üzerinden SQL Server örneğine bağlanma gösterilmektedir. Ancak, SQL Server sanal makinenizi çalıştıran her iki şirket içi uygulamalarınızı ve Azure erişilebilir hale getirme için aynı adımları uygulayın.

SQL Server örneği başka bir VM veya Internet'ten bağlanmadan önce izleyen bölümlerde açıklandığı gibi aşağıdaki görevleri tamamlamanız gerekir:

* [Sanal makine için bir TCP uç noktası oluşturma](#create-a-tcp-endpoint-for-the-virtual-machine)
* [Windows Güvenlik Duvarı'nda açık TCP bağlantı noktaları](#open-tcp-ports-in-the-windows-firewall-for-the-default-instance-of-the-database-engine)
* [SQL Server'ın TCP protokolünün dinleyecek şekilde yapılandırma](#configure-sql-server-to-listen-on-the-tcp-protocol)
* [SQL Server için karma mod kimlik doğrulamasını yapılandırma](#configure-sql-server-for-mixed-mode-authentication)
* [SQL Server kimlik doğrulama oturumları oluşturma](#create-sql-server-authentication-logins)
* [Sanal makinenin DNS adını belirleme](#determine-the-dns-name-of-the-virtual-machine)
* [Başka bir bilgisayardan veritabanı altyapısına bağlanma](#connect-to-the-database-engine-from-another-computer)

Aşağıdaki diyagram tarafından bağlantı yolu özetlenmiştir:

![Bir SQL Server sanal makinesine bağlanma](../../../../includes/media/virtual-machines-sql-server-connection-steps/SQLServerinVMConnectionMap.png)

[!INCLUDE [Connect to SQL Server in a VM Classic TCP Endpoint](../../../../includes/virtual-machines-sql-server-connection-steps-classic-tcp-endpoint.md)]

[!INCLUDE [Connect to SQL Server in a VM](../../../../includes/virtual-machines-sql-server-connection-steps.md)]

[!INCLUDE [Connect to SQL Server in a VM Classic Steps](../../../../includes/virtual-machines-sql-server-connection-steps-classic.md)]

## <a name="next-steps"></a>Sonraki Adımlar
Yüksek kullanılabilirlik ve olağanüstü durum kurtarma için AlwaysOn Kullanılabilirlik grupları kullanacak planlıyorsanız, bir dinleyici uygulama düşünmelisiniz. Veritabanı istemcileri dinleyicisi yerine SQL Server örnekleri birine doğrudan bağlanır. Dinleyici istemcileri kullanılabilirlik grubunda birincil çoğaltmaya yönlendirir. Daha fazla bilgi için bkz: [Azure'da AlwaysOn Kullanılabilirlik grupları için bir ILB dinleyicisi yapılandırın](../classic/ps-sql-int-listener.md).

Bir Azure sanal makinede çalışan SQL Server için tüm en iyi güvenlik uygulamaları gözden geçirilmesi önemlidir. Daha fazla bilgi için bkz. [Azure Sanal Makineler'de SQL Server için Güvenlikle İlgili Dikkat Edilmesi Gerekenler](../sql/virtual-machines-windows-sql-security.md).

Azure Virtual Machines’de SQL Server için.[Öğrenme Yolunu keşfedin](https://azure.microsoft.com/documentation/learning-paths/sql-azure-vm/). 

Azure Vm'lerde SQL Server çalıştırma ile ilgili diğer konular için bkz: [Azure Virtual Machines'de SQL Server](../sql/virtual-machines-windows-sql-server-iaas-overview.md).

