---
title: Zorunlu yapıtlar için Azure DevTest Labs'teki belirtin. | Microsoft Docs
description: Yüklü laboratuvarındaki sanal makinelerde (VM) üzerinde kullanıcı tarafından seçilen yapıtların yüklemeden önce yapmanız zorunlu yapıtları belirleme konusunda bilgi edinin.
services: devtest-lab,virtual-machines
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/23/2018
ms.author: spelluru
ms.openlocfilehash: 090236ec3647c7c3e38eb862780a615f854e952b
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59795415"
---
# <a name="specify-mandatory-artifacts-for-your-lab-in-azure-devtest-labs"></a>Azure DevTest Labs'de laboratuvarınız için zorunlu yapıtları belirtin
Laboratuvar sahibi olarak, laboratuvar'da oluşturulan her makineye uygulanan zorunlu yapıtları belirtebilirsiniz. Her makine şirket ağınıza bağlanması laboratuvarınızda istediğiniz senaryoyu düşünün. Bu durumda, her bir laboratuvar kullanıcı, kendi makine şirket etki alanına bağlı olduğundan emin olmak için sanal makine oluşturma sırasında bir etki alanı birleşim yapıya eklemeniz gerekir. Diğer bir deyişle, Laboratuvar kullanıcıları aslında bunlar, makinede zorunlu yapıları uygulamak unutmanız durumunda bir makine yeniden oluşturmanız gerekir. Laboratuvar sahibi olarak, etki alanı birleşim yapıya laboratuvarınızda zorunlu bir yapıt yapın. Bu adımı her makine şirket ağına ve zaman ve çaba Laboratuvar kullanıcılarınız için kaydetme bağlı olduğundan emin olur.
 
Zorunlu diğer yapıları, takım projenizin kullandığı ortak bir araç veya vb. varsayılan olarak her bir makine olmalıdır bir platformla ilgili güvenlik paketi içerebilir. Kısacası, zorunlu bir yapıt laboratuvarınızda her makineye sahip olması gereken herhangi bir genel yazılım olur. Zorunlu yapıtları uygulanmış olan bir makineden özel bir görüntü oluşturun ve ardından yeni bir makine bu görüntüden oluşturun, zorunlu yapıları oluşturma sırasında makinede yeniden uygulanır. Bu davranışı özel görüntüyü bir makine bundan en güncel sürümünü zorunlu yapıtları oluşturduğunuz her zaman eski olsa bile, oluşturma sırasında akışı uygulanmış anlamına gelir. 
 
Parametresi olmayan yapılar zorunlu çıktığı haliyle desteklenir. Laboratuvar kullanıcınızın Laboratuvar oluşturma ve bu nedenle VM oluşturma işlemini basit hale getirme sırasında ek parametreler girin gerekmez. 

## <a name="specify-mandatory-artifacts"></a>Zorunlu yapıtları belirtme
Windows ve Linux makineler için zorunlu yapıtları ayrı ayrı seçebilirsiniz. Ayrıca, bunları uygulanacak istediğiniz düzene bağlı olarak bu yapıtları yeniden sıralayabilirsiniz. 

1. Laboratuvarınızın giriş sayfasında, seçin **yapılandırması ve ilkelerini** altında **ayarları**. 
3. Seçin **zorunlu yapıtları** altında **dış KAYNAKLARA**. 
4. Seçin **Düzenle** içinde **Windows** bölüm veya **Linux** bölümü. Bu örnekte **Windows** seçeneği. 

    ![Zorunlu yapıları sayfası - Düzenle düğmesi](media/devtest-lab-mandatory-artifacts/mandatory-artifacts-edit-button.png)
4. Bir yapı seçin. Bu örnekte **7-Zip** seçeneği. 
5. Üzerinde **yapıt ekleme** sayfasında **Ekle**. 

    ![Zorunlu yapıları sayfası - 7-zip ekleme](media/devtest-lab-mandatory-artifacts/add-seven-zip.png)
6. Başka bir yapıt eklemek için makaleyi seçip **Ekle**. Bu örnek ekler **Chrome** zorunlu ikinci yapıt olarak.

    ![Zorunlu yapıları sayfası - Chrome Ekle](media/devtest-lab-mandatory-artifacts/add-chrome.png)
