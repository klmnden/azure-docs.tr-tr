## <a name="typical-output"></a>Normal çıktı

Aşağıdaki örnek günlük dosyasına Hello World örneği tarafından yazılmış çıkış gösterir. Çıktı daha okunaklı olması için biçimlendirilir:

```json
[{
    "time": "Mon Apr 11 13:42:50 2016",
    "content": "Log started"
}, {
    "time": "Mon Apr 11 13:42:50 2016",
    "properties": {
        "helloWorld": "from Azure IoT Gateway SDK simple sample!"
    },
    "content": "aGVsbG8gd29ybGQ="
}, {
    "time": "Mon Apr 11 13:42:55 2016",
    "properties": {
        "helloWorld": "from Azure IoT Gateway SDK simple sample!"
    },
    "content": "aGVsbG8gd29ybGQ="
}, {
    "time": "Mon Apr 11 13:43:00 2016",
    "properties": {
        "helloWorld": "from Azure IoT Gateway SDK simple sample!"
    },
    "content": "aGVsbG8gd29ybGQ="
}, {
    "time": "Mon Apr 11 13:45:00 2016",
    "content": "Log stopped"
}]
```

## <a name="code-snippets"></a>Kod parçacıkları

Bu bölümde merhaba\_dünya örneğindeki kodun bazı önemli bölümleri ele alınmaktadır.

### <a name="iot-edge-gateway-creation"></a>IOT sınır ağ geçidi oluşturma

Bir ağ geçidi oluşturmak için uygulaması bir *ağ geçidi işlem*. Bu program, iç altyapı (Aracısı) oluşturur, IOT kenar modüllerini yükler ve ağ geçidi işlem yapılandırır. IoT Edge, bir JSON dosyasından ağ geçidini önyüklemenizi sağlayan **Gateway\_Create\_From\_JSON** işlevini sağlar. Kullanılacak **ağ geçidi\_oluşturma\_gelen\_JSON** işlev, yüklemek için IOT kenar modülleri belirten bir JSON dosyası yolu geçirin.

Ağ geçidi işleminde kodu bulabilirsiniz *Hello World* içinde örnek [main.c] [ lnk-main-c] dosya. Okunaklılık için aşağıdaki kod parçacığında ağ geçidi işlem kodunun kısaltılmış sürümü gösterilmektedir. Bu örnek program bir ağ geçidi oluşturur ve ağ geçidini çıkarmadan önce kullanıcının **ENTER** tuşuna basmasını bekler.

```C
int main(int argc, char** argv)
{
    GATEWAY_HANDLE gateway;
    if ((gateway = Gateway_Create_From_JSON(argv[1])) == NULL)
    {
        printf("failed to create the gateway from JSON\n");
    }
    else
    {
        printf("gateway successfully created from JSON\n");
        printf("gateway shall run until ENTER is pressed\n");
        (void)getchar();
        Gateway_LL_Destroy(gateway);
    }
    return 0;
}
```

JSON ayarları dosyasını yüklemek için IOT kenar modülleri ve modülleri arasındaki bağlantıları listesini içerir. Her IOT kenar modülü a: belirtmeniz gerekir

