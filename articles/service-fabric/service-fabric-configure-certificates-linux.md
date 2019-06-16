---
title: Linux üzerinde Azure Service Fabric uygulamaları için sertifikaları yapılandırma | Microsoft Docs
description: Bir Linux kümesinde Service Fabric çalışma zamanı ile uygulamanız için sertifikaları yapılandırma
services: service-fabric
documentationcenter: NA
author: JimacoMS2
manager: chackdan
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: NA
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/26/2018
ms.author: v-jamebr
ms.openlocfilehash: c0580b75544a9613bc8caf2faaac11ba1ba6708e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60881377"
---
# <a name="certificates-and-security-on-linux-clusters"></a>Sertifikalar ve güvenlik Linux kümelerinde

Bu makalede Linux kümelerinde X.509 sertifikaları yapılandırma hakkında bilgi sağlar.

## <a name="location-and-format-of-x509-certificates-on-linux-nodes"></a>Konum ve X.509 sertifikaları Linux düğümlerinde biçimi

Service Fabric genel bulunması X.509 sertifikaları bekliyor */var/lib/sfcerts* Linux küme düğümlerinde dizin. Bu, küme sertifikası, istemci sertifikaları, vb. geçerlidir. Bazı durumlarda, bir konum dışında belirtebilirsiniz *var/lib/sfcerts* sertifikaları için klasör. Örneğin, güvenilir Hizmetleri ile Service Fabric Java SDK'sı kullanılarak oluşturulan bazı uygulamaya özgü sertifikalar için yapılandırma paketi (Settings.xml) üzerinden farklı bir konum belirtebilirsiniz. Daha fazla bilgi için bkz. [sertifikaları yapılandırma paketini (Settings.xml) başvurulan](#certificates-referenced-in-the-configuration-package-settingsxml).

Linux kümeleri için Service Fabric sertifikaları sertifika ve özel anahtarı içeren bir .pem dosyasını ya da sertifikasını içeren bir .crt dosyası ve özel anahtarı içeren .key dosyası olarak veya mevcut olması için bekler. Tüm dosyaları PEM biçiminde olmalıdır. 

Sertifikanızı kullanarak Azure Key Vault'tan yüklerseniz, bir [Resource Manager şablonu](./service-fabric-cluster-creation-create-template.md) veya [PowerShell](https://docs.microsoft.com/powershell/module/azurerm.servicefabric/?view=latest#service_fabric) komutlar, sertifikanın doğru biçimde yüklü */var/ lib/sfcerts* her düğümde dizin. Başka bir yöntem aracılığıyla sertifika yüklerseniz, sertifika, küme düğümleri üzerinde doğru şekilde yüklendiğinden emin olmanız gerekir.

## <a name="certificates-referenced-in-the-application-manifest"></a>Uygulama bildiriminde atıf yapılan sertifikaları

