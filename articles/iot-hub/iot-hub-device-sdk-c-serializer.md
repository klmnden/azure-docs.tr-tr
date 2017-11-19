---
title: "Azure IOT cihaz SDK'sı c - seri hale getirici | Microsoft Docs"
description: "Azure IOT cihaz SDK'sı C için seri hale getirici kitaplıkta bir IOT hub ile iletişim cihaz uygulamaları oluşturmak için nasıl kullanılacağını."
services: iot-hub
documentationcenter: 
author: olivierbloch
manager: timlt
editor: 
ms.assetid: defbed34-de73-429c-8592-cd863a38e4dd
ms.service: iot-hub
ms.devlang: cpp
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/06/2016
ms.author: obloch
ms.openlocfilehash: d8b9e147b68d16c6c166e92cbabf5b5b63e23e8d
ms.sourcegitcommit: 933af6219266cc685d0c9009f533ca1be03aa5e9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/18/2017
---
# <a name="azure-iot-device-sdk-for-c--more-about-serializer"></a>Azure IOT cihaz SDK'sı için C – seri hale getirici hakkında daha fazla bilgi
[İlk makale](iot-hub-device-sdk-c-intro.md) sunulan bu serideki **C için Azure IOT cihaz SDK'sı**. Daha ayrıntılı bir açıklaması sonraki makalede sağlanan [ **IoTHubClient**](iot-hub-device-sdk-c-iothubclient.md). Bu makalede daha ayrıntılı bir açıklama kalan bileşeninin sağlayarak SDK'ın kapsamı tamamlar: **seri hale getirici** kitaplığı.

Giriş makalesi nasıl kullanılacağını açıklanan **seri hale getirici** olayları göndermek ve IOT Hub'ından iletileri almak için kitaplık. Bu makalede, biz bu tartışma verilerinizle modellemek nasıl daha eksiksiz bir açıklaması sağlayarak genişletmek **seri hale getirici** makro dili. Makale ayrıca nasıl kitaplığı iletileri serileştiren hakkında daha fazla ayrıntı içerir (ve bazı durumlarda serileştirme davranışı nasıl kontrol edebilir). Biz de oluşturduğunuz modelleri boyutunu belirleyen değiştirebileceğiniz bazı parametreleri açıklayan.

Son olarak, makaleyi ileti ve özellik işleme gibi önceki makalelerinde kapsanan bazı konular revisits. Biz bulma, bu özellikler iş aynı şekilde kullanarak **seri hale getirici** kitaplığı ile göründükleri **IoTHubClient** kitaplığı.

Bu makalede açıklanan her şeyi dayanır **seri hale getirici** SDK örnekleri. İzlemek istiyorsanız, bkz: **simplesample\_amqp** ve **simplesample\_http** c için Azure IOT cihaz SDK'sı eklenmiş olan uygulamalar

