---
title: C - seri hale getirici için Azure IOT cihaz SDK'sı | Microsoft Docs
description: Bir IOT hub ile iletişim kuran cihaz uygulamaları oluşturmak için Azure IOT cihaz SDK'sını C için seri hale getirici kitaplıkta kullanma
author: yzhong94
ms.service: iot-hub
services: iot-hub
ms.devlang: c
ms.topic: conceptual
ms.date: 09/06/2016
ms.author: yizhon
ms.openlocfilehash: 0a7e30be374ae5095e206ce0e519e51bb58f1f00
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60399270"
---
# <a name="azure-iot-device-sdk-for-c--more-about-serializer"></a>Azure IOT cihaz SDK'sını C için– seri hale getirici hakkında daha fazla bilgi

Kullanıma sunulan bu serinin ilk makalesinde [C için Azure IOT cihaz SDK'sına giriş](iot-hub-device-sdk-c-intro.md). Daha ayrıntılı bir açıklaması olan sonraki makalede sağlanan [C--sı: Iothubclient için Azure IOT cihaz SDK'sını](iot-hub-device-sdk-c-iothubclient.md). Bu makalede SDK'ın kapsamı kalan bileşenin daha ayrıntılı bir açıklama sağlayarak tamamlanır: **seri hale getirici** kitaplığı.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

Giriş makalesi, nasıl kullanılacağını açıklanan **seri hale getirici** kitaplığını kullanarak olayları göndermek ve IOT Hub'ından iletiler alan. Bu makalede, biz bu tartışma ile veri modelinizi nasıl daha eksiksiz bir açıklaması sağlayarak genişletmek **seri hale getirici** makro dili. Makale ayrıca nasıl kitaplığı iletileri serileştiren hakkında daha fazla ayrıntı içerir (ve bazı durumlarda, serileştirme davranışı nasıl kontrol edebilir). Oluşturduğunuz modelleri boyutunu belirleyen değiştirebileceğiniz bazı parametreler de açıklayacağız.

Son olarak, makalenin önceki makalelerde ileti ve özellik işleme gibi bazı konuları gelişimin. Öğrenin, bu özellikleri iş aynı şekilde kullandığımız **seri hale getirici** kitaplığı ile göründükleri **sı: Iothubclient** kitaplığı.

Bu makalede açıklanan her şeyi dayanır **seri hale getirici** SDK örnekleri. Örneği takip etmek istiyorsanız, bkz. **simplesample\_amqp** ve **simplesample\_http** c için Azure IOT cihaz SDK'sını eklenmiş olan uygulamalar

