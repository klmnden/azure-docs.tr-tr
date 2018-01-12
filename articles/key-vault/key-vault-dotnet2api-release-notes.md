---
title: "Anahtar kasası .NET 2.x API Sürüm Notları | Microsoft Docs"
description: ".NET geliştiricileri için Azure anahtar kasası bu API için kod kullanır"
services: key-vault
author: BrucePerlerMS
manager: mbaldwin
editor: bruceper
ms.assetid: 1cccf21b-5be9-4a49-8145-483b695124ba
ms.service: key-vault
ms.devlang: CSharp
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/02/2017
ms.author: bruceper
ms.openlocfilehash: 5b03f5092ee4236ca3e7b12db37dc47bd6d3a309
ms.sourcegitcommit: c4cc4d76932b059f8c2657081577412e8f405478
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/11/2018
---
# <a name="azure-key-vault-net-20---release-notes-and-migration-guide"></a>Azure anahtar kasası .NET 2.0 - sürüm notları ve Geçiş Kılavuzu
Azure anahtar kasası .NET ile birlikte çalışan geliştiricilere aşağıdaki notları ve rehberlik içindir / C# Kitaplığı. 1.0 sürümünden 2.0 sürümüne geçiş içinde güncelleştirme sayısı kodunuzu işlevsel iyileştirmeler yararlanabilir ve ekler gibi özellik için sırayla geçiş işlerinde gerektiren bu yapıldı **anahtar kasası sertifikaları**  destekler.

## <a name="key-vault-certificates"></a>Anahtar kasası sertifikaları

Anahtar kasası sertifikaları destek sağlar, x509 yönetimi için sertifikaları ve aşağıdaki davranışları:  

* Bir anahtar kasası oluşturma işlemi veya varolan bir sertifikayı alma aracılığıyla bir sertifika oluşturmak bir sertifika sahibinin verir. Bu otomatik olarak imzalanan hem de içerir ve sertifika yetkilisi sertifikaları oluşturulur.
* Güvenli Depolama ve X509 Yönetimi uygulamak bir anahtar kasası sertifika sahibinin verir sertifikaları özel anahtar malzemesi etkileşim olmadan.  
* Bir sertifika yaşam döngüsünü yönetmek için anahtar kasası yönlendiren bir ilke oluşturmak bir sertifika sahibinin verir.  
* Kişi hakkında bilgi için bildirim yaşam döngüsü olayları geçerlilik süresi ve sertifikanın yenilenmesini sağlamak sertifika sahipleri sağlar.  
* Otomatik olarak yenilenmesi destekler seçili verenler - anahtar kasası iş ortağı X509 ile sertifika sağlayıcıları / sertifika yetkilileri.
  
  * Not - ortaklık olmayan sağlayıcıları/yetkilileri de izin verilir ancak, otomatik yenileme özelliği desteklemez.

## <a name="net-support"></a>.NET desteği

* **.NET 4.0** Azure anahtar kasası .NET kitaplığı 2.0 sürümü tarafından desteklenmiyor
* **.NET framework 4.5.2** Azure anahtar kasası .NET kitaplığı 2.0 sürümü tarafından desteklenmiyor
* **.NET standart 1.4** Azure anahtar kasası .NET kitaplığı 2.0 sürümü tarafından desteklenmiyor

## <a name="namespaces"></a>Ad Alanları

* Ad alanı için **modelleri** değiştirilirse **Microsoft.Azure.KeyVault** için **Microsoft.Azure.KeyVault.Models**.
* **Microsoft.Azure.KeyVault.Internal** ad alanı bırakıldı.
* Azure SDK'sı bağımlılıkları ad alanı değiştirildi **Hyak.Common** ve **Hyak.Common.Internals** için **Microsoft.Rest** ve  **Microsoft.Rest.Serialization**

## <a name="type-changes"></a>Türü değişiklikleri

