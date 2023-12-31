# Хакатон 2023 - 💼 КОМАНДА "EXCELLENT'S"
<img src=PRESENTATIONS/logo.jpg width=150px height=150px>

## Авторы
САМОЙЛОВ Павел, КАДИРОВ Михаил, КЕЛАСКИНА Елизавета, ЛЕОНТЬЕВА Ольга, ЕПИШКОВ Владислав, ПЕРЕВЕРЗЕВ Петр.
## Задача
По поведению пользователей на площадке **rabota.ru** предсказать, на какие вакансии пользователь в дальнейшем откликнется или позвонит.
В рамках задачи вам будет необходимо разработать модель, которая будет опираться на взаимодействия пользователей (соискателей) с вакансией. На выходе модель должна отдавать список из n рекомендованных вакансий, отсортированных по релевантности.

## Метрика
Результаты модели будут оцениваться по метрике **precision@5**. При демонстрации своего решения по завершению хакатона, вам необходимо будет предоставить код для обучения и инференса модели, презентацию по проделанной работе и файл с предсказаниями для пользователей из списка test_private_users_mfti.parquet в формате файла test_private_sample_submission_mfti.parquet

## Рекомендации по решению
- В таких задачах, чаще всего, baseline моделью считается рекомендация самых популярных вакансий для всех пользователей. Попробуйте написать полный pipeline с использованием этой простой модели и отталкиваться от него в процессе ваших исследований
- Обратите внимания на то, что если пользователь каким-либо образом взаимодействовал с вакансией в обучающих данных, то для этого пользователя данной вакансии не будет в списке меток теста. Мы не хотим рекомендовать пользователю то, что он уже видел и с чем взаимодействовал.
- Постарайтесь не использовать файл test_public_mfti.parquet для постоянной валидации своих моделей, это может привести к переобучению. Вместо этого лучше подготовить свой валидационный датасет.

## Структура репозитория
📁 **OUR_ANSWER** содержится финальный код, с помощью которого мы получаем итоговый ответ и собственно файл, в котором содержится предсказание для пользователей по задаче  
> 📑 *implict_v_final.ipynb* – код в результате которого мы получаем предсказание для задачи, в нем удалены все лишние блоки которые касаются экспериментов, но включены некоторые текстовые блоки поясняющие наш выбор подхода или параметра  
> 📑 *our_test_private_sample_submission_mfti.parquet* – файл с ответом на задание по требуемой форме  

📁 **DATA** данные для которые использовались для обучения и их описание (пока разрешение на их загрузку от индустриального партнера не получено)  
> 📑 *train_mfti.parquet* – сырые данные, которые можно использовать для обучения модели  
- event_date – дата взаимодейтсвия
-	event_timestamp – timestamp взаимодействия в секундах 
-	vacancy_id_ - id вакансии, с которой было взаимодействие
-	cookie_id – id пользователя по его браузеру/ip/устройству
-	user_id – id пользователя на сайте rabota.ru (есть только для зарегистрированных пользователей)
-	event_type – тип взаимодействия

1) show_vacancy - просмотр вакансии
2) preview_click_vacancy - клик по карточке вакансии
3) click_response - отклик со страницы вакансии - target
4) preview_click_response - отклик с карточки вакансии - target
5) click_favorite - добавление вакансии в избранное со страницы вакансии
6) preview_click_favorite - добавление вакансии в избраное с карточки вакансии
7) click_contacts - клик на контакты со страницы вакансии - target
8) preview_click_contacts - клик на контакты из карточки вакансии- target
9) click_phone - клик на номер телефона, указанный в вакансии- target
10) preview_click_phone - клик на номер телефона из карточки вакансии- target

> 📑 *test_public_mfti.parquet* – часть теста, с открытым таргетом, для проверки работоспособности решений.
-	cookie_id - id пользователя по его браузеру/ip/устройству
-	vacancy_id_ - список вакансий, на которые пользователь откликнулся или позвонил в течение месяца после окончания данных train
> 📑 *test_private_users_mfti.parquet* – часть теста, с закрытым таргетом для итоговой проверки решений
-	cookie_id - id пользователя по его браузеру/ip/устройству
> 📑 *test_private_sample_submission_mfti.parquet* – файл с примером предсказаний, который требуется получить по итогу хакатона 
-	cookie_id - id пользователя по его браузеру/ip/устройству
-	predictions – список из 5 id вакансий, которые модель предсказала как наиболее релеватные для данного пользователя

‼️ С учетом того, что нами было подписано соглашение с индустриальным партнером хакатона, данные на которых проходило обучение моделей в открытом доступе выложены не будут.‼️  

