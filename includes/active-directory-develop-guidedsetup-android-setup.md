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
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/13/2018
ms.author: dadobali
ms.custom: include file
ms.openlocfilehash: f55bdc774437d280db51fb69f060bea7dc579a26
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60250564"
---
## <a name="set-up-your-project"></a>Projenizi ayarlama

Bunun yerine bu örneği ait Android Studio projeyi indirmek istiyor musunuz? [Bir projeyi indirirken](https://github.com/Azure-Samples/active-directory-android-native-v2/archive/master.zip)ve atlamak [yapılandırma adımı](#register-your-application) yürütmeden önce onun kod örneği yapılandırmak için.

### <a name="create-a-new-project"></a>Yeni bir proje oluşturma

1. Android Studio'yu açın ve ardından **dosya** > **yeni** > **yeni proje**.
2. Uygulamanızı adlandırın ve ardından **sonraki**.
3. Seçin **API 21 ya da daha yeni (Android 5.0)** ve ardından **sonraki**.
4. Bırakın **boş etkinlik** olduğu gibi seçin **sonraki**ve ardından **son**.

### <a name="add-msal-to-your-project"></a>MSAL projenize ekleyin.

1. Android Studio'da **Gradle betiklerini** > **build.gradle (modül: uygulama)**.
2. Altında **bağımlılıkları**, aşağıdaki kodu yapıştırın:

    ```gradle  
    compile ('com.microsoft.identity.client:msal:0.1.+') {
        exclude group: 'com.android.support', module: 'appcompat-v7'
    }
    compile 'com.android.volley:volley:1.0.0'
    ```

<!--start-collapse-->
### <a name="about-this-package"></a>Bu paket hakkında

Önceki kodda paketi alınırken, önbelleğe alma, yenileme, silme gibi tüm belirteç işlemleri işleyen Microsoft kimlik doğrulama Kitaplığı'ne (MSAL) yükler. Belirteç Microsoft kimlik platformu tarafından korunan API'lerine erişmek için gereklidir.
<!--end-collapse-->

## <a name="create-the-apps-ui"></a>Uygulamanın kullanıcı Arabirimi oluşturma

1. Git **res** > **Düzen**ve ardından açın **activity_main.xml**.
2. Etkinlik düzenden değiştirme `android.support.constraint.ConstraintLayout` veya için diğer `LinearLayout`.
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
