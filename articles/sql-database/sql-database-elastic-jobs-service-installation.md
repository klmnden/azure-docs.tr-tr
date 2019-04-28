---
title: Elastik veritabanı işleri yükleme | Microsoft Docs
description: Elastik iş özelliğini yüklemesi yol.
services: sql-database
ms.service: sql-database
ms.subservice: scale-out
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
manager: craigg
ms.date: 12/04/2018
ms.openlocfilehash: 32df3e7ac6dc22e247bd4aecea4f39bf6d3a8017
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61475788"
---
# <a name="installing-elastic-database-jobs-overview"></a>Yükleme esnek veritabanı işlerine genel bakış

[!INCLUDE [elastic-database-jobs-deprecation](../../includes/sql-database-elastic-jobs-deprecate.md)]



[**Elastik veritabanı işleri** ](sql-database-elastic-jobs-overview.md) PowerShell aracılığıyla veya Azure portalı üzerinden yüklenebilir. Yalnızca PowerShell paketi yüklerseniz PowerShell API'sini kullanarak işleri oluşturmak ve yönetmek için erişim sağlayabilir. Ayrıca, PowerShell API'lerini sağlamak önemli ölçüde daha fazla işlevsellik portal bu anda.

Zaten yüklediyseniz **elastik veritabanı işleri** mevcut bir Portal üzerinden **elastik havuz**, en son PowerShell Önizleme var olan yüklemeyi yükseltmek için komut dosyalarını içerir. Yüklemenizin en son sürüme yükseltmek için önemle tavsiye edilir **elastik veritabanı işleri** PowerShell API'leri kullanıma sunulan yeni işlevsellikten yararlanmak için bileşenleri.

