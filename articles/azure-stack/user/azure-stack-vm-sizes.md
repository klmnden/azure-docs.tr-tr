---
title: Azure yığınında desteklenen sanal makine boyutlarını | Microsoft Docs
description: Azure yığınında desteklenen VM boyutları için başvuru.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/17/2018
ms.author: brenduns
ms.openlocfilehash: b84744a52426123f2f3e7dd4c14419fef8b779cc
ms.sourcegitcommit: 96089449d17548263691d40e4f1e8f9557561197
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2018
---
# <a name="virtual-machine-sizes-supported-in-azure-stack"></a>Azure yığınında desteklenen sanal makine boyutları

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

Bu makalede Azure yığınında kullanılabilir sanal makine (VM) boyutları listeler.

## <a name="general-purpose"></a>Genel amaçlı

Genel amaçlı VM boyutları dengeli bir CPU ve bellek oranı sağlar. Bunlar için geliştirme, test ve küçük ve orta veritabanları ve düşük Orta trafiği web sunucuları için kullanılır.

### <a name="basic-a"></a>Temel A
|Boyutu - Size\Name |Sanal işlemci     |Bellek | En fazla geçici disk boyutu | Max OS disk verimlilik: (IOPS) | En büyük geçici depolama verimliliğini (IOPS) | En fazla veri diski işleme (IOPS) | En fazla NIC |    
|-----------------|-----|---------|---------|-----|------|-----------|----|
|**A0\Basic_A0**  |1    |768 MB   | 20 GB   |300  | 300  |1 / 1 x 300  |1   |
|**A1\Basic_A1**  |1    |1,75 GB  | 40 GB   |300  | 300  |2 / 2 x 300  |1   |
|**A2\Basic_A2**  |2    |3,5 GB   | 60 GB   |300  | 300  |4 / 4 x 300  |1   |
|**A3\Basic_A3**  |4    |7 GB     | 120 GB  |300  | 300  |8 / 8 x 300  |1   |
|**A4\Basic_A4**  |8    |14 GB    | 240 GB  |300  | 300  |6 / 16 X 300 |1   |

### <a name="standard-a"></a>Standart bir 
|Boyut     |Sanal işlemci     |Bellek (GiB) | Geçici depolama (GiB)  | Max OS disk verimliliği (IOPS) | En büyük geçici depolama verimliliğini (IOPS) | En büyük veri diskleri / üretilen işi (IOPS) | NIC max / ağ bant genişliği (Mbps) bekleniyor |    
|----------------|--|------|----|----|----|-------|---------|
|**Standard_A0** |1 |0,768 |20  |500 |500 |1x500  |1 / 100  |
|**Standard_A1** |1 |1,75  |70  |500 |500 |2x500  |1 / 500  |
|**Standard_A2** |2 |3,5   |135 |500 |500 |4x500  |1 / 500  |
|**Standard_A3** |4 |7     |285 |500 |500 |8x500  |2 / 1000 |
|**Standard_A4** |8 |14    |605 |500 |500 |16x500 |4 / 2000 |
|**Standard_A5** |2 |14    |135 |500 |500 |4x500  |2 / 500  |
|**Standard_A6** |4 |28    |285 |500 |500 |8x500  |2 / 1000 |
|**Standard_A7** |8 |56    |605 |500 |500 |16x500 |4 / 2000 |

### <a name="av2-series"></a>Av2 Serisi
*Azure yığın 1804 veya sonraki bir sürümü gerektiriyor*

|Boyut     |Sanal işlemci     |Bellek (GiB) | Geçici depolama (GiB)  | Max OS disk verimliliği (IOPS) | En büyük geçici depolama verimliliğini (IOPS) | En büyük veri diskleri / üretilen işi (IOPS) | NIC max / ağ bant genişliği (Mbps) bekleniyor |
|-----------------|----|----|-----|-----|------|--------------|---------|
|**Standard_A1_v2**  |1   |2   |10   |500 |1000  |2/2x500   |2 / 250  |
|**Standard_A2_v2**  |2   |4   |20   |500 |2000  |4/4x500   |2 / 500  |
|**Standard_A4v2**   |4   |8   |40   |500 |4000  |8/8x500   |4 / 1000 |
|**Standard_A8_v2**  |8   |16  |80   |500 |8000  |16/16x500 |8 / 2000 |
|**Standard_A2m_v2** |2   |16  |20   |500 |2000  |4/4x500   |2 / 500  |
|**Standard_A4m_v2** |4   |32  |40   |500 |4000  |8/8x500   |4 / 1000 |
|**Standard_A8m_v2** |8   |64  |80   |500 |8000  |16/16x500 |8 / 2000 |

