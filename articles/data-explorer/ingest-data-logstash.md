---
title: Azure Veri Gezgini Logstash verileri alma
description: Bu makalede, Azure veri Gezgini'nde Logstash içine (yükle) verilerin alımı öğrenin
author: tamirkamara
ms.author: takamara
ms.reviewer: orspodek
ms.service: data-explorer
ms.topic: conceptual
ms.date: 06/03/2019
ms.openlocfilehash: 86f6732cbf2409d3c79a3d7709100e8af24988a0
ms.sourcegitcommit: 600d5b140dae979f029c43c033757652cddc2029
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66494534"
---
# <a name="ingest-data-from-logstash-to-azure-data-explorer"></a>Azure Veri Gezgini Logstash verileri alma

[Logstash](https://www.elastic.co/products/logstash) açık kaynaklı, sunucu tarafı veri işleme işlem hattı, aynı anda birçok kaynaktan verileri alır, verileri dönüştüren ve sonra "Hazırlama" Sık kullandığınız için verileri gönderir. Bu makalede, hızlı ve yüksek oranda ölçeklenebilir bir veri keşfetme hizmeti günlük ve telemetri verileri için olan bu verileri Azure veri Gezgini'ne göndereceğiz. İlk olarak bir tablo ve veri eşleme bir test kümesini oluşturmak ve sonra tabloya veri göndermek ve sonuçları doğrulamak için Logstash doğrudan.

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Yoksa, oluşturun bir [ücretsiz Azure hesabı](https://azure.microsoft.com/free/) başlamadan önce.
* Bir Azure Veri Gezgini [küme ve veritabanını test etme](create-cluster-database-portal.md)
* 6 + sürümü Logstash [yükleme yönergeleri](https://www.elastic.co/guide/en/logstash/current/installing-logstash.html)

## <a name="create-a-table"></a>Bir tablo oluşturma

Bir küme ve bir veritabanı oluşturduktan sonra bir tablo oluşturmak için zaman var.

1. Bir tablo oluşturmak için veritabanı sorgu penceresinde aşağıdaki komutu çalıştırın:

    ```Kusto
    .create table logs (timestamp: datetime, message: string)
    ```

1. Yeni Tablo onaylamak için aşağıdaki komutu çalıştırın `logs` oluşturuldu ve boş olduğunu:
    ```Kusto
    logs
    | count
    ```

## <a name="create-a-mapping"></a>Bir eşleme oluşturma

Eşleme, hedef tablonun şemasına gelen verileri dönüştürmek için Azure Veri Gezgini tarafından kullanılır. Aşağıdaki komut adlı yeni bir eşleme oluşturur `basicmsg` , ayıklar özellikleri tarafından belirtildiği gelen json'dan `path` ve bunlara çıkaran `column`.

Sorgu penceresinde aşağıdaki komutu çalıştırın:

```Kusto
.create table logs ingestion json mapping 'basicmsg' '[{"column":"timestamp","path":"$.@timestamp"},{"column":"message","path":"$.message"}]'
```

## <a name="install-the-logstash-output-plugin"></a>Logstash çıkış eklentisini yükleme

Logstash çıkış eklentisi, Azure Veri Gezgini ile iletişim kurar ve veri hizmetine gönderir.
Eklentisini yüklemek için Logstash kök dizinini aşağıdaki komutu çalıştırın:

```sh
bin/logstash-plugin install logstash-output-kusto
```

## <a name="configure-logstash-to-generate-a-sample-dataset"></a>Bir örnek veri oluşturmak için Logstash'i yapılandırma

Logstash bir uçtan uca işlem hattı test etmek için kullanılan örnek olaylar oluşturabilir.
Logstash zaten kullanıyorsanız ve kendi olay stream'e erişiminiz varsa, sonraki bölüme atlayın. 

> [!NOTE]
> Kendi verilerinizi kullanıyorsanız, önceki adımlarda tanımlanan tablo ve eşleme nesnelerini değiştirin.

1. (Vi kullanarak) gerekli işlem hattı ayarlarını içerecek yeni bir metin dosyasını düzenleyin:

    ```sh
    vi test.conf
    ```

1. 1000 test olayları oluşturmak için Logstash'i söyleyecektir aşağıdaki ayarları yapıştırın:

    ```ruby
    input {
        stdin { }
        generator {
            message => "Test Message 123"
            count => 1000
        }
    }
    ```

Bu yapılandırma ayrıca içerir `stdin` daha fazla ileti kendiniz yazmak sağlayacak giriş Eklentisi (kullandığınızdan emin olun *Enter* ardışık düzende göndermeniz).

## <a name="configure-logstash-to-send-data-to-azure-data-explorer"></a>Azure veri Gezgini'ne veri göndermek için Logstash'i yapılandırma

Aşağıdaki ayarlar, önceki adımda kullandığınız aynı config dosyasına yapıştırın. Yer tutucuları kurulumunuzu için uygun değerlerle değiştirin. Daha fazla bilgi için [AAD uygulaması oluşturuluyor](/azure/kusto/management/access-control/how-to-provision-aad-app). 

```ruby
output {
    kusto {
            path => "/tmp/kusto/%{+YYYY-MM-dd-HH-mm-ss}.txt"
            ingest_url => "https://ingest-<cluster name>.kusto.windows.net/"
            app_id => "<application id>"
            app_key => "<application key/secret>"
            app_tenant => "<tenant id>"
            database => "<database name>"
            table => "<target table>" # logs as defined above
            mapping => "<mapping name>" # basicmsg as defined above
    }
}
```

| Parametre Adı | Açıklama |
| --- | --- |
| **Yolu** | Logstash eklentisi, Azure veri Gezgini'ne göndermeden önce geçici dosyaları olayları yazar. Bu parametre, dosyaları burada yazılmış bir yol ve dosya döndürme karşıya yükleme Azure Veri Gezgini hizmetine tetiklemek bir zaman ifadesi içerir.|
| **ingest_url** | Kusto uç noktası alma ile ilgili iletişim için.|
| **app_id**, **app_key**, ve **app_tenant**| Azure veri Gezgini'ne bağlanmak için gereken kimlik bilgileri. Alma ayrıcalıklarına sahip bir uygulama kullanma emin olun. |
| **Veritabanı**| Olayları yerleştirmek için veritabanı adı. |
| **Tablo** | Olayları yerleştirmek için hedef tablo adı. |
| **Eşleme** | Eşleme, gelen olay json dizesi (hangi sütuna hangi özelliğinin gider tanımlar) doğru satır biçime eşleştirmek için kullanılır. |

## <a name="run-logstash"></a>Logstash çalıştırın

Şimdi Logstash çalıştırmak ve test ayarları hazırız.

1. Logstash kök dizininde aşağıdaki komutu çalıştırın:

    ```sh
    bin/logstash -f test.conf
    ```

    Ekrana yazdırılan bilgiler ve ardından bizim örnek yapılandırma tarafından oluşturulan 1000 ileti görmeniz gerekir. Bu noktada, ayrıca daha fazla ileti el ile girebilirsiniz.

1. Birkaç dakika sonra tanımlanan tablo iletileri görmek için aşağıdaki veri Gezgini sorguyu çalıştırın:

    ```Kusto
    logs
    | order by timestamp desc
    ```

1. Logstash çıkmak için CTRL + C seçin

## <a name="clean-up-resources"></a>Kaynakları temizleme

Veritabanınızda temizlemek için aşağıdaki komutu çalıştırarak `logs` tablosu:

```Kusto
.drop table logs
```

## <a name="next-steps"></a>Sonraki adımlar

* [Sorgu yazma](write-queries.md)