---
title: Azure Service Fabric ters proxy güvenli iletişim | Microsoft Docs
description: Güvenli uçtan uca iletişimi etkinleştirmek için ters proxy yapılandırın.
services: service-fabric
documentationcenter: .net
author: kavyako
manager: vipulm
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 08/10/2017
ms.author: kavyako
ms.openlocfilehash: 237a72fd282b29d3032675ccf3fb350f8db59ef7
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
---
# <a name="connect-to-a-secure-service-with-the-reverse-proxy"></a>Ters proxy ile güvenli bir hizmete bağlanma

Bu makalede, ters proxy ve bu nedenle bir uçtan uca güvenli kanal Etkinleştirme Hizmetleri arasında güvenli bir bağlantı kurmak açıklanmaktadır.

Ters proxy HTTPS üzerinde dinleme yapılandırıldığında, güvenli hizmetlerine bağlanan desteklenir. Belgenin geri kalanında, bu durumda olduğunu varsayar.
Başvurmak [ters proxy Azure Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-reverseproxy) ters proxy Service Fabric yapılandırmak için.

## <a name="secure-connection-establishment-between-the-reverse-proxy-and-services"></a>Ters proxy ve hizmetleri arasında güvenli bağlantı kurma 

### <a name="reverse-proxy-authenticating-to-services"></a>Kimlik doğrulama hizmetleri için ters proxy:
Kendisini tanımlayan ters proxy ile belirtilen sertifikasını kullanarak hizmetlerine ***reverseProxyCertificate*** özelliğinde **küme** [kaynak türü bölümü](../azure-resource-manager/resource-group-authoring-templates.md). Hizmetleri ters Ara sunucu tarafından sunulan sertifika doğrulamak için mantığı uygulayabilirsiniz. Hizmetleri yapılandırma ayarları yapılandırma paketi olarak kabul edilen istemci sertifikası ayrıntıları belirtebilirsiniz. Bu çalışma zamanında okunabilir ve ters proxy sunucu tarafından sunulan sertifika doğrulamak için kullanılır. Başvurmak [uygulama parametreleri yönetmek](service-fabric-manage-multiple-environment-app-configuration.md) yapılandırma ayarlarını eklemek için. 

### <a name="reverse-proxy-verifying-the-services-identity-via-the-certificate-presented-by-the-service"></a>Ters proxy hizmeti tarafından sunulan sertifika yoluyla hizmetin kimlik doğrulama:
Hizmetler tarafından sunulan sertifikaların sunucu sertifikası doğrulamayı gerçekleştirmek için ters proxy aşağıdaki seçeneklerden birini destekler: None, ServiceCommonNameAndIssuer ve ServiceCertificateThumbprints.
Bu üç seçenekten birini seçmek için **ApplicationCertificateValidationPolicy** ApplicationGateway/Http öğesinin altında Parametreler bölümünde [fabricSettings](service-fabric-cluster-fabric-settings.md).

```json
{
"fabricSettings": [
          ...
          {
            "name": "ApplicationGateway/Http",
            "parameters": [
              {
                "name": "ApplicationCertificateValidationPolicy",
                "value": "None"
              }
            ]
          }
        ],
        ...
}
```

Bu seçeneklerin her biri için ek yapılandırma hakkında ayrıntılı bilgi için sonraki bölüme bakın.

### <a name="service-certificate-validation-options"></a>Hizmet sertifika doğrulama seçenekleri 

- **Hiçbiri**: ters proxy yönlendirilirken hizmet sertifikası doğrulama atlar ve güvenli bir bağlantı kurar. Bu varsayılan davranıştır.
Belirtin **ApplicationCertificateValidationPolicy** değerle **hiçbiri** ApplicationGateway/Http öğesinin Parametreler bölümünde.

- **ServiceCommonNameAndIssuer**: ters proxy sertifikanın ortak adı ve hemen verenin parmak izi bağlı hizmeti tarafından sunulan sertifika doğrular: belirtin **ApplicationCertificateValidationPolicy** değerle **ServiceCommonNameAndIssuer** ApplicationGateway/Http öğesinin Parametreler bölümünde.

