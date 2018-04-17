---
title: ReportViewer Web sitesi kullanma | Microsoft Docs
description: Bu konuda, bir Microsoft Azure sanal makinesinde depolanan bir rapor görüntüler Visual Studio ReportViewer denetimi ile Microsoft Azure Web sitesi oluşturma açıklar.
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
ms.openlocfilehash: af8a4a9c25005925bed3ddb78ced618e669f7f09
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="use-reportviewer-in-a-web-site-hosted-in-azure"></a>Azure’da Barındırılan bir Web Sitesinde ReportViewer Kullanma
> [!IMPORTANT] 
> Azure oluşturmak ve kaynaklarla çalışmak için iki farklı dağıtım modeli vardır: [Resource Manager ve klasik](../../../azure-resource-manager/resource-manager-deployment-model.md). Bu makalede, Klasik dağıtım modeli kullanarak yer almaktadır. Microsoft, yeni dağıtımların çoğunun Resource Manager modelini kullanmasını önerir.

Bir Microsoft Azure sanal makinesinde depolanan bir rapor görüntüler Visual Studio ReportViewer denetimi ile Microsoft Azure Web sitesi oluşturabilirsiniz. ReportViewer Web uygulamasında ASP.NET Web uygulaması şablonu kullanılarak yapı denetimdir.

> [!IMPORTANT]
> ASP.NET MVC Web uygulaması şablonları ReportViewer Denetimi'ni desteklemez.

Microsoft Azure Web sitenize ReportViewer eklemek için aşağıdaki görevleri tamamlamanız gerekir.

* **Ekleme** Dağıtım paketine derlemeler
* **Yapılandırma** kimlik doğrulama ve yetkilendirme
* **Yayımlama** Azure ASP.NET Web uygulaması

