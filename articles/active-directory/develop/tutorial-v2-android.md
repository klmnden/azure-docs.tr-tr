---
title: Android - Microsoft kimlik platformu ile çalışmaya başlama | Azure
description: Nasıl bir Android uygulamasına erişim belirteci almak ve Microsoft Graph API'si veya Microsoft kimlik platformu erişim belirteçlerini gerektiren API çağrısı.
services: active-directory
documentationcenter: dev-center-name
author: danieldobalian
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/09/2019
ms.author: jmprieur
ms.reviwer: brandwe
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 71c6b0d4cd664b12dbd0fbd4e9423240c8dbebb3
ms.sourcegitcommit: 0ebc62257be0ab52f524235f8d8ef3353fdaf89e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67723803"
---
# <a name="sign-in-users-and-call-the-microsoft-graph-from-an-android-app"></a>Bir Android uygulamasından Microsoft Graph'i çağırmaya ve kullanıcılarının oturumunu

Bu öğreticide, bir Android uygulaması Microsoft kimlik platformu ile tümleştirmeyi öğreneceksiniz. Uygulamanız bir kullanıcının oturumunu, Microsoft Graph API'sini çağırmak için erişim belirteci almak ve Microsoft Graph API için bir istek oluşturun.  

Kılavuzu tamamladıktan sonra uygulamanızın oturum açma işlemleri kişisel Microsoft hesabı (outlook.com, live.com ve diğerleri dahil) kabul eder ve herhangi bir şirket veya Azure Active Directory kullanan kuruluş iş veya Okul hesapları.

## <a name="how-this-tutorial-works"></a>Bu öğreticide nasıl çalışır?

![Bu öğretici tarafından oluşturulan örnek uygulamasını nasıl çalıştığını gösterir](../../../includes/media/active-directory-develop-guidedsetup-android-intro/android-intro.svg)

Bu örnek uygulamasında kullanıcılarının oturumunu ve onların adına veri alın.  Bu veriler, yetkilendirme gerektiren bir korumalı API aracılığıyla (Microsoft Graph API) erişilir.

Daha ayrıntılı belirtmek gerekirse:

* Uygulamanızı bir tarayıcı veya Microsoft Authenticator ve Intune Şirket portalı yoluyla kullanıcının oturum açmasını.
* Son kullanıcının, uygulamanın istediği izinleri kabul eder. 
* Uygulamanızı Microsoft Graph API'si için bir erişim belirteci verilir.
* HTTP isteği Web API'sine erişim belirtecinin dahil edilir.
* Microsoft Graph yanıta işler.

Bu örnek, kimlik doğrulaması uygulamak için Microsoft Authentication kitaplığı için Android (MSAL) kullanır. MSAL otomatik olarak yenileme belirteçleri, çoklu oturum açma (SSO) arasında cihazdaki diğer uygulamalar sunun ve hesapları yönetin.

## <a name="prerequisites"></a>Önkoşullar

* Bu Kılavuzlu Kurulum Android Studio kullanır.
* Android 16 veya üzeri gereklidir (19 + önerilir).

## <a name="library"></a>Kitaplık

Bu kılavuz, aşağıdaki kimlik doğrulama kitaplığı kullanır:

