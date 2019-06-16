---
title: Azure SQL veritabanı yönetilen örneği, uygulama bağlayın | Microsoft Docs
description: Bu makalede, uygulamanızı Azure SQL veritabanı yönetilen örneğine bağlanma açıklanır.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: sstein, bonova, carlrab
manager: craigg
ms.date: 11/09/2018
ms.openlocfilehash: 6cbfdc9e595ebdf682356990ec975dbd0514035d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66297085"
---
# <a name="connect-your-application-to-azure-sql-database-managed-instance"></a>Azure SQL veritabanı yönetilen örneği uygulamanızı bağlayın

Bugün nasıl ve nerede uygulamanızı barındırmak verirken birden çok seçeneğiniz vardır.

Azure App Service veya Azure App Service ortamı, sanal makine, sanal makine ölçek kümesi gibi Azure'nın tümleşik sanal ağ (VNet) seçeneklerinden bazılarını kullanılarak uygulamayı bulutta barındırmak tercih edebilirsiniz. Ayrıca hibrit bulut yaklaşımı benimseyin ve uygulamaları şirket içi tutun.

Oluşturduğunuz her seçenek bir yönetilen örneğe bağlanabilir.  

![yüksek kullanılabilirlik](./media/sql-database-managed-instance/application-deployment-topologies.png)

## <a name="connect-an-application-inside-the-same-vnet"></a>Aynı sanal ağ içindeki bir uygulama bağlama

Bu tasarımıdır senaryodur. Sanal makineler VNet içinde içinde farklı alt ağlarda olsalar bile birbirlerine doğrudan bağlanabilirsiniz. Tüm Azure uygulama ortamı veya sanal makine içinde uygulama bağlanmak için gereken bağlantı dizesi uygun şekilde ayarlanmış olması anlamına gelir.  

## <a name="connect-an-application-inside-a-different-vnet"></a>Farklı bir sanal ağ içindeki bir uygulama bağlama

Yönetilen örnek kendi Vnet'ine özel IP adresi olduğundan bu biraz daha karmaşık bir senaryodur. Bağlanmak için bir uygulama yönetilen örneği dağıtıldığı sanal ağa erişimi gerekir. Bu nedenle, ilk, uygulama ve yönetilen örnek VNet arasında bir bağlantı yapmanız gerekir. Sanal ağlar bu senaryonun işe yaraması için sırayla aynı abonelikte olması gerekmez.

Sanal ağları bağlama için iki seçenek vardır:

- [Azure sanal ağ eşlemesi](../virtual-network/virtual-network-peering-overview.md)
- VNet-VNet VPN ağ geçidi ([Azure portalında](../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md), [PowerShell](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md), [Azure CLI](../vpn-gateway/vpn-gateway-howto-vnet-vnet-cli.md))

Eşleme Microsoft omurga ağı bu nedenle, bağlantı açısından kullandığından tercih bir eşleme seçenektir, eşlenmiş VNet ve aynı VNet içindeki sanal makineler arasında gecikme dikkat çekici fark yoktur. VNet eşlemesi, aynı bölgede ağlara sınırlıdır.  

