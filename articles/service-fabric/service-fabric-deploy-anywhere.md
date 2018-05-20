---
title: Windows Server ve Linux üzerinde Azure Service Fabric kümeleri oluşturma | Microsoft Docs
description: Windows Server ve Linux, anlamına gelir, dağıtabilmesi ve ana bilgisayar Service Fabric uygulamaları herhangi bir yerden çalışan Service Fabric kümeleri, Windows Server veya Linux çalıştırabilirsiniz.
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
ms.date: 02/28/2018
ms.author: dekapur
ms.openlocfilehash: 3d427d99f6919991c29fc5947ebe0082670a1cc1
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
---
# <a name="create-service-fabric-clusters-on-windows-server-or-linux"></a>Windows Server veya Linux Service Fabric kümeleri oluşturma
Azure Service Fabric kümesi bir ağa bağlı içine, mikro dağıtılır ve yönetilen sanal veya fiziksel makineler kümesidir. Bir küme düğümü bir makine ya da bir kümenin parçasıysa VM adı verilir. Küme düğümleri binlerce ölçeklendirebilirsiniz. Kümeye yeni düğümler eklerseniz, Service Fabric hizmeti çoğaltmalarını ve örnekleri sayısının artması düğümleri arasında yeniden dengeler. Genel uygulama performansını artıran ve bellek erişimi için Çekişme azaltır. Kümedeki düğümler verimli bir şekilde kullanılmayan, kümedeki düğüm sayısını azaltabilirsiniz. Service Fabric yeniden örnekleri ve çoğaltmalarını azalmasına her bir düğümüne donanım daha iyi kullanılmasını sağlamak için düğüm sayısı arasında yeniden dengeler.

Service Fabric herhangi bir VM veya Windows Server veya Linux çalıştıran bilgisayarlar üzerinde Service Fabric kümeleri oluşturulmasını sağlar. Bu, dağıtmak ve Service Fabric uygulamaları birbirine bağlı bir Windows Server veya Linux bilgisayarlar kümesi sahip olduğu herhangi bir ortamda çalıştırılabilir, şirket içi, Microsoft Azure olması mümkün gelir veya herhangi bir bulut sağlayıcısı ile.