Bulabileceğiniz [ **C için Azure IOT cihaz SDK'sı** ](https://github.com/Azure/azure-iot-sdk-c) GitHub depo ve görünüm ayrıntılarını API [C API Başvurusu](https://azure.github.io/azure-iot-sdk-c/index.html).

## <a name="the-modeling-language"></a>Modelleme Dili
[Giriş makalesi](iot-hub-device-sdk-c-intro.md) sunulan bu serideki **C için Azure IOT cihaz SDK'sı** sağlanan örnek aracılığıyla dil modelleme **simplesample\_amqp** uygulama:

```
BEGIN_NAMESPACE(WeatherStation);

DECLARE_MODEL(ContosoAnemometer,
WITH_DATA(ascii_char_ptr, DeviceId),
WITH_DATA(double, WindSpeed),
WITH_ACTION(TurnFanOn),
WITH_ACTION(TurnFanOff),
WITH_ACTION(SetAirResistance, int, Position)
);

END_NAMESPACE(WeatherStation);
```

Gördüğünüz gibi modelleme dil C makroları temel alır. Her zaman tanımınız ile başlayan **başlangıç\_ad alanı** ve her zaman bittiğini **son\_ad alanı**. Şirketinizde veya bu örnekte, üzerinde çalıştığınız proje olduğu gibi ad alanı adı için yaygın bir durumdur.

Ad alanı içinde unsurları olan model tanımları. Bu durumda, tek bir model için bir anemometer yoktur. Bir kez daha, model bir şey adlandırılabilir, ancak bu aygıt veya IOT Hub ile değiştirmek istediğiniz veri türü için tipik olarak adlandırılır.  

Modelleri içeren bir tanımı olaylarını IOT Hub'ına giriş olabilir ( *veri*) IOT Hub'ından alabilir iletileri yanı sıra ( *Eylemler*). Örnekte görebildiğiniz gibi olayları bir türü ve bir ada sahip; Eylemler, bir ad ve isteğe bağlı parametreler (her bir türüyle) sahip.

SDK'sı tarafından desteklenen ek veri türleri olan ne Bu örnekte gösterilmez. Biz bu sonraki ele alacağız.

> [!NOTE]
> IOT hub'ı, bir cihazın gönderdiği olarak veri başvurduğu *olayları*olarak Modelleme Dili başvurduğu yaparken *veri* (kullanılarak tanımlanan **WITH_DATA**). Benzer şekilde, IOT hub'ı veri Gönder aygıtlara başvurduğu *iletileri*olarak Modelleme Dili başvurduğu yaparken *Eylemler* (kullanılarak tanımlanan **WITH_ACTION**). Bu makalede birbirinin yerine kullanılabilir bu koşulları unutmayın.
> 
> 

### <a name="supported-data-types"></a>Desteklenen veri türleri
Aşağıdaki veri türleri ile oluşturulan modellerinde desteklenir **seri hale getirici** kitaplığı:

| Tür | Açıklama |
| --- | --- |
| Çift |çift duyarlıklı kayan nokta sayısı |
| Int |32 bit tamsayı |
| Kayan nokta |tek duyarlıklı kayan nokta sayısı |
| uzun |uzun tamsayı |
| int8\_t |8 bit tam sayı |
| int16\_t |16 bit tam sayı |
| Int32\_t |32 bit tamsayı |
| int64\_t |64 bit tamsayı |
| bool |Boole değeri |
| ASCII\_char\_ptr |ASCII dizesi |
| EDM\_TARİH\_ZAMAN\_UZAKLIĞI |Tarih Saat farkı |
| EDM\_GUID |GUID |
| EDM\_İKİLİ |İkili |
| BİLDİRME\_YAPISI |karmaşık veri türü |

Son veri türüyle başlayalım. **DECLARE\_YAPISI** gruplandırmaları başka ilkel türleri olan karmaşık veri türlerini tanımlamanızı sağlar. Bu gruplandırmalar bize şuna benzer bir modeli tanımlamak izin ver:

```
DECLARE_STRUCT(TestType,
double, aDouble,
int, aInt,
float, aFloat,
long, aLong,
int8_t, aInt8,
uint8_t, auInt8,
int16_t, aInt16,
int32_t, aInt32,
int64_t, aInt64,
bool, aBool,
ascii_char_ptr, aAsciiCharPtr,
EDM_DATE_TIME_OFFSET, aDateTimeOffset,
EDM_GUID, aGuid,
EDM_BINARY, aBinary
);

DECLARE_MODEL(TestModel,
WITH_DATA(TestType, Test)
);
```

Bir tek veri olayı türü modelimizi içeriyor **TestType**. **TestType** topluca tarafından desteklenen ilkel türler gösteren birkaç üyeleri içeren karmaşık bir tür **seri hale getirici** model oluşturma dili.

Bir modelin bu şekilde size aşağıdaki gibi görünen IOT Hub veri göndermek için kod yazabilirsiniz:

```
TestModel* testModel = CREATE_MODEL_INSTANCE(MyThermostat, TestModel);

testModel->Test.aDouble = 1.1;
testModel->Test.aInt = 2;
testModel->Test.aFloat = 3.0f;
testModel->Test.aLong = 4;
testModel->Test.aInt8 = 5;
testModel->Test.auInt8 = 6;
testModel->Test.aInt16 = 7;
testModel->Test.aInt32 = 8;
testModel->Test.aInt64 = 9;
testModel->Test.aBool = true;
testModel->Test.aAsciiCharPtr = "ascii string 1";

time_t now;
time(&now);
testModel->Test.aDateTimeOffset = GetDateTimeOffset(now);

EDM_GUID guid = { { 0x00, 0x01, 0x02, 0x03, 0x04, 0x05, 0x06, 0x07, 0x08, 0x09, 0x0A, 0x0B, 0x0C, 0x0D, 0x0E, 0x0F } };
testModel->Test.aGuid = guid;

unsigned char binaryArray[3] = { 0x01, 0x02, 0x03 };
EDM_BINARY binaryData = { sizeof(binaryArray), &binaryArray };
testModel->Test.aBinary = binaryData;

SendAsync(iotHubClientHandle, (const void*)&(testModel->Test));
```

Temel olarak, size bir değer her üyesine atama **Test** yapısı ve ardından çağırma **SendAsync** göndermek için **Test** buluta veri olayı. **SendAsync** tek bir veri olayı, IOT Hub'ına gönderir yardımcı işlevdir:

```
void SendAsync(IOTHUB_CLIENT_LL_HANDLE iotHubClientHandle, const void *dataEvent)
{
    unsigned char* destination;
    size_t destinationSize;
    if (SERIALIZE(&destination, &destinationSize, *(const unsigned char*)dataEvent) ==
    {
        // null terminate the string
        char* destinationAsString = (char*)malloc(destinationSize + 1);
        if (destinationAsString != NULL)
        {
            memcpy(destinationAsString, destination, destinationSize);
            destinationAsString[destinationSize] = '\0';
            IOTHUB_MESSAGE_HANDLE messageHandle = IoTHubMessage_CreateFromString(destinationAsString);
            if (messageHandle != NULL)
            {
                IoTHubClient_SendEventAsync(iotHubClientHandle, messageHandle, sendCallback, (void*)0);

                IoTHubMessage_Destroy(messageHandle);
            }
            free(destinationAsString);
        }
        free(destination);
    }
}
```

Bu işlev, verilen veri olay serileştirir ve IOT kullanarak Hub'ına gönderir **IoTHubClient\_SendEventAsync**. Önceki makalede ele alınan aynı kodu budur (**SendAsync** mantığı kullanışlı bir işlevdeki yalıtır).

Önceki kod içinde kullanılan bir diğer yardımcı işlevdir **GetDateTimeOffset**. Bu işlev belirli bir zaman türünde bir değer dönüşümler **EDM\_tarih\_zaman\_UZAKLIĞI**:

```
EDM_DATE_TIME_OFFSET GetDateTimeOffset(time_t time)
{
    struct tm newTime;
    gmtime_s(&newTime, &time);
    EDM_DATE_TIME_OFFSET dateTimeOffset;
    dateTimeOffset.dateTime = newTime;
    dateTimeOffset.fractionalSecond = 0;
    dateTimeOffset.hasFractionalSecond = 0;
    dateTimeOffset.hasTimeZone = 0;
    dateTimeOffset.timeZoneHour = 0;
    dateTimeOffset.timeZoneMinute = 0;
    return dateTimeOffset;
}
```

Bu kodu çalıştırmak, şu iletiyi IOT Hub'ına gönderilir:

```
{"aDouble":1.100000000000000, "aInt":2, "aFloat":3.000000, "aLong":4, "aInt8":5, "auInt8":6, "aInt16":7, "aInt32":8, "aInt64":9, "aBool":true, "aAsciiCharPtr":"ascii string 1", "aDateTimeOffset":"2015-09-14T21:18:21Z", "aGuid":"00010203-0405-0607-0809-0A0B0C0D0E0F", "aBinary":"AQID"}
```

Oluşturulan biçimi JSON seri hale getirme unutmayın tarafından **seri hale getirici** kitaplığı. Ayrıca her üyenin seri hale getirilmiş JSON nesnesinin üyelerini eşleştiğini unutmayın **TestType** biz bizim modelde tanımlanmış. Değerleri de tam olarak kod içinde kullanılan eşleştiğinden. Ancak, ikili verileri base64 ile kodlanmış olduğuna dikkat edin: "AQID" olan base64 kodlamasını {0x01, 0x02, 0x03}.

Bu örnek kullanmanın avantajı gösterir **seri hale getirici** kitaplığı--etkinleştirir JSON seri hale getirme uygulamamızdaki açıkça uğraşmanız gerek kalmadan buluta göndermemizi. Tüm hakkında endişelenmeye gerek sahibiz ayarlanması veri olaylarını değerlerini bizim modeli ve buluta olayları göndermek için basit API'ler çağırma.

Bu bilgi ile biz desteklenen veri türleri, (sizi bile başka karmaşık türler içindeki karmaşık tür dahil olabilir) karmaşık türleri dahil olmak üzere çeşitli içeren modellerinin tanımlayabilirsiniz. Ancak, kullanıcı tarafından oluşturulan JSON seri hale getirilmiş yukarıdaki örnekte önemli noktanız getirir. *Nasıl* size verilerle göndereceğiz **seri hale getirici** kitaplığı belirler tam olarak nasıl JSON biçimi. Bu belirli ne şu sonraki konulara değineceğiz noktasıdır.

## <a name="more-about-serialization"></a>Seri hale getirme hakkında daha fazla bilgi
Önceki bölümde tarafından oluşturulan çıkış vurgular **seri hale getirici** kitaplığı. Bu bölümde, nasıl kitaplığı veri serileştirir ve seri hale getirme API'leri kullanılarak bu davranışı nasıl kontrol edebilir açıklayacağız.

Seri hale getirme tartışma ilerletmek için biz thermostat üzerinde temel alan yeni bir modeli çalışmak. İlk olarak, şimdi biz adresine çalıştığınız senaryoya bazı arka plan sağlayın.

Sıcaklık ve nem ölçer thermostat model istiyoruz. IOT Hub'ına farklı gönderilmek üzere her veri parçası geçiyor. Varsayılan olarak, thermostat ingresses sıcaklık olay 2 dakikada; bir nem her 15 dakikada ingressed olayıdır. Her iki olay ingressed olduğunda karşılık gelen sıcaklık veya nem ölçülen süreyi gösterir bir zaman damgası dahil etmeniz gerekir.

Bu senaryo verildiğinde, veri modeli için iki farklı yol göstermek ve etkisi açıklayacağız modelleme serileştirilmiş çıktı sahiptir.

### <a name="model-1"></a>Model 1
Önceki senaryoda destekleyen bir model ilk sürümü şöyledir:

```
BEGIN_NAMESPACE(Contoso);

DECLARE_STRUCT(TemperatureEvent,
int, Temperature,
EDM_DATE_TIME_OFFSET, Time);

DECLARE_STRUCT(HumidityEvent,
int, Humidity,
EDM_DATE_TIME_OFFSET, Time);

DECLARE_MODEL(Thermostat,
WITH_DATA(TemperatureEvent, Temperature),
WITH_DATA(HumidityEvent, Humidity)
);

END_NAMESPACE(Contoso);
```

Model iki veri olaylarını içerdiğini unutmayın: **sıcaklık** ve **nem**. Önceki örneklerde farklı olarak, her olay kullanılarak tanımlanmış bir yapı türünde **DECLARE\_YAPISI**. **TemperatureEvent** ısı ölçümü ve zaman damgası; içerir **HumidityEvent** nem ölçümü ve zaman damgası içeriyor. Bu model bize yukarıda açıklanan senaryo için veri modeli için doğal bir yol sağlar. Bir olay buluta gönderirken ya da sıcaklık/zaman damgası veya bir nem/zaman damgası çifti göndereceğiz.

Aşağıdaki gibi kod kullanarak buluta sıcaklık olay gönderebiliriz:

```
time_t now;
time(&now);
thermostat->Temperature.Temperature = 75;
thermostat->Temperature.Time = GetDateTimeOffset(now);

unsigned char* destination;
size_t destinationSize;
if (SERIALIZE(&destination, &destinationSize, thermostat->Temperature) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

Biz sabit kodlanmış değerler sıcaklık ve nem örnek kodda kullanır, ancak biz aslında bu değerleri thermostat üzerinde karşılık gelen algılayıcı örnekleyerek alma olduğunu düşünün.

Yukarıdaki kullanan kodu **GetDateTimeOffset** önceden sunulan Yardımcısı. Clear sonraki olacak nedeniyle, bu kod açıkça seri hale getirme ve olay gönderme görevi ayırır. Önceki kod sıcaklık olay bir arabelleğe serileştirir. Ardından, **sendMessage** yardımcı işlevi (dahil **simplesample\_amqp**), IOT Hub'ına olay gönderir:

```
static void sendMessage(IOTHUB_CLIENT_HANDLE iotHubClientHandle, const unsigned char* buffer, size_t size)
{
    static unsigned int messageTrackingId;
    IOTHUB_MESSAGE_HANDLE messageHandle = IoTHubMessage_CreateFromByteArray(buffer, size);
    if (messageHandle != NULL)
    {
        IoTHubClient_SendEventAsync(iotHubClientHandle, messageHandle, sendCallback, (void*)(uintptr_t)messageTrackingId);

        IoTHubMessage_Destroy(messageHandle);
    }
    free((void*)buffer);
}
```

Bu kod bir alt kümesidir **SendAsync** biz üzerine yeniden buraya gidin olmaz şekilde önceki bölümde açıklanan Yardımcısı.

Biz sıcaklık olay göndermek için önceki kod çalıştırdığınızda, olayın serileştirilmiş bu formu IOT Hub'ına gönderilir:

```
{"Temperature":75, "Time":"2015-09-17T18:45:56Z"}
```

Biz türü olan bir sıcaklık gönderiyorsanız **TemperatureEvent** ve bu yapı içeren bir **sıcaklık** ve **zaman** üyesi. Bu, doğrudan seri duruma getirilmiş verilerde yansıtılır.

Benzer şekilde, biz bu kodu nem olayla gönderebilirsiniz:

```
thermostat->Humidity.Humidity = 45;
thermostat->Humidity.Time = GetDateTimeOffset(now);
if (SERIALIZE(&destination, &destinationSize, thermostat->Humidity) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

IOT Hub'ına gönderilen serileştirilmiş form aşağıdaki gibi görünür:

```
{"Humidity":45, "Time":"2015-09-17T18:45:56Z"}
```

Yeniden, bu beklenen biçimde değil.

Bu model ile nasıl ek olaylar hayal edebildiğiniz kolayca eklenebilir. Kullanarak daha fazla yapıları tanımladığınız **DECLARE\_YAPISI**ve karşılık gelen olay kullanarak modeli dahil **ile\_veri**.

Şimdi, model böylece aynı verileri içerir, ancak farklı bir yapıya sahip değiştirelim.

### <a name="model-2"></a>Model 2
Bu alternatif model yukarıdakine göz önünde bulundurun:

```
DECLARE_MODEL(Thermostat,
WITH_DATA(int, Temperature),
WITH_DATA(int, Humidity),
WITH_DATA(EDM_DATE_TIME_OFFSET, Time)
);
```

Bu durumda biz olmadığı anlaşılmış olur **DECLARE\_YAPISI** makroları ve Senaryomuzda basit türler Modelleme Dili kullanarak veri öğelerinden yalnızca tanımlama.

Yalnızca şu anda şirketinizdeki Yoksay **zaman** olay. Bu aralık ile giriş koda işte **sıcaklık**:

```
time_t now;
time(&now);
thermostat->Temperature = 75;

unsigned char* destination;
size_t destinationSize;
if (SERIALIZE(&destination, &destinationSize, thermostat->Temperature) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

Bu kod, IOT Hub'ına aşağıdaki serileştirilmiş olay gönderir:

```
{"Temperature":75}
```

Ve nem olay göndermek için kod aşağıdaki gibidir:

```
thermostat->Humidity = 45;
if (SERIALIZE(&destination, &destinationSize, thermostat->Humidity) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

Bu kod bu IOT Hub'ına gönderir:

```
{"Humidity":45}
```

Şu ana kadar var. yine de hiçbir beklenmeyen durumları. Şimdi SERİLEŞTİRME makrosu nasıl kullanırız değiştirelim.

**SERİLEŞTİRME** makrosu bağımsız değişken birden çok veri olaylarını alabilir. Bu seri hale getirmemize sağlar **sıcaklık** ve **nem** birlikte olay ve tek çağrıda IOT Hub'ına gönderebilirsiniz:

```
if (SERIALIZE(&destination, &destinationSize, thermostat->Temperature, thermostat->Humidity) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

Bu kodun sonucunda iki veri olayları IOT Hub'ına gönderilir olduğunu tahmin:

[

{"Sıcaklık": 75},

{"Nem": 45}

]

Diğer bir deyişle, bu kod gönderme ile aynı olduğunu bekleyebilirsiniz **sıcaklık** ve **nem** ayrı olarak. Her iki olayları iletmek için yalnızca kolaylık sağlamak olan **SERİLEŞTİRME** aynı çağrısında. Ancak, bu durumda değil. Bunun yerine, yukarıdaki kod bu tek bir veri olayı, IOT Hub'ına gönderir:

{"Sıcaklık": 75 "nem": 45}

Modelimizi tanımladığından Bu tuhaf görünebilir **sıcaklık** ve **nem** iki olarak *ayrı* olayları:

```
DECLARE_MODEL(Thermostat,
WITH_DATA(int, Temperature),
WITH_DATA(int, Humidity),
WITH_DATA(EDM_DATE_TIME_OFFSET, Time)
);
```

Daha noktasına Biz bu olayları modelini alamadık nerede **sıcaklık** ve **nem** aynı yapısında şunlardır:

```
DECLARE_STRUCT(TemperatureAndHumidityEvent,
int, Temperature,
int, Humidity,
);

DECLARE_MODEL(Thermostat,
WITH_DATA(TemperatureAndHumidityEvent, TemperatureAndHumidity),
);
```

Bu model kullansaydık anlamak daha kolay olacaktır nasıl **sıcaklık** ve **nem** aynı serileştirilmiş iletisinde gönderilebilir. Her iki veri olaylarını geçirdiğinizde bu şekilde çalıştığını neden ancak onu Temizle olmayabilir **SERİLEŞTİRME** kullanarak modeli 2.

Bu davranış, varsayımlar, biliyorsanız anlamak daha kolay **seri hale getirici** kitaplığı yapma. Bu bizim modele geri edelim anlamlı için:

```
DECLARE_MODEL(Thermostat,
WITH_DATA(int, Temperature),
WITH_DATA(int, Humidity),
WITH_DATA(EDM_DATE_TIME_OFFSET, Time)
);
```

Bu model nesne yönelimli koşullarında düşünün. Bu durumda biz fiziksel bir aygıtı (thermostat) modelleme ve o aygıtı gibi özniteliklerini içerir **sıcaklık** ve **nem**.

Modelimizi aşağıdaki gibi kod ile tüm durumunu gönderebiliriz:

```
if (SERIALIZE(&destination, &destinationSize, thermostat->Temperature, thermostat->Humidity, thermostat->Time) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

Sıcaklık değerlerini varsayıldığında, nem ve saatini ayarlayın, biz bu IOT Hub'ına gönderilen gibi bir olay görürsünüz:

```
{"Temperature":75, "Humidity":45, "Time":"2015-09-17T18:45:56Z"}
```

Bazı durumlarda, yalnızca göndermek isteyebilirsiniz *bazı* (Bu, çok sayıda veri olaylarını modelinizi içeriyorsa, özellikle true) buluta modelin özellikleri. Önceki örneğimizde gibi yalnızca bir alt veri olayların göndermek yararlıdır:

```
{"Temperature":75, "Time":"2015-09-17T18:45:56Z"}
```

Tanımlanan olarak, bu tam olarak aynı serileştirilmiş olay oluşturur bir **TemperatureEvent** ile bir **sıcaklık** ve **zaman** üye, biz ile 1 model gibi. Bu durumda biz adlı olduğundan farklı bir model (model 2) kullanarak tam olarak aynı serileştirilmiş olayı Oluştur bulduk **SERİLEŞTİRME** farklı bir şekilde.

Birden çok veri olaylarını geçirirseniz olan en önemli nokta **SERİLEŞTİRME,** sonra her olay tek bir JSON nesnesi özelliğinde olduğunu varsayar.

En iyi yaklaşımı, ve modelinizi hakkında nasıl düşündüğünüz bağlıdır. Buluta "olayları" gönderiyorsanız ve her olay tanımlanmış bir özellik kümesi içeriyorsa, ilk yaklaşım çok algılama hale getirir. Bu durumda kullanacağınız **DECLARE\_YAPISI** her olay yapısını tanımlar ve bunları modelinizi dahil **ile\_veri** makrosu. Daha sonra ilk yukarıdaki örnekte yaptığımız gibi her olay gönderin. Bu yaklaşım, yalnızca bir tek veri olayı geçip geçmeyeceğini **seri hale GETİRİCİ**.

Nesne odaklı bir şekilde modelinizde hakkında düşünüyorsanız, ikinci yaklaşımın size uygun. Bu durumda, öğeleri tanımlanan kullanarak **ile\_veri** nesnenizin "Özellikler". Her alt olayların geçirdiğiniz **SERİLEŞTİRME** , bağlı olarak, istediğiniz buluta göndermek için "nesnenin" durumu ne kadarının ister.

Doğru veya yanlış bir bağlanamadı yaklaşımdır. Yalnızca, nasıl unutmayın **seri hale getirici** kitaplığı çalışır ve çekme gereksinimlerine en uygun modelleme yaklaşım.

## <a name="message-handling"></a>İleti işleme
Şu ana kadar bu makalede, IOT Hub'ına gönderen olayları ele alınan yalnızca ve iletileri alma ele kurmadı. Bu, biz iletileri alma hakkında bilmeniz gerekenler için nedeni büyük ölçüde içinde ele alınmış bir [önceki makalesi](iot-hub-device-sdk-c-intro.md). Bu makaleden bir ileti geri çağırma işlevini kaydederek iletileri işleme geri çağırma:

```
IoTHubClient_SetMessageCallback(iotHubClientHandle, IoTHubMessage, myWeather)
```

Ardından, bir ileti alındığında çağrılan geri çağırma işlevi de yazın:

```
static IOTHUBMESSAGE_DISPOSITION_RESULT IoTHubMessage(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
{
    IOTHUBMESSAGE_DISPOSITION_RESULT result;
    const unsigned char* buffer;
    size_t size;
    if (IoTHubMessage_GetByteArray(message, &buffer, &size) != IOTHUB_MESSAGE_OK)
    {
        printf("unable to IoTHubMessage_GetByteArray\r\n");
        result = EXECUTE_COMMAND_ERROR;
    }
    else
    {
        /*buffer is not zero terminated*/
        char* temp = malloc(size + 1);
        if (temp == NULL)
        {
            printf("failed to malloc\r\n");
            result = EXECUTE_COMMAND_ERROR;
        }
        else
        {
            memcpy(temp, buffer, size);
            temp[size] = '\0';
            EXECUTE_COMMAND_RESULT executeCommandResult = EXECUTE_COMMAND(userContextCallback, temp);
            result =
                (executeCommandResult == EXECUTE_COMMAND_ERROR) ? IOTHUBMESSAGE_ABANDONED :
                (executeCommandResult == EXECUTE_COMMAND_SUCCESS) ? IOTHUBMESSAGE_ACCEPTED :
                IOTHUBMESSAGE_REJECTED;
            free(temp);
        }
    }
    return result;
}
```

Bu uygulaması, **IoTHubMessage** modelinizdeki her eylem için belirli bir işlevi çağırır. Örneğin bu eylemi modelinizi tanımlar:

```
WITH_ACTION(SetAirResistance, int, Position)
```

Bu imza işleviyle tanımlamanız gerekir:

```
EXECUTE_COMMAND_RESULT SetAirResistance(ContosoAnemometer* device, int Position)
{
    (void)device;
    (void)printf("Setting Air Resistance Position to %d.\r\n", Position);
    return EXECUTE_COMMAND_SUCCESS;
}
```

**SetAirResistance** aygıtınıza ileti gönderildiğinde daha sonra çağrılır.

Ne biz açıklandığı henüz henüz iletisinin seri duruma getirilmiş sürüm ne aşağıdaki gibidir. Diğer bir deyişle, göndermek istiyorsanız bir **SetAirResistance** Cihazınızı iletiye, ne görünüm ister?

Bir cihaza bir ileti gönderiyorsanız, Azure IOT hizmeti SDK bunu. Hala belirli bir eylemi çağırmak göndermek için hangi dize bilmeniz gerekir. Bir ileti göndermek için genel biçim şu şekilde görünür:

```
{"Name" : "", "Parameters" : "" }
```

Serileştirilmiş bir JSON nesnesi iki özellikleriyle gönderiyorsanız: **adı** (ileti) eylem adı ve **parametreleri** Bu eylem parametrelerini içerir.

Örneğin, çağrılacak **SetAirResistance** bu iletiyi bir cihaza gönderebilirsiniz:

```
{"Name" : "SetAirResistance", "Parameters" : { "Position" : 5 }}
```

Eylem adı, modelde tanımlanan bir eylem tam olarak eşleşmelidir. Parametre adları de eşleşmelidir. Ayrıca, büyük küçük harfe duyarlılığın unutmayın. **Ad** ve **parametreleri** her zaman büyük harfle yazılır. Eylem adı ve modelinizdeki parametreleri büyük küçük harf duyarlı emin olun. Bu örnekte, eylem adı "SetAirResistance" değil "setairresistance" ise.

İki diğer eylemler **TurnFanOn** ve **TurnFanOff** bir aygıta bu iletiler göndererek çağrılır:

```
{"Name" : "TurnFanOn", "Parameters" : {}}
{"Name" : "TurnFanOff", "Parameters" : {}}
```

Bu bölümde alma ve gönderme olayları zaman iletileri ile bilmeniz gereken her şeyi açıklanan **seri hale getirici** kitaplığı. Devam etmeden önce şimdi ne kadar büyük modelinizi denetleyebilir yapılandırabileceğiniz bazı parametreler kapsar.

## <a name="macro-configuration"></a>Makro yapılandırma
Kullanıyorsanız, **seri hale getirici** SDK'sı dikkat edilmesi gereken önemli bir kısmını azure-c-paylaşılan-yardımcı programı Kitaplığı'nda bulunan kitaplık.
GitHub--özyinelemeli seçeneğini kullanarak Azure-IOT-sdk-c depodan klonlanmış varsa, bu paylaşılan yardımcı kitaplık yer alır:

```
.\\c-utility
```

Kitaplık klonlanmış değil, bulabilirsiniz [burada](https://github.com/Azure/azure-c-shared-utility).

Paylaşılan yardımcı kitaplıkta şu klasörü bulacaksınız:

```
azure-c-shared-utility\\macro\_utils\_h\_generator.
```

Adlı bir Visual Studio çözümü bu klasörde **makrosu\_yardımcı programları\_h\_generator.sln**:

  ![](media/iot-hub-device-sdk-c-serializer/01-macro_utils_h_generator.PNG)

Bu çözümdeki programın oluşturduğu **makrosu\_utils.h** dosya. Varsayılan makrosu yoktur\_SDK ile birlikte bulunan utils.h dosyası. Bu çözüm, bu parametrelere göre üst bilgi dosyasını yeniden oluşturun ve bazı parametreler değiştirmek sağlar.

İle ilgili olarak iki anahtar parametreleri **nArithmetic** ve **nMacroParameters** makro içinde bulunan bu iki satır içinde tanımlanan\_utils.tt:

```
<#int nArithmetic=1024;#>
<#int nMacroParameters=124;/*127 parameters in one macro deﬁnition in C99 in chapter 5.2.4.1 Translation limits*/#>

```

Bu değerler SDK ile birlikte dahil varsayılan parametreleridir. Her bir parametreyi şu anlama sahiptir:

* nMacroParameters – denetimleri içinde bir DECLARE olabilir kaç parametreleri\_modeli makro tanımı.
* nArithmetic – üyeleri bir model izin verilen toplam sayısını denetler.

Bunlar denetlemek için bu parametreler önemli modelinizi ne kadar büyük olabilir nedenidir. Örneğin, bu model tanımı göz önünde bulundurun:

```
DECLARE_MODEL(MyModel,
WITH_DATA(int, MyData)
);
```

Daha önce belirtildiği gibi **DECLARE\_MODEL** yalnızca C makro. Model adını ve **ile\_veri** deyimi (henüz başka bir makrosu) olan parametrelerinin **DECLARE\_modeli**. **nMacroParameters** kaç parametreleri eklenebilir tanımlar **DECLARE\_modeli**. Etkili bir şekilde sağlayabilirsiniz kaç tane veri olay ve eylem bildirimleri tanımlar. Bu nedenle, 124 varsayılan sınır ile bu 60 eylemleri ve veri olayları birleşimi ile bir model tanımlayabilirsiniz anlamına gelir. Bu sınırı aşarsanız çalışırsanız, aşağıdakine benzer derleyici hataları alırsınız:

  ![](media/iot-hub-device-sdk-c-serializer/02-nMacroParametersCompilerErrors.PNG)

**NArithmetic** parametredir uygulamanızı'den daha fazla Internet'in iç makro dili hakkında.  Modelinizde, olabilir üyeleri toplam sayısını denetlediği de dahil olmak üzere **DECLARE_STRUCT** makroları. Bu gibi derleyici hataları görmeye başlayacaksınız sonra artırmayı denemelisiniz **nArithmetic**:

   ![](media/iot-hub-device-sdk-c-serializer/03-nArithmeticCompilerErrors.PNG)

Bu parametreleri değiştirmek istiyorsanız, makrosu değiştirin\_utils.tt dosya, makrosu derlenir\_yardımcı programları\_h\_generator.sln çözüm ve derlenmiş programını çalıştırın. Bunu yaptığınızda bunu yeni bir makro\_utils.h dosya oluşturulur ve yerleştirilir.\\ Ortak\\Inc dizini.

Makro yeni sürümü kullanmak için\_utils.h, kaldırma **seri hale getirici** NuGet paketi çözümünüzden ve onun yerine dahil **seri hale getirici** Visual Studio projesi. Bu seri hale getirici kitaplığı karşı Kaynak Kodu derlemek kodunuzu sağlar. Bu güncelleştirilmiş makrosu içerir\_utils.h. Bunu yapmak istiyorsanız, **simplesample\_amqp**, seri hale getirici kitaplığı NuGet paketi çözümden kaldırarak Başlat:

   ![](media/iot-hub-device-sdk-c-serializer/04-serializer-github-package.PNG)

Daha sonra bu projeyi Visual Studio çözümünüzü ekleyin:

> . \\c\\seri hale getirici\\yapı\\windows\\serializer.vcxproj
> 
> 

İşiniz bittiğinde, çözümünüzün aşağıdaki gibi görünmelidir:

   ![](media/iot-hub-device-sdk-c-serializer/05-serializer-project.PNG)

Şimdi ne zaman, derleme güncelleştirilmiş makrosu çözümünüzü\_utils.h ikili dosyanız yer almaktadır.

Bu değerleri yeterince artırma derleyici sınırları aşabilir unutmayın. Bu noktaya **nMacroParameters** endişelenmeniz kullanılacak ana parametresidir. En az 127 parametrelerden biri izin verilen bir makro tanımı'nda C99 spec belirtir. Microsoft derleyici spec tam olarak aşağıdaki (ve 127 sınırı), artırmak mümkün olmayacaktır **nMacroParameters** varsayılan ötesinde. Diğer derleyiciler, bunu yapmak izin verebilir (örneğin, GNU derleyici daha yüksek bir sınır destekler).

Şu ana kadar biz neredeyse koduyla yazma hakkında bilmeniz gereken her şeyi kapsamdaki **seri hale getirici** kitaplığı. Tamamlanırken önce şimdi bazı konular hakkında merak ediyor önceki makalelerden yeniden ziyaret.

## <a name="the-lower-level-apis"></a>Alt düzey API'ları
Bu makale üzerinde odaklanmış örnek uygulama **simplesample\_amqp**. Bu örnek daha üst düzey (olmayan-"tümü") kullanan iletileri olayları göndermek ve almak için API'ler. Bu API'leri kullanırsanız, hem olayları ileti gönderme ve alma, mvc'deki bir arka plan iş parçacığı çalıştırır. Ancak, bu arka plan iş parçacığı ortadan kaldırmak ve açık denetim olayları gönderirken veya buluttan iletilerini almak için alt düzey (Tümü) API'ları kullanabilirsiniz.

Bölümünde açıklandığı gibi bir [önceki makale](iot-hub-device-sdk-c-iothubclient.md), üst düzey API'lerinin oluşan bir dizi işlevleri vardır:

* IoTHubClient\_CreateFromConnectionString
* IoTHubClient\_SendEventAsync
* IoTHubClient\_SetMessageCallback
* IoTHubClient\_yok

Bu API'leri örneklerde gösterildiği **simplesample\_amqp**.

Ayrıca vardır benzer bir alt düzey API kümesi.

* IoTHubClient\_ÜM\_CreateFromConnectionString
* IoTHubClient\_ÜM\_SendEventAsync
* IoTHubClient\_ÜM\_SetMessageCallback
* IoTHubClient\_ÜM\_yok

Alt düzey API'ları önceki makalelerinde açıklandığı gibi tamamen aynı şekilde çalıştığını unutmayın. Gönderen olaylar ve alıcı iletileri işlemek için bir arka plan iş parçacığı isterseniz ilk dizi API kullanabilirsiniz. Açık denetim zaman göndermek ve IOT Hub'ından veri almak istiyorsanız, ikinci dizi API kullanın. API iş ayarlayın ya da eşit ile yanı sıra **seri hale getirici** kitaplığı.

Alt düzey API'ları ile nasıl kullanıldığı bir örnek için **seri hale getirici** kitaplığı bkz **simplesample\_http** uygulama.

## <a name="additional-topics"></a>Ek konu başlıkları
Söz değerinde birkaç diğer yeniden işleme, diğer aygıt kimlik bilgisi ve yapılandırma seçeneklerini kullanarak özellik konulardır. Tüm konuları ele bunlar bir [önceki makalede](iot-hub-device-sdk-c-iothubclient.md). Bu özelliklerin tümü, ile aynı şekilde çalıştığını ana nokta olan **seri hale getirici** kitaplığı ile göründükleri **IoTHubClient** kitaplığı. Örneğin, Özellikler modelinizin dışında bir olaya bağlamak istiyorsanız, kullandığınız **IoTHubMessage\_özellikleri** ve **harita**\_**örnek**, daha önce açıklanan aynı şekilde:

```
MAP_HANDLE propMap = IoTHubMessage_Properties(message.messageHandle);
sprintf_s(propText, sizeof(propText), "%d", i);
Map_AddOrUpdate(propMap, "SequenceNumber", propText);
```

Gelen olay oluşturulup oluşturulmadığını **seri hale getirici** kitaplığı veya el ile kullanılarak oluşturulan **IoTHubClient** kitaplığı önemli değildir.

Kullanarak alternatif aygıt kimlik bilgisi için **IoTHubClient\_ÜM\_oluşturma** da çalışır olarak **IoTHubClient\_CreateFromConnectionString** için ayrılırken bir **IOTHUB\_istemci\_İŞLEMEK**.

Son olarak, kullanıyorsanız, **seri hale getirici** kitaplığı ile yapılandırma seçeneklerini ayarlayabilirsiniz **IoTHubClient\_ÜM\_SetOption** kullanırkenyaptığınızgibi**IoTHubClient** kitaplığı.

İçin benzersiz bir özelliğini **seri hale getirici** kitaplığı olan API'leri başlatma. Kitaplığı ile çalışmaya başlamadan önce çağırmalısınız **seri hale getirici\_init**:

```
serializer_init(NULL);
```

Yalnızca arama yapmadan önce bu yapılır **IoTHubClient\_CreateFromConnectionString**.

Benzer şekilde, bitirdiğinizde kullanmaktır yaptığınız son çağrının kitaplıkla çalışırken, **seri hale getirici\_deinit**:

```
serializer_deinit();
```

Aksi takdirde, yukarıda listelenen diğer özelliklerin tümü aynı iş **seri hale getirici** kitaplığı göründükleri **IoTHubClient** kitaplığı. Bu konular hakkında daha fazla bilgi için bkz: [önceki makalede](iot-hub-device-sdk-c-iothubclient.md) bu serideki.

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede benzersiz yönleri ayrıntılı olarak **seri hale getirici** bulunan kitaplık **C için Azure IOT cihaz SDK'sı**. Modelleri IOT Hub'ından ileti alıp olayları göndermek için nasıl kullanılacağını bir iyi anlamış olmanız gerekir bilgi ile sağlanan.

Bu üç bölümlük ile uygulamalar geliştirme konusunda da sonucuna **C için Azure IOT cihaz SDK'sı**. Bu, yalnızca başlamanıza ancak nasıl API'leri işe kapsamlı olarak anlamayı vermek için yeterli bilgi olmalıdır. Ek bilgi için burada kapsanmayan SDK'ın bazı örnekler vardır. Aksi takdirde, [SDK Belgeleri](https://github.com/Azure/azure-iot-sdk-c) ek bilgi için iyi bir kaynaktır.

IOT Hub için geliştirme hakkında daha fazla bilgi için bkz: [Azure IOT SDK'ları][lnk-sdks].

Daha fazla IOT hub'ı özelliklerini keşfetmek için bkz:

* [AI ile Azure IOT kenar sınır cihazları için dağıtma][lnk-iotedge]

[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-iotedge]: ../iot-edge/tutorial-simulate-device-linux.md
