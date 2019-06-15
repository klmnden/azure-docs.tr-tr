---
title: Azure sanal makine kullanımını anlama | Microsoft Docs
description: Sanal makine kullanım ayrıntılarını anlayın
services: virtual-machines
documentationcenter: ''
author: mmccrory
manager: jeconnoc
editor: ''
tags: azure-virtual-machine
ms.assetid: ''
ms.service: ''
ms.devlang: ''
ms.topic: article
ms.tgt_pltfrm: vm
ms.workload: infrastructure-services
ms.date: 12/04/2017
ms.author: memccror
ms.openlocfilehash: b515a0b226723989b1cc73356f1377da421dc9aa
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61485664"
---
# <a name="understanding-azure-virtual-machine-usage"></a>Azure sanal makine kullanımını anlama
Azure kullanım verilerinizi analiz etmeye tarafından güçlü tüketim öngörüleri elde edebileceğimize – etkinleştirebilirsiniz ınsights daha iyi maliyet yönetim ve kuruluşunuz genelinde ayırma. Bu belge, Azure işlem tüketim ayrıntılarınızı ayrıntılar sağlar. Genel Azure kullanımı hakkında daha fazla ayrıntı için gidin [faturanızı anlama](../../billing/billing-understand-your-bill.md).

## <a name="download-your-usage-details"></a>Kullanım ayrıntılarınızı indirin
Başlamak için [kullanım ayrıntılarını karşıdan](../../billing/billing-download-azure-invoice-daily-usage-date.md). Aşağıdaki tabloda, Azure Resource Manager üzerinden dağıtılan sanal makineler için kullanım tanımı ve örnek değerleri sağlar. Bu belge, Klasik modelimizi dağıtılan VM'ler için ayrıntılı bilgi içermiyor.