## <a name="prerequisites"></a>Önkoşullar
* Azure aboneliği. Ücretsiz deneme için bkz: [ücretsiz deneme sürümü](https://azure.microsoft.com/pricing/free-trial/).
* Azure PowerShell. En son sürümünü kullanarak yüklemeniz [Web Platformu yükleyicisi](https://go.microsoft.com/fwlink/p/?linkid=320376). Ayrıntılı bilgi için bkz. [Azure PowerShell'i yükleme ve yapılandırma](/powershell/azure/overview).
* [NuGet komut satırı yardımcı programı](https://nuget.org/nuget.exe) elastik veritabanı işleri paketini yüklemek için kullanılır. Daha fazla bilgi için bkz. https://docs.nuget.org/docs/start-here/installing-nuget.

## <a name="download-and-import-the-elastic-database-jobs-powershell-package"></a>İndirme ve elastik veritabanı işleri PowerShell paketi içeri aktarma
1. Microsoft Azure PowerShell komut penceresini başlatın ve NuGet komut satırı yardımcı programı (nuget.exe) karşıdan yüklediğiniz dizine gidin.
2. İndirme ve içeri aktarma **elastik veritabanı işleri** pakete aşağıdaki komutla geçerli dizin:
   
        PS C:\>.\nuget install Microsoft.Azure.SqlDatabase.Jobs -prerelease
   
    **Elastik veritabanı işleri** adlı bir klasörde yerel dizindeki dosyalar yerleştirilir **Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x** burada *x.x.xxxx.x* yansıtır sürüm numarası. (Gerekli istemci DLL'ler dahil) PowerShell cmdlet'leri bulunur **tools\ElasticDatabaseJobs** alt dizini ve yüklemek, yükseltmek ve kaldırmak için PowerShell komut dosyalarını da bulunan **araçları** alt dizini.
3. Cd araçları, örneğin yazarak Microsoft.Azure.SqlDatabase.Jobs.x.x.xxx.x klasörü altındaki Araçlar alt dizinine gidin:
   
        PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*>cd tools

4. ElasticDatabaseJobs dizini $home\Documents\WindowsPowerShell\Modules kopyalanacak.\InstallElasticDatabaseJobsCmdlets.ps1 betiği yürütün. Bu da otomatik olarak kullanım için modül örneğin alınır:
   
        PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools>Unblock-File .\InstallElasticDatabaseJobsCmdlets.ps1
        PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools>.\InstallElasticDatabaseJobsCmdlets.ps1

## <a name="install-the-elastic-database-jobs-components-using-powershell"></a>PowerShell kullanarak elastik veritabanı işleri bileşenlerini yükleme
1. Microsoft Azure PowerShell komut penceresini başlatın ve \tools Microsoft.Azure.SqlDatabase.Jobs.x.x.xxx.x klasörü altında alt dizinine gidin: CD \tools yazın
   
        PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*>cd tools

2. PowerShell Betiği.\InstallElasticDatabaseJobs.ps1 yürütün ve istenen değişkenlerini için değerler sağlayın. Bu betik içinde açıklanan bileşenlere oluşturacak [elastik veritabanı işleri bileşenleri ve fiyatlandırma](sql-database-elastic-jobs-overview.md#components-and-pricing) bağımlı bileşenleri uygun şekilde kullanmak için Azure bulut hizmeti yapılandırma yanı sıra.

        PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools>Unblock-File .\InstallElasticDatabaseJobs.ps1
        PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools>.\InstallElasticDatabaseJobs.ps1

Bu komutu çalıştırdığınızda bir pencere açılır girmenizi isteyen bir **kullanıcı adı** ve **parola**. Azure kimlik bilgilerinizi değil, kullanıcı adını girin ve sonra da yeni sunucu için oluşturmak istediğiniz yönetici kimlik bilgileri parola.

Bu örnek çağrıda sağlanan parametreleri için istenen ayarlarınız değiştirilebilir. Aşağıdaki her bir parametre davranışı üzerinde daha fazla bilgi sağlar:

<table style="width:100%">
  <tr>
    <th>Parametre</th>
    <th>Açıklama</th>
  </tr>

<tr>
    <td>ResourceGroupName</td>
    <td>Yeni oluşturulan Azure bileşenlerini içerecek şekilde oluşturulan Azure kaynak grubu adı sağlar. Bu parametre, varsayılan olarak "__ElasticDatabaseJob için". Bu değeri değiştirmeniz önerilmez.</td>
    </tr>

<tr>
    <td>ResourceGroupLocation</td>
    <td>Yeni oluşturulan Azure bileşenleri için kullanılacak Azure konumdur. Bu parametre, Orta ABD konumu varsayılan olarak.</td>
</tr>

<tr>
    <td>ServiceWorkerCount</td>
    <td>Yüklenecek hizmet çalışanların sayısını sağlar. Bu parametre, varsayılan olarak 1. Daha fazla çalışan sayısı, hizmetin ölçeğini genişletin ve yüksek kullanılabilirlik sağlamak için kullanılabilir. "2" hizmetinin yüksek kullanılabilirlik gerektiren dağıtımları için kullanmak üzere önerilir.</td>
</tr>

<tr>
    <td>ServiceVmSize</td>
    <td>Bulut hizmeti dahilinde kullanım için VM boyutu sağlar. Bu parametre için A0 varsayar. Parametre değerlerini... /.. / A3 kabul edildiği bir çok küçük/küçük/Orta/büyük boyutu sırasıyla kullanılacak çalışan rolü neden. Çalışan rolü boyutları hakkında daha fazla bilgi için bkz. <a href="https://docs.microsoft.com/azure/sql-database/sql-database-elastic-jobs-overview#components-and-pricing">elastik veritabanı işleri bileşenleri ve fiyatlandırma</a>.</td>
</tr>

<tr>
    <td>SqlServerDatabaseSlo</td>
    <td>İşlem boyutu Standard bir sürümü sağlar. Bu parametre için S0 varsayar. Parametre değerlerini... /.. /.. /.. / S9/S12 kabul edildiği ilgili işlem boyutu kullanmak Azure SQL veritabanı neden. SQL veritabanı işlem boyutları hakkında daha fazla bilgi için bkz. <a href="https://docs.microsoft.com/azure/sql-database/sql-database-elastic-jobs-overview#components-and-pricing">elastik veritabanı işleri bileşenleri ve fiyatlandırma</a>.</td>
</tr>

<tr>
    <td>SqlServerAdministratorUserName</td>
    <td>Yeni oluşturulan Azure SQL veritabanı sunucusu için yönetici kullanıcı adı sağlar. Belirtilmediğinde, kimlik bilgileri istemek için bir PowerShell kimlik bilgileri penceresi açılır.</td>
</tr>

<tr>
    <td>SqlServerAdministratorPassword</td>
    <td>Yeni oluşturulan Azure SQL veritabanı sunucusu için yönetici parolasını sağlar. Sağlanan değil, kimlik bilgilerini soracak şekilde bir PowerShell kimlik bilgileri penceresi açılır.</td>
</tr>
</table>

Çok sayıda paralel çok sayıda veritabanı karşı çalışan işleri sahip hedef sistemleri için parametreleri belirtmek için önerilir: - ServiceWorkerCount 2 - ServiceVmSize A2 - SqlServerDatabaseSlo S2.

    PS C:\*Microsoft.Azure.SqlDatabase.Jobs.dll.x.x.xxx.x*\tools>Unblock-File .\InstallElasticDatabaseJobs.ps1
    PS C:\*Microsoft.Azure.SqlDatabase.Jobs.dll.x.x.xxx.x*\tools>.\InstallElasticDatabaseJobs.ps1 -ServiceWorkerCount 2 -ServiceVmSize A2 -SqlServerDatabaseSlo S2

## <a name="update-an-existing-elastic-database-jobs-components-installation-using-powershell"></a>PowerShell kullanarak var olan esnek veritabanı işleri bileşenleri yüklemesi güncelleştir
**Elastik veritabanı işleri** ölçeklendirme ve yüksek kullanılabilirlik için varolan bir yükleme içine güncelleştirilebilir. Bu işlem, hizmet kodunu gelecekteki yükseltmeler için bırakın ve denetimi veritabanı yeniden oluşturmak zorunda kalmadan sağlar. Hizmetinin VM boyutunu veya sunucu çalışan sayısını değiştirmek için bu işlem ayrıca aynı sürümü içinde kullanılabilir.

Bir yükleme VM boyutunu güncelleştirmek için seçtiğiniz değerlere güncelleştirilmiş parametrelerle aşağıdaki betiği çalıştırın.

    PS C:\*Microsoft.Azure.SqlDatabase.Jobs.dll.x.x.xxx.x*\tools>Unblock-File .\UpdateElasticDatabaseJobs.ps1
    PS C:\*Microsoft.Azure.SqlDatabase.Jobs.dll.x.x.xxx.x*\tools>.\UpdateElasticDatabaseJobs.ps1 -ServiceVmSize A1 -ServiceWorkerCount 2

<table style="width:100%">
  <tr>
  <th>Parametre</th>
  <th>Açıklama</th>
</tr>

<tr>
    <td>ResourceGroupName</td>
    <td>Elastik veritabanı iş bileşenler ilk yüklendiğinde kullanılan Azure kaynak grubu adını tanımlar. Bu parametre, varsayılan olarak "__ElasticDatabaseJob için". Bu değeri değiştirmeniz önerilmez olduğundan, bu parametre belirtmeniz gerekmez.</td>
</tr>

<tr>
    <td>ServiceWorkerCount</td>
    <td>Yüklenecek hizmet çalışanların sayısını sağlar.  Bu parametre, varsayılan olarak 1.  Daha fazla çalışan sayısı, hizmetin ölçeğini genişletin ve yüksek kullanılabilirlik sağlamak için kullanılabilir.  "2" hizmetinin yüksek kullanılabilirlik gerektiren dağıtımları için kullanmak üzere önerilir.</td>
</tr>

<tr>
    <td>ServiceVmSize</td>
    <td>Bulut hizmeti dahilinde kullanım için VM boyutu sağlar. Bu parametre için A0 varsayar. Parametre değerlerini... /.. / A3 kabul edildiği bir çok küçük/küçük/Orta/büyük boyutu sırasıyla kullanılacak çalışan rolü neden. Çalışan rolü boyutları hakkında daha fazla bilgi için bkz. <a href="https://docs.microsoft.com/azure/sql-database/sql-database-elastic-jobs-overview#components-and-pricing">elastik veritabanı işleri bileşenleri ve fiyatlandırma</a>.</td>
</tr>

</table>

## <a name="install-the-elastic-database-jobs-components-using-the-portal"></a>Portalı kullanarak elastik veritabanı işleri bileşenlerini yükleme
Yapılandırmasını tamamladıktan [elastik havuz oluşturulan](sql-database-elastic-pool-manage-portal.md), yükleyebileceğiniz **elastik veritabanı işleri** elastik havuzdaki her bir veritabanında yönetici görevleri yürütülmesini etkinleştirmek için bileşenleri. Ne zaman aksine kullanarak **elastik veritabanı işleri** PowerShell API'lerini portal arabirimi şu anda yalnızca mevcut havuzlardan karşı yürütülen kısıtlanmış.

**Tahmini tamamlanma süresi:** 10 dakika.

1. Pano görünümü bir esnek havuzun [Azure portalında](https://portal.azure.com/#) , tıklayın **oluşturma işi**.
2. Bir işi ilk kez oluşturuyorsanız, yüklemelisiniz **elastik veritabanı işleri** tıklayarak **Önizleme koşullarını**.
3. Onay kutusuna tıklayarak koşullarını kabul edin.
4. "Hizmetlerini yükleyin" görünümüne tıklayın **iş kimlik bilgileri**.
   
    ![Hizmetler yükleniyor][1]
5. Bir kullanıcı adı ve bir veritabanı yöneticisi parolasını girin Yeni bir Azure SQL veritabanı sunucusu yüklemesinin bir parçası olarak oluşturulur. Bu yeni sunucu içinde denetim veritabanı olarak bilinen yeni bir veritabanı oluşturulur ve elastik veritabanı işleri için meta veri kapsamak için kullanılmış. Kullanıcı adını ve parolasını burada oluşturulan denetimi veritabanında oturum açma amacıyla kullanılır. Ayrı bir kimlik bilgisi, havuzdaki veritabanlarının karşı betiğin yürütülmesi için kullanılır.
   
    ![Kullanıcı adı ve parola oluşturun][2]
6. Tamam düğmesine tıklayın. Bileşenleri sizin için birkaç dakika sonra yeni oluşturulan [kaynak grubu](../azure-resource-manager/resource-group-overview.md). Yeni kaynak grubu, aşağıda gösterildiği gibi başlangıç panosuna sabitlenir. Oluşturulan, elastik veritabanı işleri (bulut hizmeti, SQL veritabanı, Service Bus ve depolama) tüm grubunda oluşturulduktan sonra.
   
    ![Başlangıç Panosu kaynak grubunda][3]
7. Oluşturmak veya elastik veritabanı işleri yüklüyor ancak sağlanırken bir işi yönetmek çalışırsanız **kimlik bilgilerini** aşağıdaki iletiyi görürsünüz.
   
    ![Dağıtımı hala devam ediyor][4]

Kaldırma işlemi gerekiyorsa, kaynak grubunu silin. Bkz: [elastik veritabanı iş bileşenleri kaldırma](sql-database-elastic-jobs-uninstall.md).

## <a name="next-steps"></a>Sonraki adımlar
Betik yürütme grubunda daha fazla bilgi için her bir veritabanı oluşturulur için uygun haklara sahip bir kimlik bilgisi sağlamak [SQL veritabanınızın güvenliğini sağlama](sql-database-manage-logins.md).
Bkz: [oluşturma ve elastik veritabanı işleri yönetme](sql-database-elastic-jobs-create-and-manage.md) kullanmaya başlamak için.

<!--Image references-->
[1]: ./media/sql-database-elastic-jobs-service-installation/screen-1.png
[2]: ./media/sql-database-elastic-jobs-service-installation/credentials.png
[3]: ./media/sql-database-elastic-jobs-service-installation/start-board.png
[4]: ./media/sql-database-elastic-jobs-service-installation/not-done.png
