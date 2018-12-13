---
title: C - Azure Event Hubs'ı kullanarak olayları gönderme | Microsoft Docs
description: Bu makalede, Azure Event Hubs'a olayları gönderen bir C uygulaması oluşturmak için bir kılavuz sağlar.
services: event-hubs
documentationcenter: ''
author: ShubhaVijayasarathy
manager: timlt
editor: ''
ms.assetid: ''
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: c
ms.devlang: csharp
ms.topic: article
ms.custom: seodec18
ms.date: 12/06/2018
ms.author: shvija
ms.openlocfilehash: 8c134ae9944517d6ae66fcd22e06bbfc599912b4
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2018
ms.locfileid: "53076401"
---
# <a name="send-events-to-azure-event-hubs-using-c"></a>C kullanarak Azure Event Hubs için olayları gönderme

## <a name="introduction"></a>Giriş
Azure Event Hubs saniyede milyonlarca olay alıp işleme kapasitesine sahip olan bir Büyük Veri akış platformu ve olay alma hizmetidir. Event Hubs dağıtılan yazılımlar ve cihazlar tarafından oluşturulan olayları, verileri ve telemetrileri işleyebilir ve depolayabilir. Bir olay hub’ına gönderilen veriler, herhangi bir gerçek zamanlı analiz sağlayıcısı ve işlem grubu oluşturma/depolama bağdaştırıcıları kullanılarak dönüştürülüp depolanabilir. Olay Hub’larının ayrıntılı genel bakışı için bkz. [Olay Hub’larına genel bakış](event-hubs-about.md) ve [Olay Hub’ları özellikleri](event-hubs-features.md).

Bu öğretici, c dilinde bir konsol uygulaması kullanarak olay hub'ına olayları göndermek açıklar 

