---
title: Azure HANA büyük örnekleri, Azure Portalı aracılığıyla denetim | Microsoft Docs
description: Yol tanımlayabilir ve portal aracılığıyla Azure HANA büyük örnekleri ile etkileşim açıklar
services: virtual-machines-linux,virtual-machines-windows
documentationcenter: ''
author: msjuergent
manager: patfilot
editor: ''
tags: azure-resource-manager
keywords: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 04/02/2019
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 8240308b3e0955b1d4d3ef2e82cad215daf95b00
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61482304"
---
# <a name="azure-hana-large-instances-control-through-azure-portal"></a>Azure portalı üzerinden Azure HANA Büyük Örnekler denetimi
Bu belge yolu nasıl etkinleştireceğinizi de açıklar [HANA büyük örnekleri](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-overview-architecture) sunulur [Azure portalında](https://portal.azure.com) ve hangi etkinlikler sizin için dağıtılan HANA büyük örneği birimleri ile Azure portalı üzerinden gerçekleştirilir. HANA büyük örnekleri görünürlüğünü Azure portalında şu anda genel Önizleme aşamasında olan HANA büyük örnekleri, bir Azure kaynak sağlayıcısı aracılığıyla sağlanır

## <a name="register-hana-large-instance-resource-provider"></a>HANA büyük örnek kaynak sağlayıcısını kaydetme
Genellikle HANA büyük örnek dağıtımlar için kullanmakta olduğunuz Azure aboneliğinize HANA büyük örnek kaynak sağlayıcısı için kaydedilir. Ancak, HANA büyük örneği birimleri dağıtılan göremiyorsanız, Azure aboneliğinizde kaynak sağlayıcısını kaydetmeniz. İki yolu vardır HANA büyük örneği kaynak sağlayıcısını kaydetme

### <a name="register-through-cli-interface"></a>CLI arabirimi aracılığıyla kaydetme
Azure CLI aracılığıyla HANA büyük örnek dağıtımı için kullanılan Azure aboneliğiniz oturum açmış olmanız gerekir. (Re-) kaydı HANA büyük örnek sağlayıcısı bu komut ile yapabilecekleriniz:
    
    az provider register --namespace Microsoft.HanaOnAzure

Daha fazla bilgi için bkz [Azure kaynak sağlayıcıları ve türleri](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-supported-services#azure-cli)


### <a name="register-through-azure-portal"></a>Azure Portalı aracılığıyla kaydolun
(Re-) kaydı Azure Portalı aracılığıyla HANA büyük örneği kaynak sağlayıcısını kullanabilirsiniz. Azure portalında aboneliğinizi listelemek ve çift tıklayın, HANA büyük örneği birim dağıtmak için kullanılan abonelik gerekir. Bir aboneliğiniz genel bakış sayfasında olduğunuz aşağıda gösterildiği gibi "Kaynak sağlayıcıları" seçin ve arama penceresine "HANA" yazın. 

![HLI RP Azure Portalı aracılığıyla kaydolun](./media/hana-li-portal/portal-register-hli-rp.png)

Gösterilen ekran görüntüsünde, kaynak sağlayıcısı zaten kaydedildi. Kaynak Sağlayıcısı'nın henüz kaydedilmemiş durumunda, "yeniden Kaydet" veya "Kaydet" tuşuna basın.

Daha fazla bilgi için bkz [Azure kaynak sağlayıcıları ve türleri](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-supported-services#azure-powershell)


## <a name="display-of-hana-large-instance-units-in-the-azure-portal"></a>Azure portalında HANA büyük örneği birimlerinin görüntüleme
Bir HANA büyük örneği dağıtım isteği gönderilirken, HANA büyük örneklerine de bağlandığınız Azure aboneliğini belirtmeniz istenir. Bu, HANA büyük örneği birimleri karşı çalışan SAP uygulama katmanına dağıtmak için kullandığınız aynı aboneliği kullanmanız önerilir.
İlk HANA büyük örnekler, yeni bir dağıtılan [Azure kaynak grubu](https://docs.microsoft.com/azure/azure-resource-manager/manage-resources-portal) gönderdiğiniz dağıtım isteğinde, HANA büyük örnekleri için Azure aboneliği oluşturulur.  Yeni kaynak grubu belirli bir abonelikte dağıtılmış, HANA büyük örneği birimleri listeler.

Yeni bir Azure kaynak grubu bulmak için kaynak grubunun aboneliğinizde Azure portalının sol gezinti bölmesi aracılığıyla giderek listesi

![Azure portalındaki Gezinti Bölmesi](./media/hana-li-portal/portal-resource-group.png)

Kaynak grupları listesinde, listelenen, dağıtılan HANA büyük örnekler için kullanılan abonelikte filtrelemeniz gerekebilir

![Azure portalında kaynak grupları Filtrele](./media/hana-li-portal/portal-filtering-subscription.png)

Doğru abonelik filtreleme sonrasında, uzun bir kaynak gruplarının listesi yine de olabilir. Aşağıdakilerden sonrası düzeltme ile arayın **- Txxx** "xxx" üç basamak olduğunda, ister **-T050**. 

Kaynak grubu bulunamadı gibi ayrıntılarını listeler. Aldığınız listesi gibi görünebilir:

![Azure portalında HLI listesi](./media/hana-li-portal/portal-hli-units-list.png)

Aboneliğinizde dağıtılan tek bir HANA büyük örneği birim listelenen tüm birimleri temsil eder. Bu durumda, aboneliğinizde dağıtılmış sekiz farklı HANA büyük örneği birimleri bakın.

Aynı Azure aboneliği altında birden çok HANA büyük örneği kiracıyı dağıttıysanız, birden fazla Azure kaynak grubu bulabilirsiniz. 


## <a name="look-at-attributes-of-single-hli-unit"></a>Tek HLI birim öznitelikleri Ara
HANA büyük örneği birimler listesinde, tek bir birim üzerinde tıklayın ve tek bir HANA büyük örneği birim ayrıntılarını alın. 

Genel Bakış ekranda biriminin şuna benzer bir sunu alıyorsanız:

![Genel Bakış HLI birimi Göster](./media/hana-li-portal/portal-show-overview.png)

Gösterilen farklı öznitelikler sırasında bakarak, özniteliklerle Azure VM öznitelikleri zor farklı görünür. Sol taraftaki başlığa, kaynak grubu, Azure bölgesi, abonelik adı ve kimliği yanı sıra, eklediğiniz bazı etiketler gösterir. Varsayılan olarak, HANA büyük örneği birimleri atanmış hiçbir etiket var. Sağ üst tarafında, birim adını dağıtım tamamlandığında atanmış olarak listelenir. İşletim sisteminin yanı sıra IP adresi gösterilir. Olarak CPU sayısı ile HANA büyük örneği birim türü vm'lerle iş parçacıkları ve bellek gösterilmemektedir de. Burada farklı HANA büyük örneği birimleri hakkında daha fazla ayrıntı gösterilir:

- [HLI için kullanılabilen SKU’lar](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-available-skus)
- [SAP HANA (büyük örnekler) depolama mimarisi](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-storage-architecture) 

Başka bir alan başlığın sağ sütunda HANA büyük örneği birim güç durumu hakkında sizi bilgilendirir.

> [!NOTE]
> Donanım birim açıp desteklenen güç durumunu açıklar. İşletim sistemi olan ayarlama ve çalıştırma hakkında bilgi sağlamaz. Bir HANA büyük örneği birim yeniden gibi küçük birim durumunu burada değişiklikler bir saat yaşar **başlangıç** durumunu taşımak için **başlatıldı**. Şu durumunda olan **başlatıldı** işletim sistemi başlatılıyor veya işletim sistemi tamamen başlatıldığını anlamına gelir. Sonuç olarak, birimin yeniden başlatma durumu geçer hemen sonra birim içine hemen oturum beklediğiniz olamaz **başlatıldı**.
> 


## <a name="check-activities-of-a-single-hana-large-instance-unit"></a>Tek bir HANA büyük örneği birim etkinliklerini denetleme 
HANA büyük örneği birimleri genel bakışını veren dışında özel birim etkinliklerini kontrol edebilirsiniz. Bir etkinlik günlüğü gibi görünebilir:

![Azure portalındaki Gezinti Bölmesi](./media/hana-li-portal/portal-activity-list.png)

Kaydedilen ana etkinlikleri biridir birim yeniden başlatılır. Etkinlik, etkinlik tetiklenen, zaman damgası / abonelik kimliği durumunu listelenen verileri içeren etkinliği tetikleyen ve etkinliği tetikleyen Azure kullanıcı. 

Azure meta veri biriminde değişiklikler kaydedilir başka bir etkinliği var. Başlatılan yeniden başlatma yanı sıra etkinliğini görebilirsiniz **yazma HANAInstances**. Bu tür bir etkinlik, HANA büyük örneği birim kendisini hiçbir değişiklik yapar, ancak azure'da biriminin meta verilerde yapılan değişiklikleri belgelemek. Listelenen durumda, biz eklenir ve bir etiket silindi (sonraki bölüme bakın).

## <a name="add-and-delete-an-azure-tag-to-a-hana-large-instance-unit"></a>Ekleme ve bir Azure HANA büyük örneği birim etiketini silme
Sahip olduğunuz başka bir olasılık ise bir [etiketi](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags) HANA büyük örneği birim. Etiketleri atadığınızdan yöntemini Vm'lere etiketleri atamadan farklı değildir. Olarak vm'lerle etiketleri Azure meta verilerinde mevcut ve HANA büyük örnekler için sanal makineler için etiketleri aynı kısıtlamalara sahip.

Etiketleri silme gibi sanal makineleri ile aynı şekilde çalışır. Uygulama ve bir etiketi silme hem etkinlikleri belirli HANA büyük örneği birim etkinlik günlüğünde listelenir.

## <a name="check-properties-of-a-hana-large-instance-unit"></a>Bir HANA büyük örneği birimin özelliklerini denetleyin
Bölüm **özellikleri** size edilmeden örnekleri olduğunda aldığınız önemli bilgiler içerir. Destek gerekli olabilecek tüm bilgileri nereden bir bölüm olduğu durumlarda veya anlık görüntü depolama yapılandırması gerekir. Bu nedenle bu bölümde Azure ve depolama arka ucu örneği bağlantısı örneğinizin geçici veri oluşan bir koleksiyondur. Bölümün üst şuna benzer:


![üst kısmında, Azure portalında HLI özellikleri](./media/hana-li-portal/portal-properties-top.png)

İlk birkaç veri öğelerini, genel bakış ekranda zaten yayımlandı. Ancak veri önemli bir bölümü olarak ilk dağıtılan birimine teslim, aldığınız ExpressRoute bağlantı hattı kimliği. Destek bazı durumlarda, bu veriler için sorulan. Önemli veri girişi, ekran alt kısmında gösterilir. Görüntülenen veri depolama yalıtır NFS depolama baş IP adresidir, **Kiracı** HANA büyük örneği yığınında. Bu IP adresi de düzenlerken gerekli [yapılandırma dosya depolaması için anlık görüntü yedeklemeleri](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-backup-restore#set-up-storage-snapshots). 

Özellik bölmesinde aşağı kaydırın gibi HANA büyük örneği birim için bir benzersiz kaynak kimliği veya dağıtıma atandı abonelik kimliği gibi ek verileri elde edersiniz.

## <a name="restart-a-hana-large-instance-unit-through-azure-portal"></a>Azure Portalı aracılığıyla bir HANA büyük örneği birim yeniden başlatın
Linux işletim sistemi yeniden başlatma, burada işletim sistemi yeniden başlatma başarıyla tamamlayamadı çeşitli durumlarda vardı. Yeniden başlatma zorlamak için Microsoft Operasyon HANA büyük örneği birim güç yeniden gerçekleştirmek için bir hizmet isteği açmak gerekli. Güç yeniden HANA büyük örneği birim işlevselliğini artık Azure portalında tümleşiktir. HANA büyük örneği birim genel bakış parçası olduğundan, veri bölümü üzerinde yeniden başlatma düğmesini bakın

![#1. adım, Azure portalında yeniden başlatın](./media/hana-li-portal/portal-restart-first-step.png)

Yeniden başlatma düğmesine basarak gibi gerçekten birim yeniden isteyip istemediğinizi istenir. "Evet" düğmesine basarak onaylayın gibi birim yeniden başlatılır.

> [!NOTE]
> Yeniden başlatma işleminde küçük birim durumunu burada değişiklikler bir saat yaşar **başlangıç** durumunu taşımak için **başlatıldı**. Şu durumunda olan **başlatıldı** işletim sistemi başlatılıyor veya işletim sistemi tamamen başlatıldığını anlamına gelir. Sonuç olarak, birimin yeniden başlatma durumu geçer hemen sonra birim içine hemen oturum beklediğiniz olamaz **başlatıldı**.

> [!IMPORTANT]
> HANA büyük örneği birimi cinsinden bellek miktarını bağımlı bir yeniden başlatma ve yeniden başlatma ve donanım ve işletim sistemi bir saate kadar sürebilir


## <a name="open-a-support-request-for-hana-large-instances"></a>HANA büyük örnekleri için bir destek isteği açın
Dışında Azure portal ekran HANA büyük örneği birim, özellikle bir HANA büyük örneği birim için de destek istekleri oluşturabilirsiniz. Bağlantıyı takip **yeni destek isteği** 

![Azure portalında hizmet isteği adım #1 başlatmak](./media/hana-li-portal/portal-initiate-support-request.png)

SAP HANA büyük sonraki ekranda listelenen örnekler hizmet alabilmek için seçmeniz gerekebilir ' tüm hizmetler "aşağıda gösterildiği gibi

![Azure portalında tüm hizmetleri seçin](./media/hana-li-portal/portal-create-service-request.png)

Hizmetler listesinde hizmetini bulabileceğiniz **SAP HANA büyük örneği**. Bu hizmet seçerken, gösterildiği gibi belirli sorun türleri seçebilirsiniz:


![Azure Portalı'nda sorun sınıfı seçin](./media/hana-li-portal/portal-select-problem-class.png)

Türlerinin her biri farklı bir sorun altında sorununuzu daha fazla özelleştirmek için seçmenize gerek sorun subtypes seçimi sunulur. Alt seçtikten sonra artık konu adı verebilirsiniz. Seçim işlemi ile işiniz bittiğinde, sonraki adıma oluşturma taşıyabilirsiniz. İçinde **çözümleri** bölümünde, işaret belgeler için HANA büyük örnekleri çevresinde, hangi bulunun bir işaretçi sorununuzu bir çözüme. Önerilen belgelerinde sorununuz için bir çözüm bulamazsanız, sonraki adıma gidin. Sonraki adımda, HANA büyük örneği birimleri veya Vm'leri ile sorun olup olmadığını güvenmenizin sorulmasını seçeceksiniz. Bu bilgiler doğru uzmanlarıyla destek isteği yönlendirmek için yardımcı olur. 

![Azure portalında bir destek olayı ayrıntıları](./media/hana-li-portal/portal-support-request-details.png)

Sorulara verdiğiniz yanıtlara ve ek ayrıntılar sağlanan destek talebi ve gönderme gözden geçirmek için sonraki adım gidebilir.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure'da SAP HANA (büyük örnekler) izleme](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/troubleshooting-monitoring)
- [HANA tarafından izleme ve sorun giderme](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-monitor-troubleshoot)

