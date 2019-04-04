---
title: Azure Stack ölçek düğümleri ekleyin | Microsoft Docs
description: Azure Stack'te birimleri ölçeklendirme düğümler ekleyin.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/12/2019
ms.author: jeffgilb
ms.reviewer: thoroet
ms.lastreviewed: 09/17/2018
ms.openlocfilehash: 17da540bd6077b8e045f125fd3cf13dc0e043000
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2019
ms.locfileid: "58882048"
---
# <a name="add-additional-scale-unit-nodes-in-azure-stack"></a>Azure Stack'te ek ölçek birimi düğümleri Ekle

Azure Stack operatörleri, başka bir fiziksel bilgisayar ekleyerek mevcut bir ölçek birimi genel kapasitesini artırabilir. Fiziksel bilgisayar ölçek birimi düğüm olarak adlandırılır. Eklediğiniz her yeni ölçek birimi düğüm homojen CPU türü, bellek ve disk numarası ve ölçek birimi varsa düğümlerin boyutu olmalıdır.

Bir ölçek birimi düğümü eklemek için Azure Stack'te hareket ve, donanım ekipman üreticisi (OEM) Araçları'nı çalıştırın. OEM araçları, konakta yeni fiziksel bilgisayar var olan düğümleri aynı üretici yazılımı düzeyinde eşleştiğinden emin olmak için donanım yaşam döngüsü (HLH) çalıştırır.

Bir ölçek birimi düğüm eklemek için genel süreç aşağıdaki akış diyagramı gösterir.

![Ölçek birimi akış ekleme](media/azure-stack-add-scale-node/add-node-flow.png) &#42; *OEM donanım satıcınıza fiziksel sunucu rafa yerleştirme enacts ve üretici yazılımı güncelleştirmeleri, destek sözleşmenize göre değişir.*

Yeni bir düğüm ekleme işlemi birkaç saat veya gün tamamlanması alabilir.

> [!Note]  
> Aşağıdaki işlemlerden birini, bir ekleme ölçek birimi düğüm işlemi zaten devam ederken kullanmayın:
>
>  - Azure Stack güncelleştir
>  - Sertifika döndürme
>  - Azure Stack Durdur
>  - Onarım ölçek birimi düğümü


## <a name="add-scale-unit-nodes"></a>Ölçek birimi düğümleri Ekle

Bir düğüm ekleme üst düzey bir genel bakış adımlardır. İlk OEM tarafından sağlanan kapasite genişletmesi belgelerinize bakarak olmadan adımları izlemeyin.

