---
title: "Azure SQL veri eşitleme sorunlarını giderme | Microsoft Docs"
description: "Azure SQL veri eşitleme ile ilgili genel sorunları gidermek bilgi edinin"
services: sql-database
ms.date: 11/2/2017
ms.topic: article
ms.service: sql-database
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 29fb935ada4aa7f2571b128f82d4c037bbb88eb1
ms.sourcegitcommit: dcf5f175454a5a6a26965482965ae1f2bf6dca0a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
# <a name="troubleshoot-issues-with-azure-sql-data-sync-preview"></a>Azure SQL veri eşitleme (Önizleme) ile ilgili sorunları giderme

Bu makalede SQL veri eşitleme (Önizleme) ekibin bilinen geçerli sorunlarının nasıl giderileceği açıklanmaktadır. Bir sorun için geçici bir çözüm varsa, burada sağlanır.

SQL veri eşitleme genel bakış için bkz: [verileri Eşitle birden çok Bulut ve şirket içi veritabanları arasında Azure SQL veri eşitleme (Önizleme) ile](sql-database-sync-data.md).
                                                           
## <a name="my-client-agent-doesnt-work"></a>My istemci Aracısı çalışmıyor

### <a name="description-and-symptoms"></a>Açıklama ve belirtileri

İstemci Aracısı'nı kullanmaya çalıştığınızda aşağıdaki hata iletilerinden alın.

"Parametresi www.microsoft.com/.../05:GetBatchInfoResult serisi kaldırılmaya çalışılırken bir hata oluştu durumla eşitleme başarısız oldu. Ayrıntılar için InnerException'a bakın.

"İç özel durum iletisi: 'Microsoft.Synchronization.ChangeBatch' türünde geçersiz koleksiyon türü varsayılan bir oluşturucu olmadığından."


### <a name="cause"></a>Nedeni

Bu hata SQL veri eşitleme (Önizleme) ile ilgili bir sorun var.

Bu sorunun en olası nedeni:

-   Windows 8 Geliştirici önizlemesi, kullanmakta olduğunuz veya

-   .NET 4. 5'in yüklü olması.

### <a name="solution-or-workaround"></a>Çözüm ya da geçici çözüm

İstemci Aracısı Windows 8 Geliştirici önizlemesi çalıştırmayan bir bilgisayara yükleyin ve .NET Framework 4.5 yüklenmediğinden emin olun.

## <a name="my-client-agent-doesnt-work-after-i-cancel-the-uninstall"></a>Kaldırma işlemi iptal sonra my istemci Aracısı çalışmıyor

### <a name="description-and-symptoms"></a>Açıklama ve belirtileri

Kendi kaldırma işlemi iptal olsa bile istemci Aracısı çalışmıyor.

### <a name="cause"></a>Nedeni

SQL veri eşitleme (Önizleme) istemci Aracısı kimlik bilgilerini depolamaz çünkü bu sorun oluşur.

### <a name="solution-or-workaround"></a>Çözüm ya da geçici çözüm

Denemek için iki çözümü vardır:

-   İlk olarak, istemci aracısı için kimlik bilgilerinizi yeniden girmek için services.msc kullanın.

