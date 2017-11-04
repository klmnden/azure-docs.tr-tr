---
title: "Azure'da yüksek kullanılabilirlik bağlantı noktalarını genel bakış | Microsoft Docs"
description: "Yüksek oranda kullanılabilir bağlantı noktaları yük bir iç yük dengeleyici Dengeleme hakkında bilgi edinin"
services: load-balancer
documentationcenter: na
author: rdhillon
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 46b152c5-6a27-4bfc-bea3-05de9ce06a57
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/26/2017
ms.author: kumud
ms.openlocfilehash: e72fc0d4323f7a2d203fee66311c3fea10ad7a09
ms.sourcegitcommit: 3ab5ea589751d068d3e52db828742ce8ebed4761
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2017
---
# <a name="high-availability-ports-overview-preview"></a>Yüksek kullanılabilirlik bağlantı noktalarına genel bakış (Önizleme)

Azure yük dengeleyici standart bir yeni bakiye TCP ve UDP akışları tüm bağlantı noktalarındaki aynı anda bir iç yük dengeleyici kullanırken yükleme olanağı sunar. 

>[!NOTE]
> Yüksek kullanılabilirlik bağlantı noktası özelliği, yük dengeleyici standart ve şu anda Önizleme'de kullanılabilir. Önizleme sırasında bu özellik genel kullanılabilirlik sunumundaki özelliklerle aynı seviyede kullanılabilirliğe ve güvenilirliğe sahip olmayabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Microsoft Azure Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). Yük Dengeleyici standart yük dengeleyici standart kaynaklarla HA bağlantı noktalarını kullanacak biçimde önizleme için kaydolmak gereklidir. Yönergeleri izleyin yük dengeleyici yanı sıra kaydolma [Standard Önizleme](https://aka.ms/lbpreview#preview-sign-up) de.

Bir HA bağlantı noktası kuralı bir bir Yük Dengeleme kuralı bir iç yük dengeleyici standart yapılandırılmış çeşididir.  Senaryo, bir iç yük dengeleyici standart ön uç tüm bağlantı noktalarında gelen tüm TCP ve UDP akışları yükünü dengelemek için tek bir LB kural sağlayarak basitleştirilmiştir. Yük Dengeleme karar akış 5-tanımlama grubu üzerinde kaynak IP adresi, kaynak bağlantı noktası, hedef IP adresi, hedef bağlantı noktası ve protokol tabanlı başına yapılır.

HA bağlantı noktalarını etkinleştirir kritik senaryolarda gibi çok sayıda bağlantı noktaları nerede olmalıdır diğer senaryolar yanı sıra yüksek kullanılabilirlik ve ölçek için ağ sanal Gereçleri (NVA) sanal ağlar içinde dengeli yükleyin. 

HA bağlantı noktalarını yapılandırılmış ön uç ve arka uç bağlantı noktaları ayarlayarak **0** ve protokol **tüm**.  İç yük dengeleyici kaynak bağlantı noktası numarası bağımsız olarak tüm TCP ve UDP akışlar şimdi dengeler.

## <a name="why-use-ha-ports"></a>HA bağlantı noktalarını neden kullanılır?

### <a name="nva"></a>Ağ sanal Gereçleri

Güvenlik tehditlerine birden çok tür Azure İş yükünüzün güvenliğini sağlamak için ağ sanal Gereçleri (NVA) kullanabilirsiniz. NVA bu senaryolarda kullanıldığında, güvenilir, yüksek oranda kullanılabilir ve genişletmek için isteğe bağlı olmaları gerekir.

Sadece NVA örnekleri Azure iç yük dengeleyici arka uç havuzuna ekleme ve bir HA bağlantı noktalarını yük dengeleyici kuralı yapılandırma senaryonuzda bu hedefleri elde edebilirsiniz.

HA bağlantı noktaları gibi çeşitli avantajları NVA HA senaryoları için sağlar:
- Örnek sistem durumu araştırmalarının başına sağlıklı örnekleriyle hızlı devreden
- n etkin örneklerine genişleme ile daha yüksek performans
- n etkin ve Etkin-pasif senaryoları
- uygulamaları izleme Zookeeper düğümleri gibi karmaşık çözümleri gereksinimini

Aşağıdaki örnekte bir hub ve bağlı sanal ağ dağıtımı ile bağlı bileşen zorlamalı güvenilen alanı ayrılmadan önce kendi trafiği hub sanal ağ ve NVA üzerinden tünel gösterir. Bir iç yük dengeleyici standart HA bağlantı noktası yapılandırması ile arkasında NVAs var.  Tüm trafiği işlenebilir ve buna uygun olarak iletebilir. 

![Ha örnek bağlantı noktaları](./media/load-balancer-ha-ports-overview/nvaha.png)

Şekil 1 - Hub-and-spoke NVAs HA modunda dağıtılmış olan sanal ağ

Lütfen ağ sanal Gereçleri kullanıyorsanız, ilgili sağlayıcı ile en iyi HA bağlantı noktalarını kullanacak biçimde nasıl ve hangi senaryolar desteklenir onaylayın.

### <a name="load-balancing-large-numbers-of-ports"></a>Yük Dengeleme çok sayıda bağlantı noktaları

