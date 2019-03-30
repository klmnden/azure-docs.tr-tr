---
title: Genel bakış, Azure ve tek başına Service Fabric kümeleri | Microsoft Docs
description: Tüm Vm'leri veya Windows Server veya Linux çalıştıran bilgisayarlarda, Service Fabric kümeleri oluşturabilirsiniz. Başka bir deyişle, dağıtmak ve bir Windows Server veya Linux bilgisayarları birbirine-şirket içi, Microsoft Azure veya tüm bulut sağlayıcıları ile sahip olduğunuz herhangi bir ortamda Service Fabric uygulamaları çalıştırmak kullanabilirsiniz.
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: chackdan
editor: ''
ms.assetid: 19ca51e8-69b9-4952-b4b5-4bf04cded217
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/01/2019
ms.author: dekapur
ms.openlocfilehash: 6d5169d8ea4480e95e09228f9eb02bd78fdd0be8
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58661336"
---
# <a name="comparing-azure-and-standalone-service-fabric-clusters-on-windows-server-and-linux"></a>Windows Server ve Linux kümeleri, Azure ve tek başına Service Fabric karşılaştırması
Service Fabric kümesi bir ağa bağlı, mikro hizmetlerin dağıtıldığı ve yönetildiği sanal veya fiziksel makine kümesidir. Bir makine ya da bir kümenin parçası olan sanal makine bir küme düğümü adı verilir. Kümeler binlerce düğümde için ölçeklendirme yapabilir. Kümeye yeni düğümler eklerseniz, Service Fabric örnekleri ve hizmet bölüm çoğaltmaları sayısının artması düğümleri arasında yeniden dengeler. Genel uygulama performansını artıran ve bellek erişim çekişmesini azaltır. Kümedeki düğümler verimli bir şekilde kullanılmayan, kümedeki düğümlerin sayısını azaltabilirsiniz. Service Fabric yeniden örnekleri ve bölüm çoğaltmalarını azalan her düğümde donanım daha iyi kullanabilmesine için düğüm sayısını arasında yeniden dengeler.

Service Fabric Service Fabric kümeleri herhangi bir VM veya Windows Server veya Linux çalıştıran bilgisayarlar üzerinde oluşturulmasını sağlar. Bu, dağıtmak ve birbirlerine bağlanış Windows Server veya Linux bilgisayarlar kümesi sahip olduğu herhangi bir ortamda Service Fabric uygulamaları çalıştırın, şirket içi, Microsoft Azure, mümkün olduğu anlamına gelir veya tüm bulut sağlayıcıları ile.

## <a name="benefits-of-clusters-on-azure"></a>Azure'da kümeler avantajları
Azure'da tümleştirme diğer Azure özellikleri ve operasyon ve küme yönetimi daha kolay ve daha güvenilir olmasını sağlayan hizmetler ile sunuyoruz.

* **Azure portalı:** Azure portalı küme oluşturmak ve yönetmek kolay hale getirir.
* **Azure Resource Manager:** Azure Resource Manager kullanımı, bir birim olarak küme tarafından kullanılan tüm kaynakları kolay yönetilmesini sağlar ve maliyet izleme ve faturalandırma basitleştirir.
* **Service Fabric kümesi bir Azure kaynağı olarak** bir Service Fabric kümesi olan bir Azure kaynağı azure'daki diğer kaynakları olduğu gibi modelini oluşturabilir.
* **Azure altyapı tümleştirmesi** kullanılabilirliği ve güvenilirliği iyileştirmek işletim sistemi, ağ ve diğer yükseltme işlemleri için temel alınan Azure altyapısı sayesinde Service Fabric düzenler.  
* **Tanılama:** Azure'da, Azure Tanılama ile tümleştirme sunuyoruz ve Azure İzleyici günlüğe kaydeder.
* **Otomatik ölçeklendirme:** Azure'da kümeler için yerleşik otomatik ölçeklendirmeyi işlevselliği nedeniyle sanal makine ölçek kümeleri sunuyoruz. Şirket içinde ve diğer bulut ortamları, kendi otomatik ölçeklendirme özelliği veya el ile kümeleri ölçeklendirme için Service Fabric sunan API'lerini kullanarak ölçek oluşturmak gerekir.

## <a name="benefits-of-standalone-clusters"></a>Tek başına kümeler avantajları
* Kümenizi barındırmak amacıyla bulut sağlayıcıları seçebilirsiniz.
* Service Fabric uygulamaları, bir kez yazılan ile birden çok barındırma ortamlarında değişiklik yapmadan en az çalıştırılabilir.
* Service Fabric uygulamaları oluşturmak, bilgi barındıran bir ortamdan diğerine taşır.
* Çalıştıran ve yöneten Service Fabric çalışma deneyimi taşıyan üzerinden bir ortamdan diğerine kümeleri.
* Barındırma ortamı kısıtlamaları tarafından geniş ulaşarak sınırsızdır.
* Bir veri merkezi veya Bulut sağlayıcısı bir Kararma varsa, hizmetler için başka bir dağıtım ortamı taşıyabilirsiniz nedeniyle ek bir koruma katmanı güvenilirlik ve yaygın kesintilerine karşı koruma bulunmaktadır.

## <a name="next-steps"></a>Sonraki adımlar

* Genel Bakış okuyun [Azure'da Service Fabric kümeleri](service-fabric-azure-clusters-overview.md)
* Genel Bakış okuyun [Service Fabric tek başına kümeler](service-fabric-standalone-clusters-overview.md)
* [Service Fabric destek seçenekleri](service-fabric-support.md) hakkında bilgi edinin