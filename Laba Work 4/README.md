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

### 4. Метод DELETE для удаления пациента
```python
@app.route('/patients/<int:id>', methods=['DELETE'])
def delete_patient(id):
    db.delete_patient(id)
    return jsonify({"message": f"Пациент с ID {id} успешно удален."}), 200
```

### 5. Метод GET для получения результатов диагностики
```python
@app.route('/diagnosis_results', methods=['GET'])
def get_diagnosis_results():
    results = db.get_diagnosis_results()
    return jsonify({"results": results}), 200
```

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
