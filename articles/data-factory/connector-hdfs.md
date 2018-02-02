---
title: Azure Data Factory kullanarak HDFS veri kopyalama | Microsoft Docs
description: "Desteklenen havuz veri depoları için bir bulut veya şirket içi HDFS kaynaktan kopyalama etkinliği Azure Data Factory ardışık düzeninde kullanarak verileri kopyalamak öğrenin."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/29/2018
ms.author: jingwang
ms.openlocfilehash: fb4802a6a3bed163f0d2bba04cf9d80a917ba7ba
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2018
---
# <a name="copy-data-from-hdfs-using-azure-data-factory"></a>Azure Data Factory kullanarak HDFS verilerini
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Sürüm 1 - Genel Kullanım](v1/data-factory-hdfs-connector.md)
> * [Sürüm 2 - Önizleme](connector-hdfs.md)

Bu makalede kopya etkinliği Azure Data Factory'de HDFS verileri kopyalamak için nasıl kullanılacağı açıklanmaktadır. Derlemeler [etkinlik genel bakış kopyalama](copy-activity-overview.md) makale kopyalama etkinliği genel bir bakış sunar.

> [!NOTE]
> Bu makale şu anda önizleme sürümünde olan Data Factory sürüm 2 için geçerlidir. Genel olarak kullanılabilir (GA) Data Factory Hizmeti'ne 1 sürümünü kullanıyorsanız bkz [V1 HDFS Bağlayıcısı](v1/data-factory-hdfs-connector.md).


## <a name="supported-capabilities"></a>Desteklenen özellikler

Verileri HDFS tüm desteklenen havuz veri deposuna kopyalayabilirsiniz. Kaynakları/havuzlarını kopyalama etkinliği tarafından desteklenen veri depoları listesi için bkz: [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats) tablo.

Özellikle, bu HDFS bağlayıcı destekler:

- Dosya kopyalarken kullanarak **Windows** (Kerberos) veya **anonim** kimlik doğrulaması.
- Dosya kopyalarken kullanarak **webhdfs** protokolü veya **yerleşik Distcp'yi** destekler.
- Dosyaları olarak kopyalama- ya da ayrıştırma/oluşturma dosyalarıyla [desteklenen dosya biçimleri ve sıkıştırma codec](supported-file-formats-and-compression-codecs.md).

## <a name="prerequisites"></a>Önkoşullar

Genel olarak erişilebilir değil bir HDFS verileri kopyalamak için bir Self-hosted tümleştirmesi çalışma zamanı ayarlamanız gerekir. Bkz: [Self-hosted tümleştirmesi çalışma zamanı](concepts-integration-runtime.md) daha ayrıntılı bilgi için makalenin.

## <a name="getting-started"></a>Başlarken

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Aşağıdaki bölümler için HDFS Data Factory varlıklarını belirli tanımlamak için kullanılan özellikleri hakkında ayrıntılı bilgi sağlar.

## <a name="linked-service-properties"></a>Bağlantılı hizmet özellikleri

