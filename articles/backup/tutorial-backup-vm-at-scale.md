---
title: "Ölçekli Azure sanal makineleri yedekleyin | Microsoft Docs"
description: "Aynı anda birden çok sanal makineleri Azure'a yedekleme"
services: backup
keywords: "sanal makine yedeklemesi; sanal makine yedeklemek; VM yedeklemesi geri; Yedekleme vm; Azure vm backup; Yedekleme ve olağanüstü durum kurtarma"
author: markgalioto
ms.author: markgal
ms.date: 09/16/2017
ms.topic: tutorial
ms.service: backup
ms.custom: mvc
ms.openlocfilehash: 74ccf95b559b690eb53c2f4df14513dab5a94677
ms.sourcegitcommit: 7136d06474dd20bb8ef6a821c8d7e31edf3a2820
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2017
---
# <a name="use-azure-portal-to-back-up-multiple-virtual-machines"></a>Birden çok sanal makineleri yedeklemek için Azure portalını kullanma

Azure veri yedeklediğinizde, Kurtarma Hizmetleri kasası adlı bir Azure kaynağı bu verileri depolar. Kurtarma Hizmetleri kasası kaynak çoğu Azure Hizmetleri ayarları menüsünden kullanılabilir. Çoğu Azure Hizmetleri ayarları menüsüne tümleştirilmiş kurtarma Hizmetleri kasası sahip yararı verileri yedeklemek çok kolay hale getirir. Ancak, tek tek her veritabanı ya da sanal makineyi işinizde çalışma yorucu olabilir. Bir departmandaki ya da bir konumdaki tüm sanal makineler için verileri yedeklemek isterseniz ne? Bir yedekleme ilkesi oluşturma ve bu ilkeyi istenen sanal makineler için uygulama tarafından birden çok sanal makineleri yedekleme kolaydır. Bu öğretici açıklar nasıl yapılır:

> [!div class="checklist"]
> * Kurtarma Hizmetleri kasası oluşturma
> * Bir yedekleme ilkesi tanımlama
> * Birden çok sanal makine korumak için yedekleme ilkesini uygula
> * Korumalı sanal makineler için bir talep üzerine yedekleme işi tetikleyeceğinden

## <a name="log-in-to-the-azure-portal"></a>Azure portalında oturum açma

