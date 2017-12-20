---
title: ".NET ile ContentKeys oluşturma"
description: "Varlıkları için güvenli erişim sağlamaya içerik anahtarları oluşturmayı öğrenin."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 225b05e5-7d30-409c-b5b7-3ef0634310c7
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: juliako
ms.openlocfilehash: 3280a6fcde59bae360da7cb9fea4bb649f984e43
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2017
---
# <a name="create-contentkeys-with-net"></a>.NET ile ContentKeys oluşturma
> [!div class="op_single_selector"]
> * [REST](media-services-rest-create-contentkey.md)
> * [.NET](media-services-dotnet-create-contentkey.md)
> 
> 

Media Services, oluşturma ve şifrelenmiş varlıklar teslim olanak sağlar. A **ContentKey** güvenli erişim sağlayan, **varlık**s. 

Yeni bir varlık oluşturduğunuzda (örneğin, önce [dosyaları karşıya yükleme](media-services-dotnet-upload-files.md)), aşağıdaki şifreleme seçenekleri belirleyebilirsiniz: **StorageEncrypted**, **CommonEncryptionProtected**, veya **EnvelopeEncryptionProtected**. 

İstemcilerinize varlıklar iletirken yapabilecekleriniz [dinamik olarak şifrelenmesini varlıkları için yapılandırma](media-services-dotnet-configure-asset-delivery-policy.md) aşağıdaki iki şifrelemeleri biriyle: **DynamicEnvelopeEncryption** veya **DynamicCommonEncryption**.

Şifrelenmiş varlıklar sahip ilişkilendirilecek **ContentKey**s. Bu makalede, bir içerik anahtarı oluşturmayı açıklar.

> [!NOTE]
> Yeni bir oluştururken **StorageEncrypted** Media Services .NET SDK kullanarak varlık **ContentKey** otomatik olarak oluşturulan ve varlık ile bağlanır.
> 
> 

## <a name="contentkeytype"></a>ContentKeyType
Ne zaman ayarlamalısınız değerlerden biri oluşturma içerik anahtarıdır içerik anahtar türü. Aşağıdaki değerlerden birini seçin. 

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

## <a id="envelope_contentkey"></a>Zarf türü ContentKey oluşturma
Aşağıdaki kod parçacığını bir içerik anahtarı Zarf şifreleme türü oluşturur. Ardından anahtar belirtilen varlık ile ilişkilendirir.

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

Arama

    IContentKey key = CreateEnvelopeTypeContentKey(encryptedsset);



## <a id="common_contentkey"></a>Ortak tür ContentKey oluşturma
Aşağıdaki kod parçacığını bir içerik anahtarı ortak şifreleme türü oluşturur. Ardından anahtar belirtilen varlık ile ilişkilendirir.

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
Arama

    IContentKey key = CreateCommonTypeContentKey(encryptedsset); 


## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