| Alanlar             | Anlamı                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       | Örnek değerler                                                                                                                                                                                                                                                                                                                                                   |
|--------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Kullanım Tarihi         | Kaynak kullanıldığında tarih.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |  “11/23/2017”                                                                                                                                                                                                                                                                                                                                                     |
| Ölçüm Kimliği           | Bu kullanımın ait olduğu için üst düzey hizmeti belirtir.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           | "Sanal makineler"                                                                                                                                                                                                                                                                                                                                               |
| Ölçüm Alt Kategorisi | Faturalanan ölçüm tanımlayıcısı. <ul><li>İşlem Saati kullanımı için her bir VM boyutu + işletim sistemi (Windows, Windows dışı) için bir ölçüm yok + bölge.</li><li>Premium yazılım kullanımı için her bir yazılım türü için bir ölçüm yoktur. Çoğu premium yazılım görüntüleri her çekirdek boyutu için farklı ölçümlere sahip. Daha fazla bilgi için ziyaret [Fiyatlandırma sayfasında işlem.](https://azure.microsoft.com/pricing/details/virtual-machines/)</li></ul>                                                                                                                                                                                                                                                                                                                                         | “2005544f-659d-49c9-9094-8e0aea1be3a5”                                                                                                                                                                                                                                                                                                                           |
| Ölçüm Adı         | Bu, her Azure hizmeti için özgüdür. İşlem için her zaman "işlem" saattir.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    | "İşlem saatleri"                                                                                                                                                                                                                                                                                                                                                  |
| Ölçüm Bölgesi       | Veri merkezi konumuna bağlı olarak ücretlendirilen belirli hizmetler için veri merkezinin konumunu belirtir.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |  "JA Doğu"                                                                                                                                                                                                                                                                                                                                                       |
| Birim               | Hizmetin ücretlendirildiği birimi tanımlar. İşlem kaynakları, saat başına faturalandırılır.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    | "Saat"                                                                                                                                                                                                                                                                                                                                                          |
| Kullanılan           | Belirli bir günde tüketilen kaynak miktarını. İşlem için VM belirtilen saat için (en fazla 6 doğruluk ondalık) çalıştıran her dakika için yaparız.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |    “1”, “0.5”                                                                                                                                                                                                                                                                                                                                                    |
| Kaynak Konumu  | Kaynağın çalıştığı veri merkezini tanımlar.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      | "JA Doğu"                                                                                                                                                                                                                                                                                                                                                        |
| Kullanılan Hizmet   | Kullandığınız Azure platform hizmetini.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     | "Microsoft.Compute"                                                                                                                                                                                                                                                                                                                                              |
| Kaynak Grubu     | Dağıtılan kaynağın içinde çalıştığı kaynak grubu. Daha fazla bilgi için [Azure Resource Manager'a genel bakış.](../../azure-resource-manager/resource-group-overview.md)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |    "MyRG"                                                                                                                                                                                                                                                                                                                                                        |
| Örnek Kimliği        | Kaynağın tanımlayıcısı. Tanımlayıcı, kaynağı oluştururken belirttiğiniz adı içerir. VM'ler için örnek kimliği Subscriptionıd, ResourceGroupName ve VMName içerecektir (veya ölçek kümesi adı ölçek kümesi kullanım için).                                                                                                                                                                                                                                                                                                                                                                                                                    | "/ abonelikler/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx / resourceGroups/MyRG/providers/Microsoft.Compute/virtualMachines/MyVM1"<br><br>or<br><br>"/ abonelikler/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx / resourceGroups/MyRG/providers/Microsoft.Compute/virtualMachineScaleSets/MyVMSS1"                                                                                           |
| Tags               | Etiket için kaynak atayın. Fatura kayıtların gruplandırılması için etiketleri kullanın. Bilgi edinmek için nasıl [sanal makinelerinizi etiketi.](tag.md) Bu yalnızca Resource Manager sanal makineler için kullanılabilir.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                | "{"myDepartment":"RD","Kullanicim":"myName"}"                                                                                                                                                                                                                                                                                                                        |
| Ek Bilgi    | Hizmete özgü meta veriler. VM'ler için size aşağıdaki ek bilgi alanındaki doldurun: <ul><li>Çalıştırdığınız görüntü türü-özel görüntü. Desteklenen dizelerini resim türleri altında tam listesini bulabilirsiniz.</li><li>Hizmet türü: dağıttığınız boyutu.</li><li>VMName: sanal makinenizin adıdır. Ölçek kümesi Vm'lerine yalnızca doldurulur. Gerekirse Vm'leri ölçek için sanal Makinenizin adını ayarlayın, yukarıdaki örnek kimliği dizesinde bulabilirsiniz.</li><li>UsageType: Bu, bu gösteren kullanım türünü belirtir.<ul><li>ComputeHR işler için standart_d1_v2 gibi temel alınan sanal makine için işlem saati kullanımdır.</li><li>VM, Microsoft R Server gibi premium yazılım kullanıyorsa ComputeHR_SW premium yazılım ücretlendirme yapılır.</li></ul></li></ul>    | Sanal makineler {"ImageType": "Canonical", "ServiceType": "Standard_DS1_v2", "VMName": "", "UsageType": "ComputeHR"}<br><br>Sanal makine ölçek kümeleri {"ImageType": "Canonical", "ServiceType": "Standard_DS1_v2", "VMName": "myVM1", "UsageType": "ComputeHR"}<br><br>Premium yazılım {"ImageType": "","ServiceType": "Standard_DS1_v2", "VMName": "", "UsageType": "ComputeHR_SW"} |

## <a name="image-type"></a>Görüntü türü
Azure Galerisi'nde bulunan bazı görüntüleri için görüntü türü ek bilgi alanında doldurulur. Bu kullanıcıların anlamak ve bunlar kendi sanal makinesinde ne dağıttıysanız izlemenizi sağlar. Dağıttığınız görüntüyü temel alarak bu alandaki doldurulmuş değerler şunlardır:
  - BitRock 
  - Canonical 
  - FreeBSD 
  - Açık mantıksal 
  - Oracle 
  - SAP için SLES 
  - SQL Server 14 Preview üzerinde Windows Server 2012 R2 Önizleme 
  - SUSE
  - SUSE Premium
  - StorSimple Cloud Appliance 
  - Red Hat
  - SAP iş uygulamaları için Red Hat     
  - SAP HANA için Red Hat 
  - Windows istemci KLG 
  - Windows Server KLG 
  - Windows Server önizlemesi 

## <a name="service-type"></a>Hizmet Türü
Hizmet türü alanı ek bilgi alanındaki dağıttığınız tam VM boyutuna karşılık gelir. Premium depolama Vm'lerini (SSD tabanlı) ve premium olmayan depolama (HDD tabanlı) Vm'leri aynı fiyatlandırılır. Standart gibi bir SSD tabanlı boyutu dağıtırsanız\_DS2\_v2, SSD olmayan boyut bakın (' standart\_D2\_v2 VM') ölçüm alt kategorisi sütun ve SSD boyutu (' standart\_DS2\_ v2') ek bilgi alanındaki.

## <a name="region-names"></a>Bölge adları
Kullanım ayrıntılarını kaynak konum alanında doldurulur bölge adı, Azure Resource Manager ile kullanılan bölge adı farklılık gösterir. Bölge değerleri arasında bir eşleme şu şekildedir:

|    **Resource Manager bölge adı**       |    **Kaynak konumda kullanım ayrıntıları**    |
|--------------------------|------------------------------------------|
|    australiaeast         |    AU Doğu                               |
|    australiasoutheast    |    AU Güneydoğu                          |
|    brazilsouth           |    BR Güney                              |
|    CanadaCentral         |    CA Orta                            |
|    CanadaEast            |    CA Doğu                               |
|    CentralIndia          |    IN Orta                            |
|    centralus             |    Orta ABD                            |
|    chinaeast             |    Çin Doğu                            |
|    chinanorth            |    Çin Kuzey                           |
|    eastasia              |    Doğu Asya                             |
|    eastus                |    Doğu ABD                               |
|    eastus2               |    Doğu ABD 2                             |
|    GermanyCentral        |    DE Orta                            |
|    GermanyNortheast      |    DE Kuzeydoğu                          |
|    japaneast             |    JA Doğu                               |
|    japanwest             |    JA Batı                               |
|    KoreaCentral          |    KR Orta                            |
|    KoreaSouth            |    KR Güney                              |
|    northcentralus        |    Orta Kuzey ABD                      |
|    northeurope           |    Kuzey Avrupa                          |
|    southcentralus        |    Orta Güney ABD                      |
|    southeastasia         |    Güneydoğu Asya                        |
|    SouthIndia            |    IN Güney                              |
|    UKNorth               |    ABD Kuzey                              |
|    uksouth               |    Birleşik Krallık Güney                              |
|    UKSouth2              |    UK Güney 2                            |
|    ukwest                |    Birleşik Krallık Batı                               |
|    USDoDCentral          |    US DoD Orta                        |
|    USDoDEast             |    US DoD Doğu                           |
|    USGovArizona          |    USGov Arizona                         |
|    usgoviowa             |    USGov Iowa                            |
|    USGovTexas            |    USGov Texas                           |
|    usgovvirginia         |    USGov Virginia                        |
|    westcentralus         |    ABD Orta Batı                       |
|    westeurope            |    Batı Avrupa                           |
|    WestIndia             |    IN Batı                               |
|    westus                |    Batı ABD                               |
|    westus2               |    ABD Batı 2                             |


## <a name="virtual-machine-usage-faq"></a>Sanal makine kullanımı ile ilgili SSS
### <a name="what-resources-are-charged-when-deploying-a-vm"></a>Hangi kaynakların bir VM dağıtılırken ücretlendirilir mi?    
VM'ler için sanal Makinenin kendisini, VM ile ilişkili depolama account\managed disk VM üzerinde çalışan herhangi bir premium yazılım maliyetleri almak ve ağ bant genişliği VM'den aktarır.
### <a name="how-can-i-tell-if-a-vm-is-using-azure-hybrid-benefit-in-the-usage-csv"></a>Bir VM içinde kullanım CSV Azure hibrit avantajı kullanıp kullanmadığını nasıl anlayabilirim?
Kullanarak dağıtım yapıyorsanız [Azure hibrit avantajı](https://azure.microsoft.com/pricing/hybrid-benefit/), buluta kendi lisansını getirme beri Non-Windows VM fiyatına ücretlendirilir. Ya da sahip oldukları için Azure hibrit avantajı Resource Manager Vm'lerinde çalışan ayırt faturanızda "Windows\_Server KLG" veya "Windows\_istemci KLG" ImageType sütunda.
### <a name="how-are-basic-vs-standard-vm-types-differentiated-in-the-usage-csv"></a>Temel vs şeklini. Standart VM türleri kullanım CSV'ye Ayrıştırılan?
Temel ve standart A serisi VM'ler sunulur. Ölçüm alt kategorisi, temel bir VM dağıtırsanız, "Temel" dizesi içeriyor Standart A serisi VM dağıtırsanız, standart varsayılan olduğundan VM boyutu "A1 VM" olarak görünür. Temel ve standart arasındaki farklar hakkında daha fazla bilgi için bkz. [Fiyatlandırma sayfasında](https://azure.microsoft.com/pricing/details/virtual-machines/).
### <a name="what-are-extrasmall-small-medium-large-and-extralarge-sizes"></a>Çok küçük, küçük, Orta, büyük ve çok büyük boyutlarda nelerdir?
Eski adlar standart extraSmall - ExtraLarge\_A0-standart\_A4. Klasik sanal makine kullanım kayıtları, bu kural bu boyutları dağıttıysanız kullanılan görebilirsiniz.
### <a name="what-is-the-difference-between-meter-region-and-resource-location"></a>Ölçüm bölgesi ve kaynak konumu arasındaki fark nedir?
Ölçüm bölgesi ölçümü ile ilişkilidir. Tüm bölgeler için tek fiyat kullanan bazı Azure Hizmetleri için ölçüm bölgesi alanın boş olabilir. Ancak, Vm'leri fiyatlar bölge başına ayrılmış sanal makineler için olduğundan, bu alan doldurulur. Benzer şekilde, sanal makineler için kaynak konum, sanal Makinenin dağıtıldığı konumdur. Her iki alan bir Azure bölgesinde aynıdır, bölge adı farklı dize kuralını gerekebilir.
### <a name="why-is-the-imagetype-value-blank-in-the-additional-info-field"></a>Neden ek bilgi alanında ImageType değer boş mi?
ImageType alanın, görüntüleri bir alt kümesi için yalnızca doldurulur. Yukarıdaki görüntülerden birini dağıtmadıysanız ImageType boştur.
### <a name="why-is-the-vmname-blank-in-the-additional-info"></a>Neden VMName içindeki ek bilgi boş mi?
VMName yalnızca ek bilgi alanında VM'ler için bir ölçek kümesindeki doldurulur. Olmayan bir ölçek kümesi Vm'lerine InstanceId alan VM adını içerir.
### <a name="what-does-computehr-mean-in-the-usagetype-field-in-the-additional-info"></a>Ek bilgi UsageType alanında ne ComputeHR anlama geliyor?
İşlem için temel alınan altyapı maliyetini kullanım olayı temsil eden saat ComputeHR gösterir. UsageType ComputeHR ise\_SW, kullanım olayı VM premium yazılım ücreti temsil eder.
### <a name="how-do-i-know-if-i-am-charged-for-premium-software"></a>Premium yazılım için ücretlendirilirim durumunda olduğunu nasıl öğrenebilirim?
Kullanıma emin olun, ihtiyaçlarınıza uygun en iyi hangi VM görüntüsü keşfetme, [Azure Marketi](https://azuremarketplace.microsoft.com/marketplace/apps/category/compute). Görüntünün yazılım planı ücrete sahiptir. "Ücretsiz" ücretine görürseniz, yazılımı için ek ücret yoktur. 
### <a name="what-is-the-difference-between-microsoftclassiccompute-and-microsoftcompute-in-the-consumed-service"></a>Tüketilen hizmetinde Microsoft.ClassicCompute Microsoft.Compute arasındaki fark nedir?
Microsoft.ClassicCompute Azure Service Manager aracılığıyla dağıtılan Klasik kaynakları temsil eder. Ardından, Resource Manager aracılığıyla dağıtırsanız, Microsoft.Compute tüketilen hizmetinde doldurulur. Daha fazla bilgi edinin [Azure dağıtım modelleri](../../azure-resource-manager/resource-manager-deployment-model.md).
### <a name="why-is-the-instanceid-field-blank-for-my-virtual-machine-usage"></a>Neden InstanceId alan sanal makine kullanımı için boş olur?
Klasik dağıtım modeli aracılığıyla dağıtırsanız, InstanceId dize kullanılabilir değil.
### <a name="why-are-the-tags-for-my-vms-not-flowing-to-the-usage-details"></a>Neden Vm'lerimin kullanım ayrıntıları için akan değil için etiketleri misiniz?
Etiket yalnızca, kullanım CSV Resource Manager Vm'leri için yalnızca akış. Klasik kaynak etiketleri kullanım ayrıntıları kullanılabilir değil.
### <a name="how-can-the-consumed-quantity-be-more-than-24-hours-one-day"></a>Tüketilen Miktar, 24 saatten fazla bir gün ne olabilir?
Klasik modeldeki kaynaklar için faturalandırma bulut hizmet düzeyinde toplanır. Aynı fatura ölçümünde kullanan bir bulut hizmetinde birden fazla VM varsa, kullanımınızı araya toplanır. Bu toplama konvansiyonu'nun uygulanmayacağını için Resource Manager üzerinden dağıtılan Vm'leri VM düzeyinde faturalandırılır.
### <a name="why-is-pricing-not-available-for-dsfsgsls-sizes-on-the-pricing-page"></a>Fiyatlandırma sayfasında DS/FS/GS/LS boyutları kullanılabilir fiyatlandırma yok neden?
Premium depolama özellikli VM'ler premium olmayan depolama aynı oranda faturalandırılır özellikli VM'ler. Yalnızca depolama maliyetlerinizi farklılık gösterir. Ziyaret [depolama fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/storage/unmanaged-disks/) daha fazla bilgi için.

## <a name="next-steps"></a>Sonraki adımlar
Kullanım ayrıntılarınızı hakkında daha fazla bilgi için bkz: [Microsoft Azure için faturanızı anlayın.](../../billing/billing-understand-your-bill.md)

