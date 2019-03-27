---
title: Veri eşitleme için Azure SQL Data Sync'i aracı | Microsoft Docs
description: Yükleme ve veri eşitleme Aracısı şirket içi SQL Server veritabanları ile veri eşitleme için Azure SQL Data Sync için çalıştırma hakkında bilgi edinin
services: sql-database
ms.service: sql-database
ms.subservice: data-movement
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: allenwux
ms.author: xiwu
ms.reviewer: carlrab
manager: craigg
ms.date: 12/20/2018
ms.openlocfilehash: adb8917605a00208b328e7fd15f96d28c7838988
ms.sourcegitcommit: 0dd053b447e171bc99f3bad89a75ca12cd748e9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58485213"
---
# <a name="data-sync-agent-for-azure-sql-data-sync"></a>Verileri Azure SQL Data Sync için eşitleme Aracısı

Yükleme ve Azure SQL Data Sync için veri eşitleme Aracısı yapılandırarak verileri şirket içi SQL Server veritabanları ile eşitleyin. SQL Data Sync hakkında daha fazla bilgi için bkz. [verileri Eşitle birden fazla Bulut ve şirket içi veritabanında SQL Data Sync ile](sql-database-sync-data.md).

> [!IMPORTANT]
> Azure SQL Data Sync mu **değil** şu anda Azure SQL veritabanı yönetilen örneği destekler.

## <a name="download-and-install"></a>İndir ve yükle

