---
title: "Azure işlem - Linux tanılama uzantısını | Microsoft Docs"
description: "Azure Linux tanılama uzantısını (ölçümleri toplamak ve Linux Azure'da çalışan Vm'lerden gelen olayları günlüğe LAD) yapılandırmak nasıl."
services: virtual-machines-linux
author: jasonzio
manager: anandram
ms.service: virtual-machines-linux
ms.tgt_pltfrm: vm-linux
ms.topic: article
ms.date: 05/09/2017
ms.author: jasonzio
ms.openlocfilehash: ebb963236a069f272499fce59945d0cf0d3d647f
ms.sourcegitcommit: c7215d71e1cdeab731dd923a9b6b6643cee6eb04
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/17/2017
---
# <a name="use-linux-diagnostic-extension-to-monitor-metrics-and-logs"></a>Ölçümleri ve günlükleri izlemek için Linux tanılama uzantısını kullanın

Bu belgede sürüm 3.0 ve Linux tanılama uzantısı'nın daha yeni açıklanmaktadır.

> [!IMPORTANT]
> 2.3 ve eski sürümü hakkında daha fazla bilgi için bkz: [bu belgeyi](./classic/diagnostic-extension-v2.md).

## <a name="introduction"></a>Giriş

Linux tanılama uzantısını kullanıcı İzleyicisi sistem durumu Microsoft Azure üzerinde çalışan bir Linux VM yardımcı olur. Aşağıdaki özellikleri içerir:

* Sanal makineden sistem performans ölçümleri toplar ve bunları belirtilen depolama hesabı belirli bir tabloda depolar.
* Syslog günlüğü olaylarını alır ve bunları belirtilen depolama hesabında belirli bir tabloda depolar.
* Kullanıcıların, toplanan ve karşıya veri ölçümlerini özelleştirme olanak tanır.
* Syslog tesisler ve toplanan ve karşıya olayların önem düzeyleri özelleştirmek kullanıcıların sağlar.
* Belirtilen günlük dosyalarını karşıya yüklemek için atanmış depolama tablosu olanak tanır.
* Rastgele EventHub uç noktaları ve atanan depolama hesabındaki BLOB JSON biçimli ölçümleri ve günlük olayları göndermeyi destekler.

Bu uzantı, hem Azure dağıtım modelleri ile çalışır.

## <a name="installing-the-extension-in-your-vm"></a>VM uzantısı yükleme

Azure PowerShell cmdlet'lerini, Azure CLI betikleri veya Azure dağıtım şablonları kullanarak bu uzantı etkinleştirebilirsiniz. Daha fazla bilgi için bkz: [uzantıları özelliklerinin](./extensions-features.md).

Azure Portalı'nı etkinleştirmek veya LAD 3.0 yapılandırmak için kullanılamaz. Bunun yerine, yükler ve sürüm 2.3 yapılandırır. Azure portal grafikleri ve uyarıları her iki sürümünün de uzantısı verilerle çalışır.