-   İkinci olarak, bu istemci aracısının kaldırın ve yenisini yükleyin. Son istemci Aracısı'ndan yükleyip [Yükleme Merkezi'nden](http://go.microsoft.com/fwlink/?linkid=221479).

## <a name="my-database-isnt-listed-in-the-agent-dropdown"></a>Veritabanım aracı açılır listede değil

### <a name="description-and-symptoms"></a>Açıklama ve belirtileri

Varolan bir SQL Server veritabanını bir eşitleme grubuna eklemeye çalıştığınızda, veritabanı açılır menüde listelenmez.

### <a name="cause"></a>Nedeni

Bu sorunun olası nedenleri şunlardır:

-   İstemci aracısı ve eşitleme grubu içinde farklı veri merkezlerine olan.

-   İstemci aracısının veritabanlarının listesini geçerli değil.

### <a name="solution"></a>Çözüm

Çözüm nedenini bağlıdır.

#### <a name="the-client-agent-and-sync-group-are-in-different-data-centers"></a>İstemci aracısı ve eşitleme grubu olan farklı veri merkezleri

Aynı veri merkezinde hem istemci Aracısı hem de eşitleme grubu olması gerekir. Aşağıdakilerden birini yaparak bu yapılandırmasının ayarlayabilirsiniz:

-   Yeni bir aracı aynı veri merkezinde eşitleme grup olarak oluşturun. Ardından bu aracı ile veritabanı kaydedin. Bkz: [nasıl yapılır: bir SQL veri eşitleme (Önizleme) istemci aracısı yükleyin](#install-a-sql-data-sync-client-agent) daha fazla bilgi için.

-   Geçerli eşitleme grubunu silin. Yeniden aracı aynı veri merkezinde oluşturun.

#### <a name="the-client-agents-list-of-databases-is-not-current"></a>İstemci aracısının veritabanlarının listesini geçerli değil

Ardından istemci Aracısı hizmetini durdurup yeniden başlatın.
Yerel aracı yalnızca ilk gönderme Aracısı anahtarın, sonraki Aracısı anahtar gönderimlerini değil, ilişkili veritabanlarının listesini indirir. Bu nedenle, bir aracı taşıma işlemi sırasında kayıtlı veritabanları özgün Aracısı örneğinde görünmüyor.

## <a name="client-agent-doesnt-start-error-1069"></a>İstemci Aracısı (hata 1069) başlamıyor

### <a name="description-and-symptoms"></a>Açıklama ve belirtileri

SQL Server'ı barındıran bir bilgisayara aracı çalışmadığından bulur. Aracıyı el ile başlatmak denediğinizde, hata iletişim kutusu hata iletisiyle Al "hata 1069: hizmeti oturum açma hatası nedeniyle başlatılamadı."

![Veri eşitleme hatası 1069 iletişim kutusu](media/sql-database-troubleshoot-data-sync/sync-error-1069.png)

### <a name="cause"></a>Nedeni

Bu hatanın olası bir nedeni, aracının oluşturulur ve oturum açma parolası vermiş beri yerel sunucuda parolanın değiştirilmesi ' dir.

### <a name="solution-or-workaround"></a>Çözüm ya da geçici çözüm

Aracının parola geçerli sunucu parolanızı güncelleştirin.

1. SQL veri eşitleme (Önizleme) istemci Aracısı Önizleme hizmeti bulun.

    a. Tıklatın **Başlat**.

    b. "Services.msc olanağını" arama kutusuna yazın.

    c. Arama sonuçlarında "Hizmetler"'i tıklatın.

    d. İçinde **Hizmetleri** penceresinde, girişine kaydırma **SQL veri eşitleme (Önizleme) aracısı Önizleme**.

2. Girişi sağ tıklatın ve seçin **durdurmak**.

3. Girişi sağ tıklatın ve ardından **özellikleri**.

4. İçinde **SQL veri eşitleme (Önizleme) aracısı Önizleme özellikleri** penceresinde tıklatın **oturum** sekmesi.

5. Parolanızı, parola metin kutusuna girin.

6. Parolayı Onayla metin kutusuna parolanızı onaylayın.

7. **Apple (Uygula)** seçeneğine ve ardından **Ok (Tamam)** seçeneğine tıklayın.

8. İçinde **Hizmetleri** penceresinde sağ **SQL veri eşitleme (Önizleme) aracısı Önizleme** hizmet ve ardından **Başlat**.

9. Kapat **Hizmetleri** penceresi.

## <a name="i-get-a-disk-out-of-space-message"></a>"Disk alanı yetersiz" iletisi alıyorum

### <a name="cause"></a>Nedeni

Silinmesi dosyaları arkasında kaldığında "disk alanı yetersiz" iletisi görüntülenebilir. Bu koşul nedeniyle virüsten koruma yazılımı ortaya veya silme işlemleri çalışırken dosyaları olduğundan açın.

### <a name="solution"></a>Çözüm

Altında eşitleme dosyaları el ile silmeye çözümdür `%temp%` (`del \*sync\* /s`) ve ardından alt dizinler de kaldırın.

> [!IMPORTANT]
> Tüm dosyaları silmeden önce eşitleme işlemi tamamlanana kadar bekleyin.

## <a name="i-cannot-delete-my-sync-group"></a>Eşitleme grubum silemiyorum

### <a name="description-and-symptoms"></a>Açıklama ve belirtileri

Eşitleme grubu silme denemeniz başarısız.

### <a name="causes"></a>Neden olur.

Aşağıdakilerden birini eşitleme grubunu silmek için bir başarısız olmasına neden olabilir.

-   Çevrimdışı istemci aracısıdır.

-   İstemci aracısı yüklü olmayan veya eksik olabilir. 

-   Bir veritabanı çevrimdışıdır. 

-   Eşitleme grubunu sağlama veya eşitleme. 

### <a name="solutions"></a>Çözümler

Eşitleme grubu silme hatası gidermek için şunları denetleyin:

-   İstemci Aracısı çevrimiçi olduğundan emin olun, sonra yeniden deneyin.

-   İstemci Aracısı yüklenmemiş veya başka türlü eksik ise:

    a. Dosya varsa agent XML dosyası SQL veri eşitleme (Önizleme) yükleme klasöründen kaldırın.

    b. İçi aynı ve başka bir bilgisayara aracı yüklemek için çevrimdışı gösteren aracısı için oluşturulan portalından aracı anahtarını Gönder.

-   **SQL veri eşitleme (Önizleme) hizmeti durdurulur.**

    a. İçinde **Başlat** menüsü, hizmetleri arayın.

    b. Arama sonuçlarında Hizmetler'i tıklatın.

    c. Bul **SQL veri eşitleme (Önizleme) Önizleme** hizmet.

    d. Hizmet durumu ise **durduruldu**, hizmet adını sağ tıklatın ve seçin **Başlat** açılan menüden.

-   Tüm çevrimiçi olduğundan emin olmak için SQL veritabanları ve SQL Server veritabanlarınızı denetleyin.

-   Sağlama veya eşitleme işlemi tamamlanana kadar bekleyin. Sonra eşitleme grubunu silme işlemini yeniden deneyin.

## <a name="sync-fails-in-the-portal-ui-for-on-premises-databases-associated-with-the-client-agent"></a>Şirket içi veritabanları için kullanıcı Arabirimi portalında eşitleme başarısız olursa istemci aracı ile ilişkili

### <a name="description-and-symptoms"></a>Açıklama ve belirtileri

Şirket içi veritabanları için SQL veri eşitleme (Önizleme) portal UI eşitleme başarısız aracı ile ilişkili. Aracıyı çalıştıran yerel bilgisayarda System.IO.ıoexception hataları diskte yeterli alan olduğunu bildiren olay günlüğüne bakın.

### <a name="solution-or-workaround"></a>Çözüm ya da geçici çözüm

% TEMP % dizininde yer aldığı sürücüde daha fazla alan oluşturun.

## <a name="i-cant-unregister-an-on-premises-sql-server-database"></a>Bir şirket içi SQL Server veritabanı kaydı silinemiyor

### <a name="cause"></a>Nedeni

Büyük olasılıkla zaten silinmiş bir veritabanı kaydını deniyorsunuz.

### <a name="solution-or-workaround"></a>Çözüm ya da geçici çözüm

Bir şirket içi SQL Server veritabanı kaydını silmek için veritabanını seçin ve **zorla silme**.

Veritabanı eşitleme gruptan kaldırmak bu işlemi başarısız olursa şunları yapın:

1. Durdurun ve sonra istemci Aracısı ana bilgisayar hizmeti yeniden başlatın.

    a. Başlat menüsünü tıklatın.

    b. Girin *services.msc* arama kutusuna.

    c. Sonuçlar bölmesinin bölümü programlarda çift **Hizmetleri**.

    d. Bulma ve hizmeti **SQL veri eşitleme (Önizleme)**.

    e. Hizmeti çalışıyorsa durdurun.

    f. Sağ tıklatıp **Başlat**.

    g. Veritabanı artık kayıtlı olup olmadığını denetleyin. Artık kayıtlı değilse, bitirdiniz. Aksi halde sonraki adıma geçin.

2. İstemci Aracısı uygulamasını (SqlAzureDataSyncAgent) açın.

3. Tıklatın **kimlik bilgilerini Düzenle** ve erişilebilir olması için veritabanı için kimlik bilgilerini sağlayın.

4. Kayıt kaldırma ile devam edin.

## <a name="i-cannot-submit-the-agent-key"></a>Aracı anahtarını Gönder olamaz

### <a name="description-and-symptoms"></a>Açıklama ve belirtileri

Oluşturma veya aracı için bir anahtar yeniden sonra SqlAzureDataSyncAgent uygulaması aracılığıyla anahtara göndermeyi deneyin ancak tamamlamak gönderme başarısız olur.

![Eşitleme hata iletişim kutusu - aracı anahtarını gönderemiyor](media/sql-database-troubleshoot-data-sync/sync-error-cant-submit-agent-key.png)

Devam etmeden önce aşağıdaki koşulların biri başarısız sorunu neden olmadığından emin olun.

-   SQL veri eşitleme (Önizleme) Windows hizmeti çalışıyor.

-   SQL veri eşitleme (Önizleme) önizleme Windows hizmeti için hizmet hesabı ağ erişimi vardır.

-   Konumlandırıcı Hizmeti ile bağlantı kurabiliyor istemci aracısıdır. Aşağıdaki kayıt defteri anahtarı değerini "https://locator.sync.azure.com/LocatorServiceApi.svc" olup olmadığını denetleyin

    -   Bir x86 üzerinde bilgisayar:`HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Microsoft\\SQL Azure Data Sync\\LOCATORSVCURI`

    -   X x64 üzerinde bilgisayar:`HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Wow6432Node\\Microsoft\\SQL Azure Data Sync\\LOCATORSVCURI`

### <a name="cause"></a>Nedeni

Aracı anahtarını her yerel aracı benzersiz olarak tanımlar. Anahtar bunu çalıştırmak iki koşullara uyması gerekir:

-   SQL veri eşitleme (Önizleme) sunucusu ve yerel bilgisayarda istemci Aracısı anahtar aynı olmalıdır.

-   İstemci Aracısı anahtar yalnızca bir kez kullanılabilir.

### <a name="solution-or-workaround"></a>Çözüm ya da geçici çözüm

Aracınızı çalışmıyorsa birine veya ikisine de bu koşulları karşılanmadı nedeni. Yeniden çalışma için aracınızı almak için:

1. Yeni bir anahtar oluşturun.

2. Yeni anahtar aracıya uygulayın.

Yeni anahtar aracıya uygulamak için şunları yapın:

1. Aracı yükleme dizinine gidin için dosya Gezgini'ni kullanın. Varsayılan yükleme dizini `c:\\program files (x86)\\microsoft sql data sync`.

2. Çift `bin` alt dizin.

3. Başlatma `SqlAzureDataSyncAgent` uygulama.

4. Tıklatın **aracı anahtarını Gönder**.

5. Panonuza anahtarından sağlanan alana yapıştırın.

6. **Tamam** düğmesine tıklayın.

7. Programı kapatın.

## <a name="i-do-not-have-sufficient-privileges-to-start-system-services"></a>Sistem hizmetlerini başlatmak için yeterli ayrıcalıklara sahip değil

### <a name="cause"></a>Nedeni

Bu hata, iki durumlarda oluşur:

-   Kullanıcı adı ve/veya parolası yanlış.

-   Belirtilen kullanıcı hesabı, bir hizmet olarak oturum açmak için yeterli ayrıcalıklara sahip değil.

### <a name="solution-or-workaround"></a>Çözüm ya da geçici çözüm

Kullanıcı hesabı için günlük üzerinde-a-hizmet olarak kimlik bilgilerini verin.

1. Gidin **Başlat | Denetim Masası | Yönetimsel Araçlar |  Yerel Güvenlik İlkesi | Yerel ilke | Kullanıcı Hakları Yönetimi**.

2. Bulun ve tıklatın **hizmet oturum açma** girişi.

3. Kullanıcı hesabını eklemek **özellikleri hizmet oturum açma** iletişim.

4. Tıklatın **uygulamak** sonra **Tamam**.

5. Pencereleri kapatın.

## <a name="local-sync-agent-app-is-unable-to-connect-to-the-local-sync-service"></a>Yerel eşitleme Aracısı uygulama yerel eşitleme hizmetine bağlanamıyor

### <a name="solution-or-workaround"></a>Çözüm ya da geçici çözüm

Aşağıdaki adımları deneyin:

1. Uygulama çıkın.

2. Bileşen Hizmetleri panelini açın.

    a. Görev çubuğunda arama kutusuna "services.msc" yazın

    b. "Hizmetler" arama sonuçlarında çift tıklayın.

3. Durdurun ve "SQL veri eşitleme (Önizleme) Önizleme" hizmetini durdurup yeniden başlatın.

4. Uygulamayı yeniden başlatın.

## <a name="install-uninstall-or-repair-fails"></a>Yükleme, kaldırma veya onarım başarısız

### <a name="cause"></a>Nedeni

Hatanın birçok olası nedeni vardır. Bu hatanın belirli nedenini belirlemek için günlüklerine bakın gerekir.

### <a name="solution-or-workaround"></a>Çözüm ya da geçici çözüm

Karşılaştığınız hatası için belirli nedenini bulmak için oluşturmak ve Windows Installer günlüklerine bakın gerekir. Komut satırından günlük kaydını etkinleştirebilirsiniz. Örneğin, indirilen AgentServiceSetup.msi dosya LocalAgentHost.msi olduğunu varsayın. Oluşturun ve aşağıdaki komut satırı kullanarak günlük dosyalarını inceleyin:

-   Yüklemeler için:`msiexec.exe /i SQLDataSyncAgent-Preview-ENU.msi /l\*v LocalAgentSetup.InstallLog`

-   İçin kaldırır:`msiexec.exe /x SQLDataSyncAgent-se-ENU.msi /l\*v LocalAgentSetup.InstallLog`

Windows Installer tarafından gerçekleştirilen tüm yüklemeleri için günlük kaydını etkinleştirebilirsiniz. Microsoft Bilgi Bankası makalesi [Windows Installer günlüğünün nasıl etkinleştirileceği](https://support.microsoft.com/help/223300/how-to-enable-windows-installer-logging) Windows Installer için günlük kaydını etkinleştirmek için tek tıklamayla çözümü sağlar. Ayrıca, bu günlükler konumunu sağlar.

## <a name="a-database-has-an-out-of-date-status"></a>Bir veritabanı "Süresi geçmiş" durumunda

### <a name="cause"></a>Nedeni

SQL veri eşitleme (Önizleme) kaldırır 45 gün veya daha fazla (veritabanı çevrimdışı oluştu saati sayılan gibi) çevrimdışı veritabanları hizmetinden. Bir veritabanı 45 gün veya daha fazla bilgi için çevrimdışı ise ve daha sonra yeniden çevrimiçi olur, durumu "Güncel" ayarlanmış

### <a name="solution-or-workaround"></a>Çözüm ya da geçici çözüm

"Süresi geçmiş" durum 45 gün veya daha fazla bilgi için veritabanlarının hiçbiri çevrimdışı gidin, sağlayarak önleyebilirsiniz.

Bir veritabanının durumu ise şunları yapmanız gerekir "Güncel":

1. "Süresi geçmiş" veritabanı eşitleme grubundan kaldırın.

2. Veritabanı Ekle eşitleme grubuna yedekleyin.

> [!WARNING]
> Çevrimdışı durumdayken bu veritabanına yapılan tüm değişiklikler kaybedilir.

## <a name="a-sync-group-has-an-out-of-date-status"></a>Eşitleme grubu "Süresi geçmiş" durumunda

### <a name="cause"></a>Nedeni

45 gün tüm saklama dönemi için uygulanacak bir veya daha fazla değişiklik başarısız olursa, bir eşitleme grubu eski haline gelebilir.

### <a name="solution-or-workaround"></a>Çözüm ya da geçici çözüm

"Süresi geçmiş" durumu önlemek için Eşitleme işleri düzenli olarak, geçmiş Görüntüleyicisi'nde sonuçlarını inceleyin ve araştırın ve değişiklikleri uygulamak için başarısız çözümleyin.

Bir eşitleme grubun durumundaysa eşitleme grubunu silip yeniden oluşturmanız gerekir "Güncel".

## <a name="i-see-erroneous-data-in-my-tables"></a>Hatalı veri my tablolarda görüyorum

### <a name="description-and-symptoms"></a>Açıklama ve belirtileri

Tablolar aynı ada sahip ancak bir veritabanı farklı şemalarda eşitlenmiş söz konusuysa, bu tablolardaki hatalı veri eşitleme sonrasında bakın.

### <a name="cause-and-fix"></a>Nedeni ve düzeltme

SQL veri eşitleme (Önizleme) sağlama işlemi tablolar aynı ada sahip ancak farklı şemalarda aynı izleme tablosu kullanır. Sonuç olarak, her iki tablodan değişiklikler aynı izleme tabloda yansıtılır ve eşitleme sırasında hatalı veri neden bu davranış değişir.

### <a name="resolution-or-workaround"></a>Çözüm veya geçici çözüm

Farklı şemaya ait oldukları olsa bile eşitleme katılan tabloların adlarını farklı olduğundan emin olun.

## <a name="i-see-inconsistent-primary-key-data-after-a-successful-synchronization"></a>Başarılı bir eşitlemeden sonra tutarsız birincil anahtar veri görüyorum

### <a name="description-and-symptoms"></a>Açıklama ve belirtileri

Bir eşitleme sonrasında, başarılı olarak bildirilir ve birincil anahtar veri eşitleme grubunu veritabanları arasında tutarsız olup olmadığına bakın hiçbir başarısız veya Atlanan satır günlük gösterir.

### <a name="cause"></a>Nedeni

Bu davranış tasarım gereğidir. Burada kullanılan birincil anahtarı değiştirildi satırlardaki tutarsız veriler, herhangi bir birincil anahtar sütunu değişiklikleri sonuçlanır.

### <a name="resolution-or-workaround"></a>Çözüm veya geçici çözüm

Bu sorunu önlemek için birincil anahtar sütunu yok verilerde değiştiğini emin olun.

Bu gerçekleştikten sonra bu sorunu gidermek için eşitleme grubundaki tüm uç noktaları gelen etkilenen satır bırakın ve satırı yeniden gerekir.

## <a name="i-see-a-significant-degradation-in-performance"></a>Önemli bir performans düşüşü bakın

### <a name="description-and-symptoms"></a>Açıklama ve belirtileri

Performansınızı büyük olasılıkla önemli ölçüde, burada bile veri eşitleme UI başlatamazlar noktasına düşürür.

### <a name="cause"></a>Nedeni

En olası nedeni bir eşitleme döngüsü oluşturur. Bir eşitleme döngüsü hangi sırayla eşitleme eşitleme grubu A'daki tarafından tetikleyen bir eşitleme eşitleme grubu A Tetikleyicileri eşitleme eşitleme grubu B, a tarafından oluşur. Fiili durumu Döngüdeki birden fazla iki eşitleme grubu içeren daha karmaşık olabilir ancak önemli bir döngüsel bir diğer çakışan eşitleme grubu tarafından neden eşitlemeler, tetikleme olduğu unsurdur.

### <a name="resolution-or-workaround"></a>Çözüm veya geçici çözüm

En iyi düzeltme önleme bulunur. Döngüsel başvurulara eşitleme gruplarınızı sahip emin olun. Bir eşitleme grubu tarafından eşitlenen herhangi bir satırın başka bir eşitleme grubu tarafından eşitlenemiyor.

## <a name="client-agent-cannot-be-deleted-from-the-portal-if-its-associated-on-premises-database-is-unreachable"></a>İstemci Aracısı içi ilişkili veritabanını erişilemediğinde portaldan silinemez.

### <a name="description-and-symptoms"></a>Açıklama ve belirtileri

Bir yerel bir SQL veri eşitleme (Önizleme) istemci Aracısı ile kayıtlı ucuna (diğer bir deyişle, bir veritabanı) ulaşılamaz hale gelirse, istemci Aracısı silinemiyor.

### <a name="cause"></a>Nedeni

Yerel aracı ulaşılamaz veritabanı aracı ile kayıtlı olduğu için silinemiyor. Aracı silmeye çalıştığınızda, silme işlemi başarısız veritabanı ulaşmaya çalışır.

### <a name="resolution-or-workaround"></a>Çözüm veya geçici çözüm

"Sil zorla" kullanmak ulaşılamaz veritabanı silinemiyor.

> [!NOTE]
> Eğer sonra bunları temizlemek için kullanım deprovisioningutil.exe "zorla Sil" eşitleme meta veri tablolarına kalır.

## <a name="a-sync-group-cannot-be-deleted-within-three-minutes-of-uninstallingstopping-the-agent"></a>Üç Aracı kaldırma/durdurma dakika içinde bir eşitleme grubu silinemez.

### <a name="description-and-symptoms"></a>Açıklama ve belirtileri

Üç ilişkili SQL veri eşitleme (Önizleme) istemci aracısı kaldırma/durdurma dakika içinde bir eşitleme grubunu silmek mümkün değildir.

### <a name="resolution-or-workaround"></a>Çözüm veya geçici çözüm

1. İlişkili eşitleme aracıları çevrimiçi durumdayken bir eşitleme grubunu kaldırmak (önerilen).

2. Aracı çevrimdışı ancak yüklü ise, şirket içi bilgisayarda çevrimiçi duruma getirin. Ardından SQL veri eşitleme (Önizleme) Portalı'nda çevrimiçi olarak görünür aracı durumu için bekleyin ve eşitleme grubunu kaldırın.

3. Bunu kaldırıldığından aracı çevrimdışıysa, şunları yapın. Daha sonra eşitleme grubu silmeyi deneyin.

    a.  Dosya varsa agent XML dosyası SQL veri eşitleme (Önizleme) yükleme klasöründen kaldırın.

    b.  Aynı aracı yüklemek veya başka bir şirket içi bilgisayar çevrimdışı gösteren aracısı için oluşturulan portalından aracı anahtarını Gönder.

## <a name="my-sync-group-is-stuck-in-the-processing-state"></a>Eşitleme grubum işleme durumunda takıldı

### <a name="description-and-symptoms"></a>Açıklama ve belirtileri

SQL veri eşitleme (Önizleme) eşitleme grubunda uzun bir süre için işleme durumda kaldıysa, Dur komutuna yanıt değil ve günlükleri hiçbir yeni girdi gösterir.

### <a name="causes"></a>Neden olur.

Aşağıdaki koşullardan herhangi biri bir eşitleme grubu işleme durumunda kalmış neden olabilir.

-   **Çevrimdışı istemci aracısıdır.** İstemci Aracısı çevrimiçi olduğundan emin olun, sonra yeniden deneyin.

-   **İstemci aracısı yüklü olmayan veya eksik olabilir.** İstemci Aracısı yüklenmemiş veya başka türlü eksik ise:

    1. Dosya varsa agent XML dosyası SQL veri eşitleme (Önizleme) yükleme klasöründen kaldırın.

    2. Aracı içi aynı ve başka bir bilgisayara yükleyin. Ardından çevrimdışı olarak gösteren aracısı için oluşturulan portalından aracı anahtarını Gönder.

-   **SQL veri eşitleme (Önizleme) hizmeti durdurulur.**

    1. İçinde **Başlat** menüsü, hizmetleri arayın.

    2. Arama sonuçlarında Hizmetler'i tıklatın.

    3. Bul **SQL veri eşitleme (Önizleme)** hizmet.

    4. Hizmet durumu ise **durduruldu**, hizmet adını sağ tıklatın ve seçin **Başlat** açılan menüden.

### <a name="solution-or-workaround"></a>Çözüm ya da geçici çözüm

Sorunu düzeltmek erişemiyorsanız, eşitleme grubunuzun durumunu sıfırlanabilir Microsoft desteği tarafından. Sıfırlama, bir forum gönderisi oluşturmak durumunuzu olması için [Azure SQL veritabanı Forumu](https://social.msdn.microsoft.com/Forums/azure/home?forum=ssdsgetstarted)ve abonelik Kimliğinizi ve sıfırlanması gerekir grubunun eşitleme Grup Kimliğini içerir. Bir Microsoft destek mühendisi postanızı için yanıt ve ne zaman durumu sıfırlandı size bildirmek.

## <a name="next-steps"></a>Sonraki adımlar
SQL veri eşitleme hakkında daha fazla bilgi için bkz:

-   [Eşitleme verilerle birden çok Bulut ve şirket içi veritabanları arasında Azure SQL veri eşitleme](sql-database-sync-data.md)
-   [Azure SQL veri eşitlemeye başlama](sql-database-get-started-sql-data-sync.md)
-   [Azure SQL veri eşitleme için en iyi yöntemler](sql-database-best-practices-data-sync.md)
-   [OMS günlük analizi ile İzleyici Azure SQL veri eşitleme](sql-database-sync-monitor-oms.md)

-   SQL veri eşitleme yapılandırmayı gösterir PowerShell örnekleri tamamlayın:
    -   [Birden çok Azure SQL veritabanları arasında eşitlemek için PowerShell kullanma](scripts/sql-database-sync-data-between-sql-databases.md)
    -   [Bir Azure SQL Database ve SQL Server içi veritabanı arasında eşitlemek için PowerShell kullanma](scripts/sql-database-sync-data-between-azure-onprem.md)

-   [SQL veri eşitleme REST API belgelerini indirebilirsiniz](https://github.com/Microsoft/sql-server-samples/raw/master/samples/features/sql-data-sync/Data_Sync_Preview_REST_API.pdf?raw=true)

SQL veritabanı hakkında daha fazla bilgi için bkz:

-   [SQL veritabanı genel bakış](sql-database-technical-overview.md)
-   [Veritabanı yaşam döngüsü yönetimi](https://msdn.microsoft.com/library/jj907294.aspx)
