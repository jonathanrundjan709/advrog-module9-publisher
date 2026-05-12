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
