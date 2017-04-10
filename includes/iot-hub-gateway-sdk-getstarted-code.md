## <a name="typical-output"></a>Normal çıktı

Merhaba Dünya örneği tarafından göre günlük dosyasına yazılmış çıktının bir örneği aşağıda verilmiştir. Çıktı daha okunaklı olması için biçimlendirilir:

```json
[{
    "time": "Mon Apr 11 13:48:07 2016",
    "content": "Log started"
}, {
    "time": "Mon Apr 11 13:48:48 2016",
    "properties": {
        "helloWorld": "from Azure IoT Gateway SDK simple sample!"
    },
    "content": "aGVsbG8gd29ybGQ="
}, {
    "time": "Mon Apr 11 13:48:55 2016",
    "properties": {
        "helloWorld": "from Azure IoT Gateway SDK simple sample!"
    },
    "content": "aGVsbG8gd29ybGQ="
}, {
    "time": "Mon Apr 11 13:49:01 2016",
    "properties": {
        "helloWorld": "from Azure IoT Gateway SDK simple sample!"
    },
    "content": "aGVsbG8gd29ybGQ="
}, {
    "time": "Mon Apr 11 13:49:04 2016",
    "content": "Log stopped"
}]
```

## <a name="code-snippets"></a>Kod parçacıkları

Bu bölümde merhaba\_dünya örneğindeki kodun bazı önemli bölümleri ele alınmaktadır.

### <a name="gateway-creation"></a>Ağ geçidi oluşturma

Geliştirici *ağ geçidi işlemini* yazmalıdır. Bu program iç altyapıyı (aracı) oluşturur, modülleri yükler ve her şeyi doğru çalışacak şekilde ayarlar. SDK bir JSON dosyasından ağ geçidini önyüklemenizi sağlayan **Gateway\_Create\_From\_JSON** işlevini sağlar. **Gateway\_Create\_From\_JSON** işlevini kullanmak için yüklenecek modülleri belirten bir JSON dosyası yoluna geçirmeniz gerekir.

Hello World örneğindeki ağ geçidi işleminin kodunu [main.c][lnk-main-c] dosyasına bulabilirsiniz. Okunaklılık için aşağıdaki kod parçacığında ağ geçidi işlem kodunun kısaltılmış sürümü gösterilmektedir. Bu örnek program bir ağ geçidi oluşturur ve ağ geçidini çıkarmadan önce kullanıcının **ENTER** tuşuna basmasını bekler.

```c
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

JSON ayarlar dosyası, yüklenecek modüllerin ve modüller arası bağlantıların bir listesini içerir. Her modülü aşağıdakileri belirtmelidir:

* **name**: Modül için benzersiz bir ad.
* **loader**: İstenen modülün nasıl yükleneceğini bilen bir yükleyici. Yükleyiciler, farklı türlerdeki modüllerin yüklenmesi için bir uzantı noktasıdır. Yerel olarak C, Node.js, Java ve .NET dillerinde yazılan modüllerle kullanıma yönelik yükleyiciler sağlıyoruz. Merhaba Dünya örneğindeki tüm modüller C dilinde yazılan dinamik kitaplıklar olduğundan, bu örnek yalnızca yerel C yükleyicisi kullanır. Farklı dillerde yazılmış modülleri kullanma hakkında daha fazla bilgi için [Node.js](https://github.com/Azure/azure-iot-gateway-sdk/blob/master/samples/nodejs_simple_sample/), [Java](https://github.com/Azure/azure-iot-gateway-sdk/tree/master/samples/java_sample) veya [.NET](https://github.com/Azure/azure-iot-gateway-sdk/tree/master/samples/dotnet_binding_sample) örneklerine bakın.
    * **name**: Modülü yüklemek için kullanılan yükleyicinin adı.
    * **entrypoint**: Modülü içeren kitaplığın yolu. Linux için bu kitaplık bir .so dosyası, Windows'ta ise bir .dll dosyasıdır. Bu giriş noktası kullanılan yükleyici türüne özeldir. Node.js yükleyicisinin giriş noktası bir .js dosyasıdır. Java yükleyicisinin giriş noktası bir sınıf yoluna ek olarak sınıf adıdır. .NET yükleyicisinin giriş noktası bir bütünleştirilmiş kod adına ek olarak sınıf adıdır.

* **args**: modül için gereken tüm yapılandırma bilgileri.

Aşağıdaki kod, Linux’ta Merhaba Dünya örneğinin tüm modüllerini bildirmek için kullanılan JSON’u göstermektedir. Bir modülün herhangi bir bağımsız değişken gerektirip gerektirmediği modülün tasarımına bağlıdır. Bu örnekte günlükçü modülü, çıkış dosyasının yolu olan bir bağımsız değişkeni alır ve merhaba\_dünya modülü herhangi bir bağımsız değişken içermez.

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

* **kaynak**: `modules` bölümünden bir modül adı veya "\*".
* **havuz**: `modules` bölümünden bir modül adı.

Her bağlantı bir ileti yolu ve yönü tanımlar. `source` modülünden gelen iletiler `sink` modülüne teslim edilir. Herhangi bir modülden gelen iletilerin `sink` tarafından alınacağını belirtmek üzere `source` ayarı "\*" olarak belirlenebilir.

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

```c
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

### <a name="helloworld-module-message-processing"></a>Merhaba\_dünya modülü ileti işleme

Merhaba\_dünya modülü, diğer modüllerin aracıda yayımladığı hiçbir iletiyi işlemez. Bu nedenle, merhaba\_dünya modülünde ileti geri çağırma işleminin uygulanması işlemsiz bir işlevdir.

```c
static void HelloWorld_Receive(MODULE_HANDLE moduleHandle, MESSAGE_HANDLE messageHandle)
{
    /* No action, HelloWorld is not interested in any messages. */
}
```

### <a name="logger-module-message-publishing-and-processing"></a>Günlükçü modülü ileti yayımlama ve işleme

Günlükçü modülü iletileri aracıdan alır ve bir dosyaya yazar. Hiçbir zaman bir ileti yayımlamaz. Bu nedenle, günlükçü modülünün kodu **Broker_Publish** işlevini hiçbir zaman çağırmaz.

[logger.c][lnk-logger-c] dosyasındaki **Logger_Recieve** işlevi, aracının günlükçü modülüne iletileri ulaştırmak üzere çağırdığı geri çağırmadır. Aşağıdaki kod parçacığı, okunaklılık için açıklama eklenen ve bazı hata kodlarının kaldırıldığı değiştirilmiş bir sürümü göstermektedir:

```c
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

IoT Gateway SDK’sını kullanma hakkında bilgi için aşağıdaki makalelere bakın:

* [IoT Gateway SDK’sı – Linux][lnk-gateway-simulated] kullanarak sanal bir cihazla cihazdan buluta iletiler gönderir.
* GitHub'da [Azure IoT Ağ Geçidi SDK'sı][lnk-gateway-sdk].

<!-- Links -->
[lnk-main-c]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/samples/hello_world/src/main.c
[lnk-helloworld-c]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/modules/hello_world/src/hello_world.c
[lnk-logger-c]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/modules/logger/src/logger.c
[lnk-gateway-sdk]: https://github.com/Azure/azure-iot-gateway-sdk/
[lnk-gateway-simulated]: ../articles/iot-hub/iot-hub-linux-gateway-sdk-simulated-device.md