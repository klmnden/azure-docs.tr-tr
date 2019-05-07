---
title: Visual Studio Code (Önizleme) ile Azure Stream Analytics'i keşfedin
description: Bu makalede Azure Stream Analytics işi, bir yerel projeye listesi işleri ve varlıkları işi görüntüle, dışarı aktarma ve Stream Analytics işiniz için bir CI/CD işlem hattı kurmak gösterilmektedir.
ms.service: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.date: 05/06/2019
ms.topic: conceptual
ms.openlocfilehash: 640a81fcb94194e2e907e9339f016bd35e279fdd
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65079218"
---
# <a name="explore-azure-stream-analytics-with-visual-studio-code-preview"></a>Visual Studio Code (Önizleme) ile Azure Stream Analytics'i keşfedin

Visual Studio Code uzantısı için Azure Stream Analytics, geliştiricilerin Stream Analytics işlerini yönetmek için basit bir deneyim sunar. Azure Stream Analytics uzantısı ile şunları yapabilirsiniz:

- [Oluşturma](quick-create-vs-code.md), başlatmak ve durdurmak işleri
- Mevcut dışarı aktarma işleri için yerel bir proje
- Sorguları yerel olarak çalıştırma
- Bir CI/CD işlem hattı ayarlayın

## <a name="export-a-job-to-a-local-project"></a>Bir işi dışarı aktarma için yerel bir proje

Yerel bir proje için bir iş dışarı aktarmak için dışarı aktarmak istediğiniz işi bulun **Stream Analytics Gezgini'nde** Visual Studio code'da. Ardından, projeniz için bir klasör seçin. Proje, seçtiğiniz klasöre aktarılır ve Visual Studio Code'dan işi yönetmek devam edebilirsiniz. Stream Analytics işlerini yönetmek için Visual Studio Code kullanmayla ilgili daha fazla bilgi için bkz. Visual Studio Code [hızlı](quick-create-vs-code.md).

![Visual Studio code'da ASA işi Dışarı Aktar](./media/vs-code-how-to/export-job.png)

## <a name="run-queries-locally"></a>Sorguları yerel olarak çalıştırma

Visual Studio Code için Azure Stream Analytics uzantısı, Stream Analytics işleri örnek veriler içeren yerel olarak test etmek için kullanabilirsiniz.

1. Stream Analytics işinizi oluşturduktan sonra kullanma **Ctrl + Shift + P** komut paleti açın. Seçin ve ardından yazın **ASA: Girdi ekleme**.

    ![Visual Studio code'da ASA Girişi Ekle](./media/vs-code-how-to/add-input.png)

2. Seçin **yerel giriş**.

    ![Visual Studio code'da ASA yerel giriş Ekle](./media/vs-code-how-to/add-local-input.png)

3. Seçin **+ yeni yerel giriş**.

    ![Visual Studio code'da Giriş bir yeni yerel ASA Ekle](./media/vs-code-how-to/add-new-local-input.png)

4. Sorgunuzda kullanılan aynı giriş diğer adı girin.

    ![Yeni bir ASA yerel giriş diğer ad Ekle](./media/vs-code-how-to/new-local-input-alias.png)

5. İçinde **LocalInput_DefaultLocalStream.json** dosyasında, yerel veri dosyanızın bulunduğu dosya yolunu girin.

    ![Visual Studio'da yerel dosya yolu girin](./media/vs-code-how-to/local-file-path.png)

6. İade, sorgu Düzenleyicisi ve seçin **yerel olarak çalıştırma**.

    ![Sorgu Düzenleyicisi'nde Çalıştır'ı yerel olarak seçin](./media/vs-code-how-to/run-locally.png)

## <a name="set-up-a-cicd-pipeline"></a>Bir CI/CD işlem hattı ayarlayın

Sürekli tümleştirme ve dağıtım kullanarak Azure Stream Analytics işleri için etkinleştirebilirsiniz **asa cıcd Araçları** NPM paketi. NPM paketi, otomatik dağıtım için Stream Analytics Visual Studio Code projeleri araçları sağlar. Bunu Windows, macOS ve Linux üzerinde Visual Studio Code yükleme olmadan kullanılabilir.

Yapılandırmasını tamamladıktan [paketini karşıdan](https://usqldownload.blob.core.windows.net/ext/asa/asa-cicd-0.0.1-preview-beta.tar), Azure Resource Manager şablonları çıktısını almak için aşağıdaki komutu kullanın. Varsa **outputPath** belirtilmezse, şablonları yerleştirilecek **Dağıt** projenin altında klasör **bin** klasör.

```powershell
asa-cicd build -scriptPath <scriptFullPath> -outputPath <outputPath>
```

## <a name="next-steps"></a>Sonraki adımlar

* [Visual Studio Code'da (Önizleme) Azure Stream Analytics bulut işi oluşturma](quick-create-vs-code.md)