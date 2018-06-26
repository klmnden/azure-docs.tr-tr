---
title: Uygulamanızı Azure’a yedekleme
description: Azure App Service'te uygulamalarınızı yedeklerini oluşturmayı öğrenin.
services: app-service
documentationcenter: ''
author: cephalin
manager: erikre
editor: jimbe
ms.assetid: 6223b6bd-84ec-48df-943f-461d84605694
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/06/2016
ms.author: cephalin
ms.openlocfilehash: b87838a80c7c7706b9af2bd4ea274335d04a5c52
ms.sourcegitcommit: 6eb14a2c7ffb1afa4d502f5162f7283d4aceb9e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/25/2018
ms.locfileid: "36751522"
---
# <a name="back-up-your-app-in-azure"></a>Uygulamanızı Azure’a yedekleme
Yedekleme ve geri yükleme özelliği [Azure App Service](app-service-web-overview.md) el ile veya bir zamanlamaya göre uygulama yedeklemeleri kolayca oluşturmanıza olanak sağlar. Uygulama mevcut uygulamanın üzerine veya başka bir uygulamaya geri anlık görüntü önceki bir duruma geri yükleyebilirsiniz. 

Bir uygulama yedekten geri yükleme hakkında daha fazla bilgi için bkz: [Azure bir uygulamada geri](web-sites-restore.md).

<a name="whatsbackedup"></a>

## <a name="what-gets-backed-up"></a>Ne yedeklenir
Uygulama hizmeti bir Azure depolama hesabı ve kullanmak için uygulamanızı yapılandırdığınız kapsayıcı aşağıdaki bilgilerini yedekleyebilirsiniz. 

* Uygulama yapılandırması
* Dosya içeriği
* Uygulamanıza bağlı veritabanı

