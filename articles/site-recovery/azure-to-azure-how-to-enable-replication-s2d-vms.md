---
title: Azure Site Recovery'de (S2d) VM'ler depolama alanları doğrudan için çoğaltma yapılandırma | Microsoft Docs
description: Bu makalede bir Azure bölgesinden diğerine Site RECOVERY'yi kullanarak, S2D sahip VM'ler için çoğaltma yapılandırma açıklanır.
services: site-recovery
author: asgang
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 01/29/2019
ms.author: asgang
ms.openlocfilehash: 6c639d4503b170660abed5767e3571c8a2bf24b9
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60790342"
---
# <a name="replicate-azure-virtual-machines-using-storage-spaces-direct-to-another-azure-region"></a>Depolama alanları doğrudan kullanarak başka bir Azure bölgesine çoğaltma Azure sanal makineler

Bu makalede, Azure Vm'lerinde çalışan depolama alanları doğrudan için olağanüstü durum kurtarmayı etkinleştirmeyi açıklar.

>[!NOTE]
>Yalnızca kilitlenme tutarlı kurtarma noktaları, depolama alanları doğrudan kümeleri için desteklenir.
>

## <a name="introduction"></a>Giriş 
[Depolama alanları doğrudan (S2D)](https://docs.microsoft.com/windows-server/storage/storage-spaces/deploy-storage-spaces-direct) oluşturmak için bir yol sağlayan bir yazılım tanımlı depolama, [Konuk kümeleri](https://blogs.msdn.microsoft.com/clustering/2017/02/14/deploying-an-iaas-vm-guest-clusters-in-microsoft-azure) azure'da.  Bir konuk Microsoft azure'da Iaas sanal makinelerin yük devretme oluşan kümedir. Tek bir Azure VM sağladığından daha yüksek kullanılabilirlik SLA'sı uygulamalar elde Konuk kümeleri arasında yük devretmek, barındırılan VM iş yükleri sağlar. Burada SQL veya ölçek kritik bir uygulamayı barındıran VM bu dosya sunucusuna vb. gibi senaryolarda yararlıdır.

## <a name="disaster-recovery-of-azure-virtual-machines-using-storage-spaces-direct"></a>Olağanüstü durum kurtarma, Azure depolama alanları doğrudan kullanarak sanal makineleri
Tipik bir senaryoda, azure'da sanal makineler Konuk küme için genişleme dosya sunucusuna ölçek gibi uygulamanızın daha yüksek bir dayanıklılık olabilir. Bu, daha yüksek kullanılabilirlik sağlarken, herhangi bir bölge düzeyinde hata için Site RECOVERY'yi kullanarak bu uygulamaları korumak ister misiniz? Site Recovery, veriler bir bölgeden başka bir Azure bölgesine çoğaltılır ve olağanüstü durum kurtarma bölgesinde bir olay, yük devretme kümesinde getirir.

Aşağıdaki diyagramda iki resimsel temsilini gösteren Azure Vm'leri yük devretme kümesi kullanarak depolama alanları doğrudan.

![storagespacesdirect](./media/azure-to-azure-how-to-enable-replication-s2d-vms/storagespacedirect.png)

 
- Windows Yük devretme kümesinde iki Azure sanal makineleri ve her bir sanal makine iki veya daha fazla veri diski var.
- S2D, verileri diskteki verileri eşitler ve bir depolama havuzu olarak eşitlenmiş depolama sunar.
- Depolama havuzu, yük devretme kümesine Küme Paylaşılan birimi (CSV) olarak gösterir.
- Yük devretme kümesine CSV veri sürücüleri için kullanır.

**Olağanüstü durum kurtarma değerlendirmeleri**

1. Ne zaman ayarladığınız [bulut tanığı](https://docs.microsoft.com/windows-server/failover-clustering/deploy-cloud-witness#CloudWitnessSetUp) kümesi için Tanık olağanüstü durum kurtarma bölgesinde tutun.
2. Daha sonra alt ağda kaynak bölgeden farklı DR bölgesindeki sanal makinelere yük devretmek için kullanacaksanız olması değiştirmek, yük devretme işleminden sonra küme IP adresi gerekir.  ASR kullanması gereken küme IP değiştirmek için [kurtarma planı betiği.](https://docs.microsoft.com/azure/site-recovery/site-recovery-runbook-automation)</br>
[Örnek komut dosyası](https://github.com/krnese/azure-quickstart-templates/blob/master/asr-automation-recovery/scripts/ASR-Wordpress-ChangeMysqlConfig.ps1) özel betik uzantısını kullanarak VM içinde komutu yürütmek için 

### <a name="enabling-site-recovery-for-s2d-cluster"></a>Site Recovery için S2D kümesi etkinleştirme:

1. İçinde bir kurtarma Hizmetleri kasası, tıklayın "+ Çoğalt"
1. Kümedeki tüm düğümleri seçin ve bunları parçası olan bir [çoklu VM tutarlılığı grubu](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-common-questions#multi-vm-consistency)
1. Çoğaltma İlkesi ile uygulama tutarlılığı kapalı seçin. * (kilitlenme tutarlılığı desteği yalnızca kullanılabilir)
1. Çoğaltmayı etkinleştirme

   ![storagespacesdirect koruma](./media/azure-to-azure-how-to-enable-replication-s2d-vms/multivmgroup.png)

2. Çoğaltılan öğeler'e gidin ve her iki sanal makine durumunu görebilirsiniz. 
3. Sanal makinelerin korunur ve aynı zamanda çoklu VM tutarlılığı grubunun bir parçası gösterilir.

   ![storagespacesdirect koruma](./media/azure-to-azure-how-to-enable-replication-s2d-vms/storagespacesdirectgroup.PNG)

## <a name="creating-a-recovery-plan"></a>Bir kurtarma planı oluşturma
Bir kurtarma planı yük devretme sırasında çok katmanlı bir uygulama çeşitli katmanları sıralama destekler. Sıralama, özel olarak uygulama tutarlılığı sürdürmeye yardımcı olur. Çok katmanlı web uygulaması için bir kurtarma planı oluşturduğunuzda, tam adımlar açıklanan [Site Recovery kullanarak bir kurtarma planı oluşturma](site-recovery-create-recovery-plans.md).

### <a name="adding-virtual-machines-to-failover-groups"></a>Sanal makineler için yük devretme grupları ekleme

1.  Sanal makineler ekleyerek bir kurtarma planı oluşturun.
2.  'Sanal makineleri gruplandırmak için Özelleştir' tıklayın. Varsayılan olarak, tüm VM'ler 'Grubu 1' bir parçasıdır.


### <a name="add-scripts-to-the-recovery-plan"></a>Betikleri kurtarma planına ekleyin
Uygulamalarınızın düzgün çalışması, yük devretme işleminden sonra veya bir yük devretme testi sırasında Azure sanal makinelerinde bazı işlemler yapmanız gerekebilir. Bazı yük devretme sonrası işlemleri otomatik hale getirebilirsiniz. Örneğin, burada size loadbalancer bağladığınız ve küme IP değiştirme.


### <a name="failover-of-the-virtual-machines"></a>Sanal makinelerin yük devretme 
Sanal makinelerin her iki düğüm gerekiyor başarısız olması yerine [ASR kurtarma planı](https://docs.microsoft.com/azure/site-recovery/site-recovery-create-recovery-plans) 

![storagespacesdirect koruma](./media/azure-to-azure-how-to-enable-replication-s2d-vms/recoveryplan.PNG)

## <a name="run-a-test-failover"></a>Yük devretme testi çalıştırma
1.  Azure portalında kurtarma Hizmetleri kasanızı seçin.
2.  Oluşturduğunuz kurtarma planı seçin.
3.  **Yük Devretme Testi**'ni seçin.
4.  Test yük devretme işlemini başlatmak için kurtarma noktası ve Azure sanal ağı'nı seçin.
5.  İkincil bir ortamı yukarı olduğunda doğrulamaları gerçekleştirin.
6.  Yük devretme ortamı temizlemek için doğrulamaları tamamlandığı zaman seçin **yük devretme testini Temizle**.

Daha fazla bilgi için [Azure Site recovery'de yük devretme testi](site-recovery-test-failover-to-azure.md).

## <a name="run-a-failover"></a>Yük devretme çalıştırma

1.  Azure portalında kurtarma Hizmetleri kasanızı seçin.
2.  SAP uygulamaları için oluşturduğunuz kurtarma planı seçin.
3.  **Yük devretme**'yi seçin.
4.  Yük devretme işlemini başlatmak için kurtarma noktasını seçin.

Daha fazla bilgi için [Site recovery'de yük devretme](site-recovery-failover.md).
## <a name="next-steps"></a>Sonraki adımlar

[Daha fazla bilgi edinin](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-tutorial-failover-failback) yeniden çalışma çalıştırma hakkında.