HA bağlantı noktalarını bağlantı noktalarının çok sayıda yük balanicng gerektiren uygulama senaryoları için de kullanabilirsiniz. Bu senaryolar, iç kullanılarak basitleştirilebilir [yük dengeleyici standart](https://aka.ms/lbpreview) HA burada tek bir Yük Dengeleme kuralı değiştirir birden çok ayrı Yük Dengeleme kuralları, her bağlantı noktası için bir bağlantı noktasına sahip.

## <a name="region-availability"></a>Bölge kullanılabilirliği

HA bağlantı noktaları kullanılabilir [aynı bölgeleri yük dengeleyici standart olarak](https://aka.ms/lbpreview#region-availability).  

## <a name="preview-sign-up"></a>Önizleme kaydolma

Yük Dengeleyici standart HA bağlantı noktalarını özelliği Önizlemesi'na katılmak için Azure CLI 2.0 veya PowerShell kullanarak erişmek için aboneliğinizi kaydedin.  Bu üç adımı izleyin:

>[!NOTE]
>Bu özelliği kullanmak için ayrıca kaydolma yük dengeleyici için gereken [Standard Önizleme](https://aka.ms/lbpreview#preview-sign-up) HA bağlantı noktalarının yanı sıra. HA bağlantı noktaları veya yük dengeleyici standart önizlemeleri kaydı için bir saat sürebilir.

### <a name="sign-up-using-azure-cli-20"></a>Azure CLI 2.0 kullanan kaydolun

1. Bu özellik sağlayıcı ile kaydetme
    ```cli
    az feature register --name AllowILBAllPortsRule --namespace Microsoft.Network
    ```
    
2. Önceki işlemi tamamlamak için 10 dakika sürebilir.  Aşağıdaki komutla işlemin durumunu denetleyebilirsiniz:

    ```cli
    az feature show --name AllowILBAllPortsRule --namespace Microsoft.Network
    ```
    
    Özellik kayıt durumu 'aşağıda gösterildiği gibi kayıtlı' döndürdüğünde 3. adıma devam edin:
   
    ```json
    {
       "id": "/subscriptions/foo/providers/Microsoft.Features/providers/Microsoft.Network/features/AllowLBPreview",
       "name": "Microsoft.Network/AllowILBAllPortsRule",
       "properties": {
          "state": "Registered"
       },
       "type": "Microsoft.Features/providers/features"
    }
    ```
    
3. Lütfen Önizleme kayıt kaynak sağlayıcısı aboneliğinizle yeniden kaydederek tamamlayın:

    ```cli
    az provider register --namespace Microsoft.Network
    ```
    
### <a name="sign-up-using-powershell"></a>PowerShell kullanarak kaydolun

1. Bu özellik sağlayıcı ile kaydetme
    ```powershell
    Register-AzureRmProviderFeature -FeatureName AllowILBAllPortsRule -ProviderNamespace Microsoft.Network
    ```
    
2. Önceki işlemi tamamlamak için 10 dakika sürebilir.  Aşağıdaki komutla işlemin durumunu denetleyebilirsiniz:

    ```powershell
    Get-AzureRmProviderFeature -FeatureName AllowILBAllPortsRule -ProviderNamespace Microsoft.Network
    ```
    Özellik kayıt durumu 'aşağıda gösterildiği gibi kayıtlı' döndürdüğünde 3. adıma devam edin:
   
    ```
    FeatureName          ProviderName      RegistrationState
    -----------          ------------      -----------------
    AllowILBAllPortsRule Microsoft.Network Registered
    ```
    
3. Lütfen Önizleme kayıt kaynak sağlayıcısı aboneliğinizle yeniden kaydederek tamamlayın:

    ```powershell
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Network
    ```


## <a name="limitations"></a>Sınırlamalar

HA bağlantı noktaları için özel durumlar ve desteklenen yapılandırmalar şunlardır:

- Tek DSR dışı yük dengeleyici kuralı HA bağlantı noktasına sahip olabilir veya tek bir ön uç IP yapılandırmasını tek bir DSR yük dengeleyici kuralı HA bağlantı noktasına sahip olabilir. Her ikisi de olamaz.
- Tek bir ağ arabirimi IP yapılandırması yalnızca bir olmayan-yük dengeleyici kuralı HA bağlantı noktasıyla DSR olabilir. Başka bir kural bu ipconfig için yapılandırılabilir.
- Tek bir ağ arabirimi IP yapılandırmasına sahip olabilir veya HA tüm ilgili ön uç IP yapılandırmalarını sağlanan bağlantı noktaları, daha fazla DSR yük dengeleyici kuralları benzersizdir.
- Tüm Yük Dengeleme kuralları izliyorsanız olan HA bağlantı noktaları (yalnızca DSR) veya, tüm kurallar olmayan HA bağlantı noktaları (DSR & DSR olmayan), iki (veya daha fazla) yük dengeleyici kuralları aynı arka uç havuzu can için işaret eden bir arada bulunur. HA ve olmayan - HA bağlantı noktaları kuralları bileşimini ise iki tür Yük Dengeleme kuralları birlikte bulunamaz.
- HA bağlantı noktalarını IPv6 için kullanılabilir değil.
- Akış simetrisi NVA senaryoları için yalnızca tek NIC ile desteklenir. Açıklama bakın ve için diyagram [ağ sanal Gereçleri](#nva). 



## <a name="next-steps"></a>Sonraki adımlar

- [Bir iç yük dengeleyici standart HA bağlantı noktalarını yapılandırma](load-balancer-configure-ha-ports.md)
- [Yük Dengeleyici Standard önizleme hakkında bilgi edinin](https://aka.ms/lbpreview)

