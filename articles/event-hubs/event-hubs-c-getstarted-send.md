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
ms.date: 12/4/2017
ms.author: sethm
ms.openlocfilehash: 2b714c5de96a8fb7ed66a30c62daaa38b84fdc5b
ms.sourcegitcommit: 7136d06474dd20bb8ef6a821c8d7e31edf3a2820
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2017
---
# <a name="send-events-to-azure-event-hubs-using-c"></a>Azure Event Hubs'a C kullanarak olayları Gönder

## <a name="introduction"></a>Giriş
Olay hub'ları saniye başına milyonlarca olayı alma, bir uygulamaya etkinleştirme işlemek ve veri bağlı cihazlarınız ve uygulamalarınız tarafından üretilen oldukça büyük miktardaki analiz bir yüksek düzeyde ölçeklenebilir bir alım sistemine olur. Bir olay hub'ına toplandığında, dönüştürme ve herhangi bir gerçek zamanlı analiz sağlayıcısı veya depolama kümesi kullanarak verileri depolar.

Daha fazla bilgi için lütfen bkz [Event Hubs'a genel bakış] [Event Hubs'a genel bakış].

Bu öğretici, c dilinde bir konsol uygulaması kullanarak bir olay hub'ına olayları göndermeyi açıklar Olayları alma hakkında bilgi edinmek için sol taraftaki İçindekiler uygun alıcı dilde'ı tıklatın.

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

* Bir C geliştirme ortamı. Bu öğretici Azure Linux VM'de Ubuntu 14.04 ile gcc yığın varsayar.
* [Microsoft Visual Studio](https://www.visualstudio.com/).
* Etkin bir Azure hesabı. Hesabınız yoksa yalnızca birkaç dakika içinde ücretsiz bir deneme sürümü hesabı oluşturabilirsiniz. Ayrıntılı bilgi için bkz. [Azure Ücretsiz Deneme Sürümü](https://azure.microsoft.com/pricing/free-trial/).

## <a name="send-messages-to-event-hubs"></a>Event Hubs’a ileti gönderme
Bu bölümde, olay hub'ınıza olayları göndermek için C uygulama yazmak gösterilmiştir. Kodu Proton AMQP Kitaplığı'ndan kullanan [Apache Qpid proje](http://qpid.apache.org/). Bu hizmet veri yolu sıraları ve konuları AMQP ile C'den gösterildiği gibi kullanmaya benzer [bu örnekteki](https://code.msdn.microsoft.com/Using-Apache-Qpid-Proton-C-afd76504). Daha fazla bilgi için bkz: [Qpid Proton belgelerine](http://qpid.apache.org/proton/index.html).

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
5. İş dizininizde adlı yeni bir dosya oluşturun **sender.c** aşağıdaki kod ile. SAS anahtar/adı, olay hub'ı adı ve ad alanı için değerleri değiştirmek unutmayın. Anahtar için URL kodlu sürümü yerine gerekir **SendRule** daha önce oluşturduğunuz. URL kodlamak için bunu [burada](http://www.w3schools.com/tags/ref_urlencode.asp).
   
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
    > Bu kodu 1 giden bir pencere iletileri mümkün olan en kısa sürede zorlamak için kullanır. Uygulamanızı verimliliğini artırmak için toplu iletileri deneyin önerilir. Bkz: [Qpid AMQP Messenger sayfa](https://qpid.apache.org/proton/messenger.html) bu ve diğer ortamlara ve bağlamaları sağlanan platformları Qpid Proton kitaplığını kullanma hakkında bilgi için (şu anda Perl, PHP, Python ve Ruby).


## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki bağlantıları inceleyerek Event Hubs hakkında daha fazla bilgi edinebilirsiniz:

* [Event Hubs’a genel bakış](event-hubs-what-is-event-hubs.md)
* [Event Hubs ile ilgili SSS](event-hubs-faq.md)

<!-- Images. -->
[21]: ./media/event-hubs-c-ephcs-getstarted/run-csharp-ephcs1.png
[24]: ./media/event-hubs-c-ephcs-getstarted/receive-eph-c.png
