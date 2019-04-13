---
title: -Azure App Service uygulamasını yedekleme
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
ms.custom: seodec18
ms.openlocfilehash: 7e697329e83b530157e490b04f5155d28d243bb6
ms.sourcegitcommit: 031e4165a1767c00bb5365ce9b2a189c8b69d4c0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2019
ms.locfileid: "59549497"
---
# <a name="back-up-your-app-in-azure"></a>Uygulamanızı Azure’a yedekleme
Yedekleme ve geri yükleme özelliği [Azure App Service](overview.md) uygulama yedeklerini el ile veya bir zamanlamaya göre kolayca oluşturmanıza olanak sağlar. Uygulama mevcut uygulamanın üzerine yazarak veya başka bir uygulamaya geri önceki bir durumun anlık görüntüye geri yükleyebilirsiniz. 

Bir uygulama yedekten geri yükleme hakkında daha fazla bilgi için bkz: [uygulamanızı Azure'a geri](web-sites-restore.md).

<a name="whatsbackedup"></a>

## <a name="what-gets-backed-up"></a>Ne yedeklenir
App Service aşağıdaki bilgileri, bir Azure depolama hesabı ve kullanmak için uygulamanızı yapılandırdığınız kapsayıcı yedekleyebilirsiniz. 

* Uygulama yapılandırması
* Dosya içeriği
* Uygulamanıza bağlı veritabanı

Aşağıdaki veritabanı çözümleri ile yedekleme özelliği desteklenir: 
   - [SQL Veritabanı](https://azure.microsoft.com/services/sql-database/)
   - [MySQL için Azure Veritabanı](https://azure.microsoft.com/services/mysql)
   - [PostgreSQL için Azure Veritabanı](https://azure.microsoft.com/services/postgresql)
   - [Uygulama içi MySQL](https://blogs.msdn.microsoft.com/appserviceteam/2017/03/06/announcing-general-availability-for-mysql-in-app)
 

> [!NOTE]
>  Her bir uygulama, bir Artımlı güncelleştirme çevrimdışı tam kopyasıdır.
>  

<a name="requirements"></a>

## <a name="requirements-and-restrictions"></a>Gereksinimler ve kısıtlamalar
* Yedekleme ve geri yükleme özelliği olduğu App Service planı gerektirir **standart** katmanı veya **Premium** katmanı. App Service planınızı daha yüksek bir katmana kullanmak üzere ölçeklendirme hakkında daha fazla bilgi için bkz. [azure'da uygulamanın ölçeğini](web-sites-scale.md).  
  **Premium** katmanı sağlayan daha fazla sayıda günlük yedekleme değerinden çıkarır **standart** katmanı.
* Bir Azure depolama hesabı ve kapsayıcı yedeklemek istediğiniz uygulama ile aynı abonelikte ihtiyacınız var. Azure depolama hesapları hakkında daha fazla bilgi için bkz. [Azure depolama hesabına genel bakış](https://docs.microsoft.com/azure/storage/common/storage-account-overview).
* Yedekleme, uygulama ve veritabanı içeriğinin 10 GB'a kadar olabilir. Yedekleme boyutu bu sınırı aşarsa, bir hata alırsınız.
* MySQL desteklenmiyor için Azure veritabanı yedeklerini SSL etkin. Bir yedekleme yapılandırılmışsa, başarısız olmuş yedeklemeler alırsınız.
* PostgreSQL desteklenmiyor için Azure veritabanı yedeklerini SSL etkin. Bir yedekleme yapılandırılmışsa, başarısız olmuş yedeklemeler alırsınız.
* Uygulama içi MySQL veritabanları herhangi bir yapılandırma otomatik olarak yedeklenir. Uygulama içi MySQL veritabanları için bağlantı dizelerini ekleme gibi ayarları el ile yaptığınız yedeklemeleri düzgün çalışmayabilir.
* Hedef yedeklemeleriniz için desteklenmediğinden, bir Güvenlik Duvarı'nı kullanarak depolama hesabı etkin. Bir yedekleme yapılandırılmışsa, başarısız olmuş yedeklemeler alırsınız.


<a name="manualbackup"></a>

## <a name="create-a-manual-backup"></a>El ile yedekleme oluşturun
1. İçinde [Azure portalında](https://portal.azure.com), uygulamanızın sayfasına gidin, seçin **yedeklemeleri**. **Yedeklemeleri** sayfası görüntülenir.
   
    ![Yedeklemeleri sayfası][ChooseBackupsPage]
   
   > [!NOTE]
   > Aşağıdaki iletiyi görürseniz, yedeklemeleri ile devam etmeden önce App Service planınızı yükseltmek için tıklayın.
   > Daha fazla bilgi için [azure'da uygulamanın ölçeğini](web-sites-scale.md).  
   > ![Depolama hesabı seçin](./media/web-sites-backup/01UpgradePlan1.png)
   > 
   > 

2. İçinde **yedekleme** sayfası, tıklayın **yapılandırma**
![Yapılandır'a tıklayın](./media/web-sites-backup/ClickConfigure1.png)
3. İçinde **yedekleme yapılandırması** sayfasında **Depolama: Yapılandırılmamış** bir depolama hesabı yapılandırmak için.
   
    ![Depolama hesabı seç][ChooseStorageAccount]
4. Yedekleme Hedefinizi seçerek bir **depolama hesabı** ve **kapsayıcı**. Depolama hesabının, yedeklemek istediğiniz uygulama ile aynı aboneliğe ait olmalıdır. İsterseniz ilgili sayfaları'nda yeni bir depolama hesabı veya yeni bir kapsayıcı oluşturabilirsiniz. İşiniz bittiğinde tıklayın **seçin**.
   
    ![Depolama hesabı seç](./media/web-sites-backup/02ChooseStorageAccount1-1.png)
5. İçinde **yedekleme yapılandırması** hala açık bırakılan sayfasında yapılandırabilir **Backup Database**, ardından (SQL veritabanı veya MySQL) yedeklemeler, eklemek istediğiniz veritabanlarını seçin ve ardından tıklayın**Tamam**.  
   
    ![Depolama hesabı seç](./media/web-sites-backup/03ConfigureDatabase1.png)
   
   > [!NOTE]
   > Bu listede görünmesi için bir veritabanı için bağlantı dizesini içinde bulunmalıdır **bağlantı dizeleri** bölümünü **uygulama ayarları** uygulamanız için sayfa. 
   >
   > Uygulama içi MySQL veritabanları herhangi bir yapılandırma otomatik olarak yedeklenir. Uygulama içi MySQL veritabanları için bağlantı dizelerini ekleme gibi ayarları el ile yaptığınız yedeklemeleri düzgün çalışmayabilir.
   > 
   > 
6. İçinde **yedekleme yapılandırması** sayfasında **Kaydet**.    
7. İçinde **yedeklemeleri** sayfasında **yedekleme**.
   
    ![BackUpNow düğmesi][BackUpNow]
   
    Yedekleme işlemi sırasında bir ilerleme iletisi görürsünüz.

Depolama hesabı ve kapsayıcı yapılandırıldıktan sonra istediğiniz zaman el ile yedekleme başlatabilir.  

<a name="automatedbackups"></a>

## <a name="configure-automated-backups"></a>Otomatik yedeklemeleri yapılandırma
1. İçinde **yedekleme yapılandırması** sayfasında **zamanlanmış yedekleme** için **üzerinde**. 
   
    ![Depolama hesabı seç](./media/web-sites-backup/05ScheduleBackup1.png)
2. Yedekleme zamanlama seçeneklerini gösterir, **zamanlanmış yedekleme** için **üzerinde**, ardından yedekleme zamanlaması istediğiniz gibi yapılandırın ve tıklayın **Tamam**.
   
    ![Otomatik yedeklemeleri etkinleştirin][SetAutomatedBackupOn]

<a name="partialbackups"></a>

## <a name="configure-partial-backups"></a>Kısmi yedeklemeleri yapılandırma
Bazen her şeyi uygulamanızı yedekle istemezsiniz. İşte birkaç örnek:

* [Haftalık yedeklemeler ayarlayabilir](#configure-automated-backups) uygulamanızın statik içeriği olan hiçbir zaman, eski blog gönderilerini veya görüntüleri gibi değiştirir.
* Uygulamanızın üzerinde 10 GB'a kadar (yani aynı anda yedekleyebilirsiniz maksimum tutar) vardır.
* Günlük dosyalarını yedeklemek istemediğiniz.

Kısmi yedeklerin tam olarak yedeklemek istediğiniz dosyaları seçin izin verir.

> [!NOTE]
> En fazla 4 GB yedekleme bulunan tek veritabanlarını olabilir ancak yedekleme toplam en büyük boyutu 10 GB'tır

### <a name="exclude-files-from-your-backup"></a>Yedekleme dosyaları dışarıda bırak
Günlük dosyaları ve bir kez yedekleme atanmış olması ve değiştirmek için yapmayacağınız statik görüntüler içeren bir uygulama olduğunu varsayalım. Böyle durumlarda, gelecekteki yedeklemeler içinde depolanan bu klasörleri ve dosyaları dışlayabilirsiniz. Yedeklemelerinizi dosya ve klasörleri dışlamak için oluşturma bir `_backup.filter` dosyası `D:\home\site\wwwroot` uygulamanızın klasör. Dosya ve klasörleri bu dosyayı çıkarmak istediğiniz listesini belirtin. 

Dosyalarınıza erişmek için kolay bir yol, Kudu kullanmaktır. Tıklayın **Gelişmiş Araçlar -> Git** Kudu erişmek web uygulamanız için ayarlama.

![Portalı kullanarak kudu][kudu-portal]

Yedeklemelerinizi dışlamak istediğiniz klasörleri belirleyin.  Örneğin, vurgulanan klasör ve dosyaları filtrelemek istersiniz.

![Resimler klasöründeki][ImagesFolder]

Adlı bir dosya oluşturun `_backup.filter` ve yukarıdaki listede dosyasına yerleştirir, ancak kaldırma `D:\home`. Bir dizin veya dosya her satırda listeleyin. Bu nedenle dosyanın içeriği olmalıdır:
 ```bash
    \site\wwwroot\Images\brand.png
    \site\wwwroot\Images\2014
    \site\wwwroot\Images\2013
```

Karşıya yükleme `_backup.filter` dosyasını `D:\home\site\wwwroot\` kullanarak site dizininde [ftp](deploy-ftp.md) veya başka bir yöntem. İsterseniz, doğrudan Kudu kullanarak bu dosyayı oluşturabilirsiniz `DebugConsole` ve orada içeriği ekleyin.

Aynı şekilde, normalde yaptığınız, yedeklemeleri çalıştırma [el ile](#create-a-manual-backup) veya [otomatik olarak](#configure-automated-backups). Şimdi, dosya ve klasörleri belirtilen `_backup.filter` zamanlanmış veya el ile başlatma gelecekteki yedeklemelerin çıkarılır. 

> [!NOTE]
> Yaptığınız şekilde sitenizin kısmi yedekleri geri [normal bir yedeklemeyi geri](web-sites-restore.md). Geri yükleme işleminin doğru şeyi yapar.
> 
> Bir yedekten geri yüklendiğinde, tüm site içeriğini yedeklemeye ne olursa olsun ile değiştirilir. Bir sitede, ancak yedekleme dosyasıysa silinir. Ancak, kısmi yedekleme geri yüklendiğinde kara listeye alınan dizinlerden biri veya herhangi bir kara listeye alınan dosya bulunan herhangi bir içerik olduğu gibi bırakılır.
> 


<a name="aboutbackups"></a>

## <a name="how-backups-are-stored"></a>Yedeklemeler nasıl depolanır
Uygulamanız için bir veya daha fazla yedekleme yaptıktan sonra yedeklemeleri görünür **kapsayıcıları** sayfasında, depolama hesabınızı ve uygulamanızı. Her yedekleme oluşur depolama hesabında bir`.zip` yedekleme verilerini içeren dosyanın ve `.xml` içeren bir bildirimi dosyası `.zip` dosya içeriği. Sıkıştırmasını açın ve bir uygulamayı geri yükleme işlemi yapmadan yedeklemeleriniz erişmek istiyorsanız, bu dosyalara göz atın.

Uygulama için bir veritabanı yedeği .zip dosyasının kök dizininde depolanır. Bir SQL veritabanı için bir BACPAC dosyası (dosya uzantısı) ve bu içeri aktarılabilir. Bir SQL veritabanının BACPAC dışarı aktarma üzerinde oluşturmak için bkz [yeni bir kullanıcı veritabanı oluşturmak için bir BACPAC dosyasını içeri](https://technet.microsoft.com/library/hh710052.aspx).

> [!WARNING]
> Tüm dosyaların değiştirilmesi, **websitebackups** kapsayıcı geçersiz ve bu nedenle olmayan-geri yüklenebilen olmak yedekleme neden olabilir.
> 
> 

## <a name="automate-with-scripts"></a>Betiklerle otomatikleştirme

Yedekleme Yönetimi betiklerle kullanarak otomatikleştirebilirsiniz [Azure CLI](/cli/azure/install-azure-cli) veya [Azure PowerShell](/powershell/azure/overview).

Örnekleri için bkz:

- [Azure CLI örnekleri](samples-cli.md)
- [Azure PowerShell örnekleri](samples-powershell.md)

<a name="nextsteps"></a>

## <a name="next-steps"></a>Sonraki Adımlar
Bir uygulama bir yedekten geri yükleme hakkında daha fazla bilgi için bkz: [uygulamanızı Azure'a geri](web-sites-restore.md). 


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

