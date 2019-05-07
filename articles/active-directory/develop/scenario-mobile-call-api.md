---
title: Mobil uygulama çağrıları API'ler - bir web API'sini çağırma web | Microsoft kimlik platformu
description: (Bir Web API'sini çağıran) Web API'leri çağıran bir mobil uygulama oluşturmayı öğrenin
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
ms.author: dadobali
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2fd65b9f97c373c55a3486e06e83fca7cf824cad
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65075123"
---
# <a name="mobile-app-that-calls-web-apis---call-a-web-api"></a>Web API - çağıran mobil uygulama web API'si çağırma

Uygulama oturum açmış bir kullanıcı ve alınan belirteçleri sonra MSAL birkaç bölümü ele alınmakta kullanıcı, kendi ortamlarında ve verilen belirteçler hakkında bilgi gösterir. Uygulamanızı bir web API'si çağırma veya bir kullanıcıya Hoş Geldiniz iletisi görüntülemek için bu değerleri kullanabilirsiniz.

İlk olarak biz MSAL sonucu, ardından bir erişim belirteci kullanma inceleyeceksiniz `AuthenticationResult` veya `result` korumalı bir web API'sini çağırmak için.

## <a name="msal-result"></a>MSAL sonucu

- `AccessToken`: Bir taşıyıcı HTTP isteğinde korumalı web API'leri çağırmak için kullanılır.
- `IdToken`: Depolama için oturum açmış olan kullanıcının kendi adı, ana Kiracı ve benzersiz tanımlayıcı gibi hakkında yararlı talepleri içerir.
- `ExpiresOn`: belirtecin süre sonu zamanı. MSAL, uygulamalar için otomatik yenileme işler.
- `TenantId`: Oturum açmak için kullanılan kullanıcı Kiracı tanımlayıcısı. Konuk kullanıcılar (Azure AD B2B), bu Kiracı değil kendi giriş kiracısında oturum, kullanıcı oturumu açık olacaktır.  
- `Scopes`: belirtecinizi ile verilen kapsamlar. Bu, istenen'ın bir alt kümesi olabilir.

MSAL ayrıca için bir Özet sağlar ek olarak, bir `Account`. Hesabınız, hesabında oturum açmış geçerli kullanıcının temsil eder.

- `HomeAccountIdentifier`: Kullanıcının ana Kiracı tanımlayıcısı.
- `UserName`: Kullanıcının tercih edilen kullanıcı adı. Bu, Azure AD B2C kullanıcıları için boş olabilir.
- `AccountIdentifier`: Oturum açmış olan kullanıcı tanımlayıcısı. Bu aynı olacaktır `HomeAccountIdentifier` çoğu durumda başka bir kiracıya bir Konuk kullanıcı değilse.

## <a name="calling-an-api"></a>Bir API çağırma

Erişim belirteci hazır olduktan sonra bir web API'sini çağırmak basit bir işlemdir. Uygulamanız bu belirteci almak, bir HTTP isteği oluşturmak ve çalıştırmak.

### <a name="android"></a>Android

```Java
        RequestQueue queue = Volley.newRequestQueue(this);
        JSONObject parameters = new JSONObject();

        try {
            parameters.put("key", "value");
        } catch (Exception e) {
            // Error when constructing
        }
        JsonObjectRequest request = new JsonObjectRequest(Request.Method.GET, MSGRAPH_URL,
                parameters,new Response.Listener<JSONObject>() {
            @Override
            public void onResponse(JSONObject response) {
                // Successfully called graph, process data and send to UI 
            }
        }, new Response.ErrorListener() {
            @Override
            public void onErrorResponse(VolleyError error) {
                // Error
            }
        }) {
            @Override
            public Map<String, String> getHeaders() throws AuthFailureError {
                Map<String, String> headers = new HashMap<>();
                
                // Put Access Token in HTTP request 
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

        // Put Access token in HTTP Request
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

            // Successfully got data from Graph
            self.updateLogging(text: "Result from Graph: \(result))")
        }.resume()
```

### <a name="xamarin"></a>Xamarin

```CSharp
httpClient = new HttpClient();

// Put Access token in HTTP request 
httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.AccessToken);

// Call Graph
HttpResponseMessage response = await _httpClient.GetAsync(apiUri);
...
}
```

## <a name="making-several-api-requests"></a>Çeşitli API istekler yapma

Aynı API birkaç kez çağırmanız gerekir veya birden çok API, ek hususlar uygulamanızı derlerken varsa:

- ***Artımlı onay***: Uygulama izni gerekli yerine tüm açık olduğundan kullanıcı onayı almak Microsoft kimlik platformu sağlar. Bir API'yi çağırmak uygulamanızı hazır her zaman yalnızca kullanmayı düşünüyor kapsamları istemeniz gerekir.
- ***Koşullu erişim***: Belirli senaryolarda birkaç API istekleri yaparken ek koşullu erişim gereksinimleri alabilirsiniz. Bu senaryonun işlenmesi için sessiz isteklerinden hataları yakalamasına ve etkileşimli bir istekte bulunmak hazırlıklı olun emin olun. Hiçbir koşullu erişim ilkelerinin uygulandığı ilk istek varsa ve uygulamanızı sessizce koşullu erişim gerektiren yeni bir API erişmeyi denediğinde bu durum ortaya çıkabilir. Daha fazla bilgi için bkz. [koşullu erişim için yönergeler](conditional-access-dev-guide.md).

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Üretim aşamasına geçme](scenario-mobile-production.md)
