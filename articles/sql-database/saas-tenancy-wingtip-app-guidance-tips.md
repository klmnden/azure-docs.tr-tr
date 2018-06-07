---
title: SQL veritabanı çok kiracılı uygulama örneğin - Wingtip SaaS Kılavuzu | Microsoft Docs
description: Yükleme ve Azure SQL Database, Wingtip biletleri SaaS örnek kullanan örnek çok kiracılı uygulama çalıştıran için adımları ve yönergeler sağlar.
keywords: sql veritabanı öğreticisi
services: sql-database
author: MightyPen
manager: craigg
ms.service: sql-database
ms.custom: scale out apps
ms.topic: conceptual
ms.date: 04/01/2018
ms.author: genemi
ms.openlocfilehash: 6c352298c701c827cd01c0ed7f427b7ed6015e29
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34646686"
---
# <a name="general-guidance-for-working-with-wingtip-tickets-sample-saas-apps"></a>Wingtip biletleri ile çalışmak için genel rehberlik örnek SaaS uygulamaları

Bu makale, Azure SQL veritabanı kullanan Wingtip biletleri örnek SaaS uygulamaları çalıştırmak için genel yönergeler içerir. 

## <a name="download-and-unblock-the-wingtip-tickets-saas-scripts"></a>Karşıdan yükleme ve Wingtip biletleri SaaS betikleri Engellemeyi Kaldır

