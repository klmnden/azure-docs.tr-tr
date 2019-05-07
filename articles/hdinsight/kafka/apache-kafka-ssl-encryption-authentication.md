---
title: SSL şifreleme ve Azure HDInsight, Apache Kafka için kimlik doğrulaması ayarlama
description: Kafka istemcileri ile Kafka aracılarına de sızmasını Kafka aracıları arasındaki iletişim için SSL şifrelemesi ayarlayın. SSL kimlik doğrulamasını istemcilerin ayarlama.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/01/2019
ms.author: hrasheed
ms.openlocfilehash: e526908f5ba9feea53b1c1abebbbfc1bd9a51c54
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65147960"
---
# <a name="set-up-secure-sockets-layer-ssl-encryption-and-authentication-for-apache-kafka-in-azure-hdinsight"></a>Güvenli Yuva Katmanı (SSL) şifreleme ve Azure HDInsight, Apache Kafka için kimlik doğrulaması ayarlama

Bu makalede, Apache Kafka istemcileri ve Apache Kafka aracıları arasında SSL şifrelemesini ayarlama işlemini göstermektedir. Bu ayrıca, (bazen iki yönlü SSL adlandırılır) istemci kimlik doğrulaması ayarlama işlemini göstermektedir.

> [!Important]
> Kafka uygulamalar için kullanabileceğiniz iki istemcisi vardır: bir Java istemci ve bir konsol istemcisi. Java istemci `ProducerConsumer.java` SSL üreten hem tüketim için kullanabilirsiniz. Konsol üretici istemcisi `console-producer.sh` SSL ile çalışmaz.

## <a name="apache-kafka-broker-setup"></a>Apache Kafka Aracısı Kurulumu

Kafka SSL Aracısı kurulumu, aşağıdaki şekilde dört HDInsight kümesi Vm'lerini kullanır:

* baş düğüm 0 - sertifika yetkilisi (CA)
* çalışan düğümü 0, 1 ve 2 - aracıları

> [!Note] 
> Bu kılavuzda, otomatik olarak imzalanan sertifikaları kullanır, ancak Güvenilen CA'lar tarafından verilen sertifikaları kullanmak için en güvenli çözümüdür.

Aracısı Kurulum işlemi özetini aşağıdaki gibidir:

1. Aşağıdaki adımlar, her üç çalışan düğümü üzerinde yinelenir:

    1. Bir sertifika oluşturun.
    1. Bir sertifika imzalama isteği oluşturun.
    1. Sertifika imzalama isteği için sertifika yetkilisi (CA) gönderin.
    1. CA için oturum açın ve istek oturum açın.
    1. SCP imzalı sertifika çalışan düğümüne yedekleyin.
    1. SCP çalışan düğümüne CA ortak sertifika.

1. Tüm sertifikalar sertifika deposuna yerleştirmek sertifikalarını olduğunda.
1. Ambarı'na gidin ve yapılandırmaları değiştirin.

Aracı Kurulumu tamamlamak için aşağıdaki ayrıntılı yönergeleri kullanın:

> [!Important]
> Aşağıdaki kod parçacıkları wnX üç alt düğümlerinden biri için bir kısaltmadır ve ile değiştirilen `wn0`, `wn1` veya `wn2` uygun şekilde. `WorkerNode0_Name` ve `HeadNode0_Name` ilgili makinelerin adlarını ile gibi yerine kullanılacağını `wn0-abcxyz` veya `hn0-abcxyz`.

1. İlk kurulum ' % s'rolü, sertifika yetkilisi (CA) doldurur, HDInsight baş düğümde 0, gerçekleştirin.

    ```bash
    # Create a new directory 'ssl' and change into it
    mkdir ssl
    cd ssl

    # Export
    export SRVPASS=MyServerPassword123
    ```

1. Aynı ilk kurulum her aracıları (0, 1 ve 2 çalışan düğümü) gerçekleştirin.

    ```bash
    # Create a new directory 'ssl' and change into it
    mkdir ssl
    cd ssl

    # Export
    export MyServerPassword123=MyServerPassword123
    ```

