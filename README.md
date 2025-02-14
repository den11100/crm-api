## Общее
* https://github.com/den11100/crm-api#readme
* Во всех запросах В header нужно передавать параметр `X-Api-Key` иначе будет ошибка `bad authorization` → 401

* msg - сообщение об ошибке
* data - данные (может быть пустой массив)


### Получить список офисов
`GET /api/v1/full-schedule/offices` → 200, 400

#### Ответы

200
```json
{
    "msg": "",
    "data": [
        {
            "id": 1,
            "name": "МО Москва",
            "city": "Москва"
        },
        {
            "id": 2,
            "name": "МО Воронеж",
            "city": "Воронеж"
        }
    ]
}
```



### Получить список активных врачей

* `officeId` необязательный параметр

* `GET /api/v1/full-schedule/doctors` → 200, 400 вернёт всех активных врачей из всех офисов
* `GET /api/v1/full-schedule/doctors?officeId=1` → 200, 400 вернёт всех активных врачей в офисе Москва officeId = 1

#### Ответы
200
```json
{
    "msg": "",
    "data": [
        {
            "id": 26,
            "fio": "Фамилия Имя Отчество",
            "office_id": 17,
            "city": "Челябинск",
            "specialty_name": "Генетик",
            "grid_pitch": 60,
            "time_zone": "+05:00",
            "have_free_slots": 1
        },
        {
            "id": 91,
            "fio": "Фамилия Имя Отчество",
            "office_id": 1,
            "city": "Москва",          
            "specialty_name": "Генетик",
            "grid_pitch": 60,
            "time_zone": "+03:00",
            "have_free_slots": 0
        },
        {
            "id": 92,
            "fio": "Фамилия Имя Отчество",
            "office_id": 1,
            "city": "Москва",            
            "specialty_name": "Невролог",
            "grid_pitch": 60,
            "time_zone": "+03:00",
            "have_free_slots": 0
        }
    ]
}
```
* grid_pitch - это время приёма в минутах

400
```json
{
    "msg": "Текст ошибки",
    "data": []
}
```


### Получить информацию об одном враче

* `id` обязательный параметр

`GET /api/v1/full-schedule/doctor?id=26` → 200, 400
#### Ответы
200
```json
{
    "msg": "",
    "data": [
        {
            "id": 26,
            "fio": "Фамилия Имя Отчество",
            "office_id": 17,
            "city": "Челябинск",
            "specialty_name": "Генетик",
            "grid_pitch": 60,
            "time_zone": "+05:00"
        }       
    ]
}
```
* grid_pitch - это время приёма в минутах

#### Если у доктора неактивно взаимодействие по API, вернётся пустой массив
200
```json
{
    "msg": "",
    "data": []
}
```

400
```json
{
    "msg": "Не указан параметр id",
    "data": []
}
```




### Получить ячейки расписания для одного доктора
* `doctorId` обязательный параметр
```
  type TYPE_OFFLINE = 1; // (очно)
       TYPE_ONLINE_OFFICE = 2; // (онлайн офис) Физически в офисе в кабинете, но консультация онлайн
       TYPE_ONLINE_REMOTE = 3; // онлайн дистанционно
```

`GET /api/v1/full-schedule/cell-list-for-one?doctorId=31` → 200, 400

#### Ответы

200
```json
{
    "msg": "",
    "data": [
        {
            "dt": "2023-06-22",
            "time_start": "10:00:00",
            "time_end": "11:00:00",
            "free": 0,
            "type": 1
        },
        {
            "dt": "2023-06-22",
            "time_start": "11:00:00",
            "time_end": "12:00:00",
            "free": 1,
            "type": 1
        },
        {
            "dt": "2023-06-23",
            "time_start": "10:00:00",
            "time_end": "11:00:00",
            "free": 1,
            "type": 1
        }
    ]
}
```
* free отвечает за занятость ячейки 0 - занято, 1 - свободно


#### Если у доктора не стоит флаг в CRM доступен для API, то его расписание будет недоступно, вернётся пустой массив

`GET /api/v1/full-schedule/cell-list-for-one?doctorId=32` → 200, 400

#### Ответы

200
```json
{
    "msg": "",
    "data": []
}
```

400
```json
{
    "msg": "Не указан параметр doctorId",
    "data": []
}
```
