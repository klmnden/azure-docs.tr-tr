---
title: Yük Dengeleyici yapılandırmasına her zaman için SQL Server | Microsoft Docs
description: SQL Server Always On ile çalışır ve bir yük dengeleyici SQL Server uygulaması oluşturmak için PowerShell kullanmayı öğrenmek için yük dengeleyici yapılandırma
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
ms.openlocfilehash: a0c2345b47b9103ac6a7ae998f13a12332e3907e
ms.sourcegitcommit: c3d53d8901622f93efcd13a31863161019325216
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2018
ms.locfileid: "30262526"
---
# <a name="configure-a-load-balancer-for-sql-server-always-on"></a>SQL Server Always On için bir yük dengeleyici yapılandırmak



SQL Server Always On kullanılabilirlik grupları, bir iç yük dengeleyici ile artık çalıştırabilirsiniz. Bir kullanılabilirlik grubu SQL Server'ın flagship için yüksek kullanılabilirlik ve olağanüstü durum kurtarma çözümüdür. Kullanılabilirlik grubu dinleyicisi istemci yapılandırmasında çoğaltmaların sayısı yedeklemiş birincil çoğaltma sorunsuz bir şekilde bağlanmak uygulamaları sağlar.

Dinleyici (DNS) adı, bir yük dengeli IP adresine eşlenir. Azure yük dengeleyici yalnızca birincil sunucu için gelen trafiğe kopya kümesinde yönlendirir.

İç yük dengeleyici desteği SQL Server Always On (dinleyici) uç noktaları için kullanabilirsiniz. Şimdi dinleyicisi erişilebilirliğini denetime sahip olursunuz. Yük dengeli IP adresi, sanal ağınızda bulunan belirli bir alt ağdan seçebilirsiniz.

Yük Dengeleyici dinleyicisi, SQL sunucusu uç noktası üzerinde bir iç kullanarak (örneğin, sunucu tcp:ListenerName, 1433; = veritabanı DatabaseName =) yalnızca tarafından erişilebilen:

* Hizmetleri ve sanal makineleri aynı sanal ağda.
* Hizmetleri ve sanal makineleri bağlı şirket içi ağlardan.
* Hizmetleri ve sanal makineleri birbirine bağlı sanal ağlardan.

![SQL Server Always On iç yük dengeleyici](./media/load-balancer-configure-sqlao/sqlao1.png)

## <a name="add-an-internal-load-balancer-to-the-service"></a>Hizmet için bir iç yük dengeleyici Ekle

1. Aşağıdaki örnekte, 'Alt ağ-1' adlı bir alt ağ içeren bir sanal ağ yapılandırın:

    ```powershell
    Add-AzureInternalLoadBalancer -InternalLoadBalancerName ILB_SQL_AO -SubnetName Subnet-1 -ServiceName SqlSvc
    ```
2. Bir iç yük dengeleyici için yük dengeli uç nokta her VM ekleyin.

    ```powershell
    Get-AzureVM -ServiceName SqlSvc -Name sqlsvc1 | Add-AzureEndpoint -Name "LisEUep" -LBSetName "ILBSet1" -Protocol tcp -LocalPort 1433 -PublicPort 1433 -ProbePort 59999 -ProbeProtocol tcp -ProbeIntervalInSeconds 10 -
    DirectServerReturn $true -InternalLoadBalancerName ILB_SQL_AO | Update-AzureVM

    Get-AzureVM -ServiceName SqlSvc -Name sqlsvc2 | Add-AzureEndpoint -Name "LisEUep" -LBSetName "ILBSet1" -Protocol tcp -LocalPort 1433 -PublicPort 1433 -ProbePort 59999 -ProbeProtocol tcp -ProbeIntervalInSeconds 10 -DirectServerReturn $true -InternalLoadBalancerName ILB_SQL_AO | Update-AzureVM
    ```

    Önceki örnekte bulut hizmetinde "Sqlsvc" Çalıştır "sqlsvc1" ve "sqlsvc2" adlı iki VM vardır. İç yük dengeleyici oluşturduktan sonra `DirectServerReturn` geçiş, yük dengeli uç noktalar iç yük dengeleyiciye ekleyin. Yük dengeli uç noktaları kullanılabilirlik grupları için dinleyicileri yapılandırmak için SQL Server izin verin.

SQL Server Always On hakkında daha fazla bilgi için bkz: [Azure'da bir Always On kullanılabilirlik grubu için bir iç yük dengeleyici yapılandırma](../virtual-machines/windows/sql/virtual-machines-windows-portal-sql-alwayson-int-listener.md).

## <a name="see-also"></a>Ayrıca bkz.
* [Bir genel yük dengeleyiciye yapılandırma kullanmaya başlama](load-balancer-get-started-internet-arm-ps.md)
* [Bir iç yük dengeleyici yapılandırmaya başlayın](load-balancer-get-started-ilb-arm-ps.md)
* [Yük dengeleyici dağıtım modu yapılandırma](load-balancer-distribution-mode.md)
* [Yük dengeleyiciniz için boşta TCP zaman aşımı ayarlarını yapılandırma](load-balancer-tcp-idle-timeout.md)
