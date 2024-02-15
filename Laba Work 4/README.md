## Реализуемое API:

### 1. Метод GET для получения списка пациентов
- **URL:** `/patients`
- **Описание запроса:** Получение списка всех зарегистрированных пациентов.
- **Параметры запроса:** Отсутствуют.
- **Формат ответа:** JSON
- **Пример ответа:**
```json
{
    "patients": [
        {
            "id": 1,
            "name": "Иванов Иван",
            "age": 45,
            "gender": "Мужской",
            "diagnosis": "Бронхит"
        },
        {
            "id": 2,
            "name": "Петрова Анна",
            "age": 32,
            "gender": "Женский",
            "diagnosis": "Астма"
        }
    ]
}
```

### 2. Метод POST для добавления нового пациента
- **URL:** `/patients`
- **Описание запроса:** Добавление нового пациента в базу данных.
- **Параметры запроса:** JSON объект с данными пациента (имя, возраст, пол, диагноз).
- **Формат передаваемых данных:**
```json
{
    "name": "Сидорова Елена",
    "age": 50,
    "gender": "Женский",
    "diagnosis": "Пневмония"
}
```
- **Формат ответа:** JSON с подтверждением добавления пациента.
- **Пример ответа:**
```json
{
    "message": "Пациент Сидорова Елена успешно добавлен."
}
```
### Принятые проектные решения:

1. Использование RESTful подхода для построения API.
2. Использование JSON формата для обмена данными между клиентом и сервером.
3. Защита API с помощью токенов аутентификации.
4. Использование структурированных URL для удобства использования API.
5. Реализация механизма обработки ошибок и возврата соответствующих HTTP статус кодов.
6. Логирование действий при обращении к API для отслеживания и отладки.
7. Валидация входных данных на сервере для обеспечения корректности и безопасности операций.
8. Документирование API методов для удобства использования и интеграции.

## Реализация API:

### 1. Метод GET для получения списка пациентов
```python
@app.route('/patients', methods=['GET'])
def get_patients():
    patients = db.get_all_patients()
    return jsonify({"patients": patients}), 200
```
![image_2024-02-15_23-27-00](https://github.com/DenisShelby/paps_hse/assets/100212027/7fc9efe3-8af5-4877-9bbb-6d52a84862f9)
В данном коде возвращается информация о всех пациентах, которая была получена из базы данных. Информация о пациентах включает их идентификатор, имя, возраст, пол и диагноз. Возвращаемая информация возвращается в формат JSON с ключом "patients".
### 2. Метод POST для добавления нового пациента
```python
@app.route('/patients', methods=['POST'])
def add_patient():
    data = request.get_json()
    new_patient = {
        "name": data["name"],
        "age": data["age"],
        "gender": data["gender"],
        "diagnosis": data["diagnosis"]
    }
    db.add_patient(new_patient)
    return jsonify({"message": f"Пациент {data['name']} успешно добавлен."}), 201
```
![image_2024-02-15_23-31-34](https://github.com/DenisShelby/paps_hse/assets/100212027/2360c5d2-b6e8-4207-b306-cc05f3fb5097)
В данном коде возвращается JSON файл с сообщением о том, что пациент успешно добавлен. Сообщение содержит имя пациента, которое было передано в запросе. Формат JSON выглядит следующим образом:
```json
{
    "message": "Пациент {имя пациента} успешно добавлен."
}
```
### 3. Метод PUT для обновления данных пациента
```python
@app.route('/patients/<int:id>', methods=['PUT'])
def update_patient(id):
    data = request.get_json()
    updated_patient = {
        "name": data["name"],
        "age": data["age"],
        "gender": data["gender"],
        "diagnosis": data["diagnosis"]
    }
    db.update_patient(id, updated_patient)
    return jsonify({"message": f"Данные пациента {data['name']} успешно обновлены."}), 200
```
![image_2024-02-15_23-35-03](https://github.com/DenisShelby/paps_hse/assets/100212027/379e387b-ea62-43b9-a491-62cd31cadd46)
В данном коде возвращается JSON файл с сообщением о том, что данные пациента успешно обновлены. Сообщение содержит имя пациента, данные которого были обновлены. Формат JSON выглядит следующим образом:
```json
{
    "message": "Данные пациента {имя пациента} успешно обновлены."
}
```
### 4. Метод DELETE для удаления пациента
```python
@app.route('/patients/<int:id>', methods=['DELETE'])
def delete_patient(id):
    db.delete_patient(id)
    return jsonify({"message": f"Пациент с ID {id} успешно удален."}), 200
```
![image_2024-02-15_23-33-09](https://github.com/DenisShelby/paps_hse/assets/100212027/946fd00f-33a1-46ca-9fa0-5d9e85104f07)
В данном коде возвращается JSON файл с сообщением о том, что пациент успешно удален из базы данных. Сообщение содержит ID удаленного пациента. Формат JSON выглядит следующим образом:
```json
{
    "message": "Пациент с ID {удаленный ID} успешно удален."
}
```
### 5. Метод GET для получения результатов диагностики
```python
@app.route('/diagnosis_results', methods=['GET'])
def get_diagnosis_results():
    results = db.get_diagnosis_results()
    return jsonify({"results": results}), 200
```
![image_2024-02-15_23-36-37](https://github.com/DenisShelby/paps_hse/assets/100212027/a80521d3-ecb1-425a-88d1-2a6c61996530)
В данном коде возвращается JSON файл с информацией о результате диагностики. Формат JSON выглядит следующим образом:
```json
{
    "results": [
        {
            "id": 1,
            "patient_id": 123,
            "diagnosis": "Грипп",
            "date": "2022-10-15"
        },
        {
            "id": 2,
            "patient_id": 456,
            "diagnosis": "Ангина",
            "date": "2022-10-16"
        },
        ...
    ]
}
```
В каждом элементе списка results содержится информация о результате диагностики, включая ID результата, ID пациента, диагноз и дату проведения диагностики.
### 6. Метод POST для добавления результатов диагностики
```python
@app.route('/diagnosis_results', methods=['POST'])
def add_diagnosis_result():
    data = request.get_json()
    new_result = {
        "patient_id": data["patient_id"],
        "date": data["date"],
        "result": data["result"]
    }
    db.add_diagnosis_result(new_result)
    return jsonify({"message": f"Результаты диагностики для пациента с ID {data['patient_id']} успешно добавлены."}), 201
```
![image_2024-02-15_23-37-27](https://github.com/DenisShelby/paps_hse/assets/100212027/ec463986-507e-4ee5-bef4-240266c958e0)
В данном коде возвращается JSON файл с информацией о добавлении результатов диагностики. Формат JSON выглядит следующим образом:
```json
{
    "message": "Результаты диагностики для пациента с ID {patient_id} успешно добавлены."
}
```
В данном JSON файле содержится только одно поле message, которое сообщает об успешном добавлении результатов диагностики для пациента с указанным ID.