Şu veritabanı çözümleri ile yedekleme özelliği desteklenir: 
   - [SQL Database](https://azure.microsoft.com/services/sql-database/)
   - [Azure veritabanı için MySQL (Önizleme)](https://azure.microsoft.com/services/mysql)
   - [Azure veritabanı için PostgreSQL (Önizleme)](https://azure.microsoft.com/services/postgresql)
   - [Uygulama MySQL](https://blogs.msdn.microsoft.com/appserviceteam/2017/03/06/announcing-general-availability-for-mysql-in-app)
 

> [!NOTE]
>  Her yedekleme bir değil Artımlı güncelleştirme, uygulamanızın çevrimdışı tam kopyasıdır.
>  

<a name="requirements"></a>

## <a name="requirements-and-restrictions"></a>Gereksinimleri ve kısıtlamaları
* Yedekleme ve geri yükleme özelliği olması için uygulama hizmeti planı gerektirir **standart** katmanı veya **Premium** katmanı. Uygulama hizmeti planınızı daha yüksek bir katmanı kullanmak üzere ölçeklendirme hakkında daha fazla bilgi için bkz: [Azure bir uygulamada ölçeklendirin](web-sites-scale.md).  
  **Premium** katmanı sağlayan daha fazla sayıda günlük geri ups daha **standart** katmanı.
* Bir Azure depolama hesabı ve kapsayıcı yedeklemek istediğiniz uygulama ile aynı abonelikte gerekir. Azure depolama hesapları hakkında daha fazla bilgi için bkz: [bağlantılar](#moreaboutstorage) bu makalenin sonunda.
* Yedekleme 10 GB uygulama ve veritabanı içeriğinin olabilir. Yedekleme boyutu bu sınırı aşarsa, bir hata alıyorsunuz.

<a name="manualbackup"></a>

## <a name="create-a-manual-backup"></a>El ile yedekleme oluşturun
1. İçinde [Azure portal](https://portal.azure.com), uygulamanızın sayfasına gidin, seçin **yedeklemeleri**. **Yedeklemeleri** sayfası görüntülenir.
   
    ![Yedeklemeleri sayfası][ChooseBackupsPage]
   
   > [!NOTE]
   > Aşağıdaki iletiyi görürseniz, yedeklemeler sayesinde devam etmeden önce uygulama hizmeti planınızı yükseltmek için tıklayın.
   > Daha fazla bilgi için bkz: [Azure bir uygulamada ölçeklendirin](web-sites-scale.md).  
   > ![Depolama hesabı seçin](./media/web-sites-backup/01UpgradePlan1.png)
   > 
   > 

2. İçinde **yedekleme** sayfasında, tıklatın **yapılandırma**
![tıklatın yapılandırın](./media/web-sites-backup/ClickConfigure1.png)
3. İçinde **yedekleme yapılandırması** sayfasında, **Depolama: yapılandırılmamış** bir depolama hesabı yapılandırmak için.
   
    ![Depolama hesabı seç][ChooseStorageAccount]
4. Yedekleme Hedefinizi seçerek bir **depolama hesabı** ve **kapsayıcı**. Depolama hesabı, yedeklemek istediğiniz uygulama aynı aboneliğe ait olmalıdır. İsterseniz, yeni bir depolama hesabı veya yeni bir kapsayıcı ilgili sayfaları oluşturabilirsiniz. İşiniz bittiğinde tıklatın **seçin**.
   
    ![Depolama hesabı seç](./media/web-sites-backup/02ChooseStorageAccount1-1.png)
5. İçinde **yedekleme yapılandırması** hala açık kaldığını sayfasında, **Backup Database**, ardından yedekleri (SQL veritabanı veya MySQL) dahil etmek istediğiniz veritabanlarını seçin ve ardından **Tamam**.  
   
    ![Depolama hesabı seç](./media/web-sites-backup/03ConfigureDatabase1.png)
   
   > [!NOTE]
   > Bağlantı dizesi bu listesinde görünmesi için bir veritabanı, mevcut olmalıdır **bağlantı dizeleri** bölümünü **uygulama ayarları** sayfasında uygulamanız için.
   > 
   > 
6. İçinde **yedekleme yapılandırması** sayfasında, **kaydetmek**.    
7. İçinde **yedeklemeleri** sayfasında, **yedekleme**.
   
    ![BackUpNow düğmesi][BackUpNow]
   
    Yedekleme işlemi sırasında bir ilerleme iletisini görürsünüz.

Depolama hesabı ve kapsayıcı yapılandırılmış sonra istediğiniz zaman el ile yedekleme başlatabilir.  

<a name="automatedbackups"></a>

## <a name="configure-automated-backups"></a>Otomatik yedeklemeleri yapılandırma
1. İçinde **yedekleme yapılandırması** sayfasında **zamanlanmış yedekleme** için **üzerinde**. 
   
    ![Depolama hesabı seç](./media/web-sites-backup/05ScheduleBackup1.png)
2. Seçenekleri gösterir, yedekleme zamanlamasını ayarlamak **zamanlanmış yedekleme** için **üzerinde**, ardından yedekleme zamanlamasını istediğiniz gibi yapılandırın ve tıklayın **Tamam**.
   
    ![Otomatik yedeklemeler etkinleştir][SetAutomatedBackupOn]

<a name="partialbackups"></a>

## <a name="configure-partial-backups"></a>Kısmi yedeklemeleri yapılandırma
Bazen her şeyi uygulamanıza yedekle istemezsiniz. İşte birkaç örnek:

* [Haftalık yedeklemeler kurabilir](web-sites-backup.md#configure-automated-backups) uygulamanızın hiçbir zaman, eski blog gönderileri veya görüntüleri gibi değişiklikler statik içerik içerir.
* Uygulamanızın üzerinde 10 GB (aynı anda yedekleyebilirsiniz maks tutar) içerik var.
* Günlük dosyalarını yedeklemek istemezsiniz.

Kısmi yedeklenmesine izin vermek tam olarak yedeklemek istediğiniz dosyaları seçin.

### <a name="exclude-files-from-your-backup"></a>Yedekleme dosyaları dışarıda bırak
Günlük dosyaları ve bir kez yedekleme atanmış olması ve değiştirmek için yapmayacağınız statik görüntüleri içeren bir uygulama olduğunu varsayalım. Böyle durumlarda, bu dosya ve klasörleri, gelecekteki yedeklemeler depolanan dışlayabilirsiniz. Dosya ve klasörleri yedeklemelerinizi dışlamak için Oluştur bir `_backup.filter` dosyasını `D:\home\site\wwwroot` , uygulamanızın klasör. Dosyaları ve bu dosyada dışlamak istediğiniz klasörleri belirtin. 

Dosyalarınıza erişmek için kolay bir yol Kudu kullanmaktır. Tıklatın **Gelişmiş Araçlar -> Git** Kudu erişmek web uygulamanız için ayarlama.

![Portal kullanarak kudu][kudu-portal]

Yedeklemelerinizi dışlamak istediğiniz klasörleri tanımlayın.  Örneğin, vurgulanan klasör ve dosyaları filtre uygulamak istediğiniz.

![Görüntüler klasörüne][ImagesFolder]

Adlı bir dosya oluşturun `_backup.filter` ve yukarıdaki listede dosyaya yerleştirin, ancak kaldırma `D:\home`. Bir dizin veya dosya her satırda listeleyin. Böylece dosyanın içeriği olmalıdır:
 ```bash
    \site\wwwroot\Images\brand.png
    \site\wwwroot\Images\2014
    \site\wwwroot\Images\2013
```

Karşıya yükleme `_backup.filter` dosya `D:\home\site\wwwroot\` kullanarak site dizininde [ftp](app-service-deploy-ftp.md) veya başka bir yöntem. İsterseniz, Kudu kullanarak doğrudan dosyası oluşturabilirsiniz `DebugConsole` ve içeriği var. ekleyin.

Aşağıdakini normalde yapın, aynı şekilde yedeklemeleri çalıştırma [el ile](#create-a-manual-backup) veya [otomatik olarak](#configure-automated-backups). Şimdi, tüm dosya ve klasörlerin içinde belirtilen `_backup.filter` zamanlanmış veya el ile başlatma gelecekteki yedeklemelerden dışlandı. 

> [!NOTE]
> Yaptığınız şekilde sitenizin kısmi yedekleri geri [normal bir yedeklemeyi geri](web-sites-restore.md). Geri yükleme işlemi sağ şeyi yapar.
> 
> Tam yedekleme geri yüklendiğinde, sitedeki tüm içeriğin yedeklemeye ne olursa olsun ile değiştirilir. Bir sitede, ancak yedekleme dosyasıysa silinir. Ancak, kısmi yedekleme geri yüklendiğinde kara listede dizinlerden biri veya herhangi bir kara listede dosyanın içinde bulunan herhangi bir içerik olduğu gibi bırakılır.
> 


<a name="aboutbackups"></a>

## <a name="how-backups-are-stored"></a>Yedeklemeler nasıl depolanır
Uygulamanız için bir veya daha fazla yedekleme yaptıktan sonra yedeklemeleri görünür **kapsayıcıları** sayfasında depolama hesabınız ve uygulamanızı. Her yedekleme depolama hesabı'nda oluşan bir`.zip` yedekleme verilerini içeren dosyası ve bir `.xml` bir bildirimi içeren dosya `.zip` dosya içeriği. Sıkıştırmasını açın ve uygulama geri yükleme işlemini gerçekleştirmeden Yedeklemelerinizin erişmek istiyorsanız, bu dosyalara gözatın.

Uygulama için veritabanı yedeklemesi .zip dosyasının kök dizininde depolanır. Bir SQL veritabanı için bu bir BACPAC dosyası (dosya uzantısı yok) ve içeri aktarılabilir. BACPAC dışarı aktarma üzerinde dayalı bir SQL veritabanı oluşturmak için bkz: [yeni bir kullanıcı veritabanı oluşturmak için bir BACPAC dosyasını içeri](http://technet.microsoft.com/library/hh710052.aspx).

> [!WARNING]
> Tüm dosyaların değiştirilmesini, **websitebackups** kapsayıcı yedekleme geçersiz ve bu nedenle olmayan-geri yüklenebilen hale gelmesine neden olabilir.
> 
> 

## <a name="automate-with-scripts"></a>Betiklerle otomatikleştirme

Yedekleme yönetimi komut dosyaları ile kullanarak otomatikleştirebilirsiniz [Azure CLI](/cli/azure/install-azure-cli) veya [Azure PowerShell](/powershell/azure/overview).

Örnekler için bkz:

- [Azure CLI örnekleri](app-service-cli-samples.md)
- [Azure PowerShell örnekleri](app-service-powershell-samples.md)

<a name="nextsteps"></a>

## <a name="next-steps"></a>Sonraki Adımlar
Bir uygulama bir yedekten geri yükleme hakkında daha fazla bilgi için bkz: [Azure bir uygulamada geri](web-sites-restore.md). 


<!-- IMAGES -->
[ChooseBackupsPage]: ./media/web-sites-backup/01ChooseBackupsPage1.png
[ChooseStorageAccount]: ./media/web-sites-backup/02ChooseStorageAccount-1.png
[BackUpNow]: ./media/web-sites-backup/04BackUpNow1.png
[SetAutomatedBackupOn]: ./media/web-sites-backup/06SetAutomatedBackupOn1.png
[SaveIcon]: ./media/web-sites-backup/10SaveIcon.png
[ImagesFolder]: ./media/web-sites-backup/11Images.png
[LogsFolder]: ./media/web-sites-backup/12Logs.png
[GhostUpgradeWarning]: ./media/web-sites-backup/13GhostUpgradeWarning.png
[kudu-portal]:./media/web-sites-backup/kudu-portal.PNG

