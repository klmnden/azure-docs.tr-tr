---
title: "Bulut Hizmetleri uygulamasıyla Azure tanılama akışında izleme | Microsoft Docs"
description: "İzleme iletileri hata ayıklama, performans, izleme, trafik analizi ve daha fazla ölçme yardımcı olmak için bir Azure uygulamaya ekleyin."
services: cloud-services
documentationcenter: .net
author: rboucher
manager: jwhit
editor: 
ms.assetid: 09934772-cc07-4fd2-ba88-b224ca192f8e
ms.service: cloud-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 02/20/2016
ms.author: robb
ms.openlocfilehash: 35b4a4270846c54a1ca760e803ef7adba60cf03b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="trace-the-flow-of-a-cloud-services-application-with-azure-diagnostics"></a>Bulut Hizmetleri uygulaması Azure Tanılama ile akışı izleme
İzleme, çalışırken, uygulamanızın yürütülmesini izlemek bir yoldur. Kullanabileceğiniz [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace.aspx), [System.Diagnostics.Debug](https://msdn.microsoft.com/library/system.diagnostics.debug.aspx), ve [System.Diagnostics.TraceSource](https://msdn.microsoft.com/library/system.diagnostics.tracesource.aspx) hatalarla ilgili bilgileri kaydetmek için sınıflar ve Uygulama yürütme günlükleri, metin dosyaları veya diğer cihazları daha sonra çözümlemek için. İzleme hakkında daha fazla bilgi için bkz: [izleme ve düzenleme uygulamaları](https://msdn.microsoft.com/library/zs6s4h68.aspx).

## <a name="use-trace-statements-and-trace-switches"></a>İzleme deyimleri ve izleme anahtarları kullanın
Uygulama izleme ekleyerek, bulut Hizmetleri uygulamanızda [DiagnosticMonitorTraceListener](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.diagnosticmonitortracelistener.aspx) uygulama yapılandırmasını ve System.Diagnostics.Trace veya System.Diagnostics.Debug içinde çağrıları yapma, uygulama kodu. Yapılandırma dosyası kullanın *app.config* çalışan rolleri için ve *web.config* web rolleri için. Visual Studio şablon kullanarak yeni bir barındırılan hizmet oluşturduğunuzda, Azure tanılama projenize otomatik olarak eklenir ve DiagnosticMonitorTraceListener eklediğiniz rolleri için uygun yapılandırma dosyasına eklenir.

İzleme deyimleri yerleştirme hakkında daha fazla bilgi için bkz: [nasıl yapılır: uygulama koduna izleme deyimleri ekleme](https://msdn.microsoft.com/library/zd83saa2.aspx).

Yerleştirerek [izleme anahtarları](https://msdn.microsoft.com/library/3at424ac.aspx) kodunuzda izleme olup oluşur ve ne kadar kapsamlı olduğunu denetleyebilirsiniz. Uygulamanızı bir üretim ortamında durumunu izlemenize izin verir. Birden çok bilgisayar üzerinde çalışan birden çok bileşenleri kullanan bir iş uygulaması bu durum özellikle önemlidir. Daha fazla bilgi için bkz: [nasıl yapılır: izleme anahtarları yapılandırma](https://msdn.microsoft.com/library/t06xyy08.aspx).

## <a name="configure-the-trace-listener-in-an-azure-application"></a>Bir Azure uygulamasında İzleme dinleyicisi yapılandırın
İzleme, hata ayıklama ve TraceSource, "toplamak ve gönderilen iletileri kaydetmek için dinleyicileri" ayarlamanızı gerektirir. Dinleyicileri toplamak, depolamak ve izleme iletileri yönlendirebilir. Bunlar, izleme çıktısı günlüğü, pencere veya metin dosyası gibi uygun bir hedefe yönlendirir. Azure tanılama kullanan [DiagnosticMonitorTraceListener](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.diagnosticmonitortracelistener.aspx) sınıfı.

Aşağıdaki yordamı tamamlamadan önce Azure Tanılama izleme başlatması gerekir. Bunu yapmak için bkz: [Microsoft Azure tanılama etkinleştirme](cloud-services-dotnet-diagnostics.md).

Visual Studio tarafından sağlanan şablonları kullanırsanız, dinleyiciyi yapılandırmasını otomatik olarak sizin yerinize eklendiğine dikkat edin.

### <a name="add-a-trace-listener"></a>İzleme dinleyicisi ekleme
1. Rolünüz için web.config veya app.config dosyasını açın.
2. Dosyasına aşağıdaki kodu ekleyin. Version özniteliği başvurduğunuz derlemenin sürüm numarasını kullanmak üzere değiştirin. Güncelleştirmeleri olmadıkça derleme sürümünü her Azure SDK sürümüyle mutlaka değiştirmez.
   
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
   > Proje başvurusu Microsoft.WindowsAzure.Diagnostics derleme olduğundan emin olun. Başvurulan Microsoft.WindowsAzure.Diagnostics derleme sürümüyle eşleşecek şekilde yukarıdaki xml sürüm numarasını güncelleştirin.
   > 
   > 
3. Yapılandırma dosyasını kaydedin.

Dinleyicileri hakkında daha fazla bilgi için bkz: [izleme dinleyicileri](https://msdn.microsoft.com/library/4y5y10s7.aspx).

Dinleyici ekleme adımları tamamladıktan sonra kodunuzu izleme deyimleri ekleyebilirsiniz.

### <a name="to-add-trace-statement-to-your-code"></a>Kodunuzda izleme deyimi eklemek için
1. Uygulamanız için bir kaynak dosyasını açın. Örneğin, <RoleName>web rolü ve çalışan rolü için .cs dosyası.
2. Aşağıdakileri ekleyin zaten eklenemiyor deyimi kullanarak:
    ```
        using System.Diagnostics;
    ```
3. Uygulamanızı durumuyla ilgili bilgileri yakalamak istediğiniz izleme deyimleri ekleyin. İzleme deyimi çıktısını biçimlendirmek için çeşitli yöntemler kullanabilirsiniz. Daha fazla bilgi için bkz: [nasıl yapılır: uygulama koduna izleme deyimleri ekleme](https://msdn.microsoft.com/library/zd83saa2.aspx).
4. Kaynak dosyayı kaydedin.