|Kitaplık|Açıklama|
|---|---|
|[com.microsoft.identity.client](https://javadoc.io/doc/com.microsoft.identity.client/msal)|Microsoft Authentication Library (MSAL)|

## <a name="create-a-project"></a>Proje oluşturma

Bu öğreticide, yeni bir proje oluşturur. Tamamlanan öğretici bunun yerine, karşıdan yüklemek isterseniz [kodu indirmek](https://github.com/Azure-Samples/active-directory-android-native-v2/archive/master.zip).

1. Android Studio'yu açın ve seçin **yeni bir Android Studio projesi Başlat**
2. Seçin **temel etkinlik** tıklatıp **sonraki**.
3. Uygulamanızı adlandırın
4. Paket adı kaydedin. Daha sonra Azure portalında girdiğiniz. 
5. Ayarlama **en düşük API düzeyi** için **API 19** veya daha yüksek tıklatıp **son**.
6. Proje Görünümü'nde seçin **proje** açık kaynak ve kaynak olmayan proje dosyalarını görüntülemek için açılan listede **app/build.gradle** ayarlayıp `targetSdkVersion` için `27`.

## <a name="register-your-application"></a>Uygulamanızı kaydetme

1. [Azure portal](https://aka.ms/MobileAppReg)'a gidin
2. Açık [uygulama kayıtları dikey penceresinde](https://ms.portal.azure.com/?feature.broker=true#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredAppsPreview) tıklatıp **+ yeni kaydı**.
3. Girin bir **adı** uygulamanız için ve daha sonra bir yeniden yönlendirme URI'si ayarı olmadan **kaydetme**.
4. İçinde **Yönet** görünürse, bölmenin seçin **kimlik doğrulaması** >  **+ bir platform Ekle** > **Android**.
5. Proje paket adı girin. Kod indirdiyseniz, bu değer, `com.azuresamples.msalandroidapp`.
6. İçinde **imza karma** bölümünü **Android uygulamanızı yapılandırın** sayfasında **bir geliştirme imza karma oluşturma.** ve platformunuz için kullanılacak KeyTool komutunu kopyalayın. Unutmayın, KeyTool.exe Java Development Kit (JDK) bir parçası olarak yüklenir ve ayrıca KeyTool komutu yürütmek için OpenSSL aracı yüklenmiş olması gerekir.
7. Girin **imza karma** KeyTool tarafından oluşturulur.
8. Tıklayın `Configure` kaydedip **MSAL yapılandırma** , görünür **Android yapılandırma** uygulamanızı daha sonra yapılandırdığınızda girebilmeniz için sayfa.  **Bitti**’ye tıklayın.

## <a name="build-your-app"></a>Uygulamanızı oluşturun

### <a name="configure-your-android-app"></a>Android uygulamanızı yapılandırın

1. Android Studio projesi bölmesinde gidin **app\src\main\res**.
2. Sağ **res** ve **yeni** > **dizin**. Girin `raw` tıklayın ve yeni dizin adı olarak **Tamam**.
3. İçinde **uygulama** > **src** > **res** > **ham**, adlı yeni bir JSON dosyası oluşturun`auth_config.json`daha önce kaydettiğiniz MSAL yapılandırmayı yapıştırın. Bkz: [daha fazla bilgi için MSAL yapılandırma](https://github.com/AzureAD/microsoft-authentication-library-for-android/wiki/Configuring-your-app).
   <!-- Workaround for Docs conversion bug -->
4. İçinde **uygulama** > **src** > **ana** > **AndroidManifest.xml**, ekleme`BrowserTabActivity`aşağıdaki etkinlik. Bu giriş, Microsoft'un kimlik doğrulaması tamamlandıktan sonra uygulamanıza geri arama sağlar:

    ```xml
    <!--Intent filter to capture System Browser or Authenticator calling back to our app after sign-in-->
    <activity
        android:name="com.microsoft.identity.client.BrowserTabActivity">
        <intent-filter>
            <action android:name="android.intent.action.VIEW" />
            <category android:name="android.intent.category.DEFAULT" />
            <category android:name="android.intent.category.BROWSABLE" />
            <data android:scheme="msauth"
                android:host="Enter_the_Package_Name"
                android:path="/Enter_the_Signature_Hash" />
        </intent-filter>
    </activity>
    ```

    Azure Portalı'nda kayıtlı paket adı yerine `android:host=` değeri.
    Azure Portalı'nda kayıtlı anahtar karması yerine `android:path=` değeri. İmza karma URL kodlanmış olmamalıdır.

5. İçinde **AndroidManifest.xml**, yukarıdaki `<application>` etiketi, aşağıdaki izinleri ekleyin:

    ```xml
    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
    ```

### <a name="create-the-apps-ui"></a>Uygulamanın kullanıcı Arabirimi oluşturma

1. Android Studio projesi penceresinde gidin **uygulama** > **src** > **ana** > **res**  >  **Düzen** açın **activity_main.xml** açın **metin** görünümü.
2. Etkinlik, örneğin düzenini `<androidx.coordinatorlayout.widget.CoordinatorLayout` için `<androidx.coordinatorlayout.widget.LinearLayout`.
3. Ekleme `android:orientation="vertical"` özelliğini `LinearLayout` düğümü.
4. Aşağıdaki kodu yapıştırın `LinearLayout` düğümünün geçerli içerik değiştirme:

    ```xml
    <TextView
        android:text="Welcome, "
        android:textColor="#3f3f3f"
        android:textSize="50px"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginLeft="10dp"
        android:layout_marginTop="15dp"
        android:id="@+id/welcome"
        android:visibility="invisible"/>

    <Button
        android:id="@+id/callGraph"
        android:text="Call Microsoft Graph"
        android:textColor="#FFFFFF"
        android:background="#00a1f1"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_marginTop="200dp"
        android:textAllCaps="false" />

    <TextView
        android:text="Getting Graph Data..."
        android:textColor="#3f3f3f"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_marginLeft="5dp"
        android:id="@+id/graphData"
        android:visibility="invisible"/>

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="0dip"
        android:layout_weight="1"
        android:gravity="center|bottom"
        android:orientation="vertical" >

        <Button
            android:text="Sign Out"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_marginBottom="15dp"
            android:textColor="#FFFFFF"
            android:background="#00a1f1"
            android:textAllCaps="false"
            android:id="@+id/clearCache"
            android:visibility="invisible" />
    </LinearLayout>
    ```

### <a name="add-msal-to-your-project"></a>MSAL projenize ekleyin.

1. Android Studio projesi penceresinde gidin **uygulama** > **src** > **build.gradle**.
2. Altında **bağımlılıkları**, aşağıdakini yapıştırın:

    ```gradle  
    implementation 'com.android.volley:volley:1.1.1'
    implementation 'com.microsoft.identity.client:msal:0.3.+'
    ```

### <a name="use-msal"></a>MSAL kullanın

İçinde değişiklik artık `MainActivity.java` ekleyip MSAL uygulamanızda kullanma.
Android Studio projesi penceresinde gidin **uygulama** > **src** > **ana** > **java**  >  **com.example.msal**açın `MainActivity.java`

#### <a name="required-imports"></a>Gerekli içeri aktarmaları

Ekranın üst kısmında aşağıdaki içeri aktarmaları ekleyin `MainActivity.java`:

```java
import android.app.Activity;
import android.content.Intent;
import android.util.Log;
import android.view.View;
import android.widget.Button;
import android.widget.TextView;
import android.widget.Toast;
import com.android.volley.*;
import com.android.volley.toolbox.JsonObjectRequest;
import com.android.volley.toolbox.Volley;
import org.json.JSONObject;
import java.util.HashMap;
import java.util.List;
import java.util.Map;
import com.microsoft.identity.client.*;
import com.microsoft.identity.client.exception.*;
```

#### <a name="instantiate-msal"></a>MSAL örneği

İçinde `MainActivity` sınıfı, gereken MSAL örneklemek hangi hakkında birkaç yapılandırmalarını uygulama kapsamlar dahil olmak üzere yapın ve web API'sine erişmek için istiyoruz.

İçine aşağıdaki değişkenleri kopyalayın `MainActivity` sınıfı:

```java
final static String SCOPES [] = {"https://graph.microsoft.com/User.Read"};
final static String MSGRAPH_URL = "https://graph.microsoft.com/v1.0/me";

/* UI & Debugging Variables */
private static final String TAG = MainActivity.class.getSimpleName();
Button callGraphButton;
Button signOutButton;

/* Azure AD Variables */
private PublicClientApplication sampleApp;
private IAuthenticationResult authResult;
```

Öğesinin içeriğini değiştirin `onCreate()` MSAL örneği oluşturmak için aşağıdaki kod ile:

```java
super.onCreate(savedInstanceState);
setContentView(R.layout.activity_main);

callGraphButton = (Button) findViewById(R.id.callGraph);
signOutButton = (Button) findViewById(R.id.clearCache);

callGraphButton.setOnClickListener(new View.OnClickListener() {
    public void onClick(View v) {
        onCallGraphClicked();
    }
});

signOutButton.setOnClickListener(new View.OnClickListener() {
    public void onClick(View v) {
        onSignOutClicked();
    }
});

/* Configure your sample app and save state for this activity */
sampleApp = new PublicClientApplication(
        this.getApplicationContext(),
        R.raw.auth_config);

/* Attempt to get a user and acquireTokenSilent */
sampleApp.getAccounts(new PublicClientApplication.AccountsLoadedCallback() {
    @Override
    public void onAccountsLoaded(final List<IAccount> accounts) {
        if (!accounts.isEmpty()) {
            /* This sample doesn't support multi-account scenarios, use the first account */
            sampleApp.acquireTokenSilentAsync(SCOPES, accounts.get(0), getAuthSilentCallback());
        } else {
            /* No accounts */
        }
    }
});
```

Yukarıdaki kod, uygulamanız aracılığıyla açtıklarında sessizce kullanıcılarının oturumunu dener `getAccounts()` ve başarılı olursa, `acquireTokenSilentAsync()`.  Sonraki birkaç bölümde var. çalışması için oturum açmış olan hesap yok size geri çağırma işleyiciyi uygulayacaksınız.

#### <a name="use-msal-to-get-tokens"></a>MSAL belirteçlerini almak için kullanın

Şimdi biz uygulamanın UI işleme mantığı ve belirteçleri MSAL etkileşimli olarak alma uygulayabilirsiniz.

MSAL, belirteç almaya yönelik başlıca iki yöntem sunar: `acquireTokenSilentAsync()` ve `acquireToken()`.  

`acquireTokenSilentAsync()` bir kullanıcı ve get belirteçleri hesabınız varsa herhangi bir kullanıcı etkileşimi olmadan oturum açtığında. MSAL başarılı olursa, bu başarısız olursa, uygulama belirteçleri üretir iletimli olacak bir `MsalUiRequiredException`.  Bu özel durum oluşturulur veya kullanıcının bir etkileşimli oturum deneyimini (kimlik bilgileri, mfa veya diğer koşullu ilkeleri olabilir veya gerekli olmayabilir erişim) ve ardından kullanmak istediğiniz `acquireToken()`.  

`acquireToken()` kullanıcının oturum açmasını ve belirteçleri almak çalışırken, kullanıcı Arabirimi görüntüler. Ancak, bu tarayıcıda oturum tanımlama bilgileri veya Microsoft Authenticator'ı bir hesabı SSO etkileşimli deneyim sağlamak üzere kullanabilirsiniz.

İçinde aşağıdaki üç kullanıcı Arabirimi yöntemleri oluşturun `MainActivity` sınıfı:

```java
/* Set the UI for successful token acquisition data */
private void updateSuccessUI() {
    callGraphButton.setVisibility(View.INVISIBLE);
    signOutButton.setVisibility(View.VISIBLE);
    findViewById(R.id.welcome).setVisibility(View.VISIBLE);
    ((TextView) findViewById(R.id.welcome)).setText("Welcome, " +
            authResult.getAccount().getUsername());
    findViewById(R.id.graphData).setVisibility(View.VISIBLE);
}

/* Set the UI for signed out account */
private void updateSignedOutUI() {
    callGraphButton.setVisibility(View.VISIBLE);
    signOutButton.setVisibility(View.INVISIBLE);
    findViewById(R.id.welcome).setVisibility(View.INVISIBLE);
    findViewById(R.id.graphData).setVisibility(View.INVISIBLE);
    ((TextView) findViewById(R.id.graphData)).setText("No Data");

    Toast.makeText(getBaseContext(), "Signed Out!", Toast.LENGTH_SHORT)
            .show();
}

/* Use MSAL to acquireToken for the end-user
 * Callback will call Graph api w/ access token & update UI
 */
private void onCallGraphClicked() {
    sampleApp.acquireToken(getActivity(), SCOPES, getAuthInteractiveCallback());
}
```

Geçerli etkinlik alın ve sessiz & etkileşimli geri çağırmaları işlemek için aşağıdaki yöntemleri ekleyin:

```java
public Activity getActivity() {
    return this;
}

/* Callback used in for silent acquireToken calls.
 * Looks if tokens are in the cache (refreshes if necessary and if we don't forceRefresh)
 * else errors that we need to do an interactive request.
 */
private AuthenticationCallback getAuthSilentCallback() {
    return new AuthenticationCallback() {

        @Override
        public void onSuccess(IAuthenticationResult authenticationResult) {
            /* Successfully got a token, call graph now */
            Log.d(TAG, "Successfully authenticated");

            /* Store the authResult */
            authResult = authenticationResult;

            /* call graph */
            callGraphAPI();

            /* update the UI to post call graph state */
            updateSuccessUI();
        }

        @Override
        public void onError(MsalException exception) {
            /* Failed to acquireToken */
            Log.d(TAG, "Authentication failed: " + exception.toString());

            if (exception instanceof MsalClientException) {
                /* Exception inside MSAL, more info inside the exception */
            } else if (exception instanceof MsalServiceException) {
                /* Exception when communicating with the STS, likely config issue */
            } else if (exception instanceof MsalUiRequiredException) {
                /* Tokens expired or no session, retry with interactive */
            }
        }

        @Override
        public void onCancel() {
            /* User cancelled the authentication */
            Log.d(TAG, "User cancelled login.");
        }
    };
}

/* Callback used for interactive request.  If succeeds we use the access
 * token to call the Microsoft Graph. Does not check cache
 */
private AuthenticationCallback getAuthInteractiveCallback() {
    return new AuthenticationCallback() {

        @Override
        public void onSuccess(IAuthenticationResult authenticationResult) {
            /* Successfully got a token, call graph now */
            Log.d(TAG, "Successfully authenticated");
            Log.d(TAG, "ID Token: " + authenticationResult.getIdToken());

            /* Store the auth result */
            authResult = authenticationResult;

            /* call graph */
            callGraphAPI();

            /* update the UI to post call graph state */
            updateSuccessUI();
        }

        @Override
        public void onError(MsalException exception) {
            /* Failed to acquireToken */
            Log.d(TAG, "Authentication failed: " + exception.toString());

            if (exception instanceof MsalClientException) {
                /* Exception inside MSAL, more info inside the exception */
            } else if (exception instanceof MsalServiceException) {
                /* Exception when communicating with the STS, likely config issue */
            }
        }

        @Override
        public void onCancel() {
            /* User cancelled the authentication */
            Log.d(TAG, "User cancelled login.");
        }
    };
}
```

#### <a name="use-msal-for-sign-out"></a>Kullanmak için MSAL oturum kapatma

Ardından, için destek ekleme oturum kapatma.

> [!Important]
> MSAL ile oturumunuzu bir kullanıcı ile ilgili tüm bilinen bilgileri uygulamadan kaldırır, ancak kullanıcı kendi cihazında hala etkin bir oturuma sahip olur. Kullanıcının oturum açmak çalışırsa yeniden, oturum açma kullanıcı Arabirimi görebilirsiniz, ancak cihaz oturumu hala etkin olduğu için kimlik bilgilerini yeniden girmeniz gerekmez.

Oturum kapatma özelliği eklemek için içine aşağıdaki yöntemi ekleyin. `MainActivity` sınıfı. Bu yöntem, tüm hesapları arasında geçiş yapar ve bunları kaldırır:

```java
/* Clears an account's tokens from the cache.
 * Logically similar to "sign out" but only signs out of this app.
 * User will get interactive SSO if trying to sign back-in.
 */
private void onSignOutClicked() {
    /* Attempt to get a user and acquireTokenSilent
     * If this fails we do an interactive request
     */
    sampleApp.getAccounts(new PublicClientApplication.AccountsLoadedCallback() {
        @Override
        public void onAccountsLoaded(final List<IAccount> accounts) {

            if (accounts.isEmpty()) {
                /* No accounts to remove */

            } else {
                for (final IAccount account : accounts) {
                    sampleApp.removeAccount(
                            account,
                            new PublicClientApplication.AccountsRemovedCallback() {
                        @Override
                        public void onAccountsRemoved(Boolean isSuccess) {
                            if (isSuccess) {
                                /* successfully removed account */
                            } else {
                                /* failed to remove account */
                            }
                        }
                    });
                }
            }

            updateSignedOutUI();
        }
    });
}
```

#### <a name="call-the-microsoft-graph-api"></a>Microsoft Graph API çağırma

Biz bir belirteci aldıktan sonra bir istek yapabiliyoruz [Microsoft Graph API](https://graph.microsoft.com) erişim belirteci içinde olacak `AuthenticationResult` kimlik doğrulama geri çağırma 's içinde `onSuccess()` yöntemi. Uygulamanızı, yetkili bir isteği oluşturmak için HTTP üst bilgisi için erişim belirteci eklemeniz gerekir:

| Üstbilgi anahtarı    | value                 |
| ------------- | --------------------- |
| Authorization | Taşıyıcı \<erişim belirteci > |

Aşağıdaki iki yöntemi içinde ekleme `MainActivity` çağrı grafı ve kullanıcı arabirimini güncelleştirmek için sınıfı:

```java
/* Use Volley to make an HTTP request to the /me endpoint from MS Graph using an access token */
private void callGraphAPI() {
    Log.d(TAG, "Starting volley request to graph");

    /* Make sure we have a token to send to graph */
    if (authResult.getAccessToken() == null) {return;}

    RequestQueue queue = Volley.newRequestQueue(this);
    JSONObject parameters = new JSONObject();

    try {
        parameters.put("key", "value");
    } catch (Exception e) {
        Log.d(TAG, "Failed to put parameters: " + e.toString());
    }
    JsonObjectRequest request = new JsonObjectRequest(Request.Method.GET, MSGRAPH_URL,
            parameters,new Response.Listener<JSONObject>() {
        @Override
        public void onResponse(JSONObject response) {
            /* Successfully called graph, process data and send to UI */
            Log.d(TAG, "Response: " + response.toString());

            updateGraphUI(response);
        }
    }, new Response.ErrorListener() {
        @Override
        public void onErrorResponse(VolleyError error) {
            Log.d(TAG, "Error: " + error.toString());
        }
    }) {
        @Override
        public Map<String, String> getHeaders() {
            Map<String, String> headers = new HashMap<>();
            headers.put("Authorization", "Bearer " + authResult.getAccessToken());
            return headers;
        }
    };

    Log.d(TAG, "Adding HTTP GET to Queue, Request: " + request.toString());

    request.setRetryPolicy(new DefaultRetryPolicy(
            3000,
            DefaultRetryPolicy.DEFAULT_MAX_RETRIES,
            DefaultRetryPolicy.DEFAULT_BACKOFF_MULT));
    queue.add(request);
}

/* Sets the graph response */
private void updateGraphUI(JSONObject graphResponse) {
    TextView graphText = findViewById(R.id.graphData);
    graphText.setText(graphResponse.toString());
}
```

#### <a name="multi-account-applications"></a>Birden çok hesap uygulamalar

Bu uygulama, tek bir hesap senaryo için oluşturulmuştur. MSAL birden çok hesap senaryoları da destekler, ancak bazı ek işleri uygulamalardan gerektirir. Belirteçleri gerektiren her eylem için kullanmak istediğiniz hesabı seçin kullanıcının yardımcı olmak için kullanıcı Arabirimi oluşturmanız gerekir. Alternatif olarak, uygulamanızı aracılığıyla kullanacağınız hesabı seçmek için bir buluşsal yöntem uygulayabilirsiniz `getAccounts()` yöntemi.

## <a name="test-your-app"></a>Uygulamanızı test etme

### <a name="run-locally"></a>Yerel olarak çalıştırma

Oluşturun ve bir test cihaza veya öykünücüye uygulamayı dağıtın. Oturum açmak ve belirteçleri almak için Azure AD veya kişisel olmalıdır Microsoft hesapları.

Oturum açtıktan sonra uygulamayı Microsoft Graph'teki döndürülen veriler görüntüler `/me` uç noktası.

### <a name="consent"></a>Onayı

Herhangi bir kullanıcı oturum açtığında, uygulamanıza ilk kez bunlar Microsoft kimlik tarafından istenen izinleri kabul istenir.  Çoğu kullanıcı tümleştirip özelliğine sahip olsa da, bazı Azure AD kiracılarıyla yöneticilerin tüm kullanıcılar adına onay vermesini gerektiren kullanıcı onayı devre dışı bırakıldı. Bu senaryoyu desteklemek için Azure Portal'da uygulamanızın kapsamları kaydedin.

## <a name="get-help"></a>Yardım alın

Ziyaret [Yardım ve Destek](https://docs.microsoft.com/azure/active-directory/develop/developer-support-help-options) Bu öğretici veya Microsoft kimlik platformu ile bir sorun varsa.
