
## <a name="add-the-applications-registration-information-to-your-app"></a>Uygulamanın kayıt bilgileri ekleme

Bu adımda, istemci kimliği projenize eklemeniz gerekir.

1.  Açık `MainActivity` (altında `app`  >  `java`  >   *`{host}.{namespace}`* )
2.  İle başlayan satırı değiştirin `final static String CLIENT_ID` ile:
```java
final static String CLIENT_ID = "[Enter the application Id here]";
```
3. Aç: `app` > `manifests` > `AndroidManifest.xml`
4. Aşağıdaki etkinlik eklemek `manifest\application` düğümü. Bu kayıt bir `BrowserTabActivity` uygulamanızın kimlik doğrulamasını tamamladıktan sonra devam etmek işletim sistemi izin vermek için:

```xml
<!--Intent filter to capture System Browser calling back to our app after Sign In-->
<activity
    android:name="com.microsoft.identity.client.BrowserTabActivity">
    <intent-filter>
        <action android:name="android.intent.action.VIEW" />
        <category android:name="android.intent.category.DEFAULT" />
        <category android:name="android.intent.category.BROWSABLE" />

        <!--Add in your scheme/host from registered redirect URI-->
        <!--By default, the scheme should be similar to 'msal[appId]' -->
        <data android:scheme="msal[Enter the application Id here]"
            android:host="auth" />
    </intent-filter>
</activity>
```

