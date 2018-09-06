---
title: Azure için MongoDB, Angular ve Node öğreticisi - 6. Bölüm | Microsoft Belgeleri
description: Azure Cosmos DB üzerinde Angular ve Node ile MongoDB için kullandığınız aynı API'leri kullanarak bir MongoDB uygulaması oluşturma öğreticisi dizisinin 6. bölümü
services: cosmos-db
author: johnpapa
manager: kfile
editor: ''
ms.service: cosmos-db
ms.component: cosmosdb-mongo
ms.devlang: nodejs
ms.topic: tutorial
ms.date: 06/17/2018
ms.author: jopapa
ms.custom: mvc
ms.openlocfilehash: a1705913e1656901d0a87a3cebb2eb69a6c7ad63
ms.sourcegitcommit: cb61439cf0ae2a3f4b07a98da4df258bfb479845
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/05/2018
ms.locfileid: "43698596"
---
# <a name="create-a-mongodb-app-with-angular-and-azure-cosmos-db---part-6-add-post-put-and-delete-functions-to-the-app"></a>Angular ve Azure Cosmos DB ile MongoDB uygulaması oluşturma - 6. Bölüm: Uygulamaya Post, Put ve Delete işlevleri ekleme

Bu çok bölümlü öğretici, Express ve Angular ile Node.js kullanılarak yazılmış yeni bir [MongoDB API](mongodb-introduction.md) uygulamasının nasıl oluşturulacağını ve Azure Cosmos DB veritabanına nasıl bağlanacağını gösterir.

Öğreticinin 6. bölümünde [5. bölümdeki](tutorial-develop-mongodb-nodejs-part5.md) konular genişletilir ve aşağıdaki görevler yer alır:

> [!div class="checklist"]
> * Hero hizmeti için Post, Put ve Delete işlevleri oluşturma
> * Uygulamayı çalıştırma

> [!VIDEO https://www.youtube.com/embed/Y5mdAlFGZjc]

## <a name="prerequisites"></a>Ön koşullar

Öğreticinin bu bölümüne başlamadan önce öğreticinin [5. bölümündeki](tutorial-develop-mongodb-nodejs-part5.md) adımları tamamladığınızdan emin olun.

> [!TIP]
> Bu öğretici, uygulamanızı adım adım oluşturmanızı sağlayacak adımlarla size yol gösterir. Tamamlanmış projeyi indirmek isterseniz, tamamlanmış uygulamayı github'daki [angular-cosmosdb deposundan](https://github.com/Azure-Samples/angular-cosmosdb) alabilirsiniz.

## <a name="add-a-post-function-to-the-hero-service"></a>Hero hizmetine bir Post işlevi ekleme

1. Visual Studio Code'da **routes.js**’yi ve **hero.service.js**’yi **Bölünmüş Düzenleyici** düğmesine ![Visual Studio'daki Bölünmüş Düzenleyici düğmesi](./media/tutorial-develop-mongodb-nodejs-part6/split-editor-button.png) basarak yan yana açın.

    Routes.js’de 7. satırda, **hero.service.js**’nin 5. satırındaki `getHeroes` işlevinin çağrıldığına dikkat edin.  Post, put ve delete işlevleri için de aynı eşleşmeyi oluşturmamız gerekiyor. 

    ![Visual Studio Code’da routes.js ve hero.service.js](./media/tutorial-develop-mongodb-nodejs-part6/routes-heroservicejs.png)
    
    Hero hizmetini kodlayarak başlayalım. 

2. **hero.service.js**’de `getHeroes` işlevinden sonra, `module.exports` bölümünden önce aşağıdaki kodu kopyalayın. Bu kod:  
   * Yeni bir hero göndermek için hero modelini kullanır.
   * Bir hata olup olmadığını görmek için yanıtları denetler ve 500 durum değeri döndürür.

   ```javascript
   function postHero(req, res) {
     const originalHero = { uid: req.body.uid, name: req.body.name, saying: req.body.saying };
     const hero = new Hero(originalHero);
     hero.save(error => {
       if (checkServerError(res, error)) return;
       res.status(201).json(hero);
       console.log('Hero created successfully!');
     });
   }

   function checkServerError(res, error) {
     if (error) {
       res.status(500).send(error);
       return error;
     }
   }
   ```

3. **hero.service.js**’de, `module.exports` değerini yeni `postHero` işlevini içerecek şekilde güncelleştirin. 

    ```javascript
    module.exports = {
      getHeroes,
      postHero
    };
    ```

4. **routes.js**’de `get` yönlendiricisinden sonra `post` işlevi için bir yönlendirici ekleyin. Bu yönlendirici aynı anda bir hero gönderir. Yönlendirici dosyasını böyle yapılandırdığınızda tüm kullanılabilir API uç noktalarını rahatça görebilirsiniz ve asıl iş **hero.service.js** dosyası tarafından yapılır.

    ```javascript
    router.post('/hero', (req, res) => {
      heroService.postHero(req, res);
    });
    ```

5. Uygulamayı çalıştırarak her şeyin çalıştığından emin olun. Visual Studio Code’da tüm değişikliklerinizi kaydedin, sol taraftaki **Hata ayıkla** düğmesine ![Visual Studio Code’da hata ayıkla simgesi](./media/tutorial-develop-mongodb-nodejs-part6/debug-button.png) tıklayın, ardından **Hata Ayıklamayı Başlat** düğmesine ![ Visual Studio Code’da Hata ayıklamayı başlat simgesi](./media/tutorial-develop-mongodb-nodejs-part6/start-debugging-button.png) tıklayın.

