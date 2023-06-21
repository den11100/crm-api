## Общее
Во всех запросах В header нужно передавать параметр `X-Api-Key`
иначе будет ошибка `bad authorization` → 401

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



### Получить список врачей в одно офисе в параметрах передавать

* `officeId` обязательный параметр

`GET /api/v1/full-schedule/doctors?officeId=1` → 200, 400

200
```json
{
    "msg": "",
    "data": [
        {
            "id": 91,
            "fio": "Фамилия Имя Отчество",
            "office_id": 1,
            "city": "Москва",          
            "specialty_name": "Генетик",
            "grid_pitch": 60
        },
        {
            "id": 92,
            "fio": "Фамилия Имя Отчество",
            "office_id": 1,
            "city": "Москва",            
            "specialty_name": "Невролог",
            "grid_pitch": 60
        }
    ]
}
```
* grid_pitch - это время приёма в минутах

400
```json
{
    "msg": "Не указан officeId",
    "data": []
}
```




### Получить ячейки расписания
* Тут надо протестировать для какого количества врачей можно запрашивать ячейки, пока что ограничений нет. (Но если будет сильно нагружать сервер, то придётся ограничить.)
* Если у доктора не стоит флаг в CRM доступен по ip, то его расписание будет недоступно
* В теле запроса передать id врачей через запятую
```json
{
    "ids": "91,92,93"
}
```
`POST /api/v1/full-schedule/cell-list` → 200, 400

200
```json
{
    "msg": "",
    "data": [
        {
            "id": 91,
            "dt": "2023-06-22",
            "t_start": "10:00",
            "t_end": "11:00",
            "free": 0
        },
        {
            "id": 91,
            "dt": "2023-06-22",
            "t_start": "11:00",
            "t_end": "12:00",
            "free": 1
        },
        {
            "id": 92,
            "dt": "2023-06-22",
            "t_start": "10:00",
            "t_end": "12:00",
            "free": 1
        }
    ]
}
```
* id это id врача
* free отвечает за занятость ячейки 0 - занято, 1 - свободно


400
```json
{
    "msg": "Отсутствует параметр ids",
    "data": []
}
```
