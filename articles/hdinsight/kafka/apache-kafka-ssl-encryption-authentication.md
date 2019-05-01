---
title: SSL şifreleme ve Azure HDInsight, Apache Kafka için kimlik doğrulaması kurulumu
description: Kafka istemcileri ile Kafka aracılarına de sızmasını Kafka aracıları arasındaki iletişim için SSL şifrelemesi ayarlayın. Kurulum SSL istemci kimlik doğrulaması.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 01/15/2019
ms.author: hrasheed
ms.openlocfilehash: 9d8d5e57d0dd7d7022e65a061360c8450848fb4b
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64682912"
---
# <a name="setup-secure-sockets-layer-ssl-encryption-and-authentication-for-apache-kafka-in-azure-hdinsight"></a>Güvenli Yuva Katmanı (SSL) şifreleme ve Azure HDInsight, Apache Kafka için kimlik doğrulaması kurulumu

Bu makalede, Apache Kafka istemcileri ve Apache Kafka aracıları arasında SSL şifrelemesini ayarlama açıklanır. Ayrıca, istemcilerin (bazen iki yönlü SSL adlandırılır) Kurulum kimlik doğrulaması için gerekli olan adımları sağlar.

## <a name="server-setup"></a>Sunucu Kurulumu

İlk adım bir anahtar deposu ve truststore her Kafka Aracısı'nın oluşturmaktır. Sertifika yetkilisi (CA) ve Aracı sertifikaları bu oluşturulduktan sonra bu depolara içeri aktarın.

> [!Note] 
> Bu kılavuzda, otomatik olarak imzalanan sertifikaları kullanır, ancak Güvenilen CA'lar tarafından verilen sertifikaları kullanmak için en güvenli çözümüdür.

Sunucu Kurulumu tamamlamak için şunları yapın:

1. SSL adlı bir klasör oluşturun ve sunucu parolasını bir ortam değişkeni dışarı aktarın. Bu kılavuzda kalanı için değiştirin `<server_password>` sunucunun gerçek yönetici parolası ile.
1. Ardından, bir java keystore'un (kafka.server.keystore.jks) ve bir CA sertifikası oluşturun.
1. Ardından, alma CA tarafından imzalanan önceki adımda oluşturulan sertifika imzalama isteği oluşturun.
1. Şimdi, imzalama isteğini CA'ya göndermek ve otomatik olarak imzalanan bir sertifika bu alın. Biz otomatik olarak imzalanan bir sertifika kullandığından, size sertifika bizim CA kullanarak oturum `openssl` komutu.
1. Güven deposu oluşturun ve CA sertifikasını içeri aktarın.
1. Ortak CA sertifikası, anahtar deposu aktarın.
1. İmzalı sertifika anahtar deposu aktarın.

Komutlar bu adımları tamamlamak için aşağıdaki kod parçacığında gösterilmektedir.

```bash
export SRVPASS=<server_password>
mkdir ssl
cd ssl

# Create a java keystore (kafka.server.keystore.jks) and a CA certificate.

keytool -genkey -keystore kafka.server.keystore.jks -validity 365 -storepass $SRVPASS -keypass $SRVPASS -dname "CN=wn0-umakaf.xvbseke35rbuddm4fyvhm2vz2h.cx.internal.cloudapp.net" -storetype pkcs12

# Create a signing request to get the certificate created in the previous step signed by the CA.

keytool -keystore kafka.server.keystore.jks -certreq -file cert-file -storepass $SRVPASS -keypass $SRVPASS

# Send the signing request to the CA and get this certificate signed.

openssl x509 -req -CA ca-cert -CAkey ca-key -in cert-file -out cert-signed -days 365 -CAcreateserial -passin pass:$SRVPASS

# Create a trust store and import the certificate of the CA.

keytool -keystore kafka.server.truststore.jks -alias CARoot -import -file ca-cert -storepass $SRVPASS -keypass $SRVPASS -noprompt

# Import the public CA certificate into the keystore.

keytool -keystore kafka.server.keystore.jks -alias CARoot -import -file ca-cert -storepass $SRVPASS -keypass $SRVPASS -noprompt

# Import the signed certificate into the keystore.

keytool -keystore kafka.server.keystore.jks -alias CARoot -import -file ca-cert -storepass $SRVPASS -keypass $SRVPASS -noprompt

# The output should say "Certificate reply was added to keystore"
```

