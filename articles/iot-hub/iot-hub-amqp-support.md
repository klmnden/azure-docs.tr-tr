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
ms.openlocfilehash: c304c9b7fe02e3396d49aee0b70576071d9fac92
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67055391"
---
# <a name="communicate-with-your-iot-hub-by-using-the-amqp-protocol"></a>IOT hub'ınıza AMQP protokolünü kullanarak iletişim kurar.

Azure IOT hub'ın desteklediği [OASIS Advanced Message Queuing Protocol (AMQP) 1.0 sürümü](http://docs.oasis-open.org/amqp/core/v1.0/os/amqp-core-complete-v1.0-os.pdf) cihaz hem de hizmet'e yönelik uç noktalarla işlevleri çeşitli sunmak için. Bu belge, IOT hub'ı işlevselliği kullanmak için bir IOT hub'a bağlanmak için AMQP istemcileri kullanımını açıklar.

## <a name="service-client"></a>Hizmet istemcisi

### <a name="connect-and-authenticate-to-an-iot-hub-service-client"></a>Bağlanmak ve bir IOT hub'ına (hizmeti istemcisi) kimlik doğrulaması
AMQP kullanarak bir IOT hub'ına bağlanmak için bir istemci kullanabilir [talep tabanlı güvenlik (CBS)](https://www.oasis-open.org/committees/download.php/60412/amqp-cbs-v1.0-wd03.doc) veya [Basit kimlik doğrulaması ve güvenlik katmanı (SASL) kimlik doğrulaması](https://en.wikipedia.org/wiki/Simple_Authentication_and_Security_Layer).

Hizmet istemcisi için aşağıdaki bilgiler gereklidir:

