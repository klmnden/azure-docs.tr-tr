## <a name="customize-and-extend-the-device-management-actions"></a>Cihaz yönetim eylemleri genişletir ve özelleştirme

IOT çözümlerinizi cihaz yönetim modellerini tanımlı bir dizi genişletebilir veya özel düzenleri cihaz ikizi ve bulut-cihaz yöntemi temelleri kullanarak etkinleştirin. Diğer cihaz yönetim eylemleri Fabrika sıfırlaması, üretici yazılımı güncelleştirmesi, yazılım güncelleştirmesi, güç yönetimi, ağ ve bağlantı yönetimi ve veri şifrelemesi örneklerindendir.

## <a name="device-maintenance-windows"></a>Cihaz bakım pencereleri

Genellikle, cihazları teker teker kesintiler ve kapalı kalma süresini en aza indirir eylemleri gerçekleştirmek için yapılandırın. Cihaz bakım pencereleri, bir cihaz yapılandırmasını güncelleştirmeniz gerekir süre tanımlamak için yaygın olarak kullanılan bir desen ' dir. Arka uç çözümlerinizi tanımlayın ve Cihazınızda bir bakım penceresi sağlayan bir ilke etkinleştirmek için cihaz çiftinin istenen özelliklerini kullanabilirsiniz. Bir cihaz, bakım penceresi İlkesi aldığında, bu ilkenin durumunu bildirmek için cihaz çiftinin bildirilen özellik kullanabilirsiniz. Arka uç uygulaması, cihazlar ve her ilke uyumluluğunu gerçekleştiğini doğrulamak için cihaz çifti sorguları daha sonra kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir doğrudan yöntem cihaz üzerinde Uzaktan yeniden başlatma tetiklemek için kullanılır. Bir CİHAZDAN son yeniden başlatma zamanı bildirmek üzere bildirilen özellikleri kullanılan ve buluttan cihazın son yeniden başlatma zamanı bulmak için cihaz ikizi sorgulandı.

IOT Hub ve cihaz yönetim modellerini uzaktan gibi hava üretici yazılımı güncelleştirme kullanmaya başlama devam etmek için bkz:

[Öğretici: nasıl üretici yazılımlarını güncelleştirme][lnk-fwupdate]

IOT çözümü ve zamanlama yöntemi çağıran birden çok cihazda genişletmek öğrenmek için bkz [işleri zamanlama ve yayınlama] [ lnk-tutorial-jobs] öğretici.

IOT Hub kullanmaya başlamaya devam etmek için bkz: [IOT Edge'i kullanmaya başlama][lnk-iot-edge].

[lnk-fwupdate]: ../articles/iot-hub/tutorial-firmware-update.md
[lnk-tutorial-jobs]: ../articles/iot-hub/iot-hub-node-node-schedule-jobs.md
[lnk-iot-edge]: ../articles/iot-edge/tutorial-simulate-device-linux.md