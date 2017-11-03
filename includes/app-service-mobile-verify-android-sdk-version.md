Devam eden geliştirme nedeniyle Android Studio'da yüklü Android SDK sürümü kodu sürümünde eşleşmeyebilir. Bu öğreticide başvurulan Android SDK 23 yazma zaman en son sürümüdür. Sürüm numarası, SDK'sının yeni sürümler görünür ve kullanılabilir en son sürümünü kullanmanızı öneririz artırabilir.

Sürüm uyuşmazlığı iki belirtileri şunlardır:

- Derleme ya da projeyi oluşturmak gibi Gradle hata iletileri alabilirsiniz "**hedef Google Inc.:Google APIs:n bulunamadı**".
- Standart Android nesneleri çözümlenmelidir kod temelinde `import` ifadeleri hata iletileri oluşturur.

Bunlardan görünürse, Android Studio'da yüklü Android SDK sürümünü indirilen projedeki SDK hedefinin eşleşmeyebilir. Sürüm numarasını doğrulamak için aşağıdaki değişiklikleri yapın:

1. Android Studio'da sırasıyla **Araçları** > **Android** > **SDK Manager**. SDK Platform en son sürümü yüklü değilse, daha sonra yüklemek için tıklayın. Sürüm numarasını not edin.
2. Üzerinde **Proje Gezgini** sekmesinde, altında **Gradle betikleri**, dosyayı açma **build.gradle (modeule: uygulama)**. Emin **compileSdkVersion** ve **buildToolsVersion** son SDK sürüm olarak ayarlanır. Etiketler şuna benzeyebilir:

             compileSdkVersion 'Google Inc.:Google APIs:23'
            buildToolsVersion "23.0.2"
3. Android Studio Proje Gezgini'nde, proje düğümüne sağ tıklayın, seçin **özellikleri**, sol sütunda seçin **Android**. Emin **proje derleme hedefi** aynı SDK sürümü ayarlanmış **targetSdkVersion**.

Android Studio'da bildirim dosyası artık hedef SDK ve Eclipse durumuyla aksine en düşük SDK sürümü belirtmek için kullanılır.