* *Gizli* değiştirildi *SecretBundle*
* *Sözlük* değiştirildi *IDictionary*
* *Liste<T>, string []* değiştirildi *IList<T>*
* *NextList* değiştirildi *NextPageLink*

## <a name="return-types"></a>Dönüş türleri

* **KeyList** ve **SecretList** döndürülecek *IPage<T>*  yerine *ListKeysResponseMessage*
* Oluşturulan **BackupKeyAsync** döndürülecek *BackupKeyResult* içeren *değeri* (Yedekleme blob). Yöntem sarmalandı önce ve yalnızca değer döndürüyor.

## <a name="exceptions"></a>Özel durumlar

* *KeyVaultClientException* değiştirilir *KeyVaultErrorException*
* Hizmet hatası değiştirilirse *özel durum. Hata* için *özel durum. Body.Error.Message*.
* Ek bilgi için hata iletisini kaldırıldı **[JsonExtensionData]**.

## <a name="constructors"></a>Oluşturucular

* Kabul yerine bir *HttpClient* oluşturucu bağımsız değişkeni olarak yalnızca Oluşturucusu kabul *HttpClientHandler* veya *DelegatingHandler []*.

## <a name="downloaded-packages"></a>İndirilen paketler

Bir istemci bir anahtar kasası bağımlılığı işlerken aşağıdaki yüklenen

### <a name="previous-package-list"></a>Önceki paket listesi

* id="Hyak.Common paketini" Sürüm "1.0.2" = targetFramework "net45" =
* id="Microsoft.Azure.Common paketini" Sürüm "2.0.4" = targetFramework "net45" =
* id="Microsoft.Azure.common.Dependencies paketini" Sürüm "1.0.0" = targetFramework "net45" =
* id="Microsoft.Azure.KeyVault paketini" Sürüm "1.0.0" = targetFramework "net45" =
* id="Microsoft.BCL paketini" Sürüm "1.1.9" = targetFramework "net45" =
* id="Microsoft.BCL.Async paketini" Sürüm "1.0.168" = targetFramework "net45" =
* id="Microsoft.BCL.Build paketini" Sürüm "1.0.14" = targetFramework "net45" =
* id="Microsoft.NET.http paketini" Sürüm "2.2.22" = targetFramework "net45" =

### <a name="current-package-list"></a>Geçerli paket listesi

* id="Microsoft.Azure.KeyVault paketini" Sürüm "2.0.0-preview" targetFramework = "net45" =
* id="Microsoft.REST.ClientRuntime paketini" Sürüm "2.2.0" = targetFramework "net45" =
* id="Microsoft.REST.ClientRuntime.Azure paketini" Sürüm "3.2.0" = targetFramework "net45" =

## <a name="class-changes"></a>Sınıf değişiklikleri

* **UnixEpoch** sınıfı kaldırıldı
* **Base64UrlConverter** sınıfı yeniden adlandırılmış **Base64UrlJsonConverter**

## <a name="other-changes"></a>Diğer değişiklikler

* Bu API sürümüne KV işlemi geçici hataları yeniden deneme ilkesi yapılandırması için destek eklendi.

## <a name="microsoftazuremanagementkeyvault-nuget"></a>Microsoft.Azure.Management.KeyVault NuGet

* Döndürülen işlemleri için bir *kasa*, dönüş türü bir kasa özelliği bulunan bir sınıf oluştu. Dönüş türü sunulmuştur *kasa*.
* *PermissionsToKeys* ve *PermissionsToSecrets* artık *Permissions.Keys* ve *Permissions.Secrets*
* Dönüş türleri değişikliklerden bazıları denetim-düzeyi için de geçerlidir.

## <a name="microsoftazurekeyvaultextensions-nuget"></a>Microsoft.Azure.KeyVault.Extensions NuGet

* Paket kadar bozuk **Microsoft.Azure.KeyVault.Extensions** ve **Microsoft.Azure.KeyVault.Cryptography** şifreleme işlemleri için.

