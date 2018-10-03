---
title: Eşleme stilleri Azure eşlemelerinde desteklenen | Microsoft Docs
description: Azure haritalar tarafından desteklenen eşleme stilleri
author: walsehgal
ms.author: v-musehg
ms.date: 10/02/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: c8edaba8de597e3e76e760e1f5109006338a663c
ms.sourcegitcommit: 1981c65544e642958917a5ffa2b09d6b7345475d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/03/2018
ms.locfileid: "48238829"
---
# <a name="azure-maps-supported-map-styles"></a>Azure haritalar desteklenen eşleme stilleri
Azure haritalar dört farklı yerleşik eşleme stilleri destekler. Açıklamalarının stillerle aşağıda listelenmiştir.

## <a name="road"></a>Yol
A **yol** haritasıdır yollar, doğal görüntüler standart bir harita ve bu özellikler için etiketlerin yanı sıra yapay özellikleri.

![Yol](./media/supported-map-styles/road.png)

**İlgili API'ler:**
* [Harita resminin](https://docs.microsoft.com/rest/api/maps/render/getmapimage)
* [Harita kutucuğunu](https://docs.microsoft.com/rest/api/maps/render/getmaptile)
* JS harita denetimi

## <a name="satellite"></a>Uydu 
**Uydu** stili, uydu ve hava gözünüzde bir birleşimi.

![Uydu](./media/supported-map-styles/satellite.png)

**İlgili API'ler:**
* [Uydu kutucuğu](https://docs.microsoft.com/rest/api/maps/render/getmapimagerytilepreview)
* JS harita denetimi

## <a name="satelliteroadlabels"></a>satellite_road_labels
Yollar ve etiketleri uydu ve hava tanımayı üzerine yayılan karma bu harita stilidir.

![satellite_road_labels](./media/supported-map-styles/satellite_road_labels.png)

**İlgili API'ler:**
* JS harita denetimi

## <a name="grayscaledark"></a>grayscale_dark
**Koyu gri tonlamalı** yol haritası Stili Koyu bir sürümüdür.

![gray_scale](./media/supported-map-styles/grayscale_dark.png)

**İlgili API'ler:**
* JS harita denetimi 