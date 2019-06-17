---
title: Yük Dengeleyici için Azure Resource Manager desteği | Microsoft Docs
description: Azure Resource Manager ile yük dengeleyici için PowerShell kullanma. Yük Dengeleyici için şablonları kullanma
services: load-balancer
documentationcenter: na
author: KumudD
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: kumud
ms.openlocfilehash: 596ac871067886ee3124c0f21beb35cb3b8fe1ae
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60889001"
---
# <a name="using-azure-resource-manager-support-with-azure-load-balancer"></a>Azure Resource Manager desteği, Azure Load Balancer ile kullanma



Azure Resource Manager, Azure Hizmetleri için tercih edilen Yönetim çerçevedir. Azure Load Balancer, Azure Resource Manager tabanlı API'ler ve araçlar kullanılarak yönetilebilir.

## <a name="concepts"></a>Kavramlar

Resource Manager ile Azure Load Balancer aşağıdaki alt kaynakları içerir:

* Ön uç IP yapılandırmasını – yoksa sanal IP (VIP) bilinen bir veya daha fazla ön uç IP adresi bir yük dengeleyici ekleyebilirsiniz. Bu IP adresleri trafik için bir giriş işlevi görür.
* Arka uç adres havuzu – bunlar ilişkili sanal makine ağ arabirim kartı (yükleme için dağıtılan NIC ile) IP adresleridir.
* Yük Dengeleme kuralları: bir kural özelliğine belirli ön uç IP ve bağlantı noktası birleşimini bir dizi arka uç IP adresleri ve bağlantı noktası bileşimiyle eşler. Tek bir yük dengeleyici birden çok dengeleme kuralına sahip olabilir. Her kural, bir ön uç IP bağlantı noktası ve arka uç IP ve bağlantı noktası Vm'lerle ilişkilendirilmiş birleşimidir.
* Araştırmalar – araştırmaları sanal makine örneklerinin durumunu izlemenize olanak sağlar. Durum araştırması başarısız olursa, sanal makine örneğine otomatik olarak döndürme dışına alınır.
* NAT – gelen NAT kuralları, bir ön uç IP üzerinden gelen trafiği tanımlayan kuralları ve arka uç IP dağıtılır.

![](./media/load-balancer-arm/load-balancer-arm.png)

## <a name="quickstart-templates"></a>Hızlı başlangıç şablonları

Azure Resource Manager, uygulamalarınızı bildirim temelli bir şablon aracılığıyla sağlamanıza olanak tanır. Tek bir şablonda birden çok hizmeti bağımlılıklarıyla birlikte dağıtabilirsiniz. Uygulama yaşam döngüsünün her aşamasında uygulamanızı tekrar tekrar dağıtmak için aynı şablonu kullanırsınız.

Şablonlar, sanal makineler, sanal ağlar, kullanılabilirlik kümeleri, ağ arabirimlerini (NIC'ler), depolama hesapları, yük Dengeleyiciler, ağ güvenlik grupları ve genel IP'ler için tanımları ekleyebilirsiniz. Şablonlar sayesinde, karmaşık bir uygulama için ihtiyacınız olan her şey oluşturabilirsiniz. Şablon dosyası, sürüm denetimi ve işbirliği için içerik yönetim sistemi olarak denetlenebilir.

[Şablonlar hakkında daha fazla bilgi edinin](../azure-resource-manager/resource-manager-template-walkthrough.md)

[Ağ kaynakları hakkında daha fazla bilgi edinin](../networking/networking-overview.md)

Azure Load Balancer'ı kullanarak hızlı başlangıç şablonları için bkz: [GitHub deposu](https://github.com/Azure/azure-quickstart-templates) oluşturulan topluluk şablonları kümesi barındıran.

Şablon örneklerini:

* [Load Balancer ve Yük Dengeleme kuralları içinde 2 VM](https://go.microsoft.com/fwlink/?LinkId=544799)
* [Bir sanal ağ ile bir iç yük dengeleyici ve yük dengeleyici kuralları içinde 2 VM](https://go.microsoft.com/fwlink/?LinkId=544800)
* [Bir yük dengeleyici içinde 2 VM ve LB'de NAT kurallarını yapılandırma](https://go.microsoft.com/fwlink/?LinkId=544801)

## <a name="setting-up-azure-load-balancer-with-a-powershell-or-cli"></a>PowerShell veya CLI ile Azure Load Balancer oluşturma

Azure Resource Manager cmdlet'leri, komut satırı araçları ve REST API'lerini kullanmaya başlama

* [Azure ağ cmdlet'leri](https://docs.microsoft.com/powershell/module/az.network#networking) yük dengeleyici oluşturmak için kullanılabilir.
* [Azure Resource Manager'ı kullanarak bir yük dengeleyici oluşturma](load-balancer-get-started-ilb-arm-ps.md)
* [Azure kaynak yönetimi ile Azure CLI kullanma](../xplat-cli-azure-resource-manager.md)
* [Yük Dengeleyici REST API'leri](https://msdn.microsoft.com/library/azure/mt163651.aspx)

## <a name="next-steps"></a>Sonraki adımlar

Ayrıca [Internet'e yönelik Yük Dengeleyici oluşturmaya başlama](load-balancer-get-started-internet-arm-ps.md) ve ne tür yapılandırma [dağıtım modunu](load-balancer-distribution-mode.md) belirli yük dengeleyici ağ trafiği davranışı için.

Nasıl yöneteceğinizi öğrenin [boşta kalma TCP zaman aşımı ayarları yük dengeleyici](load-balancer-tcp-idle-timeout.md). Bir yük dengeleyici arkasındaki sunucular için bağlantıları canlı uygulamanız gerektiğinde bu önemlidir.
