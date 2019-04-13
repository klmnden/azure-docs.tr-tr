---
author: yashar
ms.author: banders
ms.service: virtual-machines-windows
ms.topic: include
ms.date: 11-20-2018
ms.openlocfilehash: 05820cc5f7b7d61d83f73ea5b62b05f8712e0997
ms.sourcegitcommit: 031e4165a1767c00bb5365ce9b2a189c8b69d4c0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2019
ms.locfileid: "59551649"
---
# <a name="virtual-machine-size-flexibility-with-reserved-vm-instances"></a>Ayrılmış VM örnekleri ile sanal makine boyutu esnekliği

Örneği için boyutu esneklik için iyileştirilmiş bir ayrılmış sanal makine örneğiyle aynı boyutu seri grubu içindeki sanal makineler (VM) boyutlarına satın aldığınız ayırma uygulayabilirsiniz. Örneğin, gibi Standard_DS5_v2, DSv2 serisi tabloda listelenen bir VM boyutu için bir ayırma satın alırsanız, aynı tabloda listelenen diğer dört boyutları ayırma indirimi uygulayabilirsiniz:

- Standard_DS1_v2
- Standard_DS2_v2
- Standard_DS3_v2
- Standard_DS4_v2

Ancak, bu ayırma indirimi DSv2 serisi, yüksek bellek tabloda nedir gibi farklı tablolarda listelenen VM boyutları için geçerli değildir: Standard_DS11_v2, Standard_DS12_v2 ve benzeri.

Boyutu seri grubu içinde ayırma indirimi uygulandığı VM'ler bir ayırma satın aldığınızda tarifeniz seçtiğiniz VM boyutuna bağlıdır. Ayrıca, olan Vm'leri boyutlarına bağlıdır çalışıyor. Aşağıdaki tablolarda listelenenler oranı sütun, o grup içindeki her VM boyutu için göreli ayak izini karşılaştırır. Ayırma indirimi Vm'lere, uygulanması hakkında hesaplamak için oran değerine sahip kullanım çalışıyor.

## <a name="examples"></a>Örnekler

Aşağıdaki örnekler DSv2 serisi tabloda boyutları ve oranlarını kullanın.

 Oran veya diğer boyutlarda serisi ile karşılaştırıldığında göreli ayak izini 8 olduğu Standard_DS4_v2 boyutu ile ayrılmış VM örneği satın aldığınız.

- Senaryo 1: Sekiz çalıştırma Standard_DS1_v2 Vm'leri 1 oranında ile boyutlandırılmış. Ayırma indirimini bu VM'lerin tüm sekiz için geçerlidir.
- Senaryo 2: İki çalıştırma Standard_DS2_v2 Vm'leri 2 her oranı ile boyutlandırılmış. Ayrıca bir Standard_DS3_v2 4 oranı ile VM boyutu. Toplam boyut 2 + 2 + 4 = 8. Bu nedenle, ayırma indirimini üçünü de bu sanal makineler için geçerlidir.
- Senaryo 3: Bir Standard_DS5_v2 16 oranında ile çalıştırın. Ayırma indirimini bu VM'nin yarım işlem maliyeti için geçerlidir.

Ayrılmış VM örneği satın aldığınızda, hangi boyutları aynı boyutu serisi grubunda olan aşağıdaki bölümlerde Göster boyutu esneklik örneği için en iyi duruma getirilmiş.

## <a name="b-series"></a>B serisi

| Boyut | Oranı|
|---|---|
| Standard_B1s | 1 |
|Standard_B2s|4|

Daha fazla bilgi için [B serisi seri aktarıma uygun sanal makine boyutları](../articles/virtual-machines/windows/b-series-burstable.md).

## <a name="b-series-high-memory"></a>B serisi yüksek bellek

| Boyut | Oranı|
|---|---|
| Standard_B1ms |1|
|Standard_B2ms|4|
|Standard_B4ms|8|
|Standard_B8ms|16|

Daha fazla bilgi için [B serisi seri aktarıma uygun sanal makine boyutları](../articles/virtual-machines/windows/b-series-burstable.md).

