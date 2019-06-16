---
title: include dosyası
description: include dosyası
services: virtual-machines
author: shandilvarun
ms.service: virtual-machines
ms.topic: include
ms.date: 08/09/2018
ms.author: vashan, cynthn, rajsqr
ms.custom: include file
ms.openlocfilehash: 57f557a812ec5e4eea75b76ca1394ca360a85d30
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66164655"
---
Azure sanal makineleri (VM'ler) halinde kategorilere farklı durumları geçtikleri *sağlama* ve *güç* durumları. Bu makalenin amacı, bu durumları açıklayan ve müşterilerin olduğunda özellikle vurgulamak için örneği için kullanım faturalandırılır ' dir. 

## <a name="power-states"></a>Güç durumları

Güç durumu, sanal makinenin bilinen son durumu temsil eder.

![VM güç durumu diyagramı](./media/virtual-machines-common-states-lifecycle/vm-power-states.png)

<br>
Aşağıdaki tabloda, her örnek durum açıklamasını sağlar ve örnek kullanım için veya faturalandırılır olup olmadığını gösterir.

<table>
<tr>
<th>
Eyalet
</th>
<th>
Açıklama
</th>
<th>
Örnek Kullanım faturalandırma
</th>
</tr>
<tr>
<td>
<p><b>Başlatma</b></p>
</td>
<td>
<p>VM başlatılıyor.</p>
<code>"statuses": [<br>
   {<br>
      "code": "PowerState/starting",<br>
       "level": "Info",<br>
        "displayStatus": "VM starting"<br>
    }<br>
    ]</code><br>
</td>
<td>
<p><b>Faturalandırılmaz</b></p>
</td>
</tr>
<tr>
<td>
<p><b>Çalıştıran</b></p>
</td>
<td>
<p>Bir VM için normal çalışma durumu</p>
<code>"statuses": [<br>
 {<br>
 "code": "PowerState/running",<br>
 "level": "Info",<br>
 "displayStatus": "VM running"<br>
 }<br>
 ]</code><br>
</td>
<td>
<p><b>Faturalandırılır</b></p>
</td>
</tr>
<tr>
<td>
<p><b>Durduruluyor</b></p>
</td>
<td>
<p>Bu geçici bir durumdur. Tamamlandığında, olarak görünür **durduruldu**.</p>
<code>"statuses": [<br>
 {<br>
 "code": "PowerState/stopping",<br>
 "level": "Info",<br>
 "displayStatus": "VM stopping"<br>
 }<br>
 ]</code><br>
</td>
<td>
<p><b>Faturalandırılır</b></p>
</td>
</tr>
<tr>
<td>
<p><b>Durduruldu</b></p>
</td>
<td>
<p>VM Kapat konuk işletim sistemi içinde aşağı gelen veya değiştiremiyor API'lerini kullanarak.</p>
<p>Donanım yine de VM ayrılır ve konak üzerinde kalır. </p>
<code>"statuses": [<br>
 {<br>
 "code": "PowerState/stopped",<br>
 "level": "Info",<br>
 "displayStatus": "VM stopped"<br>
 }<br>
 ]</code><br>
</td>
<td>
<p><b>Faturalandırılır&#42;</b></p>
</td>
</tr>
<tr>
<td>
<p><b>Serbest bırakılıyor</b></p>
</td>
<td>
<p>Geçiş durumu. Tamamlandığında, VM olarak görünür **Deallocated**.</p>
<code>"statuses": [<br>
 {<br>
 "code": "PowerState/deallocating",<br>
 "level": "Info",<br>
 "displayStatus": "VM deallocating"<br>
 }<br>
 ]</code><br>
</td>
<td>
<p><b>Faturalandırılmaz&#42;</b></p>
</td>
</tr>
<tr>
<td>
<p><b>Serbest bırakıldı</b></p>
</td>
<td>
<p>VM başarıyla durduruldu ve ana bilgisayardan kaldırılır. </p>
<code>"statuses": [<br>
 {<br>
 "code": "PowerState/deallocated",<br>
 "level": "Info",<br>
 "displayStatus": "VM deallocated"<br>
 }<br>
 ]</code><br>
</td>
<td>
<p><b>Faturalandırılmaz</b></p>
</td>
</tr>
</tbody>
</table>


&#42;Diskler ve ağ gibi bazı Azure kaynak ücretleri. Yazılım lisanslarını örneğinde ücretleri uygulanmaz.

## <a name="provisioning-states"></a>Sağlama durumları

Sağlama durumu, sanal makine üzerindeki bir kullanıcı tarafından başlatılan, Denetim düzlemi işleminin durumudur. Bu durumlar, sanal makinenin güç durumu ayrıdır.

- **Oluşturma** – VM oluşturma.

- **Güncelleştirme** – var olan bir VM için model güncelleştirir. Başlangıç/yeniden başlatma da kalan altında güncelleştirme gibi model olmayan bazı VM değiştirir.

- **Silme** – sanal Makineyi silme.

- **Serbest** – burada bir VM durduruldu ve ana bilgisayardan kaldırılır. Bir VM serbest bırakılıyor, güncelleştirmeyle ilgili sağlama durumları görüntülemesi için bir güncelleştirmeyi kabul edilir.



Kullanıcı tarafından başlatılan bir eylem platformu kabul ettikten sonra geçiş işlemi durumlar şunlardır:

<br>

<table>
<tbody>
<tr>
<td width="162">
<p><b>Durumlar</b></p>
</td>
<td width="366">
<p>Açıklama</p>
</td>
</tr>
<tr>
<td width="162">
<p><b>Oluşturma</b></p>
</td>
<td width="366">
<code>"statuses": [<br>
 {<br>
 "code": "ProvisioningState/creating",<br>
 "level": "Info",<br>
 "displayStatus": "Creating"<br>
 }</code><br>
</td>
</tr>
<tr>
<td width="162">
<p><b>Güncelleştirme</b></p>
</td>
<td width="366">
<code>"statuses": [<br>
 {<br>
 "code": "ProvisioningState/updating",<br>
 "level": "Info",<br>
 "displayStatus": "Updating"<br>
 }<br>
 ]</code><br>
</td>
</tr>
<tr>
<td width="162">
<p><b>Silme</b></p>
</td>
<td width="366">
<code>"statuses": [<br>
 {<br>
 "code": "ProvisioningState/deleting",<br>
 "level": "Info",<br>
 "displayStatus": "Deleting"<br>
 }<br>
 ]</code><br>
</td>
</tr>
<tr>
<td width="162">
<p><b>İşletim sistemi sağlama durumları</b></p>
</td>
<td width="366">
<p>Bir işletim sistemi görüntüsüne ve özelleştirilmiş bir görüntü değil ile oluşturulan bir VM'yi, aşağıdaki alt durum gösterilebilir:</p>
<p>1. <b>OSProvisioningInprogress</b> &ndash; VM çalışıyor ve konuk işletim sistemi yüklemesi devam ediyor. <p /> 
<code> "statuses": [<br>
 {<br>
 "code": "ProvisioningState/creating/OSProvisioningInprogress",<br>
 "level": "Info",<br>
 "displayStatus": "OS Provisioning In progress"<br>
 }<br>
]</code><br>
<p>2. <b>OSProvisioningComplete</b> &ndash; kısa süreli durumu. VM için hızlı bir şekilde geçiş **başarı** sürece herhangi bir uzantısı yüklü olması gerekir. Uzantıları Yükleme zaman alabilir. <br />
<code> "statuses": [<br>
 {<br>
 "code": "ProvisioningState/creating/OSProvisioningComplete",<br>
 "level": "Info",<br>
 "displayStatus": "OS Provisioning Complete"<br>
 }<br>
]</code><br>
<p><b>Not</b>: İşletim sistemi sağlama geçiş için **başarısız** bir işletim sistemi hatası veya işletim sistemi sürede yüklemez. Müşteriler, altyapı üzerinde dağıtılan sanal makine için faturalandırılırsınız.</p>
</td>
</tr>
</table>


İşlem tamamlandıktan sonra VM şu durumlardan birini geçer:

- **Başarılı** – kullanıcı tarafından başlatılan Eylemler tamamladınız.

    ```
  "statuses": [ 
  {
     "code": "ProvisioningState/succeeded",
     "level": "Info",
     "displayStatus": "Provisioning succeeded",
     "time": "time"
  }
  ]
    ```

 

- **Başarısız** – başarısız olan bir işlemi temsil eder. Daha fazla bilgi ve olası çözümleri almak için hata kodlarını bakın.

    ```
  "statuses": [
    {
      "code": "ProvisioningState/failed/InternalOperationError",
      "level": "Error",
      "displayStatus": "Provisioning failed",
      "message": "Operation abandoned due to internal error. Please try again later.",
      "time": "time"
    }
    ]
    ```



## <a name="vm-instance-view"></a>Sanal makine örnek görünümü

Örnek görünümü API VM çalışma durumu bilgileri sağlar. Daha fazla bilgi için [sanal makineler - örnek görünümü](https://docs.microsoft.com/rest/api/compute/virtualmachines/instanceview) API belgeleri.

Azure kaynak Gezgini, VM'nin çalışır durumda görüntülemek için basit bir kullanıcı Arabirimi sağlar: [Kaynak Gezgini](https://resources.azure.com/).

Sağlama durumları VM özelliklerini ve örnek görünümünü görülebilir. Güç durumları VM örneği görünümünde kullanılabilir. 

