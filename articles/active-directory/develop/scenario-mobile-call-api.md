---
title: Mobil uygulama çağrıları API'ler - bir web API'sini çağırma web | Microsoft kimlik platformu
description: Web API (web API'si çağırma) çağıran bir mobil uygulama oluşturmayı öğrenin
services: active-directory
documentationcenter: dev-center-name
author: danieldobalian
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: jmprieur
ms.reviwer: brandwe
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7fc8c21db0f42bbb6804c00e27e82f840d7038c2
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67111184"
---
# <a name="mobile-app-that-calls-web-apis---call-a-web-api"></a>Web API - çağıran mobil uygulama web API'si çağırma

MSAL, uygulamanızın oturum açmış bir kullanıcı ve alınan belirteçleri sonra birkaç bölümü ele alınmakta kullanıcı, kullanıcı ortamının ve verilen belirteçler hakkında bilgi sunar. Uygulamanızı bir web API'si çağırma veya bir karşılama iletisi kullanıcıya göstermek için bu değerleri kullanabilirsiniz.

MSAL sonucunda ilk olarak inceleyeceğiz. Bir erişim belirteci kullanma inceleyeceğiz sonra `AuthenticationResult` veya `result` korumalı bir web API'sini çağırmak için.

## <a name="msal-result"></a>MSAL sonucu
MSAL, aşağıdaki değerleri sağlar: 

- `AccessToken`: Taşıyıcı HTTP isteğinden korumalı web API'leri çağırmak için kullanılır.
- `IdToken`: Oturum açmış kullanıcının kullanıcı adını, giriş kiracısında ve depolama için benzersiz bir tanımlayıcı gibi hakkında yararlı bilgiler içerir.
- `ExpiresOn`: Belirteç sona erme saati. MSAL, uygulamalar için otomatik yenileme işler.
- `TenantId`: Kullanıcı ile oturum Kiracı tanımlayıcısı. Konuk kullanıcılar (Azure Active Directory B2B), bu değeri değil kullanıcının giriş kiracısında oturum, kullanıcı oturumu Kiracı tanımlar.  
- `Scopes`: Belirtecinizi ile verilen kapsamlar. Verilen kapsamlar istediğiniz kapsamları bir alt kümesi olabilir.

MSAL, ayrıca için bir Özet sağlar bir `Account`. Bir `Account` geçerli kullanıcının oturum açma hesabını temsil eder.

- `HomeAccountIdentifier`: Kullanıcının ana Kiracı tanımlayıcısı.
- `UserName`: Kullanıcının tercih edilen kullanıcı adı. Bu, Azure Active Directory B2C kullanıcıları için boş olabilir.
- `AccountIdentifier`: Oturum açmış kullanıcı tanımlayıcısı. Bu değer aynı olacaktır `HomeAccountIdentifier` çoğu durumda, başka bir kiracıya bir Konuk kullanıcı değilse değer.

## <a name="call-an-api"></a>Bir API çağrısı

Erişim belirteci aldıktan sonra bir web API'sini çağırmak kolay bir işlemdir. Uygulamanızı bir HTTP isteği oluşturun ve ardından isteği çalıştırmak için belirteci kullanın.

### <a name="android"></a>Android

```Java
        RequestQueue queue = Volley.newRequestQueue(this);
        JSONObject parameters = new JSONObject();

        try {
            parameters.put("key", "value");
        } catch (Exception e) {
            // Error when constructing.
        }
        JsonObjectRequest request = new JsonObjectRequest(Request.Method.GET, MSGRAPH_URL,
                parameters,new Response.Listener<JSONObject>() {
            @Override
            public void onResponse(JSONObject response) {
                // Successfully called Graph. Process data and send to UI.
            }
        }, new Response.ErrorListener() {
            @Override
            public void onErrorResponse(VolleyError error) {
                // Error.
            }
        }) {
            @Override
            public Map<String, String> getHeaders() throws AuthFailureError {
                Map<String, String> headers = new HashMap<>();
                
                // Put access token in HTTP request.
                headers.put("Authorization", "Bearer " + authResult.getAccessToken());
                return headers;
            }
        };

        request.setRetryPolicy(new DefaultRetryPolicy(
                3000,
                DefaultRetryPolicy.DEFAULT_MAX_RETRIES,
                DefaultRetryPolicy.DEFAULT_BACKOFF_MULT));
        queue.add(request);
```

### <a name="ios"></a>iOS

```swift
        let url = URL(string: kGraphURI)
        var request = URLRequest(url: url!)

        // Put access token in HTTP request.
        request.setValue("Bearer \(self.accessToken)", forHTTPHeaderField: "Authorization")

        URLSession.shared.dataTask(with: request) { data, response, error in
            if let error = error {
                self.updateLogging(text: "Couldn't get graph result: \(error)")
                return
            }
            guard let result = try? JSONSerialization.jsonObject(with: data!, options: []) else {
                self.updateLogging(text: "Couldn't deserialize result JSON")
                return
            }

            // Successfully got data from Graph.
            self.updateLogging(text: "Result from Graph: \(result))")
        }.resume()
```

### <a name="xamarin"></a>Xamarin

```CSharp
httpClient = new HttpClient();

// Put access token in HTTP request.
httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);

// Call Graph.
HttpResponseMessage response = await _httpClient.GetAsync(apiUri);
...
}
```

## <a name="making-several-api-requests"></a>Çeşitli API istekler yapma

Uygulamanız oluştururken gördüğünüz aynı API birkaç kez çağırmanız gerekir ya da birden çok API'leri çağırmak gerekiyorsa, aşağıdaki noktaları dikkate alın:

- **Artımlı onay**: Uygulama izinleri gerekli olduğundan yerine tüm başında kullanıcı onayı almak Microsoft kimlik platformu sağlar. Bir API'yi çağırmak uygulamanızı hazır her zaman yalnızca kullanması gereken kapsam istemeniz gerekir.
- **Koşullu erişim**: Çeşitli API isteği yaptığınız zaman, belirli senaryolarda ek koşullu erişim gereksinimleri alabilirsiniz. Hiçbir koşullu erişim ilkelerinin uygulandığı ilk istek varsa ve uygulamanızı sessizce koşullu erişim gerektiren yeni bir API erişmeyi denediğinde bu durum ortaya çıkabilir. Bu senaryonun işlenmesi için sessiz isteklerinden hataları yakalamasına ve etkileşimli bir istekte bulunmak hazırlıklı olun emin olun.  Daha fazla bilgi için bkz. [koşullu erişim için yönergeler](conditional-access-dev-guide.md).

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Üretim aşamasına geçme](scenario-mobile-production.md)