Sertifikalar uygulamada belirtilen bildirimi, örneğin, aracılığıyla [ **SecretsCertificate** ](https://docs.microsoft.com/azure/service-fabric/service-fabric-service-model-schema-elements#secretscertificate-element) veya [ **EndpointCertificate** ](https://docs.microsoft.com/azure/service-fabric/service-fabric-service-model-schema-elements#endpointcertificate-element)öğeleri içinde bulunmalıdır */var/lib/sfcerts* dizin. Sertifikaları varsayılan dizininde mevcut olmalıdır. uygulama bildiriminde sertifikalarını belirtmek için kullanılan öğeleri bir path özniteliği almaz. Bu öğeleri isteğe bağlı olarak ele **X509StoreName** özniteliği. "My", işaret varsayılandır */var/lib/sfcerts* Linux düğümlerinde dizin. Bir Linux kümesinde başka bir değer tanımsız olur. Atlarsanız, öneririz **X509StoreName** Linux kümelerinde çalıştırılan uygulamalar için özniteliği. 

## <a name="certificates-referenced-in-the-configuration-package-settingsxml"></a>Yapılandırma paketi (Settings.xml) başvurulan sertifikaları

Bazı hizmetler için X.509 sertifikaları yapılandırabileceğiniz [ConfigPackage](./service-fabric-application-and-service-manifests.md) (varsayılan olarak, Settings.xml). Örneğin, Service Fabric .NET Core veya Java SDK'ları ile oluşturulmuş Reliable Services hizmetleri için RPC kanalların güvenliğini sağlamak için kullanılan sertifikaları bildirdiğinizde bu durum geçerlidir. Yapılandırma paketi başvurusu sertifikaları için iki yolu vardır. Destek .NET Core ve Java SDK'ları arasında değişir.

### <a name="using-x509-securitycredentialstype"></a>X509 kullanarak SecurityCredentialsType

.NET veya Java SDK'ları ile belirtebileceğiniz **X509** için **SecurityCredentialsType**. Bu karşılık gelir `X509Credentials` ([.NET](https://msdn.microsoft.com/library/system.fabric.x509credentials.aspx)/[Java](https://docs.microsoft.com/java/api/system.fabric.x509credentials)) tür `SecurityCredentials` ([.NET](https://msdn.microsoft.com/library/system.fabric.securitycredentials.aspx)/[Java](https://docs.microsoft.com/java/api/system.fabric.securitycredentials)).

**X509** başvuru, bir sertifika deposunda sertifika bulur. Aşağıdaki XML sertifikasının konumunu belirtmek için kullanılan parametreler gösterilmektedir:

```xml
    <Parameter Name="SecurityCredentialsType" Value="X509" />
    <Parameter Name="CertificateStoreLocation" Value="LocalMachine" />
    <Parameter Name="CertificateStoreName" Value="My" />
```

Linux üzerinde çalışan bir hizmetin **LocalMachine**/**My** sertifikalar, varsayılan konumu işaret */var/lib/sfcerts* dizin. Linux, diğer herhangi bir kombinasyonu **CertificateStoreLocation** ve **SertifikaDeposuAdı** tanımsızdır. 

Her zaman belirtin **LocalMachine** için **CertificateStoreLocation** parametresi. Belirtmek için gerek yoktur **SertifikaDeposuAdı** parametresi olduğundan, "My" için varsayılanları. İle bir **X509** başvuru, sertifika dosyalarını bulunması gerekir içinde */var/lib/sfcerts* küme düğümünde dizin.  

Aşağıdaki XML gösterildiği bir **TransportSettings** bölümü bu stil dayalı:

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

Java SDK ile birlikte belirttiğiniz **X509_2** için **SecurityCredentialsType**. Bu karşılık gelir `X509Credentials2` ([Java](https://docs.microsoft.com/java/api/system.fabric.x509credentials2)) tür `SecurityCredentials` ([Java](https://docs.microsoft.com/java/api/system.fabric.securitycredentials)). 

İle bir **X509_2** başvuru, bir yol parametresi dışında bir dizinde sertifika bulabilir belirtin */var/lib/sfcerts*.  Aşağıdaki XML sertifikasının konumunu belirtmek için kullanılan parametreler gösterilmektedir: 

```xml
     <Parameter Name="SecurityCredentialsType" Value="X509_2" />
     <Parameter Name="CertificatePath" Value="/path/to/cert/BD1C71E248B8C6834C151174DECDBDC02DE1D954.crt" />
```

Aşağıdaki XML gösterildiği bir **TransportSettings** bölümüne dayalı bu stil.

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
> Sertifika, .crt önceki XML dosyası olarak belirtilir. Bu olduğunu da aynı konumda özel anahtarı içeren bir .key dosya anlamına gelir.

## <a name="configure-a-reliable-services-app-to-run-on-linux-clusters"></a>Linux kümeleri üzerinde çalıştırmak için bir Reliable Services uygulaması yapılandırma

Service Fabric SDK'ları Service Fabric çalışma zamanı API'ları platformdan yararlanın ile iletişim kurmasına olanak sağlar. Güvenli bir Linux kümelerinde bu işlevselliği kullanan tüm uygulamaları çalıştırdığınızda, Service Fabric çalışma zamanı ile doğrulamak için kullanabileceğiniz bir sertifika ile uygulamanızı yapılandırmanız gerekir. Java SDK'ları ve .NET Core kullanarak Service Fabric güvenilir hizmeti hizmetleri içeren uygulamalar, bu yapılandırma gerektirir. 

Bir uygulamayı yapılandırmak için Ekle bir [ **SecretsCertificate** ](https://docs.microsoft.com/azure/service-fabric/service-fabric-service-model-schema-elements#secretscertificate-element) öğesi altında **sertifikaları** altında bulunan etiket **ApplicationManifest**  içindeki *ApplicationManifest.xml* dosya. Aşağıdaki XML bir sertifika parmak izi tarafından başvurulan gösterir: 

```xml
   <Certificates>
       <SecretsCertificate X509FindType="FindByThumbprint" X509FindValue="0A00AA0AAAA0AAA00A000000A0AA00A0AAAA00" />
   </Certificates>   
```

Küme sertifikası ya da her küme düğümüne yükleme bir sertifika başvurabilirsiniz. Sertifika dosyaları, Linux üzerinde mevcut olmalıdır */var/lib/sfcerts* dizin. Daha fazla bilgi için bkz. [konumu ve X.509 sertifikaları Linux düğümlerinde biçimini](#location-and-format-of-x509-certificates-on-linux-nodes).



