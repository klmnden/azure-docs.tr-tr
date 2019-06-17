---
title: .NET ile ContentKeys oluşturma
description: Varlıklara güvenli erişim sağlayan bir içerik anahtarı oluşturmayı öğrenin.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.assetid: 225b05e5-7d30-409c-b5b7-3ef0634310c7
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/18/2019
ms.author: juliako
ms.openlocfilehash: ab26be3b9ac5d209cfe8117bdf9e87e0c7e74188
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61465584"
---
# <a name="create-contentkeys-with-net"></a>.NET ile ContentKeys oluşturma 
> [!div class="op_single_selector"]
> * [REST](media-services-rest-create-contentkey.md)
> * [.NET](media-services-dotnet-create-contentkey.md)
> 
> 

Media Services, oluşturmak ve şifrelenmiş varlıklarını teslim etmek sağlar. A **ContentKey** güvenli erişim sağlayan, **varlık**s. 

Yeni bir varlık oluşturduğunuzda (örneğin, önce [dosyaları karşıya yükleme](media-services-dotnet-upload-files.md)), aşağıdaki şifreleme seçenekleri belirleyebilirsiniz: **StorageEncrypted**, **CommonEncryptionProtected**, veya **EnvelopeEncryptionProtected**. 

Varlıklar, istemcilere teslim zaman [varlıklarını dinamik olarak şifrelenmesini yapılandırma](media-services-dotnet-configure-asset-delivery-policy.md) aşağıdaki iki şifrelerinden biri ile: **DynamicEnvelopeEncryption** veya **DynamicCommonEncryption**.

Şifrelenmiş varlıklar sahip ilişkilendirilecek **ContentKey**s. Bu makalede, bir içerik anahtarı oluşturma işlemini açıklar.

> [!NOTE]
> Yeni bir oluştururken **StorageEncrypted** Media Services .NET SDK kullanarak varlık **ContentKey** otomatik olarak oluşturulur ve varlığı ile bağlantılı.
> 
> 

## <a name="contentkeytype"></a>ContentKeyType
İçerik oluşturma zaman ayarlamalısınız değerlerinden anahtardır içerik anahtar türü. Aşağıdaki değerlerden birini seçin. 

```csharp
    public enum ContentKeyType
    {
        /// <summary>
        /// Specifies a content key for common encryption.
        /// </summary>
        /// <remarks>This is the default value.</remarks>
        CommonEncryption = 0,

        /// <summary>
        /// Specifies a content key for storage encryption.
        /// </summary>
        StorageEncryption = 1,

        /// <summary>
        /// Specifies a content key for configuration encryption.
        /// </summary>
        ConfigurationEncryption = 2,

        /// <summary>
        /// Specifies a content key for Envelope encryption.  Only used internally.
        /// </summary>
        EnvelopeEncryption = 4
    }
```

## <a id="envelope_contentkey"></a>Zarf türü ContentKey oluşturma
Aşağıdaki kod parçacığı bir içerik anahtarı Zarf şifreleme türü oluşturur. Ardından anahtar belirli bir varlık ile ilişkilendirir.

```csharp
    static public IContentKey CreateEnvelopeTypeContentKey(IAsset asset)
    {
        // Create envelope encryption content key
        Guid keyId = Guid.NewGuid();
        byte[] contentKey = GetRandomBuffer(16);

        IContentKey key = _context.ContentKeys.Create(
                                keyId,
                                contentKey,
                                "ContentKey",
                                ContentKeyType.EnvelopeEncryption);

        asset.ContentKeys.Add(key);

        return key;
    }

    static private byte[] GetRandomBuffer(int size)
    {
        byte[] randomBytes = new byte[size];
        using (RNGCryptoServiceProvider rng = new RNGCryptoServiceProvider())
        {
            rng.GetBytes(randomBytes);
        }

        return randomBytes;
    }

call

    IContentKey key = CreateEnvelopeTypeContentKey(encryptedsset);
```


## <a id="common_contentkey"></a>Ortak tür ContentKey oluşturma
Aşağıdaki kod parçacığı, ortak şifreleme türünün bir içerik anahtarı oluşturur. Ardından anahtar belirli bir varlık ile ilişkilendirir.

```csharp
    static public IContentKey CreateCommonTypeContentKey(IAsset asset)
    {
        // Create common encryption content key
        Guid keyId = Guid.NewGuid();
        byte[] contentKey = GetRandomBuffer(16);

        IContentKey key = _context.ContentKeys.Create(
                                keyId,
                                contentKey,
                                "ContentKey",
                                ContentKeyType.CommonEncryption);

        // Associate the key with the asset.
        asset.ContentKeys.Add(key);

        return key;
    }

    static private byte[] GetRandomBuffer(int length)
    {
        var returnValue = new byte[length];

        using (var rng =
            new System.Security.Cryptography.RNGCryptoServiceProvider())
        {
            rng.GetBytes(returnValue);
        }

        return returnValue;
    }
call

    IContentKey key = CreateCommonTypeContentKey(encryptedsset); 
```

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

