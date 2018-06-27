---
title: Linux üzerinde Azure Service Fabric uygulamaları için sertifikaları yapılandırma | Microsoft Docs
description: Service Fabric çalışma zamanı Linux kümede uygulamanız için sertifikaları yapılandırın
services: service-fabric
documentationcenter: NA
author: JimacoMS2
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: NA
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/26/2018
ms.author: v-jamebr
ms.openlocfilehash: 2d6d387ed12e7261d09669686c0710786a4302dd
ms.sourcegitcommit: 0fa8b4622322b3d3003e760f364992f7f7e5d6a9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37025946"
---
# <a name="certificates-and-security-on-linux-clusters"></a>Sertifikalar ve güvenlik Linux kümeleri hakkında

Bu makalede, Linux kümelerinde X.509 sertifikalarını yapılandırma hakkında bilgi sağlar.

## <a name="location-and-format-of-x509-certificates-on-linux-nodes"></a>Konum ve Linux düğümleri X.509 sertifikaları biçimi

Service Fabric genellikle bulunması X.509 sertifikalarını bekliyor */var/lib/sfcerts* Linux küme düğümlerinde dizin. Bu, küme sertifikalar, istemci sertifikaları, vb. geçerlidir. Bazı durumlarda, bir konum dışındaki belirtebilirsiniz *lib/var/sfcerts* sertifikalar için klasör. Örneğin, güvenilir Service Fabric Java SDK kullanılarak oluşturulan Hizmetleri ile bazı uygulamaya özgü sertifikalar için bir yapılandırma paketi (Settings.xml) yoluyla farklı bir konum belirtebilirsiniz. Daha fazla bilgi için bkz: [yapılandırma paketi (Settings.xml) başvurulan sertifikaları](#certificates-referenced-in-the-configuration-package-settingsxml).

Linux kümeleri için Service Fabric sertifika ve özel anahtarı içeren ya da bir .pem dosyasını veya sertifikayı içeren bir .crt dosyası ve özel anahtarı içeren bir .key dosyası olarak bulunması için sertifikaları bekliyor. Tüm dosyaları PEM biçiminde olmalıdır. 

Sertifikanızı kullanarak Azure anahtar Kasası'nı yüklerseniz, bir [Resource Manager şablonu](./service-fabric-cluster-creation-via-arm.md#create-a-service-fabric-cluster-resource-manager-template) veya [PowerShell](https://docs.microsoft.com/powershell/module/azurerm.servicefabric/?view=latest#service_fabric) komutlar, sertifikanın doğru biçimde yüklü */var/ LIB/sfcerts* her düğümde dizin. Başka bir yöntemle bir sertifika yüklerseniz, sertifika küme düğümleri üzerinde doğru şekilde yüklenmiş olduğundan emin olmanız gerekir.

## <a name="certificates-referenced-in-the-application-manifest"></a>Uygulama bildiriminde başvurulan sertifikaları

Uygulama içinde belirtilen sertifikaları bildirim, örneğin, aracılığıyla [ **Applicationmanifest** ](https://docs.microsoft.com/azure/service-fabric/service-fabric-service-model-schema-elements#secretscertificate-element) veya [ **EndpointCertificate** ](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-service-model-schema-elements#endpointcertificate-element)öğeleri, mevcut olmalıdır */var/lib/sfcerts* dizini. Uygulama bildiriminde sertifikalarını belirtmek için kullanılan öğelerin yol özniteliğinin geçmeyecek, sertifikaları varsayılan dizininde mevcut olması gerekir. Bu öğeler isteğe bağlı bir ele **X509StoreName** özniteliği. "Benim", hangi işaret varsayılandır */var/lib/sfcerts* Linux düğümleri üzerinde dizin. Başka bir değer Linux kümede tanımlanmamıştır. Atlayın öneririz **X509StoreName** özniteliği Linux kümelerinde çalışan uygulamalar için. 

## <a name="certificates-referenced-in-the-configuration-package-settingsxml"></a>Yapılandırma paketi (Settings.xml) başvurulan sertifikaları

Bazı hizmetler için X.509 sertifikaları yapılandırabilirsiniz [ConfigPackage](./service-fabric-application-and-service-manifests.md) (varsayılan olarak, Settings.xml). Service Fabric .NET Core veya Java SDK'ları ile oluşturulmuş Reliable Services hizmetleri için RPC kanalların güvenliğini sağlamak için kullanılan sertifikaları bildirirken Örneğin, bu bir durumdur. Yapılandırma paketi başvurusu sertifikaları için iki yolu vardır. Destek .NET Core ve Java SDK'ları arasında değişir.

### <a name="using-x509-securitycredentialstype"></a>X509 kullanarak SecurityCredentialsType

.NET veya Java SDK'ları ile belirtebilirsiniz **X509** için **SecurityCredentialsType**. Bu karşılık `X509Credentials` ([.NET](https://msdn.microsoft.com/library/system.fabric.x509credentials.aspx)/[Java](https://docs.microsoft.com/java/api/system.fabric._x509_credentials)) tür `SecurityCredentials` ([.NET](https://msdn.microsoft.com/library/system.fabric.securitycredentials.aspx)/[Java](https://docs.microsoft.com/java/api/system.fabric._security_credentials)).

**X509** başvuru sertifika bir sertifika depolama alanında bulur. Aşağıdaki XML sertifikasının konumunu belirtmek için kullanılan parametreler gösterir:

```xml
    <Parameter Name="SecurityCredentialsType" Value="X509" />
    <Parameter Name="CertificateStoreLocation" Value="LocalMachine" />
    <Parameter Name="CertificateStoreName" Value="My" />
```

Linux üzerinde çalışan bir hizmet için **LocalMachine**/**My** sertifikaları için varsayılan konum işaret */var/lib/sfcerts* dizini. Linux, diğer herhangi bir kombinasyonu için **CertificateStoreLocation** ve **SertifikaDeposuAdı** tanımlanmaz. 

Her zaman belirtmeniz **LocalMachine** için **CertificateStoreLocation** parametresi. Belirtmek için gerek yoktur **SertifikaDeposuAdı** parametresi, "Benim" için varsayılanları olduğundan. İle bir **X509** başvuru, sertifika dosyalarını yer alması gerekir */var/lib/sfcerts* küme düğümü üzerindeki dizin.  

Aşağıdaki XML gösterildiği bir **TransportSettings** bölümüne dayalı bu stili:

```xml
<Section Name="HelloWorldStatefulTransportSettings">
    <Parameter Name="MaxMessageSize" Value="10000000" />
    <Parameter Name="SecurityCredentialsType" Value="X509" />
    <Parameter Name="CertificateFindType" Value="FindByThumbprint" />
    <Parameter Name="CertificateFindValue" Value="4FEF3950642138446CC364A396E1E881DB76B48C" />
    <Parameter Name="CertificateRemoteThumbprints" Value="9FEF3950642138446CC364A396E1E881DB76B483" />
    <Parameter Name="CertificateStoreLocation" Value="LocalMachine" />
    <Parameter Name="CertificateProtectionLevel" Value="EncryptAndSign" />
    <Parameter Name="CertificateRemoteCommonNames" Value="ServiceFabric-Test-Cert" />
</Section>
```

### <a name="using-x5092-securitycredentialstype"></a>X509_2 SecurityCredentialsType kullanma

Java SDK ile birlikte belirttiğiniz **X509_2** için **SecurityCredentialsType**. Bu karşılık `X509Credentials2` ([Java](https://docs.microsoft.com/java/api/system.fabric._x509_credentials2)) tür `SecurityCredentials` ([Java](https://docs.microsoft.com/java/api/system.fabric._security_credentials)). 

İle bir **X509_2** başvuru, belirttiğiniz bir yol parametresi sertifika başka bir dizindeki bulabilir */var/lib/sfcerts*.  Aşağıdaki XML sertifikasının konumunu belirtmek için kullanılan parametreler gösterir: 

```xml
     <Parameter Name="SecurityCredentialsType" Value="X509_2" />
     <Parameter Name="CertificatePath" Value="/path/to/cert/BD1C71E248B8C6834C151174DECDBDC02DE1D954.crt" />
```

Aşağıdaki XML gösterildiği bir **TransportSettings** bölümüne dayalı bu stili.

```xml
<!--Section name should always end with "TransportSettings".-->
<!--Here we are using a prefix "HelloWorldStateless".-->
<Section Name="HelloWorldStatelessTransportSettings">
    <Parameter Name="MaxMessageSize" Value="10000000" />
    <Parameter Name="SecurityCredentialsType" Value="X509_2" />
    <Parameter Name="CertificatePath" Value="/path/to/cert/BD1C71E248B8C6834C151174DECDBDC02DE1D954.crt" />
    <Parameter Name="CertificateProtectionLevel" Value="EncryptandSign" />
    <Parameter Name="CertificateRemoteThumbprints" Value="BD1C71E248B8C6834C151174DECDBDC02DE1D954" />
</Section>
```

> [!NOTE]
> Sertifika, önceki XML .crt dosyası olarak belirtilir. Bu, olduğunu da aynı konumda özel anahtarı içeren bir .key dosyasının anlamına gelir.

## <a name="configure-a-reliable-services-app-to-run-on-linux-clusters"></a>Linux kümeleri üzerinde çalıştırmak için güvenilir hizmetler uygulamasını yapılandırma

Service Fabric SDK, Service Fabric çalışma zamanı platform yararlanmak için API'ler ile iletişim kurmasına izin verir. Bu işlevselliği güvenli Linux kümelerinde kullanan herhangi bir uygulama çalıştırdığınızda, Service Fabric çalışma zamanı ile doğrulamak için kullanabileceğiniz bir sertifika ile uygulamanızı yapılandırmanız gerekir. .NET Core veya Java SDK kullanılarak yazılan doku güvenilir hizmeti hizmetleri içeren uygulamalar, bu yapılandırma gerektirir. 

Bir uygulamayı yapılandırmak için ekleyin bir [ **Applicationmanifest** ](https://docs.microsoft.com/azure/service-fabric/service-fabric-service-model-schema-elements#secretscertificate-element) öğesinin altında **sertifikaları** altında bulunan etiketi **ApplicationManifest**  içinde etiketi *ApplicationManifest.xml* dosya. Aşağıdaki XML, parmak izi tarafından başvurulan bir sertifika gösterir: 

```xml
   <Certificates>
       <SecretsCertificate X509FindType="FindByThumbprint" X509FindValue="0A00AA0AAAA0AAA00A000000A0AA00A0AAAA00" />
   </Certificates>   
```

Küme veya her küme düğümünde yüklediğiniz bir sertifika başvuruda bulunabilir. Linux üzerinde sertifika dosyaları bulunmalıdır */var/lib/sfcerts* dizini. Daha fazla bilgi için bkz: [konumu ve Linux düğümleri X.509 sertifikaları biçimini](#location-and-format-of-x509-certificates-on-linux-nodes).



