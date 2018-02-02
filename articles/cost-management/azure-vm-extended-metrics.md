---
title: "Azure sanal makineleri için genişletilmiş ölçüm Ekle | Microsoft Docs"
description: "Bu makalede etkinleştirmek ve Azure Vm'leriniz için genişletilmiş tanılama ölçümleri yapılandırmanıza yardımcı olur."
services: cost-management
keywords: 
author: bandersmsft
ms.author: banders
ms.date: 01/30/2018
ms.topic: article
ms.service: cost-management
manager: carmonm
ms.custom: 
ms.openlocfilehash: 91797aaab1dca96e78643f57776eb16d336e894b
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2018
---
# <a name="add-extended-metrics-for-azure-virtual-machines"></a>Azure sanal makineleri için genişletilmiş ölçüm Ekle

Maliyet Yönetimi yazılımını Azure Vm'leriniz Azure ölçüm verileri kaynakları hakkında bilgi ayrıntılı göstermek için kullanır. Performans sayaçları, olarak da bilinir ölçüm verileri raporlar üretmek için maliyet Yönetimi tarafından kullanılır. Ancak, maliyet yönetim otomatik olarak tüm Azure ölçüm verileri Konuk Vm'lerden toplayın değildir — ölçüm koleksiyonu etkinleştirmeniz gerekir. Bu makalede etkinleştirmek ve ek tanılama ölçümleri Azure Vm'leriniz için yapılandırmanıza yardımcı olur.

Ölçüm koleksiyonu etkinleştirdikten sonra şunları yapabilirsiniz:

- Vm'leriniz kendi bellek, disk ve CPU sınırları zaman ulaşıyor bilirsiniz.
- Kullanım eğilimleri ve anormallikleri algıla.
- Kullanım göre boyutlandırma tarafından maliyetleriniz denetler.
- Maliyet etkili maliyet yönetimi en iyi duruma getirme önerileri boyutlandırma alın.

Örneğin, % CPU ve bellek yüzdesi Azure Vm'leriniz izlemek isteyebilirsiniz. Azure VM ölçümleri karşılık _[ana bilgisayar] Yüzde CPU_ ve _[Konuk] bellek yüzdesi_.

## <a name="verify-that-metrics-are-enabled-on-vms"></a>Ölçümleri VM'ler üzerinde etkinleştirildiğini doğrulayın

1. http://portal.azure.com sayfasından Azure portalda oturum açın.
2. Altında **sanal makineleri**, bir VM seçin ve ardından **izleme**seçin **ölçümleri**. Kullanılabilir ölçümler listesi gösterilir.
3. Bazı ölçümleri seçin ve veri için bir grafik görüntüler.  
    ![Örnek ölçüm – konak Yüzde CPU](./media/azure-vm-extended-metrics/metric01.png)

Önceki örnekte, ana bilgisayarlar için standart ölçümleri sınırlı sayıda kullanılabilir, ancak bellek ölçümleri değildir. Bellek ölçümleri genişletilmiş ölçümleri bir parçasıdır. Genişletilmiş ölçümleri etkinleştirmek için bazı ek adımlar gerçekleştirmeniz gerekir. Aşağıdaki bilgiler etkinleştirme adımları sırasında size kılavuzluk eder.

## <a name="enable-extended-metrics-in-the-azure-portal"></a>Azure portalında genişletilmiş ölçümleri etkinleştir

Ana bilgisayar ölçümleri standart ölçümleridir. _[Ana bilgisayar] Yüzde CPU_ ölçüm bir örnek verilmiştir. Ayrıca Konuk VM'ler için temel ölçümleri vardır ve genişletilmiş ölçümleri de adlandırılırlar. Genişletilmiş ölçümleri örneklerindendir _[Konuk] bellek yüzdesi_ ve _[Konuk] bellek_.

