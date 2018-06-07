---
title: System Center 2012 R2 DPM ile Azure Backup’a Exchange sunucusu yedekleme
description: System Center 2012 R2 DPM kullanarak Azure yedekleme için bir Exchange server'ı Yedekle öğrenin
services: backup
author: MaanasSaran
manager: NKolli1
ms.service: backup
ms.topic: troubleshooting
ms.date: 09/08/2017
ms.author: adigan
ms.openlocfilehash: 4edec499d12261add398e5a9297f039ecfb252e9
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34605109"
---
# <a name="back-up-an-exchange-server-to-azure-backup-with-system-center-2012-r2-dpm"></a>System Center 2012 R2 DPM ile Azure Backup’a Exchange sunucusu yedekleme
Bu makalede Azure yedekleme için bir Microsoft Exchange sunucusunu yedeklemek için System Center 2012 R2 Data Protection Manager (DPM) sunucusunun nasıl yapılandırılacağı açıklanmaktadır.  

## <a name="updates"></a>Güncelleştirmeler
Azure yedekleme ile DPM sunucusu başarıyla kaydetmek için System Center 2012 R2 DPM ve Azure Yedekleme aracısı en son sürümü için en son güncelleştirme paketini yüklemeniz gerekir. En son güncelleştirme paketini gelen alma [Microsoft Catalog](http://catalog.update.microsoft.com/v7/site/Search.aspx?q=System%20Center%202012%20R2%20Data%20protection%20manager).

> [!NOTE]
> Bu makaledeki örneklerde, Azure Yedekleme aracısı 2.0.8719.0 sürümü yüklendi ve güncelleştirme paketi 6 System Center 2012 R2 DPM üzerinde yüklü.
>
>

## <a name="prerequisites"></a>Önkoşullar
Devam etmeden önce emin olun tüm [Önkoşullar](backup-azure-dpm-introduction.md#prerequisites) iş yüklerini korumak için Microsoft Azure Yedekleme kullanılarak karşılandığından. Bu Önkoşullar şunlardır:

* Bir yedekleme kasası Azure sitede oluşturuldu.
* DPM sunucusuna aracısını ve kasa kimlik bilgileri indirildi.
* Aracıyı DPM sunucusuna yüklenir.
* Kasa kimlik bilgileri, DPM sunucusunu kaydetmek için kullanılmıştır.
* Lütfen Exchange 2016 koruyorsanız, DPM 2012 R2 UR9 veya sonraki bir sürüme yükseltin

## <a name="dpm-protection-agent"></a>DPM koruma Aracısı
Exchange sunucusunda DPM koruma aracısı yüklemek için aşağıdaki adımları izleyin:

1. Güvenlik duvarları doğru şekilde yapılandırıldığından emin olun. Bkz: [aracı için güvenlik duvarı özel durumlarını yapılandırma](https://technet.microsoft.com/library/Hh758204.aspx).
2. Aracısı'nı tıklatarak Exchange sunucusunda yükleyin **Yönetim > aracıları > yükleme** DPM yönetici Konsolu'ndaki. Bkz: [DPM koruma Aracısı yükleme](https://technet.microsoft.com/library/hh758186.aspx?f=255&MSPPError=-2147217396) ayrıntılı adımlar için.

## <a name="create-a-protection-group-for-the-exchange-server"></a>Exchange server için bir koruma grubu oluşturma
1. DPM Yönetici Konsolu'nda **koruma**ve ardından **yeni** açmak için araç şeridinde **yeni koruma grubu oluşturma** Sihirbazı.
2. Üzerinde **Hoş Geldiniz** Sihirbazı'nı ekranın **sonraki**.
3. Üzerinde **koruma grubu türünü seçin** ekranında, seçin **sunucuları** tıklatıp **sonraki**.
4. Tıklatıp korumak istediğiniz Exchange server veritabanını seçin **sonraki**.

   > [!NOTE]
   > Exchange 2013 koruyorsanız, denetleme [Exchange 2013 önkoşulları](https://technet.microsoft.com/library/dn751029.aspx).
   >
   >

    Aşağıdaki örnekte, Exchange 2010 veritabanı seçilir.

    ![Grup üyelerini seçin](./media/backup-azure-backup-exchange-server/select-group-members.png)
5. Veri koruma yöntemini seçin.

    Koruma grubu adı ve her ikisi de aşağıdaki seçeneklerden birini seçin:

   * Disk kullanılan kısa vadeli koruma istiyorum.
   * Çevrimiçi koruma istiyorum.
6. **İleri**’ye tıklayın.
7. Seçin **veri bütünlüğünü denetlemek için Eseutil'i Çalıştır** Exchange Server veritabanlarının bütünlüğünü kontrol etmek istiyorsanız seçeneği.

    Bu seçeneği belirledikten sonra yedekleme tutarlılığı denetleniyor çalıştırarak oluşturulan g/ç trafiği önlemek için DPM sunucusunda çalıştırılır **eseutil** Exchange sunucusundaki komutu.

   > [!NOTE]
   > Bu seçeneği kullanmak için DPM sunucusunda C:\Program Files\Microsoft System Center 2012 R2\DPM\DPM\bin dizinine Ese.dll ve Eseutil.exe dosyalarını kopyalamanız gerekir. Aksi takdirde, aşağıdaki hata tetiklenir:  
   > ![Eseutil hata](./media/backup-azure-backup-exchange-server/eseutil-error.png)
   >
   >
8. **İleri**’ye tıklayın.
9. Veritabanı için seçin **kopya yedekleme**ve ardından **sonraki**.

   > [!NOTE]
   > Bir veritabanı en az bir DAG kopyası için "tam yedekleme" seçeneğini belirlemezseniz günlükler kesilmez.
   >
   >
10. Hedefler için yapılandırma **kısa vadeli yedekleme**ve ardından **sonraki**.
11. Kullanılabilir disk alanını gözden geçirin ve ardından **sonraki**.
12. Hangi DPM sunucusunu ilk çoğaltma oluşturur ve ardından istediğiniz saati seçin **sonraki**.
13. Tutarlılık denetimi seçeneklerini seçin ve ardından **sonraki**.
14. Azure'a yedeklemek ve ardından istediğiniz veritabanını seçin **sonraki**. Örneğin:

    ![Çevrimiçi koruma verilerini belirtin](./media/backup-azure-backup-exchange-server/specify-online-protection-data.png)
15. İçin zamanlama tanımla **Azure Backup**ve ardından **sonraki**. Örneğin:

    ![Çevrimiçi Yedekleme zamanlamasını belirtin](./media/backup-azure-backup-exchange-server/specify-online-backup-schedule.png)

    > [!NOTE]
    > Çevrimiçi kurtarma noktası hızlı temel unutmayın tam kurtarma noktaları. Bu nedenle, belirtilen süre için hızlı tam kurtarma noktası sonra çevrimiçi kurtarma noktası zamanlamanız gerekir.
    >
    >
16. Bekletme İlkesi yapılandırma **Azure Backup**ve ardından **sonraki**.
17. Çevrimiçi çoğaltma seçeneğini belirleyin ve tıklatın **sonraki**.

    Büyük bir veritabanınız varsa, ağ üzerinden oluşturulacak ilk yedekleme uzun bir süredir ele geçirebilir. Bu sorunu önlemek için Çevrimdışı Yedekleme oluşturabilirsiniz.  

    ![Çevrimiçi bekletme ilkesini belirtin](./media/backup-azure-backup-exchange-server/specify-online-retention-policy.png)
18. Ayarları onaylayın ve ardından **Grup Oluştur**.
19. **Kapat**’a tıklayın.

## <a name="recover-the-exchange-database"></a>Exchange veritabanını kurtarma
1. Bir Exchange veritabanını kurtarmak için **kurtarma** DPM Yönetici Konsolu'nun.
2. Kurtarmak istediğiniz Exchange veritabanı bulun.
3. Bir çevrimiçi kurtarma noktası seçin *kurtarma süresini* aşağı açılan liste.
4. Tıklatın **kurtarmak** başlatmak için **Kurtarma Sihirbazı'nı**.

Çevrimiçi kurtarma noktaları için beş kurtarma türü vardır:

* **Özgün Exchange Server konumuna Kurtar:** verileri özgün Exchange Server'a kurtarılır.
* **Exchange Server üzerindeki başka bir veritabanına Kurtar:** verileri başka bir Exchange server üzerindeki başka bir veritabanına kurtarıldı.
* **Kurtarma veritabanına Kurtar:** verileri bir Exchange kurtarma veritabanına (RDB) kurtarılacak.
* **Bir ağ klasörüne kopyala:** verileri bir ağ klasörüne kurtarılacak.
* **Banda Kopyala:** bir bant kitaplığını veya bağlı ve DPM sunucusunda yapılandırılmış tek başına bant sürücüsü varsa, kurtarma noktası bir boş banda kopyalanacak.

    ![Çevrimiçi çoğaltma seçin](./media/backup-azure-backup-exchange-server/choose-online-replication.png)

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Backup ile ilgili SSS](backup-azure-backup-faq.md)