### <a name="d-series"></a>D Serisi
|Boyut     |Sanal işlemci     |Bellek (GiB) | Geçici depolama (GiB)  | Max OS disk verimliliği (IOPS) | En büyük geçici depolama verimliliğini (IOPS) | En büyük veri diskleri / üretilen işi (IOPS) | NIC max / ağ bant genişliği (Mbps) bekleniyor |
|----------------|----|----|-----|----|------|------------|---------|
|**Standard_D1** |1   |3,5 |50   |500 |3000  |4/4x500   |1 / 500  |
|**Standart D2** |2   |7   |100  |500 |6000  |8/8x500   |2 / 1000 |
|**Standart D3** |4   |14  |200  |500 |12000 |16/16x500 |4 / 2000 |
|**Standard_D4** |8   |28  |400  |500 |24000 |32/32x500 |8 / 4000 |


### <a name="ds-series"></a>DS serisi
|Boyut     |Sanal işlemci     |Bellek (GiB) | Geçici depolama (GiB)  | Max OS disk verimliliği (IOPS) | En büyük geçici depolama verimliliğini (IOPS) | En büyük veri diskleri / üretilen işi (IOPS) | NIC max / ağ bant genişliği (Mbps) bekleniyor |
|-----------------|----|----|-----|-----|------|-------------|---------|
|**Standard_DS1** |1   |3,5 |7    |1000 |4000  |4 / 4 x 2300   |1 / 500  |
|**Standard_DS2** |2   |7   |14   |1000 |8000  |8 / 8 x 2300   |2 / 1000 |
|**Standard_DS3** |4   |14  |28   |1000 |16000 |16 / 16 x 2300 |4 / 2000 |
|**Standard_DS4** |8   |28  |56   |1000 |32000 |32 / 32 x 2300 |8 / 4000 |

### <a name="dv2-series"></a>Dv2 Serisi
|Boyut     |Sanal işlemci     |Bellek (GiB) | Geçici depolama (GiB)  | Max OS disk verimliliği (IOPS) | En büyük geçici depolama verimliliğini (IOPS) | En büyük veri diskleri / üretilen işi (IOPS) | NIC max / ağ bant genişliği (Mbps) bekleniyor |
|-------------------|----|----|-----|----|------|------------|---------|
|**Standard_D1_v2** |1   |3,5 |50   |500 |3000  |4/4x500   |1 / 500  |
|**Standard_D2_v2** |2   |7   |100  |500 |6000  |8/8x500   |2 / 1000 |
|**Standard_D3_v2** |4   |14  |200  |500 |12000 |16/16x500 |4 / 2000 |
|**Standard_D4_v2** |8   |28  |400  |500 |24000 |32/32x500 |8 / 4000 |
|**Standard_D5_v2** |16  |56  |800  |500 |48000 |64 / 64 x 500 |8 / 8000 |

### <a name="dsv2-series"></a>DSv2 serisi
|Boyut     |Sanal işlemci     |Bellek (GiB) | Geçici depolama (GiB)  | Max OS disk verimliliği (IOPS) | En büyük geçici depolama verimliliğini (IOPS) | En büyük veri diskleri / üretilen işi (IOPS) | NIC max / ağ bant genişliği (Mbps) bekleniyor |
|--------------------|----|----|----|-----|------|-------------|---------|
|**Standard_DS1_v2** |1   |3,5 |7   |1000 |4000  |4 / 4 x 2300   |1 / 750  |
|**Standard_DS2_v2** |2   |7   |14  |1000 |8000  |8 / 8 x 2300   |2 / 1500 |
|**Standard_DS3_v2** |4   |14  |28  |1000 |16000 |16 / 16 x 2300 |4 / 3000 |
|**Standard_DS4_v2** |8   |28  |56  |1000 |32000 |32 / 32 x 2300 |8 / 6000 |
|**Standard_DS5_v2** |16  |56  |112 |1000 |64000 |64 / 64 x 2300 |8 / 10000 |


