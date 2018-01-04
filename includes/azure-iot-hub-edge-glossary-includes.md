## <a name="iot-edge"></a>IoT Edge
Azure IOT kenar bulut tabanlı dağıtım Azure hizmetlerinin ve şirket içi cihazlar için çözüm özgü kodu sağlar. Verileri buluta gönderilmeden önce IOT sınır cihazları diğer bilgi işlem gerçekleştirmek için cihazlar ve analizi veri toplayabilirsiniz. Daha fazla bilgi için lütfen bkz [Azure IOT kenar](https://docs.microsoft.com/en-us/azure/iot-edge/).

## <a name="iot-edge-agent"></a>IOT kenar Aracısı
IOT kenar çalışma zamanı dağıtma ve modülleri izleme sorumlu parçası.

## <a name="iot-edge-device"></a>IoT Edge cihazı
IOT sınır cihazları IOT çalışma zamanı yüklü ve cihaz Ayrıntıları "IOT sınır cihazı" olarak işaretlenen kenar sahip. Bilgi edinmek için nasıl [Linux sanal bir cihaz üzerinde Azure IOT kenar dağıtma - Önizleme](https://docs.microsoft.com/en-us/azure/iot-edge/tutorial-simulate-device-linux).

## <a name="iot-edge-deployment"></a>IOT kenar dağıtım
Bir IOT kenar dağıtım modülleri IOT kenar kümesini çalıştırmak için IOT sınır cihazları hedef kümesini yapılandırır. Her dağıtım, hedef durumu eşleşen tüm aygıtları modülleri belirtilen kümesi çalıştıran, bile yeni aygıtları oluşturulan ya da hedef durumu eşleşecek şekilde değiştirilmiş sürekli olarak sağlar. Her IOT sınır cihazı, yalnızca hedef durumu karşıladığından en yüksek öncelikli dağıtım alır. Daha fazla bilgi edinmek [IOT kenar dağıtım](https://docs.microsoft.com/en-us/azure/iot-edge/module-deployment-monitoring).

## <a name="iot-edge-deployment-manifest"></a>IOT kenar dağıtım bildirimi
Modüller, yolları ve ilişkili modül kümesini dağıtmak için bir veya daha fazla IOT sınır cihazları modülü twin(s) içinde kopyalanacak bilgileri içeren bir Json belgesi özelliklerini istenen.

## <a name="iot-edge-gateway-device"></a>IOT sınır ağ geçidi cihazı
Aşağı Akış cihaz sahip bir IOT kenar cihazı. Aşağı Akış cihaz IOT kenar veya IOT sınır cihazı olabilir.

## <a name="iot-edge-hub"></a>IOT kenar hub
IOT kenar çalışma zamanı (doğru IOT Hub) Yukarı Akış ve aşağı akış (çıktığınızda, IOT Hub) modül için modülü iletişim sorumlu parçası iletişim. 

## <a name="iot-edge-leaf-device"></a>IOT sınır yaprak cihazı
IOT sınır cihazı ile aşağı akış aygıtı yok. 

## <a name="iot-edge-module"></a>IOT kenar Modülü
IOT kenar cihazlara dağıtabileceğiniz bir Docker kapsayıcısı bir IOT kenar modülüdür. Bir CİHAZDAN bir ileti alma, bir ileti dönüştürme ya da bir IOT hub'ına ileti gönderme gibi belirli bir görevi gerçekleştirir. Diğer modüller ile iletişim kurar ve IOT kenar çalışma zamanına verileri gönderir. [IOT kenar modülleri geliştirmek için Araçlar ve gereksinimleri anlamanız](https://docs.microsoft.com/en-us/azure/iot-edge/module-development).

## <a name="iot-edge-module-identity"></a>IOT kenar modül kimliği
Bir sınır hub veya IOT Hub ile kimlik doğrulaması için bir modül tarafından kullanılacak varlığı ve güvenlik kimlik bilgileri ayrıntılı IOT hub'ı modülü kimlik kayıt defteri kaydında.

## <a name="iot-edge-module-image"></a>IOT kenar modülü görüntüsü
IOT kenar çalışma zamanı tarafından modülü örnekleri örneği oluşturmak için kullanılan docker resim.

## <a name="iot-edge-module-twin"></a>IOT kenar modülü twin
Bir modül örneğinin durumu bilgilerini depolar IOT Hub'ında bir Json belgesi kalıcı.

## <a name="iot-edge-priority"></a>IOT kenar önceliği
İki IOT kenar dağıtımları aynı aygıt hedeflediğinizde, yüksek öncelikli dağıtım uygulanmış olur. İki dağıtım aynı önceliğe sahip, dağıtımı daha sonraki oluşturulma tarihi ile uygulanan. Daha fazla bilgi edinmek [öncelik](https://docs.microsoft.com/en-us/azure/iot-edge/module-deployment-monitoring#priority).

## <a name="iot-edge-runtime"></a>IoT Edge çalışma zamanı
IOT kenar çalışma zamanı IOT kenar cihaza yüklenmesi için Microsoft dağıtır her şeyi içerir. Edge Aracısı, kenar hub ve kenar CTL aracı dahil.

## <a name="iot-edge-set-modules-to-a-single-device"></a>IOT kenar modülleri için tek bir cihazı ayarlayın.
Bir cihaz üzerinde bir IOT kenar bildiriminin içeriği kopyalar bir işlem ' modülü çifti. Genel temel API'dir 'yapılandırma apply' yalnızca aldığı IOT kenar bildiriminde bir girdi olarak.

## <a name="iot-edge-target-condition"></a>IOT kenar hedef durumu
Bir IOT kenar dağıtımında hedef durumu dağıtımın hedef cihazlar örneğin seçmek için cihaz çiftlerini etiketleri hakkında herhangi Boolean durumdur "tag.environment = prod". Hedef durumu gereksinimlerini karşılayan yeni aygıtları dahil etmek veya artık yapmak aygıtları kaldırmak için sürekli olarak değerlendirilir. Daha fazla bilgi edinmek [hedef koşulu](https://docs.microsoft.com/en-us/azure/iot-edge/module-deployment-monitoring#target-condition)