> [!IMPORTANT]
> Yönetilen örneği için sanal ağ eşleme senaryo ağlara nedeniyle aynı bölgede sınırlı [sınırlamalar genel sanal ağ eşlemesi](../virtual-network/virtual-network-manage-peering.md#requirements-and-constraints).

## <a name="connect-an-on-premises-application"></a>Şirket içi uygulamaya bağlanma

Yönetilen örnek, yalnızca özel bir IP adresi erişilebilir. Şirket içinden erişmek için uygulama ve yönetilen örnek sanal ağ arasında siteden siteye bağlantı yapmanız gerekir.

Şirket içi Azure Vnet'e bağlanma iki seçenek vardır:

- Siteden siteye VPN bağlantısı ([Azure portalında](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md), [PowerShell](../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md), [Azure CLI](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-cli.md))
- [ExpressRoute](../expressroute/expressroute-introduction.md) bağlantı  

Şirket içi Azure bağlantısı başarıyla kuruldu ve yönetilen örnek bağlantı kuramıyor, güvenlik duvarınızı yeniden yönlendirme için bağlantı noktası aralığını 11000 11999 yanı sıra SQL bağlantı noktası 1433 üzerinde giden bağlantı olup olmadığını denetleyin.

## <a name="connect-an-application-on-the-developers-box"></a>Geliştiriciler kutusunda uygulamayı bağlama

Yönetilen örnek yalnızca özel bir IP adresi, geliştirici kutusundan erişmek için bu nedenle erişilebilir, önce Geliştirici box'ınızı ve yönetilen örnek VNet arasında bir bağlantı yapmanız gerekir. Bunu yapmak için yerel Azure sertifika doğrulaması kullanarak bir sanal ağa noktadan siteye bağlantı yapılandırın. Daha fazla bilgi için [şirket içi bilgisayardan bir Azure SQL veritabanı yönetilen örneğine bağlanmak için noktadan siteye bağlantı yapılandırma](sql-database-managed-instance-configure-p2s.md).

## <a name="connect-from-on-premises-with-vnet-peering"></a>Şirket içinden sanal ağ eşlemesi ile bağlanma

Müşteriler tarafından uygulanan başka bir VPN ağ geçidi ayrı bir sanal ağ ve bir abonelikten bir barındırma yönetilen örneği, yüklü olduğu bir senaryodur. Ardından eşlenen iki sanal ağ. Aşağıdaki örnek mimarisi diyagramı, bunu nasıl uygulanabileceği gösterilmektedir.

![VNet eşlemesi](./media/sql-database-managed-instance-connect-app/vnet-peering.png)

Temel altyapıyı ayarlamak aldıktan sonra VPN ağ geçidi yönetilen örneğini barındıran sanal ağdaki IP adresleri görebilmeniz için bazı ayarı değiştirmeniz gerekir. Bunu yapmak için aşağıdaki belirli değişiklik **eşleme ayarları**.

1. VPN ağ geçidini barındıran Vnet'te Git **eşlemeler**sonra yönetilen örneği'ne VNet bağlantısı eşlenen ve ardından **ağ geçidi aktarımına izin ver**.
2. Yönetilen örneğini barındıran sanal ağda Git **eşlemeler**sonra VPN ağ geçidi için sanal ağ bağlantısını eşlenmiş ve ardından **uzak ağ geçitlerini kullan**.

## <a name="connect-an-azure-app-service-hosted-application"></a>Azure App Service, barındırılan uygulamayı bağlama

Azure App Service'ten erişmek için öncelikle uygulamayı yönetilen örnek Vnet'iniz arasında bağlantı yapmak gerekir böylece yönetilen örneği yalnızca özel bir IP adresi erişilebilir. Bkz: [uygulamanızı bir Azure sanal ağı ile tümleştirme](../app-service/web-sites-integrate-with-vnet.md).  

Sorun giderme için bkz: [sorun giderme sanal ağlar ve uygulamaları](../app-service/web-sites-integrate-with-vnet.md#troubleshooting). Bir bağlantı kurulamıyorsa deneyin [ağ yapılandırmasını senkronize](sql-database-managed-instance-sync-network-configuration.md).

Tümleştirildiğinde, Azure App Service ağ yönetilen örneğini sanal ağa eşlenen Azure App Service, yönetilen örneğe bağlanma özel bir durum oluşturur. Bu durumda, ayarlanması için şu yapılandırmayı gerektirir:

- Yönetilen örnek sanal ağ geçidi sahip olmamalıdır  
- Yönetilen örnek sanal ağ kullanım uzak ağ geçitlerini seçenek kümesi olmalıdır
- Eşlenmiş VNet izin ağ geçidi geçiş seçeneği ayarlanmış olması gerekir

Bu senaryo, aşağıdaki diyagramda gösterilmiştir:

![tümleşik uygulama eşlemesi](./media/sql-database-managed-instance/integrated-app-peering.png)

>[!NOTE]
>VNet tümleştirme özelliğini bir uygulama, bir ExpressRoute ağ geçidi olan bir VNet ile tümleşmez. ExpressRoute ağ geçidi bir arada bulunma modunda yapılandırılmış olsa bile, sanal ağ tümleştirmesi çalışmaz. Ardından ExpressRoute bağlantısı yoluyla kaynaklara erişmeye ihtiyacınız varsa, sanal ağınızda çalışır bir App Service ortamı kullanabilirsiniz.

## <a name="troubleshooting-connectivity-issues"></a>Bağlantı sorunlarını giderme

Bağlantı sorunlarını gidermek için aşağıdakileri gözden geçirin:

- Aynı sanal ağa ancak farklı bir alt ağ içinde bir Azure sanal makinesinden yönetilen örneğe bağlanma erişimi engelleme VM alt ağı üzerinde ayarlanmış bir ağ güvenlik grubu olup olmadığını denetleyin. Ayrıca unutmayın bu Azure sınırının içindeki yeniden yönlendirmesi aracılığıyla bağlanmak için gerekli olan bu yana 11000 11999 aralığında bağlantı noktalarının yanı sıra SQL bağlantı noktası 1433 üzerinde giden bağlantı açmanız gerekir.
- BGP yayma ayarlandığından emin olun **etkin** sanal ağ ile ilişkili yol tablosuna için.
- P2S VPN kullanarak, görüyorsanız, görmek için Azure portalında yapılandırmayı denetleyin. **giriş/çıkış** sayı. Sıfır olmayan sayılar, Azure için buralardan şirket içi trafiği yönlendirme olduğunu gösterir.

   ![Giriş/Çıkış numaraları](./media/sql-database-managed-instance-connect-app/ingress-egress-numbers.png)

- (Çalışan VPN istemcisi) istemci makine erişmesi gereken tüm sanal ağları için rota girişleri olup olmadığını denetleyin. Yollar depolanan `%AppData%\ Roaming\Microsoft\Network\Connections\Cm\<GUID>\routes.txt`.

   ![route.txt](./media/sql-database-managed-instance-connect-app/route-txt.png)

   Bu görüntüde gösterildiği gibi katılan her sanal ağa iki girişe ve Portalı'nda yapılandırılmış VPN uç noktası için üçüncü bir giriş vardır.

   Yollar denetlemek için başka bir aşağıdaki komutu yoludur. Çıktı, çeşitli alt ağlar için yol gösterir:

   ```cmd
   C:\ >route print -4
   ===========================================================================
   Interface List
   14...54 ee 75 67 6b 39 ......Intel(R) Ethernet Connection (3) I218-LM
   57...........................rndatavnet
   18...94 65 9c 7d e5 ce ......Intel(R) Dual Band Wireless-AC 7265
   1...........................Software Loopback Interface 1
   Adapter===========================================================================

   IPv4 Route Table
   ===========================================================================
   Active Routes:
   Network Destination        Netmask          Gateway       Interface  Metric
          0.0.0.0          0.0.0.0       10.83.72.1     10.83.74.112     35
         10.0.0.0    255.255.255.0         On-link       172.26.34.2     43
         10.4.0.0    255.255.255.0         On-link       172.26.34.2     43
   ===========================================================================
   Persistent Routes:
   None
   ```

- VNet eşlemesi kullanıyorsanız, ayar için yönergeleri izlediğinizden emin olun [ağ geçidi aktarımına izin ver ve uzak ağ geçitlerini kullan](#connect-from-on-premises-with-vnet-peering).

## <a name="required-versions-of-drivers-and-tools"></a>Gerekli sürücüleri ve araçlarını sürümleri

Yönetilen örneğe bağlanmak istiyorsanız aşağıdaki en düşük sürümleri araçları ve sürücüleri önerilir:

| Sürücü/aracı | Version |
| --- | --- |
|.NET Framework | 4.6.1 (veya .NET Core) |
|ODBC sürücüsü| v17 |
|PHP sürücüsü| 5.2.0 |
|JDBC sürücüsü| 6.4.0 |
|Node.js sürücüsü| 2.1.1 |
|OLEDB sürücüsü| 18.0.2.0 |
|SSMS| 18.0 veya [daha yüksek](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) |
|[SMO](https://docs.microsoft.com/sql/relational-databases/server-management-objects-smo/sql-server-management-objects-smo-programming-guide) | [150](https://www.nuget.org/packages/Microsoft.SqlServer.SqlManagementObjects) veya üzeri |

## <a name="next-steps"></a>Sonraki adımlar

- Yönetilen Örnek hakkında daha fazla bilgi için bkz. [Yönetilen Örnek nedir?](sql-database-managed-instance.md).
- Yeni bir yönetilen örneğin nasıl oluşturulacağını gösteren bir öğretici için bkz [bir yönetilen örnek oluşturma](sql-database-managed-instance-get-started.md).
