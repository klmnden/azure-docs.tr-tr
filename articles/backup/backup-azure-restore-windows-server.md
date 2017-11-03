---
title: "Azure veri geri bir Windows Server veya Windows bilgisayarı | Microsoft Docs"
description: "Windows Server veya Windows bilgisayarı Azure'a depolanan veri geri yüklemeyi öğrenin."
services: backup
documentationcenter: 
author: saurabhsensharma
manager: shivamg
editor: 
ms.assetid: 742f4b9e-c0ab-4eeb-8e22-ee29b83c22c4
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 8/16/2017
ms.author: saurse;trinadhk;markgal;
ms.openlocfilehash: 231dd61f95267b3a504ed70e9b3a5abc470b69b2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="restore-files-to-a-windows-server-or-windows-client-machine-using-resource-manager-deployment-model"></a>Resource Manager dağıtım modelini kullanarak bir Windows sunucusu veya Windows istemci makinesine dosyaları geri yükleme
> [!div class="op_single_selector"]
> * [Azure portal](backup-azure-restore-windows-server.md)
> * [Klasik portal](backup-azure-restore-windows-server-classic.md)
>
>

Bu makalede, verileri bir yedekleme Kasası'nı geri yüklemek açıklanmaktadır. Verileri geri yüklemek için Microsoft Azure kurtarma Hizmetleri (MARS) aracısı Veri Kurtarma Sihirbazı'nı kullanın. Verileri geri yüklediğinizde, mümkün olur:

* Veri yedekleri gerçekleştirilecek aynı makineye geri yükleyebilirsiniz.
* Verileri başka bir makineye geri yükleyin.

Ocak 2017 ' Microsoft MARS aracısı için Önizleme güncelleştirme yayımladı. Hata düzeltmeleri yanı sıra, bu güncelleştirme, anlık yazılabilir kurtarma noktası anlık görüntü kurtarma birimi olarak bağlamak izin veren geri yükleme sağlar. Ardından böylece seçerek dosyaları geri yükleme yerel bir bilgisayara kurtarma birimi ve kopyalama dosyalarını gözden geçirebilirsiniz.

