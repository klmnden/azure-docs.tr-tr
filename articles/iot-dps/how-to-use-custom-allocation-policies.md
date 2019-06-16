---
title: Azure IOT Hub cihazı sağlama hizmeti ile özel ayırma ilkelerini kullanma | Microsoft Docs
description: Azure IOT Hub cihazı sağlama hizmeti ile özel ayırma ilkelerini kullanma
author: wesmc7777
ms.author: wesmc
ms.date: 04/10/2019
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
manager: philmea
ms.openlocfilehash: 03d39ed01907a2ad61e089946673b96b8a2cc83e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65916882"
---
# <a name="how-to-use-custom-allocation-policies"></a>Özel ayırma ilkelerini kullanma


Bir özel ayırma ilkesi, cihazların bir IOT hub'ına nasıl atanacağını üzerinde daha fazla denetim sağlar. Bu özel kodda kullanılarak başarılır bir [Azure işlevi](../azure-functions/functions-overview.md) cihazların bir IOT hub'ına atamak için. Cihaz sağlama hizmeti, cihaz ve kayıt ilgili tüm bilgileri sağlayan Azure işlevi kodu çağırır. İşlev kodunuzu çalıştırılır ve cihaz sağlama için kullanılan IOT hub bilgilerini döndürür.

Özel ayırma ilkelerini kullanarak cihaz sağlama hizmeti tarafından sağlanan ilkeleri, senaryonuzun gereksinimlerini karşılamıyor, kendi ayırma ilkeleri tanımlarsınız.

Örneğin, belki de, bir cihaz sağlama sırasında kullanarak sertifikayı incelemek istediğiniz ve bir sertifika özelliğine dayalı bir IOT hub cihaz atayın. Belki de cihazlarınız için bir veritabanında saklanan bilgilere sahip ve hangi IOT hub'a bir cihaz atanması gerektiğini belirlemek için veritabanını sorgulamak gerekir.


Bu makalede, C# dilinde yazılmış bir Azure işlevi kullanarak bir özel ayırma ilkesi gösterilmektedir. İki yeni IOT hub'ları oluşturulan temsil eden bir *Contoso Tebrikleri bölme* ve *Contoso ısı Pompalara bölme*. Sağlama isteyen cihazlar sağlamak için kabul edilmesi için bir kayıt kimliği şu son ekleri birine sahip olmalıdır:

- **contoso - tstrsd 007**: Contoso Tebrikleri bölme
- **contoso - hpsd 088**: Contoso ısı Pompalara bölme

