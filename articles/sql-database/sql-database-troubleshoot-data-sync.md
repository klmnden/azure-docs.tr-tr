---
title: Azure SQL Data Sync sorunlarını giderme | Microsoft Docs
description: Azure SQL Data Sync ile ilgili yaygın sorunları gidermeyi öğrenin.
services: sql-database
ms.date: 07/16/2018
ms.topic: conceptual
ms.service: sql-database
author: allenwux
ms.author: xiwu
manager: craigg
ms.custom: data-sync
ms.openlocfilehash: 8ba4b32f45dd978439b08650e498c3030c618aab
ms.sourcegitcommit: 35ceadc616f09dd3c88377a7f6f4d068e23cceec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/08/2018
ms.locfileid: "39618718"
---
# <a name="troubleshoot-issues-with-sql-data-sync"></a>SQL Data Sync ile ilgili sorunları giderme

Bu makalede, Azure SQL Data Sync ile ilgili bilinen sorunlar giderilir açıklar. Bir sorun için bir çözüm varsa, burada sağlanır.

SQL Data Sync hizmetine genel bakış için bkz. [Azure SQL Data Sync ile birden fazla bulut ve şirket içi veritabanı arasında veri eşitleme](sql-database-sync-data.md).

## <a name="sync-issues"></a>Eşitleme sorunları

