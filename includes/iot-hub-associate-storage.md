## <a name="associate-an-azure-storage-account-to-iot-hub"></a>Bir Azure Storage hesabı IOT Hub'ına ilişkilendirme

Sanal cihaz uygulamasının bir blob için bir dosya yükler için bilmeniz gereken bir [Azure Storage](../articles/storage/common/storage-create-storage-account.md#create-a-storage-account) IOT Hub'ına ilişkili hesap. Bir Azure depolama hesabı bir IOT hub ile ilişkilendirdiğinizde, IOT hub'ı bir SAS URI'sini oluşturur. Bir aygıt bu SAS URI'sini güvenli bir şekilde bir blob kapsayıcısına bir dosyayı karşıya yüklemek için kullanabilirsiniz. IOT Hub hizmeti ve cihaz SDK'ları SAS URI'sini oluşturur ve bir dosyayı karşıya yüklemek için kullanılacak bir cihaz için kullanılabilir hale getirir işlem koordinatı.

' Ndaki yönergeleri izleyin [yapılandırma dosya yüklemeleri Azure portalını kullanarak](../articles/iot-hub/iot-hub-configure-file-upload.md) bir Azure Storage hesabı IOT hub'ınıza ilişkilendirilecek. Bir blob kapsayıcısını, IOT hub ile ilişkili olduğunu ve dosya bildirimlerini etkin olduğundan emin olun.

![Portalda dosya bildirimlerini etkinleştir](media/iot-hub-associate-storage/enable-file-notifications.png)