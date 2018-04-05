---
title: Azure sanal makine ölçek ayarlar ile ilgili SSS | Microsoft Docs
description: Sanal makine ölçek kümeleri hakkında sık sorulan soruların yanıtlarını alın.
services: virtual-machine-scale-sets
documentationcenter: ''
author: gatneil
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 76ac7fd7-2e05-4762-88ca-3b499e87906e
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/12/2017
ms.author: negat
ms.custom: na
ms.openlocfilehash: e7fc12c9b4cc79109975e34f64f236394c33af25
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2018
---
# <a name="azure-virtual-machine-scale-sets-faqs"></a>Azure sanal makine ölçek SSS ayarlar

Azure'da sanal makine ölçek kümeleri hakkında sık sorulan soruların yanıtlarını alın.

## <a name="top-frequently-asked-questions-for-scale-sets"></a>Üst ölçek kümeleri için sık sorulan sorular
**S.** Bir ölçek kümesinde kaç tane sanal makinem olabilir?

**C.** Ölçek kümeleri, platform görüntülerini temel alan 0 ila 1.000 VM’ye veya özel görüntüleri temel alan 0 ila 300 VM’ye sahip olabilir. 

**S.** Ölçek kümelerinde veri diskleri destekleniyor mu?

**C.** Evet. Bir ölçek kümesi, kümedeki tüm sanal makineler için geçerli olan bağlı veri diski yapılandırmasını tanımlayabilir. Daha fazla bilgi için bkz. [Azure ölçek kümeleri ve bağlı veri diskleri](virtual-machine-scale-sets-attached-disks.md). Veri depolamayla ilgili diğer seçenekler şunlardır:

* Azure dosyaları (paylaşılan SMB sürücüleri)
* İşletim sistemi sürücüsü
* Geçici sürücü (yerel, Azure Depolama tarafından yedeklenmez)
* Azure veri hizmeti (örneğin Azure tabloları, Azure blobları)
* Dış veri hizmeti (örneğin, uzak veritabanı)

**S.** Hangi Azure bölgeleri ölçek kümelerini destekler?

**C.** Tüm bölgeler ölçek kümelerini destekler.

**S.** Özel bir görüntü kullanarak nasıl ölçek kümesi oluşturabilirim?

