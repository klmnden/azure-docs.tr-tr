---
title: Azure Key Vault azaltma kılavuzu
description: Key Vault azaltma, kaynakların aşırı kullanımını önlemek için eş zamanlı çağrılar sayısını sınırlar.
services: key-vault
author: msmbaldwin
manager: barbkess
tags: ''
ms.service: key-vault
ms.topic: conceptual
ms.date: 05/10/2018
ms.author: mbaldwin
ms.openlocfilehash: 06b96ac6ea0202b74638b9331d754c0e0cc28ad0
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64695239"
---
# <a name="azure-key-vault-throttling-guidance"></a>Azure Key Vault azaltma kılavuzu

Azaltma, kaynakların aşırı kullanımını önlemek için bir Azure hizmetine eş zamanlı çağrı sayısını sınırlayan başlatma bir işlemdir. Azure Key Vault (AKV) büyük hacimde istekleri işlemek için tasarlanmıştır. Büyük bir istek sayısı ortaya çıkarsa, istemcinin istekleri azaltma en iyi performans ve güvenilirlik AKV hizmetinin korumasına yardımcı olur.

Azaltma sınırları senaryoya bağlı olarak değişiklik gösterir. Örneğin, büyük hacimli yazma gerçekleştiriyorsanız için azaltma olanağı, yalnızca okuma işlemi yapıyorsanız daha yüksek olur.

## <a name="how-does-key-vault-handle-its-limits"></a>Key Vault, kendi sınırlarına nasıl işliyor?

Kaynakları kötüye kullanımı önlemesi ve Key Vault'un istemciler için hizmet kalitesi emin olmak için Key Vault hizmet sınırları vardır. Bir hizmeti eşiği aşıldığında, Key Vault istemciden gelen ek istekleri bir süre için sınırlar. Bu durumda, HTTP durum kodu 429 Key Vault döndürür (çok fazla istek), ve istekleri başarısız olur. Ayrıca, Key Vault tarafından izlenen azaltma sınırları doğrultusunda 429 bir sayı döndürür isteği başarısız oldu. 

İşle ilgili geçerli durum azaltma sınırları için varsa, lütfen bizimle iletişime geçin.


## <a name="how-to-throttle-your-app-in-response-to-service-limits"></a>Nasıl kısıtlanacağını uygulamanızın yanıt olarak hizmet sınırları

Aşağıdaki **en iyi uygulamalar** hizmetinizi kısıtlanan olduğunda uygulamanız gerekir:
- İstek başına işlemlerin sayısını azaltın.
- İstekleri sıklığını azaltın.
- Hemen yeniden deneme kaçının. 
    - Tüm istekler, kullanım sınırlarını karşı tahakkuk eder.

Uygulamanızın hata işleme uygularken, istemci tarafı azaltma ihtiyacına algılamak için HTTP hata kodu 429 kullanın. İstek yine bir HTTP 429 hata koduyla başarısız olursa, bir Azure hizmeti sınırını hala karşılaşıyoruz. Önerilen yöntem azaltma, başarılı olana kadar istek yeniden deneniyor taraflı kullanmaya devam edin.

Üstel geri alma uygulayan kodu aşağıda gösterilmiştir. 
```
    public sealed class RetryWithExponentialBackoff
    {
        private readonly int maxRetries, delayMilliseconds, maxDelayMilliseconds;

        public RetryWithExponentialBackoff(int maxRetries = 50,
            int delayMilliseconds = 200,
            int maxDelayMilliseconds = 2000)
        {
            this.maxRetries = maxRetries;
            this.delayMilliseconds = delayMilliseconds;
            this.maxDelayMilliseconds = maxDelayMilliseconds;
        }

        public async Task RunAsync(Func<Task> func)
        {
            ExponentialBackoff backoff = new ExponentialBackoff(this.maxRetries,
                this.delayMilliseconds,
                this.maxDelayMilliseconds);
            retry:
            try
            {
                await func();
            }
            catch (Exception ex) when (ex is TimeoutException ||
                ex is System.Net.Http.HttpRequestException)
            {
                Debug.WriteLine("Exception raised is: " +
                    ex.GetType().ToString() +
                    " –Message: " + ex.Message +
                    " -- Inner Message: " +
                    ex.InnerException.Message);
                await backoff.Delay();
                goto retry;
            }
        }
    }

    public struct ExponentialBackoff
    {
        private readonly int m_maxRetries, m_delayMilliseconds, m_maxDelayMilliseconds;
        private int m_retries, m_pow;

        public ExponentialBackoff(int maxRetries, int delayMilliseconds,
            int maxDelayMilliseconds)
        {
            m_maxRetries = maxRetries;
            m_delayMilliseconds = delayMilliseconds;
            m_maxDelayMilliseconds = maxDelayMilliseconds;
            m_retries = 0;
            m_pow = 1;
        }

        public Task Delay()
        {
            if (m_retries == m_maxRetries)
            {
                throw new TimeoutException("Max retry attempts exceeded.");
            }
            ++m_retries;
            if (m_retries < 31)
            {
                m_pow = m_pow << 1; // m_pow = Pow(2, m_retries - 1)
            }
            int delay = Math.Min(m_delayMilliseconds * (m_pow - 1) / 2,
                m_maxDelayMilliseconds);
            return Task.Delay(delay);
        }
    }
```


Bu kodu kullanarak C istemci olarak\# uygulamasıdır basit. Aşağıdaki örnekte gösterildiği nasıl HttpClient sınıfını kullanma.

```csharp
public async Task<Cart> GetCartItems(int page)
{
    _apiClient = new HttpClient();
    //
    // Using HttpClient with Retry and Exponential Backoff
    //
    var retry = new RetryWithExponentialBackoff();
    await retry.RunAsync(async () =>
    {
        // work with HttpClient call
        dataString = await _apiClient.GetStringAsync(catalogUrl);
    });
    return JsonConvert.DeserializeObject<Cart>(dataString);
}
```

Bu kod yalnızca bir kavram kanıtı uygun olduğunu unutmayın. 

### <a name="recommended-client-side-throttling-method"></a>Önerilen istemci-tarafı azaltma yöntemi

HTTP hata kodu 429 üzerinde bir üstel geri alma yaklaşımı kullanarak istemci azaltma başlayın:

1. 1 saniye, yeniden deneme isteği bekleyin
2. Yine de 2 saniye bekleyin kısıtlanan, isteği yeniden deneyin.
3. Yine de 4 saniye bekleyin kısıtlanan, isteği yeniden deneyin.
4. Yine de 8 saniye bekleyin kısıtlanan, isteği yeniden deneyin.
5. Hala 16 saniye bekleyin kısıtlanan, isteği yeniden deneyin.

Bu noktada, HTTP 429 yanıtı kodları alamıyorsanız.

## <a name="see-also"></a>Ayrıca bkz.

Microsoft Cloud azaltma derin yönlendirmesi için bkz. [kısıtlama düzeni](https://docs.microsoft.com/azure/architecture/patterns/throttling).

