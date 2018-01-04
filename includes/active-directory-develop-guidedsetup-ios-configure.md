
## <a name="create-an-application-express"></a>(Hızlı) uygulama oluşturma
Uygulamanızı kaydetmeniz gerekir artık *Microsoft uygulama kayıt portalı*:
1. Uygulamanızı aracılığıyla kaydetme [Microsoft uygulama kayıt portalı](https://apps.dev.microsoft.com/portal/register-app?appType=mobileAndDesktopApp&appTech=ios&step=configure)
2.  Uygulamanız ve e-posta için bir ad girin
3.  Kurulum destekli seçeneğinin işaretli olduğundan emin olun
4.  Uygulama Kimliğini almak ve kodunuza yapıştırmak için yönergeleri izleyin

### <a name="add-your-application-registration-information-to-your-solution-advanced"></a>Uygulama kayıt bilgilerinizi çözümünüze (Gelişmiş) ekleyin

1.  Git [Microsoft uygulama kayıt portalı](https://apps.dev.microsoft.com/portal/register-app)
2.  Uygulamanız ve e-posta için bir ad girin
3.  Destekli kurulumu için seçeneğinin işaretli olduğundan emin olun
4.  Tıklatın `Add Platform`seçeneğini belirleyip `Native Application` ve'ı tıklatın`Save`
5.  Xcode için geri dönün. İçinde `ViewController.swift`, ile başlayan satırı Değiştir '`let kClientID`' yalnızca kayıtlı uygulama kimliği:

```swift
let kClientID = "Your_Application_Id_Here"
```

<!-- Workaround for Docs conversion bug -->
<ol start="6">
<li>
Control + tıklatın <code>Info.plist</code> bağlam menüsünü açmak ve ardından: <code>Open As</code>> <code>Source Code</code>
</li>
<li>
Altında <code>dict</code> kök düğümü, aşağıdakileri ekleyin:
</li>
</ol>

```xml
<key>CFBundleURLTypes</key>
<array>
    <dict>
        <key>CFBundleTypeRole</key>
        <string>Editor</string>
        <key>CFBundleURLName</key>
        <string>$(PRODUCT_BUNDLE_IDENTIFIER)</string>
        <key>CFBundleURLSchemes</key>
        <array>
            <string>msal[Your_Application_Id_Here]</string>
            <string>auth</string>
        </array>
    </dict>
</array>
```
<ol start="8">
<li>
Değiştir <i> <code>[Your_Application_Id_Here]</code> </i> yalnızca kayıtlı uygulama kimliği
</li>
</ol>
