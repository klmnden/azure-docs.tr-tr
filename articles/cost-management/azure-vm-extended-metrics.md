---
title: Azure sanal makineleri için genişletilmiş ölçümler ekleme | Microsoft Docs
description: Bu makalede etkinleştirmek ve Azure sanal makineleriniz için genişletilmiş bir tanılama ölçümünü yapılandırmanıza yardımcı olur.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 06/12/2018
ms.topic: conceptual
ms.service: cost-management
manager: dougeby
ms.custom: ''
ms.openlocfilehash: b7e4665dc3579f357ce1e28bf34be35c931736bd
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2018
ms.locfileid: "35647671"
---
# <a name="add-extended-metrics-for-azure-virtual-machines"></a>Azure sanal makineleri için genişletilmiş ölçümler ekleme

Maliyet Yönetimi kaynakları hakkında bilgi ayrıntılı göstermek için Azure sanal makinelerinize Azure ölçüm verileri kullanır. Ölçüm verilerini, performans sayaçları, olarak da bilinen maliyet yönetimi ile raporlar oluşturmak için kullanılır. Bununla birlikte, maliyet Yönetimi otomatik olarak tüm Azure ölçüm verileri Konuk Vm'lerden toplamak — ölçüm toplama etkinleştirmeniz gerekir. Bu makalede, Azure sanal makineleriniz için ek tanılama ölçümünü yapılandırmak ve etkinleştirmek yardımcı olur.

Ölçüm toplama etkinleştirdikten sonra şunları yapabilirsiniz:

- Vm'lerinizi, bellek, disk ve CPU sınırlarını ulaşma ne zaman bilirsiniz.
- Kullanım eğilimleri ve anormallikleri algılar.
- Boyutlandırma kullanım göre maliyetlerinizi kontrol.
- Maliyet etkin boyutlandırma maliyet Yönetimi iyileştirme önerileri alın.

Örneğin, Azure sanal makinelerinizin bellek % ve % CPU izlemek isteyebilirsiniz. Azure VM ölçümleri karşılık _[konak] CPU yüzdesi_ ve _[Konuk] bellek yüzdesi_.

> [!NOTE]
> Genişletilmiş ölçüm verileri toplama yalnızca Azure Konuk düzeyinde izlemeyi ile desteklenir. Maliyet Yönetimi Log Analytics VM uzantısı ile uyumlu değil.

## <a name="verify-that-metrics-are-enabled-on-vms"></a>Ölçümleri VM'ler üzerinde etkinleştirildiğini doğrulayın

1. http://portal.azure.com adresinden Azure portalında oturum açın.
2. Altında **sanal makineler**, bir sanal Makineyi seçin ve ardından altındaki **izleme**seçin **ölçümleri**. Kullanılabilir ölçümler bir listesi gösterilir.
3. Bazı ölçümler seçin ve bunlar için verileri bir grafiği görüntüler.  
    ![Örnek ölçüm – konak CPU yüzdesi](./media/azure-vm-extended-metrics/metric01.png)

Yukarıdaki örnekte, sınırlı sayıda standart ölçüm konaklarınız için kullanılabilir, ancak bellek ölçümleri değil. Bellek ölçümleri genişletilmiş ölçümler bir parçasıdır. Genişletilmiş ölçümleri etkinleştirmek üzere bazı ek adımlar gerçekleştirmeniz gerekir. Aşağıdaki bilgiler, etkinleştirme işleminde size yol gösterir.

## <a name="enable-extended-metrics-in-the-azure-portal"></a>Azure portalında genişletilmiş ölçümlerini etkinleştir

Standart ölçümler, ana bilgisayar ölçümleridir. _[Konak] CPU yüzdesi_ ölçüm, bir örnek verilmiştir. Genişletilmiş ölçümler de adlandırılırlar ve Konuk sanal makineler için temel ölçümleri de vardır. Genişletilmiş ölçümler örnekler _[Konuk] bellek yüzdesi_ ve _[Konuk] bellek_.

