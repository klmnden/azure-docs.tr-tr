---
title: "Yük Dengeleyici yapılandırmasına her zaman için SQL | Microsoft Docs"
description: "SQL ile her zaman çalışmak üzere yük dengeleyici ve yük dengeleyici SQL uygulaması oluşturmak için powershell yararlanmak nasıl yapılandırın"
services: load-balancer
documentationcenter: na
author: KumudD
manager: timlt
ms.assetid: d7bc3790-47d3-4e95-887c-c533011e4afd
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: kumud
ms.openlocfilehash: 3ebbf1c4009d89b1f18b2ff8ff5dd243c456dff8
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="configure-load-balancer-for-sql-always-on"></a>Yük Dengeleyici SQL için her zaman yapılandırın

[!INCLUDE [load-balancer-basic-sku-include.md](../../includes/load-balancer-basic-sku-include.md)]

SQL Server AlwaysOn Kullanılabilirlik grupları şimdi ILB ile çalıştırabilirsiniz. Kullanılabilirlik grubu SQL Server'ın flagship için yüksek kullanılabilirlik ve olağanüstü durum kurtarma çözümüdür. Kullanılabilirlik grubu dinleyicisi istemci yapılandırmasında çoğaltmaların sayısı yedeklemiş birincil çoğaltma sorunsuz bir şekilde bağlanmak uygulamaları sağlar.

Dinleyici (DNS) adı yük dengeli bir IP adresine eşlenir ve Azure'nın yük dengeleyici yalnızca birincil sunucu için gelen trafiğe kopya kümesinde yönlendirir.

ILB desteği SQL Server AlwaysOn (dinleyici) uç noktaları için kullanabilirsiniz. Şimdi dinleyicisi erişilebilirliğini üzerinde denetimi yoktur ve yük dengeli IP adresi, sanal ağ (VNet) belirli bir alt ağdan seçebilirsiniz.

ILB dinleyicisi, SQL sunucusu uç noktası kullanarak (örneğin Server 1433; tcp:ListenerName, = veritabanı DatabaseName =) yalnızca tarafından erişilebilen:

* Hizmetleri ve sanal makineleri aynı sanal ağda
* Hizmetler ve bağlı şirket içi ağ üzerinden VM'ler
* Hizmetler ve VM'lerin birbirine bağlı sanal ağlar

![ILB_SQLAO_NewPic](./media/load-balancer-configure-sqlao/sqlao1.png)

Şekil 1 - SQL AlwaysOn Internet'e yönelik Yük Dengeleyici ile yapılandırılmış

## <a name="add-internal-load-balancer-to-the-service"></a>İç yük dengeleyici hizmete ekleyin

1. Aşağıdaki örnekte, biz 'Alt ağ-1' adlı bir alt ağ içeren bir sanal ağ yapılandırın:

    ```powershell
    Add-AzureInternalLoadBalancer -InternalLoadBalancerName ILB_SQL_AO -SubnetName Subnet-1 -ServiceName SqlSvc
    ```
2. Her bir VM üzerinde ILB için yük dengeli uç noktaları ekleme

    ```powershell
    Get-AzureVM -ServiceName SqlSvc -Name sqlsvc1 | Add-AzureEndpoint -Name "LisEUep" -LBSetName "ILBSet1" -Protocol tcp -LocalPort 1433 -PublicPort 1433 -ProbePort 59999 -ProbeProtocol tcp -ProbeIntervalInSeconds 10 -
    DirectServerReturn $true -InternalLoadBalancerName ILB_SQL_AO | Update-AzureVM

    Get-AzureVM -ServiceName SqlSvc -Name sqlsvc2 | Add-AzureEndpoint -Name "LisEUep" -LBSetName "ILBSet1" -Protocol tcp -LocalPort 1433 -PublicPort 1433 -ProbePort 59999 -ProbeProtocol tcp -ProbeIntervalInSeconds 10 -DirectServerReturn $true -InternalLoadBalancerName ILB_SQL_AO | Update-AzureVM
    ```

    Yukarıdaki örnekte, 2 VM'ın adlandırılan "sqlsvc1" ve "sqlsvc2" çalışan sahip bulutta "Sqlsvc" hizmet. ILB ile oluşturduktan sonra `DirectServerReturn` anahtarı, eklediğiniz yük dengeli uç noktalar ILB dinleyicileri kullanılabilirlik grupları için yapılandırmak SQL izin vermek için.

SQL AlwaysOn hakkında daha fazla bilgi için bkz: [Azure'da AlwaysOn Kullanılabilirlik grubu için bir iç yük dengeleyici yapılandırma](../virtual-machines/windows/sql/virtual-machines-windows-portal-sql-alwayson-int-listener.md).

## <a name="see-also"></a>Ayrıca Bkz.
[Internet'e yönelik Yük Dengeleyici yapılandırma kullanmaya başlama](load-balancer-get-started-internet-arm-ps.md)

[Bir iç yük dengeleyici yapılandırma kullanmaya başlama](load-balancer-get-started-ilb-arm-ps.md)

[Yük dengeleyici dağıtım modu yapılandırma](load-balancer-distribution-mode.md)

[Yük dengeleyiciniz için boşta TCP zaman aşımı ayarlarını yapılandırma](load-balancer-tcp-idle-timeout.md)
