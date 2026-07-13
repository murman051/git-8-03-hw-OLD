# Домашнее задание к занятию `«Ansible. Часть 2»` - `Литвинов Сергей`


### Инструкция по выполнению домашнего задания

   1. Сделайте `fork` данного репозитория к себе в Github и переименуйте его по названию или номеру занятия, например, https://github.com/имя-вашего-репозитория/git-hw или  https://github.com/имя-вашего-репозитория/7-1-ansible-hw).
   2. Выполните клонирование данного репозитория к себе на ПК с помощью команды `git clone`.
   3. Выполните домашнее задание и заполните у себя локально этот файл README.md:
      - впишите вверху название занятия и вашу фамилию и имя
      - в каждом задании добавьте решение в требуемом виде (текст/код/скриншоты/ссылка)
      - для корректного добавления скриншотов воспользуйтесь [инструкцией "Как вставить скриншот в шаблон с решением](https://github.com/netology-code/sys-pattern-homework/blob/main/screen-instruction.md)
      - при оформлении используйте возможности языка разметки md (коротко об этом можно посмотреть в [инструкции  по MarkDown](https://github.com/netology-code/sys-pattern-homework/blob/main/md-instruction.md))
   4. После завершения работы над домашним заданием сделайте коммит (`git commit -m "comment"`) и отправьте его на Github (`git push origin`);
   5. В личном кабинете прикрепите и отправьте ссылку на решение в виде md-файла в вашем Github.
   6. Любые вопросы по выполнению заданий спрашивайте в разделе “Вопросы по заданию” в личном кабинете.
   
Желаем успехов в выполнении домашнего задания!
   
### Дополнительные материалы, которые могут быть полезны для выполнения задания

1. [Руководство по оформлению Markdown файлов](https://gist.github.com/Jekins/2bf2d0638163f1294637#Code)


---

## Задание 1

### 1. Плейбук для скачивания и распаковки архива (`playbook_kafka.yml`)
```yaml
---
- name: Скачивание и распаковка локального архива
  hosts: webservers
  become: true
  tasks:

    - name: Создать целевую директорию на серверах
      ansible.builtin.file:
        path: /opt/my_unpacked_archive
        state: directory
        mode: '0755'

    - name: Копировать архив с управляющей машины и распаковать его
      ansible.builtin.unarchive:
        src: ./test_archive.tar.gz
        dest: /opt/my_unpacked_archive
        remote_src: false
```

### 2. Плейбук для установки и запуска tuned (`playbook_tuned.yml`)
```yaml
---
- name: Установка и настройка tuned
  hosts: webservers
  become: true
  tasks:

    - name: Установить пакет tuned
      ansible.builtin.apt:
        name: tuned
        state: present
        update_cache: true

    - name: Запустить tuned и добавить в автозагрузку
      ansible.builtin.service:
        name: tuned
        state: started
        enabled: true
```

### 3. Плейбук для изменения приветствия системы motd (`playbook_motd.yml`)
```yaml
---
- name: Изменение приветствия системы (motd)
  hosts: webservers
  become: true
  vars:
    custom_welcome_message: "Welcome to Sergey's Server! Managed by Ansible."

  tasks:
    - name: Изменить содержимое файла /etc/motd
      ansible.builtin.copy:
        content: "{{ custom_welcome_message }}\n"
        dest: /etc/motd
        mode: '0644'
```
