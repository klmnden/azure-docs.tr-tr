---
title: Anahtar kasası .NET 2.x API Sürüm Notları | Microsoft Docs
description: .NET geliştiricileri için Azure anahtar kasası bu API için kod kullanır
services: key-vault
author: lleonard-msft
manager: mbaldwin
editor: alleonar
ms.assetid: 1cccf21b-5be9-4a49-8145-483b695124ba
ms.service: key-vault
ms.devlang: CSharp
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/02/2017
ms.author: alleonar
ms.openlocfilehash: a7735f8c1c4332bf2472bc83c0c37baf49019004
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2018
ms.locfileid: "27909763"
---
# <a name="azure-key-vault-net-20---release-notes-and-migration-guide"></a>Azure anahtar kasası .NET 2.0 - sürüm notları ve Geçiş Kılavuzu
Aşağıdaki bilgiler, C# ve .NET için Azure anahtar kasası kitaplığı 2.0 sürümüne geçirme yardımcı olur.  Önceki sürümleri için yazılmış uygulamalar en son sürümünü destekleyecek şekilde güncelleştirilmesi gerekir.  Bu değişiklikler tam olarak yeni ve geliştirilmiş özellikler gibi desteklemek için gereken **anahtar kasası sertifikaları**.

## <a name="key-vault-certificates"></a>Anahtar kasası sertifikaları

Anahtar kasası sertifikaları yönetme x509 sertifikaları ve aşağıdaki davranışları destekler:  

* Bir anahtar kasası oluşturma işlemi aracılığıyla sertifikaları oluşturun veya varolan bir sertifikayı alın. Bu otomatik olarak imzalanan hem de içerir ve sertifika yetkilisi (CA) sertifikaları oluşturulur.
* Güvenli şekilde depolamak ve yönetmek x509 özel anahtar malzemesi kullanarak etkileşimi olmadan depolama sertifika.  
* Sertifika yaşam döngüsü yönetmek için anahtar kasası doğrudan ilkeleri tanımlar.  
* Süre sonu uyarılarını ve yenileme bildirimler gibi yaşam döngüsü olayları için kişi bilgilerini sağlayın.  
* Seçili verenler (anahtar kasası iş ortağı X509 sertifika sağlayıcıları ve sertifika yetkililerini) ile sertifikaları otomatik olarak yenileyin. * alternatif (ortak olmayan) destek sertifikadan sağlar ve sertifika yetkilileri (Otomatik yenileme desteklemez).  

## <a name="net-support"></a>.NET desteği

* **.NET 4.0** Azure anahtar kasası .NET kitaplığı 2.0 sürümü tarafından desteklenmiyor
* **.NET framework 4.5.2** Azure anahtar kasası .NET kitaplığı 2.0 sürümü tarafından desteklenmiyor
* **.NET standart 1.4** Azure anahtar kasası .NET kitaplığı 2.0 sürümü tarafından desteklenmiyor

## <a name="namespaces"></a>Ad Alanları

* Ad alanı için **modelleri** değiştirilirse **Microsoft.Azure.KeyVault** için **Microsoft.Azure.KeyVault.Models**.
* **Microsoft.Azure.KeyVault.Internal** ad alanı bırakıldı.
* Aşağıdaki Azure SDK'sı bağımlılıkları ad sahip 

    - **Hyak.Common** artık **Microsoft.Rest**.
    - **Hyak.Common.Internals** artık **Microsoft.Rest.Serialization**.

## <a name="type-changes"></a>Türü değişiklikleri

* *Gizli* değiştirildi *SecretBundle*
* *Sözlük* değiştirildi *IDictionary*
* *Liste<T>, string []* değiştirildi *IList<T>*
* *NextList* değiştirildi *NextPageLink*

## <a name="return-types"></a>Dönüş türleri

* **KeyList** ve **SecretList** şimdi döndürür *IPage<T>*  yerine *ListKeysResponseMessage*
* Oluşturulan **BackupKeyAsync** şimdi döndürür *BackupKeyResult*, içeren *değeri* (Yedekleme blob). Daha önce yöntemi Sarmalanan ve yalnızca değer döndürdü.

## <a name="exceptions"></a>Özel durumlar

* *KeyVaultClientException* değiştirilir *KeyVaultErrorException*
* Hizmet hatası değiştirildi *özel durum. Hata* için *özel durum. Body.Error.Message*.
* Ek bilgi için hata iletisini kaldırıldı **[JsonExtensionData]**.

## <a name="constructors"></a>Oluşturucular

* Kabul yerine bir *HttpClient* oluşturucu bağımsız değişkeni olarak yalnızca Oluşturucusu kabul *HttpClientHandler* veya *DelegatingHandler []*.

## <a name="downloaded-packages"></a>İndirilen paketler

Bir istemci bir anahtar kasası bağımlılık işlediğinde, aşağıdaki paketleri yüklenir:

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
* **Base64UrlConverter** sınıfı yeniden adlandırılmış **Base64UrlJsonConverter**.

## <a name="other-changes"></a>Diğer değişiklikler

* Bu API sürümüne KV işlemi geçici hataları yeniden deneme ilkesi yapılandırması için destek eklendi.

## <a name="microsoftazuremanagementkeyvault-nuget"></a>Microsoft.Azure.Management.KeyVault NuGet

* Döndürülen işlemleri için bir *kasa*, içerdiği bir sınıf dönüş türü olan bir **kasa** özelliği. Dönüş türü sunulmuştur *kasa*.
* *PermissionsToKeys* ve *PermissionsToSecrets* artık *Permissions.Keys* ve *Permissions.Secrets*
* Bazı dönüş türleri değişiklikler denetim-düzeyi için de geçerlidir.

## <a name="microsoftazurekeyvaultextensions-nuget"></a>Microsoft.Azure.KeyVault.Extensions NuGet

* Paket kadar bozuk **Microsoft.Azure.KeyVault.Extensions** ve **Microsoft.Azure.KeyVault.Cryptography** şifreleme işlemleri için.

