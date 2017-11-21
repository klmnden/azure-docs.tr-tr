Devam eden geliştirme nedeniyle Android Studio'da yüklü Android SDK sürümü kodu sürümünde eşleşmeyebilir. Bu öğreticide başvurulan Android SDK 26, yazma zaman en son sürümüdür. Sürüm numarası, SDK'sının yeni sürümler görünür ve kullanılabilir en son sürümünü kullanmanızı öneririz artırabilir.

Sürüm uyuşmazlığı iki belirtileri şunlardır:

- Derleme ya da projeyi oluşturmak gibi Gradle hata iletileri alabilirsiniz `Gradle sync failed: Failed to find target with hash string 'android-XX'`.
- Standart Android nesneleri çözümlenmelidir kod temelinde `import` ifadeleri hata iletileri oluşturur.

Bunlardan görünürse, Android Studio'da yüklü Android SDK sürümünü indirilen projedeki SDK hedefinin eşleşmeyebilir. Sürüm numarasını doğrulamak için aşağıdaki değişiklikleri yapın:

1. Android Studio'da sırasıyla **Araçları** > **Android** > **SDK Manager**. SDK Platform en son sürümü yüklü değilse, daha sonra yüklemek için tıklayın. Sürüm numarasını not edin.

2. Üzerinde **Proje Gezgini** sekmesinde, altında **Gradle komut dosyaları**, dosyayı açma **build.gradle (modül: uygulama)**. Emin **compileSdkVersion** ve **targetSdkVersion** son SDK sürüm olarak ayarlanır. `build.gradle` Şuna benzeyebilir:

    ```gradle
    android {
        compileSdkVersion 26
        defaultConfig {
            targetSdkVersion 26
        }
    }
    ```