| Bilgi | Değer | 
|-------------|--------------|
| IOT hub ana bilgisayar adı | `<iot-hub-name>.azure-devices.net` |
| Anahtar adı | `service` |
| Erişim anahtarı | Hizmetle ilişkili birincil veya ikincil anahtarı |
| Paylaşılan erişim imzası | Aşağıdaki biçimde bir kısa süreli bir paylaşılan erişim imzası: `SharedAccessSignature sig={signature-string}&se={expiry}&skn={policyName}&sr={URL-encoded-resourceURI}`. Bu imza oluşturmak için kodu alma için bkz: [IOT hub'a erişimi denetleme](./iot-hub-devguide-security.md#security-token-structure).

Aşağıdaki kod parçacığı kullandığı [Python uAMQP kitaplıkta](https://github.com/Azure/azure-uamqp-python) gönderen bağlantı üzerinden bir IOT hub'a bağlanmak için.

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

### <a name="invoke-cloud-to-device-messages-service-client"></a>Bulut-cihaz iletilerini (hizmeti istemcisi) Çağır
Bulut-cihaz ileti exchange hizmeti ile IOT hub'ı ve IOT hub ile cihaz arasındaki hakkında bilgi edinmek için bkz. [IOT hub'ından bulut buluttan cihaza ileti gönderme](iot-hub-devguide-messages-c2d.md). İletiler gönderip geri bildirim için daha önce hizmet istemci kullanan iki bağlantı cihazlardan aşağıdaki tabloda açıklandığı gibi gönderilen iletileri:

| Tarafından oluşturulan | Bağlantı türü | Bağlantı yolu | Açıklama |
|------------|-----------|-----------|-------------|
| Hizmet | Gönderen bağlantısı | `/messages/devicebound` | Cihazlar için hedeflenen bulut-cihaz iletilerini Bu bağlantı hizmet tarafından gönderilir. Bu bağlantı üzerinden gönderilen iletilere kendi `To` özelliği hedef cihazın alıcı bağlantı yoluna ayarlanmış `/devices/<deviceID>/messages/devicebound`. |
| Hizmet | Alıcı bağlantıya | `/messages/serviceBound/feedback` | Cihazlardan alınan bu bağlantıya hizmeti tarafından gelen tamamlama, ret ve abandonment geri bildirim iletileri. Geri bildirim iletileri hakkında daha fazla bilgi için bkz: [bir IOT hub'ından bulut buluttan cihaza ileti gönderme](./iot-hub-devguide-messages-c2d.md#message-feedback). |

Aşağıdaki kod parçacığı, bir bulut-cihaz iletisi oluşturun ve kullanarak bir cihaza göndermek gösterilmiştir [Python uAMQP kitaplıkta](https://github.com/Azure/azure-uamqp-python).

```python
import uuid
# Create a message and set message property 'To' to the device-bound link on device
msg_id = str(uuid.uuid4())
msg_content = b"Message content goes here!"
device_id = '<device-id>'
to = '/devices/{device_id}/messages/devicebound'.format(device_id=device_id)
ack = 'full' # Alternative values are 'positive', 'negative', and 'none'
app_props = { 'iothub-ack': ack }
msg_props = uamqp.message.MessageProperties(message_id=msg_id, to=to)
msg = uamqp.Message(msg_content, properties=msg_props, application_properties=app_props)

# Send the message by using the send client that you created and connected to the IoT hub earlier
send_client.queue_message(msg)
results = send_client.send_all_messages()

# Close the client if it's not needed
send_client.close()
```

Geri bildirim almak için alıcının bağlantı hizmeti istemcisi oluşturur. Aşağıdaki kod parçacığını kullanarak bir bağlantı oluşturmak nasıl gösterir [Python uAMQP kitaplıkta](https://github.com/Azure/azure-uamqp-python):

```python
import json

operation = '/messages/serviceBound/feedback'

# ...
# Re-create the URI by using the preceding feedback path and authenticate it
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

Yukarıdaki kodda gösterildiği gibi bir bulut-cihaz geri bildirim iletisi bir içerik türüne sahip *application/vnd.microsoft.iothub.feedback.json*. Özgün iletinin teslim durumunu çıkarsanacak iletinin JSON gövdesine özelliklerini kullanabilirsiniz:
* Anahtar `statusCode` geri gövdesi aşağıdaki değerlerden birine sahiptir: *Başarı*, *süresi*, *DeliveryCountExceeded*, *reddedilen*, veya *temizleneceği*.
* Anahtar `deviceId` geri hedef cihazın kimliği gövdeye sahip.
* Anahtar `originalMessageId` geri bildirim hizmeti tarafından gönderilen özgün bulut-cihaz iletinin Kimliğini gövdeye sahip. Bulut-cihaz iletilerini geri ilişkilendirmek için bu teslim durumu kullanabilirsiniz.

### <a name="receive-telemetry-messages-service-client"></a>Telemetri iletilerini (hizmeti istemcisi)

Varsayılan olarak, IOT hub'ınıza bir yerleşik bir olay hub'ında cihaz alınan telemetri iletilerini depolar. Hizmet istemci saklı olaylarını almak için AMQP protokolünü kullanabilirsiniz.

Bu amaçla hizmeti istemcisi IOT hub uç noktasını bağlamak ve bir yeniden yönlendirme adresine yerleşik event hubs'ı almak öncelikle gerekir. Hizmeti istemcisi, yerleşik bir olay hub'ına bağlanmak için sağlanan adres ardından kullanır.

Her adımda şu bilgilere sunmak istemcinin gerekir:
* Geçerli hizmet kimlik bilgilerini (hizmet paylaşılan erişim İmza belirteci).
* Kuyruktan alınacak düşünüyor tüketici grubu bölümü iyi biçimlendirilmiş bir yolu. Belirli bir tüketici grubu ve bölüm için bir kimlik, yol aşağıdaki biçime sahiptir: `/messages/events/ConsumerGroups/<consumer_group>/Partitions/<partition_id>` (varsayılan bir tüketici grubu olan `$Default`).
* Bölümündeki bir başlangıç noktası belirtmek için isteğe bağlı bir filtre koşulu. Bu koşul, bir sıra numarası, offset veya sıraya alınan zaman damgası biçiminde olabilir.

Aşağıdaki kod parçacığı kullandığı [Python uAMQP kitaplıkta](https://github.com/Azure/azure-uamqp-python) önceki adımları göstermek için:

```python
import json
import uamqp
import urllib
import time

# Use the generate_sas_token implementation that's available here: https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-security#security-token-structure
from helper import generate_sas_token

iot_hub_name = '<iot-hub-name>'
hostname = '{iot_hub_name}.azure-devices.net'.format(iot_hub_name=iot_hub_name)
policy_name = 'service'
access_key = '<primary-or-secondary-key>'
operation = '/messages/events/ConsumerGroups/{consumer_group}/Partitions/{p_id}'.format(consumer_group='$Default', p_id=0)

username = '{policy_name}@sas.root.{iot_hub_name}'.format(policy_name=policy_name, iot_hub_name=iot_hub_name)
sas_token = generate_sas_token(hostname, access_key, policy_name)
uri = 'amqps://{}:{}@{}{}'.format(urllib.quote_plus(username), urllib.quote_plus(sas_token), hostname, operation)

# Optional filtering predicates can be specified by using endpoint_filter
# Valid predicates include:
# - amqp.annotation.x-opt-sequence-number
# - amqp.annotation.x-opt-offset
# - amqp.annotation.x-opt-enqueued-time
# Set endpoint_filter variable to None if no filter is needed
endpoint_filter = b'amqp.annotation.x-opt-sequence-number > 2995'

# Helper function to set the filtering predicate on the source URI
def set_endpoint_filter(uri, endpoint_filter=''):
  source_uri = uamqp.address.Source(uri)
  source_uri.set_filter(endpoint_filter)
  return source_uri

receive_client = uamqp.ReceiveClient(set_endpoint_filter(uri, endpoint_filter), debug=True)
try:
  batch = receive_client.receive_message_batch(max_batch_size=5)
except uamqp.errors.LinkRedirect as redirect:
  # Once a redirect error is received, close the original client and recreate a new one to the re-directed address
  receive_client.close()

  sas_auth = uamqp.authentication.SASTokenAuth.from_shared_access_key(redirect.address, policy_name, access_key)
  receive_client = uamqp.ReceiveClient(set_endpoint_filter(redirect.address, endpoint_filter), auth=sas_auth, debug=True)

# Start receiving messages in batches
batch = receive_client.receive_message_batch(max_batch_size=5)
for msg in batch:
  print('*** received a message ***')
  print(''.join(msg.get_data()))
  print('\t: ' + str(msg.annotations['x-opt-sequence-number']))
  print('\t: ' + str(msg.annotations['x-opt-offset']))
  print('\t: ' + str(msg.annotations['x-opt-enqueued-time']))
```

Belirli bir cihaz kimliği için IOT hub'ı bir cihaz kimliği, iletileri depolamak için hangi bölümünün belirlemek için kullanır. Yukarıdaki kod parçacığında olayları tek bir nasıl alınacağını gösterir. Bu tür bir bölüm. Ancak, tüm olay hub'ı bölümleri içinde depolanan olayları almak tipik bir uygulama genelde gerektiğini unutmayın.


## <a name="device-client"></a>Cihaz istemcisi

### <a name="connect-and-authenticate-to-an-iot-hub-device-client"></a>Bağlanmak ve bir IOT hub'ına (cihaz istemcisi) kimlik doğrulaması
AMQP kullanarak bir IOT hub'ına bağlanmak için bir cihaz kullanabilirsiniz [talep tabanlı güvenlik (CBS)](https://www.oasis-open.org/committees/download.php/60412/amqp-cbs-v1.0-wd03.doc) veya [Basit kimlik doğrulaması ve güvenlik katmanı (SASL)](https://en.wikipedia.org/wiki/Simple_Authentication_and_Security_Layer) kimlik doğrulaması.

Cihaz istemcisi için aşağıdaki bilgiler gereklidir:

| Bilgi | Değer | 
|-------------|--------------|
| IOT hub ana bilgisayar adı | `<iot-hub-name>.azure-devices.net` |
| Erişim anahtarı | Cihazla ilişkili birincil veya ikincil anahtarı |
| Paylaşılan erişim imzası | Aşağıdaki biçimde bir kısa süreli bir paylaşılan erişim imzası: `SharedAccessSignature sig={signature-string}&se={expiry}&skn={policyName}&sr={URL-encoded-resourceURI}`. Bu imza oluşturmak için kodu alma için bkz: [IOT hub'a erişimi denetleme](./iot-hub-devguide-security.md#security-token-structure).


Aşağıdaki kod parçacığı kullandığı [Python uAMQP kitaplıkta](https://github.com/Azure/azure-uamqp-python) gönderen bağlantı üzerinden bir IOT hub'a bağlanmak için.

```python
import uamqp
import urllib
import uuid

# Use generate_sas_token implementation available here: https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-security#security-token-structure
from helper import generate_sas_token

iot_hub_name = '<iot-hub-name>'
hostname = '{iot_hub_name}.azure-devices.net'.format(iot_hub_name=iot_hub_name)
device_id = '<device-id>'
access_key = '<primary-or-secondary-key>'
username = '{device_id}@sas.{iot_hub_name}'.format(device_id=device_id, iot_hub_name=iot_hub_name)
sas_token = generate_sas_token('{hostname}/devices/{device_id}'.format(hostname=hostname, device_id=device_id), access_key, None)

operation = '<operation-link-name>' # e.g., '/devices/{device_id}/messages/devicebound'
uri = 'amqps://{}:{}@{}{}'.format(urllib.quote_plus(username), urllib.quote_plus(sas_token), hostname, operation)

receive_client = uamqp.ReceiveClient(uri, debug=True)
send_client = uamqp.SendClient(uri, debug=True)
```

Aşağıdaki bağlantı yolları cihaz işlemleri desteklenir:

| Tarafından oluşturulan | Bağlantı türü | Bağlantı yolu | Açıklama |
|------------|-----------|-----------|-------------|
| Cihazlar | Alıcı bağlantıya | `/devices/<deviceID>/messages/devicebound` | Cihazlar için hedeflenen bulut-cihaz iletilerini bu bağlantıya her hedef cihaz tarafından alınır. |
| Cihazlar | Gönderen bağlantısı | `/devices/<deviceID>messages/events` | Bir CİHAZDAN gönderilen CİHAZDAN buluta iletileri, bu bağlantı üzerinden gönderilir. |
| Cihazlar | Gönderen bağlantısı | `/messages/serviceBound/feedback` | Bulut-cihaz ileti bu bağlantıdan cihazlar tarafından gönderilen geri bildirim. |


### <a name="receive-cloud-to-device-commands-device-client"></a>Bulut-cihaz komutları (cihaz istemcisi) alma
Cihazlara gönderilen bulut-cihaz komutlarını geldiğinde üzerinde bir `/devices/<deviceID>/messages/devicebound` bağlantı. Cihazlar toplu olarak bu ileti alma ve ileti veri yükü, ileti özelliklerini, ek açıklamalar veya uygulama özellikleri iletisinde gerektiğinde kullanın.

Aşağıdaki kod parçacığı kullandığı [Python uAMQP kitaplıkta](https://github.com/Azure/azure-uamqp-python)) bir cihaz tarafından bulut-cihaz iletilerini almak için.

```python
# ... 
# Create a receive client for the cloud-to-device receive link on the device
operation = '/devices/{device_id}/messages/devicebound'.format(device_id=device_id)
uri = 'amqps://{}:{}@{}{}'.format(urllib.quote_plus(username), urllib.quote_plus(sas_token), hostname, operation)

receive_client = uamqp.ReceiveClient(uri, debug=True)
while True:
  batch = receive_client.receive_message_batch(max_batch_size=5)
  for msg in batch:
    print('*** received a message ***')
    print(''.join(msg.get_data()))

    # Property 'to' is set to: '/devices/device1/messages/devicebound',
    print('\tto:                     ' + str(msg.properties.to))

    # Property 'message_id' is set to value provided by the service
    print('\tmessage_id:             ' + str(msg.properties.message_id))

    # Other properties are present if they were provided by the service
    print('\tcreation_time:          ' + str(msg.properties.creation_time))
    print('\tcorrelation_id:         ' + str(msg.properties.correlation_id))
    print('\tcontent_type:           ' + str(msg.properties.content_type))
    print('\treply_to_group_id:      ' + str(msg.properties.reply_to_group_id))
    print('\tsubject:                ' + str(msg.properties.subject))
    print('\tuser_id:                ' + str(msg.properties.user_id))
    print('\tgroup_sequence:         ' + str(msg.properties.group_sequence))
    print('\tcontent_encoding:       ' + str(msg.properties.content_encoding))
    print('\treply_to:               ' + str(msg.properties.reply_to))
    print('\tabsolute_expiry_time:   ' + str(msg.properties.absolute_expiry_time))
    print('\tgroup_id:               ' + str(msg.properties.group_id))

    # Message sequence number in the built-in event hub
    print('\tx-opt-sequence-number:  ' + str(msg.annotations['x-opt-sequence-number']))
```

### <a name="send-telemetry-messages-device-client"></a>Telemetri göndermek (cihaz istemcisi)
AMQP kullanarak bir CİHAZDAN telemetri iletilerini de gönderebilirsiniz. Cihaz, uygulama özellikleri sözlüğü isteğe bağlı olarak sağlayabilir veya çeşitli özellikleri, ileti kimliği gibi iletisi

Aşağıdaki kod parçacığı kullandığı [Python uAMQP kitaplıkta](https://github.com/Azure/azure-uamqp-python) bir CİHAZDAN cihaz-bulut iletileri göndermek için.


```python
# ... 
# Create a send client for the device-to-cloud send link on the device
operation = '/devices/{device_id}/messages/events'.format(device_id=device_id)
uri = 'amqps://{}:{}@{}{}'.format(urllib.quote_plus(username), urllib.quote_plus(sas_token), hostname, operation)

send_client = uamqp.SendClient(uri, debug=True)

# Set any of the applicable message properties
msg_props = uamqp.message.MessageProperties()
msg_props.message_id = str(uuid.uuid4())
msg_props.creation_time = None
msg_props.correlation_id = None
msg_props.content_type = None
msg_props.reply_to_group_id = None
msg_props.subject = None
msg_props.user_id = None
msg_props.group_sequence = None
msg_props.to = None
msg_props.content_encoding = None
msg_props.reply_to = None
msg_props.absolute_expiry_time = None
msg_props.group_id = None

# Application properties in the message (if any)
application_properties = { "app_property_key": "app_property_value" }

# Create message
msg_data = b"Your message payload goes here"
message = uamqp.Message(msg_data, properties=msg_props, application_properties=application_properties)

send_client.queue_message(message)
results = send_client.send_all_messages()

for result in results:
    if result == uamqp.constants.MessageState.SendFailed:
        print result
```

## <a name="additional-notes"></a>Ek notlar
* AMQP bağlantıları, bir ağ sorun veya kimlik doğrulamasının süre sonu nedeniyle kesilmiş (kodda oluşturulan) belirteci. Hizmeti istemcisi şu durumlarda işlemek ve gerekirse bağlantı ve bağlantıları, yeniden kurmanız gerekir. Bir kimlik doğrulama belirtecinin süresi dolarsa, istemci bağlantı bırakma belirteç, süre sonundan önce proaktif bir şekilde yenileyerek önleyebilirsiniz.
* Bazen istemci bağlantıyı yeniden yönlendirmeleri düzgün işleyebilmesi olmalıdır. Bu tür bir işlem anlamak için AMQP istemcisi belgelerinize bakın.

## <a name="next-steps"></a>Sonraki adımlar

AMQP protokolünü hakkında daha fazla bilgi için bkz: [AMQP v1.0 belirtimi](http://www.amqp.org/sites/amqp.org/files/amqp.pdf).

IOT Hub mesajlaşması hakkında daha fazla bilgi için bkz:

* [Bulut-cihaz iletilerini](./iot-hub-devguide-messages-c2d.md)
* [Ek protokoller için desteği](iot-hub-protocol-gateway.md)
* [Message Queuing Telemetri Transport (MQTT) protokolü desteği](./iot-hub-mqtt-support.md)