Aşağıdaki özellikler HDFS bağlantılı hizmeti için desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Type özelliği ayarlanmalıdır: **Hdfs**. | Evet |
| url |HDFS URL'si |Evet |
| authenticationType | İzin verilen değerler: **anonim**, veya **Windows**. <br><br> Kullanılacak **Kerberos kimlik doğrulaması** HDFS bağlayıcı için başvurmak [Bu bölümde](#use-kerberos-authentication-for-hdfs-connector) şirket içi ortamınıza uygun şekilde ayarlamak için. |Evet |
| Kullanıcı adı |Kullanıcı adı için Windows kimlik doğrulaması. Kerberos kimlik doğrulaması için belirtmek `<username>@<domain>.com`. |Evet (Windows kimlik doğrulaması için) |
| password |Windows kimlik doğrulaması için parola. Bu alan SecureString işaretleyin. |Evet (Windows kimlik doğrulaması için) |
| connectVia | [Tümleştirmesi çalışma zamanı](concepts-integration-runtime.md) veri deposuna bağlanmak için kullanılacak. (Veri deposu genel olarak erişilebilir ise) Self-hosted tümleştirmesi çalışma zamanı veya Azure tümleştirmesi çalışma zamanı kullanabilirsiniz. Belirtilmezse, varsayılan Azure tümleştirmesi çalışma zamanı kullanır. |Hayır |

**Örnek: Anonim kimlik doğrulamasını kullanma**

```json
{
    "name": "HDFSLinkedService",
    "properties": {
        "type": "Hdfs",
        "typeProperties": {
            "url" : "http://<machine>:50070/webhdfs/v1/",
            "authenticationType": "Anonymous",
            "userName": "hadoop"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Örnek: Windows kimlik doğrulaması kullanma**

```json
{
    "name": "HDFSLinkedService",
    "properties": {
        "type": "Hdfs",
        "typeProperties": {
            "url" : "http://<machine>:50070/webhdfs/v1/",
            "authenticationType": "Windows",
            "userName": "<username>@<domain>.com (for Kerberos auth)",
            "password": {
                "type": "SecureString",
                "value": "<password>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Veri kümesi özellikleri

Bölümleri ve veri kümelerini tanımlamak için kullanılabilen özellikleri tam listesi için veri kümeleri makalesine bakın. Bu bölümde HDFS veri kümesi tarafından desteklenen özellikler listesini sağlar.

HDFS verileri kopyalamak için kümesine tür özelliği ayarlamak **FileShare**. Aşağıdaki özellikler desteklenir:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Veri kümesi türü özelliği ayarlamak: **dosya paylaşımı** |Evet |
| folderPath | Klasör yolu. Örneğin: klasör/alt / |Evet |
| fileName | Dosya adını belirtin **folderPath** belirli bir dosyadan kopyalamak istiyorsanız. Bu özellik için herhangi bir değer belirtmezseniz, veri kümesi klasördeki tüm dosyaları kaynak olarak işaret eder. |Hayır |
| Biçimi | İsterseniz **olarak dosyaları kopyalama-olduğu** dosya tabanlı depoları arasında (ikili kopya), her iki girdi ve çıktı veri kümesi tanımlarında Biçim bölümü atlayın.<br/><br/>Belirli bir biçime sahip dosyaları ayrıştırma istiyorsanız, aşağıdaki dosya biçimi türleri desteklenir: **TextFormat**, **JsonFormat**, **AvroFormat**,  **OrcFormat**, **ParquetFormat**. Ayarlama **türü** şu değerlerden biri biçimine altında özellik. Daha fazla bilgi için bkz: [metin biçimi](supported-file-formats-and-compression-codecs.md#text-format), [Json biçimine](supported-file-formats-and-compression-codecs.md#json-format), [Avro biçimi](supported-file-formats-and-compression-codecs.md#avro-format), [Orc biçimi](supported-file-formats-and-compression-codecs.md#orc-format), ve [Parquet biçimi](supported-file-formats-and-compression-codecs.md#parquet-format) bölümler. |Hayır (yalnızca ikili kopyalama senaryosu) |
| Sıkıştırma | Veri sıkıştırma düzeyini ve türünü belirtin. Daha fazla bilgi için bkz: [desteklenen dosya biçimleri ve sıkıştırma codec](supported-file-formats-and-compression-codecs.md#compression-support).<br/>Desteklenen türler: **GZip**, **Deflate**, **Bzıp2**, ve **ZipDeflate**.<br/>Desteklenen düzeyler: **Optimal** ve **en hızlı**. |Hayır |

**Örnek:**

```json
{
    "name": "HDFSDataset",
    "properties": {
        "type": "FileShare",
        "linkedServiceName":{
            "referenceName": "<HDFS linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "folderPath": "folder/subfolder/",
            "fileName": "myfile.csv.gz",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ",",
                "rowDelimiter": "\n"
            },
            "compression": {
                "type": "GZip",
                "level": "Optimal"
            }
        }
    }
}
```

## <a name="copy-activity-properties"></a>Kopyalama etkinliğinin özellikleri

Bölümleri ve etkinlikleri tanımlamak için kullanılabilen özellikleri tam listesi için bkz: [ardışık düzen](concepts-pipelines-activities.md) makalesi. Bu bölümde HDFS kaynak tarafından desteklenen özellikler listesini sağlar.

### <a name="hdfs-as-source"></a>Kaynak olarak HDFS

HDFS verileri kopyalamak için kopyalama etkinliği için kaynak türünü ayarlayın. **HdfsSource**. Aşağıdaki özellikler kopyalama etkinliği desteklenen **kaynak** bölümü:

| Özellik | Açıklama | Gerekli |
|:--- |:--- |:--- |
| type | Kopyalama etkinliği kaynağı tür özelliği ayarlamak: **HdfsSource** |Evet |
| Özyinelemeli | Belirtilen klasörün alt klasörleri ya da yalnızca verileri özyinelemeli olarak okunur olup olmadığını gösterir. Özyinelemeli true ve havuz için ayarlandığında Not dosya tabanlı depolama, boş klasör/alt-folder havuz kopyalanır ve oluşturulan olmaz.<br/>İzin verilen değerler: **true** (varsayılan), **false** | Hayır |
| distcpSettings | HDFS Distcp'yi kullanırken özellik grubu. | Hayır |
| resourceManagerEndpoint | Yarn ResourceManager uç noktası | Evet Distcp'yi kullanıyorsanız |
| tempScriptPath | Geçici Distcp'yi komut dosyasını depolamak için kullanılan bir klasör yolu. Komut dosyası veri fabrikası tarafından oluşturulur ve kopyalama işini tamamladıktan sonra kaldırılır. | Evet Distcp'yi kullanıyorsanız |
| distcpOptions | Distcp'yi komutu için sağlanan ek seçenekler. | Hayır |

**Örnek: HDFS kaynağında UNLOAD kullanarak kopyalama etkinliği**

```json
"source": {
    "type": "HdfsSource",
    "distcpSettings": {
        "resourceManagerEndpoint": "resourcemanagerendpoint:8088",
        "tempScriptPath": "/usr/hadoop/tempscript",
        "distcpOptions": "-m 100"
    }
}
```

HDFS sonraki bölümünden verimli bir şekilde veri kopyalamak için Distcp'yi kullanma hakkında daha fazla bilgi edinin.

## <a name="use-distcp-to-copy-data-from-hdfs"></a>HDFS verileri kopyalamak için Distcp'yi kullanın

[Distcp'yi](https://hadoop.apache.org/docs/current3/hadoop-distcp/DistCp.html) Hadoop kümesi dağıtılmış kopya yapmak için bir Hadoop yerel komut satırı aracıdır. Distcp'yi komutu çalıştırdığınızda, ilk kopyalanması, Hadoop kümesine birkaç harita işleri oluşturmak için tüm dosyaları listelenir ve her bir harita iş ikili kopyalama havuzu kaynağından ne yapacağını.

Etkinlik destek dosyaları olarak kopyalamak için Distcp'yi kullanarak kopyalama-Azure blob'a olduğu (de dahil olmak üzere [kopyalama hazırlanan](copy-activity-performance.md) veya Azure Data Lake Store; bu durumda, tam olarak Self-hosted tümleştirme çalışma zamanında çalıştırmak yerine kümenizin güç yararlanabilirsiniz . Özellikle, kümeniz çok güçlü olduğunda daha iyi kopyalama verimlilik sağlar. Azure Data Factory yapılandırmanıza bağlı olarak, kopyalama etkinliği otomatik olarak distcp'yi komutu oluşturmak, Hadoop kümenize göndermek ve kopya durumunu izleyebilirsiniz.

### <a name="prerequsites"></a>Prerequsites

Kopyalamak için Distcp'yi kullanmak için dosyaları olarak-olan Azure Blob (hazırlanmış kopyalama dahil) veya Azure Data Lake Store HDFS Hadoop kümenizi gereksinimleri karşıladığından emin olun:

1. MapReduce ve Yarn hizmetleri etkinleştirildi.
2. Yarn sürümüdür 2.5 veya üstü.
3. HDFS sunucu, hedef veri deposuyla - Azure Blob veya Azure Data Lake Store tümleştirilir:

    - Azure Blob dosya sistemi yerel olarak Hadoop 2.7 itibaren desteklenir. Yalnızca Hadoop env yapılandırmada jar yolu belirtmeniz gerekir.
    - Azure Data Lake Store dosya sistemi, Hadoop 3.0.0-alpha1 ' başlayarak paketlenmiştir. Hadoop kümesi bu sürümden daha düşük ise, ilgili ADLS el ile içeri aktarmanız gerekir kümeden içine paketler (azure datalake store.jar) jar [burada](https://hadoop.apache.org/releases.html)ve Hadoop env yapılandırmada jar yolunu belirtin.

4. HDFS geçici bir klasörde hazırlayın. Bu geçici klasörü KB düzeyi alanı kaplar için Distcp'yi Kabuk betiği depolamak için kullanılır.
5. Emin olun, HDFS bağlantılı hizmetteki sağlanan kullanıcı hesabının iznine sahip bir) Yarn; uygulamada gönderme b) alt klasör oluşturun, geçici klasörün üstünde altında dosyaları okuma/yazma iznine sahip.

### <a name="configurations"></a>Yapılandırmalar

Verileri HDFS Distcp'yi kullanarak Azure Blob kopyalamak için kopyalama etkinliği yapılandırma örneği aşağıdadır:

**Örnek:**

```json
"activities":[
    {
        "name": "CopyFromHDFSToBlob",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "HDFSDataset",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "BlobDataset",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "HdfsSource",
                "distcpSettings": {
                    "resourceManagerEndpoint": "resourcemanagerendpoint:8088",
                    "tempScriptPath": "/usr/hadoop/tempscript",
                    "distcpOptions": "-strategy dynamic -map 100"
                }
            },
            "sink": {
                "type": "BlobSink"
            }
        }
    }
]
```

## <a name="use-kerberos-authentication-for-hdfs-connector"></a>HDFS Bağlayıcısı için Kerberos kimlik doğrulamasını kullan

HDFS Bağlayıcısı Kerberos kimlik doğrulamasını kullanmak amacıyla şirket içi ortamını ayarlamak için iki seçenek vardır. Bir durumunuz daha iyi uyduğunu seçebilirsiniz.
* Seçenek 1: [Kerberos alanı katılma Self-hosted tümleştirmesi çalışma zamanı makinede](#kerberos-join-realm)
* Seçenek 2: [Windows etki alanı Kerberos alanı arasında karşılıklı güven etkinleştir](#kerberos-mutual-trust)

### <a name="kerberos-join-realm"></a>Seçenek 1: Self-hosted tümleştirmesi çalışma zamanı makine Kerberos bölgesinde katılın.

#### <a name="requirements"></a>Gereksinimler

* Self-hosted tümleştirme çalışma zamanı makine Kerberos alanı katılmak gereken ve herhangi bir Windows etki alanına katılamıyor.

#### <a name="how-to-configure"></a>Nasıl yapılandırılır

**Self-Hosted tümleştirme çalışma zamanı makinede:**

1.  Çalıştırma **Ksetup** Kerberos KDC sunucu ve bölge yapılandırma yardımcı programı.

    Kerberos alanı bir Windows etki alanından farklı olduğundan makine bir çalışma grubunun bir üyesi yapılandırılmalıdır. Bu işlem, Kerberos alanı ayarlama ve şu şekilde bir KDC sunucusu eklemeyi de yararlanılabilir. Değiştir *REALM.COM* gerektiği gibi ilgili kendi bölge ile.

            C:> Ksetup /setdomain REALM.COM
            C:> Ksetup /addkdc REALM.COM <your_kdc_server_address>

    **Yeniden** makineyi 2 Bu komutları yürütülürken sonra.

2.  Yapılandırmayla doğrulayın **Ksetup** komutu. Çıktısı gibi olmalıdır:

            C:> Ksetup
            default realm = REALM.COM (external)
            REALM.com:
                kdc = <your_kdc_server_address>

**Azure Data Factory'de:**

* HDFS Bağlayıcısı'nı kullanarak yapılandırma **Windows kimlik doğrulaması** Kerberos asıl adı ve HDFS veri kaynağına bağlanmak için parola ile birlikte. Denetleme [HDFS bağlantılı hizmet özellikleri](#linked-service-properties) yapılandırma ayrıntıları bölümü.

### <a name="kerberos-mutual-trust"></a>Seçenek 2: Windows etki alanı Kerberos alanı arasında karşılıklı güven etkinleştirme

#### <a name="requirements"></a>Gereksinimler

*   Self-hosted tümleştirme çalışma zamanı makine bir Windows etki alanına katılması gerekir.
*   Etki alanı denetleyicisinin ayarlarını güncelleştirmek için izniniz olmalıdır.

#### <a name="how-to-configure"></a>Nasıl yapılandırılır

> [!NOTE]
> REALM.COM ve AD.COM aşağıdaki öğreticide kendi ilgili bölge ve gerektiği gibi etki alanı denetleyicisi ile değiştirin.

**KDC sunucusunda:**

1.  KDC yapılandırmasında Düzenle **krb5.conf** dosya KDC izin vermek için aşağıdaki yapılandırma şablonuna başvuran Windows etki alanı güven. Varsayılan olarak, yapılandırmanın bulunur **/etc/krb5.conf**.

            [logging]
             default = FILE:/var/log/krb5libs.log
             kdc = FILE:/var/log/krb5kdc.log
             admin_server = FILE:/var/log/kadmind.log

            [libdefaults]
             default_realm = REALM.COM
             dns_lookup_realm = false
             dns_lookup_kdc = false
             ticket_lifetime = 24h
             renew_lifetime = 7d
             forwardable = true

            [realms]
             REALM.COM = {
              kdc = node.REALM.COM
              admin_server = node.REALM.COM
             }
            AD.COM = {
             kdc = windc.ad.com
             admin_server = windc.ad.com
            }

            [domain_realm]
             .REALM.COM = REALM.COM
             REALM.COM = REALM.COM
             .ad.com = AD.COM
             ad.com = AD.COM

            [capaths]
             AD.COM = {
              REALM.COM = .
             }

  **Yeniden** yapılandırmadan sonra KDC Hizmeti.

2.  Adlı bir asıl hazırlama  **krbtgt/REALM.COM@AD.COM**  aşağıdaki komutla KDC Server'daki:

            Kadmin> addprinc krbtgt/REALM.COM@AD.COM

3.  İçinde **hadoop.security.auth_to_local** HDFS hizmet yapılandırma dosyası, ekleme `RULE:[1:$1@$0](.*@AD.COM)s/@.*//`.

**Etki alanı denetleyicisinde:**

1.  Aşağıdaki komutu çalıştırarak **Ksetup** komutları bir bölge girdisi eklemek için:

            C:> Ksetup /addkdc REALM.COM <your_kdc_server_address>
            C:> ksetup /addhosttorealmmap HDFS-service-FQDN REALM.COM

2.  Kerberos alanı için Windows etki alanından güven oluşturun. [parola] olan asıl parola  **krbtgt/REALM.COM@AD.COM** .

            C:> netdom trust REALM.COM /Domain: AD.COM /add /realm /passwordt:[password]

3.  İçinde Kerberos kullanılan şifreleme algoritmasını seçin.

    1. Sunucu Yöneticisi gidin > Grup İlkesi Yönetimi > etki alanı > Grup İlkesi nesneleri > Varsayılan veya etkin etki alanı ilkesi ve düzenleyin.

    2. İçinde **Grup İlkesi Yönetimi Düzenleyicisi** açılan pencerede, bilgisayar yapılandırması gidin > ilkeler > Windows Ayarları > Güvenlik Ayarları > yerel ilkeler > güvenlik seçenekleri ve yapılandırma **ağ güvenliği: yapılandırma şifreleme türleri için Kerberos izin**.

    3. Seçin, kullanmak istediğiniz şifreleme algoritması için KDC bağlayın. Genellikle, yalnızca tüm seçeneklerini belirleyebilirsiniz.

        ![Kerberos için yapılandırma şifreleme türleri](media/connector-hdfs/config-encryption-types-for-kerberos.png)

    4. Kullanım **Ksetup** üzerinde belirli bölge kullanılacak şifreleme algoritmasını belirtmek için komutu.

                C:> ksetup /SetEncTypeAttr REALM.COM DES-CBC-CRC DES-CBC-MD5 RC4-HMAC-MD5 AES128-CTS-HMAC-SHA1-96 AES256-CTS-HMAC-SHA1-96

4.  Windows etki alanında Kerberos asıl adı kullanmasını için etki alanı hesabını ve Kerberos asıl arasında eşleme oluşturun.

    1. Yönetimsel Araçları başlatmak > **Active Directory Kullanıcıları ve Bilgisayarları**.

    2. Tıklayarak Gelişmiş özellikleri yapılandırma **Görünüm** > **Gelişmiş Özellikler**.

    3. Hangi eşlemeleri oluşturmak istediğiniz ve görüntülemek için sağ hesabını bulun **adı eşlemeleri** > tıklatın **Kerberos adları** sekmesi.

    4. Sorumlu bölge ekleyin.

        ![Güvenlik kimliğine eşleyin](media/connector-hdfs/map-security-identity.png)

**Self-Hosted tümleştirme çalışma zamanı makinede:**

* Aşağıdaki komutu çalıştırarak **Ksetup** bir bölge girdisi eklemek için komutları.

            C:> Ksetup /addkdc REALM.COM <your_kdc_server_address>
            C:> ksetup /addhosttorealmmap HDFS-service-FQDN REALM.COM

**Azure Data Factory'de:**

* HDFS Bağlayıcısı'nı kullanarak yapılandırma **Windows kimlik doğrulaması** , etki alanı hesabı veya Kerberos asıl HDFS veri kaynağına bağlanmak için birlikte. Denetleme [HDFS bağlantılı hizmet özellikleri](#linked-service-properties) yapılandırma ayrıntıları bölümü.


## <a name="next-steps"></a>Sonraki adımlar
Kaynakları ve havuzlarını Azure Data Factory kopyalama etkinliği tarafından desteklenen veri depoları listesi için bkz: [desteklenen veri depoları](copy-activity-overview.md#supported-data-stores-and-formats).