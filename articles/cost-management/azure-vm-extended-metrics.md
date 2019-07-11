---
title: Azure sanal makineleri için genişletilmiş ölçümler ekleme | Microsoft Docs
description: Bu makalede etkinleştirmek ve Azure sanal makineleriniz için genişletilmiş bir tanılama ölçümünü yapılandırmanıza yardımcı olur.
services: cost-management
keywords: ''
author: bandersmsft
manager: vitavor
ms.author: banders
ms.date: 05/21/2019
ms.topic: conceptual
ms.service: cost-management
ms.custom: seodec18
ms.openlocfilehash: 6a4f7f5671562679a245d97ad8491764657cbb34
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66242457"
---
# <a name="add-extended-metrics-for-azure-virtual-machines"></a>Azure sanal makineleri için genişletilmiş ölçümler ekleme

Cloudyn Azure vm'lerinizden Azure ölçüm veri kaynakları hakkında bilgi ayrıntılı göstermek için kullanır. Ölçüm verilerini, performans sayaçları, olarak da bilinir, Cloudyn tarafından raporlar oluşturmak için kullanılır. Bununla birlikte, Cloudyn otomatik olarak tüm Azure ölçüm verileri Konuk Vm'lerden toplamak — ölçüm toplama etkinleştirmeniz gerekir. Bu makalede, Azure sanal makineleriniz için ek tanılama ölçümünü yapılandırmak ve etkinleştirmek yardımcı olur.

Ölçüm toplama etkinleştirdikten sonra şunları yapabilirsiniz:

- Vm'lerinizi, bellek, disk ve CPU sınırlarını ulaşma ne zaman bilirsiniz.
- Kullanım eğilimleri ve anormallikleri algılar.
- Boyutlandırma kullanım göre maliyetlerinizi kontrol.
- Maliyet etkin boyutlandırma Cloudyn iyileştirme önerileri alın.

Örneğin, Azure sanal makinelerinizin bellek % ve % CPU izlemek isteyebilirsiniz. Azure VM ölçümleri karşılık _CPU yüzdesi_ ve _\Memory\% bayt kullanımda kaydedilen_.

> [!NOTE]
> Genişletilmiş ölçüm verileri toplama yalnızca Azure Konuk düzeyinde izlemeyi ile desteklenir. Cloudyn Azure İzleyici günlüklerine VM uzantısı ile uyumlu değil.

## <a name="determine-whether-extended-metrics-are-enabled"></a>Genişletilmiş ölçümler etkinleştirilip etkinleştirilmediğini belirleme

