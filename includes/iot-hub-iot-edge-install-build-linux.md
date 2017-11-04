## <a name="install-the-prerequisites"></a>Yükleme önkoşulları

Bu öğreticideki adımlardan Ubuntu Linux çalıştırdığınızı varsayın.

Önkoşul yüklemek için bir Kabuğu'nu açın ve aşağıdaki komutları çalıştırın:

```bash
sudo apt-get update
sudo apt-get install curl build-essential libcurl4-openssl-dev git cmake pkg-config libssl-dev uuid-dev valgrind libglib2.0-dev libtool autoconf
```

Kabuğu'nda yerel makinenize Azure IOT kenar GitHub deposuna kopyalamak için aşağıdaki komutu çalıştırın:

```bash
git clone https://github.com/Azure/iot-edge.git
```

## <a name="how-to-build-the-sample"></a>Örnek oluşturma

Artık IOT kenar çalışma zamanı ve örnekleri yerel makinenizde oluşturabilirsiniz:

1. Bir kabuk açın.

1. **iot-edge** deposunun yerel kopyasındaki kök klasöre gidin.

1. Yapı betiği aşağıdaki gibi çalıştırın:

    ```sh
    tools/build.sh --disable-native-remote-modules
    ```

Bu betik **cmake** yardımcı programını kullanarak **iot-edge** deposu yerel kopyasının kök klasöründe **build** adlı bir klasör ve bir derleme görevleri dosyası oluşturur. Betik daha sonra çözümü derler ve birim testleri ile uçtan uca testleri atlar. İstiyorsanız derleme ve birim testleri çalıştırma, ekleme `--run-unittests` parametresi. Derleme ve uçtan uca testler, eklemek istiyorsanız, `--run-e2e-tests`.

> [!NOTE]
> **build.sh** betiğini her çalıştırdığınızda betik **iot-edge** deposu yerel kopyasının kök klasöründe bulunan **build** klasörünü siler ve yeniden oluşturur.