## <a name="create-service-fabric-clusters-on-azure"></a>Azure'da Service Fabric kümeleri oluşturma
Azure üzerinde bir küme oluşturma yapılır ya da bir kaynak modeli şablonu aracılığıyla veya [Azure portal](https://portal.azure.com). Okuma [Resource Manager şablonu kullanarak bir Service Fabric kümesi oluşturma](service-fabric-cluster-creation-via-arm.md) veya [Azure portalından bir Service Fabric kümesi oluştur](service-fabric-cluster-creation-via-portal.md) daha fazla bilgi için.

## <a name="supported-operating-systems-for-clusters-on-azure"></a>Azure üzerinde kümeleri için desteklenen işletim sistemleri
Bu işletim sistemlerini çalıştıran sanal makinelere kümeleri oluşturabilirsiniz:

* Windows Server 2012 R2
* Windows Server 2016 
* Windows Server 1709
* Linux Ubuntu 16.04

> [!NOTE]
> Windows Server 1709 üzerinde Service Fabric dağıtmaya karar verirseniz (1), bir uzun süreli bakım dalı, böylece sürümleri gelecekte taşımanız gerekebilir ve (2) kapsayıcıları dağıtırsanız, Windows Server'da Windows Server 2016 yerleşik kapsayıcıları işe yaramazsa olmadığını unutmayın  1709, bunun tam tersi (bunları dağıtmak için bunları yeniden gerekecek).
>

## <a name="create-service-fabric-standalone-clusters-on-premises-or-with-any-cloud-provider"></a>Service Fabric tek başına içi kümeleri oluşturmak veya herhangi bir bulut sağlayıcısı ile
Service Fabric, tek başına Service Fabric kümeleri şirket içi oluşturmak için veya herhangi bir bulut sağlayıcısına üzerinde bir yükleme paketi sağlar.

Windows Server'da tek başına Service Fabric ayarlama hakkında daha fazla bilgi kümeleri için okuma [Windows Server için Service Fabric kümesi oluşturma](service-fabric-cluster-creation-for-windows-server.md)

  > [!NOTE]
  > Tek başına kümelerinin Linux için şu anda desteklenmiyor. Linux Kutulu geliştirme ve Azure Linux çoklu makine kümeleri için desteklenir.
  >

### <a name="any-cloud-deployments-vs-on-premises-deployments"></a>Şirket içi dağıtımları ve tüm bulut dağıtımları
Şirket içi bir Service Fabric kümesi oluşturma işlemi, sanal makineleri bir dizi olan tercih ettiğiniz herhangi bir buluta bir küme oluşturma işlemi benzerdir. VM'ler sağlamak için ilk adım bulut sağlayıcısı veya kullanmakta olduğunuz şirket içi ortamı tarafından yönetilir. Aralarında etkin ağ bağlantısı ile sanal makineleri kümesine sahip sonra sonra Service Fabric paketi, ayarlamak için adımları küme ayarlarını düzenleme ve küme oluşturma çalıştırın ve yönetim komut dosyaları aynıdır. Bu bilgi ve işletim ve Service Fabric kümelerini yönetme deneyimi transfer edilebilir yeni barındırma ortamları hedef seçtiğinizde sağlar.

### <a name="benefits-of-creating-standalone-service-fabric-clusters"></a>Tek başına Service Fabric kümeleri oluşturma avantajları
* Kümenizi barındırmak için herhangi bir bulut sağlayıcısına seçmek boş.
* Bir kez yazılmış, Service Fabric uygulamaları ile birden çok barındırma ortamlarında herhangi bir değişiklik en az çalıştırılabilir.
* Service Fabric uygulamaları oluşturmak, bilgiye bir barındırma ortamdan diğerine taşır.
* Service Fabric çalıştırma ve işletimsel deneyimi taşıyan üzerinden bir ortamdan diğerine kümeleri.
* Geniş müşteri ulaşma ortamı kısıtlamaları barındırarak sınırsız.
* Bir veri merkezi veya Bulut sağlayıcısı bir Kararma varsa, hizmetler için başka bir dağıtım ortamı taşıyabilirsiniz var olduğundan, güvenilirlik ve yaygın kesintilere karşı koruma fazladan bir katmanı.

## <a name="supported-operating-systems-for-standalone-clusters"></a>Tek başına kümeleri için desteklenen işletim sistemleri
VM'ler (Linux henüz desteklenmemektedir) bu işletim sistemlerini çalıştıran bilgisayarlarda veya kümeleri oluşturabilirsiniz:

* Windows Server 2012 R2
* Windows Server 2016 

## <a name="advantages-of-service-fabric-clusters-on-azure-over-standalone-service-fabric-clusters-created-on-premises"></a>Şirket içi Azure Service Fabric kümelerinde tek başına Service Fabric kümeleri üzerinden avantajları oluşturuldu
Service Fabric kümeleri Azure üzerinde çalışan bunu kümeleriniz çalıştırdığı için belirli gereksinimler yoksa şirket içi avantajları, seçeneği sağlar sonra Azure üzerinde çalıştırmanızı öneririz. Azure üzerinde biz diğer Azure özellikleri ve işlemler ve küme yönetiminin daha kolay ve daha güvenilir yapar Hizmetleri ile tümleştirme sağlar.

* **Azure portal:** Azure portal oluşturmak ve kümeleri yönetmek kolaylaştırır.
* **Azure Resource Manager:** kullanım Azure Kaynak Yöneticisi'nin bir birim olarak küme tarafından kullanılan tüm kaynakları kolay yönetilmesine izin verir ve maliyet izleme ve faturalama basitleştirir.
* **Service Fabric kümesi bir Azure kaynağı olarak** A Service Fabric kümesi diğer kaynaklara yaptığınız gibi model bir Azure kaynağı olduğundan.
* **Azure altyapısı ile tümleştirme** Service Fabric koordinatları işletim sistemi, ağ ve diğer yükseltmeler kullanılabilirliğini ve uygulamalarınızı güvenilirliğini artırmak için Azure altyapının ile.  
* **Tanılama:** Azure üzerinde tümleştirme Azure tanılama ve günlük analizi ile sağlıyoruz.
* **Otomatik ölçeklendirme:** Azure ile ilgili daha fazla kümeler için sanal makine ölçek kümeleri nedeniyle yerleşik otomatik ölçeklendirme işlevi sağlıyoruz. Şirket içi ve diğer bulut ortamları, kendi otomatik ölçeklendirme özelliğini el ile kümeleri ölçekleme için Service Fabric gösterir API'lerini kullanarak ölçek derleme veya sahiptir.

## <a name="next-steps"></a>Sonraki adımlar

* Vm'leri veya Windows Server çalıştıran bilgisayarlarda bir küme oluşturun: [Windows Server için Service Fabric kümesi oluşturma](service-fabric-cluster-creation-for-windows-server.md)
* Sanal makineleri veya Linux çalıştıran bilgisayarlarda bir küme oluşturun: [Linux kümesi oluşturma](service-fabric-cluster-creation-via-portal.md)
* [Service Fabric destek seçenekleri](service-fabric-support.md) hakkında bilgi edinin