```json
{
"fabricSettings": [
          ...
          {
            "name": "ApplicationGateway/Http",
            "parameters": [
              {
                "name": "ApplicationCertificateValidationPolicy",
                "value": "ServiceCommonNameAndIssuer"
              }
            ]
          }
        ],
        ...
}
```

Hizmet ortak adı ve verenin parmak izleri listesini belirtmek için ekleyin bir **Http/ApplicationGateway/ServiceCommonNameAndIssuer** öğesinin aşağıda gösterildiği gibi fabricSettings altında. Birden çok sertifika ortak adı ve verenin parmak izi çiftleri parametreleri dizi öğesindeki eklenebilir. 

Uç nokta ters proxy sunar için ortak olan bir sertifika bağlanılıyorsa burada belirtilen değerlerden herhangi birini adı ve verenin parmak iziyle eşleşen, SSL kanalı oluşturulur. Sertifika ayrıntılarını eşleşecek şekilde başarısızlık durumunda, ters proxy istemcinin istek 502 (hatalı ağ geçidi) durum kodu ile başarısız olur. HTTP durum satırı da "Geçersiz SSL sertifikası" ifadesini içerir 

```json
{
"fabricSettings": [
          ...
          {
             "name": "ApplicationGateway/Http/ServiceCommonNameAndIssuer",
            "parameters": [
              {
                "name": "WinFabric-Test-Certificate-CN1",
                "value": "b3 44 9b 01 8d 0f 68 39 a2 c5 d6 2b 5b 6c 6a c8 22 b4 22 11"
              },
              {
                "name": "WinFabric-Test-Certificate-CN2",
                "value": "b3 44 9b 01 8d 0f 68 39 a2 c5 d6 2b 5b 6c 6a c8 22 11 33 44"
              }
            ]
          }
        ],
        ...
}
```


- **ServiceCertificateThumbprints**: ters proxy kendi parmak izine dayanan yönlendirilirken hizmet sertifika doğrulama. Hizmetleri kendini ile yapılandırıldığında, bu rota imzalı sertifikaları gitmek seçebilirsiniz: belirtin **ApplicationCertificateValidationPolicy** değerle **ServiceCertificateThumbprints** ApplicationGateway/Http öğesinin Parametreler bölümünde.

```json
{
"fabricSettings": [
          ...
          {
            "name": "ApplicationGateway/Http",
            "parameters": [
              {
                "name": "ApplicationCertificateValidationPolicy",
                "value": "ServiceCertificateThumbprints"
              }
            ]
          }
        ],
        ...
}
```

Ayrıca parmak izleriyle belirtin bir **ServiceCertificateThumbprints** giriş parametreleri bölümüne ApplicationGateway/Http öğesinin altında. Birden çok parmak izleri aşağıda gösterildiği gibi değeri alanına, virgülle ayrılmış liste olarak belirtilebilir:

```json
{
"fabricSettings": [
          ...
          {
            "name": "ApplicationGateway/Http",
            "parameters": [
                ...
              {
                "name": "ServiceCertificateThumbprints",
                "value": "78 12 20 5a 39 d2 23 76 da a0 37 f0 5a ed e3 60 1a 7e 64 bf,78 12 20 5a 39 d2 23 76 da a0 37 f0 5a ed e3 60 1a 7e 64 b9"
              }
            ]
          }
        ],
        ...
}
```

Sunucu sertifikasının parmak izi bu yapılandırma girişte listeleniyorsa, ters proxy SSL bağlantı başarılı olur. Aksi takdirde, bağlantıyı sonlandırır ve istemcinin istek 502 (hatalı ağ geçidi) ile başarısız olur. HTTP durum satırı da "Geçersiz SSL sertifikası" ifadesini içerir