## <a name="prerequisites"></a>Önkoşullar
Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Bir C geliştirme ortamı. Bu öğreticide, Azure Linux VM'de Ubuntu 14.04 ile gcc yığında varsayılır.
* [Microsoft Visual Studio](https://www.visualstudio.com/).

## <a name="create-an-event-hubs-namespace-and-an-event-hub"></a>Event Hubs ad alanı ve bir olay hub’ı oluşturma
İlk adımda [Azure portalını](https://portal.azure.com) kullanarak Event Hubs türünde bir ad alanı oluşturun, ardından uygulamanızın olay hub’ı ile iletişim kurması için gereken yönetim kimlik bilgilerini edinin. Bir ad alanı ve olay hub'ı oluşturmak için verilen yordamı izleyin [bu makalede](event-hubs-create.md).

Olay hub'ı için erişim anahtarı değeri makalesindeki yönergeleri izleyerek almak: [bağlantı dizesi alma](event-hubs-get-connection-string.md#get-connection-string-from-the-portal). Bu öğreticinin ilerleyen bölümlerinde yazdığınız kod erişim anahtarını kullanın. Varsayılan anahtar adı: **RootManageSharedAccessKey**.

Şimdi, bu öğreticide aşağıdaki adımlarla devam edin.

## <a name="write-code-to-send-messages-to-event-hubs"></a>Event Hubs'a ileti göndermek için kod yazma
Bu bölümde, olay hub'ınıza olayları göndermek için bir C uygulaması yazma işlemi gösterilmektedir. Kod Proton AMQP Kitaplığı'ndan kullanır [Apache Qpid proje](http://qpid.apache.org/). Bu Service Bus kuyrukları ve konular ile AMQP C'den gösterildiği gibi kullanarak benzer [bu](https://code.msdn.microsoft.com/Using-Apache-Qpid-Proton-C-afd76504). Daha fazla bilgi için [Qpid Proton belgeleri](http://qpid.apache.org/proton/index.html).

1. Gelen [Qpid AMQP Messenger sayfa](https://qpid.apache.org/proton/messenger.html), ortamınıza bağlı olarak Qpid Proton yüklemek için yönergeleri izleyin.
2. Proton kitaplığı derlemek için aşağıdaki paketleri yükleyin:
   
    ```shell
    sudo apt-get install build-essential cmake uuid-dev openssl libssl-dev
    ```
3. İndirme [Qpid Proton Kitaplığı](http://qpid.apache.org/proton/index.html)ve, örneğin ayıklayın:
   
    ```shell
    wget http://archive.apache.org/dist/qpid/proton/0.7/qpid-proton-0.7.tar.gz
    tar xvfz qpid-proton-0.7.tar.gz
    ```
4. Bir derleme dizini, derleme ve yükleme oluşturun:
   
    ```shell
    cd qpid-proton-0.7
    mkdir build
    cd build
    cmake -DCMAKE_INSTALL_PREFIX=/usr ..
    sudo make install
    ```
5. Çalışma dizininizde adlı yeni bir dosya oluşturmak **sender.c** aşağıdaki kod ile. SAS anahtarı/adı, olay hub'ı adı ve ad alanı değerleri değiştirmeyi unutmayın. Anahtar için URL kodlu sürümünü de yerine gerekir **SendRule** daha önce oluşturduğunuz. URL kodlaması için bunu [burada](http://www.w3schools.com/tags/ref_urlencode.asp).
   
    ```c
    #include "proton/message.h"
    #include "proton/messenger.h"
   
    #include <getopt.h>
    #include <proton/util.h>
    #include <sys/time.h>
    #include <stddef.h>
    #include <stdio.h>
    #include <string.h>
    #include <unistd.h>
    #include <stdlib.h>
   
    #define check(messenger)                                                     \
      {                                                                          \
        if(pn_messenger_errno(messenger))                                        \
        {                                                                        \
          printf("check\n");                                                     \
          die(__FILE__, __LINE__, pn_error_text(pn_messenger_error(messenger))); \
        }                                                                        \
      }
   
    pn_timestamp_t time_now(void)
    {
      struct timeval now;
      if (gettimeofday(&now, NULL)) pn_fatal("gettimeofday failed\n");
      return ((pn_timestamp_t)now.tv_sec) * 1000 + (now.tv_usec / 1000);
    }  
   
    void die(const char *file, int line, const char *message)
    {
      printf("Dead\n");
      fprintf(stderr, "%s:%i: %s\n", file, line, message);
      exit(1);
    }
   
    int sendMessage(pn_messenger_t * messenger) {
        char * address = (char *) "amqps://{SAS Key Name}:{SAS key}@{namespace name}.servicebus.windows.net/{event hub name}";
        char * msgtext = (char *) "Hello from C!";
   
        pn_message_t * message;
        pn_data_t * body;
        message = pn_message();
   
        pn_message_set_address(message, address);
        pn_message_set_content_type(message, (char*) "application/octect-stream");
        pn_message_set_inferred(message, true);
   
        body = pn_message_body(message);
        pn_data_put_binary(body, pn_bytes(strlen(msgtext), msgtext));
   
        pn_messenger_put(messenger, message);
        check(messenger);
        pn_messenger_send(messenger, 1);
        check(messenger);
   
        pn_message_free(message);
    }
   
    int main(int argc, char** argv) {
        printf("Press Ctrl-C to stop the sender process\n");
   
        pn_messenger_t *messenger = pn_messenger(NULL);
        pn_messenger_set_outgoing_window(messenger, 1);
        pn_messenger_start(messenger);
   
        while(true) {
            sendMessage(messenger);
            printf("Sent message\n");
            sleep(1);
        }
   
        // release messenger resources
        pn_messenger_stop(messenger);
        pn_messenger_free(messenger);
   
        return 0;
    }
    ```
6. Dosyanın derleme varsayılarak **gcc**:
   
    ```
    gcc sender.c -o sender -lqpid-proton
    ```

    > [!NOTE]
    > Bu kodu 1 giden bir pencere iletilerini olabildiğince çabuk zorlamak için kullanır. Bu, uygulamanızın aktarım hızı artırmak için toplu iletileri deneyin önerilir. Bkz: [Qpid AMQP Messenger sayfa](https://qpid.apache.org/proton/messenger.html) bu ve diğer ortamlara ve bağlamaları sağlanan platformlarından Qpid Proton kitaplığını kullanma hakkında bilgi için (şu anda Perl, PHP, Python ve Ruby).

Olay hub'ına ileti göndermek için uygulamayı çalıştırın. 

Tebrikler! Bir olay hub'ına ileti gönderdiniz.

## <a name="next-steps"></a>Sonraki adımlar
Olay hub'ından olay alma hakkında bilgi edinmek için altında uygun alma diline tıklayın **olayları bir event hub'ından alma** İçindekiler düğümü.


<!-- Images. -->
[21]: ./media/event-hubs-c-ephcs-getstarted/run-csharp-ephcs1.png
[24]: ./media/event-hubs-c-ephcs-getstarted/receive-eph-c.png