İmzalı sertifika anahtar deposu alma truststore ve anahtar deposu için bir Kafka aracısını yapılandırmak için gereken son adımdır.

## <a name="update-kafka-configuration-to-use-ssl-and-restart-brokers"></a>SSL kullanmak ve aracıları yeniden başlatmak için Kafka yapılandırmasını güncelleştirme

Artık her Kafka ile bir anahtar deposu ve truststore aracı ve doğru sertifikaların içeri Kurulumu var.  Ardından, Ambari kullanarak ilgili Kafka yapılandırma özelliklerini değiştirmek ve Kafka aracılarına yeniden başlatın. 

Yapılandırma değişikliği tamamlamak için aşağıdaki adımları uygulayın:

1. Azure portalında oturum açın ve Azure HDInsight Apache Kafka kümenizi seçin.
1. Ambari UI tıklayarak Git **Ambari giriş** altında **küme panoları**.
1. Altında **Kafka Aracısı** ayarlamak **dinleyicileri** özelliği `PLAINTEXT://localhost:9092,SSL://localhost:9093`
1. Altında **kafka aracısını Advanced** ayarlamak **security.inter.broker.protocol** özelliği `SSL`

    ![Ambari Kafka ssl yapılandırma özelliklerini düzenleme](./media/apache-kafka-ssl-encryption-authentication/editing-configuration-ambari.png)

1. Altında **özel kafka aracısını** ayarlamak **ssl.client.auth** özelliğini `required`. Bu adım yalnızca, kimlik doğrulaması yanı sıra şifreleme ayarını, gerekli.

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

İstemci Kurulumu tamamlamak için aşağıdaki adımları uygulayın:

1. İstemci makinesinde (hn1) oturum açın.
1. İstemci parolası dışarı aktarın. Değiştirin `<client_password>` ile Kafka istemci makinede gerçek yönetici parolası.
1. Bir java keystore'un oluşturun ve aracısı için imzalı bir sertifika alın. Ardından CA'ın çalıştırıldığı VM'ye sertifika kopyalayın.
1. İstemci sertifikasını imzalamak için CA makineye (wn0) geçin.
1. İstemci bilgisayarın (hn1) gidin ve gidin `~/ssl` klasör. İmzalı sertifika istemci bilgisayara kopyalayın.

```bash
export CLIPASS=<client_password>
cd ssl

# Create a java keystore and get a signed certificate for the broker. Then copy the certificate to the VM where the CA is running.

keytool -genkey -keystore kafka.client.keystore.jks -validity 365 -storepass $CLIPASS -keypass $CLIPASS -dname "CN=mylaptop1" -alias my-local-pc1 -storetype pkcs12

keytool -keystore kafka.client.keystore.jks -certreq -file client-cert-sign-request -alias my-local-pc1 -storepass $CLIPASS -keypass $CLIPASS

# Copy the cert to the vm where the CA is
scp client-cert-sign-request3 sshuser@wn0-umakaf:~/tmp1/client-cert-sign-request

# Switch to the CA machine (wn0) to sign the client certificate.
cd ssl
openssl x509 -req -CA ca-cert -CAkey ca-key -in /tmp1/client-cert-sign-request -out /tmp1/client-cert-signed -days 365 -CAcreateserial -passin pass:<server_password>

# Return to the client machine (hn1), navigate to ~/ssl folder and copy signed cert to client machine
scp -i ~/kafka-security.pem sshuser@wn0-umakaf:/tmp1/client-cert-signed

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

Kimlik doğrulama gerekmiyorsa, yalnızca SSL şifrelemesi ayarlama adımları şunlardır:

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
