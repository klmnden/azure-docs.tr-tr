---
title: "SQL veritabanı çok kiracılı uygulama örneğin - Wingtip SaaS Kılavuzu | Microsoft Docs"
description: "Yükleme ve Azure SQL Database, Wingtip SaaS örnek kullanan örnek çok kiracılı uygulama çalıştıran için adımları ve yönergeler sağlar."
keywords: "sql veritabanı öğreticisi"
services: sql-database
author: MightyPen
manager: craigg
ms.service: sql-database
ms.custom: scale out apps
ms.workload: On Demand
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/12/2017
ms.author: genemi
ms.openlocfilehash: fbfaea938676991cf6280e5dd8c1e1190aa268a8
ms.sourcegitcommit: 732e5df390dea94c363fc99b9d781e64cb75e220
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/14/2017
---
# <a name="guidance-and-tips-for-azure-sql-database-multi-tenant-saas-app-example"></a>Kılavuzu ve Azure SQL veritabanı çok kiracılı SaaS uygulama örneği için ipuçları


## <a name="download-and-unblock-the-wingtip-saas-scripts"></a>Karşıdan yükleme ve Wingtip SaaS betikleri Engellemeyi Kaldır

ZIP dosyaları bir dış kaynaktan yüklediğiniz ve açtığınız zaman yürütülebilir içeriği (komut dosyaları, DLL'ler) Windows tarafından engellenmiş olabilir. Komut dosyaları zip dosyasından çıkarılırken ***ayıklanıyor önce .zip dosyası engellemesini kaldırmak için aşağıdaki adımları izleyin***. Bu komut dosyalarını çalıştırma izni sağlar.

1. Gözat [Wingtip SaaS github deposuna](https://github.com/Microsoft/WingtipSaaS).
2. Tıklatın **Kopyala veya indir**.
3. Tıklatın **ZIP'i indir** ve dosyayı kaydedin.
4. Sağ **WingtipSaaS-master.zip** dosyasını bulun ve seçin **özellikleri**.
5. Üzerinde **genel** sekmesine **Engellemeyi Kaldır**.
6. **Tamam** düğmesine tıklayın.
7. Dosyaları ayıklayın.

Komut dosyaları içinde bulunur *... \\WingtipSaaS ana\\öğrenme modülleri* klasör.


## <a name="working-with-the-wingtip-saas-powershell-scripts"></a>Wingtip SaaS PowerShell komut dosyaları ile çalışma

En iyi örnek almak için sağlanan komut dosyalarına daha yakından inceleyin gerekir. Farklı SaaS desenleri nasıl uygulandığını ayrıntılarını inceleyerek komut dosyalarıyla adım ve kesme noktaları kullanın. Sağlanan komut dosyalarını ve modülleri için en iyi anlama aracılığıyla kolayca adım için kullanmanızı öneririz [PowerShell ISE](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/introducing-the-windows-powershell-ise).

### <a name="update-the-configuration-file-for-your-deployment"></a>Dağıtımınız için yapılandırma dosyasını güncelleştir

Düzen **UserConfig.psm1** dosya dağıtımı sırasında ayarladığınız kaynak grubu ve kullanıcı değerine sahip:

1. Açık *PowerShell ISE* ve yükle... \\Modülleri öğrenme\\*UserConfig.psm1* 
2. Güncelleştirme *ResourceGroupName* ve *adı* (10 ve 11 yalnızca satırlarındaki) dağıtımınız belirli değerleri içeren.
3. Değişiklikleri kaydedin!

Bu değerleri ayarı burada basitçe, her komut dosyası bu dağıtım özgü değerleri güncelleştirmek zorunda kalmaktan tutar.

### <a name="execute-scripts-by-pressing-f5"></a>F5’e basarak Betikleri çalıştırma

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

Dağıtım - bağlanmak için iki SQL veritabanı sunucularının başlangıçta sahip *tenants1 -&lt;kullanıcı&gt; * sunucu ve *katalog -&lt;kullanıcı&gt; * Sunucu. Başarılı demo bağlantı sağlamak için her iki sunucuyu sahip bir [güvenlik duvarı kuralı](sql-database-firewall-configure.md) aracılığıyla tüm IP'ler izin verme.


1. *SSMS*’yi açın ve *tenants1-&lt;User&gt;.database.windows.net* sunucusuna bağlanın.
2. **Bağlan** > **Veritabanı Altyapısı...**:

   ![katalog sunucusu seçeneğine tıklayın](media/saas-dbpertenant-wingtip-app-guidance-tips/connect.png)

3. Tanıtım kimlik bilgileri: Kullanıcı adı = *developer*, Parola = *P@ssword1*

   ![bağlantı](media/saas-dbpertenant-wingtip-app-guidance-tips/tenants1-connect.png)

4. 2-3. adımları tekrarlayın ve *catalog-&lt;User&gt;.database.windows.net* sunucusuna bağlanın.


Başarıyla bağlandıktan sonra her iki sunucuyu da görmeniz gerekir. Veritabanlarının listesini sağlanmış kiracılar bağlı olarak farklı olabilir.

![nesne gezgini](media/saas-dbpertenant-wingtip-app-guidance-tips/object-explorer.png)



## <a name="next-steps"></a>Sonraki adımlar

[Wingtip SaaS uygulamasına dağıtmak](saas-dbpertenant-get-started-deploy.md)

