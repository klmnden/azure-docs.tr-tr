---
title: Azure Tanılama ile bulut Hizmetleri uygulamada akışı izleme | Microsoft Docs
description: İzleme iletileri, hata ayıklama, performans, izleme, trafik çözümlemesi ve daha fazla ölçüm yardımcı olmak için bir Azure uygulamasına ekleyin.
services: cloud-services
documentationcenter: .net
author: jpconnock
manager: timlt
editor: ''
ms.assetid: 09934772-cc07-4fd2-ba88-b224ca192f8e
ms.service: cloud-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 02/20/2016
ms.author: jeconnoc
ms.openlocfilehash: f597bc760a3f3825416912642ee66a53dfb91696
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60336873"
---
# <a name="trace-the-flow-of-a-cloud-services-application-with-azure-diagnostics"></a>Azure Tanılama ile bulut Hizmetleri uygulamasının akışı izleme
İzleme, çalışırken uygulamanızın yürütmesini izlemek bir yoldur. Kullanabileceğiniz [System.Diagnostics.Trace](/dotnet/api/system.diagnostics.trace), [System.Diagnostics.Debug](/dotnet/api/system.diagnostics.debug), ve [System.Diagnostics.TraceSource](/dotnet/api/system.diagnostics.tracesource) hatalarıyla ilgili bilgileri kaydetmek için sınıfları ve Uygulama yürütme günlükleri, metin dosyaları veya diğer cihazlar daha sonra çözümlemek için. İzleme hakkında daha fazla bilgi için bkz: [izleme ve İşaretleme uygulamaları](/dotnet/framework/debug-trace-profile/tracing-and-instrumenting-applications).

## <a name="use-trace-statements-and-trace-switches"></a>İzleme deyimleri ve izleme anahtarları kullanın
Uygulama izleme ekleyerek, Cloud Services uygulamanızda [DiagnosticMonitorTraceListener](/previous-versions/azure/reference/ee758610(v=azure.100)) uygulama yapılandırması ve System.Diagnostics.Trace veya System.Diagnostics.Debug içinde çağrı yapmak için uygulama kodu. Yapılandırma dosyası kullanın *app.config* çalışan rolleri için ve *web.config* web rolleri için. Visual Studio şablon kullanarak yeni bir barındırılan hizmet oluşturduğunuzda, Azure Tanılama'yı otomatik olarak projeye eklenir ve DiagnosticMonitorTraceListener eklediğiniz rolleri için uygun yapılandırma dosyasına eklenir.

İzleme deyimleri yerleştirme hakkında daha fazla bilgi için bkz: [nasıl yapılır: Uygulama koduna izleme deyimleri ekleme](/dotnet/framework/debug-trace-profile/how-to-add-trace-statements-to-application-code).

Yerleştirerek [izleme anahtarları](/dotnet/framework/debug-trace-profile/trace-switches) kodunuzda olup izleme gerçekleşir ve ne kadar kapsamlı olduğunu denetleyebilirsiniz. Bu, bir üretim ortamında, uygulamanızın durumunu izlemenize olanak sağlar. Birden çok bilgisayar üzerinde çalışan birden çok bileşen kullanan bir iş uygulaması bu durum özellikle önemlidir. Daha fazla bilgi için [nasıl yapılır: İzleme anahtarları yapılandırma](/dotnet/framework/debug-trace-profile/how-to-create-initialize-and-configure-trace-switches).

## <a name="configure-the-trace-listener-in-an-azure-application"></a>Bir Azure uygulamasında İzleme dinleyicisi yapılandırma
İzleme, hata ayıklama ve TraceSource, "toplama ve gönderilen iletilerin kaydetmek için dinleyici" ayarlamanızı gerektirir. Dinleyicileri toplamak, depolamak ve izleme iletilerini yönlendirmek. Bunlar, günlük, pencere veya metin dosyası gibi uygun bir hedef izleme çıkışa doğrudan. Azure Tanılama'yı kullanan [DiagnosticMonitorTraceListener](/previous-versions/azure/reference/ee758610(v=azure.100)) sınıfı.

Aşağıdaki yordamı tamamlamadan önce Azure tanılama İzleyicisi'ni başlatmanız gerekir. Bunu yapmak için bkz: [Microsoft azure'da tanılamayı etkinleştirme](cloud-services-dotnet-diagnostics.md).

Visual Studio tarafından sağlanan şablonları kullanıyorsanız, dinleyici yapılandırması otomatik olarak sizin için eklendiğine dikkat edin.

### <a name="add-a-trace-listener"></a>İzleme dinleyicisi Ekle
1. Rolünüz için web.config veya app.config dosyasını açın.
2. Dosyaya aşağıdaki kodu ekleyin. Version özniteliği başvurduğunuz derlemenin sürüm numarasını kullanmak üzere değiştirin. Güncelleştirmeleri olmadıkça mutlaka her Azure SDK sürümüyle derleme sürümünü değiştirmez.
   
    ```
    <system.diagnostics>
        <trace>
            <listeners>
                <add type="Microsoft.WindowsAzure.Diagnostics.DiagnosticMonitorTraceListener,
                  Microsoft.WindowsAzure.Diagnostics,
                  Version=2.8.0.0,
                  Culture=neutral,
                  PublicKeyToken=31bf3856ad364e35"
                  name="AzureDiagnostics">
                    <filter type="" />
                </add>
            </listeners>
        </trace>
    </system.diagnostics>
    ```
   > [!IMPORTANT]
   > Bir proje başvurusu Microsoft.WindowsAzure.Diagnostics derlemesine sahip olduğunuzdan emin olun. Başvurulan Microsoft.WindowsAzure.Diagnostics derleme sürümüyle eşleşecek şekilde xml yukarıdaki sürüm numarasını güncelleştirin.
   > 
   > 
3. Yapılandırma dosyasını kaydedin.

Dinleyicileri hakkında daha fazla bilgi için bkz: [izleme dinleyicilerine](/dotnet/framework/debug-trace-profile/trace-listeners).

Dinleyici eklemek için adımları tamamladıktan sonra izleme deyimleri kodunuza ekleyebilirsiniz.

### <a name="to-add-trace-statement-to-your-code"></a>İzleme deyimi için kod eklemek için
1. Uygulamanız için bir kaynak dosyasını açın. Örneğin, \<RoleName > web rolü ve çalışan rolü için .cs dosyası.
2. Aşağıdakileri ekleyin, değil zaten eklenmişse using deyimi:
    ```
        using System.Diagnostics;
    ```
3. Uygulama durumuyla ilgili bilgileri yakalamak için istediğiniz izleme deyimleri ekleyin. İzleme çıkışını biçimlendirmek için çeşitli yöntemler kullanabilirsiniz. Daha fazla bilgi için [nasıl yapılır: Uygulama koduna izleme deyimleri ekleme](/dotnet/framework/debug-trace-profile/how-to-add-trace-statements-to-application-code).
4. Kaynak dosyayı kaydedin.