1. Her çalışan düğümü üzerinde aşağıdaki kod parçacığını kullanarak aşağıdaki adımları uygulayın.
    1. Bir anahtar deposu oluşturun ve yeni bir özel sertifika ile doldurun.
    1. Sertifika imzalama isteği oluşturun.
    1. SCP (headnode0) CA'ya sertifika imzalama isteği

    ```bash
    keytool -genkey -keystore kafka.server.keystore.jks -validity 365 -storepass "MyServerPassword123" -keypass "MyServerPassword123" -dname "CN=FQDN_WORKER_NODE" -storetype pkcs12
    keytool -keystore kafka.server.keystore.jks -certreq -file cert-file -storepass "MyServerPassword123" -keypass "MyServerPassword123"
    scp cert-file sshuser@HeadNode0_Name:~/ssl/wnX-cert-sign-request
    ```

1. CA makineye değiştirmek ve tüm alınan sertifika imzalama istekleri oturum açın:

    ```bash
    openssl x509 -req -CA ca-cert -CAkey ca-key -in wn0-cert-sign-request -out wn0-cert-signed -days 365 -CAcreateserial -passin pass:"MyServerPassword123"
    openssl x509 -req -CA ca-cert -CAkey ca-key -in wn1-cert-sign-request -out wn1-cert-signed -days 365 -CAcreateserial -passin pass:"MyServerPassword123"
    openssl x509 -req -CA ca-cert -CAkey ca-key -in wn2-cert-sign-request -out wn2-cert-signed -days 365 -CAcreateserial -passin pass:"MyServerPassword123"
    ```

1. Otomatik imzalı sertifikaları CA'dan (headnode0) çalışan düğümlerine gönderin.

    ```bash
    scp wn0-cert-signed sshuser@WorkerNode0_Name:~/ssl/cert-signed
    scp wn1-cert-signed sshuser@WorkerNode1_Name:~/ssl/cert-signed
    scp wn2-cert-signed sshuser@WorkerNode2_Name:~/ssl/cert-signed
    ```

1. CA ortak sertifikasını her çalışan düğümüne gönderin.

    ```bash
    scp ca-cert sshuser@WorkerNode0_Name:~/ssl/ca-cert
    scp ca-cert sshuser@WorkerNode1_Name:~/ssl/ca-cert
    scp ca-cert sshuser@WorkerNode2_Name:~/ssl/ca-cert
    ```

1. Her çalışan düğümü üzerinde anahtar deposu ve truststore CA ortak sertifika ekleyin. Ardından alt düğümün kendi imzalı sertifika için anahtar deposu ekleyin

    ```bash
    keytool -keystore kafka.server.truststore.jks -alias CARoot -import -file ca-cert -storepass "MyServerPassword123" -keypass "MyServerPassword123" -noprompt
    keytool -keystore kafka.server.keystore.jks -alias CARoot -import -file ca-cert -storepass "MyServerPassword123" -keypass "MyServerPassword123" -noprompt
    keytool -keystore kafka.server.keystore.jks -import -file cert-signed -storepass "MyServerPassword123" -keypass "MyServerPassword123" -noprompt

    ```

## <a name="update-kafka-configuration-to-use-ssl-and-restart-brokers"></a>SSL kullanmak ve aracıları yeniden başlatmak için Kafka yapılandırmasını güncelleştirme

Artık bir anahtar deposu ve truststore her Kafka Aracısı ayarlama ve doğru sertifikaların içeri. Ardından, Ambari kullanarak ilgili Kafka yapılandırma özelliklerini değiştirmek ve Kafka aracılarına yeniden başlatın.

Yapılandırma değişikliği tamamlamak için aşağıdaki adımları uygulayın:

1. Azure portalında oturum açın ve Azure HDInsight Apache Kafka kümenizi seçin.
1. Ambari UI tıklayarak Git **Ambari giriş** altında **küme panoları**.
1. Altında **Kafka Aracısı** ayarlamak **dinleyicileri** özelliği `PLAINTEXT://localhost:9092,SSL://localhost:9093`
1. Altında **kafka aracısını Advanced** ayarlamak **security.inter.broker.protocol** özelliği `SSL`

    ![Ambari Kafka ssl yapılandırma özelliklerini düzenleme](./media/apache-kafka-ssl-encryption-authentication/editing-configuration-ambari.png)

