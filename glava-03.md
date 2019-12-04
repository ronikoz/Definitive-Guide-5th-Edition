# Глава 3. Установка Asterisk

> Я долго шла к великим и благородным целям, но мой главный долг выполнять простые задачи так, как будто они великие и благородные. Мир движется не только мощными импульсами, которые дают ему герои, но также и крошечными толчками каждого трудящегося человека.
>
> -- Хелен Келлер

В этой главе мы рассмотрим установку Asterisk из исходного кода. Многие люди уклоняются от этого метода, утверждая, что это слишком сложно и отнимает много времени. Наша цель здесь - продемонстрировать, что установка Asterisk из исходного кода на самом деле не так уж сложна. Что еще более важно, мы хотим предоставить вам лучшую платформу Asterisk, на которой можно учиться.

В этой книге мы поможем вам построить функционирующую систему Asterisk с нуля. Для достижения этой цели в этой главе мы построим базовую платформу для вашей системы Asterisk. Поскольку мы устанавливаем из исходного кода, потенциально существует множество вариантов того, как вы можете это сделать. Наша цель - поставить стандартный вид платформы, подходящую для исследований во множестве областей. Можно снять звездочку до самых основ и запустить на очень слабой машине; однако это упражнение остается на усмотрение читателя. Процесс, который мы обсуждаем здесь, предназначен для быстрого и простого запуска, без кратковременного изменения доступа к интересным функциям.

