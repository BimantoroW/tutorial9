# Modul 9

1. What are the key differences between unary, server streaming, and bi-directional streaming RPC (Remote Procedure Call) methods, and in what scenarios would each be most suitable? \
    - Unary: Client mengirimkan satu request kepada server dan server mengembalikan 1 respons. Metode ini cocok ketika client memerlukan data yang relatif pendek atau ingin melakukan operasi sederhana.
    - Server streaming: Client mengirimkan satu request kepada server dan server mengembalikan sebuah _stream_ respons dibandingkan mengembalikan 1 respons saja. Metode ini cocok ketika server harus mengirim data yang besar kepada client sehingga akan berguna jika responsnya dipecah-pecah.
    - Bi-directional streaming: Client mengirimkan _stream_ request dan server mengembalikan _stream_ respons. Metode ini cocok ketika diperlukan komunikasi real-time yang tidak terputus antara client dan server.
2. What are the potential security considerations involved in implementing a gRPC service in Rust, particularly regarding authentication, authorization, and data encryption?
    - Karena gRPC hanya berfungsi untuk bertukar pesan antar client dan server saja, perlu diimplementasikan suatu cara untuk menjamin keamanan pengiriman dan pemrosesan pesan-pesannya. Autentikasi dapat digunakan untuk memverifikasi identitas client. Dalam gRPC, autentikasi dapat dilakukan dengan menggunakan hal-hal seperti sertifikat dan token. Setelah client diverifikasi, perlu dilakukan suatu cara untuk menentukan hak apa saja yang dimiliki client ini dalam server. Proses ini disebut otorisasi. Salah satu penerapan otorisasi adalah dengan mengatur sebuah service agar hanya client-client tertentu saja yang hanya dapat mengakses data-data tertentu. Kemudian, untuk memastikan pesan yang dikirim tidak dapat dibaca atau diubah oleh seorang _attacker_ apabila ia menyadap pesannya, pesan tersebut dapat dienkripsi terlebih dahulu. Pada gRPC, enkripsi data biasanya ditangani melalui penggunaan TLS, yang menyediakan enkripsi data secara end-to-end saat pengiriman.
3. What are the potential challenges or issues that may arise when handling bidirectional streaming in Rust gRPC, especially in scenarios like chat applications?
    - Menangani beberapa stream secara bersamaan: Dalam aplikasi chat, beberapa pengguna mungkin mengirim dan menerima pesan secara bersamaan. Hal ini dapat menyebabkan situasi di mana server perlu menangani beberapa stream secara bersamaan sehingga dapat menjadi tantangan.
    - Scalibility: Dengan bertambahnya jumlah pengguna, server harus mampu men-scale untuk menangani beban baru tersebut. Hal ini termasuk memastikan bahwa server dapat menangani peningkatan jumlah streaming, dan bahwa proses streaming tidak menjadi hambatan.
    - Keamanan: Server perlu memastikan bahwa hanya pengguna yang terotoriasi saja yang dapat membuat dan menggunakan stream dan bahwa data yang dikirimkan aman.
4. What are the advantages and disadvantages of using the tokio_stream::wrappers::ReceiverStream for streaming responses in Rust gRPC services?
    - Kelebihan 
        - Operasi asinkronus
        - Streaming yang efisien
        - Dapat di-scale menjadi besar
    - Kekurangan
        - Kompleks
        - Susah untuk di-debug
        - Harus memahami lingkungan Tokio
5. In what ways could the Rust gRPC code be structured to facilitate code reuse and modularity, promoting maintainability and extensibility over time?
    - Modularize: Pecah kode menjadi modul-modul yang lebih kecil yang dapat digunakan kembali di berbagai bagian aplikasi.
    - Gunakan trait: Buat trait-trait yang dapat diimplementasikan oleh struct berbeda untuk menyediakan interface umum untuk berbagai jenis data atau fungsi.
    - Gunakan implementasi default: Gunakan implementasi default untuk trait-trait yang dapat dioverride oleh struct tertentu sehingga kode dapat digunakan kembali dan menghindari duplikasi.
6. In the MyPaymentService implementation, what additional steps might be necessary to handle more complex payment processing logic?
    - Validasi data sebelum memrosesnya.
    - Menggunakan streaming agar dapat menangani pengiriman data dalam jumlah besar
7. What impact does the adoption of gRPC as a communication protocol have on the overall architecture and design of distributed systems, particularly in terms of interoperability with other technologies and platforms?
    - Dengan menggunakan gRPC, client tidak perlu lagi memikirkan bagaimana cara membuat request HTTP dan mengirimkannya ke server karena gRPC akan menyambungkan fungsi yang ingin dipanggil client pada server secara otomatis. Hal ini dapat terjadi karena adanya file proto. Dengan ini, komunikasi dan operasi antar sistem dapat dipermudah.
8. What are the advantages and disadvantages of using HTTP/2, the underlying protocol for gRPC, compared to HTTP/1.1 or HTTP/1.1 with WebSocket for REST APIs?
    - Kelebihan
        - Multiplexing: HTTP/2 memungkinkan beberapa request dan respons dikirim melalui satu koneksi TCP, mengurangi overhead dan latensi HTTP/1.1
        - Kompresi Header: HTTP/2 menyediakan kompresi header, yang secara signifikan mengurangi overhead transmisi header dengan setiap permintaan dan respons
    - Kekurangan
        - Tidak Cocok untuk Aplikasi Real-Time: HTTP/2 tidak dirancang untuk komunikasi bi-directional secara real-time, yang merupakan fitur utama WebSocket
        - Dibatasi oleh Model Request-Respon: HTTP/2 dibatasi oleh model request-respons, yang mungkin tidak cocok untuk beberapa skenario real-time yang memerlukan transfer data yang tidak diminta
        - Pemblokiran Head-of-Line: HTTP/2 dapat mengalami pemblokiran head-of-line, yang dapat menyebabkan penundaan atau timeout jika satu stream diblokir oleh stream lainnya
9. How does the request-response model of REST APIs contrast with the bidirectional streaming capabilities of gRPC in terms of real-time communication and responsiveness?
    - REST API mengikuti model request-respons, di mana client mengirimkan request ke server dan server memberi respons. Model ini didasarkan pada protokol HTTP, biasanya menggunakan HTTP/1.1. Model ini cocok untuk aplikasi yang memerlukan pola komunikasi sederhana. gRPC, di sisi lain, mendukung bi-directional streaming, di mana client dan server dapat mengirim dan menerima beberapa request dan respons secara bersamaan. Hal ini memungkinkan komunikasi secara real-time sehingga cocok untuk aplikasi yang memerlukan komunikasi berlatensi rendah dan berkinerja tinggi. 
10. What are the implications of the schema-based approach of gRPC, using Protocol Buffers, compared to the more flexible, schema-less nature of JSON in REST API payloads?
    - Dengan pendekatan skemanya gRPC, data dapat dikirim dengan lebih efisien karena protobuf merupakan format binary yang memungkinkan format yang terkompresi tinggi dan mengurangi ukuran pesan. Selain itu, adanya skema juga membantu menjamin struktur data konsisten dan dapat digunakan oleh berbagai sistem. Di sisi lain, JSON memberikan fleksibilitas dalam pengiriman dan pemrosesan data. JSON mudah diimplementasikan karena tidak memerlukan skema atau struktur tertentu. Namun, fleksibilitas ini dapat meningkatkan risiko kesalahan dalam pengiriman data.
