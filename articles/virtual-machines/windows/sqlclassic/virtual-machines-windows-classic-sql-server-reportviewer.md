---
title: Bir Web sitesinde ReportViewer kullanma | Microsoft Docs
description: Bu konuda, bir Microsoft Azure sanal makinesi üzerinde depolanan bir raporu görüntüleyen Visual Studio ReportViewer denetimiyle Microsoft Azure Web sitesinin nasıl oluşturulduğu açıklanır.
services: virtual-machines-windows
documentationcenter: na
author: markingmyname
manager: erikre
editor: monicar
tags: azure-service-management
ms.assetid: 78b76318-d9bf-48ef-9d9e-d1b7d8cf3042
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 01/11/2017
ms.author: maghan
ms.openlocfilehash: b554dc1fa33519d87aa0c9c5ba9130b47cbea142
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60580077"
---
# <a name="use-reportviewer-in-a-web-site-hosted-in-azure"></a>Azure’da Barındırılan bir Web Sitesinde ReportViewer Kullanma
> [!IMPORTANT]
> Azure'da oluşturmaya ve kaynaklarla çalışmaya yönelik iki farklı dağıtım modeli vardır: [Resource Manager ve klasik](../../../azure-resource-manager/resource-manager-deployment-model.md). Bu makalede, Klasik dağıtım modelini incelemektedir. Microsoft, yeni dağıtımların çoğunun Resource Manager modelini kullanmasını önerir.

Bir Microsoft Azure sanal Makinesi'nde depolanan bir raporu görüntüleyen Visual Studio ReportViewer denetimiyle Microsoft Azure Web sitesi oluşturabilirsiniz. ASP.NET Web uygulaması şablonunu kullanarak oluşturduğunuz bir Web uygulamasında ReportViewer denetimi var.

> [!IMPORTANT]
> ASP.NET MVC Web uygulaması şablonları ReportViewer denetimi desteklemez.

ReportViewer'ı Microsoft Azure Web sitenize eklemek için aşağıdaki görevleri tamamlamanız gerekir.

* **Ekleme** derlemeler için dağıtım paketi
* **Yapılandırma** kimlik doğrulama ve yetkilendirme
* **Yayımlama** azure'a bir ASP.NET Web uygulaması

