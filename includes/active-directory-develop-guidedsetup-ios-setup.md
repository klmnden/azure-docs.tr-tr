---
title: include dosyası
description: include dosyası
services: active-directory
documentationcenter: dev-center-name
author: danieldobalian
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: ios
ms.workload: identity
ms.date: 09/19/2018
ms.author: dadobali
ms.custom: include file
ms.openlocfilehash: e72c4b0cf8f77a057ff07f8bce7acae4e834e28d
ms.sourcegitcommit: 807c318f5c034f8256f91c241e9d6f8f4d7de90a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2019
ms.locfileid: "64951391"
---
## <a name="setting-up-your-ios-application"></a>İOS uygulamanızı ayarlama

Bu bölümde, ile nasıl tümleştireceğinizi bir iOS uygulaması (Swift) göstermek için yeni proje oluşturma adım adım yönergeler sağlar. *Microsoft ile oturum açma* bir belirteç gerektiren bir Web API sorgulayabilmesi.

> Bunun yerine bu örnek'ın XCode projesi indirme mi tercih ediyorsunuz? [Bir projeyi indirirken](https://github.com/Azure-Samples/active-directory-ios-swift-native-v2/archive/master.zip) ve atlamak [yapılandırma adımı](#register-your-application) çalıştırmadan önce kodu örneği yapılandırmak için.

## <a name="install-carthage-to-download-and-build-msal"></a>İndirip MSAL derleme Carthage yükleyin

Carthage Paket Yöneticisi MSAL Önizleme dönemi boyunca kullanılan – yeteneği kitaplığa değişiklik yapmak Microsoft korurken XCode ile tümleşir.

