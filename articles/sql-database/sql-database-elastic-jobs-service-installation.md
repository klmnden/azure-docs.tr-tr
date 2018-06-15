---
title: Esnek veritabanı işleri yükleme | Microsoft Docs
description: Esnek iş özellik yüklenmesinde size yol gösterir.
services: sql-database
manager: craigg
author: ddove
ms.service: sql-database
ms.custom: scale out apps
ms.topic: conceptual
ms.date: 04/01/2018
ms.author: sstein
ms.openlocfilehash: 5760ca693f347068e03770b348d88b3b2adbf678
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34645621"
---
# <a name="installing-elastic-database-jobs-overview"></a>Yükleme esnek veritabanı işleri genel bakış
[**Esnek veritabanı iş** ](sql-database-elastic-jobs-overview.md) PowerShell aracılığıyla veya Azure portalı üzerinden yüklenebilir. Yalnızca PowerShell paketi yüklerseniz PowerShell API'yi kullanarak işleri oluşturmak ve yönetmek için erişim sağlayabilir. Ayrıca, PowerShell API'lerinden sağlayın portal önemli ölçüde daha fazla işlevsellik bu anda.

Zaten yüklediyseniz, **esnek veritabanı işleri** varolan bir Portal üzerinden **esnek havuz**, en son Powershell Önizleme, mevcut yüklemenizi yükseltmek için komut dosyalarını içerir. Yüklemenizin en son sürüme yükseltmek için önerilir **esnek veritabanı işleri** PowerShell API'leri kullanıma sunulan yeni işlevsellikten yararlanmak için bileşenleri.

