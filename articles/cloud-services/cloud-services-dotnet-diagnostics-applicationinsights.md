---
title: "Bulut Hizmetleri kullanarak uygulama sorunlarını giderme | Microsoft Docs"
description: "Azure Tanılama verileri işlemek için Application Insights kullanarak bulut hizmeti sorunlarını giderme öğrenin."
services: cloud-services
documentationcenter: .net
author: sbtron
manager: timlt
editor: tysonn
ms.assetid: e93f387b-ef29-4731-ae41-0676722accb6
ms.service: cloud-services
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/23/2017
ms.author: saurabh
ms.openlocfilehash: 4001ca908ff00b1a40829d687589080e9b07b18a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshoot-cloud-services-using-application-insights"></a>Bulut Hizmetleri Application Insights kullanarak sorun giderme
İle [Azure SDK 2.8](https://azure.microsoft.com/downloads/) ve Azure tanılama uzantısını 1.5, gönderebilirsiniz Azure Tanılama verileri bulut hizmetiniz için doğrudan Application Insights'a. Günlükleri Azure tanılama tarafından toplanan&mdash;uygulama günlükleri, Windows olay günlükleri, ETW günlükleri ve performans sayaçları dahil olmak üzere&mdash;Application Insights'a gönderilebilir. Ardından bu bilgileri Application Insights portalında UI görselleştirebilirsiniz. Ardından, ölçümleri ve uygulamanıza yanı sıra sistem ve Azure tanılama gelen altyapı düzeyinde veri alınması günlükleri Öngörüler almak için Application Insights SDK'sı kullanabilirsiniz.

## <a name="configure-azure-diagnostics-to-send-data-to-application-insights"></a>Azure tanılama verilerini Application Insights'a göndermek için yapılandırma
Application Insights'a Azure Tanılama verileri göndermek için bulut hizmeti projenizi ayarlamak için aşağıdaki adımları izleyin.

1. Visual Studio Çözüm Gezgini'nde, bir role sağ tıklayın ve seçin **özellikleri** rol Tasarımcısı'nı açın.

    ![Çözüm Gezgini rolü özellikleri][1]

2. İçinde **tanılama** select rol Tasarımcısı bölümünü **tanılama verilerini Application Insights'a gönderme** seçeneği.

    ![Rol Tasarımcısı tanılama verilerini application ınsights'a gönderme][2]

3. Açılan iletişim kutusunda Azure Tanılama verileri göndermek istediğiniz Application Insights kaynağı seçin. İletişim kutusu, varolan bir Application Insights kaynağı aboneliğinizden seçmek için veya bir izleme anahtarı Application Insights kaynağı için el ile belirtmek için sağlar. Application Insights kaynağı oluşturma hakkında daha fazla bilgi için bkz: [yeni bir Application Insights kaynağı oluşturma](../application-insights/app-insights-create-new-resource.md).

    ![Uygulama ınsights kaynağını seçin][3]

    Application Insights kaynağı ekledikten sonra bu kaynak için izleme anahtarını ada sahip bir hizmet yapılandırma ayarı olarak depolanan **appınsıghts_ınstrumentatıonkey**. Her bir hizmet yapılandırması veya ortamı için bu yapılandırma ayarı değiştirebilirsiniz. Bunu yapmak için farklı bir yapılandırmasından seçin **hizmet yapılandırmasını** listesi ve bu yapılandırma için yeni bir izleme anahtarını belirtin.

    ![Hizmet yapılandırmasını seçin][4]

    **Appınsıghts_ınstrumentatıonkey** yapılandırma ayarı yayımlama sırasında uygun Application Insights kaynak bilgileriyle tanılama uzantısını yapılandırmak için Visual Studio tarafından kullanılır. Yapılandırma ayarı farklı hizmet yapılandırması için farklı izleme anahtarı tanımlamanın uygun bir yoludur. Visual Studio bu ayarı çevirin ve tanılama uzantı genel yapılandırması yayımlama işlemi sırasında takın. Tanılama uzantısını PowerShell ile yapılandırma işlemini basitleştirmek için Visual Studio Paketi çıktısını uygun Application Insights izleme anahtarı ile ortak yapılandırma XML de içerir. Genel yapılandırma dosyaları Uzantıları klasöründe oluşturulur ve desenler izleyen *PaaSDiagnostics.&lt; RoleName&gt;. PubConfig.xml*. Tüm PowerShell tabanlı dağıtımlar, her yapılandırma bir role eşleştirmek için bu deseni kullanabilirsiniz.

4) Tüm performans sayaçları ve Application Insights'a Azure Tanılama aracı tarafından toplanan hata düzeyi günlükleri göndermek için Azure tanılama yapılandırmak için etkinleştirmeniz **tanılama verilerini Application Insights'a gönderme** seçeneği. 

    Daha fazla hangi verilerin yapılandırmak istiyorsanız, gönderilen Application Insights'a el ile düzenlemeniz gerekir *diagnostics.wadcfgx* her rol için dosya. Bkz: [Application Insights'a veri göndermek için Azure Tanılama'yı yapılandırmak](#configure-azure-diagnostics-to-send-data-to-application-insights) el ile yapılandırmasını güncelleştirme hakkında daha fazla bilgi için.

Bulut hizmeti application ınsights'a Azure Tanılama verileri göndermek için yapılandırıldığında, onu Azure'a normalde, Azure tanılama uzantısının etkinleştirildiğinden emin dağıtabilirsiniz. Daha fazla bilgi için bkz: [Visual Studio kullanarak bir bulut hizmeti yayımlama](../vs-azure-tools-publishing-a-cloud-service.md).  

## <a name="viewing-azure-diagnostics-data-in-application-insights"></a>Application Insights'ta Azure Tanılama verileri görüntüleme
Bulut hizmeti için yapılandırılmış Application Insights kaynak Azure tanılama telemetri görünür.

Bu yolla Application Insights kavramlarına Azure tanılama günlük türleri eşleyin:

* Performans sayaçları, Application ınsights'ta özel ölçümleri olarak görüntülenir.
* Windows olay günlüklerini, izlemeleri ve Application Insights özel olaylar olarak gösterilir.
* Uygulama günlükleri, ETW günlükleri ve herhangi bir tanılama altyapısı günlük izlemelerini Application ınsights'ta olarak gösterilir.

Application Insights'ta Azure Tanılama verileri görüntülemek için aşağıdakilerden birini yapın:

* Kullanım [ölçüm Gezgini](../application-insights/app-insights-metrics-explorer.md) herhangi bir özel performans sayaçları veya Windows olay günlüğü olaylarını farklı türlerde sayısı görselleştirmek için.

    ![Ölçümleri Explorer'da özel ölçümleri][5]

* Kullanım [arama](../application-insights/app-insights-diagnostic-search.md) Azure tanılama tarafından gönderilen izleme günlükleri arasında aramak için. İşlenmeyen bir özel durum kilitlenmesine ve Geri Dönüşüm rol neden oldu, örneğin, özel durum hakkında bilgi gösterir *uygulama* kanalı *Windows olay günlüğü*. Windows olay günlüğü hatasında bakın ve sorunun nedenini bulmak özel durum için tam yığın izlemesi almak için aramayı kullanabilirsiniz.

    ![Arama izlemeleri][6]

## <a name="next-steps"></a>Sonraki Adımlar
* [Application Insights SDK'sı, bulut hizmetine eklemek](../application-insights/app-insights-cloudservices.md) uygulamanızdan ilgili istekleri, özel durumlar, bağımlılıklar ve tüm özel telemetri verileri göndermek için. Bu bilgiler Azure Tanılama verileri ile birlikte kullanıldığında, uygulama ve sistem, aynı uygulama Insight kaynak tümünde tam görünümünü elde edebilirsiniz.  

<!--Image references-->
[1]: ./media/cloud-services-dotnet-diagnostics-applicationinsights/solution-explorer-properties.png
[2]: ./media/cloud-services-dotnet-diagnostics-applicationinsights/role-designer-sendtoappinsights.png
[3]: ./media/cloud-services-dotnet-diagnostics-applicationinsights/select-appinsights-resource.png
[4]: ./media/cloud-services-dotnet-diagnostics-applicationinsights/role-designer-appinsights-serviceconfig.png
[5]: ./media/cloud-services-dotnet-diagnostics-applicationinsights/metrics-explorer-custom-metrics.png
[6]: ./media/cloud-services-dotnet-diagnostics-applicationinsights/search-windowseventlog-error.png
