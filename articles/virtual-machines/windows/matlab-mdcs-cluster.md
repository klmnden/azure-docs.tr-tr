---
title: MATLAB kümesi üzerindeki sanal makinelerin | Microsoft Docs
description: Microsoft Azure sanal makineler, yoğun işlem gücü kullanımlı paralel MATLAB iş yüklerini çalıştırmak için MATLAB dağıtılmış bilgi işlem sunucusu kümeleri oluşturmak için kullanın
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
ms.openlocfilehash: 49824741facc8822a9417306794f1028fc180e16
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60555137"
---
# <a name="create-matlab-distributed-computing-server-clusters-on-azure-vms"></a>Azure Vm'lerinde MATLAB dağıtılmış bilgi işlem sunucusu kümeleri oluşturma
Microsoft Azure sanal makineler, yoğun işlem gücü kullanımlı paralel MATLAB iş yüklerini çalıştırmak için bir veya daha fazla MATLAB dağıtılmış bilgi işlem sunucusu kümeleri oluşturmak için kullanın. Temel görüntü olarak ve bir Azure Hızlı Başlangıç şablonu veya Azure PowerShell Betiği kullanmak, bir VM'de MATLAB dağıtılmış bilgi işlem sunucu yazılımınızı yüklemeniz (kullanılabilir [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/matlab-cluster)) dağıtmak ve kümeyi yönetmek için. Dağıtımdan sonra iş yüklerinizi çalıştırmak için kümeye bağlanın.