Genişletilmiş ölçümlerini etkinleştirme oldukça basittir. Her VM için konuk düzeyinde izlemeyi etkinleştir. Konuk düzeyinde izlemeyi etkinleştirdiğinizde, Azure tanılama aracısını sanal makinede yüklü. Aşağıdaki normal ve klasik VM'ler için aynı ve Windows ve Linux Vm'leri için aynı işlemidir.

Bir depolama hesabı hem Azure hem de Linux Konuk düzeyinde izlemeyi gerektiğini aklınızda bulundurun. Daha sonra mevcut bir depolama hesabı seçmezseniz, Konuk düzeyinde izlemeyi etkinleştirdiğinizde, bir sizin için oluşturulur.

### <a name="enable-guest-level-monitoring-on-existing-vms"></a>Var olan Vm'lerde Konuk düzeyinde izlemeyi etkinleştir

1. İçinde **sanal makineler**, sanal makinelerinizin listenizi görüntüleyin ve sonra bir sanal Makineyi seçin.
2. Altında **izleme**seçin **ölçümleri**.
3. Tıklayın **tanılama ayarları**.
4. Tanılama Ayarları sayfasında tıklayın **Konuk düzeyinde izlemeyi etkinleştir**.  
    ![Konuk düzeyinde izlemeyi etkinleştir](./media/azure-vm-extended-metrics/enable-guest-monitoring.png)
5. Birkaç dakika sonra Azure tanılama aracısını sanal makinede yüklü. Sayfayı yenileyin ve Konuk ölçümleri ile kullanılabilir ölçümlerin listesi güncelleştirilir.  
    ![Genişletilmiş ölçümler](./media/azure-vm-extended-metrics/extended-metrics.png)

### <a name="enable-guest-level-monitoring-on-new-vms"></a>Yeni Vm'lerde Konuk düzeyinde izlemeyi etkinleştir

Yeni VM'ler oluşturduğunuzda, belirlediğinizden emin olun **konuk işletim sistemi tanılama**.

![Konuk işletim sistemi tanılamayı etkinleştirme](./media/azure-vm-extended-metrics/new-enable-diag.png)

## <a name="resource-manager-credentials"></a>Kaynak Yöneticisi kimlik bilgileri

Genişletilmiş ölçümler etkinleştirdikten sonra maliyet Yönetimi erişimi olduğundan emin olun, [Resource Manager kimlik bilgilerini](activate-subs-accounts.md). Kimlik bilgilerinizi toplamak ve Vm'leriniz için performans verilerini görüntülemek maliyet yönetimi için gereklidir. Maliyet iyileştirme önerileri oluşturmak için de kullanılırlar. Maliyet yönetimi, performans verileri örneğini bir aday downsizing önerinin olup olmadığını belirlemek için en az üç gün gerekir.

## <a name="enable-vm-metrics-with-a-script"></a>Bir betik ile VM ölçümlerini etkinleştir

Azure PowerShell betikleri ile VM ölçümlerini etkinleştirebilirsiniz. Ölçümler üzerinde etkinleştirmek istediğiniz birçok VM olduğunda işlemini otomatikleştirmek için bir komut dosyası kullanabilirsiniz. Örnek betikleridir github'da [Azure tanılamayı etkinleştir](https://github.com/Cloudyn/azure-enable-diagnostics).

## <a name="view-azure-performance-metrics"></a>Azure performans ölçümlerini görüntüleme

Cloudyn portalında, Azure örnekleri üzerinde performans ölçümlerini görüntülemek için gidin **varlıklar** > **işlem** > **örnek Gezgini**. Sanal makine örneklerinin listesini, örneğini genişletin ve ardından ayrıntılarını görüntülemek için bir kaynak genişletin.

![Örnek Gezgini](./media/azure-vm-extended-metrics/instance-explorer.png)

## <a name="next-steps"></a>Sonraki adımlar

- Hesaplarınız için zaten Azure Resource Manager API'si erişimini etkinleştirmediyseniz devam [etkinleştirme Azure aboneliklerini ve hesaplarını](activate-subs-accounts.md).
