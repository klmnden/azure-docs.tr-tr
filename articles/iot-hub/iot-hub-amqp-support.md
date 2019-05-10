---
title: Azure IOT hub'ı AMQP destek anlama | Microsoft Docs
description: Geliştirici Kılavuzu - IOT hub'a bağlanan cihazlar için destek ve AMQP protokolünü kullanarak cihaz hem de hizmet'e yönelik uç noktalar. Yerleşik AMQP destekleyen Azure IOT cihaz SDK'ları hakkında bilgi içerir.
author: rezasherafat
manager: ''
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 04/30/2019
ms.author: rezas
ms.openlocfilehash: 703e2c842fb42bad8aa112d84c516a29c2327378
ms.sourcegitcommit: 399db0671f58c879c1a729230254f12bc4ebff59
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/09/2019
ms.locfileid: "65473514"
---
# <a name="communicate-with-your-iot-hub-using-the-amqp-protocol"></a>AMQP protokolünü kullanarak IOT hub ile iletişim

IOT hub'ın desteklediği [AMQP 1.0 sürümü](http://docs.oasis-open.org/amqp/core/v1.0/os/amqp-core-complete-v1.0-os.pdf) cihaz hem de hizmet'e yönelik uç noktalarla işlevleri çeşitli sunmak için. Bu belge, IOT hub'ı işlevselliği kullanmak için IOT Hub'ına bağlanmak için AMQP istemcileri kullanımını açıklar.

## <a name="service-client"></a>Hizmet istemcisi

### <a name="connection-and-authenticating-to-iot-hub-service-client"></a>Bağlantı ve IOT Hub'ına (hizmeti istemcisi) kimlik doğrulaması
AMQP kullanarak IOT hub'a bağlanmak için bir istemci kullanabilir [talep tabanlı güvenlik (CBS)](https://www.oasis-open.org/committees/download.php/60412/amqp-cbs-v1.0-wd03.doc) veya [Basit kimlik doğrulaması ve güvenlik katmanı (SASL) kimlik doğrulaması](https://en.wikipedia.org/wiki/Simple_Authentication_and_Security_Layer).

Hizmet istemcisi için aşağıdaki bilgiler gereklidir:

