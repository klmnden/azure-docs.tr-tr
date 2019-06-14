---
title: Bir şifreleme sertifikası ayarlayın ve Azure Service Fabric Windows Küme parolaları şifrelemek | Microsoft Docs
description: Windows Küme parolaları şifrelemek ve bir şifreleme sertifikasını ayarlama hakkında bilgi edinin.
services: service-fabric
documentationcenter: .net
author: vturecek
manager: chackdan
editor: ''
ms.assetid: 94a67e45-7094-4fbd-9c88-51f4fc3c523a
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/04/2019
ms.author: vturecek
ms.openlocfilehash: 3d324c54d10433520a73f2bd836c26bd79f1b3bb
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60615264"
---
# <a name="set-up-an-encryption-certificate-and-encrypt-secrets-on-windows-clusters"></a>Windows Küme parolaları şifrelemek ve bir şifreleme sertifikasını ayarlama
Bu makalede, bir şifreleme sertifikası ayarlayın ve Windows Küme parolaları şifrelemek için kullanma gösterilmektedir. Linux kümeleri için bkz: [ayarlamak Linux kümelerinde parolaları şifrelemek ve bir şifreleme sertifikası ayarlama.][secret-management-linux-specific-link]

[Azure Key Vault] [ key-vault-get-started] burada Azure Service Fabric kümelerinde yüklü sertifikaları almak için bir yol ve sertifika için bir güvenli depolama konumu olarak kullanılır. Azure'a dağıtmıyor, Service Fabric uygulamaları içindeki gizli verileri yönetmek için Key Vault'u kullanın gerekmez. Ancak, *kullanarak* gizli bir uygulamadaki herhangi bir yerde barındırılan bir kümeye dağıtılması uygulamalara izin vermek üzere platformdan bağımsız bulut. 

## <a name="obtain-a-data-encipherment-certificate"></a>Veri şifreleme sertifikası alın
Veri şifreleme sertifikası kesinlikle şifrelenmesi ve şifresinin çözülmesi için kullanılan [parametreleri] [ parameters-link] bir hizmetin Settings.XML dosyasının içinde ve [ortam değişkenlerini] [ environment-variables-link] bir hizmetin ServiceManifest.xml içinde. Bu şifre metninin imzalama veya kimlik doğrulaması için kullanılabilir değildir. Sertifikanın aşağıdaki gereksinimleri karşılaması gerekir:

* Sertifika özel anahtar içermelidir.
* Sertifikanın bir kişisel bilgi değişimi (.pfx) dosyasına aktarılabilen anahtar değişimi için oluşturulmuş olması gerekir.
* Sertifika anahtar kullanımı, veri şifreleme (10) içermelidir ve sunucu kimlik doğrulaması veya istemci kimlik doğrulaması içermemelidir. 
  
  Örneğin, PowerShell kullanarak otomatik olarak imzalanan sertifika oluşturulurken `KeyUsage` bayrağı ayarlanmalıdır `DataEncipherment`:
  
  ```powershell
  New-SelfSignedCertificate -Type DocumentEncryptionCert -KeyUsage DataEncipherment -Subject mydataenciphermentcert -Provider 'Microsoft Enhanced Cryptographic Provider v1.0'
  ```

## <a name="install-the-certificate-in-your-cluster"></a>Kümenizin sertifikasını yükleme
Bu sertifika, kümedeki her düğümde yüklü olmalıdır. Bkz: [Azure Resource Manager kullanarak küme oluşturma işlemini] [ service-fabric-cluster-creation-via-arm] kurulum yönergeleri. 

## <a name="encrypt-application-secrets"></a>Uygulama parolaları şifrelemek
Aşağıdaki PowerShell komutunu bir şifrelemek için kullanılır. Bu komut, yalnızca değer şifreler; Mevcut **değil** şifre metni oturum açın. Ciphertext gizli değerleri oluşturmak için kümenizdeki yüklü aynı şifreleme sertifikası kullanmanız gerekir:

```powershell
Invoke-ServiceFabricEncryptText -CertStore -CertThumbprint "<thumbprint>" -Text "mysecret" -StoreLocation CurrentUser -StoreName My
```

Sonuçta elde edilen base-64 kodlama dizesi, hem gizli ciphertext hem de bunu şifrelemek için kullanılan sertifikayla ilgili bilgileri içerir.

## <a name="next-steps"></a>Sonraki adımlar
Bilgi edinmek için nasıl [uygulamada şifrelenmiş parolalar belirtin.][secret-management-specify-encrypted-secrets-link]

<!-- Links -->
[key-vault-get-started]:../key-vault/key-vault-overview.md
[service-fabric-cluster-creation-via-arm]: service-fabric-cluster-creation-via-arm.md
[parameters-link]:service-fabric-how-to-parameterize-configuration-files.md
[environment-variables-link]: service-fabric-how-to-specify-environment-variables.md
[secret-management-linux-specific-link]: service-fabric-application-secret-management-linux.md
[secret-management-specify-encrypted-secrets-link]: service-fabric-application-secret-management.md#specify-encrypted-secrets-in-an-application