📁 **DATA ANALYSIS** содержит различные аналитические исследования датасета, ожидается что по завершении хакатона, эти материалы будут реализованы в отдельный отчет по исследованию. 
> 📑 *first_look.ipynb* – как и отражено в названии самый поверхностный просмотр данных, все 4 предоставленных файла, по сути head() и info(), но если действительно очень простой и полезный способ первого знакомства с данными  
> 📑 *one_user.ipynb* – прослеживаем активность одного пользователя  
> 📑 *some_graph1.ipynb* – аналитические графики по данным  
> 📑 *some_graph2.ipynb* – аналитические графики по данным  
> 📑 *analitics.ipynb* – аналитический обзор ориентированный на время взаимодействий  
> 📑 *vacancy_metrics.parquet* – (9 вариантов ТОП) – содержат оценки и рейтинги вакансий по 15 различным вариантам определения топ, очень любопытные файлы как результат исследования  
> 📑 *vavancy_rangs.xlsx* – (9 вариантов ТОП) – содержат оценки и рейтинги вакансий по 15 различным вариантам определения топ, очень любопытные файлы как результат исследования  
> 📑 *Некоторые аналитические выкладки.docx* - содержит некоторые интересные аналитические выкладки и наблюдения

📁 **DATA PREPROCESSING** содержит различные варианты предварительной обработки данных, скрипты присутствуют не все многие разлетелись по алгоритмам, поэтому основной интерес в данной директории представляет собой папка - TRAIN AND TEST SETS – в которой содержится разработанный командой способ разделения данных на тренировочную и тестовую выборки.  
> 📁 **TRAIN AND TEST SETS** – в которой содержится разработанный командой способ разделения данных на тренировочную и тестовую выборки.  
>> 📑 *Test_train_set.ipynb* – код с подробным описанием логики и последовательности разбиения  
>> 📑 *df_test_list_top.csv* – тестовый набор данных, включающий 1790 строк по уникальным пользователям, для каждого из которых подобраны по 5 вакансий с которыми у пользователя было положительное взаимодействие  

> 📑 *Precision_5.ipynb* – функция для расчёта метрики precision@5  
> 📑 *sunrise_date.ipynb* – различные варианты представления данных, отражают первые попытки и эксперименты  
> 📑 *Unseenvacancies.ipynb* – функции, которые позволяют выделять вакансии, с которыми не было взаимодействий (работают очень долго в текущей разработке не актуальны, но как исторический этап)  

❗ В ноутбуках с моделями есть и другие способы выделения из данных дополнительных признаков, больше всего признаков выделяется в архаичном алгоритме, который находится в файле *square_vampir.ipynb* (📁 **REJECTED ALGORITHMS**). В дальнейшем мы эту папку почистим, но история исследования должна сохраняться  

📁 **IN DEVELOPMENT** на данный момент содержит алгоритмы, которые находятся в стадии активной разработки и наиболее перспективны. Код построен таким образом, что все участники команды могут работать над подбором гиперпараметров и способов предварительной кодировки типов взаимодействия и их силы, как показали исследования от этого довольно сильно варьируется результат. В настоящий момент в папке 4 алгоритма. Два для стадии основного прогона – выдает персональные рекомендации, и два для второй стадии – фильтрации по популярности.  
Два для стадии основного прогона – выдает персональные рекомендации:
> 📁 *1_choise_implict* – работает на библиотеке implicit (готовый pipeline на стадии подгонки параметров)  
>> 📑 *implict_build_v3.ipynb* – базовая версия для подгонки параметров  
>> 📑 *implict_build_v4.ipynb* – расширенная версия, в которой реализовано соединение cookie_id и user_id а так же алгоритм учитывающий уникальные комбинации сразу 3х столбцов cookie, vacancy и event_type. Однако эти идеи не принесли улучшения метрики, а совсем даже наоборот. Возможно тут мы что то не доработали, поэтому оставили этот код в папке для разработки  
>> 📑 *implict_on_our_traim_test.ipynb* – это код в котором базовый алгоритм тестировался на нашем варианте разбиения train и test. На них мы получали очень хорошие метрики.  

> 📁 *1_choise_lightFM* – работает на библиотеке LightFM (готовый pipeline на стадии подгонки параметров)  
>> 📑 *lightFM_v1.ipynb* – базовая версия для подгонки параметров  
>> 📑 *rabota_split_features.ipynb* - разделение на train_test, выделение признаков для item (vacancy)

И два для второй стадии – фильтрации по популярности. То есть мы просим основной алгоритм отдать не 5, а 10 вакансий, а потом выбираем 5 по ранжиру ТОП или наиболее похожих, например, по времени просмотра:  

