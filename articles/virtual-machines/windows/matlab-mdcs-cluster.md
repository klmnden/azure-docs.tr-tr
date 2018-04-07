---
title: MATLAB kümeleri sanal makinelerde | Microsoft Docs
description: Microsoft Azure sanal makineleri, işlem yoğunluklu paralel MATLAB iş yüklerini çalıştırmak için MATLAB dağıtılmış bilgi işlem sunucusu kümeleri oluşturmak için kullanın
services: virtual-machines-windows
documentationcenter: ''
author: mscurrell
manager: jeconnoc
editor: ''
ms.assetid: e9980ce9-124a-41f1-b9ec-f444c8ea5c72
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: Windows
ms.workload: infrastructure-services
ms.date: 05/09/2016
ms.author: markscu
ms.openlocfilehash: 695833fb12c0c7a130e98fe9b3bdfa502672ab29
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="create-matlab-distributed-computing-server-clusters-on-azure-vms"></a>Azure Vm'lerinde MATLAB dağıtılmış bilgi işlem sunucusu kümeleri oluşturma
Microsoft Azure sanal makineleri, işlem yoğunluklu paralel MATLAB iş yüklerini çalıştırmak için bir veya daha fazla MATLAB dağıtılmış bilgi işlem sunucusu kümeleri oluşturmak için kullanın. Temel görüntü ve bir Azure Hızlı Başlangıç şablonu veya Azure PowerShell Betiği kullanmak için bir VM MATLAB dağıtılmış bilgi işlem sunucusu yazılım yükleme (kullanılabilir [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/matlab-cluster)) dağıtmak ve kümeyi yönetmek için. Dağıtımdan sonra iş yüklerini çalıştırmak için kümeye bağlanın.

