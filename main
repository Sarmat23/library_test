import json
from typing import List, Dict, Union


class Book:
    """Класс для представления книги."""

    def __init__(self, title: str, author: str, year: int):
        self.id = None  # ID будет назначен автоматически
        self.title = title
        self.author = author
        self.year = year
        self.status = "в наличии"

    def to_dict(self) -> Dict[str, Union[int, str]]:
        """Преобразует объект книги в словарь."""
        return {
            "id": self.id,
            "title": self.title,
            "author": self.author,
            "year": self.year,
            "status": self.status,
        }

    @staticmethod
    def from_dict(data: Dict[str, Union[int, str]]) -> "Book":
        """Создает объект книги из словаря."""
        book = Book(data["title"], data["author"], data["year"])
        book.id = data["id"]
        book.status = data["status"]
        return book


class Library:
    """Класс для управления библиотекой."""

    def __init__(self, data_file: str = "library.json"):
        self.data_file = data_file
        self.books: List[Book] = []
        self.load_books()

    def load_books(self):
        """Загружает данные о книгах из файла."""
        try:
            with open(self.data_file, "r", encoding="utf-8") as f:
                data = json.load(f)
                self.books = [Book.from_dict(book) for book in data]
        except FileNotFoundError:
            self.books = []
        except json.JSONDecodeError:
            print("Ошибка чтения файла данных. Файл поврежден.")

    def save_books(self):
        """Сохраняет данные о книгах в файл."""
        with open(self.data_file, "w", encoding="utf-8") as f:
            json.dump([book.to_dict() for book in self.books], f, ensure_ascii=False, indent=4)

    def add_book(self, title: str, author: str, year: int):
        """Добавляет новую книгу в библиотеку."""
        new_book = Book(title, author, year)
        new_book.id = len(self.books) + 1
        self.books.append(new_book)
        self.save_books()
        print(f"Книга '{title}' успешно добавлена с ID {new_book.id}.")

    def remove_book(self, book_id: int):
        """Удаляет книгу из библиотеки по ID."""
        for book in self.books:
            if book.id == book_id:
                self.books.remove(book)
                self.save_books()
                print(f"Книга с ID {book_id} успешно удалена.")
                return
        print(f"Книга с ID {book_id} не найдена.")

    def search_books(self, key: str, value: Union[str, int]):
        """Ищет книги по указанному ключу."""
        results = [book for book in self.books if str(getattr(book, key, "")).lower() == str(value).lower()]
        if results:
            print("Найдено:")
            for book in results:
                print(book.to_dict())
        else:
            print("Книги по заданному запросу не найдены.")

    def display_books(self):
        """Выводит список всех книг."""
        if not self.books:
            print("Библиотека пуста.")
            return
        for book in self.books:
            print(book.to_dict())

    def update_status(self, book_id: int, new_status: str):
        """Обновляет статус книги по ID."""
        for book in self.books:
            if book.id == book_id:
                if new_status in ["в наличии", "выдана"]:
                    book.status = new_status
                    self.save_books()
                    print(f"Статус книги с ID {book_id} обновлен на '{new_status}'.")
                else:
                    print("Неверный статус. Используйте 'в наличии' или 'выдана'.")
                return
        print(f"Книга с ID {book_id} не найдена.")


def main():
    library = Library()

    while True:
        print("\nМеню:")
        print("1. Добавить книгу")
        print("2. Удалить книгу")
        print("3. Искать книгу")
        print("4. Показать все книги")
        print("5. Изменить статус книги")
        print("0. Выйти")

        choice = input("Выберите действие: ").strip()
        if choice == "1":
            title = input("Введите название книги: ").strip()
            author = input("Введите автора книги: ").strip()
            year = int(input("Введите год издания: ").strip())
            library.add_book(title, author, year)
        elif choice == "2":
            book_id = int(input("Введите ID книги для удаления: ").strip())
            library.remove_book(book_id)
        elif choice == "3":
            key = input("Введите поле для поиска (title, author, year): ").strip()
            value = input("Введите значение для поиска: ").strip()
            library.search_books(key, value)
        elif choice == "4":
            library.display_books()
        elif choice == "5":
            book_id = int(input("Введите ID книги: ").strip())
            new_status = input("Введите новый статус (в наличии/выдана): ").strip()
            library.update_status(book_id, new_status)
        elif choice == "0":
            print("До свидания!")
            break
        else:
            print("Неверный выбор. Попробуйте снова.")


if __name__ == "__main__":
    main()
