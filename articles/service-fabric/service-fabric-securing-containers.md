---
title: Azure Service Fabric üzerinde çalışan bir kapsayıcıya alın sertifikaları içeri aktarma | Microsoft Docs
description: Artık bir Service Fabric kapsayıcı hizmetinde sertifika dosyalarını içeri öğrenin.
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: chackdan
editor: ''
ms.assetid: ab49c4b9-74a8-4907-b75b-8d2ee84c6d90
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 2/23/2018
ms.author: aljo, subramar
ms.openlocfilehash: ac461c0d512a5a58ce687623a53e99eeeca399e5
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60837700"
---
# <a name="import-a-certificate-file-into-a-container-running-on-service-fabric"></a>Service Fabric üzerinde çalışan bir kapsayıcı içine bir sertifika dosyasını içe

Bir sertifika belirterek, kapsayıcı Hizmetleri güvenli hale getirebilirsiniz. Service Fabric Hizmetleri (sürüm 5.7 veya daha yüksek) bir Windows veya Linux küme düğümlerinde yüklü bir sertifika erişmek için bir kapsayıcı içinde bir mekanizma sağlar. Kümenin tüm düğümlerinde altında LocalMachine sertifika deposunda sertifika yüklü olmalıdır. Sertifikanın özel anahtar karşılık gelen, mevcut, erişilebilir ve - Windows - dışarı aktarılabilir olmalıdır. Sertifika bilgileri altındaki uygulama bildirimini sağlanan `ContainerHostPolicies` etiketi aşağıdaki kod parçacığında gösterildiği gibi:

```xml
  <ContainerHostPolicies CodePackageRef="NodeContainerService.Code">
    <CertificateRef Name="MyCert1" X509StoreName="My" X509FindValue="[Thumbprint1]"/>
    <CertificateRef Name="MyCert2" X509FindValue="[Thumbprint2]"/>
 ```

Windows kümeleri uygulama başlatılırken bir PFX dosyasına rastgele oluşturulmuş bir parolayla korunan her başvurulan sertifikası ve karşılık gelen özel anahtarıyla çalışma zamanı verir. PFX ve parola dosyaları, sırasıyla aşağıdaki ortam değişkenlerini kullanarak kapsayıcı içinde erişilebilir: 

* Certificates_ServicePackageName_CodePackageName_CertName_PFX
* Certificates_ServicePackageName_CodePackageName_CertName_Password

Linux kümeleri için sertifikalar (PEM) üzerinden X509StoreName tarafından belirtilen kapsayıcıyı deposundan kopyalanır. Linux üzerinde karşılık gelen ortam değişkenleri şunlardır:

* Certificates_ServicePackageName_CodePackageName_CertName_PEM
* Certificates_ServicePackageName_CodePackageName_CertName_PrivateKey

Alternatif olarak, zaten sertifikaların gerekli biçiminde olması ve kapsayıcı içinde erişmek istediğiniz veri paketi, uygulama paketi içinde oluşturabilir ve aşağıdakileri belirtin uygulama bildiriminizi içinde:

```xml
<ContainerHostPolicies CodePackageRef="NodeContainerService.Code">
  <CertificateRef Name="MyCert1" DataPackageRef="[DataPackageName]" DataPackageVersion="[Version]" RelativePath="[Relative Path to certificate inside DataPackage]" Password="[password]" IsPasswordEncrypted="[true/false]"/>
 ```

Kapsayıcı hizmeti veya işlem kapsayıcıya sertifika dosyalarını içeri aktarma için sorumludur. Sertifikayı içeri aktarmak için kullanabileceğiniz `setupentrypoint.sh` komut dosyaları veya özel kod kapsayıcısı işlemiyle yürütün. Örnek kodda işte C# PFX dosyasını içeri aktarmak için:

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
Bu PFX sertifika kimlik doğrulaması, uygulama veya hizmet veya diğer hizmetleri ile güvenli iletişim için kullanılabilir. Varsayılan olarak, yalnızca sisteme ACLed dosyalarıdır. ACL için hizmet gerektirdiği diğer hesapları için.

Sonraki adım olarak, bu makaleleri okuyun:

* [Service fabric'e Windows Server 2016 üzerinde bir Windows kapsayıcısı dağıtma](service-fabric-get-started-containers.md)
* [Linux üzerinde Service Fabric için bir Docker kapsayıcısı dağıtma](service-fabric-get-started-containers-linux.md)
