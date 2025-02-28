Отчет по виртуалке Перед началой установки, нужно установить Linux Oracle на VirtualBox, для этого нужно: Иметь образ Linux Выделить 2+ ядер. Выделать 4096+ МБ оперативы, и обязательно при установки мы выбираем английский язык. Мы переходим к установке docker с использованием grafana. Вводим следующий код: Устанавливаем утилиту Wget
Устанавливает утилиту wget на вашу систему
![image](https://github.com/user-attachments/assets/ecb64709-7db8-43d9-bfb1-0dfd27ebc1fd)
Устанавливаем утилиту Curl
![image](https://github.com/user-attachments/assets/e230a04d-22ed-442b-8178-6184a7df5498)
Скачиваем файл репозитория sudo wget -P /etc/yum.repos.d/ https://download.docker.com/linux/centos/docker-ce.repo
![image](https://github.com/user-attachments/assets/a5d80745-ce08-4678-b184-fdc7652c9932)
Установливаем dosker sudo yum install docker-ce docker-ce-cli containerd.io
![image](https://github.com/user-attachments/assets/d19e43e1-48f2-462a-b130-a0c7736a4330)
Мы разрешаем автозапуск и запускаем sudo systemctl enable docker --now
![image](https://github.com/user-attachments/assets/2d89d544-6ede-416c-9d3d-ffc2b6adaf54)

