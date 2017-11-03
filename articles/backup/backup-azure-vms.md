---
title: "Bir yedekleme kasası için Azure sanal makineleri Klasik dağıtılan yedekleyin | Microsoft Docs"
description: "Bul kaydetmek ve sanal makinelerinizi Azure sanal makine yedeklemesi için bu yordamları ile yedekleyin."
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
keywords: "sanal makine yedeklemesi; sanal makineyi geri; Yedekleme ve olağanüstü durum kurtarma; VM yedekleme"
ms.assetid: c0ab5469-65fd-4af5-ae9b-f5d183f82ce8
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/02/2017
ms.author: markgal;trinadhk;
ms.openlocfilehash: e1da8bce96078a43c656f84005cefc8bbe81c9e3
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="back-up-azure-virtual-machines-classic-portal"></a>Azure sanal makineleri yedeklemek (Klasik portalı)
> [!div class="op_single_selector"]
> * [Vm'leri kurtarma Hizmetleri kasasına yedekleme](backup-azure-arm-vms.md)
> * [Yedekleme Kasası'na Vm'leri yedekleme](backup-azure-vms.md)
>
>

Bu makale, bir Klasik dağıtılan Azure sanal makinesini (VM) bir yedekleme Kasası'na yedeklemeye için yordamlar sağlar. İlişkin bir Azure sanal makinesini yedeklenmeden önce halletmeniz için gereken bazı görevler vardır. Zaten yapmadıysanız, tamamlamak [Önkoşullar](backup-azure-vms-prepare.md) , VM'lerin yedeklenmesi için ortamınızı hazırlamak için.

Ek bilgi için üzerinde makalelerine bakın [azure'da VM yedekleme altyapınızı planlama](backup-azure-vms-introduction.md) ve [Azure sanal makineleri](https://azure.microsoft.com/documentation/services/virtual-machines/).

> [!NOTE]
> Azure'da kaynak oluşturmaya ve kaynaklarla çalışmaya yönelik iki dağıtım modeli mevcuttur: [Resource Manager ve Klasik](../azure-resource-manager/resource-manager-deployment-model.md). Bir yedekleme kasası yalnızca klasik dağıtılan VM'ler koruyabilirsiniz. Resource Manager tarafından dağıtılan Vm'leri bir yedekleme kasası ile koruyamazsınız. Bkz: [Vm'leri kurtarma Hizmetleri kasasına yedekleme](backup-azure-arm-vms.md) kurtarma Hizmetleri ile çalışma hakkında ayrıntılar için kasaları.
>
>

Azure sanal makineleri yedekleme üç anahtar adımları içerir:

![Bir Azure Iaas VM'yi yedeklemek için üç adım](./media/backup-azure-vms/3-steps-for-backup.png)

> [!NOTE]
> Sanal makineleri yedekleme işlemi, yerel bir işlemdir. Bir bölgedeki sanal makineleri yedeklemek, başka bir bölgedeki bir yedek kasaya yedekleyemezsiniz. Her Azure bölgesinde bir yedekleme kasası oluşturmanız gerekir böylece yedeklenecek VM'ler olduğu.
>
> [!IMPORTANT]
> Mart 2017’den itibaren Backup kasaları oluşturmak için klasik portalı kullanamayacaksınız.
> Artık Backup kasalarınızı Kurtarma Hizmetleri kasalarına yükseltebilirsiniz. Ayrıntılı bilgi için [Backup kasasını Kurtarma Hizmetleri kasasına yükseltme](backup-azure-upgrade-backup-to-recovery-services.md) makalesine bakın. Microsoft, Backup kasalarınızı Kurtarma Hizmetleri kasalarına yükseltmenizi önerir.<br/> 15 Ekim 2017’den itibaren, PowerShell kullanarak Backup kasaları oluşturamayacaksınız. **1 Kasım 2017’ye kadar**:
>- Yükseltilmemiş olan tüm Backup kasaları Kurtarma Hizmetleri kasalarına otomatik olarak yükseltilecektir.
>- Klasik portalda yedekleme verilerinize erişemeyeceksiniz. Bunun yerine, Kurtarma Hizmetleri kasalarındaki yedekleme verilerinize erişmek için Azure portalını kullanabilirsiniz.
>

## <a name="step-1---discover-azure-virtual-machines"></a>1. adım - Azure sanal makineleri Bul
Kaydetmeden önce aboneliğe eklenmiş tüm yeni sanal makineler (VM'ler) tanımlanan emin olmak için bulma işlemini çalıştırın. Bu işlem, bulut hizmeti adı ve bölge gibi ek bilgilerle birlikte abonelikteki sanal makinelerin listesi için Azure'ı sorgular.

1. Oturum [Klasik portal](http://manage.windowsazure.com/)
2. Azure hizmetleri listesinde tıklatın **kurtarma Hizmetleri** yedekleme ve Site kurtarma kasalarının listesini açın.
    ![Açık kasası listesi](./media/backup-azure-vms/choose-vault-list.png)
3. Yedekleme kasaları listesinde, bir VM'nin yedekleneceği kasayı seçin.

    Bu yeni bir kasa ise için portalda açılır **Hızlı Başlangıç** sayfası.

    ![Kayıtlı Öğeler menüsünü açın](./media/backup-azure-vms/vault-quick-start.png)

    Kasa önceden yapılandırılmışsa, portal için en son kullanılan menüsü açılır.
4. Tıklayın (sayfanın en üstündeki) Kasa menüsünden **kayıtlı öğeler**.

    ![Kayıtlı Öğeler menüsünü açın](./media/backup-azure-vms/vault-menu.png)
5. **Tür** menüsünden **Azure Sanal Makinesi**'ni seçin.

    ![İş yükünü seçme](./media/backup-azure-vms/discovery-select-workload.png)
6. Sayfanın alt kısmındaki **BUL** seçeneğine tıklayın.
    ![Bul düğmesi](./media/backup-azure-vms/discover-button-only.png)

    Bulma işlemi, sanal makinelerin tabloya alınması nedeniyle birkaç dakika sürebilir. Ekranın alt kısmında, işlemin çalıştığını bildiren bir bildirim bulunur.

    ![VM'leri bulma](./media/backup-azure-vms/discovering-vms.png)

    İşlem tamamlandığında bildirim değişir. İlk bulma işlemi, sanal makineleri bulamadı, sanal makineleri mevcut emin olun. Sanal makineleri varsa, yedekleme kasası ile aynı bölgede sanal makineleri olduğundan emin olun. Sanal makineleri aynı bölgede bulunduğunu ve sanal makineleri zaten bir yedekleme Kasası'na kayıtlı olmayan olun. Bir VM için bir yedekleme kasası atanmışsa diğer yedekleme kasalarını atanacak kullanılabilir değil.

    ![Bulma işlemi bitti](./media/backup-azure-vms/discovery-complete.png)

    Yeni öğeler bulunan sonra adım 2'ye gidin ve Vm'lerinizi kaydedin.

## <a name="step-2---register-azure-virtual-machines"></a>2. adım - kayıt Azure sanal makineler
Azure Backup hizmeti ile ilişkilendirmek için bir Azure sanal makinesi kaydedin. Bu genellikle tek seferlik bir etkinliktir.

1. Yedekleme kasasına gidin **kurtarma Hizmetleri** Azure portal ve ardından **kayıtlı öğeler**.
2. Açılan menüden **Azure Sanal Makine**'yi seçin.

    ![İş yükünü seçme](./media/backup-azure-vms/discovery-select-workload.png)
3. Sayfanın alt kısmındaki **KAYDET** seçeneğine tıklayın.
    ![Kaydet düğmesi](./media/backup-azure-vms/register-button-only.png)
4. **Öğeleri Kaydet** kısayol menüsünde, kaydetmek istediğiniz sanal makineleri seçin. Aynı ada sahip iki veya daha fazla sanal makine varsa, bunlar arasında ayrım yapmak için bulut hizmetini kullanın.

   > [!TIP]
   > Birden çok sanal makine aynı anda kaydedilebilir.
   >
   >

    Seçtiğiniz her sanal makine için bir iş oluşturulur.
5. **İşler** sayfasına gitmek için bildirim içinde **İşi Görüntüle**'ye tıklayın.

    ![İşi kaydetme](./media/backup-azure-vms/register-create-job.png)

    Sanal makine ayrıca, kayıt işleminin durumu ile birlikte kayıtlı öğeler listesinde de görünür.

    ![Kayıt durumu 1](./media/backup-azure-vms/register-status01.png)

    İşlem tamamlandığında, durum *kayıtlı* durumunu yansıtacak şekilde değişir.

    ![Kayıt durumu 2](./media/backup-azure-vms/register-status02.png)

## <a name="step-3---protect-azure-virtual-machines"></a>3. adım - Azure sanal makinelerini koruma
Şimdi sanal makine için bir yedekleme ve bekletme ilkesi ayarlayabilirsiniz. Tek bir kullanarak birden çok sanal makineler korunabilir eylem koruyun.

Mayıs 2015 ile birlikte gelir sonra oluşturulan azure yedekleme kasaları varsayılan bir ilke kasaya yerleşiktir. Bu varsayılan ilke, varsayılan saklama 30 gün ve bir kez günlük yedekleme zamanlaması ile birlikte gelir.

1. Yedekleme kasasına gidin **kurtarma Hizmetleri** Azure portal ve ardından **kayıtlı öğeler**.
2. Açılan menüden **Azure Sanal Makine**'yi seçin.

    ![Portalda iş yükünü seçme](./media/backup-azure-vms/select-workload.png)
3. Sayfanın alt kısmındaki **KORU** seçeneğine tıklayın.

    **Öğeleri koruma Sihirbazı'nı** görüntülenir. Sihirbaz yalnızca kayıtlı ve korumalı sanal makineleri listeler. Korumak istediğiniz sanal makineleri seçin.

    Aynı ada sahip iki veya daha fazla sanal makine varsa, sanal makineleri ayırt etmek için bulut hizmetini kullanın.

   > [!TIP]
   > Bir seferde birden fazla sanal makineyi koruyabilir.
   >
   >

    ![Korumayı ölçekli olarak yapılandırma](./media/backup-azure-vms/protect-at-scale.png)

4. Seçin bir **yedekleme zamanlaması** seçmiş olduğunuz sanal makineleri yedeklemek için. Var olan ilkeleri kümesinden seçin veya yeni bir tane tanımlayın.

    Her bir yedekleme ilkesi, kendisiyle ilişkili birden çok sanal makineye sahip olabilir. Ancak, sanal makine yalnızca zamanında verilen herhangi bir noktada bir ilkesiyle ilişkili olabilir.

    ![Yeni ilke ile koruma](./media/backup-azure-vms/policy-schedule.png)

   > [!NOTE]
   > Yedekleme ilkesi, zamanlanmış yedeklemeler için bir bekletme düzeni içerir. Mevcut bir yedekleme ilkesini seçerseniz sonraki adımda bekletme seçeneklerini değiştiremezsiniz.
   >
   >

5. Seçin bir **bekletme aralığı** yedeklemelerini ile ilişkilendirilecek.

    ![Esnek bekletme ile koruma](./media/backup-azure-vms/policy-retention.png)

    Bekletme ilkesi, bir yedeklemenin depolanacağı süreyi belirtir. Yedeklemenin ne zaman oluşturulduğuna bağlı olarak, farklı bekletme ilkeleri belirtebilirsiniz. Örneğin, (işletimsel kurtarma noktası olarak hizmet veren) günlük mü bir yedekleme noktası 90 gün boyunca korunması. Buna karşılık, (Denetim amacıyla) her üç aylık dönemin sonunda yapılan bir yedekleme noktasının birçok ay veya yıl korunması gerekebilir.

    ![Sanal makine, kurtarma noktası ile yedeklenir](./media/backup-azure-vms/long-term-retention.png)

    Bu örnek görüntüde:

   * **Günlük Bekletme İlkesi**: günlük alınan yedeklemeler 30 gün süreyle depolanır.
   * **Haftalık Bekletme İlkesi**: her hafta Pazar günü alınan yedeklemeler 104 hafta boyunca korunur.
   * **Aylık Bekletme İlkesi**: her ay son Pazar günü alınan yedeklemeler için 120 ayda korunur.
   * **Yıllık Bekletme İlkesi**: her Ocak ilk Pazar günü alınan yedeklemeler için 99 yıla korunur.

     Koruma ilkesi yapılandırma ve bu ilkeyi, seçtiğiniz her bir sanal makine için sanal makinelere ilişkilendirmek için bir iş oluşturulur.
6. Listesini görüntülemek için **korumayı Yapılandır** kasa menüsünden işleri tıklatın **işleri** seçip **korumayı Yapılandır** gelen **işlemi** Filtresi.

    ![Korumayı yapılandır işi](./media/backup-azure-vms/protect-configureprotection.png)

## <a name="initial-backup"></a>İlk yedekleme
Sanal makine bir ilke ile korumalı sonra altında görüntülersiniz **korunan öğeler** durumunu sekmesiyle *korumalı - (ilk yedekleme bekleniyor)*. Varsayılan olarak, ilk zamanlanmış yedekleme *ilk yedekleme* olacaktır.

İlk yedeklemeyi hemen koruma yapılandırdıktan sonra tetiklemek için:

1. Ekranın alt kısmındaki **korunan öğeler** sayfasında, **Şimdi Yedekle**.

    Azure Backup hizmeti, ilk yedekleme işlemi için bir yedekleme işi oluşturur.
2. İşlerin listesini görüntülemek için **İşler**'e tıklayın.

    ![Yedekleme devam ediyor](./media/backup-azure-vms/protect-inprogress.png)

> [!NOTE]
> Yedekleme işlemi sırasında Azure Backup hizmeti yedekleme uzantısına tüm yazma işleri boşaltmaya ve tutarlı bir anlık görüntü almak için her sanal makine için bir komut verir.
>
>

İlk yedekleme tamamlandığında, sanal makinenin durumunu **korunan öğeler** sekmesi *korumalı*.

![Sanal makine, kurtarma noktası ile yedeklenir](./media/backup-azure-vms/protect-backedupvm.png)

## <a name="viewing-backup-status-and-details"></a>Yedekleme durumu ve ayrıntıları görüntüleme
Korumalı sonra sanal makine sayısı da içinde artırır **Pano** Özet sayfası. **Pano** sayfa son 24 olan saat işlerden sayısını da gösterir *başarılı*, sahip *başarısız*ve *devam eden*. Üzerinde **işleri** sayfasında, kullanmak **durum**, **işlemi**, veya **gelen** ve **için** menüleri işleri filtreleyin.

![Pano sayfasındaki yedekleme durumu](./media/backup-azure-vms/dashboard-protectedvms.png)

Pano değerleri her 24 saatte bir yenilenir.

## <a name="troubleshooting-errors"></a>Sorun giderme
Sanal makineniz yedekleme sırasında sorunları içine çalıştırırsanız bakmak [VM sorun giderme makalesi](backup-azure-vms-troubleshoot.md) Yardım.

## <a name="next-steps"></a>Sonraki adımlar
* [Sanal makinelerinizi yönetme ve izleme](backup-azure-manage-vms.md)
* [Sanal makineleri geri yükleme](backup-azure-restore-vms.md)
