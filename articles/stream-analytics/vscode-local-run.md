---
title: Azure Stream Analytics sorguları Visual Studio Code (Önizleme) ile yerel olarak test etme
description: Bu makalede, Visual Studio Code için Azure Stream Analytics araçları sorgularla yerel olarak test etmek açıklar.
ms.service: stream-analytics
author: su-jie
ms.author: sujie
ms.date: 05/15/2019
ms.topic: conceptual
ms.openlocfilehash: f477a0f99c3eaa82568d8188bfaae03818fb72dc
ms.sourcegitcommit: be9fcaace62709cea55beb49a5bebf4f9701f7c6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2019
ms.locfileid: "65827958"
---
# <a name="test-stream-analytics-queries-locally-with-visual-studio-code"></a>Stream Analytics sorguları Visual Studio Code ile yerel olarak test etme

Visual Studio Code için Azure Stream Analytics araçları, Stream Analytics işleri örnek veriler içeren yerel olarak test etmek için kullanabilirsiniz.

Bunu kullanın [hızlı](quick-create-vs-code.md) Visual Studio Code kullanarak Stream Analytics işi oluşturma hakkında bilgi edinmek için.

## <a name="run-queries-locally"></a>Sorguları yerel olarak çalıştırma

Visual Studio Code için Azure Stream Analytics uzantısı, Stream Analytics işleri örnek veriler içeren yerel olarak test etmek için kullanabilirsiniz.

1. Stream Analytics işinizi oluşturduktan sonra kullanma **Ctrl + Shift + P** komut paletini açın. Seçin ve ardından yazın **ASA: Girdi ekleme**.

    ![Visual Studio code'da ASA Girişi Ekle](./media/vscode-local-run/add-input.png)

2. Seçin **yerel giriş**.

    ![Visual Studio code'da ASA yerel giriş Ekle](./media/vscode-local-run/add-local-input.png)

3. Seçin **+ yeni yerel giriş**.

    ![Visual Studio code'da Giriş bir yeni yerel ASA Ekle](./media/vscode-local-run/add-new-local-input.png)

4. Sorgunuzda kullanılan aynı giriş diğer adı girin.

    ![Yeni bir ASA yerel giriş diğer ad Ekle](./media/vscode-local-run/new-local-input-alias.png)

5. İçinde **LocalInput_DefaultLocalStream.json** dosyasında, yerel veri dosyanızın bulunduğu dosya yolunu girin.

    ![Visual Studio'da yerel dosya yolu girin](./media/vscode-local-run/local-file-path.png)

6. İade, sorgu Düzenleyicisi ve seçin **yerel olarak çalıştırma**.

    ![Sorgu Düzenleyicisi'nde Çalıştır'ı yerel olarak seçin](./media/vscode-local-run/run-locally.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Visual Studio Code'da (Önizleme) Azure Stream Analytics bulut işi oluşturma](quick-create-vs-code.md)

* [Visual Studio Code (Önizleme) ile Azure Stream Analytics işleri keşfedin](vscode-explore-jobs.md)
