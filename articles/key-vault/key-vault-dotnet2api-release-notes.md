---
title: Anahtar kasası .NET 2.x API Sürüm Notları | Microsoft Docs
description: .NET geliştiricileri için Azure anahtar kasası kod bu API'ye kullanır
services: key-vault
author: msmbaldwin
manager: barbkess
editor: bryanla
ms.service: key-vault
ms.topic: conceptual
ms.date: 05/02/2017
ms.author: mbaldwin
ms.openlocfilehash: f9dd8a48da08f00cea1219f72940dd84dd3a97ac
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64725515"
---
# <a name="azure-key-vault-net-20---release-notes-and-migration-guide"></a>Azure anahtar kasası .NET 2.0 - sürüm notları ve Geçiş Kılavuzu
Aşağıdaki bilgiler, C# ve .NET için Azure anahtar kasası kitaplığı 2.0 sürümüne geçirme yardımcı olur.  Önceki sürümler için yazılmış uygulamalar en son sürümünü destekleyecek şekilde güncelleştirilmesi gerekir.  Yeni ve geliştirilmiş özellikler gibi tam olarak desteklemek için gereken bu değişiklikler **Key Vault sertifikaları**.

## <a name="key-vault-certificates"></a>Key Vault sertifikaları

Key Vault sertifikaları yönetme x509 sertifikaları ve aşağıdaki davranışları destekler:  

* Bir Key Vault oluşturma işlemi aracılığıyla sertifika oluşturun veya var olan bir sertifikayı içeri aktarın. Sertifika yetkilisi (CA) sertifikaları oluşturulur ve bu otomatik olarak imzalanan hem de içerir.
* Güvenli bir şekilde depolayıp yönetmenize x509 depolama sertifika özel anahtar malzemesi kullanarak etkileşim.  
* Key Vault, sertifika yaşam döngüsü yönetmek için doğrudan ilkeler tanımlayın.  
* Süre sonu uyarılarını ve yenileme bildirimler gibi yaşam döngüsü olayları için kişi bilgilerini sağlayın.  
* Seçili verenler (Key Vault iş ortağı X509 sertifika sağlayıcıları ve sertifika yetkililerini) ile sertifikaları otomatik olarak yenileyin. * alternatif (iş ortağı olmayan) sertifikadan desteği sağlar ve sertifika yetkilileri (Otomatik yenilemeyi desteklemez).  

## <a name="net-support"></a>.NET desteği

* **.NET 4.0** Azure anahtar kasası .NET kitaplığı 2.0 sürümü tarafından desteklenmiyor
* **.NET framework 4.5.2** Azure anahtar kasası .NET kitaplığı 2.0 sürümü tarafından desteklenmiyor
* **.NET standard 1.4** Azure anahtar kasası .NET kitaplığı 2.0 sürümü tarafından desteklenmiyor

## <a name="namespaces"></a>Ad Alanları

* Ad alanı için **modelleri** değiştirilirse **Microsoft.Azure.KeyVault** için **Microsoft.Azure.KeyVault.Models**.
* **Microsoft.Azure.KeyVault.Internal** ad alanı bırakıldı.
* Aşağıdaki Azure SDK'sı bağımlılıkları ad sahip 

    - **Hyak.Common** artık **Microsoft.Rest**.
    - **Hyak.Common.Internals** artık **Microsoft.Rest.Serialization**.

## <a name="type-changes"></a>Tür değişikliği

* *Gizli dizi* değiştirilecek *SecretBundle*
* *Sözlük* değiştirilecek *IDictionary*
* *Liste<T>, string []* değiştirilecek *IList<T>*
* *NextList* değiştirilecek *NextPageLink*

## <a name="return-types"></a>Dönüş türleri

* **KeyList** ve **SecretList** artık döndürür *IPage<T>*  yerine *ListKeysResponseMessage*
* Oluşturulan **BackupKeyAsync** artık döndürür *BackupKeyResult*, içeren *değer* (Yedekleme blob). Daha önce yöntemin sarmalandı ve döndürülen değer.

## <a name="exceptions"></a>Özel durumlar

* *KeyVaultClientException* değiştirilir *KeyVaultErrorException*
* Hizmet hatası değiştirildi *özel durum. Hata* için *özel durum. Body.Error.Message*.
* Ek bilgi için hata iletisini kaldırılır **[JsonExtensionData]**.

## <a name="constructors"></a>Oluşturucular

* Kabul etmek yerine bir *HttpClient* Oluşturucu yalnızca bir oluşturucu bağımsız değişken olarak kabul *HttpClientHandler* veya *DelegatingHandler []*.

## <a name="downloaded-packages"></a>İndirilen paketler

Bir istemci bir Key Vault bağımlılık işlediğinde, aşağıdaki paketler yüklenir:

### <a name="previous-package-list"></a>Önceki paket listesi

* `package id="Hyak.Common" version="1.0.2" targetFramework="net45"`
* `package id="Microsoft.Azure.Common" version="2.0.4" targetFramework="net45"`
* `package id="Microsoft.Azure.Common.Dependencies" version="1.0.0" targetFramework="net45"`
* `package id="Microsoft.Azure.KeyVault" version="1.0.0" targetFramework="net45"`
* `package id="Microsoft.Bcl" version="1.1.9" targetFramework="net45"`
* `package id="Microsoft.Bcl.Async" version="1.0.168" targetFramework="net45"`
* `package id="Microsoft.Bcl.Build" version="1.0.14" targetFramework="net45"`
* `package id="Microsoft.Net.Http" version="2.2.22" targetFramework="net45"`

### <a name="current-package-list"></a>Geçerli paket listesi

* `package id="Microsoft.Azure.KeyVault" version="2.0.0-preview" targetFramework="net45"`
* `package id="Microsoft.Rest.ClientRuntime" version="2.2.0" targetFramework="net45"`
* `package id="Microsoft.Rest.ClientRuntime.Azure" version="3.2.0" targetFramework="net45"`

## <a name="class-changes"></a>Sınıf değişiklikleri

* **UnixEpoch** sınıfı kaldırıldı.
* **Base64UrlConverter** sınıfı yeniden adlandırıldı **Base64UrlJsonConverter**.

## <a name="other-changes"></a>Diğer değişiklikler

* Bu API sürümüne KV işlemi geçici hataları yeniden deneme ilkesi yapılandırması için destek eklendi.

## <a name="microsoftazuremanagementkeyvault-nuget"></a>Microsoft.Azure.Management.KeyVault NuGet

* Döndürülen işlemleri için bir *kasası*, dönüş türü içeren bir sınıfı olan bir **kasası** özelliği. Dönüş türü artık olduğu *kasası*.
* *PermissionsToKeys* ve *PermissionsToSecrets* artık *Permissions.Keys* ve *Permissions.Secrets*
* Belirli dönüş türleri değişiklikler, Denetim düzlemi için de geçerlidir.

## <a name="microsoftazurekeyvaultextensions-nuget"></a>Microsoft.Azure.KeyVault.Extensions NuGet

* En fazla paket bozuk **Microsoft.Azure.KeyVault.Extensions** ve **Microsoft.Azure.KeyVault.Cryptography** şifreleme işlemleri için.