| Bilgi | Değer | 
|-------------|--------------|
| IoT Hub Ana Bilgisayar Adı | `<iot-hub-name>.azure-devices.net` |
| Anahtar adı | `service` |
| Erişim anahtarı | Hizmetle ilişkili birincil veya ikincil anahtarı |
| Paylaşılan Erişim İmzası | Kısa süreli SAS şu biçimde: `SharedAccessSignature sig={signature-string}&se={expiry}&skn={policyName}&sr={URL-encoded-resourceURI}` (Bu imzayı üretmek için kod bulunabilir [burada](./iot-hub-devguide-security.md#security-token-structure)).


Kodu aşağıdaki kod parçacığını kullanır [Python uAMQP kitaplıkta](https://github.com/Azure/azure-uamqp-python) gönderen bağlantı üzerinden IOT hub'a bağlanmak için.

```python
import uamqp
import urllib
import time

# Use generate_sas_token implementation available here: https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-security#security-token-structure
from helper import generate_sas_token

iot_hub_name = '<iot-hub-name>'
hostname = '{iot_hub_name}.azure-devices.net'.format(iot_hub_name=iot_hub_name)
policy_name = 'service'
access_key = '<primary-or-secondary-key>'
operation = '<operation-link-name>' # e.g., '/messages/devicebound'

username = '{policy_name}@sas.root.{iot_hub_name}'.format(iot_hub_name=iot_hub_name, policy_name=policy_name)
sas_token = generate_sas_token(hostname, access_key, policy_name)
uri = 'amqps://{}:{}@{}{}'.format(urllib.quote_plus(username), urllib.quote_plus(sas_token), hostname, operation)

# Create a send or receive client
send_client = uamqp.SendClient(uri, debug=True)
receive_client = uamqp.ReceiveClient(uri, debug=True)
```

### <a name="invoking-cloud-to-device-messages-service-client"></a>Bulut-cihaz iletilerini (hizmeti istemcisi) çağırma
Bulut-cihaz ileti exchange hizmeti ve IOT hub'ı arasındaki yanı sıra IOT Hub ile cihaz arasındaki açıklanan [burada](iot-hub-devguide-messages-c2d.md). Hizmet istemcisi daha önce gönderilen iletiler için geri bildirim alabilir ve iletileri göndermek için aşağıda açıklanan iki bağlantı kullanır.

| Oluşturan | Bağlantı türü | Bağlantı yolu | Açıklama |
|------------|-----------|-----------|-------------|
| Hizmet | Gönderen bağlantısı | `/messages/devicebound` | Cihazları hedefleyen C2D iletileri için bu bağlantı hizmet tarafından gönderilir. Bu bağlantı üzerinden gönderilen iletilere kendi `To` özelliği hedef cihazın alıcı bağlantı yolu: başka bir deyişle, `/devices/<deviceID>/messages/devicebound`. |
| Hizmet | Alıcı bağlantıya | `/messages/serviceBound/feedback` | Bu bağlantıya hizmeti tarafından alınan cihazlardan gelen tamamlama, ret ve abandonment geri bildirim iletileri. Bkz: [burada](./iot-hub-devguide-messages-c2d.md#message-feedback) geri bildirim iletileri hakkında daha fazla bilgi. |

Aşağıdaki kod parçacığında, bir C2D iletisi oluşturun ve bir cihaz kullanarak göndermek gösterilmiştir [Python uAMQP kitaplıkta](https://github.com/Azure/azure-uamqp-python).

```python
import uuid
# Create a message and set message property 'To' to the devicebound link on device
msg_id = str(uuid.uuid4())
msg_content = b"Message content goes here!"
device_id = '<device-id>'
to = '/devices/{device_id}/messages/devicebound'.format(device_id=device_id)
ack = 'full' # Alternative values are 'positive', 'negative', and 'none'
app_props = { 'iothub-ack': ack }
msg_props = uamqp.message.MessageProperties(message_id=msg_id, to=to)
msg = uamqp.Message(msg_content, properties=msg_props, application_properties=app_props)

# Send the message using the send client created and connected IoT Hub earlier
send_client.queue_message(msg)
results = send_client.send_all_messages()

# Close the client if not needed
send_client.close()
```

Geri bildirim almak için alıcının bağlantı hizmeti istemcisi oluşturur. Aşağıdaki kod parçacığını kullanarak bunu yapmayı gösteren [Python uAMQP kitaplıkta](https://github.com/Azure/azure-uamqp-python).

```python
import json

operation = '/messages/serviceBound/feedback'

# ...
# Recreate the URI using the feedback path above and authenticate
uri = 'amqps://{}:{}@{}{}'.format(urllib.quote_plus(username), urllib.quote_plus(sas_token), hostname, operation)

receive_client = uamqp.ReceiveClient(uri, debug=True)
batch = receive_client.receive_message_batch(max_batch_size=10)
for msg in batch:
  print('received a message')
  # Check content_type in message property to identify feedback messages coming from device
  if msg.properties.content_type == 'application/vnd.microsoft.iothub.feedback.json':
    msg_body_raw = msg.get_data()
    msg_body_str = ''.join(msg_body_raw)
    msg_body = json.loads(msg_body_str)
    print(json.dumps(msg_body, indent=2))
    print('******************')
    for feedback in msg_body:
      print('feedback received')
      print('\tstatusCode: ' + str(feedback['statusCode']))
      print('\toriginalMessageId: ' + str(feedback['originalMessageId']))
      print('\tdeviceId: ' + str(feedback['deviceId']))
      print
  else:
    print('unknown message:', msg.properties.content_type)
```

Yukarıda gösterildiği gibi bir C2D geri bildirim iletisi içerik türüne sahip `application/vnd.microsoft.iothub.feedback.json` ve orijinal iletinin teslim durumunu çıkarsamak için JSON gövdesinde özellikler kullanılabilir:
* Anahtar `statusCode` geri bildirim gövdesi ya da bu değerleri vardır: `['Success', 'Expired', 'DeliveryCountExceeded', 'Rejected', 'Purged']`.
* Anahtar `deviceId` geri bildirim hedef cihaz Kimliğini gövdeye sahip.
* Anahtar `originalMessageId` geri bildirim hizmeti tarafından gönderilen orijinal C2D iletinin Kimliğini gövdeye sahip. Bu geri bildirim C2D iletileri ilişkilendirmek için kullanılabilir.

### <a name="receive-telemetry-messages-service-client"></a>Telemetri iletilerini (hizmeti istemcisi)


### <a name="additional-notes"></a>Ek notlar
* AMQP bağlantıları ağ sorun veya (kodda oluşturulan) kimlik doğrulama belirteci süre sonu nedeniyle kesilmiş olabilir. Hizmeti istemcisi, şu durumlarda işlemek ve bağlantı ve gerekirse bağlantıları yeniden oluşturun. Kimlik doğrulama belirteci süre sonu durum istemci bağlantı bırakma önlemek için sona erme önce belirteç de proaktif bir şekilde yenileyebilirsiniz.
* Bazı durumlarda, istemci bağlantıyı yeniden yönlendirmeleri doğru bir şekilde işleyebilir olması gerekir. Bunun nasıl yapılacağı AMQP istemcisi belgelerinize bakın.

### <a name="receive-cloud-to-device-messages-device-and-module-client"></a>Bulut-cihaz iletilerini (cihaz ve modül istemci)
AMQP bağlantıları cihaz tarafında kullanılan aşağıdaki gibidir:

| Oluşturan | Bağlantı türü | Bağlantı yolu | Açıklama |
|------------|-----------|-----------|-------------|
| Cihazlar | Alıcı bağlantıya | `/devices/<deviceID>/messages/devicebound` | Cihazları hedefleyen C2D iletiler bu bağlantıya her hedef cihaz tarafından alınır. |
| Cihazlar | Gönderen bağlantısı | `/messages/serviceBound/feedback` | Bu bağlantı üzerinden cihazlar tarafından gönderilen C2D ileti geri bildirim. |
| Modüller | Alıcı bağlantıya | `/devices/<deviceID>/modules/<moduleID>/messages/devicebound` | Modüller için hedeflenen C2D iletiler bu bağlantıya her hedef modülü tarafından alınır. |
| Modüller | Gönderen bağlantısı | `/messages/serviceBound/feedback` | C2D ileti geri bildirim hizmetine modülleri tarafından bu bağlantı üzerinden gönderilir. |


## <a name="next-steps"></a>Sonraki adımlar

AMQP protokolünü hakkında daha fazla bilgi için bkz: [AMQP v1.0 belirtimi](http://www.amqp.org/sites/amqp.org/files/amqp.pdf).

IOT Hub mesajlaşması hakkında daha fazla bilgi için bkz:

* [Bulut-cihaz iletilerini](./iot-hub-devguide-messages-c2d.md)
* [Ek protokol desteği](iot-hub-protocol-gateway.md)
* [MQTT protokolünü desteği](./iot-hub-mqtt-support.md)