## <a name="prerequisites"></a>Önkoşullar
"Genel öneri ve en iyi uygulamalar" bölümünde gözden [SQL Server Business Intelligence Azure Virtual Machines'de](../classic/ps-sql-bi.md).

> [!NOTE]
> ReportViewer denetimleri, Visual Studio, Standard Edition veya üzeri aktarılır. Web Developer Express Edition kullanıyorsanız, yüklemelisiniz [MICROSOFT Rapor Görüntüleyicisi 2012 çalışma zamanı](https://www.microsoft.com/download/details.aspx?id=35747) ReportViewer çalışma zamanı özellikleri kullanmak için.
> 
> Yapılandırılan yerel işleme modunda ReportViewer Microsoft Azure'da desteklenmiyor.

## <a name="adding-assemblies-to-the-deployment-package"></a>Dağıtım paketi derlemeler ekleme
ASP.NET uygulama şirket içi barındırdığınızda ReportViewer derlemeler genellikle doğrudan genel derleme önbelleğinde IIS sunucusunun (GAC) Visual Studio yükleme sırasında yüklenir ve uygulama tarafından doğrudan erişilebilir. ASP.NET uygulamanızı bulutta barındırdığınızda, ancak Microsoft Azure ReportViewer derlemelerin yerel olarak uygulamanız için kullanılabilir olduğundan emin olmanız gerekir böylece GAC içine yüklenecek bir şey izin vermiyor. Projenizde bunları başvurular ekleyerek bunu ve bunları yerel olarak kopyalanacak yapılandırabilirsiniz.

Uzaktan işleme modunda ReportViewer Denetimi'ni şu derlemeleri kullanır:

* **Microsoft.ReportViewer.WebForms.dll**: ReportViewer sayfanızda kullanmanıza gerek ReportViewer kodunu içerir. Projenizde ASP.NET sayfaya ReportViewer Denetimi'ni bıraktığınızda projenize bu derleme için bir başvuru eklenir.
* **Microsoft.ReportViewer.Common.dll**: çalışma zamanında ReportViewer denetimi tarafından kullanılan sınıfları içerir. Projenize otomatik olarak eklenmez.

### <a name="to-add-a-reference-to-microsoftreportviewercommon"></a>Microsoft.ReportViewer.Common başvuru eklemek için
* Projenizin sağ **başvuruları** düğümü ve seçin **Başvuru Ekle**derleme .NET sekmesini seçin ve'ı tıklatın **Tamam**.

### <a name="to-make-the-assemblies-locally-accessible-by-your-aspnet-application"></a>Derlemeler, ASP.NET uygulaması tarafından yerel olarak erişilebilir yapma
1. İçinde **başvuruları** klasörü, böylece özelliklerini Özellikler bölmesinde görünür Microsoft.ReportViewer.Common derleme'yi tıklayın.
2. Özellikler bölmesinde ayarlayın **kopya yerel** true.
3. Adım 1 ve 2 Microsoft.ReportViewer.WebForms için yineleyin.

### <a name="to-get-reportviewer-language-pack"></a>ReportViewer dil paketi
1. Uygun Microsoft Rapor Görüntüleyicisi 2012 çalışma zamanı yeniden dağıtılabilir paketi yükleme [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=317386).
2. Açılır listeden dili seçin ve sayfanın karşılık gelen İndirme Merkezi sayfasına yönlendirilir.
3. Tıklatın **karşıdan** ReportViewerLP.exe yüklemeyi başlatmak için.
4. ReportViewerLP.exe indirdikten sonra tıklatın **çalıştırmak** tıklayın veya yükledikten hemen **kaydetmek** bilgisayarınıza kaydetmek için. Tıklatırsanız **kaydetmek**, dosyayı kaydetmek klasörün adını unutmayın.
5. Dosyayı kaydettiğiniz klasörü bulun. ReportViewerLP.exe sağ tıklayın, **yönetici olarak çalıştır**ve ardından **Evet**.
6. ReportViewerLP.exe çalıştırdıktan sonra c:\windows\assembly sahip kaynak dosyaları görürsünüz **Microsoft.ReportViewer.Webforms.Resources** ve **Microsoft.ReportViewer.Common.Resources**.

### <a name="to-configure-for-localized-reportviewer-control"></a>Yerelleştirilmiş ReportViewer denetiminin yapılandırmak için
1. Karşıdan yükle ve yukarıda belirtilen yönergeleri izleyerek Microsoft Rapor Görüntüleyicisi 2012 çalışma zamanı yeniden dağıtılabilir paketini yükleyin.
2. Oluşturma <language> kopyalama ve proje klasöründe bir ilişkili kaynak assembly dosyaları vardır. Kopyalanacak kaynak assembly dosyalar: **Microsoft.ReportViewer.Webforms.Resources.dll** ve **Microsoft.ReportViewer.Common.Resources.dll**. Kaynak assembly dosyaları seçin ve Özellikler bölmesinde ayarlayın **çıktı dizinine Kopyala** için "**her zaman Kopyala**".
3. Kültür & UICulture web projesi için Ayarla. UI kültürü ve kültür bir ASP.NET Web sayfası için ayarlama hakkında daha fazla bilgi için bkz: [nasıl yapılır: ASP.NET Web sayfası Genelleştirme için UI kültürü ve kültür ayarlayın](http://go.microsoft.com/fwlink/?LinkId=237461).

## <a name="configuring-authentication-and-authorization"></a>Kimlik doğrulama ve yetkilendirme yapılandırma
ReportViewer rapor sunucusu ile kimlik doğrulaması için uygun kimlik bilgilerini kullanması gerekir ve kimlik bilgileri rapor sunucusu tarafından istediğiniz raporlara erişim yetkisi olması gerekir. Kimlik doğrulama hakkında daha fazla bilgi için teknik incelemesine bakın [Reporting Services Rapor Görüntüleyicisi denetimi ve Microsoft Azure sanal makine tabanlı rapor sunucuları](https://msdn.microsoft.com/library/azure/dn753698.aspx).

## <a name="publish-the-aspnet-web-application-to-azure"></a>Azure için ASP.NET Web uygulaması yayımlama
Bir ASP.NET Web uygulaması için Azure yayımlama ile ilgili yönergeler için bkz: [nasıl yapılır: geçirmek ve Azure Visual Studio'dan bir Web uygulamasına yayımlamak](../../../vs-azure-tools-migrate-publish-web-app-to-cloud-service.md) ve [Web uygulamaları ve ASP.NET kullanmaya başlama](../../../app-service/app-service-web-get-started-dotnet.md).

> [!IMPORTANT]
> Azure dağıtım projesi eklemek veya Azure bulut hizmeti projesi Ekle komutu Çözüm Gezgini'nde kısayol menüsünde görünmüyorsa, projenin hedef çerçevesini .NET Framework 4'e değiştirmeniz gerekebilir.
> 
> İki komutları temelde aynı işlevselliği sağlar. Bir ya da diğer komutu kısayol menüsünde yüklediğiniz Microsoft Azure SDK'sı sürümüne bağlı olarak görünür.
> 
> 

## <a name="resources"></a>Kaynaklar
[Microsoft raporları](http://go.microsoft.com/fwlink/?LinkId=205399)

[Azure Sanal Makinelerde SQL Server İş Zekası](../classic/ps-sql-bi.md)

[Yerel Mod Rapor Sunucusu ile Azure VM Oluşturmak için PowerShell Kullanma](../classic/ps-sql-report.md)

