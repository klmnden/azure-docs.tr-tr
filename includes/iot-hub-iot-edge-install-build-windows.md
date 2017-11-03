## <a name="install-the-prerequisites"></a>Yükleme önkoşulları

1. Yükleme [Visual Studio 2015 veya 2017](https://www.visualstudio.com). Lisans gereksinimlerini karşılıyorsa, ücretsiz Community sürümü kullanabilirsiniz. Visual C++ ve NuGet Paket Yöneticisi eklediğinizden emin olun.

1. Yükleme [git](http://www.git-scm.com) ve git.exe komut satırından çalıştırabilir emin olun.

1. Yükleme [CMake](https://cmake.org/download/) ve cmake.exe komut satırından çalıştırabilir emin olun. CMake sürüm 3.7.2 veya üzeri önerilir. **.Msi** yükleyicisi, Windows'da kolay seçeneği bulunur. En az CMake yolu Ekle yükleyici istediğinde geçerli kullanıcı.

1. Yükleme [Python 2.7](https://www.python.org/downloads/release/python-27). Python için eklemenize emin olun, `PATH` ortam değişkeni. Git **Denetim Masası** > **sistem ve güvenlik** > **sistem** > **Gelişmiş Sistem ayarları**  >  **Ortam değişkenleri**. Ekleme `C:\Python27` yolunuz için. 

1. Bir komut isteminde yerel makinenize Azure IOT kenar GitHub deposuna kopyalamak için aşağıdaki komutu çalıştırın:

    ```cmd
    git clone https://github.com/Azure/iot-edge.git
    ```

## <a name="how-to-build-the-sample"></a>Örnek oluşturma

Artık IOT kenar çalışma zamanı ve örnekleri yerel makinenizde oluşturabilirsiniz:

1. Açık **VS 2015 için geliştirici komut istemi** veya **VS 2017 için geliştirici komut istemi**sürümünüze bağlı olarak.

1. **iot-edge** deposunun yerel kopyasındaki kök klasöre gidin.

1. Yapı betiği aşağıdaki gibi çalıştırın:

    ```cmd
    tools\build.cmd --disable-native-remote-modules
    ```

Bu komut dosyasını bir Visual Studio çözümü dosyası oluşturur ve çözüm oluşturur. Visual Studio çözümünde bulabilirsiniz **yapı** yerel kopyasını klasöründe **IOT kenar** deposu. İstiyorsanız derleme ve birim testleri çalıştırma, ekleme `--run-unittests` parametresi. Derleme ve uçtan uca testler, eklemek istiyorsanız, `--run-e2e-tests`.

> [!NOTE]
> Her çalıştırdığınızda **build.cmd** komut dosyası, onu siler ve sonra yeniden oluşturur **yapı** klasörü kök klasöründe yerel kopyasının **IOT kenar** deposu.