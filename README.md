import csv
import json
import os

# Задача 1 - Создание CSV файла animals.csv
def create_animals_csv():
    data = [
        ['Животное', 'Среда обитания'],
        ['Медведь', 'Лес'],
        ['Дельфин', 'Океан'],
        ['Верблюд', 'Пустыня']
    ]
    
    with open('animals.csv', 'w', newline='', encoding='utf-8') as file:
        writer = csv.writer(file)
        writer.writerows(data)
    
    print("Файл animals.csv создан успешно!")
    print("Содержимое файла:")
    with open('animals.csv', 'r', encoding='utf-8') as file:
        print(file.read())

# Задача 5 - Преобразование CSV в JSON
def csv_to_json():
    animals_list = []
    
    with open('animals.csv', 'r', encoding='utf-8') as csv_file:
        reader = csv.DictReader(csv_file)
        for row in reader:
            animals_list.append(row)
    
    with open('zoo.json', 'w', encoding='utf-8') as json_file:
        json.dump(animals_list, json_file, indent=4, ensure_ascii=False)
    
    print("\nФайл zoo.json создан успешно!")
    print("Содержимое файла:")
    with open('zoo.json', 'r', encoding='utf-8') as file:
        print(file.read())

# Задача 1 (вторая часть) - Чтение CSV файла с фильтрацией
def read_employees_csv():
    employees_data = [
        ['Имя', 'Возраст', 'Город', 'Должность'],
        ['Иван', '35', 'Москва', 'Инженер'],
        ['Мария', '28', 'Санкт-Петербург', 'Дизайнер'],
        ['Алексей', '42', 'Казань', 'Менеджер'],
        ['Ольга', '31', 'Екатеринбург', 'Аналитик'],
        ['Дмитрий', '25', 'Новосибирск', 'Разработчик'],
        ['Анна', '45', 'Москва', 'Директор']
    ]
    
    with open('csv_file.csv', 'w', newline='', encoding='utf-8') as file:
        writer = csv.writer(file)
        writer.writerows(employees_data)
    
    print("\nТестовый файл csv_file.csv создан")
    
    try:
        with open('csv_file.csv', 'r', encoding='utf-8') as file:
            reader = csv.DictReader(file)
            
            print("\nСотрудники старше 30 лет:")
            for row in reader:
                try:
                    age = int(row['Возраст'])
                    if age > 30:
                        print(f"Имя: {row['Имя']}, Возраст: {age}, Должность: {row['Должность']}")
                except ValueError:
                    continue
    except FileNotFoundError:
        print("Файл не найден")

# Задача 5 (вторая часть) - Функции для преобразования
def convert_csv_to_json(csv_path: str, json_path: str) -> None:
    """Преобразует CSV файл в JSON файл"""
    try:
        data_list = []
        
        with open(csv_path, 'r', encoding='utf-8') as csv_file:
            reader = csv.DictReader(csv_file)
            for row in reader:
                data_list.append(row)
        
        with open(json_path, 'w', encoding='utf-8') as json_file:
            json.dump(data_list, json_file, indent=4, ensure_ascii=False)
        
        print(f"\nФайл {json_path} успешно создан из {csv_path}")
        
    except FileNotFoundError:
        print(f"Ошибка: файл {csv_path} не найден")
    except Exception as e:
        print(f"Ошибка при преобразовании CSV в JSON: {e}")

def convert_json_to_csv(json_path: str, csv_path: str) -> None:
    """Преобразует JSON файл в CSV файл"""
    try:
        with open(json_path, 'r', encoding='utf-8') as json_file:
            data_list = json.load(json_file)
        
        if not data_list:
            print("Ошибка: JSON файл пуст")
            return
        
        fieldnames = list(data_list[0].keys())
        
        with open(csv_path, 'w', newline='', encoding='utf-8') as csv_file:
            writer = csv.DictWriter(csv_file, fieldnames=fieldnames)
            writer.writeheader()
            writer.writerows(data_list)
        
        print(f"Файл {csv_path} успешно создан из {json_path}")
        
    except FileNotFoundError:
        print(f"Ошибка: файл {json_path} не найден")
    except Exception as e:
        print(f"Ошибка при преобразовании JSON в CSV: {e}")

def main():
    print("=" * 50)
    print("ЗАДАЧА 1: Создание CSV файла animals.csv")
    print("=" * 50)
    create_animals_csv()
    
    print("\n" + "=" * 50)
    print("ЗАДАЧА 5: Преобразование animals.csv в zoo.json")
    print("=" * 50)
    csv_to_json()
    
    print("\n" + "=" * 50)
    print("ЗАДАЧА 1 (вторая): Фильтрация сотрудников по возрасту")
    print("=" * 50)
    read_employees_csv()
    
    print("\n" + "=" * 50)
    print("ЗАДАЧА 5 (вторая): Функции для преобразования форматов")
    print("=" * 50)
    
    print("\n1. Тестируем convert_csv_to_json:")
    convert_csv_to_json('csv_file.csv', 'employees.json')
    
    print("\nСодержимое созданного employees.json:")
    with open('employees.json', 'r', encoding='utf-8') as file:
        print(file.read())
    
    print("\n2. Тестируем convert_json_to_csv:")
    convert_json_to_csv('zoo.json', 'animals_from_json.csv')
    
    print("\nСодержимое созданного animals_from_json.csv:")
    with open('animals_from_json.csv', 'r', encoding='utf-8') as file:
        print(file.read())
    
    print("\n3. Дополнительный тест: преобразуем employees.json обратно в CSV:")
    convert_json_to_csv('employees.json', 'employees_from_json.csv')
    
    print("\n" + "=" * 50)
    print("Список созданных файлов:")
    print("=" * 50)
    files = ['animals.csv', 'zoo.json', 'csv_file.csv', 'employees.json', 
             'animals_from_json.csv', 'employees_from_json.csv']
    
    for file in files:
        if os.path.exists(file):
            print(f" {file} - {os.path.getsize(file)} байт")
        else:
            print(f" {file} - не найден")

if __name__ == "__main__":
    main()
