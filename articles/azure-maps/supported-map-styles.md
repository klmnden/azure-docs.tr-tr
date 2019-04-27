---
title: Eşleme stilleri Azure eşlemelerinde desteklenen | Microsoft Docs
description: Azure haritalar tarafından desteklenen eşleme stilleri
author: walsehgal
ms.author: v-musehg
ms.date: 02/12/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: 76ab49c7f28260249483bf3bc4387e8cbaca13b2
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60767244"
---
# <a name="azure-maps-supported-map-styles"></a>Azure haritalar desteklenen eşleme stilleri
Azure haritalar, aşağıda açıklandığı gibi birçok farklı yerleşik eşleme stilleri destekler.

## <a name="road"></a>Yol
A **yol** haritasıdır yollar, doğal görüntüler standart bir harita ve bu özellikler için etiketlerin yanı sıra yapay özellikleri.

![Yol](./media/supported-map-styles/road.png)

**İlgili API'ler:**
* [Harita resminin](https://docs.microsoft.com/rest/api/maps/render/getmapimage)
* [Harita kutucuğunu](https://docs.microsoft.com/rest/api/maps/render/getmaptile)
* JS harita denetimi
* Android harita denetimi

## <a name="satellite"></a>Uydu 
**Uydu** stili, uydu ve hava gözünüzde bir birleşimi.

![Uydu](./media/supported-map-styles/satellite.png)

**İlgili API'ler:**
* [Uydu kutucuğu](https://docs.microsoft.com/rest/api/maps/render/getmapimagerytilepreview)
* JS harita denetimi
* Android harita denetimi

## <a name="satelliteroadlabels"></a>satellite_road_labels
Yollar ve etiketleri uydu ve hava tanımayı üzerine yayılan karma bu harita stilidir.

![satellite_road_labels](./media/supported-map-styles/satellite_road_labels.png)

**İlgili API'ler:**
* JS harita denetimi
* Android harita denetimi

## <a name="grayscaledark"></a>grayscale_dark
**Koyu gri tonlamalı** yol haritası Stili Koyu bir sürümüdür.

![gray_scale](./media/supported-map-styles/grayscale_dark.png)

**İlgili API'ler:**
* JS harita denetimi 
* Android harita denetimi

## <a name="night"></a>gece
**gece** koyu renkli yollar ve semboller yol haritası stiliyle sürümüdür.

![gece](./media/supported-map-styles/night.png)

**İlgili API'ler:**
* JS harita denetimi
* Android harita denetimi

## <a name="roadshadedrelief"></a>road_shaded_relief
**yol gölgeli Tahliye** olduğu bir Azure haritalar ana stili dağılımlarını dünya ile tamamlandı.

![Gölgeli Tahliye](./media/supported-map-styles/shaded-relief.png)

**İlgili API'ler:**
* [Harita kutucuğunu](https://docs.microsoft.com/rest/api/maps/render/getmaptile)
* JS harita denetimi
* Android harita denetimi
