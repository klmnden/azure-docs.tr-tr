---
title: SQL veritabanı çok kiracılı uygulama örneğin - Wingtip SaaS Kılavuzu | Microsoft Docs
description: Yükleme ve Azure SQL veritabanı örnek Wingtip bilet SaaS kullanan örnek çok kiracılı uygulamayı çalıştırmak için adımları ve yönergeleri sağlar.
services: sql-database
ms.service: sql-database
ms.subservice: scenario
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: MightyPen
ms.author: genemi
ms.reviewer: sstein
manager: craigg
ms.date: 12/18/2018
ms.openlocfilehash: 758cb47760f4a15e262a4d682089ac7d9fee64e8
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60326297"
---
# <a name="general-guidance-for-working-with-wingtip-tickets-sample-saas-apps"></a>Wingtip bilet ile çalışmak için genel kılavuz örnek SaaS uygulamaları

Bu makale, Azure SQL veritabanı kullanan Wingtip bilet örnek SaaS uygulamaları çalıştırmak için genel yönergeler içerir. 

## <a name="download-and-unblock-the-wingtip-tickets-saas-scripts"></a>İndirin ve Wingtip bilet SaaS betikleri engelini kaldırma

Zip dosyaları bir dış kaynaktan indirilip ayıklanan zaman yürütülebilir içeriği (betikler, DLL'ler) Windows tarafından engelleniyor olabilir. Komut dosyaları bir zip dosyasından ayıklanırken **ayıklamadan önce .zip dosyasını engelini kaldırmak için aşağıdaki adımları**. Bu komut dosyalarının çalışmasına izin verilen sağlar.

1. Keşfetmek istediğiniz veritabanı kiralama deseni için Wingtip bilet SaaS GitHub deposunu göz atın: 
    - [WingtipTicketsSaaS StandaloneApp](https://github.com/Microsoft/WingtipTicketsSaaS-StandaloneApp)
    - [WingtipTicketsSaaS DbPerTenant](https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant)
    - [WingtipTicketsSaaS MultiTenantDb](https://github.com/Microsoft/WingtipTicketsSaaS-MultiTenantDb)
2. Tıklayın **Kopyala veya indir**.
3. Tıklayın **Download ZIP** ve dosyayı kaydedin.
4. Zip dosyasını sağ tıklatın ve seçin **özellikleri**. Zip dosya adı, depo adına karşılık gelir. (ör. _WingtipTicketsSaaS-DbPerTenant-master.zip_)
5. Üzerinde **genel** sekmesinde **Engellemeyi Kaldır**.
6. **Tamam** düğmesine tıklayın.
7. Dosyaları ayıklayın.

Betikleri yerleştirilir *... \\Öğrenme modülleri* klasör.


## <a name="working-with-the-wingtip-tickets-powershell-scripts"></a>Wingtip biletleri PowerShell betikleriyle çalışma

En iyi bir örnek için sağlanan betikleri ayrıntılı şekilde inceleyin gerekir. Kesme noktaları kullanma ve betiklerde yürütün ve farklı SaaS düzenlerinin nasıl uygulandığını inceleyin. Sağlanan betikleri ve modülleri en iyi anlamak için bir kolayca adım için kullanmanızı öneririz [PowerShell ISE](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/introducing-the-windows-powershell-ise).

### <a name="update-the-configuration-file-for-your-deployment"></a>Dağıtımınız için yapılandırma dosyasını güncelleştirme

Düzen **UserConfig.psm1** dosyayı dağıtım sırasında ayarladığınız kaynak grubu ve kullanıcı değeri:

1. Açık *PowerShell ISE* ve yükle... \\Öğrenme modülleri\\*UserConfig.psm1* 
2. Güncelleştirme *ResourceGroupName* ve *adı* dağıtımınız (10 ve 11 yalnızca satırlarındaki) için belirli değerlere sahip.
3. Değişiklikleri Kaydet!

Bu değerleri ayarı burada yalnızca, bu her betik dağıtımına özgü değerleri güncelleştirmek zorunda kalmaktan tutar.

### <a name="execute-the-scripts-by-pressing-f5"></a>F5 tuşuna basarak betikleri çalıştırma

Birçok betik kullanmak *$PSScriptRoot* klasörleri gidin ve *$PSScriptRoot* tuşlarına basarak komut dosyaları yürütme sırasında yalnızca değerlendirilir **F5**.  Vurgulama ve bir seçim çalıştıran (**F8**) hatalara neden, bu nedenle basın **F5** betikleri çalışırken.

### <a name="step-through-the-scripts-to-examine-the-implementation"></a>Uygulamayı incelemek üzere betiklerde ilerleme

Komut dosyalarını anlamak için en iyi yolu, neler yaptığını görmek için bloblarda adımla ' dir. Dahil edilen denetleyin **Demo -** üst düzey iş akışı izlemek bir kolayca sunmak betikler. **Demo -** betikleri Göster her görevi yerine getirmek, kesme noktaları olacak şekilde ayarlamanız ve incelemek için gerekli adımları daha ayrıntılı farklı SaaS düzenlerinin uygulama ayrıntılarını görmek için çağrıları tek tek içine.

İçin keşfetmek ve PowerShell betikleri Adımlama ipuçları:

- Açık **Demo -** PowerShell ISE'de betiklerde.
- Execute veya devam **F5** (kullanarak **F8** değerlendirilmediğinden *$PSScriptRoot* betik seçimleri çalıştırılırken değerlendirilmez).
- Bir çizgiye tıklayarak veya çizgiyi seçerek ve **F9**’a basarak kesme noktaları yerleştirin.
- **F10**’u kullanarak bir işlev veya betiği atlayın.
- **F11**’i kullanarak bir işlev veya betiğe gidin.
- **Shift + F11**’i kullanarak geçerli işlev veya betikten çıkın.


## <a name="explore-database-schema-and-execute-sql-queries-using-ssms"></a>Veritabanı şemasını keşfetme ve SSMS kullanarak SQL sorguları yürütme

Kullanım [SQL Server Management Studio (SSMS)](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) bağlanın ve uygulama sunucuları ve veritabanlarına göz atın.

Dağıtımı, kiracılar ve Katalog SQL veritabanı sunucularına bağlanmak için başlangıçta sahiptir. Adlandırma sunucularının veritabanı kiralama desen (ayrıntıları için aşağıya bakın) bağlıdır. 

   - **Tek başına uygulama:** sunucular için her bir kiracı (ör. *contosoconcerthall -&lt;kullanıcı&gt;*  sunucusu) ve *Kataloğu-sa -&lt;kullanıcı&gt;*
   - **Kiracı başına veritabanı:** *tenants1-dpt -&lt;kullanıcı&gt;*  ve *Kataloğu-dpt -&lt;kullanıcı&gt;*  sunucuları
   - **Çok kiracılı veritabanı:** *tenants1-mt -&lt;kullanıcı&gt;*  ve *Kataloğu-mt -&lt;kullanıcı&gt;*  sunucuları

Başarılı tanıtım bağlantısı sağlamak için tüm sunucularda yüklü bir [güvenlik duvarı kuralı](sql-database-firewall-configure.md) aracılığıyla tüm IP'lere izin verme.


1. Açık *SSMS* ve kiracılar için bağlanın. Sunucu adı (ayrıntıları için aşağıya bakın), seçtiğiniz veritabanı kiralama deseni bağlıdır:
    - **Tek başına uygulama:** sunucuları tek tek Kiracı (ör. *contosoconcerthall -&lt;kullanıcı&gt;. database.windows.net*) 
    - **Kiracı başına veritabanı:** *tenants1-dpt -&lt;kullanıcı&gt;. database.windows.net*
    - **Çok kiracılı veritabanı:** *tenants1-mt -&lt;kullanıcı&gt;. database.windows.net* 
2. **Bağlan** > **Veritabanı Altyapısı...**:

   ![katalog sunucusu seçeneğine tıklayın](media/saas-tenancy-wingtip-app-guidance-tips/connect.png)

3. Tanıtım kimlik bilgileri şunlardır: Oturum açma = *Geliştirici*, parola = *P\@ssword1*

    Oturum açma için aşağıdaki görüntüde gösterilmiştir *Kiracı başına veritabanı* deseni. 
    ![bağlantı](media/saas-tenancy-wingtip-app-guidance-tips/tenants1-connect.png)
    
   

4. 2-3. adımları tekrarlayın ve (Seçili veritabanı kiralama deseni temel alınarak belirli sunucu adları için aşağıya bakın) katalog sunucusuna bağlanın
    - **Tek başına uygulama:** *Kataloğu-sa -&lt;kullanıcı&gt;. database.windows.net*
    - **Kiracı başına veritabanı:** *Kataloğu-dpt -&lt;kullanıcı&gt;. database.windows.net*
    - **Çok kiracılı veritabanı:** *Kataloğu-mt -&lt;kullanıcı&gt;. database.windows.net*


Bağlantı başarıyla kurulduktan sonra tüm sunucuların görmeniz gerekir. Veritabanları listesi, sağlanmış kiracılar bağlı olarak farklı olabilir.

Oturum açma için aşağıdaki görüntüde gösterilmiştir *Kiracı başına veritabanı* deseni.

![nesne gezgini](media/saas-tenancy-wingtip-app-guidance-tips/object-explorer.png)



## <a name="next-steps"></a>Sonraki adımlar
- [Wingtip bilet SaaS tek başına uygulamayı dağıtma](saas-standaloneapp-get-started-deploy.md)
- [Wingtip bilet SaaS veritabanı başına Kiracı uygulama dağıtma](saas-dbpertenant-get-started-deploy.md)
- [Wingtip bilet SaaS çok kiracılı veritabanı uygulamayı dağıtma](saas-multitenantdb-get-started-deploy.md)