7. Üzerinde **zorunlu yapıtları** sayfasında seçili yapıların sayısını belirten bir ileti görürsünüz. İletiye tıklayarak seçtiğiniz yapıtları görürsünüz. Seçin **Kaydet** kaydetmek için. 

    ![Zorunlu yapıları sayfası - yapıtları Kaydet](media/devtest-lab-mandatory-artifacts/save-artifacts.png)
8. Linux Vm'leri için zorunlu yapıtları belirtmek için bu adımları yineleyin. 
    
    ![Zorunlu yapıları sayfası - Windows ve Linux yapıtları](media/devtest-lab-mandatory-artifacts/windows-linux-artifacts.png)
9. İçin **Sil** seçin listeden bir yapıt **... (üç nokta)**  seçin ve satır sonunda **Sil**. 
10. İçin **yeniden sıralama** listede, yapıt üzerinde gezdirin fare yapıtları seçin **... (üç nokta)**  satırın başlangıcında gösterilir ve öğeyi yeni konuma sürükleyin. 
11. Laboratuar ortamında zorunlu yapıtları kaydetmek için seçmeniz **Kaydet**. 

    ![Zorunlu yapıları sayfası - Laboratuvardaki yapıtları Kaydet](media/devtest-lab-mandatory-artifacts/save-to-lab.png)
12. Kapat **yapılandırması ve ilkelerini** sayfa (seçin **X** sağ üst köşedeki) laboratuvarınız için giriş sayfasına geri dönmek için.  

## <a name="delete-a-mandatory-artifact"></a>Zorunlu bir yapıtı Sil
Zorunlu bir yapıt bir laboratuvarda silmek için aşağıdaki eylemleri gerçekleştirin: 

1. Seçin **yapılandırması ve ilkelerini** altında **ayarları**. 
2. Seçin **zorunlu yapıtları** altında **dış KAYNAKLARA**. 
3. Seçin **Düzenle** içinde **Windows** bölüm veya **Linux** bölümü. Bu örnekte **Windows** seçeneği. 
4. İletinin üst zorunlu yapıların sayısını seçin. 

    ![Zorunlu yapıları sayfası - ileti seçin](media/devtest-lab-mandatory-artifacts/select-message-artifacts.png)
5. Üzerinde **seçili yapıtları** sayfasında **... (üç nokta)**  silinecek ve seçmek yapıtı **Kaldır**. 
    
    ![Zorunlu yapıları sayfası - Remove yapıt](media/devtest-lab-mandatory-artifacts/remove-artifact.png)
6. Seçin **Tamam** kapatmak için **seçili yapıtları** sayfası. 
7. Seçin **Kaydet** üzerinde **zorunlu yapıtları** sayfası.
8. İçin adımları yineleyin **Linux** gerekirse görüntüler. 
9. Seçin **Kaydet** laboratuvara tüm değişiklikleri kaydedin. 

## <a name="view-mandatory-artifacts-when-creating-a-vm"></a>Bir VM oluşturulurken zorunlu yapıları görüntüle
Artık, bir laboratuvar kullanıcı olarak laboratuvarda bir VM oluşturulurken zorunlu yapıtların listesini görüntüleyebilirsiniz. Düzenleyemez veya Laboratuvar, laboratuvarı sahibi tarafından ayarlanan zorunlu yapıtları silin.

1. Laboratuvarınız için giriş sayfasında, seçin **genel bakış** menüsünde.
2. Bir VM'yi laboratuvara ekleme için seçin **+ Ekle**. 
3. Seçin bir **temel görüntü**. Bu örnekte **Windows Server 1709 sürümü**.
4. İçin bir ileti gördüğünüz dikkat edin **Yapıtları** numarasıyla zorunlu yapıtlar seçildi. 
5. Seçin **Yapıtları**. 
6. Gördüğünüzü onaylayın **zorunlu yapıtları** Laboratuvar yapılandırması ve ilkelerini belirtildi. 

    ![VM - zorunlu yapıları oluşturma](media/devtest-lab-mandatory-artifacts/create-vm-artifacts.png)

## <a name="next-steps"></a>Sonraki adımlar
* Bilgi edinmek için nasıl [laboratuvara Git yapıt deposu ekleme](devtest-lab-add-artifact-repo.md).