- [İstemci Aracısı ile ilişkili olan şirket içi veritabanları için portal UI eşitleme başarısız](#sync-fails)

- [Eşitleme grubunuza işleme durumda takılmasına neden olabilir](#sync-stuck)

- [Tablolarımın içinde hatalı veri görüyorum](#sync-baddata)

- [Başarılı bir eşitlemeden sonra tutarsız birincil anahtar veri görüyorum](#sync-pkdata)

- [Önemli bir performans düşüşü görüyorum](#sync-perf)

- [Bu iletiyi görüyorum: "sütununa NULL değer eklenemiyor <column>. Sütun null değerlere izin vermiyor." Bunun anlamı nedir ve nasıl düzeltebilirim?](#sync-nulls)

- [Veri eşitleme, döngüsel başvurular nasıl işliyor? Diğer bir deyişle, ne zaman aynı verileri birden çok eşitleme gruplarında eşitlenen ve bunun sonucunda değişir mi?](#sync-circ)

### <a name="sync-fails"></a> İstemci Aracısı ile ilişkili olan şirket içi veritabanları için portal UI eşitleme başarısız

İstemci Aracısı ile ilişkili olan şirket içi veritabanları için SQL Data Sync portalında UI eşitleme başarısız. Aracıyı çalıştıran yerel bilgisayarda System.IO.ıoexception hataları olay günlüğüne bakın. Hataları, diskte yeterli alan olduğunu varsayalım.

- **Neden**. Sürücü yeterli alan yok.

- **Çözüm**. % TEMP % dizininde bulunduğu sürücüde daha fazla alan oluşturun.

### <a name="sync-stuck"></a> Eşitleme grubunuza işleme durumda takılmasına neden olabilir

SQL Data Sync'deki bir eşitleme grubuna uzun bir süre içinde işleme durumu kaldırıldı. Yanıt vermiyor **Durdur** komut ve günlükleri göster yeni giriş yok.

Aşağıdaki koşullardan herhangi biri işleme durumunda takılması bir eşitleme grubu neden olabilir:

- **Neden**. İstemci Aracısı çevrimdışı.

- **Çözüm**. İstemci Aracısı çevrimiçi olduğundan emin olun ve işlemi yeniden deneyin.

- **Neden**. İstemci aracısı yüklü olmayan veya eksik olabilir.

- **Çözüm**. İstemci aracısı yüklü olmayan veya eksik aksi ise:

    1. Dosya varsa SQL Data Sync yükleme klasöründen aracı XML dosyasını kaldırın.
    1. (Bunu aynı veya farklı bir bilgisayar olabilir) bir şirket içi bilgisayara aracıyı yükleyin. Ardından, portalda çevrimdışı olarak gösteren aracı için oluşturulan aracı anahtarı gönderin.

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

### <a name="sync-nulls"></a> Bu iletiyi görüyorum: "sütununa NULL değer eklenemiyor <column>. Sütun null değerlere izin vermiyor." Bunun anlamı nedir ve nasıl düzeltebilirim? 
Bu hata iletisini iki aşağıdaki sorunlardan biri oluştuğunu gösterir:
-  Bir tabloda bir birincil anahtar yok. Bu sorunu gidermek için birincil anahtarı eşitleniyor tüm tabloları ekleyin.
-  CREATE INDEX deyiminde WHERE yan tümcesi yoktur. Veri eşitleme, bu durum işlemiyor. Bu sorunu gidermek için WHERE yan tümcesini kaldırın veya tüm veritabanları için el ile değişiklik. 
 
### <a name="sync-circ"></a> Veri eşitleme, döngüsel başvurular nasıl işliyor? Diğer bir deyişle, ne zaman aynı verileri birden çok eşitleme gruplarında eşitlenen ve bunun sonucunda değişir mi?
Veri eşitleme, döngüsel başvurular işlemiyor. Kaçınma emin olun. 

## <a name="client-agent-issues"></a>İstemci aracı sorunları

- [İstemci aracı yükleme, kaldırma veya onarım başarısız](#agent-install)

- [Ben kaldırma işlemi iptal ettikten sonra istemci Aracısı çalışmıyor](#agent-uninstall)

- [Veritabanım aracı listesinde listelenmiyor](#agent-list)

- [İstemci Aracısı (hata 1069) başlamıyor](#agent-start)

- [Aracı anahtarını Gönder olamaz](#agent-key)

- [İstemci Aracısı, ilişkili şirket içi veritabanı ulaşılamaz durumdaysa portaldan silinemiyor](#agent-delete)

- [Yerel bir eşitleme Aracısı uygulaması yerel eşitleme hizmetine bağlanamıyor](#agent-connect)

### <a name="agent-install"></a> İstemci aracı yükleme, kaldırma veya onarım başarısız

- **Neden**. Birçok senaryo bu hataya neden olabilir. Bu hatanın nedenini belirlemek için günlüklere bakın.

- **Çözüm**. Belirli bir hatanın nedenini bulmak için oluşturmak ve Windows Yükleyici günlüklerine bakın. Bir komut isteminde günlük kaydını etkinleştirebilirsiniz. Örneğin, indirilen AgentServiceSetup.msi dosya LocalAgentHost.msi ise, oluşturun ve aşağıdaki komut satırlarını kullanarak günlük dosyalarını inceleyin:

    -   Yüklemeler için: `msiexec.exe /i SQLDataSyncAgent-Preview-ENU.msi /l\*v LocalAgentSetup.InstallLog`
    -   İçin kaldırır: `msiexec.exe /x SQLDataSyncAgent-se-ENU.msi /l\*v LocalAgentSetup.InstallLog`

    Ayrıca Windows Installer tarafından gerçekleştirilen tüm yüklemeleri için günlük kaydını etkinleştirebilirsiniz. Microsoft Bilgi Bankası makalesi [Windows Installer günlüğe kaydetmenin nasıl etkinleştirileceği](https://support.microsoft.com/help/223300/how-to-enable-windows-installer-logging) Windows Yükleyicisi için günlük kaydını etkinleştirmek için tek tıklamayla çözümü sağlar. Ayrıca, günlüklerinin konumunu sağlar.

### <a name="agent-uninstall"></a> Ben kaldırma işlemi iptal ettikten sonra istemci Aracısı çalışmıyor

İstemci Aracısı, hatta kendi kaldırma iptal edildikten sonra çalışmaz.

- **Neden**. Bu durum, SQL Data Sync istemci Aracısı, kimlik bilgilerini depolamaz kaynaklanır.

- **Çözüm**. Bu iki çözüm de deneyebilirsiniz:

    -   İstemci aracısı için kimlik bilgilerini girmek için Services.msc dosyasını kullanın.
    -   Bu istemci aracısını kaldırın ve yenisini yükleyin. En son istemci Aracısı'ndan yükleyip [İndirme Merkezi](http://go.microsoft.com/fwlink/?linkid=221479).

### <a name="agent-list"></a> Veritabanım aracı listesinde listelenmiyor

Bir eşitleme grubuna mevcut bir SQL Server veritabanını eklemeye çalıştığınızda, veritabanı aracıları listesinde görünmez.

Bu senaryolar, bu soruna neden olabilir:

- **Neden**. Farklı veri merkezlerinde istemci aracısı ve eşitleme grubu var.

- **Çözüm**. İstemci aracısı ve eşitleme grubu aynı veri merkezinde olması gerekir. Bunu ayarlamak için iki seçeneğiniz vardır:

    -   Eşitleme grubu bulunduğu veri merkezine yeni bir aracı oluşturun. Ardından, veritabanı, bu aracı ile kaydedin.
    -   Geçerli eşitleme grubunu silin. Ardından, aracıyı bulunduğu veri merkezinde eşitleme grubunu yeniden oluşturun.

- **Neden**. Veritabanı listesi istemci aracısının geçerli değil.

- **Çözüm**. Durdurun ve ardından istemci Aracısı hizmetini yeniden başlatın.

    Yerel aracı yalnızca aracı anahtarı ilk teslimini ilişkili veritabanlarını listesini indirir. İlişkili veritabanlarında sonraki aracı anahtar gönderimler listesini indirmek değil. Bir aracı taşıma sırasında kayıtlı veritabanları özgün aracı örneğinde gösterme.

### <a name="agent-start"></a> İstemci Aracısı (hata 1069) başlamıyor

SQL Server'ı barındıran bir bilgisayara aracı çalışmadığından emin keşfedin. Aracıyı elle başlatmayı deneyin iletisini gösteren bir iletişim kutusu görürsünüz "hatası 1069: Hizmet, bir oturum açma hatası nedeniyle başlatılamadı."

![Veri Eşitleme 1069 hata iletişim kutusu](media/sql-database-troubleshoot-data-sync/sync-error-1069.png)

- **Neden**. Bu hatanın olası bir nedeni, Aracısı parolanın ve aracıyı oluşturduktan sonra yerel sunucuda parola değiştirildi ' dir.

- **Çözüm**. Aracının parola geçerli sunucu parolanızı güncelleştirin:

  1. SQL Data Sync istemcisi aracı hizmetini bulun.  
    a. Seçin **Başlat**.  
    b. Arama kutusuna **services.msc**.  
    c. Arama sonuçlarında seçin **Hizmetleri**.  
    d. İçinde **Hizmetleri** penceresinde girişine kaydırma **SQL veri eşitleme Aracısı**.  
  1. Sağ **SQL veri eşitleme Aracısı**ve ardından **Durdur**.
  1. Sağ **SQL veri eşitleme Aracısı**ve ardından **özellikleri**.
  1. Üzerinde **SQL veri eşitleme Aracısı Özellikleri**seçin **oturum** sekmesi.
  1. İçinde **parola** kutusuna, parolanızı girin.
  1. İçinde **parolayı onayla** kutusunda, isterse parolanızı tekrar girmelisiniz.
  1. **Uygula**’yı ve sonra **Tamam**’ı seçin.
  1. İçinde **Hizmetleri** penceresinde sağ **SQL veri eşitleme Aracısı** hizmet ve ardından **Başlat**.
  1. Kapat **Hizmetleri** penceresi.

### <a name="agent-key"></a> Aracı anahtarını Gönder olamaz

Oluşturmak veya bir aracıda, aracı yeniden anahtar oluşturma sonra SqlAzureDataSyncAgent uygulama anahtarı göndermeyi deneyin. Gönderim tamamlamak başarısız olur.

![Eşitleme hata iletişim kutusu - aracı anahtarı gönderilemiyor](media/sql-database-troubleshoot-data-sync/sync-error-cant-submit-agent-key.png)

- **Önkoşullar**. Devam etmeden önce şu önkoşulları denetleyin:

  - SQL veri eşitleme Windows hizmeti çalışıyor.

  - SQL veri eşitleme Windows hizmeti için hizmet hesabının ağ erişimi vardır.

  - Yerel Güvenlik Duvarı kuralınız giden 1433 numaralı bağlantı noktaları açıktır.

  - Yerel IP sunucuda veya eşitleme meta verileri veritabanı için veritabanı güvenlik duvarı kuralı eklenir.

- **Neden**. Aracı anahtarı her yerel aracı benzersiz olarak tanımlar. Anahtar iki koşulları karşılaması gerekir:

  -   İstemci aracı anahtarı SQL Data Sync server ve yerel bilgisayar üzerinde aynı olmalıdır.
  -   İstemci aracı anahtarı yalnızca bir kez kullanılabilir.

- **Çözüm**. Aracınızı çalışmıyorsa, bir ya da bu koşulların her ikisinin de karşılanmadı olmasıdır. Yeniden çalışma için aracınızı almak için:

  1. Yeni bir anahtar oluşturun.
  1. Yeni anahtar aracıya uygulayın.

  Yeni anahtar aracısına uygulamak için:

  1. Dosya Gezgini'nde, aracı yükleme dizinine gidin. Varsayılan yükleme dizini şöyledir\\Program dosyaları (x86)\\Microsoft SQL Data Sync.
  1. Bin alt çift tıklayın.
  1. SqlAzureDataSyncAgent uygulamasını açın.
  1. Seçin **aracı anahtarını Gönder**.
  1. Sağlanan alana panonuzdan anahtarını yapıştırın.
  1. **Tamam**’ı seçin.
  1. Programı kapatın.

### <a name="agent-delete"></a> İstemci Aracısı, ilişkili şirket içi veritabanı ulaşılamaz durumdaysa portaldan silinemiyor

Bir SQL Data Sync istemci Aracısı ile kayıtlı bir yerel uç noktası (diğer bir deyişle, bir veritabanı) ulaşılamaz hale gelirse, istemci Aracısı silinemiyor.

- **Neden**. Yerel aracı ulaşılamaz veritabanı hala aracı ile kayıtlı olduğundan silinemiyor. Aracıyı silmeye çalıştığınızda, silme işlemi başarısız veritabanı erişmeye çalışır.

- **Çözüm**. "Zorla Sil" kullanmak ulaşılamaz veritabanı silinemiyor.

> [!NOTE]
> Eşitleme meta veri tablolarını bir "zorla sonra silme", kullanım kalırsa `deprovisioningutil.exe` temizlenmesi.

### <a name="agent-connect"></a> Yerel bir eşitleme Aracısı uygulaması yerel eşitleme hizmetine bağlanamıyor

- **Çözüm**. Aşağıdaki adımları deneyin:

  1. Uygulamadan çıkmak.  
  1. Bileşen Hizmetleri panelini açın.  
    a. Görev çubuğundaki arama kutusuna girin **services.msc**.  
    b. Arama sonuçlarında çift **Hizmetleri**.  
  1. Durdur **SQL Data Sync** hizmeti.
  1. Yeniden **SQL Data Sync** hizmeti.  
  1. Uygulamasını yeniden açın.

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

- **Neden**. Çevrimdışı istemci aracısıdır.

- **Çözüm**. İstemci Aracısı çevrimiçi olduğundan emin olun ve işlemi yeniden deneyin.

- **Neden**. İstemci aracısı yüklü olmayan veya eksik olabilir.

- **Çözüm**. İstemci aracısı yüklü olmayan veya eksik aksi ise:  
    a. Dosya varsa SQL Data Sync yükleme klasöründen aracı XML dosyasını kaldırın.  
    b. (Bunu aynı veya farklı bir bilgisayar olabilir) bir şirket içi bilgisayara aracıyı yükleyin. Ardından, portalda çevrimdışı olarak gösteren aracı için oluşturulan aracı anahtarı gönderin.

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
  1. **Uygula**’yı ve sonra **Tamam**’ı seçin.
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
    a.  Dosya varsa SQL Data Sync yükleme klasöründen aracı XML dosyasını kaldırın.  
    b.  (Bunu aynı veya farklı bir bilgisayar olabilir) bir şirket içi bilgisayara aracıyı yükleyin. Ardından, portalda çevrimdışı olarak gösteren aracı için oluşturulan aracı anahtarı gönderin.  
    c. Eşitleme grubu silmeyi deneyin.

### <a name="setup-restore"></a> Kayıp veya bozuk bir veritabanı geri yükleyebilirim ne olur?

Kayıp veya bozuk bir veritabanı bir yedeklemeden geri yüklerseniz, bir yakınsaması veri veritabanının ait olduğu eşitleme grupları olabilir.

## <a name="next-steps"></a>Sonraki adımlar
SQL Data Sync hakkında daha fazla bilgi için bkz:

-   [Azure SQL Data Sync ile birden fazla bulut ve şirket içi veritabanı arasında veri eşitleme](sql-database-sync-data.md)  
-   [Azure SQL Data Sync’i ayarlama](sql-database-get-started-sql-data-sync.md)  
-   [Azure SQL Data Sync için en iyi yöntemler](sql-database-best-practices-data-sync.md)  
-   [Azure SQL Data Sync’i Log Analytics ile izleme](sql-database-sync-monitor-oms.md)  
-   SQL Data Sync’in nasıl yapılandırılacağını gösteren tam PowerShell örnekleri:  
    -   [PowerShell kullanarak birden çok Azure SQL veritabanı arasında eşitleme](scripts/sql-database-sync-data-between-sql-databases.md)  
    -   [PowerShell kullanarak bir Azure SQL Veritabanı ile SQL Server şirket içi veritabanı arasında eşitleme](scripts/sql-database-sync-data-between-azure-onprem.md)  

SQL veritabanı hakkında daha fazla bilgi için bkz:

-   [SQL Veritabanı'na Genel Bakış](sql-database-technical-overview.md)
-   [Veritabanı Yaşam Döngüsü Yönetimi](https://msdn.microsoft.com/library/jj907294.aspx)
