---
title: System Center 2012 R2 DPM ile Azure Backup’a Exchange sunucusu yedekleme
description: System Center 2012 R2 DPM kullanarak Azure Backup için bir Exchange sunucusunu yedeklemek hakkında bilgi edinin
services: backup
author: kasinh
manager: vvithal
ms.service: backup
ms.topic: conceptual
ms.date: 01/31/2019
ms.author: kasinh
ms.openlocfilehash: ef976667ec580ea75dd1b8566c7bdddf35eeb0fc
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60647275"
---
# <a name="back-up-an-exchange-server-to-azure-backup-with-system-center-2012-r2-dpm"></a>System Center 2012 R2 DPM ile Azure Backup’a Exchange sunucusu yedekleme
Bu makalede, Azure Backup için bir Microsoft Exchange sunucusunu yedeklemek için System Center 2012 R2 Data Protection Manager (DPM) sunucusunun nasıl yapılandırılacağı açıklanır.  

## <a name="updates"></a>Güncelleştirmeler
DPM sunucusu Azure yedekleme ile başarıyla kaydetmek için System Center 2012 R2 DPM ve Azure yedekleme Aracısı'nın en son sürümü için en son güncelleştirme paketini yüklemeniz gerekir. En son güncelleştirme paketi Al [Microsoft Catalog](https://catalog.update.microsoft.com/v7/site/Search.aspx?q=System%20Center%202012%20R2%20Data%20protection%20manager).

> [!NOTE]
> Bu makaledeki örnekler, Azure Backup Aracısı 2.0.8719.0 sürümü yüklü ve güncelleştirme paketi 6 System Center 2012 R2 DPM üzerinde yüklenir.
>
>