> 📁 *2_filter_TOP* – работает на baseline алгоритме наиболее популярных вакансий (слабо реализован)  
>> 📑 *implict_build_ant_top_filter.ipynb* – алгоритм в котором после первого извлечения рекомендаций 10 или 15 штук, мы ранжируем их сообразно ТОП полученному на baseline. Алгоритм показывает метрику хуже базовой версии, поэтому пока что оставили в разработке  

> 📁 *2_filter_VVJ* – работает на алгоритме векторного сдвига (слабо реализован)  
>> 📑 *Наброски кода и картинки не заслуживают внимания*  
>> 📑 *Замысел метода векторного сдвига.docx* – предлагаем ознакомиться с кратким изложением замысла фильтра, к сожалению, реализовать мы его не успели

📁 **PRESENTATIONS** в ней содержатся презентации, которые мы сдавали по ходу «Хакатона» и визитки участников команды.  
> 📑 *OUR_TEAM.pptx* – визитки участников команды и их контакты  
> 📑 *Презентация № 1 Excellent's.pptx* – презентация по первому check point  
> 📑 *Презентация № 2 Excellent's.pptx* – презентация по второму check point  
> 📑 *Презентация № 3 Excellent's.pptx* – итоговая краткая презентация по проекту  

📁 **READY-MADE ALGORITHMS** ПОКА ЧТО содержится всего 2 полностью готовых алгоритма.  
> 📑 *Baseline_(Работа_ru).ipynb* – алгоритм который вычисляет 9 различных вариантов подсчета ТОП вакансий и ранжирует все вакансии по критериям, после чего вычисляет метрику precision@5 выдавая каждому пользователю по топ-5 вакансий из тех что он еще не смотрел (по каждому из 9 вариантов ранжирования)  
> 📑 *Baseline (Работа_ru)_new_version.ipynb* – усовершенствованный алгоритм, вычисления гораздо быстрее и уже 15 вариантов топов  
> 📑 *Random_prediction.ipynb* – алгоритм предсказания основанный на чистой случайности. Реализован, просто ради прикола. Возможно эта случайность как-то ему помогает, но его метрика хоть и не велика, но отнюдь не 0, как у некоторых «алгоритмов»  
> 📑 *implicit_v_final.ipynb* – алгоритм по которому был рассчитан ответ на задачу, + в нем есть 1 дополнительная рекомендация по использованию (ранжирование определенных вакансий)  

❗ У нас есть еще очень много готовых алгоритмов, по тем или иным причинам мы отправили их либо в папку утиль ( 📁 **REJECTED ALGORITHMS**), либо они, будучи в принципе готовыми находятся в папке в разработке (📁 **IN DEVELOPMENT**)  

📁 **REJECTED ALGORITHMS** мы поместили алгоритмы, от реализации которых мы полностью отказались, но просто удалить их мы не решились. Во-первых, это история нашего обучения, до хакатона никто из нас не занимался рекомендательными системами, поэтому ошибки это и не ошибки совсем, а уроки. Во-вторых, в них есть полезные скрипты, которые мы помним и перетаскиваем в актуальные алгоритмы. В-третьих, в науке отрицательный результат — это тоже результат, мы поняли почему не сработали эти алгоритмы и вовремя переключились на поиск других.  
> 📑 *square_vampir.ipynb* – первый алгоритм, его можно назвать базой для оценки и выделения признаков, его идея была в том, что бы создать уникальный вектор пользователя и уникальный вектор вакансии (по среднему значению различных параметров) и искать похожие вакансии для пользователя по косинусному расстоянию, минимальной разности, евклидовому расстоянию. Алгоритм не дошел, до масштабных тестов, потому что был реализован очень тяжело в вычислительном плане, но ряд идей, появившихся в ходе его разработки, продолжают развивается в алгоритме 2 фильтрации основанной на векторном сдвиге  

Серия алгоритмов SVD(), реализованы на библиотеке surprise, первые надежды и большое разочарование – максимальная метрика 0,007. Возможно алгоритм и предскажет оценку которую поставит пользователь вакансии, но для работы на исторических данных он не годится, потому что не факт, что эту вакансию ему покажут на сайте. И в целом система surprise рассчитана на работу с явными взаимодействиями (а у нас очень много зашли на сайт так посмотреть). Тем не менее именно на базе SVD команде полностью удалось реализовать пайплайн обучения и проверки модели.  
> 📑 *SVD_all.ipynb*  
> 📑 *SVD_base.ipynb*  
> 📑 *SVD_low.ipynb*   

> 📑 *sunrise_v0.1(KNN).ipynb* – алгоритм, который очень неплохо предсказывал оценки, но по тем же причинами не был реализован до конца, он заслуживает упоминания потому, что будучи сложным вычислительно (боже мы его запускали без GPU!!!) он проваливался с ошибкой, но мы не остановились и реализовали под него систему до-обучения на базе добавления batch.
