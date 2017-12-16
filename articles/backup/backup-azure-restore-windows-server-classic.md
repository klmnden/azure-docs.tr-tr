---
title: "Klasik dağıtım modeli kullanılarak Azure'dan veri geri bir Windows Server veya Windows İstemcisi | Microsoft Docs"
description: "Bir Windows Server veya Windows İstemcisi geri yüklemeyi öğrenin."
services: backup
documentationcenter: 
author: saurabhsensharma
manager: shivamg
editor: 
ms.assetid: 85585dfc-c764-4e8c-8f0e-40b969640ac2
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/10/2017
ms.author: cwatson
ms.openlocfilehash: 7ad69e1fd17da34f4ad7247aee2303bcf76965fc
ms.sourcegitcommit: 357afe80eae48e14dffdd51224c863c898303449
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/15/2017
---
# <a name="restore-files-to-a-windows-server-or-windows-client-machine-using-the-classic-deployment-model"></a>Klasik dağıtım modelini kullanarak bir Windows sunucusu veya Windows istemci makinesine dosyaları geri yükleme
> [!div class="op_single_selector"]
> * [Klasik portal](backup-azure-restore-windows-server-classic.md)
> * [Azure portal](backup-azure-restore-windows-server.md)
>
>

Bu makalede, bir yedekleme Kasası'nı verileri kurtarmak ve bir sunucu veya bilgisayara geri yüklemek açıklanmaktadır. Mart 2017'dan başlayarak, Klasik portalda yedekleme kasaları artık oluşturabilirsiniz.

> [!IMPORTANT]
> Artık Backup kasalarınızı Kurtarma Hizmetleri kasalarına yükseltebilirsiniz. Ayrıntılı bilgi için [Backup kasasını Kurtarma Hizmetleri kasasına yükseltme](backup-azure-upgrade-backup-to-recovery-services.md) makalesine bakın. Microsoft, Backup kasalarınızı Kurtarma Hizmetleri kasalarına yükseltmenizi önerir.<br/> Sonra **30 Kasım 2017**, yedekleme kasaları oluşturmak için PowerShell kullanmanız mümkün olmaz. <br/> **30 Kasım 2017 başlangıç**:
>- Yükseltilmemiş olan tüm Backup kasaları Kurtarma Hizmetleri kasalarına otomatik olarak yükseltilecektir.
>- Klasik portalda yedekleme verilerinize erişemeyeceksiniz. Bunun yerine, Kurtarma Hizmetleri kasalarındaki yedekleme verilerinize erişmek için Azure portalını kullanabilirsiniz.
>

Verileri geri yüklemek için Microsoft Azure kurtarma Hizmetleri (MARS) aracısı Veri Kurtarma Sihirbazı'nı kullanın. Verileri geri yüklediğinizde, mümkün olur:

* Veri yedekleri gerçekleştirilecek aynı makineye geri yükleyebilirsiniz.
* Verileri başka bir makineye geri yükleyin.

Ocak 2017 ' Microsoft MARS aracısı için Önizleme güncelleştirme yayımladı. Hata düzeltmeleri yanı sıra, bu güncelleştirme, anlık yazılabilir kurtarma noktası anlık görüntü kurtarma birimi olarak bağlamak izin veren geri yükleme sağlar. Ardından böylece seçerek dosyaları geri yükleme yerel bir bilgisayara kurtarma birimi ve kopyalama dosyalarını gözden geçirebilirsiniz.

