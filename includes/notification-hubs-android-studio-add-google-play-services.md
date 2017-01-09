1. Android Studio’nun araç çubuğundaki simgeye tıklayarak ya da menüde **Araçlar** -> **Android** -> **SDK Manager** ’a tıklayarak Android SDK Manager’ı açın. Projenizde kullanılan hedef Android SDK sürümünü bulup, **Paket Ayrıntılarını Göster** ’e tıklayarak bunu açın ve henüz yüklenmemişse **Google API'ler** ’i seçin.
2. **SDK Araçları** sekmesine tıklayın. Google Play Hizmeti’ni zaten yüklemediyseniz **Google Play Hizmetleri** ’ne, aşağıda gösterildiği gibi tıklayın. Ardından, yüklemek için **Uygula** ’ya tıklayın. 
   
    SDK yolunun sonraki bir adım için olduğunu unutmayın. 
   
       ![](./media/notification-hubs-android-studio-add-google-play-services/notification-hubs-android-studio-sdk-manager.png)
3. Uygulama dizinindeki **build.gradle** dosyasını açın.
   
    ![](./media/notification-hubs-android-studio-add-google-play-services/notification-hubs-android-studio-add-google-play-dependency.png)
4. *Bağımlılıklar* ’ın altına bu satırı ekleyin: 
   
           compile 'com.google.android.gms:play-services-gcm:9.2.0'
5. Araç çubuğundaki **Projeyi Gradle Dosyalarıyla Eşitle** simgesine tıklayın.
6. **AndroidManifest.xml** dosyasını açın ve bu etiketi *uygulama* etiketine ekleyin.
   
        <meta-data android:name="com.google.android.gms.version"
            android:value="@integer/google_play_services_version" />



<!--HONumber=Jan17_HO1-->