1. Yeni fiziksel sunucuda rafa yerleştirin ve uygun şekilde kablo. 
2. Fiziksel anahtar bağlantı noktalarını etkinleştirme ve erişim denetim listeleri (ACL'ler) varsa ayarlayın.
3. Temel kart yönetim denetleyicisi (BMC) doğru IP adresi yapılandırın ve OEM tarafından sağlanan belgelerinize başına tüm BIOS ayarlarını uygulayın.
4. Geçerli bellenim temel HLH üzerinde çalıştırılan donanım üreticisi tarafından sağlanan araçları kullanarak tüm bileşenleri için geçerlidir.
5. Düğüm ekleme işlemi Azure Stack Yönetici portalı'nda çalıştırın.
6. Düğüm ekleme işlemi başarılı olduğunu doğrulayın. Bunu yapmak için kontrol [ **durumu** ölçek birimi](#monitor-add-node-operations). 

## <a name="add-the-node"></a>Düğüm Ekle

Yeni bir düğüm eklemek için yönetim portalını veya PowerShell'i kullanabilirsiniz. Düğüm ekleme işlemi, ilk olarak kullanılabilir bilgi işlem kapasitesi yeni ölçek birimi düğümü ekler ve otomatik olarak depolama kapasitesini genişletir. Azure Stack bir hiper yakınsanmış sistemi olduğundan kapasiteyi otomatik olarak genişleyen burada *işlem* ve *depolama* birlikte ölçeklendirin.

### <a name="use-the-admin-portal"></a>Yönetici portalını kullanma

1. Azure Stack operatörü Azure Stack Yönetici portalında oturum açın.
2. Gidin **+ kaynak Oluştur** > **kapasite** > **ölçek birimi düğüm**.
   ![Ölçek birimi düğümü](media/azure-stack-add-scale-node/select-node1.png)
3. Üzerinde **Ekle düğüm** bölmesinde *bölge*ve ardından *ölçek birimi* düğüme eklemek istediğiniz. Ayrıca belirtin *BMC IP adresi* eklemekte olduğunuz ölçek birimi düğüm. Bu gibi durumlarda, bir düğüm yalnızca bir kerede ekleyebilirsiniz.
   ![Düğüm ayrıntıları ekleyin](media/azure-stack-add-scale-node/select-node2.png)
 

### <a name="use-powershell"></a>PowerShell kullanma

Kullanım **yeni AzsScaleUnitNodeObject** cmdlet'i bir düğüm eklemek için.  

Değerleri değiştirmek için aşağıdaki PowerShell betiklerine herhangi birini kullanmadan önce *düğüm adları* ve *IP adresleri* , Azure Stack ortamınıza değerlerle.

  > [!Note]  
  > Bir düğüm adlandırırken adı 15 karakterden uzun tutmanız gerekir. Ayrıca, bir boşluk veya şu karakterlerden herhangi birini içeriyorsa bir ad kullanamazsınız: `\`, `/`, `:`, `*`, `?`, `"`, `<`, `>`, `|`, `\`, `~`, `!`, `@`, `#`, `$`, `%`, `^`, `&`, `(`, `)`, `{`, `}`, `_`.

**Bir düğüm ekleyin:**
  ```powershell
  ## Add a single Node 
  $NewNode=New-AzsScaleUnitNodeObject -computername "<name_of_new_node>" -BMCIPv4Address "<BMCIP_address_of_new_node>" 
 
  Add-AzsScaleUnitNode -NodeList $NewNode -ScaleUnit "<name_of_scale_unit_cluster>" 
  ```  

## <a name="monitor-add-node-operations"></a>İzleyici düğümü işlemleri Ekle 
Düğüm ekleme işlemi durumunu almak için yönetim portalını veya PowerShell'i kullanabilirsiniz. Düğüm işlemleri gün tamamlanması birkaç saat sürebilir ekleyin.

### <a name="use-the-admin-portal"></a>Yönetici portalını kullanma 
Yeni bir düğüm eklenmesi izlemek için Yönetim Portalı'nda ölçek birimi gözden geçirmek veya birim düğüm nesneleri ölçeklendirin. Bunu yapmak için Git **bölge Yönetimi** > **ölçek birimleri**. Ardından, gözden geçirmek istediğiniz ölçek birimi düğüm ve ölçek birimi seçin. 

### <a name="use-powershell"></a>PowerShell kullanma
PowerShell kullanarak aşağıdaki gibi ölçek birimi ve ölçek birimi düğümlerin durumunun alınabilir:
  ```powershell
  #Retrieve Status for the Scale Unit
  Get-AzsScaleUnit|select name,state
 
  #Retrieve Status for each Scale Unit Node
  Get-AzsScaleUnitNode |Select Name, ScaleUnitNodeStatus
```

### <a name="status-for-the-add-node-operation"></a>Düğüm ekleme işlemi durumu 
**Bir ölçek birimi için:**

|Durum               |Açıklama  |
|---------------------|---------|
|Çalışıyor              |Tüm düğümler etkin bir şekilde ölçek birimi katılıyor.|
|Durduruldu              |Ölçek birimi aşağı veya erişilemeyen düğümüdür.|
|Genişletme            |Bir veya daha fazla ölçek birimi düğümler şu anda işlem kapasitesi gibi eklenmektedir.|
|Depolama yapılandırma  |İşlem kapasitesini genişletildi ve depolama yapılandırması çalışıyor.|
|Düzeltmesi gerektiriyor |Onarılması için bir veya daha fazla ölçek birimi düğümü gerektiren bir hata algılandı.|


**Bir ölçek birimi düğümü için:**

|Durum                |Açıklama  |
|----------------------|---------|
|Çalışıyor               |Düğüm, etkin bir şekilde ölçek birimi katılıyor.|
|Durduruldu               |Düğümü, kullanılamıyor.|
|Ekleniyor                |Düğüm etkin bir şekilde Ölçek birimine ekleniyor.|
|Onarılıyor             |Düğüm etkin bir şekilde onarılıyor.|
|Bakım           |Düğüm duraklatılmadan ve hiçbir etkin kullanıcı iş yükü çalışıyor. |
|Düzeltmesi gerektiriyor  |Onarılması düğümü gerektiren bir hata algılandı.|


## <a name="troubleshooting"></a>Sorun giderme
Bir düğüm ekleme sırasında görülen yaygın sorunlar aşağıda verilmiştir. 

**Senaryo 1:**  Ölçek birimi düğüm ekleme işlemi başarısız olur, ancak bir veya daha fazla düğüm durdurulmuş durumuyla listelenir.  
- Düzeltme: Bir veya daha fazla düğüm onarmak için onarım işlemi kullanın. Yalnızca tek bir onarım işlemi aynı anda çalıştırabilirsiniz.

**Senaryo 2:** Bir veya daha fazla ölçek birimi düğüm eklenmiştir, ancak depolama genişletme başarısız oldu. Bu senaryoda, Ölçek birimi düğüm nesnesi çalıştırma durumunu bildirir ancak Vmm'de depolamayı yapılandırma görevi başlattığınız değil.  
- Düzeltme: Ayrıcalıklı uç noktası, aşağıdaki PowerShell cmdlet'ini çalıştırarak depolama durumunu gözden geçirmek için kullanın:
  ```powershell
     Get-VirtualDisk -CimSession s-cluster | Get-StorageJob
  ```
 
**Senaryo 3:** Depolama ölçeklendirme işi başarısız oldu belirten bir uyarı aldığınızı.  
- Düzeltme: Bu durumda, depolama yapılandırma görevi başarısız oldu. Bu sorun, destek ile iletişime geçmenizi gerektirir.


## <a name="next-steps"></a>Sonraki adımlar 
[Genel IP adresleri ekleme](azure-stack-add-ips.md) 
