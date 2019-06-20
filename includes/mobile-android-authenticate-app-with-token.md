---
author: conceptdev
ms.service: app-service-mobile
ms.topic: include
ms.date: 11/25/2018
ms.author: crdun
ms.openlocfilehash: deb94cab97bd9a402676cdc5c0239da8d07ed8b2
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67189059"
---
Önceki örnekte oturum açma, istemci uygulama her başlatıldığında kimlik sağlayıcısı hem de arka uç Azure hizmeti bağlantı gerektiren standart gösterdi. Bu yöntem verimsizdir ve birçok müşteri, uygulamanız aynı anda başlatmaya çalışırsanız kullanımı ile ilgili sorunlar olabilir. Daha iyi bir yaklaşım, Azure hizmet tarafından döndürülen yetkilendirme belirteci önbelleğe almak ve bir sağlayıcı tabanlı oturum açma kullanmadan önce bu ilk kullanmaya çalıştığınızda oluşturmaktır.

> [!NOTE]
> Arka uç istemcisi tarafından yönetilen ya da hizmetle yönetilen kimlik doğrulama kullanmanıza bakılmaksızın Azure hizmeti tarafından verilen belirteç önbelleğe alabilir. Bu öğretici, hizmet tarafından yönetilen kimlik doğrulaması kullanır.
>
>

1. ToDoActivity.java dosyasını açın ve aşağıdaki import deyimlerini ekleyin:

    ```java
    import android.content.Context;
    import android.content.SharedPreferences;
    import android.content.SharedPreferences.Editor;
    ```

2. Aşağıdaki üye ekleme `ToDoActivity` sınıfı.

    ```java
    public static final String SHAREDPREFFILE = "temp";
    public static final String USERIDPREF = "uid";
    public static final String TOKENPREF = "tkn";
    ```

3. Aşağıdaki tanım ToDoActivity.java dosyasında, ekleme `cacheUserToken` yöntemi.

    ```java
    private void cacheUserToken(MobileServiceUser user)
    {
        SharedPreferences prefs = getSharedPreferences(SHAREDPREFFILE, Context.MODE_PRIVATE);
        Editor editor = prefs.edit();
        editor.putString(USERIDPREF, user.getUserId());
        editor.putString(TOKENPREF, user.getAuthenticationToken());
        editor.commit();
    }
    ```

    Bu yöntem kullanıcı kimliği ve belirteç özel olarak işaretlenmiş bir tercih dosyasında depolar. Cihazdaki diğer uygulamaların belirtecine erişimi yoktur, bu önbelleğe erişimi korur. Uygulama için korumalı tercihi. Birisi cihaza erişim kazanırsa, ancak bunlar arasında başka yollarla belirteç önbelleğe erişim kazanabilmesi mümkündür.

   > [!NOTE]
   > Belirteç şifreleme ile verilerinize erişim belirteci yüksek oranda gizli olarak kabul edilir ve birisi cihaz erişim elde, daha iyi koruyabilirsiniz. Tamamen güvenli bir çözümü bu öğreticinin kapsamı dışındadır ancak ve güvenlik gereksinimlerinize bağlıdır.
   >
   >

4. Aşağıdaki tanım ToDoActivity.java dosyasında, ekleme `loadUserTokenCache` yöntemi.

    ```java
    private boolean loadUserTokenCache(MobileServiceClient client)
    {
        SharedPreferences prefs = getSharedPreferences(SHAREDPREFFILE, Context.MODE_PRIVATE);
        String userId = prefs.getString(USERIDPREF, null);
        if (userId == null)
            return false;
        String token = prefs.getString(TOKENPREF, null);
        if (token == null)
            return false;

        MobileServiceUser user = new MobileServiceUser(userId);
        user.setAuthenticationToken(token);
        client.setCurrentUser(user);

        return true;
    }
    ```

5. İçinde *ToDoActivity.java* dosyası, değiştirin `authenticate` ve `onActivityResult` belirteç önbelleği kullanan aşağıdaki fiyatlarla yöntemleri. Google dışında bir hesap kullanmak istiyorsanız, oturum açma sağlayıcısı değiştirin.

    ```java
    private void authenticate() {
        // We first try to load a token cache if one exists.
        if (loadUserTokenCache(mClient))
        {
            createTable();
        }
        // If we failed to load a token cache, sign in and create a token cache
        else
        {
            // Sign in using the Google provider.
            mClient.login(MobileServiceAuthenticationProvider.Google, "{url_scheme_of_your_app}", GOOGLE_LOGIN_REQUEST_CODE);
        }
    }

    @Override
    protected void onActivityResult(int requestCode, int resultCode, Intent data) {
        // When request completes
        if (resultCode == RESULT_OK) {
            // Check the request code matches the one we send in the sign-in request
            if (requestCode == GOOGLE_LOGIN_REQUEST_CODE) {
                MobileServiceActivityResult result = mClient.onActivityResult(data);
                if (result.isLoggedIn()) {
                    // sign-in succeeded
                    createAndShowDialog(String.format("You are now signed in - %1$2s", mClient.getCurrentUser().getUserId()), "Success");
                    cacheUserToken(mClient.getCurrentUser());
                    createTable();
                } else {
                    // sign-in failed, check the error message
                    String errorMessage = result.getErrorMessage();
                    createAndShowDialog(errorMessage, "Error");
                }
            }
        }
    }
    ```

6. Uygulama ve test kimlik doğrulaması kullanarak geçerli bir hesap oluşturun. Çalıştırın, en az iki kere. İlk çalıştırma sırasında oturum açın ve belirteç önbelleği oluşturmak için bir istem almanız gerekir. Bundan sonra her farklı çalıştır kimlik doğrulaması için belirteç önbelleği yüklemeyi dener. Oturum açmak için gerekli olmamalıdır.