[Azure Portal](https://portal.azure.com/)’da oturum açın.

## <a name="create-a-recovery-services-vault"></a>Kurtarma Hizmetleri kasası oluşturma

Kurtarma Hizmetleri kasasına yedekleme verilerini ve korumalı sanal makinelere uygulanan yedekleme ilkesini içerir. Sanal makineleri yedekleme işlemi, yerel bir işlemdir. Bir sanal makineyi bir konumdan başka bir konuma kurtarma Hizmetleri kasasına yedekleyemezsiniz. Bu nedenle, yedeklenmesi için sanal makine içeren her Azure konumu için en az bir kurtarma Hizmetleri kasası ilgili konumda mevcut olmalıdır.

1. Sol taraftaki menüden seçin **daha fazla hizmet** ve Hizmetler listesinde yazın *kurtarma Hizmetleri*. Siz yazarken kaynakların listesini filtrelenir. Kurtarma Hizmetleri kasaları listesinde gördüğünüzde, Kurtarma Hizmetleri kasaları menüsünü açmak için seçin.

    ![Kurtarma Hizmetleri kasası menü Aç](./media/tutorial-backup-vm-at-scale/full-browser-open-rs-vault.png) <br/>

2. İçinde **kurtarma Hizmetleri kasaları** menüsünde tıklatın **Ekle** kurtarma Hizmetleri kasası menüsünü açın.

    ![Açık kasası menüsü](./media/tutorial-backup-vm-at-scale/provide-vault-detail-2.png)

3. Kurtarma Hizmetleri kasası menüsünde 

    - Tür *myRecoveryServicesVault* içinde **adı**,
    - Geçerli abonelik kimliği görünür **abonelik**. Ek Abonelikleriniz varsa, yeni kasa için başka bir aboneliği seçebilirsiniz.
    - İçin **kaynak grubu** seçin **var olanı kullan** ve *myResourceGroup*. Varsa *myResourceGroup* mevcut değil, seçin **Yeni Oluştur** ve türü *myResourceGroup*.
    - Gelen **konumu** açılır menü seçin *Batı Avrupa*.
    - Tıklatın **oluşturma** , Kurtarma Hizmetleri kasası oluşturmak için.

Kurtarma Hizmetleri kasası, korunan sanal makineler ile aynı konumda olmalıdır. Birden çok bölgede sanal makineniz varsa her bölgede bir kurtarma Hizmetleri kasası oluşturun. Bu öğretici bir kurtarma Hizmetleri kasasına oluşturur *Batı Avrupa* , nerede olduğundan *myVM* (Hızlı Başlangıç ile oluşturulan sanal makinenin) oluşturuldu.

Kurtarma Hizmetleri kasasının oluşturulması birkaç dakika sürebilir. Portalın sağ üst kısmından durum bildirimlerini izleyin. Kasanız oluşturulduktan sonra Kurtarma Hizmetleri kasaları listesinde görünür.

Kurtarma Hizmetleri kasası oluşturduğunuzda, varsayılan olarak coğrafi olarak yedekli depolama kasası sahiptir. Veri dayanıklılık sağlamak için coğrafi olarak yedekli depolama verileri iki Azure bölgeler arasında birden çok kez çoğaltır.

## <a name="set-backup-policy-to-protect-vms"></a>Vm'leri korumak için yedekleme ilkenizi ayarlayın

Kurtarma Hizmetleri kasası oluşturduktan sonra sonraki adım, veri türü için kasa yapılandırmak ve yedekleme ilkesini ayarlamak için içerir. Yedekleme İlkesi ne sıklıkta ve ne zaman kurtarma noktaları alınır zamanlamadır. İlke aynı zamanda kurtarma noktaları için bekletme aralığını içerir. Bu öğretici için varsayalım işletmenizin Spor bir otel, stadyum ve Restoran ve teslimleri karmaşık olan ve sanal makinelerde veri koruma. Aşağıdaki adımlar, finansal verileri için bir yedekleme ilkesi oluşturun.

1. Kurtarma Hizmetleri kasalarının listesinden seçin **myRecoveryServicesVault** kendi panosunu açın.

   ![Senaryo menüsünü açın](./media/tutorial-backup-vm-at-scale/open-vault-from-list.png)

2. Kasa Panosu menüsünden tıklatın **yedekleme** yedekleme menüsünü açın.

3. Yedekleme hedefi menüsünde de **, iş yükünü çalıştırdığı** açılır menü seçin *Azure*. Gelen **neleri yedeklemek istiyorsunuz** açılan listesinde, seçin *sanal makine*, tıklatıp **yedekleme**.

    Bu Eylemler, bir sanal makine ile etkileşim için kurtarma Hizmetleri kasası hazırlayın. Kurtarma Hizmetleri kasaları her gün bir geri yükleme noktası oluşturan ve geri yükleme noktaları için 30 gün saklar bir varsayılan ilke içerir.

    ![Senaryo menüsünü açın](./media/tutorial-backup-vm-at-scale/backup-goal.png)

4. Yedekleme İlkesi menüsünde yeni bir ilke oluşturmak için **yedekleme ilkesi seçin** açılır menüsünde, select *Yeni Oluştur*.

    ![İş yükünü seçme](./media/tutorial-backup-vm-at-scale/create-new-policy.png)

5. İçinde **yedekleme İlkesi** menüsünde için **ilke adı** türü *Finans*. Aşağıdaki değişiklikler için yedekleme İlkesi girin: 
    - İçin **yedekleme sıklığı** için saat dilimi ayarlamak *merkezi saat*. Spor karmaşık Texas içinde olduğundan, yerel olarak zamanlama sahibi istemektedir. Her gün 03: 30'da kümesine yedekleme sıklığı bırakın.
    - İçin **günlük yedekleme noktası bekletme**, süresi 90 gün olarak ayarlayın.
    - İçin **haftalık yedekleme noktası bekletme**, kullanın *Pazartesi* geri yükleme noktası ve 52 hafta boyunca Beklet.
    - İçin **aylık yedekleme noktası bekletme**, ayın ilk Pazar geri yükleme noktasından kullanın ve 36 ay için korur.
    - Seçimini **yıllık yedekleme noktası bekletme** seçeneği. Finans öncü 36 aydan daha uzun verileri tutmak istememektedir.
    - Yedekleme ilkesini oluşturmak için **Tamam**’a tıklayın.

    ![İş yükünü seçme](./media/tutorial-backup-vm-at-scale/set-new-policy.png) 

    Yedekleme ilkenizi oluşturduktan sonra ilkeyi sanal makineler ile ilişkilendirin.

6. İçinde **sanal makine Seç** iletişim kutusunda *myVM* tıklatıp **Tamam** sanal makineler için yedekleme ilkesini dağıtmak için. 

    Aynı konumda olan ve bir yedekleme İlkesi ile ilişkili olmayan tüm sanal makineler görünür. *myVMH1* ve *myVMR1* ilişkilendirilecek seçili *Finans* ilkesi.

    ![İş yükünü seçme](./media/tutorial-backup-vm-at-scale/choose-vm-to-protect.png) 

    Dağıtım tamamlandığında, bu dağıtımı başarıyla tamamlandı bir bildirim alırsınız.

## <a name="initial-backup"></a>İlk yedekleme

Yedekleme için kurtarma Hizmetleri kasalarının etkinleştirdiyseniz, ancak ilk yedekleme oluşturulmadı. İlk yedeklemeyi tetiklemek için olağanüstü durum kurtarma en iyi uygulama, böylelikle verileriniz korunur. 

Bir talep üzerine yedekleme işi çalıştırmak için:

1. Kasa Panosu üzerinde tıklatın **3** altında **yedekleme öğeleri**, yedekleme öğeleri menüsünü açın.

    ![Ayarlar simgesi](./media/tutorial-backup-vm-at-scale/tutorial-vm-back-up-now.png)

    **Yedekleme öğeleri** menüsü açılır.

2. Üzerinde **yedekleme öğeleri** menüsünde tıklatın **Azure sanal makine** kasayla ilişkili sanal makinelerin listesini açın.

    ![Ayarlar simgesi](./media/tutorial-backup-vm-at-scale/three-virtual-machines.png)

    **Yedekleme Öğeleri** listesi açılır.

    ![Tetiklenmiş yedekleme işi](./media/tutorial-backup-vm-at-scale/initial-backup-context-menu.png)

3. **Yedekleme Öğeleri** listesinde üç noktaya **...** tıklayarak Bağlam menüsünü açın.

4. Bağlam menüsünde seçin **Şimdi Yedekle**.

    ![Bağlam menüsü](./media/tutorial-backup-vm-at-scale/context-menu.png)

    Şimdi Yedekle menüsü açılır.

5. Şimdi Yedekle menüsünde tıklatın ve kurtarma noktası korumak son gününü girin **yedekleme**.

    ![Şimdi Yedekle kurtarma noktasının korunduğu son günü ayarlayın](./media/tutorial-backup-vm-at-scale/backup-now-short.png)

    Dağıtım bildirimleri, yedekleme işinin tetiklendiğini ve Yedekleme işleri sayfasında işin ilerleme durumunu izleyebileceğinizi bilmenizi sağlar. Sanal makine boyutuna bağlı olarak, ilk yedekleme oluşturma biraz zaman alabilir.

    İlk yedekleme işi tamamlandığında yedekleme işi menüde durumunu görebilirsiniz. Talep üzerine yedekleme işi için ilk geri yükleme noktası oluşturulan *myVM*. Diğer sanal makineleri yedeklemek istiyorsanız, her sanal makine için bu adımları yineleyin. 

    ![Yedekleme İşleri kutucuğu](./media/tutorial-backup-vm-at-scale/initial-backup-complete.png)
  
## <a name="clean-up-resources"></a>Kaynakları temizleme

Sonraki öğreticilerde ile çalışmaya devam etmek planlıyorsanız, temiz Bu öğreticide oluşturduğunuz kaynakları yukarı değil. Devam etmek düşünmüyorsanız Bu öğreticide Azure portal tarafından oluşturulan tüm kaynakları silmek için aşağıdaki adımları kullanın.

1. Üzerinde **myRecoveryServicesVault** panoyu tıklatın **3** altında **yedekleme öğeleri**, yedekleme öğeleri menüsünü açın.

    ![Ayarlar simgesi](./media/tutorial-backup-vm-at-scale/tutorial-vm-back-up-now.png)


2. Üzerinde **yedekleme öğeleri** menüsünde tıklatın **Azure sanal makine** kasayla ilişkili sanal makinelerin listesini açın.

    ![Ayarlar simgesi](./media/tutorial-backup-vm-at-scale/three-virtual-machines.png)

    **Yedekleme Öğeleri** listesi açılır.

3. İçinde **yedekleme öğeleri** menüsünde bağlam menüsünü açmak için üç nokta işaretine tıklayın.

    ![Ayarlar simgesi](./media/tutorial-backup-vm-at-scale/context-menu-to-delete-vm.png)

4. Bağlam menüsünde seçin **Dur yedekleme** Durdur yedekleme menüsünü açın. 

    ![Ayarlar simgesi](./media/tutorial-backup-vm-at-scale/context-menu-for-delete.png)

5. İçinde **durdurmak yedekleme** menüsünde üst açılır menü seçip **yedekleme verilerini Sil**.

6. İçinde **yedekleme öğesinin adını yazın** iletişim, türü *myVM*.
 
7. Yedekleme öğesi doğrulandıktan sonra (bir onay işareti görünür), **Dur yedekleme** düğmesi etkindir. Tıklatın **durdurmak yedekleme** İlkesi durdurmak ve geri yükleme noktaları silmek için. 

    ![Stop yedekleme kasası silmek için tıklayın](./media/tutorial-backup-vm-at-scale/provide-reason-for-delete.png).

8. İçinde **myRecoveryServicesVault** menüsünde tıklatın **silmek**.

    ![Stop yedekleme kasası silmek için tıklayın](./media/tutorial-backup-vm-at-scale/deleting-the-vault.png)

    Kasa silindikten sonra kurtarma Hizmetleri kasalarının listesi döndürür.


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide Azure portalına kullanılır:

> [!div class="checklist"]
> * Kurtarma Hizmetleri kasası oluşturma
> * Sanal makineleri korumak için kasa ayarlayın
> * Özel yedekleme ve bekletme ilkesi oluşturma
> * Birden çok sanal makine korumak için ilke atama
> * İsteğe bağlı geri sanal makineler için Tetikle

Bir Azure sanal makinesi diskten geri yüklemek için sonraki öğretici devam edin. 

> [!div class="nextstepaction"]
> [CLI kullanarak sanal makineleri geri yükleme](./tutorial-restore-disk.md)