## <a name="about-matlab-and-matlab-distributed-computing-server"></a>MATLAB dağıtılmış bilgi işlem sunucusu ve MATLAB hakkında
[MATLAB](http://www.mathworks.com/products/matlab/) platform, mühendislik ve bilimsel sorunları çözmek için getirilmiştir. Büyük ölçekli benzetimleri ve veri işleme görevlerini MATLAB kullanıcılarla hesaplama kümeleri ve kılavuz Hizmetleri yararlanarak, işlem yoğunluklu iş yüklerini hızlandırmak için ürün bilgi işlem MathWorks paralel kullanabilirsiniz. [Paralel bilgi işlem araç](http://www.mathworks.com/products/parallel-computing/) MATLAB kullanıcıların uygulamaları paralel hale ve çok çekirdekli işlemciler, GPU, yararlanabilir ve hesaplama kümeleri izin verir. [MATLAB dağıtılmış bilgi işlem sunucusu](http://www.mathworks.com/products/distriben/) MATLAB kullanıcıların bir işlem kümesi birçok bilgisayar kullanmasına olanak tanır.

Azure sanal makineleri kullanarak, aynı mekanizmaları etkileşimli işleri, toplu işleri, bağımsız görevleri ve iletişim kuran görevler gibi şirket içi kümeleri olarak paralel iş göndermek kullanılabilir olan MATLAB dağıtılmış bilgi işlem sunucusu kümeleri oluşturabilirsiniz. Azure MATLAB platform ile birlikte kullanarak sağlama için karşılaştırıldığında birçok avantaj vardır ve geleneksel kullanarak şirket içi donanım: sanal makine bir dizi boyutları, kümeleri, yalnızca işlem kaynaklarını, kullanımı ve modelleri ölçekte test olanağı ödeme için isteğe bağlı oluşturulmasını.  

## <a name="prerequisites"></a>Önkoşullar
* **İstemci bilgisayar** -dağıtımdan sonra Azure ve MATLAB dağıtılmış bilgi işlem sunucusu küme ile iletişim kurmak için bir Windows tabanlı bir istemci bilgisayar gerekir.
* **Azure PowerShell** -bkz [Azure PowerShell'i yükleme ve yapılandırma nasıl](/powershell/azure/overview) , istemci bilgisayara yüklemek için.
* **Azure aboneliği** -bir aboneliğiniz yoksa oluşturabileceğiniz bir [ücretsiz bir hesap](https://azure.microsoft.com/free/) yalnızca birkaç dakika içinde. Daha büyük kümeleri için Kullandıkça Öde aboneliğine veya diğer satın alma seçenekleri göz önünde bulundurun.
* **Vcpu'lar kota** -büyük bir küme veya birden fazla MATLAB dağıtılmış bilgi işlem sunucusu küme dağıtmak için vCPU Kotayı artırmak gerekebilir. Bir Kotayı artırmak için [bir çevrimiçi müşteri destek isteği açma](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) herhangi bir ücret alınmaz.
* **MATLAB, paralel bilgi işlem araç ve MATLAB dağıtılmış bilgi işlem sunucusu lisansları** -betikleri varsayımında [MathWorks barındırılan Lisans Yöneticisi](http://www.mathworks.com/products/parallel-computing/mathworks-hosted-license-manager/) için tüm lisanslar kullanılıyor.  
* **MATLAB dağıtılmış bilgi işlem sunucusu yazılım** -VMs küme için temel VM görüntü kullanılacak bir VM üzerinde yüklü.

## <a name="high-level-steps"></a>Yüksek düzey adımları
MATLAB dağıtılmış bilgi işlem sunucusu kümeleri için Azure sanal makineleri kullanmak için aşağıdaki üst düzey adımları gereklidir. Ayrıntılı yönergeler olan Hızlı Başlangıç şablonu ve betikleri üzerinde eşlik eden belgelerinde [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/matlab-cluster).

1. **Temel VM görüntüsü oluşturma**  

   * Bu VM MATLAB dağıtılmış bilgi işlem sunucusu yazılımları yükleyip yeniden açın.

     > [!NOTE]
     > Bu işlem birkaç saat sürebilir ancak MATLAB her sürümü için kullandığınız sonra bunu yapmak yeterlidir.   
     >
     >
2. **Bir veya daha fazla kümeleri oluşturma**  

   * Sağlanan PowerShell betiğini kullanın veya bir küme temel VM görüntüsünü oluşturmak için Hızlı Başlangıç şablonu kullanın.   
   * Liste, duraklatma, sürdürme olanak tanıyan sağlanan PowerShell Betiği kullanılarak kümelerini yönetmek ve kümeleri silin.

## <a name="cluster-configurations"></a>Küme yapılandırmaları
Şu anda, şablon ve küme oluşturma komut dosyasında tek bir MATLAB dağıtılmış bilgi işlem sunucusu topolojisi oluşturmanızı sağlar. İsterseniz, bir veya daha fazla ek kümeler, çalışan sanal makineleri farklı VM boyutlarını kullanarak, farklı sayıda sahip her küme oluşturun ve benzeri.

### <a name="matlab-client-and-cluster-in-azure"></a>MATLAB istemci ve küme Azure
MATLAB istemcisi düğümü, MATLAB İş Zamanlayıcısı düğümü ve MATLAB dağıtılmış bilgi işlem sunucusu "alt" düğümler tüm Azure Vm'leri olarak bir sanal ağ aşağıdaki resimde gösterildiği gibi yapılandırılır.


* Küme kullanmak için istemci düğüme tarafından Uzak Masaüstü'nü bağlayın. İstemci düğüm MATLAB istemci çalıştırır.
* İstemci düğümün tüm çalışanlar tarafından erişilebilir bir dosya paylaşımı vardır.
* MathWorks barındırılan Lisans Yöneticisi tüm MATLAB yazılımlar için lisans denetimleri için kullanılır.
* Varsayılan olarak, bir MATLAB dağıtılmış bilgi işlem sunucusu çalışan vCPU başına çalışan sanal makineleri üzerinde oluşturulur, ancak herhangi bir sayı belirtin.

## <a name="use-an-azure-based-cluster"></a>Azure tabanlı bir küme kullanın
Olarak MATLAB dağıtılmış bilgi işlem sunucu kümeleri, diğer türleri, küme Profil Yöneticisi'nde (VM istemcide) MATLAB istemci MATLAB İş Zamanlayıcısı küme profili oluşturmak için kullanmanız gerekir.

![Küme Profil Yöneticisi](./media/matlab-mdcs-cluster/cluster_profile_manager.png)

## <a name="next-steps"></a>Sonraki adımlar
* Dağıtma ve Azure MATLAB dağıtılmış bilgi işlem sunucusu kümelerinde yönetme hakkında ayrıntılı yönergeler için bkz [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/matlab-cluster) şablonları ve komut dosyaları içeren havuz.
* Git [MathWorks site](http://www.mathworks.com/) ilgili ayrıntılı bilgiler MATLAB ve MATLAB dağıtılmış bilgi işlem sunucusu için.
