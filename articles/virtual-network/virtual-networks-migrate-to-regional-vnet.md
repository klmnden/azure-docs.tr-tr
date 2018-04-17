---
title: Bir Azure sanal ağ (Klasik) bir benzeşim grubu bir bölgeye geçirme | Microsoft Docs
description: Bir sanal ağ (Klasik) bir benzeşim grubu bir bölgeye geçirmek öğrenin.
services: virtual-network
documentationcenter: na
author: genli
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
ms.openlocfilehash: b8b4fbdbf92e49a0287cd6c440513213279d24d0
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="migrate-a-virtual-network-classic-from-an-affinity-group-to-a-region"></a>Bir sanal ağ (Klasik) bir benzeşim grubu bir bölge'ye geçirme

> [!IMPORTANT]
> Azure oluşturmak ve kaynaklarla çalışmak için iki farklı dağıtım modeli vardır: [Resource Manager ve klasik](../resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Bu makale klasik dağıtım modelini incelemektedir. Microsoft, en yeni dağıtımların Resource Manager dağıtım modelini kullanmasını önerir.

Benzeşim grupları aynı benzeşim grubunda oluşturulan kaynakları fiziksel olarak daha hızlı iletişim kurmak bu kaynakları etkinleştirme birbirine yakın olan sunucuları tarafından barındırılan emin olun. Geçmişte, benzeşim grupları sanal ağları (Klasik) oluşturmak için bir gereksinim yoktu. O anda sanal ağları (Klasik) yönetilen Ağ Yöneticisi hizmeti yalnızca fiziksel sunucularda veya ölçek birimi kümesinde işe yarayabilir. Mimari geliştirmeleri bir bölgeye ağ yönetim kapsamını artırmıştır.

Bu mimari geliştirmeler sonucunda benzeşim grupları artık önerilen veya sanal ağları (Klasik) için gerekli. Benzeşim grupları sanal ağları (Klasik) için kullanımı bölgeleri değiştirilir. (Klasik) bölgeleri ile ilişkilendirilmiş sanal ağları bölgesel sanal ağ adı verilir.

Benzeşim grupları genel kullanmamanızı öneririz. Sanal ağ gereksinimi yanı sıra benzeşim grupları ayrıca işlem (Klasik) ve depolama (Klasik) gibi kaynaklar sağlamak için kullanın önemli, diğer yerleştirildi. Ancak, geçerli Azure ağ mimarisiyle bu yerleştirme gereksinimleri artık gerekli değildir.

> [!IMPORTANT]
> Bir benzeşim grubu ile ilişkili bir sanal ağ oluşturma hala teknik olarak mümkün olsa da, bunu yapmak için ilgi çekici bir neden yoktur. Ağ güvenlik grupları gibi birçok sanal ağ özellikleri bölgesel bir sanal ağ kullanırken yalnızca kullanılabilir ve benzeşim gruplarıyla ilişkili sanal ağlar için kullanılamıyor.
> 
> 

## <a name="edit-the-network-configuration-file"></a>Ağ yapılandırma dosyasını düzenleyin

1. Ağ yapılandırma dosyasını dışarı aktarın. PowerShell veya Azure komut satırı arabirimi (CLI) 1.0 kullanarak bir ağ yapılandırma dosyasını dışarı aktarmak öğrenmek için bkz: [bir ağ yapılandırma dosyası kullanarak bir sanal ağ yapılandırma](virtual-networks-using-network-configuration-file.md#export).
2. Ağ yapılandırma dosyasını düzenlemenize değiştirme **AffinityGroup** ile **konumu**. Bir Azure belirttiğiniz [bölge](https://azure.microsoft.com/regions) için **konumu**.
   
   > [!NOTE]
   > **Konumu** , sanal ağ (Klasik) ile ilişkili benzeşim grubu için belirtilen bölgedir. Örneğin, sanal ağ (Klasik) geçirdiğinizde, Batı ABD içinde bulunan bir benzeşim grubu ile ilişkili ise, **konumu** Batı ABD işaret etmelidir. 
   > 
   > 
   
    Değerleri kendi değerlerinizle değiştirerek aşağıdaki satırları ağ yapılandırma dosyanızı düzenleyin: 
   
    **Eski değer:** \<VirtualNetworkSitename "VNetUSWest" AffinityGroup = "VNetDemoAG" =\> 
   
    **Yeni değer:** \<VirtualNetworkSitename = "VNetUSWest" Konum "Batı ABD" =\>
3. Değişikliklerinizi kaydetmek ve [alma](virtual-networks-using-network-configuration-file.md#import) Azure ağ yapılandırması.

> [!NOTE]
> Bu geçiş kapalı kalma süresi hizmetlerinize neden olmaz.
> 
> 

## <a name="what-to-do-if-you-have-a-vm-classic-in-an-affinity-group"></a>Bir benzeşim grubunda bir VM'ye (Klasik) varsa yapmanız gerekenler
(Klasik), şu anda bir benzeşim grubunda yer alan VM'ler benzeşim grubundan kaldırılmış gerekmez. Bir VM dağıtıldığında, bir tek ölçek birimi dağıtılır. Benzeşim grupları kullanılabilir VM boyutları yeni VM Dağıtım için sınırlamak ancak dağıtılan var olan VM VM boyutları VM dağıtıldığı ölçek biriminde kullanılabilir kümesine zaten sınırlanır. VM için bir ölçek birimi zaten dağıtılmış olduğundan, bir VM bir benzeşim grubundan kaldırma VM üzerinde etkisi yoktur.