Cihazları bir kayıt kimliği. Bu gerekli eklerinde göre sağlanacak. Bu cihazlar dahil sağlama bir örnek kullanarak simulated [Azure IOT C SDK'sı](https://github.com/Azure/azure-iot-sdk-c). 

Bu makalede aşağıdaki adımları gerçekleştireceksiniz:

* İki Contoso bölme IOT hub'ları oluşturmak için Azure CLI'yi kullanma (**Contoso Tebrikleri bölme** ve **Contoso ısı Pompalara bölme**)
* Bir Azure işlevi için özel ayırma ilkesini kullanarak yeni bir grup kaydı oluşturma
* İki cihaz simülasyonu için cihaz anahtarları oluşturun.
* İçin Azure IOT C SDK'sı geliştirme ortamını ayarlama
* Örnek kod özel ayırma ilkesinin göre sağlanır görmek için cihazların benzetimini yapma


[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Önkoşullar

* Tamamlanmasından [IOT Hub cihazı sağlama hizmetini Azure portalıyla ayarlama](./quick-setup-auto-provision.md) hızlı başlangıç.
* [Visual Studio](https://visualstudio.microsoft.com/vs/) 2015 veya üzeri ile [' ile masaüstü geliştirme C++'](https://www.visualstudio.com/vs/support/selecting-workloads-visual-studio-2017/) iş yükünün etkinleştirilmiş.
* [Git](https://git-scm.com/download/)'in en son sürümünün yüklemesi.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="create-two-divisional-iot-hubs"></a>İki bölümsel IOT hub oluşturma

Bu bölümde, Azure Cloud Shell'i temsil eden iki yeni IOT hub'ları oluşturmak için kullanacağı **Contoso Tebrikleri bölme** ve **Contoso ısı Pompalara bölme**.

1. Bir kaynak grubu oluşturmak için Azure Cloud Shell'i kullanmak [az grubu oluşturma](/cli/azure/group#az-group-create) komutu. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. 

    Aşağıdaki örnekte adlı bir kaynak grubu oluşturur *contoso-us-resource-group* içinde *eastus* bölge. Bu makalede oluşturulan tüm kaynakları için bu grubu kullanmanızı öneririz. Tamamladıktan sonra bu yaklaşım daha kolay temizleme.

    ```azurecli-interactive 
    az group create --name contoso-us-resource-group --location eastus
    ```

2. Azure Cloud Shell'i oluşturulacağı **Contoso Tebrikleri bölme** IOT hub ile [az IOT hub'ı oluşturma](/cli/azure/iot/hub#az-iot-hub-create) komutu. IOT hub'ı eklenecek *contoso-us-resource-group*.

    Aşağıdaki örnekte adlı bir IOT hub oluşturulur *contoso-Tebrikleri-hub-1098* içinde *eastus* konumu. Kendi benzersiz hub adınızı kullanmanız gerekir. Hub adı yerine kendi soneki oluşturan **1098**. Örnek kod için özel ayırma ilkesini gerektirir `-toasters-` hub adı.

    ```azurecli-interactive 
    az iot hub create --name contoso-toasters-hub-1098 --resource-group contoso-us-resource-group --location eastus --sku S1
    ```
    
    Bu komutun tamamlanması birkaç dakika sürebilir.

3. Oluşturmak için Azure Cloud Shell'i kullanmak **Contoso ısı Pompalara bölme** IOT hub ile [az IOT hub oluşturma](/cli/azure/iot/hub#az-iot-hub-create) komutu. Bu IOT hub'ı da eklenir *contoso-us-resource-group*.

    Aşağıdaki örnekte adlı bir IOT hub oluşturulur *contoso-heatpumps-hub-1098* içinde *eastus* konumu. Kendi benzersiz hub adınızı kullanmanız gerekir. Hub adı yerine kendi soneki oluşturan **1098**. Örnek kod için özel ayırma ilkesini gerektirir `-heatpumps-` hub adı.

    ```azurecli-interactive 
    az iot hub create --name contoso-heatpumps-hub-1098 --resource-group contoso-us-resource-group --location eastus --sku S1
    ```
    
    Bu komut ayrıca tamamlanması birkaç dakika sürebilir.




## <a name="create-the-enrollment"></a>Kayıt oluşturma

Bu bölümde, özel ayırma ilkesini kullanan yeni bir kayıt grubu oluşturur. Kolaylık olması için bu makalede kullanan [simetrik anahtar kanıtı](concepts-symmetric-key-attestation.md) kaydı. Daha güvenli bir çözüm için kullanmayı [X.509 sertifikası kanıtlama](concepts-security.md#x509-certificates) ile bir güven zinciri.

1. Oturum [Azure portalında](https://portal.azure.com), cihaz sağlama hizmeti örneğinizi açın.

2. Seçin **kayıtları Yönet** sekmesine ve ardından **kayıt grubu Ekle** sayfanın üstünde düğme. 

3. Üzerinde **kayıt grubu Ekle**, aşağıdaki bilgileri girin ve tıklayın **Kaydet** düğmesi.

    **Grup adı**: Girin **contoso-özel-ayrılan-cihazları**.

    **Kanıtlama türü**: Seçin **simetrik anahtar**.

    **Anahtarları otomatik olarak oluştur**: Bu onay kutusu zaten işaretlenmelidir.

    **Hub'lara cihazları atamak istediğiniz seçin**: Seçin **özel (Azure işlevi kullanın)** .

    ![Simetrik anahtar kanıtı için özel ayırma kayıt grubu Ekle](./media/how-to-use-custom-allocation-policies/create-custom-allocation-enrollment.png)


4. Üzerinde **kayıt grubu Ekle**, tıklayın **yeni bir IOT hub bağlantı** bölümsel, yeni IOT hub'larının her ikisini bağlamak için. 

    Bu adımı her iki bölümsel, IOT hub'larınız için yürütmeniz gerekir.

    **Abonelik**: Birden fazla aboneliğiniz varsa bölümsel IOT hub'ları oluşturduğunuz aboneliği seçin.

    **IOT hub'ı**: Oluşturduğunuz bölümsel hub'ları birini seçin.

    **Erişim İlkesi**: Seçin **iothubowner**.

    ![Bölümsel IOT hub'ları sağlama hizmeti ile bağlama](./media/how-to-use-custom-allocation-policies/link-divisional-hubs.png)


5. Üzerinde **kayıt grubu Ekle**, bölümsel hem IOT hub'ları bağlı sonra bunları kayıt grubu için IOT Hub grubu olarak aşağıda gösterildiği gibi seçmeniz gerekir:

    ![Kayıt için bölümsel hub grubu oluşturma](./media/how-to-use-custom-allocation-policies/enrollment-divisional-hub-group.png)


6. Üzerinde **kayıt grubu Ekle**, ekranı aşağı kaydırarak **Azure işlevi seçin** 'ye tıklayın **yeni bir işlev uygulaması oluşturma**.

7. Üzerinde **işlev uygulaması** sayfası açılır;'a tıklayın ve yeni işlev için aşağıdaki ayarları girin oluşturma **Oluştur**:

    **Uygulama adı**: Benzersiz işlev uygulamanızın adını girin. **işlev uygulaması 1098 contoso** örnek olarak gösterilir.

    **Kaynak grubu**: Seçin **var olanı kullan** ve **contoso-us-resource-group** birlikte bu makalede oluşturulan tüm kaynakları korumak için.

    **Application Insights**: Bu alıştırma için bu kapatabilirsiniz.

    ![İşlev uygulaması oluşturma](./media/how-to-use-custom-allocation-policies/function-app-create.png)


8. Yeniden, **kayıt grubu Ekle** sayfasında, yeni işlev uygulamanızı seçildiğinden emin olun. İşlevi uygulama listesini yenilemek için aboneliği yeniden seçmeniz gerekebilir.

    Yeni işlev uygulamanızı seçildikten sonra tıklayın **yeni bir işlev oluşturma**.

    ![İşlev uygulaması oluşturma](./media/how-to-use-custom-allocation-policies/click-create-new-function.png)

    Yeni işlev uygulamanızı açılır.

9. İşlev uygulamanızı yeni bir işlev oluşturmak için tıklayın

    ![İşlev uygulaması oluşturma](./media/how-to-use-custom-allocation-policies/new-function.png)

    Yeni işlev için yeni bir varsayılan ayarları kullanın. **Web kancası + API** kullanarak **CSharp** dili. Tıklayın **bu işlevi Oluştur**.

    Bu adlı yeni bir C# işlevi oluşturur **HttpTriggerCSharp1**.

10. Yeni C# işlev kodunu tıklayın ve aşağıdaki kod ile değiştirin. **Kaydet**:    

    ```C#
    #r "Newtonsoft.Json"
    using System.Net;
    using System.Text;
    using Newtonsoft.Json;

    public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
    {
        // Just some diagnostic logging
        log.Info("C# HTTP trigger function processed a request.");
        log.Info("Request.Content:...");    
        log.Info(req.Content.ReadAsStringAsync().Result);

        // Get request body
        dynamic data = await req.Content.ReadAsAsync<object>();

        // Get registration ID of the device
        string regId = data?.deviceRuntimeContext?.registrationId;

        string message = "Uncaught error";
        bool fail = false;
        ResponseObj obj = new ResponseObj();

        if (regId == null)
        {
            message = "Registration ID not provided for the device.";
            log.Info("Registration ID : NULL");
            fail = true;
        }
        else
        {
            string[] hubs = data?.linkedHubs.ToObject<string[]>();

            // Must have hubs selected on the enrollment
            if (hubs == null)
            {
                message = "No hub group defined for the enrollment.";
                log.Info("linkedHubs : NULL");
                fail = true;
            }
            else
            {
                // This is a Contoso Toaster Model 007
                if (regId.Contains("-contoso-tstrsd-007"))
                {
                    //Find the "-toasters-" IoT hub configured on the enrollment
                    foreach(string hubString in hubs)
                    {
                        if (hubString.Contains("-toasters-"))
                            obj.iotHubHostName = hubString;
                    }

                    if (obj.iotHubHostName == null)
                    {
                        message = "No toasters hub found for the enrollment.";
                        log.Info(message);
                        fail = true;
                    }
                }
                // This is a Contoso Heat pump Model 008
                else if (regId.Contains("-contoso-hpsd-088"))
                {
                    //Find the "-heatpumps-" IoT hub configured on the enrollment
                    foreach(string hubString in hubs)
                    {
                        if (hubString.Contains("-heatpumps-"))
                            obj.iotHubHostName = hubString;
                    }

                    if (obj.iotHubHostName == null)
                    {
                        message = "No heat pumps hub found for the enrollment.";
                        log.Info(message);
                        fail = true;
                    }
                }
                // Unrecognized device.
                else
                {
                    fail = true;
                    message = "Unrecognized device registration.";
                    log.Info("Unknown device registration");
                }
            }
        }

        return (fail)
            ? req.CreateResponse(HttpStatusCode.BadRequest, message)
            : new HttpResponseMessage(HttpStatusCode.OK)
            {
                Content = new StringContent(JsonConvert.SerializeObject(obj, Formatting.Indented), Encoding.UTF8, "application/json")
            };
    }    

    public class DeviceTwinObj
    {
        public string deviceId {get; set;}
    }

    public class ResponseObj
    {
        public string iotHubHostName {get; set;}
        public string IoTHub {get; set;}
        public DeviceTwinObj initialTwin {get; set;}
        public string[] linkedHubs {get; set;}
        public string enrollment {get; set;}
    }
    ```


11. Geri dönüp, **kayıt grubu Ekle** sayfasında ve yeni işlev seçildiğinden emin olun. İşlev uygulaması, işlev listeyi yenilemek için yeniden seçmeniz gerekebilir.

    Yeni işlevinizde seçildikten sonra tıklayın **Kaydet** kayıt grubunu kaydetmek için.

    ![Son olarak, kayıt grubunu kaydedin](./media/how-to-use-custom-allocation-policies/save-enrollment.png)


12. Kaydı kaydetme sonra yeniden açın ve Not **birincil anahtar**. Oluşturulan anahtarları için ilk kayıt kaydetmeniz gerekir. Bu anahtar, sanal cihazlar için benzersiz bir cihaz anahtarları oluşturmak için kullanılır.


## <a name="derive-unique-device-keys"></a>Benzersiz cihaz anahtarları türetin

Bu bölümde, iki benzersiz cihaz anahtarları oluşturacaksınız. Bir anahtar toaster sanal cihaz için kullanılır. Başka bir tuşa ısı pompa sanal cihaz için kullanılır.

Cihaz anahtarı oluşturmak için kullanacağınız **birincil anahtar** hesaplamak için daha önce Not [HMAC SHA256](https://wikipedia.org/wiki/HMAC) her bir cihaz ve sonucu Base64 biçimine dönüştürmek için cihaz kayıt kimliği. Kayıt grupları ile türetilen cihaz anahtarları oluşturma hakkında daha fazla bilgi için Grup kayıtları bölümüne bakın. [simetrik anahtar kanıtı](concepts-symmetric-key-attestation.md).

Örneğin bu makalede, aşağıdaki iki cihaz kayıt kimliklerini ve cihaz anahtarı hem de cihazlar için işlem. Her iki kayıt kimlikleri özel ayırma ilkesi için örnek kod ile çalışmak için geçerli bir son eke sahiptir:

- **contoso tstrsd 007 breakroom499**
- **mainbuilding167-contoso-hpsd-088**

#### <a name="linux-workstations"></a>Linux iş istasyonları

Bir Linux iş istasyonu kullanıyorsanız, aşağıdaki örnekte gösterildiği gibi türetilen cihaz anahtarları oluşturmak için openssl kullanabilirsiniz.

1. Değiştirin **anahtarı** ile **birincil anahtar** daha önce not ettiğiniz.

    ```bash
    KEY=oiK77Oy7rBw8YB6IS6ukRChAw+Yq6GC61RMrPLSTiOOtdI+XDu0LmLuNm11p+qv2I+adqGUdZHm46zXAQdZoOA==

    REG_ID1=breakroom499-contoso-tstrsd-007
    REG_ID2=mainbuilding167-contoso-hpsd-088

    keybytes=$(echo $KEY | base64 --decode | xxd -p -u -c 1000)
    devkey1=$(echo -n $REG_ID1 | openssl sha256 -mac HMAC -macopt hexkey:$keybytes -binary | base64)
    devkey2=$(echo -n $REG_ID2 | openssl sha256 -mac HMAC -macopt hexkey:$keybytes -binary | base64)

    echo -e $"\n\n$REG_ID1 : $devkey1\n$REG_ID2 : $devkey2\n\n"
    ```

    ```bash
    breakroom499-contoso-tstrsd-007 : JC8F96eayuQwwz+PkE7IzjH2lIAjCUnAa61tDigBnSs=
    mainbuilding167-contoso-hpsd-088 : 6uejA9PfkQgmYylj8Zerp3kcbeVrGZ172YLa7VSnJzg=
    ```


#### <a name="windows-based-workstations"></a>Windows tabanlı iş istasyonları

Windows tabanlı bir iş istasyonu kullanıyorsanız, aşağıdaki örnekte gösterildiği gibi türetilen cihaz anahtarı oluşturmak için PowerShell kullanabilirsiniz.

1. Değiştirin **anahtarı** ile **birincil anahtar** daha önce not ettiğiniz.

    ```powershell
    $KEY='oiK77Oy7rBw8YB6IS6ukRChAw+Yq6GC61RMrPLSTiOOtdI+XDu0LmLuNm11p+qv2I+adqGUdZHm46zXAQdZoOA=='

    $REG_ID1='breakroom499-contoso-tstrsd-007'
    $REG_ID2='mainbuilding167-contoso-hpsd-088'

    $hmacsha256 = New-Object System.Security.Cryptography.HMACSHA256
    $hmacsha256.key = [Convert]::FromBase64String($key)
    $sig1 = $hmacsha256.ComputeHash([Text.Encoding]::ASCII.GetBytes($REG_ID1))
    $sig2 = $hmacsha256.ComputeHash([Text.Encoding]::ASCII.GetBytes($REG_ID2))
    $derivedkey1 = [Convert]::ToBase64String($sig1)
    $derivedkey2 = [Convert]::ToBase64String($sig2)

    echo "`n`n$REG_ID1 : $derivedkey1`n$REG_ID2 : $derivedkey2`n`n"
    ```

    ```powershell
    breakroom499-contoso-tstrsd-007 : JC8F96eayuQwwz+PkE7IzjH2lIAjCUnAa61tDigBnSs=
    mainbuilding167-contoso-hpsd-088 : 6uejA9PfkQgmYylj8Zerp3kcbeVrGZ172YLa7VSnJzg=
    ```


Sanal cihazlar, simetrik anahtar kanıtı gerçekleştirmek için her bir kayıt kimliği ile türetilen cihaz anahtarlarını kullanır.




## <a name="prepare-an-azure-iot-c-sdk-development-environment"></a>Azure IoT C SDK'sı için geliştirme ortamını hazırlama

Bu bölümde, [Azure IoT C SDK'sını](https://github.com/Azure/azure-iot-sdk-c) oluşturmak için kullanılan geliştirme ortamını hazırlayacaksınız. SDK'sı sanal cihaz için örnek kod içerir. Simülasyon cihazı, cihazın önyükleme dizisi sırasında sağlamayı dener.

Bu bölüm, Windows tabanlı bir iş istasyonu doğru yönlendirilmiş yöneliktir. Kurulum sanal makinelerin bir Linux örneği için bkz [çoklu müşteri mimarisi için sağlama](how-to-provision-multitenant.md).

1. İndirme [CMake derleme sistemini](https://cmake.org/download/).

    `CMake` yüklemesine başlamadan **önce** makinenizde Visual Studio önkoşullarının (Visual Studio ve "C++ ile masaüstü geliştirme" iş yükü) yüklenmiş olması önemlidir. Önkoşullar sağlandıktan ve indirme doğrulandıktan sonra, CMake derleme sistemini yükleyin.

2. Komut istemini veya Git Bash kabuğunu açın. Aşağıdaki komutu yürüterek Azure IoT C SDK'sı GitHub deposunu kopyalayın:
    
    ```cmd/sh
    git clone https://github.com/Azure/azure-iot-sdk-c.git --recursive
    ```
    Bu işlemin tamamlanması için birkaç dakika beklemeniz gerekebilir.


3. Git deposunun kök dizininde bir `cmake` alt dizini oluşturun ve o klasöre gidin. 

    ```cmd/sh
    cd azure-iot-sdk-c
    mkdir cmake
    cd cmake
    ```

4. SDK’nın geliştirme istemci platformunuza ve özgü bir sürümünü oluşturmak için aşağıdaki komutu çalıştırın. `cmake` dizininde simülasyon cihazı için bir Visual Studio çözümü de oluşturulur. 

    ```cmd
    cmake -Dhsm_type_symm_key:BOOL=ON -Duse_prov_client:BOOL=ON  ..
    ```
    
    `cmake`, C++ derleyicinizi bulamazsa yukarıdaki komutu çalıştırırken derleme hatalarıyla karşılaşabilirsiniz. Bu durumda bu komutu [Visual Studio komut isteminde](https://docs.microsoft.com/dotnet/framework/tools/developer-command-prompt-for-vs) çalıştırmayı deneyin. 

    Derleme başarılı olduktan sonra, son birkaç çıkış satırı aşağıdaki çıkışa benzer olacaktır:

    ```cmd/sh
    $ cmake -Dhsm_type_symm_key:BOOL=ON -Duse_prov_client:BOOL=ON  ..
    -- Building for: Visual Studio 15 2017
    -- Selecting Windows SDK version 10.0.16299.0 to target Windows 10.0.17134.
    -- The C compiler identification is MSVC 19.12.25835.0
    -- The CXX compiler identification is MSVC 19.12.25835.0

    ...

    -- Configuring done
    -- Generating done
    -- Build files have been written to: E:/IoT Testing/azure-iot-sdk-c/cmake
    ```




## <a name="simulate-the-devices"></a>Cihazların benzetimini yapma

Bu bölümde, adlı bir sağlama örnek güncelleştirecektir **prov\_geliştirme\_istemci\_örnek** Azure IOT C ayarladığınız daha önce SDK'sı bulunur. 

Bu örnek kod, cihaz sağlama hizmeti örneğinizi sağlama isteği gönderdiği cihaz önyükleme sırası benzetimini yapar. Önyükleme sırasını tanınır ve özel ayırma ilkesini kullanarak IOT hub'ına atanma toaster cihazın neden olur.

1. Azure Portal'da Cihaz Sağlama hizmetiniz için **Genel Bakış** sekmesini seçin ve **_Kimlik Kapsamı_** değerini not alın.

    ![Portal dikey penceresinden Cihaz Sağlama Hizmeti uç noktası bilgilerini ayıklama](./media/quick-create-simulated-device-x509/extract-dps-endpoints.png) 

2. Visual Studio'da açın **azure_iot_sdks.sln** CMake çalıştırarak daha önce oluşturulan çözüm dosyası. Çözüm dosyası şu konumda olmalıdır:

    ```
    \azure-iot-sdk-c\cmake\azure_iot_sdks.sln
    ```

3. Visual Studio'nun *Çözüm Gezgini* penceresinde **Sağlama\_Örnekleri** klasörüne gidin. **prov\_dev\_client\_sample** adlı örnek projeyi genişletin. **Kaynak Dosyalar**'ı genişletin ve **prov\_dev\_client\_sample.c** dosyasını açın.

4. `id_scope` sabitini bulun ve değeri daha önce kopyalamış olduğunuz **Kimlik Kapsamı** değerinizle değiştirin. 

    ```c
    static const char* id_scope = "0ne00002193";
    ```

5. Aynı dosyada `main()` işlevinin tanımını bulun. `hsm_type` değişkeninin aşağıda gösterildiği gibi `SECURE_DEVICE_TYPE_SYMMETRIC_KEY` değerine ayarlandığından emin olun:

    ```c
    SECURE_DEVICE_TYPE hsm_type;
    //hsm_type = SECURE_DEVICE_TYPE_TPM;
    //hsm_type = SECURE_DEVICE_TYPE_X509;
    hsm_type = SECURE_DEVICE_TYPE_SYMMETRIC_KEY;
    ```

6. **prov\_dev\_client\_sample** projesine sağ tıklayın ve **Başlangıç Projesi Olarak Ayarla**’yı seçin. 


#### <a name="simulate-the-contoso-toaster-device"></a>Contoso toaster cihazının simülasyonunu gerçekleştirme

1. Toaster cihaz benzetimini yapmak için çağrı Bul `prov_dev_set_symmetric_key_info()` içinde **prov\_geliştirme\_istemci\_sample.c** hangi dışında bırakılır.

    ```c
    // Set the symmetric key if using they auth type
    //prov_dev_set_symmetric_key_info("<symm_registration_id>", "<symmetric_Key>");
    ```

    İşlev çağrısının açıklamasını kaldırın ve (açılı ayraçlar dahil) yer tutucu değerlerini toaster kayıt kimliği ve daha önce oluşturulan türetilen cihaz anahtarı ile değiştirin. Anahtar değeri **JC8F96eayuQwwz + PkE7IzjH2lIAjCUnAa61tDigBnSs =** gösterilen aşağıda yalnızca örnek olarak verilmiştir.

    ```c
    // Set the symmetric key if using they auth type
    prov_dev_set_symmetric_key_info("breakroom499-contoso-tstrsd-007", "JC8F96eayuQwwz+PkE7IzjH2lIAjCUnAa61tDigBnSs=");
    ```
   
    Dosyayı kaydedin.

2. Çözümü çalıştırmak için Visual Studio menüsünde **Hata Ayıkla** > **Hata ayıklama olmadan başlat**'ı seçin. Projeyi yeniden derleme isteminde **Evet**'e tıklayarak, çalıştırmadan önce projeyi yeniden derleyin.

    Aşağıdaki çıktı, Yedekleme başarıyla önyüklenmesini ve özel ayırma ilkesi tarafından Tebrikleri IOT hub'ına atanacak sağlama hizmeti örneğine bağlanan sanal toaster cihaz örneğidir:

    ```cmd
    Provisioning API Version: 1.2.9

    Registering Device

    Provisioning Status: PROV_DEVICE_REG_STATUS_CONNECTED
    Provisioning Status: PROV_DEVICE_REG_STATUS_ASSIGNING
    Provisioning Status: PROV_DEVICE_REG_STATUS_ASSIGNING

    Registration Information received from service: contoso-toasters-hub-1098.azure-devices.net, deviceId: breakroom499-contoso-tstrsd-007

    Press enter key to exit:
    ```


#### <a name="simulate-the-contoso-heat-pump-device"></a>Contoso ısı pompa cihazının simülasyonunu gerçekleştirme

1. Isı pompa cihaz benzetimini yapmak için çağrı güncelleştirme `prov_dev_set_symmetric_key_info()` içinde **prov\_geliştirme\_istemci\_sample.c** yeniden ısı pompa kayıt kimliği ve türetilen cihaz anahtarı ile daha önce oluşturulan . Anahtar değeri **6uejA9PfkQgmYylj8Zerp3kcbeVrGZ172YLa7VSnJzg =** gösterilen aşağıda ayrıca yalnızca bir örnek olarak verilmiştir.

    ```c
    // Set the symmetric key if using they auth type
    prov_dev_set_symmetric_key_info("mainbuilding167-contoso-hpsd-088", "6uejA9PfkQgmYylj8Zerp3kcbeVrGZ172YLa7VSnJzg=");
    ```
   
    Dosyayı kaydedin.

2. Çözümü çalıştırmak için Visual Studio menüsünde **Hata Ayıkla** > **Hata ayıklama olmadan başlat**'ı seçin. Projeyi yeniden derleme isteminde **Evet**'e tıklayarak, çalıştırmadan önce projeyi yeniden derleyin.

    Aşağıdaki çıktı, Yedekleme başarıyla önyüklenmesini ve özel ayırma ilkesi tarafından Contoso ısı pompalara IOT hub'ına atanacak sağlama hizmeti örneğine bağlanan sanal ısı pompa cihaz örneğidir:

    ```cmd
    Provisioning API Version: 1.2.9

    Registering Device

    Provisioning Status: PROV_DEVICE_REG_STATUS_CONNECTED
    Provisioning Status: PROV_DEVICE_REG_STATUS_ASSIGNING
    Provisioning Status: PROV_DEVICE_REG_STATUS_ASSIGNING

    Registration Information received from service: contoso-heatpumps-hub-1098.azure-devices.net, deviceId: mainbuilding167-contoso-hpsd-088

    Press enter key to exit:
    ```


## <a name="troubleshooting-custom-allocation-policies"></a>Özel ayırma ilkeleri sorunlarını giderme

Aşağıdaki tabloda, beklenen senaryoları ve karşılaşabileceğiniz sonuçları hata kodları gösterilir. Bu tablo, Azure işlevleri ile özel ayırma ilkesi hataları gidermeye yardımcı olmak için kullanın.


| Senaryo | Sağlama Hizmeti'nden kayıt sonucu | SDK'sı sonuçları sağlama |
| -------- | --------------------------------------------- | ------------------------ |
| Web kancası 200 Tamam 'geçerli bir IOT hub konak adına ayarlayın iotHubHostName' döndürür | Sonuç durumu: Atanan  | SDK'sı PROV_DEVICE_RESULT_OK yanı sıra hub bilgilerini döndürür. |
| Web kancası 200 Tamam 'iotHubHostName' yanıtta sunmak, ancak bir boş dize veya null döndürür. | Sonuç durumu: Başarısız<br><br> Hata kodu: CustomAllocationIotHubNotSpecified (400208) | SDK'sı PROV_DEVICE_RESULT_HUB_NOT_SPECIFIED döndürür |
| Web kancası 401 Yetkisiz döndürür | Sonuç durumu: Başarısız<br><br>Hata kodu: CustomAllocationUnauthorizedAccess (400209) | SDK returns PROV_DEVICE_RESULT_UNAUTHORIZED |
| Cihaz devre dışı bırakmak için bireysel kayıt oluşturuldu | Sonuç durumu: Devre dışı | SDK'sı PROV_DEVICE_RESULT_DISABLED döndürür |
| Web kancası hata kodu döndürür > 429 = | DPS düzenleme için birkaç kez yeniden deneyecek. Yeniden deneme ilkesi şu anda şöyledir:<br><br>&nbsp;&nbsp;-Yeniden deneme sayısı: 10<br>&nbsp;&nbsp;-Başlangıç aralığı: 1s<br>&nbsp;&nbsp;-Artış: 9s | SDK'sı hatasını görmezden Gel ve belirtilen süre içinde başka bir get durum iletisi gönderin |
| Web kancası herhangi bir durum kodu döndürür. | Sonuç durumu: Başarısız<br><br>Hata kodu: CustomAllocationFailed (400207) | SDK'sı PROV_DEVICE_RESULT_DEV_AUTH_ERROR döndürür |


## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu makalede oluşturulan kaynakları ile çalışmaya devam etmeyi planlıyorsanız, bunları bırakabilirsiniz. Kaynağı kullanmaya devam etmeyi planlamıyorsanız, gereksiz ücretlerden kaçınmak için bu makaleyi tarafından oluşturulan tüm kaynakları silmek için aşağıdaki adımları kullanın.

Oluşturduğunuz tüm kaynakları bu makaledeki adlı kaynak grubunda belirtildiği gibi buradaki adımları varsayar **contoso-us-resource-group**.

> [!IMPORTANT]
> Silinen kaynak grupları geri alınamaz. Kaynak grubu ve içindeki tüm kaynaklar kalıcı olarak silinir. Yanlış kaynak grubunu veya kaynakları yanlışlıkla silmediğinizden emin olun. IoT Hub'ı tutmak istediğiniz kaynakların bulunduğu mevcut bir kaynak grubunda oluşturduysanız kaynak grubunu silmek yerine IoT Hub kaynağını silin.
>

Ada göre kaynak grubunu silmek için:

1. [Azure portalında](https://portal.azure.com) oturum açın ve **Kaynak grupları**’na tıklayın.

2. İçinde **ada göre Filtrele...**  metin kutusuna kaynak adını grubunda kaynaklarınızın içeren **contoso-us-resource-group**. 

3. Sonuç listesinde kaynak grubunuzun sağ tarafında **...** ve sonra **Kaynak grubunu sil**'e tıklayın.

4. Kaynak grubunun silinmesini onaylamanız istenir. Onaylamak için kaynak grubunuzun adını tekrar yazın ve **Sil**'e tıklayın. Birkaç dakika sonra kaynak grubu ve içerdiği kaynakların tümü silinir.

## <a name="next-steps"></a>Sonraki adımlar

- Reprovisioning daha fazla bilgi edinmek için [IOT Hub cihaz kavramları çıkış](concepts-device-reprovision.md) 
- Daha fazla sağlama kaldırmayı bilgi edinmek için [nasıl daha önce otomatik olarak sağlanan cihazları sağlamasını kaldırmak](how-to-unprovision-devices.md) 











