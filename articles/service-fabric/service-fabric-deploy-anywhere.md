---
title: Windows Server ve Linux üzerinde Service Fabric kümeleri oluşturma | Microsoft Docs
description: Windows Server ve Linux, yani sizin dağıtmayı mümkün olacaktır ve konak Service Fabric uygulamaları her yerde çalışan Service Fabric kümeleri, Windows Server veya Linux çalıştırabilirsiniz.
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: ''
ms.assetid: 19ca51e8-69b9-4952-b4b5-4bf04cded217
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/28/2018
ms.author: dekapur
ms.openlocfilehash: e4540076b29cf3cd51f03239a1868e18a41781d9
ms.sourcegitcommit: 333d4246f62b858e376dcdcda789ecbc0c93cd92
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/01/2018
ms.locfileid: "52726534"
---
# <a name="create-service-fabric-clusters-on-windows-server-or-linux"></a>Windows Server veya Linux Service Fabric kümeleri oluşturma
Service Fabric kümesi bir ağa bağlı, mikro hizmetlerin dağıtıldığı ve yönetildiği sanal veya fiziksel makine kümesidir. Bir makine ya da bir kümenin parçası olan sanal makine bir küme düğümü adı verilir. Kümeler binlerce düğümde için ölçeklendirme yapabilir. Kümeye yeni düğümler eklerseniz, Service Fabric örnekleri ve hizmet bölüm çoğaltmaları sayısının artması düğümleri arasında yeniden dengeler. Genel uygulama performansını artıran ve bellek erişim çekişmesini azaltır. Kümedeki düğümler verimli bir şekilde kullanılmayan, kümedeki düğümlerin sayısını azaltabilirsiniz. Service Fabric yeniden örnekleri ve bölüm çoğaltmalarını azalan her düğümde donanım daha iyi kullanabilmesine için düğüm sayısını arasında yeniden dengeler.

Service Fabric Service Fabric kümeleri herhangi bir VM veya Windows Server veya Linux çalıştıran bilgisayarlar üzerinde oluşturulmasını sağlar. Bu, dağıtmak ve Service Fabric uygulamaları birbirlerine bağlanış Windows Server veya Linux bilgisayarlar kümesi sahip olduğu herhangi bir ortamda çalıştırmak, şirket içi, Microsoft Azure olması mümkün olduğu anlamına gelir veya tüm bulut sağlayıcıları ile.

## <a name="create-service-fabric-clusters-on-azure"></a>Azure'da Service Fabric kümeleri oluşturma
Azure'da küme oluşturma yapılır ya da bir kaynak modeli şablon aracılığıyla veya [Azure portalında](https://portal.azure.com). Okuma [Resource Manager şablonu kullanarak bir Service Fabric kümesi oluşturma](service-fabric-cluster-creation-via-arm.md) veya [Azure portalında bir Service Fabric kümesi oluşturma](service-fabric-cluster-creation-via-portal.md) daha fazla bilgi için.

## <a name="supported-operating-systems-for-clusters-on-azure"></a>Azure'da kümeler için desteklenen işletim sistemleri
Bu işletim sistemlerini çalıştıran sanal makinelere kümeleri oluşturmak kullanabilirsiniz:

* Windows Server 2012 R2
* Windows Server 2016 
* Windows Server 1709
* Windows Server 1803
* Linux Ubuntu 16.04
* Red Hat Enterprise Linux 7.4 (Önizleme desteği)

> [!NOTE]
> Service Fabric'te Windows Server 1709 dağıtmaya karar verirseniz, (1), uzun süreli bakım dalı, sürüm gelecekte taşımanız gerekebilir ve (2) kapsayıcıları dağıtma, kapsayıcıları Windows Server 2016'da oluşturulmuş Windows Server üzerinde çalışmaz olmadığını unutmayın  1709 ve bunun tersi de geçerlidir (bunları dağıtmak için bunları yeniden oluşturmak gerekir).
>

## <a name="create-service-fabric-standalone-clusters-on-premises-or-with-any-cloud-provider"></a>Service Fabric tek başına kümeler şirket içi oluşturma veya tüm bulut sağlayıcıları ile
Service Fabric, tek başına Service Fabric kümeleri şirket içi oluşturmak için veya tüm bulut sağlayıcılarından kurulum paketi sağlar.

Windows Server'da tek başına Service Fabric hakkında daha fazla bilgi kümeleri için okuma [Windows Server için Service Fabric kümesi oluşturma](service-fabric-cluster-creation-for-windows-server.md)

  > [!NOTE]
  > Linux için tek başına kümeler şu anda desteklenmiyor. Linux üzerinde çalıştırma geliştirme ve Azure Linux çoklu makine kümeleri için desteklenir.
  >