Bu yükleme yönergeleri ve [indirilebilir örnek yapılandırma](https://raw.githubusercontent.com/Azure/azure-linux-extensions/master/Diagnostic/tests/lad_2_3_compatible_portal_pub_settings.json) LAD 3.0 yapılandırın:

* yakalama ve LAD 2.3 tarafından sağlanan ölçümler aynı depolama;
* dosya sistemi ölçümleri, LAD 3.0 için yeni yararlı bir dizi yakalama;
* Yakalama LAD 2.3 ile etkin varsayılan syslog toplama;
* Grafik ve VM ölçümleri uyarmak için Azure portal deneyimi sağlar.

İndirilebilir yapılandırma yalnızca bir örnektir; kendi gereksinimlerinize uyacak şekilde değiştirin.

### <a name="prerequisites"></a>Ön koşullar

* **Azure Linux Aracısı sürüm 2.2.0 veya daha sonra**. Çoğu Azure VM Linux galeri görüntüleri sürüm 2.2.7 dahil etmek veya daha sonra. Çalıştırma `/usr/sbin/waagent -version` VM'de yüklü sürümünü onaylamak için. VM Konuk aracısının daha eski bir sürümünü çalıştırıyorsa, izleyin [bu yönergeleri](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/update-agent) güncelleştirin.
* **Azure CLI**. [Azure CLI 2.0 ayarlama](https://docs.microsoft.com/cli/azure/install-azure-cli) makinenizde ortamı.
* Zaten yoksa wget komutu: çalıştırmak `sudo apt-get install wget`.
* Var olan bir Azure aboneliği ve içerdiği verileri depolamak için varolan bir depolama hesabı.

### <a name="sample-installation"></a>Örnek yükleme

İlk üç satırını doğru parametreleri doldurun, sonra da kök olarak bu betiği yürütün:

```bash
# Set your Azure VM diagnostic parameters correctly below
my_resource_group=<your_azure_resource_group_name_containing_your_azure_linux_vm>
my_linux_vm=<your_azure_linux_vm_name>
my_diagnostic_storage_account=<your_azure_storage_account_for_storing_vm_diagnostic_data>

# Should login to Azure first before anything else
az login

# Select the subscription containing the storage account
az account set --subscription <your_azure_subscription_id>

# Download the sample Public settings. (You could also use curl or any web browser)
wget https://raw.githubusercontent.com/Azure/azure-linux-extensions/master/Diagnostic/tests/lad_2_3_compatible_portal_pub_settings.json -O portal_public_settings.json

# Build the VM resource ID. Replace storage account name and resource ID in the public settings.
my_vm_resource_id=$(az vm show -g $my_resource_group -n $my_linux_vm --query "id" -o tsv)
sed -i "s#__DIAGNOSTIC_STORAGE_ACCOUNT__#$my_diagnostic_storage_account#g" portal_public_settings.json
sed -i "s#__VM_RESOURCE_ID__#$my_vm_resource_id#g" portal_public_settings.json

# Build the protected settings (storage account SAS token)
my_diagnostic_storage_account_sastoken=$(az storage account generate-sas --account-name $my_diagnostic_storage_account --expiry 9999-12-31T23:59Z --permissions wlacu --resource-types co --services bt -o tsv)
my_lad_protected_settings="{'storageAccountName': '$my_diagnostic_storage_account', 'storageAccountSasToken': '$my_diagnostic_storage_account_sastoken'}"

# Finallly tell Azure to install and enable the extension
az vm extension set --publisher Microsoft.Azure.Diagnostics --name LinuxDiagnostic --version 3.0 --resource-group $my_resource_group --vm-name $my_linux_vm --protected-settings "${my_lad_protected_settings}" --settings portal_public_settings.json
```

Örnek yapılandırmasını ve içeriğini, URL'sini olan değiştirilebilir. Portal ayarlarını JSON dosyasının bir kopyasını indirin ve gereksinimlerinize göre özelleştirin. Tüm Şablonları ya da Otomasyon, oluşturmak, her zaman bu URL'yi indirmek yerine kendi kopyanızı kullanmanız gerekir.

### <a name="updating-the-extension-settings"></a>Uzantı ayarları güncelleştiriliyor

Korumalı veya genel ayarları değiştirdikten sonra bunları VM'ye aynı komutu çalıştırarak dağıtın. Herhangi bir şey ayarlarında değiştirdiyseniz, güncelleştirilmiş ayarlar uzantısı gönderilir. LAD yapılandırmasını yeniden yükler ve kendisini yeniden başlatır.

### <a name="migration-from-previous-versions-of-the-extension"></a>Uzantı eski sürümlerinden geçiş

Uzantısı'nın en son sürüm **3.0**. **Eski sürümlerini (2.x) kullanım dışıdır ve ve 31 Temmuz 2018 sonrasında yayımdan**.

> [!IMPORTANT]
> Bu uzantı uzantısı yapılandırma için önemli değişiklikler tanıtır. Bu tür bir değişiklik uzantısı güvenliğini geliştirmek üzere yapılmıştır; Sonuç olarak, geriye dönük uyumluluk 2.x ile korunmasını değil. Ayrıca, bu uzantı için uzantı yayımcı yayımcı 2.x sürümleri için farklıdır.
>
> Bu yeni sürümü uzantısı 2.x geçirmek için (altında eski Yayımcı adı) eski uzantısını Kaldır, sonra uzantısı 3 sürümünü yükleyin.

Öneriler:

* Uzantı etkin otomatik alt sürüm yükseltme işlemine yükleyin.
  * Azure XPLAT CLI veya Powershell ile uzantısı yüklüyorsanız, Klasik dağıtım modeli VM'ler, '3.*' sürüm olarak belirtin.
  * Sanal makineleri üzerinde Azure Resource Manager dağıtım modeli, dahil ' "olan": true' VM Dağıtım şablonu olarak.
* Yeni/farklı bir depolama hesabı LAD 3.0 için kullanın. Bir hesap sorunlu paylaşımı olun birkaç küçük uyumsuzlukları LAD 2.3 LAD 3.0 arasındaki vardır:
  * LAD 3.0 syslog olayları farklı bir ada sahip bir tablo olarak depolar.
  * CounterSpecifier dizeleri için `builtin` ölçümleri farklı LAD 3. 0 '.

## <a name="protected-settings"></a>Korumalı ayarları

Bu yapılandırma bilgileri kümesi genel görünümünde, örneğin, depolama kimlik korunması gereken hassas bilgiler içerir. Bu ayarlar için iletilen ve uzantısı ile şifrelenmiş biçimde depolanır.

```json
{
    "storageAccountName" : "the storage account to receive data",
    "storageAccountEndPoint": "the hostname suffix for the cloud for this account",
    "storageAccountSasToken": "SAS access token",
    "mdsdHttpProxy": "HTTP proxy settings",
    "sinksConfig": { ... }
}
```

Ad | Değer
---- | -----
storageAccountName | Veri uzantısı tarafından yazılmış depolama hesabı adı.
storageAccountEndPoint | (isteğe bağlı) Depolama hesabının bulunduğu bulut tanımlayan uç noktası. Bu ayar olmazsa LAD Azure genel bulut için varsayılan olarak `https://core.windows.net`. Bir depolama hesabı Azure Almanya, Azure kamu ya da Azure Çin'de kullanmak için bu değeri uygun şekilde ayarlayın.
storageAccountSasToken | Bir [hesap SAS belirteci](https://azure.microsoft.com/blog/sas-update-account-sas-now-supports-all-storage-services/) Blob ve tablo hizmeti (`ss='bt'`), kapsayıcılar ve nesneler için geçerlidir (`srt='co'`), hangi verir ekleyin, oluşturmak, liste, güncelleştirme ve yazma izinleri (`sp='acluw'`). Yapmak *değil* başında soru işareti (?) içerir.
mdsdHttpProxy | (isteğe bağlı) Belirtilen depolama hesabına ve uç noktası'na bağlanmak uzantıyı etkinleştirmek için gereken HTTP proxy bilgileri.
sinksConfig | (isteğe bağlı) Alternatif hedefler için ölçümleri ve olayları teslim edilebilir ayrıntıları. Genişletme tarafından desteklenen her veri havuzu belirli ayrıntılarını izleyen bölümlerde açıklanmıştır.

Azure portalı üzerinden gerekli SAS belirteci kolayca oluşturabilirsiniz.

1. Uzantı yazmak için istediğiniz genel amaçlı depolama hesabı seçin
1. "Erişim imzası soldaki menüden ayarları bölümünden paylaşılan" seçin
1. Daha önce açıklandığı gibi uygun bölümleri olun
1. "SAS oluştur" düğmesine tıklayın.

![Görüntü](./media/diagnostic-extension/make_sas.png)

Oluşturulan SAS storageAccountSasToken alana kopyalayın; önde gelen soru işareti kaldırın ("?").

### <a name="sinksconfig"></a>sinksConfig

```json
"sinksConfig": {
    "sink": [
        {
            "name": "sinkname",
            "type": "sinktype",
            ...
        },
        ...
    ]
},
```

Bu isteğe bağlı bir bölüm, uzantı topladığı bilgileri gönderdiği ek hedefleri tanımlar. "Havuz" dizi her ek veri havuzu için bir nesne içeriyor. "Tür" özniteliği nesnesindeki diğer öznitelikleri belirler.

Öğe | Değer
------- | -----
ad | Bu havuz başka bir uzantı yapılandırmasındaki başvurmak için kullanılan bir dize.
type | Tanımlanan Havuz türü. Diğer değerler, bu tür durumlarda (varsa) belirler.

Linux tanılama uzantısını 3.0 sürümünü iki havuz türlerini destekler: EventHub ve JsonBlob.

#### <a name="the-eventhub-sink"></a>EventHub havuz

```json
"sink": [
    {
        "name": "sinkname",
        "type": "EventHub",
        "sasURL": "https SAS URL"
    },
    ...
]
```

"SasURL" giriş olay verileri yayımlanması gerekir Hub için SAS belirteci dahil tam URL'yi içerir. LAD Gönder sağlayan bir ilke adlandırma SAS talep gerektirir. Örnek:

* Adlı bir olay hub'ları ad alanı oluşturma`contosohub`
* Bir Event Hub'adlı ad alanı oluşturma`syslogmsgs`
* Olay adlı hub'ına bir paylaşılan erişim ilkesi oluşturun `writer` gönderme talep sağlar

Bir SAS iyi 1 Ocak 2018 gece yarısı UTC kadar oluşturduysanız sasURL değeri olabilir:

```url
https://contosohub.servicebus.windows.net/syslogmsgs?sr=contosohub.servicebus.windows.net%2fsyslogmsgs&sig=xxxxxxxxxxxxxxxxxxxxxxxxx&se=1514764800&skn=writer
```

Olay hub'ları için SAS belirteci oluşturma hakkında daha fazla bilgi için bkz: [bu web sayfası](../../event-hubs/event-hubs-authentication-and-security-model-overview.md).

#### <a name="the-jsonblob-sink"></a>JsonBlob havuz

```json
"sink": [
    {
        "name": "sinkname",
        "type": "JsonBlob"
    },
    ...
]
```

JsonBlob havuzuna yönlendirilen verileri BLOB'ları Azure depolama alanında depolanır. Her örneği LAD blob her saat için her bir havuz adı oluşturur. Her bir blob her zaman nesne sözdizimsel olarak geçerli bir JSON dizisi içerir. Yeni girişler dizisine otomatik olarak eklenir. BLOB'ları, havuz olarak aynı ada sahip bir kapsayıcıda depolanır. Blob kapsayıcı adları için Azure depolama kuralları JsonBlob havuzlarını adları için geçerlidir: küçük harf alfasayısal ASCII karakterler veya tire 3 ile 63 arasında.

## <a name="public-settings"></a>Genel ayarları

Bu yapı çeşitli bloklarını uzantısı tarafından toplanan bilgiler denetleyen ayarları içerir. Her ayar isteğe bağlıdır. Belirtirseniz `ladCfg`, de belirtmeniz gerekir `StorageAccount`.

```json
{
    "ladCfg":  { ... },
    "perfCfg": { ... },
    "fileLogs": { ... },
    "StorageAccount": "the storage account to receive data",
    "mdsdHttpProxy" : ""
}
```

Öğe | Değer
------- | -----
StorageAccount | Veri uzantısı tarafından yazılmış depolama hesabı adı. Belirtilen ada olmalıdır [ayarların korumalı](#protected-settings).
mdsdHttpProxy | (isteğe bağlı) Aynı olarak [ayarların korumalı](#protected-settings). Ortak değeri, varsa özel değer tarafından geçersiz ayarlayın. Bir parola gibi bir gizlilik bulunması proxy ayarlarını yerleştirin [ayarların korumalı](#protected-settings).

Kalan öğeleri aşağıdaki bölümlerde ayrıntılı olarak açıklanmıştır.

### <a name="ladcfg"></a>ladCfg

```json
"ladCfg": {
    "diagnosticMonitorConfiguration": {
        "eventVolume": "Medium",
        "metrics": { ... },
        "performanceCounters": { ... },
        "syslogEvents": { ... }
    },
    "sampleRateInSeconds": 15
}
```

Bu isteğe bağlı yapısı denetimleri ölçümleri ve Azure ölçümleri hizmetine ve diğer veri teslimi için günlükleri toplama iç havuzlar. Ya da belirtmeniz gerekir `performanceCounters` veya `syslogEvents` veya her ikisini de. Belirtmeniz gerekir `metrics` yapısı.

Öğe | Değer
------- | -----
eventVolume | (isteğe bağlı) Depolama tablo içinde oluşturulan bölümlere sayısını denetler. Biri olmalıdır `"Large"`, `"Medium"`, veya `"Small"`. Belirtilmezse, varsayılan değer: `"Medium"`.
sampleRateInSeconds | (isteğe bağlı) Ham (unaggregated) ölçümleri topluluğu arasındaki varsayılan zaman aralığı. En küçük desteklenen örnek hızı 15 saniyedir. Belirtilmezse, varsayılan değer: `15`.

#### <a name="metrics"></a>metrics

```json
"metrics": {
    "resourceId": "/subscriptions/...",
    "metricAggregation" : [
        { "scheduledTransferPeriod" : "PT1H" },
        { "scheduledTransferPeriod" : "PT5M" }
    ]
}
```

Öğe | Değer
------- | -----
resourceId | VM ait olduğu Azure Resource Manager kaynak kimliği VM veya sanal makine ölçek kümesi. Bu ayar olmalıdır herhangi JsonBlob havuz yapılandırmada kullanılırsa da belirtilmiş.
scheduledTransferPeriod | Hesaplanan ve Azure 8601 olduğu zaman aralığı ifade edilen ölçümleri, aktarılan toplam ölçümleri olan sıklığı. En küçük aktarım süresi 60, diğer bir deyişle, PT1M saniyedir. En az bir scheduledTransferPeriod belirtmeniz gerekir.

Performans sayaçları bölümünde belirtilen ölçümleri örnekleri 15 dakikada toplanan veya örnek oranı açıkça sayaç için tanımlanmış. Birden çok scheduledTransferPeriod sıklıklarını (örnekte olduğu gibi) görünüyorsa, her bir toplama bağımsız olarak hesaplanır.

#### <a name="performancecounters"></a>performans sayaçları

```json
"performanceCounters": {
    "sinks": "",
    "performanceCounterConfiguration": [
        {
            "type": "builtin",
            "class": "Processor",
            "counter": "PercentIdleTime",
            "counterSpecifier": "/builtin/Processor/PercentIdleTime",
            "condition": "IsAggregate=TRUE",
            "sampleRate": "PT15S",
            "unit": "Percent",
            "annotation": [
                {
                    "displayName" : "Aggregate CPU %idle time",
                    "locale" : "en-us"
                }
            ]
        }
    ]
}
```

Bu isteğe bağlı bir bölüm ölçümleri koleksiyonunu denetler. Ham örnekleri her biri için toplanmış [scheduledTransferPeriod](#metrics) bu değerleri oluşturmak için:

* Ortalama
* en az
* Maksimum
* Son toplanan değeri
* Toplama hesaplamak için kullanılan ham örneklerin sayısı

Öğe | Değer
------- | -----
İç havuzlar | (isteğe bağlı) Hangi LAD gönderir ölçüm sonuçlarını toplanan havuzlarını adlarının virgülle ayrılmış listesi. Tüm toplanan ölçümler için listelenen her havuz yayımlanır. Bkz: [sinksConfig](#sinksconfig). Örnek: `"EHsink1, myjsonsink"`.
type | Ölçümün gerçek sağlayıcısını tanımlar.
sınıfı | "Sayacı" ile birlikte sağlayıcının ad alanı içindeki belirli ölçüm tanımlar.
Sayaç | "Sınıf" ile birlikte sağlayıcının ad alanı içindeki belirli ölçüm tanımlar.
counterSpecifier | Azure ölçümleri ad alanı içindeki belirli ölçüm tanımlar.
Koşul | (isteğe bağlı) Ölçüm uygular veya toplama söz konusu nesne tüm örneklerinde seçer nesne belirli bir örneği seçer. Daha fazla bilgi için bkz: [ `builtin` ölçüm tanımlarını](#metrics-supported-by-builtin).
sampleRate | Toplanan ve bu ölçüm için ham örnek hızı ayarlar 8601 aralığı BELİRTİR. Ayarlanmadı, toplama aralığı değeri olarak ayarlanmış olup olmadığını [sampleRateInSeconds](#ladcfg). En kısa desteklenen örnek hızı 15 (PT15S) saniyedir.
Birim | Bu dizeler biri olmalıdır: "Count", "Bayt sayısı", "Saniye", "Yüzde", "CountPerSecond", "BytesPerSecond", "Milisaniyelik". Ölçü birimi tanımlar. Toplanan veri tüketicileri bu birimi eşleştirmek için toplanan veri değerleri bekler. Bu alan LAD yoksayar.
Görünen adı | Etiket (ilişkili yerel ayarı tarafından belirtilen dilde) bu verileri Azure ölçümleri eklenmiş. Bu alan LAD yoksayar.

CounterSpecifier rasgele bir tanımlayıcıdır. Tüketiciler ölçümleri, Azure portal grafik ister ve özelliği, uyarı counterSpecifier "bir ölçüm veya bir ölçüm örneğini tanımlayan anahtar olarak" kullanın. İçin `builtin` ölçümleri, öneririz ile başlayan counterSpecifier değerleri kullandığınız `/builtin/`. Ölçüm belirli bir örneği topluyorsanız counterSpecifier değerine örneğinin tanıtıcısı ekleme öneririz. Bazı örnekler:

* `/builtin/Processor/PercentIdleTime`-Boşta kalma süresi tüm Vcpu'lar ortalaması
* `/builtin/Disk/FreeSpace(/mnt)`-/Mnt dosya sistemi boş alan
* `/builtin/Disk/FreeSpace`-Boş alan tüm takılı bağlanan dosya sistemlerinin ortalaması

Ne LAD ne de Azure portalında herhangi bir desenle eşleşen counterSpecifier değeri bekler. CounterSpecifier değerleri nasıl oluşturmak tutarlı olması.

Belirttiğinizde `performanceCounters`, LAD her zaman Yazar verileri Azure depolama alanında bir tablo. JSON BLOB'ları ve/veya olay hub'ları yazılan aynı veri olabilir, ancak bir tabloya veri depolama devre dışı bırakılamıyor. Tanılama uzantısını tüm örnekleri aynı depolama hesabı adı kullanmak üzere yapılandırılmış ve uç nokta, aynı tabloya kendi ölçümleri ve günlükleri ekleyin. Çok fazla sayıda sanal makineleri aynı tablo bölüme yazıyorsanız, Azure yazma daraltabilir bu bölümü. EventVolume ayar 1 (küçük), 10 (Orta) üzerinden yayılan ya da 100 (büyük) farklı bölümleri sağlanacak girişlerinin neden olur. Genellikle, "Orta" trafik değil kısıtlanan emin olmak yeterli olur. Azure Portalı'nın Azure ölçümleri özelliği veri grafikleri üretmek için veya uyarıları tetiklemek için bu tabloyu kullanır. Bu dizeler birleşimini tablo adıdır:

* `WADMetrics`
* Tabloda depolanan toplanmış değerler için "scheduledTransferPeriod"
* `P10DV2S`
* 10 günde değişiklikleri "YYYYAAGG" biçiminde bir tarih

Örnekler `WADMetricsPT1HP10DV2S20170410` ve `WADMetricsPT1MP10DV2S20170609`.

#### <a name="syslogevents"></a>syslogEvents

```json
"syslogEvents": {
    "sinks": "",
    "syslogEventConfiguration": {
        "facilityName1": "minSeverity",
        "facilityName2": "minSeverity",
        ...
    }
}
```

Bu isteğe bağlı bir bölüm syslog günlük olayları koleksiyonu denetler. Bölüm atlanırsa, syslog olayları hiç yakalanmaz.

SyslogEventConfiguration koleksiyon her syslog özelliğini ilgi için bir giriş içeriyor. MinSeverity "Hiçbiri" için belirli bir özellik varsa veya bu tesis öğesinde hiç görünmüyorsa, bu tesis hiçbir olaylarından yakalanır.

Öğe | Değer
------- | -----
İç havuzlar | Ayrı günlük olayları yayımlanan havuzlarını adlarının virgülle ayrılmış listesi. SyslogEventConfiguration kısıtlamalarına eşleşen tüm günlük olayları için listelenen her havuz yayımlanır. Örnek: "EHforsyslog"
facilityName | Bir syslog tesis adı (gibi "günlük\_kullanıcı" veya "günlük\_LOCAL0"). "Özelliği" bölümüne bakın [syslog adam sayfa](http://man7.org/linux/man-pages/man3/syslog.3.html) tam listesi için.
minSeverity | Bir syslog önem düzeyi (gibi "günlük\_hata" veya "günlük\_bilgileri"). "Düzeyi" bölümüne bakın [syslog adam sayfa](http://man7.org/linux/man-pages/man3/syslog.3.html) tam listesi için. Uzantı veya belirtilen düzeyin üstü tesis gönderilen olayları yakalar.

Belirttiğinizde `syslogEvents`, LAD her zaman Yazar verileri Azure depolama alanında bir tablo. JSON BLOB'ları ve/veya olay hub'ları yazılan aynı veri olabilir, ancak bir tabloya veri depolama devre dışı bırakılamıyor. Bu tablo için bölümleme davranışı için açıklandığı gibi aynıdır `performanceCounters`. Bu dizeler birleşimini tablo adıdır:

* `LinuxSyslog`
* 10 günde değişiklikleri "YYYYAAGG" biçiminde bir tarih

Örnekler `LinuxSyslog20170410` ve `LinuxSyslog20170609`.

### <a name="perfcfg"></a>perfCfg

Bu isteğe bağlı bir bölüm rasgele yürütülmesi denetimleri [OMI](https://github.com/Microsoft/omi) sorgular.

```json
"perfCfg": [
    {
        "namespace": "root/scx",
        "query": "SELECT PercentAvailableMemory, PercentUsedSwap FROM SCX_MemoryStatisticalInformation",
        "table": "LinuxOldMemory",
        "frequency": 300,
        "sinks": ""
    }
]
```

Öğe | Değer
------- | -----
Namespace | (isteğe bağlı) Sorgu içinde yürütülmesi gereken OMI ad alanı. Belirtilmezse, "kök/tarafından uygulanan scx", varsayılan değer: [System Center platformlar arası sağlayıcıları](http://scx.codeplex.com/wikipage?title=xplatproviders&referringTitle=Documentation).
sorgu | Yürütülecek OMI sorgu.
Tablo | (isteğe bağlı) Azure depolama tablosunda belirtilen depolama hesabı (bkz [ayarların korumalı](#protected-settings)).
frequency | (isteğe bağlı) Sorgu yürütme arasındaki saniye sayısı. 300 (5 dakika); varsayılan değer: en düşük değer 15 saniyedir.
İç havuzlar | (isteğe bağlı) Ham örnek ölçüm sonuçlarını yayımlanan ek havuzlarını adlarının virgülle ayrılmış listesi. Bu ham örnekleri toplama yok veya Azure ölçümleri uzantısı tarafından hesaplanır.

"Tablo" veya "İç havuzlar" ya da her ikisini de belirtilmesi gerekir.

### <a name="filelogs"></a>fileLogs

Günlük dosyalarının yakalama denetler. LAD dosyasına yazılır gibi yeni metin satırlarını yakalar ve tablo satırları ve/veya belirtilen tüm havuzlarını (JsonBlob veya EventHub) yazar.

```json
"fileLogs": [
    {
        "file": "/var/log/mydaemonlog",
        "table": "MyDaemonEvents",
        "sinks": ""
    }
]
```

Öğe | Değer
------- | -----
Dosya | İzlenen ve yakalanan için günlük dosyasının tam yol adı. Yol, tek bir dosya adı olmalıdır; bir dizin adı veya joker karakterler içeriyor.
Tablo | (isteğe bağlı) İçine yeni dosya "kuyruğu" satırlarından yazılır belirtilen depolama hesabında (belirtildiği şekilde korumalı yapılandırma), Azure depolama tablo.
İç havuzlar | (isteğe bağlı) Günlük gönderilen hatları için ek havuzlarını adlarının virgülle ayrılmış listesi.

"Tablo" veya "İç havuzlar" ya da her ikisini de belirtilmesi gerekir.

## <a name="metrics-supported-by-the-builtin-provider"></a>Yerleşik sağlayıcı tarafından desteklenen ölçümleri

Yerleşik ölçüm ölçümleri geniş bir kullanıcı kümesi için en ilgi çekici bir kaynak sağlayıcıdır. Bu ölçümler beş geniş sınıflara ayrılır:

* İşlemci
* Bellek
* Ağ
* Dosya sistemi
* Disk

### <a name="builtin-metrics-for-the-processor-class"></a>İşlemci sınıfı için yerleşik ölçümleri

Ölçümleri işlemci sınıfının VM'deki işlemci kullanımı hakkında bilgi sağlar. Yüzdeleri toplanırken ortalama tüm CPU'lar arasında sonucudur. İki vCPU VM ile bir vCPU % 100 meşgul ve diğer % 100 boşta şeklindeydi durumunda bildirilen PercentIdleTime 50 olur. Her vCPU % 50 aynı dönem için meşgul ise, bildirilen sonuç da 50 olur. Bir vCPU % 100 meşgul ve diğerleri boşta olan bir dört vCPU VM bildirilen PercentIdleTime 75 olacaktır.

Sayaç | Anlamı
------- | -------
PercentIdleTime | İşlemci çekirdeği boşta döngü yürütülmekte toplama penceresi sırasında zamanı yüzdesi
percentProcessorTime | Boş olmayan bir iş parçacığı yürütme zamanı yüzdesi
PercentIOWaitTime | G/ç işlemlerinin tamamlanması beklenirken zaman yüzdesi
PercentInterruptTime | Donanım/yazılım kesmeler ve DPC'ler (ertelenmiş yordam çağrılarını) yürütme zamanı yüzdesi
PercentUserTime | Boş olmayan süresini toplama penceresi sırasında zamanın normal öncelik en fazla kullanıcı yüzdesi
PercentNiceTime | Boş olmayan süre yüzdesi alçaltılmış (iyi) öncelikli harcanan
PercentPrivilegedTime | Boş olmayan süresini yüzde ayrıcalıklı (çekirdek) modda harcanan

İlk dört sayaçları % 100'e sum. Son üç sayaçlar da toplam % 100; Bunlar PercentProcessorTime, PercentIOWaitTime ve PercentInterruptTime toplamını ayırabilir.

Tüm işlemciler arasında toplanan tek bir ölçüm elde etmek için ayarlama `"condition": "IsAggregate=TRUE"`. İkinci mantıksal işlemci dört vCPU VM gibi belirli bir işlemci için bir ölçüm elde etmek için ayarlama `"condition": "Name=\\"1\\""`. Mantıksal işlemci numaralarıdır aralığında `[0..n-1]`.

### <a name="builtin-metrics-for-the-memory-class"></a>Bellek sınıfı için yerleşik ölçümleri

Ölçümleri bellek sınıfının disk belleği ve değiştirmeyi bellek kullanımı hakkında bilgi sağlar.

Sayaç | Anlamı
------- | -------
AvailableMemory | MIB kullanılabilir fiziksel bellek
PercentAvailableMemory | Kullanılabilir fiziksel belleğin toplam bellek yüzdesi
UsedMemory | Kullanımda fiziksel bellek (MIB)
PercentUsedMemory | Kullanımdaki fiziksel belleğin toplam bellek yüzdesi
PagesPerSec | Toplam disk belleği (okuma/yazma)
PagesReadPerSec | Depolama (takas dosyası, program dosyası, eşlenmiş dosya, vb.) yedekleme sayfaları oku
PagesWrittenPerSec | Yedekleme deposu (takas dosyası, eşlenmiş dosya, vb.) yazılan sayfa
AvailableSwap | Kullanılmayan takas alanı (MIB)
PercentAvailableSwap | Kullanılmayan değiştirme alanının toplam değiştirme yüzdesi
UsedSwap | Kullanımda takas alanı (MIB)
PercentUsedSwap | Değiştirme alanının toplam değiştirme yüzdesi olarak kullanımda

Bu sınıf ölçümleri yalnızca tek bir örneği vardır. "Koşul" özniteliği yok yararlı ayarlara sahip ve alınmamalıdır.

### <a name="builtin-metrics-for-the-network-class"></a>Ağ sınıfı için yerleşik ölçümleri

Ölçümleri ağ sınıfı, önyüklemeden tek tek ağ arabirimleri üzerinde ağ etkinliği hakkında bilgi sağlar. LAD ana ölçümleri alınabilir bant genişliği ölçümleri kullanıma sunmuyor.

Sayaç | Anlamı
------- | -------
BytesTransmitted | Önyüklemeden gönderilen toplam bayt sayısı
BytesReceived | Önyüklemeden alınan toplam bayt sayısı
BytesTotal | Gönderilen veya alınan önyüklemeden toplam bayt sayısı
PacketsTransmitted | Önyüklemeden gönderilen toplam paket sayısı
PacketsReceived | Önyüklemeden alınan toplam paket sayısı
TotalRxErrors | Sayısı önyüklemeden hataları alırsınız
TotalTxErrors | Sayısı önyüklemeden iletme işlemi hataları
TotalCollisions | Çakışmaları önyüklemeden ağ bağlantı noktaları tarafından bildirilen sayısı

 Bu sınıf instanced rağmen LAD tüm ağ aygıtlarını toplanan yakalama ağ ölçümleri desteklemez. Eth0 gibi belirli bir arabirim için ölçümler elde etmek için ayarlama `"condition": "InstanceID=\\"eth0\\""`.

### <a name="builtin-metrics-for-the-filesystem-class"></a>dosya sistemi sınıfı için yerleşik ölçümleri

Dosya sistemi sınıfı ölçümleri filesystem kullanımı hakkında bilgi sağlar. Mutlak ve yüzde değerleri, normal bir kullanıcıya (kök değil) görüntülenen olarak bildirilir.

Sayaç | Anlamı
------- | -------
FreeSpace | Bayt cinsinden kullanılabilir disk alanı
UsedSpace | Kullanılan disk alanı bayt
PercentFreeSpace | Boş alan yüzdesi
PercentUsedSpace | Kullanılan yüzde alanı
PercentFreeInodes | Kullanılmayan inode yüzdesi
PercentUsedInodes | Tüm bağlanan dosya sistemlerinin arasında toplamı (kullanımda) ayrılmış inode yüzdesi
BytesReadPerSecond | Saniye başına okunan bayt
BytesWrittenPerSecond | Saniye başına yazılan bayt
BytesPerSecond | Okunan veya saniye başına yazılan bayt sayısı
ReadsPerSecond | Saniye başına okuma işlemleri
WritesPerSecond | Saniye başına işlem yazma
TransfersPerSecond | Saniye başına okuma veya yazma işlemi

Tüm dosya sistemleri arasında toplanmış değerler elde edilebilir ayarlayarak `"condition": "IsAggregate=True"`. Belirli bağlı dosya sistemi için aşağıdaki gibi değerler "/ mnt", ayarlayarak elde `"condition": 'Name="/mnt"'`.

### <a name="builtin-metrics-for-the-disk-class"></a>Disk sınıfı için yerleşik ölçümleri

Ölçümleri Disk sınıfının disk Aygıt kullanımı hakkında bilgi sağlar. Bu istatistikler sürücünün tamamını uygulayın. Bir cihazda birden çok dosya sistemleri varsa, bu aygıt için sayaçları, etkili bir şekilde, bunların tümünün toplanan var.

Sayaç | Anlamı
------- | -------
ReadsPerSecond | Saniye başına okuma işlemleri
WritesPerSecond | Saniye başına işlem yazma
TransfersPerSecond | Saniye başına toplam işlem
AverageReadTime | Okuma işlemi başına ortalama saniye
AverageWriteTime | Yazma işlemi başına ortalama saniye
AverageTransferTime | İşlem başına ortalama saniye
AverageDiskQueueLength | Sıraya alınan disk işlemleri ortalama sayısı
ReadBytesPerSecond | Saniye başına okunan bayt sayısı
WriteBytesPerSecond | Saniye başına yazılan bayt sayısı
BytesPerSecond | Okunan veya saniye başına yazılan bayt sayısı

Tüm diskler boyunca toplanan değerler elde edilebilir ayarlayarak `"condition": "IsAggregate=True"`. Belirli bir aygıt (örneğin, / dev/sdf1) için bilgi almak için ayarlanmış `"condition": "Name=\\"/dev/sdf1\\""`.

## <a name="installing-and-configuring-lad-30-via-cli"></a>Yükleme ve LAD 3.0 CLI üzerinden yapılandırma

Korumalı ayarlarınızı PrivateConfig.json dosyasında ve genel yapılandırma bilgilerinizi PublicConfig.json varsayılarak, şu komutu çalıştırın:

```azurecli
az vm extension set *resource_group_name* *vm_name* LinuxDiagnostic Microsoft.Azure.Diagnostics '3.*' --private-config-path PrivateConfig.json --public-config-path PublicConfig.json
```

Komutu, Azure CLI Azure kaynak yönetimi modunu (arm) kullandığınızı varsayar. Klasik dağıtım için LAD yapılandırmak için (ASM) VM'ler model, "asm" moda geç (`azure config mode asm`) ve kaynak grubu adı komutta atlayın. Daha fazla bilgi için bkz: [platformlar arası CLI belgelerine](https://docs.microsoft.com/azure/xplat-cli-connect).

## <a name="an-example-lad-30-configuration"></a>Bir örnek LAD 3.0 yapılandırma

Önceki tanımları bağlı olarak, bazı açıklama ile örnek bir LAD 3.0 uzantısı yapılandırma aşağıda verilmiştir. Bu örnek çalışmanıza uygulamak için kendi depolama hesabı adı, hesap SAS belirteci ve EventHubs SAS belirteçleri kullanmanız gerekir.

### <a name="privateconfigjson"></a>PrivateConfig.json

Bu özel ayarları yapılandırın:

* bir depolama hesabı
* eşleşen hesap SAS belirteci
* birkaç havuzlarını (JsonBlob veya EventHubs SAS belirteci ile)

```json
{
  "storageAccountName": "yourdiagstgacct",
  "storageAccountSasToken": "sv=xxxx-xx-xx&ss=bt&srt=co&sp=wlacu&st=yyyy-yy-yyT21%3A22%3A00Z&se=zzzz-zz-zzT21%3A22%3A00Z&sig=fake_signature",
  "sinksConfig": {
    "sink": [
      {
        "name": "SyslogJsonBlob",
        "type": "JsonBlob"
      },
      {
        "name": "FilelogJsonBlob",
        "type": "JsonBlob"
      },
      {
        "name": "LinuxCpuJsonBlob",
        "type": "JsonBlob"
      },
      {
        "name": "MyJsonMetricsBlob",
        "type": "JsonBlob"
      },
      {
        "name": "LinuxCpuEventHub",
        "type": "EventHub",
        "sasURL": "https://youreventhubnamespace.servicebus.windows.net/youreventhubpublisher?sr=https%3a%2f%2fyoureventhubnamespace.servicebus.windows.net%2fyoureventhubpublisher%2f&sig=fake_signature&se=1808096361&skn=yourehpolicy"
      },
      {
        "name": "MyMetricEventHub",
        "type": "EventHub",
        "sasURL": "https://youreventhubnamespace.servicebus.windows.net/youreventhubpublisher?sr=https%3a%2f%2fyoureventhubnamespace.servicebus.windows.net%2fyoureventhubpublisher%2f&sig=yourehpolicy&skn=yourehpolicy"
      },
      {
        "name": "LoggingEventHub",
        "type": "EventHub",
        "sasURL": "https://youreventhubnamespace.servicebus.windows.net/youreventhubpublisher?sr=https%3a%2f%2fyoureventhubnamespace.servicebus.windows.net%2fyoureventhubpublisher%2f&sig=yourehpolicy&se=1808096361&skn=yourehpolicy"
      }
    ]
  }
}
```

### <a name="publicconfigjson"></a>PublicConfig.json

Bu genel ayarları için LAD neden:

* Yüzde işlemci zamanı ve kullanılan disk alanı ölçümlere karşıya `WADMetrics*` tablosu
* Syslog tesis "kullanıcı" ve önem derecesi "bilgisi" iletileri karşıya `LinuxSyslog*` tablosu
* Ham OMI sorgu sonuçları (PercentProcessorTime ve PercentIdleTime) adlandırılmış karşıya `LinuxCPU` tablosu
* Dosyasına eklenen satır karşıya `/var/log/myladtestlog` için `MyLadTestLog` tablosu

Her durumda için veri yüklenir:

* Azure Blob storage (kapsayıcı adı: JsonBlob havuzunda tanımlandığı gibi)
* EventHubs uç noktası (EventHubs havuzunda belirtildiği şekilde)

```json
{
  "StorageAccount": "yourdiagstgacct",
  "sampleRateInSeconds": 15,
  "ladCfg": {
    "diagnosticMonitorConfiguration": {
      "performanceCounters": {
        "sinks": "MyMetricEventHub,MyJsonMetricsBlob",
        "performanceCounterConfiguration": [
          {
            "unit": "Percent",
            "type": "builtin",
            "counter": "PercentProcessorTime",
            "counterSpecifier": "/builtin/Processor/PercentProcessorTime",
            "annotation": [
              {
                "locale": "en-us",
                "displayName": "Aggregate CPU %utilization"
              }
            ],
            "condition": "IsAggregate=TRUE",
            "class": "Processor"
          },
          {
            "unit": "Bytes",
            "type": "builtin",
            "counter": "UsedSpace",
            "counterSpecifier": "/builtin/FileSystem/UsedSpace",
            "annotation": [
              {
                "locale": "en-us",
                "displayName": "Used disk space on /"
              }
            ],
            "condition": "Name=\"/\"",
            "class": "Filesystem"
          }
        ]
      },
      "metrics": {
        "metricAggregation": [
          {
            "scheduledTransferPeriod": "PT1H"
          },
          {
            "scheduledTransferPeriod": "PT1M"
          }
        ],
        "resourceId": "/subscriptions/your_azure_subscription_id/resourceGroups/your_resource_group_name/providers/Microsoft.Compute/virtualMachines/your_vm_name"
      },
      "eventVolume": "Large",
      "syslogEvents": {
        "sinks": "SyslogJsonBlob,LoggingEventHub",
        "syslogEventConfiguration": {
          "LOG_USER": "LOG_INFO"
        }
      }
    }
  },
  "perfCfg": [
    {
      "query": "SELECT PercentProcessorTime, PercentIdleTime FROM SCX_ProcessorStatisticalInformation WHERE Name='_TOTAL'",
      "table": "LinuxCpu",
      "frequency": 60,
      "sinks": "LinuxCpuJsonBlob,LinuxCpuEventHub"
    }
  ],
  "fileLogs": [
    {
      "file": "/var/log/myladtestlog",
      "table": "MyLadTestLog",
      "sinks": "FilelogJsonBlob,LoggingEventHub"
    }
  ]
}
```

`resourceId` Yapılandırmada VM veya sanal makine ölçek kümesi aynı olmalıdır.

* Grafik ve uyarı azure platformu ölçümleri, üzerinde çalıştığınız VM ResourceId bilir. Verileri ResourceId kullanarak, VM için arama anahtarı bulmak bekliyor.
* Azure otomatik ölçeklendirme kullanırsanız, otomatik ölçeklendirme yapılandırmasında ResourceId LAD tarafından kullanılan ResourceId eşleşmesi gerekir.
* ResourceId LAD tarafından yazılan JsonBlobs adlarını içinde yerleşik olarak bulunur.

## <a name="view-your-data"></a>Verilerinizi görüntüleme

Performans verilerini görüntülemek veya uyarıları ayarlamak için Azure portalını kullanın:

![Görüntü](./media/diagnostic-extension/graph_metrics.png)

`performanceCounters` Verileri her zaman bir Azure Storage tablosunda depolanır. Azure depolama API'leri, birçok diller ve platformlar için kullanılabilir.

JsonBlob havuzlarını gönderilen veriler adlı depolama hesabındaki BLOB depolanır [ayarların korumalı](#protected-settings). Tüm Azure Blob Depolama API'leri kullanarak blob verileri kullanabilir.

Ayrıca, Azure depolama alanındaki verilere erişmek için bu UI araçları kullanabilirsiniz:

* Visual Studio Sunucu Gezgini.
* [Microsoft Azure Storage Gezgini](https://azurestorageexplorer.codeplex.com/ "Azure Storage Gezgini").

Bu oturumunun anlık görüntüsü, bir Microsoft Azure Storage Gezgini test VM üzerinde oluşturulan Azure Storage tablolarının ve doğru yapılandırılmış bir LAD 3.0 uzantısı kapsayıcılardan gösterir. Görüntü ile tam olarak eşleşmiyor [örnek LAD 3.0 yapılandırma](#an-example-lad-30-configuration).

![Görüntü](./media/diagnostic-extension/stg_explorer.png)

İlgili bkz [EventHubs belgelerine](../../event-hubs/event-hubs-what-is-event-hubs.md) EventHubs uç noktasına yayımlanan iletilerin kullanma hakkında bilgi edinmek için.

## <a name="next-steps"></a>Sonraki adımlar

* Ölçüm uyarıları oluşturma [Azure İzleyici](../../monitoring-and-diagnostics/insights-alerts-portal.md) topladığınız ölçümünün.
* Oluşturma [izleme grafikleri](../../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md) ölçümlerinizi için.
* Bilgi nasıl [bir sanal makine ölçek kümesi oluşturma](/azure/virtual-machines/linux/tutorial-create-vmss) ölçümlerinizi otomatik ölçeklendirmeyi denetlemek için kullanma.
