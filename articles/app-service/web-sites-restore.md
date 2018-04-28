---
title: Uygulamanızı Azure’a geri yükleme
description: Uygulamanızı bir yedek kopyadan geri öğrenin.
services: app-service
documentationcenter: ''
author: cephalin
manager: erikre
editor: jimbe
ms.assetid: 4444dbf7-363c-47e2-b24a-dbd45cb08491
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/06/2016
ms.author: cephalin
ms.openlocfilehash: 5c37a693e1594f9afe40effba45d7b05a68fd49c
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="restore-an-app-in-azure"></a>Uygulamanızı Azure’a geri yükleme
Bu makalede, bir uygulamada geri yükleme gösterilmektedir [Azure App Service](../app-service/app-service-web-overview.md) önceden yedeklediğiniz (bkz [uygulamanızı Azure yedekleme](web-sites-backup.md)). Uygulamanızı kendi bağlantılı veritabanı isteğe bağlı bir önceki durumuna geri yüklemek veya özgün uygulamanızın yedekleri birini temel alan yeni bir uygulama oluşturun. Azure uygulama hizmeti, yedekleme ve geri yükleme için aşağıdaki veritabanlarını destekler:
- [SQL Database](https://azure.microsoft.com/services/sql-database/)
- [Azure veritabanı MySQL için)](https://azure.microsoft.com/services/mysql)
- [PostgreSQL için Azure Veritabanı](https://azure.microsoft.com/services/postgres)
- [Uygulama MySQL](https://blogs.msdn.microsoft.com/appserviceteam/2017/03/06/announcing-general-availability-for-mysql-in-app)

Yedeklerden geri yükleme, çalışan uygulamalar için kullanılabilir **standart** ve **Premium** katmanı. Uygulamanızı ölçeklendirme hakkında daha fazla bilgi için bkz: [Azure bir uygulamada ölçeklendirin](web-sites-scale.md). **Premium** katmanı sağlayan daha gerçekleştirilecek günlük yedeklemeler daha fazla sayıda **standart** katmanı.

<a name="PreviousBackup"></a>

## <a name="restore-an-app-from-an-existing-backup"></a>Bir uygulama olan bir yedekten geri yükleme
1. Üzerinde **ayarları** sayfa Azure portalında uygulamanızı tıklatın **yedeklemeleri** görüntülemek için **yedeklemeleri** sayfası. Ardından **geri**.
   
    ![Şimdi Geri Yükle'yi seçin][ChooseRestoreNow]
2. İçinde **geri** sayfasında, ilk yedekleme kaynak seçin.
   
    ![](./media/web-sites-restore/021ChooseSource1.png)
   
    **Uygulama yedekleme** seçeneği geçerli uygulamanın tüm var olan yedekleri gösterir ve kolayca birini seçebilirsiniz.
    **Depolama** seçeneği mevcut Azure depolama hesabı ve kapsayıcı aboneliğinizde tüm yedekleme ZIP dosyası seçin olanak sağlar.
    Başka bir uygulama bir yedeğini geri yüklemek çalışıyorsanız, kullanın **depolama** seçeneği.
3. Ardından, uygulama geri yükleme hedefini belirtin **geri yükleme hedefini**.
   
    ![](./media/web-sites-restore/022ChooseDestination1.png)
   
   > [!WARNING]
   > Seçerseniz **üzerine yaz**, tüm geçerli uygulamanızda mevcut verileri silinir ve üzerine. Tıklamadan önce **Tamam**, onu tam olarak ne yapmak istiyorsunuz olduğundan emin olun.
   > 
   > 
   
    Seçebileceğiniz **var olan bir uygulamayı** aynı kaynak kaydının gruptaki başka bir uygulama için uygulama yedeklemeyi geri yükleme. Bu seçeneği kullanmadan önce zaten başka bir uygulama bir uygulama yedekleme tanımlanan veritabanı yapılandırması yansıtma ile kaynak grubu oluşturmuş olmalıdır. Ayrıca bir **yeni** içeriğinize geri yüklemek için uygulama.

4. **Tamam**’a tıklayın.

<a name="StorageAccount"></a>

## <a name="download-or-delete-a-backup-from-a-storage-account"></a>İndirme veya depolama hesabından bir yedek Sil
1. Ana **Gözat** select Azure portalının sayfasında **depolama hesapları**. Varolan depolama hesaplarınızı listesi görüntülenir.
2. İndirme veya silmek istediğiniz yedeği içeren depolama hesabı seçin. Depolama hesabı için sayfası görüntülenir.
3. Depolama hesabı sayfasında istediğiniz kapsayıcıyı seçin
   
    ![Görünüm kapsayıcıları][ViewContainers]
4. İndirme veya silmek istediğiniz yedekleme dosyasını seçin.
   
    ![ViewContainers](./media/web-sites-restore/03ViewFiles.png)
5. Tıklatın **karşıdan** veya **silmek** yapmak istediğinize bağlı olarak.  

<a name="OperationLogs"></a>

## <a name="monitor-a-restore-operation"></a>İzleyici bir geri yükleme işlemi
Başarı veya başarısızlık uygulama geri yükleme işleminin ayrıntılarını görmek için gidin **etkinlik günlüğü** Azure portalında sayfası.  
 

İstenen geri yükleme işlemi ve seçmek için tıklatın bulmak için aşağı kaydırın.

Ayrıntılar sayfası geri yükleme işlemiyle ilgili mevcut bilgileri görüntüler.

## <a name="automate-with-scripts"></a>Betiklerle otomatikleştirme

Yedekleme yönetimi komut dosyaları ile kullanarak otomatikleştirebilirsiniz [Azure CLI](/cli/azure/install-azure-cli) veya [Azure PowerShell](/powershell/azure/overview).

Örnekler için bkz:

- [Azure CLI örnekleri](app-service-cli-samples.md)
- [Azure PowerShell örnekleri](app-service-powershell-samples.md)

<!-- ## Next Steps
You can backup and restore App Service apps using REST API. -->


<!-- IMAGES -->
[ChooseRestoreNow]: ./media/web-sites-restore/02ChooseRestoreNow1.png
[ViewContainers]: ./media/web-sites-restore/03ViewContainers.png
