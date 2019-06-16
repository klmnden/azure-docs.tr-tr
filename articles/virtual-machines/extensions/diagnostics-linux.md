---
title: Azure işlem - Linux tanılama uzantısı | Microsoft Docs
description: Azure Linux tanılama uzantısı (ölçümleri toplamak ve Azure'da çalışan sanal makineleri olayları günlüğe LAD) yapılandırma
services: virtual-machines-linux
author: abhijeetgaiha
manager: sankalpsoni
ms.service: virtual-machines-linux
ms.tgt_pltfrm: vm-linux
ms.topic: article
ms.date: 12/13/2018
ms.author: agaiha
ms.openlocfilehash: e43ba83581b6ce012c619036317361a7c1c0bf4f
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64710401"
---
# <a name="use-linux-diagnostic-extension-to-monitor-metrics-and-logs"></a>Ölçüm ve günlükleri izlemek için Linux tanılama uzantısı kullanma

Bu belgede, 3.0 ve Linux tanılama uzantısı'nın daha yeni sürümü açıklanmaktadır.

> [!IMPORTANT]
> 2\.3 ve eski sürümü hakkında daha fazla bilgi için bkz: [bu belgeyi](../linux/classic/diagnostic-extension-v2.md).

## <a name="introduction"></a>Giriş

Linux tanılama uzantısı, bir kullanıcı İzleyici sistem durumunu bir Linux VM, Microsoft Azure üzerinde çalışan yardımcı olur. Bunu aşağıdaki özellikleri içerir:

* Sanal makineden sistem performans ölçümleri toplar ve bunları belirtilen depolama hesabındaki belirli bir tabloda depolar.
* Syslog günlüğü olaylarını alır ve belirtilen depolama hesabındaki belirli bir tabloda depolar.
* Toplanan ve karşıya veri ölçümlerini özelleştirme olanağı sağlar.
* Toplanan ve karşıya olayların önem düzeyleri ve syslog olanakları özelleştirmek kullanıcıların sağlar.
* Belirtilen depolama tablosu için belirtilen günlük dosyalarını karşıya yüklemek kullanıcıların sağlar.
* Rastgele EventHub uç noktaları ve JSON biçimli blob'lara belirtilen depolama hesabında ölçüm ve günlük olayları göndermeyi destekler.

Bu uzantı, her iki Azure dağıtım modeli ile çalışır.

## <a name="installing-the-extension-in-your-vm"></a>Sanal uzantısı yükleniyor

Bu uzantı, Azure PowerShell cmdlet'lerini, Azure CLI betikleri, ARM şablonları veya Azure portalı kullanarak etkinleştirebilirsiniz. Daha fazla bilgi için [uzantıları özelliklerinin](features-linux.md).

Bu yükleme yönergeleri ve [indirilebilir örnek yapılandırma](https://raw.githubusercontent.com/Azure/azure-linux-extensions/master/Diagnostic/tests/lad_2_3_compatible_portal_pub_settings.json) LAD 3.0 yapılandırın:

* yakalama ve LAD 2.3 tarafından sağlanan ölçümler aynı depolama;
* dosya sistemi ölçümleri, LAD 3.0 için yeni yararlı birtakım yakalama;
* Yakalama LAD 2.3 ile etkinleştirilmiş varsayılan syslog koleksiyonu;
* Grafik ve VM ölçümler üzerinde uyarı Azure portalı deneyiminde etkinleştirin.

İndirilebilir yapılandırma yalnızca bir örnektir; kendi gereksinimlerinize uyacak şekilde değiştirin.

### <a name="prerequisites"></a>Önkoşullar

* **Azure Linux Aracısı sürümü 2.2.0 veya üzeri**. Çoğu Azure VM Linux galeri görüntüleri sürümünü 2.2.7 içerir veya üzeri. Çalıştırma `/usr/sbin/waagent -version` sanal makinede yüklü olan sürümünü onaylamak için. VM Konuk aracısının eski bir sürümünü çalıştırıyorsa izleyin [bu yönergeleri](https://docs.microsoft.com/azure/virtual-machines/linux/update-agent) güncelleştirmek için.
* **Azure CLI**. [Azure CLI'yı ayarlama](https://docs.microsoft.com/cli/azure/install-azure-cli) makinenizde ortam.
* Zaten sahip değilseniz wget komutu: `sudo apt-get install wget` öğesini çalıştırın.
* Mevcut bir Azure aboneliği ve var olan bir depolama hesabı içindeki verileri depolamak için.
* Desteklenen Linux dağıtımları listesi açıktır https://github.com/Azure/azure-linux-extensions/tree/master/Diagnostic#supported-linux-distributions

### <a name="sample-installation"></a>Örnek yükleme

İlk üç satırını doğru parametreleri doldurun ve ardından kök olarak bu betiği yürütün:

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
my_diagnostic_storage_account_sastoken=$(az storage account generate-sas --account-name $my_diagnostic_storage_account --expiry 2037-12-31T23:59:00Z --permissions wlacu --resource-types co --services bt -o tsv)
my_lad_protected_settings="{'storageAccountName': '$my_diagnostic_storage_account', 'storageAccountSasToken': '$my_diagnostic_storage_account_sastoken'}"

# Finallly tell Azure to install and enable the extension
az vm extension set --publisher Microsoft.Azure.Diagnostics --name LinuxDiagnostic --version 3.0 --resource-group $my_resource_group --vm-name $my_linux_vm --protected-settings "${my_lad_protected_settings}" --settings portal_public_settings.json
```

Örnek Yapılandırması ve içerikleri, URL, değişikliğe tabidir. Portal ayarları JSON dosyasının bir kopyasını indirin ve gereksinimlerinize göre özelleştirin. Herhangi bir şablon veya Otomasyon oluşturmak, her zaman bu URL'yi indirmek yerine kendi kopyanızı kullanmanız gerekir.

### <a name="updating-the-extension-settings"></a>Uzantı ayarları güncelleştiriliyor

Korumalı veya genel ayarları değiştirdikten sonra bunları sanal Makineye aynı komutu çalıştırarak dağıtın. Herhangi bir şey ayarlarını değiştirdiyseniz, güncelleştirilmiş ayarlar uzantısı gönderilir. LAD yapılandırmayı yeniden yükler ve kendisini yeniden başlatır.

### <a name="migration-from-previous-versions-of-the-extension"></a>Uzantı'nın önceki sürümlerinden geçiş

Uzantının en son sürüm **3.0**. **Tüm eski sürümlerini (2.x) kullanım dışıdır ve 31 Temmuz 2018'den sonra ya da yayımdan**.

> [!IMPORTANT]
> Bu uzantı uzantısı yapılandırmanız için bozucu değişiklikler yapılmıştır. Böyle bir değişiklik, uzantı güvenliğini geliştirmek üzere yapılmıştır; Sonuç olarak, geriye dönük uyumluluk 2.x ile tutulması değil. Ayrıca, bu uzantının uzantı yayımcısı yayımcı 2.x sürümleri için farklıdır.
>
> 2\.x uzantısı'nın bu yeni sürüme geçirmek için (eski Yayımcı adı altında) eski uzantıyı kaldırın, sonra uzantıyı 3 sürümünü yükleyin.

Öneriler:

* Etkin otomatik ikincil sürüm yükseltme işlemine uzantıyı yükleyin.
  * Azure XPLAT CLI veya Powershell aracılığıyla uzantı yüklüyorsanız, Klasik dağıtım modelinde sanal makineleri '3.*' bir sürüm belirtin.
  * Sanal makineleri Azure Resource Manager dağıtım modeli, dahil ' "autoUpgradeMinorVersion": true' VM dağıtımı şablonunda.
* LAD 3.0 için yeni/farklı bir depolama hesabı kullanın. Sorunlu bir hesap Paylaşımı yapan birkaç küçük arasında uyumsuzluk LAD 2.3 ve LAD 3.0 vardır:
  * Syslog olayları LAD 3.0 farklı bir ada sahip bir tablo depolar.
  * CounterSpecifier dizeleri için `builtin` ölçümleri farklı LAD 3. 0 '.

## <a name="protected-settings"></a>Korumalı ayarları

Bu yapılandırma bilgileri kümesi genel görünümünde, örneğin, depolama kimlik bilgileri korunması gereken hassas bilgiler içerir. Bu ayarlar için gönderilen ve uzantısı şifreli biçimde depolanır.

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
storageAccountName | Veri uzantısı tarafından yazıldığı depolama hesabının adıdır.
storageAccountEndPoint | (isteğe bağlı) Depolama hesabının bulunduğu bulut tanımlayan uç noktası. Bu ayar yoksa, Azure genel bulutunda LAD varsayılanları `https://core.windows.net`. Azure Almanya, Azure kamu veya Azure Çin'de bir depolama hesabı kullanmak için bu değeri uygun şekilde ayarlayın.
storageAccountSasToken | Bir [hesap SAS belirtecini](https://azure.microsoft.com/blog/sas-update-account-sas-now-supports-all-storage-services/) Blob ve tablo hizmetlerine (`ss='bt'`), kapsayıcılar ve nesneler için geçerlidir (`srt='co'`), hangi verir ekleyin, oluşturma, liste, güncelleştirme ve yazma izinleri (`sp='acluw'`). Yapmak *değil* lider soru işareti (?) içerir.
mdsdHttpProxy | (isteğe bağlı) Belirtilen depolama hesabı ve uç nokta bağlanmak uzantıyı etkinleştirmek için gereken HTTP proxy bilgileri.
sinksConfig | (isteğe bağlı) Alternatif hedefler, ölçümleri ve olayları dağıtılabilecek ayrıntıları. Uzantı tarafından desteklenen her veri havuzu belirli Ayrıntılar aşağıdaki bölümlerde ele alınmıştır.


> [!NOTE]
> Azure dağıtım şablonu uzantısıyla dağıtırken, depolama hesabı ve SAS belirteci önceden oluşturulmalı ve sonra şablona geçirildi. VM, depolama hesabı dağıtmak ve tek bir şablonda uzantısını yapılandırın. Şablon içinde bir SAS belirteci oluşturma şu anda desteklenmiyor.

Azure Portalı aracılığıyla gerekli SAS belirteci kolayca oluşturabilirsiniz.

1. Uzantı yazmak için istediğiniz genel amaçlı depolama hesabı seçin
1. "Paylaşılan erişim imzası sol menüdeki Ayarlar bölümünden" seçin
1. Daha önce açıklandığı gibi uygun bölümleri olun
1. "SAS oluştur" düğmesine tıklayın.

![image](./media/diagnostics-linux/make_sas.png)

Oluşturulan SAS storageAccountSasToken alana kopyalayın; önde gelen soru işaretini kaldırın ("?").

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

Bu isteğe bağlı bir bölüm, uzantı topladığı bilgileri gönderdiği ek hedefler tanımlar. "Havuz" dizi, her ek veri havuzu için bir nesne içerir. "Type" özniteliği, nesne diğer özniteliklerini belirler.

Öğe | Değer
------- | -----
name | Bu havuzu genişletmesinin içindeki başka bir yerde başvurmak için kullanılan bir dize.
türü | Tanımlanan Havuz türü. Diğer değerleri, bu tür durumlarda (varsa) belirler.

Linux tanılama uzantısının 3.0 sürümü iki havuz türlerini destekler: EventHub ve JsonBlob.

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

SAS belirteci, olay veri yayımlanmasına yönelik bir merkez dahil tam URL'yi, "sasURL" giriş içerir. Talep gönderme sağlayan bir ilke adlandırma SAS LAD gerektirir. Örnek:

* Adlı bir Event Hubs ad alanı oluşturma `contosohub`
* Adlı ad alanındaki bir olay hub'ı oluşturma `syslogmsgs`
* Olay adlı Hub'ında bir paylaşılan erişim ilkesi oluşturma `writer` gönderme talep sağlayan

Bir SAS iyi gece yarısı UTC 1 Ocak 2018 tarihine kadar oluşturduysanız sasURL değeri olabilir:

```url
https://contosohub.servicebus.windows.net/syslogmsgs?sr=contosohub.servicebus.windows.net%2fsyslogmsgs&sig=xxxxxxxxxxxxxxxxxxxxxxxxx&se=1514764800&skn=writer
```

SAS belirteçleri oluşturmak için Event Hubs hakkında daha fazla bilgi için bkz. [bu web sayfası](../../event-hubs/event-hubs-authentication-and-security-model-overview.md).

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

Azure Depolama'daki blobları JsonBlob havuza yönlendirilmiş veriler depolanır. LAD her örneği, her bir havuz adı için saatte bir blob oluşturur. Her blob, her zaman nesnesinin sözdizimsel olarak geçerli bir JSON dizisi içerir. Yeni girişler için bir dizi atomik olarak eklenir. Bloblar, havuz olarak aynı ada sahip bir kapsayıcıda depolanır. JsonBlob havuzlarını adları için blob kapsayıcısı adları için Azure depolama kurallar geçerlidir: küçük harf alfasayısal ASCII karakterler veya tire 3 ile 63 arasında.

## <a name="public-settings"></a>Genel ayarları

Bu yapı, çeşitli bloklarını uzantısı tarafından toplanan bilgiler denetleyen ayarları içerir. Her bir ayar isteğe bağlıdır. Belirtirseniz `ladCfg`, de belirtmeniz gerekir `StorageAccount`.

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
Depolama hesabı | Veri uzantısı tarafından yazıldığı depolama hesabının adıdır. Belirtilen adın aynısını olmalıdır [korumalı ayarlarından](#protected-settings).
mdsdHttpProxy | (isteğe bağlı) Olarak aynı [korumalı ayarlarından](#protected-settings). Genel değer özel değere göre geçersiz kılınan ayarlayın. Yerleştirin, parola gibi bir gizli dizi içerdiğini proxy ayarlarını [korumalı ayarlarından](#protected-settings).

Kalan öğeler aşağıdaki bölümlerde ayrıntılı olarak açıklanmıştır.

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

Bu isteğe bağlı yapısı denetimleri ölçümlerini ve günlüklerini teslimat Azure ölçümleri hizmetine ve diğer veri toplamayı başlatır. Belirtmeli `performanceCounters` veya `syslogEvents` veya her ikisini de. Belirtmelisiniz `metrics` yapısı.

Öğe | Değer
------- | -----
eventVolume | (isteğe bağlı) Depolama tablosu içinde oluşturulan bölüm sayısını denetler. Biri olmalıdır `"Large"`, `"Medium"`, veya `"Small"`. Belirtilmezse, varsayılan değer: `"Medium"`.
sampleRateInSeconds | (isteğe bağlı) Ham (unaggregated) ölçümleri koleksiyonunu varsayılan zaman aralığıdır. En düşük desteklenen örnek hızı 15 saniyedir. Belirtilmezse, varsayılan değer: `15`.

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
resourceId | VM'nin ait olduğu Azure Resource Manager kaynak kimliği VM'nin veya sanal makine ölçek kümesi. Bu ayarı herhangi bir JsonBlob havuza yapılandırmada kullanılırsa da belirtilmiş.
scheduledTransferPeriod | Hesaplanan ve bir olan 8601 zaman aralığı ifade edilen Azure ölçümlerine aktarılan için toplu ölçümleri olan sıklığı. En küçük aktarım süresi 60, diğer bir deyişle, PT1M saniyedir. En az bir scheduledTransferPeriod belirtmeniz gerekir.

PerformanceCounters bölümünde belirtilen ölçüm örnekleri her 15 saniyede toplanan veya örneğine oranı açıkça sayaç için tanımlanmış. Birden çok scheduledTransferPeriod sıklığı (örnekte olduğu gibi) görünüyorsa, her bir toplama bağımsız olarak hesaplanır.

#### <a name="performancecounters"></a>PerformanceCounters

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

Bu isteğe bağlı bir bölüm ölçüm toplanmasını denetler. Ham örnekleri her biri için toplanmış [scheduledTransferPeriod](#metrics) bu değerleri oluşturmak için:

* Ortalama
* en az
* en fazla
* Son toplanan değer
* Toplama hesaplamak için kullanılan ham örneklerin sayısı

Öğe | Değer
------- | -----
havuzlar | (isteğe bağlı) Havuzlar için hangi LAD gönderdiği toplu ölçüm sonuçları adlarını virgülle ayrılmış listesi. Tüm toplanan ölçümler için listelenen her havuz yayımlanır. Bkz: [sinksConfig](#sinksconfig). Örnek: `"EHsink1, myjsonsink"`.
türü | Ölçüm gerçek sağlayıcısı tanımlar.
sınıf | Sağlayıcının ad alanındaki belirli ölçüm "sayaç" ile birlikte tanımlar.
counter | "Class" ile birlikte, belirli bir ölçüm sağlayıcının ad alanı içinde tanımlar.
counterSpecifier | Azure ölçümleri ad alanındaki belirli ölçüm tanımlar.
condition | (isteğe bağlı) Belirli bir ölçüm uygular veya toplama söz konusu nesne tüm örneklerinde seçer nesne örneğini seçer. Daha fazla bilgi için `builtin` ölçüm tanımları.
sampleRate | Bu ölçüm için ham örnekleri toplanan oranı ayarlayan 8601 ARALIĞIDIR. Ayarlı değil, toplama aralığı değeri olarak ayarlanıp ayarlanmadığını [sampleRateInSeconds](#ladcfg). Kısa desteklenen Örnek 15 saniye (PT15S) oranıdır.
Birim | Bu dizelerin biri olmalıdır: "Say", "Bayt", "Saniye", "Yüzde", "CountPerSecond", "BytesPerSecond", "Milisaniyelik". Ölçüm için birimi tanımlar. Toplanan veri tüketicileri bu birimi eşleştirmek için toplanan verileri değerleri bekler. Bu alan LAD yoksayar.
displayName | Etiket (ilişkili yerel ayar tarafından belirtilen dilde) bu verileri Azure ölçümleri eklenecek. Bu alan LAD yoksayar.

CounterSpecifier rastgele bir tanımlayıcıdır. Ölçüm, Tüketicileri, Azure portal grafik ister ve özelliği, uyarı counterSpecifier "bir ölçüm veya bir ölçüm örneğini tanımlayan anahtar" kullanın. İçin `builtin` ölçümleri, kullanmanızı öneririz, ile başlayan counterSpecifier değerler `/builtin/`. Size bir ölçüm belirli bir örneğini kullanıyorsanız, counterSpecifier değerine örneğinin tanımlayıcısı ekleme öneririz. Bazı örnekler:

* `/builtin/Processor/PercentIdleTime` -Tüm Vcpu ortalaması alınan boşta kalma süresi
* `/builtin/Disk/FreeSpace(/mnt)` -Boş alan /mnt dosya sistemi
* `/builtin/Disk/FreeSpace` -Tüm bağlı dosya sistemleri ortalaması alınan boş alan

LAD ya da Azure portalında herhangi bir desenle eşleşen counterSpecifier değeri bekliyor. CounterSpecifier değerleri oluşturmada nasıl içinde olabilir.

Belirttiğinizde `performanceCounters`, LAD her zaman Yazar verileri Azure depolamada bir tablo. JSON bloblarını ve/veya olay hub'ları için yazılmış aynı verilere sahip olabilir, ancak bir tablo için verilerin depolanması devre dışı bırakılamıyor. Tanılama uzantısı'nın tüm örnekleri aynı depolama hesabı adı kullanmak için yapılandırılmış ve uç noktası aynı tabloya, ölçüm ve günlükleri ekleyin. Azure, çok fazla Vm'leri aynı tablo bölüme yazıyorsanız, yazma daraltabilir bölümü için. EventVolume ayarı 1 (küçük), 10 (Orta) üzerinden yayılan veya 100 (büyük) farklı bölüm olmasını girişleri neden olur. Genellikle, "Orta" trafiği olmayan kısıtlanan emin olmak yeterli olur. Azure portal'ın Azure ölçümleri özelliği grafikleri oluşturmak için veya uyarıları tetiklemek için bu tablodaki verileri kullanır. Bu dize bitiştirme tablo adıdır:

* `WADMetrics`
* Tabloda depolanan toplanan değerler için "scheduledTransferPeriod"
* `P10DV2S`
* Her 10 gün değişiklikleri "YYYYMMDD" biçiminde bir tarih

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

Bu isteğe bağlı bir bölüm syslog olayları günlük toplamayı denetler. Syslog olayları bölümü atlanırsa, tüm yakalanmaz.

SyslogEventConfiguration koleksiyonda her syslog özelliğini ilgi için bir giriş var. MinSeverity "NONE" belirli bir özellik için ise veya bu özelliği öğesinde hiç görünmüyorsa, olay, tesisten yakalanır.

Öğe | Değer
------- | -----
havuzlar | Tek tek günlüğü olaylarını yayımlanan havuzlarını adlarını virgülle ayrılmış listesi. Tüm günlük olaylar syslogEventConfiguration kısıtlamalarına eşleşen her listelenen havuz için yayımlanır. Örnek: "EHforsyslog"
facilityName | Syslog özellik adı (örneğin "günlük\_kullanıcı" veya "günlük\_LOCAL0"). "Özelliği" bölümüne bakın [syslog man sayfa](http://man7.org/linux/man-pages/man3/syslog.3.html) tam listesi için.
minSeverity | Syslog önem derecesi (gibi "günlük\_hata" veya "günlük\_bilgileri"). "Level" bölümüne bakın [syslog man sayfa](http://man7.org/linux/man-pages/man3/syslog.3.html) tam listesi için. Uzantı belirtilen düzeyi veya üzerindeki tesis gönderilen olayları yakalar.

Belirttiğinizde `syslogEvents`, LAD her zaman Yazar verileri Azure depolamada bir tablo. JSON bloblarını ve/veya olay hub'ları için yazılmış aynı verilere sahip olabilir, ancak bir tablo için verilerin depolanması devre dışı bırakılamıyor. Bu tablo bölümleme davranışı için tanımlanan aynı mıdır `performanceCounters`. Bu dize bitiştirme tablo adıdır:

* `LinuxSyslog`
* Her 10 gün değişiklikleri "YYYYMMDD" biçiminde bir tarih

Örnekler `LinuxSyslog20170410` ve `LinuxSyslog20170609`.

### <a name="perfcfg"></a>perfCfg

Bu isteğe bağlı bir bölüm rastgele yürütülmesini denetimleri [OMI](https://github.com/Microsoft/omi) sorgular.

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
ad alanı | (isteğe bağlı) İçinde sorgunun yürütülmesi gereken OMI ad alanı. Belirtilmemişse, varsayılan değer "kök/tarafından uygulanan scx",: [System Center platformlar arası sağlayıcıları](https://scx.codeplex.com/wikipage?title=xplatproviders&referringTitle=Documentation).
sorgu | Yürütülecek OMI sorgu.
table | (isteğe bağlı) Belirtilen depolama hesabında bir Azure depolama tablosu (bkz [korumalı ayarlarından](#protected-settings)).
frequency | (isteğe bağlı) Sorgu yürütme arasındaki saniye sayısı. 300 (5 dakika); varsayılan değer: en düşük değer 15 saniyedir.
havuzlar | (isteğe bağlı) Ham örnek ölçüm sonuçlarını yayımlanmasına ek havuzlarını adlarının virgülle ayrılmış listesi. Bu ham örnekleri toplama yoktur, Azure ölçümleri veya uzantısı tarafından hesaplanır.

"Tablo" veya "havuzlarını" veya her ikisi de belirtilmelidir.

### <a name="filelogs"></a>fileLogs

Günlük dosyalarının yakalama denetler. LAD dosyaya yazılırken yeni metin satırlarını yakalar ve tablo satırları ve/veya hiçbir belirtilen havuzlarını (JsonBlob veya EventHub) yazar.

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
file | İzlenen ve yakalanan günlük dosyasının tam yol adı. Yol, tek bir dosya adı olmalıdır; bir dizin adı veya joker karakterlerini içermelidir.
table | (isteğe bağlı) Belirtilen depolama hesabında içine dosya "kuyruğunu" Yeni satırlardan yazılır (belirtildiği gibi korumalı yapılandırma), Azure depolama tablosu.
havuzlar | (isteğe bağlı) Gönderilen günlük satırları için ek havuzlarını adlarının virgülle ayrılmış listesi.

"Tablo" veya "havuzlarını" veya her ikisi de belirtilmelidir.

## <a name="metrics-supported-by-the-builtin-provider"></a>Yerleşik sağlayıcı tarafından desteklenen ölçümler

Yerleşik ölçüm sağlayıcısı bir ölçüm en çok sayıda kullanıcı için ilginç kaynağıdır. Bu ölçümler beş geniş sınıflara ayrılır:

* İşlemci
* Bellek
* Ağ
* dosya sistemi
* Disk

### <a name="builtin-metrics-for-the-processor-class"></a>Yerleşik ölçümleri işlemci sınıfı

Ölçüm işlemci sınıf VM işlemci kullanımı hakkında bilgi sağlar. Yüzde toplanırken ortalama tüm CPU'lar arasında oluşur. İki vCPU VM ile bir vCPU % 100 meşgul olduğu ve diğer %100 boşta olduğu bildirilen PercentIdleTime 50 olacaktır. Her vCPU %50 aynı dönem için meşgul olursa, bildirilen sonucu ayrıca 50 olur. Bir vCPU %100 meşgul ve diğerleri boş bir dört vCPU VM içinde bildirilen PercentIdleTime 75 olacaktır.

counter | Anlamı
------- | -------
PercentIdleTime | İşlemci çekirdeği boşta döngü yürütme toplama penceresi sırasında sürenin yüzdesi
percentProcessorTime | Boş olmayan bir iş parçacığı yürütme süresi yüzdesi
PercentIOWaitTime | G/ç işlemleri için beklerken süre yüzdesi
PercentInterruptTime | Donanım/yazılım kesmeler ve DPC'ler oluşturarak (ertelenmiş yordam çağrılarını) yürütme süresi yüzdesi
PercentUserTime | Boş olmayan toplama penceresi sırasında süresi yüzdesini normal öncelikli daha fazla bilgi için kullanıcı için harcanan süre
PercentNiceTime | Boş olmayan'ın, yüzde azaltılmış (iyi) öncelik için harcanan süre
PercentPrivilegedTime | Boş olmayan süresini yüzde ayrıcalıklı (çekirdek) modunda harcanan

% 100'e ilk dört sayaçları toplamak. Son üç sayaçları da toplam % 100; Bunlar PercentProcessorTime PercentIOWaitTime ve PercentInterruptTime toplamına alt bölümlere ayırır.

Tüm işlemciler arasında toplanmış tek bir ölçüm elde etmek için ayarlama `"condition": "IsAggregate=TRUE"`. Dört vCPU VM, ikinci bir mantıksal işlemci gibi belirli bir işlemci için bir ölçüm elde etmek için ayarlama `"condition": "Name=\\"1\\""`. Mantıksal işlemci sayılardır aralığında `[0..n-1]`.

### <a name="builtin-metrics-for-the-memory-class"></a>Yerleşik ölçümleri bellek sınıfı

Bellek sınıf ölçüm, disk belleği ve değiştirmeyi bellek kullanımı hakkında bilgi sağlar.

counter | Anlamı
------- | -------
AvailableMemory | MIB'deki kullanılabilir fiziksel bellek
PercentAvailableMemory | Toplam belleğin yüzdesi olarak kullanılabilir fiziksel bellek
UsedMemory | Kullanımdaki fiziksel bellek (MIB)
PercentUsedMemory | Kullanımdaki toplam belleğin yüzdesi olarak fiziksel bellek
PagesPerSec | Toplam disk belleği (okuma/yazma)
PagesReadPerSec | Yedekleme deposu (takas dosyası, program dosyası, eşleşen dosya, vb.) öğesinden sayfaları oku
PagesWrittenPerSec | Yedekleme deposu (takas dosyası, eşleşen dosya, vb.) yazılan sayfa
AvailableSwap | Kullanılmayan takas alanı (MIB)
PercentAvailableSwap | Kullanılmayan değiştirme alanının toplam değiştirme yüzdesi
UsedSwap | Kullanımdaki takas alanı (MIB)
PercentUsedSwap | Değiştirme alanının toplam değiştirme yüzdesi olarak kullanımda

Ölçümler, bu sınıfın yalnızca tek bir örneği vardır. "Koşul" özniteliği yararlı ayarları yok ve atlanmış olabilir.

### <a name="builtin-metrics-for-the-network-class"></a>Ağ sınıf için yerleşik ölçümleri

Ağ sınıf ölçüm, önyüklemeden bir tek tek ağ arabirimleri üzerinde ağ etkinliğiyle ilgili bilgi sağlar. LAD konak ölçümleri alınabilir bant genişliği ölçümler, kullanıma sunmuyor.

counter | Anlamı
------- | -------
BytesTransmitted | Önyüklemeden gönderilen toplam bayt sayısı
BytesReceived | Önyüklemeden alınan toplam bayt sayısı
BytesTotal | Gönderilen veya alınan önyüklemeden toplam bayt sayısı
PacketsTransmitted | Önyüklemeden gönderilen toplam paket sayısı
PacketsReceived | Önyüklemeden alınan toplam paket sayısı
TotalRxErrors | Hataları önyüklemeden alma sayısı
TotalTxErrors | Sayısı önyüklemeden iletme işlemi hataları
TotalCollisions | Önyüklemeden ağ bağlantı noktaları tarafından bildirilen çakışmaların sayısı

 Bu sınıf örnekli olsa da, tüm ağ aygıtları arasında toplu yakalama ağ ölçümleri LAD desteklemez. Eth0 gibi belirli bir arabirim için ölçümleri elde etmek için ayarlama `"condition": "InstanceID=\\"eth0\\""`.

### <a name="builtin-metrics-for-the-filesystem-class"></a>dosya sistemi sınıfı için yerleşik ölçümleri

Dosya sistemi sınıf ölçüm dosya sistemi kullanımı hakkında bilgi sağlar. Mutlak ve yüzde değerleri, normal bir kullanıcı (kök değil) görüntülenmekteydi olarak bildirilir.

counter | Anlamı
------- | -------
FreeSpace | Bayt cinsinden kullanılabilir disk alanı
UsedSpace | Kullanılan disk alanı bayt
PercentFreeSpace | Boş alan yüzdesi
PercentUsedSpace | Kullanılan yüzde alanı
PercentFreeInodes | Kullanılmayan inode yüzdesi
PercentUsedInodes | Tüm dosya sistemleri arasında toplamı (kullanımda) ayrılmış inode yüzdesi
BytesReadPerSecond | Saniye başına okunan bayt
BytesWrittenPerSecond | Saniye başına yazılan bayt sayısı
BytesPerSecond | Okunan veya saniye başına yazılan bayt
ReadsPerSecond | Saniye başına okuma işlemleri
WritesPerSecond | Yazma işlemi / saniye
TransfersPerSecond | Saniye başına okuma veya yazma işlemleri

Tüm dosya sistemleri arasında toplanmış değerler elde edilebilir ayarlayarak `"condition": "IsAggregate=True"`. Gibi belirli bir bağlı dosya sistemi için değer "/ mnt", ayarlayarak alınabilir `"condition": 'Name="/mnt"'`. 

**NOT**: Azure portalı yerine JSON kullanıyorsanız, doğru bir koşulu alan formun addır ='/ mnt'

### <a name="builtin-metrics-for-the-disk-class"></a>Disk sınıfı için yerleşik ölçümleri

Disk sınıf ölçüm, disk cihaz kullanımı hakkında bilgi sağlar. Bu istatistikler sürücünün tamamını uygulayın. Bir cihazda birden çok dosya sistemleri varsa, bu cihaz için etkili bir şekilde, tüm bunların arasında toplanmış sayaçlarıdır.

counter | Anlamı
------- | -------
ReadsPerSecond | Saniye başına okuma işlemleri
WritesPerSecond | Yazma işlemi / saniye
TransfersPerSecond | Saniye başına toplam işlem
AverageReadTime | Okuma işlemi başına ortalama saniye
AverageWriteTime | Yazma işlemi başına ortalama saniye
AverageTransferTime | İşlem başına ortalama saniye
AverageDiskQueueLength | Kuyruğa alınmış disk işlemleri ortalama sayısı
ReadBytesPerSecond | Saniye başına okunan bayt sayısı
WriteBytesPerSecond | Saniye başına yazılan bayt sayısı
BytesPerSecond | Okunan veya saniye başına yazılan bayt sayısı

Tüm disklerde bulunan toplam değerler elde edilebilir ayarlayarak `"condition": "IsAggregate=True"`. Belirli bir cihazda (örneğin, / dev/sdf1) daha fazla bilgi almak üzere `"condition": "Name=\\"/dev/sdf1\\""`.

## <a name="installing-and-configuring-lad-30-via-cli"></a>Yükleme ve LAD 3.0 CLI aracılığıyla yapılandırma

Korumalı ayarlarınızın PrivateConfig.json dosyasında ve genel yapılandırma bilgilerinizi PublicConfig.json içinde olduğu varsayıldığında, şu komutu çalıştırın:

```azurecli
az vm extension set *resource_group_name* *vm_name* LinuxDiagnostic Microsoft.Azure.Diagnostics '3.*' --private-config-path PrivateConfig.json --public-config-path PublicConfig.json
```

Komutu, Azure CLI'ın (arm) Azure kaynak yönetimi modunda kullandığınızı varsayar. Klasik dağıtım için LAD yapılandırmak için Vm'leri (ASM) modeli, "asm" moduna geçin (`azure config mode asm`) ve kaynak grubu adı komutta çıkarın. Daha fazla bilgi için [platformlar arası CLI belgeleri](https://docs.microsoft.com/azure/xplat-cli-connect).

## <a name="an-example-lad-30-configuration"></a>Bir örnek LAD 3.0 yapılandırma

Önceki tanımlarını temel alarak, bazı açıklamalar ile örnek LAD 3.0 uzantısı yapılandırması aşağıda verilmiştir. Bu örnek durumunuz için uygulamak için kendi depolama hesabı adı, hesap SAS belirtecini ve EventHubs SAS belirteçleri kullanmanız gerekir.

### <a name="privateconfigjson"></a>PrivateConfig.json

Bu özel ayarları yapılandırın:

* Bir depolama hesabı
* eşleşen hesap SAS belirteci
* birden çok havuz (JsonBlob veya SAS belirteçleri ile EventHubs)

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

Bu genel ayarlar için LAD neden:

* Yüzde işlemci zamanı ve kullanılan disk alanı ölçümleri için karşıya yükleme `WADMetrics*` tablo
* Syslog tesis "kullanıcı" ve önem derecesi "bilgisi" iletileri yükleme `LinuxSyslog*` tablo
* Ham OMI sorgu sonuçları (PercentProcessorTime ve PercentIdleTime) adlandırılmış karşıya `LinuxCPU` tablo
* Eklenen satırları dosya karşıya yükleme `/var/log/myladtestlog` için `MyLadTestLog` tablo

Her durumda için veri yüklenir:

* Azure Blob Depolama (kapsayıcı adı: JsonBlob havuzunda tanımlandığı şekilde)
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

`resourceId` Yapılandırmasında sanal makine veya sanal makine ölçek kümesi ile eşleşmelidir.

* Grafik ve uyarı azure platformu ölçümler üzerinde çalıştığınız sanal makinenin ResourceId bilir. Arama anahtarı sanal makinenizin ResourceId kullanarak verileri bulmak bekliyor.
* Azure otomatik ölçeklendirme kullanırsanız, otomatik ölçeklendirme yapılandırması ResourceId LAD tarafından kullanılan ResourceId eşleşmesi gerekir.
* ResourceId LAD tarafından yazılan JsonBlobs adlarını yerleşik olarak bulunur.

## <a name="view-your-data"></a>Verilerinizi görüntüleyin

Performans verilerini görüntülemek veya uyarıları ayarlamak için Azure portalını kullanın:

![image](./media/diagnostics-linux/graph_metrics.png)

`performanceCounters` Veriler her zaman bir Azure depolama tablosunda depolanır. Azure depolama API'leri, birçok diller ve platformlar için kullanılabilir.

JsonBlob havuzlarından gönderilen verileri, adlı depolama hesabındaki BLOB'ları depolanır [korumalı ayarlarından](#protected-settings). Herhangi bir Azure Blob Depolama API'lerini kullanarak blob verileri kullanabilir.

Ayrıca, Azure Depolama'daki verilere erişmek için bu kullanıcı Arabirimi araçları kullanabilirsiniz:

* Visual Studio Sunucu Gezgini.
* [Microsoft Azure Depolama Gezgini](https://azurestorageexplorer.codeplex.com/ "Azure Depolama Gezgini").

Bu anlık görüntü bir Microsoft Azure Depolama Gezgini oturumu test VM üzerinde oluşturulan Azure depolama tabloları ve doğru şekilde yapılandırılmış bir LAD 3.0 uzantısı kapsayıcılardan gösterir. Görüntü ile tam olarak eşleşmiyor [örnek LAD 3.0 yapılandırma](#an-example-lad-30-configuration).

![image](./media/diagnostics-linux/stg_explorer.png)

İlgili bkz [EventHubs belgeleri](../../event-hubs/event-hubs-what-is-event-hubs.md) bir EventHubs uç noktasına yayımlanan iletilerin kullanma hakkında bilgi edinmek için.

## <a name="next-steps"></a>Sonraki adımlar

* Ölçüm uyarıları oluşturma [Azure İzleyici](../../monitoring-and-diagnostics/insights-alerts-portal.md) topladığınız ölçümler için.
* Oluşturma [izleme grafikleri](../../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md) ölçümlerinizi için.
* Bilgi nasıl [bir sanal makine ölçek kümesi oluşturma](../linux/tutorial-create-vmss.md) ölçümlerinizi otomatik ölçeklendirme denetlemek için kullanma.