## <a name="compute-optimized"></a>İşlem için iyileştirilmiş
### <a name="f-series"></a>F Serisi
*Azure yığın 1804 veya sonraki bir sürümü gerektiriyor*

|Boyut     |Sanal işlemci     |Bellek (GiB) | Geçici depolama (GiB)  | Max OS disk verimliliği (IOPS) | En büyük geçici depolama verimliliğini (IOPS) | En büyük veri diskleri / üretilen işi (IOPS) | NIC max / ağ bant genişliği (Mbps) bekleniyor |
|-----------------|----|----|-----|----|------|------------|---------|
|**Standard_F1**  |1   |2   |16   |500 |3000  |4 / 4 x 400   |2 / 750  |
|**Standard_F2**  |2   |4   |32   |500 |6000  |8/8x500   |2 / 1500 |
|**Standard_F4**  |4   |8   |64   |500 |12000 |16/16x500 |4 / 3000 |
|**Standard_F8**  |8   |16  |128  |500 |24000 |32/32x500 |8 / 6000 |
|**Standard_F16** |16  |32  |256  |500 |48000 |64 / 64 x 500 |8 / 6000 12000  |


### <a name="fs-series"></a>Fs serisi
*Azure yığın 1804 veya sonraki bir sürümü gerektiriyor*  

|Boyut     |Sanal işlemci     |Bellek (GiB) | Geçici depolama (GiB)  | Max OS disk verimliliği (IOPS) | En büyük geçici depolama verimliliğini (IOPS) | En büyük veri diskleri / üretilen işi (IOPS) | NIC max / ağ bant genişliği (Mbps) bekleniyor |
|------------------|----|----|----|-----|------|-------------|---------|
|**Standard_F1s**  |1   |2   |4   |1000 |4000  |4 / 4 x 2300   |2 / 750  |
|**Standard_F2s**  |2   |4   |8   |1000 |8000  |8 / 8 x 2300   |2 / 1500 |
|**Standard_F4s**  |4   |8   |16  |1000 |16000 |16 / 16 x 2300 |4 / 3000 |
|**Standard_F8s**  |8   |16  |32  |1000 |32000 |32 / 32 x 2300 |8 / 6000 |
|**Standard_F16s** |16  |32  |64  |1000 |64000 |64 / 64 x 2300 |8 / 6000 12000  |


### <a name="fsv2-series"></a>Fsv2-serisi
*Azure yığın 1804 veya sonraki bir sürümü gerektiriyor* 

|Boyut     |Sanal işlemci     |Bellek (GiB) | Geçici depolama (GiB)  | Max OS disk verimliliği (IOPS) | En büyük geçici depolama verimliliğini (IOPS) | En büyük veri diskleri / üretilen işi (IOPS) | NIC max / ağ bant genişliği (Mbps) bekleniyor |
|---------------------|----|----|-----|-----|-------|--------------|---------|
|**Standard_F2s_v2**  |2   |4   |16   |1000 |4000   |4 / 4 x 2300    |Orta |
|**Standard_F4s_v2**  |4   |8   |32   |1000 |8000   |8 / 8 x 2300    |Orta |
|**Standard_F8s_v2**  |8   |16  |64   |1000 |16000  |16 / 16 x 2300  |Yüksek     |
|**Standard_F16s_v2** |16  |32  |128  |1000 |32000  |32 / 32 x 2300  |Yüksek     |
|**Standard_F32s_v2** |32  |64  |256  |1000 |64000  |32 / 32 x 2300  |Yüksek  |
|**Standard_F64s_v2** |64  |128 |512  |1000 |128000 |32 / 32 x 2300  |Çok yüksek  |


## <a name="memory-optimized"></a>Bellek için iyileştirilmiş

