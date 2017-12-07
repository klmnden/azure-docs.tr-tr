## <a name="customize-and-extend-the-device-management-actions"></a>Özelleştirme ve aygıt yönetimi eylemleri genişletme

IOT çözümlerinizi aygıt yönetimi desenleri tanımlı kümesini genişletin veya özel desenler cihaz çifti ve bulut-cihaz yöntemi temelleri kullanarak etkinleştirin. Aygıt Yönetimi eylemleri diğer örnekleri Fabrika sıfırlaması, ürün yazılımı güncelleştirmesi, yazılım güncelleştirmesi, güç yönetimi, ağ ve bağlantı yönetimi ve veri şifrelemesi içerir.

## <a name="device-maintenance-windows"></a>Cihaz bakım pencereleri

Genellikle, cihazların kesintiler ve kapalı kalma süresini en aza indiren aynı anda eylemleri gerçekleştirmek üzere yapılandırın. Cihaz bakım pencereleri zaman bir aygıt yapılandırmasını güncelleştirmeniz gerekir zaman tanımlamak için yaygın olarak kullanılan bir desen ' dir. Arka uç çözümlerinizi cihaz çifti istenen özelliklerini tanımlamak ve bir ilke bir bakım penceresi sağlar, Cihazınızda etkinleştirmek için kullanabilirsiniz. Bir aygıt bakım penceresi İlkesi aldığında, bu ilkenin durumunu bildirmek için cihaz çiftinin bildirilen özelliğini kullanabilirsiniz. Arka uç uygulama için cihazlar ve her ilke uyumluluğunu şifreli olarak cihaz çiftine sorguları daha sonra kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir cihazda Uzaktan yeniden başlatma tetiklemek için doğrudan bir yöntem kullanılır. Son yeniden başlatma zamanı aygıttan raporlamak için bildirilen özellikler kullanılan ve buluttan cihaza son yeniden başlatma zamanı bulmak için cihaz çifti sorgulanan.

IOT Hub ve Uzaktan gibi cihaz yönetim düzenleri hava bellenim güncelleştirme kullanmaya Başlarken devam etmek için bkz:

[Öğretici: bir üretici yazılımı güncelleştirmesi yapma][lnk-fwupdate]

Çözüm ve zamanlama yöntemini çağıran birden fazla cihazda, IOT genişletmek öğrenmek için bkz: [zamanlama ve yayın işleri] [ lnk-tutorial-jobs] Öğreticisi.

IOT Hub ile çalışmaya başlama devam etmek için bkz: [IOT Edge ile çalışmaya başlama][lnk-iot-edge].

[lnk-fwupdate]: ../articles/iot-hub/iot-hub-node-node-firmware-update.md
[lnk-tutorial-jobs]: ../articles/iot-hub/iot-hub-node-node-schedule-jobs.md
[lnk-iot-edge]: ../articles/iot-edge/tutorial-simulate-device-linux.md