1. Altında **özel kafka aracısını** ayarlamak **ssl.client.auth** özelliğini `required`. Bu adım yalnızca, kimlik doğrulama ve şifreleme ayarlama, gerekli.

    ![Ambari kafka ssl yapılandırma özelliklerini düzenleme](./media/apache-kafka-ssl-encryption-authentication/editing-configuration-ambari2.png)

1. Yapılandırma özellikleri eklemek için Kafka `server.properties` IP adresleri yerine tam etki alanı adı (FQDN) bildirmek için dosya.

    ```bash
    IP_ADDRESS=$(hostname -i)
    echo advertised.listeners=$IP_ADDRESS
    sed -i.bak -e '/advertised/{/advertised@/!d;}' /usr/hdp/current/kafka-broker/conf/server.properties
    echo "advertised.listeners=PLAINTEXT://$IP_ADDRESS:9092,SSL://$IP_ADDRESS:9093" >> /usr/hdp/current/kafka-broker/conf/server.properties
    echo "ssl.keystore.location=/home/sshuser/ssl/kafka.server.keystore.jks" >> /usr/hdp/current/kafka-broker/conf/server.properties
    echo "ssl.keystore.password=<server_password>" >> /usr/hdp/current/kafka-broker/conf/server.properties
    echo "ssl.key.password=<server_password>" >> /usr/hdp/current/kafka-broker/conf/server.properties
    echo "ssl.truststore.location=/home/sshuser/ssl/kafka.server.truststore.jks" >> /usr/hdp/current/kafka-broker/conf/server.properties
    echo "ssl.truststore.password=<server_password>" >> /usr/hdp/current/kafka-broker/conf/server.properties
    ```

1. Önceki doğru yapılan değişiklikler, isteğe bağlı olarak aşağıdaki satırları Kafka'da mevcut olduğunu denetleyebilirsiniz doğrulamak için `server.properties` dosya.

    ```bash
    advertised.listeners=PLAINTEXT://10.0.0.11:9092,SSL://10.0.0.11:9093
    ssl.keystore.location=/home/sshuser/ssl/kafka.server.keystore.jks
    ssl.keystore.password=<server_password>
    ssl.key.password=<server_password>
    ssl.truststore.location=/home/sshuser/ssl/kafka.server.truststore.jks
    ssl.truststore.password=<server_password>
    ```

1. Tüm Kafka aracıları yeniden başlatın.

## <a name="client-setup-with-authentication"></a>İstemci Kurulumu (ile kimlik doğrulaması)

