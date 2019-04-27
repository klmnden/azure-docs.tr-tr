---
title: App - Azure uygulama hizmeti geri yükleme
description: Uygulamanızı yedekten geri yükleme konusunda bilgi edinin.
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
ms.custom: seodec18
ms.openlocfilehash: 1e8bebdb3f54ac59ec19ef798cc3e794473bbec0
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60832508"
---
# <a name="restore-an-app-in-azure"></a>Uygulamanızı Azure’a geri yükleme
Bu makalede, uygulamanızı geri yükleme işlemini göstermektedir [Azure App Service](../app-service/overview.md) önceden yedeklediğiniz (bkz [uygulamanızı Azure'a yedekleme](manage-backup.md)). Uygulamanızı isteğe bağlı olarak bağlı veritabanlarıyla birlikte önceki bir duruma geri yükleyebilir veya özgün uygulamanızın bir yedeğini temel alan yeni bir uygulama oluşturabilirsiniz. Azure App Service, yedekleme ve geri yükleme için şu veritabanlarını destekler:
- [SQL Veritabanı](https://azure.microsoft.com/services/sql-database/)
- [MySQL için Azure Veritabanı](https://azure.microsoft.com/services/mysql)
- [PostgreSQL için Azure Veritabanı](https://azure.microsoft.com/services/postgresql)
- [Uygulama içi MySQL](https://blogs.msdn.microsoft.com/appserviceteam/2017/03/06/announcing-general-availability-for-mysql-in-app)

Yedeklerden geri yükleme, çalışan uygulamalar için kullanılabilir **standart** ve **Premium** katmanı. Uygulamanızın ölçeğini genişletme hakkında daha fazla bilgi için bkz: [azure'da uygulamanın ölçeğini](web-sites-scale.md). **Premium** katmanı sağlayan daha gerçekleştirilecek günlük yedeklemeler daha fazla sayıda **standart** katmanı.

<a name="PreviousBackup"></a>

## <a name="restore-an-app-from-an-existing-backup"></a>Uygulamayı var olan bir yedekten geri yükleme
1. Üzerinde **ayarları** sayfası uygulamanızı Azure portalında, tıklayın **yedeklemeleri** görüntülenecek **yedeklemeleri** sayfası. Ardından **geri**.
   
    ![Şimdi Geri Yükle'yi seçin][ChooseRestoreNow]
2. İçinde **geri** sayfasında, ilk yedekleme kaynağını seçin.
   
    ![](./media/web-sites-restore/021ChooseSource1.png)
   
    **Uygulama yedeğini** seçeneği geçerli uygulamanın var olan tüm yedeklemeler gösterir ve bir kolayca seçebilirsiniz.
    **Depolama** seçeneği sayesinde var olan Azure depolama hesabı ve kapsayıcı, aboneliğinizdeki tüm yedekleme ZIP dosyası seçin.
    Başka bir uygulama yedeğini geri çalıştığınız kullanırsanız **depolama** seçeneği.
3. Ardından, uygulamayı geri yükleme hedefini belirtin **geri yükleme hedefini**.
   
    ![](./media/web-sites-restore/022ChooseDestination1.png)
   
   > [!WARNING]
   > Seçerseniz **üzerine yaz**, tüm geçerli uygulamanıza mevcut veriler silinir ve üzerine. Tıklamadan önce **Tamam**, bu tam olarak ne yapmak istiyorsunuz olduğundan emin olun.
   > 
   > 
   
   > [!WARNING]
   > Geri yüklediğiniz sırada App Service veritabanına veri yazıyor demektir, birincil anahtar ve veri kaybı ihlali gibi belirtileri sonuçlanabilir. Veritabanını geri yüklemek başlatmadan önce App Service önce durdurmanız önerilir.
   > 
   > 
   
    Seçebileceğiniz **var olan bir uygulamayı** aynı kaynak grubunda başka bir uygulama için uygulama yedeğini geri yüklemek için. Bu seçeneği kullanmadan önce başka bir uygulama kaynak grubunuzda bir uygulama yedeğini içinde tanımlanan yapılandırma veritabanı yansıtma ile oluşturmuş olmanız. Ayrıca oluşturabileceğiniz bir **yeni** içeriğinize geri yüklemek için uygulama.

4. **Tamam** düğmesine tıklayın.

<a name="StorageAccount"></a>

## <a name="download-or-delete-a-backup-from-a-storage-account"></a>İndirme veya bir depolama hesabından bir yedek silme
1. Ana **Gözat** sayfa seçin Azure portalının **depolama hesapları**. Var olan depolama hesaplarınızı listesi görüntülenir.
2. İndirme veya silmek istediğiniz yedeği içeren depolama hesabını seçin. Depolama hesabı için bir sayfa görüntülenir.
3. Depolama hesabı sayfasında istediğiniz kapsayıcıyı seçin
   
    ![Görünüm kapsayıcıları][ViewContainers]
4. İndirme veya silmek istediğiniz yedekleme dosyasını seçin.
   
    ![ViewContainers](./media/web-sites-restore/03ViewFiles.png)
5. Tıklayın **indirme** veya **Sil** yapmak istediğinize bağlı olarak.  

<a name="OperationLogs"></a>

## <a name="monitor-a-restore-operation"></a>Geri yükleme işlemi izleme
Başarı veya başarısızlık uygulama geri yükleme işleminin ayrıntılarını görmek için gidin **etkinlik günlüğü** Azure portalında sayfası.  
 

İstenen geri yükleme işlemi ve seçmek için tıklatın bulmak için aşağı kaydırın.

Ayrıntılar sayfası, geri yükleme işlemiyle ilgili mevcut bilgileri görüntüler.

## <a name="automate-with-scripts"></a>Betiklerle otomatikleştirme

Yedekleme Yönetimi betiklerle kullanarak otomatikleştirebilirsiniz [Azure CLI](/cli/azure/install-azure-cli) veya [Azure PowerShell](/powershell/azure/overview).

Örnekleri için bkz:

- [Azure CLI örnekleri](samples-cli.md)
- [Azure PowerShell örnekleri](samples-powershell.md)

<!-- ## Next Steps
You can backup and restore App Service apps using REST API. -->


<!-- IMAGES -->
[ChooseRestoreNow]: ./media/web-sites-restore/02ChooseRestoreNow1.png
[ViewContainers]: ./media/web-sites-restore/03ViewContainers.png
