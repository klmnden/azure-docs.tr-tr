---
title: 'Azure yedekleme: Windows Server sistem durumunu geri yükle'
description: Windows Server sistem durumunu azure'da bir yedekten geri yüklemek için adım açıklama adımla.
services: backup
author: saurabhsensharma
manager: shivamg
ms.service: backup
ms.topic: conceptual
ms.date: 8/18/2017
ms.author: saurse
ms.openlocfilehash: 6619611bee96089e465feb6f50d38caeada06dd9
ms.sourcegitcommit: 399db0671f58c879c1a729230254f12bc4ebff59
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/09/2019
ms.locfileid: "65472497"
---
# <a name="restore-system-state-to-windows-server"></a>Windows Server sistem durumunu geri yükle

Bu makalede, Windows Server sistem durumu yedeklemeleri bir Azure kurtarma Hizmetleri kasasından geri yükleme açıklanmaktadır. Sistem durumunu geri yüklemek için bir sistem durumu yedeklemesi olmalıdır (yönergeleri kullanılarak oluşturulan [sistem durumu yedekleme](backup-azure-system-state.md#back-up-windows-server-system-state)ve yüklediğinizden emin olun [en son sürümünü Microsoft Azure kurtarma Hizmetleri (MARS) Aracı](https://aka.ms/azurebackup_agent). Windows Server Sistem Durumu verilerini bir Azure kurtarma Hizmetleri kasasından kurtarma iki adımlı bir işlemdir:

1. Azure yedekleme dosyalarından olarak sistem durumunu geri yükleme. Azure yedekleme dosyalarından olarak sistem durumu geri yüklerken şunlardan birini yapabilirsiniz:
   * Sistem durumunu geri yükleme yeri yedekleri gerçekleştirilen, aynı sunucuya veya
   * Başka bir sunucu için sistem durumunu geri yükleme dosyası.

2. Geri yüklenen sistem durumu dosyaları, bir Windows Server için geçerlidir.


## <a name="recover-system-state-files-to-the-same-server"></a>Aynı sunucuya kurtarma sistem durumu dosyaları
Aşağıdaki adımlar, Windows Server yapılandırmanız için önceki bir duruma geri alma işlemleri açıklanmaktadır. Sunucu yapılandırmanızı, bilinen ve kararlı bir duruma geri alma, son derece yararlı olabilir. Aşağıdaki adımlar, bir kurtarma Hizmetleri kasasından sunucunun sistem durumu geri yükleyin.

1. **Microsoft Azure Backup** ek bileşenini açın. Ek bileşenini yüklendiği bilmiyorsanız, bilgisayar veya sunucu için arama **Microsoft Azure Backup**.

    Masaüstü uygulaması, arama sonuçlarında görüntülenmesi gerekir.

2. Tıklayın **veri kurtarma** Sihirbazı'nı başlatın.

    ![Verileri kurtarma](./media/backup-azure-restore-windows-server/recover.png)

3. Üzerinde **Başlarken** verileri aynı sunucuya veya bilgisayara geri yüklemek için bölmesinde seçin **bu sunucu (`<server name>`)** tıklatıp **sonraki**.

    ![Bu sunucu seçeneği aynı makinede verileri geri yüklemek için](./media/backup-azure-restore-system-state/samemachine.png)

4. Üzerinde **kurtarma modunu Seç** bölmesinde seçin **sistem durumu** ve ardından **sonraki**.

    ![Dosyalara göz atın](./media/backup-azure-restore-system-state/recover-type-selection.png)

5. Takvim üzerinde **birim ve tarih seçin** bölmesinde, bir kurtarma noktası.

    Zaman içinde herhangi bir kurtarma noktasından geri yükleyebilirsiniz. Tarihler **kalın** en az bir kurtarma noktasının kullanılabilirliğini gösterir. Birden fazla kurtarma noktası mevcutsa bir tarih seçtiğinizde belirli bir kurtarma noktasından seçin **zaman** açılan menüsü.

    ![Birim ve tarih](./media/backup-azure-restore-system-state/select-date.png)

6. Geri yüklemek için kurtarma noktası seçtikten sonra tıklayın **sonraki**.

    Azure Backup, yerel kurtarma noktası bağlar ve kurtarma birimi olarak kullanır.

7. Sonraki bölmesinde, kurtarılan sistem durumu dosyaları hedefini belirtin ve tıklayın **Gözat** Windows Gezgini'ni açın ve istediğiniz klasörleri ve dosyaları bulun. Seçenek **böylece hem sürümlerde kopya oluşturma**, tüm sistem durumu arşiv kopyasını oluşturmak yerine var olan bir sistem durumu Dosya arşiv tek tek dosyaların kopyalarını oluşturur.

    ![Kurtarma Seçenekleri](./media/backup-azure-restore-system-state/recover-as-files.png)

8. Kurtarma ayrıntılarını doğrulayın **onay** bölmesi ve tıklatın **kurtarmak**.

   ![Kurtarma kurtarma eylemi onaylamak için tıklayın](./media/backup-azure-restore-system-state/confirm-recovery.png)

9. Kopyalama *WindowsImageBackup* kurtarma hedef sunucunun kritik olmayan bir birime dizin. Genellikle, Windows işletim sistemi birimi kritik bir birimdir.

10. Kurtarma başarılı olduktan sonra bölümdeki adımları [Uygula sistem durumu dosyaları Windows Server'a geri](backup-azure-restore-system-state.md), sistem durumu kurtarma işlemini tamamlamak için.

## <a name="recover-system-state-files-to-an-alternate-server"></a>Başka bir sunucu için sistem durumunu kurtarma dosyaları

Bozuk ya da erişilemez, Windows Server ve Windows Server sistem durumunu kurtarma tarafından kararlı bir duruma geri yüklemek istediğiniz, bozuk sunucu sistem durumu başka bir sunucudan geri yükleyebilirsiniz. Ayrı bir sunucuda sistem durumu geri yükleme için aşağıdaki adımları kullanın.  

Bu adımlarda kullanılan terminolojiyi içerir:

- *Kaynak makine* – özgün makineden alındığı ve hangi şu anda kullanılamıyor.
- *Hedef makine* – olduğu veri kurtarılıyor makine.
- *Örnek kasası* – kurtarma Hizmetleri kasası için *kaynak makine* ve *hedef makine* kaydedilir. <br/>

> [!NOTE]
> Bir makineden alınan yedeklemeler, işletim sisteminin önceki bir sürümü çalıştıran bir makineye yüklenemez. Örneğin, bir Windows Server 2016 makineden alınan yedeklemeler Windows Server 2012 R2'ye geri yüklenemez. Ancak tersi de mümkündür. Yedeklemeler Windows Server 2012 R2'den Windows Server 2016'yı geri yüklemek için kullanabilirsiniz.
>

1. Açık **Microsoft Azure Backup** ek *hedef makine*.
2. Emin *hedef makine* ve *kaynak makine* aynı kurtarma Hizmetleri kasasına kayıtlı.
3. Tıklayın **veri kurtarma** iş akışını başlatmak için.
4. Seçin **başka bir sunucu**

    ![Başka bir sunucu](./media/backup-azure-restore-system-state/anotherserver.png)

5. Karşılık gelen kasa kimlik bilgilerini sağlayın *örnek kasası*. Kasa kimlik bilgilerini geçersiz (veya süresi dolmuş) ise, yeni bir kasa kimlik bilgileri dosyasını indirin *örnek kasası* Azure portalında. Kasa kimlik bilgilerini sağlandıktan sonra kasa kimlik bilgileri dosyası ile ilişkilendirilen bir kurtarma Hizmetleri kasası görünür.

6. Yedekleme sunucusu seçin bölmeden *kaynak makine* görüntülenen makineler listesinden.
7. Kurtarma modunu Seç bölmesinde **sistem durumu** tıklatıp **sonraki**.

    ![Arama](./media/backup-azure-restore-system-state/recover-type-selection.png)

8. Takvim üzerinde **birim ve tarih seçin** bölmesinde, bir kurtarma noktası. Zaman içinde herhangi bir kurtarma noktasından geri yükleyebilirsiniz. Tarihler **kalın** en az bir kurtarma noktasının kullanılabilirliğini gösterir. Birden fazla kurtarma noktası mevcutsa bir tarih seçtiğinizde belirli bir kurtarma noktasından seçin **zaman** açılan menüsü.

    ![Arama öğeleri](./media/backup-azure-restore-system-state/select-date.png)

9. Geri yüklemek için kurtarma noktası seçtikten sonra tıklayın **sonraki**.

10. Üzerinde **sistem durumu kurtarma modunu Seç** bölmesinde sistem durumu dosyaları kurtarılması'a tıklayın, istediğiniz hedefi belirtin **sonraki**.

    ![Şifreleme](./media/backup-azure-restore-system-state/recover-as-files.png)

    Seçenek **böylece hem sürümlerde kopya oluşturma**, tüm sistem durumu arşiv kopyasını oluşturmak yerine var olan bir sistem durumu Dosya arşiv tek tek dosyaların kopyalarını oluşturur.

11. Onay bölmesinde kurtarma ayrıntılarını doğrulayın ve tıklayın **kurtarmak**.

    ![kurtarma işlemini onaylamak için kurtarma düğmesine tıklayın](./media/backup-azure-restore-system-state/confirm-recovery.png)

12. Kopyalama *WindowsImageBackup* dizin sunucunun kritik olmayan bir birime (örneğin, D:\). Genellikle Windows işletim sistemi birimi kritik bir birimdir.

13. Kurtarma işlemini tamamlamak için aşağıdaki bölüme kullanın [Windows Server'da geri yüklenen sistem durumu dosyaları geçerli](#apply-restored-system-state-on-a-windows-server).




## <a name="apply-restored-system-state-on-a-windows-server"></a>Windows Server'da geri yüklenen sistem durumu uygulayın

Bir kez sistem durumu olarak Azure kurtarma Hizmetleri Aracısı'nı kullanarak dosyaları kurtardı, kurtarılan sistem durumunu Windows sunucusuna uygulamak için Windows sunucu yedeklemesi yardımcı programını kullanın. Windows sunucu yedeklemesi yardımcı programını, sunucu üzerinde zaten mevcuttur. Aşağıdaki adımlarda, kurtarılan sistem durumu uygulamak açıklanmaktadır.

1. Sunucunuzu yeniden başlatmak için aşağıdaki komutları kullanın *dizin hizmetleri onarım Modu'nda*. Yükseltilmiş bir komut istemi'nde:

    ```
    PS C:\> Bcdedit /set safeboot dsrepair
    PS C:\> Shutdown /r /t 0
    ```

2. Yeniden başlatıldıktan sonra Windows Server Yedekleme ek bileşenini açın. Ek bileşenini yüklendiği bilmiyorsanız, bilgisayar veya sunucu için arama **Windows Server Yedekleme**.

    Masaüstü uygulamasını arama sonuçlarında görünür.

3. Ek bileşeninde, seçin **yerel yedekleme**.

    ![buradan geri yüklemek için yerel yedeği seçin](./media/backup-azure-restore-system-state/win-server-backup-local-backup.png)

4. Yerel yedekleme konsolu içinde **Eylemler bölmesinde**, tıklayın **kurtarmak** Kurtarma Sihirbazı'nı açın.

5. Seçeneğini **başka bir konumda depolanan bir yedek**, tıklatıp **sonraki**.

   ![farklı bir sunucuya kurtarmak seçin](./media/backup-azure-restore-system-state/backup-stored-in-diff-location.png)

6. Konum türü belirtirken seçin **paylaşılan uzak klasör** sistem durumu yedeklemenizin başka bir sunucuya kurtarıldı. Yerel Sistem durumunuzu yapılıyorsa, ardından **yerel sürücüler**.

    ![Yerel sunucu ya da başka bir kurtarma olup olmadığını seçin](./media/backup-azure-restore-system-state/ss-recovery-remote-shared-folder.png)

7. Yineleme'nin *WindowsImageBackup* dizin ya kurtarılan Azure Recovery Services ile sistem durumu dosyaları kurtarma işleminin parçası olarak bu dizin (örneğin, D:\WindowsImageBackup) içeren yerel bir sürücü seçin Aracı ve tıklatın **sonraki**.

    ![Paylaşılan dosyasının yolu](./media/backup-azure-restore-system-state/ss-recovery-remote-folder.png)

8. Geri yükleme ve istediğiniz sistem durumu sürümü seçin **sonraki**.

9. Kurtarma türünü seçin bölmesinde seçin **sistem durumu** tıklatıp **sonraki**.

10. Sistem durumu kurtarma konumunu seçin **özgün konumuna**, tıklatıp **sonraki**.

11. Onay ayrıntıları gözden geçirin, yeniden başlatma ayarları doğrulayın ve tıklayın **kurtarmak** geri yüklenen sistem durumu dosyaları uygulamak için.

    ![geri yükleme işlemi başlatma sistem durumu dosyaları](./media/backup-azure-restore-system-state/launch-ss-recovery.png)

## <a name="special-considerations-for-system-state-recovery-on-active-directory-server"></a>Active Directory sunucusu üzerindeki sistem durumu kurtarma için özel hususlar

Sistem durumu yedeklemesi, Active Directory verilerini içerir. Aşağıdaki adımlarda Active Directory etki alanı hizmeti (AD DS) geri yüklemek için önceki bir duruma geçerli durumunu kullanın.

1. Etki alanı denetleyicisi Dizin Hizmetleri geri yükleme modu (DSRM) yeniden başlatın.
2. Adımları [burada](https://technet.microsoft.com/library/cc794755(v=ws.10).aspx) AD DS kurtarmak için Windows Server Yedekleme cmdlet'lerini kullanmak için.


## <a name="troubleshoot-failed-system-state-restore"></a>Başarısız sistem durumu geri yükleme sorunlarını giderme

Önceki sistem durumu uygulama işlemini başarıyla tamamlanmazsa, Windows Server'ı kurtarmak için Windows Kurtarma Ortamı'nı (Win RE) kullanın. Aşağıdaki adımlar, Win RE kullanarak kurtarma işlemleri açıklanmaktadır. Yalnızca Windows Server normalde bir sistem durumu geri yüklemeden sonra önyükleme yapmaz, bu seçeneği kullanın. Aşağıdaki işlem sistemi olmayan verileri, dikkatli siler.

1. Windows Server, Windows Kurtarma Ortamı'nı (Win RE) içine önyükleyin.

2. Sorun giderme üç kullanılabilir seçenekler arasından seçim.

    ![Açılış menüsü](./media/backup-azure-restore-system-state/winre-1.png)

3. Gelen **Gelişmiş Seçenekler** ekranındayken **komut istemi** sunucusu yönetici kullanıcı adı ve parolasını girin.

   ![Açılış menüsü](./media/backup-azure-restore-system-state/winre-2.png)

4. Sunucu yönetici kullanıcı adını ve parolasını belirtin.

    ![Açılış menüsü](./media/backup-azure-restore-system-state/winre-3.png)

5. Yönetici modunda komut istemini açın, aşağıdaki sistem durumu yedekleme sürümlerini almak için bir komut çalıştırın.

    ```
    Wbadmin get versions -backuptarget:<Volume where WindowsImageBackup folder is copied>:
    ```
    ![Sistem Durumu yedekleme sürümlerini edinme](./media/backup-azure-restore-system-state/winre-4.png)

6. Yedeklemeye kullanılabilir tüm birimleri almak için aşağıdaki komutu çalıştırın.

    ```
    Wbadmin get items -version:<copy version from above step> -backuptarget:<Backup volume>
    ```

    ![Sistem Durumu yedekleme sürümlerini edinme](./media/backup-azure-restore-system-state/winre-5.png)

7. Aşağıdaki komut, sistem durumu yedeklemesi parçası olan tüm birimleri kurtarır. Bu adım yalnızca sistem durumu bir parçası olan kritik birimleri kurtardığını unutmayın. Tüm sistem dışı verileri silinir.

    ```
    Wbadmin start recovery -items:C: -itemtype:Volume -version:<Backupversion> -backuptarget:<backup target volume>
    ```
     ![Sistem Durumu yedekleme sürümlerini edinme](./media/backup-azure-restore-system-state/winre-6.png)



## <a name="next-steps"></a>Sonraki adımlar
* Dosya ve klasörlerinizi kurtarma yaptıktan sonra şunları yapabilirsiniz [yedeklemelerinizi yönetme](backup-azure-manage-windows-server.md).
