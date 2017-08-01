---
title: "İlk Bakış: Azure sanal makinelerini bir yedekleme kasasıyla yedekleme | Microsoft Belgeleri"
description: "Azure VM’leri bir Backup kasasına yedeklemek için klasik portalı kullanın. Bu öğreticide Backup kasası oluşturma, VM’leri kaydetme, yedekleme ilkesi oluşturma ve ilk yedekleme işini çalıştırma dahil tüm aşamalar açıklanmaktadır."
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
ms.assetid: 722820dc-b65f-425c-a9e5-c1946e896a87
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 6/14/2017
ms.author: markgal;
ms.translationtype: Human Translation
ms.sourcegitcommit: ef1e603ea7759af76db595d95171cdbe1c995598
ms.openlocfilehash: 61328e32763faea90074fc6d499e660c4109ab6d
ms.contentlocale: tr-tr
ms.lasthandoff: 06/16/2017


---
# <a name="first-look-backing-up-azure-virtual-machines"></a>İlk bakış: Azure sanal makinelerini yedekleme
> [!div class="op_single_selector"]
> * [VM'leri bir kurtarma hizmetleri kasasıyla koruma](backup-azure-vms-first-look-arm.md)
> * [Azure VM'lerini yedekleme kasasıyla koruma](backup-azure-vms-first-look.md)
>
>

Bu öğretici bir Azure sanal makinesini (VM) Azure'daki bir yedekleme kasasına yedeklemeye yönelik adımlar boyunca size yol gösterir. Bu makalede sanal makineleri yedeklemeye yönelik klasik model veya Service Manager dağıtım modeli açıklanmaktadır. Aşağıdaki adımlar yalnızca klasik portalda oluşturulan Backup kasaları için geçerlidir. Microsoft, yeni dağıtımlar için Resource Manager modelinin kullanılmasını önerir.

Bir sanal makineyi bir Kaynak Grubuna ait Kurtarma Hizmetleri kasasına yedeklemek istiyorsanız bkz. [İlk bakış: Sanal makineleri bir kurtarma hizmetleri kasasıyla koruma](backup-azure-vms-first-look-arm.md).

Aşağıdaki öğreticiyi başarıyla tamamlamak için şu önkoşulların mevcut olması gerekir:

* Azure aboneliğinizde bir VM oluşturmuş olmanız gerekir.
* VM'nin Azure genel IP adreslerine bağlantısı olmalıdır. Ek bilgi için bkz. [Ağ bağlantısı](backup-azure-vms-prepare.md#network-connectivity).


> [!NOTE]
> Azure'da kaynak oluşturmaya ve kaynaklarla çalışmaya yönelik iki dağıtım modeli mevcuttur: [Resource Manager ve Klasik](../azure-resource-manager/resource-manager-deployment-model.md). Bu öğretici, klasik portalda oluşturulan sanal makinelerle birlikte kullanıma yöneliktir.
>
>

## <a name="create-a-backup-vault"></a>Yedekleme kasası oluşturma
Yedekleme kasası, zaman içinde oluşturulan tüm yedeklemeleri ve kurtarma noktalarını depolayan bir varlıktır. Yedekleme kasası, yedeklenmekte olan sanal makinelere uygulanan yedekleme ilkelerini de içerir.

> [!IMPORTANT]
> Mart 2017’den itibaren Backup kasaları oluşturmak için klasik portalı kullanamayacaksınız.
> Artık Backup kasalarınızı Kurtarma Hizmetleri kasalarına yükseltebilirsiniz. Ayrıntılı bilgi için [Backup kasasını Kurtarma Hizmetleri kasasına yükseltme](backup-azure-upgrade-backup-to-recovery-services.md) makalesine bakın. Microsoft, Backup kasalarınızı Kurtarma Hizmetleri kasalarına yükseltmenizi önerir.<br/> **1 Kasım 2017'den itibaren**:
>- Yükseltilmemiş olan tüm Backup kasaları Kurtarma Hizmetleri kasalarına otomatik olarak yükseltilecektir.
>- Klasik portalda yedekleme verilerinize erişemeyeceksiniz. Bunun yerine, Kurtarma Hizmetleri kasalarındaki yedekleme verilerinize erişmek için Azure portalını kullanabilirsiniz.
>

## <a name="discover-and-register-azure-virtual-machines"></a>Azure sanal makinelerini bulma ve kaydetme
VM'yi bir kasaya kaydetmeden önce yeni VM'leri tanımlamak için bulma işlemini çalıştırın. Bu, bulut hizmeti adı ve bölge gibi ek bilgilerle birlikte, abonelikteki sanal makinelerin listesini döndürür.

1. [Klasik Azure portalında](http://manage.windowsazure.com/) oturum açın
2. Klasik Azure portalında, Kurtarma Hizmetleri kasalarının listesini açmak için **Kurtarma Hizmetleri**'ne tıklayın.
    ![İş yükünü seçme](./media/backup-azure-vms-first-look/recovery-services-icon.png)
3. Kasa listesinden bir VM'nin yedekleneceği kasayı seçin.

    Kasanızı seçtiğinizde kasa **Hızlı Başlangıç** sayfasında açılır
4. Kasa menüsünden **Kayıtlı Öğeler**'e tıklayın.

    ![İş yükünü seçme](./media/backup-azure-vms-first-look/configure-registered-items.png)
5. **Tür** menüsünden **Azure Sanal Makinesi**'ni seçin.

    ![İş yükünü seçme](./media/backup-azure-vms/discovery-select-workload.png)
6. Sayfanın alt kısmındaki **BUL** seçeneğine tıklayın.
    ![Bul düğmesi](./media/backup-azure-vms/discover-button-only.png)

    Bulma işlemi, sanal makinelerin tabloya alınması nedeniyle birkaç dakika sürebilir. Ekranın alt kısmında, işlemin çalıştığını bildiren bir bildirim bulunur.

    ![VM'leri bulma](./media/backup-azure-vms/discovering-vms.png)

    İşlem tamamlandığında bildirim değişir.

    ![Bulma işlemi bitti](./media/backup-azure-vms-first-look/discovery-complete.png)
7. Sayfanın alt kısmındaki **KAYDET** seçeneğine tıklayın.
    ![Kaydet düğmesi](./media/backup-azure-vms-first-look/register-icon.png)
8. **Öğeleri Kaydet** kısayol menüsünde, kaydetmek istediğiniz sanal makineleri seçin.

   > [!TIP]
   > Birden çok sanal makine aynı anda kaydedilebilir.
   >
   >

    Seçtiğiniz her sanal makine için bir iş oluşturulur.
9. **İşler** sayfasına gitmek için bildirim içinde **İşi Görüntüle**'ye tıklayın.

    ![İşi kaydetme](./media/backup-azure-vms/register-create-job.png)

    Sanal makine ayrıca, kayıt işleminin durumu ile birlikte kayıtlı öğeler listesinde de görünür.

    ![Kayıt durumu 1](./media/backup-azure-vms/register-status01.png)

    İşlem tamamlandığında, durum *kayıtlı* durumunu yansıtacak şekilde değişir.

    ![Kayıt durumu 2](./media/backup-azure-vms/register-status02.png)

## <a name="install-the-vm-agent-on-the-virtual-machine"></a>VM Aracısı'nı sanal makineye yükleme
Backup uzantısının çalışması için Azure VM Aracısı'nın Azure sanal makinesine yüklenmesi gerekir. VM'niz Azure galerisinden oluşturulmuşsa VM Aracısı VM üzerinde zaten mevcuttur; [VM’lerinizi koruma](backup-azure-vms-first-look.md#create-the-backup-policy) bölümüne geçebilirsiniz.

VM'nizin bir şirket içi veri merkezinden geçişi sağlandıysa VM için VM Aracısı büyük olasılıkla yüklü değildir. VM'yi koruma aşamasına geçmeden önce sanal makine üzerinde VM Aracısı'nı yüklemeniz gerekir. VM Aracısı'nı yükleme konusunda ayrıntılı adımlar için bkz. [VM'leri Yedekleme makalesinin VM Aracısı bölümü](backup-azure-vms-prepare.md#vm-agent).

## <a name="create-the-backup-policy"></a>Yedekleme ilkesi oluşturma
İlk yedekleme işini tetiklemeden önce, yedekleme anlık görüntülerinin alınma zamanlamasını ayarlayın. Yedekleme anlık görüntülerinin alınma zamanlaması ve bu anlık görüntülerin tutulma süresinin uzunluğu, yedekleme ilkesini oluşturur. Bekletme bilgileri, Üst öğe-orta öğe-alt öğe rotasyon düzenini temel alır.

1. Klasik Azure portalında **Kurtarma Hizmetleri**'nin altındaki yedekleme kasasına gidin ve **Kayıtlı Öğeler**'e tıklayın.
2. Açılan menüden **Azure Sanal Makine**'yi seçin.

    ![Portalda iş yükünü seçme](./media/backup-azure-vms/select-workload.png)
3. Sayfanın alt kısmındaki **KORU** seçeneğine tıklayın.
    ![Koru'ya tıklama](./media/backup-azure-vms-first-look/protect-icon.png)

    **Öğeleri Koruma sihirbazı** görünür ve *yalnızca* kayıtlı ve korumasız sanal makineleri listeler.

    ![Korumayı ölçekli olarak yapılandırma](./media/backup-azure-vms/protect-at-scale.png)
4. Korumak istediğiniz sanal makineleri seçin.

    Aynı ada sahip iki veya daha fazla sanal makine mevcutsa sanal makineleri ayırt etmek için Bulut Hizmeti'ni kullanın.
5. **Korumayı yapılandır** menüsünde, tanımladığınız sanal makineleri korumak için var olan bir ilkeyi seçin veya yeni bir ilke oluşturun.

    Yeni Yedekleme kasaları, kasayla ilişkili bir varsayılan ilke içerir. Bu ilke, her akşam günlük bir anlık görüntü alır ve günlük anlık görüntü 30 gün süreyle tutulur. Her bir yedekleme ilkesi, kendisiyle ilişkili birden çok sanal makineye sahip olabilir. Ancak sanal makine tek seferde yalnızca bir ilkeyle ilişkili olabilir.

    ![Yeni ilke ile koruma](./media/backup-azure-vms/policy-schedule.png)

   > [!NOTE]
   > Yedekleme ilkesi, zamanlanmış yedeklemeler için bir bekletme düzeni içerir. Var olan bir yedekleme ilkesini seçerseniz sonraki adımda bekletme seçeneklerini değiştiremezsiniz.
   >
   >
6. **Bekletme Aralığı** üzerinde belirli yedekleme noktaları için günlük, haftalık, aylık ve yıllık kapsam tanımlayın.

    ![Sanal makine, kurtarma noktası ile yedeklenir](./media/backup-azure-vms/long-term-retention.png)

    Bekletme ilkesi, bir yedeklemenin depolanacağı süreyi belirtir. Yedeklemenin ne zaman oluşturulduğuna bağlı olarak, farklı bekletme ilkeleri belirtebilirsiniz.
7. **Korumayı Yapılandır** işlerinin listesini görüntülemek için **İşler**'e tıklayın.

    ![Korumayı yapılandır işi](./media/backup-azure-vms/protect-configureprotection.png)

    İlkeyi belirlediğinize göre, artık sonraki adıma geçebilir ve ilk yedeklemeyi çalıştırabilirsiniz.

## <a name="initial-backup"></a>İlk yedekleme
Bir sanal makine bir ilke ile koruma altına alındıktan sonra, bu ilişkiyi **Korumalı Öğeler** sekmesinde görüntüleyebilirsiniz. İlk yedekleme gerçekleşene kadar, **Koruma Durumu** **Korumalı - (ilk yedekleme bekleniyor)** olarak gösterilir. Varsayılan olarak, ilk zamanlanmış yedekleme *ilk yedekleme* olacaktır.

![Yedekleme beklemede](./media/backup-azure-vms-first-look/protection-pending-border.png)

İlk yedeklemeyi hemen başlatmak için:

1. **Korumalı Öğeler** sayfasında, sayfanın alt kısmındaki **Şimdi Yedekle** seçeneğine tıklayın.
    ![Şimdi Yedekle simgesi](./media/backup-azure-vms-first-look/backup-now-icon.png)

    Azure Backup hizmeti, ilk yedekleme işlemi için bir yedekleme işi oluşturur.
2. İşlerin listesini görüntülemek için **İşler**'e tıklayın.

    ![Yedekleme devam ediyor](./media/backup-azure-vms-first-look/protect-inprogress.png)

    İlk yedekleme tamamlandığında sanal makinenin **Korumalı Öğeler** sekmesindeki durumu *Korumalı* olacaktır.

    ![Sanal makine, kurtarma noktası ile yedeklenir](./media/backup-azure-vms/protect-backedupvm.png)

   > [!NOTE]
   > Sanal makineleri yedekleme işlemi, yerel bir işlemdir. Bir bölgedeki sanal makineleri, başka bir bölgedeki bir yedek kasaya yedekleyemezsiniz. Bu nedenle, yedeklenmesi gereken VM'lerin bulunduğu her Azure bölgesi için bu bölgede en az bir yedekleme kasası oluşturulmalıdır.
   >
   >

## <a name="next-steps"></a>Sonraki adımlar
Bir VM'yi başarıyla yedeklediğinize göre, sonraki birkaç adım ilginizi çekebilir. En mantıklı adım, bir VM'ye veri geri yükleme konusunda bilgi edinmektir. Bununla birlikte, verilerinizin güvenliğini nasıl koruyacağınızı ve maliyetleri nasıl en aza indireceğinizi anlamanıza yardımcı olacak yönetim görevleri mevcuttur.

* [Sanal makinelerinizi yönetme ve izleme](backup-azure-manage-vms.md)
* [Sanal makineleri geri yükleme](backup-azure-restore-vms.md)
* [Sorun giderme rehberi](backup-azure-vms-troubleshoot.md)

## <a name="questions"></a>Sorularınız mı var?
Sorularınız varsa veya dahil edilmesini istediğiniz herhangi bir özellik varsa [bize geri bildirim gönderin](http://aka.ms/azurebackup_feedback).