6. Şimdi İnternet tarayıcınıza dönün ve çoğu makinede F12 tuşuna basarak açılan Geliştirici Araçları Ağı sekmesini açın. Ağ üzerinden yapılan çağrıları izlemek için [http://localhost:3000](http://localhost:3000) adresine gidin.

    ![Chrome’da ağ etkinliğini gösteren ağ sekmesi](./media/tutorial-develop-mongodb-nodejs-part6/add-new-hero.png)

7. **Yeni Hero Ekle** düğmesine tıklayarak yeni bir hero ekleyin. "999" kimliğini, "Fred" adını ve "Hello" ifadesini girin, ardından **Kaydet**’e tıklayın. Ağ sekmesinde yeni bir hero için POST isteği gönderdiğinizi görmeniz gerekir. 

    ![Get ve Post işlevleri için ağ etkinliğini gösteren Chrome ağ sekmesi](./media/tutorial-develop-mongodb-nodejs-part6/post-new-hero.png)

    Şimdi geri dönüp uygulamaya Put ve Delete işlevlerini ekleyelim.

## <a name="add-the-put-and-delete-functions"></a>Put ve Delete işlevlerini ekleme

1. **routes.js**’de gönderme yönlendiricisinden sonra `put` ve `delete` yönlendiricilerini ekleyin.

    ```javascript
    router.put('/hero/:uid', (req, res) => {
      heroService.putHero(req, res);
    });

    router.delete('/hero/:uid', (req, res) => {
      heroService.deleteHero(req, res);
    });
    ```

2. **hero.service.js**’de `checkServerError` işlevinden sonra aşağıdaki kodu kopyalayın. Bu kod:
   * `put` ve `delete` işlevlerini oluşturur
   * Heronun bulunup bulunmadığını denetler
   * Hata işlemeyi gerçekleştirir 

   ```javascript
   function putHero(req, res) {
     const originalHero = {
       uid: parseInt(req.params.uid, 10),
       name: req.body.name,
       saying: req.body.saying
     };
     Hero.findOne({ uid: originalHero.uid }, (error, hero) => {
       if (checkServerError(res, error)) return;
       if (!checkFound(res, hero)) return;

      hero.name = originalHero.name;
       hero.saying = originalHero.saying;
       hero.save(error => {
         if (checkServerError(res, error)) return;
         res.status(200).json(hero);
         console.log('Hero updated successfully!');
       });
     });
   }

   function deleteHero(req, res) {
     const uid = parseInt(req.params.uid, 10);
     Hero.findOneAndRemove({ uid: uid })
       .then(hero => {
         if (!checkFound(res, hero)) return;
         res.status(200).json(hero);
         console.log('Hero deleted successfully!');
       })
       .catch(error => {
         if (checkServerError(res, error)) return;
       });
   }

   function checkFound(res, hero) {
     if (!hero) {
       res.status(404).send('Hero not found.');
       return;
     }
     return hero;
   }
   ```

3. **hero.service.js**’de yeni modülleri dışarı aktarın:

   ```javascript
    module.exports = {
      getHeroes,
      postHero,
      putHero,
      deleteHero
    };
    ```

4. Kodu güncelleştirdikten sonra Visual Studio Code’da **Yeniden başlat** düğmesine ![Visual Studio Code’da Yeniden Başlat düğmesi](./media/tutorial-develop-mongodb-nodejs-part6/restart-debugger-button.png) tıklayın.

5. İnternet tarayıcınızda sayfayı yenileyin ve **Yeni Hero Ekle** düğmesine tıklayın. "9" kimliğine, "Starlord" adına ve "Hi" metnine sahip bir hero ekleyin. Yeni heroyu kaydetmek için **Kaydet** düğmesine tıklayın.

6. Şimdi **Starlord** adlı heroyu seçin ve "Hi" ifadesini "Bye" ifadesiyle değiştirip **Kaydet** düğmesine tıklayın. 

    Artık yükü görmek için ağ sekmesinde kimliği seçebilirsiniz. Yükte ifadenin "Bye" olarak ayarlandığını görebilirsiniz.

    ![Heroes uygulaması ve yükü gösteren Ağ sekmesi](./media/tutorial-develop-mongodb-nodejs-part6/put-hero-function.png) 

    Kullanıcı arabiriminde herolardan birini silin ve silme işlemini tamamlamak için gereken süreye bakın. Bunu "Fred" adlı hero için "Sil" düğmesine tıklayarak deneyebilirsiniz.

    ![Heroes uygulaması ve işlevleri tamamlamak için gereken zamanı gösteren Ağ sekmesi](./media/tutorial-develop-mongodb-nodejs-part6/times.png) 

    Sayfayı yenilerseniz Ağ sekmesi heroları almak için gereken zamanı gösterir. Bu süreler kısa olsa da, büyük ölçüde verilerinizin dünyanın neresinde olduğuna ve kullanıcılarınıza yakın bir konuma coğrafi çoğaltma olanağınıza bağlıdır. Coğrafi çoğaltma hakkında daha fazla bilgiyi, yakında yayınlanacak bir sonraki öğreticide bulabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Öğreticinin bu bölümünde aşağıdakileri yaptınız:

> [!div class="checklist"]
> * Uygulamaya Post, Put ve Delete işlevleri ekleme 

Bu öğretici serisine eklenecek videolar için burayı daha sonra tekrar ziyaret edin.

