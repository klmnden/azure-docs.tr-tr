---
title: Azure yedekleme sunucusu Azure yedekleme bir Exchange sunucusu yedekleme | Microsoft Docs
description: "Azure yedekleme sunucusu kullanarak Azure yedekleme için bir Exchange server'ı Yedekle öğrenin"
services: backup
documentationcenter: 
author: pvrk
manager: shivamg
editor: 
ms.assetid: e46557e8-2eaf-4ee0-99ea-00fbb8687dca
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: pullabhk
ms.openlocfilehash: 60b784fd00013c2b9504f8635c6b5c4c592563be
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="back-up-an-exchange-server-to-azure-backup-with-azure-backup-server"></a>Azure yedekleme ile Azure yedekleme sunucusu için bir Exchange server'ı Yedekle
Bu makalede, Azure için Microsoft Exchange sunucusunu yedeklemek için Microsoft Azure yedekleme sunucusu (MABS) yapılandırmak açıklar.  

## <a name="prerequisites"></a>Ön koşullar
Devam etmeden önce Azure yedekleme sunucusu olduğundan emin olun [yüklü ve hazırlanan](backup-azure-microsoft-azure-backup.md).

## <a name="mabs-protection-agent"></a>MABS koruma Aracısı
Exchange server üzerinde MABS koruma aracısı yüklemek için aşağıdaki adımları izleyin:

1. Güvenlik duvarları doğru şekilde yapılandırıldığından emin olun. Bkz: [aracı için güvenlik duvarı özel durumlarını yapılandırma](https://technet.microsoft.com/library/Hh758204.aspx).
2. Aracısı'nı tıklatarak Exchange sunucusunda yükleyin **Yönetim > aracıları > yükleme** MABS Yönetici konsolunda. Bkz: [MABS koruma aracısını yüklemek](https://technet.microsoft.com/library/hh758186.aspx?f=255&MSPPError=-2147217396) ayrıntılı adımlar için.

## <a name="create-a-protection-group-for-the-exchange-server"></a>Exchange server için bir koruma grubu oluşturma
1. MABS Yönetici Konsolu'nda **koruma**ve ardından **yeni** açmak için araç şeridinde **yeni koruma grubu oluşturma** Sihirbazı.
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

    Bu seçeneği belirledikten sonra yedekleme tutarlılığı denetleniyor çalıştırarak oluşturulan g/ç trafiği önlemek için MABS üzerinde çalıştırılacak **eseutil** Exchange sunucusundaki komutu.

   > [!NOTE]
   > Bu seçeneği kullanmak için Ese.dll ve Eseutil.exe dosyalarını MAB sunucusunda C:\Program Files\Microsoft Azure Backup\DPM\DPM\bin dizinine kopyalamanız gerekir. Aksi takdirde, aşağıdaki hata tetiklenir:  
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
12. Hangi MAB sunucunun ilk çoğaltma oluşturur ve ardından istediğiniz saati seçin **sonraki**.
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
1. Bir Exchange veritabanını kurtarmak için **kurtarma** MABS Yönetici konsolunda.
2. Kurtarmak istediğiniz Exchange veritabanı bulun.
3. Bir çevrimiçi kurtarma noktası seçin *kurtarma süresini* aşağı açılan liste.
4. Tıklatın **kurtarmak** başlatmak için **Kurtarma Sihirbazı'nı**.

Çevrimiçi kurtarma noktaları için beş kurtarma türü vardır:

* **Özgün Exchange Server konumuna Kurtar:** verileri özgün Exchange Server'a kurtarılır.
* **Exchange Server üzerindeki başka bir veritabanına Kurtar:** verileri başka bir Exchange server üzerindeki başka bir veritabanına kurtarıldı.
* **Kurtarma veritabanına Kurtar:** verileri bir Exchange kurtarma veritabanına (RDB) kurtarılacak.
* **Bir ağ klasörüne kopyala:** verileri bir ağ klasörüne kurtarılacak.
* **Banda Kopyala:** bir bant kitaplığını veya bağlı ve MABS üzerinde yapılandırılmış tek başına bant sürücüsü varsa, kurtarma noktası bir boş banda kopyalanacak.

    ![Çevrimiçi çoğaltma seçin](./media/backup-azure-backup-exchange-server/choose-online-replication.png)

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Backup ile ilgili SSS](backup-azure-backup-faq.md)