Bulabilirsiniz [ **C için Azure IOT cihaz SDK'sını** ](https://github.com/Azure/azure-iot-sdk-c) API'de GitHub deposu ve görünüm ayrıntılarını [C API Başvurusu](https://docs.microsoft.com/azure/iot-hub/iot-c-sdk-ref/).

## <a name="the-modeling-language"></a>Modelleme Dili

[C için Azure IOT cihaz SDK'sını](iot-hub-device-sdk-c-intro.md) sunulan bu seriyi makalesinde **C için Azure IOT cihaz SDK'sını** aracılığıyla sağlanan örnek dil modelleme **simplesample\_ amqp** uygulama:

```C
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

Gördüğünüz gibi modelleme dili C makroları temel alır. Her zaman tanımınız ile başlayan **başlangıç\_ad alanı** ve her zaman bitmelidir **son\_ad alanı**. Ad alanı, şirketinizin veya bu örnekte, üzerinde çalıştığınız proje olduğu gibi ad yaygındır.

Ad alanı içinde unsurları olan model tanımı. Bu durumda, bir anemometer için tek bir model yok. Bir kez daha, model herhangi bir ad verilebilir, ancak genellikle cihaz veya IOT Hub ile exchange istediğiniz veri türü için model olarak adlandırılır.  

Modelleri içeren bir tanımı IOT hub'ına giriş olaylarını yapabilir ( *veri*) alma IOT Hub'ından iletiler yanı sıra ( *eylemleri*). Örnekte görebileceğiniz gibi olaylar bir tür ve ad sahiptir; Eylemler, bir ad ve isteğe bağlı parametreler (her bir tür) sahip.

Ne Bu örnekte gösterilmiştir değil SDK tarafından desteklenen ek veri türleridir. Biz de şimdi ele alacağız.

> [!NOTE]
> IOT hub'ı, bir cihazın gönderdiği olarak veri başvurduğu *olayları*olarak Modelleme Dili başvurduğu korurken *veri* (kullanılarak tanımlanan **WITH_DATA**). Benzer şekilde, IOT Hub cihazları gönderdiğiniz veri başvurduğu *iletileri*olarak Modelleme Dili başvurduğu korurken *eylemleri* (kullanılarak tanımlanan **WITH_ACTION**). Bu terimleri bu makalede birbirlerinin yerine kullanılabilir dikkat edin.
> 
> 

## <a name="supported-data-types"></a>Desteklenen veri türleri

Aşağıdaki veri türleri ile oluşturulan modeller desteklenir **seri hale getirici** kitaplığı:

| Tür | Açıklama |
| --- | --- |
| double |çift duyarlıklı kayan nokta sayısı |
| int |32 bit tamsayı |
| float |tek duyarlıklı kayan nokta sayısı |
| long |uzun tamsayı |
| int8\_t |8 bit tamsayı |
| Int16\_t |16 bit tam sayı |
| Int32\_t |32 bit tamsayı |
| Int64\_t |64 bit tamsayı |
| bool |boole |
| ascii\_char\_ptr |ASCII dizesi |
| EDM\_DATE\_TIME\_OFFSET |Tarih Saat farkı |
| EDM\_GUID |GUID |
| EDM\_BINARY |binary |
| DECLARE\_STRUCT |Karmaşık veri türü |

Son veri türüyle başlayalım. **DECLARE\_yapı** gruplandırmaları olan diğer temel eleman türleri ve karmaşık veri türlerini tanımlamanızı sağlar. Bu gruplandırmalar bize şuna benzer bir modeli tanımlamak izin ver:

```C
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

Bir tek veri olayı türü modelimizi içeren **TestType**. **TestType** topluca tarafından desteklenmeyen temel türler gösteren birkaç üye içeren karmaşık bir türdür **seri hale getirici** dil modelleme.

Bir modelin bu şekilde IOT hub'a gibi görünen veri göndermek için kod yazabiliriz:

```C
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

Temelde, biz bir değer her üyesine atama **Test** yapısı ve ardından arama **SendAsync** göndermek için **Test** buluta olay verileri. **SendAsync** bir tek veri olayı, IOT Hub'ına gönderir. bir yardımcı işlevdir:

```C
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

Bu işlev, belirtilen veri olay serileştirir ve kullanarak IOT hub'a gönderir **sı: Iothubclient\_SendEventAsync**. Önceki makalelerinde açıklanan aynı kod budur (**SendAsync** kullanışlı işleve mantığı kapsülleyen).

Önceki kod içinde kullanılan diğer bir yardımcı işlev, **GetDateTimeOffset**. Bu işlev, türünde bir değer verilen süre dönüştüren **EDM\_tarih\_zaman\_UZAKLIĞI**:

```C
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

Bu kodu çalıştırırsanız, IOT Hub'ına aşağıdaki ileti gönderilir:

```C
{"aDouble":1.100000000000000, "aInt":2, "aFloat":3.000000, "aLong":4, "aInt8":5, "auInt8":6, "aInt16":7, "aInt32":8, "aInt64":9, "aBool":true, "aAsciiCharPtr":"ascii string 1", "aDateTimeOffset":"2015-09-14T21:18:21Z", "aGuid":"00010203-0405-0607-0809-0A0B0C0D0E0F", "aBinary":"AQID"}
```

Seri hale getirme biçimi JSON'da olduğuna dikkat edin tarafından oluşturulan **seri hale getirici** kitaplığı. Ayrıca her üyenin seri hale getirilmiş JSON nesnesinin üyelerinin eşleştiğini unutmayın **TestType** biz bizim modelde tanımlı. Değerler de tam olarak kod içinde kullanılan eşleşir. Ancak, base64 ile kodlanmış ikili veri olduğuna dikkat edin: "AQID" olan base64 kodlaması {0x01, 0x02, 0x03}.

Bu örnek kullanmanın avantajı gösterir **seri hale getirici** kitaplığı--bağlayabileceğinizi JSON ile seri hale getirme uygulamamızdaki açıkça uğraşmak zorunda kalmadan buluta göndermemizi. Tüm yükleme konusunda endişelenmenize gerek sahibiz ayarı veri olaylarını değerlerini modelimizi ve ardından buluta bu olayları göndermek için basit API'ler çağırma.

Bu bilgileri kullanarak şu desteklenen veri türleri (biz bile diğer karmaşık türler içinde karmaşık türler dahil olabilir) karmaşık türler dahil olmak üzere, aralığını içeren modelleri tanımlayabilirsiniz. Ancak, yukarıdaki örneği tarafından oluşturulan seri hale getirilmiş JSON önemli bir nokta getirir. *Nasıl* verilerle göndereceğiz **seri hale getirici** kitaplığı tam olarak JSON nasıl biçimlendirildiğini belirler. Bu belirli şu sonraki konulara değineceğiz noktasıdır.

## <a name="more-about-serialization"></a>Seri hale getirme hakkında daha fazla bilgi

Tarafından oluşturulan çıktının bir örneği önceki bölümde vurgular **seri hale getirici** kitaplığı. Bu bölümde, nasıl kitaplığı verileri serileştirir ve Serileştirme API'leri kullanarak bu davranışı nasıl denetleyebileceğiniz açıklayacağız.

Serileştirme hakkında bir tartışma ilerletmek için size bir thermostat üzerinde yeni bir model ile çalışırsınız. İlk olarak, şimdi biz adresine çalıştığınız senaryoya bazı arka plan bilgileri sağlar.

Sıcaklık ve nem ölçer bir thermostat model istiyoruz. Her veri parçası farklı IOT Hub'ına gönderilecek geçiyor. Varsayılan olarak, thermostat ingresses sıcaklık olay 2 dakikada; bir nem her 15 dakikada ingressed olayıdır. Her iki olay ingressed olduğunda, karşılık gelen sıcaklık ve nem ölçülmüştür zamanı gösteren bir zaman damgası içermesi gerekir.

Bu senaryoyu göz önünde bulundurulduğunda, biz verilerinizi modellemek için iki farklı şekilde görmenin yanı sıra etkisi açıklayacağız modelleme serileştirilmiş çıktı sahiptir.

### <a name="model-1"></a>Model 1

Ürününün ilk sürümünü önceki senaryoyu destekleyen model şu şekildedir:

```C
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

Model iki veri olaylarını içerdiğine dikkat edin: **Sıcaklık** ve **nem**. Önceki örneklerde farklı olarak, her olay kullanılarak tanımlanmış bir yapı türüdür **DECLARE\_yapı**. **TemperatureEvent** sıcaklık ölçüm ve; zaman damgası içerir **HumidityEvent** nem ölçüm ve bir zaman damgası içerir. Bu model için yukarıdaki senaryoyu verilerinizi modellemek için doğal bir yol sağlıyor. Bir olay buluta gönderdiğinizde ya da bir sıcaklık/zaman damgası veya nem/zaman damgası çifti göndereceğiz.

Biz aşağıdaki gibi bir kod kullanarak bulutta sıcaklık olay gönderebilirsiniz:

```C
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

Biz sıcaklık ve nem örnek kodda sabit kodlu değer kullanır ancak biz aslında bu değerleri sıcaklığının karşılık gelen sensörlerini yeniden örnekleyerek alınırken olduğunu düşünün.

Yukarıdaki kullanan kodu **GetDateTimeOffset** , daha önce sunulan Yardımcısı. Clear sonraki olacak nedeniyle, bu kod açıkça seri hale getirme ve olay gönderme görevini ayırır. Önceki kod sıcaklık olay bir arabelleğe serileştirir. Ardından, **sendMessage** yardımcı işlev, (dahil **simplesample\_amqp**), IOT Hub'ına olay gönderir:

```C
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

Bu kod bir alt kümesidir **SendAsync** biz üzerine yeniden buraya gitmiyor şekilde önceki bölümde açıklanan Yardımcısı.

Bu olayı seri hala getirilmiş biçimini, sıcaklık olay göndermek için önceki kod çalıştırıyoruz, IOT Hub'ına gönderilir:

```C
{"Temperature":75, "Time":"2015-09-17T18:45:56Z"}
```

Türü olan bir sıcaklık gönderiyoruz **TemperatureEvent** ve bu yapı içeren bir **sıcaklık** ve **zaman** üyesi. Bu, serileştirilmiş veriler doğrudan yansıtılır.

Benzer şekilde, ki bu kod bir nem olay gönderebilirsiniz:

```C
thermostat->Humidity.Humidity = 45;
thermostat->Humidity.Time = GetDateTimeOffset(now);
if (SERIALIZE(&destination, &destinationSize, thermostat->Humidity) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

IOT Hub'ına gönderilen seri hale getirilmiş biçimlerini aşağıdaki gibi görünür:

```C
{"Humidity":45, "Time":"2015-09-17T18:45:56Z"}
```

Yeniden beklendiği gibi budur.

Bu modelde, nasıl ek olaylar hayal edebildiğiniz kolayca eklenebilir. Daha fazla yapıları kullanarak tanımladığınız **DECLARE\_yapı**ve modelini kullanarak karşılık gelen bir olay ekleyin **ile\_veri**.

Şimdi, böylece aynı verileri içeren ancak farklı bir yapı modeli şimdi değiştirin.

### <a name="model-2"></a>Model 2

Bu alternatif bir model için yukarıdaki göz önünde bulundurun:

```C
DECLARE_MODEL(Thermostat,
WITH_DATA(int, Temperature),
WITH_DATA(int, Humidity),
WITH_DATA(EDM_DATE_TIME_OFFSET, Time)
);
```

Bu durumda biz anlaşılmış olur **DECLARE\_yapı** makroları ve senaryomuz basit türler Modelleme Dili kullanarak veri öğelerinden yalnızca tanımlarsınız.

Yalnızca şimdilik yoksayın **zaman** olay. Bu kenara ile giriş koda işte **sıcaklık**:

```C
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

```C
{"Temperature":75}
```

Ve nem olay göndermek için kod aşağıdaki gibi görünür:

```C
thermostat->Humidity = 45;
if (SERIALIZE(&destination, &destinationSize, thermostat->Humidity) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

Bu kod, bu IOT Hub'ına gönderir:

```C
{"Humidity":45}
```

Şu ana kadar hala var. artık sürprizle karşılaşmazsınız. Artık SERİLEŞTİRME makrosu nasıl kullandığımızı değiştirelim.

**SERİLEŞTİRME** makrosu, birden çok veri olay bağımsız değişkenleri olarak alabilir. Bu seri hale getirmek sağlıyor **sıcaklık** ve **nem** birlikte olay ve tek çağrıda IOT Hub'ına gönderebilirsiniz:

```C
if (SERIALIZE(&destination, &destinationSize, thermostat->Temperature, thermostat->Humidity) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

Bu kod sonucu, iki veri olaylarını IOT Hub'ına gönderilir olduğunu tahmin:

[{"Sıcaklık": 75}, {"Nem": 45}]

Diğer bir deyişle, bu kod gönderme ile aynı olduğunu beklediğiniz **sıcaklık** ve **nem** ayrı olarak. Yalnızca her iki olayları için kullanışlı olan **SERİLEŞTİRME** aynı çağrısında. Ancak, bu durumda değil. Bunun yerine, yukarıdaki kod, IOT Hub'ına bu tek veri olay gönderir:

{"Sıcaklık": "Nem" 75:45}

Modelimiz tanımladığından Bu tuhaf görünebilir **sıcaklık** ve **nem** iki olarak *ayrı* olayları:

```C
DECLARE_MODEL(Thermostat,
WITH_DATA(int, Temperature),
WITH_DATA(int, Humidity),
WITH_DATA(EDM_DATE_TIME_OFFSET, Time)
);
```

Daha noktasına Biz bu olayları modelini alamadık burada **sıcaklık** ve **nem** aynı yapıda şunlardır:

```C
DECLARE_STRUCT(TemperatureAndHumidityEvent,
int, Temperature,
int, Humidity,
);

DECLARE_MODEL(Thermostat,
WITH_DATA(TemperatureAndHumidityEvent, TemperatureAndHumidity),
);
```

Bu model kullandık, daha kolay olurdu nasıl **sıcaklık** ve **nem** içinde aynı serileştirilmiş ileti gönderilir. Her iki veri olaylarına geçirdiğinizde bu şekilde çalıştığını neden ancak bunu anlaşılamayabilir **SERİLEŞTİRME** 2 modelini kullanma.

Bu varsayımların, bildiğiniz olmadığını anlamak kolaydır, davranıştır **seri hale getirici** kitaplığı yapma. Bu, anlamlı geri modelimizi Bahsedelim:

```C
DECLARE_MODEL(Thermostat,
WITH_DATA(int, Temperature),
WITH_DATA(int, Humidity),
WITH_DATA(EDM_DATE_TIME_OFFSET, Time)
);
```

Bu modelde, nesne yönelimli koşulları düşünün. Bu durumda biz fiziksel bir cihaz (thermostat) modelleme ve bu cihaz öznitelikleri gibi içerir **sıcaklık** ve **nem**.

Modelimiz aşağıdaki gibi kod ile tüm durumunu gönderebiliriz:

```C
if (SERIALIZE(&destination, &destinationSize, thermostat->Temperature, thermostat->Humidity, thermostat->Time) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

Sıcaklık değerleri varsayıldığında, nem ve saat olarak ayarlanır, bu IOT Hub'ına gönderilen gibi bir olay görüyoruz:

```C
{"Temperature":75, "Humidity":45, "Time":"2015-09-17T18:45:56Z"}
```

Bazen yalnızca göndermek isteyebilirsiniz *bazı* (Bu, özellikle çok sayıda veri olaylarını modelinizi içeriyorsa, true) buluta modelin özellikleri. Önceki örneğimizde gibi yalnızca bir alt kümesini veri olayları göndermek yararlıdır:

```C
{"Temperature":75, "Time":"2015-09-17T18:45:56Z"}
```

Tanımladığımız gibi bu tam olarak aynı seri hale getirilmiş olayı oluşturan bir **TemperatureEvent** ile bir **sıcaklık** ve **zaman** üyesi, biz ile 1 model gibi. Bu durumda biz size adlı farklı bir model (model 2) kullanarak aynı seri hale getirilmiş olay üretebilir **SERİLEŞTİRME** farklı bir yolla.

Birden çok veri olaylarına geçirirseniz olan en önemli nokta **SERİLEŞTİRME,** her olay tek bir JSON nesnesinde bir özellik olduğunu varsayar.

En iyi yaklaşım ve modelinizi hakkında ne düşündüğünüzü bağlıdır. "Olaylar" buluta gönderiyor olun ve her olay özellikleri tanımlı bir dizi varsa, ilk yaklaşım mantıklı hale getirir. Bu durumda, kullanacağınız **DECLARE\_yapı** her olay yapısını tanımlayın ve bunları modelinizi dahil **ile\_veri** makrosu. Yukarıdaki ilk örnekte yaptığımız gibi her bir olay gönderin. Bu yaklaşım yalnızca tek bir veri olayına geçecekse **seri hale GETİRİCİ**.

Nesne odaklı bir biçimde modelinizde hakkında düşünüyorsanız, ikinci yaklaşım, uygun olmayabilir. Bu durumda, öğeleri tanımlanan kullanarak **ile\_veri** nesnenizin "Özellikler". Tüm alt olayların geçirdiğiniz **SERİLEŞTİRME** , bağlı olarak, "nesnenin" durumuna buluta göndermek istediğiniz ne kadar ister.

Doğru veya yanlış bir nether yaklaşımdır. Yalnızca, nasıl haberdar olmanız **seri hale getirici** kitaplığı çalışır ve gereksinimlerinize en uygun modelleme yaklaşımı seçme.

## <a name="message-handling"></a>İleti işleme

Şu ana kadar bu makalede yalnızca IOT hub'ına gönderme olayları ele ve iletilerini alma ele edilmemiş. Bu, biz iletilerini alma hakkında bilmeniz gerekenler için neden büyük ölçüde makalesinde ele alınmış [C için Azure IOT cihaz SDK'sını](iot-hub-device-sdk-c-intro.md). Bu makaleden bir ileti geri çağırma işlevini kaydederek iletileri işleyecek Hatırla:

```C
IoTHubClient_SetMessageCallback(iotHubClientHandle, IoTHubMessage, myWeather)
```

Ardından, bir ileti alındığında çağrılan geri çağırma işlevi de yazın:

```C
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

Bu uygulaması **IoTHubMessage** modelinizi her bir eylemin belirli bir işlev çağırır. Örneğin bu eylem, modelinize tanımlar:

```C
WITH_ACTION(SetAirResistance, int, Position)
```

Bu imzaya sahip bir işlev tanımlamanız gerekir:

```C
EXECUTE_COMMAND_RESULT SetAirResistance(ContosoAnemometer* device, int Position)
{
    (void)device;
    (void)printf("Setting Air Resistance Position to %d.\r\n", Position);
    return EXECUTE_COMMAND_SUCCESS;
}
```

**SetAirResistance** cihazınıza ileti gönderildiğinde daha sonra çağrılır.

Biz ne açıklanan henüz henüz iletisinin seri duruma getirilmiş sürüm benzer olduğunu. Diğer bir deyişle, göndermek istiyorsanız bir **SetAirResistance** cihazınıza iletisi, ne, aşağıdaki gibi görünür?

Bir cihaza ileti gönderme, Azure IOT hizmeti SDK'sı aracılığıyla Bunu yapmak. Yine de belirli bir eylemi çağırmak göndermek için hangi dize bilmeniz gerekir. Bir ileti göndermek için genel biçimi aşağıdaki gibidir:

```C
{"Name" : "", "Parameters" : "" }
```

İki özelliğe sahip serileştirilmiş bir JSON nesnesi gönderiyor olun: **Adı** (mesaj) eylem adı ve **parametreleri** Bu eylem parametrelerini içerir.

Örneğin, çağrılacak **SetAirResistance** bu iletiyi bir cihaza gönderebilirsiniz:

```C
{"Name" : "SetAirResistance", "Parameters" : { "Position" : 5 }}
```

Eylem adı, modelinizde tanımlı bir eylem olarak tam olarak eşleşmelidir. Parametre adları de eşleşmesi gerekir. Ayrıca büyük/küçük harfe duyarlılık unutmayın. **Adı** ve **parametreleri** her zaman büyük harfle yazılır. Eylem adı ve modelinizdeki parametreleri büyük küçük harf duyarlı emin olun. Bu örnekte, eylem adı "SetAirResistance" ve değil "setairresistance" dir.

İki diğer eylemleri **TurnFanOn** ve **TurnFanOff** bu iletiler göndererek bir cihazın çağrılabilir:

```C
{"Name" : "TurnFanOn", "Parameters" : {}}
{"Name" : "TurnFanOff", "Parameters" : {}}
```

Bu bölümde, olayları gönderme ve alma iletileri ile bilmeniz gereken her şeyi açıklanan **seri hale getirici** kitaplığı. Devam etmeden önce modelinizi ne kadar büyük olduğunu denetleyen, yapılandırabileceğiniz bazı parametreler şimdi kapsar.

## <a name="macro-configuration"></a>Makro yapılandırma

Kullanıyorsanız **seri hale getirici** kitaplığı SDK'sı dikkat edilmesi gereken önemli bir parçası, azure-c-paylaşılan yardımcı Kitaplığı'nda bulunur.

Ardından--özyinelemeli seçeneğini kullanarak GitHub Azure-iot-sdk-c depodan kopyaladıysanız bu paylaşılan yardımcı program kitaplığı burada bulabilirsiniz:

```C
.\\c-utility
```

Kitaplık kopyalanan değil, bulabilirsiniz [burada](https://github.com/Azure/azure-c-shared-utility).

Paylaşılan yardımcı program kitaplığı içinde şu klasörde bulacaksınız:

```C
azure-c-shared-utility\\macro\_utils\_h\_generator.
```

Bu klasörde adlı bir Visual Studio Çözüm **makrosu\_utils\_h\_generator.sln**:

  ![Visual Studio çözüm maco_utils_h_generator ekran görüntüsü](media/iot-hub-device-sdk-c-serializer/01-macro_utils_h_generator.png)

Bu çözümde bir program oluşturur **makrosu\_utils.h** dosya. Varsayılan makrosu yoktur\_SDK'da utils.h dosya. Bu çözüm, bazı parametreleri değiştirin ve ardından bu parametrelere göre üst bilgi dosyası yeniden sağlar.

İle ilgili olarak iki anahtar parametreleri **nArithmetic** ve **nMacroParameters** makroda bulunamadı şu iki satırı içinde tanımlandığı\_utils.tt:

```C
<#int nArithmetic=1024;#>
<#int nMacroParameters=124;/*127 parameters in one macro deﬁnition in C99 in chapter 5.2.4.1 Translation limits*/#>
```

Bu değerler, SDK'da bulunan varsayılan parametrelerdir. Her parametre şu anlama sahiptir:

* nMacroParameters – denetimleri içinde bir DECLARE olabilir kaç parametreleri\_modeli makro tanımı.
* nArithmetic – üyeleri bir modelde izin verilen toplam sayısını denetler.

Bu parametreleri önemli bir nedenle modelinizi ne kadar büyük olabilir, Denetim olmasıdır. Örneğin, bu model tanımı göz önünde bulundurun:

```C
DECLARE_MODEL(MyModel,
WITH_DATA(int, MyData)
);
```

Daha önce de belirtildiği **DECLARE\_modeli** yalnızca C makrodur. Model adını ve **ile\_veri** deyimi (henüz bir makro) olan parametreleri **DECLARE\_MODEL**. **nMacroParameters** kaç parametreleri eklenebilir tanımlar **DECLARE\_MODEL**. Etkili bir şekilde, sahip olduğunuz kaç veri bir olay ve eylemle bildirimleri tanımlar. Bu nedenle, 124 varsayılan sınırı ile bu yaklaşık 60 Eylemler ve veri olaylarını modeliyle tanımlayabilirsiniz anlamına gelir. Bu sınırı aşan denerseniz, şuna benzer bir derleyici hataları alırsınız:

  ![Ekran görüntüsü makro parametrelerini derleyici hataları](media/iot-hub-device-sdk-c-serializer/02-nMacroParametersCompilerErrors.png)

**NArithmetic** parametredir uygulamanızı daha fazla makro dili iç işleyişini hakkında.  Toplam sayısı, modelinizdeki sahip üyeler denetimleri de dahil olmak üzere **DECLARE_STRUCT** makroları. Bunun gibi derleyici hataları görmeye başlamak sonra artırmayı denemelisiniz **nArithmetic**:

   ![Ekran görüntüsü aritmetik derleyici hataları](media/iot-hub-device-sdk-c-serializer/03-nArithmeticCompilerErrors.png)

Bu parametreleri değiştirmek istiyorsanız, makro değerleri değiştirin\_utils.tt dosya, makro derlemeniz\_utils\_h\_generator.sln çözüm ve derlenmiş programını çalıştırın. Bunu yaptığınızda bunu yeni bir makro\_utils.h dosya oluşturulur ve yerleştirilir.\\ Ortak\\dahil edilen dizin.

Makro yeni sürümü kullanmak için\_utils.h, kaldırma **seri hale getirici** çözümünüzden ve onun yerine NuGet paketini içerecek **seri hale getirici** Visual Studio projesi. Bu seri hale getirici kitaplığı kaynak kodunu karşı derlemek kodunuzu sağlar. Bu güncelleştirilmiş makro içerir\_utils.h. Bunu yapmak istiyorsanız **simplesample\_amqp**, seri hale getirici kitaplığı NuGet paketini çözümden kaldırarak başlayın:

   ![Seri hale getirici kitaplığı NuGet paketini kaldırma işleminin ekran görüntüsü](media/iot-hub-device-sdk-c-serializer/04-serializer-github-package.png)

Daha sonra bu projeyi Visual Studio çözümünüze ekleyin:

> . \\c\\seri hale getirici\\derleme\\windows\\serializer.vcxproj
> 
> 

İşiniz bittiğinde, çözümünüz şöyle görünmelidir:

   ![Visual Studio çözümü simplesample_amqp ekran görüntüsü](media/iot-hub-device-sdk-c-serializer/05-serializer-project.png)

Artık derleme yaptığınızda, çözümünüzün güncelleştirilmiş makrosu\_utils.h ikili dosyanız içinde bulunur.

Bu değerleri yeterince yüksek artırma derleyici sınırları aşabilir unutmayın. Bu noktaya **nMacroParameters** önceliğiniz olması kullanılacak ana parametredir. C99 belirtimi 127 parametreleri en az bir Makro tanımında verilir belirtir. Microsoft derleyicisi belirtimi tam olarak takip eden (ve 127 sınırı vardır), imkanımız olmayacaktır **nMacroParameters** varsayılan değer. Diğer derleyiciler, bunu yapmak izin verebilir (örneğin, daha yüksek bir sınır GNU derleyici destekler).

Şu ana kadar neredeyse ile kod yazma hakkında bilmeniz gereken her şey ele aldığımız **seri hale getirici** kitaplığı. Son bölümünde önce bazı konular, hakkında merak ediyor olabilirsiniz, önceki makalelerden şimdi yeniden ziyaret edin.

## <a name="the-lower-level-apis"></a>Alt düzey API'ler
Bu makalede, odaklanmış örnek uygulama **simplesample\_amqp**. Bu örnek daha üst düzey kullanır (olmayan**LL**) ileti olayları göndermek ve almak için API'ler. Bu API kullanıyorsanız, hem olayları ileti gönderme ve alma, üstlenir arka plan iş parçacığı çalıştırır. Ancak, bu arka plan iş parçacığı ortadan kaldırmak ve açık denetim olayları gönderirken veya buluttan iletileri almak için alt düzey (LL) API'ları kullanabilirsiniz.

Bölümünde anlatıldığı gibi bir [önceki makalede](iot-hub-device-sdk-c-iothubclient.md), üst düzey API'lerinin oluşan bir işlevler kümesi vardır:

* Sı: Iothubclient\_CreateFromConnectionString
* Sı: Iothubclient\_SendEventAsync
* Sı: Iothubclient\_SetMessageCallback
* Sı: Iothubclient\_yok

Bu API'ler, gösterilen **simplesample\_amqp**.

Ayrıca mevcuttur benzer bir alt düzey API kümesi.

* Sı: Iothubclient\_LL\_CreateFromConnectionString
* Sı: Iothubclient\_LL\_SendEventAsync
* Sı: Iothubclient\_LL\_SetMessageCallback
* Sı: Iothubclient\_LL\_yok

Alt düzey API'ler önceki makalelerinde açıklandığı gibi tamamen aynı şekilde çalıştığını unutmayın. Olayları gönderme ve alma iletileri işlemek için arka plan iş parçacığı isterseniz ilk API kümesini kullanabilirsiniz. Açık denetim zaman göndermek ve IOT Hub'ından veri almak istiyorsanız ikinci bir API kümesini kullanın. Ya da API'ler çalışma kümesi ile eşit derecede iyi **seri hale getirici** kitaplığı.

Alt düzey API'ler ile nasıl kullanıldığı bir örnek için **seri hale getirici** kitaplığı bkz **simplesample\_http** uygulama.

## <a name="additional-topics"></a>Ek konu başlıkları
Bahseden değer birkaç diğer konular, yeniden işleme, diğer cihaz kimlik bilgilerini ve yapılandırma seçenekleri kullanarak özellik şunlardır. Tüm konuları ele bunlar bir [önceki makalede](iot-hub-device-sdk-c-iothubclient.md). Bu özelliklerin tümü, ile aynı şekilde çalıştığını ana nokta olan **seri hale getirici** kitaplığı ile göründükleri **sı: Iothubclient** kitaplığı. Örneğin, özellikleri modelinizden bir olaya bağlamak istiyorsanız, kullanmanız **IoTHubMessage\_özellikleri** ve **harita**\_**AddorUpdate**, daha önce açıklanan aynı şekilde:

```C
MAP_HANDLE propMap = IoTHubMessage_Properties(message.messageHandle);
sprintf_s(propText, sizeof(propText), "%d", i);
Map_AddOrUpdate(propMap, "SequenceNumber", propText);
```

Gelen olay oluşturulup oluşturulmadığını **seri hale getirici** kitaplığı veya el ile kullanılarak oluşturulan **sı: Iothubclient** kitaplığı önemli değildir.

Diğer cihaz kimlik bilgilerini kullanarak **sı: Iothubclient\_LL\_Oluştur** ekleyebiliyorsa çalışıyor olarak **sı: Iothubclient\_CreateFromConnectionString** için ayırma bir **IOTHUB\_istemci\_İŞLEMEK**.

Son olarak, kullanıyorsanız **seri hale getirici** kitaplığı ile yapılandırma seçeneklerini ayarlayabilirsiniz **sı: Iothubclient\_LL\_SetOption** kullanırkenyaptığınızgibi**Sı: Iothubclient** kitaplığı.

Benzersiz bir özellik **seri hale getirici** kitaplığı olan API'leri başlatma. Kitaplığı ile çalışmaya başlamadan önce çağırmalısınız **seri hale getirici\_init**:

```C
serializer_init(NULL);
```

Yalnızca çağırmadan önce bu yapılır **sı: Iothubclient\_CreateFromConnectionString**.

Benzer şekilde, bitirdiğinizde olmaktır yaptığınız son çağrının kitaplığı ile çalışma, **seri hale getirici\_deinit**:

```C
serializer_deinit();
```

Aksi takdirde, yukarıda listelenen diğer özelliklerin tümü, aynı iş **seri hale getirici** kitaplığı göründükleri **sı: Iothubclient** kitaplığı. Bu konular hakkında daha fazla bilgi için bkz. [önceki makalede](iot-hub-device-sdk-c-iothubclient.md) bu seri.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede benzersiz yönleri ayrıntılı olarak **seri hale getirici** bulunan kitaplık **C için Azure IOT cihaz SDK'sını**. Olayları göndermek ve IOT Hub'ından iletiler alan modelleri kullanma iyi anlamış olmanız gerekir ile bilgiler sağlanmıştır.

Ayrıca üç bölümü serisini ile uygulama geliştirme konusunda eğitiminin **C için Azure IOT cihaz SDK'sını**. Bu, yalnızca başlamanıza yardımcı olmak ancak API'leri nasıl işe iyi anlaşılmasını sağlamak için yeterli bilgi olmalıdır. Ek bilgi için burada ele alınmamaktadır SDK'da bazı örnekler vardır. Aksi takdirde, [Azure IOT SDK'sı belgeleri](https://github.com/Azure/azure-iot-sdk-c) ek bilgi için iyi bir kaynaktır.

İçin IOT Hub ile geliştirme hakkında daha fazla bilgi edinmek için [Azure IOT SDK'ları](iot-hub-devguide-sdks.md).

IOT hub'ı yeteneklerini daha iyi keşfedilebilmesi için bkz: [Azure IOT Edge dağıtımı AI uç cihazlara](../iot-edge/tutorial-simulate-device-linux.md).
