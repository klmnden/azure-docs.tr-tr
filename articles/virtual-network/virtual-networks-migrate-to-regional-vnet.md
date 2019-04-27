---
title: Bir Azure sanal ağ (Klasik) bir benzeşim grubundan bölgeye geçiş | Microsoft Docs
description: Bir sanal ağ (Klasik) bir benzeşim grubundan bölgeye geçiş öğrenin.
services: virtual-network
documentationcenter: na
author: genlin
manager: cshepard
editor: ''
tags: azure-service-management
ms.assetid: 84febcb9-bb8b-4e79-ab91-865ad9de41cb
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2016
ms.author: genli
ms.openlocfilehash: d3bb93d12a217e6d9066d037ff92f071b6139ab3
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60648644"
---
# <a name="migrate-a-virtual-network-classic-from-an-affinity-group-to-a-region"></a>Bir sanal ağ (Klasik) bir benzeşim grubundan bölgeye geçiş

> [!IMPORTANT]
> Azure'da oluşturmaya ve kaynaklarla çalışmaya yönelik iki farklı dağıtım modeli vardır: [Resource Manager ve klasik](../resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Bu makale klasik dağıtım modelini incelemektedir. Microsoft, en yeni dağıtımların Resource Manager dağıtım modelini kullanmasını önerir.

Benzeşim grupları aynı benzeşim grubunda oluşturulan kaynakları fiziksel olarak daha hızlı iletişim kurmak bu kaynakları etkinleştirme birbirine yakın olan sunucuları tarafından barındırılan emin olun. Geçmişte, benzeşim grupları sanal ağlar (Klasik) oluşturmaya yönelik bir gereksinim olan. Bu sırada, yönetilen sanal ağlar (Klasik) Ağ Yöneticisi hizmeti yalnızca fiziksel sunucularda ya da Ölçek birimi bir dizi içinde işe yarayabilir. Mimari geliştirmeleri bir bölgeye ağ yönetim kapsamını artırmıştır.

Bu mimari geliştirmeler nedeniyle benzeşim grupları artık önerilen veya sanal ağlar (Klasik) gerekli. Benzeşim grupları sanal ağlar (Klasik) için kullanımı bölgeleri tarafından değiştirilir. Bölge ile ilişkili sanal ağlar (Klasik), bölgesel sanal ağlar olarak adlandırılır.

Benzeşim grupları genel kullanmamanızı öneririz. Sanal ağ gereksinimi dışında benzeşim grupları da (Klasik) işlem ve depolama (Klasik) gibi kaynakları sağlamak için kullanın önemli, birbirine yakın yerleştirildi. Ancak, geçerli Azure ağ mimarisi ile bu yerleştirme gereksinimleri artık gerekli değil.

> [!IMPORTANT]
> Bir benzeşim grubu ile ilişkili olan bir sanal ağ oluşturma hala teknik olarak mümkün olsa da, bunu yapmak için ilgi çekici bir neden yoktur. Ağ güvenlik grupları gibi çok sayıda sanal ağ özelliği, bölgesel bir sanal ağ kullanılırken yalnızca kullanılabilir ve benzeşim grupları ile ilişkili sanal ağlar için kullanılamıyor.
> 
> 

## <a name="edit-the-network-configuration-file"></a>Ağ yapılandırma dosyasını Düzenle

1. Ağ yapılandırma dosyasını dışarı aktarın. PowerShell veya Azure komut satırı arabirimi (CLI) 1.0 kullanarak bir ağ yapılandırma dosyasını dışarı aktarmanıza öğrenmek için bkz. [bir ağ yapılandırma dosyası kullanarak bir sanal ağ yapılandırma](virtual-networks-using-network-configuration-file.md#export).
2. Ağ yapılandırma dosyasını düzenleyin değiştirerek **AffinityGroup** ile **konumu**. Azure, belirttiğiniz [bölge](https://azure.microsoft.com/regions) için **konumu**.
   
   > [!NOTE]
   > **Konumu** , sanal ağ (Klasik) ile ilişkili bir benzeşim grubu için belirttiğiniz bölgedir. Örneğin, sanal ağınız (Klasik) geçiş yaptığınızda, Batı ABD bölgesinde bulunan bir benzeşim grubu ile ilişkili ise, **konumu** Batı ABD için işaret etmelidir. 
   > 
   > 
   
    Değerleri kendi değerlerinizle değiştirerek aşağıdaki satırları ağ yapılandırma dosyanızdaki düzenleyin: 
   
    **Eski değer:** \<VirtualNetworkSitename="VNetUSWest" AffinityGroup="VNetDemoAG"\> 
   
    **Yeni değer:** \<VirtualNetworkSitename="VNetUSWest" Location="West US"\>
3. Değişikliklerinizi kaydetmek ve [alma](virtual-networks-using-network-configuration-file.md#import) azure'a ağ yapılandırması.

> [!NOTE]
> Bu geçiş kapalı kalma süresi Services hesabınıza neden olmaz.
> 
> 

## <a name="what-to-do-if-you-have-a-vm-classic-in-an-affinity-group"></a>Bir benzeşim grubunda bir VM (Klasik) varsa yapmanız gerekenler
Şu anda bir benzeşim grubunda olan VM'ler (Klasik) benzeşim grubundan kaldırılması gerek yoktur. VM dağıtıldıktan sonra bir tek bir ölçek birimi için dağıtılır. Benzeşim grupları yeni bir VM dağıtımı için kullanılabilir VM boyutlarını sınırlamak, ancak dağıtılan var olan bir VM'yi sanal Makinenin dağıtıldığı ölçek birimindeki kullanılabilir VM boyutları kümesine zaten sınırlıdır. VM Ölçek birimine zaten dağıtıldığından bir VM bir benzeşim grubundan kaldırılıyor. VM üzerinde etkisi yoktur.
