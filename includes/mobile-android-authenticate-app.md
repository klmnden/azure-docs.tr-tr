
1. Projeyi Android Studio'da açın.

2. İçinde **Proje Gezgini** Android Studio'da açmak `ToDoActivity.java` dosyasını açıp aşağıdaki içeri aktarma deyimlerini ekleyin:

    ```java
    import java.util.concurrent.ExecutionException;
    import java.util.concurrent.atomic.AtomicBoolean;

    import android.content.Context;
    import android.content.SharedPreferences;
    import android.content.SharedPreferences.Editor;

    import com.microsoft.windowsazure.mobileservices.authentication.MobileServiceAuthenticationProvider;
    import com.microsoft.windowsazure.mobileservices.authentication.MobileServiceUser;
    ```

3. Aşağıdaki yöntemi ekleyin **ToDoActivity** sınıfı:

    ```java
    // You can choose any unique number here to differentiate auth providers from each other. Note this is the same code at login() and onActivityResult().
    public static final int GOOGLE_LOGIN_REQUEST_CODE = 1;

    private void authenticate() {
        // Login using the Google provider.
        mClient.login(MobileServiceAuthenticationProvider.Google, "{url_scheme_of_your_app}", GOOGLE_LOGIN_REQUEST_CODE);
    }

    @Override
    protected void onActivityResult(int requestCode, int resultCode, Intent data) {
        // When request completes
        if (resultCode == RESULT_OK) {
            // Check the request code matches the one we send in the login request
            if (requestCode == GOOGLE_LOGIN_REQUEST_CODE) {
                MobileServiceActivityResult result = mClient.onActivityResult(data);
                if (result.isLoggedIn()) {
                    // login succeeded
                    createAndShowDialog(String.format("You are now logged in - %1$2s", mClient.getCurrentUser().getUserId()), "Success");
                    createTable();
                } else {
                    // login failed, check the error message
                    String errorMessage = result.getErrorMessage();
                    createAndShowDialog(errorMessage, "Error");
                }
            }
        }
    }
    ```

    Bu kod, Google kimlik doğrulama işlemi işlemek için bir yöntem oluşturur. Bir iletişim kutusu, kimliği doğrulanmış kullanıcının Kimliğini görüntüler. Yalnızca başarılı bir kimlik doğrulaması devam edebilirsiniz.

    > [!NOTE]
    > Google dışında bir kimlik sağlayıcısı kullanıyorsanız, geçirilen değerini değiştirmek **oturum açma** yöntemi aşağıdaki değerlerden birine: _MicrosoftAccount_, _Facebook_, _Twitter_, veya _windowsazureactivedirectory_.

4. İçinde **onCreate** yöntemi, sonra başlatır kodu aşağıdaki kod satırını ekleyin `MobileServiceClient` nesnesi.

    ```java
    authenticate();
    ```

    Bu çağrı, kimlik doğrulama işlemi başlatır.

5. Kalan koddan sonra taşıma `authenticate();` içinde **onCreate** yöntemi yeni bir **createTable** yöntemi:

    ```java
    private void createTable() {

        // Get the table instance to use.
        mToDoTable = mClient.getTable(ToDoItem.class);

        mTextNewToDo = (EditText) findViewById(R.id.textNewToDo);

        // Create an adapter to bind the items with the view.
        mAdapter = new ToDoItemAdapter(this, R.layout.row_list_to_do);
        ListView listViewToDo = (ListView) findViewById(R.id.listViewToDo);
        listViewToDo.setAdapter(mAdapter);

        // Load the items from Azure.
        refreshItemsFromTable();
    }
    ```

6. Yeniden yönlendirme works beklendiği gibi sağlamak için aşağıdaki kod parçacığını ekleyin `RedirectUrlActivity` için `AndroidManifest.xml`:

    ```xml
    <activity android:name="com.microsoft.windowsazure.mobileservices.authentication.RedirectUrlActivity">
        <intent-filter>
            <action android:name="android.intent.action.VIEW" />
            <category android:name="android.intent.category.DEFAULT" />
            <category android:name="android.intent.category.BROWSABLE" />
            <data android:scheme="{url_scheme_of_your_app}"
                android:host="easyauth.callback"/>
        </intent-filter>
    </activity>
    ```

7. Ekleme `redirectUriScheme` için `build.gradle` Android uygulamanızın.

    ```gradle
    android {
        buildTypes {
            release {
                // ...
                manifestPlaceholders = ['redirectUriScheme': '{url_scheme_of_your_app}://easyauth.callback']
            }
            debug {
                // ...
                manifestPlaceholders = ['redirectUriScheme': '{url_scheme_of_your_app}://easyauth.callback']
            }
        }
    }
    ```

8. Ekleme `com.android.support:customtabs:23.0.1` bağımlılıklar için `build.gradle`:

    ```gradle
    dependencies {
        // ...
        compile 'com.android.support:customtabs:23.0.1'
    }
    ```

9. Gelen **çalıştırmak** menüsünde tıklatın **uygulama çalıştırma** uygulama ve seçilen kimlik sağlayıcınız ile oturum başlatmak için.

> [!WARNING]
> Belirtilen URL şeması büyük/küçük harf duyarlıdır. Emin tüm oluşumlarını `{url_scheme_of_you_app}` aynı durumda kullanın.

Başarıyla oturum açtığında, uygulama hatasız çalışması gerektiğini ve arka uç hizmetine sorgu ve veri güncelleştirmeleri yapabilirler olmalıdır.
