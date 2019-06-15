---
title: Azure Service Fabric Mesh Maven başvurusu | Microsoft Docs
description: Service Fabric Mesh için Maven plugin kullanmak için bir başvuru içeriyor
services: service-fabric-mesh
keywords: maven, java, CLI
author: suhuruli
ms.author: suhuruli
ms.date: 11/26/2018
ms.topic: reference
ms.service: service-fabric-mesh
manager: subramar
ms.openlocfilehash: 08e842f5b91bd0ca5f8e8b2a7866f3f9a689ac28
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60811621"
---
# <a name="maven-plugin-for-service-fabric-mesh"></a>Service Fabric kafes için maven eklentisi

## <a name="prerequisites"></a>Önkoşullar

- Java SDK
- Maven
- Azure CLI ile mesh uzantısı
- Service Fabric CLI

## <a name="goals"></a>Hedefleri

### `azure-sfmesh:init`
- Oluşturur bir `servicefabric` içeren klasörü bir `appresources` olan klasör `application.yaml` dosya. 

### `azure-sfmesh:addservice`
- İçinde bir klasör oluşturur `servicefabric` hizmet adını taşıyan klasörü ve hizmetin YAML dosyası oluşturur. 

### `azure-sfmesh:addnetwork`
- Oluşturur bir `network` YAML içinde sağlanan ağ adıyla `appresources` klasörü 

### `azure-sfmesh:addgateway`
- Oluşturur bir `gateway` sağlanan ağ geçidi adla YAML `appresources` klasörü 

### `azure-sfmesh:addsecret`
- Oluşturur bir `secret` YAML içinde sağlanan gizli dizi adı ile `appresources` klasörü 

### `azure-sfmesh:addsecretvalue`
- Oluşturur bir `secretvalue` YAML içinde belirtilen gizli anahtar ve gizli değer adıyla `appresources` klasörü 

### `azure-sfmesh:deploy`
- Gelen yamls birleştirir `servicefabric` klasörü ve geçerli klasörde bir Azure Resource Manager şablonu JSON'ı oluşturur.
- Tüm kaynaklar için Azure Service Fabric Mesh ortamı dağıtır. 

### `azure-sfmesh:deploytocluster`
- Bir klasör oluşturur (`meshDeploy`) Service Fabric kümeleri için geçerli olan yamls üretilen Json'lerini dağıtım içerir
- Tüm kaynaklar için Service Fabric kümesi dağıtır.
 

## <a name="usage"></a>Kullanım

Maven plugin Maven Java uygulamanızı kullanmak için aşağıdaki kod parçacığı pom.xml dosyanıza ekleyin:

```XML
<project>
  ...
  <build>
    ...
    <plugins>
      ...
      <plugin>
        <groupId>com.microsoft.azure</groupId>
          <artifactId>azure-sfmesh-maven-plugin</artifactId>
          <version>0.1.0</version>
      </plugin>
    </plugins>
  </build>
</project>
```

## <a name="common-configuration"></a>Genel yapılandırma

Maven plugin, Azure için Maven eklentileri yaygın yapılandırmaları şu anda desteklemiyor.

## <a name="how-to"></a>Nasıl Yapılır Konuları

### <a name="initialize-maven-project-for-azure-service-fabric-mesh"></a>Azure Service Fabric Mesh için Maven projesi başlatın.
Uygulama kaynak YAML dosyası oluşturmak için aşağıdaki komutu çalıştırın.

```cmd
mvn azure-sfmesh:init -DapplicationName=helloworldserver
```

- Adlı bir klasör oluşturur `servicefabric->appresources` YAML kök klasörünüzde içeren bir uygulama adı `app_helloworldserver`

### <a name="add-resource-to-your-application"></a>Kaynak uygulamanıza ekleyin

#### <a name="add-a-new-network-to-your-application"></a>Yeni bir ağ uygulamanıza ekleyin
Ağ kaynak yaml oluşturmak için aşağıdaki komutu çalıştırın. 

```cmd
mvn azure-sfmesh:addnetwork -DnetworkName=helloworldservicenetwork -DnetworkAddressPrefix=10.0.0.4/22
```

- Bir ağ YAML klasöründe oluşturur `servicefabric->appresources` adlı `network_helloworldservicenetwork`

#### <a name="add-a-new-service-to-your-application"></a>Yeni bir hizmeti uygulamanıza ekleyin
Bir hizmet yaml oluşturmak için aşağıdaki komutu çalıştırın. 

```cmd
mvn azure-sfmesh:addservice -DapplicationName=helloworldserver -DserviceName=helloworldservice -DimageName=helloworldserver:latest -DlistenerPort=8080 -DnetworkRef=helloworldservicenetwork
```

