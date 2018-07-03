## <a name="associate-an-azure-storage-account-to-iot-hub"></a>IOT hub'ı bir Azure depolama hesabına ilişkilendirin

Sanal cihaz uygulaması, bir dosyayı bloba yükler. çünkü olmalıdır bir [Azure depolama](../articles/storage/common/storage-create-storage-account.md#create-a-storage-account) IOT Hub'ına ilişkili hesabı. Bir Azure depolama hesabı bir IOT hub ile ilişkilendirdiğinizde, IOT hub'ı SAS URI'sini oluşturur. Bir cihaz bu SAS URI'sini güvenli bir şekilde bir blob kapsayıcısına bir dosyayı karşıya yüklemek için kullanabilirsiniz. IOT Hub hizmeti ve cihaz SDK'ları SAS URI'sini oluşturur ve bir dosyayı karşıya yüklemek için kullanılacak bir cihaz için kullanılabilir hale getirir işlemi koordine edin.

Bölümündeki yönergeleri [yapılandırma dosyasını karşıya yükler Azure portalını kullanarak](../articles/iot-hub/iot-hub-configure-file-upload.md) IOT hub'ınıza bir Azure depolama hesabı ilişkilendirilecek. Bir blob kapsayıcısı, IOT hub'ı ile ilişkili olduğunu ve dosya bildirimlerini etkinleştirildiğinden emin olun.

![Portalda dosya bildirimlerini etkinleştirme](media/iot-hub-associate-storage/enable-file-notifications.png)