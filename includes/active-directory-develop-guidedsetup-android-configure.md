
## <a name="register-your-application"></a>Uygulamanızı kaydetme
Uygulamanızın iki yoldan biriyle sonraki iki bölümde açıklandığı gibi kaydedebilirsiniz.

### <a name="option-1-express-mode"></a>Seçenek 1: Hızlı mod
Aşağıdakileri yaparak, uygulamanızın hızlı bir şekilde kaydedebilirsiniz:
1. Git [Microsoft uygulama kayıt portalı](https://apps.dev.microsoft.com/portal/register-app?appType=mobileAndDesktopApp&appTech=android&step=configure).
2.  İçinde **uygulama adı** kutusuna, uygulamanız için bir ad girin.

3. Emin **destekli Kurulum** onay kutusu seçili ve ardından **oluşturma**.

4. Uygulama Kimliği alma yönergeleri izleyin ve kodunuza yapıştırın.

### <a name="option-2-advanced-mode"></a>Seçenek 2: Gelişmiş mod
Uygulamanızı kaydetme ve uygulama kayıt bilgilerinizi çözümünüze eklemek için aşağıdakileri yapın:
1. Uygulamanızı kaydolmadıysanız Git [Microsoft uygulama kayıt portalı](https://apps.dev.microsoft.com/portal/register-app).
2. İçinde **uygulama adı** kutusuna, uygulamanız için bir ad girin. 

3. Emin **destekli Kurulum** onay kutusunun temizlenmiş ve ardından **oluşturma**.

4. Seçin **eklemek Platform**seçin **yerel uygulama**ve ardından **kaydetmek**.

5. Altında **uygulama** > **java** > **{konak}. { ad alanı}**, açık `MainActivity`. 

6.  Değiştir *[uygulama kimliği buraya girin]* yalnızca kayıtlı uygulama kimliği aşağıdaki satırı:

    ```java
    final static String CLIENT_ID = "[Enter the application Id here]";
    ```
<!-- Workaround for Docs conversion bug -->
7. Altında **uygulama** > **bildirimleri**, açık *AndroidManifest.xml* dosya.

8. İçinde `manifest\application` düğümü, aşağıdaki etkinlik ekleyin. Bunu yazmaçlar bulunurken bir `BrowserTabActivity` kimlik doğrulama tamamlandıktan sonra uygulamanızın sürdürmek işletim sistemi sağlayan etkinlik:

    ```xml
    <!--Intent filter to capture System Browser calling back to our app after sign-in-->
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
<!-- Workaround for Docs conversion bug -->
9. İçinde `BrowserTabActivity` düğümü yerine `[Enter the application Id here]` uygulama kimliğiyle