- Bir hizmet YAML klasöründe oluşturur `servicefabric->helloworldservice` adlı `service_helloworldservice` başvuran `helloworldservicenetwork` & `helloworldserver` uygulama
- Hizmeti 8080 bağlantı noktasında dinleme
- Hizmeti kullanmaya ***helloworldserver:latest*** olarak olduğundan kapsayıcı görüntüsü.

#### <a name="add-a-new-gateway-resource-to-your-application"></a>Yeni bir ağ geçidi kaynağı uygulamanıza ekleyin
Bir ağ geçidi kaynak yaml oluşturmak için aşağıdaki komutu çalıştırın. 

```cmd
mvn azure-sfmesh:addgateway -DapplicationName=helloworldserver -DdestinationNetwork=helloworldservicenetwork -DgatewayName=helloworldgateway -DlistenerName=helloworldserviceListener -DserviceName=helloworldservice -DsourceNetwork=open -DtcpPort=80
```

- Yeni bir ağ geçidi YAML klasöründe oluşturur `servicefabric->appresources` adlı `gateway_helloworldgateway`
- Başvuruları `helloworldservicelistener` çağrılar, bu ağ geçidinden dinlediği service dinleyici olarak. Ayrıca başvuran `helloworldservice` hizmet olarak `helloworldservicenetwork` ağ ve `helloworldserver` uygulama. 
- İstek bağlantı noktası 80 üzerinde dinler

#### <a name="add-a-new-volume-to-your-application"></a>Uygulamanıza yeni birim ekleme
Bir birim kaynak yaml oluşturmak için aşağıdaki komutu çalıştırın. 

```cmd
mvn azure-sfmesh:addvolume -DvolumeAccountKey=key -DvolumeAccountName=name -DvolumeName=vol1 -DvolumeShareName=share
```

- Bir birim YAML klasöründe oluşturur `servicefabric->appresources` adlı `volume_vol1`
- Gerekli Parametreler için özellikleri ayarlar `volumeAccountKey`, ve `volumeShareName` yukarıdaki gibi
- Aşağıdaki birim oluşturduğunuzda bu başvuru konusunda daha fazla bilgi için ziyaret [Azure dosyaları birim kullanarak uygulama dağıtma](service-fabric-mesh-howto-deploy-app-azurefiles-volume.md)

#### <a name="add-a-new-secret-resource-to-your-application"></a>Yeni bir gizli dizi kaynak uygulamanıza ekleyin
Gizli kaynak yaml oluşturmak için aşağıdaki komutu çalıştırın. 

```cmd
mvn azure-sfmesh:addsecret -DsecretName=secret1
```

- Gizli bir YAML klasöründe oluşturur `servicefabric->appresources` adlı `secret_secret1`
- Bu gizli oluşturulan başvuru konusunda daha fazla bilgi için aşağıdaki ziyaret [gizli anahtarları yönetme](service-fabric-mesh-howto-manage-secrets.md)

#### <a name="add-a-new-secretvalue-resource-to-your-application"></a>Yeni bir secretvalue kaynak uygulamanıza ekleyin
Secretvalue kaynak yaml oluşturmak için aşağıdaki komutu çalıştırın. 

```cmd
mvn azure-sfmesh:addsecretvalue -DsecretValue=someVal -DsecretValueName=secret1/v1
```

- Bir YAML secretvalue klasörü oluşturmak `servicefabric->appresources` adlı `secretvalue_secret1_v1`

### <a name="run-the-application-locally"></a>Uygulamayı yerel olarak çalıştırma

Hedef yardımıyla `azure-sfmesh:deploytocluster`, yerel olarak aşağıdaki komutu kullanarak uygulamayı çalıştırabilirsiniz:

```cmd
mvn azure-sfmesh:deploytocluster
```

Varsayılan olarak bu amaç için yerel küme kaynakları dağıtır. Yerel kümeye dağıtıyorsanız, yerel bir Service Fabric kümesinde çalışır olduğunu varsayar. Yerel Service Fabric küme kaynakları için şu anda yalnızca üzerinde desteklenir [Windows](service-fabric-mesh-howto-setup-developer-environment-sdk.md).

- Service Fabric kümeleri için geçerli olan yamls Json'lerini oluşturur
- Sonra küme uç noktasına dağıtır.

### <a name="deploy-application-to-azure-service-fabric-mesh"></a>Azure Service Fabric Mesh uygulamasını dağıtma

Hedef yardımıyla `azure-sfmesh:deploy`, aşağıdaki komutu çalıştırarak Service Fabric Mesh ortamına dağıtabilirsiniz:

```cmd
mvn azure-sfmesh:deploy -DresourceGroup=todoapprg -Dlocation=eastus
```

- Adlı bir kaynak grubu oluşturur `todoapprg` yoksa.
- Bir Azure Resource Manager şablon JSON YAMLs birleştirerek oluşturur. 
- JSON için Azure Service Fabric ağ ortamı dağıtır.