ZIP dosyaları bir dış kaynaktan yüklediğiniz ve açtığınız zaman yürütülebilir içeriği (komut dosyaları, DLL'ler) Windows tarafından engellenmiş olabilir. Komut dosyaları zip dosyasından çıkarılırken **ayıklanıyor önce .zip dosyası engellemesini kaldırmak için aşağıdaki adımları izleyin**. Bu komut dosyalarını çalıştırma izni sağlar.

1. Wingtip biletleri SaaS GitHub deposuna keşfetmek istediğiniz veritabanı kiralama deseni için şuraya gidin: 
    - [WingtipTicketsSaaS StandaloneApp](https://github.com/Microsoft/WingtipTicketsSaaS-StandaloneApp)
    - [WingtipTicketsSaaS DbPerTenant](https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant)
    - [WingtipTicketsSaaS MultiTenantDb](https://github.com/Microsoft/WingtipTicketsSaaS-MultiTenantDb)
2. Tıklatın **Kopyala veya indir**.
3. Tıklatın **indirme zip** ve dosyayı kaydedin.
4. Zip dosyasını sağ tıklatın ve seçin **özellikleri**. Zip dosyası adı depodaki adına karşılık gelir. (örneğin. _WingtipTicketsSaaS-DbPerTenant-master.zip_)
5. Üzerinde **genel** sekmesine **Engellemeyi Kaldır**.
6. **Tamam**’a tıklayın.
7. Dosyaları ayıklayın.

Komut dosyaları içinde bulunur *... \\Öğrenme modülleri* klasör.


## <a name="working-with-the-wingtip-tickets-powershell-scripts"></a>Wingtip biletleri PowerShell komut dosyaları ile çalışma

En iyi örnek almak için sağlanan komut dosyalarına daha yakından inceleyin gerekir. Yürütme ve farklı SaaS desenleri nasıl uygulandığını inceleyin komut dosyalarıyla adım ve kesme noktaları kullanın. Sağlanan komut dosyalarını ve modülleri için en iyi anlama aracılığıyla kolayca adım için kullanmanızı öneririz [PowerShell ISE](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/introducing-the-windows-powershell-ise).

### <a name="update-the-configuration-file-for-your-deployment"></a>Dağıtımınız için yapılandırma dosyasını güncelleştir

Düzen **UserConfig.psm1** dosya dağıtımı sırasında ayarladığınız kaynak grubu ve kullanıcı değerine sahip:

1. Açık *PowerShell ISE* ve yükle... \\Modülleri öğrenme\\*UserConfig.psm1* 
2. Güncelleştirme *ResourceGroupName* ve *adı* (10 ve 11 yalnızca satırlarındaki) dağıtımınız belirli değerleri içeren.
3. Değişiklikleri kaydedin!

Bu değerleri ayarı burada basitçe, her komut dosyası bu dağıtım özgü değerleri güncelleştirmek zorunda kalmaktan tutar.

### <a name="execute-the-scripts-by-pressing-f5"></a>F5 tuşuna basarak komut dosyalarını çalıştır

Birkaç betiklerini kullanın *$PSScriptRoot* klasörleri gidin ve *$PSScriptRoot* tuşuna basarak komut yürütüldüğünde yalnızca değerlendirilir **F5**.  Vurgulama ve bir seçim çalıştıran (**F8**) neden hataları, bu nedenle basın **F5** betikleri çalışırken.

### <a name="step-through-the-scripts-to-examine-the-implementation"></a>Uygulamayı incelemek üzere betiklerde ilerleme

Komut dosyalarını anlamak için en iyi ne yaptıklarını görmek için aralarında adımla yoludur. Dahil edilen denetleyin **Demo -** kolay bir üst düzey iş akışı izleyin sunmak komut dosyaları. **Demo -** komut dosyaları göster her görevi, kesme noktaları olacak şekilde ayarlamanız ve incelemek için gerekli adımları derin farklı SaaS desenler için uygulama ayrıntılarını görmek için tek tek çağrıları içine.

Keşfetmek ve PowerShell komut dosyalarıyla Adımlama ipuçları:

- Açık **Demo -** PowerShell ISE komut.
- Execute veya devam **F5** (kullanarak **F8** çünkü önerilmez *$PSScriptRoot* seçimleri komut dosyası çalıştırılırken değerlendirilmez).
- Bir çizgiye tıklayarak veya çizgiyi seçerek ve **F9**’a basarak kesme noktaları yerleştirin.
- **F10**’u kullanarak bir işlev veya betiği atlayın.
- **F11**’i kullanarak bir işlev veya betiğe gidin.
- **Shift + F11**’i kullanarak geçerli işlev veya betikten çıkın.


## <a name="explore-database-schema-and-execute-sql-queries-using-ssms"></a>Veritabanı şemasını keşfetme ve SSMS kullanarak SQL sorguları yürütme

Kullanım [SQL Server Management Studio (SSMS)](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) bağlanmayı ve uygulama sunucuları ve veritabanları göz atın.

Dağıtımı, kiracılar ve Katalog SQL veritabanı sunucularına bağlanmak için başlangıçta sahiptir. Sunucular adlandırma veritabanı kiralama deseni (özellikleri için aşağıya bakın) bağlıdır. 

   - **Tek başına uygulama:** sunucuları için her bir kiracı (örneğin. *contosoconcerthall -&lt;kullanıcı&gt;*  sunucu) ve *katalog-sa -&lt;kullanıcı&gt;*
   - **Veritabanı Kiracı başına:** *tenants1-dpt -&lt;kullanıcı&gt;*  ve *katalog-dpt -&lt;kullanıcı&gt;*  sunucuları
   - **Çok Kiracı veritabanı:** *tenants1-mt -&lt;kullanıcı&gt;*  ve *katalog-mt -&lt;kullanıcı&gt;*  sunucuları

Başarılı demo bağlantı sağlamak için tüm sunucuların sahip bir [güvenlik duvarı kuralı](sql-database-firewall-configure.md) aracılığıyla tüm IP'ler izin verme.


1. Açık *SSMS* ve kiracıların bağlanın. Sunucu adı (özellikleri için aşağıya bakın) seçtiğiniz veritabanı kiralama düzeni bağlıdır:
    - **Tek başına uygulama:** sunucuları tek tek kiracılar (örneğin, *contosoconcerthall -&lt;kullanıcı&gt;. database.windows.net*) 
    - **Veritabanı Kiracı başına:** *tenants1-dpt -&lt;kullanıcı&gt;. database.windows.net*
    - **Çok Kiracı veritabanı:** *tenants1-mt -&lt;kullanıcı&gt;. database.windows.net* 
2. **Bağlan** > **Veritabanı Altyapısı...**:

   ![katalog sunucusu seçeneğine tıklayın](media/saas-tenancy-wingtip-app-guidance-tips/connect.png)

3. Tanıtım kimlik bilgileri: Kullanıcı adı = *developer*, Parola = *P@ssword1*

    Oturum açma için aşağıdaki görüntü gösteren *veritabanı Kiracı başına* düzeni. 
    ![Bağlantı](media/saas-tenancy-wingtip-app-guidance-tips/tenants1-connect.png)
    
   

4. 2-3 arasındaki adımları yineleyin ve katalog sunucusu (Seçili veritabanı kiralama deseni temel alınarak belirli sunucu adları için aşağıya bakın) bağlanma
    - **Tek başına uygulama:** *katalog-sa -&lt;kullanıcı&gt;. database.windows.net*
    - **Veritabanı Kiracı başına:** *katalog-dpt -&lt;kullanıcı&gt;. database.windows.net*
    - **Çok Kiracı veritabanı:** *katalog-mt -&lt;kullanıcı&gt;. database.windows.net*


Başarıyla bağlandıktan sonra tüm sunucuların görmeniz gerekir. Veritabanlarının listesini sağlanmış kiracılar bağlı olarak farklı olabilir.

Aşağıdaki görüntü için günlükte gösterir *veritabanı Kiracı başına* düzeni.

![nesne gezgini](media/saas-tenancy-wingtip-app-guidance-tips/object-explorer.png)



## <a name="next-steps"></a>Sonraki adımlar
- [Wingtip biletleri SaaS tek başına uygulamayı dağıtma](saas-standaloneapp-get-started-deploy.md)
- [Kiracı uygulama başına Wingtip biletleri SaaS veritabanı dağıtma](saas-dbpertenant-get-started-deploy.md)
- [Wingtip biletleri SaaS çok Kiracı veritabanı uygulaması dağıtma](saas-multitenantdb-get-started-deploy.md)