* **name**: Modül için benzersiz bir ad.
* **loader**: İstenen modülün nasıl yükleneceğini bilen bir yükleyici. Yükleyiciler, farklı türlerdeki modüllerin yüklenmesi için bir uzantı noktasıdır. IOT kenar yükleyicilerini kullanım için yerel C, Node.js, Java ve .NET içinde yazılmış modüller sağlar. Hello World örneği bu örnekteki tüm modülleri dinamik kitaplıkları c dilinde yazılmış olduğundan yalnızca yerel C yükleyicisi kullanır. Farklı dillerde yazılmış IOT kenar modüllerini kullanma hakkında daha fazla bilgi için bkz: [Node.js](https://github.com/Azure/iot-edge/blob/master/samples/nodejs_simple_sample/), [Java](https://github.com/Azure/iot-edge/tree/master/samples/java_sample), veya [.NET](https://github.com/Azure/iot-edge/tree/master/samples/dotnet_binding_sample) örnekleri.
    * **ad**: modülünü yüklemek için kullanılan yükleyici adı.
    * **entrypoint**: Modülü içeren kitaplığın yolu. Linux üzerinde bu kitaplığı .so dosya, Windows bu kitaplığı bir .dll dosyası. Bu giriş noktası kullanılan yükleyici türüne özeldir. Node.js yükleyicisi giriş noktası bir .js dosyasıdır. Java yükleyicisi giriş noktası bir sınıf ve sınıf adı ' dir. .NET yükleyicisi giriş noktası bir derleme adı ve bir sınıf adıdır.

* **args**: modül için gereken tüm yapılandırma bilgileri.

Aşağıdaki kod, tüm IOT kenar Hello World örnek modülleri Linux'ta bildirmek için kullanılan JSON gösterir. Bir modülün herhangi bir bağımsız değişken gerektirip gerektirmediği modülün tasarımına bağlıdır. Bu örnekte günlükçü modülü, çıkış dosyasının yolu olan bir bağımsız değişkeni alır ve merhaba\_dünya modülü herhangi bir bağımsız değişken içermez.

```json
"modules" :
[
    {
        "name" : "logger",
        "loader": {
          "name": "native",
          "entrypoint": {
            "module.path": "./modules/logger/liblogger.so"
        }
        },
        "args" : {"filename":"log.txt"}
    },
    {
        "name" : "hello_world",
        "loader": {
          "name": "native",
          "entrypoint": {
            "module.path": "./modules/hello_world/libhello_world.so"
        }
        },
        "args" : null
    }
]
```

JSON dosyası ayrıca aracıya geçirilen modüller arasındaki bağlantıları içerir. Bir bağlantı iki özelliğe sahiptir:

* **Kaynak**: Modül adı `modules` bölümünde veya `\*`.
* **havuz**: `modules` bölümünden bir modül adı.

Her bağlantı bir ileti yolu ve yönü tanımlar. Gelen iletileri **kaynak** modülü için teslim edilir **havuz** modülü. Ayarlayabileceğiniz **kaynak** modülüne `\*`, hangi gösterir **havuz** modülü herhangi bir modül iletileri alır.

Aşağıdaki kod, Linux’ta merhaba\_dünya örneğinde kullanılan modüller arasında bağlantı yapılandırmak için kullanılan JSON’u göstermektedir. `hello_world` modülü tarafından üretilen her ileti `logger` modülü tarafından kullanılır.

```json
"links":
[
    {
        "source": "hello_world",
        "sink": "logger"
    }
]
```

### <a name="helloworld-module-message-publishing"></a>Merhaba\_dünya modülü ileti yayımlama

Merhaba\_dünya modülü tarafından ileti yayımlamak amacıyla kullanılan modülü ['hello_world.c'][lnk-helloworld-c] dosyasında bulabilirsiniz. Aşağıdaki kod parçacığı, okunaklılık için açıklama eklenen ve bazı hata kodlarının kaldırıldığı değiştirilmiş bir kod sürümünü göstermektedir:

```C
int helloWorldThread(void *param)
{
    // create data structures used in function.
    HELLOWORLD_HANDLE_DATA* handleData = param;
    MESSAGE_CONFIG msgConfig;
    MAP_HANDLE propertiesMap = Map_Create(NULL);

    // add a property named "helloWorld" with a value of "from Azure IoT
    // Gateway SDK simple sample!" to a set of message properties that
    // will be appended to the message before publishing it. 
    Map_AddOrUpdate(propertiesMap, "helloWorld", "from Azure IoT Gateway SDK simple sample!")

    // set the content for the message
    msgConfig.size = strlen(HELLOWORLD_MESSAGE);
    msgConfig.source = HELLOWORLD_MESSAGE;

    // set the properties for the message
    msgConfig.sourceProperties = propertiesMap;

    // create a message based on the msgConfig structure
    MESSAGE_HANDLE helloWorldMessage = Message_Create(&msgConfig);

    while (1)
    {
        if (handleData->stopThread)
        {
            (void)Unlock(handleData->lockHandle);
            break; /*gets out of the thread*/
        }
        else
        {
            // publish the message to the broker
            (void)Broker_Publish(handleData->brokerHandle, helloWorldMessage);
            (void)Unlock(handleData->lockHandle);
        }

        (void)ThreadAPI_Sleep(5000); /*every 5 seconds*/
    }

    Message_Destroy(helloWorldMessage);

    return 0;
}
```

Merhaba\_world modülü hiçbir zaman diğer IOT kenar modüller Aracısı yayımlama iletileri işler. Bu nedenle, merhaba\_dünya modülünde ileti geri çağırma işleminin uygulanması işlemsiz bir işlevdir.

```C
static void HelloWorld_Receive(MODULE_HANDLE moduleHandle, MESSAGE_HANDLE messageHandle)
{
    /* No action, HelloWorld is not interested in any messages. */
}
```

### <a name="logger-module-message-processing"></a>Günlükçü modülü ileti işleme

Günlükçü modülü iletileri aracıdan alır ve bir dosyaya yazar. Hiçbir zaman bir ileti yayımlamaz. Bu nedenle, günlükçü modülünün kodu **Broker_Publish** işlevini hiçbir zaman çağırmaz.

**Logger_Receive** işlevi [logger.c] [ lnk-logger-c] dosyasıdır Aracısı çağırır Günlükçü modülüne iletileri sunmak için geri çağırma. Aşağıdaki kod parçacığı, okunaklılık için açıklama eklenen ve bazı hata kodlarının kaldırıldığı değiştirilmiş bir sürümü göstermektedir:

```C
static void Logger_Receive(MODULE_HANDLE moduleHandle, MESSAGE_HANDLE messageHandle)
{

    time_t temp = time(NULL);
    struct tm* t = localtime(&temp);
    char timetemp[80] = { 0 };

    // Get the message properties from the message
    CONSTMAP_HANDLE originalProperties = Message_GetProperties(messageHandle); 
    MAP_HANDLE propertiesAsMap = ConstMap_CloneWriteable(originalProperties);

    // Convert the collection of properties into a JSON string
    STRING_HANDLE jsonProperties = Map_ToJSON(propertiesAsMap);

    //  base64 encode the message content
    const CONSTBUFFER * content = Message_GetContent(messageHandle);
    STRING_HANDLE contentAsJSON = Base64_Encode_Bytes(content->buffer, content->size);

    // Start the construction of the final string to be logged by adding
    // the timestamp
    STRING_HANDLE jsonToBeAppended = STRING_construct(",{\"time\":\"");
    STRING_concat(jsonToBeAppended, timetemp);

    // Add the message properties
    STRING_concat(jsonToBeAppended, "\",\"properties\":"); 
    STRING_concat_with_STRING(jsonToBeAppended, jsonProperties);

    // Add the content
    STRING_concat(jsonToBeAppended, ",\"content\":\"");
    STRING_concat_with_STRING(jsonToBeAppended, contentAsJSON);
    STRING_concat(jsonToBeAppended, "\"}]");

    // Write the formatted string
    LOGGER_HANDLE_DATA *handleData = (LOGGER_HANDLE_DATA *)moduleHandle;
    addJSONString(handleData->fout, STRING_c_str(jsonToBeAppended);
}
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, iletileri bir günlük dosyasına yazar basit bir IOT sınır ağ geçidi verdi. IOT Hub'ına iletileri gönderen bir örneği çalıştırmak için bkz:

- [IOT kenar – Linux kullanarak sanal bir cihaz ile cihaz-bulut iletileri gönderme][lnk-gateway-simulated-linux] 
- [IOT kenar – Windows kullanımı için sanal bir cihaz cihaz bulut iletilerini göndermek][lnk-gateway-simulated-windows].


<!-- Links -->
[lnk-main-c]: https://github.com/Azure/iot-edge/blob/master/samples/hello_world/src/main.c
[lnk-helloworld-c]: https://github.com/Azure/iot-edge/blob/master/modules/hello_world/src/hello_world.c
[lnk-logger-c]: https://github.com/Azure/iot-edge/blob/master/modules/logger/src/logger.c
[lnk-iot-edge]: https://github.com/Azure/iot-edge/
[lnk-gateway-simulated-linux]: ../articles/iot-hub/iot-hub-linux-iot-edge-simulated-device.md
[lnk-gateway-simulated-windows]: ../articles/iot-hub/iot-hub-windows-iot-edge-simulated-device.md