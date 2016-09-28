<!--author=SharS last changed: 9/17/15-->

#### Seri konsol üzerinden bağlanmak için

1. Seri kabloyu cihaza bağlayın (doğrudan veya bir USB seri bağdaştırıcısı aracılığıyla).

2. **Denetim Masası**’nı açın; sonra da **Cihaz Yöneticisi**'ni açın.

3. COM bağlantı noktasını aşağıdaki çizimde gösterildiği gibi belirleyin.

     ![Seri konsol üzerinden bağlanma](./media/storsimple-use-putty/HCS_ConnectingDeviceS-include.png)

4. PuTTY’yi başlatın. 

5. Sağ bölmede, **Bağlantı türü**’nü **Seri** olarak değiştirin.

6. Sağ bölmede, uygun COM bağlantı noktasını yazın. Seri yapılandırma parametrelerinin buradaki gibi ayarlandığından emin olun:
  - Hız: 115.200
  - Veri bitleri: 8
  - Dur bitleri: 1
  - Eşlik: Yok
  - Akış denetimi: Yok

    Bu ayarlar aşağıdaki çizimde gösterilmiştir.

     ![PuTTY ayarları](./media/storsimple-use-putty/HCS_PuttyConfig-include.png) 

    > [AZURE.NOTE] Var sayılan akış denetimi ayarı işlemezse akış denetimini XON/XOFF olarak ayarlamayı deneyin.

7. Seri oturum başlatmak için **Aç**’a tıklayın.
 

<!--HONumber=Sep16_HO3-->


