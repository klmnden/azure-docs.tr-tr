---
title: İOS - Microsoft kimlik platformu ile çalışmaya başlama | Azure
description: İOS (Swift) uygulamalar bir API, nasıl arayabilir, erişim belirteçleri kullanarak Microsoft kimlik platformu gerektirir
services: active-directory
documentationcenter: dev-center-name
author: danieldobalian
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/26/2019
ms.author: dadobali
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7e84d97a4ef389981fa30aa08433fef1662367ae
ms.sourcegitcommit: c53a800d6c2e5baad800c1247dce94bdbf2ad324
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2019
ms.locfileid: "64935943"
---
# <a name="sign-in-users-and-call-the-microsoft-graph-from-an-ios-app"></a>Kullanıcılar oturum ve bir iOS uygulamasından Microsoft Graph'i çağırmaya

Bu öğreticide, bir iOS uygulaması oluşturma ve Microsoft kimlik platformuyla tümleştirebilir öğreneceksiniz. Özellikle, bu uygulama bir kullanıcının oturumunu, Microsoft Graph API'sini çağırmak için erişim belirteci almak ve Microsoft Graph API için temel bir istekte.  

Kılavuzu tamamladıktan sonra uygulamanızın oturum açma işlemleri kişisel Microsoft hesabı (outlook.com, live.com ve diğerleri dahil) kabul eder ve herhangi bir şirket veya Azure Active Directory kullanan kuruluş iş veya Okul hesapları.

## <a name="how-this-guide-works"></a>Bu kılavuz nasıl çalışır?

![Bu öğretici tarafından oluşturulan örnek uygulamasını nasıl çalıştığını gösterir](../../../includes/media/active-directory-develop-guidedsetup-ios-introduction/iosintro.svg)

Bu örnek uygulamasında kullanıcılarının oturumunu ve onların adına veri alın.  Bu veriler, yetkilendirme gerektirir ve aynı zamanda Microsoft kimlik platformu tarafından korunan korumalı bir API (Bu durumda Microsoft Graph API) aracılığıyla erişilir.

Daha ayrıntılı belirtmek gerekirse:

* Uygulamanızı Microsoft Authenticator ya da bir tarayıcı aracılığıyla kullanıcının oturum açmasını.
* Son kullanıcının, uygulamanın istediği izinleri kabul eder. 
* Uygulamanızı Microsoft Graph API'si için bir erişim belirteci verilir.
* HTTP isteği Web API'sine erişim belirtecinin dahil edilir.
* Microsoft Graph yanıta işler.

Bu örnek, kimlik doğrulaması uygulamak için Microsoft Authentication library (MSAL) kullanır. MSAL otomatik olarak yenileme belirteçleri, cihazdaki diğer uygulamalar arasında SSO sunun ve hesapları yönetin.

## <a name="prerequisites"></a>Önkoşullar