Veri Eşitleme aracısını indirmek için Git [SQL Azure veri eşitleme Aracısı](https://www.microsoft.com/download/details.aspx?id=27693).

### <a name="install-silently"></a>Sessiz yükleme

Veri Eşitleme aracısını komut isteminden sessizce yüklemek için aşağıdaki örneğe benzer bir komut girin. İndirilen .msi dosyasının dosya adını denetleyin ve sağlamak için kendi değerlerinizi **TARGETDIR** ve **SERVICEACCOUNT** bağımsız değişkenler.

- İçin bir değer sağlamıyorsa **TARGETDIR**, varsayılan değer `C:\Program Files (x86)\Microsoft SQL Data Sync 2.0`.

- Sağlarsanız `LocalSystem` değeri olarak **SERVICEACCOUNT**, şirket içi SQL Server'a bağlanmak için aracıyı yapılandırdığınızda, SQL Server kimlik doğrulaması kullanın.

- Değeri olarak bir etki alanı kullanıcı hesabı veya yerel kullanıcı hesabı sağlarsanız **SERVICEACCOUNT**, parola ile sağlamak de **SERVICEPASSWORD** bağımsız değişken. Örneğin, `SERVICEACCOUNT="<domain>\<user>"  SERVICEPASSWORD="<password>"`.

```cmd
msiexec /i "SQLDataSyncAgent-2.0-x86-ENU.msi" TARGETDIR="C:\Program Files (x86)\Microsoft SQL Data Sync 2.0" SERVICEACCOUNT="LocalSystem" /qn
```

## <a name="sync-data-with-sql-server-on-premises"></a>Şirket içi SQL Server ile veri eşitleme

Veri Eşitleme Aracısı bir veya daha fazla şirket içi SQL Server veritabanları ile veri eşitleyebilirsiniz şekilde yapılandırmak için bkz [bir şirket içi SQL Server veritabanı ekleyin](sql-database-get-started-sql-data-sync.md#add-on-prem).

## <a name="agent-faq"></a> Veri Eşitleme Aracısı hakkında SSS

### <a name="why-do-i-need-a-client-agent"></a>Bir istemci Aracısı neden gerekiyor

SQL Data Sync hizmeti, istemci Aracısı aracılığıyla SQL Server veritabanları ile iletişim kurar. Bu güvenlik özelliğini bir güvenlik duvarının arkasındaki veritabanları ile doğrudan iletişim engeller. Ne zaman SQL Data Sync hizmeti iletişim kurar aracıyla, bunu kullanarak yaptığı şifrelenmiş bağlantılar ve benzersiz bir belirteç veya *aracı anahtarı*. SQL Server veritabanları bağlantı dizesi ve aracı anahtarını kullanarak aracı kimlik doğrulaması. Bu tasarım, güvenlik verileriniz için yüksek düzeyde sağlar.

### <a name="how-many-instances-of-the-local-agent-ui-can-be-run"></a>Kullanıcı Arabirimi yerel aracı kaç örneklerini çalıştırılabilir.

Kullanıcı Arabirimi yalnızca bir örneği çalıştırılabilir.

### <a name="how-can-i-change-my-service-account"></a>Hizmet Hesabımı nasıl değiştirebilirim

Bir istemci Aracısı yükledikten sonra hizmet hesabını değiştirmek için tek kaldırıp yeni hizmet hesabını yeni bir istemci Aracısı yükleme yoludur.

### <a name="how-do-i-change-my-agent-key"></a>Aracı anahtarımı nasıl değiştirebilirim

Bir aracı anahtarı, bir aracı tarafından yalnızca bir kez kullanılabilir. Kaldırın, sonra yeni bir aracıyı yeniden yükleyin veya birden çok aracı tarafından yeniden kullanılabilir olduğunda kullanılamayacak. Var olan bir aracı için yeni bir anahtar oluşturmanız gerekiyorsa, SQL Data Sync hizmet ve istemci Aracısı ile aynı anahtar kaydedilir emin olmanız gerekir.

### <a name="how-do-i-retire-a-client-agent"></a>Bir istemci Aracısı nasıl devre dışı bırakma

Hemen geçersiz kılmak veya aracıyı devre dışı bırakmak için Portalı'nda kendi anahtarını yeniden ancak aracı Arabiriminde bulunmayın. Bir anahtar yeniden oluşturuluyor, karşılık gelen aracı çevrimiçi veya çevrimdışı ise belirtilmediğine önceki anahtar geçersiz kılar.

### <a name="how-do-i-move-a-client-agent-to-another-computer"></a>Başka bir bilgisayara bir istemci Aracısı nasıl taşırım

Şu anda açıktır olandan farklı bir bilgisayardan yerel aracı çalıştırmak istiyorsanız, şunları yapın:

1. İstediğiniz bilgisayara aracıyı yükleyin.
2. SQL Data Sync portalında oturum açın ve yeni aracı için bir aracı anahtarı yeniden oluştur.
3. Yeni aracı anahtarı göndermek için yeni aracının kullanıcı Arabirimi kullanın.
4. İstemci Aracısı, daha önce kaydedilmiş şirket içi veritabanlarının listesini yüklerken bekleyin.
5. Veritabanı kimlik bilgileri olarak ulaşılamaz görüntüleyen tüm veritabanları için sağlar. Bu veritabanları, aracının yüklü olduğu yeni bilgisayardan erişilebilir olmalıdır.

## <a name="agent-tshoot"></a> Veri Eşitleme Aracısı sorunlarını giderme

- [İstemci aracı yükleme, kaldırma veya onarım başarısız](#agent-install)

- [Ben kaldırma işlemi iptal ettikten sonra istemci Aracısı çalışmıyor](#agent-uninstall)

- [Veritabanım aracı listesinde listelenmiyor](#agent-list)

- [İstemci Aracısı (hata 1069) başlamıyor](#agent-start)

- [Aracı anahtarını Gönder olamaz](#agent-key)

- [İstemci Aracısı, ilişkili şirket içi veritabanı ulaşılamaz durumdaysa portaldan silinemiyor](#agent-delete)

- [Yerel bir eşitleme Aracısı uygulaması yerel eşitleme hizmetine bağlanamıyor](#agent-connect)

### <a name="agent-install"></a> İstemci aracı yükleme, kaldırma veya onarım başarısız

- **Neden**. Birçok senaryo bu hataya neden olabilir. Bu hatanın nedenini belirlemek için günlüklere bakın.

- **Çözüm**. Belirli bir hatanın nedenini bulmak için oluşturmak ve Windows Yükleyici günlüklerine bakın. Bir komut isteminde günlük kaydını etkinleştirebilirsiniz. Örneğin, indirilen yükleme dosyasını ise `SQLDataSyncAgent-2.0-x86-ENU.msi`, oluşturun ve aşağıdaki komut satırlarını kullanarak günlük dosyalarını inceleyin:

  - Yüklemeler için: `msiexec.exe /i SQLDataSyncAgent-2.0-x86-ENU.msi /l*v LocalAgentSetup.Log`
  - İçin kaldırır: `msiexec.exe /x SQLDataSyncAgent-2.0-x86-ENU.msi /l*v LocalAgentSetup.Log`

    Ayrıca Windows Installer tarafından gerçekleştirilen tüm yüklemeleri için günlük kaydını etkinleştirebilirsiniz. Microsoft Bilgi Bankası makalesi [Windows Installer günlüğe kaydetmenin nasıl etkinleştirileceği](https://support.microsoft.com/help/223300/how-to-enable-windows-installer-logging) Windows Yükleyicisi için günlük kaydını etkinleştirmek için tek tıklamayla çözümü sağlar. Ayrıca, günlüklerinin konumunu sağlar.

### <a name="agent-uninstall"></a> Ben kaldırma işlemi iptal ettikten sonra istemci Aracısı çalışmıyor

İstemci Aracısı, hatta kendi kaldırma iptal edildikten sonra çalışmaz.

- **Neden**. Bu durum, SQL Data Sync istemci Aracısı, kimlik bilgilerini depolamaz kaynaklanır.

- **Çözüm**. Bu iki çözüm de deneyebilirsiniz:

    -   İstemci aracısı için kimlik bilgilerini girmek için Services.msc dosyasını kullanın.
    -   Bu istemci aracısını kaldırın ve yenisini yükleyin. En son istemci Aracısı'ndan yükleyip [İndirme Merkezi](https://go.microsoft.com/fwlink/?linkid=221479).

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

SQL Server'ı barındıran bir bilgisayara aracı çalışmadığından emin keşfedin. Aracıyı elle başlatmayı deneyin iletisini gösteren bir iletişim kutusu görürsünüz "hatası 1069: Hizmet bir oturum açma hatası nedeniyle başlatılamadı."

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
  1. Uygulamayı yeniden açmak.

## <a name="run-the-data-sync-agent-from-the-command-prompt"></a>Veri Eşitleme aracısını komut isteminden Çalıştır

Komut isteminden aşağıdaki veri eşitleme Aracısı komutları çalıştırabilirsiniz:

### <a name="ping-the-service"></a>Ping hizmeti

#### <a name="usage"></a>Kullanım

```cmd
SqlDataSyncAgentCommand.exe -action pingsyncservice
```

#### <a name="example"></a>Örnek

```cmd
SqlDataSyncAgentCommand.exe -action "pingsyncservice"
```

### <a name="display-registered-databases"></a>Kayıtlı veritabanları görüntüleme

#### <a name="usage"></a>Kullanım

```cmd
SqlDataSyncAgentCommand.exe -action displayregistereddatabases
```

#### <a name="example"></a>Örnek

```cmd
SqlDataSyncAgentCommand.exe -action "displayregistereddatabases"
```

### <a name="submit-the-agent-key"></a>Aracı anahtarını Gönder

#### <a name="usage"></a>Kullanım

```cmd
Usage: SqlDataSyncAgentCommand.exe -action submitagentkey -agentkey [agent key]  -username [user name] -password [password]
```

#### <a name="example"></a>Örnek

```cmd
SqlDataSyncAgentCommand.exe -action submitagentkey -agentkey [agent key generated from portal, PowerShell, or API] -username [user name to sync metadata database] -password [user name to sync metadata database]
```

### <a name="register-a-database"></a>Bir veritabanına kaydetme

#### <a name="usage"></a>Kullanım

```cmd
SqlDataSyncAgentCommand.exe -action registerdatabase -servername [on-premisesdatabase server name] -databasename [on-premisesdatabase name]  -username [domain\\username] -password [password] -authentication [sql or windows] -encryption [true or false]
```

#### <a name="examples"></a>Örnekler

```cmd
SqlDataSyncAgentCommand.exe -action "registerdatabase" -serverName localhost -databaseName testdb -authentication sql -username <user name> -password <password> -encryption true

SqlDataSyncAgentCommand.exe -action "registerdatabase" -serverName localhost -databaseName testdb -authentication windows -encryption true

```

### <a name="unregister-a-database"></a>Bir veritabanı kaydını sil

Bir veritabanı kaydını kaldırmak için bu komutu kullanırken, veritabanı tamamen deprovisions. Veritabanı diğer eşitleme gruplarında yer alıyorsa, bu işlem bir eşitleme gruplarına ayırır.

#### <a name="usage"></a>Kullanım

```cmd
SqlDataSyncAgentCommand.exe -action unregisterdatabase -servername [on-premisesdatabase server name] -databasename [on-premisesdatabase name]
```

#### <a name="example"></a>Örnek

```cmd
SqlDataSyncAgentCommand.exe -action "unregisterdatabase" -serverName localhost -databaseName testdb
```

### <a name="update-credentials"></a>Kimlik bilgilerini güncelleştirme

#### <a name="usage"></a>Kullanım

```cmd
SqlDataSyncAgentCommand.exe -action updatecredential -servername [on-premisesdatabase server name] -databasename [on-premisesdatabase name]  -username [domain\\username] -password [password] -authentication [sql or windows] -encryption [true or false]
```

#### <a name="examples"></a>Örnekler

```cmd
SqlDataSyncAgentCommand.exe -action "updatecredential" -serverName localhost -databaseName testdb -authentication sql -username <user name> -password <password> -encryption true

SqlDataSyncAgentCommand.exe -action "updatecredential" -serverName localhost -databaseName testdb -authentication windows -encryption true
```

## <a name="next-steps"></a>Sonraki adımlar

SQL Data Sync hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

-   Genel Bakış - [verileri Eşitle birden fazla Bulut ve şirket içi veritabanı arasında Azure SQL Data Sync ile](sql-database-sync-data.md)
-   Data Sync'i Ayarla
    - Portalda - [Öğreticisi: Azure SQL veritabanı ve SQL Server arasında verileri eşitlemek amacıyla şirket içi SQL Data Sync'i Ayarla](sql-database-get-started-sql-data-sync.md)
    - PowerShell ile
        -  [PowerShell kullanarak birden çok Azure SQL veritabanı arasında eşitleme](scripts/sql-database-sync-data-between-sql-databases.md)
        -  [PowerShell kullanarak bir Azure SQL Veritabanı ile SQL Server şirket içi veritabanı arasında eşitleme](scripts/sql-database-sync-data-between-azure-onprem.md)
-   En iyi uygulamalar - [en iyi uygulamalar için Azure SQL Data Sync](sql-database-best-practices-data-sync.md)
-   İzleyici - [SQL Data Sync'i Azure İzleyici ile izleme günlükleri](sql-database-sync-monitor-oms.md)
-   Sorun giderme - [Azure SQL Data Sync ile ilgili sorunları giderme](sql-database-troubleshoot-data-sync.md)
-   Eşitleme şemasını güncelleştirmek
    -   Transact-SQL ile- [Azure SQL Data Sync şema değişikliklerinin çoğaltmayı otomatik hale getirme](sql-database-update-sync-schema.md)
    -   PowerShell ile- [var olan bir eşitleme grubunda eşitleme şemasını güncelleştirmek için PowerShell kullanma](scripts/sql-database-sync-update-schema.md)