> [!Note]
> Yalnızca her iki SSL şifrelemeyi ayarlama ayarlıyorsanız aşağıdaki adımlar gereklidir **ve** kimlik doğrulaması. Şifrelemeyi ayarlama yalnızca ayarlıyorsanız, Lütfen devam [kimlik doğrulaması olmadan İstemci Kurulumu](apache-kafka-ssl-encryption-authentication.md#client-setup-without-authentication)

İstemci Kurulumu tamamlamak için aşağıdaki adımları tamamlayın:

1. İstemci bilgisayarın (hn1) oturum açın.
1. İstemci parolası dışarı aktarın. Değiştirin `<client_password>` ile Kafka istemci makinede gerçek yönetici parolası.
1. Bir java keystore'un oluşturun ve aracısı için imzalı bir sertifika alın. Ardından CA'ın çalıştırıldığı VM'ye sertifika kopyalayın.
1. İstemci sertifikasını imzalamak için CA makineye (hn0) geçin.
1. İstemci bilgisayarın (hn1) gidin ve gidin `~/ssl` klasör. İmzalı sertifika istemci bilgisayara kopyalayın.

```bash
export CLIPASS=<client_password>
cd ssl

# Create a java keystore and get a signed certificate for the broker. Then copy the certificate to the VM where the CA is running.

keytool -genkey -keystore kafka.client.keystore.jks -validity 365 -storepass $CLIPASS -keypass $CLIPASS -dname "CN=mylaptop1" -alias my-local-pc1 -storetype pkcs12

keytool -keystore kafka.client.keystore.jks -certreq -file client-cert-sign-request -alias my-local-pc1 -storepass $CLIPASS -keypass $CLIPASS

# Copy the cert to the CA
scp client-cert-sign-request3 sshuser@HeadNode0_Name:~/tmp1/client-cert-sign-request

# Switch to the CA machine (hn0) to sign the client certificate.
cd ssl
openssl x509 -req -CA ca-cert -CAkey ca-key -in /tmp1/client-cert-sign-request -out /tmp1/client-cert-signed -days 365 -CAcreateserial -passin pass:<server_password>

# Return to the client machine (hn1), navigate to ~/ssl folder and copy signed cert from the CA (hn0) to client machine
scp -i ~/kafka-security.pem sshuser@HeadNode0_Name:/tmp1/client-cert-signed

# Import CA cert to trust store
keytool -keystore kafka.client.truststore.jks -alias CARoot -import -file ca-cert -storepass $CLIPASS -keypass $CLIPASS -noprompt

# Import CA cert to key store
keytool -keystore kafka.client.keystore.jks -alias CARoot -import -file ca-cert -storepass $CLIPASS -keypass $CLIPASS -noprompt

# Import signed client (cert client-cert-signed1) to keystore
keytool -keystore kafka.client.keystore.jks -import -file client-cert-signed -alias my-local-pc1 -storepass $CLIPASS -keypass $CLIPASS -noprompt
```

Son olarak, dosyayı görüntülemek `client-ssl-auth.properties` komutu `cat client-ssl-auth.properties`. Aşağıdaki satırları sahip olmanız gerekir:

```bash
security.protocol=SSL
ssl.truststore.location=/home/sshuser/ssl/kafka.client.truststore.jks
ssl.truststore.password=<client_password>
ssl.keystore.location=/home/sshuser/ssl/kafka.client.keystore.jks
ssl.keystore.password=<client_password>
ssl.key.password=<client_password>
```

## <a name="client-setup-without-authentication"></a>İstemci Kurulumu (olmadan kimlik doğrulaması)

Kimlik doğrulama gerekmiyorsa, yalnızca SSL şifrelemesi ayarlamak için adımları şunlardır:

1. İstemci bilgisayarın (hn1) oturum açın ve gidin `~/ssl` klasörü
1. İstemci parolası dışarı aktarın. Değiştirin `<client_password>` ile Kafka istemci makinede gerçek yönetici parolası.
1. İmzalı sertifika CA makineden (wn0) istemci bilgisayara kopyalayın.
1. CA sertifikası için truststore içeri aktarma
1. Keystore için bir CA sertifikası alma

Aşağıdaki kod parçacığında bu adımları gösterilmektedir.

```bash
export CLIPASS=<client_password>
cd ssl

# Copy signed cert to client machine
scp -i ~/kafka-security.pem sshuser@wn0-umakaf:/home/sshuser/ssl/ca-cert .

# Import CA cert to truststore
keytool -keystore kafka.client.truststore.jks -alias CARoot -import -file ca-cert -storepass $CLIPASS -keypass $CLIPASS -noprompt

# Import CA cert to keystore
keytool -keystore kafka.client.keystore.jks -alias CARoot -import -file cert-signed -storepass $CLIPASS -keypass $CLIPASS -noprompt
```

Son olarak, dosyayı görüntülemek `client-ssl-auth.properties` komutu `cat client-ssl-auth.properties`. Aşağıdaki satırları sahip olmanız gerekir:

```bash
security.protocol=SSL
ssl.truststore.location=/home/sshuser/ssl/kafka.client.truststore.jks
ssl.truststore.password=<client_password>
```

## <a name="next-steps"></a>Sonraki adımlar

* [HDInsight üzerinde Apache Kafka nedir?](apache-kafka-introduction.md)