## <a name="prerequisites"></a>Önkoşullar
* Azure aboneliği. Ücretsiz deneme için bkz: [ücretsiz deneme sürümü](https://azure.microsoft.com/pricing/free-trial/).
* Azure PowerShell. En son sürümünü kullanarak yüklemeniz [Web Platformu yükleyicisi](http://go.microsoft.com/fwlink/p/?linkid=320376). Ayrıntılı bilgi için bkz. [Azure PowerShell'i yükleme ve yapılandırma](/powershell/azure/overview).
* [NuGet komut satırı yardımcı programı](https://nuget.org/nuget.exe) esnek veritabanı işleri paketini yüklemek için kullanılır. Daha fazla bilgi için bkz: http://docs.nuget.org/docs/start-here/installing-nuget.

## <a name="download-and-import-the-elastic-database-jobs-powershell-package"></a>Karşıdan yükle ve esnek veritabanı işleri PowerShell paketi Al
1. Microsoft Azure PowerShell komut penceresini başlatın ve NuGet komut satırı yardımcı programı (nuget.exe) indirdiğiniz dizine gidin.
2. İndirme ve içeri aktarma **esnek veritabanı işleri** geçerli dizin aşağıdaki komutu kullanarak pakete:
   
        PS C:\>.\nuget install Microsoft.Azure.SqlDatabase.Jobs -prerelease
   
    **Esnek veritabanı işleri** dosyaları adlı bir klasör yerel dizininde yerleştirilir **Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x** nerede *x.x.xxxx.x* sürüm numarasını yansıtır. (Gerekli istemci .dll dahil) PowerShell cmdlet'leri bulunur **tools\ElasticDatabaseJobs** alt dizini ve yüklemek, yükseltmek ve kaldırmak için PowerShell komut dosyalarını da bulunan **Araçları** alt dizini.
3. Cd Araçlar, örneğin yazarak Microsoft.Azure.SqlDatabase.Jobs.x.x.xxx.x klasörü altındaki Araçlar alt dizinine gidin:
   
        PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*>cd tools

4. ElasticDatabaseJobs dizine $home\Documents\WindowsPowerShell\Modules kopyalamak için.\InstallElasticDatabaseJobsCmdlets.ps1 betiğini yürütün. Bu da otomatik olarak kullanmak için modülü örneğin alınır:
   
        PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools>Unblock-File .\InstallElasticDatabaseJobsCmdlets.ps1
        PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools>.\InstallElasticDatabaseJobsCmdlets.ps1

## <a name="install-the-elastic-database-jobs-components-using-powershell"></a>PowerShell kullanarak esnek veritabanı işleri bileşenlerini yükle
1. Microsoft Azure PowerShell komut penceresini başlatın ve Microsoft.Azure.SqlDatabase.Jobs.x.x.xxx.x klasörü altında \tools alt dizinine gidin: cd \tools yazın
   
        PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*>cd tools

2. PowerShell Betiği.\InstallElasticDatabaseJobs.ps1 yürütün ve istenen değişkenleri için değerleri girin. Bu komut dosyası açıklanan bileşenleri oluşturacak [esnek veritabanı iş bileşenleri ve fiyatlandırma](sql-database-elastic-jobs-overview.md#components-and-pricing) uygun şekilde bağımlı bileşenleri kullanmak için Azure bulut hizmeti yapılandırma yanı sıra.

        PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools>Unblock-File .\InstallElasticDatabaseJobs.ps1
        PS C:\*Microsoft.Azure.SqlDatabase.Jobs.x.x.xxxx.x*\tools>.\InstallElasticDatabaseJobs.ps1

Bu komutu çalıştırdığınızda bir pencere açılır girmenizi isteyen bir **kullanıcı adı** ve **parola**. Bu Azure kimlik bilgilerinizi değil, kullanıcı adını girin ve yeni sunucu için oluşturmak istediğiniz yönetici kimlik bilgileri parola.

Bu örnek çağrısının sağlanan parametreler için istenen ayarlarınız değiştirilebilir. Aşağıdaki her bir parametreyi davranışı üzerinde daha fazla bilgi sağlar:

<table style="width:100%">
  <tr>
    <th>Parametre</th>
    <th>Açıklama</th>
  </tr>

<tr>
    <td>ResourceGroupName</td>
    <td>Yeni oluşturulan Azure bileşenleri içeren için oluşturduğunuz Azure kaynak grubu adı sağlar. Bu parametre "__ElasticDatabaseJob" varsayılan olarak ayarlanır. Bu değeri değiştirmek için önerilmez.</td>
    </tr>

</tr>

    <tr>
    <td>ResourceGroupLocation</td>
    <td>Yeni oluşturulan Azure bileşenleri için kullanılacak Azure konum sağlar. Bu parametre Orta ABD konuma varsayılan olarak ayarlanır.</td>
</tr>

<tr>
    <td>ServiceWorkerCount</td>
    <td>Yüklemek için hizmet çalışanların sayısını sağlar. Bu parametre varsayılan olarak 1. Daha yüksek bir çalışan sayısı, hizmetin ölçeğini genişletin ve yüksek kullanılabilirlik sağlamak için kullanılabilir. "2" hizmetinin yüksek kullanılabilirlik gerektiren dağıtımlar için kullanılması önerilir.</td>
    </tr>

</tr>
    <tr>
    <td>ServiceVmSize</td>
    <td>VM boyutu bulut hizmeti içindeki kullanım sağlar. Bu parametre için A0 varsayılan olarak ayarlanır. Parametre değerlerini A0/A1/A2/A3 çok küçük/küçük/Orta/büyük boyutu kullanmak sırasıyla çalışan rolü neden kabul edilir. FO çalışan rolü boyutları hakkında daha fazla bilgi görmek [esnek veritabanı iş bileşenleri ve fiyatlandırma](sql-database-elastic-jobs-overview.md#components-and-pricing).</td>
</tr>

</tr>
    <tr>
    <td>SqlServerDatabaseSlo</td>
    <td>Hizmet düzeyi hedefi Standard edition için sağlar. Bu parametre için S0 varsayılan olarak ayarlanır. Parametre değerlerini S0/S1/S2/S3/S4/S6/S9/S12 ilgili SLO kullanmak Azure SQL veritabanı neden kabul edilir. SQL veritabanı Slo'lar hakkında daha fazla bilgi için bkz: [esnek veritabanı iş bileşenleri ve fiyatlandırma](sql-database-elastic-jobs-overview.md#components-and-pricing).</td>
</tr>

</tr>
    <tr>
    <td>SqlServerAdministratorUserName</td>
    <td>Yeni oluşturulan Azure SQL veritabanı sunucusu yönetici kullanıcı adı sağlar. Belirtilmediğinde, kimlik bilgileri istemek için bir PowerShell kimlik bilgileri penceresi açılır.</td>
</tr>

</tr>
    <tr>
    <td>SqlServerAdministratorPassword</td>
    <td>İçin yeni oluşturulan Azure SQL veritabanı sunucusu yönetici parolasını sağlar. Sağlanan değil, kimlik bilgilerini soracak şekilde bir PowerShell kimlik bilgileri penceresi açılır.</td>
</tr>
</table>

Çok sayıda paralel çok sayıda veritabanı karşı çalışan işleri sahip hedef sistemleri için parametreleri belirtmek için önerilir: - ServiceWorkerCount 2 - ServiceVmSize A2 - SqlServerDatabaseSlo S2.

    PS C:\*Microsoft.Azure.SqlDatabase.Jobs.dll.x.x.xxx.x*\tools>Unblock-File .\InstallElasticDatabaseJobs.ps1
    PS C:\*Microsoft.Azure.SqlDatabase.Jobs.dll.x.x.xxx.x*\tools>.\InstallElasticDatabaseJobs.ps1 -ServiceWorkerCount 2 -ServiceVmSize A2 -SqlServerDatabaseSlo S2

## <a name="update-an-existing-elastic-database-jobs-components-installation-using-powershell"></a>PowerShell kullanarak var olan esnek veritabanı işleri bileşenleri yüklemeyi güncelleştirme
**Esnek veritabanı iş** içinde var olan bir yüklemesini ölçek ve yüksek kullanılabilirlik için güncelleştirilmiştir. Bu işlem, bırakma ve Denetim veritabanı yeniden oluşturmak zorunda kalmadan servis kodu gelecekteki yükseltmeler için sağlar. Bu işlem ayrıca hizmet VM boyutu veya sunucu çalışan sayısını değiştirmek için aynı sürüm içinde kullanılabilir.

Bir yükleme VM boyutu güncelleştirmek için tercih ettiğiniz değerleri için güncelleştirilmiş parametrelerle aşağıdaki betiği çalıştırın.

    PS C:\*Microsoft.Azure.SqlDatabase.Jobs.dll.x.x.xxx.x*\tools>Unblock-File .\UpdateElasticDatabaseJobs.ps1
    PS C:\*Microsoft.Azure.SqlDatabase.Jobs.dll.x.x.xxx.x*\tools>.\UpdateElasticDatabaseJobs.ps1 -ServiceVmSize A1 -ServiceWorkerCount 2

<table style="width:100%">
  <tr>
  <th>Parametre</th>
  <th>Açıklama</th>
</tr>

  <tr>
    <td>ResourceGroupName</td>
    <td>Esnek veritabanı iş bileşenler ilk yüklendiğinde kullanılan Azure kaynak grubu adını tanımlar. Bu parametre "__ElasticDatabaseJob" varsayılan olarak ayarlanır. Bu değeri değiştirmek için önerilmez olduğundan, bu parametre belirtmeniz gerekmez.</td>
    </tr>
</tr>

</tr>

  <tr>
    <td>ServiceWorkerCount</td>
    <td>Yüklemek için hizmet çalışanların sayısını sağlar.  Bu parametre varsayılan olarak 1.  Daha yüksek bir çalışan sayısı, hizmetin ölçeğini genişletin ve yüksek kullanılabilirlik sağlamak için kullanılabilir.  "2" hizmetinin yüksek kullanılabilirlik gerektiren dağıtımlar için kullanılması önerilir.</td>
</tr>

</tr>

    <tr>
    <td>ServiceVmSize</td>
    <td>VM boyutu bulut hizmeti içindeki kullanım sağlar. Bu parametre için A0 varsayılan olarak ayarlanır. Parametre değerlerini A0/A1/A2/A3 çok küçük/küçük/Orta/büyük boyutu kullanmak sırasıyla çalışan rolü neden kabul edilir. FO çalışan rolü boyutları hakkında daha fazla bilgi görmek [esnek veritabanı iş bileşenleri ve fiyatlandırma](sql-database-elastic-jobs-overview.md#components-and-pricing).</td>
</tr>

</table>

## <a name="install-the-elastic-database-jobs-components-using-the-portal"></a>Portalı kullanarak esnek veritabanı işleri bileşenlerini yükle
Bulduktan sonra [bir esnek havuz oluşturulan](sql-database-elastic-pool-manage-portal.md), yükleyebileceğiniz **esnek veritabanı işleri** esnek havuzdaki her veritabanında yönetimsel görevlerin yürütülmesine izin verecek biçimde bileşenleri. Ne zaman aksine kullanarak **esnek veritabanı işleri** PowerShell API'leri, portal arabiriminde yalnızca var olan bir havuzu karşı yürütülen şu anda kısıtlı.

**Tahmini tamamlanma süresi:** 10 dakika.

1. Esnek havuz Pano görünümden [Azure portal](https://portal.azure.com/#) , tıklatın **oluşturma işi**.
2. Bir işi ilk kez oluşturuyorsanız, yüklemelisiniz **esnek veritabanı işleri** tıklayarak **Önizleme koşulları**.
3. Onay kutusunu tıklatarak koşullarını kabul edin.
4. 'Hizmetleri Yükle"Görüntüle'yi tıklatın **iş kimlik bilgilerini**.
   
    ![Hizmetleri Yükleniyor][1]
5. Bir kullanıcı adı ve veritabanı yönetici parolasını yazın Yüklemesinin bir parçası olarak, yeni bir Azure SQL veritabanı sunucusu oluşturulur. Bu yeni sunucu içinde denetim veritabanı olarak bilinen yeni bir veritabanı oluşturulur ve esnek veritabanı işleri için meta veri kapsamak için kullanılmış. Kullanıcı adı ve parola burada oluşturulan denetim veritabanına oturum açma amacıyla kullanılır. Ayrı bir kimlik bilgisi komut dosyası yürütme havuzdaki veritabanları için kullanılır.
   
    ![Kullanıcı adı ve parola oluşturun.][2]
6. Tamam düğmesine tıklayın. Bileşenleri sizin için yeni birkaç dakika içinde oluşturulan [kaynak grubu](../azure-resource-manager/resource-group-overview.md). Yeni kaynak grubu, aşağıda gösterildiği gibi başlangıç panosuna sabitlendi. Oluşturduktan sonra esnek veritabanı işleri (bulut hizmeti, SQL veritabanı, hizmet veri yolu ve depolama) tüm grubunda oluşturulur.
   
    ![Başlangıç Panosu kaynak grubunda][3]
7. Oluşturmak veya esnek veritabanı işleri yüklüyor olsa da, sağlanırken bir işi yönetmek çalışırsanız **kimlik bilgileri** aşağıdaki iletiyi görürsünüz.
   
    ![Dağıtımı hala devam ediyor][4]

Kaldırma işlemi gerekiyorsa, kaynak grubunu silin. Bkz: [esnek veritabanı iş bileşenleri de kaldırmayı nasıl](sql-database-elastic-jobs-uninstall.md).

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi için grubunda, her bir veritabanına komut dosyası yürütme oluşturulan için uygun haklara sahip bir kimlik bilgisi sağlamak [SQL veritabanınızın güvenliğini sağlama](sql-database-manage-logins.md).
Bkz: [oluşturma ve bir esnek veritabanı işlerini yönetme](sql-database-elastic-jobs-create-and-manage.md) başlamak için.

<!--Image references-->
[1]: ./media/sql-database-elastic-jobs-service-installation/screen-1.png
[2]: ./media/sql-database-elastic-jobs-service-installation/credentials.png
[3]: ./media/sql-database-elastic-jobs-service-installation/start-board.png
[4]: ./media/sql-database-elastic-jobs-service-installation/not-done.png