### <a name="any-cloud-deployments-vs-on-premises-deployments"></a>Tüm bulut dağıtımları ve şirket içi dağıtımlar
Şirket içi bir Service Fabric kümesi oluşturma işlemi, bir VM kümesi ile kendi tercih ettiğiniz herhangi bir bulut üzerindeki bir küme oluşturma işlemi benzerdir. Vm'leri sağlama işlemi ilk adımlarında, kullanmakta olduğunuz şirket içi ortam ve bulut sağlayıcısı tarafından yönetilir. Bunlar arasında etkin ağ bağlantısı ile bir VM kümesi oluşturduktan sonra sonra adımları, Service Fabric paketi küme ayarları düzenleyin ve küme oluşturma çalıştırın ve yönetim komut dosyaları aynıdır. Bu deneyimi ve Service Fabric kümeleri yönetmek ve Bilgi Bankası aktarılamaz, yeni barındırma ortamları hedeflemek seçtiğinizde sağlar.

### <a name="benefits-of-creating-standalone-service-fabric-clusters"></a>Tek başına Service Fabric kümeleri oluşturma avantajları
* Kümenizi barındırmak amacıyla bulut sağlayıcıları seçebilirsiniz.
* Service Fabric uygulamaları, bir kez yazılan ile birden çok barındırma ortamlarında değişiklik yapmadan en az çalıştırılabilir.
* Service Fabric uygulamaları oluşturmak, bilgi barındıran bir ortamdan diğerine taşır.
* Çalıştıran ve yöneten Service Fabric çalışma deneyimi taşıyan üzerinden bir ortamdan diğerine kümeleri.
* Barındırma ortamı kısıtlamaları tarafından geniş ulaşarak sınırsızdır.
* Bir veri merkezi veya Bulut sağlayıcısı bir Kararma varsa, hizmetler için başka bir dağıtım ortamı taşıyabilirsiniz nedeniyle ek bir koruma katmanı güvenilirlik ve yaygın kesintilerine karşı koruma bulunmaktadır.

## <a name="supported-operating-systems-for-standalone-clusters"></a>Tek başına kümeler için desteklenen işletim sistemleri
Vm'leri veya (Linux henüz desteklenmiyor) bu işletim sistemlerini çalıştıran bilgisayarlara kümeleri oluşturmak kullanabilirsiniz:

* Windows Server 2012 R2
* Windows Server 2016 

## <a name="advantages-of-service-fabric-clusters-on-azure-over-standalone-service-fabric-clusters-created-on-premises"></a>Şirket içi Azure Service Fabric kümelerinde tek başına Service Fabric kümeleri üzerinde avantajları oluşturuldu
Service Fabric kümeleri Azure üzerinde çalışan bunu kümeleriniz, çalıştırdığınız için belirli gereksinimleriniz yoksa, şirket içi avantajları, seçenek sağlar, sonra Azure'da çalıştırmanızı öneririz. Azure'da tümleştirme diğer Azure özellikleri ve operasyon ve küme yönetimi daha kolay ve daha güvenilir olmasını sağlayan hizmetler ile sunuyoruz.

* **Azure portalı:** Azure portalı küme oluşturmak ve yönetmek kolaylaştırır.
* **Azure Resource Manager:** Azure Resource Manager kullanımı kolay bir birim olarak küme tarafından kullanılan tüm kaynakların yönetilmesine izin verir ve maliyet izleme ve faturalandırma basitleştirir.
* **Service Fabric kümesi bir Azure kaynağı olarak** bir Service Fabric kümesi olan bir Azure kaynağı azure'daki diğer kaynakları olduğu gibi modelini oluşturabilir.
* **Azure altyapı tümleştirmesi** kullanılabilirliği ve güvenilirliği iyileştirmek işletim sistemi, ağ ve diğer yükseltme işlemleri için temel alınan Azure altyapısı sayesinde Service Fabric düzenler.  
* **Tanılama:** Azure'da tümleştirme Log Analytics ve Azure Tanılama ile sunuyoruz.
* **Otomatik ölçeklendirme:** azure'da kümeler için yerleşik otomatik ölçeklendirmeyi işlevselliği nedeniyle sanal makine ölçek kümeleri sunuyoruz. Şirket içinde ve diğer bulut ortamları, kendi otomatik ölçeklendirme özelliği veya el ile kümeleri ölçeklendirme için Service Fabric sunan API'lerini kullanarak ölçek oluşturmak gerekir.

## <a name="next-steps"></a>Sonraki adımlar

* Küme Vm'leri veya Windows Server çalıştıran bilgisayarlara oluşturma: [Windows Server için Service Fabric kümesi oluşturma](service-fabric-cluster-creation-for-windows-server.md)
* Küme sanal makineleri veya Linux çalıştıran bilgisayarlara oluşturma: [bir Linux kümesi oluşturma](service-fabric-cluster-creation-via-portal.md)
* [Service Fabric destek seçenekleri](service-fabric-support.md) hakkında bilgi edinin

