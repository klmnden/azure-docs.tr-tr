---
title: Bulut Hizmetleri rolü için iletişim | Microsoft Docs
description: Cloud Services rol örneklerini dışına veya diğer rol örnekleri arasında iletişim kuran kendileri için tanımlanan uç noktaları (http, https, tcp, udp) olabilir.
services: cloud-services
documentationcenter: ''
author: jpconnock
manager: timlt
editor: ''
ms.assetid: 7008a083-acbe-4fb8-ae60-b837ef971ca1
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/14/2016
ms.author: jeconnoc
ms.openlocfilehash: 8b521ebe869210b66ac3b3efeebda873f7c0e50b
ms.sourcegitcommit: f093430589bfc47721b2dc21a0662f8513c77db1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2019
ms.locfileid: "58918170"
---
# <a name="enable-communication-for-role-instances-in-azure"></a>Azure rol örnekleri için iletişimi etkinleştirin
Bulut hizmeti rolleri, iç ve dış bağlantıları iletişim kurar. Dış bağlantılar denir **giriş uç noktaları** iç bağlantı olarak adlandırılır ancak **iç uç nokta**. Bu konu nasıl değiştirileceğini açıklar [servicedefinition](cloud-services-model-and-package.md#csdef) uç noktalar oluşturmak için.

## <a name="input-endpoint"></a>Giriş uç noktası
Giriş uç noktası dışındaki bir bağlantı noktasına göstermek istediğinizde kullanılır. Protokol türü ve ardından her iki iç ve dış bağlantı noktaları için uç noktası için geçerli uç nokta bağlantı noktası belirtin. İsterseniz, farklı bir iç bağlantı uç noktası için belirtebileceğiniz [yerel bağlantı noktası](/previous-versions/azure/reference/gg557552(v=azure.100)#InputEndpoint) özniteliği.

Giriş uç noktası şu protokolden kullanabilirsiniz: **http, https, tcp, udp**.

Giriş uç noktası oluşturmak için **Inputendpoint** alt öğeye **uç noktaları** bir web veya çalışan rolü öğesidir.

```xml
<Endpoints>
  <InputEndpoint name="StandardWeb" protocol="http" port="80" localPort="80" />
</Endpoints> 
```

## <a name="instance-input-endpoint"></a>Örnek giriş uç noktası
Örnek giriş uç noktaları giriş uç noktaları ancak sayesinde benzer yük dengeleyicide bağlantı noktası iletme'yi kullanarak her tek tek rol örneği için belirli genel kullanıma yönelik bağlantı noktalarını eşleme. Tek bir genel kullanıma yönelik bağlantı noktası veya bir bağlantı noktası aralığı belirtebilirsiniz.

Örnek giriş uç noktası yalnızca kullanabilirsiniz **tcp** veya **udp** protokol olarak.

Örnek giriş uç noktası oluşturmak için **InstanceInputEndpoint** alt öğeye **uç noktaları** bir web veya çalışan rolü öğesidir.

```xml
<Endpoints>
  <InstanceInputEndpoint name="Endpoint2" protocol="tcp" localPort="10100">
    <AllocatePublicPortFrom>
      <FixedPortRange max="10109" min="10105" />
    </AllocatePublicPortFrom>
  </InstanceInputEndpoint>
</Endpoints>
```

## <a name="internal-endpoint"></a>İç uç nokta
İç uç noktalar örneği, örnek iletişim için kullanılabilir. Bağlantı noktası isteğe bağlıdır ve dinamik bir bağlantı noktası belirtilmezse, uç noktaya atanır. Bir bağlantı noktası aralığı kullanılabilir. Beş iç uç noktalar için rol başına bir sınırlama yoktur.

İç uç nokta şu protokolden kullanabilirsiniz: **http, tcp, udp, tüm**.

İç giriş uç noktası oluşturmak için **InternalEndpoint** alt öğeye **uç noktaları** bir web veya çalışan rolü öğesidir.

```xml
<Endpoints>
  <InternalEndpoint name="Endpoint3" protocol="any" port="8999" />
</Endpoints> 
```

Bir bağlantı noktası aralığı de kullanabilirsiniz.

```xml
<Endpoints>
  <InternalEndpoint name="Endpoint3" protocol="any">
    <FixedPortRange max="8999" min="8995" />
  </InternalEndpoint>
</Endpoints>
```


## <a name="worker-roles-vs-web-roles"></a>Çalışan rolleri vs. Web rolleri
Hem çalışan hem de web rolleri ile çalışırken, uç noktaları küçük bir fark yoktur. Web rolü en azından kullanarak tek bir giriş uç noktası olmalıdır **HTTP** protokolü.

```xml
<Endpoints>
  <InputEndpoint name="StandardWeb" protocol="http" port="80" localPort="80" />
  <!-- more endpoints may be declared after the first InputEndPoint -->
</Endpoints>
```

## <a name="using-the-net-sdk-to-access-an-endpoint"></a>Bir uç noktasına erişmek için .NET SDK kullanarak
Azure yönetilen kitaplık çalışma zamanında iletişim kurmak rol örnekleri için yöntemler sağlar. Bir rol örneğinin içinde çalışan koddan varlığı ve diğer rol örnekleri kendi uç noktaları hakkında bilgi yanı sıra, geçerli rol örneği hakkında bilgi alabilirsiniz.

> [!NOTE]
> Yalnızca bulut hizmetinizi çalıştıran ve en az bir iç uç nokta tanımlayan rol örnekleri hakkında bilgi alabilirsiniz. Farklı bir hizmet olarak çalışan rolü örnekleri hakkında veri alınamıyor.
> 
> 

Kullanabileceğiniz [örnekleri](/previous-versions/azure/reference/ee741904(v=azure.100)) bir rolün örnekleri almak için özellik. İlk olarak [CurrentRoleInstance](/previous-versions/azure/reference/ee741907(v=azure.100)) geçerli rol örneği için bir başvuru döndürmeyi ve daha sonra [rol](/previous-versions/azure/reference/ee741918(v=azure.100)) rolü bir başvuru döndürmek için özellik.

.NET SDK'sı aracılığıyla programlı olarak bir rol örneği bağlandığınızda, uç nokta bilgileri erişmek oldukça kolaydır. Örneğin, bir özel rol ortamı için zaten bağlandıktan sonra bu kod ile belirli bir uç nokta bağlantı noktası alabilirsiniz:

```csharp
int port = RoleEnvironment.CurrentRoleInstance.InstanceEndpoints["StandardWeb"].IPEndpoint.Port;
```

**Örnekleri** özellik koleksiyonunu döndürür **Roleınstance** nesneleri. Bu koleksiyon her zaman geçerli örneğini içerir. Bir iç uç nokta rolü tanımlamıyorsa, koleksiyon geçerli örneğin ancak başka bir örnek içerir. Rol örneklerinin koleksiyondaki her zaman 1 iç uç nokta rolü için tanımlandığı durumda olacaktır. Bir iç uç nokta rolü tanımlar, örneklerini çalışma zamanında bulunabilir ve koleksiyondaki örneklerinin rol hizmeti yapılandırma dosyasında belirtilen örnek sayısını karşılık gelir.

> [!NOTE]
> Azure yönetilen kitaplığı diğer rol örneklerinin durumunu belirleyen bir yol sağlamaz, ancak hizmetiniz bu işlevler gerekiyorsa, bu tür sistem durumu değerlendirme kendiniz uygulayabilirsiniz. Kullanabileceğiniz [Azure tanılama](cloud-services-dotnet-diagnostics.md) rol örneklerini çalıştırma hakkında bilgi edinmek için.
> 
> 

Bir rol örneğinde bir iç uç nokta bağlantı noktası numarasını belirlemek için kullanabileceğiniz [InstanceEndpoints](/previous-versions/azure/reference/ee741917(v=azure.100)) dönmesini uç nokta adlarını ve karşılık gelen IP içeren bir sözlük nesnesi adresleri ve bağlantı noktaları. [IPEndpoint](/previous-versions/azure/reference/ee741919(v=azure.100)) özelliği, belirtilen bir uç noktası için bağlantı noktası ve IP adresi döndürür. **PublicIPEndpoint** özelliği, bir yük dengeli uç noktası için bağlantı noktasını döndürür. IP adresi bölümü **PublicIPEndpoint** özelliği kullanılmaz.

Rol örnekleri yinelenen bir örnek aşağıda verilmiştir.

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

Hizmet tanımı kullanıma sunulan endpoint alır ve bağlantıları dinlemeyi başlatan bir çalışan rolü örneği aşağıda verilmiştir.

> [!WARNING]
> Bu kod, yalnızca dağıtılmış bir hizmet için çalışır. Azure işlem öykünücüsü'nde çalıştırırken, doğrudan bir bağlantı noktası uç noktaları oluşturma yapılandırma öğeleri hizmet (**InstanceInputEndpoint** öğeleri) göz ardı edilir.
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
      // see the MSDN topic at https://go.microsoft.com/fwlink/?LinkId=166357.
      return base.OnStart();
    }
  }
}
```

## <a name="network-traffic-rules-to-control-role-communication"></a>Roller arası iletişimi denetlemek için ağ trafiği kuralları
İç uç nokta tanımladıktan sonra rol örnekleri birbirleriyle nasıl iletişim denetim (oluşturduğunuz Uç noktalara bağlı olarak) ağ trafiği kuralları ekleyebilirsiniz. Aşağıdaki diyagramda roller arası iletişimi denetlemek için bazı yaygın senaryolar gösterilmektedir:

![Ağ trafik kuralı senaryoları](./media/cloud-services-enable-communication-role-instances/scenarios.png "ağ trafik kuralı senaryoları")

Aşağıdaki kod örneği önceki Diyagramda gösterilen rolleri için rol tanımları gösterir. Her rol tanımı tanımlanmış en az bir iç uç nokta içerir:

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
> Kısıtlama rolleri arasındaki iletişim, hem sabit hem otomatik olarak bağlantı noktaları atar. iç uç noktaları ile ortaya çıkabilir.
> 
> 

Bir iç uç nokta tanımlandıktan sonra varsayılan olarak, bir rol herhangi bir kısıtlama olmadan iç uç noktasına herhangi bir rol iletişim akabilir. İletişimi sınırlandırmak için eklemelisiniz bir **NetworkTrafficRules** öğesine **ServiceDefinition** hizmet tanımı dosyasında öğe.

### <a name="scenario-1"></a>Senaryo 1
Yalnızca gelen ağ trafiğini izin **WebRole1** için **WorkerRole1**.

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
Yalnızca gelen ağ trafiğine izin verecek **WebRole1** için **WorkerRole1** ve **WorkerRole2**.

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
Yalnızca gelen ağ trafiğine izin verecek **WebRole1** için **WorkerRole1**, ve **WorkerRole1** için **WorkerRole2**.

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
Yalnızca gelen ağ trafiğine izin verecek **WebRole1** için **WorkerRole1**, **WebRole1** için **WorkerRole2**, ve **WorkerRole1**  için **WorkerRole2**.

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

Yukarıda kullanılan öğeler için bir XML Şeması Başvurusu bulunabilir [burada](/previous-versions/azure/reference/gg557551(v=azure.100)).

## <a name="next-steps"></a>Sonraki adımlar
Bulut hizmeti hakkında daha fazla bilgiyi [model](cloud-services-model-and-package.md).

