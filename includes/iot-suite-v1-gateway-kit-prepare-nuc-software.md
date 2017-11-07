## <a name="build-iot-edge"></a>IOT kenar derleme

Bu öğretici özel IOT kenar modüller Uzaktan izleme önceden yapılandırılmış çözümü ile iletişim kurmak için kullanır. Bu nedenle, özel kaynak kodu modüllerden IOT kenar yapı gerekir. Aşağıdaki bölümlerde IOT kenar yüklemek ve özel IOT kenar modülü oluşturmak nasıl açıklanmaktadır.

### <a name="install-iot-edge"></a>IOT kenar yükleyin

Aşağıdaki adımlar, Intel NUC üzerinde önceden derlenmiş IOT kenar yazılımı yüklemek açıklanmaktadır:

1. Gerekli akıllı paket depoları Intel NUC üzerinde aşağıdaki komutları çalıştırarak yapılandırın:

    ```bash
    smart channel --add IoT_Cloud type=rpm-md name="IoT_Cloud" baseurl=http://iotdk.intel.com/repos/iot-cloud/wrlinux7/rcpl13/ -y
    smart channel --add WR_Repo type=rpm-md baseurl=https://distro.windriver.com/release/idp-3-xt/public_feeds/WR-IDP-3-XT-Intel-Baytrail-public-repo/RCPL13/corei7_64/
    ```

    Girin `y` zaman komutu ister **bu kanal içerir?**.

1. Aşağıdaki komutu çalıştırarak akıllı Paket Yöneticisi güncelleştirin:

    ```bash
    smart update
    ```

1. Azure IOT kenar paket, aşağıdaki komutu çalıştırarak yükleyin:

    ```bash
    smart config --set rpm-check-signatures=false
    smart install packagegroup-cloud-azure -y
    ```

1. "Hello world" örnek çalıştırarak yüklemeyi doğrulayın. Bu örnek bir hello world iletisini her beş saniyede log.txT dosyasına yazar. "Hello world" örnek aşağıdaki komutları çalıştırın:

    ```bash
    cd /usr/share/azureiotgatewaysdk/samples/hello_world/
    ./hello_world hello_world.json
    ```

    Yoksay **geçersiz bağımsız değişken** örnek durdurduğunuzda iletileri.

    Günlük dosyasının içeriğini görüntülemek için aşağıdaki komutu kullanın:

    ```bash
    cat log.txt | more
    ```

### <a name="troubleshooting"></a>Sorun giderme

"Hiçbir paket kul linux geliştirme sağlar" hatasını alırsanız, Intel NUC yeniden başlatmayı deneyin.