> [!NOTE]
> [Ocak 2017 Azure Backup güncelleştirmesini](https://support.microsoft.com/en-us/help/3216528?preview) anlık geri verileri geri yüklemek için kullanmak istiyorsanız gereklidir. Ayrıca Yedekleme verileri yerel destek makalesinde listelenen kasalarında korunmalıdır. Başvurun [Ocak 2017 Azure Backup güncelleştirmesini](https://support.microsoft.com/en-us/help/3216528?preview) anlık geri yükleme desteği yerel ayarları en son listesi için. Anlık geri yükleme **değil** tüm bölgelerde kullanılabilir.
>

Anlık geri yükleme Azure portal ve klasik portalda yedekleme kasaları kurtarma Hizmetleri kasalarının kullanmak için kullanılabilir. Anlık geri kullanmak istiyorsanız, MARS güncelleştirmeyi indirin ve anlık geri Bahsediyor yordamları izleyin.


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
    > Çıkarma değil tıklatırsanız, Kurtarma birimi zaman takıldı zamandan altı saat bağlı kalır. Birim bağlıyken hiçbir yedekleme işlemleri çalıştırın. Birimin zaman bağlı, süre içinde çalıştırılmak üzere zamanlanmış herhangi bir yedekleme işlemi kurtarma birimi kaldırılan sonra çalışır.
    >


## <a name="recover-data-to-the-same-machine"></a>Aynı makineye verilerini kurtarma
Yanlışlıkla silinen bir dosya ve (yedeğin alındığı) aynı makineye geri yüklemek istiyorsanız, aşağıdaki adımları verileri kurtarmanıza yardımcı olur.

1. Açık **Microsoft Azure yedekleme** içinde Vurgu.
2. Tıklatın **verileri kurtarabilirsiniz** iş akışını başlatmak için.

    ![Verileri kurtarma](./media/backup-azure-restore-windows-server-classic/recover.png)
3. Seçin  **bu sunucunun (*yourmachinename*) ** dosyasını aynı makine üzerindeki yedeklenen geri yüklemek için seçeneği.

    ![Aynı makine](./media/backup-azure-restore-windows-server-classic/samemachine.png)
4. Tercih **gözatma için dosyaları** veya **dosya aramak için**.

    Dizinin yolu bilinen bir veya daha fazla dosya geri yüklemeyi planlıyorsanız varsayılan seçeneği bırakın. Klasör yapısı hakkında emin değilseniz, ancak bir dosyayı aramak istediğiniz çekme **dosya aramak için** seçeneği. Bu bölümde amacıyla biz varsayılan seçeneği ile devam eder.

    ![Gözatma dosyaları](./media/backup-azure-restore-windows-server-classic/browseandsearch.png)
5. Dosyayı geri yüklemek istediğiniz birimi seçin.

    Zaman içinde herhangi bir noktadan geri yükleyebilirsiniz. Görüntülenen tarihler **kalın** Takvim denetiminde bir geri yükleme noktası kullanılabilirliğini gösterir. Bir tarih seçtikten sonra yedekleme zamanlamanızı (ve bir yedekleme işlemi başarılı) göre bir nokta zamandan seçebileceğiniz **zaman** açılır.

    ![Birimi ve tarih](./media/backup-azure-restore-windows-server-classic/volanddate.png)
6. Kurtarmak için öğeleri seçin. Çoklu seçim klasörleri/dosyaları geri yüklemek istediğiniz kullanabilirsiniz.

    ![Dosya seçme](./media/backup-azure-restore-windows-server-classic/selectfiles.png)
7. Kurtarma parametreleri belirtin.

    ![Kurtarma Seçenekleri](./media/backup-azure-restore-windows-server-classic/recoveroptions.png)

   * (Hangi dosya/klasör üzerine yazılacağını) özgün konumuna veya başka bir konumda aynı makineye geri bir seçeneğiniz vardır.
   * Hedef konumda bulunan, geri yüklemek istediğiniz dosyayı/klasörü zaten varsa, kopyalar (aynı dosyanın iki sürümü) oluşturmak, hedef konumda dosyaların üzerine yazıp veya, hedef mevcut dosyaların kurtarma işlemini atla.
   * Kurtarılmakta dosyaları ACL'lerin geri yüklenmesi varsayılan seçeneği bırakın önerilir.
8. Bu girişleri sağlanan sonra tıklayın **sonraki**. Dosyaları bu makineye geri yükler, Kurtarma iş akışı başlar.

## <a name="recover-to-an-alternate-machine"></a>Alternatif bir makine kurtarması
Tüm sunucunuzu kaybolursa, verileri Azure yedekten başka bir makine kurtarmaya devam edebilirsiniz. Aşağıdaki adımlar, iş akışı gösterilmektedir.  

Bu adımlarda kullanılan terminolojiyi içerir:

* *Kaynak makine* – özgün makineden yedeğin alındığı ve hangi şu anda kullanılamıyor.
* *Hedef makine* – olduğu verilerin kurtarıldığı makine.
* *Örnek kasa* – yedekleme kasasına *kaynak makine* ve *hedef makine* kaydedilir. <br/>

> [!NOTE]
> Bir makineden alınan yedeklemeler, işletim sisteminin önceki bir sürümünü çalıştıran bir makineye geri yüklenemez. Yedeklemeler Windows 7 makineden alınır, örneğin, bir Windows 8 veya üstü makine geri yüklenebilir. Ancak, tam tersini true tutmaz.
>
>

1. Açık **Microsoft Azure yedekleme** üzerinde içinde Vurgu *hedef makine*.
2. Emin *hedef makine* ve *kaynak makine* aynı yedekleme Kasası'na kayıtlı.
3. Tıklatın **verileri kurtarabilirsiniz** iş akışını başlatmak için.

    ![Verileri kurtarma](./media/backup-azure-restore-windows-server-classic/recover.png)
4. Seçin **başka bir sunucu**

    ![Başka bir sunucu](./media/backup-azure-restore-windows-server-classic/anotherserver.png)
5. Karşılık gelen kasa kimlik bilgilerini sağlayın *örnek kasa*. Kasa kimlik bilgilerini geçersiz (veya süresi dolmuş) ise yeni bir kasa kimlik bilgileri dosyasını indirin *örnek kasa* Klasik Azure portalındaki. Kasa kimlik bilgilerini sağlanan sonra yedekleme kasasının kasa kimlik bilgilerini karşı görüntülenir.
6. Seçin *kaynak makine* görüntülenmesini makineler listesinden.

    ![Makineler listesi](./media/backup-azure-restore-windows-server-classic/machinelist.png)
7. Şunlardan birini seçin **dosya aramak için** veya **gözatma için dosyaları** seçeneği. Bu bölümde amacıyla kullanacağız **dosya aramak için** seçeneği.

    ![Arama](./media/backup-azure-restore-windows-server-classic/search.png)
8. Sonraki ekranda, tarih ve birim seçin. Geri yüklemek istediğiniz klasör/dosya adı arayın.

    ![Arama öğeleri](./media/backup-azure-restore-windows-server-classic/searchitems.png)
9. Dosyaları geri yüklenmesi gereken yeri konumu seçin.

    ![Geri yükleme konumu](./media/backup-azure-restore-windows-server-classic/restorelocation.png)
10. Sırasında sağlanan şifreleme parolası sağlamak *kaynak makinenin* kaydı için *örnek kasa*.

    ![Şifreleme](./media/backup-azure-restore-windows-server-classic/encryption.png)
11. Giriş sağlanan sonra tıklayın **kurtarmak**, sağlanan hedef Yedeklenen dosyaları geri yükleme tetikler.

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
    > Çıkarma değil tıklatırsanız, Kurtarma birimi zaman takıldı zamandan altı saat bağlı kalır. Birim bağlıyken hiçbir yedekleme işlemleri çalıştırın. Birimin zaman bağlı, süre içinde çalıştırılmak üzere zamanlanmış herhangi bir yedekleme işlemi kurtarma birimi kaldırılan sonra çalışır.
    >


## <a name="next-steps"></a>Sonraki adımlar
* [Azure Backup ile ilgili SSS](backup-azure-backup-faq.md)
* Ziyaret [Azure yedekleme Forumu](http://go.microsoft.com/fwlink/p/?LinkId=290933).

## <a name="learn-more"></a>Daha fazla bilgi edinin
* [Azure yedekleme genel bakış](http://go.microsoft.com/fwlink/p/?LinkId=222425)
* [Yedekleme Azure sanal makineler](backup-azure-vms-introduction.md)
* [Microsoft iş yüklerini yedekleme](backup-azure-dpm-introduction.md)
