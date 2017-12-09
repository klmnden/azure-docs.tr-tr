
## <a name="set-up-your-project"></a>Projenizin kurulumunu

> Bunun yerine bu örneği ait Android Studio projesi indirmeyi tercih ediyorsunuz? [Bir proje indirme](https://github.com/Azure-Samples/active-directory-android-native-v2/archive/master.zip) ve geçin [yapılandırma adımı](#create-an-application-express) kod örneği çalıştırmadan önce yapılandırmak için.


### <a name="create-a-new-project"></a>Yeni bir proje oluşturma 
1.  Android Studio'yu açın, gidin:`File` > `New` > `New Project`
2.  Uygulamanızı adlandırın ve'ı tıklatın`Next`
3.  Seçtiğinizden emin olun *API 21 ya da daha yeni (Android 5.0)* ve'ı tıklatın`Next`
4.  Bırakın `Empty Activity`, tıklatın `Next`, ardından`Finish`


### <a name="add-the-microsoft-authentication-library-msal-to-your-project"></a>Microsoft kimlik doğrulama kitaplığı (MSAL) projenize ekleyin
1.  Android Studio'da gidin:`Gradle Scripts` > `build.gradle (Module: app)`
2.  Altında aşağıdaki kodu kopyalayıp yapıştırın `Dependencies`:

```ruby  
compile ('com.microsoft.identity.client:msal:0.1.+') {
    exclude group: 'com.android.support', module: 'appcompat-v7'
}
compile 'com.android.volley:volley:1.0.0'
```

<!--start-collapse-->
### <a name="about-this-package"></a>Bu paketi hakkında

Yukarıdaki paket Microsoft kimlik doğrulama kitaplığı (MSAL) yükler. MSAL alınırken, önbelleğe alma ve Azure Active Directory v2 bitiş noktası tarafından korunan API'leri erişmek için kullanılan kullanıcı belirteçleri yenilemeyi işler.
<!--end-collapse-->

## <a name="create-your-applications-ui"></a>Uygulamanızın kullanıcı Arabirimi oluşturma

1.  Açık: `activity_main.xml` altında`res` > `layout`
2.  Etkinlik düzeninden değiştirmek `android.support.constraint.ConstraintLayout` veya için diğer`LinearLayout`
3.  Ekleme `android:orientation="vertical"` özelliğine `LinearLayout` düğümü
4.  Aşağıdaki kodu kopyalayıp `LinearLayout` düğüm, geçerli içerik değiştirme:

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

