---
title: Azure SQL Data Sync sorunlarını giderme | Microsoft Docs
description: Azure SQL Data Sync ile ilgili yaygın sorunları gidermeyi öğrenin.
services: sql-database
ms.service: sql-database
ms.subservice: data-movement
ms.custom: data sync
ms.devlang: ''
ms.topic: conceptual
author: allenwux
ms.author: xiwu
ms.reviewer: carlrab
manager: craigg
ms.date: 12/20/2018
ms.openlocfilehash: 4e2808378834a0270586ce674e1043ca443320c5
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60331205"
---
# <a name="troubleshoot-issues-with-sql-data-sync"></a>SQL Data Sync ile ilgili sorunları giderme

Bu makalede, Azure SQL Data Sync ile ilgili bilinen sorunlar giderilir açıklar. Bir sorun için bir çözüm varsa, burada sağlanır.

SQL Data Sync hizmetine genel bakış için bkz. [Azure SQL Data Sync ile birden fazla bulut ve şirket içi veritabanı arasında veri eşitleme](sql-database-sync-data.md).

> [!IMPORTANT]
> Azure SQL Data Sync mu **değil** şu anda Azure SQL veritabanı yönetilen örneği destekler.

## <a name="sync-issues"></a>Eşitleme sorunları

- [İstemci Aracısı ile ilişkili olan şirket içi veritabanları için portal UI eşitleme başarısız](#sync-fails)

- [Eşitleme grubunuza işleme durumda takılmasına neden olabilir](#sync-stuck)

- [Tablolarımın içinde hatalı veri görüyorum](#sync-baddata)

- [Başarılı bir eşitlemeden sonra tutarsız birincil anahtar veri görüyorum](#sync-pkdata)

- [Önemli bir performans düşüşü görüyorum](#sync-perf)

- [Bu iletiyi görüyorum: "Sütununa NULL değer eklenemiyor \<sütun >. Sütun null değerlere izin vermiyor." Bunun anlamı nedir ve nasıl düzeltebilirim?](#sync-nulls)

- [Veri eşitleme, döngüsel başvurular nasıl işliyor? Diğer bir deyişle, ne zaman aynı verileri birden çok eşitleme gruplarında eşitlenen ve bunun sonucunda değişir mi?](#sync-circ)

### <a name="sync-fails"></a> İstemci Aracısı ile ilişkili olan şirket içi veritabanları için portal UI eşitleme başarısız

İstemci Aracısı ile ilişkili olan şirket içi veritabanları için SQL Data Sync portalında UI eşitleme başarısız. Aracıyı çalıştıran yerel bilgisayarda System.IO.ıoexception hataları olay günlüğüne bakın. Hataları, diskte yeterli alan olduğunu varsayalım.

- **Neden**. Sürücü yeterli alan yok.

- **Çözüm**. % TEMP % dizininde bulunduğu sürücüde daha fazla alan oluşturun.

### <a name="sync-stuck"></a> Eşitleme grubunuza işleme durumda takılmasına neden olabilir

SQL Data Sync'deki bir eşitleme grubuna uzun bir süre içinde işleme durumu kaldırıldı. Yanıt vermiyor **Durdur** komut ve günlükleri göster yeni giriş yok.

Aşağıdaki koşullardan herhangi biri işleme durumunda takılması bir eşitleme grubu neden olabilir:

- **Neden**. İstemci aracısı çevrimdışı

- **Çözüm**. İstemci aracısının çevrimiçi olduğundan emin olun ve işlemi yeniden deneyin.

- **Neden**. İstemci aracısı yüklü değil veya eksik.

- **Çözüm**. İstemci aracısı yüklü değilse veya eksikse:

    1. Aracı XML dosyası mevcutsa, dosyayı SQL Data Sync yükleme klasöründen kaldırın.
    1. Aracıyı bir şirket içi bilgisayara yükleyin (aynı bilgisayar veya farklı bir bilgisayar olabilir). Ardından, portalda çevrimdışı olarak gösterilen aracı için oluşturulan aracı anahtarını gönderin.

- **Neden**. SQL Data Sync hizmeti durduruldu.

- **Çözüm**. SQL Data Sync hizmetini yeniden başlatın.

    1. İçinde **Başlat** menüsünde **Hizmetleri**.
    1. Arama sonuçlarında seçin **Hizmetleri**.
    1. Bulma **SQL Data Sync** hizmeti.
    1. Hizmet durumu ise **durduruldu**hizmet adını sağ tıklatın ve ardından **Başlat**.

> [!NOTE]
> Yukarıdaki bilgiler eşitleme grubunuz işleme durumu dışında hareket etmediği Microsoft Support eşitleme grubunuz durumunu sıfırlayabilirsiniz. Eşitleme grubu durumunda, buna sıfırlama [Azure SQL veritabanının Forumu](https://social.msdn.microsoft.com/Forums/azure/home?forum=ssdsgetstarted), bir gönderi oluşturun. İletide, abonelik Kimliğiniz ve sıfırlanması gerekiyor grubu için eşitleme grubu kimliği içerir. Bir Microsoft Support mühendisi gönderiniz için yanıt vereceğini ve bunu ne zaman durumu sıfırlandı size bildirir.

### <a name="sync-baddata"></a> Tablolarımın içinde hatalı veri görüyorum

Aynı ada sahip olan ancak farklı veritabanı şemalardan olan tabloları eşitleme dahil edilirse, eşitleme sonrasında hatalı tablolardaki verileri görürsünüz.

- **Neden**. SQL Data Sync sağlama işlemi, aynı ada sahip olan ancak farklı şemalarda olan tablolar için aynı izleme tablolarını kullanır. Bu nedenle, her iki tablodaki değişiklik izleme tabloda yansıtılır. Bu, eşitleme sırasında hatalı veri değişiklikleri neden olur.

- **Çözüm**. Farklı şemalara bir veritabanında tabloları ait olsa bile eşitleme söz konusu olan tablo adları farklı olduğundan emin olun.

### <a name="sync-pkdata"></a> Başarılı bir eşitlemeden sonra tutarsız birincil anahtar veri görüyorum

Eşitleme başarılı olarak bildirildi ve günlük başarısız veya Atlanan satır gösterir, ancak birincil anahtar veri eşitleme grubundaki veritabanları arasında tutarsız olup olmadığına bakın.

- **Neden**. Tasarım gereği oluşur. Birincil anahtarı nerede değiştirildi satırlardaki tutarsız veri değişiklikleri tüm birincil anahtar sütunu sonuçlanır.

- **Çözüm**. Bu sorunu önlemek için birincil anahtar sütunu içinde hiç veri değiştirildiğinde emin olun. Gerçekleştikten sonra bu sorunu gidermek için tüm uç noktalarından tutarsız veri eşitleme grubunda bulunan satır silin. Ardından, satır takın.

### <a name="sync-perf"></a> Önemli bir performans düşüşü görüyorum

Performansınızı önemli ölçüde büyük olasılıkla veri eşitleme UI bile burada açılamıyor noktasına düşürür.

- **Neden**. Bir eşitleme döngüsü en olası nedeni. Eşitleme ile eşitleme grubu A tarafından eşitleme eşitleme grubu A'daki ardından tetikler tarafından eşitleme eşitleme grubu B, tetiklendiğinde bir eşitleme döngüsü gerçekleşir. Gerçek durum daha karmaşık olabilir ve birden fazla iki eşitleme grubu döngüsünde gerektirebilir. Sorun bir döngüsel eşitlemeyi tetikleme olan başka bir çakışan eşitleme gruplarına göre neden olur.

- **Çözüm**. En iyi önleme açıklanmıştır. Eşitleme gruplarınızı döngüsel başvurular olmadığından emin olun. Bir eşitleme grubu tarafından eşitlenen herhangi bir satırın başka bir eşitleme grubu ile eşitlenemiyor.

### <a name="sync-nulls"></a> Bu iletiyi görüyorum: "Sütununa NULL değer eklenemiyor \<sütun >. Sütun null değerlere izin vermiyor." Bunun anlamı nedir ve nasıl düzeltebilirim? 
Bu hata iletisini iki aşağıdaki sorunlardan biri oluştuğunu gösterir:
-  Bir tabloda bir birincil anahtar yok. Bu sorunu gidermek için birincil anahtarı eşitleniyor tüm tabloları ekleyin.
-  CREATE INDEX deyiminde WHERE yan tümcesi yoktur. Veri eşitleme, bu durum işlemiyor. Bu sorunu gidermek için WHERE yan tümcesini kaldırın veya tüm veritabanları için el ile değişiklik. 
 
### <a name="sync-circ"></a> Veri eşitleme, döngüsel başvurular nasıl işliyor? Diğer bir deyişle, ne zaman aynı verileri birden çok eşitleme gruplarında eşitlenen ve bunun sonucunda değişir mi?
Veri eşitleme, döngüsel başvurular işlemiyor. Kaçınma emin olun. 

## <a name="client-agent-issues"></a>İstemci aracı sorunları

İstemci Aracısı ile ilgili sorunları gidermek için bkz: [veri eşitleme Aracısı sorunlarını giderme sorunları](sql-database-data-sync-agent.md#agent-tshoot).

## <a name="setup-and-maintenance-issues"></a>Kurulum ve Bakım konuları

- ["Disk alanı yetersiz" iletisi alıyorum](#setup-space)

- [Eşitleme grubunuza silemiyorum](#setup-delete)

- [Bir şirket içi SQL Server veritabanı kaydı silinemiyor](#setup-unreg)

- [Sistem hizmetlerini başlatmak için yeterli ayrıcalıklara sahip değilsiniz](#setup-perms)

- [Bir veritabanı "Süresi geçmiş" durumunda](#setup-date)

- [Bir eşitleme grubu bir "Süresi geçmiş" durumunda](#setup-date2)

- [Kaldırma veya Aracısı durduruluyor üç dakika içinde bir eşitleme grubu silinemiyor](#setup-delete2)

- [Kayıp veya bozuk bir veritabanı geri yükleyebilirim ne olur?](#setup-restore)

### <a name="setup-space"></a> "Disk alanı yetersiz" iletisi alıyorum

- **Neden**. Silinecek kalan dosyaları gereksiniminiz varsa "disk alanı yetersiz" iletisi görüntülenir. Bu tarafından virüsten koruma yazılımı kaynaklanabilir veya dosya açık olduğunda silme işlemleri çalıştı.

- **Çözüm**. % Temp % klasöründe eşitleme dosyaları el ile silin (`del \*sync\* /s`). Daha sonra alt dizinleri % temp % klasörünü silin.

> [!IMPORTANT]
> Eşitleme işlemi devam ederken dosyalarla silmeyin.

### <a name="setup-delete"></a> Eşitleme grubunuza silemiyorum

Eşitleme grubunu silme denemesi başarısız olur. Aşağıdaki senaryolardan herhangi bir eşitleme grubunu silmek için hatasına neden olabilir:

- **Neden**. İstemci aracısı çevrimdışı.

- **Çözüm**. İstemci Aracısı çevrimiçi olduğundan emin olun ve işlemi yeniden deneyin.

- **Neden**. İstemci aracısı yüklü değil veya eksik.

- **Çözüm**. İstemci aracısı yüklü değilse veya eksikse:  
    a. Aracı XML dosyası mevcutsa, dosyayı SQL Data Sync yükleme klasöründen kaldırın.  
    b. Aracıyı bir şirket içi bilgisayara yükleyin (aynı bilgisayar veya farklı bir bilgisayar olabilir). Ardından, portalda çevrimdışı olarak gösterilen aracı için oluşturulan aracı anahtarını gönderin.

- **Neden**. Bir veritabanının çevrimdışı olduğundan.

- **Çözüm**. SQL veritabanları ve SQL Server veritabanları çevrimiçi olduğundan emin olun.

- **Neden**. Eşitleme grubu sağlama veya eşitleniyor.

- **Çözüm**. Sağlama veya eşitleme işlemi tamamlanana kadar bekleyin ve sonra eşitleme grubu silme işlemini yeniden deneyin.

### <a name="setup-unreg"></a> Bir şirket içi SQL Server veritabanı kaydı silinemiyor

- **Neden**. Büyük olasılıkla zaten silinmiş olan bir veritabanı kaydını çalışıyorsunuz.

- **Çözüm**. Bir şirket içi SQL Server veritabanı kaydını kaldırmak için veritabanını seçin ve ardından **zorla silme**.

  Veritabanı eşitleme gruptan kaldırmak bu işlem başarısız olursa:

  1. Durdur ve istemci Aracısı konak hizmeti yeniden başlatın:  
    a. Seçin **Başlat** menüsü.  
    b. Arama kutusuna **services.msc**.  
    c. İçinde **programlar** bölümünü arama sonuçları bölmesinde çift **Hizmetleri**.  
    d. Sağ **SQL Data Sync** hizmeti.  
    e. Hizmeti çalışıyorsa durdurun.  
    f. Hizmete sağ tıklayın ve ardından **Başlat**.  
    g. Veritabanı hala kayıtlı olup olmadığını denetleyin. Artık kayıtlı değilse, hazırsınız. Aksi halde, sonraki adımla devam edin.
  1. İstemci Aracısı uygulamasını (SqlAzureDataSyncAgent) açın.
  1. Seçin **bilgilerini Düzenle**, veritabanı için kimlik bilgilerini girin.
  1. Kayıt silme ile devam edin.

### <a name="setup-perms"></a> Sistem hizmetlerini başlatmak için yeterli ayrıcalıklara sahip değilsiniz

- **Neden**. Bu hata, iki durumda gerçekleşir:
  -   Kullanıcı adı ve/veya parola yanlış.
  -   Belirtilen kullanıcı hesabı, hizmet olarak oturum açmak için yeterli ayrıcalıklara sahip değil.

- **Çözüm**. Kullanıcı hesabı için log-üzerinde-bir hizmet olarak kimlik bilgileri verin:

  1. Git **Başlat** > **Denetim Masası** > **Yönetimsel Araçlar** > **yerel güvenlik ilkesi**  >  **Yerel ilke** > **Kullanıcı Hakları Yönetimi**.
  1. Seçin **hizmet oturum açma**.
  1. İçinde **özellikleri** iletişim kutusunda, kullanıcı hesabını ekleyin.
  1. Seçin **Uygula**ve ardından **Tamam**.
  1. Tüm pencereleri kapatın.

### <a name="setup-date"></a> Bir veritabanı "Süresi geçmiş" durumunda

- **Neden**. SQL Data Sync çevrimdışı hizmetinden 45 gün veya daha fazla (veritabanı çevrimdışı olduğu zamandan sayılan gibi) için olan veritabanlarını kaldırır. Bir veritabanı 45 gün veya daha fazla bilgi için çevrimdışı ise ve daha sonra yeniden çevrimiçi olduktan durumundadır **güncel**.

- **Çözüm**. Önlemek bir **güncel** veritabanlarınızı hiçbiri 45 gün veya daha fazla bilgi için çevrimdışı olmasını sağlayarak durumu.

  Bir veritabanının durumu ise **güncel**:

  1. Olan veritabanını Kaldır bir **güncel** eşitleme grubu durumu.
  1. Veritabanı ekleme eşitleme grubuna yeniden.

  > [!WARNING]
  > Çevrimdışı durumdayken bu veritabanına yapılan tüm değişiklikleri kaybedersiniz.

### <a name="setup-date2"></a> Bir eşitleme grubu bir "Süresi geçmiş" durumunda

- **Neden**. Tüm saklama süresi 45 gün için uygulanacak bir veya daha fazla değişiklik başarısız olursa, bir eşitleme grubu güncel olmayan hale gelebilir.

- **Çözüm**. Önlemek için bir **güncel** bir eşitleme grubu durumu, Eşitleme işleri düzenli olarak Geçmiş Görüntüleyicisi'nde sonuçları inceleyin. Araştırmak ve uygulamak için başarısız olan değişiklikleri çözün.

  Bir eşitleme grubunun durumu ise **güncel**, eşitleme grubunu silip yeniden oluşturun.

### <a name="setup-delete2"></a> Kaldırma veya Aracısı durduruluyor üç dakika içinde bir eşitleme grubu silinemiyor

Kaldırma veya ilişkili SQL Data Sync istemci Aracısı durduruluyor üç dakika içinde bir eşitleme grubu silinemiyor.

- **Çözüm**.

  1. İlişkili eşitleme aracıları çevrimiçi durumdayken eşitleme grubu Kaldır (önerilir).
  1. Aracı çevrimdışı, ancak yüklenir, şirket içi bilgisayarda çevrimiçi duruma getirin. Olarak görünmesi için aracı durumunu bekleyin **çevrimiçi** SQL Data Sync portalında. Ardından, eşitleme grubunu kaldırın.
  1. Kaldırılıp kaldırılmadığını çünkü aracı çevrimdışıysa:  
    a.  Aracı XML dosyası mevcutsa, dosyayı SQL Data Sync yükleme klasöründen kaldırın.  
    b.  Aracıyı bir şirket içi bilgisayara yükleyin (aynı bilgisayar veya farklı bir bilgisayar olabilir). Ardından, portalda çevrimdışı olarak gösterilen aracı için oluşturulan aracı anahtarını gönderin.  
    c. Eşitleme grubu silmeyi deneyin.

### <a name="setup-restore"></a> Kayıp veya bozuk bir veritabanı geri yükleyebilirim ne olur?

Kayıp veya bozuk bir veritabanı bir yedeklemeden geri yüklerseniz, bir yakınsaması veri veritabanının ait olduğu eşitleme grupları olabilir.

## <a name="next-steps"></a>Sonraki adımlar
SQL Data Sync hakkında daha fazla bilgi için bkz:

-   Genel Bakış - [verileri Eşitle birden fazla Bulut ve şirket içi veritabanı arasında Azure SQL Data Sync ile](sql-database-sync-data.md)
-   Data Sync'i Ayarla
    - Portalda - [Öğreticisi: Azure SQL veritabanı ve SQL Server arasında verileri eşitlemek amacıyla şirket içi SQL Data Sync'i Ayarla](sql-database-get-started-sql-data-sync.md)
    - PowerShell ile
        -  [PowerShell kullanarak birden çok Azure SQL veritabanı arasında eşitleme](scripts/sql-database-sync-data-between-sql-databases.md)
        -  [PowerShell kullanarak bir Azure SQL Veritabanı ile SQL Server şirket içi veritabanı arasında eşitleme](scripts/sql-database-sync-data-between-azure-onprem.md)
-   Veri Eşitleme Aracısı - [veri Aracısı Azure SQL Data Sync için eşitleme](sql-database-data-sync-agent.md)
-   En iyi uygulamalar - [en iyi uygulamalar için Azure SQL Data Sync](sql-database-best-practices-data-sync.md)
-   İzleyici - [SQL Data Sync'i Azure İzleyici ile izleme günlükleri](sql-database-sync-monitor-oms.md)
-   Eşitleme şemasını güncelleştirmek
    -   Transact-SQL ile- [Azure SQL Data Sync şema değişikliklerinin çoğaltmayı otomatik hale getirme](sql-database-update-sync-schema.md)
    -   PowerShell ile- [var olan bir eşitleme grubunda eşitleme şemasını güncelleştirmek için PowerShell kullanma](scripts/sql-database-sync-update-schema.md)

SQL veritabanı hakkında daha fazla bilgi için bkz:

-   [SQL Veritabanı'na Genel Bakış](sql-database-technical-overview.md)
-   [Veritabanı Yaşam Döngüsü Yönetimi](https://msdn.microsoft.com/library/jj907294.aspx)
