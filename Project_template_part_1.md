Это шаблон для решения **первой части** проектной работы. Структура этого файла повторяет структуру заданий. Заполняйте его по мере работы над решением.

# Задание 1. Анализ и планирование

Чтобы составить документ с описанием текущей архитектуры приложения, можно часть информации взять из описания компании условия задания. Это нормально.

### 1. Описание функциональности монолитного приложения

**Управление отоплением:**

- Пользователи могут удалённо включать и выключать систему отопления в своих домах.
- Устанавливать целевую температуру.
- Система поддерживает управление через REST API.

**Мониторинг температуры:**

- Пользователи могут запрашивать текущую температуру в доме.
- Система получает данные с датчиков температуры и обновляет значения в базе данных.

### 2. Анализ архитектуры монолитного приложения

- Язык программирования: Java (Spring Boot)
- База данных: PostgreSQL
- Взаимодействие: Синхронное, REST API
- Развёртывание: Kubernetes, Helm, Terraform
- Структура: Вся бизнес-логика сосредоточена в одном монолите, API управляется контроллером HeatingSystemController.


### 3. Определение доменов и границы контекстов

- Домашний отопительный домен (Heating Domain):
     - Функции управления отоплением, включая включение/выключение и установку целевой температуры.
- Домен температурного мониторинга (Temperature Monitoring Domain):
     - Сбор данных с датчиков температуры, обновление и хранение информации в базе данных.
- Домен управления устройствами (Device Management Domain):
     - Подключение и настройка дополнительных устройств.
- Домен самообслуживания (Self-Service Domain):
     - Обеспечение возможности пользователям самостоятельно подключать и настраивать устройства, не прибегая к централизованным изменениям.
- Домен управления пользователями (User Management Domain):
     - Аутентификация, авторизация и управление профилями пользователей.

### **4. Проблемы монолитного решения**

- Масштабируемость:
     - Монолит сложно масштабировать, так как нагрузку приходится распределять на все функции сразу, даже если требуется масштабировать только один из аспектов (например, сбор данных с датчиков).
- Высокая связанность компонентов:
     - Любое изменение в одной части приложения может повлиять на остальные, что повышает риск ошибок и усложняет тестирование.
- Сложности развёртывания:
     - Обновление или модификация одной функциональности требует остановки всего приложения, что негативно сказывается на доступности сервиса.
- Отсутствие поддержки самообслуживания:
     - Пользователи не могут самостоятельно подключать или настраивать устройства, что снижает гибкость системы и увеличивает нагрузку на техническую поддержку.


### 5. Визуализация контекста системы — диаграмма С4