Bellek ilişkisel veritabanı sunucuları, Orta ve büyük önbellekler ve bellek içi analytics için en iyi duruma getirilmiş VM boyutları için tasarlanmış bir yüksek bellek CPU oranı sağlar.

### <a name="mo-d"></a>D-serisi
|Boyut     |Sanal işlemci     |Bellek (GiB) | Geçici depolama (GiB)  | Max OS disk verimliliği (IOPS) | En büyük geçici depolama verimliliğini (IOPS) | En büyük veri diskleri / üretilen işi (IOPS) | NIC max / ağ bant genişliği (Mbps) bekleniyor |
|------------------|---|----|----|--------|------|------------|---------|
|**Standard_D11**  |2  |14  |100 |500     |6000  |8/8x500   |2 / 1000 |
|**Standart d12**  |4  |28  |200 |500     |12000 |16/16x500 |4 / 2000 |
|**Standard_D13**  |8  |56  |400 |500     |24000 |32/32x500 |8 / 4000 |
|**Standart D14**  |16 |112 |800 |500     |48000 |64 / 64 x 500 |8 / 8000 |

### <a name="mo-ds"></a>DS serisi
|Boyut     |Sanal işlemci     |Bellek (GiB) | Geçici depolama (GiB)  | Max OS disk verimliliği (IOPS) | En büyük geçici depolama verimliliğini (IOPS) | En büyük veri diskleri / üretilen işi (IOPS) | NIC max / ağ bant genişliği (Mbps) bekleniyor |
|-------------------|---|----|----|--------|------|-------------|---------|
|**Standard_DS11**  |2  |14  |28  |1000    |8000  |8 / 8 x 2300   |2 / 1000 |
|**Standard_DS12**  |4  |28  |56  |1000    |12000 |16 / 16 x 2300 |4 / 2000 |
|**Standard_DS13**  |8  |56  |112 |1000    |32000 |32 / 32 x 2300 |8 / 4000 |
|**Standard_DS14**  |16 |112 |224 |1000    |64000 |64 / 64 x 2300 |8 / 8000 |

### <a name="mo-dv2"></a>Dv2-serisi
|Boyut     |Sanal işlemci     |Bellek (GiB) | Geçici depolama (GiB)  | Max OS disk verimliliği (IOPS) | En büyük geçici depolama verimliliğini (IOPS) | En büyük veri diskleri / üretilen işi (IOPS) | NIC max / ağ bant genişliği (Mbps) bekleniyor |
|--------------------|----|----|-----|----|-------|-------------|---------|
|**Standard_D11_v2** |2   |14  |100  |500 |6000   |8/8x500    |2 / 1500 |
|**Standard_D12_v2** |4   |28  |200  |500 |12000  |16/16x500  |4 / 3000 |
|**Standard_D13_v2** |8   |56  |400  |500 |24000  |32/32x500  |8 / 6000 |
|**Standard_D14_v2** |16  |112 |800  |500 |48000  |64 / 64 x 500  |8 / 10000 |


### <a name="mo-dsv2"></a>DSv2 serisi
|Boyut     |Sanal işlemci     |Bellek (GiB) | Geçici depolama (GiB)  | Max OS disk verimliliği (IOPS) | En büyük geçici depolama verimliliğini (IOPS) | En büyük veri diskleri / üretilen işi (IOPS) | NIC max / ağ bant genişliği (Mbps) bekleniyor |
|---------------------|----|----|-----|-----|-------|--------------|---------|
|**Standard_DS11_v2** |2   |14  |28   |1000 |8000   |8 / 8 x 2300    |2 / 1500 |
|**Standard_DS12_v2** |4   |28  |56   |1000 |16000  |16 / 16 x 2300  |4 / 3000 |
|**Standard_DS13_v2** |8   |56  |112  |1000 |32000  |32 / 32 x 2300  |8 / 6000 |
|**Standard_DS14_v2** |16  |112 |224  |1000 |64000  |64 / 64 x 2300  |8 / 10000 |


## <a name="next-steps"></a>Sonraki adımlar

[Sanal makineler Azure yığınında dikkate alınacak noktalar](azure-stack-vm-considerations.md)
