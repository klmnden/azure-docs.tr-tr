---
title: Donanım - Microsoft Azure FXT Edge dosyalayıcı Başlat
description: Bir başlangıç parolası Azure FXT Edge dosyalayıcı düğümlerinde ayarlama
author: ekpgh
ms.service: fxt-edge-filer
ms.topic: tutorial
ms.date: 06/20/2019
ms.author: v-erkell
ms.openlocfilehash: 11cf9f49014648fff1e78aff91c5a724a812e9e7
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67450299"
---
# <a name="tutorial-set-hardware-passwords"></a>Öğretici: Kümesi donanım parolaları

İlk kez, bir Azure FXT Edge dosyalayıcı düğümü güç bir kök parola ayarlamanız gerekir. Donanım düğümleri varsayılan parola ile birlikte gönderilmeyen. 

Ağ bağlantı noktaları kadar parolayı ayarlayın ve kök kullanıcı oturum açtıktan sonra devre dışı bırakıldı.

Yükleme ve düğüm kablo sonra ancak kümeyi oluşturmak denemeden önce bu adımı uygulayın. 

Bu öğreticide, donanım düğüme bağlanmak ve parola ayarlama açıklanmaktadır. 

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz: 

> [!div class="checklist"]
> * Klavye ve monitör düğüme bağlanmak ve açın
> * Bu düğümde iDRAC bağlantı noktası ve kök kullanıcı için parola ayarlama
> * Kök olarak oturum açın 

Kümenizde kullanacağınız her düğüm için bu adımları yineleyin. 

Bu öğreticinin tamamlanması yaklaşık 15 dakika sürer. 

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiye başlamadan önce aşağıdaki adımları tamamlayın: 

* [Yükleme](fxt-install.md) her Azure FXT Edge dosyalayıcı düğümünde bir donanım raf, güç kabloları ekleme ve ağ erişimi açıklandığı [önceki öğretici](fxt-network-power.md). 
* USB bağlantılı bir klavye ve donanım düğümlere ekleyebilirsiniz VGA bağlı bir izleyici bulun. (Parola ayarlamadan önce düğümün seri bağlantı noktası etkin değil.)

## <a name="connect-a-keyboard-and-monitor-to-the-node"></a>Klavye ve monitör düğümüne bağlanma

Fiziksel olarak izleme ve klavye Azure FXT Edge dosyalayıcı düğüme bağlanılamadı. 

* İzleyici VGA bağlantı noktasına bağlayın.
* Klavyenin USB bağlantı noktalarından birine bağlanın. 

Kasa arkasında bağlantı noktalarını bulmak için bu başvuru diyagramı kullanın. 

> [!NOTE]
> Parola ayarlandıktan sonra seri bağlantı noktası kadar etkin değil. 

![Diyagram, Azure FXT Edge dosyalayıcı arkasına seri, VGA, ile ve USB bağlantı noktaları](media/fxt-back-serial-vga-usb.png)

Birden fazla düğüm aynı çevre birimlerine bağlanmak istiyorsanız, KVM anahtarı kullanabilirsiniz. 

Ön güç düğmesine basarak düğümde gücü. 

![Güç düğmesi hepsini Azure FXT Edge dosyalayıcı - ön diyagramı üst kısımda doğru etiketli](media/fxt-front-annotated.png)

## <a name="set-initial-passwords"></a>İlk parolaları ayarlama 

Azure FXT Edge dosyalayıcı düğüm önyükleme yaparken izlemek için çeşitli iletilerini yazdırır. Birkaç dakika sonra böyle bir kurulum ilk ekran gösterilmektedir:

```
------------------------------------------------------
        Microsoft FXT node initial setup
------------------------------------------------------
Password Setup
---------------
Enter a password to set iDRAC and temporary root password.
Minimum password length is 8.
Enter new password:
```

Girdiğiniz parola iki şey için kullanılır: 

* Bu Azure FXT Edge dosyalayıcı düğüm için geçici kök paroladır. 

  Bu düğümü kullanarak bir küme oluşturduğunuzda ya da bu düğüm kümeye eklediğinizde bu parola değişecektir. Küme yönetimi parolası (kullanıcıyla ilişkili ``admin``) da bir kümedeki tüm düğümler için kök parola değil.

* İDRAC/IPMI donanım yönetim bağlantı noktası için uzun vadeli paroladır.

  Bir donanım sorunu gidermek için oturum IPMI daha sonra oturum açmak gerekirse parolayı anımsa emin olun.

Girin ve parolayı onaylayın: 

```
Enter new password:**********
Re-enter password:**********
Loading AvereOS......
```

Parolasını girdikten sonra sistem önyüklemeye devam eder. Tamamlandığında, verir bir ``login:`` istemi. 

## <a name="sign-in-as-root"></a>Kök olarak oturum açın

Olarak oturum ``root`` parolayla ayarlamanız yeterlidir. 

```
login: root
Password:**********
```

Kök olarak oturum açtıktan sonra ağ bağlantı noktalarının etkindir ve DHCP sunucusu IP adreslerinin bağlantı kurar. 

## <a name="next-steps"></a>Sonraki adımlar

Düğüm kümesinin parçası olacak şekilde hazırdır. Azure FXT Edge dosyalayıcı kümeyi oluşturmak için kullanabilir veya [mevcut bir kümeye ekleme](fxt-add-nodes.md). 

> [!div class="nextstepaction"]
> [Küme oluşturma](fxt-cluster-create.md)