Большинство команд, которые вы видите, будут лучше всего обрабатываться с помощью ряда операций копирования-вставки \(на самом деле, мы настоятельно рекомендуем Вам иметь электронную версию этой книги под рукой именно для этой цели\).[1](https://learning.oreilly.com/library/view/asterisk-the-definitive/9781492031598/ch03.html#idm46178409180936) Хотя это выглядит как большой набор команд, которые мы даем вам, получить конфигурацию можно от начала до конца менее чем за 30 минут, так что это действительно не так сложно, как может показаться. Мы запускаем некоторые предварительные условия, некоторую компиляцию и некоторую конфигурацию после установки, и Asterisk готов к работе.

Для краткости эти шаги будут выполнены на системе CentOS 7. Это функционально эквивалентно RHEL и достаточно похоже на Fedora, так что шаги должны быть очень похожи. Для других платформ, таких как Debian/Ubuntu и так далее, инструкции также будут аналогичны, но вам нужно будет настроить по мере необходимости для вашей платформы.[2](https://learning.oreilly.com/library/view/asterisk-the-definitive/9781492031598/ch03.html#idm46178409178472)

Первая часть инструкции по установке не будет касаться Asterisk как такового, а скорее некоторых зависимостей, которые требуются Asterisk или необходимы для некоторых более полезных функций \(таких как интеграция с базой данных\). Мы постараемся сохранить инструкции достаточно общими, чтобы они были полезны при любом распределении по вашему выбору.

В этих инструкциях предполагается, что вы являетесь опытным администратором Linux.[3](https://learning.oreilly.com/library/view/asterisk-the-definitive/9781492031598/ch03.html#idm46178409176504) Полностью работающая система Asterisk будет состоять из достаточно обособленных частей, так что вам будет сложно справиться со всем этим, если у вас мало или нет основ Linux. Мы по-прежнему рекомендуем вам погрузиться, но пожалуйста, учтите тот факт, что будет крутая кривая обучения, если у вас еще нет твердого опыта командной строки Linux.

{% hint style="info" %}
**Примечание**

Если вы хотите изучить командную строку Linux, одна из лучших книг, которые мы нашли - это _Командная строка Linux_ Уильяма Шоттса, которая была выпущена под лицензией Creative Commons и погружается прямо во все знания, необходимые для эффективного использования оболочки Linux. Её можно найти по адресу [linuxcommand.org](http://linuxcommand.org/) вы можете изучить книгу от начала до конца, и почти все, что вы узнаете будет тем, что стоит знать любому опытному администратору Linux.

Еще одна фантастическая книга - это, конечно же, легендарное _Руководство по системному администрированию UNIX и Linux_ Дэна Макина, Бена Уэйли, Трента Р. Хейна, Гарта Снайдера и Эви Немет \(Prentice Hall\). Настоятельно рекомендуем.
{% endhint %}

{% hint style="success" %}
#### Пакеты Asterisk

Существуют пакеты Asterisk, которые можно установить с помощью систем управления пакетами, таких как _yum_ или _apt-get_. Вы можете использовать их как только ознакомитесь с Asterisk.

Если вы используете RHEL, Asterisk доступен из [репозитория EPEL](http://fedoraproject.org/wiki/EPEL) из проекта Fedora. Пакеты Asterisk доступны в репозитории Юниверса для Ubuntu.
{% endhint %}

Вы также должны отметить, что из-за истории Asterisk он может интегрироваться со множеством технологий телефонии; однако новичку в Asterisk лучше сперва изучить интеграцию SIP, прежде чем беспокоиться о более сложных, устаревших или периферийных типах каналов. После того, как вы освоитесь с Asterisk в чистой среде SIP, вам будет намного проще смотреть на интеграцию других типов каналов.

{% hint style="success" %}
#### Проекты основанные на Asterisk

Многие проекты используют Asterisk в качестве базовой платформы. Некоторые из них, такие как графический интерфейс FreePBX, стали настолько популярными, что многие люди ошибочно принимают его за сам продукт Asterisk. На самом деле, графический интерфейс FreePBX настолько распространен, что его можно найти в большинстве известных проектов на основе Asterisk. Эти проекты берут базовый продукт Asterisk и добавляют веб-интерфейс администрирования, сложную базу данных и внешние функции, которые полезны в типичной УАТС \(например преднастройка \(provisioning\) аппаратов, сервер времени и т.д.\).

Мы решили не освещать эти проекты в этой книге, по нескольким причинам:

* Эта книга пытается, насколько это возможно, сосредоточиться на Asterisk и только Asterisk.
* О многих из этих проектов, основанных на Asterisk, уже написаны книги.
* Мы считаем, что если вы изучите Asterisk так, как мы будем учить вас, знания будут хорошо служить вам независимо от того, решите ли вы в конечном итоге использовать одну из этих предварительно упакованных версий Asterisk.
* Если вы хотите понять, что происходит под капотом системы на базе FreePBX, эта книга познакомит вас с некоторыми навыками, которые вам понадобятся.
* Для нас сила Asterisk заключается в том, что он не пытается решить ваши проблемы за вас. Эти проекты являются поистине удивительными примерами того, что можно построить с помощью Asterisk. Однако, если вы хотите создать свое собственное приложение Asterisk \(чем на самом деле является Asterisk\), эти проекты могут создавать ненужные препятствия, просто потому, что они направлены на упрощение процесса создания бизнес-АТС, а не на то, чтобы сделать возможным доступ к полному потенциалу платформы Asterisk.

Некоторые из самых популярных проектов на основе Asterisk включают \(в определенном порядке\):

[AsteriskNOW](http://www.asterisk.org/asterisknow)

Управляется Digium. Использует графический интерфейс FreePBX.

[Issabel](https://www.issabel.org/)

Ответвление оригинальных релизов open source продукта Elastix.[4](https://learning.oreilly.com/library/view/asterisk-the-definitive/9781492031598/ch03.html%22%20/l%20%22idm46178409149976) Использует графический интерфейс FreePBX.

[The Official FreePBX Distro](http://www.freepbx.org/freepbx-distro)

Официальный дистрибутив проекта FreePBX. Управляется Sangoma.

[Asterisk for Raspberry Pi](http://www.raspberry-asterisk.org/)

Полная установка Asterisk и FreePBX для Raspberry Pi.

[AstLinux](https://www.astlinux-project.org/)

Проект AstLinux обслуживает сообщество, которое хочет запускать Asterisk на небольших, маломощных твердотельных устройствах. Размер установки всего решения измеряется в мегабайтах \(AstLinux был первоначально разработан, чтобы поместиться на картах CompactFlash\). Если вы очарованы маленькими компьютерами и хотите играть с АТС в коробке, которая помещается в вашем кармане, AstLinux может быть вам интересен.

Мы рекомендуем вам проверить их.[5](https://learning.oreilly.com/library/view/asterisk-the-definitive/9781492031598/ch03.html%22%20/l%20%22idm46178409144568)
{% endhint %}

## Установка Linux

Asterisk разрабатывается с использованием Linux, и если вам не очень удобно переносить программное обеспечение между различными платформами, то вам необходимо использовать Linux.

В этой книге мы будем использовать CentOS в качестве платформы. Если вы предпочитаете другой дистрибутив Linux, ожидается, что у вас есть достаточные навыки его администрирования чтобы понять, что некоторые из различий могут быть. В наши дни так легко и дешево запустить экземпляр любого общего дистрибутива, что нет никакого реального вреда в использовании CentOS для обучения, а затем перейти на то, что предпочтете вы, когда будете готовы.

Мы рекомендуем установить минимальную версию CentOS, так как в процессе установки мы будем проходить через обработку всех необходимых условий. Это также гарантирует, что вы не устанавливаете ничего лишнего.

### Выбор вашей платформы

Итак, строго говоря, мы уже выбрали вашу платформу для вас, но есть несколько различных способов запустить сервер CentOS \(см. [Таблицу 3-1](https://github.com/Krotesk1/definitive-guide-5th/tree/25dd8a8bd31ab9128242e9ca42e5fc842f433f2f/3.%20Installing%20Asterisk%20-%20Asterisk%20%20The%20Definitive%20Guide,%205th%20Edition.htm%22%20/l%20%22table03linux/README.md)\).

Таблица 3-1. Сравнение платформ Linux, подходящих для Asterisk

| Платформа | Перспективы | Учтите |
| :--- | :--- | :--- |
| OpenStack \(DigitalOcean, Linode, VULTR, etc.\) | Через несколько минут все будет готово. Недорогой в эксплуатации. Не требует никаких ресурсов в вашей локальной системе. Доступен из любой точки мира. Может использоваться в продакшене. Фантастический для быстрого прототипирования проектов. | Вы платите, пока он работает. IP-адрес является вашим только до тех пор, пока система работает. Требуются некоторые навыки DevOps, если вы хотите развернуть в продакшене. По умолчанию брандмауэр отсутствует. |
| VirtualBox \(or other PC-based platform\) | Бесплатен в использовании. Нет внешнего воздействия. Отлично подходит для небольших лабораторных проектов. | Требует больше лошадиных сил в вашей системе. Требуется место для хранения в локальной системе. Не так просто развернуть в рабочей среде. |
| AWS and/or Lightsail | Недорогой в эксплуатации. Не требует никаких ресурсов в вашей локальной системе. Доступен из любой точки мира. Может использоваться в производственной среде. Вес до огромных размеров. | Вы платите, пока он работает. Несколько больше навыков требуется, чтобы собрать все необходимые ресурсы. |
| Физическое оборудование | Выделенная платформа. Может быть загружено и установлено везде. Полный контроль над всеми аспектами окружающей среды, оборудования, сети и так далее. | Риск отказа компонентов. Потребляемая мощность. Шум. Потенциальные затраты на хостинг. Не присуща избыточность. |
| Другое \(на самом деле все, что будет работать с CentOS 7, должно быть в порядке\) | Вы можете использовать среду, с которой вы знакомы. | Вы сам себе хозяин. |
| Другие Linux \(на самом деле вам не нужно запускать CentOS\) | Вы можете запустить такую среду, которую захотите. | Вы должны иметь хорошие навыки администрирования Linux. |

Для целей обучения мы рекомендуем один из двух простых способов начать работу:

* _Если вы используете Windows в качестве рабочего стола:_ загрузите VirtualBox, затем загрузите CentOS 7 Minimal ISO и установите на локальном компьютере.
* _Если вам удобно работать с подключениями на основе SSH с ключами к удаленным системам:_ создайте размещенную систему \(например, DigitalOcean CentOS droplet\).

Эта книга была разработана и протестирована с использованием как VirtualBox, так и DigitalOcean.

### Шаги для VirtualBox

Загрузите Minimal ISO с веб-сайта [Centos](https://www.centos.org/download/).

Возьмите копию VirtualBox с [веб-сайта платформы](https://www.virtualbox.org/wiki/Downloads) и установите ее.

Загрузите [PuTTY](http://bit.ly/2J0ftwK) если используете Windows.

* Тип: Linux
* Версия: Red Hat \(64-bit\)
* Объем памяти: 2048 MB
* Жёсткий диск: Создать новый виртуальный жесткий диск
* Расположение файла: выберите подходящее место для хранения образов виртуальных машин
* Размер файла: 16 ГБ подходит для наших целей, но для продакшена потребуется куда больше

Создайте новую виртуальную машину со следующими характеристиками:

* Носители: под пунктом **Носители, Контроллер: IDE** ...
  1. Вы должны увидеть крошечный значок диска CD/DVD с надписью **Пусто**.
  2. Нажмите на него и справа под **Атрибуты** появится еще один значок крошечного диска.
  3. Нажмите на него, и он попросит вас **выбрать образ оптического диска**.
  4. Найдите на жестком диске Minimal ISO, загруженный с CentOS и выберите его.
  5. Теперь трей носителей должен отображать CentOS ISO.
* Сеть: Адаптер 1

Attached to: **Bridged Adapter**

Как только базовая машина будет определена, вам нужно будет настроить ее следующим образом:

* Дата и время: при желании можно настроить часовой пояс.
* Сеть и имя хоста: **Ethernet** - переключите с off на **on** \(он должен немедленно захватить IP-адрес из вашей сети; если нет, установите вручную\). Нажмите кнопку **Готово**.
* Installation destination: It may require you to confirm the target, but you shouldn’t need to change anything. Press the Done button.
* Вот и все. **Начать установку**.

Во время установки установите пароль root, а также создайте пользователя с именем `astmin`. Сделайте `astmin` администратором.

Установка займет несколько минут. Выпейте кофе!

После завершения установки программа установки попросит вас перезагрузиться. Перезагрузка должна занять всего 15 секунд или около того.

Поздравляем, ваша система готова. Войдите в систему как `root`.

Запустите только что созданную машину, и она проведет вас через базовую установку CentOS. Вот несколько элементов, которые вы хотите указать \(для всего остального, оставляйте значения по умолчанию\):

### Linux \(OpenStack\) Host

Очевидно, вам понадобится учетная запись на хостинге Linux-провайдера, если вы собираетесь использовать этот метод \(мы обнаружили, что предложения на основе OpenStack являются самыми дешевыми с предлагаемыми качеством/производительностью/простотой\). Мы использовали DigitalOcean в течение многих лет, но также обнаружили, что Linode и VULTR являются отличными поставщиками услуг в этом пространстве.[6](https://learning.oreilly.com/library/view/asterisk-the-definitive/9781492031598/ch03.html#idm46178409086616) После сортировки вы можете войти в систему и создать новую систему примерно следующим образом:

* CentOS 7 \(последняя версия, 64-бит\)
* 4 Гб 2vCPU \(нам действительно не нужно 4 ГБ оперативной памяти, но хорошо иметь 2xCPU; вы, вероятно, можете уйти с 2 ГБ 1xCPU, если вы действительно сознательны в расходах\)
* Data center closest to you

Как только это будет запущено, войдите в систему как пользователь по умолчанию \(на момент написания этой статьи это `centos`\).

{% hint style="danger" %}
**ПРЕДУПРЕЖДЕНИЕ**

Обратите внимание, что экземпляры DigitalOcean по умолчанию не имеют файрволла. Вместо этого они предоставляют его как часть своей среды. Таким образом, система, которую вы создадите, не будет иметь собственного файрволла и будет подвергаться внешним атакам сразу после завершения настройки \(вы увидите это в консоли Asterisk\). У разных провайдеров будут разные политики брандмауэра. Вы несете ответственность за то, чтобы ваш файрволл работал правильно. Мы обсудим вопросы безопасности и борьбы с мошенничеством более подробно позже в этой книге.
{% endhint %}

## Зависимости

Система, которую вы только что построили, на самом деле не намного больше, чем базовая загрузочная система. Для того, чтобы подготовить её к установке Asterisk, есть несколько вещей, которые нам нужно будет установить в первую очередь.

Следующие команды можно ввести из командной строки или добавить в простой скрипт оболочки и запустить таким образом.

```text
sudo yum -y update &&
sudo yum -y install epel-release &&
sudo yum -y install python-pip &&
sudo yum -y install vim wget dnf&&
sudo pip install alembic ansible &&
sudo pip install --upgrade pip &&
sudo mkdir /etc/ansible &&
sudo chown astmin:astmin /etc/ansible &&
sudo echo "[starfish]" >> /etc/ansible/hosts &&
sudo echo "localhost ansible_connection=local" >> /etc/ansible/hosts &&
mkdir -p ~/ansible/playbooks
```

Мы установили Ansible просто потому, что это быстрый и простой способ получить все зависимости. Мы написали playbook для выполнения следующих операций:

1. Установка `dnf`, `vim`, `wget`, и `MySQL-python`.
2. Установка репозитория MySQL community-edition.
3. Установка `mysql-server`.
4. Настройка некоторых переменных при установке `mysql-server`.
5. Запуск демона mysql-server.
6. Изменение некоторых данных MySQL \(создание пользователей, установка паролей\).
7. Создание базы данных/схемы MySQL для использования в Asterisk.
8. Применение некоторых рекомендаций по безопасности \(удаление анонимного пользователя, тестовая база данных и т.д.\).
9. Создание пользователя `asterisk`.
10. Создание пользователя `astmin`.
11. Установка зависимостей для ODBC.
12. Установка некоторых диагностических инструментов.
13. Настройка брандмауэра для разрешения трафика SIP и RTP.
14. Редактирование некоторых файлов конфигурации ODBC.

Все это можно сделать вручную, но это просто много для набора и Ansible действительно хорош в оптимизации этого процесса.

Создайте план Ansible в файле _~/ansible/playbooks/starfish.yml_.

{% hint style="info" %}
**Примечание**

Файл _libmyodbc8a.so_ является версионным, поэтому, если у вас нет версии 8 UnixODBC:

1. Запустите playbook в первый раз \(чтобы установить библиотеку UnixODBC\).
2. Узнайте, какой файл был установлен в _/usr/lib64/libmyodbc&lt;version&gt;a.so_.
3. Отредактируйте playbook соответствующим образом \(укажите правильное имя файла\).
4. Сохраните и повторно запустите playbook \(который затем обновит файлы конфигурации, чтобы указать на правильную библиотеку\).
{% endhint %}

Вот тебе план:

```text
---
- hosts: starfish
  become: yes
  vars:
# Use these on the first run of this playbook
    current_mysql_root_password: ""
    updated_mysql_root_password: "YouNeedAReallyGoodPassword"
    current_mysql_asterisk_password: ""
    updated_mysql_asterisk_password: "YouNeedAReallyGoodPasswordHereToo"
# Comment the above out after the first run

# Uncomment these for subsequent runs
#    current_mysql_root_password: "YouNeedAReallyGoodPassword"
#    updated_mysql_root_password: "{{ current_mysql_root_password }}"
#    current_mysql_asterisk_password: "YouNeedAReallyGoodPasswordHereToo"
#    updated_mysql_asterisk_password: "{{ current_mysql_asterisk_password }}"

  tasks:

  - name: Install epel-release
    dnf:
      name: epel-release
      state: present

  - name: Install dependencies
    dnf:
      name: ['vim', 'wget', 'MySQL-python']
      state: present

  - name: Install the MySQL repo.
    dnf:
      name: http://repo.mysql.com/mysql-community-release-el7-5.noarch.rpm
      state: present

  - name: Install mysql-server
    dnf:
      name: mysql-server
      state: present

  - name: Override variables for MySQL (RedHat).
    set_fact:
      mysql_daemon: mysqld
      mysql_packages: ['mysql-server']
      mysql_log_error: /var/log/mysqld.err
      mysql_syslog_tag: mysqld
      mysql_pid_file: /var/run/mysqld/mysqld.pid
      mysql_socket: /var/lib/mysql/mysql.sock
    when: ansible_os_family == "RedHat"

  - name: Ensure MySQL server is running
    service:
      name: mysqld
      state: started
      enabled: yes

  - name: update mysql root pass for localhost root account from local servers
    mysql_user:
      login_user: root
      login_password: "{{ current_mysql_root_password }}"
      name: root
      host: "{{ item }}"
      password: "{{ updated_mysql_root_password }}"
    with_items:
        - localhost

  - name: update mysql root password for all other local root accounts
    mysql_user:
      login_user: root
      login_password: "{{ updated_mysql_root_password }}"
      name: root
      host: "{{ item }}"
      password: "{{ updated_mysql_root_password }}"
    with_items:
        - "{{ inventory_hostname }}"
        - 127.0.0.1
        - ::1
        - localhost.localdomain

  - name: create asterisk database
    mysql_db:
      login_user: root
      login_password: "{{ updated_mysql_root_password }}"
      name: asterisk
      state: present

  - name: asterisk mysql user
    mysql_user:
      login_user: root
      login_password: "{{ updated_mysql_root_password }}"
      name: asterisk
      host: "{{ item }}"
      password: "{{ updated_mysql_asterisk_password }}"
      priv: "asterisk.*:ALL"
    with_items:
        - "{{ inventory_hostname }}"
        - 127.0.0.1
        - ::1
        - localhost
        - localhost.localdomain

  - name: remove anonymous user
    mysql_user:
      login_user: root
      login_password: "{{ updated_mysql_root_password }}"
      name: ""
      state: absent
      host: "{{ item }}"
    with_items:
        - localhost
        - "{{ inventory_hostname }}"
        - 127.0.0.1
        - ::1
        - localhost.localdomain

  - name: remove test database
    mysql_db:
      login_user: root
      login_password: "{{ updated_mysql_root_password }}"
      name: test
      state: absent

  - user:
      name: asterisk
      state: present
      createhome: yes

  - group:
      name: asterisk
      state: present

  - user:
      name: astmin
      groups: asterisk,wheel
      state: present

  - name: Install other dependencies
    dnf:
      name: 
      - unixODBC
      - unixODBC-devel
      - mysql-connector-odbc
      - MySQL-python
      - tcpdump
      - ntp
      - ntpdate
      - jansson
      - bind-utils
    state: present

#   Tweak the firewall for UDP/SIP
  - firewalld:
      port: 5060/udp
      permanent: true
      state: enabled

#   Tweak firewall for UDP/RTP
  - firewalld:
      port: 10000-20000/udp
      permanent: true
      state: enabled

  - name: Ensure NTP is running
    service:
      name: ntpd
      state: started
      enabled: yes

# The libmyodbc8a.so file is versioned, so if you don't have version 8, see what the
# /usr/lib64/libmyodbc<version>a.so file is, and refer to that instead 
# on your 'Driver64' line, and then run the playbook again
  - name: update odbcinst.ini
    lineinfile:
      dest: /etc/odbcinst.ini
      regexp: "{{ item.regexp }}"
      line: "{{ item.line }}"
      state: present
    with_items:
      - regexp: "^Driver64"
        line: "Driver64 = /usr/lib64/libmyodbc8a.so"
      - regexp: "^Setup64"
        line: "Setup64 = /usr/lib64/libodbcmyS.so"

  - name: create odbc.ini
    blockinfile:
      path: /etc/odbc.ini
      create: yes
      block: |
        [asterisk]
        Driver = MySQL
        Description = MySQL connection to 'asterisk' database
        Server = localhost
        Port = 3306
        Database = asterisk
        UserName = asterisk
        Password = {{ updated_mysql_asterisk_password }}
        #Socket = /var/run/mysqld/mysqld.sock
        Socket = /var/lib/mysql/mysql.sock
...
```

Запустите playbook с помощью следующей команды:

```text
$ ansible-playbook ~/ansible/playbooks/starfish.yml
```

Сядьте поудобнее и наблюдайте, как происходит волшебство.

Как только Ansible выполнит назначенные задачи, убедитесь что ODBC может подключиться к базе данных с использованием учетных данных пользователя `asterisk`.

```text
$ echo "select 1" | isql -v asterisk asterisk password
```

Вы должны увидеть результат вроде этого:

```text
+---------------------------------------+
| Connected!                            |
| sql-statement                         |
| help [tablename]                      |
| quit                                  |
+---------------------------------------+
SQL> select 1
+---------------------+
| 1                   |
+---------------------+
| 1                   |
+---------------------+
SQLRowCount returns 1
1 rows fetched
```

Если вы не видите сообщение `Connected!`, значит вам необходимо устранить неполадки в вашей базе данных и установке ODBC. Первое, что вы должны сделать - это убедиться, что можете войти в базу данных из командной строки с помощью пользователя `asterisk` \(`mysql -u asterisk -p`\). Большинство проблем ODBC, как правило, заканчиваются проблемами с учетными данными \(т.е. неправильным паролем или именем пользователя\), поэтому вернитесь назад, чтобы убедиться, что все учетные данные работают так, как должны, и дважды проверьте, что вы не получили никаких сообщений об ошибках от Ansible.

На момент написания этой статьи версия _jansson_, установленная из репозитория EPEL, является более старой версией, чем требуется для Asterisk, поэтому придется установить ее вручную.

Теперь система готова, и мы готовы загрузить и установить Asterisk.

## Установка Asterisk

Asterisk официально поставляется в виде архива \(как исходный код\), и его необходимо загрузить, извлечь и скомпилировать.[7](https://learning.oreilly.com/library/view/asterisk-the-definitive/9781492031598/ch03.html#idm46178409043048) Это не трудно сделать когда у вас удовлетворены все зависимости. Между написанием этой книги и вашим чтением ее, возможно, были некоторые изменения в различных зависимостях, поэтому вам процесс установки, возможно, придется запускать немного иначе. Часто бывает трудно понять разницу между сообщением об ошибке, которое можно безопасно проигнорировать, и сообщением, указывающим на критическую проблему; однако, как правило, вы должны были выявить и устранить все ошибки в предыдущих процессах, прежде чем приступать к этому шагу. Если ваши зависимости отсортированы, установка Asterisk будет проходить гладко.

### Загрузка и необходимые компоненты

Выйдите из системы и снова войдите в систему как пользователь `astmin`.[8](https://learning.oreilly.com/library/view/asterisk-the-definitive/9781492031598/ch03.html#idm46178409035896)

Введите следующие команды в командной строке чтобы загрузить исходный код Asterisk:

{% hint style="info" %}
**Примечание**

Когда вы видите, что мы указываем &lt;TAB&gt; в имени файла, то мы имеем в виду, что вы должны нажать клавишу Tab на клавиатуре и разрешить автозаполнение возможными вариантами. Затем следует остальная часть набора текста.
{% endhint %}

```text
$ mkdir ~/src
$ cd ~/src
$ wget https://downloads.asterisk.org/pub/telephony/asterisk/asterisk-16-current.tar.gz
$ tar zxvf asterisk-16-current.tar.gz
$ cd asterisk-16.<TAB> # вкладка должна заполниться автоматически (если она не имеет более одного совпадения)
```

Теперь мы можем выполнить несколько предварительных условий, определенных командой Asterisk, а также проверить среду:

```text
$ cd contrib/scripts (or cd ~/src/asterisk-16.<TAB>/contrib/scripts
$ sudo ./install_prereq install # asterisk has a few prerequisites that this simplifies
$ cd ../..
$ ./configure --with-jansson-bundled
```

Asterisk теперь готов к компиляции и установке, но есть несколько настроек, которые стоит внести в конфигурацию перед компиляцией.

### Компиляция и установка

```text
$ make menuselect
```

Вы увидите меню, в котором представлены различные параметры, которые можно выбрать для компилятора. Для перемещения используйте клавиши со стрелками и Tab, а для выбора/отмены выбора - клавишу Enter. По большей части, значения по умолчанию должны быть в порядке, но мы хотим сделать несколько настроек для звуковых файлов, чтобы убедиться, что у нас есть все звуки, которые мы хотим, в лучшем формате.

{% hint style="info" %}
**Примечание**

На этом этапе вы также можете выбрать другие языки, которые захотите иметь в своей системе. Мы рекомендуем вам выбрать форматы WAV и G722 \(а также G729, если вам необходимо его поддерживать\).
{% endhint %}

Под Codec Translators \(`--- External ---`\):

* Выбрать `[*] codec_opus`
* Выбрать `[*] codec_silk`
* Выбрать `[*] codec_siren7`
* Выбрать `[*] codec_siren14`
* Выбрать `[*] codec_g729a`

Под Core Sound Packages:

* Убрать `[*] CORE-SOUNDS-EN-GSM`
* Выбрать `[*] CORE-SOUNDS-EN-WAV`
* Выбрать `[*] CORE-SOUNDS-EN-G722`

Под Extras Sound Packages:

* Выбрать `[*] EXTRA-SOUNDS-EN-WAV`
* Выбрать `[*] EXTRA-SOUNDS-EN-G722`

Save and Exit \(Сохранить и выйти\).

Еще три команды и Asterisk установлена:

```text
$ make              # это займет несколько минут
                    # (в зависимости от скорости вашей системы)
$ sudo make install # вы должны запустить это с повышенными привилегиями
$ sudo make config  # и это
```

{% hint style="danger" %}
**Предупреждение**

По завершении выполнения команды `make config` будут предложены некоторые команды для установки примеров файлов конфигурации. Для целей этой книги, _вы **не** должны этого делать_. Мы будем создавать необходимые файлы вручную, поэтому примеры файлов будут служить только тому, чтобы нарушить и запутать этот процесс. Сказав это, отметим что примеры файлов полезны, и мы будем упоминать их на протяжении всей этой книги, так как они являются отличным справочным материалом.
{% endhint %}

_Перезагрузите систему._

Как только загрузка будет завершена, войдите в систему как пользователь `astmin` и временно установите SELinux на `Permissive` \(после каждой загрузки он будет возвращаться к `Enforcing`, поэтому до тех пор, пока мы не разобрались с частью установки SELinux, это должно происходить на каждой загрузке\):

```text
$ sudo setenforce Permissive
$ sudo sestatus
```

Это должно показать `Current mode: permissive`

Убедитесь, что Asterisk работает со следующей командой:

```text
$ ps -ef | grep asterisk
```

Вы можете увидеть, что демон `/user/sbin/asterisk` запущен \(в настоящее время как пользователь `root`, но мы исправим это в ближайшее время\). Asterisk теперь установлен и работает; однако, есть несколько параметров конфигурации, которые нам нужно сделать, прежде чем система будет полезна.

### Начальная конфигурация

По умолчанию Asterisk сохраняет свои конфигурационные файлы в директории _/etc/asterisk_. Сам процесс Asterisk для запуска не нуждается в каких-либо файлах конфигурации; однако он пока не будет использоваться, поскольку ни одна из функций, которые он предоставляет, не была указана. Сейчас мы займемся некоторыми задачами начальной настройки.

{% hint style="info" %}
**Примечание**

Файлы конфигурации Asterisk используют символ точки с запятой \(;\) для комментариев, главным образом потому, что хэш-символ \(\#\) является допустимым символом на телефонной клавиатуре.
{% endhint %}

Файл _modules.conf_ дает вам полный контроль над тем, какие модули Asterisk будут \(и не будут\) загружаться. Обычно нет необходимости явно определять каждый модуль в этом файле, но вы можете это сделать если захотите. Мы собираемся создать очень простой файл, как например:

```text
$ sudo chown asterisk:asterisk /etc/asterisk ; sudo chmod 664 /etc/asterisk
$ sudo -u asterisk vim /etc/asterisk/modules.conf
[modules]
autoload=yes
preload=res_odbc.so
preload=res_config_odbc.so
```

Мы используем ODBC для загрузки многих конфигураций других модулей, и нам нужно, чтобы этот коннектор был доступен до того, как Asterisk попытается загрузить что-либо еще, поэтому мы предварительно загрузим его.

Далее, мы собираемся подкорректировать файл _logger.conf_ предоставляемый по умолчанию.

```text
$ sudo -u asterisk vim /etc/asterisk/logger.conf
[general]
exec_after_rotate=gzip -9 ${filename}.2;
[logfiles]
;debug => debug
;security => security
console => notice,warning,error,verbose
;console => notice,warning,error,debug
messages => notice,warning,error
full => notice,warning,error,debug,verbose,dtmf,fax
;full-json => [json]debug,verbose,notice,warning,error,dtmf,fax
;syslog keyword : This special keyword logs to syslog facility
;syslog.local0 => notice,warning,error
```

Вы заметите, что многие строки закомментированы. Они здесь для справки, потому что как выяснится при отладке системы вы можете часто настраивать этот файл. Мы выяснили, что лучше подготовить и закомментировать несколько строк, чем искать синтаксис каждый раз.

Следующий файл, _asterisk.conf,_ определяющий различные директории, необходимые для нормальной работы, а также параметры, необходимые для запуска от имени пользователя `asterisk`:

```text
$ sudo -u asterisk vim /etc/asterisk/asterisk.conf
[directories](!)
astetcdir => /etc/asterisk
astmoddir => /usr/lib/asterisk/modules
astvarlibdir => /var/lib/asterisk
astdbdir => /var/lib/asterisk
astkeydir => /var/lib/asterisk
astdatadir => /var/lib/asterisk
astagidir => /var/lib/asterisk/agi-bin
astspooldir => /var/spool/asterisk
[options]
astrundir => /var/run/asterisk
astlogdir => /var/log/asterisk
astsbindir => /usr/sbin
runuser = asterisk ; The user to run as. The default is root.
rungroup = asterisk ; The group to run as. The default is root
```

Мы настроим остальные файлы позже, а пока это все, что нам нужно на данный момент.

Давайте обновим права владельца на файлы, чтобы пользователь `asterisk` имел к ним надлежащий доступ.

```text
$ sudo chown -R asterisk:asterisk {/etc,/var/lib,/var/spool,/var/log,/var/run}/asterisk
```

Также может понадобится добавить правило в директорию _/etc/tmpfiles.d_ для разрешения Asterisk создавать сокет во время выполнения.

```text
$ sudo vim /etc/tmpfiles.d/asterisk.conf
d /var/run/asterisk 0775 asterisk asterisk
```

\(Смотри `man tmpfiles.d` для большей информации\)

Далее мы инициализируем базу данных таблицами, необходимыми Asterisk для настройки на основе ODBC.

Исходные файлы Asterisk включают в себя вклад, который люди Digium поддерживают как часть Asterisk для управления версиями необходимых таблиц базы данных. Это значительно упрощает поддержание корректности базы данных в процессе обновления.

Перейдите в соответствующий каталог и сделайте копию файла конфигурации.

```text
$ cd ~/src/asterisk-16.<TAB>/contrib/ast-db-manage
$ cp config.ini.sample config.ini
```

Теперь мы собираемся открыть файл и предоставить ему учетные данные для нашей базы данных \(которые определены в Ansible playbook с именем _starfish.yml_, под переменной `current_mysql_asterisk_password`, которую мы использовали в начале этой главы\):

```text
$ vim config.ini
```

Найдите строки, которые выглядят примерно так:

```text
#sqlalchemy.url = postgresql://user:pass@localhost/asterisk
sqlalchemy.url = mysql://user:pass@localhost/asterisk

# Logging configuration
[loggers]
keys = root,sqlalchemy,alembic
```

Сделайте её копию, раскомментируйте и отредактируйте с правильными учетными данными:

```text
#sqlalchemy.url = postgresql://user:pass@localhost/asterisk
#sqlalchemy.url = mysql://user:pass@localhost/asterisk
sqlalchemy.url = mysql://asterisk:YouNeedAReallyGoodPasswordHereToo@localhost/asterisk

# Logging configuration
[loggers]
keys = root,sqlalchemy,alembic
```

Теперь, с этой очень простой конфигурацией мы можем использовать Alembic для автоматической настройки базы данных идеальной для Asterisk \(это было несколько болезненно делать в прошлых версиях Asterisk\):

```text
$ alembic -c ./config.ini upgrade head
```

{% hint style="info" %}
**Подсказка**

Alembic не используется Asterisk, поэтому конфигурация, которую вы только что выполнили, не позволяет Asterisk использовать базу данных. Все, что он делает, это запускает скрипт, который создает схему и таблицы, которые будет использовать Asterisk \(вы также можете сделать это вручную, но поверьте нам, лучше чтобы Alembic справился с этим\). Это часть процесса установки/обновления. _Это особенно полезно, если у вас есть живые таблицы с реальными данными в них, и вы хотите обновить и сохранить соответствующую конфигурацию._
{% endhint %}

Войдите в базу данных и просмотрите все созданные таблицы:

```text
$ mysql -u asterisk -p
mysql> use asterisk;
mysql> show tables;
```

Вы должны увидеть список, похожий на этот:

```text
| alembic_version_config      |
| extensions                  |
| iaxfriends                  |
| meetme                      |
| musiconhold                 |
| ps_aors                     |
| ps_asterisk_publications    |
| ps_auths                    |
| ps_contacts                 |
| ps_domain_aliases           |
| ps_endpoint_id_ips          |
| ps_endpoints                |
| ps_globals                  |
| ps_inbound_publications     |
| ps_outbound_publishes       |
| ps_registrations            |
| ps_resource_list            |
| ps_subscription_persistence |
| ps_systems                  |
| ps_transports               |
| queue_members               |
| queue_rules                 |
| queues                      |
| sippeers                    |
| voicemail                   |
```

Мы пока не собираемся ничего настраивать в базе данных. Сначала нам нужно сделать еще несколько базовых настроек.

Выйдите из CLI базы данных.

Теперь, когда у нас есть структура базы данных для обработки конфигурации Asterisk, мы расскажем Asterisk, как подключиться к базе данных.

```text
$ sudo -u asterisk vim /etc/asterisk/res_odbc.conf
```

Еще раз, вам понадобятся учетные данные, которые вы определили в своем Ansible playbook.

```text
[asterisk]
enabled => yes
dsn => asterisk
username => asterisk
password => YouNeedAReallyGoodPasswordHereToo
pre-connect => yes
```

### Настройки SELinux

Мы собираемся установить некоторые инструменты SELinux и внести изменения в конфигурацию SELinux для правильной загрузки системы.

{% hint style="info" %}
**Примечание**

Общий подход состоит в том, чтобы просто отредактировать _/etc/selinux/config_ и установить `enforcing=disabled.` Мы не рекомендуем этого делать, т.к. это полностью отключает SELinux и делает следующие шаги ненужными.
{% endhint %}

```text
$ sudo dnf -y install setools setroubleshoot setroubleshoot-server
$ sudo vim /etc/selinux/config
```

Вы собираетесь изменить строку `SELINUX=enforcing` на `SELINUX=permissive`. Это гарантирует, что файлы журналов будут показывать потенциальные ошибки SELinux, не блокируя соответствующие процессы.

Далее мы передадим Asterisk право собственности на файл _/etc/odbc.ini_.

```text
$ sudo chown asterisk:asterisk /etc/odbc.ini
$ sudo semanage fcontext -a -t asterisk_etc_t /etc/odbc.ini
$ sudo restorecon -v /etc/odbc.ini
$ sudo ls -Z /etc/odbc.ini
```

Если все хорошо, то вы должны увидеть, что контекст для этого файла был установлен в `asterisk_etc_t`:

```text
-rw-r--r--. asterisk asterisk system_u:object_r:asterisk_etc_t:s0 /etc/odbc.ini
```

Есть еще несколько ошибок SELinux, которые мы видели здесь во время написания книги. Они могут быть исправлены к тому времени, когда вы читаете это, но не должно быть никакого вреда в их появлении:

```text
$ sudo /sbin/restorecon -vr /var/lib/asterisk/*
$ sudo /sbin/restorecon -vr /etc/asterisk*
```

Перезагрузите систему и проверьте журнал на наличие каких-либо ошибок SELinux, прежде чем мы установим его в `enforcing`.

```text
$ sudo grep -i sealert /var/log/messages
```

Там может быть несколько сообщений, жалующихся на то, что Asterisk не нужно \(например, скрытый файл с именем _.odbc.ini_\), но до тех пор, пока он не полон ошибок о всевозможных важных компонентах Asterisk, всё должно быть хорошо. Последнее, что вам нужно изменить - это SELinux Boolean, позволяющее Asterisk создавать TTY.

```text
$ sudo setsebool -P daemons_use_tty 1
```

Отредактируйте файл _/etc/selinux/config_ еще раз, на этот раз установив `SELINUX=enforcing`. Сохраните и перезагрузите еще раз.

Убедитесь что Asterisk запущен \(от пользователя `asterisk`\).

```text
$ ps -ef | grep asterisk
asterisk 3992 3985 0 16:38 ? 00:00:01 /usr/sbin/asterisk -f -vvvg -c
```

Хорошо, мы почти закончили с установкой.

### Настройки файрволла

Мы сделаем пару настроек файрволла чтобы подготовить нашу систему для SIP \(и безопасности SIP\).

```text
$ sudo firewall-cmd --zone=public --add-service=sip --permanent
$ sudo firewall-cmd --zone=public --add-service=sips --permanent
```

### Финишные настройки

Ваша система Asterisk готова к запуску.

Давайте поместим некоторые исходные данные в конфигурационные файлы, чтобы в следующей главе мы могли начать работать с нашей новой системой Asterisk.

Поскольку мы собираемся использовать канал PJSIP для всех вызовов, мы укажем Asterisk искать конфигурацию PJSIP в базе данных:

```text
$ sudo -u asterisk vim /etc/asterisk/sorcery.conf

[res_pjsip] ; Мастер настройки Realtime PJSIP
; в конечном итоге больше модулей будут использовать волшебство 
; для подключения к базе данных, но в настоящее время только PJSIP использует это
endpoint=realtime,ps_endpoints
auth=realtime,ps_auths
aor=realtime,ps_aors
domain_alias=realtime,ps_domain_aliases
contact=realtime,ps_contacts

[res_pjsip_endpoint_identifier_ip]
identify=realtime,ps_endpoint_id_ips

$ sudo -u asterisk vim /etc/asterisk/extconfig.conf

[settings] ; старый механизм для подключения всех других модулей к базе данных
ps_endpoints => odbc,asterisk
ps_auths => odbc,asterisk
ps_aors => odbc,asterisk
ps_domain_aliases => odbc,asterisk
ps_endpoint_id_ips => odbc,asterisk
ps_contacts => odbc,asterisk

$ sudo -u asterisk vim /etc/asterisk/modules.conf

[modules]
autoload=yes
preload=res_odbc.so
preload=res_config_odbc.so
noload=chan_sip.so ; устаревший SIP-модуль с прошлых дней
```

Теперь мы должны поместить один бит конфигурации в файл _pjsip.conf_, определяющий механизм транспорта.

```text
$ sudo -u asterisk vim /etc/asterisk/pjsip.conf
[transport-udp]
type=transport
protocol=udp
bind=0.0.0.0
```

Наконец, давайте войдем в базу данных и определим некоторые примеры конфигураций для PJSIP:

```text
$ mysql -D asterisk -u asterisk -p

mysql>

insert into asterisk.ps_aors (id, max_contacts) values ('0000f30A0A01', 1);
insert into asterisk.ps_aors (id, max_contacts) values ('0000f30B0B02', 1);
insert
  into asterisk.ps_auths
  (id, auth_type, password, username)
  values
  ('0000f30A0A01', 'userpass', 'not very secure', '0000f30A0A01');
insert
  into asterisk.ps_auths
  (id, auth_type, password, username)
  values
  ('0000f30B0B02', 'userpass', 'hardly to be trusted', '0000f30B0B02');
insert
  into asterisk.ps_endpoints
  (id, transport, aors, auth, context, disallow, allow, direct_media)
  values
  ('0000f30A0A01', 'transport-udp', '0000f30A0A01', '0000f30A0A01',
  'sets', 'all', 'ulaw', 'no');
insert
  into asterisk.ps_endpoints
  (id, transport, aors, auth, context, disallow, allow, direct_media)
  values
  ('0000f30B0B02', 'transport-udp', '0000f30B0B02', '0000f30B0B02',
  'sets', 'all', 'ulaw', 'no');
exit
```

Давайте перезагрузимся, а затем войдем в нашу новую систему Asterisk и посмотрим, что мы создали.

## Проверка вашей новой системы Asterisk

На этом этапе нам не нужно слишком глубоко погружаться в систему, поскольку все последующие главы будут делать именно это.

Поэтому все, что нам нужно сделать сейчас - это проверить что мы можем войти в систему и конечные точки PJSIP, которые мы создали, находятся там.

```text
$ sudo asterisk -rvvvv
*CLI> pjsip show endpoints
```

Вы должны увидеть две конечные точки, которые мы создали, представленные ниже:

```text
Endpoint: <Endpoint/CID.....................................> <State.....> <Channels.>
   I/OAuth: <AuthId/UserName...........................................................>
       Aor: <Aor............................................> <MaxContact>
     Contact: <Aor/ContactUri..........................> <Hash....> <Status> <RTT(ms)..>
 Transport: <TransportId........> <Type> <cos> <tos> <BindAddress..................>
  Identify: <Identify/Endpoint.........................................................>
       Match: <criteria.........................>
   Channel: <ChannelId......................................> <State.....> <Time.....>
       Exten: <DialedExten...........> CLCID: <ConnectedLineCID.......>
==========================================================================================
 Endpoint: 0000f30A0A01 Not in use 0 of inf
     InAuth: 1/0000f30A0A01
  Transport: transport-udp udp 0 0 0.0.0.0:5060

 Endpoint: 0000f30B0B02 Unavailable 0 of inf
     InAuth: 2/0000f30B0B02
  Transport: transport-udp udp 0 0 0.0.0.0:5060
Objects found: 2
```

Если вы не видите две конечные точки в списке - значит имеется проблема с конфигурацией. Вам придется работать в обратном направлении, чтобы убедиться в отсутствии ошибок, которые мешают Asterisk подключаться к базе данных и создавать экземпляры этих двух конечных точек.

## Распространенные ошибки установки

Следующие условия \(в произвольном порядке\) вызывают большинство ошибок установки:

Синтаксическая ошибка

В некоторых случаях замены табуляции на пробел может быть достаточно, чтобы что-то сломать. UnixODBC, например, оказался чувствительным к отсутствию пробелов между определениями `key = value`. Лучший совет, который мы можем здесь дать - это использовать копирование/вставку, когда это возможно, в отличие от ручного ввода.

Проблемы с разрешениями

Они могут раздражать, но сообщения об ошибках, как правило, содержат необходимы подсказки. Файл _/var/log/messages_ часто является золотой жилой для полезных подсказок.

Недостающие шаги

Пропущенный шаг может не иметь заметных эффектов до тех пор, пока не пройдет много шагов. Дважды проверяйте все и проверяйте функциональность, прежде чем двигаться дальше.

Проблемы с учетными данными

Всегда проверяйте вручную, что созданные пользователи и пароли работают, прежде чем использовать их в файле конфигурации.

Невозможно и не нужно копаться в каждом предупреждении и сообщении об ошибке, которое вы можете увидеть, но если мы предусмотрели тестовый запуск и он не производит ничего, как мы указали, вы, вероятно, должны снова пройти этот шаг, пока не выясните, что происходит.

## Некоторые финальные заметки по конфигурации

После установки Asterisk создаст среду для себя на вашей Linux-машине. В следующих разделах приведены некоторые полезные сведения о том, как можно взаимодействовать с новой установкой Asterisk.

### Примеры файлов конфигурации для дальнейшего использования

Несмотря на то, что мы предупреждали вас не запускать команду `sudo make samples` во время установки \(потому что это заполнит ваш каталог _/etc/asterisk_ кучей вещей, которые вам не нужны\), файлы примеров тем не менее являются фантастической ссылкой. В директории с исходниками Астериска, вы найдете их в двух следующих каталогах:

```text
~/src/asterisk-16.<TAB>/configs/basic-pbx
~/src/asterisk-16.<TAB>/configs/samples
```

Файлы в этих папках стоит прочитать \(особенно для любого модуля, с которым вы работаете и хотите исследовать, как что-то сделать\).

Прочтите их когда у вас появится возможность.

{% hint style="danger" %}
**ПРЕДУПРЕЖДЕНИЕ**

Запуск `make samples` в системе, в которой уже есть файлы конфигурации, приведет к перезаписи существующих файлов.
{% endhint %}

### Командная оболочка Asterisk

Asterisk можно запустить как демон или как приложение. В общем, вы можете запустить его как приложение, когда создаете, тестируете и устраняете неполадки, и как демон при запуске его в продакшен.

Команда запуска Asterisk одинакова независимо от того, выполняется ли он как демон или как приложение:

```text
asterisk
```

Однако без каких-либо аргументов эта команда будет принимать определенные значения по умолчанию и запускать Asterisk в качестве фонового приложения. Другими словами, не стоит запускать команду `asterisk` самостоятельно, а лучше передавать ей некоторые параметры, чтобы лучше определить поведение, которое вы ожидаете. В следующем списке приведены некоторые примеры общего использования:

`-h`

Эта команда отображает список полезных возможных параметров для использования. Для получения полного списка всех параметров и их описаний выполните команду `man asterisk`.

`-c`

Эта опция запускает Asterisk как приложение \(на переднем плане\). Это означает, что Asterisk привязан к сеансу пользователя. Другими словами, если вы закроете сеанс пользователя, выйдя из системы или потеряв соединение, Asterisk умрет. Этот параметр обычно используется при создании, тестировании и отладке, но его не следует использовать в рабочей среде. Если вы запустили Asterisk таким образом, введите `core stop now` в командной строке CLI, чтобы остановить Asterisk и выйти.

`-v, -vv, -vvv, -vvvv`, etc.

Эта опция может использоваться с другими опциями \(например -`cvvv`\) для увеличения детализации вывода консоли. Она делает точно то же самое, что и команда CLI `core set verbose` _`n`_, где _`n`_-любое целое число между 0 и 5 \(любое целое число больше 5 будет работать, но не будет предоставлять больше подробностей\). Иногда полезно вообще не задавать уровень детализации. Например, если вы хотите видеть только ошибки запуска, уведомления и предупреждения, отключение детализации предотвратит отображение всех других сообщений запуска.

`-d, -dd, -ddd, -dddd`, etc.

Этот параметр можно использовать так же, как и `-v`, но вместо обычного вывода он будет указывать уровень вывода отладки \(что в первую очередь полезно для разработчиков, которые устраняют проблемы с кодом\). Также необходимо включить вывод отладочной информации в файл _logger.conf_ \(который мы рассмотрим более подробно в [Главе 21](glava-21.md)\).

`-r`

Эта команда необходима если вы хотите подключиться к CLI процесса Asterisk, работающего как демон. Вы, вероятно, будете использовать этот параметр больше чем любой другой для систем Asterisk, которые находятся в продакшене. Этот параметр будет работать только в том случае, если у вас уже запущен демонизированный экземпляр Asterisk. Чтобы выйти из интерфейса командной строки при использовании этого параметра, введите `exit`.

`-T`

Эта опция добавит метку времени к выводу CLI.

`-x`

Эта команда позволяет передать строку в Asterisk, которая будет выполнена так, как если бы она была введена в CLI. Например, чтобы получить быстрый список всех используемых каналов без необходимости запуска консоли Asterisk, просто введите `asterisk-rx "core show channels"` из оболочки, и получите результат в котором нуждаетесь.

`-g`

Этот параметр указывает Asterisk сделать дамп ядра, если оно аварийно завершает работу.

Мы рекомендуем вам попробовать несколько комбинаций этих команд, чтобы увидеть их в действии.

### safe\_asterisk

При установке Asterisk с помощью директивы `make config` создается скрипт _safe\_asterisk_, который запускается в процессе `init` Linux при каждой загрузке.

Скрипт _safe\_asterisk_ предоставляет следующие преимущества:

* Автоматическая перезагрузка Asterisk после аварии
* Можно настроить отправку электронной почты администратору в случае сбоя
* Определяет, где хранятся файлы сбоев \(по умолчанию_/tmp_\) 
* При сбое выполняет скрипт

Вам не нужно знать слишком много об этом скрипте, кроме понимания того, что он должен работать. В большинстве сред этот скрипт отлично работает в формате по умолчанию.

## Вывод

В этой главе мы представили кураторский пример того, как должна проходить установка Asterisk. Мы выбрали дистрибутив Linux и сервер MySQL для вас ради краткости, но отметили, что Asterisk на самом деле довольно гибок в таких вопросах. Теперь у нас есть прочный фундамент, на котором можно построить систему Asterisk. В следующих главах мы рассмотрим, как подключить устройства к нашей системе Asterisk для того, чтобы начать совершать вызовы внутри и работать над все более сложными концепциями в последующих главах \(таких как видеоконференции и WebRTC\).

[1](https://learning.oreilly.com/library/view/asterisk-the-definitive/9781492031598/ch03.html%22%20/l%20%22idm46178409180936-marker) Он был выпущен под лицензией Creative Commons, поэтому, если вы приобрели печатную копию \(и мы благодарим вас!\), вы также можете скачать программную копию для поиска и копирования/вставки.

[2](https://learning.oreilly.com/library/view/asterisk-the-definitive/9781492031598/ch03.html%22%20/l%20%22idm46178409178472-marker) Asterisk должен работать практически на любой платформе Linux, и если вы знакомы с основным процессом установки программного обеспечения на Linux, вы должны найти установку Asterisk довольно простой.

[3](https://learning.oreilly.com/library/view/asterisk-the-definitive/9781492031598/ch03.html%22%20/l%20%22idm46178409176504-marker) Под этим мы в основном подразумеваем, что вам удобно управлять системой исключительно из оболочки.

4 Elastix больше не является продуктом на основе Asterisk или open source.

[5](https://learning.oreilly.com/library/view/asterisk-the-definitive/9781492031598/ch03.html%22%20/l%20%22idm46178409144568-marker) После того, как вы прочтете нашу книгу, конечно же.

[6](https://learning.oreilly.com/library/view/asterisk-the-definitive/9781492031598/ch03.html%22%20/l%20%22idm46178409086616-marker) Новый сервис Lightsail от Amazon также обещает упростить создание размещенных Linux-машин.

[7](https://learning.oreilly.com/library/view/asterisk-the-definitive/9781492031598/ch03.html%22%20/l%20%22idm46178409043048-marker) Обратите внимание, что члены сообщества также будут создавать пакетные версии Asterisk. Например, репозиторий EPEL поддерживает версию, которая может быть установлена с помощью `dnf` \(`yum`\). На момент написания этой статьи официально поддерживается только версия tarball, и мы рекомендуем этот метод в настоящее время, в основном из-за множества различных модулей, которые поставляются с Asterisk, и полезности в возможности создавать то, что вам нужно из источника.

[8](https://learning.oreilly.com/library/view/asterisk-the-definitive/9781492031598/ch03.html%22%20/l%20%22idm46178409035896-marker) На экземпляре DigitalOcean вам нужно будет убедиться, что ваш SSH-ключ находится в файле _/home/astmin/.ssh/authorized\_keys_.