[Ссылка на диаграму](//www.plantuml.com/plantuml/png/NP31IWCn48RlUOgXzxw01wcWK0-5OjUJU0XrM1UsMSX6GKIeBU9XFGfU15zXKOj5kzjN-EUDJ5AfQ2vX_l_xCpFfbKvRTp79bHwxmb87BsZn9G_uyFShQfbX4UeEwPAEIxPRy36OSeybFYbIAwona6cKvpALAcP6RCh2AcLvxBH9S_RHPxHeGXyiPKOonogMRsW9xsXzMSVie315dhbpkbRapqcVjSjCwj-qbzcOJpTgmZbFsDchVtgObWRQwDJRnlse-noGF_gDKf7jTdT_M-vGFqc6T7GmIcXlUAYsHg0sIta6JwYnzaktSYjLcBv6WqOgkLCzDcV-Pxy1)


# Задание 2. Проектирование микросервисной архитектуры

В этом задании вам нужно предоставить только диаграммы в модели C4. Мы не просим вас отдельно описывать получившиеся микросервисы и то, как вы определили взаимодействия между компонентами To-Be системы. Если вы правильно подготовите диаграммы C4, они и так это покажут.

**Диаграмма контейнеров (Containers)**

[Диаграма контейнеров](//www.plantuml.com/plantuml/png/TLJTQXD15BwVfpZaIYzAxxsGAe8LBQHsVO0n7KCntU3kH2aY9AtQYnHx8oWe5D_0HfV6ncvVuSmR-StPtMvcC86GdVFzd3dppMOZjpgpe_s1qZqxpAZ3Prpomftm1Nvd_DCTSk7N7T8vFSjDTaFyx45tp7E-967zvMikt5ZAHsxifgBPCMa-p9JAJC_gj3ymrBdNVKGFjJLlz85GiAwN0Os_UsyObcuYnZEUkuctVQi8doNgfj5sB-sZMVPwhsV4jQx5rCGCpBwnsK50lgd6T3zomFI5UXrp7nXVfIsTwZxSKXi8AxIHlWj7_SfNxhsStGIdDtP7V2cdY4epxgws-hdEmUuESzlFpCxsayhulk2lHcO4yB5-yL9BNPvyeoe2Mbkxsyv9TyWdo5TeFEM2kFnlY41kRMqdoVfcadHlgrOEiwg1Dph3VizvuUE2y2DYEYN-tRYx3w4E6_QYBy2l9T6JWBpYCYdf9lecRPZyu9BFeJf4xuByWl1q0Na1FtoncgzoR7o2MM97uXccMhXFxhZnM-UPcyP5KA4fqMHHbTnflULsv6_0Vm4PL--M20joawMMVvIIA-T5xNi1iG-0dXgadDjSjT3oYKJbDkq822vn24NNTCEak_Xt-0S0)

**Диаграмма компонентов (Components)**

[Heating service](//www.plantuml.com/plantuml/png/PL6zIWD14ExtAORqmTv28jT4H11JYiMGhE78tMxOMmKHmH-mKSWRQ65Xcmi4pn_FLvXz8sVsBf2GPc6-ds--ONk5H-SlyanDSJZ1iOPENwDFpHW6sbscGzs1QmLmkdzy08artjaiq-uCi026X4bidgCPGP-V5TLYGWyyUWr7Q3Yy8IFI0g8uqLDRfDwwgqZBAF0XdayGaYpLnaSY0isiRfIIHjtk7gpsxmAzK1bkQKubrU6UlcWEzC7Z7uDrkAEQ5gfLYtEZYDZVM2ds1NtJadwfeYMmZQsSLeTP0z6FsZ1AadIMX7SMjiwTPZLLx7xUYWcpzIRhfqdCQtYWaajKSaDu57bCuFEUEAEYJzNJPiJVzWy0)

[Telemetry service](//www.plantuml.com/plantuml/png/LP2nIWD154Nx-Of7-lo0XGIq4HinTcBnM9zXoMOcp8n24C44hBGH13k_8Hiief3iBzpv8sTcqNNB--wvUvOPES_MtyugDUTYoXEXmOLKCXDl5pGMUrSMCg1xHNGv5ksCfMFsJ2Tw8iuRUqNi_aA2SdjcJI7EjNMNKdykS-FA22zobs8wd_BDbEceAaNxNEH3czM3KhyA6WuFkm_I0U43TNZ46ZNsOOLlh0cVCUwm2y_XINLiMYPT6hr7ei4sBCDB3oTi2Azuwy_-_qnMHb6JfPii34liKSVz2bjygP7ew_YWFm00)

[User service](//www.plantuml.com/plantuml/png/TP1DIWD148NtVOeY-rp0XKHSkH34dvMuQCP2XupTeQSZY0ZgMl06Rhn09p0O4frSuSsDh2v6690ity_xkkX97Ms8yxlAp6mnjJTCeukQ0vrnk2yB7j6Z8ReyE3cYG-zYy5N5uOfiJM9fCe-tx69Ps5XwHyVM2QDlIDJht5JiASzyNKOV7d95PSxE_PJTtvkQgDCRvicOVC5ul3-4qXxX0ulk6GaBjDqhraY4Bv5RCTlk1IsMPhWGWhwPfv_QIbXYWmO_MA4XR6MowTu48bCaOJQYt_6DrJ_GyCHTG06qMAjC2fcmkvRV_WK0)

[Device management service](//www.plantuml.com/plantuml/png/XL2zIWD14ExtAORqmTv28h4H6x4LYoMEuSZTRTXT1H71ADWeMDg9dUqPERW8cbVuzesSESvmLdRp_Iyx2j7wU5xaPcudCpjb6kpnHJPXAcmfiE0oKc2lrE3A41tl7kxJs9NHkpndVq8sa52gT47Fqon4VzbAHntueyzToABxTGXKzS2UpUmupsImvNlON1kZiyFXpgQGjWclgF619YtIRRf1bUxbVg9qrn7VMEc5fTc4j447DVsC2kis_RY5D_7L-4A3-j-2tikqvqKSQzt74f-eA2qa-9uUX6wmCYCkJ-L_Vm00)

**Диаграмма кода (Code)**

[Heating service пример](//www.plantuml.com/plantuml/png/rPBH2e8m543VznLxLCeVy26aUYW8Wkm7bhkkmTpaxYf8zE-PWMRC3-Xb3sTdtCEsp9D0QYh32YEyvmSGfAtAEqzGxPmbTCO0yWVZVAM0PlkHbYa_EQlYT7vmJ-xjKuFsR2ThP6KvXnxeAb3rArGreEWb68qrfuccNBhcPY1cJnPApW5RNS3Vz5wWTbwJm-wJN6ehjzrHBgOEuqKBqVsR_aZ3jAMdJ99DyCvO28vse9XbO6N_TLu0)

# Задание 3. Разработка ER-диаграммы

[ER-диаграмма](//www.plantuml.com/plantuml/png/ZPB1IWCn48Rl-nIXHs7x03trK4JFjkSbDeC6ajqbcLHAjr3rXKIj54hr5R9lv2GkRGgkU2cPFt--VsQwOulSeDCgWn8bBjbWPi4C6BSi7DWgO-o2IS56R3Qxdb2Lv_HJSWqatQ2HvHtLeBbKgL1pQnyg8qxQEZj6N5NEGio8fxuGBeG1QrCikKrnCYRC2Ipa_0SQZxrK8aYkYjBfMp0fso97TvPs7xehKW8kQ_WhVqhtIG-X_EyA1TYvaJNAglnrQLDGe07DSiAHnuoZqXSXOXFd0qWDDAYkII8GJAaEJc8MORYqemLNBWYNgk8OMebVHDFaURNg_haHywz-xr_ykz_wRVkKHRdpT4iWK_leFuEbtVWFloDRTxDkXiEcIPT5m2zoxVmXGlU4JLzqv6HqUl16faE0at7JFiS-nb0AfzKt)
