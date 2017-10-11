---
title: "Azure yedekleme: Bir Windows Server sistem durumunu geri yükle | Microsoft Docs"
description: "Windows Server sistem durumu azure'da bir yedekten geri yüklemek için adım açıklama tarafından adım."
services: backup
documentationcenter: 
author: saurabhsensharma
manager: shivamg
editor: 
ms.assetid: 
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 8/18/2017
ms.author: saurse;trinadhk;markgal;
ms.openlocfilehash: 320c85f8045d9b72cf7f430d2e2736ba8e5ec269
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2017
---
# <a name="restore-system-state-to-windows-server"></a>Windows Server sistem durumunu geri yükle

Bu makalede, Azure kurtarma Hizmetleri Kasası'nı Windows Server sistem durumu yedeği açıklanmaktadır. Sistem durumunu geri yüklemek için bir sistem durumu yedeklemesi olmalıdır ('ndaki yönergeleri kullanarak oluşturduğunuz [sistem durumu yedekleme](backup-azure-system-state.md#back-up-windows-server-system-state-preview)) ve yüklediğinizden emin olun [en son sürümünü Microsoft Azure Recovery Services'ın (MARS) Aracı](http://aka.ms/azurebackup_agent). Azure kurtarma Hizmetleri Kasası'nı Windows Server Sistem Durumu verilerini kurtarma iki adımlı bir işlemdir:

1. Sistem durumu, Azure yedekleme dosyalarını farklı geri yükle. Azure yedekleme dosyalarından olarak sistem durum geri yüklerken şunlardan birini yapabilirsiniz:
  * Burada yedeklemeleri gerçekleştirilecek, geri yükleme sistem durumu aynı sunucuya veya
  * Başka bir sunucu dosyasına sistem durumunu geri yükle.

2. Geri yüklenen sistem durumu dosyaları, Windows Server için geçerli.


## <a name="recover-system-state-files-to-the-same-server"></a>Aynı sunucu sistem durumunu kurtarma dosyaları
Aşağıdaki adımlar, Windows Server yapılandırmanızı önceki bir duruma geri açıklanmaktadır. Sunucu yapılandırması için bilinen, kararlı bir duruma geri, son derece yararlı olabilir. Aşağıdaki adımlar sunucunun sistem durumu kurtarma Hizmetleri Kasası'nı geri yükleyin. 

1. Açık **Microsoft Azure yedekleme** ek bileşenini. Ek bileşenini yüklendiği bilmiyorsanız, bilgisayar veya sunucu için arama **Microsoft Azure yedekleme**.

    Masaüstü uygulaması arama sonuçlarında görüntülenmesi gerekir.

2. Tıklatın **verileri kurtarabilirsiniz** Sihirbazı'nı başlatın.

    ![Verileri kurtarma](./media/backup-azure-restore-windows-server/recover.png)

3. Üzerinde **Başlarken** verileri aynı sunucu veya bilgisayara geri yüklemek için bölmeyi seçin **bu sunucunun (`<server name>`)** tıklatıp **sonraki**.

    ![Bu sunucu seçeneği aynı makineye verileri geri yüklemek için](./media/backup-azure-restore-system-state/samemachine.png)

4. Üzerinde **seçin kurtarma moduna** bölmesinde seçin **sistem durumu** ve ardından **sonraki**.

    ![Gözatma dosyaları](./media/backup-azure-restore-system-state/recover-type-selection.png)

5. Takvimde **birim seçin ve tarih** bölmesinde seçin kurtarma noktası. 

    Zaman içinde herhangi bir kurtarma noktasından geri yükleyebilirsiniz. İçinde tarihleri **kalın** en az bir kurtarma noktası kullanılabilirliğini gösterir. Birden fazla kurtarma noktası mevcutsa, bir tarih seçtikten sonra belirli bir kurtarma noktasından seçin **zaman** açılır menü.

    ![Birimi ve tarih](./media/backup-azure-restore-system-state/select-date.png)

6. Geri yüklemek için kurtarma noktası seçtikten sonra tıklatın **sonraki**.

    Azure yedekleme, yerel kurtarma noktası bağlar ve kurtarma birimi olarak kullanır.

7. Sonraki bölmesinde, kurtarılan sistem durumu dosyaları hedefini belirtin ve tıklatın **Gözat** Windows Gezgini'ni açın ve istediğiniz klasörleri ve dosyaları bulmak için. Seçenek **böylece her iki sürümünü de sahip kopya oluşturma**, tüm sistem durumu arşiv kopyasını oluşturmak yerine var olan bir sistem durumu Dosya arşiv tek tek dosyaların kopyalarını oluşturur.

    ![Kurtarma Seçenekleri](./media/backup-azure-restore-system-state/recover-as-files.png)

8. Kurtarma ayrıntılarını doğrulayın **onay** bölmesinde ve tıklatın **kurtarmak**.

   ![kurtarma eylemi onaylamak için Kurtar'ı tıklatın](./media/backup-azure-restore-system-state/confirm-recovery.png)

9. Kopya *WindowsImageBackup* sunucunun kritik olmayan bir birim için kurtarma hedef dizin. Genellikle, Windows işletim sistemi birimi kritik bir birimdir.

10. Kurtarma başarılı olduktan sonra bölümdeki adımları [Uygula Windows Server için sistem durumu dosyaları geri](backup-azure-restore-system-state.md#apply-restored-system-state-files-to-the-windows-server), sistem durumu kurtarma işlemini tamamlamak için.

## <a name="recover-system-state-files-to-an-alternate-server"></a>Başka bir sunucu sistem durumunu kurtarma dosyaları

Windows Server'ınızın bozuk veya erişilemez durumda ise ve Windows Server sistem durumunu kurtarma tarafından kararlı bir duruma geri yüklemek istediğiniz, başka bir sunucudan bozuk sunucunun sistem durumu geri yükleyebilirsiniz. Ayrı bir sunucuda sistem durumu geri yükleme için aşağıdaki adımları kullanın.  

Bu adımlarda kullanılan terminolojiyi içerir:

- *Kaynak makine* – özgün makineden yedeğin alındığı ve hangi şu anda kullanılamıyor.
- *Hedef makine* – olduğu verilerin kurtarıldığı makine.
- *Örnek kasa* – kurtarma Hizmetleri kasası için *kaynak makine* ve *hedef makine* kaydedilir. <br/>

> [!NOTE]
> Bir makineden alınan yedeklemeler, işletim sisteminin önceki bir sürümünü çalıştıran bir bilgisayara geri yüklenemez. Örneğin, bir Windows Server 2016 makineden alınan yedeklemeler Windows Server 2012 R2'ye geri yüklenemez. Ancak, ters mümkündür. Windows Server 2016 geri yüklemek için Windows Server 2012 R2 yedeklemelerden kullanabilirsiniz.
>

1. Açık **Microsoft Azure yedekleme** ek bileşenini *hedef makine*.
2. Emin *hedef makine* ve *kaynak makine* aynı kurtarma Hizmetleri kasasına kayıtlı.
3. Tıklatın **verileri kurtarabilirsiniz** iş akışını başlatmak için.

    ![Verileri kurtarma](./media/backup-azure-restore-windows-server-classic/recover.png)

4. Seçin **başka bir sunucu**

    ![Başka bir sunucu](./media/backup-azure-restore-system-state/anotherserver.png)

5. Karşılık gelen kasa kimlik bilgilerini sağlayın *örnek kasa*. Kasa kimlik bilgilerini geçersiz (veya süresi dolmuş) varsa, yeni bir kasa kimlik bilgileri dosyasını indirin *örnek kasa* Azure portalında. Kasa kimlik bilgilerini sağlanan sonra kasa kimlik bilgileri dosyasıyla ilişkili kurtarma Hizmetleri kasası görünür.

6. Yedekleme sunucusu seçin bölmesinde seçin *kaynak makine* görüntülenmesini makineler listesinden.

    ![Makineler listesi](./media/backup-azure-restore-windows-server-classic/machinelist.png)

7. Seçim kurtarma moduna bölmesinde seçin **sistem durumu** tıklatıp **sonraki**. 

    ![Arama](./media/backup-azure-restore-system-state/recover-type-selection.png)

8. Takvimde **birim seçin ve tarih** bölmesinde seçin kurtarma noktası. Zaman içinde herhangi bir kurtarma noktasından geri yükleyebilirsiniz. İçinde tarihleri **kalın** en az bir kurtarma noktası kullanılabilirliğini gösterir. Birden fazla kurtarma noktası mevcutsa, bir tarih seçtikten sonra belirli bir kurtarma noktasından seçin **zaman** açılır menü. 

    ![Arama öğeleri](./media/backup-azure-restore-system-state/select-date.png)

9. Geri yüklemek için kurtarma noktası seçtikten sonra tıklatın **sonraki**.

10. Üzerinde **sistem durumu kurtarma modunu seçin** bölmesinde sistem durumu dosyaları kurtarılması ve ardından istediğiniz hedef belirtin **sonraki**.

    ![Şifreleme](./media/backup-azure-restore-system-state/recover-as-files.png)

    Seçenek **böylece her iki sürümünü de sahip kopya oluşturma**, tüm sistem durumu arşiv kopyasını oluşturmak yerine var olan bir sistem durumu Dosya arşiv tek tek dosyaların kopyalarını oluşturur.

11. Onay bölmesinde kurtarma ayrıntılarını doğrulayın ve tıklatın **kurtarmak**. 

    ![kurtarma işlemini onaylamak için kurtarma düğmesini tıklatın.](./media/backup-azure-restore-system-state/confirm-recovery.png)

12. Kopya *WindowsImageBackup* sunucunun kritik olmayan bir birime dizin (örneğin D:\). Genellikle Windows işletim sistemi birimi kritik bir birimdir.

13. Kurtarma işlemini tamamlamak için aşağıdaki bölümü kullanın [geri yüklenen sistem durumu dosyaları bir Windows sunucusu üzerindeki geçerli](backup-azure-restore-system-state.md#apply-restored-system-state-on-a-windows-server).




## <a name="apply-restored-system-state-on-a-windows-server"></a>Bir Windows sunucusuna geri yüklenen sistem durumu Uygula

Azure kurtarma Hizmetleri aracısını kullanarak dosyaları olarak sistem durumu kez kurtardı, Windows Server kurtarılan sistem durumu uygulamak için Windows Server Yedekleme yardımcı programı kullanın. Windows Server Yedekleme yardımcı programı, sunucu üzerinde zaten mevcuttur. Aşağıdaki adımlar, kurtarılan sistem durumunu uygulamak açıklanmaktadır.

1. Sunucuyu yeniden başlatmak için aşağıdaki komutları kullanın *dizin hizmetleri onarım modunda*. Yükseltilmiş bir komut istemi'nde:

    ```
    PS C:\> Bcdedit /set safeboot dsrepair
    PS C:\> Shutdown /r /t 0
    ```

2. Yeniden başlatıldıktan sonra Windows Server Yedekleme ek bileşenini açın. Ek bileşenini yüklendiği bilmiyorsanız, bilgisayar veya sunucu için arama **Windows Server Yedekleme**.

    Masaüstü uygulama arama sonuçlarında görünür.

3. Ek bileşeninde, seçin **yerel yedekleme**.

    ![buradan geri yüklemek için yerel yedekleme seçin](./media/backup-azure-restore-system-state/win-server-backup-local-backup.png)

4. Yerel yedekleme konsolunda içinde **Eylemler bölmesi**, tıklatın **kurtarmak** Kurtarma Sihirbazı'nı açmak için.

5. Seçeneğini **başka bir konumda depolanan bir yedek**, tıklatıp **sonraki**.

   ![farklı bir sunucuya kurtarmak seçin](./media/backup-azure-restore-system-state/backup-stored-in-diff-location.png)

6. Konum türü belirtirken seçin **uzaktan paylaşılan klasörünü** , sistem durumu yedeklemesi başka bir sunucuya kurtarıldı. Sistem durumu yerel olarak yapılıyorsa, ardından **yerel sürücüler**. 

    ![Yerel sunucu ya da başka bir kurtarma olup olmadığını seçin](./media/backup-azure-restore-system-state/ss-recovery-remote-shared-folder.png)

7. Yolu girin *WindowsImageBackup* dizini ya da Azure kurtarma Hizmetleri kullanarak sistem durumu dosyaları kurtarmanın bir parçası kurtarılan bu dizin (örneğin, D:\WindowsImageBackup) içeren yerel sürücüyü seçin Aracı ve tıklatın **sonraki**.

    ![Paylaşılan dosyasının yolu](./media/backup-azure-restore-system-state/ss-recovery-remote-folder.png)

8. Tıklatın ve geri yüklemek istediğiniz sistem durumu sürümü seçin **sonraki**.

9. Kurtarma türünü seçin bölmesinde seçin **sistem durumu** tıklatıp **sonraki**.

10. Sistem durumu kurtarma konumunu seçin **özgün konumuna**, tıklatıp **sonraki**.

11. Onay ayrıntılarını gözden geçirin, yeniden başlatma ayarları doğrulayın ve tıklatın **kurtarmak** applly geri yüklenen sistem durumu için dosyaları.

    ![Geri Yüklemeyi Başlat sistem durumu dosyaları](./media/backup-azure-restore-system-state/launch-ss-recovery.png)

## <a name="special-considerations-for-system-state-recovery-on-active-directory-server"></a>Active Directory sunucuda sistem durumu kurtarma için özel hususlar

Sistem durumu yedeklemesi Active Directory verilerini içerir. Aşağıdaki adımlarda Active Directory etki alanı hizmeti (AD DS) geri yüklemek için önceki bir duruma geçerli durumunu kullanın.

1. Etki alanı denetleyicisi Dizin Hizmetleri geri yükleme modu (DSRM) yeniden başlatın.
2. Adımları [burada](https://technet.microsoft.com/en-us/library/cc794755(v=ws.10).aspx) AD DS kurtarmak için Windows Server Yedekleme cmdlet'lerini kullanmak için.


## <a name="troubleshoot-failed-system-state-restore"></a>Başarısız sistem durumu geri yükleme sorunlarını giderme

Sistem durumu uygulama önceki işlem başarıyla tamamlanmazsa, Windows sunucunuzu kurtarmak için Windows Kurtarma Ortamı'nı (Win RE) kullanın. Aşağıdaki adımlar, Win RE kullanarak kurtarma açıklanmaktadır. Yalnızca Windows Server normalde bir sistem durumu geri yüklendikten sonra önyükleme yapmaz, bu seçeneği kullanın. Aşağıdaki işlem sistem dışı verileri, dikkatli siler. 

1. Windows Server'ınızın Windows Kurtarma Ortamı'nı (Win RE) içine önyükleme yapın.

2. Sorun giderme üç kullanılabilir seçenekler arasından seçin.

    ![Açılış menüsü](./media/backup-azure-restore-system-state/winre-1.png)

3. Gelen **Gelişmiş Seçenekler** ekran, select **komut istemi** server yönetici kullanıcı adı ve parolasını girin.

   ![Açılış menüsü](./media/backup-azure-restore-system-state/winre-2.png)

4. Sunucu yönetici kullanıcı adı ve parola belirtin.

    ![Açılış menüsü](./media/backup-azure-restore-system-state/winre-3.png)

5. Yönetici modunda bir komut istemi açın, sistem durumu yedekleme sürümlerini almak için komutu aşağıdaki çalıştırın.

    ```
    Wbadmin get versions -backuptarget:<Volume where WindowsImageBackup folder is copied>:
    ```
    ![Sistem Durumu yedekleme sürümlerini alma](./media/backup-azure-restore-system-state/winre-4.png)

6. Yedekleme kullanılabilir tüm birimleri almak için aşağıdaki komutu çalıştırın.

    ```
    Wbadmin get items -version:<copy version from above step> -backuptarget:<Backup volume>
    ```

    ![Sistem Durumu yedekleme sürümlerini alma](./media/backup-azure-restore-system-state/winre-5.png)

7. Aşağıdaki komut, sistem durumu yedeklemesi parçası olan tüm birimleri kurtarır. Bu adım yalnızca sistem durumu parçası olan kritik birimleri kurtarır unutmayın. Tüm sistem dışı veriler silinir.

    ```
    Wbadmin start recovery -items:C: -itemtype:Volume -version:<Backupversion> -backuptarget:<backup target volume>
    ```
     ![Sistem Durumu yedekleme sürümlerini alma](./media/backup-azure-restore-system-state/winre-6.png)



## <a name="next-steps"></a>Sonraki adımlar
* Dosya ve klasörleri kurtarma yaptıktan, yapabilecekleriniz [Yedeklemelerinizin yönetmek](backup-azure-manage-windows-server.md).
