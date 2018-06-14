---
title: Azure sanal makine kullanımını anlama | Microsoft Docs
description: Sanal makine kullanım ayrıntılarını anlama
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
ms.openlocfilehash: c87c4256aa193a4971b75c3230d1996c2efdc352
ms.sourcegitcommit: 7f1ce8be5367d492f4c8bb889ad50a99d85d9a89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2017
ms.locfileid: "26345456"
---
# <a name="understanding-azure-virtual-machine-usage"></a>Azure sanal makinesi öngörme
Azure kullanım verilerini analiz etme tarafından güçlü tüketim Öngörüler elde – etkinleştirebilirsiniz Öngörüler daha iyi yönetim ve kuruluşunuz genelinde ayırma maliyeti. Bu belgede Azure işlem tüketim ayrıntılarınızı içine derinlemesine bir bakış sağlar. Genel Azure kullanımı hakkında daha fazla ayrıntı için gidin [faturanızı anlamak](/billing/billing-understand-your-bill.md).

## <a name="download-your-usage-details"></a>Kullanım ayrıntılarını indirin
Başlamak için [kullanım ayrıntılarınızı karşıdan](/billing/billing-download-azure-invoice-daily-usage-date#download-usage-from-the-account-center-csv.md). Aşağıdaki tabloda, Azure Resource Manager aracılığıyla dağıtılan sanal makineleri için kullanım tanımı ve örnek değerleri sağlar. Bu belge bizim Klasik modeli aracılığıyla dağıtılan VM'ler için ayrıntılı bilgileri içermiyor.


| Alanlar             | Anlamı                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       | Örnek değerler                                                                                                                                                                                                                                                                                                                                                   |
|--------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Kullanım Tarihi         | Kaynağın ne zaman kullanıldığı tarih.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |  “11/23/2017”                                                                                                                                                                                                                                                                                                                                                     |
| Ölçüm Kimliği           | Bu kullanım ait olduğu için üst düzey hizmet tanımlar.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           | "Sanal makineler"                                                                                                                                                                                                                                                                                                                                               |
| Ölçüm Alt Kategorisi | Faturalanan ölçüm tanımlayıcısı. <ul><li>İşlem Saati kullanım için bir ölçüm her VM boyutu + (Windows, Windows olmayan) işletim sistemi için olduğundan + bölge.</li><li>Premium yazılım kullanımı için her yazılım türü için bir ölçüm yoktur. Çoğu premium yazılım resimler her çekirdek boyutu için farklı ölçümler vardır. Daha fazla bilgi için ziyaret [Fiyatlandırma sayfasında işlem.](https://azure.microsoft.com/pricing/details/virtual-machines/)</li></ul>                                                                                                                                                                                                                                                                                                                                         | "2005544f-659d-49c9-9094-8e0aea1be3a5"                                                                                                                                                                                                                                                                                                                           |
| Ölçüm Adı         | Bu, Azure her hizmet için özeldir. İşlem için her zaman "işlem" saattir.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    | "İşlem saatleri"                                                                                                                                                                                                                                                                                                                                                  |
| Ölçüm Bölgesi       | Veri merkezi konumuna bağlı olarak ücretlendirilen belirli hizmetler için veri merkezinin konumunu belirtir.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |  "JA Doğu"                                                                                                                                                                                                                                                                                                                                                       |
| Birim               | Hizmet olarak ücretlendirilir birimi tanımlar. İşlem kaynakları saat başına faturalandırılır.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    | "Saat"                                                                                                                                                                                                                                                                                                                                                          |
| Kullanılan           | O gün için kullanılan kaynak miktarı. İşlem için belirli bir saat için (doğruluk 6 ondalık basamak) VM çalıştıran her dakika biz faturalandıracaktır.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |    “1”, “0.5”                                                                                                                                                                                                                                                                                                                                                    |
| Kaynak Konumu  | Kaynağın çalıştığı veri merkezini tanımlar.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      | "JA Doğu"                                                                                                                                                                                                                                                                                                                                                        |
| Kullanılan Hizmet   | Kullandığınız Azure platformu hizmet.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     | "Microsoft.Compute"                                                                                                                                                                                                                                                                                                                                              |
| Kaynak Grubu     | Dağıtılan kaynağın içinde çalıştığı kaynak grubu. Daha fazla bilgi için bkz: [Azure Resource Manager'a genel bakış.](/azure-resource-manager/resource-group-overview.md)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |    "MyRG"                                                                                                                                                                                                                                                                                                                                                        |
| Örnek Kimliği        | Kaynak tanımlayıcısı. Tanımlayıcı, kaynağı oluştururken belirttiğiniz adı içerir. VM için örnek kimliği Subscriptionıd, ResourceGroupName ve VMName içerir (veya ölçek kümesi kullanım için ad ölçek kümesi) kullanın.                                                                                                                                                                                                                                                                                                                                                                                                                    | "/ abonelikleri/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx / resourceGroups/MyRG/providers/Microsoft.Compute/virtualMachines/MyVM1"<br><br>or<br><br>"/ abonelikleri/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx / resourceGroups/MyRG/providers/Microsoft.Compute/virtualMachineScaleSets/MyVMSS1"                                                                                           |
| Etiketler               | Etiket kaynağa atayın. Fatura Kayıtları gruplandırmak için etiketler kullanın. Bilgi edinmek için nasıl [sanal makinelerinizi etiketi.](tag.md) Bu yalnızca Resource Manager VM'ler için kullanılabilir.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                | "{"myDepartment":"RD","myUser":"myName"}"                                                                                                                                                                                                                                                                                                                        |
| Ek Bilgi    | Hizmete özgü meta veriler. VM'ler için aşağıdaki ek bilgileri alanını doldurun: <ul><li>Çalıştırdığınız görüntü türüne özgü resim. Desteklenen dizelerin resim türleri altında tam listesini bulabilirsiniz.</li><li>Hizmet türü: dağıttığınız boyutu.</li><li>VMName: VM adıdır. Bu, yalnızca VM ölçek kümesi için doldurulur. Gerekirse VM'ler ölçek için VM adı ayarlayın, yukarıdaki örnek kimliği dizesi içinde bulabilirsiniz.</li><li>UsageType: Bu kullanım bu temsil eden türünü belirtir.<ul><li>ComputeHR Standard_D1_v2 gibi temel VM için işlem saati kullanımdır.</li><li>VM Microsoft R Server gibi premium yazılım kullanıyorsanız ComputeHR_SW premium yazılım ücret ' dir.</li></ul></li></ul>    | Sanal makineler {"ImageType": "Kurallı", "ServiceType": "Standard_DS1_v2", "VMName": "", "UsageType": "ComputeHR"}<br><br>Sanal makine ölçek kümeleri {"ImageType": "Kurallı", "ServiceType": "Standard_DS1_v2", "VMName": "myVM1", "UsageType": "ComputeHR"}<br><br>Premium yazılım {"ImageType": "","ServiceType": "Standard_DS1_v2", "VMName": "", "UsageType": "ComputeHR_SW"} |

## <a name="image-type"></a>Görüntü Türü
İçin bazı görüntüleri Azure galerisinde görüntü türü ek bilgi alanında doldurulur. Bu anlamak ve hangi kullanıcılar kendi sanal makinede dağıttıysanız izlemesine olanak sağlar. Dağıttığınız görüntüyü temel alarak bu alandaki doldurulan değerleri şunlardır:
  - BitRock 
  - Canonical 
  - FreeBSD 
  - Açık mantığı 
  - Oracle 
  - SAP için SLES 
  - SQL Server 14 Preview Windows Server 2012 R2 Önizleme 
  - SUSE
  - SUSE Premium
  - StorSimple Cloud Appliance 
  - Red Hat
  - Red Hat SAP Business uygulamaları için     
  - SAP HANA için Red Hat 
  - Windows İstemcisi KLG 
  - Windows Server KLG 
  - Windows Server Önizleme 

## <a name="service-type"></a>Hizmet Türü
Ek bilgi alan hizmet türü alanında dağıttığınız tam VM boyutu karşılık gelir. Premium depolama Vm'leri (SSD tabanlı) ve premium olmayan depolama Vm'leri (HDD tabanlı) aynı fiyatlandırılır. Standart gibi bir SSD tabanlı boyutu dağıtırsanız\_DS2\_v2, SSD olmayan boyutu bakın (' standart\_D2\_v2 VM') ölçer alt kategori sütunu ve SSD boyutu (' standart\_DS2\_ v2') ek bilgileri alanında.

## <a name="region-names"></a>Bölge adları
Azure Kaynak Yöneticisi'nde kullanılan bölge adı kullanım ayrıntılarını kaynak konumu alanında doldurulur bölge adı değişir. Bölge değerleri arasında bir eşleme şöyledir:

|    **Resource Manager bölge adı**       |    **Kaynak konumda kullanım ayrıntıları**    |
|--------------------------|------------------------------------------|
|    australiaeast         |    AU Doğu                               |
|    australiasoutheast    |    AU Güneydoğu                          |
|    brazilsouth           |    BR Güney                              |
|    CanadaCentral         |    Kanada Orta                            |
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
|    USGovArizona          |    ABD hükümeti Arizona                         |
|    usgoviowa             |    USGov Iowa                            |
|    USGovTexas            |    ABD hükümeti Texas                           |
|    usgovvirginia         |    USGov Virginia                        |
|    westcentralus         |    ABD Batı Orta                       |
|    westeurope            |    Batı Avrupa                           |
|    WestIndia             |    IN Batı                               |
|    westus                |    Batı ABD                               |
|    westus2               |    ABD Batı 2                             |


## <a name="virtual-machine-usage-faq"></a>Sanal makine kullanımı ile ilgili SSS
### <a name="what-resources-are-charged-when-deploying-a-vm"></a>Hangi kaynaklara VM dağıtırken ücretlendirilen?    
VM için kendisini, VM, VM ile ilişkili depolama account\managed disk üzerinde çalışan herhangi bir premium yazılım maliyetleri VM'ler edinmeli ve ağ bant genişliği VM'den aktarır.
### <a name="how-can-i-tell-if-a-vm-is-using-azure-hybrid-benefit-in-the-usage-csv"></a>Bir VM kullanım CSV'ye Azure karma avantajı kullanıp kullanmadığını nasıl anlayabilirim?
Kullanarak dağıtırsanız [Azure karma avantajı](https://azure.microsoft.com/pricing/hybrid-benefit/), kendi lisansını buluta getiren beri Windows olmayan VM oranı ücretlendirilirsiniz. Ya da sahip oldukları için hangi kaynak yöneticisi Vm'leri Azure karma avantajı çalıştığında ayırt faturanızı, "Windows\_Server KLG" veya "Windows\_istemci KLG" ImageType sütun.
### <a name="how-are-basic-vs-standard-vm-types-differentiated-in-the-usage-csv"></a>Temel vs şeklini. Standart VM türler kullanım CSV'ye Ayrıştırılan?
Temel ve standart A-Series VM'ler sunulur. Ölçer alt kategorisinde temel bir VM dağıtırsanız "Temel" dizesi içeriyor Standart bir A-Series VM dağıtırsanız, standart varsayılan olduğundan VM boyutu "A1 VM" görünür. Temel ve standart arasındaki farklar hakkında daha fazla bilgi için bkz: [Fiyatlandırma sayfasında](https://azure.microsoft.com/pricing/details/virtual-machines/).
### <a name="what-are-extrasmall-small-medium-large-and-extralarge-sizes"></a>Çok küçük, küçük, Orta, büyük ve çok boyutları nelerdir?
Standart eski adlarını extraSmall - ExtraLarge\_A0 – standart\_A4. Klasik VM kullanım kayıtlarında kullanılan bu boyutları dağıttıysanız, bu kural görebilirsiniz.
### <a name="what-is-the-difference-between-meter-region-and-resource-location"></a>Ölçer bölge ve kaynak konumu arasındaki fark nedir?
Ölçer bölge ölçer ile ilişkilidir. Tüm bölgeler için bir fiyat kullanan bazı Azure Hizmetleri için ölçüm Bölge alanı boş olabilir. Ancak, sanal makineleri fiyatlar bölge başına sanal makineler için ayrılmış olduğundan, bu alan doldurulur. Benzer şekilde, sanal makineler için kaynak konumu, VM dağıtıldığı konumdur. Her iki alan Azure bölgesinde aynıdır, bölge adı için farklı bir dize kuralı olabilir ancak.
### <a name="why-is-the-imagetype-value-blank-in-the-additional-info-field"></a>Neden ek bilgi alanında ImageType değer boştur?
ImageType alanın yalnızca bir alt kümesi görüntüleri için doldurulur. Yukarıdaki görüntülerden birini dağıtmadıysanız ImageType boştur.
### <a name="why-is-the-vmname-blank-in-the-additional-info"></a>Neden VMName ek bilgi boş mi?
VMName yalnızca ek bilgileri alanında VM'ler için bir ölçek kümesinde doldurulur. Ölçek dışı VM'ler kümesi için InstanceId alan VM adını içerir.
### <a name="what-does-computehr-mean-in-the-usagetype-field-in-the-additional-info"></a>Ek bilgi UsageType alanında ne ComputeHR anlama geliyor?
İşlem, temel alınan infrasturcture maliyet kullanım olayı temsil eden saat ComputeHR anlamına gelir. UsageType ComputeHR ise\_SW, kullanım olayı VM için premium yazılım ücret temsil eder.
### <a name="how-do-i-know-if-i-am-charged-for-premium-software"></a>I premium yazılım için ücret olmadığını nasıl anlayabilirim?
Hangi VM görüntüsü en iyi keşfetme ihtiyaçlarınıza uygun olduğunda, kullanıma mutlaka [Azure Marketi](https://azuremarketplace.microsoft.com/marketplace/apps/category/compute). Görüntü yazılım plan oranı vardır. "Serbest" oranını görürseniz, yazılım için ek bir maliyet yoktur. 
### <a name="what-is-the-difference-between-microsoftclassiccompute-and-microsoftcompute-in-the-consumed-service"></a>Kullanılan hizmet Microsoft.ClassicCompute ve Microsoft.Compute arasındaki fark nedir?
Microsoft.ClassicCompute Azure Service Manager aracılığıyla dağıtılan Klasik kaynakları temsil eder. Resource Manager aracılığıyla dağıtırsanız, Microsoft.Compute tüketilen hizmetinde doldurulur. Daha fazla bilgi edinmek [Azure dağıtım modelleri](/azure-resource-manager/resource-manager-deployment-model.md).
### <a name="why-is-the-instanceid-field-blank-for-my-virtual-machine-usage"></a>Neden InstanceId alan my sanal makine kullanımı için boş olur?
Klasik dağıtım modeli aracılığıyla dağıtırsanız, InstanceId dizesi kullanılabilir değil.
### <a name="why-are-the-tags-for-my-vms-not-flowing-to-the-usage-details"></a>Etiketler için neden Vm'lerimin kullanım ayrıntıları için akan değil?
Etiketleri yalnızca, kullanım CSV Resource Manager VM'ler için yalnızca akış. Klasik kaynak etiketleri kullanım ayrıntıları kullanılabilir değil.
### <a name="how-can-the-consumed-quantity-be-more-than-24-hours-one-day"></a>Nasıl Tüketilen miktarın 24 saatten uzun bir gün olabilir mi?
Klasik modeli kaynakları için fatura bulut hizmet düzeyinde toplanır. Aynı fatura ölçer kullanan bir bulut hizmetinde birden fazla VM varsa, kullanımınızı birlikte toplanır. Bu toplama uygulanmaz şekilde Resource Manager aracılığıyla dağıtılan VM'ler VM düzeyinde faturalandırılır.
### <a name="why-is-pricing-not-available-for-dsfsgsls-sizes-on-the-pricing-page"></a>Fiyatlandırma sayfasında kullanılabilir DS/FS/GS/LS boyutları için fiyatlandırma değil neden?
Premium depolama yeteneğine sahip sanal makineleri premium olmayan depolama aynı hızda faturalandırılır yeteneğine sahip sanal makineleri. Yalnızca depolama maliyetleriniz farklılık gösterir. Ziyaret [depolama fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/storage/unmanaged-disks/) daha fazla bilgi için.

## <a name="next-steps"></a>Sonraki adımlar
Kullanım ayrıntıları hakkında daha fazla bilgi için bkz: [Microsoft Azure için faturanızı anlamak.](/billing/billing-understand-your-bill.md)

