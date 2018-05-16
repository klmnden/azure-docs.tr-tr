---
title: Azure AD Cordova Başlarken | Microsoft Docs
description: Oturum açma için Azure AD ile tümleştirilen ve OAuth kullanarak Azure AD korumalı API çağıran bir Cordova uygulaması oluşturma.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: b1a8d7bd-7ad6-44d5-8ccb-5255bb623345
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: javascript
ms.topic: article
ms.date: 11/30/2017
ms.author: celested
ms.custom: aaddev
ms.openlocfilehash: 6d6d514875aa675bf160ee08a3e94b58944020ee
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2018
---
# <a name="azure-ad-cordova-getting-started"></a>Azure AD Cordova Başlarken
[!INCLUDE [active-directory-devquickstarts-switcher](../../../includes/active-directory-devquickstarts-switcher.md)]

[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

Apache Cordova mobil aygıtlarda tam özellikli yerel uygulamalar olarak çalıştırabilirsiniz HTML5/JavaScript uygulamaları geliştirmek için kullanabilirsiniz. Azure Active Directory ile (Azure AD), Cordova uygulamalarınızı Kurumsal düzeyde kimlik doğrulama özellikleri ekleyebilirsiniz.

Cordova eklentisi Azure sarmalar AD yerel SDK'ları iOS, Android, Windows mağazası ve Windows Phone. Kullanarak eklenti, oturum açma, kullanıcılarınızın Windows Server Active Directory hesaplarıyla desteklemek, Office 365 ve Azure API'lerini erişmek ve hatta kendi özel web API çağrıları korunmasına yardımcı olmak için uygulamanızın geliştirebilirsiniz.

Bu öğreticide, aşağıdaki özellikler ekleyerek basit bir uygulama geliştirmek için Active Directory Authentication Library (ADAL) için eklenti Apache Cordova kullanacağız:

* Yalnızca birkaç satır kod ile bir kullanıcının kimliğini doğrulamak ve bir belirteç elde edin.
* Bu dizini sorgulayabilir ve sonuçları görüntülemek için grafik API'sini çağırmak için bu belirteci kullanın. 
* Kullanıcı için kimlik doğrulaması ister en aza indirmek için ADAL belirteç önbelleği kullanın.

Bu geliştirmeler yapmak için aktarmanız gerekir:

1. Bir uygulamayı Azure AD'ye kaydedin.
2. Belirteç isteme uygulamanıza kod ekleyin.
3. Grafik API'si sorgulamak için belirteci kullanın için kodu ekleyin ve sonuçları görüntüleyin.
4. Hedef, Cordova ADAL eklenti eklemek ve Öykünücüler çözümü test istediğiniz tüm platformlar ile Cordova dağıtım projesi oluşturun.

## <a name="prerequisites"></a>Önkoşullar
Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Uygulama geliştirme hakları olan bir hesabın sahip olduğu bir Azure AD kiracısı.
* Apache Cordova kullanmak üzere yapılandırılmış bir geliştirme ortamı. 

Her ikisi de varsa, ayarlamak, doğrudan adım 1 geçin.

Azure AD kiracısı yoksa kullanmak [nasıl edinebileceğinizi yönergeler](active-directory-howto-tenant.md).

Apache Cordova makinenizde ayarlanan yoksa, aşağıdaki yükleyin:

* [Git](http://git-scm.com/book/en/v2/Getting-Started-Installing-Git)
* [Node.js](https://nodejs.org/download/)
* [Cordova CLI](https://cordova.apache.org/) (NPM Paket Yöneticisi kolayca yüklenebilir: `npm install -g cordova`)

Önceki yüklemeleri PC ve Mac çalışması gerekir

Her hedef platformu farklı Önkoşullar vardır:

* Derleme ve Windows Tablet/PC veya Windows Phone için bir uygulamayı çalıştırmak için:
  * Yükleme [Windows Update 2 veya sonraki sürümü için Visual Studio 2013](http://www.visualstudio.com/downloads/download-visual-studio-vs#d-express-windows-8) (Express veya başka bir sürümü) veya [Visual Studio 2015](https://www.visualstudio.com/downloads/download-visual-studio-vs#d-community).

* Derleme ve iOS için uygulama çalıştırmak için:

  * Xcode yükleme 6.x veya sonraki bir sürümü. İndirin [Apple Developer site](http://developer.apple.com/downloads) veya [Mac App Store](http://itunes.apple.com/us/app/xcode/id497799835?mt=12).
  * Yükleme [ios-SIM](https://www.npmjs.org/package/ios-sim). İOS uygulamaları iOS simülatörü komut satırından başlatmak için kullanabilirsiniz. (Kolayca terminal yükleyebilirsiniz: `npm install -g ios-sim`.)
* Derleme ve Android için uygulama çalıştırmak için:

  * Yükleme [Java Geliştirme Seti (JDK) 7](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) veya sonraki bir sürümü. Emin olun `JAVA_HOME` (ortam değişkeni) JDK yükleme yolu (örneğin, C:\Program Files\Java\jdk1.7.0_75) göre doğru olarak ayarlayın.
  * Yükleme [Android SDK](http://developer.android.com/sdk/installing/index.html?pkg=tools) ve ekleme `<android-sdk-location>\tools` konuma (örneğin, C:\tools\Android\android-sdk\tools), `PATH` ortam değişkeni.
  * Android SDK Yöneticisi'ni açın (örneğin, terminal aracılığıyla: `android`) ve yükleyin:
    * *Android 5.0.1 (API 21)* platform SDK'si
    * *Android SDK derleme araçlarını* sürüm 19.1.0 veya daha yenisi
    * *Android desteği depo* (ek özellikler)

  Android SDK varsayılan öykünücü örnek sağlamaz. Çalıştırarak oluşturmak `android avd` terminalde seçilerek **oluşturma**, Android uygulaması bir öykünücüsünde çalıştırmak istiyorsanız. 19 veya daha yüksek bir API düzeyi öneririz. Android öykünücü ve oluşturma seçenekleri hakkında daha fazla bilgi için bkz: [AVD Yöneticisi](http://developer.android.com/tools/help/avd-manager.html) Android sitesinde.

## <a name="step-1-register-an-application-with-azure-ad"></a>1. adım: bir uygulamayı Azure AD ile kaydedin.
Bu adım isteğe bağlıdır. Bu öğretici, kendi Kiracı sağlama yapmadan eylem örnek görmek için kullanabileceğiniz önceden sağlanan değerler sağlar. Ancak, bu adımı gerçekleştirmek ve kendi uygulamaları oluştururken gerekli olacağı için işlemiyle aşina öneririz.

Azure AD yalnızca bilinen uygulamalara belirteçleri. Azure AD, uygulamanızdan kullanabilmeniz için önce bir girdi için kiracınızda oluşturmanız gerekir. Yeni bir uygulama kiracınızda kaydetmek için:

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Üst çubuğunda hesabınızı tıklatın. İçinde **Directory** listesinde, Azure AD Kiracı uygulamanızı kaydetmek istediğiniz yeri seçin.
3. Tıklatın **tüm hizmetleri** sol bölmesinde ve seçip **Azure Active Directory**.
4. Tıklatın **uygulama kayıtlar**ve ardından **Ekle**.
5. Komut istemlerini izleyin ve oluşturun bir **yerel istemci uygulaması**. (Cordova uygulamaları HTML bağlı olsa da, bir yerel istemci uygulaması oluşturuyoruz. **Yerel istemci uygulaması** seçeneği seçili olmalıdır veya uygulama çalışmaz.)
  * **Ad** kullanıcılar uygulamanıza açıklar.
  * **Yeniden yönlendirme URI'si** belirteçleri uygulamanıza döndürmek için kullanılan bir URI. Girin **http://MyDirectorySearcherApp**.

Kayıt işlemini tamamladıktan sonra Azure AD benzersiz uygulama kimliği uygulamanıza atar. Bu değeri sonraki bölümlerde bulunan gerekir. Yeni oluşturulan uygulama uygulama sekmesinde bulabilirsiniz.

Çalıştırmak için `DirSearchClient Sample`, Azure AD Graph API sorgulamak için yeni oluşturulan uygulama izni verin:

1. Gelen **ayarları** sayfasında, **gerekli izinler**ve ardından **Ekle**. 
2. Azure Active Directory uygulaması için seçin **Microsoft Graph** API olarak ve ekleme **gibi oturum açan kullanıcının dizine erişim** altında izni **izinlere temsilci**. Bu kullanıcılar için grafik API'si sorgulamak uygulamanızı sağlar.

## <a name="step-2-clone-the-sample-app-repository"></a>2. adım: örnek uygulama depoyu kopyalayın
Kabuk veya komut satırı, aşağıdaki komutu yazın:

    git clone -b skeleton https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-Cordova.git

## <a name="step-3-create-the-cordova-app"></a>3. adım: Cordova uygulaması oluşturma
Cordova uygulamaları oluşturmak için birden çok yolu vardır. Bu öğreticide, Cordova komut satırı arabirimi (CLI) kullanacağız.

1. Kabuk veya komut satırı, aşağıdaki komutu yazın:

        cordova create DirSearchClient

   Bu komut, Cordova projesi için askılamayı ve klasör yapısı oluşturur.

2. Yeni DirSearchClient klasöre gidin:

        cd .\DirSearchClient

3. Başlangıç projesi içeriğini Kabuğu'nda Dosya Yöneticisi veya aşağıdaki komutu kullanarak www alt klasöründe kopyalayın:

  * Windows: `xcopy ..\NativeClient-MultiTarget-Cordova\DirSearchClient www /E /Y`
  * Mac: `cp -r  ../NativeClient-MultiTarget-Cordova/DirSearchClient/* www`

4. Beyaz liste eklentisini ekleyin. Bu grafik API'sini çağırma için gereklidir.

        cordova plugin add cordova-plugin-whitelist

5. Desteklemek istediğiniz tüm platformlar ekleyin. Bir çalışma örneği için en az biri aşağıdaki komutları yürütün gerekir. Windows İos'ta öykünmek veya bir Mac bilgisayar Windows'ta öykünmek mümkün olmayacaktır unutmayın

        cordova platform add android
        cordova platform add ios
        cordova platform add windows

6. ADAL Cordova eklentisi projenize ekleyin:

        cordova plugin add cordova-plugin-ms-adal

## <a name="step-4-add-code-to-authenticate-users-and-obtain-tokens-from-azure-ad"></a>4. adım: kullanıcıların kimlik doğrulaması ve Azure AD'den belirteçleri elde etmek için kod ekleme
Bu öğreticide geliştirirken uygulama bir Basit Dizin arama özelliğini sağlar. Kullanıcı dizindeki diğer herhangi bir kullanıcı adını yazın ve bazı temel öznitelikler görselleştirin. Başlangıç projesi uygulamada (www/index.html) temel kullanıcı arabiriminin tanımını içerir ve temel uygulama olay bağlayan yapı iskelesi, kullanıcı arabirimi bağlamaları geçiş yapar ve görüntü mantığında (www/js/index.js) sonuçlanır. Sizin için sol yalnızca kimlik görevleri uygulayan mantığı eklemek için bir görevdir.

Kodunuzda yapmanız gereken ilk şey, Azure AD uygulamanızı ve kaynakları tanımlamak için kullandığı Protokolü değerleri, hedefleyen tanıtmak ' dir. Bu değerler, belirteç isteklerini daha sonra oluşturmak için kullanılır. Aşağıdaki kod parçacığında index.js dosyanın üst kısmında ekleyin:

```javascript
var authority = "https://login.microsoftonline.com/common",
    redirectUri = "http://MyDirectorySearcherApp",
    resourceUri = "https://graph.windows.net",
    clientId = "a5d92493-ae5a-4a9f-bcbf-9f1d354067d3",
    graphApiVersion = "2013-11-08";
```

`redirectUri` Ve `clientId` değerleri, uygulamanızı Azure AD'de açıklayan değerler eşleşmelidir. Olanlardan bulabilirsiniz **yapılandırma** 1. adımda Bu öğreticide daha önce açıklandığı gibi Azure portalında sekmesinde.

> [!NOTE]
> Yeni bir uygulama, kendi Kiracı kayıt değil için ettiyseniz, olduğu gibi önceden yapılandırılmış değerleri yalnızca yapıştırabilirsiniz. Üretim için yöneliktir uygulamalarınız için her zaman kendi giriş oluşturmalısınız rağmen çalışan, örnek görebilirsiniz.

Ardından, belirteci isteği kodu ekleyin. Aşağıdaki kod parçacığını arasında Ekle `search` ve `renderData` tanımları:

```javascript
    // Shows the user authentication dialog box if required
    authenticate: function (authCompletedCallback) {

        app.context = new Microsoft.ADAL.AuthenticationContext(authority);
        app.context.tokenCache.readItems().then(function (items) {
            if (items.length > 0) {
                authority = items[0].authority;
                app.context = new Microsoft.ADAL.AuthenticationContext(authority);
            }
            // Attempt to authorize the user silently
            app.context.acquireTokenSilentAsync(resourceUri, clientId)
            .then(authCompletedCallback, function () {
                // We require user credentials, so this triggers the authentication dialog box
                app.context.acquireTokenAsync(resourceUri, clientId, redirectUri)
                .then(authCompletedCallback, function (err) {
                    app.error("Failed to authenticate: " + err);
                });
            });
        });

    },
```
Bu işlev, iki ana bölüme çiğnemekten tarafından inceleyelim.
Bu örnek, belirli bir bağlı tüm Kiracı çalışmak üzere tasarlanmıştır. Kullanıcının kimlik doğrulaması anında herhangi bir hesabı girmesini sağlar ve Kiracı isteğine ait olduğu yönlendirir "/ ortak" endpoint kullanır.

Bu yöntem ilk kısmı bir belirteç zaten depolanmış görmek için ADAL önbelleğin olup olmadığını denetler. Öyleyse, yöntem nereden belirteç ADAL yeniden geldiğini kiracılar kullanır. "/ Ortak", kullanım her zaman yeni bir hesabı girmesini isteyen sonuçlandığından bu fazladan istemleri önlemek gereklidir.

```javascript
        app.context = new Microsoft.ADAL.AuthenticationContext(authority);
        app.context.tokenCache.readItems().then(function (items) {
            if (items.length > 0) {
                authority = items[0].authority;
                app.context = new Microsoft.ADAL.AuthenticationContext(authority);
            }
```
Yöntemin ikinci bölümü doğru belirteci isteği gerçekleştirir. `acquireTokenSilentAsync` Yöntemi, belirtilen kaynak için bir belirteç herhangi UX göstermeden dönmek için ADAL sorar Önbellekte depolanan, uygun erişim belirteci zaten varsa oluşabilir veya bir yenileme belirteci herhangi bir istem göstermeden yeni bir erişim belirteci almak için kullanılabilir. O deneme başarısız olursa, biz geri dönmesi `acquireTokenAsync`--hangi görünür şekilde ister kullanıcının kimlik doğrulamasını.

```javascript
            // Attempt to authorize the user silently
            app.context.acquireTokenSilentAsync(resourceUri, clientId)
            .then(authCompletedCallback, function () {
                // We require user credentials, so this triggers the authentication dialog box
                app.context.acquireTokenAsync(resourceUri, clientId, redirectUri)
                .then(authCompletedCallback, function (err) {
                    app.error("Failed to authenticate: " + err);
                });
            });
```
Biz belirtece sahip olduğunuza göre biz son grafik API'sini çağırmak ve istiyoruz arama sorgusu gerçekleştirin. Aşağıdaki kod parçacığını aşağıdaki Ekle `authenticate` tanımı:

```javascript
// Makes an API call to receive the user list
    requestData: function (authResult, searchText) {
        var req = new XMLHttpRequest();
        var url = resourceUri + "/" + authResult.tenantId + "/users?api-version=" + graphApiVersion;
        url = searchText ? url + "&$filter=mailNickname eq '" + searchText + "'" : url + "&$top=10";

        req.open("GET", url, true);
        req.setRequestHeader('Authorization', 'Bearer ' + authResult.accessToken);

        req.onload = function(e) {
            if (e.target.status >= 200 && e.target.status < 300) {
                app.renderData(JSON.parse(e.target.response));
                return;
            }
            app.error('Data request failed: ' + e.target.response);
        };
        req.onerror = function(e) {
            app.error('Data request failed: ' + e.error);
        }

        req.send();
    },

```
Başlangıç noktası dosyaları, metin kutusuna bir kullanıcının diğer adı girmek için basit bir UX sağlanan. Bu yöntem, bir sorgu oluşturun, erişim belirteciyle birleştirmek, Microsoft Graph göndermek ve sonuçlarını ayrıştırmak için bu değeri kullanır. `renderData` Yöntemi, başlangıç noktası dosyasında zaten mevcut sonuçlarını görselleştirme mvc'deki.

## <a name="step-5-run-the-app"></a>5. adım: uygulama çalıştırma
Uygulamanızı çalıştırmak son hazırdır. Bu işletim basittir: uygulama başlatıldığında diğer aramak istediğiniz kullanıcının adını girin ve ardından düğmesine tıklayın. Kimlik doğrulaması için istenir. Başarılı kimlik doğrulama ve başarılı arama sırasında aranan kullanıcı özniteliklerini görüntülenir.

Sonraki çalıştırır, önceden alınmış belirtecin önbelleğinde varlığını sayesinde hiçbir istemi göstermeden arama gerçekleştirir.

Uygulamayı çalıştırmak için somut adımları platforma göre değişir.

### <a name="windows-10"></a>Windows 10
   Tablet/bilgisayar: `cordova run windows --archs=x64 -- --appx=uap`

   Mobil (bir Bilgisayara bağlı bir Windows 10 mobil aygıt gerektirir): `cordova run windows --archs=arm -- --appx=uap --phone`

   > [!NOTE]
   > İlk çalıştırmada Geliştirici lisansı oturum açmak için istenebilir. Daha fazla bilgi için bkz: [Geliştirici lisansı](https://msdn.microsoft.com/library/windows/apps/hh974578.aspx).

### <a name="windows-81-tabletpc"></a>Windows 8.1 Tablet/PC
   `cordova run windows`

   > [!NOTE]
   > İlk çalıştırmada Geliştirici lisansı oturum açmak için istenebilir. Daha fazla bilgi için bkz: [Geliştirici lisansı](https://msdn.microsoft.com/library/windows/apps/hh974578.aspx).

### <a name="windows-phone-81"></a>Windows Phone 8.1
   Bağlı bir cihaza çalıştırmak için: `cordova run windows --device -- --phone`

   Varsayılan öykünücüsünde çalıştırmak için: `cordova emulate windows -- --phone`

   Kullanım `cordova run windows --list -- --phone` tüm kullanılabilir hedefleri görmek için ve `cordova run windows --target=<target_name> -- --phone` belirli cihaz veya öykünücü üzerinde uygulamayı çalıştırmak için (örneğin, `cordova run windows --target="Emulator 8.1 720P 4.7 inch" -- --phone`).

### <a name="android"></a>Android
   Bağlı bir cihaza çalıştırmak için: `cordova run android --device`

   Varsayılan öykünücüsünde çalıştırmak için: `cordova emulate android`

   Bir öykünücü örneğinde AVD Yöneticisi'ni kullanarak daha önce "Önkoşullar" bölümünde açıklandığı gibi oluşturduğunuz emin olun.

   Kullanım `cordova run android --list` tüm kullanılabilir hedefleri görmek için ve `cordova run android --target=<target_name>` belirli cihaz veya öykünücü üzerinde uygulamayı çalıştırmak için (örneğin, `cordova run android --target="Nexus4_emulator"`).

### <a name="ios"></a>iOS
   Bağlı bir cihaza çalıştırmak için: `cordova run ios --device`

   Varsayılan öykünücüsünde çalıştırmak için: `cordova emulate ios`

   > [!NOTE]
   > Olduğundan emin olun `ios-sim` öykünücüsünde çalıştırmak için yüklü paket. Daha fazla bilgi için "Önkoşullar" bölümüne bakın.

    Use `cordova run ios --list` to see all available targets and `cordova run ios --target=<target_name>` to run the application on specific device or emulator (for example, `cordova run android --target="iPhone-6"`).

    Use `cordova run --help` to see additional build and run options.

## <a name="next-steps"></a>Sonraki adımlar
Başvuru için (yapılandırma değerleriniz olmadan) tamamlanan örnek kullanılabilir [GitHub](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-Cordova/tree/complete/DirSearchClient).

Artık daha gelişmiş (ve daha ilginç) senaryolara geçebilirsiniz. Denemek isteyebilirsiniz: [Azure AD ile bir Node.js Web API'SİNİN güvenliğini](active-directory-devquickstarts-webapi-nodejs.md).

[!INCLUDE [active-directory-devquickstarts-additional-resources](../../../includes/active-directory-devquickstarts-additional-resources.md)]
