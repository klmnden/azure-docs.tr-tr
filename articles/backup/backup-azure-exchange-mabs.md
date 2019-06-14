---
title: Azure Backup sunucusu ile Azure Backup'a bir Exchange server'ı yedekleme
description: Azure Backup sunucusu kullanarak Azure Backup için bir Exchange sunucusunu yedeklemek hakkında bilgi edinin
services: backup
author: kasinh
manager: vvithal
ms.service: backup
ms.topic: conceptual
ms.date: 03/24/2017
ms.author: kasinh
ms.openlocfilehash: 40541596b4da9e0590d497785afd7d6d7f4cbcb4
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60641508"
---
# <a name="back-up-an-exchange-server-to-azure-with-azure-backup-server"></a>Azure Backup sunucusu ile azure'a bir Exchange server'ı yedekleme
Bu makalede, Azure'da bir Microsoft Exchange sunucusunu yedeklemek için Microsoft Azure Backup sunucusu (MABS) yapılandırma açıklanır.  

## <a name="prerequisites"></a>Önkoşullar
Devam etmeden önce Azure Backup sunucusu olduğundan emin olun [yüklü ve hazırlanmış](backup-azure-microsoft-azure-backup.md).

## <a name="mabs-protection-agent"></a>MABS koruma Aracısı
Exchange sunucusunda MABS koruma aracısını yüklemek için aşağıdaki adımları izleyin:

1. Güvenlik Duvarı doğru şekilde yapılandırıldığından emin olun. Bkz: [aracı için güvenlik duvarı özel durumlarını yapılandırma](https://technet.microsoft.com/library/Hh758204.aspx).
2. Tıklayarak Exchange sunucusuna aracı yükleme **Yönetim > aracıları > yükleme** MABS Yönetici konsolunda. Bkz: [MABS koruma aracısını yüklemek](https://technet.microsoft.com/library/hh758186.aspx?f=255&MSPPError=-2147217396) ayrıntılı adımlar için.

## <a name="create-a-protection-group-for-the-exchange-server"></a>Exchange sunucusu için koruma grubu oluşturma
1. MABS Yönetici Konsolu'nda **koruma**ve ardından **yeni** açmak için araç şeridindeki **yeni koruma grubu oluşturma** Sihirbazı.
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

    Bu seçeneği belirledikten sonra yedekleme tutarlılığı denetimini çalıştırılarak oluşturulan g/ç trafiği önlemek için MABS üzerinde çalıştırılacak **eseutil** Exchange sunucusundaki komutu.

   > [!NOTE]
   > Bu seçeneği kullanmak için Ese.dll ve Eseutil.exe dosyalarını MAB sunucusundaki C:\Program Files\Microsoft Azure Backup\DPM\DPM\bin dizinine kopyalamanız gerekir. Aksi takdirde, aşağıdaki hata tetiklenir:  
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
12. Başlangıçtan MAB sunucusu ilk çoğaltma oluşturma ve ardından saati **sonraki**.
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
1. Bir Exchange veritabanını kurtarma için tıklatın **kurtarma** MABS yönetici Konsolu'ndaki.
2. Kurtarmak istediğiniz Exchange veritabanını bulun.
3. Bir çevrimiçi kurtarma noktası seçin *kurtarma zamanı* aşağı açılan listesi.
4. Tıklayın **kurtarmak** başlatmak için **Kurtarma Sihirbazı'nı**.

Çevrimiçi kurtarma noktaları için beş kurtarma türü vardır:

* **Özgün Exchange Server konumuna Kurtar:** Verileri özgün Exchange Server'a kurtarılır.
* **Exchange Server üzerindeki başka bir veritabanına Kurtar:** Verileri başka bir Exchange server üzerindeki başka bir veritabanına kurtarılır.
* **Bir kurtarma veritabanına kurtarma:** Bir Exchange kurtarma veritabanına (RDB) verileri kurtarılır.
* **Bir ağ klasörüne kopyala:** Verileri bir ağ klasörüne kurtarılır.
* **Banda Kopyala:** Bir bant kitaplığı veya bağlı ve MABS üzerinde yapılandırılmış bir tek başına bant sürücüsü varsa, kurtarma noktası bir boş banda kopyalanır.

    ![Çevrimiçi çoğaltma seçin](./media/backup-azure-backup-exchange-server/choose-online-replication.png)

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Backup ile ilgili SSS](backup-azure-backup-faq.md)
