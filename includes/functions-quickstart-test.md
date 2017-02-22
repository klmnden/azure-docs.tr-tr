
Azure İşlevleri hızlı başlangıçlarında işlevsel kod olduğundan, yeni işlevinizi hemen test edebilirsiniz.

1. **Develop (Geliştir)** sekmesinde **Code (Kod)** penceresini inceleyin ve bu Node.js kodunun, ileti gövdesinde veya bir sorgu dizesinde geçirilen bir *ad* değerine sahip bir HTTP isteğini beklediğine dikkat edin. İşlev çalıştığında bu değer yanıt iletisinde döndürülür.
   
2. İşleve ait yerleşik HTTP test isteği bölmesini görüntülemek için **Test**’e tıklayın.
 
    ![](./media/functions-quickstart-test/function-app-develop-tab-testing.png)

3. **Request body (İstek gövdesi)** metin kutusunda *ad* özelliğinin değerini adınızla değiştirin ve **Run (Çalıştır)** düğmesine tıklayın. Bir deneme HTTP isteği tarafından yürütmenin tetiklendiğini, bilgilerin günlüklere yazıldığını ve **Çıktıda** "merhaba..." yanıtının görüntülendiğini göreceksiniz. 

4. Aynı işlevin bir HTTP test aracı ya da başka bir tarayıcı penceresinden yürütülmesini tetiklemek için, **Geliştir** sekmesindeki **İşlev URL'si** değerini kopyalayın ve araca ya da tarayıcı adres çubuğuna yapıştırın. URL’ye `&name=yourname` sorgu dizesi değerini ekleyin ve isteği yürütün. Günlüklere aynı bilgilerin yazıldığını ve aynı dizenin yanıt iletisi gövdesinde yer aldığını unutmayın.

    ![](./media/functions-quickstart-test/function-app-browser-testing.png)


<!--HONumber=Feb17_HO2-->