## <a name="about-matlab-and-matlab-distributed-computing-server"></a>MATLAB ve MATLAB dağıtılmış bilgi işlem sunucusu hakkında
[MATLAB](https://www.mathworks.com/products/matlab/) platform, mühendislik ve bilimsel sorunları çözmek için getirilmiştir. Büyük ölçekli simülasyonlar ve veri işleme görevlerini MATLAB kullanıcılarla ürünleri bilgi işlem MathWorks paralel işlem kümeleri ve kılavuz Hizmetleri avantajlarından yararlanarak, yoğun işlem gücü kullanımlı iş yüklerini hızlandırmak için kullanabilirsiniz. [Paralel bilgi işlem araç kutusu](https://www.mathworks.com/products/parallel-computing/) uygulamaları paralel hale getirmek ve çok çekirdekli işlemcilerde, Gpu'lar, avantajlarından yararlanın ve işlem kümelerindeki MATLAB kullanıcıların olanak sağlar. [MATLAB dağıtılmış bilgi işlem sunucusu](https://www.mathworks.com/products/distriben/) MATLAB kullanıcıların işlem kümesi çok bilgisayar kullanmasına olanak tanır.

Azure sanal makinelerini kullanarak, aynı mekanizmaları etkileşimli işleri, toplu işleri, bağımsız görevler ve iletişim gibi şirket içi kümeleri olarak paralel işi göndermek kullanılabilir olan MATLAB dağıtılmış bilgi işlem sunucusu kümeleri oluşturabilirsiniz görevler. MATLAB platformu ile birlikte Azure'ı kullanarak sağlama için karşılaştırıldığında birçok faydası vardır ve geleneksel kullanarak şirket içi donanım: bir dizi sanal makine boyutları, kümeleri yalnızca kullandığınız işlem kaynakları için ödeme yapmanızı üzerine oluşturulmasını ve uygun ölçekte modelleri test edebilme olanağı.  

## <a name="prerequisites"></a>Önkoşullar
* **İstemci bilgisayar** -dağıtımdan sonra MATLAB dağıtılmış bilgi işlem sunucu kümesi ve Azure ile iletişim kurmak için bir Windows tabanlı bir istemci bilgisayar gerekir.
* **Azure PowerShell** -bkz [Azure PowerShell'i yükleme ve yapılandırma işlemini](/powershell/azure/overview) istemci bilgisayarınıza yüklemek için.
* **Azure aboneliği** -bir aboneliğiniz yoksa, oluşturabileceğiniz bir [ücretsiz bir hesap](https://azure.microsoft.com/free/) yalnızca birkaç dakika içinde. Daha büyük kümeler için bir Kullandıkça Öde aboneliğine veya diğer satın alma seçeneklerini göz önünde bulundurun.
* **Vcpu kotası** -büyük bir küme veya birden fazla MATLAB dağıtılmış bilgi işlem sunucusu kümesi dağıtmak için vCPU kotası artırmanız gerekebilir. Bir Kotayı artırmak için [bir çevrimiçi müşteri destek isteği açın](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) ücret olmadan.
* **MATLAB, paralel bilgi işlem araç ve MATLAB dağıtılmış bilgi işlem sunucusu lisansları** -betikleri varsayımında [MathWorks barındırılan Lisans Yöneticisi](https://www.mathworks.com/help/install/license-management.html) tüm lisansları için kullanılır.  
* **MATLAB dağıtılmış bilgi işlem sunucu yazılımı** -küme Vm'leri için temel VM görüntüsü olarak kullanılacak bir VM'de yüklü.

## <a name="high-level-steps"></a>Yüksek düzey adımları
Azure sanal makineleri MATLAB dağıtılmış bilgi işlem Sunucu kümeleriniz için kullanmak için aşağıdaki üst düzey adımları gereklidir. Ayrıntılı yönergeler olan Hızlı Başlangıç şablonu ve betikleri üzerinde eşlik eden belgelerinde [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/matlab-cluster).

1. **Temel bir VM görüntüsü oluşturma**  

   * Bu VM MATLAB dağıtılmış bilgi işlem sunucusu yazılımları indirip yeniden açın.

     > [!NOTE]
     > Bu işlem birkaç saat sürebilir ancak MATLAB her sürümü için kullandığınız sonra bunu yapmak yeterlidir.   
     >
     >
2. **Bir veya daha fazla küme oluşturma**  

   * Sağlanan PowerShell Betiği veya temel bir sanal makine görüntüsünden bir küme oluşturmak için Hızlı Başlangıç şablonu kullanın.   
   * Liste, duraklatma, sürdürme olanak tanıyan sağlanan PowerShell betiğini kullanarak kümeleri yönetme ve kümelerini silin.

## <a name="cluster-configurations"></a>Küme yapılandırmaları
Şu anda, küme oluşturma komut ve şablonu tek bir MATLAB dağıtılmış bilgi işlem sunucusu topolojisi oluşturmanıza olanak sağlar. İsterseniz, bir veya daha fazla ek kümeler, farklı sayıda çalışan VM'ler, farklı VM boyutları kullanarak sahip her küme oluşturun ve benzeri.

### <a name="matlab-client-and-cluster-in-azure"></a>MATLAB istemci ve azure'da küme
MATLAB istemci düğümü, MATLAB İş Zamanlayıcısı düğüm ve MATLAB dağıtılmış bilgi işlem sunucu "alt" düğümler tüm Azure Vm'leri olarak bir sanal ağ içinde aşağıdaki şekilde gösterildiği gibi yapılandırılır.


* Küme kullanmak için istemci düğümü tarafından Uzak Masaüstü'nü bağlanın. İstemci düğümü MATLAB istemci çalıştırır.
* İstemci düğümü tüm çalışanları tarafından erişilebilir bir dosya paylaşımı vardır.
* MathWorks barındırılan Lisans Yöneticisi, tüm MATLAB yazılımlar için lisans denetimleri için kullanılır.
* Varsayılan olarak, bir MATLAB dağıtılmış bilgi işlem sunucusu çalışan vCPU başına çalışan sanal makineleri üzerinde oluşturulur, ancak herhangi bir sayı belirtebilirsiniz.

## <a name="use-an-azure-based-cluster"></a>Azure tabanlı bir küme kullanın
Olarak MATLAB dağıtılmış bilgi işlem sunucu kümeleri, diğer türleri kümesi profili Yöneticisi'nde MATLAB istemcide (istemci VM) bir iş Zamanlayıcısı MATLAB kümesi profili oluşturmak için kullanmanız gerekir.

![Küme Profil Yöneticisi](./media/matlab-mdcs-cluster/cluster_profile_manager.png)

## <a name="next-steps"></a>Sonraki adımlar
* Dağıtma ve Azure MATLAB dağıtılmış bilgi işlem sunucusu kümelerini yönetme hakkında ayrıntılı yönergeler için bkz: [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/matlab-cluster) komut dosyaları ve şablonları içeren havuz.
* Git [MathWorks site](https://www.mathworks.com/) MATLAB ve MATLAB dağıtılmış bilgi işlem sunucusu için ayrıntılı belgeler için.