## <a name="prerequisites"></a>Önkoşullar
Devam etmeden önce emin tüm [önkoşulları](backup-azure-dpm-introduction.md#prerequisites-and-limitations) iş yüklerini korumak için Microsoft Azure Yedekleme kullanarak karşılanmış. Bu Önkoşullar şunlardır:

* Bir yedekleme kasası Azure sitesindeki oluşturuldu.
* Aracısını ve kasa kimlik bilgileri, DPM sunucusuna yüklendi.
* Aracıyı DPM sunucusuna yüklenir.
* DPM sunucusunu kaydetmek için kasa kimlik bilgilerini kullanıldı.
* Exchange 2016 koruyorsanız, lütfen DPM 2012 R2 UR9 veya sonraki bir sürüme yükseltin

## <a name="dpm-protection-agent"></a>DPM koruma Aracısı
Exchange sunucusunda DPM koruma aracısını yüklemek için aşağıdaki adımları izleyin:

1. Güvenlik Duvarı doğru şekilde yapılandırıldığından emin olun. Bkz: [aracı için güvenlik duvarı özel durumlarını yapılandırma](https://technet.microsoft.com/library/Hh758204.aspx).
2. Tıklayarak Exchange sunucusuna aracı yükleme **Yönetim > aracıları > yükleme** DPM yönetici Konsolu'ndaki. Bkz: [DPM koruma aracısını yüklemek](https://technet.microsoft.com/library/hh758186.aspx?f=255&MSPPError=-2147217396) ayrıntılı adımlar için.

## <a name="create-a-protection-group-for-the-exchange-server"></a>Exchange sunucusu için koruma grubu oluşturma
1. DPM Yönetici Konsolu'nda **koruma**ve ardından **yeni** açmak için araç şeridindeki **yeni koruma grubu oluşturma** Sihirbazı.
2. Üzerinde **Hoş Geldiniz** Sihirbazı'nı ekranın **sonraki**.
3. Üzerinde **koruma grubu türünü seçin** ekranında, seçin **sunucuları** tıklatıp **sonraki**.
4. Seçin ve korumak istediğiniz Exchange server veritabanına **sonraki**.

   > [!NOTE]
   > Exchange 2013'ü koruyorsanız denetleyin [Exchange 2013 önkoşulları](https://technet.microsoft.com/library/dn751029.aspx).
   >
   >

    Aşağıdaki örnekte, Exchange 2010 veritabanı seçilmedi.

    ![Grup üyelerini seçin](./media/backup-azure-backup-exchange-server/select-group-members.png)
5. Veri koruma yöntemini seçin.

    Koruma grubunu adlandırın ve ardından aşağıdaki seçeneklerden birini seçin:

   * Disk kullanılan kısa vadeli koruma istiyorum.
   * Çevrimiçi koruma istiyorum.
6. **İleri**’ye tıklayın.
7. Seçin **veri bütünlüğünü denetlemek için Eseutil'i** Exchange Server veritabanlarının bütünlüğünü kontrol etmek isterseniz seçeneği.

    Bu seçeneği belirledikten sonra yedekleme tutarlılığı denetimini çalıştırılarak oluşturulan g/ç trafiği önlemek için DPM sunucusunda çalıştırılacak **eseutil** Exchange sunucusundaki komutu.

   > [!NOTE]
   > Bu seçeneği kullanmak için Ese.dll ve Eseutil.exe dosyalarını DPM sunucusundaki C:\Program Files\Microsoft System Center 2012 R2\DPM\DPM\bin dizinine kopyalamanız gerekir. Aksi takdirde, aşağıdaki hata tetiklenir:  
   > ![Eseutil hata](./media/backup-azure-backup-exchange-server/eseutil-error.png)
   >
   >
8. **İleri**’ye tıklayın.
9. Veritabanı seçin **kopya yedekleme**ve ardından **sonraki**.

   > [!NOTE]
   > "Tam yedekleme" için bir veritabanının en az bir DAG kopyasını seçmezseniz, günlükler kesilmez.
   >
   >
10. Hedefler için yapılandırma **kısa vadeli yedekleme**ve ardından **sonraki**.
11. Kullanılabilir disk alanını inceleyin ve ardından **sonraki**.
12. Saati, DPM sunucusu ilk çoğaltma oluşturma ve ardından **sonraki**.
13. Tutarlılık denetimi seçeneklerini seçin ve ardından **sonraki**.
14. Azure'a yedeklemek ve ardından istediğiniz veritabanını seçin **sonraki**. Örneğin:

    ![Çevrimiçi koruma verilerini belirtin](./media/backup-azure-backup-exchange-server/specify-online-protection-data.png)
15. İçin zamanlama tanımla **Azure Backup**ve ardından **sonraki**. Örneğin:

    ![Çevrimiçi Yedekleme zamanlamasını belirtin](./media/backup-azure-backup-exchange-server/specify-online-backup-schedule.png)

    > [!NOTE]
    > Çevrimiçi kurtarma noktaları express ile temel unutmayın tam kurtarma noktaları. Bu nedenle, belirtilen süre için hızlı tam kurtarma noktası sonra çevrimiçi kurtarma noktası zamanlamanız gerekir.
    >
    >
16. Bekletme İlkesi yapılandırma **Azure Backup**ve ardından **sonraki**.
17. Çevrimiçi çoğaltma seçeneğini belirleyin ve tıklayın **sonraki**.

    Büyük bir veritabanı varsa, ağ üzerinden oluşturulacak ilk yedekleme için uzun sürebilir. Bu sorunu önlemek için bir çevrimiçi yedekleme oluşturabilirsiniz.  

    ![Çevrimiçi bekletme ilkesini belirtin](./media/backup-azure-backup-exchange-server/specify-online-retention-policy.png)
18. Ayarları onaylayın ve ardından **Grup Oluştur**.
19. **Kapat**’a tıklayın.

## <a name="recover-the-exchange-database"></a>Bir Exchange veritabanını kurtarma
1. Bir Exchange veritabanını kurtarma için tıklatın **kurtarma** DPM yönetici Konsolu'ndaki.
2. Kurtarmak istediğiniz Exchange veritabanını bulun.
3. Bir çevrimiçi kurtarma noktası seçin *kurtarma zamanı* aşağı açılan listesi.
4. Tıklayın **kurtarmak** başlatmak için **Kurtarma Sihirbazı'nı**.

Çevrimiçi kurtarma noktaları için beş kurtarma türü vardır:

* **Özgün Exchange Server konumuna Kurtar:** Verileri özgün Exchange Server'a kurtarılır.
* **Exchange Server üzerindeki başka bir veritabanına Kurtar:** Verileri başka bir Exchange server üzerindeki başka bir veritabanına kurtarılır.
* **Bir kurtarma veritabanına kurtarma:** Bir Exchange kurtarma veritabanına (RDB) verileri kurtarılır.
* **Bir ağ klasörüne kopyala:** Verileri bir ağ klasörüne kurtarılır.
* **Banda Kopyala:** Bir bant kitaplığı veya bağlı ve DPM sunucusunda yapılandırılan bir tek başına bant sürücüsü varsa, kurtarma noktası bir boş banda kopyalanır.

    ![Çevrimiçi çoğaltma seçin](./media/backup-azure-backup-exchange-server/choose-online-replication.png)

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Backup ile ilgili SSS](backup-azure-backup-faq.md)