## <a name="endpoint-selection-logic-when-services-expose-secure-as-well-as-unsecured-endpoints"></a>Hizmetleri güvenli yanı sıra güvenli uç noktalarını kullanıma sunar, uç nokta seçme mantığı
Service fabric, bir hizmet için birden fazla uç noktası yapılandırılmasını destekler. Bkz. [Bir hizmet bildiriminde kaynak belirtme](service-fabric-service-manifest-resources.md).

Ters proxy seçer dayalı istek iletmek için uç noktalardan biri **ListenerName** sorgu parametresi. Bu belirtilmezse, herhangi bir uç nokta uç noktalar listesinden seçebilirsiniz. Şimdi bu bir HTTP veya HTTPS uç noktası olabilir. Yani bir "güvenli mod" çalışması için ters proxy istediğiniz senaryolar/gereksinimleri olabilir güvenli olmayan uç noktaları isteklerini iletmek için güvenli ters proxy istemezsiniz. Bu belirterek sağlanabilir **SecureOnlyMode** yapılandırma girdisi değerle **true** ApplicationGateway/Http öğesinin Parametreler bölümünde.   

```json
{
"fabricSettings": [
          ...
          {
            "name": "ApplicationGateway/Http",
            "parameters": [
                ...
              {
                "name": "SecureOnlyMode",
                "value": true
              }
            ]
          }
        ],
        ...
}
```

> 
> İçinde çalışırken **SecureOnlyMode**, istemci belirtilmişse bir **ListenerName** HTTP(unsecured) uç noktasına karşılık gelen ters proxy istek 404 (bulunamadı) HTTP durum kodu ile başarısız olur.

## <a name="setting-up-client-certificate-authentication-through-the-reverse-proxy"></a>İstemci sertifikası kimlik doğrulaması ters proxy aracılığıyla ayarlama
Ters proxy SSL sonlandırma olur ve tüm istemci sertifikası veriler kaybolur. İstemci sertifikası kimlik doğrulaması gerçekleştirmek için hizmetler için ayarlanmış **ForwardClientCertificate** ApplicationGateway/Http öğesi Parametreler bölümünde ayarlama.

1. Zaman **ForwardClientCertificate** ayarlanır **false**ters proxy istekte için istemci sertifikası, SSL el sıkışması sırasında istemciyle.
Bu varsayılan davranıştır.

2. Zaman **ForwardClientCertificate** ayarlanır **doğru**, ters proxy istekleri istemcinin sertifika istemcisi ile kendi SSL el sıkışması sırasında.
Ardından adlı özel bir HTTP üstbilgisi sertifika verilerinde istemci iletir **X istemci sertifikası**. Üstbilgi değeri istemci sertifikasının base64 ile kodlanmış PEM biçimi dizesidir. Hizmet başarılı/uygun durum kodu istekle sertifika verileri denetledikten sonra başarısız.
İstemci sertifika sunmuyorsa ters proxy boş bir başlık iletir ve durum işleme hizmeti sağlar.

> Ters proxy yalnızca iletici ' dir. İstemci sertifikasının herhangi doğrulaması gerçekleştirmez.


## <a name="next-steps"></a>Sonraki adımlar
* Başvurmak [güvenli hizmetlerine bağlanmak için ters proxy yapılandırma](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/ReverseProxySecureSample#configure-reverse-proxy-to-connect-to-secure-services) için Azure Resource Manager şablonu örnekleri yapılandırmak için farklı hizmet sertifikası ile ters proxy doğrulama seçenekleri güvenli.
* HTTP iletişimi Hizmetleri'nde arasında örneğine bakın bir [örnek proje github'da](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started).
* [Güvenilir hizmetler remoting ile uzak yordam çağrıları](service-fabric-reliable-services-communication-remoting.md)
* [Web API OWIN güvenilir Hizmetleri'nde kullanır](service-fabric-reliable-services-communication-webapi.md)
* [Küme sertifikalarını yönetme](service-fabric-cluster-security-update-certs-azure.md)
