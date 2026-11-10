## Use Case 

1. Analisa excel file di backend dengan "agent as tool"


## Jupiter integration
Di fork ini, pengaturannya ada di Code Interpreter (bukan bagian Code Execution) pada halaman admin.

1. Buka Settings → Admin Settings → Code Execution.
2. Di bagian Code Interpreter, aktifkan Code Interpreter lalu pilih engine Jupyter.
3. Isi URL Jupyter yang bisa dijangkau dari proses/container Open WebUI. Jika keduanya ada di jaringan Docker Compose yang sama, biasanya bentuknya http://jupyter:8888; jika Jupyter berjalan di host, gunakan alamat host yang dapat dijangkau dari container.
4. Pilih autentikasi Token, masukkan token Jupyter, lalu simpan. Atur timeout jika analisis Anda perlu waktu lebih lama. Opsi URL, autentikasi, dan timeout tersedia di UI fork ini. (pengaturan Code Interpreter (src/lib/components/admin/Settings/CodeExecution.svelte:173))

Alternatifnya, konfigurasi lewat environment variable Open WebUI:

ENABLE_CODE_INTERPRETER=true
CODE_INTERPRETER_ENGINE=jupyter
CODE_INTERPRETER_JUPYTER_URL=http://jupyter:8888
CODE_INTERPRETER_JUPYTER_AUTH=token
CODE_INTERPRETER_JUPYTER_AUTH_TOKEN=<token-jupyter>
CODE_INTERPRETER_JUPYTER_TIMEOUT=60

Pastikan model yang menjalankan analisis memiliki kapabilitas Code Interpreter aktif. Kalau memakai sub-agent, aktifkan juga Sub-agents dan izinkan model tersebut untuk dipakai sebagai sub-agent. Dokumentasi Open WebUI

Catatan untuk Excel: file lampiran yang diteruskan ke sub-agent belum tentu otomatis tersedia di filesystem Jupyter. Untuk analisis dengan Jupyter, pastikan file .xlsx bisa diakses dari workspace Jupyter—misalnya dengan menaruhnya di folder bersama yang di-mount—lalu minta model membaca file dari path tersebut. Jupyter pada integrasi ini memakai lingkungan bersama, jadi jangan gunakan satu instance tanpa isolasi untuk pengguna yang tidak saling dipercaya. Dokumentasi integrasi Jupyter, panduan Code Execution


## Open Terminal Integration

**Open Terminal lebih cocok untuk analisis Excel di backend** jika Anda ingin agent memakai Python dan file di lingkungan server. Berbeda dengan Jupyter, Open Terminal menyediakan workspace dengan shell, file browser, dan Python. Dokumentasi Open WebUI menyebut analisis spreadsheet sebagai salah satu kegunaannya dan merekomendasikannya untuk eksekusi kode yang lebih nyata. [Panduan Open Terminal](https://docs.openwebui.com/features/open-terminal/)

Untuk menghubungkannya:

1. Jalankan Open Terminal sebagai container, idealnya dengan volume agar file tetap ada setelah container dimulai ulang. [Panduan instalasi](https://docs.openwebui.com/features/open-terminal/setup/installation/)
2. Di Open WebUI, buka **Settings → Admin Settings → Integrations → Open Terminal**, tambahkan koneksi, lalu isi URL server dan API key. Kalau keduanya berada di Docker Compose yang sama, URL-nya biasanya `http://open-terminal:8000`. Gunakan bagian **Open Terminal**, bukan bagian External Tools.
3. Verifikasi koneksi dan simpan. Di chat, pilih terminal dari tombol cloud.
4. Unggah Excel lewat file browser terminal atau letakkan file di workspace terminal, lalu minta agent membaca file dari path tersebut.

Di repo ini, delegasi sub-agent meneruskan terminal yang dipilih ke chat sub-agent. ([penerusan terminal ke sub-agent](</home/agustinus/projects/mine/aick-3/fork-open-webui/backend/open_webui/utils/subagents.py:538>)) Pastikan model bisa melakukan tool calling. Untuk satu chat, unggah file ke workspace terminal lalu sertakan path dalam instruksi analisis.

Open Terminal memberi agent akses untuk menjalankan perintah di workspace-nya, jadi gunakan container terisolasi dan atur aksesnya dengan sesuai. Untuk penggunaan banyak orang, dokumentasi menyarankan workspace terisolasi melalui Terminals. [Panduan koneksi](https://docs.openwebui.com/features/open-terminal/setup/connecting/), [Terminals](https://docs.openwebui.com/features/open-terminal/terminals/)