> [!NOTE]
> [Ocak 2017 Azure Backup güncelleştirmesini](https://support.microsoft.com/en-us/help/3216528?preview) anlık geri verileri geri yüklemek için kullanmak istiyorsanız gereklidir. Ayrıca Yedekleme verileri yerel destek makalesinde listelenen kasalarında korunmalıdır. Başvurun [Ocak 2017 Azure Backup güncelleştirmesini](https://support.microsoft.com/en-us/help/3216528?preview) anlık geri yükleme desteği yerel ayarları en son listesi için. Anlık geri yükleme **değil** tüm bölgelerde kullanılabilir.
>

Anlık geri yükleme Azure portal ve klasik portalda yedekleme kasaları kurtarma Hizmetleri kasalarının kullanmak için kullanılabilir. Anlık geri kullanmak istiyorsanız, MARS güncelleştirmeyi indirin ve anlık geri Bahsediyor yordamları izleyin.

[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]

## <a name="use-instant-restore-to-recover-data-to-the-same-machine"></a>Anlık aynı makineye verileri kurtarmak için geri yükleme kullanın

Yanlışlıkla silinen bir dosya ve (yedeğin alındığı) aynı makineye geri yüklemek istiyorsanız, aşağıdaki adımları verileri kurtarmanıza yardımcı olur.

1. Açık **Microsoft Azure yedekleme** içinde Vurgu. Ek bileşeninde yüklendiği bilmiyorsanız, bilgisayar veya sunucu için arama **Microsoft Azure yedekleme**.

    Masaüstü uygulaması arama sonuçlarında görüntülenmesi gerekir.

2. Tıklatın **verileri kurtarabilirsiniz** Sihirbazı'nı başlatın.

    ![Verileri kurtarma](./media/backup-azure-restore-windows-server/recover.png)

3. Üzerinde **Başlarken** verileri aynı sunucu veya bilgisayara geri yüklemek için bölmeyi seçin **bu sunucunun (`<server name>`)** tıklatıp **sonraki**.

    ![Bu sunucu seçeneği aynı makineye verileri geri yüklemek için](./media/backup-azure-restore-windows-server/samemachine_gettingstarted_instantrestore.png)

4. Üzerinde **seçin kurtarma moduna** bölmesinde seçin **dosyalara ve klasörlere** ve ardından **sonraki**.

    ![Gözatma dosyaları](./media/backup-azure-restore-windows-server/samemachine_selectrecoverymode_instantrestore.png)

5. Üzerinde **birim seçin ve tarih** bölmesinde, dosyaları ve/veya geri yüklemek istediğiniz klasörleri içeren bir birimi seçin.

    Takvimde bir kurtarma noktası seçin. Zaman içinde herhangi bir kurtarma noktasından geri yükleyebilirsiniz. İçinde tarihleri **kalın** en az bir kurtarma noktası kullanılabilirliğini gösterir. Birden fazla kurtarma noktası mevcutsa, bir tarih seçtikten sonra belirli bir kurtarma noktasından seçin **zaman** açılır menü.

    ![Birimi ve tarih](./media/backup-azure-restore-windows-server/samemachine_selectvolumedate_instantrestore.png)

6. Geri yüklemek için kurtarma noktası seçtikten sonra tıklatın **bağlama**.

    Azure yedekleme, yerel kurtarma noktası bağlar ve kurtarma birimi olarak kullanır.

7. Üzerinde **Gözat ve kurtarma dosyaları** bölmesinde tıklatın **Gözat** Windows Gezgini'ni açın ve istediğiniz klasörleri ve dosyaları bulmak için.

    ![Kurtarma Seçenekleri](./media/backup-azure-restore-windows-server/samemachine_browserecover_instantrestore.png)


8. Windows Gezgini'nde, dosyaları ve/veya geri yükleme ve sunucu veya bilgisayar için yerel herhangi bir konuma yapıştırmak için istediğiniz klasörleri kopyalayın. Açın veya kurtarma birimi dosyalarından doğrudan akış ve doğru sürümlerini kurtarılan doğrulayın.

    ![Dosyaları ve takılı birim klasörlerinden yerel bir konuma kopyalama ve yapıştırma](./media/backup-azure-restore-windows-server/samemachine_copy_instantrestore.png)

9. Dosyaları ve/veya klasörleri üzerinde geri yüklemeyi tamamladıktan sonra **göz atın ve kurtarma dosyaları** bölmesinde tıklatın **çıkarma**. Ardından **Evet** birim kaldırmak istediğinizi onaylamak için.

    ![Birim çıkarın ve onaylayın](./media/backup-azure-restore-windows-server/samemachine_unmount_instantrestore.png)

    > [!Important]
    > Çıkarma değil tıklatırsanız, Kurtarma birimi zaman takıldı zamandan 6 saat boyunca bağlı kalır. Ancak, bağlama genişletilmiş değerine kadar devam eden bir dosya kopyalama durumunda 24 saat maksimum saattir. Birim bağlıyken hiçbir yedekleme işlemleri çalıştırın. Birimin zaman bağlı, süre içinde çalıştırılmak üzere zamanlanmış herhangi bir yedekleme işlemi kurtarma birimi kaldırılan sonra çalışır.
    >


## <a name="use-instant-restore-to-restore-data-to-an-alternate-machine"></a>Verileri başka bir makineye geri yüklemek için anlık geri kullanın
Tüm sunucunuzu kaybolursa, verileri Azure yedekten başka bir makine kurtarmaya devam edebilirsiniz. Aşağıdaki adımlar, iş akışı gösterilmektedir.


Bu adımlarda kullanılan terminolojiyi içerir:

* *Kaynak makine* – özgün makineden yedeğin alındığı ve hangi şu anda kullanılamıyor.
* *Hedef makine* – olduğu verilerin kurtarıldığı makine.
* *Örnek kasa* – kurtarma Hizmetleri kasası için *kaynak makine* ve *hedef makine* kaydedilir. <br/>

> [!NOTE]
> Yedeklemeleri işletim sisteminin önceki bir sürümünü çalıştıran bir hedef bilgisayara geri yüklenemez. Örneğin, bir Windows bilgisayarı olabilir 7'den alınan bir yedeklemeyi bilgisayar bir Windows 8 veya daha sonra geri. Windows 7 bilgisayara Windows 8 bilgisayardan alınan bir yedeklemeyi geri yüklenemiyor.
>
>

1. Açık **Microsoft Azure yedekleme** üzerinde içinde Vurgu *hedef makine*.

2. Olun *hedef makine* ve *kaynak makine* aynı kurtarma Hizmetleri kasasına kayıtlı.

3. Tıklatın **verileri kurtarabilirsiniz** açmak için **Veri Kurtarma Sihirbazı'nı**.

    ![Verileri kurtarma](./media/backup-azure-restore-windows-server/recover.png)

4. Üzerinde **Başlarken** bölmesinde, **başka bir sunucu**

    ![Başka bir sunucu](./media/backup-azure-restore-windows-server/alternatemachine_gettingstarted_instantrestore.png)

5. Karşılık gelen kasa kimlik bilgilerini sağlayın *örnek kasa*, tıklatıp **sonraki**.

    Kasa kimlik bilgilerini geçersiz (veya süresi dolmuş) varsa, yeni bir kasa kimlik bilgileri dosyasını indirin *örnek kasa* Azure portalında. Geçerli kasa kimlik bilgileri sağladığınızda, ilgili yedekleme kasasının adını görüntülenir.


6. Üzerinde **yedekleme sunucusu seçin** bölmesinde, select *kaynak makine* görüntülenmesini makineler listesinden ve parola girin. Ardından **İleri**'ye tıklayın.

    ![Makineler listesi](./media/backup-azure-restore-windows-server/alternatemachine_selectmachine_instantrestore.png)

7. Üzerinde **seçin kurtarma moduna** bölmesinde seçin **dosyalara ve klasörlere** tıklatıp **sonraki**.

    ![Arama](./media/backup-azure-restore-windows-server/alternatemachine_selectrecoverymode_instantrestore.png)

8. Üzerinde **birim seçin ve tarih** bölmesinde, dosyaları ve/veya geri yüklemek istediğiniz klasörleri içeren bir birimi seçin.

    Takvimde bir kurtarma noktası seçin. Zaman içinde herhangi bir kurtarma noktasından geri yükleyebilirsiniz. İçinde tarihleri **kalın** en az bir kurtarma noktası kullanılabilirliğini gösterir. Birden fazla kurtarma noktası mevcutsa, bir tarih seçtikten sonra belirli bir kurtarma noktasından seçin **zaman** açılır menü.

    ![Arama öğeleri](./media/backup-azure-restore-windows-server/alternatemachine_selectvolumedate_instantrestore.png)

9. Tıklatın **bağlama** kurtarma birimi olarak kurtarma noktası üzerinde yerel olarak bağlamak için *hedef makine*.

10. Üzerinde **Gözat ve kurtarma dosyaları** bölmesinde tıklatın **Gözat** Windows Gezgini'ni açın ve istediğiniz klasörleri ve dosyaları bulmak için.

    ![Şifreleme](./media/backup-azure-restore-windows-server/alternatemachine_browserecover_instantrestore.png)

11. Windows Gezgini'nde, dosyaları ve/veya klasörleri kurtarma birimden kopyalayıp onlara, *hedef makine* konumu. Açın veya kurtarma birimi dosyalarından doğrudan akış ve doğru sürümlerini kurtarılan doğrulayın.

    ![Şifreleme](./media/backup-azure-restore-windows-server/alternatemachine_copy_instantrestore.png)

12. Dosyaları ve/veya klasörleri üzerinde geri yüklemeyi tamamladıktan sonra **göz atın ve kurtarma dosyaları** bölmesinde tıklatın **çıkarma**. Ardından **Evet** birim kaldırmak istediğinizi onaylamak için.

    ![Şifreleme](./media/backup-azure-restore-windows-server/alternatemachine_unmount_instantrestore.png)

    > [!Important]
    > Çıkarma değil tıklatırsanız, Kurtarma birimi zaman takıldı zamandan 6 saat boyunca bağlı kalır. Ancak, bağlama genişletilmiş değerine kadar devam eden bir dosya kopyalama durumunda 24 saat maksimum saattir. Birim bağlıyken hiçbir yedekleme işlemleri çalıştırın. Birimin zaman bağlı, süre içinde çalıştırılmak üzere zamanlanmış herhangi bir yedekleme işlemi kurtarma birimi kaldırılan sonra çalışır.
    >

## <a name="troubleshooting"></a>Sorun giderme
Azure yedekleme kurtarma birimi başarıyla tıklama bile birkaç dakika sonra bağlamaz varsa **bağlama** veya kurtarma birim bir veya daha fazla hata ile başarısız normalde kurtarma başlamak için aşağıdaki adımları izleyin.

1.  Birkaç dakika çalışıyor durumda devam eden bağlama işlemi iptal edin.

2.  Azure yedekleme Aracısı'nın en son sürümde olduğundan emin olun. Azure yedekleme Aracısı'nın sürüm bilgileri bulmak için tıklayın **hakkında Microsoft Azure kurtarma Hizmetleri Aracısı** üzerinde **Eylemler** Microsoft Azure yedekleme bölmesinde konsol ve emin**Sürüm** sayıdır eşit veya belirtilen sürümden daha yüksek [bu makalede](https://go.microsoft.com/fwlink/?linkid=229525). En son sürümü karşıdan [burada](https://go.microsoft.com/fwLink/?LinkID=288905)

3.  Git **Aygıt Yöneticisi'ni** -> **depolama alanı denetleyicileri** ve, bulabilmesini sağlama **Microsoft iSCSI başlatıcısı**. Bulabiliyorsa, doğrudan adım 7'ye gidin. 

4.  Adım 3'te belirtildiği gibi Microsoft iSCSI başlatıcısı hizmetinin bulamazsanız, altında bir giriş bulursanız denetleyin **Aygıt Yöneticisi'ni** -> **depolama alanı denetleyicileri** adlı  **Bilinmeyen aygıt** donanım kimliği ile **ROOT\ISCSIPRT**.

5.  Sağ tıklayın **bilinmeyen aygıt** seçip **sürücü yazılımı güncelleştirme**.

6.  Seçeneğini seçerek sürücüsünü güncelleştirmek **otomatik olarak güncelleştirilen sürücü yazılım Ara**. Güncelleştirmenin tamamlanması değiştirme **bilinmeyen aygıt** için **Microsoft iSCSI başlatıcısı** aşağıda gösterildiği gibi. 

    ![Şifreleme](./media/backup-azure-restore-windows-server/UnknowniSCSIDevice.png)

7.  Git **Görev Yöneticisi'ni** -> **hizmetler (yerel)** -> **Microsoft iSCSI başlatıcısı hizmetini**. 

    ![Şifreleme](./media/backup-azure-restore-windows-server/MicrosoftInitiatorServiceRunning.png)
    
8.  Hizmette tıklayarak sağ tıklayarak Microsoft iSCSI başlatıcısı hizmetini yeniden **durdurmak** ve sağ yeniden tıklatıp tıklayarak daha fazla **Başlat**.

9.  Anlık geri yükleme özelliğini kullanarak kurtarma yeniden deneyin. 

Kurtarma yine başarısız olursa, sunucu/istemci yeniden başlatın. Yeniden başlatma arzu değil veya bile sunucunun yeniden başlatmanın ardından Kurtarma yine başarısız olursa, alternatif bir makineden kurtarmayı deneyin ve giderek Azure desteği ile iletişim kurun [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) ve bir destek isteği gönderiliyor.

## <a name="next-steps"></a>Sonraki adımlar
* Dosya ve klasörleri kurtarma yaptıktan, yapabilecekleriniz [Yedeklemelerinizin yönetmek](backup-azure-manage-windows-server.md).
