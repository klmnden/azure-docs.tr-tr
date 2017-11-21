---
title: "Azure SQL veri eşitleme (Önizleme) sorunlarını giderme | Microsoft Docs"
description: "Azure SQL veri eşitleme (Önizleme) ile ilgili genel sorunları gidermek öğrenin."
services: sql-database
ms.date: 11/13/2017
ms.topic: article
ms.service: sql-database
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 50cabbaa584671e52c1ea7efbd2ad990b8438272
ms.sourcegitcommit: 1d8612a3c08dc633664ed4fb7c65807608a9ee20
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2017
---
# <a name="troubleshoot-issues-with-sql-data-sync-preview"></a>SQL veri eşitleme (Önizleme) ile ilgili sorunları giderme

Bu makalede, Azure SQL veri eşitleme (Önizleme) ile ilgili bilinen sorunlar giderilir açıklar. Bir sorun için bir çözüm varsa, burada sağlanır.

SQL veri eşitleme (Önizleme) genel bakış için bkz: [verileri Eşitle birden çok Bulut ve şirket içi veritabanları arasında Azure SQL veri eşitleme (Önizleme) ile](sql-database-sync-data.md).

## <a name="sync-issues"></a>Eşitleme sorunları

### <a name="sync-fails-in-the-portal-ui-for-on-premises-databases-that-are-associated-with-the-client-agent"></a>UI portalında istemci aracı ile ilişkili olan şirket içi veritabanlarını eşitleme başarısız

#### <a name="description-and-symptoms"></a>Açıklama ve belirtileri

Eşitleme Aracı ile ilişkili şirket içi veritabanları için SQL veri eşitleme (Önizleme) Portal UI başarısız olur. Aracıyı çalıştıran yerel bilgisayarda System.IO.ıoexception hataları olay günlüğüne bakın. Hataları diskte yeterli alan olduğunu varsayalım.

#### <a name="resolution"></a>Çözüm

% TEMP % dizininde bulunur sürücüde daha fazla alan oluşturun.

### <a name="my-sync-group-is-stuck-in-the-processing-state"></a>Eşitleme grubum işleme durumunda takıldı

#### <a name="description-and-symptoms"></a>Açıklama ve belirtileri

SQL veri eşitleme (Önizleme) eşitleme grubunda işleme durumda uzun bir süredir olmuştur. Yanıt vermiyor **durdurmak** komutu ve günlüklerini göster yeni girdi yok.

#### <a name="cause"></a>Nedeni

Aşağıdaki koşullardan herhangi biri bir eşitleme grubu işleme durumunda kalmış neden olabilir:

-   **İstemci aracısıdır çevrimdışı**. İstemci Aracısı çevrimiçi olduğundan emin olun ve işlemi yeniden deneyin.

-   **İstemci aracısı yüklü olmayan veya eksik**. İstemci Aracısı yüklenmemiş veya başka türlü eksik ise:

    1. Dosya varsa agent XML dosyası SQL veri eşitleme (Önizleme) yükleme klasöründen kaldırın.
    2. (Bu aynı veya farklı bir bilgisayar olabilir) bir şirket içi bilgisayara aracıyı yükleyin. Sonra çevrimdışı olarak gösteren aracı için portalda oluşturulan aracı anahtarını Gönder.

-   **SQL veri eşitleme hizmeti durduruldu**.

    1. İçinde **Başlat** menüsü, arama **Hizmetleri**.
    2. Arama sonuçlarında seçin **Hizmetleri**.
    3. Bul **SQL veri eşitleme (Önizleme)** hizmet.
    4. Hizmet durumu ise **durduruldu**, hizmet adını sağ tıklatın ve ardından **Başlat**.

#### <a name="resolution"></a>Çözüm

