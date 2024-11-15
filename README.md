# Prometheus and Blackbox Exporter to monitor resource availability

![Targets](https://github.com/G1fi/Prometheus-and-Blackbox-Exporter-to-monitor-resource-availability/blob/main/targets.png?raw=true "Targets")

## Для чего этот репозиторий?

Этот репозиторий содержит конфигурационные файлы для мониторинга доступности ресурса (в данном случае https://ptsecurity.com) с использованием Prometheus и Blackbox Exporter. Каждые 15 секунд Prometheus будет проверять состояние как самого Prometheus-сервера, так и целевого ресурса, отправляя HTTP-запросы через Blackbox Exporter.

Ресурс будет считаться доступным только в случае получения ответа с кодом 200 и наличия рабочего SSL-сертификата. Для этого в конфигурации Blackbox Exporter настроены следующие параметры:
- `fail_if_not_ssl`: если включено, запросы к ресурсам без SSL-сертификата будут считаться ошибочными.
- `insecure_skip_verify`: этот параметр управляет проверкой SSL-сертификата. Если `false`, то сертификат будет проверяться, и подключение будет отклонено в случае ошибок валидации сертификата.

## Подготовка окружения

Для работы с этим репозиторием необходимо наличие следующих компонентов:
- **Prometheus** — для мониторинга и сбора метрик.
- **Blackbox Exporter** — для проверки доступности ресурса с помощью HTTP-запросов.

Все компоненты должны быть доступны на локальном хосте. Если это не так, нужно изменить соответствующие адреса в конфиге `prometheus.yml` с `localhost` на нужный адрес.

Скачайте необходимые бинарники Prometheus и Blackbox Exporter, либо настройте их через Docker.

## Запуск с конфигурациями

Команды для запуска, на примере бинарных версий Prometheus и Blackbox Exporter:

```bash
./blackbox_exporter --config.file=blackbox.yml
./prometheus --config.file=prometheus.yml
```

## Дополнительно: Настройка Grafana

Для красивой визуализации метрик можно использовать готовый дашборд с ID 7587  в Grafana. Чтобы настроить его:
1. Установите Grafana и подключите Prometheus как источник данных.
2. В разделе "Dashboards" в Grafana выберите опцию импорта и введите ID дашборда 7587.

Вот как может выглядеть результат:

![Grafana](https://github.com/G1fi/Prometheus-and-Blackbox-Exporter-to-monitor-resource-availability/blob/main/grafana.png?raw=true "Grafana")

Так же в графане можно настроить алерты для уведомлений о недоступности ресурса n-ое количество времени. С помощью запроса:

```
probe_success{instance="https://ptsecurity.com", job="ptsecurity"} == 0
```