Genişletilmiş ölçümlerini etkinleştirme basittir. Her bir VM Konuk düzeyinde izlemeyi etkinleştirin. Konuk düzeyinde izlemeyi etkinleştirdiğinizde, Azure tanılama Aracısı VM üzerinde yüklü. Aşağıdaki aynı Klasik ve normal VM'ler için ve Windows ve Linux VM'ler için aynı işlemidir.

### <a name="enable-guest-level-monitoring-on-existing-vms"></a>Varolan sanal makineler Konuk düzeyinde izlemeyi etkinleştir

1. İçinde **sanal makineleri**, Vm'leriniz listesini görüntüleyin ve bir VM seçin.
2. Altında **izleme**seçin **ölçümleri**.
3. Tıklatın **tanılama ayarlarını**.
4. Tanılama Ayarları sayfasında, tıklatın **Konuk düzeyinde izlemeyi etkinleştir**. Linux VM'ler varolan bir depolama hesabı gerektirir. Ardından bir Windows VM için bir depolama hesabı seçmezseniz sizin için oluşturulur.  
    ![Konuk düzeyinde izlemeyi etkinleştir](./media/azure-vm-extended-metrics/enable-guest-monitoring.png)
5. Birkaç dakika sonra Azure tanılama Aracısı VM üzerinde yüklü. Sayfayı yenileyin ve kullanılabilir ölçümler listesi Konuk Ölçümleriyle güncelleştirilir.  
    ![Genişletilmiş ölçümleri](./media/azure-vm-extended-metrics/extended-metrics.png)

### <a name="enable-guest-level-monitoring-on-new-vms"></a>Yeni sanal makine Konuk düzeyinde izlemeyi etkinleştir

Yeni VM'ler oluşturduğunuzda, belirlediğinizden emin olun **konuk işletim sistemi tanılama**. Linux VM'ler varolan bir depolama hesabı gerektirir. Ardından bir Windows VM için bir depolama hesabı seçmezseniz sizin için oluşturulur.

![Konuk işletim sistemi tanılamayı etkinleştirin](./media/azure-vm-extended-metrics/new-enable-diag.png)

## <a name="resource-manager-credentials"></a>Kaynak Yöneticisi kimlik bilgileri

Genişletilmiş ölçümleri etkinleştirdikten sonra maliyet yönetim erişimi olduğundan emin olun, [Kaynak Yöneticisi kimlik bilgilerini](activate-subs-accounts.md). Kimlik bilgilerinizi toplamak ve VM'ler için performans verilerini görüntülemek maliyet yönetimi için gereklidir. Bunlar ayrıca maliyet iyileştirmesi öneriler oluşturmak için kullanılır. Maliyet yönetim en az üç gün örneğinden downsizing öneri aday olup olmadığını belirlemek için performans verilerini gerekir.

## <a name="enable-vm-metrics-with-a-script"></a>Bir komut dosyası ile VM ölçümleri etkinleştir

Azure PowerShell komut dosyalarıyla VM ölçümleri etkinleştirebilirsiniz. Ölçümleri etkinleştirmek istediğiniz birçok VM olduğunda işlemini otomatikleştirmek için bir komut dosyası kullanabilirsiniz. Örnek betiklerdir github'da [Azure tanılamayı etkinleştir](https://github.com/Cloudyn/azure-enable-diagnostics).

## <a name="view-azure-performance-metrics"></a>Görünüm Azure performans ölçümleri

Cloudyn portalındaki Azure örneklerinizin performans ölçümlerini görüntülemek için gidin **varlıklar** > **işlem** > **örneği Explorer**. VM örnekleri listesinde, örneği ve ayrıntılarını görüntülemek için kaynak'ı genişletin.

![Örnek Gezgini](./media/azure-vm-extended-metrics/instance-explorer.png)

## <a name="next-steps"></a>Sonraki adımlar

- Hesaplarınız için Azure Resource Manager API erişimini zaten etkinleştirmediyseniz devam [etkinleştirme Azure abonelikleri ve hesapları](activate-subs-accounts.md)
