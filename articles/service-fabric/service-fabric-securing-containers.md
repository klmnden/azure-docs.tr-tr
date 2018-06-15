---
title: Azure Service Fabric üzerinde çalışan bir kapsayıcı halinde sertifikaları içeri aktarma | Microsoft Docs
description: Şimdi bir Service Fabric kapsayıcı hizmetinde sertifika dosyalarını içeri aktarmak öğrenin.
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: ''
ms.assetid: ab49c4b9-74a8-4907-b75b-8d2ee84c6d90
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 2/23/2018
ms.author: subramar
ms.openlocfilehash: f234a6f6ca56d1833aac53f490feb5f667a6bf1b
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
ms.locfileid: "34208225"
---
# <a name="import-a-certificate-file-into-a-container-running-on-service-fabric"></a>Service Fabric üzerinde çalışan bir kapsayıcı bir sertifika dosyası alın

Bir sertifika belirterek, kapsayıcı hizmetlerini güvenliğini sağlayabilirsiniz. Service Fabric kümesindeki düğümlere bir Windows veya Linux (sürüm 5.7 veya üstü) yüklü bir sertifika erişmek için bir kapsayıcı içindeki hizmetler için bir mekanizma sağlar. Sertifika LocalMachine kümesinin tüm düğümlerine yüklenmelidir. Sertifika bilgilerini altında uygulama bildiriminde sağlanan `ContainerHostPolicies` aşağıdaki kod parçacığında gösterildiği gibi etiketi:

```xml
  <ContainerHostPolicies CodePackageRef="NodeContainerService.Code">
    <CertificateRef Name="MyCert1" X509StoreName="My" X509FindValue="[Thumbprint1]"/>
    <CertificateRef Name="MyCert2" X509FindValue="[Thumbprint2]"/>
 ```

Uygulama başlatılırken Windows kümeleri, çalışma zamanı sertifikaları okur ve bir PFX dosyası ve her sertifika için parola oluşturur. Bu PFX dosyası ve parola, aşağıdaki ortam değişkenlerini kullanma kapsayıcısı içindeki erişilebilir: 

* Certificates_ServicePackageName_CodePackageName_CertName_PFX
* Certificates_ServicePackageName_CodePackageName_CertName_Password

Linux kümeleri için sertifikalar (PEM) üzerinden kapsayıcı X509StoreName tarafından belirtilen deposundan kopyalanır. Linux üzerinde karşılık gelen ortam değişkenleri şunlardır:

* Certificates_ServicePackageName_CodePackageName_CertName_PEM
* Certificates_ServicePackageName_CodePackageName_CertName_PrivateKey

Alternatif olarak, zaten gerekli biçiminde sertifikalara sahip ve kapsayıcısı içindeki erişmek istediğiniz uygulama paketinizi bir veri paketi oluşturabilir ve aşağıdakileri belirtin, uygulama bildiriminizi içine:

```xml
<ContainerHostPolicies CodePackageRef="NodeContainerService.Code">
  <CertificateRef Name="MyCert1" DataPackageRef="[DataPackageName]" DataPackageVersion="[Version]" RelativePath="[Relative Path to certificate inside DataPackage]" Password="[password]" IsPasswordEncrypted="[true/false]"/>
 ```

Kapsayıcı hizmeti veya işlem kapsayıcıya sertifika dosyalarını içeri aktarmak için sorumludur. Sertifikayı içeri aktarmak için kullanabilirsiniz `setupentrypoint.sh` komut dosyaları veya kapsayıcı işlemi içinde özel kod yürütün. İşte örnek kod C# ' ta PFX dosyasını içeri aktarmak için:

```csharp
string certificateFilePath = Environment.GetEnvironmentVariable("Certificates_MyServicePackage_NodeContainerService.Code_MyCert1_PFX");
string passwordFilePath = Environment.GetEnvironmentVariable("Certificates_MyServicePackage_NodeContainerService.Code_MyCert1_Password");
X509Store store = new X509Store(StoreName.My, StoreLocation.CurrentUser);
string password = File.ReadAllLines(passwordFilePath, Encoding.Default)[0];
password = password.Replace("\0", string.Empty);
X509Certificate2 cert = new X509Certificate2(certificateFilePath, password, X509KeyStorageFlags.MachineKeySet | X509KeyStorageFlags.PersistKeySet);
store.Open(OpenFlags.ReadWrite);
store.Add(cert);
store.Close();
```
Bu PFX sertifika kimlik doğrulaması, uygulama veya hizmet ya da diğer hizmetleri ile güvenli iletişim için kullanılabilir. Varsayılan olarak, yalnızca sisteme ACLed dosyalarıdır. ACL yapabilecekleriniz diğer hesapları hizmeti tarafından gerektiği şekilde.

Sonraki adım olarak, aşağıdaki makaleler okuyun:

* [Windows Server 2016 Service Fabric Windows kapsayıcı dağıtma](service-fabric-get-started-containers.md)
* [Service Fabric Linux'ta Docker kapsayıcısı dağıtma](service-fabric-get-started-containers-linux.md)
