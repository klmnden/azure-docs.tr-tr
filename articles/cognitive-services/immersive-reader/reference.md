---
title: Derinlikli okuyucu SDK başvurusu
titlesuffix: Azure Cognitive Services
description: Derinlikli okuyucu SDK başvurusu
services: cognitive-services
author: metanMSFT
ms.service: cognitive-services
ms.subservice: immersive-reader
ms.topic: reference
ms.date: 06/20/2019
ms.author: metan
ms.openlocfilehash: c128608b3c4a8e1155c3ac962bcfd07f589fbf23
ms.sourcegitcommit: 5cb0b6645bd5dff9c1a4324793df3fdd776225e4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67311794"
---
# <a name="immersive-reader-sdk-reference"></a>Derinlikli okuyucu SDK başvurusu

Derinlikli okuyucu SDK'sı sürükleyici okuyucu web uygulamanızla tümleştirin olanak tanıyan bir JavaScript kitaplıktır.

## <a name="functions"></a>İşlevler

Tek bir işlev SDK'sı sunan `ImmersiveReader.launchAsync(token, resourceName, content, options)`.

### <a name="launchasync"></a>launchAsync

Derinlikli Reader başlatan bir `iframe` web uygulamanızda.

```typescript
launchAsync(token: string, resourceName: string, content: Content, options?: Options): Promise<HTMLDivElement>;
```

#### <a name="parameters"></a>Parametreler

| Ad | Tür | Açıklama |
| ---- | ---- |------------ |
| `token` | string | Çağrısından alınan erişim belirteci `issueToken` uç noktası. |
| `resourceName` | string | Ayrılmış. Ayarlanmalıdır `null`. |
| `content` | [İçeriği](#content) | Derinlikli okuyucusunda gösterilecek içeriği içeren bir nesne. |
| `options` | [Seçenekler](#options) | Derinlikli okuyucu belirli davranışları yapılandırma seçenekleri. İsteğe bağlı. |

#### <a name="returns"></a>Döndürür

Döndürür bir `Promise<HTMLDivElement>` hangi çözümler sürükleyici okuyucu zaman yüklenir. `Promise` Çözümler bir `div` olan yalnızca bir alt öğesi bir `iframe` sürükleyici okuyucu sayfayı içeren öğe.

#### <a name="exceptions"></a>Özel durumlar

Döndürülen `Promise` ile reddedilir bir [ `Error` ](#error) sürükleyici okuyucu yükleme başarısız olursa nesne. Daha fazla bilgi için [hata kodları](#error-codes).

## <a name="types"></a>Türleri

### <a name="content"></a>İçerik

Derinlikli okuyucusunda gösterilecek içeriği bulunur.

```typescript
{
    title?: string;      // Title text shown at the top of the Immersive Reader (optional)
    chunks: [ {          // Array of chunks
        content: string; // Plain text string
        lang?: string;   // Language of the text, e.g. en, es-ES (optional). Language will be detected automatically if not specified.
        mimeType?: string; // MIME type of the content (optional). Defaults to 'text/plain' if not specified.
    } ];
}
```

#### <a name="supported-mime-types"></a>Desteklenen MIME türleri

| MIME türü | Açıklama |
| --------- | ----------- |
| metin/düz | Düz metin. |
| Uygulama/mathml + xml şeklindedir | Matematik biçimlendirme dili (MathML). [Daha fazla bilgi edinin](https://developer.mozilla.org/en-US/docs/Web/MathML).

### <a name="options"></a>Seçenekler

Derinlikli okuyucu belirli davranışları yapılandırma özelliklerini içerir.

```typescript
{
    uiLang?: string;   // Language of the UI, e.g. en, es-ES (optional). Defaults to browser language if not specified.
    timeout?: number;  // Duration (in milliseconds) before launchAsync fails with a timeout error (default is 15000 ms).
    uiZIndex?: number; // Z-index of the iframe that will be created (default is 1000)
    useWebview?: boolean; // Use a webview tag instead of an iframe, for compatibility with Chrome Apps (default is false).
}
```

### <a name="error"></a>Hata

Hata hakkındaki bilgileri içerir.

```typescript
{
    code: string;    // One of a set of error codes
    message: string; // Human-readable representation of the error
}
```

#### <a name="error-codes"></a>Hata kodları

| Kod | Açıklama |
| ---- | ----------- |
| BadArgument | Sağlanan bağımsız değişkeni geçersiz, bkz: `message` Ayrıntılar için. |
| zaman aşımı | Derinlikli Okuyucu, belirtilen zaman aşımı süresi içinde yüklenemedi. |
| TokenExpired| Sağlanan belirtecin süresi doldu. |

## <a name="launching-the-immersive-reader"></a>Derinlikli okuyucu başlatılıyor

Varsayılan Stil sürükleyici okuyucu başlatmaya düğme için SDK'sı sağlar. Kullanım `immersive-reader-button` bu stil etkinleştirmek için Sınıf özniteliği.

```html
<div class='immersive-reader-button'></div>
```

### <a name="optional-attributes"></a>İsteğe bağlı öznitelikleri

Aşağıdaki öznitelikleri görünümünü düğmenin yapılandırmak için kullanın.

| Öznitelik | Açıklama |
| --------- | ----------- |
| `data-button-style` | Düğmenin stilini ayarlar. Olabilir `icon`, `text`, veya `iconAndText`. Varsayılan olarak `icon`. |
| `data-locale` | Örneğin, yerel ayarlar `en-US`, `fr-FR`. Varsayılan olarak İngilizce. |
| `data-icon-px-size` | Simge boyutunu piksel cinsinden ayarlar. {20px varsayılan olarak. |

## <a name="browser-support"></a>Tarayıcı desteği

Derinlikli okuyucu ile en iyi deneyim için aşağıdaki tarayıcılardan en son sürümlerini kullanın.

* Microsoft Edge
* Internet Explorer 11
* Google Chrome
* Mozilla Firefox
* Apple Safari

## <a name="next-steps"></a>Sonraki adımlar

* Keşfedin [sürükleyici okuyucu GitHub üzerinde SDK](https://github.com/Microsoft/immersive-reader-sdk)
* [Hızlı Başlangıç: Derinlikli okuyucu başlatan bir web uygulaması oluşturma (C#)](./quickstart.md)