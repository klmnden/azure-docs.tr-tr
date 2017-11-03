---
title: "Bulut Hizmetleri rolleri için iletişim | Microsoft Docs"
description: "Bulut Hizmetleri rol örneklerinin dışına veya diğer rol örnekleri arasında iletişim kuran kendileri için tanımlanmış uç noktaları (http, https, tcp, udp) olabilir."
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: 7008a083-acbe-4fb8-ae60-b837ef971ca1
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/14/2016
ms.author: adegeo
ms.openlocfilehash: 8e171d56bb67c971337fa383014988074ec828b1
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="enable-communication-for-role-instances-in-azure"></a>Azure rol örneklerinin iletişimi etkinleştir
Bulut hizmeti rollerinizi iç ve dış bağlantıları iletişim kurar. Dış bağlantılar denir **giriş uç noktaları** iç bağlantılar denir sırada **iç uç noktalar**. Bu konuda nasıl değiştirileceğini açıklar [hizmet tanımı](cloud-services-model-and-package.md#csdef) uç noktaları oluşturmak için.

## <a name="input-endpoint"></a>Giriş uç noktası
Bir dış bağlantı noktasında kullanıma sunmak istediğiniz giriş uç noktası kullanılır. Protokol türü ve her iki iç ve dış bağlantı noktaları için uç nokta için geçerli uç nokta bağlantı noktası belirtin. İsterseniz, farklı bir iç bağlantı noktası bitiş noktası belirtebilirsiniz [yerel bağlantı noktası](https://msdn.microsoft.com/library/azure/gg557552.aspx#InputEndpoint) özniteliği.

Giriş uç noktası aşağıdaki protokolleri kullanabilirsiniz: **http, https, tcp, udp**.

Bir giriş uç noktası oluşturmak için Ekle **Inputendpoint** alt öğeye **uç noktaları** web veya çalışan rolü öğesidir.

```xml
<Endpoints>
  <InputEndpoint name="StandardWeb" protocol="http" port="80" localPort="80" />
</Endpoints> 
```

## <a name="instance-input-endpoint"></a>Örnek giriş uç noktası
Örnek giriş uç noktaları uç noktaları ancak verir giriş benzer yük dengeleyicide bağlantı noktası iletme kullanarak her tek rol örneği için belirli genel kullanıma yönelik bağlantı noktalarını eşleme. Tek bir genel kullanıma yönelik bağlantı noktası veya bağlantı noktası aralığını belirtebilirsiniz.

Örnek giriş uç noktası yalnızca kullanabilirsiniz **tcp** veya **udp** protokol olarak.

Bir örnek giriş uç noktası oluşturmak için Ekle **InstanceInputEndpoint** alt öğeye **uç noktaları** web veya çalışan rolü öğesidir.

```xml
<Endpoints>
  <InstanceInputEndpoint name="Endpoint2" protocol="tcp" localPort="10100">
    <AllocatePublicPortFrom>
      <FixedPortRange max="10109" min="10105" />
    </AllocatePublicPortFrom>
  </InstanceInputEndpoint>
</Endpoints>
```

## <a name="internal-endpoint"></a>İç bitiş noktası
İç uç noktalar örneği, örnek iletişimi için kullanılabilir. Bağlantı noktası isteğe bağlıdır ve atlanırsa, dinamik bir bağlantı noktası bitiş noktasına atanır. Bir bağlantı noktası aralığı kullanılabilir. Beş iç uç noktalar için rol başına bir sınır yoktur.

Dahili uç noktayı aşağıdaki protokolleri kullanabilirsiniz: **http, tcp, udp herhangi**.

Bir iç giriş uç noktası oluşturmak için Ekle **InternalEndpoint** alt öğeye **uç noktaları** web veya çalışan rolü öğesidir.

```xml
<Endpoints>
  <InternalEndpoint name="Endpoint3" protocol="any" port="8999" />
</Endpoints> 
```

Bir bağlantı noktası aralığı de kullanabilirsiniz.

```xml
<Endpoints>
  <InternalEndpoint name="Endpoint3" protocol="any">
    <FixedPortRange max="8995" min="8999" />
  </InternalEndpoint>
</Endpoints>
```


## <a name="worker-roles-vs-web-roles"></a>Çalışan rollerini vs. Web rolleri
Worker ve web rolleri ile çalışırken, uç noktaları küçük bir fark yoktur. Web rolü en azından kullanarak bir tek giriş uç noktası olmalıdır **HTTP** protokolü.

```xml
<Endpoints>
  <InputEndpoint name="StandardWeb" protocol="http" port="80" localPort="80" />
  <!-- more endpoints may be declared after the first InputEndPoint -->
</Endpoints>
```

## <a name="using-the-net-sdk-to-access-an-endpoint"></a>Bir uç noktasına erişmek için .NET SDK kullanarak
Azure yönetilen kitaplık çalışma zamanında iletişim kurmak rol örnekleri için yöntemleri sağlar. İçinde bir rol örneği çalıştıran kodundan, diğer rol örneklerine ve kendi uç noktaları varlığını hakkında bilgi ve geçerli rol örneğiyle ilgili bilgiler alabilirsiniz.

> [!NOTE]
> Yalnızca bulut hizmetiniz çalıştıran ve en az bir iç uç nokta tanımlayan rol örnekleri hakkında bilgi alabilirsiniz. Farklı bir hizmet olarak çalışan rolü örnekleri hakkında veri alınamıyor.
> 
> 

Kullanabileceğiniz [örnekleri](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.role.instances.aspx) özelliği bir rolün örnekleri alınamadı. İlk kez kullanan [CurrentRoleInstance](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.currentroleinstance.aspx) geçerli rol örneğine başvuru dönmek ve daha sonra kullanmak için [rol](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleinstance.role.aspx) rol bir başvuru döndürmek için özellik.

.NET SDK'sı aracılığıyla programlı olarak bir rol örneği bağlandığınızda, uç nokta bilgileri erişim oldukça kolaydır. Örneğin, belirli bir rol ortamına zaten bağlandıktan sonra bu kod ile belirli bir uç bağlantı noktasını alabilirsiniz:

```csharp
int port = RoleEnvironment.CurrentRoleInstance.InstanceEndpoints["StandardWeb"].IPEndpoint.Port;
```

**Örnekleri** özelliği bir koleksiyonunu döndürür **RoleInstance** nesneleri. Bu koleksiyon her zaman geçerli örneğini içerir. Rol iç uç nokta tanımlamıyorsa, koleksiyonu geçerli örneği, ancak başka bir örnek içerir. Koleksiyon rol örneği sayısı her zaman 1 iç bitiş noktası rolü için tanımlandığı durumda olacaktır. Rol iç uç nokta tanımlıyorsa, onun örneklerinin çalışma zamanında bulunabilir ve koleksiyon örneği sayısı, rol hizmeti yapılandırma dosyasında belirtilen örnek sayısı karşılık gelir.

> [!NOTE]
> Azure yönetilen kitaplığı, diğer rol örneklerine durumunu belirlemek için bir araç sağlamaz, ancak hizmetiniz bu işlevsellik gerektiriyorsa, bu tür sistem durumu değerlendirmesi kendiniz uygulayabilirsiniz. Kullanabileceğiniz [Azure tanılama](cloud-services-dotnet-diagnostics.md) rol örnekleri çalıştırma hakkında bilgi edinmek için.
> 
> 

Bir rol örneğinde iç uç nokta bağlantı noktası numarasını belirlemek için kullanabileceğiniz [InstanceEndpoints](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleinstance.instanceendpoints.aspx) uç nokta adlarını ve karşılık gelen IP içeren bir sözlük nesnesi adresleri döndürmek için özellik ve bağlantı noktaları. [IPEndpoint](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleinstanceendpoint.ipendpoint.aspx) özelliği, belirtilen bir uç noktası için bağlantı noktası ve IP adresi döndürür. **PublicIPEndpoint** özelliği, bir yük dengeli uç noktası için bağlantı noktası döndürür. IP adresi kısmını **PublicIPEndpoint** özelliği kullanılamıyor.

Rol örnekleri tekrarlanan örnek aşağıda verilmiştir.

```csharp
foreach (RoleInstance roleInst in RoleEnvironment.CurrentRoleInstance.Role.Instances)
{
    Trace.WriteLine("Instance ID: " + roleInst.Id);
    foreach (RoleInstanceEndpoint roleInstEndpoint in roleInst.InstanceEndpoints.Values)
    {
        Trace.WriteLine("Instance endpoint IP address and port: " + roleInstEndpoint.IPEndpoint);
    }
}
```

Burada gösterilen uç noktası hizmet tanımını alır ve bağlantıları dinlemeyi başlatır çalışan rolü bir örneğidir.

> [!WARNING]
> Bu kod yalnızca dağıtılan bir hizmet olarak çalışır. Azure işlem öykünücüsü ' çalıştırırken, doğrudan bağlantı noktası uç noktaları oluşturma yapılandırma öğelerini service (**InstanceInputEndpoint** öğeleri) göz ardı edilir.
> 
> 

```csharp
using System;
using System.Diagnostics;
using System.Linq;
using System.Net;
using System.Net.Sockets;
using System.Threading;
using Microsoft.WindowsAzure;
using Microsoft.WindowsAzure.Diagnostics;
using Microsoft.WindowsAzure.ServiceRuntime;
using Microsoft.WindowsAzure.StorageClient;

namespace WorkerRole1
{
  public class WorkerRole : RoleEntryPoint
  {
    public override void Run()
    {
      try
      {
        // Initialize method-wide variables
        var epName = "Endpoint1";
        var roleInstance = RoleEnvironment.CurrentRoleInstance;

        // Identify direct communication port
        var myPublicEp = roleInstance.InstanceEndpoints[epName].PublicIPEndpoint;
        Trace.TraceInformation("IP:{0}, Port:{1}", myPublicEp.Address, myPublicEp.Port);

        // Identify public endpoint
        var myInternalEp = roleInstance.InstanceEndpoints[epName].IPEndpoint;

        // Create socket listener
        var listener = new Socket(
          myInternalEp.AddressFamily, SocketType.Stream, ProtocolType.Tcp);

        // Bind socket listener to internal endpoint and listen
        listener.Bind(myInternalEp);
        listener.Listen(10);
        Trace.TraceInformation("Listening on IP:{0},Port: {1}",
          myInternalEp.Address, myInternalEp.Port);

        while (true)
        {
          // Block the thread and wait for a client request
          Socket handler = listener.Accept();
          Trace.TraceInformation("Client request received.");

          // Define body of socket handler
          var handlerThread = new Thread(
            new ParameterizedThreadStart(h =>
            {
              var socket = h as Socket;
              Trace.TraceInformation("Local:{0} Remote{1}",
                socket.LocalEndPoint, socket.RemoteEndPoint);

              // Shut down and close socket
              socket.Shutdown(SocketShutdown.Both);
              socket.Close();
            }
          ));

          // Start socket handler on new thread
          handlerThread.Start(handler);
        }
      }
      catch (Exception e)
      {
        Trace.TraceError("Caught exception in run. Details: {0}", e);
      }
    }

    public override bool OnStart()
    {
      // Set the maximum number of concurrent connections 
      ServicePointManager.DefaultConnectionLimit = 12;

      // For information on handling configuration changes
      // see the MSDN topic at http://go.microsoft.com/fwlink/?LinkId=166357.
      return base.OnStart();
    }
  }
}
```

## <a name="network-traffic-rules-to-control-role-communication"></a>Rol iletişimi denetlemek için ağ trafiği kuralları
İç uç noktalar tanımladıktan sonra rol örnekleri birbirleri ile nasıl iletişim kurabilir denetimine (oluşturduğunuz uç noktalarda bağlı olarak) ağ trafiği kuralları ekleyebilirsiniz. Aşağıdaki diyagramda rol iletişimi denetlemek için bazı yaygın senaryolar gösterilmektedir:

![Ağ trafik kuralı senaryoları](./media/cloud-services-enable-communication-role-instances/scenarios.png "ağ trafik kuralı senaryoları")

Aşağıdaki kod örneğinde, önceki diyagramda gösterildiği rol için rol tanımları gösterir. Her rol tanımı tanımlanan en az bir dahili uç noktayı içerir:

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <WebRole name="WebRole1" vmsize="Medium">
    <Sites>
      <Site name="Web">
        <Bindings>
          <Binding name="HttpIn" endpointName="HttpIn" />
        </Bindings>
      </Site>
    </Sites>
    <Endpoints>
      <InputEndpoint name="HttpIn" protocol="http" port="80" />
      <InternalEndpoint name="InternalTCP1" protocol="tcp" />
    </Endpoints>
  </WebRole>
  <WorkerRole name="WorkerRole1">
    <Endpoints>
      <InternalEndpoint name="InternalTCP2" protocol="tcp" />
    </Endpoints>
  </WorkerRole>
  <WorkerRole name="WorkerRole2">
    <Endpoints>
      <InternalEndpoint name="InternalTCP3" protocol="tcp" />
      <InternalEndpoint name="InternalTCP4" protocol="tcp" />
    </Endpoints>
  </WorkerRole>
</ServiceDefinition>
```

> [!NOTE]
> Kısıtlama rolleri arasındaki iletişim, hem sabit ve bağlantı noktaları otomatik olarak atanan iç uç nokta ile ortaya çıkabilir.
> 
> 

İç uç nokta tanımlandıktan sonra varsayılan olarak, herhangi bir kısıtlama olmadan bir rolü iç uç noktası için herhangi bir rolü iletişimi akabilir. İletişim kısıtlamak için eklemelisiniz bir **NetworkTrafficRules** öğesine **ServiceDefinition** hizmet tanımı dosyasındaki öğesi.

### <a name="scenario-1"></a>Senaryo 1
Yalnızca gelen ağ trafiğinin izin **WebRole1** için **WorkerRole1**.

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo>
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP2" roleName="WorkerRole1"/>
      </Destinations>
      <AllowAllTraffic/>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WebRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
</ServiceDefinition>
```

### <a name="scenario-2"></a>Senaryo 2
Yalnızca gelen ağ trafiğine izin verir **WebRole1** için **WorkerRole1** ve **WorkerRole2**.

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo>
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP2" roleName="WorkerRole1"/>
        <RoleEndpoint endpointName="InternalTCP3" roleName="WorkerRole2"/>
      </Destinations>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WebRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
</ServiceDefinition>
```

### <a name="scenario-3"></a>Senaryo 3
Yalnızca gelen ağ trafiğine izin verir **WebRole1** için **WorkerRole1**, ve **WorkerRole1** için **WorkerRole2**.

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo>
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP2" roleName="WorkerRole1"/>
      </Destinations>
      <AllowAllTraffic/>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WebRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo>
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP3" roleName="WorkerRole2"/>
      </Destinations>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WorkerRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
</ServiceDefinition>
```

### <a name="scenario-4"></a>Senaryo 4
Yalnızca gelen ağ trafiğine izin verir **WebRole1** için **WorkerRole1**, **WebRole1** için **WorkerRole2**, ve **WorkerRole1**  için **WorkerRole2**.

```xml
<ServiceDefinition name="MyService" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo>
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP2" roleName="WorkerRole1"/>
      </Destinations>
      <AllowAllTraffic/>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WebRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo >
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP3" roleName="WorkerRole2"/>
      </Destinations>
      <AllowAllTraffic/>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WorkerRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
  <NetworkTrafficRules>
    <OnlyAllowTrafficTo >
      <Destinations>
        <RoleEndpoint endpointName="InternalTCP4" roleName="WorkerRole2"/>
      </Destinations>
      <AllowAllTraffic/>
      <WhenSource matches="AnyRule">
        <FromRole roleName="WebRole1"/>
      </WhenSource>
    </OnlyAllowTrafficTo>
  </NetworkTrafficRules>
</ServiceDefinition>
```

Yukarıda kullanılan öğeleri için bir XML Şeması Başvurusu bulunabilir [burada](https://msdn.microsoft.com/library/azure/gg557551.aspx).

## <a name="next-steps"></a>Sonraki adımlar
Bulut hizmeti hakkında daha fazla bilgiyi [modeli](cloud-services-model-and-package.md).

