# Tutorial 9

## Pertanyaan

### a. Berapa banyak data yang dikirim program publisher ke message broker dalam satu kali run?

Dalam satu kali run, program publisher mengirim 5 data atau 5 message ke message broker.

Hal ini dapat dilihat dari kode pada `src/main.rs`, yaitu terdapat 5 pemanggilan fungsi `publish_event` dengan event `user_created`. Setiap pemanggilan mengirim satu `UserCreatedEventMessage` yang berisi `user_id` dan `user_name`.

Data yang dikirim adalah:

- `user_id: "1"`, `user_name: "2406435231-Amir"`
- `user_id: "2"`, `user_name: "2406435231-Budi"`
- `user_id: "3"`, `user_name: "2406435231-Cica"`
- `user_id: "4"`, `user_name: "2406435231-Dira"`
- `user_id: "5"`, `user_name: "2406435231-Emir"`

Jadi, total data yang dikirim publisher ke message broker adalah 5 message.

### b. URL `amqp://guest:guest@localhost:5672` sama dengan program subscriber, apa artinya?

URL `amqp://guest:guest@localhost:5672` yang sama pada publisher dan subscriber berarti keduanya terhubung ke message broker yang sama, yaitu RabbitMQ yang berjalan secara lokal pada komputer yang sama.

Pada URL tersebut:

- `amqp://` menunjukkan bahwa koneksi menggunakan protokol AMQP.
- `guest` yang pertama adalah username.
- `guest` yang kedua adalah password.
- `localhost` menunjukkan bahwa RabbitMQ berjalan di komputer lokal.
- `5672` adalah port default RabbitMQ untuk koneksi AMQP.

Karena publisher dan subscriber menggunakan URL yang sama, publisher dapat mengirim message ke broker yang sama dengan broker yang didengarkan oleh subscriber. Dengan begitu, message yang dikirim oleh publisher dapat diterima dan diproses oleh subscriber.

## Monitoring Chart

Berikut adalah tampilan RabbitMQ Management setelah publisher dijalankan:

![RabbitMQ monitoring chart setelah publisher dijalankan](assets/rabbitmq-monitoring-chart.png)

Spike pada chart terjadi karena publisher dijalankan dan mengirim beberapa message ke message broker dalam waktu yang sangat singkat. Pada program ini, publisher mengirim 5 message `user_created` setiap kali dijalankan. Ketika message tersebut masuk ke RabbitMQ, grafik message rates mencatat adanya aktivitas publish sehingga muncul kenaikan sementara.

Setelah semua message selesai dikirim dan diproses oleh subscriber, rate kembali turun ke `0.00/s`. Hal ini menunjukkan bahwa spike tersebut berkaitan langsung dengan proses menjalankan publisher: saat publisher aktif mengirim message, chart naik; setelah tidak ada message baru yang dikirim, chart kembali datar.

## Running RabbitMQ as Message Broker

RabbitMQ berhasil dijalankan menggunakan Docker pada environment cloud. Management UI RabbitMQ dapat diakses melalui alamat cloud server dengan port `15672`, sedangkan aplikasi publisher dan subscriber terhubung ke RabbitMQ melalui port AMQP `5672`.

Karena eksperimen dijalankan di cloud, port yang dibutuhkan harus dibuka pada konfigurasi firewall atau security group. Port `15672` digunakan untuk mengakses RabbitMQ Management UI dari browser, sedangkan port `5672` digunakan oleh publisher dan subscriber untuk koneksi AMQP ke message broker.

## Sending and Processing Event

Ketika publisher dijalankan, publisher mengirim lima event `UserCreatedEventMessage` ke RabbitMQ melalui queue `user_created`. Subscriber yang sedang berjalan menerima event tersebut dan mencetak isi message ke terminal. Ini menunjukkan pola event-driven architecture karena publisher tidak memanggil subscriber secara langsung, melainkan mengirim event ke message broker.

Pada bagian simulasi slow subscriber, subscriber dibuat lebih lambat dalam memproses message. Akibatnya, jika publisher dijalankan beberapa kali dalam waktu singkat, message akan menumpuk sementara di queue. Setelah subscriber memproses message satu per satu, jumlah queue akan turun kembali.