**C.** Özel görüntü VHD’nizi temel alan bir yönetilen disk oluşturun ve ölçek kümesi şablonunuzda başvurun. [Bir örneği aşağıda verilmiştir](https://github.com/chagarw/MDPP/tree/master/101-vmss-custom-os).

**S.** Ölçek kümemin kapasitesini 20’den 15’e düşürürsem hangi VM’ler kaldırılır?

**C.** Sanal makineler, ölçek kümesinden güncelleştirme etki alanları ve hata etki alanları arasında eşit olacak şekilde kaldırılır. En yüksek kimlik numarasına sahip VM’ler ilk önce kaldırılır.

**S.** Kapasiteyi 15’ten 18’e yükseltirsem ne olur?

**C.** Kapasiteyi 18’e artırırsanız 3 yeni VM oluşturulur. Her defasında VM örnek kimliği önceki en yüksek değerden artırılır (örneğin 20, 21, 22). VM’ler hata etki alanlarında ve güncelleştirme etki alanlarında dengelenir.

**S.** Bir ölçek kümesinde birden fazla uzantı kullanırken bir yürütme sırası uygulamayı zorunlu kılabilir miyim?

**C.** Doğrudan olmasa da customScript uzantısı için betiğiniz başka bir uzantının tamamlanmasını bekleyebilir. Uzantı sıralama hakkında ek yönergeleri şu blog gönderisinde bulabilirsiniz: [Azure sanal makine ölçek kümelerinde Uzantı Sıralama](https://msftstack.wordpress.com/2016/05/12/extension-sequencing-in-azure-vm-scale-sets/).

**S.** Ölçek kümeleri Azure kullanılabilirlik kümeleri ile birlikte çalışır mı?

**C.** Kullanan bir bölgesel (zonal olmayan) ölçek kümesi *yerleştirme grupları*, her biri örtük kullanılabilirlik beş hata etki alanları ile kümesi olarak davranmak için yapılandırılabilir ve beş güncelleme etki alanları. 100'den fazla VM ölçek kümesi birden çok yerleştirme grupları span. Yerleştirme grupları hakkında daha fazla bilgi için bkz. [Büyük sanal makine ölçek kümeleri ile çalışma](virtual-machine-scale-sets-placement-groups.md). Bir sanal makine kullanılabilirlik kümesi, sanal makine ölçek kümesiyle aynı sanal ağda bulunabilir. Genellikle bir kullanılabilirlik kümesinde benzersiz yapılandırma gerektiren denetim düğümünü sanal makinelere, veri düğümlerini ise ölçek kümesine yerleştirmek, yaygın bir yapılandırmadır.

**S.** Ayarlar çalışma Azure kullanılabilirlik bölgeleri ile ölçeklendirme?

**C.** Evet! Daha fazla bilgi için bkz: [ölçek kümesi bölge belge](./virtual-machine-scale-sets-use-availability-zones.md).


## <a name="autoscale"></a>Otomatik Ölçeklendirme

### <a name="what-are-best-practices-for-azure-autoscale"></a>Azure otomatik ölçeklendirme için en iyi uygulamalar nelerdir?

Otomatik ölçeklendirme için en iyi yöntemler için bkz: [otomatik ölçeklendirmeyi sanal makineler için en iyi uygulamaları](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-autoscale-best-practices).

### <a name="where-do-i-find-metric-names-for-autoscaling-that-uses-host-based-metrics"></a>Ana bilgisayar tabanlı ölçümleri kullanan otomatik ölçeklendirme ölçüm adlarını nereden bulabilirim?

Ana bilgisayar tabanlı ölçümleri kullanan otomatik ölçeklendirmeyi için ölçüm adları için bkz: [desteklenen Azure İzleyicisi ile ölçümleri](https://azure.microsoft.com/documentation/articles/monitoring-supported-metrics/).

### <a name="are-there-any-examples-of-autoscaling-based-on-an-azure-service-bus-topic-and-queue-length"></a>Herhangi bir Azure Service Bus konu ve sıra uzunluğuna göre otomatik ölçeklendirmeyi örnekleri bulunur?

Evet. Bir Azure Service Bus konu ve sıra uzunluğuna göre otomatik ölçeklendirmeyi örnekleri için bkz: [Azure İzleyici otomatik ölçeklendirmeyi ortak ölçümleri](https://azure.microsoft.com/documentation/articles/insights-autoscale-common-metrics/).

Service Bus kuyruğuna için aşağıdaki JSON kullanın:

```json
"metricName": "MessageCount",
"metricNamespace": "",
"metricResourceUri": "/subscriptions/s1/resourceGroups/rg1/providers/Microsoft.ServiceBus/namespaces/mySB/queues/myqueue"
```

Bir depolama kuyruğu için aşağıdaki JSON kullanın:

```json
"metricName": "ApproximateMessageCount",
"metricNamespace": "",
"metricResourceUri": "/subscriptions/s1/resourceGroups/rg1/providers/Microsoft.ClassicStorage/storageAccounts/mystorage/services/queue/queues/mystoragequeue"
```

Örnek değerler kaynak Tekdüzen Kaynak Tanımlayıcıları (URI'ler) ile değiştirin.


### <a name="should-i-autoscale-by-using-host-based-metrics-or-a-diagnostics-extension"></a>Mıyım otomatik ölçeklendirme ana bilgisayar tabanlı ölçümleri veya tanılama uzantısını kullanarak?

Ana bilgisayar düzeyindeki ölçümlerini veya konuk işletim sistemi tabanlı ölçümleri kullanmak için bir VM üzerinde bir otomatik ölçeklendirme ayarı oluşturabilirsiniz.

Desteklenen ölçümleri listesi için bkz: [Azure İzleyici otomatik ölçeklendirmeyi ortak ölçümleri](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-autoscale-common-metrics). 

Sanal makine ölçek kümeleri için tam bir örnek için bkz: [sanal makine ölçek kümeleri için Resource Manager şablonları kullanarak gelişmiş otomatik ölçeklendirme yapılandırmasını](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-advanced-autoscale-virtual-machine-scale-sets). 

Örnek, ana bilgisayar düzeyinde CPU ölçüm ve bir ileti sayısı ölçümü kullanır.



### <a name="how-do-i-set-alert-rules-on-a-virtual-machine-scale-set"></a>Uyarı kuralları bir sanal makine ölçek kümesinde nasıl ayarlarım?

PowerShell veya Azure CLI aracılığıyla sanal makine ölçek kümeleri için ölçümler üzerinde uyarılar oluşturabilirsiniz. Daha fazla bilgi için bkz: [Azure İzleyici PowerShell hızlı başlangıç örnekleri](https://azure.microsoft.com/documentation/articles/insights-powershell-samples/#create-alert-rules) ve [Azure İzleyici platformlar arası CLI hızlı başlangıç örnekleri](https://azure.microsoft.com/documentation/articles/insights-cli-samples/#work-with-alerts).

Sanal makine ölçek kümesinin uç noktası Targetresourceıd şöyle görünür: 

/subscriptions/yoursubscriptionid/resourceGroups/yourresourcegroup/providers/Microsoft.Compute/virtualMachineScaleSets/yourvmssname

Tüm VM performans sayacı için uyarı ayarlamak için ölçüm olarak seçebilirsiniz. Daha fazla bilgi için bkz: [Resource Manager tabanlı Windows VM'ler için konuk işletim sistemi ölçümleri](https://azure.microsoft.com/documentation/articles/insights-autoscale-common-metrics/#guest-os-metrics-resource-manager-based-windows-vms) ve [Linux VM'ler için konuk işletim sistemi ölçümleri](https://azure.microsoft.com/documentation/articles/insights-autoscale-common-metrics/#guest-os-metrics-linux-vms) içinde [Azure İzleyici otomatik ölçeklendirmeyi ortak ölçümleri](https://azure.microsoft.com/documentation/articles/insights-autoscale-common-metrics/) makale.

### <a name="how-do-i-set-up-autoscale-on-a-virtual-machine-scale-set-by-using-powershell"></a>PowerShell kullanarak bir sanal makine ölçek göre otomatik ölçeklendirme nasıl ayarlayabilirim?

PowerShell kullanarak bir sanal makine ölçek göre otomatik ölçeklendirme ayarlamak için blog gönderisine bakın [bir Azure sanal makine ölçek kümesi için otomatik ölçeklendirme ekleme](https://msftstack.wordpress.com/2017/03/05/how-to-add-autoscale-to-an-azure-vm-scale-set/).




## <a name="certificates"></a>Sertifikalar

### <a name="how-do-i-securely-ship-a-certificate-to-the-vm-how-do-i-provision-a-virtual-machine-scale-set-to-run-a-website-where-the-ssl-for-the-website-is-shipped-securely-from-a-certificate-configuration-the-common-certificate-rotation-operation-would-be-almost-the-same-as-a-configuration-update-operation-do-you-have-an-example-of-how-to-do-this"></a>Nasıl bir sertifika VM güvenli bir şekilde sevk? Web sitesi için SSL sevk yerde bir Web sitesi sertifikası yapılandırmasından güvenli bir şekilde çalışacak şekilde ayarlanmış bir sanal makine ölçek nasıl sağlama? (Ortak sertifika döndürme işlemi neredeyse aynı yapılandırma güncelleştirme işlemi olur.) Bunun nasıl yapılacağı örneği var mı? 

Güvenli bir şekilde VM için bir sertifika sevk etmek için Müşteri'nin anahtar kasası Windows sertifika deposundan doğrudan bir müşteri sertifika yükleyebilirsiniz.

Aşağıdaki JSON kullanın:

```json
"secrets": [
    {
        "sourceVault": {
            "id": "/subscriptions/{subscriptionid}/resourceGroups/myrg1/providers/Microsoft.KeyVault/vaults/mykeyvault1"
        },
        "vaultCertificates": [
            {
                "certificateUrl": "https://mykeyvault1.vault.azure.net/secrets/{secretname}/{secret-version}",
                "certificateStore": "certificateStoreName"
            }
        ]
    }
]
```

Kod, Windows ve Linux destekler.

Daha fazla bilgi için bkz: [oluştur veya güncelleştir bir sanal makine ölçek kümesi](https://msdn.microsoft.com/library/mt589035.aspx).


### <a name="example-of-self-signed-certificate"></a>Otomatik olarak imzalanan sertifika örneği

1.  Kendinden imzalı bir sertifika bir anahtar kasası oluşturun.

    Aşağıdaki PowerShell komutlarını kullanın:

    ```powershell
    Import-Module "C:\Users\mikhegn\Downloads\Service-Fabric-master\Scripts\ServiceFabricRPHelpers\ServiceFabricRPHelpers.psm1"

    Login-AzureRmAccount

    Invoke-AddCertToKeyVault -SubscriptionId <Your SubID> -ResourceGroupName KeyVault -Location westus -VaultName MikhegnVault -CertificateName VMSSCert -Password VmssCert -CreateSelfSignedCertificate -DnsName vmss.mikhegn.azure.com -OutputPath c:\users\mikhegn\desktop\
    ```

    Bu komut için Azure Resource Manager şablonu giriş sunar.

    Kendinden imzalı bir sertifika bir anahtar kasası oluşturma örneği için bkz: [Service Fabric kümesi güvenlik senaryoları](https://azure.microsoft.com/documentation/articles/service-fabric-cluster-security/).

2.  Resource Manager şablonu değiştirin.

    Bu özelliğe eklemek **virtualMachineProfile**, sanal makine bir parçası olarak ölçek kümesi kaynak:

    ```json 
    "osProfile": {
        "computerNamePrefix": "[variables('namingInfix')]",
        "adminUsername": "[parameters('adminUsername')]",
        "adminPassword": "[parameters('adminPassword')]",
        "secrets": [
            {
                "sourceVault": {
                    "id": "[resourceId('KeyVault', 'Microsoft.KeyVault/vaults', 'MikhegnVault')]"
                },
                "vaultCertificates": [
                    {
                        "certificateUrl": "https://mikhegnvault.vault.azure.net:443/secrets/VMSSCert/20709ca8faee4abb84bc6f4611b088a4",
                        "certificateStore": "My"
                    }
                ]
            }
        ]
    }
    ```
  

### <a name="can-i-specify-an-ssh-key-pair-to-use-for-ssh-authentication-with-a-linux-virtual-machine-scale-set-from-a-resource-manager-template"></a>Resource Manager şablonu ayarlamak Linux sanal makine ölçek ile SSH kimlik doğrulaması için kullanılacak bir SSH anahtar çifti belirtebilir miyim?  

Evet. İçin REST API **osProfile** standart VM REST API için benzer. 

Dahil **osProfile** şablonunuzda:

```json 
"osProfile": {
    "computerName": "[variables('vmName')]",
    "adminUsername": "[parameters('adminUserName')]",
    "linuxConfiguration": {
        "disablePasswordAuthentication": "true",
        "ssh": {
            "publicKeys": [
                {
                    "path": "[variables('sshKeyPath')]",
                    "keyData": "[parameters('sshKeyData')]"
                }
            ]
        }
    }
}
```
 
Bu JSON bloğu kullanılan [101 vm sshkey GitHub hızlı başlatma şablonunu](https://github.com/Azure/azure-quickstart-templates/blob/master/101-vm-sshkey/azuredeploy.json).
 
İşletim sistemi profili de kullanılan [grelayhost.json GitHub hızlı başlatma şablonunu](https://github.com/ExchMaster/gadgetron/blob/master/Gadgetron/Templates/grelayhost.json).

Daha fazla bilgi için bkz: [oluştur veya güncelleştir bir sanal makine ölçek kümesi](https://msdn.microsoft.com/library/azure/mt589035.aspx#linuxconfiguration).
  

### <a name="how-do-i-remove-deprecated-certificates"></a>Kullanım dışı sertifikaları nasıl kaldırılsın mı? 

Kullanım dışı sertifikaları kaldırmak için eski sertifika kasası sertifikalar listesinden kaldırın. Listede, bilgisayarınızdaki kalmasını istediğiniz tüm sertifikaları bırakın. Bu sertifika, tüm VM'lerin kaldırmaz. Bu ayrıca sertifika için sanal makine ölçek kümesindeki oluşturulan yeni VM'ler eklemez. 

Varolan Vm'lerden sertifikayı kaldırmak için sertifika deposundan sertifikaları el ile kaldırmak için bir özel betik uzantısı yazın.
 
### <a name="how-do-i-inject-an-existing-ssh-public-key-into-the-virtual-machine-scale-set-ssh-layer-during-provisioning-i-want-to-store-the-ssh-public-key-values-in-azure-key-vault-and-then-use-them-in-my-resource-manager-template"></a>Nasıl ı mevcut bir SSH ortak anahtarını sanal makine ölçek kümesi SSH katmana sağlama sırasında Ekle? Azure anahtar kasası SSH ortak anahtar değerlerini depolamak ve bunları my Resource Manager şablonunda kullanmak istiyorum.

Genel SSH anahtarı ile yalnızca bir sanal makineleri sağlıyorsanız, ortak anahtarları anahtar kasasına put gerek yoktur. Ortak anahtarlar gizli değildir.
 
Bir Linux VM oluşturduğunuzda SSH ortak anahtarları düz metin sağlayabilirsiniz:

```json
"linuxConfiguration": {
    "ssh": {
        "publicKeys": [
            {
                "path": "path",
                "keyData": "publickey"
            }
        ]
    }
```
 
linuxConfiguration öğe adı | Gerekli | Tür | Açıklama
--- | --- | --- | --- |  ---
SSH | Hayır | Koleksiyon | Linux işletim sistemi SSH anahtar yapılandırması belirtir
yol | Evet | Dize | Burada SSH anahtarlarını veya sertifika yerleştirilmelidir Linux dosya yolunu belirtir
keyData | Evet | Dize | Bir base64 ile kodlanmış SSH ortak anahtarını belirtir

Bir örnek için bkz: [101 vm sshkey GitHub hızlı başlatma şablonunu](https://github.com/Azure/azure-quickstart-templates/blob/master/101-vm-sshkey/azuredeploy.json).

 
### <a name="when-i-run-update-azurermvmss-after-adding-more-than-one-certificate-from-the-same-key-vault-i-see-the-following-message"></a>I çalıştırdığınızda `Update-AzureRmVmss` birden fazla sertifika aynı anahtar Kasası'nı ekledikten sonra ı aşağıdaki iletiyi görürsünüz:
 
>Update-AzureRmVmss: /subscriptions/ < my abonelik-kimliği > yinelenen örnekleri listesi gizli içeren / devre dışı bırakılmış resourceGroups/internal-rg-dev/providers/Microsoft.KeyVault/vaults/internal-keyvault-dev.
 
Mevcut kaynak kasası için yeni bir kasa sertifika kullanmak yerine aynı kasaya yeniden eklemeyi deneyin, bu durum oluşabilir. `Add-AzureRmVmssSecret` Komutu düzgün çalışmaz ek gizli ekliyorsanız.
 
Daha fazla gizli aynı anahtar Kasası'nı eklemek için $vmss.properties.osProfile.secrets[0].vaultCertificates listesini güncelleştirin.
 
Beklenen Giriş yapısı için bkz: [bir sanal makine oluştur veya güncelleştir kümesi](https://msdn.microsoft.com/library/azure/mt589035.aspx).
 
Gizli anahtar kasasına sanal makine ölçek kümesi nesnesindeki bulun. Ardından, kasayla ilişkili listesi için bir sertifika başvurusu (URL ve gizli depo adını) ekleyin.

> [!NOTE] 
> Şu anda, sanal makine ölçek kümesi API'sini kullanarak sertifikaları Vm'lerden kaldıramazsınız.
>

Yeni VM'ler eski sertifika sahip olmaz. Ancak, sertifika sahip ve hangi zaten dağıtılmış olan VM'ler eski sertifika gerekir.
 
### <a name="can-i-push-certificates-to-the-virtual-machine-scale-set-without-providing-the-password-when-the-certificate-is-in-the-secret-store"></a>Sanal makine ölçek sertifika gizli deposunda olduğunda parola sağlamadan ayarlamak için sertifikaları gönderebilir miyim?

Sabit kodlu parolalara komut gerekmez. Parolaları dağıtım komut dosyasını çalıştırmak için kullandığınız izinleri ile dinamik olarak alabilir. Bir sertifika gizli deposu anahtarından taşınır bir komut dosyası varsa, kasa, gizli deposu `get certificate` komut ayrıca .pfx dosyasını parola çıkarır.
 
### <a name="how-does-the-secrets-property-of-virtualmachineprofileosprofile-for-a-virtual-machine-scale-set-work-why-do-i-need-the-sourcevault-value-when-i-have-to-specify-the-absolute-uri-for-a-certificate-by-using-the-certificateurl-property"></a>Bir sanal makine ölçek virtualMachineProfile.osProfile gizli özelliğinin iş nasıl ayarlamak? CertificateUrl özelliğini kullanarak bir sertifika için mutlak URI belirtmeniz gerektiğinde neden sourceVault değeri gerekiyor? 

Windows Uzaktan Yönetim (WinRM) sertifika başvuru işletim sistemi profili gizli özelliğinde mevcut olması gerekir. 

Bir kullanıcının Azure bulut hizmet modelinde var olan erişim denetimi listesi (ACL) ilkelerini zorlamak için kaynak kasası belirten amacı olan. Kaynak kasası belirtilmezse, dağıtmak veya gizli bir anahtar kasası için erişim izni olmayan kullanıcılar bir işlem kaynak sağlayıcısı (CRP aracılığıyla) için gerçekleştirebilir. ACL'ler bile mevcut kaynakları için mevcut.

Yanlış kaynak kasa kimliği geçerli anahtar kasası URL'si ancak sağlarsanız, işlemi yoklama zaman bir hata bildirilir.
 
### <a name="if-i-add-secrets-to-an-existing-virtual-machine-scale-set-are-the-secrets-injected-into-existing-vms-or-only-into-new-ones"></a>Varolan bir t gizli eklerseniz, sanal makine ölçek ayarlayın, var olan sanal makineleri veya yenilerini yalnızca içine eklenen gizli olan? 

Sertifikaları bile önceden var olanları tüm Vm'leriniz için eklenir. UpgradePolicy özelliği, sanal makine ölçek kümesi ayarlanırsa **el ile**, el ile güncelleştirme VM gerçekleştirdiğinizde sertifika VM'ye eklenir.
 
### <a name="where-do-i-put-certificates-for-linux-vms"></a>Linux VM'ler için burada sertifikaları put?

Linux VM'ler için sertifikaları dağıtma hakkında bilgi edinmek için bkz: [dağıtmak sertifikaları VM'ler için müşteri tarafından yönetilen bir anahtar kasadan](https://blogs.technet.microsoft.com/kv/2015/07/14/deploy-certificates-to-vms-from-customer-managed-key-vault/).
  
### <a name="how-do-i-add-a-new-vault-certificate-to-a-new-certificate-object"></a>Yeni bir sertifika nesnesi için nasıl yeni bir kasa sertifika eklensin mi?

Var olan bir gizli anahtarına bir kasa sertifika eklemek için aşağıdaki PowerShell örneğe bakın. Yalnızca bir gizli nesnesi kullanın.
 
```powershell
$newVaultCertificate = New-AzureRmVmssVaultCertificateConfig -CertificateStore MY -CertificateUrl https://sansunallapps1.vault.azure.net:443/secrets/dg-private-enc/55fa0332edc44a84ad655298905f1809
 
$vmss.VirtualMachineProfile.OsProfile.Secrets[0].VaultCertificates.Add($newVaultCertificate)
 
Update-AzureRmVmss -VirtualMachineScaleSet $vmss -ResourceGroup $rg -Name $vmssName
```
 
### <a name="what-happens-to-certificates-if-you-reimage-a-vm"></a>Bir VM görüntüsünü yeniden oluşturmak için sertifikalar ne olur?

Bir VM yeniden görüntü oluşturma sertifikaları silinir. Siler tüm işletim sistemi diski yeniden görüntüsünü oluşturuyor. 
 
### <a name="what-happens-if-you-delete-a-certificate-from-the-key-vault"></a>Anahtar Kasası'nı bir sertifika silersem ne olur?

Gizli anahtar Kasası'nı silinir ve ardından çalıştırdığınız `stop deallocate` tüm VM'ler için ve sonra yeniden başlatın, bir hatası karşılaşmadan. Gizli anahtar Kasası'nı almak CRP gerekiyor, ancak bu işlem gerçekleştirilemiyor hata oluşur. Bu senaryoda, sanal makine ölçek kümesi modelden sertifikaları silebilirsiniz. 

CRP bileşen müşteri gizli kalmaz. Çalıştırırsanız `stop deallocate` sanal makine ölçek kümesindeki tüm VM'ler için önbellek silinir. Bu senaryoda, gizli anahtar Kasası'nı alınır.

Azure Service Fabric gizliliği (tek doku Kiracı modelindeki) önbelleğe alınmış bir kopyasını olduğundan ölçeğini olduğunda bu sorunla karşılaştığınızda yok.
 
### <a name="why-do-i-have-to-specify-the-exact-location-for-the-certificate-url-httpsname-of-the-vaultvaultazurenet443secretsexact-location-as-indicated-in-service-fabric-cluster-security-scenarioshttpsazuremicrosoftcomdocumentationarticlesservice-fabric-cluster-security"></a>Neden sahibim sertifika URL'si tam konumunu belirtmek (https://<name of the vault>.vault.azure.net:443/secrets/<exact location>) belirtilen gibi [Service Fabric kümesi güvenlik senaryoları](https://azure.microsoft.com/documentation/articles/service-fabric-cluster-security/)?
 
Azure anahtar kasası belgelerine sürüm belirtilmezse, parolanın en son sürümünü almak gizli REST API döndürmesi gerektiğini belirtir.
 
Yöntem | URL'si
--- | ---
AL | https://mykeyvault.vault.azure.net/secrets/{secret-name}/{secret-version}?api-version={api-version}

Yerine {*gizli anahtarı adı*} adı ile değiştirin {*gizli sürüm*} almak istediğiniz gizli sürümüyle. Parolanın sürümünü dışarıda. Bu durumda, geçerli sürümü alınır.
  
### <a name="why-do-i-have-to-specify-the-certificate-version-when-i-use-key-vault"></a>Anahtar kasası kullandığınızda sertifika sürümü belirtmek neden var mı?

Sertifika sürüm belirtmek için anahtar kasası gereksinim amacı, hangi sertifika Vm'leri üzerinde dağıtılan temizleyin kullanıcıya olmasını sağlamaktır.

Bir VM oluşturun ve gizli anahtar Kasası'nda güncelleştirme, yeni sertifika Vm'leriniz için indirilmez. Ancak Vm'leriniz ona başvurmak için görünür ve yeni parolayı yeni VM'ler alın. Bu durumu önlemek için gizli bir sürüm başvurmak için gereklidir.

### <a name="my-team-works-with-several-certificates-that-are-distributed-to-us-as-cer-public-keys-what-is-the-recommended-approach-for-deploying-these-certificates-to-a-virtual-machine-scale-set"></a>Ekibin bize .cer ortak anahtarlar dağıtılan birkaç sertifika çalışır. Ne bir sanal makine ölçek bu sertifikaları dağıtmak için önerilen yaklaşım ayarlanır?

.Cer dağıtmak için ortak anahtarları bir sanal makine ölçek kümesi, yalnızca .cer dosyaları içeren bir .pfx dosyası oluşturabilirsiniz. Bunu yapmak için kullanın `X509ContentType = Pfx`. Örneğin, C# veya PowerShell x509Certificate2 nesne olarak .cer dosyasını yükleyin ve ardından yöntemini çağırın. 

Daha fazla bilgi için bkz: [X509Certificate.Export yöntemi (X509ContentType, dize)](https://msdn.microsoft.com/library/24ww6yzk(v=vs.110.aspx)).

### <a name="i-do-not-see-an-option-for-users-to-pass-in-certificates-as-base64-strings-most-other-resource-providers-have-this-option"></a>Kullanıcıların sertifikalar base64 dizesi olarak geçirmek bir seçenek görmezsiniz. Çoğu diğer kaynak sağlayıcıları bu seçeneğiniz vardır.

Bir sertifika bilgilerinde base64 dizesi olarak benzetmek için en son sürümü tutulan URL'yi bir Resource Manager şablonu ayıklayabilirsiniz. Aşağıdaki JSON özelliği, Resource Manager şablonu içerir:

```json 
"certificateUrl": "[reference(resourceId(parameters('vaultResourceGroup'), 'Microsoft.KeyVault/vaults/secrets', parameters('vaultName'), parameters('secretName')), '2015-06-01').secretUriWithVersion]"
```
 
### <a name="do-i-have-to-wrap-certificates-in-json-objects-in-key-vaults"></a>Sertifika anahtar kasalarını JSON nesneleri kaydırın var mı?

Sanal makine ölçek kümeleri ve sanal makineleri, sertifikaları JSON nesneleri alınmalıdır. 

Ayrıca uygulama/x-pkcs12 içerik türü destekliyoruz. Uygulama/x-pkcs12 kullanma ile ilgili yönergeler için bkz: [Azure anahtar kasası PFX sertifikaları](http://www.rahulpnath.com/blog/pfx-certificate-in-azure-key-vault/).
 
Şu anda .cer dosyaları desteklemiyoruz. .Cer dosyaları kullanmak için bunları .pfx kapsayıcılarına dışarı aktarın.



## <a name="compliance-and-security"></a>Uyumluluk ve güvenlik

### <a name="are-virtual-machine-scale-sets-pci-compliant"></a>Sanal makine ölçek PCI uyumlu ayarlar misiniz?

Sanal makine ölçek kümeleri, CRP’nin üzerinde ince bir API katmanıdır. Her iki bileşen de, Azure hizmet ağacındaki işlem platformunun birer parçasıdır.

Uyumluluk açısından bakıldığında, sanal makine ölçek kümeleri Azure işlem platformunun temel parçalarından birini oluşturur. CRP’nin kendisiyle takımı, araçları, süreçleri, dağıtım yöntemini, güvenlik denetimlerini, tam zamanında (JIT) derlemeyi, izlemeyi, uyarıları, vb. paylaşır. Sanal makine ölçek kümeleri Ödeme Kartı Sektörü (PCI) ile uyumludur, çünkü CRP geçerli PCI Veri Güvenliği Standardı (DSS) kanıtının bir parçasıdır.

Daha fazla bilgi için bkz. [Microsoft Güven Merkezi](https://www.microsoft.com/TrustCenter/Compliance/PCI).

### <a name="does-azure-managed-service-identityhttpsdocsmicrosoftcomazureactive-directorymsi-overview-work-with-virtual-machine-scale-sets"></a>Mu [Azure yönetilen hizmet kimliği](https://docs.microsoft.com/azure/active-directory/msi-overview) sanal makine ölçek kümeleri ile çalışma?

Evet. Bazı örnek MSI şablonları Azure hızlı başlangıç şablonlarında görebilirsiniz. Linux: [https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-msi-linux](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-msi-linux). Windows: [ https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-msi-windows ](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-msi-windows).


## <a name="extensions"></a>Uzantılar

### <a name="how-do-i-delete-a-virtual-machine-scale-set-extension"></a>Bir sanal makine ölçek kümesi uzantısının nasıl silebilirim?

Bir sanal makine ölçek kümesi uzantısı silmek için aşağıdaki PowerShell örneği kullanın:

```powershell
$vmss = Get-AzureRmVmss -ResourceGroupName "resource_group_name" -VMScaleSetName "vmssName" 

$vmss=Remove-AzureRmVmssExtension -VirtualMachineScaleSet $vmss -Name "extensionName"

Update-AzureRmVmss -ResourceGroupName "resource_group_name" -VMScaleSetName "vmssName" -VirtualMacineScaleSet $vmss
```
 
UzantıAdı değeri bulabilirsiniz `$vmss`.
   
### <a name="is-there-a-virtual-machine-scale-set-template-example-that-integrates-with-operations-management-suite"></a>Operations Management Suite ile tümleşir şablon örnek bir sanal makine ölçek kümesi var mı?

Operations Management Suite ile tümleşir şablon örnek bir sanal makine ölçek kümesi için ikinci örnekte bkz [Azure Service Fabric kümesi dağıtma ve günlük analizi kullanarak izlemeyi etkinleştir](https://github.com/krnese/AzureDeploy/tree/master/OMS/MSOMS/ServiceFabric).
   
### <a name="extensions-seem-to-run-in-parallel-on-virtual-machine-scale-sets-this-causes-my-custom-script-extension-to-fail-what-can-i-do-to-fix-this"></a>Sanal makine ölçek kümeleri üzerinde paralel olarak çalıştırmak için uzantıları gözükmüyor. Bu, my özel betik uzantısı başarısız olmasına neden olur. Bu sorunu gidermek için ne yapabilirim?

Sanal makine ölçek kümeleri uzantısı sıralaması hakkında bilgi için bkz [uzantısı sıralaması Azure sanal makine ölçek kümeleri](https://msftstack.wordpress.com/2016/05/12/extension-sequencing-in-azure-vm-scale-sets/).
 
 
### <a name="how-do-i-reset-the-password-for-vms-in-my-virtual-machine-scale-set"></a>Nasıl ı parola VM'ler için my sanal makine ölçek kümesindeki sıfırlama?

Ölçek kümesinde VM'ler için parolayı değiştirmek için başlıca iki yolu vardır.

- Sanal makine ölçek kümesi modelini doğrudan değiştirin. İle işlem API 2017-12-01 ve daha sonra kullanılabilir.

    Doğrudan (örneğin Azure kaynak Gezgini, PowerShell veya CLI kullanarak) ölçek kümesi model yönetici kimlik bilgilerini güncelleştirin. Ölçek kümesini sonra güncelleştirilmiş, tüm yeni sanal makineleri yeni kimlik bilgilerine sahip. Bunlar yeniden var olan sanal makineleri yalnızca yeni kimlik bilgileri varsa. 

- VM erişim uzantıları kullanarak parola sıfırlama.

    Aşağıdaki PowerShell örneğini kullanın:
    
    ```powershell
    $vmssName = "myvmss"
    $vmssResourceGroup = "myvmssrg"
    $publicConfig = @{"UserName" = "newuser"}
    $privateConfig = @{"Password" = "********"}
     
    $extName = "VMAccessAgent"
    $publisher = "Microsoft.Compute"
    $vmss = Get-AzureRmVmss -ResourceGroupName $vmssResourceGroup -VMScaleSetName $vmssName
    $vmss = Add-AzureRmVmssExtension -VirtualMachineScaleSet $vmss -Name $extName -Publisher $publisher -Setting $publicConfig -ProtectedSetting $privateConfig -Type $extName -TypeHandlerVersion "2.0" -AutoUpgradeMinorVersion $true
    Update-AzureRmVmss -ResourceGroupName $vmssResourceGroup -Name $vmssName -VirtualMachineScaleSet $vmss
    ```


### <a name="how-do-i-add-an-extension-to-all-vms-in-my-virtual-machine-scale-set"></a>Nasıl bir uzantı için tüm sanal makineleri my sanal makine ölçek kümesindeki ekleyebilirim?

Güncelleştirme ilkesi ayarlanmışsa **otomatik**, yeni uzantısı özellikleri şablonla dağıtarak, tüm VM'ler güncelleştirir.

Güncelleştirme ilkesi ayarlanmışsa **el ile**uzantısı güncelleştirin ve sonra Vm'leriniz tüm örneklerde el ile güncelleştirin.

  
### <a name="if-the-extensions-associated-with-an-existing-virtual-machine-scale-set-are-updated-are-existing-vms-affected-that-is-will-the-vms-not-match-the-virtual-machine-scale-set-model-or-are-they-ignored-when-an-existing-machine-is-service-healed-or-reimaged-are-the-scripts-that-are-currently-configured-on-the-virtual-machine-scale-set-executed-or-are-the-scripts-that-were-configured-when-the-vm-was-first-created-used"></a>Varolan bir sanal makine ölçek kümesi ile ilişkili uzantıları güncelleştirilmişse, etkilenen VM'ler mevcut olan? (Diğer bir deyişle, sanal makineleri olacak *değil* sanal makine ölçek kümesi modelle eşleşecek?) Veya göz ardı edilir? Var olan bir makine service-gösteriyor veya yeniden, çalıştırılan sanal makine ölçek kümesi şu anda yapılandırılmış veya olan betiklerdir VM ilk oluşturulduğunda yapılandırılmış betikleri kullanılıyor?

Sanal makine ölçek genişletme tanımında ayarlarsanız modeli güncelleştirilir ve upgradePolicy özellik kümesine **otomatik**, sanal makineleri güncelleştirir. UpgradePolicy özellik ayarlanmışsa **el ile**, uzantıları bayrakla modelin eşleşmeyen olarak. 

Mevcut bir VM'yi hizmet gösteriyor ise, yeniden başlatma görünür ve uzantıları değil yeniden çalıştırın. Bunu yeniden, işletim sistemi sürücüsü kaynak görüntü değiştirme gibi olur. En son modelden uzantıları gibi tüm uzmanlık çalıştırılır.
 
### <a name="how-do-i-join-a-virtual-machine-scale-set-to-an-azure-ad-domain"></a>Bir Azure AD etki alanı için bir sanal makine ölçek nasıl katılacak mısınız?

Bir Azure Active Directory (Azure AD) etki alanı için bir sanal makine ölçek katılmak için bir uzantı tanımlayabilirsiniz. 

Uzantı tanımlamak için JsonADDomainExtension özelliğini kullanın:

```json
"extensionProfile": {
    "extensions": [
        {
            "name": "joindomain",
            "properties": {
                "publisher": "Microsoft.Compute",
                "type": "JsonADDomainExtension",
                "typeHandlerVersion": "1.3",
                "settings": {
                    "Name": "[parameters('domainName')]",
                    "OUPath": "[variables('ouPath')]",
                    "User": "[variables('domainAndUsername')]",
                    "Restart": "true",
                    "Options": "[variables('domainJoinOptions')]"
                },
                "protectedsettings": {
                    "Password": "[parameters('domainJoinPassword')]"
                }
            }
        }
    ]
}
```
 
### <a name="my-virtual-machine-scale-set-extension-is-trying-to-install-something-that-requires-a-reboot-for-example-commandtoexecute-powershellexe--executionpolicy-unrestricted-install-windowsfeature-name-fs-resource-manager-includemanagementtools"></a>My sanal makine ölçek kümesi uzantısı yeniden başlatma gerektiren bir şey yüklemek çalışıyor. Örneğin, "commandToExecute": "powershell.exe - ExecutionPolicy Unrestricted Install-WindowsFeature – AD FS-Resource-Manager – Includemanagementtools"

Sanal makine ölçek kümesi uzantınızı yeniden başlatma gerektiren bir şey yüklenmeye çalışılırken Azure Otomasyonu istenen durum yapılandırması (Automation DSC) uzantısını sağlayabilirsiniz. İşletim sistemi Windows Server 2012 R2 ise, Azure Windows Management Framework (WMF) 5.0 Kurulumu, yeniden başlatmalar çeker ve yapılandırma ile devam eder. 
 
### <a name="how-do-i-turn-on-antimalware-in-my-virtual-machine-scale-set"></a>My sanal makine ölçek kümesindeki nasıl üzerinde kötü amaçlı yazılımdan koruma dışı?

Kötü amaçlı yazılımdan koruma, sanal makine ölçek kümesi üzerinde etkinleştirmek için aşağıdaki PowerShell örneği kullanın:

```powershell
$rgname = 'autolap'
$vmssname = 'autolapbr'
$location = 'eastus'
 
# Retrieve the most recent version number of the extension.
$allVersions= (Get-AzureRmVMExtensionImage -Location $location -PublisherName "Microsoft.Azure.Security" -Type "IaaSAntimalware").Version
$versionString = $allVersions[($allVersions.count)-1].Split(".")[0] + "." + $allVersions[($allVersions.count)-1].Split(".")[1]
 
$VMSS = Get-AzureRmVmss -ResourceGroupName $rgname -VMScaleSetName $vmssname
echo $VMSS
Add-AzureRmVmssExtension -VirtualMachineScaleSet $VMSS -Name "IaaSAntimalware" -Publisher "Microsoft.Azure.Security" -Type "IaaSAntimalware" -TypeHandlerVersion $versionString
Update-AzureRmVmss -ResourceGroupName $rgname -Name $vmssname -VirtualMachineScaleSet $VMSS 
```

### <a name="i-need-to-execute-a-custom-script-thats-hosted-in-a-private-storage-account-the-script-runs-successfully-when-the-storage-is-public-but-when-i-try-to-use-a-shared-access-signature-sas-it-fails-this-message-is-displayed-missing-mandatory-parameters-for-valid-shared-access-signature-linksas-works-fine-from-my-local-browser"></a>Özel depolama hesabında barındırılan özel bir komut dosyası yürütme gerekir. Komut dosyasını başarıyla depolama genel olduğunda, ancak bir paylaşılan erişim imzası (SAS) kullanmaya çalıştığınızda çalıştırır, başarısız olur. Bu ileti görüntülenir: "için geçerli paylaşılan erişim imzası zorunlu parametreler eksik". Bağlantı + SAS yerel Bilgisayarım tarayıcıdan düzgün çalışır.

Özel depolama hesabında barındırılan özel bir betik yürütmek için korumalı ayarlarını depolama hesabı anahtarı ve adını ayarlayın. Daha fazla bilgi için bkz: [için özel betik uzantısı Windows](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-extensions-customscript/#template-example-for-a-windows-vm-with-protected-settings).


## <a name="networking"></a>Ağ
 
### <a name="is-it-possible-to-assign-a-network-security-group-nsg-to-a-scale-set-so-that-it-applies-to-all-the-vm-nics-in-the-set"></a>Böylece kümedeki tüm VM NIC'ler uygulandığı bir ölçek kümesi için ağ güvenlik grubu (NSG) atamak mümkün mü?

Evet. Bir ağ güvenlik grubunu doğrudan bir ölçeği ağ profili Networkınterfaceconfiguration bölümünde başvurarak Ayarla uygulanabilir. Örnek:

```json
"networkProfile": {
    "networkInterfaceConfigurations": [
        {
            "name": "nic1",
            "properties": {
                "primary": "true",
                "ipConfigurations": [
                    {
                        "name": "ip1",
                        "properties": {
                            "subnet": {
                                "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/virtualNetworks/', variables('vnetName'), '/subnets/subnet1')]"
                            }
                "loadBalancerInboundNatPools": [
                                {
                                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/loadBalancers/', variables('lbName'), '/inboundNatPools/natPool1')]"
                                }
                            ],
                            "loadBalancerBackendAddressPools": [
                                {
                                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/loadBalancers/', variables('lbName'), '/backendAddressPools/addressPool1')]"
                                 }
                            ]
                        }
                    }
                ],
                "networkSecurityGroup": {
                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/networkSecurityGroups/', variables('nsgName'))]"
                }
            }
        }
    ]
}
```

### <a name="how-do-i-do-a-vip-swap-for-virtual-machine-scale-sets-in-the-same-subscription-and-same-region"></a>Sanal makine ölçek kümeleri aynı abonelik ve aynı bölge için bir VIP takası nasıl yapmak istersiniz?

Azure yük dengeleyici ön uç ile iki sanal makine ölçek kümesi varsa ve aynı abonelikte ve bölgede olduklarından, her bir ortak IP adreslerini serbest bırakma ve diğer atayın. Bkz: [VIP takası: Mavi yeşil dağıtım Azure Kaynak Yöneticisi'nde](https://msftstack.wordpress.com/2017/02/24/vip-swap-blue-green-deployment-in-azure-resource-manager/) örneğin. Bu bir gecikme anlamına gelmez yine de serbest/ayrılan ağ kaynakları gibi düzeyi. Azure uygulama ağ geçidi iki arka uç havuzları ve yönlendirme kuralı ile kullanmayı daha hızlı bir seçenektir. Alternatif olarak, uygulamanız ile barındırabilir [Azure uygulama hizmeti](https://azure.microsoft.com/services/app-service/) hazırlama ve üretim yuvası arasında hızlı geçişi için destek sağlar.
 
### <a name="how-do-i-specify-a-range-of-private-ip-addresses-to-use-for-static-private-ip-address-allocation"></a>Statik özel IP adresi ayırma kullanmak üzere özel IP adresleri aralığını nasıl belirtin?

IP adresleri, belirttiğiniz bir alt ağdan seçilir. 

Sanal makine ölçek kümesi IP adreslerinin ayırma yöntemi her zaman "dinamik" olmakla birlikte, bu IP adreslerine değiştirebilirsiniz anlamına gelmez. Bu durumda, "dinamik" yalnızca bir PUT İsteği IP adresi belirtmeyin anlamına gelir. Alt ağ kullanarak ayarlamak statik belirtin. 
    
### <a name="how-do-i-deploy-a-virtual-machine-scale-set-to-an-existing-azure-virtual-network"></a>Bir sanal makine ölçek kümesini mevcut bir Azure sanal ağa nasıl dağıtırım? 

Bir sanal makine ölçek kümesini mevcut bir Azure sanal ağa dağıtmak için bkz: [bir sanal makine ölçek kümesi varolan bir sanal ağa dağıtma](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-existing-vnet). 

### <a name="how-do-i-add-the-ip-address-of-the-first-vm-in-a-virtual-machine-scale-set-to-the-output-of-a-template"></a>Şablon çıktısı için ayarlanmış bir sanal makine ölçek ilk VM IP adresini nasıl ekleyebilirim?

Şablon çıktısı için ayarlanmış bir sanal makine ölçek ilk VM IP adresini eklemek için bkz: [Azure Resource Manager: Get sanal makine ölçek ayarlar özel IP](http://stackoverflow.com/questions/42790392/arm-get-vmsss-private-ips).

### <a name="can-i-use-scale-sets-with-accelerated-networking"></a>Ölçek kümesi ağ hızlandırılmış kullanabilir miyim?

Evet. Hızlandırılmış ağı kullanmak için enableAcceleratedNetworking true olarak kümenin Networkınterfaceconfiguration ayarları, Ölçek ayarlayın. Örneğin:
```json
"networkProfile": {
    "networkInterfaceConfigurations": [
    {
        "name": "niconfig1",
        "properties": {
        "primary": true,
        "enableAcceleratedNetworking" : true,
        "ipConfigurations": [
                ]
            }
            }
        ]
        }
    }
    ]
}
```

### <a name="how-can-i-configure-the-dns-servers-used-by-a-scale-set"></a>Ölçek kümesi tarafından kullanılan DNS sunucularının nasıl yapılandırabilir miyim?

Bir sanal makine ölçek ile özel DNS yapılandırma kümesi oluşturmak için bir dnsSettings JSON paket ölçek kümesi Networkınterfaceconfiguration bölümüne ekleyin. Örnek:
```json
    "dnsSettings":{
        "dnsServers":["10.0.0.6", "10.0.0.5"]
    }
```

### <a name="how-can-i-configure-a-scale-set-to-assign-a-public-ip-address-to-each-vm"></a>Her VM için bir ortak IP adresi atamak için ayarlanmış bir ölçek nasıl yapılandırabilir miyim?

Her VM için bir ortak IP adresi atar bir sanal makine ölçek kümesi oluşturmak için Microsoft.Compute/virtualMachineScaleSets kaynak API sürümü 2017-03-30 olduğundan emin olun ve Ekle bir _publicipaddressconfiguration_ JSON Paket ölçeğe Ipconfigurations bölümünde ayarlayın. Örnek:

```json
    "publicipaddressconfiguration": {
        "name": "pub1",
        "properties": {
        "idleTimeoutInMinutes": 15
        }
    }
```

### <a name="can-i-configure-a-scale-set-to-work-with-multiple-application-gateways"></a>Birden çok uygulama ağ geçitleri ile çalışacak biçimde ayarlamak ölçek yapılandırabilir miyim?

Evet. Kaynak kimliği için birden fazla uygulama ağ geçidi arka uç adres havuzu ekleyebileceğiniz _applicationGatewayBackendAddressPools_ listesinde _Ipconfigurations_ bölümü, Ölçek kümesi profili.

## <a name="scale"></a>Ölçek

### <a name="in-what-case-would-i-create-a-virtual-machine-scale-set-with-fewer-than-two-vms"></a>Hangi durumda ı ikiden VM'ler ile ayarlamak bir sanal makine ölçek oluşturacak?

Esnek bir sanal makine ölçek kümesinin özelliklerini kullanmak için daha az iki VM ile ayarlamak bir sanal makine ölçek oluşturmak için bir neden olacaktır. Örneğin, bir sanal makineyi ölçeği maliyetleri çalıştıran VM ödeme olmadan altyapınızı tanımlamak için sıfır VM'ler ile Ayarla dağıtabilirsiniz. Sanal makineleri dağıtmak hazır olduğunuzda, daha sonra "üretim örnek sayısı için ayarlanmış kapasitesini" sanal makine ölçek artırın.

Bir sanal makineyi ölçeği ikiden VM'ler ile Ayarla oluşturabilirsiniz başka bir, kullanılabilirlik ile ayrık VM'ler kümesi kullanarak kullanılabilirlik az endişe olup olmadığınızı nedenidir. Sanal makine ölçek kümeleri fungible protokole işlem birimleri ile çalışmak için bir yol sağlar. Sanal makine ölçek kümeleri kullanılabilirlik kümeleri karşı temel fark yatan unsur bu bütünlüğünü olur. Çok sayıda durum bilgisiz iş yükleri tek tek birim izlemez. İş yükü düşerse, bir işlem birimi ölçeklendirmek ve iş yükünü artırır, ardından çoktan ölçeği.

### <a name="how-do-i-change-the-number-of-vms-in-a-virtual-machine-scale-set"></a>Bir sanal makine ölçek kümesindeki VM'lerin sayısını nasıl değişiyor?

Azure portalında kümesi bir sanal makine ölçek VM'ler sayısını değiştirmek için sanal makine ölçek özellikler bölümü ayarlamak, "Ölçeklendirme" dikey penceresinde'ı tıklatın ve kaydırıcı çubuğu'nu kullanın. Örnek sayısı değiştirmek diğer yolları için bkz: [bir sanal makine ölçek kümesi örnek sayısını değiştirme](https://msftstack.wordpress.com/2016/05/13/change-the-instance-count-of-an-azure-vm-scale-set/).

### <a name="how-do-i-define-custom-alerts-for-when-certain-thresholds-are-reached"></a>Belirli eşikleri dolduğunda özel uyarılar nasıl tanımlamak?

Belirtilen eşikler için uyarıları nasıl işleneceğini bazı esneklik bulunur. Örneğin, özelleştirilmiş Web kancalarını tanımlayabilirsiniz. Aşağıdaki Web kancası örnek Resource Manager şablonu verilmiştir:

```json
{
    "type": "Microsoft.Insights/autoscaleSettings",
    "apiVersion": "[variables('insightsApi')]",
    "name": "autoscale",
    "location": "[parameters('resourceLocation')]",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachineScaleSets/', parameters('vmSSName'))]"
    ],
    "properties": {
        "name": "autoscale",
        "targetResourceUri": "[concat('/subscriptions/',subscription().subscriptionId, '/resourceGroups/',  resourceGroup().name, '/providers/Microsoft.Compute/virtualMachineScaleSets/', parameters('vmSSName'))]",
        "enabled": true,
        "notifications": [
            {
                "operation": "Scale",
                "email": {
                    "sendToSubscriptionAdministrator": true,
                    "sendToSubscriptionCoAdministrators": true,
                    "customEmails": [
                        "youremail@address.com"
                    ]
                },
                "webhooks": [
                    {
                        "serviceUri": "https://events.pagerduty.com/integration/0b75b57246814149b4d87fa6e1273687/enqueue",
                        "properties": {
                            "key1": "custommetric",
                            "key2": "scalevmss"
                        }
                    }
                ]
            }
        ],
```

Bir Eşiğe ulaşıldığında Bu örnekte, bir uyarı için Pagerduty.com gider.



## <a name="patching-and-operations"></a>Düzeltme eki ve işlemleri

### <a name="how-do-i-create-a-scale-set-in-an-existing-resource-group"></a>Varolan bir kaynak grubundaki bir ölçek nasıl oluşturulur?

Ölçek kümeleri oluşturma var olan bir kaynak grubu henüz Azure portalından mümkün değil, ancak bir ölçek dağıtma bir Azure Resource Manager şablonu ayarladığınızda var olan bir kaynak grubunu belirtebilirsiniz. Varolan bir kaynak grubu, Azure PowerShell veya CLI kullanarak bir ölçek oluştururken de belirtebilirsiniz.

### <a name="can-we-move-a-scale-set-to-another-resource-group"></a>Biz, başka bir kaynak grubu için bir ölçek taşıyabilir miyim?

Evet, yeni abonelik veya kaynak grubu için kaynaklar ölçek kümesi taşıyabilirsiniz.

### <a name="how-to-i-update-my-virtual-machine-scale-set-to-a-new-image-how-do-i-manage-patching"></a>Nasıl için yeni bir görüntüye ayarlamak my sanal makine ölçek güncelleştirebilirim? Düzeltme eki uygulama nasıl yönetebilirim?

Yeni bir görüntüye ayarlayın, sanal makine ölçek güncelleştirme ve düzeltme eki uygulama yönetmek için bkz: [bir sanal makine ölçek kümesini yükseltme](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-upgrade-scale-set).

### <a name="can-i-use-the-reimage-operation-to-reset-a-vm-without-changing-the-image-that-is-i-want-reset-a-vm-to-factory-settings-rather-than-to-a-new-image"></a>Bir VM görüntüsü değiştirmeden sıfırlamak için yeniden görüntü oluşturma işlemi kullanabilir miyim? (Diğer bir deyişle, bir VM fabrika ayarlarına yerine yeni bir görüntüye sıfırlama istiyorum.)

Evet, bir VM görüntüsü değiştirmeden sıfırlamak için yeniden görüntü oluşturma işlemi kullanabilirsiniz. Sanal makine ölçek kümesi varsa ancak, platform görüntüsü ile başvuran `version = latest`, çağırdığınızda, VM daha sonraki bir işletim sistemi görüntüsü güncelleştirebilirsiniz `reimage`.

Daha fazla bilgi için bkz: [bir sanal makine ölçek kümesindeki tüm sanal makineleri yönetme](https://docs.microsoft.com/rest/api/virtualmachinescalesets/manage-all-vms-in-a-set).

### <a name="is-it-possible-to-integrate-scale-sets-with-azure-oms-operations-management-suite"></a>Ölçek kümeleri Azure OMS (Operations Management Suite) ile tümleştirmek mümkün mü?

Evet, ölçeğini OMS uzantısı yükleyerek VM'ler ayarlayabilirsiniz. Azure CLI örnek aşağıda verilmiştir:
```
az vmss extension set --name MicrosoftMonitoringAgent --publisher Microsoft.EnterpriseCloud.Monitoring --resource-group Team-03 --vmss-name nt01 --settings "{'workspaceId': '<your workspace ID here>'}" --protected-settings "{'workspaceKey': '<your workspace key here'}"
```
Gerekli Workspaceıd ve workspaceKey OMS portalında bulabilirsiniz. Genel bakış sayfasında, ayarları kutucuğa tıklayın. Üst bağlı kaynaklar sekmesini tıklatın.

Not:, Ölçek ayarlarsanız _upgradePolicy_ ayarlanmış manuel olarak yükseltme üzerlerinde çağırarak kümesindeki tüm VM'ler için uzantı uygulamanız gerekir. CLI içinde bu olacaktır _az vmss güncelleştirme-örnekleri_.

## <a name="troubleshooting"></a>Sorun giderme

### <a name="how-do-i-turn-on-boot-diagnostics"></a>Önyükleme tanılaması nasıl kapatırım?

Önyükleme Tanılama'yı açmak için ilk olarak, bir depolama hesabı oluşturun. Ardından, bu JSON bloğu, sanal makine ölçek kümesindeki put **virtualMachineProfile**ve sanal makine ölçek kümesini güncelleştir:

```json
"diagnosticsProfile": {
    "bootDiagnostics": {
        "enabled": true,
        "storageUri": "http://yourstorageaccount.blob.core.windows.net"
    }
}
```

Yeni VM oluşturulduğunda, VM InstanceView özelliğinin ekran görüntüsü vb. için ayrıntıları gösterir. Bir örneği aşağıda verilmiştir:
 
```json
"bootDiagnostics": {
    "consoleScreenshotBlobUri": "https://o0sz3nhtbmkg6geswarm5.blob.core.windows.net/bootdiagnostics-swarmagen-4157d838-8335-4f78-bf0e-b616a99bc8bd/swarm-agent-9574AE92vmss-0_2.4157d838-8335-4f78-bf0e-b616a99bc8bd.screenshot.bmp",
    "serialConsoleLogBlobUri": "https://o0sz3nhtbmkg6geswarm5.blob.core.windows.net/bootdiagnostics-swarmagen-4157d838-8335-4f78-bf0e-b616a99bc8bd/swarm-agent-9574AE92vmss-0_2.4157d838-8335-4f78-bf0e-b616a99bc8bd.serialconsole.log"
  }
```


## <a name="virtual-machine-properties"></a>Sanal makine özellikleri

### <a name="how-do-i-get-property-information-for-each-vm-without-making-multiple-calls-for-example-how-would-i-get-the-fault-domain-for-each-of-the-100-vms-in-my-virtual-machine-scale-set"></a>Her VM için özellik bilgilerini birden fazla çağrı yapmadan nasıl sağlarım? Örneğin, hata etki alanı her 100 VM'ler için my sanal makine ölçek kümesindeki nereden?

Birden fazla çağrı yapmadan her VM için özellik bilgilerini almak için çağırabilirsiniz `ListVMInstanceViews` bir REST API yaparak `GET` aşağıdaki kaynak URI'si üzerinde:

/subscriptions/<subscription_id>/resourceGroups/<resource_group_name>/providers/Microsoft.Compute/virtualMachineScaleSets/<scaleset_name>/virtualMachines?$expand=instanceView&$select=instanceView

### <a name="can-i-pass-different-extension-arguments-to-different-vms-in-a-virtual-machine-scale-set"></a>Farklı uzantı bağımsız bir sanal makine ölçek kümesi farklı vm'lerinin geçirebilirsiniz?

Hayır, bir sanal makine ölçek kümesi farklı vm'lerinin farklı uzantı bağımsız değişkenlerini geçiremezsiniz. Ancak, uzantıları gibi makine adı olarak çalışan VM benzersiz özelliklerine bağlı olarak çalışabilir. Uzantıları ayrıca Sorgulayabileceğiniz örneği meta veriler üzerinde http://169.254.169.254 VM hakkında daha fazla bilgi almak için.

### <a name="why-are-there-gaps-between-my-virtual-machine-scale-set-vm-machine-names-and-vm-ids-for-example-0-1-3"></a>Neden my sanal makine ölçek kümesi VM makine adları ve VM kimlikleri arasında boşluklar var mı? Örneğin: 0, 1, 3...

Sanal makine ölçek kümesi için sanal makine ölçek kümesi VM makine adları ve VM kimlikleri arasında boşluklar olan **overprovision** özelliği varsayılan değere ayarlanmış **doğru**. İşleminin ayarlanırsa **doğru**, oluşturulan istenenden daha fazla VM'ler. Ek VM'ler silinir. Bu durumda, artırılmış dağıtım güvenilirlik elde ancak bitişik adlandırma ve bitişik ağ adresi çevirisi (NAT) ödün verme pahasına kuralları. 

Bu özelliği ayarlamak **false**. Küçük sanal makine ölçek kümeleri için bu dağıtım güvenilirlik önemli ölçüde etkilemez.

### <a name="what-is-the-difference-between-deleting-a-vm-in-a-virtual-machine-scale-set-and-deallocating-the-vm-when-should-i-choose-one-over-the-other"></a>Bir sanal makine ölçek kümesindeki VM silme ve VM serbest bırakma arasındaki fark nedir? Diğer üzerinde ne zaman seçmeniz gerekir?

Bir sanal makine ölçek kümesindeki VM silme ve VM serbest bırakma arasındaki temel fark `deallocate` sanal sabit diskleri (VHD) silmez. İle çalışmaya ilişkin depolama maliyetleri vardır `stop deallocate`. Aşağıdaki nedenlerden birinden dolayı birini veya diğerini kullanabilirsiniz:

- İşlem maliyetleri ödeme durdurmak istiyor, ancak sanal makineleri disk durumunu korumak istediğiniz.
- Sanal makineleri bir dizi bir sanal makine ölçek kümesini ölçeklendirin daha hızlı başlatmak istiyor.
  - Bu senaryo ile ilgili, kendi otomatik ölçeklendirme altyapısı ve istediğiniz daha hızlı uçtan uca ölçek oluşturmuş olabileceğiniz.
- Düz olmayan şekilde hata etki alanları veya güncelleştirme etki alanları arasında dağıtılan bir sanal makine ölçek kümesine sahiptir. Bu, seçmeli olarak VM'ler silinmiş veya VM'ler işleminin sonra silindi nedeniyle, olabilir. Çalışan `stop deallocate` arkasından `start` sanal makineyi ölçeği eşit olarak ayarla VM'ler hata etki alanları veya güncelleştirme etki alanları arasında dağıtır.

