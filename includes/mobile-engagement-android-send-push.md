
###Bildirimleri etkinleştirmek için bildirim dosyasını güncelleştirme

Aşağıdaki uygulama içi mesajlaşma kaynaklarını aşağıdaki Manifest.xml dosyasında `<application>` ve `</application>` etiketleri arasına kopyalayın.

        <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementTextAnnouncementActivity" android:theme="@android:style/Theme.Light" android:exported="false">
            <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="android.intent.category.DEFAULT" />
                <data android:mimeType="text/plain" />
            </intent-filter>
        </activity>
        <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity" android:theme="@android:style/Theme.Light" android:exported="false">
            <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ANNOUNCEMENT"/>
                <category android:name="android.intent.category.DEFAULT" />
                <data android:mimeType="text/html" />
            </intent-filter>
        </activity>
        <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementPollActivity" android:theme="@android:style/Theme.Light" android:exported="false">
            <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.POLL"/>
                <category android:name="android.intent.category.DEFAULT" />
            </intent-filter>
        </activity>
        <activity android:name="com.microsoft.azure.engagement.reach.activity.EngagementLoadingActivity" android:theme="@android:style/Theme.Dialog" android:exported="false">
            <intent-filter>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.LOADING"/>
                <category android:name="android.intent.category.DEFAULT"/>
            </intent-filter>
        </activity>
        <receiver android:name="com.microsoft.azure.engagement.reach.EngagementReachReceiver" android:exported="false">
            <intent-filter>
                <action android:name="android.intent.action.BOOT_COMPLETED"/>
                <action android:name="com.microsoft.azure.engagement.intent.action.AGENT_CREATED"/>
                <action android:name="com.microsoft.azure.engagement.intent.action.MESSAGE"/>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.ACTION_NOTIFICATION"/>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.EXIT_NOTIFICATION"/>
                <action android:name="com.microsoft.azure.engagement.reach.intent.action.DOWNLOAD_TIMEOUT"/>
            </intent-filter>
        </receiver>
        <receiver android:name="com.microsoft.azure.engagement.reach.EngagementReachDownloadReceiver">
            <intent-filter>
                <action android:name="android.intent.action.DOWNLOAD_COMPLETE"/>
            </intent-filter>
        </receiver>

###Bildirimler için simge belirtme

Manifest.xml dosyanızda yer alan aşağıdaki XML parçacığını `<application>` ve `</application>` etiketleri arasına yapıştırın.

        <meta-data android:name="engagement:reach:notification:icon" android:value="engagement_close"/>

Böylece simgenin hem sistemde, hem de uygulama içi bildirimlerde görüntüleneceği belirtilir. Bu, uygulama içi bildirimler için isteğe bağlı olsa da, sistem bildirimleri için zorunludur. Android, geçersiz simgelere sahip sistem bildirimlerini reddeder.

**drawable** klasörlerinden birinde bulunan simgeleri kullandığınızdan emin olun (örneğin, ``engagement_close.png``). **mipmap** klasörü desteklenmez.

>[AZURE.NOTE] **Başlatıcı** simgesi kullanılamaz. Bunun farklı bir çözünürlüğü vardır ve çoğunlukla desteklemediğimiz mipmap klasörlerinde yer alır.

Gerçek uygulamalar için, her[Android tasarım yönergesi](http://developer.android.com/design/patterns/notifications.html) için bir bildirime uygun simgeleri kullanabilirsiniz.

>[AZURE.TIP] Doğru simge çözünürlüğü kullandığınızdan emin olmak için [bu örneklere](https://www.google.com/design/icons) bakabilirsiniz.
Kayarak **Bildirim** bölümüne gidin, simgeye tıklayın ve ardından simge çizilebilir kümesini indirmek için `PNGS` öğesine tıklayın. Simgenin her sürümü için hangi drawable klasörlerde hangi çözünürlüğün kullanıldığını da görebilirsiniz.

###GCM anında iletme bildirimlerini almak için uygulamanızı etkinleştirme

1. Aşağıdaki kodu, Google Play konsolundan alınan `project number` değiştirildikten sonra, Manifest.xml dosyanızda `<application>` ve `</application>` etiketleri arasına yapıştırın. \n kasıtlı olarak konmuştur; bu nedenle proje numarasını bununla bitirdiğinizden emin olun.

        <meta-data android:name="engagement:gcm:sender" android:value="************\n" />

2. Aşağıdaki kodu Manifest.xml dosyanızda `<application>` ve `</application>` etiketleri arasına yapıştırın. <Your package name> paket adını değiştirin.

        <receiver android:name="com.microsoft.azure.engagement.gcm.EngagementGCMEnabler"
        android:exported="false">
            <intent-filter>
                <action android:name="com.microsoft.azure.engagement.intent.action.APPID_GOT" />
            </intent-filter>
        </receiver>

        <receiver android:name="com.microsoft.azure.engagement.gcm.EngagementGCMReceiver" android:permission="com.google.android.c2dm.permission.SEND">
            <intent-filter>
                <action android:name="com.google.android.c2dm.intent.REGISTRATION" />
                <action android:name="com.google.android.c2dm.intent.RECEIVE" />
                <category android:name="<Your package name>" />
            </intent-filter>
        </receiver>

3. `<application>` etiketinin önünde vurgulanan son izin kümesini ekleyin. `<Your package name>` öğesini uygulamanızın asıl paket adıyla değiştirin.

        <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
        <uses-permission android:name="<Your package name>.permission.C2D_MESSAGE" />
        <permission android:name="<Your package name>.permission.C2D_MESSAGE" android:protectionLevel="signature" />






<!--HONumber=Sep16_HO3-->