## <a name="prerequisites"></a>Önkoşullar
"Genel öneri ve en iyi yöntemler" bölümünü gözden geçirin [SQL Server İş Zekası Azure sanal Makineler'de](../classic/ps-sql-bi.md).

> [!NOTE]
> ReportViewer denetimleri ile Visual Studio, Standard Edition veya üstüne gönderilir. Web Developer Express Edition kullanıyorsanız, yüklemelisiniz [MICROSOFT Rapor Görüntüleyicisi 2012 çalışma zamanı](https://www.microsoft.com/download/details.aspx?id=35747) ReportViewer çalışma zamanı özellikleri kullanmak için.
>
> Yapılandırılmış yerel işleme modunda ReportViewer'ı Microsoft Azure'da desteklenmiyor.

## <a name="adding-assemblies-to-the-deployment-package"></a>Derleme, dağıtım paketi ekleme
ASP.NET uygulaması şirket içi barındırdığınızda, ReportViewer derlemeler genellikle doğrudan genel derleme önbelleğinde IIS sunucusunun (GAC) Visual Studio yüklemesi sırasında yüklenir ve uygulama tarafından doğrudan erişilebilir. ASP.NET uygulamanızı bulutta barındırın, ancak Microsoft Azure ReportViewer derlemelerin yerel olarak uygulamanız için kullanılabilir olduğundan emin olmanız gerekir, böylece GAC içine yüklenecek herhangi bir şey izin vermez. Bunları başvuruları projenize ekleyerek bunu ve bunları yerel olarak kopyalanacak yapılandırabilirsiniz.

Uzaktan işleme modunda ReportViewer denetimi aşağıdaki derlemeler kullanır:

* **Microsoft.ReportViewer.WebForms.dll**: ReportViewer sayfanızın kullanmanız gereken ReportViewer kodunu içerir. Projenizde ASP.NET sayfasına bir ReportViewer denetimi düşürdüğünüzde başvuru bu derleme için projenize eklenir.
* **Microsoft.ReportViewer.Common.dll**: Çalışma zamanında ReportViewer denetimi tarafından kullanılan sınıfları içerir. Projenize otomatik olarak eklenmez.

### <a name="to-add-a-reference-to-microsoftreportviewercommon"></a>Microsoft.ReportViewer.Common bir başvuru eklemek için
* Projenizin sağ **başvuruları** düğümünü seçip alt **Başvuru Ekle**, .NET sekmesinde derlemeyi seçin ve tıklayın **Tamam**.

### <a name="to-make-the-assemblies-locally-accessible-by-your-aspnet-application"></a>Derlemeler, ASP.NET uygulamanız tarafından yerel olarak erişilebilir hale getirmek için
1. İçinde **başvuruları** klasör özelliklerini Özellikler bölmesinde görünecek biçimde Microsoft.ReportViewer.Common derleme tıklayın.
2. Özellikler bölmesinde **Yereli Kopyala** true.
3. Adım 1 ve 2 Microsoft.ReportViewer.WebForms için yineleyin.

### <a name="to-get-reportviewer-language-pack"></a>ReportViewer dil paketini almak için
1. Uygun Microsoft Rapor Görüntüleyicisi 2012 çalışma zamanı yeniden dağıtılabilir paketi yükleme [Microsoft Download Center](https://go.microsoft.com/fwlink/?LinkId=317386).
2. Açılır listeden bir dil seçin ve sayfanın ilgili center indirme sayfasına yönlendirilir.
3. Tıklayın **indirme** ReportViewerLP.exe yüklemeyi başlatmak için.
4. ReportViewerLP.exe indirdikten sonra tıklayın **çalıştırın** hemen yükleyin veya **Kaydet** bilgisayarınıza kaydedin. Tıklarsanız **Kaydet**, dosyayı kaydettiğiniz klasörün adını unutmayın.
5. Dosyasını kaydettiğiniz klasörü bulun. ReportViewerLP.exe sağ tıklayın, **yönetici olarak çalıştır**ve ardından **Evet**.
6. ReportViewerLP.exe çalıştırdıktan sonra c:\windows\assembly sahip kaynak dosyaları görürsünüz **Microsoft.ReportViewer.Webforms.Resources** ve **Microsoft.ReportViewer.Common.Resources**.

### <a name="to-configure-for-localized-reportviewer-control"></a>Yerelleştirilmiş ReportViewer denetimi için yapılandırmak için
1. İndirin ve yukarıda belirtilen yönergeleri izleyerek Microsoft Rapor Görüntüleyicisi 2012 çalışma zamanı yeniden dağıtılabilir paketi yükleyin.
2. Oluşturma \<dil\> kopyalama ve proje klasöründe ilişkili kaynak bütünleştirilmiş kodu dosyaları yok. Kopyalanacak kaynak derleme dosyaları şunlardır: **Microsoft.ReportViewer.Webforms.Resources.dll** ve **Microsoft.ReportViewer.Common.Resources.dll**. Kaynak derleme dosyaları seçin ve Özellikler bölmesinde **çıkış dizinine Kopyala** için "**her zaman Kopyala**".
3. Culture ve UICulture için web projesine ayarlayın. Bir ASP.NET Web sayfası için kültürü ve kullanıcı Arabirimi kültürünü ayarlama hakkında daha fazla bilgi için bkz. [nasıl yapılır: ASP.NET Web sayfası Genelleştirme için kültürü ve kullanıcı Arabirimi kültürünü ayarlama](https://go.microsoft.com/fwlink/?LinkId=237461).

## <a name="configuring-authentication-and-authorization"></a>Kimlik doğrulama ve yetkilendirme yapılandırma
ReportViewer'ı rapor sunucusu ile kimlik doğrulaması için uygun kimlik bilgilerini kullanması gerekir ve kimlik bilgileri rapor sunucusu tarafından istediğiniz raporlara erişmek için yetkilendirilmesi gerekir. Kimlik doğrulaması hakkında daha fazla bilgi için bkz. [Reporting Services Rapor Görüntüleyicisi denetimi ve Microsoft Azure sanal makine tabanlı rapor sunucuları](https://msdn.microsoft.com/library/azure/dn753698.aspx).

## <a name="publish-the-aspnet-web-application-to-azure"></a>ASP.NET Web uygulamasını azure'da yayımlama
Azure'a bir ASP.NET Web uygulaması yayımlama ile ilgili yönergeler için bkz: [nasıl yapılır: Geçiş ve Visual Studio'dan bir Web uygulamasını azure'a yayımlama](../../../vs-azure-tools-migrate-publish-web-app-to-cloud-service.md) ve [Web Apps'i ve ASP.NET'i kullanmaya başlama](../../../app-service/app-service-web-get-started-dotnet.md).

> [!IMPORTANT]
> Azure dağıtım projesine eklemek veya Azure bulut hizmeti projesi Ekle komutu kısayol menüsünde Çözüm Gezgininde görünmüyorsa, proje için hedef çerçeveyi .NET Framework 4'e değişiklik yapmanız gerekebilir.
>
> İki komut aslında aynı işlevleri sağlar. Bir ya da diğer komutu kısayol menüsünde, yüklediğiniz Microsoft Azure SDK'sı sürümüne bağlı olarak görünür.
>
>

## <a name="resources"></a>Kaynaklar
[Microsoft raporları](https://go.microsoft.com/fwlink/?LinkId=205399)

[Azure Sanal Makinelerde SQL Server İş Zekası](../classic/ps-sql-bi.md)

[Yerel Mod Rapor Sunucusu ile Azure VM Oluşturmak için PowerShell Kullanma](../classic/ps-sql-report.md)
