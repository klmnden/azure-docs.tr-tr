---
title: Azure haritalar stili planlanabilecek harita | Microsoft Docs
description: Azure haritalar hakkında bilgi edinin stili ilgili işlevler için Android SDK'sı.
author: walsehgal
ms.author: v-musehg
ms.date: 04/26/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.openlocfilehash: 3c8c5d4bae16d8e15c8f2c5b1cc8e00eb14e4ce3
ms.sourcegitcommit: e7d4881105ef17e6f10e8e11043a31262cfcf3b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2019
ms.locfileid: "64870980"
---
# <a name="set-map-style-using-azure-maps-android-sdk"></a>Azure haritalar Android SDK'sını kullanarak bir harita stilini ayarlayın

Bu makalede Azure haritalar Android SDK'sını kullanarak eşleme stilleri ayarlamak için iki yol gösterir. Azure haritalar, aralarından seçim yapabileceğiniz altı farklı eşlemeler stilleri sahiptir. Desteklenen eşleme stilleri hakkında daha fazla bilgi için bkz. [eşleme stilleri Azure eşlemelerinde desteklenen](./supported-map-styles.md).


## <a name="prerequisites"></a>Önkoşullar

Bu makalede işlemi tamamlamak için yüklemeniz gerekir [Azure haritalar Android SDK'sı](https://docs.microsoft.com/azure/azure-maps/how-to-use-android-map-control-library) bir haritayı yükleyin.


## <a name="set-map-style-in-the-layout"></a>Düzende harita stilini ayarlayın

Düzen dosyası içinde etkinlik sınıfınız için bir harita stili ayarlayabilirsiniz. Düzen **res > Düzen > activity_main.xml**, bir aşağıdaki gibi görünür:

```XML
<?xml version="1.0" encoding="utf-8"?>
<FrameLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    >

    <com.microsoft.azure.maps.mapcontrol.MapControl
        android:id="@+id/mapcontrol"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        app:mapcontrol_centerLat="47.602806"
        app:mapcontrol_centerLng="-122.329330"
        app:mapcontrol_zoom="12"
        app:mapcontrol_style="grayscale_dark"
        />

</FrameLayout>
```

`mapcontrol_style` Yukarıdaki özniteliği harita stilini ayarlar **grayscale_dark**. 

<center>

![Style-grayscale_dark](./media/set-android-map-styles/grayscale-dark.png)</center>

## <a name="set-map-style-in-the-activity-class"></a>Etkinlik sınıfında harita stilini ayarlayın

Harita stili etkinliği sınıfında ayarlayabilirsiniz. Aşağıdaki kod parçacığını kopyalayıp kopyalama **onCreate()** yöntemi, `MainActivity.java` sınıfı. Bu harita stilini ayarlar **satellite_road_labels**.

```Java
    mapControl.onReady(map -> {
    //Set the camera of the map.
    map.setCamera(center(47.64, -122.33), zoom(14));

    //Set the style of the map.
    map.setStyle(style(MapStyle.SATELLITE));
});
```

<center>

![Style-uydu-yol-etiketleri](./media/set-android-map-styles/satellite-road-labels.png)</center>