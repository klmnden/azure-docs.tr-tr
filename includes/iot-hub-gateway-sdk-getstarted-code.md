## <a name="typical-output"></a>Normal çıktı
Hello World örneği tarafından göre günlük dosyasına yazılmış çıktının bir örneği aşağıda verilmiştir. Yeni Satır ve Sekme karakterleri okunabilirlik için eklenmiştir:

```
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
Bu bölümde Hello World örneğindeki kodun bazı önemli bölümleri ele alınmaktadır.

### <a name="gateway-creation"></a>Ağ geçidi oluşturma
Geliştirici *ağ geçidi işlemini* yazmalıdır. Bu program iç altyapıyı (aracı) oluşturur, modülleri yükler ve her şeyi doğru çalışacak şekilde ayarlar. SDK bir JSON dosyasından ağ geçidini önyüklemenizi sağlayan **Gateway_Create_From_JSON** işlevini sağlar. **Gateway_Create_From_JSON** işlevini kullanmak için yüklenecek modülleri belirten bir JSON dosyası yoluna geçirmeniz gerekir. 

Hello World örneğindeki ağ geçidi işleminin kodunu [main.c][lnk-main-c] dosyasına bulabilirsiniz. Okunaklılık için aşağıdaki kod parçacığında ağ geçidi işlem kodunun kısaltılmış sürümü gösterilmektedir. Bu program bir ağ geçidi oluşturur ve ağ geçidini çıkarmadan önce kullanıcının **ENTER** tuşuna basmasını bekler. 

```
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

JSON ayarlar dosyası yüklenecek modüllerin bir listesini içerir. Her modülü aşağıdakileri belirtmelidir:

* **module_name**: modül için benzersiz bir ad.
* **module_path**: modülü içeren kitaplığın yolu. Linux için bu bir .so dosyası, Windows'ta ise bir .dll dosyasıdır.
* **args**: modül için gereken tüm yapılandırma bilgileri.

JSON dosyası ayrıca aracıya geçirilecek modüller arasındaki bağlantıları içerir. Bir bağlantı iki özelliğe sahiptir:

* **kaynak**: `modules` bölümünden bir modül adı veya "\*".
* **havuz**: `modules` bölümünden bir modül adı.

Her bağlantı bir ileti yolu ve yönü tanımlar. `source` modülünden gelen iletiler `sink` modülüne teslim edilmelidir. Herhangi bir modülden gelen iletilerin `sink` tarafından alınacağını belirtmek üzere `source` ayarı "\*" olarak belirlenebilir.

Aşağıdaki örnekte Linux üzerinde Hello World örneğini yapılandırmak için kullanılan JSON ayarlar dosyası gösterilmektedir. `hello_world` modülü tarafından üretilen her ileti `logger` modülü tarafından kullanılır. Bir modülün bağımsız değişken gerektirip gerektirmediği modülün tasarımına bağlıdır. Bu örnekte günlükçü modülü, çıktı dosyasının yolu olan bir bağımsız değişkeni alır ve Hello World modülü herhangi bir bağımsız değişken almaz:

```
{
    "modules" :
    [ 
        {
            "module name" : "logger",
            "loading args": {
              "module path" : "./modules/logger/liblogger_hl.so"
            },
            "args" : {"filename":"log.txt"}
        },
        {
            "module name" : "hello_world",
            "loading args": {
              "module path" : "./modules/hello_world/libhello_world_hl.so"
            },
            "args" : null
        }
    ],
    "links" :
    [
        {
            "source" : "hello_world",
            "sink" : "logger"
        }
    ]
}
```

### <a name="hello-world-module-message-publishing"></a>Hello World modülü ileti yayımlama
"Hello world" modülü tarafından ileti yayımlamak amacıyla kullanılan kodu ['hello_world.c'][lnk-helloworld-c] dosyasında bulabilirsiniz. Aşağıdaki kod parçacığı ek açıklamalarla birlikte değiştirilmiş bir sürümü göstermektedir ve bazı hata işleme kodları okunaklılık için kaldırılmıştır:

```
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

### <a name="hello-world-module-message-processing"></a>Hello World modülü ileti işleme
Diğer modüllerin aracıya yayımladıkları herhangi bir iletiyi Hello World modülünün hiçbir zaman işlemesi gerekmez. Bu özellik Hello World modülünde ileti çağırma uygulamasını işlemsiz bir işlev haline getirir.

```
static void HelloWorld_Receive(MODULE_HANDLE moduleHandle, MESSAGE_HANDLE messageHandle)
{
    /* No action, HelloWorld is not interested in any messages. */
}
```

### <a name="logger-module-message-publishing-and-processing"></a>Günlükçü modülü ileti yayımlama ve işleme
Günlükçü modülü iletileri aracıdan alır ve bir dosyaya yazar. Hiçbir zaman bir ileti yayımlamaz. Bu nedenle, günlükçü modülünün kodu **Broker_Publish** işlevini hiçbir zaman çağırmaz.

[logger.c][lnk-logger-c] dosyasındaki **Logger_Recieve** işlevi, aracının günlükçü modülüne iletileri ulaştırmak üzere çağırdığı geri çağırmadır. Aşağıdaki kod parçacığı ek açıklamalarla birlikte değiştirilmiş bir sürümü göstermektedir ve bazı hata işleme kodları okunaklılık için kaldırılmıştır:

```
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
IoT Gateway SDK’sını kullanma hakkında bilgi için aşağıdakilere bakın:

* [IoT Gateway SDK’sı – Linux][lnk-gateway-simulated] kullanarak sanal bir cihazla cihazdan buluta iletiler gönderir.
* GitHub üzerinde [Azure IoT Gateway SDK][lnk-gateway-sdk].

<!-- Links -->
[lnk-main-c]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/samples/hello_world/src/main.c
[lnk-helloworld-c]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/modules/hello_world/src/hello_world.c
[lnk-logger-c]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/modules/logger/src/logger.c
[lnk-gateway-sdk]: https://github.com/Azure/azure-iot-gateway-sdk/
[lnk-gateway-simulated]: ../articles/iot-hub/iot-hub-linux-gateway-sdk-simulated-device.md

<!--HONumber=Nov16_HO3-->


