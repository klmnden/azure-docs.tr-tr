---
title: "Azure Event Hubs'a C kullanarak olayları göndermek | Microsoft Docs"
description: "Azure Event Hubs'a C kullanarak olayları Gönder"
services: event-hubs
documentationcenter: 
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: c
ms.devlang: csharp
ms.topic: article
ms.date: 08/15/2017
ms.author: sethm
ms.openlocfilehash: 25311958314cca049d109ecbe3f46aaaa36b694d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="send-events-to-azure-event-hubs-using-c"></a>Azure Event Hubs'a C kullanarak olayları Gönder

## <a name="introduction"></a>Giriş
Olay hub'ları saniye başına milyonlarca olayı alma, bir uygulamaya etkinleştirme işlemek ve veri bağlı cihazlarınız ve uygulamalarınız tarafından üretilen oldukça büyük miktardaki analiz bir yüksek düzeyde ölçeklenebilir bir alım sistemine olur. Bir olay hub'ına toplandığında, dönüştürme ve herhangi bir gerçek zamanlı analiz sağlayıcısı veya depolama kümesi kullanarak verileri depolar.

Daha fazla bilgi için lütfen bkz [Event Hubs'a genel bakış] [Event Hubs'a genel bakış].

Bu öğreticide, c dilinde bir konsol uygulaması kullanarak bir olay hub'ına olayları göndermek nasıl öğreneceksiniz. Olayları almak için sol taraftaki İçindekiler uygun alıcı dilde tıklayın.

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Bir C geliştirme ortamı. Bu öğreticide, Azure Linux VM'de Ubuntu 14.04 ile gcc yığın varsayacağız.
* [Microsoft Visual Studio](https://www.visualstudio.com/).
* Etkin bir Azure hesabı. Hesabınız yoksa yalnızca birkaç dakika içinde ücretsiz bir deneme sürümü hesabı oluşturabilirsiniz. Ayrıntılı bilgi için bkz. [Azure Ücretsiz Deneme Sürümü](https://azure.microsoft.com/pricing/free-trial/).

## <a name="send-messages-to-event-hubs"></a>Event Hubs’a ileti gönderme
Bu bölümde event hub'ına olayları göndermek için bir C uygulaması yazma. Kodu Proton AMQP Kitaplığı'ndan kullanan [Apache Qpid proje](http://qpid.apache.org/). Bu hizmet veri yolu sıraları ve konuları AMQP ile C'den gösterildiği gibi kullanmaya benzer [burada](https://code.msdn.microsoft.com/Using-Apache-Qpid-Proton-C-afd76504). Daha fazla bilgi için bkz: [Qpid Proton belgelerine](http://qpid.apache.org/proton/index.html).

1. Gelen [Qpid AMQP Messenger sayfa](https://qpid.apache.org/proton/messenger.html), Qpid Proton, ortamınıza bağlı olarak yüklemek için yönergeleri izleyin.
2. Protonun durgun kitaplığı derlemek için aşağıdaki paketleri yükleyin:
   
    ```shell
    sudo apt-get install build-essential cmake uuid-dev openssl libssl-dev
    ```
3. Karşıdan [Qpid Proton Kitaplığı](http://qpid.apache.org/proton/index.html)ve, örneğin ayıklayın:
   
    ```shell
    wget http://archive.apache.org/dist/qpid/proton/0.7/qpid-proton-0.7.tar.gz
    tar xvfz qpid-proton-0.7.tar.gz
    ```
4. Derleme dizini, derleme ve yükleme oluşturun:
   
    ```shell
    cd qpid-proton-0.7
    mkdir build
    cd build
    cmake -DCMAKE_INSTALL_PREFIX=/usr ..
    sudo make install
    ```
5. İş dizininizde adlı yeni bir dosya oluşturun **sender.c** aşağıdaki kod ile. Olay hub'ı adı ve ad alanı adı değerini değiştirmeyi unutmayın. Anahtar için URL kodlu sürümü yerine gerekir **SendRule** daha önce oluşturduğunuz. URL kodlamak için bunu [burada](http://www.w3schools.com/tags/ref_urlencode.asp).
   
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
6. Dosyasını derleyin varsayılarak **gcc**:
   
    ```
    gcc sender.c -o sender -lqpid-proton
    ```

    > [!NOTE]
    > Bu kodda 1 giden bir pencere iletileri mümkün olan en kısa sürede zorlamak için kullanırız. Genel olarak, uygulamanızın verimliliğini artırmak için toplu iletileri denemelisiniz. Bkz: [Qpid AMQP Messenger sayfa](https://qpid.apache.org/proton/messenger.html) bu ve diğer ortamlara ve bağlamaları sağlanan platformları Qpid Proton kitaplığını kullanma hakkında bilgi için (şu anda Perl, PHP, Python ve Ruby).


## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki bağlantıları inceleyerek Event Hubs hakkında daha fazla bilgi edinebilirsiniz:

* [Event Hubs’a genel bakış](event-hubs-what-is-event-hubs.md
)
* [Olay Hub’ı oluşturma](event-hubs-create.md)
* [Event Hubs ile ilgili SSS](event-hubs-faq.md)

<!-- Images. -->
[21]: ./media/event-hubs-c-ephcs-getstarted/run-csharp-ephcs1.png
[24]: ./media/event-hubs-c-ephcs-getstarted/receive-eph-c.png