## <a name="d-series"></a>D Serisi

| Boyut | Oranı|
|---|---|
| Standard_D1|1|
|Standard_D2|2|
|Standard_D3|4|
|Standard_D4|8|

Daha fazla bilgi için [önceki nesil sanal makine boyutları](../articles/virtual-machines/windows/sizes-previous-gen.md).

## <a name="d-series-high-memory"></a>D serisi yüksek bellek

| Boyut | Oranı|
|---|---|
| Standard_D11|1|
|Standard_D12|2|
|Standard_D13|4|
|Standard_D14|8|

Daha fazla bilgi için [önceki nesil sanal makine boyutları](../articles/virtual-machines/windows/sizes-previous-gen.md).

## <a name="ds-series"></a>DS serisi

| Boyut | Oranı|
|---|---|
|Standard_DS1|1|
|Standard_DS2|2|
|Standard_DS3|4|
|Standard_DS4|8|

Daha fazla bilgi için [önceki nesil sanal makine boyutları](../articles/virtual-machines/windows/sizes-previous-gen.md).

## <a name="ds-series-high-memory"></a>DS serisi yüksek bellek

| Boyut | Oranı|
|---|---|
|Standard_DS11|1|
|Standard_DS12|2|
|Standard_DS13|4|
|Standard_DS14|8|

Daha fazla bilgi için [önceki nesil sanal makine boyutları](../articles/virtual-machines/windows/sizes-previous-gen.md).

## <a name="dsv2-series"></a>DSv2 serisi

| Boyut | Oranı|
|---|---|
|Standard_DS1_v2|1|
|Standard_DS2_v2|2|
|Standard_DS3_v2|4|
|Standard_DS4_v2|8|
|Standard_DS5_v2|16|

Daha fazla bilgi için [önceki nesil sanal makine boyutları](../articles/virtual-machines/windows/sizes-previous-gen.md).

## <a name="dsv2-series-high-memory"></a>Yüksek bellek DSv2 serisi

| Boyut | Oranı|
|---|---|
|Standard_DS11_v2|1|
|Standard_DS11-1_v2|1|
|Standard_DS12_v2|2|
|Standard_DS12-1_v2|2|
|Standard_DS12-2_v2|2|
|Standard_DS13_v2|4|
|Standard_DS13-2_v2|4|
|Standard_DS13-4_v2|4|
|Standard_DS14_v2|8|
|Standard_DS14-4_v2|8|
|Standard_DS14-8_v2|8|
|Standard_DS15_v2|10|

Daha fazla bilgi için [önceki nesil sanal makine boyutları](../articles/virtual-machines/windows/sizes-previous-gen.md).

## <a name="dsv3-series"></a>DSv3 serisi

| Boyut | Oranı|
|---|---|
|Standard_D2s_v3|1|
|Standard_D4s_v3|2|
|Standard_D8s_v3|4|
|Standard_D16s_v3|8|
|Standard_D32s_v3|16|
|Standard_D64s_v3|32|

