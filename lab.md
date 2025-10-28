# Руководство по установке и настройке Anaconda под Windows

## 1. Скачивание и установка Anaconda

### Шаги по скачиванию
1. Перейдите на сайт Anaconda: [https://www.anaconda.com/download/success](https://www.anaconda.com/download/success).
2. Нажмите "Download" под разделом Distribution Installers для Windows-инсталлера.

### Шаги по установке
1. Перейдите в папку Загрузки/Downloads и дважды кликните на инсталлер (например, `Anaconda3-2025.06-0-Windows-x86_64.exe`).
2. Нажмите "Next", затем "I Agree" для принятия условий после ознакомления с ними.
3. Выберите опцию:
   - **Just Me (рекомендуется):** Для текущего пользователя.
   - **All Users:** Для всех (потребует прав администратора для установки).
4. Нажмите "Next".
5. Выберите папку установки и нажмите "Next". Рекомендуется использовать путь без пробелов и специальных символов (например русских букв).
6. Внимание к настройкам:
   - Добавить Anaconda3 в PATH: Не рекомендуется (может вызвать конфликты с другими инструментами, которые либо уже используют установленный в ситсеме Python, либо могут понадобиться в будущем).
7. Нажмите "Install". Процесс может занять несколько минут.
8. Нажмите "Next" дважды, затем "Finish".

### Проверка установки
1. В меню Пуск найдите "Anaconda Prompt" и запустите команду `conda --version`.
2. Или запустите Anaconda Navigator для проверки GUI.

## 2. Настройка Bioconda

[Bioconda](https://bioconda.github.io/) - это канал-репозиторий для `conda`, позволяющий устанавливать биоинформатические пакеты и инструменты (не в полной мере актуально для Windows). Настройка выполняется один раз.

1. Откройте Anaconda Prompt.
2. Добавьте каналы:
   ```
   conda config --add channels defaults
   conda config --add channels bioconda
   conda config --add channels conda-forge
   ```
3. Установите приоритет каналов (опционально):
   ```
   conda config --set channel_priority strict
   ```
   (Или `flexible`, если необходимо).

Это позволит устанавливать пакеты из Bioconda с помощью `conda install <package>` без явного указания каналов через `conda install -c bioconda <package>`.

## 3. Создание окружения с JupyterLab и nb_conda_kernels

nb_conda_kernels позволяет автоматически обнаруживать ядра (kernels) для JupyterLab из других окружений conda (используются окружения с пакетом `ipykernel`). ПРоверка выполняется один раз при старте JupyterLab.

1. Создайте новое окружение (например, `lab`):
   ```
   conda create -n lab
   ```
2. Активируйте его:
   ```
   conda activate lab
   ```
3. Установите JupyterLab и nb_conda_kernels:
   ```
   conda install -c conda-forge jupyterlab nb_conda_kernels
   ```
4. Установите `ipykernel` при необходимости использования окружения в качестве ядра (см. ниже).
5. Запустите JupyterLab:
   ```
   jupyter lab
   ```

## 4. Настройка окружений python38 и python314

Эти окружения будут иметь разные версии Python.

1. Создайте окружение python38 с Python 3.8:
   ```
   conda create -n python38 python=3.8
   ```
2. Активируйте и установите ipykernel для обнаружения в Jupyter:
   ```
   conda activate python38
   conda install ipykernel
   ```
3. Создайте окружение python314 с Python 3.14 (предполагая, что версия доступна в 2025 году):
   ```
   conda create -n python314 python=3.14
   ```
4. Активируйте и установите ipykernel:
   ```
   conda activate python314
   conda install --freeze-installed ipykernel
   ```

Обратите внимание на параметр `--freeze-installed`. Он позволяет задать дополнительное ограничение при установке пакетов.

Окружения с пакетом `ipykernel` будут автоматичеки обнаруживаться в JupyterLab (если с ним установлен пакет `nb_conda_kernels`)

## 5. Настройка окружения biopython

Это окружение будет содержать пакеты `biopython`, `pyvcf` и `pysam`.

1. Создайте окружение (можете указать конкретную версию для пакета, например `python=3.10`):
   ```
   conda create -n biopython python=3.10
   ```
2. Активируйте:
   ```
   conda activate biopython
   ```
3. Установите пакеты:
   ```
   conda install biopython
   conda install pyvcf pysam
   ```
   либо
   ```
   conda install -c conda-forge biopython
   conda install -c bioconda pyvcf pysam
   ```
   (Или все разом: `conda install -c conda-forge -c bioconda biopython pyvcf pysam`).
4. Установите ipykernel для Jupyter:
   ```
   conda install ipykernel
   ```
Теперь пакеты готовы к использованию, и ядро доступно в JupyterLab.

5. Альтернативно можно установитьв сё окружение целиком с помощью команды:
    ```
    conda create -n biopython -c conda-forge -c bioconda biopython pyvcf pysam ipykernel
    ```

Стоит помнить, что чем больше вы запросите пакетов в одной команде на установку, тем дольше Anaconda будет разрешать их зависимости, подбирая наиболее оптимальный вариант сочетания версий пакетов.

### Дополнительные советы
- Избегайте установку дополнительных пакетов в основное окружение (`base`) - это может затруднить последующие обновления с помощью `conda update ...`.
- Всегда активируйте окружение перед установкой пакетов с помощью команды `conda activate <env_name>`.
- Деактивация окружения осуществляется через команду `conda deactivate`.
- Список окружений можно вывести с помощью команды `conda env list`.
- Если возникли проблемы с каналами, обновите Anaconda: `conda update conda`.