- XCode sürüm bu kılavuzda oluşturulan örnek 10.x gereklidir. Xcode'dan indirebileceğiniz [iTunes Web sitesi](https://geo.itunes.apple.com/us/app/xcode/id497799835?mt=12 "XCode indirme URL'si").
- Microsoft kimlik doğrulama kitaplığı ([MSAL.framework](https://github.com/AzureAD/microsoft-authentication-library-for-objc)). Bağımlılık Yöneticisi'ni kullanın veya el ile ekleyin. Aşağıda bir bölümü ile [daha fazla bilgi](#add-msal). 

## <a name="set-up-your-project"></a>Projenizi ayarlama

Bu öğreticide, yeni bir proje oluşturur. Tamamlanan öğretici bunun yerine, karşıdan yüklemek isterseniz [kodu indirmek](https://github.com/Azure-Samples/active-directory-ios-swift-native-v2/archive/master.zip).

### <a name="create-a-new-project"></a>Yeni bir proje oluşturma

1. Xcode açıp seçin **yeni bir Xcode projesi oluştur**.
2. Seçin **iOS > tek görünüm uygulaması** seçip **sonraki**.
3. Bir ürün adı verin ve seçin **sonraki**.
4. Uygulamanızı oluşturun ve'ı tıklatın, bir klasör seçin *Oluştur*.

## <a name="register-your-application"></a>Uygulamanızı kaydetme 

Uygulamanızı iki yoldan biriyle sonraki iki bölümde açıklandığı gibi kaydedebilirsiniz.

### <a name="register-your-app"></a>Uygulamanızı kaydedin

1. Git [Azure portalında](https://aka.ms/MobileAppReg) > seçin `New registration`. 
2. Girin bir **adı** uygulamanızın > `Register`. **Bu aşamada bir yeniden yönlendirme URI'si ayarlamayın**. 
3. İçinde `Manage` bölümüne gidin `Authentication` > `Add a platform` > `iOS`
    - Proje paket kimliğini girin. Kodu, bu ise `com.microsoft.identitysample.MSALiOS`.
4. İsabet `Configure` ve depolamak `MSAL Configuration` için daha sonra. 

## <a name="add-msal"></a>MSAL ekleyin

### <a name="get-msal"></a>MSAL Al

#### <a name="cocoapods"></a>CocoaPods

Kullanabileceğiniz [CocoaPods](http://cocoapods.org/) yüklemek için `MSAL` ekleyerek, `Podfile` hedef altında:

```
use_frameworks!

target '<your-target-here>' do
   pod 'MSAL', '~> 0.4.0'
end
```

#### <a name="carthage"></a>Carthage

Kullanabileceğiniz [Carthage](https://github.com/Carthage/Carthage) yüklemek için `MSAL` ekleyerek, `Cartfile`: 

```
github "AzureAD/microsoft-authentication-library-for-objc" "master"
```

#### <a name="manually"></a>El ile

Ayrıca Git alt modülüdür kullanın ya da en son sürüm denetleyin ve framework kullanmak.

### <a name="add-your-app-registration"></a>Uygulama kaydınızı Ekle

Ardından, uygulama kaydınızı kodunuza ekleyin. Ekleme, ***istemci / uygulama kimliği*** için `ViewController.swift`:

```swift
let kClientID = "Your_Application_Id_Here"

// Additional variables for Auth and Graph API
let kGraphURI = "https://graph.microsoft.com/v1.0/me/"
let kScopes: [String] = ["https://graph.microsoft.com/user.read"]
let kAuthority = "https://login.microsoftonline.com/common"
var accessToken = String()
var applicationContext : MSALPublicClientApplication?
```

### <a name="configure-url-schemes"></a>URL şemaları yapılandırın

Kayıt `CFBundleURLSchemes` kullanıcı oturum açma sonra uygulamaya geri yönlendirilebilir için bir geri çağırma izni.

`LSApplicationQueriesSchemes` varsa Microsoft Authenticator'ı kullanmak için uygulamanızı kullanımına izin verir. 

Bunu yapmak için açık `Info.plist` kaynağı olarak kod ve aşağıdaki ekleme, değiştirme ***BUNDLE_ID*** ile Azure portalında yapılandırılır.

```xml
<key>CFBundleURLTypes</key>
<array>
    <dict>
        <key>CFBundleURLSchemes</key>
        <array>
            <string>msauth.[BUNDLE_ID]</string>
        </array>
    </dict>
</array>
<key>LSApplicationQueriesSchemes</key>
<array>
    <string>msauth</string>
    <string>msauthv2</string>
</array>
```

### <a name="import-msal"></a>MSAL içeri aktarma

İçeri aktarma MSAL framework `ViewController.swift` ve `AppDelegate.swift` dosyaları:

```swift
import MSAL
```

## <a name="create-your-apps-ui"></a>Uygulamanızın kullanıcı Arabirimi oluşturma

Bu öğreticide, oluşturmanız gerekir:

- Graph API'si düğmesini arayın
- Oturum açın düğmesi
- Textview günlüğe kaydetme

İçinde `ViewController.swift`, özelliklerini tanımlayın ve `initUI()` şu şekilde:

```swift

var loggingText: UITextView!
var signOutButton: UIButton!
var callGraphButton: UIButton!

func initUI() {
        // Add call Graph button
        callGraphButton  = UIButton()
        callGraphButton.translatesAutoresizingMaskIntoConstraints = false
        callGraphButton.setTitle("Call Microsoft Graph API", for: .normal)
        callGraphButton.setTitleColor(.blue, for: .normal)
        callGraphButton.addTarget(self, action: #selector(callGraphAPI(_:)), for: .touchUpInside)
        self.view.addSubview(callGraphButton)
        
        callGraphButton.centerXAnchor.constraint(equalTo: view.centerXAnchor).isActive = true
        callGraphButton.topAnchor.constraint(equalTo: view.topAnchor, constant: 50.0).isActive = true
        callGraphButton.widthAnchor.constraint(equalToConstant: 300.0).isActive = true
        callGraphButton.heightAnchor.constraint(equalToConstant: 50.0).isActive = true
        
        // Add sign out button
        signOutButton = UIButton()
        signOutButton.translatesAutoresizingMaskIntoConstraints = false
        signOutButton.setTitle("Sign Out", for: .normal)
        signOutButton.setTitleColor(.blue, for: .normal)
        signOutButton.setTitleColor(.gray, for: .disabled)
        signOutButton.addTarget(self, action: #selector(signOut(_:)), for: .touchUpInside)
        self.view.addSubview(signOutButton)
        
        signOutButton.centerXAnchor.constraint(equalTo: view.centerXAnchor).isActive = true
        signOutButton.topAnchor.constraint(equalTo: callGraphButton.bottomAnchor, constant: 10.0).isActive = true
        signOutButton.widthAnchor.constraint(equalToConstant: 150.0).isActive = true
        signOutButton.heightAnchor.constraint(equalToConstant: 50.0).isActive = true
        signOutButton.isEnabled = false
        
        // Add logging textfield
        loggingText = UITextView()
        loggingText.isUserInteractionEnabled = false
        loggingText.translatesAutoresizingMaskIntoConstraints = false
        
        self.view.addSubview(loggingText)
        
        loggingText.topAnchor.constraint(equalTo: signOutButton.bottomAnchor, constant: 10.0).isActive = true
        loggingText.leftAnchor.constraint(equalTo: self.view.leftAnchor, constant: 10.0).isActive = true
        loggingText.rightAnchor.constraint(equalTo: self.view.rightAnchor, constant: 10.0).isActive = true
        loggingText.bottomAnchor.constraint(equalTo: self.view.bottomAnchor, constant: 10.0).isActive = true
    }
```

Ardından, ekleyin `viewDidLoad()` ViewController.swift biri:

```swift
    override func viewDidLoad() {
        super.viewDidLoad()
        initUI()
        do {
            try self.initMSAL()
        } catch let error {
            self.loggingText.text = "Unable to create Application Context \(error)"
        }
    }
```

## <a name="use-msal"></a>MSAL kullanın

### <a name="initialize-msal"></a>MSAL Başlat

İlk olarak oluşturmak gereken bir `MSALPublicClientApplication` örneğiyle birlikte `MSALPublicClientConfiguration` uygulamanız için:

```swift
    func initMSAL() throws {
        
        guard let authorityURL = URL(string: kAuthority) else {
            self.loggingText.text = "Unable to create authority URL"
            return
        }
        
        let authority = try MSALAADAuthority(url: authorityURL)
        
        let msalConfiguration = MSALPublicClientApplicationConfig(clientId: kClientID, redirectUri: nil, authority: authority)
        self.applicationContext = try MSALPublicClientApplication(configuration: msalConfiguration)
    }
```

### <a name="handle-the-callback"></a>Tanıtıcı geri çağırma

Oturum açma işleminden sonra geri çağırma işlemek için ekleme `MSALPublicClientApplication.handleMSALResponse` içinde `appDelegate`:

```swift
    func application(_ app: UIApplication, open url: URL, options: [UIApplication.OpenURLOptionsKey : Any] = [:]) -> Bool {
        
        guard let sourceApplication = options[UIApplication.OpenURLOptionsKey.sourceApplication] as? String else {
            return false
        }
        
        return MSALPublicClientApplication.handleMSALResponse(url, sourceApplication: sourceApplication)
    }
```

#### <a name="acquire-tokens"></a>Belirteç Al

Şimdi biz uygulamanın UI işleme mantığı ve belirteçleri MSAL etkileşimli olarak alma uygulayabilirsiniz. 

MSAL, belirteç almaya yönelik başlıca iki yöntem sunar: `acquireTokenSilently` ve `acquireTokenInteractively`.  

`acquireTokenSilently()` bir kullanıcının oturum açmasını ve hesabınız varsa, kullanıcı etkileşimi olmadan belirteçleri almak çalışır.

`acquireTokenInteractively` her zaman kullanıcı Arabirimi, kullanıcının oturum açmasını ve belirteçleri almak çalışırken gösterilir; Ancak, bu tarayıcıda oturum tanımlama bilgileri veya Microsoft authenticator hesap etkileşimli SSO bir deneyim sunmak için kullanabilirsiniz. 

```swift
    @objc func callGraphAPI(_ sender: UIButton) {
        
        guard let currentAccount = self.currentAccount() else {
            // We check to see if we have a current logged in account.
            // If we don't, then we need to sign someone in.
            acquireTokenInteractively()
            return
        }
        
        acquireTokenSilently(currentAccount)
    }

    func currentAccount() -> MSALAccount? {
        
        guard let applicationContext = self.applicationContext else { return nil }
        
        // We retrieve our current account by getting the first account from cache
        // In multi-account applications, account should be retrieved by home account identifier or username instead
        
        do {
            let cachedAccounts = try applicationContext.allAccounts()
            if !cachedAccounts.isEmpty {
                return cachedAccounts.first
            }
        } catch let error as NSError {
            self.updateLogging(text: "Didn't find any accounts in cache: \(error)")
        }
        
        return nil
    }
```

#### <a name="get-a-token-interactively"></a>Etkileşimli bir belirteç Al

İlk kez bir belirteç almak için oluşturmanız gerekecektir bir `MSALInteractiveTokenParameters` ve çağrı `acquireToken`.

1. Oluşturma `MSALInteractiveTokenParameters` kapsamları ile.
2. Çağrı `acquireToken` oluşturulan parametrelere sahip.
3. Hata işlemelidir. Daha fazla ayrıntı için başvurmak [iOS hata işleme Kılavuzu](https://github.com/AzureAD/microsoft-authentication-library-for-objc/wiki/Error-Handling).
4. Başarılı durum işleyin. 

```swift
    func acquireTokenInteractively() {
   
        guard let applicationContext = self.applicationContext else { return }
     // #1    
        let parameters = MSALInteractiveTokenParameters(scopes: kScopes)
     // #2        
        applicationContext.acquireToken(with: parameters) { (result, error) in
     // #3            
            if let error = error {
                self.updateLogging(text: "Could not acquire token: \(error)")
                return
            }
            guard let result = result else {   
                self.updateLogging(text: "Could not acquire token: No result returned")
                return
            }
     // #4            
            self.accessToken = result.accessToken
            self.updateLogging(text: "Access token is \(self.accessToken)")
            self.updateSignOutButton(enabled: true)
            self.getContentWithToken()
        }
    }
```



#### <a name="get-a-token-silently"></a>Sessiz bir belirteç Al

Sessiz bir şekilde güncelleştirilmiş bir belirteci almak için oluşturmanız gerekecektir bir `MSALSilentTokenParameters` ve çağrı `acquireTokenSilent`:

```swift
    
    func acquireTokenSilently(_ account : MSALAccount!) {
        guard let applicationContext = self.applicationContext else { return }
        let parameters = MSALSilentTokenParameters(scopes: kScopes, account: account)
        
        applicationContext.acquireTokenSilent(with: parameters) { (result, error) in    
            if let error = error {
                let nsError = error as NSError
                if (nsError.domain == MSALErrorDomain) {
                    if (nsError.code == MSALError.interactionRequired.rawValue) {
                        DispatchQueue.main.async {
                            self.acquireTokenInteractively()
                        }
                        return
                    }
                }
                self.updateLogging(text: "Could not acquire token silently: \(error)")
                return
            }
            
            guard let result = result else {
                self.updateLogging(text: "Could not acquire token: No result returned")
                return
            }
            
            self.accessToken = result.accessToken
            self.updateLogging(text: "Refreshed Access token is \(self.accessToken)")
            self.updateSignOutButton(enabled: true)
            self.getContentWithToken()
        }
    }
```

### <a name="call-the-microsoft-graph-api"></a>Microsoft Graph API çağırma 

Aracılığıyla bir belirteç sonra `self.accessToken`, uygulamanıza, Microsoft Graph için yetkili bir isteği yapmak için HTTP üst bilgisinde bu değeri kullanabilirsiniz:

| Üstbilgi anahtarı    | value                 |
| ------------- | --------------------- |
| Yetkilendirme | Taşıyıcı < erişim belirteci > |

Ekleyin `ViewController.swift`:

```swift
    func getContentWithToken() {        
        // Specify the Graph API endpoint
        let url = URL(string: kGraphURI)
        var request = URLRequest(url: url!)
        
        // Set the Authorization header for the request. We use Bearer tokens, so we specify Bearer + the token we got from the result
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
               
        self.updateLogging(text: "Result from Graph: \(result))")
        
        }.resume()
    }
```

Daha fazla bilgi edinin [Microsoft Graph API'si](https://graph.microsoft.com)

### <a name="use-msal-for-sign-out"></a>Kullanmak için MSAL oturum kapatma

Ardından, için destek ekleyeceğiz uygulamamız için oturum kapatma. 

Unutmayın, oturum kapatma MSAL ile bir kullanıcı ile ilgili tüm bilinen bilgileri bu uygulamadan kaldırır, ancak kullanıcı kendi cihazında hala etkin bir oturuma sahip önemlidir. Kullanıcının oturum açmak çalışırsa yeniden bunlar etkileşim görebilirsiniz, ancak etkin cihaz oturumu nedeniyle kimlik bilgilerini yeniden girmeniz gerekmez. 

Oturum kapatma eklemek için aşağıdaki metodun Metoda kopyalayın, `ViewController.swift`:

```swift 
    @objc func signOut(_ sender: UIButton) {
        
        guard let applicationContext = self.applicationContext else { return }
        
        guard let account = self.currentAccount() else { return }
        
        do {
            
            /**
             Removes all tokens from the cache for this application for the provided account
             
             - account:    The account to remove from the cache
             */
            
            try applicationContext.remove(account)
            self.loggingText.text = ""
            self.signOutButton.isEnabled = false
            
        } catch let error as NSError {
            
            self.updateLogging(text: "Received error signing account out: \(error)")
        }
    }
```

### <a name="enable-token-caching"></a>Belirteç önbelleğe almayı etkinleştir

Varsayılan olarak, uygulamanızın belirteçleri iOS Anahtarlıkta MSAL önbelleğe alır. 

Belirteç önbelleğe almayı etkinleştirmek için Xcode proje ayarlarınıza gidin > `Capabilities tab`  >  `Enable Keychain Sharing` > tıklatın `Plus` > Enter **com.microsoft.adalcache**.

### <a name="add-helper-methods"></a>Yardımcı yöntemler ekler

Örneği tamamlamak için bu yardımcı yöntemler ekleyin:

``` swift
    
    func updateLogging(text : String) {
        
        if Thread.isMainThread {
            self.loggingText.text = text
        } else {
            DispatchQueue.main.async {
                self.loggingText.text = text
            }
        }
    }
    
    func updateSignOutButton(enabled : Bool) {
        if Thread.isMainThread {
            self.signOutButton.isEnabled = enabled
        } else {
            DispatchQueue.main.async {
                self.signOutButton.isEnabled = enabled
            }
        }
    }
```

### <a name="multi-account-applications"></a>Birden çok hesap uygulamalar

Bu uygulama, tek bir hesap senaryo için oluşturulmuştur. MSAL birden çok hesap senaryoları destekler, ancak bazı ek işleri uygulamalardan gerektirir. Belirteçleri gerektiren her eylem için kullanmak istediğiniz hesabı seçin kullanıcının yardımcı olmak için kullanıcı Arabirimi oluşturmanız gerekir. Alternatif olarak, uygulamanızı aracılığıyla kullanacağınız hesabı seçmek için bir buluşsal yöntem uygulayabilirsiniz `getAllAccounts(...)` yöntemi.

## <a name="test-your-app"></a>Uygulamanızı test etme

### <a name="run-locally"></a>Yerel olarak çalıştırma

Yukarıdaki kod izlediyseniz, bir test cihaz veya öykünücü uygulaması derleyecek ve deneyin. Oturum açmak ve belirteçleri almak için Azure AD veya kişisel olmalıdır Microsoft hesapları! Oturum açma kullanıcı sonra bu uygulamanın Microsoft Graph'teki döndürülen veriler görüntüler `/me` uç noktası. 

Herhangi bir sorun varsa, bu belge veya MSAL kitaplığında bir sorun açın ve bunu bize bildirin çekinmeyin. 

### <a name="consent-to-your-app"></a>Uygulamanıza onay

Herhangi bir kullanıcı oturum açtığında, uygulamanıza ilk kez bunlar Microsoft kimlik tarafından istenen izinleri kabul istenir.  Çoğu kullanıcı tümleştirip özelliğine sahip olsa da, bazı Azure AD kiracılarıyla yöneticilerin tüm kullanıcılar adına onay vermesini gerektiren kullanıcı onayı - devre dışı bırakıldı.  Bu senaryoyu desteklemek için Azure Portal'da uygulamanızın kapsamlarını kaydetmek emin olun.

## <a name="help-and-support"></a>Yardım ve Destek

Bu öğreticide veya Microsoft kimlik platformu ile herhangi bir sorun oluştu.? Bkz: [Yardım ve Destek](https://docs.microsoft.com/azure/active-directory/develop/developer-support-help-options)

