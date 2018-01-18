
## <a name="use-msal-to-get-a-token-for-the-microsoft-graph-api"></a>Microsoft Graph API için bir belirteç almak üzere MSAL kullanın

1.  Altında **uygulama** > **java** > **{etki alanı}. { Appname}**, açık `MainActivity`. 
2.  Aşağıdaki içeri aktarmaları ekleyin:

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
    ```

3. Değiştir `MainActivity` aşağıdaki kod ile sınıfı:

    ```java
    public class MainActivity extends AppCompatActivity {

        final static String CLIENT_ID = "[Enter the application Id here]";
        final static String SCOPES [] = {"https://graph.microsoft.com/User.Read"};
        final static String MSGRAPH_URL = "https://graph.microsoft.com/v1.0/me";

        /* UI & Debugging Variables */
        private static final String TAG = MainActivity.class.getSimpleName();
        Button callGraphButton;
        Button signOutButton;

        /* Azure AD Variables */
        private PublicClientApplication sampleApp;
        private AuthenticationResult authResult;

        @Override
        protected void onCreate(Bundle savedInstanceState) {
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
            sampleApp = null;
            if (sampleApp == null) {
                sampleApp = new PublicClientApplication(
                        this.getApplicationContext(),
                        CLIENT_ID);
            }

    /* Attempt to get a user and acquireTokenSilent
    * If this fails we do an interactive request
    */
            List<User> users = null;

            try {
                users = sampleApp.getUsers();

                if (users != null && users.size() == 1) {
            /* We have 1 user */

                    sampleApp.acquireTokenSilentAsync(SCOPES, users.get(0), getAuthSilentCallback());
                } else {
            /* We have no user */

            /* Let's do an interactive request */
                    sampleApp.acquireToken(this, SCOPES, getAuthInteractiveCallback());
                }
            } catch (MsalClientException e) {
                Log.d(TAG, "MSAL Exception Generated while getting users: " + e.toString());

            } catch (IndexOutOfBoundsException e) {
                Log.d(TAG, "User at this position does not exist: " + e.toString());
            }

        }

    //
    // App callbacks for MSAL
    // ======================
    // getActivity() - returns activity so we can acquireToken within a callback
    // getAuthSilentCallback() - callback defined to handle acquireTokenSilent() case
    // getAuthInteractiveCallback() - callback defined to handle acquireToken() case
    //

        public Activity getActivity() {
            return this;
        }

        /* Callback method for acquireTokenSilent calls 
        * Looks if tokens are in the cache (refreshes if necessary and if we don't forceRefresh)
        * else errors that we need to do an interactive request.
        */
        private AuthenticationCallback getAuthSilentCallback() {
            return new AuthenticationCallback() {
                @Override
                public void onSuccess(AuthenticationResult authenticationResult) {
                /* Successfully got a token, call Graph now */
                    Log.d(TAG, "Successfully authenticated");

                /* Store the authResult */
                    authResult = authenticationResult;

                /* call graph */
                    callGraphAPI();

                /* update the UI to post call Graph state */
                    updateSuccessUI();
                }

                @Override
                public void onError(MsalException exception) {
                /* Failed to acquireToken */
                    Log.d(TAG, "Authentication failed: " + exception.toString());

                    if (exception instanceof MsalClientException) {
                    /* Exception inside MSAL, more info inside MsalError.java */
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
                public void onSuccess(AuthenticationResult authenticationResult) {
                /* Successfully got a token, call graph now */
                    Log.d(TAG, "Successfully authenticated");
                    Log.d(TAG, "ID Token: " + authenticationResult.getIdToken());

                /* Store the auth result */
                    authResult = authenticationResult;

                /* call Graph */
                    callGraphAPI();

                /* update the UI to post call Graph state */
                    updateSuccessUI();
                }

                @Override
                public void onError(MsalException exception) {
                /* Failed to acquireToken */
                    Log.d(TAG, "Authentication failed: " + exception.toString());

                    if (exception instanceof MsalClientException) {
                    /* Exception inside MSAL, more info inside MsalError.java */
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

        /* Set the UI for successful token acquisition data */
        private void updateSuccessUI() {
            callGraphButton.setVisibility(View.INVISIBLE);
            signOutButton.setVisibility(View.VISIBLE);
            findViewById(R.id.welcome).setVisibility(View.VISIBLE);
            ((TextView) findViewById(R.id.welcome)).setText("Welcome, " +
                    authResult.getUser().getName());
            findViewById(R.id.graphData).setVisibility(View.VISIBLE);
        }

        /* Use MSAL to acquireToken for the end-user
        * Callback will call Graph api w/ access token & update UI
        */
        private void onCallGraphClicked() {
            sampleApp.acquireToken(getActivity(), SCOPES, getAuthInteractiveCallback());
        }

        /* Handles the redirect from the System Browser */
        @Override
        protected void onActivityResult(int requestCode, int resultCode, Intent data) {
            sampleApp.handleInteractiveRequestRedirect(requestCode, resultCode, data);
        }

    }
    ```

<!--start-collapse-->
### <a name="more-information"></a>Daha fazla bilgi
#### <a name="get-a-user-token-interactively"></a>Kullanıcı etkileşimli olarak belirteci alma
Çağırma `AcquireTokenAsync` yöntemi, kullanıcının oturum açmasına izin isteyen bir pencere sonuçlanıyor. Uygulamalar genellikle etkileşimli olarak korunan bir kaynağa erişmek için ihtiyaç duydukları ilk kez oturum açmalarını gerektirir. (Örneğin, bir kullanıcının parolasının süresi sona erdiğinde) bir belirtecini almak üzere sessiz bir işlemi başarısız olduğunda oturum açmak de gerekebilir.

#### <a name="get-a-user-token-silently"></a>Bir kullanıcı sessizce belirteci alma
`AcquireTokenSilentAsync` Yöntemi belirteci satın almalar ve herhangi bir kullanıcı etkileşimi olmadan yenilemeleri işler. Sonra `AcquireTokenAsync` ilk kez yürütüldüğünde `AcquireTokenSilentAsync` veya belirteçleri yenileme isteği için çağrıları sessizce yapmış sonraki çağrılar için korunan kaynaklara erişim belirteçleri almak için kullanılacak normal bir yöntemdir.

Sonuç olarak, `AcquireTokenSilentAsync` yöntemi başarısız olur. Hatanın nedenlerini kullanıcı oturumunuz veya başka bir cihazda kendi parolanın değiştirilmesi emin olabilir. MSAL algıladığında, etkileşimli bir eylem gerektirmeyen tarafından sorunu çözmek için harekete bir `MsalUiRequiredException` özel durum. Uygulamanız bu özel durumun iki yolla işleyebilir:

* Karşı arama yapabilmeniz için `AcquireTokenAsync` hemen. Bu çağrı, oturum açmak için kullanıcıdan içinde sonuçlanır. Bu deseni genelde çevrimiçi uygulamalarda kullanılır kullanıcı için kullanılabilir çevrimdışı içerik olduğu. Bu Destekli kurulum tarafından oluşturulan örnek, örnek yürütme eylemi ilk zamanında görebilirsiniz bu deseni takip eder. 
    * Hiçbir kullanıcı, uygulamayı kullanıldığından `PublicClientApp.Users.FirstOrDefault()` bir null değer içeriyor ve bir `MsalUiRequiredException` özel durumu oluşur. 
    * Ardından örnek kodda çağırarak özel durumu işler `AcquireTokenAsync`, oturum açmak için kullanıcıdan içinde sonuçlanır. 

* Bunun yerine oturum açmak için doğru zamanı seçebilmeniz için bir etkileşimli oturum açma gerekli olan kullanıcılar için görsel bir gösterge sunabilir. Veya uygulamayı yeniden deneyebilirsiniz `AcquireTokenSilentAsync` daha sonra. Çevrimdışı içeriği uygulamada kullanılabilir olduğunda kullanıcıların diğer uygulama işlevselliği kesintiye uğratmadan--Örneğin, ne zaman kullanabileceğini bu deseni sıkça kullanılır. Bu durumda, kullanıcıların korunan bir kaynağa erişmek veya eski bilgileri yenilemek oturum açmak istedikleri zaman karar verebilirsiniz. Yeniden denemek alternatif olarak, uygulama karar `AcquireTokenSilentAsync` zaman ağ geri geçici olarak devre dışı bırakıldı sonra. 
<!--end-collapse-->

## <a name="call-the-microsoft-graph-api-by-using-the-token-you-just-obtained"></a>Yalnızca edinilen belirteçle kullanarak Microsoft Graph API çağırma
İçine aşağıdaki yöntemleri ekleyin `MainActivity` sınıfı:

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
        public Map<String, String> getHeaders() throws AuthFailureError {
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

/* Sets the Graph response */
private void updateGraphUI(JSONObject graphResponse) {
    TextView graphText = (TextView) findViewById(R.id.graphData);
    graphText.setText(graphResponse.toString());
}
```
<!--start-collapse-->
### <a name="more-information-about-making-a-rest-call-against-a-protected-api"></a>Karşı korumalı bir API REST çağrısı yapma hakkında daha fazla bilgi

Bu örnek uygulamasında `callGraphAPI` çağrıları `getAccessToken` ve bir HTTP yapar `GET` bir belirteç gerekiyor ve içerik döndüren bir kaynak isteği. Bu yöntem alınan belirteç HTTP yetkilendirme üst bilgi ekler. Bu örnek için Microsoft Graph API kaynaktır *bana* uç noktası kullanıcı profili bilgilerini görüntüler.
<!--end-collapse-->

## <a name="set-up-sign-out"></a>Oturum kapatma ayarlama

İçine aşağıdaki yöntemleri ekleyin `MainActivity` sınıfı:

```java
/* Clears a user's tokens from the cache.
 * Logically similar to "sign out" but only signs out of this app.
 */
private void onSignOutClicked() {

    /* Attempt to get a user and remove their cookies from cache */
    List<User> users = null;

    try {
        users = sampleApp.getUsers();

        if (users == null) {
            /* We have no users */

        } else if (users.size() == 1) {
            /* We have 1 user */
            /* Remove from token cache */
            sampleApp.remove(users.get(0));
            updateSignedOutUI();

        }
        else {
            /* We have multiple users */
            for (int i = 0; i < users.size(); i++) {
                sampleApp.remove(users.get(i));
            }
        }

        Toast.makeText(getBaseContext(), "Signed Out!", Toast.LENGTH_SHORT)
                .show();

    } catch (MsalClientException e) {
        Log.d(TAG, "MSAL Exception Generated while getting users: " + e.toString());

    } catch (IndexOutOfBoundsException e) {
        Log.d(TAG, "User at this position does not exist: " + e.toString());
    }
}

/* Set the UI for signed-out user */
private void updateSignedOutUI() {
    callGraphButton.setVisibility(View.VISIBLE);
    signOutButton.setVisibility(View.INVISIBLE);
    findViewById(R.id.welcome).setVisibility(View.INVISIBLE);
    findViewById(R.id.graphData).setVisibility(View.INVISIBLE);
    ((TextView) findViewById(R.id.graphData)).setText("No Data");
}
```
<!--start-collapse-->
### <a name="more-information-about-user-sign-out"></a>Kullanıcı oturum kapatma hakkında daha fazla bilgi

`onSignOutClicked` Önceki kod yönteminde kullanıcılar yalnızca etkileşimli olarak yapılırsa, bir belirteç almak için gelecekteki bir istek başarılı olur, böylece geçerli kullanıcının unuttunuz MSAL etkili bir şekilde söyler MSAL kullanıcı önbellekten kaldırır.

Bu örnek uygulamasında tek kullanıcılı desteklese de MSAL nerede birden fazla hesap aynı zamanda oturum açmadan senaryolarını destekler. Örnek bir kullanıcı birden fazla hesap bulunduğu bir e-posta uygulamasıdır.
<!--end-collapse-->
