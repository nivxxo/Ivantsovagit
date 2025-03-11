Отчет по виртуалке Перед началой установки, нужно установить Linux Oracle на VirtualBox, для этого нужно: Иметь образ Linux Выделить 2+ ядер. Выделать 4096+ МБ оперативы, и обязательно при установки мы выбираем английский язык. Мы переходим к установке docker с использованием grafana. Вводим следующий код: Устанавливаем утилиту Wget
Устанавливает утилиту wget на вашу систему
sudo yum install wget
![image](https://github.com/user-attachments/assets/0d95760f-dc71-4376-ac44-59fc1fa623db)
Устанавливаем утилиту Curl
sudo yum install curl
![image](https://github.com/user-attachments/assets/570c9c44-f680-49be-833e-1a5a100480ba)
Скачиваем файл репозитория
sudo wget -P /etc/yum.repos.d/ https://download.docker.com/linux/centos/docker-ce.repo
![image](https://github.com/user-attachments/assets/014c6ddf-ecd1-464e-bba4-94a055f8504d)
Устанавливаем docker
sudo yum install docker-ce docker-ce-cli containerd.io
![image](https://github.com/user-attachments/assets/49937b0a-b352-4de9-897e-7b618f0adc34)
Мы разрешаем автозапуск и запускаем 
sudo systemctl enable docker --now
![image](https://github.com/user-attachments/assets/8f780f04-5f4b-47e2-98ba-d53c14aa2f9f)
Убедимся в наличие пакета curl
sudo yum install curl
![image](https://github.com/user-attachments/assets/51cf47fe-eefa-44e5-b4af-269b7aa9564e)
Объявление переменной COMVER, полученной в результате curl запроса, хранящей в себе номер последней версии Docker Compose
COMVER=$(curl -s https://api.github.com/repos/docker/compose/releases/latest | grep 'tag_name' | cut -d\" -f4)
![image](https://github.com/user-attachments/assets/e1843f8c-1841-4b5a-8be2-ec384efab739)
Теперь скачиваем скрипт docker-compose последней версии, используя объявленную ранее переменную и помещаем его в каталог /usr/bin
sudo curl -L "https://github.com/docker/compose/releases/download/$COMVER/docker-compose-$(uname -s)-$(uname -m)" -o /usr/bin/docker-compose
![image](https://github.com/user-attachments/assets/2c710c00-f3fe-4a7d-91fa-0cc676e43845)
Предоставляем права на выполнения файла docker-compose 
sudo chmod +x /usr/bin/docker-compose
![image](https://github.com/user-attachments/assets/07ea206b-3442-442a-8662-eda5ecca3439)
Проверяем установленную версию Docker Compose
docker-compose --version
![image](https://github.com/user-attachments/assets/9355b997-569f-42a0-9dac-9512e9ba571f)
Можно скачать git прямо из командной строки прописав Y
Соглашаемся скачать git
git clone https://github.com/skl256/grafana_stack_for_docker.git
![image](https://github.com/user-attachments/assets/321b68df-110c-4470-a1e5-6931fd5f1a35)
Переходим в папку 
cd grafana_stack_for_docker
![image](https://github.com/user-attachments/assets/8bbe0666-9b12-4813-82da-1eabc2a9aeec)
Создаёт полный путь /mnt/common_volume/swarm/grafana/config, включая все необходимые промежуточные каталоги
sudo mkdir -p /mnt/common_volume/swarm/grafana/config
![image](https://github.com/user-attachments/assets/5d44fd53-e349-4042-a88c-36cb65a6afad)
Создаёт структуру каталогов для Grafana и связанных с ней компонентов 
sudo mkdir -p /mnt/common_volume/grafana/{grafana-config,grafana-data,prometheus-data}
![image](https://github.com/user-attachments/assets/52566297-c9d8-4064-a119-694848e08bc1)
Все файлы и каталоги в указанных директориях будут переданы в собственность текущему пользователю и его группе.
sudo chown -R $(id -u):$(id -g) {/mnt/common_volume/swarm/grafana/config,/mnt/common_volume/grafana}
![image](https://github.com/user-attachments/assets/1a49d3eb-947f-426e-ba03-291fe135af6a)
файл grafana.ini уже существует, команда обновит его временные метки (время последнего доступа и изменения). Если файл не существует, команда создаст новый пустой файл с указанным именем по указанному пути
touch /mnt/common_volume/grafana/grafana-config/grafana.ini
![image](https://github.com/user-attachments/assets/0031fe9d-5fd6-4284-95df-369cb4d15ce2)
Команда копирует все файлы и подкаталоги из директории config в директорию
cp config/* /mnt/common_volume/swarm/grafana/config/
![image](https://github.com/user-attachments/assets/24bd623b-fd27-427b-8d5b-dbcdea478e0a)
Команда переименовывает файл grafana.yaml в docker-compose.yaml.
mv grafana.yaml docker-compose.yaml
![image](https://github.com/user-attachments/assets/f07c8674-6d51-4d92-a12b-08fb926484f2)
Команда создает и запускает контейнеры в фоновом режиме используя конфигурацию из файла docker-compose.yml. Но с правами суперпользхователя 
sudo docker compose up -d
![image](https://github.com/user-attachments/assets/79d02951-9550-47dc-b129-a48291a67a16)
Открывает файл docker-compose.yaml в текстовом редакторе vi с правами суперпользователя, что позволяет вам редактировать его содержимое. Нас перекинет в текстовый редактор. Что-бы что-то изменить в тесковом редакторе нажмите insert на клавиатуре. Что бы сохранить что-то в этом документе нажимаем Esc пишем :wq! В этом текставом редакторе мы должны поставить node-exporter после services
sudo vi docker-compose.yaml
![image](https://github.com/user-attachments/assets/507f0509-b771-4865-b309-f4abac235823)
Отображаем список запущенных контейнеров и их статусов. Эта команда показываем статусы всех сервиров определённых в файле docker-compose.yml.
sudo docker-compose ps
![image](https://github.com/user-attachments/assets/04034dc1-03db-4754-be2d-26639ea053e0)

мы сделали команду sudo docker compose stop выполяем остановку контейниров. Данная команда останавливает работу запущенных контейнеров, но не удаляет их.

![image](https://github.com/user-attachments/assets/1e773e4b-42bc-42f3-b36d-d853f552cf2a)

После мы снова вводим sudo docker compose up -d. Данная команда запускает приложение в фоновом режиме, то есть, в этом режиме контейнеры будут работать в фоновом режиме, и вывод команды не будет отображаться в терминале.

![image](https://github.com/user-attachments/assets/b04f74fa-4364-4133-b165-fa85ff822881)

Используем ещё одну команду sudo docker compose down. Данаая команда останавливает и удаляет все контейнеры, а также тома, связанные с ними, определённые в файле docker-compose.yml.

![image](https://github.com/user-attachments/assets/9da406dc-f6ed-4f54-9d5a-bb2bfafedaa6)

Ещё раз поднимаем sudo docker compose up -d для того что бы запустить контейнеры.

![image](https://github.com/user-attachments/assets/8bc7f45a-ba24-49c3-889b-395e1300e93d)

Создаем папку 1 и переносим туда файлы docker-compose.yaml и prometeus.yaml

![image](https://github.com/user-attachments/assets/a11b4b9c-3fb3-4b51-97ae-a51c9789f6f4)

Команда создаёт на компьютере точную копию репозитория, включая все его данные, историю и ветви. Используется для клонирования удалённого репозитория git clone https://githab.com/GerasikinaD/GD.git
 



