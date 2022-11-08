# Распознавание автомобильных номеров
Задачей данного учебного проекта является создание и обучение модели для распознавания номера и типы машины, для проезда под шлагбаумом на террито рию предприятия.

Обучение, расчеты и измерения метрик производилось в Google Colab на GPU Tesla T4 16Gb.

# Реализация
1. [Датасет](https://www.kaggle.com/datasets/kirillpribludenko/number-plates-50-russain-50-others) взят с kaggle и содержит три класса: легковая машина, номер, грузовая машина.
2.  Были выбраны 2 модели для обучения: [Yolov5](https://github.com/jvSett/Car_Plates/blob/main/YoloV5_V_1.ipynb) и [Yolov7](https://github.com/jvSett/Car_Plates/blob/main/YoloV7_V_1.ipynb).

При обучении Yolov5 сначала было принято решение обучать на 50 эпохах, но результат, мягко сказать, получился не очень:
![](https://github.com/jvSett/Car_Plates/blob/main/images/50_epoch.jpg)


Результат после обучения Yolov5 на 200 эпохах:
![](https://github.com/jvSett/Car_Plates/blob/main/images/200_epoch.jpg)

Результат после обучения Yolov7 на 200 эпохах:
![](https://github.com/jvSett/Car_Plates/blob/main/images/200_yolov7.jpg)

Предоставляем графики обучения Yolov5 с помощью борда [wandb](wandb.ai):

![](https://github.com/jvSett/Car_Plates/blob/main/images/model_graphs.jpg)

Модели были взять с официальных репозиториев [Yolov5](https://github.com/ultralytics/yolov5) и [Yolov7](https://github.com/WongKinYiu/yolov7)

3. Автоматическое распознавание текста реализовано с помощью библиотеки easyocr 

Результат распознования автомобиля и детекции номера:
![](https://github.com/jvSett/Car_Plates/blob/main/images/check-result1.jpg)

4. Сравнение моделей

В качестве метрики была выбрана популяная mAP для детекции, тем более, что ее расчет вшит в модели Yolo. Произвели сравнение по данной метрике, чтобы понять какая модель лучше подходит под нашу задачу.

**Метрики качества валидационного набора для Yolov5:**
![](https://github.com/jvSett/Car_Plates/blob/main/images/%D0%9C%D0%B5%D1%82%D1%80%D0%B8%D0%BA%D0%B8_Yolov5.jpg)

**Метрики качества валидационного набора для Yolov7:**
![](https://github.com/jvSett/Car_Plates/blob/main/images/%D0%9C%D0%B5%D1%82%D1%80%D0%B8%D0%BA%D0%B8_Yolov7.jpg)

Как видно из табличных данных по столбцу **mAP@.5:.95** Yolo7 лучше по всем параметрам.

Проведем дополнительные замеры по скорости скорости общего решения.

**Метрики общей скорости для Yolov5:**
![](https://github.com/jvSett/Car_Plates/blob/main/images/YoloV5_total.jpg)

**Метрики общей скорости для Yolov7:**
![](https://github.com/jvSett/Car_Plates/blob/main/images/Yolov7_total_S.jpg)

Как видно, распознавание номера сильно тормозит общее решение. Есть смысл задуматься об оптимизации данного момента.
На основании полученных результатов была выбрана модель **Yolov7**, так как ее скорость детекции немного быстрее, а качество обнаружения этой модели намного превосходит Yolov5m.

# Оборудование
Запускали эксперименты на **Intel Core i7-10700k CPU**,но для более быстрого качества обнаружения и детекции желатьно использовать GPU  
