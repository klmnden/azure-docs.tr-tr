---
title: Eşleme stilleri Azure eşlemelerinde desteklenen | Microsoft Docs
description: Azure haritalar tarafından desteklenen eşleme stilleri
author: walsehgal
ms.author: v-musehg
ms.date: 08/28/2018
ms.topic: concepts
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: 33b0f5df57623f0b4433a4a09c7cd15688783485
ms.sourcegitcommit: 63613e4c7edf1b1875a2974a29ab2a8ce5d90e3b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2018
ms.locfileid: "43191164"
---
# <a name="azure-maps-supported-map-styles"></a>Azure haritalar desteklenen eşleme stilleri
Azure haritalar dört farklı yerleşik eşleme stilleri destekler. Stilleri açıklamalarının yanı sıra aşağıda listelenmiştir.

## <a name="road"></a>Yol
A **yol** haritasıdır yollar, doğal görüntüler standart bir harita ve bu özellikler için etiketlerin yanı sıra modelimiz özellikleri.

![yol](./media/supported-map-styles/road.png)

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

## <a name="satelliteroadlabels"></a>Satellite_Road_Labels
Yollar ve etiketleri uydu ve hava tanımayı üzerine yayılan karma bu harita stilidir.

![satellite_road_labels](./media/supported-map-styles/satellite_road_labels.png)

**İlgili API'ler:**
* JS harita denetimi

## <a name="grayscaledark"></a>Grayscale_Dark
**Koyu gri tonlamalı** yol haritası Stili Koyu bir sürümüdür.

![gray_scale](./media/supported-map-styles/grayscale_dark.png)

**İlgili API'ler:**
* JS harita denetimi