1. [https://portal.azure.com](https://portal.azure.com ) adresinden Azure portalında oturum açın.
2. Altında **sanal makineler**, bir sanal Makineyi seçin ve ardından altındaki **izleme**seçin **ölçümleri**. Kullanılabilir ölçümler bir listesi gösterilir.
3. Bazı ölçümler seçin ve bunlar için verileri bir grafiği görüntüler.  
    ![Örnek ölçüm – konak CPU yüzdesi](./media/azure-vm-extended-metrics/metric01.png)

Yukarıdaki örnekte, sınırlı sayıda standart ölçüm konaklarınız için kullanılabilir, ancak bellek ölçümleri değil. Bellek ölçümleri genişletilmiş ölçümler bir parçasıdır. Bu durumda, sanal makine için genişletilmiş ölçümler etkin değil. Genişletilmiş ölçümleri etkinleştirmek üzere bazı ek adımlar gerçekleştirmeniz gerekir. Aşağıdaki bilgiler, etkinleştirme işleminde size yol gösterir.

## <a name="enable-extended-metrics-in-the-azure-portal"></a>Azure portalında genişletilmiş ölçümlerini etkinleştir

Standart ölçümler, ana bilgisayar ölçümleridir. _CPU yüzdesi_ ölçüm, bir örnek verilmiştir. Genişletilmiş ölçümler de adlandırılırlar ve Konuk sanal makineler için temel ölçümleri de vardır. Genişletilmiş ölçümler örnekler _\Memory\% bayt kullanımda kaydedilen_ ve _\Memory\Available bayt_.

Genişletilmiş ölçümlerini etkinleştirme oldukça basittir. Her VM için konuk düzeyinde izlemeyi etkinleştir. Konuk düzeyinde izlemeyi etkinleştirdiğinizde, Azure tanılama aracısını sanal makinede yüklü. Varsayılan olarak, genişletilmiş ölçümleri temel bir dizi eklenir. Aşağıdaki normal ve klasik VM'ler için aynı ve Windows ve Linux Vm'leri için aynı işlemidir.

Bir depolama hesabı hem Azure hem de Linux Konuk düzeyinde izlemeyi gerektiğini aklınızda bulundurun. Daha sonra mevcut bir depolama hesabı seçmezseniz, Konuk düzeyinde izlemeyi etkinleştirdiğinizde, bir sizin için oluşturulur.

### <a name="enable-guest-level-monitoring-on-existing-vms"></a>Var olan Vm'lerde Konuk düzeyinde izlemeyi etkinleştir

1. İçinde **sanal makineler**, sanal makinelerinizin listenizi görüntüleyin ve sonra bir sanal Makineyi seçin.
2. Altında **izleme**seçin **tanılama ayarları**.
3. Tanılama Ayarları sayfasında tıklayın **Konuk düzeyinde izlemeyi etkinleştir**.  
    ![Genel bakış sayfasında Konuk düzeyinde izlemeyi etkinleştir](./media/azure-vm-extended-metrics/enable-guest-monitoring.png)
4. Birkaç dakika sonra Azure tanılama aracısını sanal makinede yüklü. Temel ölçümler birtakım eklenir. Sayfayı yenileyin. Ek performans sayaçları genel bakış sekmesinde görünür.
5. İzleme bölümünden seçin **ölçümleri**.
6. Ölçümleri grafiğinde altında **ölçüm Namespace**seçin **Konuk (Klasik)** .
7. Ölçüm listesinde, tüm performans sayaçlarının Konuk VM görüntüleyebilirsiniz.  
    ![Genişletilmiş örnek ölçümlerin listesi](./media/azure-vm-extended-metrics/extended-metrics.png)

### <a name="enable-guest-level-monitoring-on-new-vms"></a>Yeni Vm'lerde Konuk düzeyinde izlemeyi etkinleştir

Yeni VM'ler, Yönetim sekmesinde oluşturduğunuzda **üzerinde** için **işletim sistemi Konuk tanılama**.

![Konuk işletim sistemi tanılama açık olarak ayarlayın](./media/azure-vm-extended-metrics/new-enable-diag.png)

Azure sanal makineleri için genişletilmiş ölçümler etkinleştirme hakkında daha fazla bilgi için bkz: [anlama ve Azure Linux Aracısı'nı kullanarak](../virtual-machines/extensions/agent-linux.md) ve [Azure sanal makine Aracısı genel bakış](../virtual-machines/extensions/agent-windows.md).

## <a name="resource-manager-credentials"></a>Kaynak Yöneticisi kimlik bilgileri

Genişletilmiş ölçümler etkinleştirdikten sonra Cloudyn erişimi olduğundan emin olun, [Resource Manager kimlik bilgilerini](activate-subs-accounts.md). Kimlik bilgilerinizi toplamak ve Vm'leriniz için performans verilerini görüntülemek Cloudyn gereklidir. Maliyet iyileştirme önerileri oluşturmak için de kullanılırlar. Cloudyn, en az üç gün örneğinden bir aday downsizing önerinin olup olmadığını belirlemek için performans verilerini gerekir.

## <a name="enable-vm-metrics-with-a-script"></a>Bir betik ile VM ölçümlerini etkinleştir

Azure PowerShell betikleri ile VM ölçümlerini etkinleştirebilirsiniz. Ölçümler üzerinde etkinleştirmek istediğiniz birçok VM olduğunda işlemini otomatikleştirmek için bir komut dosyası kullanabilirsiniz. Örnek betikleridir github'da [Azure tanılamayı etkinleştir](https://github.com/Cloudyn/azure-enable-diagnostics).

## <a name="view-azure-performance-metrics"></a>Azure performans ölçümlerini görüntüleme

Cloudyn portalında, Azure örnekleri üzerinde performans ölçümlerini görüntülemek için gidin **varlıklar** > **işlem** > **örnek Gezgini**. Sanal makine örneklerinin listesini, örneğini genişletin ve ardından ayrıntılarını görüntülemek için bir kaynak genişletin.

![Örnek Gezgini'nde gösterilen örnek bilgileri](./media/azure-vm-extended-metrics/instance-explorer.png)

## <a name="next-steps"></a>Sonraki adımlar

- Hesaplarınız için zaten Azure Resource Manager API'si erişimini etkinleştirmediyseniz devam [etkinleştirme Azure aboneliklerini ve hesaplarını](activate-subs-accounts.md).