Daha fazla bilgi için [genel amaçlı sanal makine boyutları](../articles/virtual-machines/windows/sizes-general.md#dsv3-series-1).

## <a name="dv2-series"></a>Dv2 Serisi

| Boyut | Oranı|
|---|---|
|Standard_D1_v2|1|
|Standard_D2_v2|2|
|Standard_D3_v2|4|
|Standard_D4_v2|8|
|Standard_D5_v2|16|

Daha fazla bilgi için [önceki nesil sanal makine boyutları](../articles/virtual-machines/windows/sizes-previous-gen.md).

## <a name="dv2-series-high-memory"></a>Dv2 serisi yüksek bellek

| Boyut | Oranı|
|---|---|
|Standard_D11_v2|1|
|Standard_D12_v2|2|
|Standard_D13_v2|4|
|Standard_D14_v2|8|
|Standard_D15_v2|10|

Daha fazla bilgi için [önceki nesil sanal makine boyutları](../articles/virtual-machines/windows/sizes-previous-gen.md).

## <a name="dv3-series"></a>Dv3 serisi

| Boyut | Oranı|
|---|---|
| Standard_D2_v3|1|
|Standard_D4_v3|2|
|Standard_D8_v3|4|
|Standard_D16_v3|8|
|Standard_D32_v3|16|
|Standard_D64_v3|32|

Daha fazla bilgi için [genel amaçlı sanal makine boyutları](../articles/virtual-machines/windows/sizes-general.md#dv3-series-1).

## <a name="esv3-series"></a>ESv3 serisi

| Boyut | Oranı|
|---|---|
|Standard_E2s_v3|1|
|Standard_E4s_v3|2|
|Standard_E4-2s_v3|2|
|Standard_E8s_v3|4|
|Standard_E8-2s_v3|4|
|Standard_E8-4s_v3|4|
|Standard_E16s_v3|8|
|Standard_E16-4s_v3|8|
|Standard_E16-8s_v3|8|
|Standard_E20s_v3|10|
|Standard_E32s_v3|16|
|Standard_E32-8s_v3|16|
|Standard_E32-16s_v3|16|
|Standard_E64s_v3|28.8|
|Standard_E64-16s_v3|28.8|
|Standard_E64-32s_v3|28.8|

Daha fazla bilgi için [bellek için iyileştirilmiş sanal makine boyutları](../articles/virtual-machines/windows/sizes-memory.md#esv3-series).

## <a name="ev3-series"></a>Ev3 serisi

| Boyut | Oranı|
|---|---|
| Standard_E2_v3|1|
|Standard_E4_v3|2|
|Standard_E8_v3|4|
|Standard_E16_v3|8|
|Standard_E20_v3|10|
|Standard_E32_v3|16|
|Standard_E64_v3|32|

Daha fazla bilgi için [bellek için iyileştirilmiş sanal makine boyutları](../articles/virtual-machines/windows/sizes-memory.md#ev3-series).

## <a name="f-series"></a>F Serisi

| Boyut | Oranı|
|---|---|
| Standard_F1|1|
|Standard_F2|2|
|Standard_F4|4|
|Standard_F8|8|
Standard_F16|16|

Daha fazla bilgi için [önceki nesil sanal makine boyutları](../articles/virtual-machines/windows/sizes-previous-gen.md).

## <a name="fs-series"></a>FS serisi

| Boyut | Oranı|
|---|---|
| Standard_F1s|1|
|Standard_F2s|2|
|Standard_F4s|4|
|Standard_F8s|8|
|Standard_F16s|16|

Daha fazla bilgi için [önceki nesil sanal makine boyutları](../articles/virtual-machines/windows/sizes-previous-gen.md).

## <a name="fsv2-series"></a>Fsv2-serisi

| Boyut | Oranı|
|---|---|
|Standard_F2s_v2|1|
|Standard_F4s_v2|2|
|Standard_F8s_v2|4|
|Standard_F16s_v2|8|
|Standard_F32s_v2|16|
|Standard_F64s_v2|32|
|Standard_F72s_v2|36|

Daha fazla bilgi için [işlem için iyileştirilmiş sanal makine boyutları](../articles/virtual-machines/windows/sizes-compute.md#fsv2-series-1).

## <a name="h-series"></a>H Serisi

| Boyut | Oranı|
|---|---|
| Standard_H8|1|
|Standard_H16|2|

Daha fazla bilgi için [yüksek performanslı işlem VM boyutları](../articles/virtual-machines/windows/sizes-hpc.md).

## <a name="h-series-high-memory"></a>H serisi, yüksek bellek

| Boyut | Oranı|
|---|---|
| Standard_H8m|1|
|Standard_H16m|2|

Daha fazla bilgi için [yüksek performanslı işlem VM boyutları](../articles/virtual-machines/windows/sizes-hpc.md).

## <a name="ls-series"></a>Ls serisi

| Boyut | Oranı|
|---|---|
| Standart_L4s|1|
|Standart_L8s|2|
|Standart_L16s|4|
|Standart_L32s|8|

Daha fazla bilgi için [depolama için iyileştirilmiş sanal makine boyutları](../articles/virtual-machines/windows/sizes-storage.md).

## <a name="m-series"></a>M serisi

| Boyut | Oranı|
|---|---|
| İşler için standart_m64s|1|
|İşler için standart_m128s|2|

Daha fazla bilgi için [bellek için iyileştirilmiş sanal makine boyutları](../articles/virtual-machines/windows/sizes-memory.md#m-series).

## <a name="m-series-fractional"></a>M serisi kesirli

| Boyut | Oranı|
|---|---|
| İşler için Standard_M16s|1|
|Standard_M32s|2|

Daha fazla bilgi için [bellek için iyileştirilmiş sanal makine boyutları](../articles/virtual-machines/windows/sizes-memory.md#m-series).

## <a name="m-series-fractional-high-memory"></a>M serisi kesirli yüksek bellek

| Boyut | Oranı|
|---|---|
|İşler için Standard_M8ms|1|
|Standard_M8-2ms|1|
|Standard_M8-4ms|1|
|İşler için Standard_M16ms|2|
|Standard_M16-4ms|2|
|Standard_M16-8ms|2|
|Standard_M32ms|4|
|Standard_M32-8ms|4|
|Standard_M32-16ms|4|

Daha fazla bilgi için [bellek için iyileştirilmiş sanal makine boyutları](../articles/virtual-machines/windows/sizes-memory.md#m-series).

## <a name="m-series-fractional-large"></a>M serisi kesirli büyük

| Boyut | Oranı|
|---|---|
| Standard_M32ls|1|
|İşler için standart_m64ls|2|

Daha fazla bilgi için [bellek için iyileştirilmiş sanal makine boyutları](../articles/virtual-machines/windows/sizes-memory.md#m-series).

## <a name="m-series-high-memory"></a>M serisi, yüksek bellek

| Boyut | Oranı|
|---|---|
| Standard_M64ms|1|
|Standard_M64-16ms|1|
|Standard_M64-32ms|1|
|İşler için standart_m128ms|2|
|Standard_M128-32ms|2|
|Standard_M128-64ms|2|

Daha fazla bilgi için [bellek için iyileştirilmiş sanal makine boyutları](../articles/virtual-machines/windows/sizes-memory.md#m-series).

## <a name="nc-series"></a>NC serisi

| Boyut | Oranı|
|---|---|
| Standard_NC6|1|
|Standard_NC12|2|
|Standard_NC24|4|

Daha fazla bilgi için [GPU için iyileştirilmiş sanal makine boyutları](../articles/virtual-machines/windows/sizes-gpu.md).

## <a name="ncv2-series"></a>NCv2 serisi

| Boyut | Oranı|
|---|---|
| Standard_NC6s_v2|1|
|Standard_NC12s_v2|2|
|Standard_NC24s_v2|4|

Daha fazla bilgi için [GPU için iyileştirilmiş sanal makine boyutları](../articles/virtual-machines/windows//sizes-gpu.md#ncv2-series).

## <a name="ncv3-series"></a>NCv3 serisi

| Boyut | Oranı|
|---|---|
| Standard_NC6s_v3|1|
|Standard_NC12s_v3|2|
|Standard_NC24s_v3|4|

Daha fazla bilgi için [GPU için iyileştirilmiş sanal makine boyutları](../articles/virtual-machines/windows//sizes-gpu.md#ncv3-series).

## <a name="nd-series"></a>ND serisi

| Boyut | Oranı|
|---|---|
| Standard_ND6s|1|
|Standard_ND12s|2|
|Standard_ND24s|4|

Daha fazla bilgi için [GPU için iyileştirilmiş sanal makine boyutları](../articles/virtual-machines/windows//sizes-gpu.md#nd-series).

## <a name="nv-series"></a>NV serisi

| Boyut | Oranı|
|---|---|
| Standard_NV6|1|
|Standard_NV12|2|
|Standard_NV24|4|

Daha fazla bilgi için [GPU için iyileştirilmiş sanal makine boyutları](../articles/virtual-machines/windows//sizes-gpu.md#nv-series).


