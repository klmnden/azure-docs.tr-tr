---
title: Bir SQL Server sanal makinesi (Klasik) azure'da bağlanın | Microsoft Docs
description: Azure'da bir sanal makinede çalışan SQL Server'a bağlanma hakkında bilgi edinin. Bu konuda, Klasik dağıtım modeli kullanır. Senaryo ağ yapılandırmasını ve istemcinin konumuna bağlı olarak farklılık gösterir.
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
manager: craigg
tags: azure-service-management
ms.assetid: 416948af-454f-4cfe-8fd2-7cf971cbd3e9
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 01/31/2017
ms.author: mathoma
ms.reviewer: jroth
experimental_id: d51f3cc6-753b-4e
ms.openlocfilehash: 51694ca085e131150217ffb3fbac9830980108cb
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62108450"
---
# <a name="connect-to-a-sql-server-virtual-machine-on-azure-classic-deployment"></a>Azure’daki bir SQL Server Sanal Makinesi’ne Bağlanma (Klasik Dağıtım)
> [!div class="op_single_selector"]
> * [Resource Manager](../sql/virtual-machines-windows-sql-connect.md)
> * [Klasik](../classic/sql-connect.md)
> 
> 

## <a name="overview"></a>Genel Bakış
Bu konu, bir Azure sanal makinesinde çalışan SQL Server Örneğinize bağlanmak açıklar. Bazılarını içermektedir [genel bağlantı senaryoları](#connection-scenarios) ve daha sonra sağlar [ayrıntılı adımlar için bir Azure VM'de SQL Server bağlantısını yapılandırmayla](#steps-for-configuring-sql-server-connectivity-in-an-azure-vm).

> [!IMPORTANT] 
> Azure'da oluşturmaya ve kaynaklarla çalışmaya yönelik iki farklı dağıtım modeli vardır: [Resource Manager ve klasik](../../../azure-resource-manager/resource-manager-deployment-model.md). Bu makalede, Klasik dağıtım modelini incelemektedir. Microsoft, yeni dağıtımların çoğunun Resource Manager modelini kullanmasını önerir. Resource Manager Vm'lerinde kullanıyorsanız bkz [Resource Manager'ı kullanarak azure'da SQL Server sanal makinesine bağlanma](../sql/virtual-machines-windows-sql-connect.md).

## <a name="connection-scenarios"></a>Bağlantı senaryoları
SQL Server'ın bir sanal makine üzerinde çalışan bir istemcinin bağlandığı şekilde istemci ve makine/ağ yapılandırma konumuna bağlı olarak farklılık gösterir. Bu senaryolar şunlardır:

* [Aynı bulut hizmetinde SQL Server'a bağlanma](#connect-to-sql-server-in-the-same-cloud-service)
* [İnternet üzerinden SQL Server'a bağlanma](#connect-to-sql-server-over-the-internet)
* [Aynı sanal ağda SQL Server'a bağlanma](#connect-to-sql-server-in-the-same-virtual-network)

> [!NOTE]
> Bu yöntemlerin herhangi biriyle ile bağlamadan önce izlemeniz gereken [bağlantısı yapılandırmak için bu makaledeki adımları](#steps-for-configuring-sql-server-connectivity-in-an-azure-vm).
> 
> 

### <a name="connect-to-sql-server-in-the-same-cloud-service"></a>Aynı bulut hizmetinde SQL Server'a bağlanma
Birden çok sanal makine aynı bulut hizmetinde oluşturulabilir. Bu sanal makineler senaryo anlamak için bkz: [sanal makineleri bir sanal ağ veya Bulut hizmeti ile bağlama](/previous-versions/azure/virtual-machines/windows/classic/connect-vms-classic#connect-vms-in-a-standalone-cloud-service). Bu senaryoda, aynı bulut hizmetinde başka bir sanal makinede çalışan SQL Server'a bağlanmak bir istemci bir sanal makine üzerinde çalışır.

Bu senaryoda, sanal Makineyi kullanarak bağlantı kurabilir **adı** (Ayrıca olarak gösterilen **bilgisayar adı** veya **ana bilgisayar adı** Portalı'nda). Bu VM için oluşturma sırasında sağladığınız addır. Örneğin, SQL VM'nizi adlı **mysqlvm**, aynı bulut hizmetindeki bir istemci VM bağlanmak için şu bağlantı dizesini kullanabilirsiniz:

    "Server=mysqlvm;Integrated Security=false;User ID=<login_name>;Password=<your_password>"

### <a name="connect-to-sql-server-over-the-internet"></a>Internet üzerinden SQL Server'a bağlanma
SQL Server veritabanı altyapısına Internet'ten bağlanmak istiyorsanız, gelen TCP iletişimi için bir sanal makine uç noktası oluşturmanız gerekir. Bu Azure yapılandırma adımı, gelen TCP bağlantı noktası trafiğini sanal makinenin erişebildiği bir TCP bağlantı noktasına yönlendirir.

İnternet üzerinden bağlanmak için sanal makinenin DNS adını ve VM uç nokta bağlantı noktası numarasını (Bu makalenin sonraki bölümlerinde yapılandırılmış) kullanmanız gerekir. DNS adını bulmak için Azure portalına gidin ve seçin **sanal makineler (Klasik)**. Ardından, sanal makineyi seçin. **DNS adı** gösterilen **genel bakış** bölümü.

Örneğin, klasik bir sanal makine adlı düşünün **mysqlvm** DNS adı ile **mysqlvm7777.cloudapp.net** ve bir sanal makine uç noktası **57500**. Düzgün bir şekilde yapılandırılmış bağlantı varsayarsak, aşağıdaki bağlantı dizesi sanal makineyi dilediğiniz yerde erişmek için kullanılabilir internet üzerinde:

    "Server=mycloudservice.cloudapp.net,57500;Integrated Security=false;User ID=<login_name>;Password=<your_password>"

Bu bağlantı dizesini bağlantı istemciler için internet üzerinden sağlar, ancak bu herkes SQL Server'ınıza bağlanmak göstermez. Dışında doğru kullanıcı adını ve parolayı istemciler vardır. Ek güvenlik için iyi bilinen bağlantı noktası 1433 genel sanal makine uç noktası için kullanmayın. Ve mümkün olduğunda, bir ACL'yi eklemeyi göz önünde bulundurun yalnızca istemcilere trafiği kısıtlamak için uç noktasında izin verir. ACL'leri kullanarak uç noktaları ile ilgili yönergeler için bkz: [bir uç nokta ACL'sini Yönet](/previous-versions/azure/virtual-machines/windows/classic/setup-endpoints#manage-the-acl-on-an-endpoint).

> [!NOTE]
> SQL Server ile iletişim kurmak için bu yöntemi kullandığınızda, normal tabi olan Azure veri merkezinden tüm giden veri [üzerinde giden veri aktarımları fiyatlandırma](https://azure.microsoft.com/pricing/details/data-transfers/).
> 
> 

### <a name="connect-to-sql-server-in-the-same-virtual-network"></a>Aynı sanal ağda SQL Server'a bağlanma
[Sanal ağ](../../../virtual-network/virtual-networks-overview.md) ek senaryoların gerçekleşmesine olanak tanır. Bu sanal makineler farklı bulut hizmetlerinde mevcut olsa bile, Vm'leri aynı sanal ağda bağlanabilirsiniz. İle bir [siteden siteye VPN](../../../vpn-gateway/vpn-gateway-site-to-site-create.md), makineleri şirket içi ağlar ile Vm'leri bağlanan karma mimarisi oluşturabilirsiniz.

Sanal ağları Azure Vm'lerinizi bir etki alanına sağlar. Bir etki alanına katılan Windows kimlik doğrulaması ile SQL Server kullanmak için tek yoludur. Bir bağlantı senaryoları, kullanıcı adları ve parolaları ile SQL kimlik doğrulaması gerektirir.

Bir etki alanı ortamı ve Windows kimlik doğrulaması yapılandırmayı planlıyorsanız, genel bir uç nokta veya SQL kimlik doğrulaması ve oturum açma bilgileri yapılandırma gerekmez. Bu senaryoda, SQL Server örneğinizi SQL Server VM adı bağlantı dizesinde belirterek bağlanabilirsiniz. Aşağıdaki örnek, Windows kimlik doğrulaması yapılandırıldı ve kullanıcı SQL Server örneğine erişim verildi varsayar.

    "Server=mysqlvm;Integrated Security=true"

## <a name="steps-for-configuring-sql-server-connectivity-in-an-azure-vm"></a>Bir Azure VM'de SQL Server bağlantısı yapılandırma adımları
Aşağıdaki adımlarda, SQL Server Management Studio (SSMS) kullanarak internet üzerinden SQL Server örneğine bağlanmak nasıl ekleyebileceğiniz gösterilmektedir. Ancak, SQL Server sanal makinenizi Azure ve hem şirket içinde çalışan uygulamalarınız için erişilebilir hale getirmek için aynı adımları uygulayın.

SQL Server örneğine başka bir VM veya İnternet'e bağlanabilmesi için önce aşağıdaki görevleri tamamlamanız gerekir:

* [Sanal makine için bir TCP uç noktası oluşturma](#create-a-tcp-endpoint-for-the-virtual-machine)
* [Windows Güvenlik duvarında TCP bağlantı noktaları açma](#open-tcp-ports-in-the-windows-firewall-for-the-default-instance-of-the-database-engine)
* [SQL Server'ı TCP protokolünde dinleyecek şekilde yapılandırma](#configure-sql-server-to-listen-on-the-tcp-protocol)
* [SQL Server'ı karma mod kimlik doğrulaması için yapılandırma](#configure-sql-server-for-mixed-mode-authentication)
* [SQL Server kimlik doğrulaması oturum açma kimliği oluşturma](#create-sql-server-authentication-logins)
* [Sanal makinenin DNS adını belirleme](#determine-the-dns-name-of-the-virtual-machine)
* [Başka bir bilgisayardan veritabanı altyapısına Bağlan](#connect-to-the-database-engine-from-another-computer)

Aşağıdaki diyagram tarafından bağlantı yolu özetlenmiştir:

![Bir SQL Server sanal makinesine bağlanma](../../../../includes/media/virtual-machines-sql-server-connection-steps/SQLServerinVMConnectionMap.png)

[!INCLUDE [Connect to SQL Server in a VM Classic TCP Endpoint](../../../../includes/virtual-machines-sql-server-connection-steps-classic-tcp-endpoint.md)]

[!INCLUDE [Connect to SQL Server in a VM](../../../../includes/virtual-machines-sql-server-connection-steps.md)]

[!INCLUDE [Connect to SQL Server in a VM Classic Steps](../../../../includes/virtual-machines-sql-server-connection-steps-classic.md)]

## <a name="next-steps"></a>Sonraki Adımlar
Yüksek kullanılabilirlik ve olağanüstü durum kurtarma için AlwaysOn Kullanılabilirlik grupları kullanmayı planlıyor musunuz, dinleyici uygulama düşünmelisiniz. Veritabanı istemcileri dinleyici yerine doğrudan SQL Server örneklerinden birine bağlanın. Dinleyici istemciler kullanılabilirlik grubunda birincil çoğaltmaya yönlendirir. Daha fazla bilgi için [Azure'da AlwaysOn Kullanılabilirlik grupları için ILB dinleyicisi yapılandırma](../classic/ps-sql-int-listener.md).

Azure sanal makinesinde çalışan SQL Server için tüm güvenlik en iyi uygulamaları incelemek önemlidir. Daha fazla bilgi için bkz. [Azure Sanal Makineler'de SQL Server için Güvenlikle İlgili Dikkat Edilmesi Gerekenler](../sql/virtual-machines-windows-sql-security.md).

Azure Virtual Machines’de SQL Server için.[Öğrenme Yolunu keşfedin](https://azure.microsoft.com/documentation/learning-paths/sql-azure-vm/). 

Azure Vm'lerinde SQL Server çalıştırma ile ilgili diğer konular için bkz [Azure Virtual Machines'de SQL Server](../sql/virtual-machines-windows-sql-server-iaas-overview.md).