Yukarıdaki bilgiler, eşitleme grubu işleme durumdan taşınmaz Microsoft Support eşitleme grubunuzun durumunu sıfırlayabilirsiniz. Eşitleme grubu durumu sağlamak için buna Sıfırla [Azure SQL veritabanı Forumu](https://social.msdn.microsoft.com/Forums/azure/home?forum=ssdsgetstarted), posta oluştur. Postasına abonelik Kimliğinizi ve sıfırlanması gerekir grubunun eşitleme Grup Kimliğini içerir. Microsoft Support mühendisi postanızı için yanıtlar ve bunu durumu ne zaman sıfırladı size bildirir.

### <a name="i-see-erroneous-data-in-my-tables"></a>Hatalı veri my tablolarda görüyorum

#### <a name="description-and-symptoms"></a>Açıklama ve belirtileri

Aynı ada sahip ancak farklı veritabanı şemalarını olan tabloları eşitleme dahil edilirse, eşitlemeden sonra hatalı veri tabloları konusuna bakın.

#### <a name="cause"></a>Nedeni

SQL veri eşitleme (Önizleme) sağlama işlemi aynı izleme tablosu, aynı ada sahip ancak farklı şemalarda olan tablolar için kullanır. Bu nedenle, her iki tabloyu değişikliklerden aynı izleme tabloya yansıtılır. Bu, eşitleme sırasında hatalı veri değişiklikleri neden olur.

#### <a name="resolution"></a>Çözüm

Bir veritabanı farklı şemalarda tabloları ait olsa bile bir eşitleme oynayan tabloların adlarını farklı olduğundan emin olun.

### <a name="i-see-inconsistent-primary-key-data-after-a-successful-sync"></a>Başarılı bir eşitlemeden sonra tutarsız birincil anahtar veri görüyorum

#### <a name="description-and-symptoms"></a>Açıklama ve belirtileri

Bir eşitleme başarılı olarak bildirilir ve başarısız veya Atlanan satır günlüğünü gösterir, ancak birincil anahtar veri eşitleme grubunu veritabanları arasında tutarsız olup olmadığına bakın.

#### <a name="cause"></a>Nedeni

Bu sonuç tasarım gereğidir. Burada kullanılan birincil anahtarı değiştirildi satırlardaki tutarsız veriler, herhangi bir birincil anahtar sütunu değişiklikleri sonuçlanır.

#### <a name="resolution"></a>Çözüm

Bu sorunu önlemek için birincil anahtar sütunu yok verilerde değiştiğini emin olun.

Bu gerçekleştikten sonra bu sorunu gidermek için tüm uç noktaları tutarsız verileri eşitleme grubunda bulunan satır silin. Ardından, satır takın.

### <a name="i-see-a-significant-degradation-in-performance"></a>Önemli bir performans düşüşü bakın

#### <a name="description-and-symptoms"></a>Açıklama ve belirtileri

Performansınızı büyük olasılıkla önemli ölçüde, burada bile veri eşitleme UI açılamıyor noktasına düşürür.

#### <a name="cause"></a>Nedeni

En olası nedeni bir eşitleme döngüsü oluşturur. Bir eşitleme tarafından eşitleme grubu A'daki tetikler eşitleme tarafından eşitleme grubu B, eşitleme tarafından eşitleme grubu A tetikleyen bir eşitleme döngüsü oluşur. Fiili durumu daha karmaşık olabilir ve birden fazla iki eşitleme grubu döngüsünde gerektirebilir. Sorun bir döngüsel eşitleme tetikleme olan bir diğer çakışan eşitleme grubu tarafından neden olur.

#### <a name="resolution"></a>Çözüm

En iyi düzeltme önleme bulunur. Eşitleme gruplarınızı döngüsel başvurulara olmadığından emin olun. Bir eşitleme grubu tarafından eşitlenen herhangi bir satırın başka bir eşitleme grubu tarafından eşitlenemiyor.

### <a name="i-see-this-message-cannot-insert-the-value-null-into-the-column-column-column-does-not-allow-nulls-what-does-this-mean-and-how-can-i-fix-it"></a>Şu mesajı görürsünüz: "sütununa NULL değer eklenemiyor \<sütun\>. Sütun null değerlere izin vermiyor." Bu ne anlama geliyor ve nasıl ı düzeltebilirsiniz? 
Bu hata iletisini iki aşağıdaki sorunlardan biriyle oluştuğunu gösterir:
-  Bir tablonun birincil anahtarı yok. Bu sorunu gidermek için birincil anahtarı eşitleniyor tüm tablolar ekleyin.
-  CREATE INDEX deyiminde WHERE yan tümcesi yok. Veri Eşitleme (Önizleme), bu durum işleyemez. Bu sorunu gidermek için WHERE yan tümcesini kaldırın veya tüm veritabanları için el ile değişiklik. 
 
### <a name="how-does-data-sync-preview-handle-circular-references-that-is-when-the-same-data-is-synced-in-multiple-sync-groups-and-keeps-changing-as-a-result"></a>Veri Eşitleme (Önizleme) döngüsel başvurulara nasıl işler? Diğer bir deyişle, ne zaman aynı verileri birden çok eşitleme gruplarında eşitlenen ve sonuç olarak değiştirerek korur?
Veri Eşitleme (Önizleme) döngüsel başvurulara işleyemez. Bunları önlemek emin olun. 

## <a name="client-agent-issues"></a>İstemci aracı sorunları

### <a name="the-client-agent-install-uninstall-or-repair-fails"></a>İstemci aracısını yüklemek, kaldırmak veya onarım başarısız

#### <a name="cause"></a>Nedeni

Birçok senaryo bu hataya neden olabilir. Bu hatanın belirli nedenini belirlemek için günlüklerine bakın.

#### <a name="resolution"></a>Çözüm

Belirli başarısızlığın nedenini bulmak için oluşturmak ve Windows Installer günlüklerine bakın. Bir komut isteminde günlük kaydını etkinleştirebilirsiniz. Örneğin, indirilen AgentServiceSetup.msi dosya LocalAgentHost.msi ise, oluşturun ve aşağıdaki komut satırlarından kullanarak günlük dosyalarını inceleyin:

-   Yüklemeler için:`msiexec.exe /i SQLDataSyncAgent-Preview-ENU.msi /l\*v LocalAgentSetup.InstallLog`
-   İçin kaldırır:`msiexec.exe /x SQLDataSyncAgent-se-ENU.msi /l\*v LocalAgentSetup.InstallLog`

Windows Installer tarafından gerçekleştirilen tüm yüklemeleri için günlüğe kaydetmeyi kapatabilirsiniz. Microsoft Bilgi Bankası makalesi [Windows Installer günlüğünün nasıl etkinleştirileceği](https://support.microsoft.com/help/223300/how-to-enable-windows-installer-logging) Windows Installer için günlük kaydını etkinleştirmek için tek tıklamayla çözümü sağlar. Ayrıca, günlükleri konumunu sağlar.

### <a name="my-client-agent-doesnt-work"></a>My istemci Aracısı çalışmıyor

#### <a name="description-and-symptoms"></a>Açıklama ve belirtileri

İstemci Aracısı'nı kullanmaya çalıştığınızda aşağıdaki iletiler Al:

"Parametresi www.microsoft.com/.../05:GetBatchInfoResult serisi kaldırılmaya çalışılırken bir hata oluştu durumla eşitleme başarısız oldu. InnerException daha fazla ayrıntı için bkz."

"İç özel durum iletisi: 'Microsoft.Synchronization.ChangeBatch' türünde geçersiz koleksiyon türü varsayılan bir oluşturucu olmadığından."

#### <a name="cause"></a>Nedeni

Bu, SQL veri eşitleme (Önizleme) yüklemesine bilinen bir sorundur. Bu iletinin en olası nedeni aşağıdakilerden biridir:

-   Windows 8 Geliştirici önizlemesi çalışıyor.
-   .NET Framework 4.5 yüklü olması.

#### <a name="resolution"></a>Çözüm

İstemci Aracısı Windows 8 Geliştirici önizlemesi çalışmayan bir bilgisayara yükleyin ve .NET Framework 4.5 yüklü değil emin olun.

### <a name="my-client-agent-doesnt-work-after-i-cancel-the-uninstall"></a>Kaldırma işlemi iptal sonra my istemci Aracısı çalışmıyor

#### <a name="description-and-symptoms"></a>Açıklama ve belirtileri

Hatta, kaldırma işlemi iptal edildikten sonra istemci Aracısı çalışmıyor.

#### <a name="cause"></a>Nedeni

Bu durum, SQL veri eşitleme (Önizleme) istemci Aracısı kimlik bilgilerini depolamak değil kaynaklanır.

#### <a name="resolution"></a>Çözüm

Bu iki çözümleri deneyebilirsiniz:

-   İstemci aracısı için kimlik bilgilerini girmek için Services.msc kullanın.
-   Bu istemci aracısının kaldırın ve yenisini yükleyin. Son istemci Aracısı'ndan yükleyip [Yükleme Merkezi'nden](http://go.microsoft.com/fwlink/?linkid=221479).

### <a name="my-database-isnt-listed-in-the-agent-list"></a>Veritabanım Aracısı listesinde değil

#### <a name="description-and-symptoms"></a>Açıklama ve belirtileri

Varolan bir SQL Server veritabanını bir eşitleme grubuna eklemeye çalıştığınızda, veritabanı aracıları listesinde görünmüyor.

#### <a name="cause"></a>Nedeni

Bu senaryolar, bu soruna neden:

-   İstemci aracısı ve eşitleme grubu olan farklı veri merkezlerinde.
-   İstemci aracısının veritabanlarının listesini geçerli değil.

#### <a name="resolution"></a>Çözüm

Çözünürlük neden bağlıdır.

- **İstemci aracısı ve eşitleme grubu olan farklı veri merkezlerinde**

    İstemci aracısı ve eşitleme grubu aynı veri merkezinde olmalıdır. Bunu ayarlamak için iki seçeneğiniz vardır:

    -   Yeni bir aracı eşitleme grubunu bulunduğu veri merkezinde oluşturun. Ardından, veritabanını bu aracı ile kaydedin.
    -   Geçerli eşitleme grubunu silin. Ardından, aracıyı bulunduğu veri merkezinde eşitleme grubunu yeniden oluşturun.

- **İstemci aracısının veritabanlarının listesini geçerli değil**

    Ardından istemci Aracısı hizmetini durdurup yeniden başlatın.

    Yerel aracı yalnızca ilk aracı anahtarı teslimini ilişkili veritabanlarını listesini yükler. Sonraki Aracısı anahtar gönderimlerini ilişkili veritabanlarını listesi indirilmedi. Bir aracı taşıma işlemi sırasında kayıtlı veritabanları özgün Aracısı örneğinde gösterme.

### <a name="client-agent-doesnt-start-error-1069"></a>İstemci Aracısı (hata 1069) başlamıyor

#### <a name="description-and-symptoms"></a>Açıklama ve belirtileri

Aracının, SQL Server barındıran bir bilgisayara çalışmadığından bulur. Aracıyı el ile başlatmak denediğinizde, ileti görüntüleyen bir iletişim kutusu görebilirsiniz "hata 1069: hizmeti oturum açma hatası nedeniyle başlatılamadı."

![Veri eşitleme hatası 1069 iletişim kutusu](media/sql-database-troubleshoot-data-sync/sync-error-1069.png)

#### <a name="cause"></a>Nedeni

Bu hatanın olası bir nedeni, aracı ve aracı parolası oluşturulmuş olduğundan yerel sunucuda parolanın değiştirilmesi ' dir.

#### <a name="resolution"></a>Çözüm

Aracının parola geçerli sunucu parolanızı güncelleştirin:

1. SQL veri eşitleme (Önizleme) istemci Aracısı Önizleme hizmeti bulun.  
    a. Seçin **Başlat**.  
    b. Arama kutusuna **services.msc**.  
    c. Arama sonuçlarında seçin **Hizmetleri**.  
    d. İçinde **Hizmetleri** penceresinde, girişine kaydırma **SQL veri eşitleme (Önizleme) aracısı Önizleme**.  
2. Sağ **SQL veri eşitleme (Önizleme) aracısı Önizleme**ve ardından **durdurmak**.
3. Sağ **SQL veri eşitleme (Önizleme) aracısı Önizleme**ve ardından **özellikleri**.
4. Üzerinde **SQL veri eşitleme (Önizleme) aracısı Önizleme özellikleri**seçin **oturum** sekmesi.
5. İçinde **parola** kutusuna, parolanızı girin.
6. İçinde **parolayı onayla** kutusunda, parolanızı yeniden girin.
7. Seçin **Uygula**ve ardından **Tamam**.
8. İçinde **Hizmetleri** penceresinde sağ **SQL veri eşitleme (Önizleme) aracısı Önizleme** hizmet ve ardından **Başlat**.
9. Kapat **Hizmetleri** penceresi.

### <a name="i-cant-submit-the-agent-key"></a>Aracı anahtarını Gönder olamaz

#### <a name="description-and-symptoms"></a>Açıklama ve belirtileri

Oluşturun veya bir anahtar oluşturmak için bir aracı yeniden sonra SqlAzureDataSyncAgent uygulama tuşuyla gönderme deneyin. Tamamlamak Gönderim başarısız.

![Eşitleme hata iletişim kutusu - aracı anahtarını gönderemiyor](media/sql-database-troubleshoot-data-sync/sync-error-cant-submit-agent-key.png)

Devam etmeden önce aşağıdaki koşulları denetleyin:

-   SQL veri eşitleme (Önizleme) Windows hizmeti çalışıyor.  
-   SQL veri eşitleme (Önizleme) önizleme Windows hizmeti için hizmet hesabı ağ erişimi vardır.    
-   İstemci Aracısı Konumlandırıcı Hizmeti başvurabilirsiniz. Aşağıdaki kayıt defteri anahtarının değeri https://locator.sync.azure.com/LocatorServiceApi.svc sahip olup olmadığını denetleyin:  
    -   Bir x86 üzerinde bilgisayar:`HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Microsoft\\SQL Azure Data Sync\\LOCATORSVCURI`  
    -   X x64 üzerinde bilgisayar:`HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Wow6432Node\\Microsoft\\SQL Azure Data Sync\\LOCATORSVCURI`

#### <a name="cause"></a>Nedeni

Aracı anahtarını her yerel aracı benzersiz olarak tanımlar. Anahtar iki koşullara uyması gerekir:

-   SQL veri eşitleme (Önizleme) sunucusu ve yerel bilgisayarda istemci Aracısı anahtar aynı olmalıdır.
-   İstemci Aracısı anahtar yalnızca bir kez kullanılabilir.

#### <a name="resolution"></a>Çözüm

Aracınızı çalışmıyorsa, bir ya da bu koşulların her ikisi de karşılanmayan nedeni. Yeniden çalışma için aracınızı almak için:

1. Yeni bir anahtar oluşturun.
2. Yeni anahtar aracıya uygulayın.

Yeni anahtar aracıya uygulamak için:

1. Dosya Gezgini'nde, aracı yükleme dizinine gidin. Varsayılan yükleme dizini C: olduğu\\Program Files (x86)\\Microsoft SQL veri eşitleme.
2. Bin alt çift tıklayın.
3. SqlAzureDataSyncAgent uygulamasını açın.
4. Seçin **aracı anahtarını Gönder**.
5. Sağlanan alana panonuzdan anahtarını yapıştırın.
6. **Tamam**’ı seçin.
7. Programı kapatın.

### <a name="the-client-agent-cant-be-deleted-from-the-portal-if-its-associated-on-premises-database-is-unreachable"></a>İstemci Aracısı içi ilişkili veritabanını erişilemediğinde portaldan silinemez.

#### <a name="description-and-symptoms"></a>Açıklama ve belirtileri

SQL veri eşitleme (Önizleme) istemci Aracısı ile kayıtlı bir yerel uç noktası (diğer bir deyişle, bir veritabanı) ulaşılamaz hale gelirse, istemci Aracısı silinemiyor.

#### <a name="cause"></a>Nedeni

Yerel aracı ulaşılamaz veritabanı aracı ile kayıtlı olduğu için silinemiyor. Aracı silmeye çalıştığınızda, silme işlemi başarısız veritabanı ulaşmaya çalışır.

#### <a name="resolution"></a>Çözüm

"Sil zorla" kullanmak ulaşılamaz veritabanı silinemiyor.

> [!NOTE]
> Eşitleme meta veri tablolarına bir "zorla sonra Sil" kalırsa, bunları temizlemek için deprovisioningutil.exe kullanın.

### <a name="local-sync-agent-app-cant-connect-to-the-local-sync-service"></a>Yerel eşitleme Aracısı uygulama yerel eşitleme hizmetine bağlanamıyor

#### <a name="resolution"></a>Çözüm

Aşağıdaki adımları deneyin:

1. Uygulama çıkın.  
2. Bileşen Hizmetleri panelini açın.  
    a. Görev çubuğunda arama kutusuna girin **services.msc**.  
    b. Arama sonuçlarında çift **Hizmetleri**.  
3. Durdur **SQL veri eşitleme (Önizleme) Önizleme** hizmet.
4. Yeniden **SQL veri eşitleme (Önizleme) Önizleme** hizmet.  
5. Uygulamasını yeniden açın.

## <a name="setup-and-maintenance-issues"></a>Kurulum ve Bakım konuları

### <a name="i-get-a-disk-out-of-space-message"></a>"Disk alanı yetersiz" iletisi alıyorum

#### <a name="cause"></a>Nedeni

Kalan dosyaları silinmesi gerekiyorsa, "disk alanı yetersiz" iletisi görüntülenebilir. Bu virüsten koruma yazılımı tarafından kaynaklanabilir veya dosyaları açık olduğunda silme işlemleri çalıştı.

#### <a name="resolution"></a>Çözüm

% Temp % klasöründe eşitleme dosyaları el ile silin (`del \*sync\* /s`). Ardından, % temp % klasöründe alt dizinleri silin.

> [!IMPORTANT]
> Eşitleme işlemi devam ederken hiçbir dosya silmeyin.

### <a name="i-cant-delete-my-sync-group"></a>Eşitleme grubum silemiyorum

#### <a name="description-and-symptoms"></a>Açıklama ve belirtileri

Eşitleme grubu silme denemesi başarısız olur.

#### <a name="causes"></a>Neden olur.

Aşağıdaki senaryoların herhangi biri, bir eşitleme grubunu silmek için hatasına neden olabilir:

-   Çevrimdışı istemci aracısıdır.
-   İstemci aracısı yüklü olmayan veya eksik olabilir. 
-   Bir veritabanı çevrimdışıdır. 
-   Eşitleme grubunu sağlama veya eşitleniyor. 

#### <a name="resolution"></a>Çözüm

Eşitleme grubu silme hatası çözmek için:

-   İstemci Aracısı çevrimiçi olduğundan emin olun ve işlemi yeniden deneyin.
-   İstemci Aracısı yüklenmemiş veya başka türlü eksik ise:  
    a. Dosya varsa agent XML dosyası SQL veri eşitleme (Önizleme) yükleme klasöründen kaldırın.  
    b. (Bu aynı veya farklı bir bilgisayar olabilir) bir şirket içi bilgisayara aracıyı yükleyin. Sonra çevrimdışı olarak gösteren aracı için portalda oluşturulan aracı anahtarını Gönder.
-   SQL veri eşitleme (Önizleme) hizmetinin çalıştığından emin olun:  
    a. İçinde **Başlat** menüsü, arama **Hizmetleri**.  
    b. Arama sonuçlarında seçin **Hizmetleri**.  
    c. Bul **SQL veri eşitleme (Önizleme) Önizleme** hizmet.  
    d. Hizmet durumu ise **durduruldu**, hizmet adını sağ tıklatın ve ardından **Başlat**.
-   SQL veritabanları ve SQL Server veritabanları tüm çevrimiçi olduğundan emin olun.
-   Sağlama veya eşitleme işlemi tamamlanana kadar bekleyin ve sonra eşitleme grubunu silme işlemini yeniden deneyin.

### <a name="i-cant-unregister-an-on-premises-sql-server-database"></a>Bir şirket içi SQL Server veritabanı kaydı silinemiyor

#### <a name="cause"></a>Nedeni

Büyük olasılıkla zaten silinmiş bir veritabanı kaydını deniyorsunuz.

#### <a name="resolution"></a>Çözüm

Bir şirket içi SQL Server veritabanı kaydını kaldırmak için veritabanını seçin ve ardından **zorla silme**.

Veritabanı eşitleme gruptan kaldırmak bu işlem başarısız olursa:

1. Durdurun ve istemci Aracısı ana bilgisayar hizmeti yeniden başlatın:  
    a. Seçin **Başlat** menüsü.  
    b. Arama kutusuna **services.msc**.  
    c. İçinde **programları** bölüm arama sonuçları bölmesinde, çift **Hizmetleri**.  
    d. Sağ **SQL veri eşitleme (Önizleme)** hizmet.  
    e. Hizmeti çalışıyorsa durdurun.  
    f. Hizmeti sağ tıklatın ve ardından **Başlat**.  
    g. Veritabanı hala kayıtlı olup olmadığını denetleyin. Artık kayıtlı değilse, bitirdiniz. Aksi halde, sonraki adımla devam edin.
2. İstemci Aracısı uygulamasını (SqlAzureDataSyncAgent) açın.
3. Seçin **kimlik bilgilerini Düzenle**ve ardından veritabanı için kimlik bilgilerini girin.
4. Kayıt kaldırma ile devam edin.

### <a name="i-dont-have-sufficient-privileges-to-start-system-services"></a>Sistem hizmetlerini başlatmak için yeterli izne sahip değilsiniz

#### <a name="cause"></a>Nedeni

Bu hata, iki durumlarda oluşur:
-   Kullanıcı adı ve/veya parolası yanlış.
-   Belirtilen kullanıcı hesabı, bir hizmet olarak oturum açmak için yeterli ayrıcalıklara sahip değil.

#### <a name="resolution"></a>Çözüm

Kullanıcı hesabı için günlük üzerinde-a-hizmet olarak kimlik bilgileri verin:

1. Git **Başlat** > **Denetim Masası** > **Yönetimsel Araçlar** > **yerel güvenlik ilkesi**  >  **Yerel ilke** > **kullanıcı Rights Management**.
2. Seçin **hizmet oturum açma**.
3. İçinde **özellikleri** iletişim kutusunda, kullanıcı hesabını ekleyin.
4. Seçin **Uygula**ve ardından **Tamam**.
5. Tüm pencereleri kapatın.

### <a name="a-database-has-an-out-of-date-status"></a>Bir veritabanı "Süresi geçmiş" durumunda

#### <a name="cause"></a>Nedeni

SQL veri eşitleme (Önizleme) 45 gün veya daha fazla (veritabanı çevrimdışı olduğu zamandan sayılan gibi) hizmetinden çevrimdışı veritabanları kaldırır. Bir veritabanı 45 gün veya daha fazla bilgi için çevrimdışı ise ve tekrar çevrimiçi duruma durumundadır **güncel**.

#### <a name="resolution"></a>Çözüm

Önlemek bir **güncel** veritabanlarınızı hiçbiri 45 gün veya daha fazla bilgi için çevrimdışı duruma sağlayarak durumu.

Bir veritabanının durumu ise **güncel**:

1. Olan veritabanını Kaldır bir **güncel** eşitleme grubu durumu.
2. Veritabanı Ekle eşitleme grubuna yedekleyin.

> [!WARNING]
> Çevrimdışı durumdayken bu veritabanına yapılan tüm değişiklikler kaybedilir.

### <a name="a-sync-group-has-an-out-of-date-status"></a>Eşitleme grubu "Süresi geçmiş" durumunda

#### <a name="cause"></a>Nedeni

45 gün tüm saklama dönemi için uygulanacak bir veya daha fazla değişiklik başarısız olursa, bir eşitleme grubu eski haline gelebilir.

#### <a name="resolution"></a>Çözüm

Önlemek için bir **güncel** bir eşitleme grubu durumu, Eşitleme işleri düzenli olarak Geçmiş Görüntüleyicisi'nde sonuçlarını inceleyin. Araştırmak ve değişiklikleri uygulamak için başarısız çözümleyin.

Bir eşitleme grubun durumundaysa **güncel**, eşitleme grubunu silip yeniden oluşturun.

### <a name="a-sync-group-cant-be-deleted-within-three-minutes-of-uninstalling-or-stopping-the-agent"></a>Eşitleme grubunu kaldırma veya aracısını durdurma üç dakika içinde silinemez.

#### <a name="description-and-symptoms"></a>Açıklama ve belirtileri

Üç kaldırma veya ilişkili SQL veri eşitleme (Önizleme) istemci aracısını durdurma dakika içinde bir eşitleme grubu silinemiyor.

#### <a name="resolution"></a>Çözüm

1. İlişkili eşitleme aracıları çevrimiçi durumdayken bir eşitleme grubunu kaldırmak (önerilen).
2. Aracı çevrimdışı ancak yüklü değilse, şirket içi bilgisayarda çevrimiçi duruma getirin. Olarak görünmesi için aracının durumunu bekleyin **çevrimiçi** SQL veri eşitleme (Önizleme) Portalı'nda. Ardından, eşitleme grubunu kaldırın.
3. Bunu kaldırıldığından aracı çevrimdışıysa:  
    a.  Dosya varsa agent XML dosyası SQL veri eşitleme (Önizleme) yükleme klasöründen kaldırın.  
    b.  (Bu aynı veya farklı bir bilgisayar olabilir) bir şirket içi bilgisayara aracıyı yükleyin. Sonra çevrimdışı olarak gösteren aracı için portalda oluşturulan aracı anahtarını Gönder.  
    c. Eşitleme grubunu silmeyi deneyin.

### <a name="what-happens-when-i-restore-a-lost-or-corrupted-database"></a>Kayıp veya bozuk bir veritabanı geri ne olur?

Kayıp veya bozuk bir veritabanını bir yedekten geri nonconvergence veritabanının ait olduğu eşitleme grubu veri olabilir.

## <a name="next-steps"></a>Sonraki adımlar
SQL veri eşitleme (Önizleme) hakkında daha fazla bilgi için bkz:

-   [Eşitleme verilerle birden çok Bulut ve şirket içi veritabanları arasında Azure SQL veri eşitleme (Önizleme)](sql-database-sync-data.md)  
-   [Azure SQL veri eşitleme (Önizleme) ayarı](sql-database-get-started-sql-data-sync.md)  
-   [Azure SQL veri eşitleme (Önizleme) için en iyi yöntemler](sql-database-best-practices-data-sync.md)  
-   [OMS günlük analizi ile İzleyici Azure SQL veri eşitleme (Önizleme)](sql-database-sync-monitor-oms.md)  
-   SQL veri eşitleme (Önizleme) yapılandırma Göster PowerShell örnekleri tamamlayın:  
    -   [Birden çok Azure SQL veritabanları arasında eşitlemek için PowerShell kullanma](scripts/sql-database-sync-data-between-sql-databases.md)  
    -   [Bir Azure SQL Database ve SQL Server içi veritabanı arasında eşitlemek için PowerShell kullanma](scripts/sql-database-sync-data-between-azure-onprem.md)  
-   [SQL veri eşitleme (Önizleme) REST API belgelerini indirebilirsiniz](https://github.com/Microsoft/sql-server-samples/raw/master/samples/features/sql-data-sync/Data_Sync_Preview_REST_API.pdf?raw=true)

SQL veritabanı hakkında daha fazla bilgi için bkz:

-   [SQL veritabanı genel bakış](sql-database-technical-overview.md)
-   [Veritabanı yaşam döngüsü yönetimi](https://msdn.microsoft.com/library/jj907294.aspx)