- Carthage en son sürümünü indirip [burada](https://github.com/Carthage/Carthage/releases "Carthage indirme URL'sini").

## <a name="creating-your-application"></a>Uygulamanızı oluşturma

1. Xcode açıp seçin **yeni bir Xcode projesi oluştur**.
2. Seçin **iOS > tek görünüm uygulaması** seçip **sonraki**.
3. Bir ürün adı verin ve seçin **sonraki**.
4. Uygulamanızı oluşturun ve'ı tıklatın, bir klasör seçin *oluştur*

## <a name="build-the-msal-framework"></a>MSAL Framework'te derleme

Çekme ve ardından en son sürümünü kullanarak Carthage MSAL kitaplıkları oluşturmak için aşağıdaki yönergeleri izleyin:

1. Bash Terminali açın ve uygulamanın kök klasöre gidin.
2. Kopyalama aşağıda 'Cartfile' dosyası oluşturmak için bash terminalde yapıştırın:

```bash
echo "github \"AzureAD/microsoft-authentication-library-for-objc\" \"master\"" > Cartfile
```
<!-- Workaround for Docs conversion bug -->
<ol start="3">
<li>
Kopyalama ve yapıştırma aşağıda. Bu komut bir Carthage/kullanıma klasöre bağımlılıkları getirir ve ardından MSAL kitaplığı oluşturur:
</li>
</ol>

```bash
carthage update
```

> Yukarıdaki işlem, indirin ve Microsoft Authentication Library (MSAL) oluşturmak için kullanılır. MSAL alınırken, önbelleğe alma ve Microsoft kimlik platformu tarafından korunan API'lerine erişmek için kullanılan kullanıcı belirteçleri yenileme işler.

## <a name="add-the-msal-framework-to-your-application"></a>MSAL framework uygulamanıza ekleyin

1. Xcode'da açın **genel** sekmesi.
2. Git **bağlantılı çerçeveler ve kitaplıklar** seçin ve bölüm **+**.
3. Seçin **diğer Ekle...** .
4. Seçin **Carthage > derleme > iOS > MSAL.framework** seçip **açık**. Görmelisiniz `MSAL.framework` listesine eklenir.
5. Git **derleme aşamaları** sekmesinde **+** simgesine tıklayın ve ardından **yeni çalıştırma betik aşama**.
6. Aşağıdaki içeriği ekleyin *betik alan*:

```text
/usr/local/bin/carthage copy-frameworks
```

<!-- Workaround for Docs conversion bug -->
<ol start="7">
<li>
Ekleyin <code>Input Files</code> tıklayarak <code>+</code>:
</li>
</ol>

```text
$(SRCROOT)/Carthage/Build/iOS/MSAL.framework
```

## <a name="creating-your-applications-ui"></a>Uygulamanızın kullanıcı Arabirimi oluşturma

Main.storyboard dosyası, proje şablonunun bir parçası olarak otomatik olarak oluşturulması gerekir. Uygulama kullanıcı Arabirimi oluşturmak için aşağıdaki yönergeleri izleyin:

1.  Control + tıklayın `Main.storyboard` bağlamsal menüyü getirin ve ardından: `Open As` > `Source Code`
2.  Değiştirin `<scenes>` düğüm aşağıdaki kod ile:

```xml
 <scenes>
    <scene sceneID="tne-QT-ifu">
        <objects>
            <viewController id="BYZ-38-t0r" customClass="ViewController" customModule="MSALiOS" customModuleProvider="target" sceneMemberID="viewController">
                <layoutGuides>
                    <viewControllerLayoutGuide type="top" id="y3c-jy-aDJ"/>
                    <viewControllerLayoutGuide type="bottom" id="wfy-db-euE"/>
                </layoutGuides>
                <view key="view" contentMode="scaleToFill" id="8bC-Xf-vdC">
                    <rect key="frame" x="0.0" y="0.0" width="375" height="667"/>
                    <autoresizingMask key="autoresizingMask" widthSizable="YES" heightSizable="YES"/>
                    <subviews>
                        <label opaque="NO" userInteractionEnabled="NO" contentMode="left" horizontalHuggingPriority="251" verticalHuggingPriority="251" fixedFrame="YES" text="Microsoft Authentication Library" textAlignment="natural" lineBreakMode="tailTruncation" baselineAdjustment="alignBaselines" adjustsFontSizeToFit="NO" translatesAutoresizingMaskIntoConstraints="NO" id="ifd-fu-zjm">
                            <rect key="frame" x="64" y="28" width="246" height="21"/>
                            <autoresizingMask key="autoresizingMask" flexibleMaxX="YES" flexibleMaxY="YES"/>
                            <fontDescription key="fontDescription" type="system" pointSize="17"/>
                            <nil key="textColor"/>
                            <nil key="highlightedColor"/>
                        </label>
                        <label opaque="NO" userInteractionEnabled="NO" contentMode="left" horizontalHuggingPriority="251" verticalHuggingPriority="251" fixedFrame="YES" text="Logging" textAlignment="natural" lineBreakMode="tailTruncation" baselineAdjustment="alignBaselines" adjustsFontSizeToFit="NO" translatesAutoresizingMaskIntoConstraints="NO" id="98g-dc-BPL">
                            <rect key="frame" x="16" y="277" width="62" height="21"/>
                            <autoresizingMask key="autoresizingMask" flexibleMaxX="YES" flexibleMaxY="YES"/>
                            <fontDescription key="fontDescription" type="system" pointSize="17"/>
                            <nil key="textColor"/>
                            <nil key="highlightedColor"/>
                        </label>
                        <button opaque="NO" contentMode="scaleToFill" fixedFrame="YES" contentHorizontalAlignment="center" contentVerticalAlignment="center" buttonType="roundedRect" lineBreakMode="middleTruncation" translatesAutoresizingMaskIntoConstraints="NO" id="2rX-Vv-T1i">
                            <rect key="frame" x="87" y="100" width="200" height="30"/>
                            <autoresizingMask key="autoresizingMask" flexibleMaxX="YES" flexibleMaxY="YES"/>
                            <state key="normal" title="Call Microsoft Graph API"/>
                            <connections>
                                <action selector="callGraphButton:" destination="BYZ-38-t0r" eventType="touchUpInside" id="Kx0-JL-Bv9"/>
                            </connections>
                        </button>
                        <textView clipsSubviews="YES" multipleTouchEnabled="YES" contentMode="scaleToFill" fixedFrame="YES" editable="NO" textAlignment="natural" selectable="NO" translatesAutoresizingMaskIntoConstraints="NO" id="qXW-2z-J7K">
                            <rect key="frame" x="16" y="324" width="343" height="291"/>
                            <autoresizingMask key="autoresizingMask" flexibleMaxX="YES" flexibleMaxY="YES"/>
                            <color key="backgroundColor" white="1" alpha="1" colorSpace="calibratedWhite"/>
                            <accessibility key="accessibilityConfiguration">
                                <accessibilityTraits key="traits" updatesFrequently="YES"/>
                            </accessibility>
                            <fontDescription key="fontDescription" type="system" pointSize="14"/>
                            <textInputTraits key="textInputTraits" autocapitalizationType="sentences"/>
                        </textView>
                        <button opaque="NO" contentMode="scaleToFill" fixedFrame="YES" contentHorizontalAlignment="center" contentVerticalAlignment="center" buttonType="roundedRect" lineBreakMode="middleTruncation" translatesAutoresizingMaskIntoConstraints="NO" id="9u4-b8-vmX">
                            <rect key="frame" x="137" y="138" width="100" height="30"/>
                            <autoresizingMask key="autoresizingMask" flexibleMaxX="YES" flexibleMaxY="YES"/>
                            <state key="normal" title="Sign Out"/>
                            <connections>
                                <action selector="signoutButton:" destination="BYZ-38-t0r" eventType="touchUpInside" id="kZT-P8-0Zy"/>
                            </connections>
                        </button>
                    </subviews>
                    <color key="backgroundColor" red="1" green="1" blue="1" alpha="1" colorSpace="custom" customColorSpace="sRGB"/>
                </view>
                <connections>
                    <outlet property="loggingText" destination="qXW-2z-J7K" id="uqO-Yw-AsK"/>
                    <outlet property="signoutButton" destination="9u4-b8-vmX" id="OCh-qk-ldv"/>
                </connections>
            </viewController>
            <placeholder placeholderIdentifier="IBFirstResponder" id="dkx-z0-nzr" sceneMemberID="firstResponder"/>
        </objects>
        <point key="canvasLocation" x="140" y="137.18140929535232"/>
    </scene>
</scenes>
```
